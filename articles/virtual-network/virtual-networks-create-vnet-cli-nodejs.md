---
title: "Vytvoření virtuální sítě pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Naučte se vytvořit virtuální síť pomocí Azure CLI 1.0 | Správce prostředků."
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: jdial
ms.openlocfilehash: f0649c5c8c04dda72d2f147601efb37217f9bade
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-network-using-the-azure-cli"></a>Vytvoření virtuální sítě pomocí rozhraní příkazového řádku Azure

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure nabízí dva modely nasazení: Azure Resource Manager a Classic. Microsoft doporučuje vytváření prostředků prostřednictvím modelu nasazení Resource Manager. Další informace o rozdílech mezi těmito dvěma modely najdete v článku [Vysvětlení modelů nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="cli-versions-to-complete-the-task"></a>Verze rozhraní příkazového řádku pro dokončení úlohy
K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:

- [Azure CLI 2.0](virtual-networks-create-vnet-arm-cli.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků
- [Azure CLI 1.0](#create-a-virtual-network) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)

 
[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a>Vytvoření virtuální sítě

Pokud chcete vytvořit virtuální síť pomocí rozhraní příkazového řádku Azure, proveďte následující kroky:

1. Instalace a konfigurace rozhraní příkazového řádku Azure pomocí následujících kroků v [instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md) článku.

2. Vytvoření virtuální sítě a podsítě:

    ```azurecli
    azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    ```

    Očekávaný výstup:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    Použité parametry:

   * **--vnet**. Název sítě VNet, která se má vytvořit. V našem scénáři je to *TestVNet*.
   * **-e (nebo--adresní prostor)**. Adresní prostor sítě VNet. Pro náš scénář *192.168.0.0*
   * **-i (nebo - cidr)**. Maska sítě ve formátu CIDR. Pro náš scénář *16*.
   * **-n (nebo--název podsítě**). Název první podsíť. V našem scénáři je to *FrontEnd*.
   * **-p (nebo--IP adresu podsítě start)**. Počáteční IP adresa pro podsíť nebo adresního prostoru podsítě. Pro náš scénář *192.168.1.0*.
   * **-r (nebo--podsítě cidr)**. Maska sítě ve formátu CIDR podsítě. Pro náš scénář *24*.
   * **-l (nebo --location)**. Oblast Azure, kde se má vytvořit síť VNet. Pro náš scénář *střed USA*.

3. Vytvoření podsítě:

    ```azurecli
    azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    ```
   
    Očekávaný výstup:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    Použité parametry:

   * **-t (nebo--vnet-name**. Název sítě VNet, ve které se vytvoří podsíť. V našem scénáři je to *TestVNet*.
   * **-n (nebo --name)**. Název nové podsítě. Pro náš scénář *back-end*.
   * **-a (nebo --address-prefixes)**. Blok CIDR podsítě. Čtyři našem scénáři *192.168.2.0/24*.
   
4. Chcete-li zobrazit vlastnosti nové sítě VNet:

    ```azurecli
    azure network vnet show
    ```
   
    Očekávaný výstup:
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

## <a name="next-steps"></a>Další kroky

Zjistěte, jak připojit:

- Virtuální počítač (VM) k virtuální síti načtením [vytvoření virtuálního počítače s Linuxem](../virtual-machines/linux/quick-create-cli.md) článku. Místo vytváření virtuální sítě a podsítě v rámci kroků v těchto článcích můžete vybrat existující virtuální síť a podsíť, ke které se má virtuální počítač připojit.
- Virtuální síť k jiným virtuálním sítím pomocí informací v článku [Propojení virtuálních sítí](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).
- Virtuální síť k místní síti pomocí virtuální privátní sítě (VPN) typu Site-to-Site nebo okruhu ExpressRoute. Zjistěte, jak načtením [připojit virtuální síť k místní síti pomocí sítě site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) a [propojení virtuální sítě k okruhu ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).