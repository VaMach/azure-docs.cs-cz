---
title: "Vytvoření virtuální sítě Azure s několika podsítěmi | Microsoft Docs"
description: "Naučte se vytvořit virtuální síť s více podsítěmi v Azure."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ad679a4-a959-4e48-a317-d9f5655a442b
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: f82a95ec9543b2d53ef28bf7f15315e23cf4893a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-network-with-multiple-subnets"></a>Vytvoření virtuální sítě s několika podsítěmi

V tomto kurzu zjistěte, jak vytvořit základní virtuální síť Azure, která má oddělené veřejné a privátní podsítě. Prostředky ve virtuálních sítích můžete komunikaci mezi sebou a s prostředky v jiných sítích připojené k virtuální síti. Prostředky Azure, jako jsou virtuální počítače, služby App Service Environment, sady škálování virtuálního počítače, Azure HDInsight a cloudové služby můžete vytvořit ve stejné nebo různých podsítích v rámci virtuální sítě. Vytváření prostředků v různých podsítích vám umožňuje filtrovat provoz sítě a odhlásit z podsítě nezávisle s [skupin zabezpečení sítě](virtual-networks-create-nsg-arm-pportal.md)a [směrovat přenos mezi podsítěmi](virtual-network-create-udr-arm-ps.md) přes síť virtuální zařízení, jako je například Brána firewall, pokud zvolíte možnost. 

Následující části obsahují postupy, které můžete vytvořit virtuální síť pomocí [portál Azure](#portal), rozhraní příkazového řádku Azure ([rozhraní příkazového řádku Azure](#azure-cli)), [prostředí Azure PowerShell](#powershell)a [šablony Azure Resource Manageru](#resource-manager-template). Výsledkem je stejný, bez ohledu na to, jaký nástroj použijete k vytvoření virtuální sítě. Kliknutím na odkaz nástroj přejděte do části tohoto kurzu. Další informace o všech [virtuální sítě](virtual-network-manage-network.md) a [podsíť](virtual-network-manage-subnet.md) nastavení.

Tento článek obsahuje kroky k vytvoření virtuální sítě pomocí modelu nasazení Resource Manager, který je model nasazení, které vám doporučujeme používat při vytváření nové virtuální sítě. Pokud potřebujete k vytvoření virtuální sítě (klasické), přečtěte si [vytvoření virtuální sítě (klasické)](create-virtual-network-classic.md). Pokud si nejste obeznámeni s modelech nasazení Azure, najdete v části [modelech nasazení Azure pochopit](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="portal"></a>Portál Azure

1. V internetovém prohlížeči, přejděte na [portál Azure](https://portal.azure.com). Přihlaste se pomocí vaší [účet Azure](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Pokud účet Azure nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Na portálu, klikněte na tlačítko **+ nový** > **sítě** > **virtuální síť**.
3. Na **vytvořit virtuální síť** okno, zadejte následující hodnoty a pak klikněte na tlačítko **vytvořit**:

    |Nastavení|Hodnota|
    |---|---|
    |Name (Název)|myVnet|
    |Adresní prostor|10.0.0.0/16|
    |Název podsítě|Veřejné|
    |Rozsah adres podsítě|10.0.0.0/24|
    |Skupina prostředků|Nechte **vytvořit nový** vybrané a potom zadejte **myResourceGroup**.|
    |Předplatné a umístění|Vyberte vaše předplatné a umístění.

    Pokud jste Azure ještě nepoužívali, další informace o [skupiny prostředků](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [odběry](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), a [umístění](https://azure.microsoft.com/regions) (také označuje jako *oblasti*).
4. Na portálu můžete vytvořit jenom jednu podsíť, když vytvoříte virtuální síť. V tomto kurzu vytvoříte druhou podsíť po vytvoření virtuální sítě. Můžete vytvořit později přístupné z Internetu prostředky v **veřejné** podsítě. Můžete také vytvořit prostředky, které nejsou přístupné z Internetu v **privátní** podsítě. Chcete-li vytvořit druhou podsíť v **vyhledávání prostředků** pole v horní části stránky, zadejte **myVnet**. Ve výsledcích hledání klikněte na tlačítko **myVnet**. Pokud máte více virtuálních sítí se stejným názvem v rámci vašeho předplatného, zkontrolujte skupiny prostředků, které jsou uvedeny v části každá virtuální síť. Ujistěte se, že kliknete **myVnet** hledání výsledek, který má skupinu prostředků **myResourceGroup**.
5. Na **myVnet** okno v části **nastavení**, klikněte na tlačítko **podsítě**.
6. Na **myVnet - podsítě** okně klikněte na tlačítko **+ podsítě**.
7. Na **přidat podsíť** okně pro **název**, zadejte **privátní**. Pro **rozsahu adres**, zadejte **10.0.1.0/24**.  Klikněte na **OK**.
8. Na **myVnet - podsítě** okně zkontrolujte podsítě. Můžete zobrazit **veřejné** a **privátní** podsítě, které jste vytvořili.
9. **Volitelné:** dokončení další kurzy uvedené v části [další kroky](#next-steps) filtrování provozu sítě a odhlásit z každé podsíti se skupinami zabezpečení sítě, směrovat provoz mezi podsítěmi prostřednictvím sítě virtuálního zařízení , nebo pro připojení virtuální sítě k jiné virtuální sítě nebo místní sítě.
10. **Volitelné:** odstranit prostředky, které v tomto kurzu vytvoříte pomocí kroků v [odstranit prostředky](#delete-portal).

## <a name="azure-cli"></a>Azure CLI

Rozhraní příkazového řádku Azure jsou stejné, zda spuštěním příkazů z Windows, Linux nebo systému macOS. Existují však skriptování rozdíly mezi součásti pro operační systém. Skript v následujících krocích se spustí v prostředí Bash. 

1. [Instalace a konfigurace rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Zajistěte, že abyste měli nejnovější verzi rozhraní příkazového řádku Azure, který je nainstalovaný. Chcete-li získat nápovědu pro příkazy rozhraní příkazového řádku, zadejte `az <command> --help`. Místo instalace rozhraní příkazového řádku a jeho požadavky, můžete použít prostředí cloudové služby Azure. Služba Azure Cloud Shell je volně dostupné prostředí Bash, které můžete spustit přímo z portálu Azure Portal. Cloudové prostředí má Azure CLI předem nainstalován a nakonfigurován pro použití s vaším účtem. Chcete-li použít cloudové prostředí, klikněte na tlačítko prostředí cloudu (**> _**) tlačítka v horní části [portál](https://portal.azure.com) nebo stačí kliknout na *vyzkoušet* tlačítko v krocích, které následují. 
2. Pokud rozhraní příkazového řádku spuštěn místně, přihlaste se k Azure pomocí `az login` příkaz. Pokud používáte cloudové prostředí, jste již přihlášeni.
3. Projděte si následující skript a její komentáře. V prohlížeči zkopírujte skript a vložte jej do relace rozhraní příkazového řádku:

    ```azurecli-interactive
    #!/bin/bash
    
    # Create a resource group.
    az group create \
      --name myResourceGroup \
      --location eastus
    
    # Create a virtual network with one subnet named Public.
    az network vnet create \
      --name myVnet \
      --resource-group myResourceGroup \
      --subnet-name Public
    
    # Create an additional subnet named Private in the virtual network.
    az network vnet subnet create \
      --name Private \
      --address-prefix 10.0.1.0/24 \
      --vnet-name myVnet \
      --resource-group myResourceGroup
    ```
    
4. Po dokončení skript spuštěn, zkontrolujte podsítě virtuální sítě. Zkopírujte následující příkaz a pak ji vložit do relace rozhraní příkazového řádku:

    ```azurecli
    az network vnet subnet list --resource-group myResourceGroup --vnet-name myVnet --output table
    ```

5. **Volitelné:** dokončení další kurzy uvedené v části [další kroky](#next-steps) filtrování provozu sítě a odhlásit z každé podsíti se skupinami zabezpečení sítě, směrovat provoz mezi podsítěmi prostřednictvím sítě virtuálního zařízení , nebo pro připojení virtuální sítě k jiné virtuální sítě nebo místní sítě.
6. **Volitelné**: Odstraňte prostředky, které v tomto kurzu vytvoříte pomocí kroků v [odstranit prostředky](#delete-cli).

## <a name="powershell"></a>PowerShell

1. Nainstalujte nejnovější verzi modulu [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) pro PowerShell. Pokud s Azure PowerShellem začínáte, podívejte se na [Přehled Azure PowerShellu](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. V relaci prostředí PowerShell přihlášení k Azure s vaší [účet Azure](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) pomocí `login-azurermaccount` příkaz.

3. Projděte si následující skript a její komentáře. V prohlížeči zkopírujte skript a vložte ho do relace prostředí PowerShell:

    ```powershell
    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name myResourceGroup `
      -Location eastus
    
    # Create the public and private subnets.
    $Subnet1 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Public `
      -AddressPrefix 10.0.0.0/24
    $Subnet2 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Private `
      -AddressPrefix 10.0.1.0/24
    
    # Create a virtual network.
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName myResourceGroup `
      -Location eastus `
      -Name myVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet1,$Subnet2
    ```

4. Chcete-li zkontrolovat podsítě virtuální sítě, zkopírujte následující příkaz a pak ji vložit do relace prostředí PowerShell:

    ```powershell
    $Vnet.subnets | Format-Table Name, AddressPrefix
    ```

5. **Volitelné:** dokončení další kurzy uvedené v části [další kroky](#next-steps) filtrování provozu sítě a odhlásit z každé podsíti se skupinami zabezpečení sítě, směrovat provoz mezi podsítěmi prostřednictvím sítě virtuálního zařízení , nebo pro připojení virtuální sítě k jiné virtuální sítě nebo místní sítě.
6. **Volitelné**: Odstraňte prostředky, které v tomto kurzu vytvoříte pomocí kroků v [odstranit prostředky](#delete-powershell).

## <a name="resource-manager-template"></a>Šablona Resource Manageru

Virtuální síť můžete nasadit pomocí šablony Azure Resource Manager. Další informace o šablonách najdete v tématu [co je správce prostředků](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#template-deployment). Pro přístup k šabloně a další informace o jeho parametrech najdete v článku [vytvořit virtuální síť se dvěma podsítěmi](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) šablony. Šablony můžete nasadit pomocí [portál](#template-portal), [rozhraní příkazového řádku Azure](#template-cli), nebo [prostředí PowerShell](#template-powershell).

Volitelné kroky po nasazení šablony:

1. Dokončení další kurzy uvedené v části [další kroky](#next-steps) vyfiltrujete síťový provoz do/z každé podsíti se skupinami zabezpečení sítě, směrovat provoz mezi podsítěmi prostřednictvím sítě virtuálního zařízení, nebo se připojit virtuální síť do jiných virtuálních sítí nebo místní sítě.
2. Odstranit prostředky, které v tomto kurzu vytvoříte pomocí kroků v jakékoli pododdílu [odstranit prostředky](#delete).

### <a name="template-portal"></a>Portál Azure

1. V prohlížeči otevřete [stránku šablony](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets).
2. Klikněte **nasadit do Azure** tlačítko. Pokud jste již přihlášeni do Azure, přihlaste se na obrazovce přihlášení k portálu Azure, který se zobrazí.
3. Přihlaste se k portálu pomocí vaší [účet Azure](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Pokud účet Azure nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/offers/ms-azr-0044p).
4. Zadejte následující hodnoty pro parametry:

    |Parametr|Hodnota|
    |---|---|
    |Předplatné|Vyberte předplatné|
    |Skupina prostředků|myResourceGroup|
    |Umístění|Vyberte umístění|
    |Název virtuální sítě|myVnet|
    |Předpona adresy virtuální sítě|10.0.0.0/16|
    |Subnet1Prefix|10.0.0.0/24|
    |Subnet1Name|Veřejné|
    |Subnet2Prefix|10.0.1.0/24|
    |Subnet2Name|Privátní|

5. S podmínkami a ujednáními a potom klikněte na **nákupu** k nasazení virtuální sítě.

### <a name="template-cli"></a>Rozhraní příkazového řádku Azure

1. [Instalace a konfigurace rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Zajistěte, že abyste měli nejnovější verzi rozhraní příkazového řádku Azure, který je nainstalovaný. Chcete-li získat nápovědu pro příkazy rozhraní příkazového řádku, zadejte `az <command> --help`. Místo instalace rozhraní příkazového řádku a jeho požadavky, můžete použít prostředí cloudové služby Azure. Služba Azure Cloud Shell je volně dostupné prostředí Bash, které můžete spustit přímo z portálu Azure Portal. Cloudové prostředí má Azure CLI předem nainstalován a nakonfigurován pro použití s vaším účtem. Chcete-li použít cloudové prostředí, klikněte na tlačítko prostředí cloudu **> _** tlačítka v horní části [portál](https://portal.azure.com), nebo stačí kliknout na **vyzkoušet** tlačítko v krocích, které následují. 
2. Pokud rozhraní příkazového řádku spuštěn místně, přihlaste se k Azure pomocí `az login` příkaz. Pokud používáte cloudové prostředí, jste již přihlášeni.
3. Pokud chcete vytvořit skupinu prostředků pro virtuální síť, zkopírujte následující příkaz a vložte jej do relace rozhraní příkazového řádku:

    ```azurecli-interactive
    az group create --name myResourceGroup --location eastus
    ```
    
4. Šablony můžete nasadit pomocí jedné z následujících možností parametry:
    - **Výchozí hodnoty parametrů**. Zadejte následující příkaz:
    
        ```azurecli-interactive
        az group deployment create --resource-group myResourceGroup --name VnetTutorial --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json`
        ```
    - **Hodnoty parametru vlastní**. Stáhnout a upravit šablonu před nasazením šablon. Také můžete nasazení šablony pomocí parametry na příkazovém řádku, nebo se souborem samostatné parametry nasazení šablony. Chcete-li stáhnout soubory šablony a parametry, klikněte na tlačítko **Procházet na Githubu** na tlačítko [vytvořit virtuální síť se dvěma podsítěmi](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) stránku šablony. V Githubu, klikněte na **azuredeploy.parameters.json** nebo **azuredeploy.json** souboru. Potom klikněte **Raw** tlačítko Zobrazit soubor. V prohlížeči zkopírujte obsah souboru. Uložte obsah do souboru ve vašem počítači. Můžete změnit hodnoty parametrů v šabloně, nebo nasazení šablony souborem samostatné parametry.  

    Další informace o tom, jak nasadit šablony pomocí těchto metod, zadejte `az group deployment create --help`.

### <a name="template-powershell"></a>Prostředí PowerShell

1. Nainstalujte nejnovější verzi modulu [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) pro PowerShell. Pokud s Azure PowerShellem začínáte, podívejte se na [Přehled Azure PowerShellu](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. V relaci prostředí PowerShell přihlásit vaše [účet Azure](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account), zadejte `login-azurermaccount`.
3. Pokud chcete vytvořit skupinu prostředků pro virtuální síť, zadejte následující příkaz:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
    ```
    
4. Šablony můžete nasadit pomocí jedné z následujících možností parametry:
    - **Výchozí hodnoty parametrů**. Zadejte následující příkaz:
    
        ```powershell
        New-AzureRmResourceGroupDeployment -Name VnetTutorial -ResourceGroupName myResourceGroup -TemplateUri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json
        ```
        
    - **Hodnoty parametru vlastní**. Stáhnout a upravit šablonu před nasazením. Také můžete nasazení šablony pomocí parametry na příkazovém řádku, nebo se souborem samostatné parametry nasazení šablony. Chcete-li stáhnout soubory šablony a parametry, klikněte na tlačítko **Procházet na Githubu** na tlačítko [vytvořit virtuální síť se dvěma podsítěmi](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) stránku šablony. V Githubu, klikněte na **azuredeploy.parameters.json** nebo **azuredeploy.json** souboru. Potom klikněte **Raw** tlačítko Zobrazit soubor. V prohlížeči zkopírujte obsah souboru. Uložte obsah do souboru ve vašem počítači. Můžete změnit hodnoty parametrů v šabloně, nebo nasazení šablony souborem samostatné parametry.  

    Další informace o tom, jak nasadit šablony pomocí těchto metod, zadejte `Get-Help New-AzureRmResourceGroupDeployment`. 

## <a name="delete"></a>Odstraňte prostředky

Po dokončení tohoto kurzu můžete chtít odstranit prostředky, které jste vytvořili, tak, aby vám zbytečně nenabíhaly poplatky za používání. Odstranění skupiny prostředků se také odstraní všechny prostředky, které jsou ve skupině prostředků.

### <a name="delete-portal"></a>Portál Azure

1. V dialogovém okně hledání portálu zadejte **myResourceGroup**. Ve výsledcích hledání klikněte na tlačítko **myResourceGroup**.
2. Na **myResourceGroup** okně klikněte **odstranit** ikonu.
3. Potvrďte odstranění, v **název skupiny prostředků typu** zadejte **myResourceGroup**a potom klikněte na **odstranit**.

### <a name="delete-cli"></a>Rozhraní příkazového řádku Azure

V relaci příkazového řádku zadejte následující příkaz:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>Prostředí PowerShell

V relaci prostředí PowerShell zadejte následující příkaz:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Další kroky

- Další informace o všech virtuální síť a podsíť nastavení najdete v tématu [spravovat virtuální sítě](virtual-network-manage-network.md#view-vnet) a [spravovat podsítě virtuální sítě](virtual-network-manage-subnet.md#create-subnet). Máte různé možnosti pro splňují odlišné požadavky pomocí virtuální sítě a podsítě v produkčním prostředí.
- Filtrovat podsíť příchozí a odchozí provoz vytvořením a použitím [skupin zabezpečení sítě](virtual-networks-nsg.md) k podsítím.
- Směrovat provoz mezi podsítěmi prostřednictvím sítě virtuálního zařízení, tak, že vytvoříte [trasy definované uživatelem](virtual-network-create-udr-arm-ps.md) a použít trasy pro každou podsíť.
- Vytvoření [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuálního počítače ve stávající virtuální síť.
- Připojení dvě virtuální sítě tak, že vytvoříte [partnerský vztah virtuální sítě](virtual-network-peering-overview.md) mezi virtuálními sítěmi.
- Připojit virtuální síť k místní síti pomocí [brány VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nebo [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) okruh.
