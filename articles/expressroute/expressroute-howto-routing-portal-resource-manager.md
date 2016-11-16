---
title: "Postup konfigurace směrování pro okruh ExpressRoute pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "Tento článek vás provede kroky pro vytváření a zřizování soukromého a veřejného partnerského vztahu a partnerského vztahu Microsoftu okruhu ExpressRoute. Tento článek také ukazuje, jak kontrolovat stav partnerských vztahů pro váš okruh, aktualizovat je nebo je odstranit."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 8c2a7ed2-ae5c-4e49-81f6-77cf9f2b2ac9
ms.service: expressroute
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 1d78bdb6cbb9870298b050207fd1fa9c5e71d674


---
# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Vytvoření a úprava směrování pro okruh ExpressRoute
> [!div class="op_single_selector"]
> [Azure Portal – Resource Manager](expressroute-howto-routing-portal-resource-manager.md)
> [PowerShell – Resource Manager](expressroute-howto-routing-arm.md)
> [PowerShell – Classic](expressroute-howto-routing-classic.md)
> 
> 

Tento článek vás provede kroky k vytvoření a správě konfigurace směrování pro okruhu ExpressRoute pomocí webu Azure Portal a modelu nasazení Resource Manager.

**O modelech nasazení Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a>Předpoklady konfigurace
* Před zahájením konfigurace se ujistěte, že jste si přečetli stránku s [předpoklady](expressroute-prerequisites.md), stránku s [požadavky směrování](expressroute-routing.md) a stránku s [pracovními postupy](expressroute-workflows.md).
* Musí mít aktivní okruh ExpressRoute. Než budete pokračovat, podle pokynů [vytvořte okruh ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho povolený vaším poskytovatelem připojení. Abyste mohli spouštět rutiny popsané dál, musí být okruh ExpressRoute zřízený a povolený.

Tyto pokyny platí jenom pro okruhy vytvořené poskytovateli služeb nabízejícími služby připojení vrstvy 2. Pokud používáte poskytovatele služeb nabízejícího spravované služby vrstvy 3 (obvykle IPVPN, např. MPLS), poskytovatel připojení provede konfiguraci a správu směrování za vás. 

> [!IMPORTANT]
> Aktuálně prostřednictvím portálu pro správu služeb neinzerujeme partnerské vztahy nakonfigurované poskytovateli služeb. Pracujeme na tom, aby tato možnost brzy fungovala. Před konfigurací partnerských vztahů protokolu BGP se poraďte se svým poskytovatelem služeb.
> 
> 

Můžete nakonfigurovat jeden, dva nebo všechny tři partnerské vztahy (soukromý Azure, veřejný Azure a Microsoft) pro okruh ExpressRoute. Partnerské vztahy můžete konfigurovat v libovolném pořadí. Musíte se ale přesvědčit, že jste vždy konfiguraci každého partnerského vztahu dokončili. 

## <a name="azure-private-peering"></a>Soukromý partnerský vztah Azure
Tato část obsahuje pokyny, jak vytvořit, získat, aktualizovat a odstranit konfiguraci soukromého partnerského vztahu Azure pro okruh ExpressRoute. 

### <a name="to-create-azure-private-peering"></a>Vytvoření soukromého partnerského vztahu Azure
1. Nakonfigurujte okruh ExpressRoute. Než budete pokračovat, ujistěte se, že je okruh poskytovatelem připojení plně zřízený.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Nakonfigurujte soukromý partnerský vztah Azure pro okruh. Před zahájením dalších kroků se ujistěte, že máte k dispozici následující položky:
   
   * Podsíť /30 pro primární propojení. Nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.
   * Podsíť /30 pro sekundární propojení. Nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.
   * Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit. Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.
   * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS. Pro tento partnerský vztah můžete použít soukromé číslo AS. Zkontrolujte, že nepoužíváte 65515.
   * Hodnota hash MD5, pokud se ji rozhodnete použít. **Tato položka je nepovinná.**
3. Vyberte řádek pro soukromý partnerský vztah Azure, jak je zobrazeno dál.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
4. Nakonfigurujte soukromý partnerský vztah. Následující obrázek ukazuje příklad konfigurace.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)
5. Po zadání všech parametrů uložte konfiguraci. Jakmile je konfigurace úspěšně přijatá, zobrazí se něco podobného jako v příkladu dál.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="to-view-azure-private-peering-details"></a>Zobrazení podrobností soukromého partnerského vztahu Azure
Vlastnosti soukromého partnerského vztahu Azure můžete zobrazit výběrem partnerského vztahu.

![](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="to-update-azure-private-peering-configuration"></a>Aktualizace konfigurace soukromého partnerského vztahu Azure
Můžete vybrat řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu. 

![](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### <a name="to-delete-azure-private-peering"></a>Odstranění soukromého partnerského vztahu Azure
Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění podle uvedeného obrázku.

![](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)

## <a name="azure-public-peering"></a>Veřejný partnerský vztah Azure
Tato část obsahuje pokyny, jak vytvořit, získat, aktualizovat a odstranit konfiguraci veřejného partnerského vztahu Azure pro okruh ExpressRoute. 

### <a name="to-create-azure-public-peering"></a>Vytvoření veřejného partnerského vztahu Azure
1. Nakonfigurujte okruh ExpressRoute. Než budete dál pokračovat, ujistěte se, že je okruh poskytovatelem připojení plně zřízený.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Nakonfigurujte veřejný partnerský vztah Azure pro okruh. Před zahájením dalších kroků se ujistěte, že máte k dispozici následující položky:
   
   * Podsíť /30 pro primární propojení. 
   * Podsíť /30 pro sekundární propojení. 
   * Všechny IP adresy použité k nastavení tohoto partnerského vztahu musí být platné IPv4 adresy.
   * Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit. Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.
   * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS.
   * Hodnota hash MD5, pokud se ji rozhodnete použít. **Tato položka je nepovinná.**
3. Vyberte řádek pro veřejný partnerský vztah Azure, jak je zobrazeno dál.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
4. Nakonfigurujte veřejný partnerský vztah. Následující obrázek ukazuje příklad konfigurace.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)
5. Po zadání všech parametrů uložte konfiguraci. Jakmile je konfigurace úspěšně přijatá, zobrazí se něco podobného jako v příkladu dál.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="to-view-azure-public-peering-details"></a>Zobrazení podrobností veřejného partnerského vztahu Azure
Vlastnosti veřejného partnerského vztahu Azure můžete zobrazit výběrem partnerského vztahu.

![](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="to-update-azure-public-peering-configuration"></a>Aktualizace konfigurace veřejného partnerského vztahu Azure
Můžete vybrat řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu. 

![](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### <a name="to-delete-azure-public-peering"></a>Odstranění veřejného partnerského vztahu Azure
Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění podle uvedeného obrázku.

![](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)

## <a name="microsoft-peering"></a>Partnerský vztah Microsoftu
Tato část obsahuje pokyny, jak vytvořit, získat, aktualizovat a odstranit konfiguraci partnerského vztahu Microsoftu pro okruh ExpressRoute. 

### <a name="to-create-microsoft-peering"></a>Vytvoření partnerského vztahu Microsoftu
1. Nakonfigurujte okruh ExpressRoute. Než budete dál pokračovat, ujistěte se, že je okruh poskytovatelem připojení plně zřízený.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Nakonfigurujte partnerský vztah Microsoftu pro okruh. Před pokračováním se ujistěte, že máte k dispozici následující informace.
   
   * Podsíť /30 pro primární propojení. Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.
   * Podsíť /30 pro sekundární propojení. Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.
   * Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit. Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.
   * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS.
   * **Inzerované předpony:** Musíte poskytnout seznam všech předpon, které plánujete inzerovat přes relaci protokolu BGP. Přijímají se jenom předpony veřejných IP adres. Pokud chcete odeslat sadu předpon, můžete odeslat seznam oddělený čárkami. Tyto předpony musí být v RIR/IRR zaregistrované na vás.
   * **Zákaznické číslo ASN:** Pokud inzerujete předpony, které nejsou registrované na číslo AS partnerského vztahu, můžete zadat číslo AS, na které jsou registrované. **Tato položka je nepovinná.**
   * **Název registru směrování:** Můžete zadat RIR/IRR, kde jsou předpony a číslo AS registrované. **Tato položka je nepovinná.**
   * Hodnota hash MD5, pokud se ji rozhodnete použít. **Tato položka je nepovinná.**
3. Můžete vybrat partnerský vztah, který chcete nakonfigurovat, jak je zobrazeno dál. Vyberte řádek partnerského vztahu Microsoftu.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
4. Nakonfigurujte partnerský vztah Microsoftu. Následující obrázek ukazuje příklad konfigurace.
   
   ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)
5. Po zadání všech parametrů uložte konfiguraci. 
   
    Pokud se váš okruh dostane do stavu potřeby ověření (jak je zobrazeno dál), musíte otevřít lístek podpory, abyste ukázali důkaz vlastnictví předpon našemu týmu podpory.    
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)

    Lístek podpory můžete otevřít přímo z portálu, jak je zobrazeno dál.     

    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


1. Jakmile je konfigurace úspěšně přijatá, zobrazí se něco podobného jako v příkladu dál.
   
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="to-view-microsoft-peering-details"></a>Zobrazení podrobností partnerského vztahu Microsoftu
Vlastnosti veřejného partnerského vztahu Azure můžete zobrazit výběrem partnerského vztahu.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)

### <a name="to-update-microsoft-peering-configuration"></a>Aktualizace konfigurace partnerského vztahu Microsoftu
Můžete vybrat řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu. 

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="to-delete-microsoft-peering"></a>Odstranění partnerského vztahu Microsoftu
Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění podle uvedeného obrázku.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)

## <a name="next-steps"></a>Další kroky
Dalším krokem je [Propojení virtuální sítě s okruhem ExpressRoute](expressroute-howto-linkvnet-arm.md).

* Další informace o pracovních postupech ExpressRoute najdete v tématu [Pracovní postupy ExpressRoute](expressroute-workflows.md).
* Další informace o partnerském vztahu okruhu najdete v tématu [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).
* Další informace o práci s virtuálními sítěmi najdete v článku [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md).




<!--HONumber=Nov16_HO2-->


