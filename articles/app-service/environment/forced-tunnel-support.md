---
title: "Konfigurace vynuceného tunelového propojení ve službě Azure App Service Environment"
description: "Umožňujete fungování služby App Service Environment s vynuceným tunelovým propojením pro odchozí přenosy"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 384cf393-5c63-4ffb-9eb2-bfd990bc7af1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/10/2017
ms.author: ccompy
ms.custom: mvc
ms.openlocfilehash: 4caaf0df3f1dd4b2cb9b76283a6beed897531c1c
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="configure-your-app-service-environment-with-forced-tunneling"></a>Konfigurace vynuceného tunelového propojení ve službě App Service Environment

App Service Environment je nasazení služby Azure App Service v instanci služby Azure Virtual Network zákazníka. Mnoho zákazníků konfiguruje své virtuální sítě jako rozšíření svých místních sítí pomocí sítí VPN nebo připojení Azure ExpressRoute. Kvůli podnikovým zásadám nebo jiným omezením z důvodu zabezpečení konfigurují trasy pro zasílání veškerých odchozích přenosů do místní sítě, odkud teprve putují na internet. Změna trasy virtuální sítě tak, aby odchozí přenosy z virtuální sítě směřovaly přes síť VPN nebo připojení ExpressRoute do místní sítě, se označuje jako vynucené tunelové propojení. 

Vynucené tunelové propojení může ve službě App Service Environment způsobit potíže. Služba App Service Environment má řadu externích závislostí uvedených v dokumentu [Architektura sítě služby App Service Environment][network]. Služba App Service Environment ve výchozím nastavení vyžaduje, aby veškerá odchozí komunikace procházela přes virtuální IP adresu zřízenou ve službě App Service Environment.

Zásadním aspektem vynuceného tunelového propojení a toho, jak s ním naložit, jsou trasy. Ve virtuální síti Azure se směrování provádí na základě nejdelší shody předpony (LPM). Pokud existuje víc tras se stejnou shodou LPM, trasa se vybere na základě původu v tomto pořadí:

* Trasa definovaná uživatelem (UDR)
* Trasa protokolu BGP (pokud se používá služba ExpressRoute)
* Systémová trasa

Další informace o směrování ve virtuální síti najdete v tématu [Trasy definované uživatelem a předávání IP][routes]. 

Pokud chcete, aby služba App Service Environment fungovala ve virtuální síti s vynuceným tunelovým propojením, máte dvě možnosti:

* Umožněte službě App Service Environment přímý přístup k internetu.
* Změňte výstupní koncový bod služby App Service Environment.

## <a name="enable-your-app-service-environment-to-have-direct-internet-access"></a>Povolení přímého přístupu k internetu ve službě App Service Environment

Pokud chcete, aby služba App Service Environment fungovala, když má vaše virtuální síť nakonfigurované připojení ExpressRoute, můžete postupovat takto:

* Nakonfigurujte připojení ExpressRoute, aby inzerovalo adresu 0.0.0.0/0. Ve výchozím nastavení provede vynucené tunelové propojení veškerých odchozích přenosů do místní sítě.
* Vytvořte trasu UDR. Použijte ji na podsíť, která obsahuje službu App Service Environment s předponou adresy 0.0.0.0/0 a dalším segmentem směrování typu Internet.

Pokud provedete tyto dvě změny, přenosy mířící na internet a pocházející z podsítě služby App Service Environment nebudou nuceně směřovat do připojení ExpressRoute a služba App Service Environment bude fungovat.

> [!IMPORTANT]
> Trasy definované v trase UDR musí být dost konkrétní, aby měly přednost před všemi trasami inzerovanými konfigurací ExpressRoute. V předchozím příkladu se používá široký rozsah adres 0.0.0.0/0. Může nechtěně dojít k jeho potlačení z důvodu inzerování tras, které používají konkrétnější rozsahy adres.
>
> Služby App Service Environment nejsou podporované v konfiguracích ExpressRoute, které křížově inzerují trasy z cesty s veřejnými partnerskými uzly do cesty se soukromými partnerskými uzly. Konfigurace ExpressRoute s nakonfigurovanými veřejnými partnerskými uzly přijímají inzerci tras od Microsoftu. Tyto inzerce obsahují velkou sadu rozsahů IP adres Microsoft Azure. Pokud probíhá křížová inzerce rozsahů adres na cestě se soukromými partnerskými uzly, všechny odchozí síťové pakety z podsítě služby App Service Environment procházejí vynuceným tunelovým propojením do místní síťové infrastruktury zákazníka. Služba App Service Environment v současné době tento tok sítí nepodporuje. Jedním řešením tohoto problému je ukončit křížovou inzerci tras z cesty s veřejnými partnerskými uzly do cesty se soukromými partnerskými uzly. Druhým řešením je povolit fungování služeb App Service Environment v konfiguraci s vynuceným tunelovým propojením.

## <a name="change-the-egress-endpoint-for-your-app-service-environment"></a>Změna výstupního koncového bodu služby App Service Environment ##

Tato část popisuje, jak umožnit službě App Service Environment práci v konfiguraci s vynuceným tunelem tím, že změníte výstupní koncový bod používaný službou App Service Environment. Pokud odchozí přenosy ze služby App Service Environment procházejí vynuceným tunelovým propojením do místní sítě, je potřeba povolit původ těchto přenosů z jiných IP adres než z virtuálních IP adres služby App Service Environment.

Služba App Service Environment má externí závislosti, a navíc musí naslouchat příchozím přenosům a reagovat na ně. Odpovědi se nedají odesílat z jiné adresy, protože by to poškodilo protokol TCP. Ke změně výstupního koncového bodu služby App Service Environment je potřeba provést tři kroky:

1. Nastavte směrovací tabulku tak, aby se mohly příchozí přenosy správy vracet ze stejné IP adresy.

2. Přidejte IP adresy, které se budou používat na výstupu, do brány firewall služby App Service Environment.

3. Nastavte trasy pro odchozí přenosy ze služby App Service Environment na tunelové propojení.

   ![Tok sítě s nuceným tunelovým propojením][1]

Ve službě App Service Environment můžete nakonfigurovat různé výstupní adresy, když už je služba App Service Environment v provozu, nebo to můžete udělat během nasazení služby App Service Environment.

### <a name="change-the-egress-address-after-the-app-service-environment-is-operational"></a>Změna výstupní adresy po uvedení služby App Service Environment do provozu ###
1. Získejte IP adresy, které chcete použít jako výstupní IP adresy pro službu App Service Environment. Pokud používáte vynucené tunelové propojení, tyto adresy pocházejí z překladu adres nebo IP adres brány. Pokud chcete směrovat odchozí přenosy služby App Service Environment přes virtuální síťové zařízení, výstupní adresou je veřejná IP adresa tohoto virtuálního síťového zařízení.

2. Nastavte výstupní adresy v konfiguračních informacích služby App Service Environment. Přejděte na resource.azure.com a potom na Subscription/<subscription id>/resourceGroups/<ase resource group>/providers/Microsoft.Web/hostingEnvironments/<ase name>. Uvidíte zápis JSON, který popisuje vaši službu App Service Environment. Zkontrolujte, jestli se nahoře píše **Čtení/zápis**. Vyberte **Upravit**. Posuňte se dolů a u položky **userWhitelistedIpRanges** změňte hodnotu **null** na hodnotu podobné následujícímu příkladu. Použijte adresy, které chcete nastavit jako výstupní rozsah adres. 

        "userWhitelistedIpRanges": ["11.22.33.44/32", "55.66.77.0/24"] 

   V horní části vyberte možnost **PUT**. Tato možnost spustí ve vaší službě App Service Environment operaci škálování a upraví bránu firewall.
 
3. Vytvořte nebo upravte tabulku směrování a vyplňte pravidla pro povolení přístupu k adresám správy, které mapují na umístění služby App Service Environment, a z těchto adres. Adresy pro správu najdete v tématu [Adresy pro správu služby App Service Environment][management].

4. Upravte trasy použité v podsíti služby App Service Environment pomocí směrovací tabulky nebo tras BGP. 

Pokud služba App Service Environment přestane reagovat vůči portálu, znamená to, že vaše změny způsobily potíže. Může se jednat o neúplný seznam výstupních adres, o ztrátu provozu nebo blokování provozu. 

### <a name="create-a-new-app-service-environment-with-a-different-egress-address"></a>Vytvoření nové služby App Service Environment s jinou výstupní adresou ###

Pokud je už vaše virtuální síť nakonfigurovaná na vynucené tunelové propojení pro všechny přenosy, bude potřeba provést další kroky a vytvořit službu App Service Environment tak, aby mohla úspěšně fungovat. Při vytváření služby App Service Environment je potřeba povolit použití dalšího výstupního koncového bodu. K tomu je potřeba vytvořit službu App Service Environment pomocí šablony, která určuje povolené výstupní adresy.

1. Získejte IP adresy, které se mají použít jako výstupní adresy pro službu App Service Environment.

2. Předem vytvořte podsíť, kterou bude služba App Service Environment používat. Potřebujete ji k nastavení tras a vyžaduje ji také šablona.

3. Vytvořte směrovací tabulku s IP adresami správy, které mapují na vaše umístění služby App Service Environment. Přiřaďte ji ke službě App Service Environment.

4. Postupujte podle pokynů v tématu [Vytvoření služby App Service Environment pomocí šablony][template]. Stáhněte příslušnou šablonu.

5. Upravte část „resources“ v souboru azuredeploy.json. Přidejte řádek pro položku **userWhitelistedIpRanges** s hodnotami, jako jsou tyto:

       "userWhitelistedIpRanges":  ["11.22.33.44/32", "55.66.77.0/30"]

Pokud tuto část správně nakonfigurujete, služba App Service Environment by se měla bez problémů spustit. 


<!--IMAGES-->
[1]: ./media/forced-tunnel-support/forced-tunnel-flow.png

<!--Links-->
[management]: ./management-addresses.md
[network]: ./network-info.md
[routes]: ../../virtual-network/virtual-networks-udr-overview.md
[template]: ./create-from-template.md
