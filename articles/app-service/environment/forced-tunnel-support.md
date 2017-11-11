---
title: "Konfigurace služby Azure App Service Environment být force tunelovým propojením"
description: "Povolit vaší App Service Environment fungovat, když platnost tunelovým propojením je odchozí provoz"
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
ms.openlocfilehash: cd89bd23074dec1de6fa0e8784d42a5c539938b1
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/11/2017
---
# <a name="configure-your-app-service-environment-with-forced-tunneling"></a>Konfigurace služby App Service Environment pomocí vynucené tunelování

V aplikaci služby prostředí řízení, je nasazení služby Azure App Service ve zákazníka Azure Virtual Network (VNet). Mnoho zákazníků nakonfigurovat jejich virtuální sítě na rozšíření své místní sítě VPN nebo ExpressRoute připojení. Z důvodu podnikové zásady nebo jiná omezení zabezpečení je potřeba nakonfigurovat směrování, aby mohl odesílat všechny odchozí přenosy na místě před můžete přejít na Internetu. Změna, směrování sítě VNet, aby toky odchozí provoz z virtuální sítě prostřednictvím sítě VPN nebo ExpressRoute připojení k místnímu nazývá vynucené tunelování.  

Vynucené tunelování, může způsobit problémy pro App Service Environment. App Service Environment má počet externí závislosti, které jsou uvedené v tomto [App Service Environment síťovou architekturu] [ network] dokumentu. App Service Environment, ve výchozím nastavení, vyžaduje, aby veškerá odchozí komunikace probíhá prostřednictvím VIP, který je zajištěný s App Service Environment.

Trasy jsou kritické aspekt vynucené tunelování novinky a řešení problémů s ním. Ve virtuální síti Azure směrování se provádí na základě na nejdelší shody předpony (LPM).  Pokud existuje víc tras se stejnou shodou LPM, pak trasa se vybere na základě původu v následujícím pořadí:

1. Trasa definovaná uživatelem
1. Trasa protokolu BGP (pokud se používá služba ExpressRoute)
1. Systémová trasa

Další informace o směrování ve virtuální síti, přečtěte si [trasách definovaných uživatelem a předávání IP][routes]. 

Pokud chcete, aby vaše App Service Environment provoz vynucené tunelové propojení virtuální sítě, máte dvě možnosti:

1. Povolit vaší App Service Environment přímý přístup k Internetu
1. Změnit výstupní koncový bod pro vaše App Service Environment

## <a name="enable-your-ase-to-have-direct-internet-access"></a>Povolit vaší App Service Environment přímý přístup k Internetu

Pro vaše App Service Environment, kterou práci, zatímco jsou vaše virtuální síť nakonfigurované ExpressRoute můžete:

* Konfigurace ExpressRoute a nabídnout 0.0.0.0/0. Ve výchozím nastavení, je vynutit tunely všechny odchozí přenosy na místě.
* Vytvořte UDR. Použijte ho pro podsítě, která obsahuje App Service Environment se předponu adresy 0.0.0.0/0 a typ dalšího segmentu z Internetu.

Pokud provedete tyto dvě změny, určené internetové komunikaci z podsítě App Service Environment není vynutit dolů ExpressRoute a App Service Environment funguje.

> [!IMPORTANT]
> Trasy definované v UDR musí být dost konkrétní, aby mají přednost před všechny trasy inzerované konfigurace ExpressRoute. V předchozím příkladu používá 0.0.0.0/0 široký rozsah adres. Se dá potenciálně omylem přepsat inzerování tras, které používají podrobnější rozsahy adres.
>
> Konfigurace ExpressRoute, které mezi Inzerovat trasy z cesty partnerského vztahu veřejný partnerský vztah privátní cestu ASEs nepodporuje. Konfigurace ExpressRoute s veřejné partnerské vztahy nakonfigurované dostávat inzerování trasy od Microsoftu. Tato oznámení obsahují velké sady rozsahů adres Microsoft Azure IP. Pokud rozsahy adres ohlášené mezi v cestě k vytvoření soukromého partnerského vztahu, jsou všechny odchozí síťových paketů z podsítě App Service Environment force tunelovým propojením zákazníka místní síťové infrastruktuře. Ve výchozím nastavení s ASEs aktuálně nepodporuje tento tok sítě. Jedno řešení tohoto problému se trasy mezi – reklamu z cesty partnerského vztahu veřejné do cesty partnerského vztahu privátní ukončit.  Jiné řešení je umožnit vaší App Service Environment pro práci v konfiguraci vynucené tunelové propojení.

## <a name="change-the-egress-endpoint-for-your-ase"></a>Změnit výstupní koncový bod pro vaše App Service Environment ##

Tato část popisuje, jak povolit App Service Environment pracovat v konfiguraci s vynuceným tunelem změnou výstupní koncový bod používán App Service Environment. Pokud je nucen odchozí provoz z App Service Environment tunelovým propojením k místní síti, pak je potřeba povolit tento provoz do zdroje z IP adresy než adresy VIP App Service Environment.

App Service Environment nejen má externí závislosti, ale je také musí naslouchat příchozí přenosy pro správu App Service Environment. App Service Environment musí být schopné reagovat na takové provoz a odpovědí nelze odeslat zpět z jiné adresy, jako který dělí TCP.  Proto existují tři kroky potřebné ke změně výstupní koncový bod pro App Service Environment.

1. Nastavit směrovací tabulku zajistit, že provoz správy příchozí můžete přejděte zpět ze stejné IP adresy
1. Přidejte že IP adres, který má být použit pro odchozí do brány firewall App Service Environment
1. Nastavit trasy pro odchozí provoz z App Service Environment pro být tunelovým propojením

![Vynutit tunelové propojení sítě toku][1]

App Service Environment můžete nakonfigurovat různé odchozí adresy po App Service Environment je již a funkční nebo lze nastavit během nasazení App Service Environment.  

### <a name="changing-the-egress-address-after-the-ase-is-operational"></a>Změna adresu odchozí po provozní App Service Environment ###
1. Získáte IP adresy, které chcete použít jako výstupní IP adresy pro vaše App Service Environment. Pokud byste vynutit pak bude vaše zařízení NAT nebo brány IP adresy tunelového připojení.  Pokud chcete pro směrování odchozího provozu App Service Environment prostřednictvím hodnocení chyb zabezpečení, adresu odchozí by veřejné IP adresy z hodnocení chyb zabezpečení.
2. Nastavte na odchozí adresy v informace o konfiguraci vašeho App Service Environment. Přejděte na resource.azure.com a přejděte do: předplatné nebo<subscription id>/resourceGroups/<ase resource group>/providers/Microsoft.Web/hostingEnvironments/<ase name> pak můžete zobrazit formátu json, který popisuje vaše App Service Environment.  Ujistěte se, že zobrazuje pro čtení a zápis v horní části.  Klikněte na Upravit přejděte dolů a změňte userWhitelistedIpRanges z  

       "userWhitelistedIpRanges": null 
      
  na něco jako následující. Použijte adresy, které chcete nastavit jako výchozí rozsah adres. 

      "userWhitelistedIpRanges": ["11.22.33.44/32", "55.66.77.0/24"] 

  V horní části klikněte na tlačítko Vložit. Tato operace škálování na vaše App Service Environment se aktivuje a upravit brány firewall.
   
3. Vytvořit nebo upravit směrovací tabulku a naplnit pravidel pro povolení přístupu z adresy pro správu, které mapují do umístění app Service Environment.  Tady jsou adresy pro správu [adresy pro správu služby App Service Environment][management] 

4. Upravte použije na podsíť App Service Environment k směrovací tabulce směrování nebo směrování BGP.  

Pokud App Service Environment přestane reagovat na portálu, je problém s vašimi změnami.  Může být seznam odchozí adresy se nedokončila, provoz došlo ke ztrátě nebo provoz byl zablokován.  

### <a name="create-a-new-ase-with-a-different-egress-address"></a>Vytvořit nový App Service Environment s adresou jinou odchozí  ###

V případě, že virtuální sítě je již nakonfigurován k vynucení tunelu veškerý provoz, musíte udělat některé dodatečné kroky k vytvoření vašeho App Service Environment tak, že může úspěšně přijít. To znamená, že je nutné povolit používání jiným koncovým bodem odchozí během vytváření App Service Environment.  K tomu potřebujete vytvořit App Service Environment pomocí šablony, která určuje odchozí povolených adres.

1. Získáte IP adresy, které má být použit jako odchozí adresy pro vaše App Service Environment.
1. Předběžné vytvoření podsítě, který se používá App Service Environment. To je nutné, abyste mohli nastavit trasy a také vzhledem k tomu, že šablona vyžaduje.  
1. Vytvořit směrovací tabulku se správou IP adres, který mapovat do umístění app Service Environment a přiřaďte ho ke vaší App Service Environment
1. Postupujte podle pokynů tady [vytvoření App Service Environment pomocí šablony][template]a stahují příslušné šablony
1. Upravte část "zdroje" soubor azuredeploy.json. Zahrnout řádek pro **userWhitelistedIpRanges** s hodnoty jako:

       "userWhitelistedIpRanges":  ["11.22.33.44/32", "55.66.77.0/30"]

Pokud to je správně nakonfigurovaná, App Service Environment začněte bez problémů.  


<!--IMAGES-->
[1]: ./media/forced-tunnel-support/forced-tunnel-flow.png

<!--Links-->
[management]: ./management-addresses.md
[network]: ./network-info.md
[routes]: ../../virtual-network/virtual-networks-udr-overview.md
[template]: ./create-from-template.md
