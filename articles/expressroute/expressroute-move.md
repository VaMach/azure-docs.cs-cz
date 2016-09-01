<properties
   pageTitle="Přesun okruhů ExpressRoute z modelu nasazení Classic do Resource Manager | Microsoft Azure"
   description="Tato stránka obsahuje přehled toho, co potřebujete vědět o přemostění modelů nasazení Classic a Resource Manager."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/20/2016"
   ms.author="ganesr"/>

# Přesun okruhů ExpressRoute z modelu nasazení Classic do Resource Manager

Tento článek obsahuje přehled toho, co znamená přesun okruhu Azure ExpressRoute z modelu nasazení Classic do Azure Resource Manager.

[AZURE.INCLUDE [vpn-gateway-sm-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Jeden okruh ExpressRoute můžete použít pro připojení k virtuálním sítím, které jsou nasazené jak v modelu nasazení Classic, tak i Resource Manager. Okruh ExpressRoute, bez ohledu na to, jak je vytvořený, se nyní může propojit k virtuálním sítím v obou modelech nasazení.

![Okruh ExpressRoute propojující se k virtuálním sítím v obou modelech nasazení](./media/expressroute-move/expressroute-move-1.png)

## Okruhy ExpressRoute vytvořené v modelu nasazení Classic

Nejprve je nutné přesunout do modelu nasazení Resource Manager okruhy ExpressRoute, které jsou vytvořené v modelu nasazení Classic, aby byla možné připojení k oběma modelům nasazení, Classic i Resource Manager. Během přesunu připojení nedojde ke ztrátě nebo přerušení připojení. Všechna propojení okruhu do virtuální sítě v modelu nasazení Classic (v rámci stejného předplatného i mezi předplatnými) se zachovají.

Po úspěšném dokončení přesunu okruh ExpressRoute vypadá a pracuje úplně stejně jako okruh ExpressRoute, který byl vytvořený v modelu nasazení Resource Manager. Nyní můžete vytvořit připojení k virtuálním sítím v modelu nasazení Resource Manager.

Po přesunutí okruhu ExpressRoute do modelu nasazení Resource Manager můžete spravovat životní cyklus okruhu ExpressRoute jenom pomocí modelu nasazení Resource Manager. To znamená, že můžete provádět operace, jako je přidání, aktualizace nebo odstranění partnerských vztahů, aktualizace vlastností okruhu (například šířky pásma, SKU a typu fakturace) a odstranění okruhů, pouze v modelu nasazení Resource Manager. V dál uvedených částech o okruzích, které jsou vytvořené v modelu nasazení Resource Manager, najdete další podrobnosti o tom, jak můžete spravovat přístup k oběma modelům nasazení.

Provedení přesunu se váš poskytovatel připojení nemusí účastnit.

## Okruhy ExpressRoute vytvořené v modelu nasazení Resource Manager

Můžete povolit, aby okruhy ExpressRoute, které jsou vytvořené v modelu nasazení Resource Manager, byly přístupné z obou modelů nasazení. Přístup z obou modelů nasazení jde povolit pro jakýkoli okruh ExpressRoute v rámci vašeho předplatného.

- Okruhy ExpressRoute, které jsou vytvořené v modelu nasazení Resource Manager, nemají standardně přístup k modelu nasazení Classic.
- Okruhy ExpressRoute, které byly přesunuté z modelu nasazení Classic do modelu nasazení Resource Manager, jsou standardně přístupné z obou modelů nasazení.
- Okruh ExpressRoute má vždy přístup k modelu nasazení Resource Manager, bez ohledu na to, jestli byl vytvořený ve modelu nasazení Classic nebo Resource Manager. To znamená, že můžete vytvořit připojení k virtuálním sítím vytvořeným v modelu nasazení Resource Manager podle pokynů v tématu [Způsob propojení virtuálních sítí](expressroute-howto-linkvnet-arm.md).
- Přístup k modelu nasazení Classic řídí parametr **allowClassicOperations** v okruhu ExpressRoute.

>[AZURE.IMPORTANT] Platí všechny kvóty, které jsou popsané na stránce [Omezení služby](../azure-subscription-service-limits.md). Například standardní okruh může mít maximálně 10 připojení virtuální sítě přes modely nasazení Classic i Resource Manager.


## Řízení přístupu k modelu nasazení Classic

Můžete povolit jeden okruh ExpressRoute k propojení virtuálních sítí v obou modelech nasazení nastavením parametru **allowClassicOperations** okruhu ExpressRoute.

Nastavení parametru **allowClassicOperations** na hodnotu TRUE vám umožní propojit virtuální sítě z obou modelů nasazení k okruhu ExpressRoute. Můžete se propojit k virtuálním sítím v modelu nasazení Classic podle pokynů v tématu [Způsob propojení virtuálních sítí v modelu nasazení Classic](expressroute-howto-linkvnet-classic.md). Můžete se propojit k virtuálním sítím v modelu nasazení Resource Manager podle pokynů v tématu [Způsob propojení virtuálních sítí v modelu nasazení Resource Manager](expressroute-howto-linkvnet-arm.md).

Nastavení parametru **allowClassicOperations** na hodnotu FALSE zablokuje přístup k obvodu z modelu nasazení Classic. Nicméně všechna propojení virtuálních sítí v modelu nasazení Classic se zachovají. V takovém případě není okruh ExpressRoute v modelu nasazení Classic viditelný.

## Podporované operace v modelu nasazení Classic

Podporovány jsou následující klasické operace pro okruh ExpressRoute, pokud je parametr **allowClassicOperations** nastavený na hodnotu TRUE:

 - Získání informací o okruhu ExpressRoute
 - Vytvoření, aktualizace, získání nebo odstranění propojení virtuální sítě ke klasickým virtuálním sítím
 - Vytvoření, aktualizace, získání nebo odstranění autorizací propojení virtuální sítě pro připojení mezi předplatnými

Nejde provádět následující klasické operace při nastavení parametru **allowClassicOperations** na hodnotu TRUE:

 - Vytvoření, aktualizace, získání nebo odstranění partnerských vztahů protokolu BGP pro soukromé partnerské vztahy Azure, veřejné partnerské vztahy Azure a partnerské vztahy Microsoftu
 - Odstranění okruhů ExpressRoute

## Komunikace mezi modely nasazení Classic a Resource Manager

Okruh ExpressRoute pracuje jako most mezi modelem nasazení Classic a modelem nasazení Resource Manager. Přenos dat mezi virtuálními počítači ve virtuálních sítích v modelu nasazení Classic a těmi ve virtuálních sítích v modelu nasazení Resource Manager prochází službou ExpressRoute, pokud jsou obě virtuální sítě propojené ke stejnému okruhu ExpressRoute.

Agregační propustnost je omezená kapacitou propustnosti brány virtuální sítě. Přenos dat v takových případech nevstupuje do sítí poskytovatele připojení nebo do vašich sítí. Přenos mezi virtuálními sítěmi je plně obsažen v rámci sítě Microsoftu.

## Přístup k prostředkům veřejných partnerských vztahů Azure a partnerských vztahů Microsoftu

Můžete nadále přístup k prostředkům, které jsou obvykle přístupné prostřednictvím veřejných partnerských vztahů Azure a partnerských vztahů Microsoftu, bez přerušení.  

## Co je podporováno

Tato část popisuje, co je podporováno pro okruhy ExpressRoute:

 - Jeden okruh ExpressRoute můžete použít pro přístup k virtuálním sítím, které jsou nasazené v modelu nasazení Classic i Resource Manager.
 - Můžete přesunout okruh ExpressRoute z modelu nasazení Classic do Resource Manager. Po přesunu okruh ExpressRoute vypadá a pracuje stejně jako kterýkoli jiný okruh ExpressRoute vytvořený v modelu nasazení Resource Manager.
 - Je možné přesunout jenom okruh ExpressRoute. Pomocí této operace nejde přesunout propojení okruhu, virtuální sítě a brány VPN.
 - Po přesunutí okruhu ExpressRoute do modelu nasazení Resource Manager můžete spravovat životní cyklus okruhu ExpressRoute jenom pomocí modelu nasazení Resource Manager. To znamená, že můžete provádět operace, jako je přidání, aktualizace nebo odstranění partnerských vztahů, aktualizace vlastností okruhu (například šířky pásma, SKU a typu fakturace) a odstranění okruhů, pouze v modelu nasazení Resource Manager.
 - Okruh ExpressRoute pracuje jako most mezi modelem nasazení Classic a modelem nasazení Resource Manager. Přenos dat mezi virtuálními počítači ve virtuálních sítích v modelu nasazení Classic a těmi ve virtuálních sítích v modelu nasazení Resource Manager prochází službou ExpressRoute, pokud jsou obě virtuální sítě propojené ke stejnému okruhu ExpressRoute.
 - Připojení mezi předplatnými je podporováno v modelu nasazení Classic i Resource Manager.

## Co není podporováno

Tato část popisuje, co není podporováno pro okruhy ExpressRoute:

 - Přesunutí propojení okruhu, bran a virtuálních sítí z model nasazení Classic do Resource Manager.
 - Správa životního cyklu okruhu ExpressRoute z modelu nasazení Classic.
 - Podpora řízení přístupu na základě role (RBAC) pro model nasazení Classic. Nejde provádět řízení RBAC na okruh v modelu nasazení Classic. Libovolný správce nebo spolusprávce předplatného může propojit virtuální sítě k okruhu nebo toto propojení zrušit.

## Konfigurace

Postupujte podle pokynů uvedených v tématu [Přesun okruhu ExpressRoute z modelu nasazení Classic do Resource Manager](expressroute-howto-move-arm.md).

## Další kroky

- Informace o pracovním postupu najdete v tématu [Pracovní postupy zřizování okruhů ExpressRoute a stavy okruhu](expressroute-workflows.md).
- Konfigurace připojení ExpressRoute:

    - [Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md)
    - [Konfigurace směrování](expressroute-howto-routing-arm.md)
    - [Propojení virtuální sítě k okruhu ExpressRoute](expressroute-howto-linkvnet-arm.md)



<!--HONumber=Aug16_HO4-->


