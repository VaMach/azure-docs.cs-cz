---
title: "Vytvoření virtuální sítě - 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Naučte se vytvořit virtuální síť pomocí Azure CLI 2.0."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 75966bcc-0056-4667-8482-6f08ca38e77a
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c7d7b3543f488aedff1ea2c68a2b497e0ca744af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-network-using-the-azure-cli-20"></a>Vytvoření virtuální sítě pomocí Azure CLI 2.0

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure nabízí dva modely nasazení: Azure Resource Manager a Classic. Microsoft doporučuje vytváření prostředků prostřednictvím modelu nasazení Resource Manager. Další informace o rozdílech mezi těmito dvěma modely najdete v článku [Vysvětlení modelů nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="cli-versions-to-complete-the-task"></a>Verze rozhraní příkazového řádku pro dokončení úlohy
K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:

- [Azure CLI 1.0](virtual-networks-create-vnet-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků
- [Azure CLI 2.0](#create-a-virtual-network) -naší nové generace rozhraní příkazového řádku pro správu model nasazení prostředku (v tomto článku).
 
    Virtuální síť můžete vytvořit také prostřednictvím modelu nasazení Resource Manager pomocí jiných nástrojů nebo prostřednictvím modelu nasazení Classic. Pokud to chcete provést, vyberte odpovídající možnost z následujícího seznamu:

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-networks-create-vnet-arm-pportal.md)
> * [PowerShell](virtual-networks-create-vnet-arm-ps.md)
> * [Rozhraní příkazového řádku](virtual-networks-create-vnet-arm-cli.md)
> * [Šablona](virtual-networks-create-vnet-arm-template-click.md)
> * [Portál (Classic)](virtual-networks-create-vnet-classic-pportal.md)
> * [PowerShell (Classic)](virtual-networks-create-vnet-classic-netcfg-ps.md)
> * [Rozhraní příkazového řádku (Classic)](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]


## <a name="create-a-virtual-network"></a>Vytvoření virtuální sítě

Pokud chcete vytvořit virtuální síť pomocí Azure CLI 2.0, proveďte následující kroky:

1. Instalace a konfigurace nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se k Azure účet pomocí [az přihlášení](/cli/azure/#login).

2. Vytvořte skupinu prostředků pro virtuální síť s použitím [vytvořit skupinu az](/cli/azure/group#create) s `--name` a `--location` argumenty:

    ```azurecli
    az group create --name TestRG --location centralus
    ```

3. Vytvoření virtuální sítě a podsítě:

    ```azurecli
    az network vnet create \
    --name TestVNet \
    --resource-group TestRG \
    --location centralus \
    --address-prefix 192.168.0.0/16 \
    --subnet-name FrontEnd \
    --subnet-prefix 192.168.1.0/24
    ```

    Očekávaný výstup:
    
    ```json
    {
        "newVNet": {
            "addressSpace": {
            "addressPrefixes": [
            "192.168.0.0/16"
            ]
            },
            "dhcpOptions": {
            "dnsServers": []
            },
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>",
            "subnets": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                "name": "FrontEnd",
                "properties": {
                "addressPrefix": "192.168.1.0/24",
                "provisioningState": "Succeeded"
                },
                "resourceGroup": "TestRG"
            }
            ]
            }
    }
    ```

    Použité parametry:

    - `--name TestVNet`: Název sítě VNet, který se má vytvořit.
    - `--resource-group TestRG`: # Název skupiny prostředků, které řídí prostředku. 
    - `--location centralus`: Umístění, do které chcete nasadit.
    - `--address-prefix 192.168.0.0/16`: Předponu adresy a bloku.  
    - `--subnet-name FrontEnd`: Název podsítě.
    - `--subnet-prefix 192.168.1.0/24`: Předponu adresy a bloku.

    Seznam základních informací pro použití v další příkaz, se můžete dotazovat pomocí virtuální sítě [filtr dotazu](/cli/azure/query-az-cli2):

    ```azurecli
    az network vnet list --query '[?name==`TestVNet`].{Where:location,Name:name,Group:resourceGroup}' -o table
    ```

    Které vytvoří následující výstup:

        Where      Name      Group

        centralus  TestVNet  TestRG

4. Vytvoření podsítě:

    ```azurecli
    az network vnet subnet create \
    --address-prefix 192.168.2.0/24 \
    --name BackEnd \
    --resource-group TestRG \
    --vnet-name TestVNet
    ```

    Očekávaný výstup:

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid> \"",
    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
    "ipConfigurations": null,
    "name": "BackEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "TestRG",
    "resourceNavigationLinks": null,
    "routeTable": null
    }
    ```

    Použité parametry:

    - `--address-prefix 192.168.2.0/24`: Blok CIDR podsítě.
    - `--name BackEnd`: Název nové podsítě.
    - `--resource-group TestRG`: Skupinu prostředků.
    - `--vnet-name TestVNet`: Název vlastnícím virtuální sítě.

5. Dotaz na vlastnosti nové sítě VNet:

    ```azurecli
    az network vnet show \
    -g TestRG \
    -n TestVNet \
    --query '{Name:name,Where:location,Group:resourceGroup,Status:provisioningState,SubnetCount:subnets | length(@)}' \
    -o table
    ```

    Očekávaný výstup:

        Name      Where      Group    Status       SubnetCount

        TestVNet  centralus  TestRG   Succeeded              2

6. Dotaz na vlastnosti podsítě:

    ```azurecli
    az network vnet subnet list \
    -g TestRG \
    --vnet-name testvnet \
    --query '[].{Name:name,CIDR:addressPrefix,Status:provisioningState}' \
    -o table
    ```

    Očekávaný výstup:

        Name      CIDR            Status

        FrontEnd  192.168.1.0/24  Succeeded
        BackEnd   192.168.2.0/24  Succeeded

## <a name="next-steps"></a>Další kroky

Zjistěte, jak připojit:

- Virtuální počítač (VM) k virtuální síti načtením [vytvoření virtuálního počítače s Linuxem](../virtual-machines/linux/quick-create-cli.md) článku. Místo vytváření virtuální sítě a podsítě v rámci kroků v těchto článcích můžete vybrat existující virtuální síť a podsíť, ke které se má virtuální počítač připojit.
- Virtuální síť k jiným virtuálním sítím pomocí informací v článku [Propojení virtuálních sítí](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).
- Virtuální síť k místní síti pomocí virtuální privátní sítě (VPN) typu Site-to-Site nebo okruhu ExpressRoute. Zjistěte, jak načtením [připojit virtuální síť k místní síti pomocí sítě site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) a [propojení virtuální sítě k okruhu ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).