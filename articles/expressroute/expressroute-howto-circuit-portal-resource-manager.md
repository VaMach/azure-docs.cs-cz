---
title: "Vytvoření a úprava okruhu ExpressRoute: portálu Azure | Microsoft Docs"
description: "Tento článek popisuje, jak vytvořit, zřízení, ověřte, aktualizovat, odstranit a zrušit jejich zřízení okruhu ExpressRoute."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 68d59d59-ed4d-482f-9cbc-534ebb090613
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/07/2017
ms.author: cherylmc;ganesr
ms.openlocfilehash: e3721cd3c031622788f553e71a6555b844f3f7dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-an-expressroute-circuit"></a>Vytvoření a úprava okruhu ExpressRoute
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video – portál Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (Classic)](expressroute-howto-circuit-classic.md)
>

Tento článek popisuje postup vytvoření okruhu Azure ExpressRoute pomocí webu Azure portal a modelu nasazení Azure Resource Manager. Následující kroky také ukazují zkontrolujte stav okruhu, aktualizaci, nebo odstranit a zrušit jejich zřízení se.


## <a name="before-you-begin"></a>Než začnete
* Zkontrolujte [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.
* Zajistěte, abyste měli přístup k [portál Azure](https://portal.azure.com).
* Ujistěte se, zda máte oprávnění k vytvoření nových síťových prostředků. Pokud nemáte správná oprávnění, obraťte se na správce účtu.
* Můžete [zobrazit video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) před zahájením pro lepší pochopení kroků.

## <a name="create-and-provision-an-expressroute-circuit"></a>Vytvořit a zřídit okruhu ExpressRoute
### <a name="1-sign-in-to-the-azure-portal"></a>1. Přihlášení k webu Azure Portal
V prohlížeči přejděte na web [Azure Portal](http://portal.azure.com) a přihlaste se pomocí svého účtu Azure.

### <a name="2-create-a-new-expressroute-circuit"></a>2. Vytvořit nový okruh ExpressRoute.
> [!IMPORTANT]
> Váš okruh ExpressRoute bude účtován od okamžiku, kdy se objeví klíč služby. Ujistěte se, při provádění této operace, pokud poskytovatel připojení je připraven ke zřízení okruhu.
> 
> 

1. Okruh ExpressRoute můžete vytvořit tak, že vyberete možnost vytvořit nový prostředek. Klikněte na tlačítko **nový** > **sítě** > **ExpressRoute**, jak je znázorněno na následujícím obrázku:
   
    ![Vytvoření okruhu ExpressRoute](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit1.png)
2. Po kliknutí na tlačítko **ExpressRoute**, uvidíte **okruh ExpressRoute vytvořit** okno. Když jste vyplnění hodnoty v tomto okně, ujistěte se, že jste zadali správné úrovně SKU a data měření.
   
   * **Úroveň** Určuje, zda je povoleno ExpressRoute standard nebo doplněk ExpressRoute premium. Můžete zadat **standardní** získat standardní SKU nebo **Premium** pro doplněk premium.
   * **Měření dat** Určuje typ fakturace. Můžete zadat **Metered** pro plán měření podle objemu dat a **neomezený** pro plán neomezená data na úrovni. Můžete změnit typ fakturace z **Metered** k **neomezený**, ale nemůžete změnit typ z **neomezený** k **Metered**.
     
     ![Konfigurace SKU vrstvy a měření dat.](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit2.png)

> [!IMPORTANT]
> Upozorňujeme, že označuje umístění partnerského vztahu [fyzické umístění](expressroute-locations.md) kde jste partnerského vztahu se společností Microsoft. Toto je **není** propojené s vlastností "Umístění", která odkazuje na geography, kde se nachází na poskytovatele síťové prostředků Azure. Při nesouvisí, je vhodné vybrat poskytovatele síťových prostředků geograficky v blízkosti umístění partnerského vztahu okruhu. 
> 
> 

### <a name="3-view-the-circuits-and-properties"></a>3. Zobrazit okruhy a vlastnosti
**Zobrazit všechny okruhy**

Můžete zobrazit všechny okruhů, které jste vytvořili výběrem **všechny prostředky** v levé nabídce.

![Okruhy zobrazení](./media/expressroute-howto-circuit-portal-resource-manager/listresource.png)

**Zobrazit vlastnosti**

    You can view the properties of the circuit by selecting it. On this blade, note the service key for the circuit. You must copy the circuit key for your circuit and pass it down to the service provider to complete the provisioning process. The circuit key is specific to your circuit.

![Zobrazení vlastností](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

### <a name="4-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>4. Klíč služby poslat svého poskytovatele připojení pro zřizování
V tomto okně **stav zprostředkovatele** poskytuje informace o aktuálním stavu zřizování na straně poskytovatele služeb. **Stav okruhu** poskytuje stav na straně společnosti Microsoft. Další informace o zřizování stavy okruhu najdete v tématu [pracovních](expressroute-workflows.md#expressroute-circuit-provisioning-states) článku.

Když vytvoříte nový okruh ExpressRoute, okruhu bude v následujícím stavu:

Stav zprostředkovatele: není zajišťováno<BR>
Stav okruhu: povoleno

![Zahájení procesu zřizování](./media/expressroute-howto-circuit-portal-resource-manager/viewstatus.png)

Okruh se změní na následující stav, když probíhá povolení můžete poskytovatele připojení:

Stav zprostředkovatele: zřizování<BR>
Stav okruhu: povoleno

Abyste mohli používat okruh ExpressRoute musí být v následujícím stavu:

Stav zprostředkovatele: zřídit<BR>
Stav okruhu: povoleno

### <a name="5-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>5. Pravidelně kontrolovat stav a stav okruhu klíče
Můžete zobrazit vlastnosti okruhu, který máte zájem výběrem. Zkontrolujte **stav zprostředkovatele** a ujistěte se, že se přesunul na **zajištěno** než budete pokračovat.

![Stav okruhu a zprostředkovatele](./media/expressroute-howto-circuit-portal-resource-manager/viewstatusprovisioned.png)

### <a name="6-create-your-routing-configuration"></a>6. Vytvořte vlastní konfiguraci směrování
Podrobné pokyny najdete v části [konfigurace směrování pro okruh ExpressRoute](expressroute-howto-routing-portal-resource-manager.md) článek k vytvoření a úpravám partnerských vztahů pro okruh.

> [!IMPORTANT]
> Tyto pokyny platí jenom pro okruhy vytvořené poskytovateli služeb, které nabízejí vrstvy 2 připojení služby. Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle virtuální privátní síť IP, např. MPLS), poskytovatel připojení nakonfigurujete a správu směrování za vás.
> 
> 

### <a name="7-link-a-virtual-network-to-an-expressroute-circuit"></a>7. Propojení virtuální sítě k okruhu ExpressRoute
V dalším kroku propojení virtuální sítě k okruhu ExpressRoute. Použití [propojování virtuálních sítí pro okruhy ExpressRoute](expressroute-howto-linkvnet-arm.md) článek při práci s modelem nasazení Resource Manager.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Získávání stav okruhu ExpressRoute
Výběrem můžete zobrazit stav okruhu. 

![Stav okruhu ExpressRoute](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

## <a name="modifying-an-expressroute-circuit"></a>Úprava okruhu ExpressRoute
Bez dopadu na připojení můžete upravit některé vlastnosti okruhu ExpressRoute.

Můžete provést následující bez výpadků:

* Povolit nebo zakázat doplněk ExpressRoute premium pro váš okruh ExpressRoute.
* Zadaný na tomto portu je dostupná kapacita, zvětšete šířku pásma okruhu ExpressRoute. Všimněte si, že šířku pásma okruhu přechod na starší verzi není podporován. 
* Změňte plán měření z – měření podle objemu dat na neomezená Data na úrovni. Všimněte si, že změna měření plánu z neomezená Data – měření podle objemu dat není podporován.
* Můžete povolit nebo zakázat *povolit klasické operace*.

Další informace o omezení a omezení, najdete v části [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).

Chcete-li upravit okruh ExpressRoute, klikněte na **konfigurace** jak je znázorněno na obrázku níže.

![Úprava okruhu](./media/expressroute-howto-circuit-portal-resource-manager/modifycircuit.png)

Můžete upravit šířku pásma, SKU, model fakturace a povolit klasické operace v rámci konfigurace okna.

> [!IMPORTANT]
> Možná budete muset znovu vytvořit okruh ExpressRoute, pokud je nedostatečné kapacity na existující port. Pokud v tomto umístění není k dispozici žádné další kapacitu, nelze upgradovat okruh.
>
> Nejde snížit šířku pásma okruhu ExpressRoute bez přerušení. Přechod na starší verzi šířky pásma vyžaduje zrušení zřízení okruh ExpressRoute a pak znova nezajistíte nové okruh ExpressRoute.
> 
> Zakázat doplněk premium operace může selhat, pokud používáte prostředky, které jsou větší než co je povoleno pro standardní okruh.
> 
> 

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Zrušení zřízení a odstraňování okruhu ExpressRoute
Váš okruh ExpressRoute můžete odstranit výběrem **odstranit** ikonu. Je třeba počítat s následujícím:

* Je nutné zrušit všechny virtuální sítě od okruhu ExpressRoute. Pokud se tato operace nezdaří, zkontrolujte, zda jsou všechny virtuální sítě propojené ke okruh.
* Pokud je poskytovatel služby okruh ExpressRoute Stav zřizování **zřizování** nebo **zajištěno** , musíte pracovat se svým poskytovatelem služeb pro zrušení zřízení okruhu na jejich straně. Budeme nadále rezervovat prostředky a dokud poskytovatele služeb dokončení zrušení okruhu a upozorní nám vám účtovat.
* Pokud má poskytovatel služeb zrušit okruhu (poskytovatele služeb Stav zřizování je nastavena na **není zajišťováno**) pak můžete odstranit okruh. Tato akce ukončí fakturace pro okruh

## <a name="next-steps"></a>Další kroky
Po vytvoření okruhu, ujistěte se, že provedete následující kroky:

* [Vytvoření a úprava směrování pro okruhu ExpressRoute](expressroute-howto-routing-portal-resource-manager.md)
* [Propojení virtuální sítě k okruhu ExpressRoute](expressroute-howto-linkvnet-arm.md)

