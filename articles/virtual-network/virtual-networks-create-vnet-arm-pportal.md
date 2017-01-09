---
title: "Vytvoření virtuální sítě pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvořit virtuální síť pomocí webu Azure Portal | Resource Manager."
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 4ad679a4-a959-4e48-a317-d9f5655a442b
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/8/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: ba7a67b8ae57da165f45bd3552a3dfac5f4ef64b
ms.openlocfilehash: c7257faad9a41174fb1d65e04c99cd96a8af3ea9


---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Vytvoření virtuální sítě pomocí Portálu Azure

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure nabízí dva modely nasazení: Azure Resource Manager a Classic. Microsoft doporučuje vytváření prostředků prostřednictvím modelu nasazení Resource Manager. Další informace o rozdílech mezi těmito dvěma modely najdete v článku [Vysvětlení modelů nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md).
 
Tento článek vysvětluje, jak vytvořit virtuální síť prostřednictvím modelu nasazení Resource Manager pomocí webu Azure Portal. Virtuální síť můžete vytvořit také prostřednictvím modelu nasazení Resource Manager pomocí jiných nástrojů nebo prostřednictvím modelu nasazení Classic. Pokud to chcete provést, vyberte odpovídající možnost z následujícího seznamu:

> [!div class="op_single_selector"]
- [Azure Portal](virtual-networks-create-vnet-arm-pportal.md)
- [PowerShell](virtual-networks-create-vnet-arm-ps.md)
- [Rozhraní příkazového řádku](virtual-networks-create-vnet-arm-cli.md)
- [Šablona](virtual-networks-create-vnet-arm-template-click.md)
- [Portál (Classic)](virtual-networks-create-vnet-classic-pportal.md)
- [PowerShell (Classic)](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [Rozhraní příkazového řádku (Classic)](virtual-networks-create-vnet-classic-cli.md)


[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a>Vytvoření virtuální sítě

Pokud chcete vytvořit virtuální síť pomocí webu Azure Portal, dokončete následující kroky:

1. V prohlížeči přejděte na http://portal.azure.com a v případě potřeby se přihlaste pomocí účtu Azure.
2. Klikněte na **Nové** > **Sítě** > **Virtuální síť**, jak je znázorněno na následujícím obrázku:

    ![Nová virtuální síť](./media/virtual-network-create-vnet-arm-pportal/1.png)

3. V okně **Virtuální síť**, které se zobrazí, se ujistěte, že je vybraná možnost *Resource Manager*, a klikněte na **Vytvořit**, jak je znázorněno na následujícím obrázku:

    ![Virtual Network](./media/virtual-network-create-vnet-arm-pportal/2.png)
    
4. V okně **Vytvořit virtuální síť**, které se zobrazilo, zadejte *TestVNet* do pole **Název**, *192.168.0.0/16* do pole **Adresní prostor**, *FrontEnd* do pole **Název podsítě**, *192.168.1.0/24* do pole **Rozsah adres podsítě**, *TestRG* do pole **Skupina prostředků**, vyberte své **Předplatné** a **Umístění** a klikněte na tlačítko **Vytvořit**, jak je znázorněno na následujícím obrázku:

    ![Vytvoření virtuální sítě](./media/virtual-network-create-vnet-arm-pportal/3.png)

    Nebo můžete vybrat existující skupinu prostředků. Další informace o skupinách prostředků najdete v článku [Přehled Resource Manageru](../azure-resource-manager/resource-group-overview.md#resource-groups). Můžete také vybrat jiné umístění. Další informace o umístěních a oblastech Azure najdete v článku [Oblasti Azure](https://azure.microsoft.com/regions).

5. Portál umožňuje při vytváření virtuální sítě vytvořit pouze jednu podsíť. Pro tento scénář je třeba po vytvoření virtuální sítě vytvořit druhou podsíť. Druhou podsíť vytvoříte kliknutím na **Všechny prostředky** a pak kliknutím na **TestVNet** v okně **Všechny prostředky**, jak je znázorněno na následujícím obrázku:

    ![Vytvořená virtuální síť](./media/virtual-network-create-vnet-arm-pportal/4.png)

6. V okně **TestVNet**, které se zobrazí, klikněte na **Podsíť**, pak na **+Podsíť**, v okně **Přidat podsíť** zadejte *BackEnd* do pole **Název**, *192.168.2.0/24* do pole **Rozsah adres** a pak klikněte na **OK**, jak je znázorněno na následujícím obrázku:

    ![Přidání podsítě](./media/virtual-network-create-vnet-arm-pportal/5.png)

7. V seznamu jsou uvedené dvě podsítě, jak je znázorněno na následujícím obrázku:
    
    ![Seznam podsítí v síti VNet](./media/virtual-network-create-vnet-arm-pportal/6.png)

Tento článek vysvětlil, jak vytvořit virtuální síť se dvěma podsítěmi pro účely testování. Před vytvářením virtuální sítě pro produkční použití doporučujeme, abyste si přečetli články [Přehled virtuální sítě](virtual-networks-overview.md) a [Plánování a návrh virtuální sítě](virtual-network-vnet-plan-design-arm.md) pro úplné pochopení virtuálních sítí a všech nastavení. 

## <a name="next-steps"></a>Další kroky

Zjistěte, jak připojit:

- Virtuální počítač k virtuální síti pomocí informací v článcích [Vytvoření virtuálního počítače s Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md) nebo [Vytvoření virtuálního počítače s Linuxem](../virtual-machines/virtual-machines-linux-quick-create-portal.md). Místo vytváření virtuální sítě a podsítě v rámci kroků v těchto článcích můžete vybrat existující virtuální síť a podsíť, ke které se má virtuální počítač připojit.
- Virtuální síť k jiným virtuálním sítím pomocí informací v článku [Propojení virtuálních sítí](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).
- Virtuální síť k místní síti pomocí virtuální privátní sítě (VPN) typu Site-to-Site nebo okruhu ExpressRoute. Informace najdete v článcích [Připojení virtuální sítě k místní síti pomocí sítě VPN typu Site-to-Site](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) a [Propojení virtuální sítě s okruhem ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).


<!--HONumber=Jan17_HO1-->


