---
title: "Postup konfigurace směrování (partnerský vztah) pro ExpressRoute okruh: Resource Manager: Azure | Microsoft Docs"
description: "Tento článek vás provede kroky pro vytváření a zřizování soukromého a veřejného partnerského vztahu a partnerského vztahu Microsoftu okruhu ExpressRoute. Tento článek také ukazuje, jak kontrolovat stav partnerských vztahů pro váš okruh, aktualizovat je nebo je odstranit."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8c2a7ed2-ae5c-4e49-81f6-77cf9f2b2ac9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: cherylmc
ms.openlocfilehash: 55ccadfea55b8098ee58dcaef942f6ba54093665
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a>Vytvářet a upravovat partnerský vztah pro okruh ExpressRoute

Tento článek pomůže při vytváření a správě konfigurace směrování pro okruh ExpressRoute v modelu nasazení Resource Manager pomocí portálu Azure. Můžete také zkontrolovat stav, aktualizace nebo odstranění a zrušení zřízení partnerských vztahů pro okruh ExpressRoute. Pokud chcete použít jinou metodu pro práci se váš okruh, vyberte článek z následujícího seznamu:


## <a name="configuration-prerequisites"></a>Předpoklady konfigurace

* Před zahájením konfigurace se ujistěte, že jste si přečetli stránku s [předpoklady](expressroute-prerequisites.md), stránku s [požadavky směrování](expressroute-routing.md) a stránku s [pracovními postupy](expressroute-workflows.md).
* Musí mít aktivní okruh ExpressRoute. Než budete pokračovat, podle pokynů [vytvořte okruh ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) a mějte ho povolený vaším poskytovatelem připojení. Okruh ExpressRoute musí být v zřízený a povolený stavu, abyste mohli spustit rutiny v následujících částech.
* Pokud budete chtít použít sdílené hodnotu hash klíče s algoritmem MD5, je nutné použít na obou stranách tunelového propojení a omezit počet znaků, který má být delší než 25.

Tyto pokyny platí jenom pro okruhy vytvořené poskytovateli služeb nabízejícími služby připojení vrstvy 2. Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle IPVPN, např. MPLS), poskytovatel připojení konfiguruje a spravuje směrování. 

> [!IMPORTANT]
> Aktuálně prostřednictvím portálu pro správu služeb neinzerujeme partnerské vztahy nakonfigurované poskytovateli služeb. Pracujeme na tom, aby tato možnost brzy fungovala. Před konfigurací partnerských vztahů protokolu BGP, zkontrolujte u vašeho poskytovatele služeb.
> 
> 

Můžete nakonfigurovat jeden, dva nebo všechny tři partnerské vztahy (soukromý Azure, veřejný Azure a Microsoft) pro okruh ExpressRoute. Partnerské vztahy můžete konfigurovat v libovolném pořadí. Musíte se ale přesvědčit, že jste vždy konfiguraci každého partnerského vztahu dokončili.

## <a name="azure-private-peering"></a>Soukromý partnerský vztah Azure

Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit Azure konfigurace soukromého partnerského vztahu pro okruh ExpressRoute.

### <a name="to-create-azure-private-peering"></a>Vytvoření soukromého partnerského vztahu Azure

1. Nakonfigurujte okruh ExpressRoute. Než budete pokračovat, ujistěte se, že je okruh poskytovatelem připojení plně zřízený.

  ![seznam](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Nakonfigurujte soukromý partnerský vztah Azure pro okruh. Před zahájením dalších kroků se ujistěte, že máte k dispozici následující položky:

  * Podsíť /30 pro primární propojení. Podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.
  * Podsíť /30 pro sekundární propojení. Podsítě nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.
  * Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit. Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.
  * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS. Pro tento partnerský vztah můžete použít soukromé číslo AS. Zkontrolujte, že nepoužíváte 65515.
  * **Volitelné -** algoritmus hash MD5, pokud se rozhodnete použít.
3. Vyberte řádek Azure privátní partnerský vztah, jak je znázorněno v následujícím příkladu:

  ![Privátní](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
4. Nakonfigurujte soukromý partnerský vztah. Následující obrázek ukazuje příklad konfigurace:

  ![Nakonfigurujte soukromý partnerský vztah](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)
5. Po zadání všech parametrů uložte konfiguraci. Po konfiguraci úspěšně přijatá, zobrazí se o něco podobného jako v následujícím příkladu:

  ![Uložit soukromého partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="to-view-azure-private-peering-details"></a>Zobrazení podrobností soukromého partnerského vztahu Azure

Vlastnosti soukromého partnerského vztahu Azure můžete zobrazit výběrem partnerského vztahu.

![Zobrazit soukromého partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="to-update-azure-private-peering-configuration"></a>Aktualizace konfigurace soukromého partnerského vztahu Azure

Můžete vybrat řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu.

![Aktualizovat soukromého partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### <a name="to-delete-azure-private-peering"></a>Odstranění soukromého partnerského vztahu Azure

Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění, jak je znázorněno na následujícím obrázku:

![Odstranění soukromého partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)

## <a name="azure-public-peering"></a>Veřejný partnerský vztah Azure

Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit veřejného partnerského vztahu konfiguraci Azure pro okruh ExpressRoute.

### <a name="to-create-azure-public-peering"></a>Vytvoření veřejného partnerského vztahu Azure

1. Nakonfigurujte okruh ExpressRoute. Než budete dál pokračovat, ujistěte se, že je okruh poskytovatelem připojení plně zřízený.

  ![seznam veřejného partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Nakonfigurujte veřejný partnerský vztah Azure pro okruh. Před zahájením dalších kroků se ujistěte, že máte k dispozici následující položky:

  * Podsíť /30 pro primární propojení. Musí se jednat o platnou předponu veřejné IPv4 adresy.
  * Podsíť /30 pro sekundární propojení. Musí se jednat o platnou předponu veřejné IPv4 adresy.
  * Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit. Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.
  * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS.
  * **Volitelné -** algoritmus hash MD5, pokud se rozhodnete použít.
3. Azure veřejného partnerského vztahu řádek, vyberte, jak je znázorněno na následujícím obrázku:

  ![Vyberte řádek pro veřejný partnerský vztah](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
4. Nakonfigurujte veřejný partnerský vztah. Následující obrázek ukazuje příklad konfigurace:

  ![Nakonfigurujte veřejný partnerský vztah](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)
5. Po zadání všech parametrů uložte konfiguraci. Po konfiguraci úspěšně přijatá, zobrazí se o něco podobného jako v následujícím příkladu:

  ![Uložte konfiguraci veřejného partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="to-view-azure-public-peering-details"></a>Zobrazení podrobností veřejného partnerského vztahu Azure

Vlastnosti veřejného partnerského vztahu Azure můžete zobrazit výběrem partnerského vztahu.

![Zobrazit vlastnosti veřejného partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="to-update-azure-public-peering-configuration"></a>Aktualizace konfigurace veřejného partnerského vztahu Azure

Můžete vybrat řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu.

![Vyberte řádek pro veřejný partnerský vztah](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### <a name="to-delete-azure-public-peering"></a>Odstranění veřejného partnerského vztahu Azure

Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění, jak je znázorněno v následujícím příkladu:

![odstranění veřejného partnerského vztahu](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)

## <a name="microsoft-peering"></a>Partnerský vztah Microsoftu

Tato část vám umožňuje vytvořit, získat, aktualizovat a odstranit konfiguraci partnerského vztahu Microsoftu pro okruh ExpressRoute.

> [!IMPORTANT]
> Okruhy ExpressRoute, které byly nakonfigurovány před 1. srpna 2017 partnerského vztahu Microsoftu bude mít všechny služby předpony inzerované prostřednictvím Microsoft partnerský vztah, i když nejsou definovány filtry tras. Okruhy ExpressRoute, které jsou nakonfigurované na nebo po 1 srpen 2017 partnerského vztahu Microsoftu nebude mít všechny předpony inzerované dokud trasy filtr je připojen k okruhu. Další informace najdete v tématu [Konfigurovat filtr trasy pro partnerský vztah Microsoftu](how-to-routefilter-powershell.md).
> 
> 

### <a name="to-create-microsoft-peering"></a>Vytvoření partnerského vztahu Microsoftu

1. Nakonfigurujte okruh ExpressRoute. Než budete dál pokračovat, ujistěte se, že je okruh poskytovatelem připojení plně zřízený.

  ![seznam partnerského vztahu Microsoftu](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Nakonfigurujte partnerský vztah Microsoftu pro okruh. Před pokračováním se ujistěte, že máte k dispozici následující informace.

  * Podsíť /30 pro primární propojení. Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.
  * Podsíť /30 pro sekundární propojení. Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.
  * Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit. Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.
  * Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS.
  * Inzerované předpony: Musíte poskytnout seznam všech předpon, které plánujete inzerovat přes relaci protokolu BGP. Přijímají se jenom předpony veřejných IP adres. Pokud chcete odeslat sadu předpon, můžete odeslat seznam oddělený čárkami. Tyto předpony musí být v RIR/IRR zaregistrované na vás.
  * **Volitelné -** zákaznické číslo ASN: Pokud inzerujete předpony, které nejsou registrované na číslo AS partnerského vztahu, můžete zadat číslo AS, do kterého jsou registrované.
  * Název registru směrování: Můžete zadat RIR/IRR, kde jsou předpony a číslo AS registrované.
  * **Volitelné -** algoritmus hash MD5, pokud se rozhodnete použít.
3. Můžete vybrat partnerský vztah, který chcete nakonfigurovat, jak je znázorněno v následujícím příkladu. Vyberte řádek partnerského vztahu Microsoftu.

  ![Vyberte řádek partnerského vztahu Microsoftu](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
4. Nakonfigurujte partnerský vztah Microsoftu. Následující obrázek ukazuje příklad konfigurace:

  ![Nakonfigurujte partnerský vztah Microsoftu](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)
5. Po zadání všech parametrů uložte konfiguraci.

  Pokud váš okruh dostane do 'Ověřování potřeby' stavu (jak je znázorněno na obrázku), musíte otevřít lístek podpory, abyste ukázali důkaz vlastnictví předpon našemu týmu podpory.

  ![Uložte konfiguraci partnerského vztahu Microsoftu](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)

  Lístek podpory můžete otevřít přímo z portálu, jak je znázorněno v následujícím příkladu:

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


1. Po konfiguraci úspěšně přijatá, zobrazí se o něco podobného jako na následujícím obrázku:

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="to-view-microsoft-peering-details"></a>Zobrazení podrobností partnerského vztahu Microsoftu

Vlastnosti veřejného partnerského vztahu Azure můžete zobrazit výběrem partnerského vztahu.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)

### <a name="to-update-microsoft-peering-configuration"></a>Aktualizace konfigurace partnerského vztahu Microsoftu

Můžete vybrat řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="to-delete-microsoft-peering"></a>Odstranění partnerského vztahu Microsoftu

Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění, jak je znázorněno na následujícím obrázku:

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)

## <a name="next-steps"></a>Další kroky

Dalším krokem je [propojení virtuální sítě k okruhu ExpressRoute](expressroute-howto-linkvnet-portal-resource-manager.md)
* Další informace o pracovních postupech ExpressRoute najdete v tématu [Pracovní postupy ExpressRoute](expressroute-workflows.md).
* Další informace o partnerském vztahu okruhu najdete v tématu [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).
* Další informace o práci s virtuálními sítěmi najdete v článku [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md).
