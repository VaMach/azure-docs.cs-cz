---
title: "Virtuální počítač s více IP adres pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Zjistěte, jak přiřadit více IP adres virtuálního počítače pomocí Azure CLI 1.0 | Správce prostředků."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: annahar
ms.openlocfilehash: 9f085dfa1fe4db36d58cb976bb550a46bf241ac7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-azure-cli-10"></a>Přiřadit více IP adres virtuálních počítačů pomocí Azure CLI 1.0

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Tento článek vysvětluje, jak vytvořit virtuální počítač (VM) pomocí modelu nasazení Azure Resource Manager pomocí Azure CLI 1.0. Nelze přiřadit více IP adres k prostředkům, které jsou vytvořené pomocí modelu nasazení classic. Další informace o modelech nasazení Azure, najdete [pochopit modely nasazení](../resource-manager-deployment-model.md) článku.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Vytvoření virtuálního počítače s více IP adres

Vám může tuto úlohu dokončit pomocí Azure CLI 1.0 (v tomto článku) nebo [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md). Kroky, které následují vysvětlují, jak vytvořit příklad virtuálních počítačů s více IP adres, jak je popsáno v tomto scénáři. Změnit názvy proměnných a typy IP adres podle potřeby týkající se vaší implementace.

1. Instalace a konfigurace Azure CLI 1.0 podle kroků v [instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článek a přihlaste se k vaší Azure účet, který `azure-login` příkaz.

2. Vytvořte skupinu prostředků:
    
    ```azurecli
    RgName=myResourceGroup
    Location=westcentralus
    azure group create --name $RgName --location $Location
    ```
3. Vytvořte virtuální síť:

    ```azurecli
    azure network vnet create --resource-group $RgName --location $Location --name myVNet \
    --address-prefixes 10.0.0.0/16
    ```
4. Vytvořte podsíť ve virtuální síti:

    ```azurecli
    azure network vnet subnet create --name mySubnet --resource-group $RgName --vnet-name myVNet \
    --address-prefix 10.0.0.0/24
    ```
    
5. Vytvořte účet úložiště pro virtuální počítač. Před spuštěním následujícího příkazu, nahraďte *můj_účet_úložiště* s jedinečným názvem. Název musí být jedinečný v rámci Azure:

    ```azurecli
    az storage account create --resource-group $RgName --location $Location --name mystorageaccount \
    --kind Storage --sku Standard_LRS
    ```

6. Konfigurace protokolu IP, síťový adaptér, vytvořit a přiřadit konfigurace protokolu IP na síťový adaptér. Můžete přidat, odebrat ani změnit konfiguraci podle potřeby. Následující konfigurace jsou popsané v tomto scénáři:

    **IPConfig-1**

    Zadejte následující příkazy, chcete-li vytvořit:

    - Prostředek veřejné IP adresy s statickou veřejnou IP adresu
    - Síťový adaptér, přiřazování veřejnou IP adresu a statickou privátní IP adresu.
    
    Nahraďte *mypublicdns* s názvem, který je v umístění v Azure jedinečný.

      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP1 \
      --domain-name-label mypublicdns --allocation-method Static
        
      azure network nic create --resource-group $RgName --location $Location --name myNic1 \
      --private-ip-address 10.0.0.4 --subnet-name mySubnet --subnet-vnet-name myVNet \
      --subnet-name mySubnet --public-ip-name myPublicIP1
      ```

      > [!NOTE]
      > Veřejné IP adresy mají nominální poplatek. Další informace o cenách IP adresu, najdete [IP adresu ceny](https://azure.microsoft.com/pricing/details/ip-addresses) stránky. Maximální počet veřejné IP adresy, které lze použít v předplatném je. Další informace o omezeních najdete v článku o [omezeních Azure](../azure-subscription-service-limits.md#networking-limits).

    **IPConfig-2**

     Zadejte následující příkazy a vytvořit nový prostředek veřejné IP adresy a nové konfigurace IP adresy se statickou veřejnou IP adresu a statickou privátní IP adresou:
    
      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP2
      --domain-name-label mypublicdns2 --allocation-method Static

      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --name IPConfig-2
      --private-ip-address 10.0.0.5 --public-ip-name myPublicIP2
      ```

    **IPConfig – 3**

    Zadejte následující příkazy a vytvořit konfiguraci IP adres se statickou privátní IP adresou a žádné veřejnou IP adresu:

      ```azurecli
      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --private-ip-address 10.0.0.6 \
      --name IPConfig-3
      ```

    >[!NOTE] 
    >Když v tomto článku přiřadí všechny konfigurace protokolu IP pro jednu síťovou kartu, můžete také přiřadit víc konfigurací IP adres na všechny síťové adaptéry ve virtuálním počítači. Naučte se vytvořit virtuální počítač s více síťovými kartami, přečtěte si vytvořením virtuálního počítače s více síťových adaptérů článku.

7. Vytvoření virtuálního počítače s Linuxem 

    ```azurecli
    az vm create --resource-group $RgName --name myVM1 --location $Location --nics myNic1 \
    --image UbuntuLTS --ssh-key-value ~/.ssh/id_rsa.pub --admin-username azureuser
    ```

    Chcete-li změnit velikost virtuálního počítače na standardní DS2 v2, například jednoduše přidejte následující vlastnost `--vm-size Standard_DS3_v2` k `azure vm create` příkazu v kroku 6.

8. Zadejte následující příkaz k zobrazení na síťový adaptér a přidružené konfigurace protokolu IP:

    ```azurecli
    azure network nic show --resource-group $RgName --name myNic1
    ```
9. Přidání privátních IP adres do operačního systému virtuálního počítače pomocí kroků pro operační systém v [přidat IP adresy na operační systém virtuálního počítače](#os-config) tohoto článku.

## <a name="add"></a>Přidání IP adres pro virtuální počítač

Další privátní a veřejné IP adresy můžete přidat do stávající síťové karty pomocí kroků, které následují. Příklady stavějí [scénář](#Scenario) popsané v tomto článku.

1. Otevřete rozhraní příkazového řádku Azure a postupujte podle pokynů v této části v rámci jedné relace rozhraní příkazového řádku. Pokud ještě nemáte rozhraní příkazového řádku Azure, instalaci a konfiguraci, proveďte kroky v [instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článek a přihlaste se k účtu Azure.

2. Proveďte kroky v jednom z těchto částí, podle potřeb:

    - **Přidejte privátní IP adresy**
    
        Chcete-li přidat privátní IP adresy pro síťový adaptér, je nutné vytvořit konfiguraci IP adres pomocí níže uvedeného příkazu. Statická adresa musí být nepoužívané adresa podsítě.

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 \
        --private-ip-address 10.0.0.7 --name IPConfig-4
        ```
        Vytvořte tolik konfigurace, jak požadujete, pomocí konfigurace jedinečné názvy a privátní IP adresy (pro konfigurace s statické IP adresy).

    - **Přidejte veřejnou IP adresu**
    
        Veřejná IP adresa se přidá tím, že přidružíte na novou konfiguraci protokolu IP nebo existující konfiguraci IP adres. Proveďte kroky v jednom z následujících, potřebujete.

        > [!NOTE]
        > Veřejné IP adresy mají nominální poplatek. Další informace o cenách IP adresu, najdete [IP adresu ceny](https://azure.microsoft.com/pricing/details/ip-addresses) stránky. Maximální počet veřejné IP adresy, které lze použít v předplatném je. Další informace o omezeních najdete v článku o [omezeních Azure](../azure-subscription-service-limits.md#networking-limits).
        >

        **Přidružte prostředek na novou konfiguraci protokolu IP**
    
        Vždy, když přidáte veřejnou IP adresu v novou konfiguraci protokolu IP, musíte taky přidat privátní IP adresy, protože všechny konfigurace protokolu IP, musí mít privátní IP adresy. Můžete přidat existující prostředek veřejné IP adresy, nebo vytvořte novou. Pokud chcete vytvořit nový, zadejte následující příkaz:

        ```azurecli
        azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP3 \
        --domain-name-label mypublicdns3
        ```

        Chcete-li vytvořit novou konfiguraci protokolu IP se statickou privátní IP adresou a přidruženého *myPublicIP3* veřejných IP adres prostředků, zadejte následující příkaz:

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic --name IPConfig-4 \
        --private-ip-address 10.0.0.8 --public-ip-name myPublicIP3
        ```

        **Přidružte prostředek k existující konfigurace IP**

        Prostředek veřejné IP adresy lze přidružit pouze pro konfiguraci IP adres, který ho přidružené neobsahuje. Můžete určit, zda má konfiguraci IP adres přidružené veřejnou IP adresu tak, že zadáte následující příkaz:

        ```azurecli
        azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
        ```

        Podívejte se na řádek podobný tomu, který následuje pro IPConfig 3 ve vrácené výstupu:

        ```         
        Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
        IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
        IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet
        ```
          
        Vzhledem k tomu **veřejnou IP adresu** sloupec pro *IpConfig 3* je prázdné, žádný prostředek veřejné IP adresy je aktuálně k ní přidružena. Můžete přidat existující prostředek veřejné IP adresy na IpConfig 3, nebo zadejte následující příkaz k jeho vytvoření:

        ```azurecli
        azure network public-ip create --resource-group  myResourceGroup --location westcentralus \
        --name myPublicIP3 --domain-name-label mypublicdns3 --allocation-method Static
        ```

        Zadejte následující příkaz pro přidružení prostředek veřejné IP adresy ke stávající konfiguraci IP adresy s názvem *IPConfig 3*:
        ```azurecli
        azure network nic ip-config set --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3 \
        --public-ip-name myPublicIP3
        ```

3. Zobrazení privátních IP adres a prostředky veřejné adresy IP adresy přiřazené na síťový adaptér tak, že zadáte následující příkaz:

    ```azurecli
    azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
    ```

      Vrácený výstup bude vypadat takto:
      ```
      Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        
      default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
      IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
      IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet  myPublicIP3
      ```
4. Přidat privátní IP adresy, které jste přidali do síťového adaptéru v operačním systému virtuálního počítače podle pokynů v [přidat IP adresy na operační systém virtuálního počítače](#os-config) tohoto článku. Nepřidávejte veřejné IP adresy v operačním systému.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
