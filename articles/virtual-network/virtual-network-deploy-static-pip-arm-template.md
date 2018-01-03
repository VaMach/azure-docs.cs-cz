---
title: "Vytvoření virtuálního počítače se statickou veřejnou IP adresu - šablony Azure Resource Manageru | Microsoft Docs"
description: "Naučte se vytvořit virtuální počítač se statickou veřejnou IP adresu pomocí šablony Azure Resource Manager."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d551085a-c7ed-4ec6-b4c3-e9e1cebb774c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2f503aa60fdd9b7cf66ef482a1041e34c88e5c01
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-an-azure-resource-manager-template"></a>Vytvoření virtuálního počítače se statickou veřejnou IP adresu pomocí šablony Azure Resource Manager

> [!div class="op_single_selector"]
> * [portál Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Šablona](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (Classic)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Resource Manager, které společnost Microsoft doporučuje pro většinu nových nasazení místo modelu nasazení classic.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-address-resources-in-a-template-file"></a>Prostředky veřejné adresy IP adresy v souboru šablony
Můžete zobrazit a stáhnout [Ukázka šablony](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).

V následující části zobrazuje definici prostředek veřejné IP, založené na výše uvedené scénáře:

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('webVMSetting').pipName]",
  "location": "[variables('location')]",
  "properties": {
    "publicIPAllocationMethod": "Static"
  },
  "tags": {
    "displayName": "PublicIPAddress - Web"
  }
},
```

Upozornění **publicIPAllocationMethod** vlastnost, která je nastavena na hodnotu *statické*. Tato vlastnost může být buď *dynamické* (výchozí hodnota) nebo *statické*. Jeho nastavení na statické záruky, které se nikdy změní veřejnou IP adresu přiřadit.

Následující část popisuje přidružení síťového rozhraní veřejné IP adresy:

```json
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('webVMSetting').nicName]",
    "location": "[variables('location')]",
    "tags": {
    "displayName": "NetworkInterface - Web"
    },
    "dependsOn": [
      "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
      "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
    ],
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
          "privateIPAllocationMethod": "Static",
          "privateIPAddress": "[variables('webVMSetting').ipAddress]",
          "publicIPAddress": {
          "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
          },
          "subnet": {
            "id": "[variables('frontEndSubnetRef')]"
          }
        }
      }
    ]
  }
},
```

Upozornění **publicIPAddress** vlastnost přejdete **Id** prostředku s názvem **variables('webVMSetting').pipName**. Je název prostředek veřejné IP uvedené výše.

Nakonec je síťové rozhraní výše uvedené v **networkProfile** vlastnost Probíhá vytváření virtuálního počítače.

```json
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }
```

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Nasazení šablony pomocí metody Click to Deploy

Ukázková šablona, která je k dispozici ve veřejném úložišti, používá soubor parametrů obsahující výchozí hodnoty, které se použijí k vygenerování výše popsaného scénáře. K nasazení této šablony pomocí, klikněte na nasadit, klikněte na tlačítko **nasadit do Azure** v souboru Readme.md [virtuální počítač s statické PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) šablony. V případě potřeby nahradit výchozí hodnoty parametrů a zadejte hodnoty pro parametry prázdné.  Postupujte podle pokynů v portálu k vytvoření virtuálního počítače se statickou veřejnou IP adresu.

## <a name="deploy-the-template-by-using-powershell"></a>Nasazení šablony pomocí prostředí PowerShell

Pokud chcete nasadit šablonu, kterou jste stáhli, pomocí prostředí PowerShell, použijte následující postup.

1. Pokud jste prostředí Azure PowerShell nikdy nepoužívali, proveďte kroky v [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.
2. V konzole PowerShell, spusťte `New-AzureRmResourceGroup` v případě potřeby vytvořte novou skupinu prostředků. Pokud už máte skupinu prostředků, který je vytvořen, přejděte ke kroku 3.

    ```powershell
    New-AzureRmResourceGroup -Name PIPTEST -Location westus
    ```

    Očekávaný výstup:
   
        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. V konzole PowerShell, spusťte `New-AzureRmResourceGroupDeployment` rutiny k nasazení šablony.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
        -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
        -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json
    ```

    Očekávaný výstup:
   
        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0
   
        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         
   
        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Nasazení šablony pomocí rozhraní příkazového řádku Azure
Pokud chcete šablonu nasadit pomocí rozhraní příkazového řádku Azure, proveďte následující kroky:

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, postupujte podle kroků v [instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md) článku nainstalovat a nakonfigurovat ho.
2. Spustit `azure config mode` příkaz Přepnout do režimu Resource Manager, jak je uvedeno níže.

    ```azurecli
    azure config mode arm
    ```

    Očekávaný výstup výše uvedeného příkazu:

        info:    New mode is arm

3. Otevřete [soubor parametrů](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), vyberte její obsah a uložte je do souboru v počítači. V tomto příkladu jsou parametry uloží do souboru s názvem *Parameters.JSON tímto kódem*. Ke změně hodnoty parametru v rámci tohoto souboru v případě potřeby, ale minimálně, se doporučuje, změňte hodnotu pro parametr adminPassword na jedinečný a složité heslo.
4. Spustit `azure group deployment create` cmd nasadíte novou síť VNet pomocí šablony a parametr souborů, které jste stáhli a upravili v předchozích krocích. V tomto příkazu nahraďte <path> s cestou jste soubor uložili. 

    ```azurecli
    azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json
    ```

    Očekávaný výstup (uvádí parametr hodnoty používané):

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/[Subscription ID]/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

