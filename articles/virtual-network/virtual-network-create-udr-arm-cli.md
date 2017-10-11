---
title: "Řídit směrování a virtuální zařízení pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Zjistěte, jak řídit směrování a virtuální zařízení pomocí Azure CLI 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5452a0b8-21a6-4699-8d6a-e2d8faf32c25
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/12/2017
ms.author: jdial
ms.openlocfilehash: e5d9519998346619093f443b740c8904283f76e8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-user-defined-routes-udr-using-the-azure-cli-20"></a>Vytvoření trasy definované uživatelem (UDR) pomocí Azure CLI 2.0

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure CLI](virtual-network-create-udr-arm-cli.md)
> * [Šablona](virtual-network-create-udr-arm-template.md)
> * [Prostředí PowerShell (nasazení Classic)](virtual-network-create-udr-classic-ps.md)
> * [Rozhraní příkazového řádku (nasazení Classic)](virtual-network-create-udr-classic-cli.md)

## <a name="cli-versions-to-complete-the-task"></a>Verze rozhraní příkazového řádku pro dokončení úlohy 

K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku: 

- [Azure CLI 1.0](virtual-network-create-udr-arm-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků 
- [Azure CLI 2.0](#Create-the-UDR-for-the-front-end-subnet) -naší nové generace rozhraní příkazového řádku pro správu model nasazení prostředku (v tomto článku)

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Níže uvedené příkazy rozhraní příkazového řádku Azure ukázka očekávat jednoduché prostředí již vytvořeny podle výše uvedené scénáře. Pokud chcete ke spuštění příkazů, jak jsou zobrazeny v tomto dokumentu, nasazením nejprve vytvořit testovací prostředí [této šablony](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klikněte na tlačítko **nasadit do Azure**, nahradí výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů v portálu.


## <a name="create-the-udr-for-the-front-end-subnet"></a>Vytvoření UDR front-end podsítě
Pokud chcete vytvořit směrovací tabulku a směrování, které jsou potřebné pro podsítě front end závislosti na scénáři výše, postupujte podle následujících kroků.

1. Vytvořit směrovací tabulku pro front-endu podsíť s [vytvoření tabulky trasy sítě az](/cli/azure/network/route-table#create) příkaz:

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --location centralus \
    --name UDR-FrontEnd
    ```

    Výstup:

    ```json
    {
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
    "location": "centralus",
    "name": "UDR-FrontEnd",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "routes": [],
    "subnets": null,
    "tags": null,
    "type": "Microsoft.Network/routeTables"
    }
    ```

2. Vytvořit trasu, která odesílá veškerý provoz, jehož k podsíti back-end (192.168.2.0/24) **FW1** virtuální počítač (192.168.0.4) pomocí [az síťovou směrovací tabulku trasu vytvořit](/cli/azure/network/route-table/route#create) příkaz:

    ```azurecli 
    az network route-table route create \
    --resource-group testrg \
    --name RouteToBackEnd \
    --route-table-name UDR-FrontEnd \
    --address-prefix 192.168.2.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

    Výstup:

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd/routes/RouteToBackEnd",
    "name": "RouteToBackEnd",
    "nextHopIpAddress": "192.168.0.4",
    "nextHopType": "VirtualAppliance",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg"
    }
    ```
    Parametry:

    * **– Název směrovací tabulky**. Název směrovací tabulka, kam bude přidána trasy. Pro náš scénář *UDR front-endu*.
    * **--address prefixes**. Předpona adresy podsítě, kde jsou pakety určené do. Pro náš scénář *192.168.2.0/24*.
    * **--Další typ směrování**. Typ objektu provozu se odešle do. Možné hodnoty jsou *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, nebo *žádné*.
    * **--Další směrování ip adresu**. IP adresa dalšího směrování. Pro náš scénář *192.168.0.4*.

3. Spustit [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update) příkaz přidružit směrovací tabulka vytvořili výše s **front-endu** podsítě:

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name FrontEnd \
    --route-table UDR-FrontEnd
    ```

    Výstup:

    ```json
    {
    "addressPrefix": "192.168.1.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/FrontEnd",
    "ipConfigurations": null,
    "name": "FrontEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "resourceNavigationLinks": null,
    "routeTable": {
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
        "location": null,
        "name": null,
        "provisioningState": null,
        "resourceGroup": "testrg",
        "routes": null,
        "subnets": null,
        "tags": null,
        "type": null
        }
    }
    ```

    Parametry:
    
    * **--vnet-name**. Název sítě VNet, kde je umístěný v podsíti. V našem scénáři je to *TestVNet*.

## <a name="create-the-udr-for-the-back-end-subnet"></a>Vytvoření UDR pro podsíť back-end

Pokud chcete vytvořit směrovací tabulku a směrování, které jsou potřeba pro back-end podsíť závislosti na scénáři výše, proveďte následující kroky:

1. Spusťte následující příkaz a vytvořte tabulku směrování pro podsíť back-end:

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --name UDR-BackEnd \
    --location centralus
    ```

2. Spusťte následující příkaz k vytvoření trasy ve směrovací tabulce odeslat veškerý provoz, jehož klientské podsíti (192.168.1.0/24) na **FW1** virtuálních počítačů (192.168.0.4):

    ```azurecli
    az network route-table route create \
    --resource-group testrg \
    --name RouteToFrontEnd \
    --route-table-name UDR-BackEnd \
    --address-prefix 192.168.1.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

3. Spusťte následující příkaz k přiřazení směrovací tabulka s **back-end** podsítě:

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name BackEnd \
    --route-table UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a>Povolení předávání IP na FW1

Povolení předávání IP v síťový adaptér používá **FW1**, proveďte následující kroky:

1. Spustit [az sítě seskupování zobrazit](/cli/azure/network/nic#show) příkazu s filtrem JMESPATH zobrazíte aktuální **povolení předávání ip** hodnota **předávání IP povolit**. Musí být nastavena na *false*.

    ```azurecli
    az network nic show \
    --resource-group testrg \
    --nname nicfw1 \
    --query 'enableIpForwarding' -o tsv
    ```

    Výstup:

        false

2. Spusťte následující příkaz pro povolení předávání IP:

    ```azurecli
    az network nic update \
    --resource-group testrg \
    --name nicfw1 \
    --ip-forwarding true
    ```

    Vyhledejte ve výstupu datového proudu ke konzole nebo jenom pro konkrétní testování **enableIpForwarding** hodnotu:

    ```azurecli
    az network nic show -g testrg -n nicfw1 --query 'enableIpForwarding' -o tsv
    ```

    Výstup:

        true

    Parametry:

    **předávání--ip**: *true* nebo *false*.

