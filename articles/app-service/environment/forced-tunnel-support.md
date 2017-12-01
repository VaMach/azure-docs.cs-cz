---
title: "Konfigurace služby Azure App Service Environment být force tunelovým propojením"
description: "Povolení služby App Service Environment fungovat, když platnost tunelovým propojením je odchozí provoz"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 384cf393-5c63-4ffb-9eb2-bfd990bc7af1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: ccompy
ms.openlocfilehash: f5f099042cefe666e22a9d561afeb4584db3d92c
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="configure-your-app-service-environment-with-forced-tunneling"></a>Konfigurace služby App Service Environment pomocí vynucené tunelování

Služba App Service Environment je nasazení služby Azure App Service v zákazníka instanci Azure Virtual Network. Mnoho zákazníků konfigurovat jejich virtuální sítě na rozšíření své místní sítě VPN nebo Azure ExpressRoute připojení. Z důvodu podnikové zásady nebo jiná omezení zabezpečení je potřeba nakonfigurovat směrování, aby mohl odesílat všechny odchozí přenosy na místě před můžete přejít na Internetu. Změna směrování virtuální sítě tak, aby toky odchozí provoz z virtuální sítě prostřednictvím sítě VPN nebo ExpressRoute připojení k místnímu nazývá vynucené tunelování. 

Vynucené tunelování, může způsobit problémy pro služby App Service Environment. Služba App Service Environment má počet externí závislosti, které jsou uvedené v [architektura sítě služby App Service Environment] [ network] dokumentu. Služba App Service Environment, ve výchozím nastavení, vyžaduje, aby veškerá odchozí komunikace probíhá prostřednictvím VIP, který je zajištěný s App Service Environment.

Trasy jsou zásadní aspekt jaké vynucené tunelování je a o tom, jak nakládat s ním. Ve virtuální síť Azure se provádí směrování podle nejdelší shody předpony (LPM). Pokud existuje víc tras se stejnou shodou LPM, trasa se vybere na základě původu v následujícím pořadí:

* Trasy definované uživatelem (UDR)
* Trasa protokolu BGP (pokud se používá služba ExpressRoute)
* Systémová trasa

Další informace o směrování ve virtuální síti, přečtěte si [trasy definované uživatelem a předávání IP][routes]. 

Pokud chcete služby App Service Environment provoz ve virtuální síti vynucené tunelu, máte dvě možnosti:

* Povolení služby App Service Environment přímý přístup k Internetu.
* Změňte výstupní koncový bod pro služby App Service Environment.

## <a name="enable-your-app-service-environment-to-have-direct-internet-access"></a>Povolení služby App Service Environment přímý přístup k Internetu

Pro App Service Environment práci, zatímco vaše virtuální síť je konfigurována s připojení ExpressRoute můžete:

* Konfigurace ExpressRoute a nabídnout 0.0.0.0/0. Ve výchozím nastavení, je vynutit tunely všechny odchozí přenosy na místě.
* Vytvořte UDR. Použijte ho pro podsítě, která obsahuje App Service Environment se předponu adresy 0.0.0.0/0 a typ dalšího segmentu z Internetu.

Pokud provedete tyto dvě změny, určené internetové komunikaci z podsítě služby App Service Environment není vynucena připojení ExpressRoute a funguje App Service Environment.

> [!IMPORTANT]
> Trasy definované v UDR musí být dost konkrétní, aby mají přednost před všechny trasy inzerované konfigurace ExpressRoute. V předchozím příkladu používá 0.0.0.0/0 široký rozsah adres. Se dá potenciálně omylem přepsat inzerování tras, které používají podrobnější rozsahy adres.
>
> Služby App Service Environment nejsou podporovány s konfigurací ExpressRoute, které mezi Inzerovat trasy z cesty partnerského vztahu veřejný partnerský vztah privátní cestu. Konfigurace ExpressRoute s veřejné partnerské vztahy nakonfigurované dostávat inzerování trasy od Microsoftu. Tato oznámení obsahují velké sady rozsahů adres Microsoft Azure IP. Pokud rozsahy adres ohlášené mezi v cestě k vytvoření soukromého partnerského vztahu, jsou všechny odchozí síťových paketů z podsítě App Service Environment force tunelovým propojením zákazníka místní síťové infrastruktuře. Tento tok sítě není aktuálně podporováno ve výchozím nastavení se prostředí App Service. Jedno řešení tohoto problému se trasy mezi – reklamu z cesty partnerského vztahu veřejné do cesty partnerského vztahu privátní ukončit. Jiným řešením je povolit služby App Service Environment pro práci v konfiguraci vynucené tunelové propojení.

## <a name="change-the-egress-endpoint-for-your-app-service-environment"></a>Změnit výstupní koncový bod pro služby App Service Environment ##

Tato část popisuje postup povolení služby App Service Environment pracovat v konfiguraci s vynuceným tunelem změnou výstupní koncový bod používán App Service Environment. Pokud odchozí provoz z App Service Environment je force tunelovým propojením k místní síti, budete muset povolit tento provoz do zdroje z IP adresy než adresy VIP prostředí App Service.

Služby App Service Environment nejen má externí závislosti, ale také musí příjem příchozích dat a reagovat na takové provoz. Odpovědi nelze odeslat zpět z jiné adresy, protože která dělí TCP. Existují tři kroky potřebné ke změně výstupní koncový bod pro služby App Service Environment:

1. Nastavte směrovací tabulku zajistit, že provoz správy příchozí můžete přejděte zpět ze stejné IP adresy.

2. Přidejte vaší IP adresy, které mají být použita pro odchozí do služby App Service Environment brány firewall.

3. Nastavte trasy pro odchozí provoz z App Service Environment, chcete-li být tunelovým propojením.

   ![Vynutit tunelové propojení sítě toku][1]

Služba App Service Environment můžete nakonfigurovat různé odchozí adresy po App Service Environment je již a funkční, nebo lze nastavit během nasazování služby App Service Environment.

### <a name="change-the-egress-address-after-the-app-service-environment-is-operational"></a>Změna adresy odchozí po provozní App Service Environment ###
1. Získáte IP adresy, které chcete použít jako výstupní IP adresy pro App Service Environment. Pokud provádíte vynucené tunelování, tyto adresy pocházet ze zařízení NAT nebo brány IP adresy. Pokud chcete směrovat odchozí komunikaci služby App Service Environment prostřednictvím hodnocení chyb zabezpečení, je adresu odchozí veřejné IP adresy z hodnocení chyb zabezpečení.

2. Nastavte na odchozí adresy v vaše informace o konfiguraci služby App Service Environment. Přejděte do resource.azure.com a přejděte na předplatné nebo<subscription id>/resourceGroups/<ase resource group>/providers/Microsoft.Web/hostingEnvironments/<ase name>. Potom můžete zobrazit formátu JSON, který popisuje služby App Service Environment. Zajistěte, aby se říká **pro čtení a zápis** v horní části. Vyberte **upravit**. Posuňte se dolů a změňte **userWhitelistedIpRanges** z hodnoty **null** něco jako následující. Použijte adresy, které chcete nastavit jako výchozí rozsah adres. 

        "userWhitelistedIpRanges": ["11.22.33.44/32", "55.66.77.0/24"] 

   Vyberte **PUT** v horní části. Tato možnost spustí operaci škálování na služby App Service Environment a upraví bránu firewall.
 
3. Vytvořit nebo upravit směrovací tabulku a naplnit pravidel pro povolení přístupu z adresy pro správu, které mapují do umístění služby App Service Environment. Adresy pro správu, najdete v tématu [adresy pro správu služby App Service Environment][management].

4. Upravte použije na podsíť služby App Service Environment k směrovací tabulce směrování nebo směrování BGP. 

Pokud služba App Service Environment přestane reagovat na portálu, došlo k potížím s vašimi změnami. Tento problém může být seznam odchozí adresy se nedokončila, provoz došlo ke ztrátě nebo byl zablokován provoz. 

### <a name="create-a-new-app-service-environment-with-a-different-egress-address"></a>Vytvoření nové služby App Service Environment pomocí různých odchozí adresy ###

Pokud virtuální sítě je již nakonfigurován k vynucení tunelu veškerý provoz, budete muset udělat dodatečné kroky k vytvoření služby App Service Environment, aby se můžete úspěšně přijít. Budete muset povolit používání jiným koncovým bodem odchozí během vytváření služby App Service Environment. K tomu potřebujete vytvořit App Service Environment pomocí šablony, která určuje odchozí povolených adres.

1. Získáte IP adresy, které má být použit jako odchozí adresy pro App Service Environment.

2. Předběžné vytvoření podsítě, který se používá služba App Service Environment. Je nutné, aby můžete nastavit trasy a také vzhledem k tomu, že šablona vyžaduje.

3. Vytvořte směrovací tabulku se správou IP adres, které jsou mapovány do umístění služby App Service Environment. Přiřaďte ho ke službě App Service Environment.

4. Postupujte podle pokynů v [vytvoření služby App Service Environment pomocí šablony][template]. Stáhněte dolů příslušné šablony.

5. Upravte část "zdroje" v souboru azuredeploy.json. Zahrnout řádek pro **userWhitelistedIpRanges** s hodnoty takto:

       "userWhitelistedIpRanges":  ["11.22.33.44/32", "55.66.77.0/30"]

Pokud v této části je správně nakonfigurovaná, App Service Environment začněte bez problémů. 


<!--IMAGES-->
[1]: ./media/forced-tunnel-support/forced-tunnel-flow.png

<!--Links-->
[management]: ./management-addresses.md
[network]: ./network-info.md
[routes]: ../../virtual-network/virtual-networks-udr-overview.md
[template]: ./create-from-template.md
