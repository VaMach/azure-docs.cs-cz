---
title: "Řídit směrování a virtuální zařízení pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Zjistěte, jak řídit směrování a virtuální zařízení pomocí Azure CLI 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/18/2017
ms.author: jdial
ms.openlocfilehash: 5f21bc7a4fcd9507ea9d6b2b752a2328a7b834f0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-user-defined-routes-udr-using-the-azure-cli-10"></a>Vytvoření trasy definované uživatelem (UDR) pomocí Azure CLI 1.0

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure CLI](virtual-network-create-udr-arm-cli.md)
> * [Šablona](virtual-network-create-udr-arm-template.md)
> * [PowerShell (Classic)](virtual-network-create-udr-classic-ps.md)
> * [Rozhraní příkazového řádku (Classic)](virtual-network-create-udr-classic-cli.md)

Vytvořte vlastní směrování a virtuální zařízení pomocí rozhraní příkazového řádku Azure.

## <a name="cli-versions-to-complete-the-task"></a>Verze rozhraní příkazového řádku pro dokončení úlohy 

K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku: 

- [Azure CLI 1.0](#Create-the-UDR-for-the-front-end-subnet) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)
- [Azure CLI 2.0](virtual-network-create-udr-arm-cli.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků 


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Níže uvedené příkazy rozhraní příkazového řádku Azure ukázka očekávat jednoduché prostředí již vytvořeny podle výše uvedené scénáře. Pokud chcete ke spuštění příkazů, jak jsou zobrazeny v tomto dokumentu, nasazením nejprve vytvořit testovací prostředí [této šablony](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klikněte na tlačítko **nasadit do Azure**, nahradí výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů v portálu.


## <a name="create-the-udr-for-the-front-end-subnet"></a>Vytvoření UDR front-end podsítě
Pokud chcete vytvořit směrovací tabulku a směrování, které jsou potřebné pro podsítě front end závislosti na scénáři výše, postupujte podle následujících kroků.

1. Spusťte následující příkaz, který vytvořit směrovací tabulku front-end podsítě:

    ```azurecli
    azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest
    ```
   
    Výstup:
   
        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK
   
    Parametry:
   
   * **-g (nebo --resource-group)**. Název skupiny prostředků, které UDR vytvoří. V našem scénáři je to *TestRG*.
   * **-l (nebo --location)**. Oblast Azure, kde bude vytvořen nový UDR. Pro náš scénář *westus*.
   * **-n (nebo --name)**. Název pro nový UDR. Pro náš scénář *UDR front-endu*.
2. Spusťte následující příkaz k vytvoření trasy ve směrovací tabulce odeslat veškerý provoz, jehož k podsíti back-end (192.168.2.0/24) na **FW1** virtuálních počítačů (192.168.0.4):

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4
    ```
   
    Výstup:
   
        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK
   
    Parametry:
   
   * **-r (nebo--název směrovací tabulky)**. Název směrovací tabulka, kam bude přidána trasy. Pro náš scénář *UDR front-endu*.
   * **-a (nebo --address-prefixes)**. Předpona adresy podsítě, kde jsou pakety určené do. Pro náš scénář *192.168.2.0/24*.
   * **-y (nebo--další typ směrování)**. Typ objektu provozu se odešle do. Možné hodnoty jsou *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, nebo *žádné*.
   * **-p (nebo--další směrování ip adresu**). IP adresa dalšího směrování. Pro náš scénář *192.168.0.4*.
3. Spusťte následující příkaz k přiřazení směrovací tabulka vytvořili výše s **front-endu** podsítě:

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    Výstup:
   
        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK
   
    Parametry:
   
   * **-e (nebo--vnet-name)**. Název sítě VNet, kde je umístěný v podsíti. V našem scénáři je to *TestVNet*.

## <a name="create-the-udr-for-the-back-end-subnet"></a>Vytvoření UDR pro podsíť back-end
Pokud chcete vytvořit směrovací tabulku a směrování, které jsou potřeba pro back-end podsíť závislosti na scénáři výše, proveďte následující kroky:

1. Spusťte následující příkaz a vytvořte tabulku směrování pro podsíť back-end:

    ```azurecli
    azure network route-table create -g TestRG -n UDR-BackEnd -l westus
    ```

2. Spusťte následující příkaz k vytvoření trasy ve směrovací tabulce odeslat veškerý provoz, jehož klientské podsíti (192.168.1.0/24) na **FW1** virtuálních počítačů (192.168.0.4):

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4
    ```

3. Spusťte následující příkaz k přiřazení směrovací tabulka s **back-end** podsítě:

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a>Povolení předávání IP na FW1
Povolení předávání IP v síťový adaptér používá **FW1**, proveďte následující kroky:

1. Spustit příkaz, který následuje a Všimněte si, hodnota **předávání IP povolit**. Musí být nastavena na *false*.

    ```azurecli
    azure network nic show -g TestRG -n NICFW1
    ```

    Výstup:
   
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK
2. Spusťte následující příkaz pro povolení předávání IP:

    ```azurecli
    azure network nic set -g TestRG -n NICFW1 -f true
    ```
   
    Výstup:
   
        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK
   
    Parametry:
   
   * **-f (nebo--povolení předávání ip)**. *Hodnota TRUE,* nebo *false*.

