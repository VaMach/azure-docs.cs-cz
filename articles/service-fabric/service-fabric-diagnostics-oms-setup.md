---
title: "Azure Service Fabric - nastavení monitorování s OMS Log Analytics | Microsoft Docs"
description: "Zjistěte, jak pro nastavení služby Operations Management Suite pro vizualizaci a analýzu událostí k monitorování clusterů Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/17/2017
ms.author: dekapur
ms.openlocfilehash: 288c7482058cd9f824b6001bb9ad36d1a5e0f8bf
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="set-up-operations-management-suite-log-analytics-for-a-cluster"></a>Nastavení pro cluster s podporou Operations Management Suite Log Analytics

Můžete nastavit pracovní prostor služby Operations Management Suite (OMS) pomocí Správce prostředků Azure, PowerShell nebo Azure Marketplace. Pokud chcete zachovat aktualizovanou šablonu Resource Manager nasazení pro budoucí použití, nastavení prostředí OMS pomocí stejné šablony. Nasazení prostřednictvím Marketplace je jednodušší, pokud již máte cluster nasadit s diagnostikou povolena. Pokud účet, do které nasazujete OMS nemají přístup na úrovni předplatného, nasaďte pomocí prostředí PowerShell nebo šablony Resource Manageru.

> [!NOTE]
> Pokud chcete nastavit OMS monitorování vašeho clusteru, musíte mít diagnostiky povoleno zobrazovat události na úrovni clusteru nebo platformu.

## <a name="deploy-oms-by-using-azure-marketplace"></a>Nasazení OMS pomocí webu Azure Marketplace

Pokud chcete přidat pracovním prostorem OMS po nasazení clusteru, přejděte na Azure Marketplace v portálu a vyhledejte **Service Fabric Analytics**:

1. Vyberte **nový** v levé navigační nabídce. 

2. Vyhledejte **služby Fabric Analytics**. Vyberte prostředek, který se zobrazí.

3. Vyberte **Vytvořit**.

    ![Analýza SF OMS v Marketplace.](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. V okně vytváření Service Fabric Analytics vyberte **vyberte pracovní prostor** pro **pracovním prostorem OMS** pole a potom **vytvořit nový pracovní prostor**. Vyplňte požadované položky. Jediným požadavkem je, že předplatné pro cluster Service Fabric a pracovním prostorem OMS je stejný. Při ověření vaše záznamy vaším pracovním prostorem OMS spustí nasazení. Nasazení trvá jenom pár minut.

5. Po dokončení vyberte **vytvořit** znovu v dolní části okna Vytvoření Service Fabric Analytics. Ujistěte se, že nový pracovní prostor se zobrazí v části **pracovním prostorem OMS**. Tato akce přidá řešení do pracovního prostoru, kterou jste vytvořili.

Pokud používáte systém Windows, pokračujte následujícími kroky pro připojení OMS k účtu úložiště, kde jsou uloženy vaše události clusteru. 

>[!NOTE]
>Povolení této funkce pro Linux clusterů ještě není k dispozici. 

### <a name="add-the-oms-agent-to-your-cluster"></a>Přidat agenta OMS do clusteru 

1. V pracovním prostoru musí být připojen k diagnostiky dat pocházejících z clusteru. Přejděte do skupiny prostředků, ve které jste vytvořili řešení Service Fabric analýzy. Vyberte **ServiceFabric\<nameOfOMSWorkspace\>**  a přejděte na stránku s jeho Přehled. Odtud můžete změnit nastavení řešení, nastavení pracovního prostoru a přístup k portálu OMS.

2. V levém navigačním nabídce v části **zdroje dat pracovního prostoru**, vyberte **účtů úložiště protokolů**.

3. Na **protokol účtu úložiště** vyberte **přidat** v horní části přidat váš cluster protokoly do pracovního prostoru.

4. Vyberte **účet úložiště** přidat odpovídající účet vytvořený v clusteru. Pokud jste použili výchozí název, účet úložiště je **sfdg\<resourceGroupName\>**. Můžete to ověřit také pomocí šablony Azure Resource Manager používá k nasazení kontrolou hodnota používaná pro cluster, **applicationDiagnosticsStorageAccountName**. Pokud název nezobrazuje, posuňte se dolů a vyberte **načtěte více**. Vyberte název účtu úložiště.

5. Zadejte typ Data. Nastavte ji na **události služby Fabric**.

6. Ujistěte se, zda je zdroj automaticky nastavena na **WADServiceFabric\*EventTable**.

7. Vyberte **OK** připojit pracovního prostoru do protokolů vašeho clusteru.

    ![Přidat protokol účtu úložiště do OMS](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

Účet se zobrazí jako součást svého účtu úložiště přihlásí zdroje dat pracovního prostoru.

Řešení Service Fabric analýzy jste přidali v pracovním prostoru analýzy protokolů OMS, který je nyní správně připojené k platformě váš cluster a tabulku protokolu aplikace. Další zdroje můžete přidat do pracovního prostoru stejným způsobem.


## <a name="deploy-oms-by-using-a-resource-manager-template"></a>Nasazení OMS pomocí šablony Resource Manageru

Když nasadíte cluster pomocí šablony Resource Manageru, šablona vytvoří nový pracovní prostor OMS, Service Fabric řešení přidá do pracovního prostoru a nakonfiguruje jej číst data z tabulky příslušné úložiště.

Můžete použít a změnit [Tato ukázka šablona](https://azure.microsoft.com/resources/templates/service-fabric-oms/) podle svých požadavků. Šablony, které poskytují různé možnosti pro nastavení pracovním prostorem OMS naleznete na adrese [Service Fabric a OMS šablony](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

Proveďte následující změny:
1. Přidat `omsWorkspaceName` a `omsRegion` na parametry přidáním následující fragment kódu do parametrech definovaných v vaše *template.json* souboru. Nebojte se, že podle potřeby upravte výchozí hodnoty. Také přidejte dva nové parametry ve vaší *Parameters.JSON tímto kódem* souboru definujte jejich hodnoty pro nasazení prostředků:
    
    ```json
    "omsWorkspacename": {
        "type": "string",
        "defaultValue": "sfomsworkspace",
        "metadata": {
            "description": "Name of your OMS Log Analytics Workspace"
        }
    },
    "omsRegion": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
            "West Europe",
            "East US",
            "Southeast Asia"
        ],
        "metadata": {
            "description": "Specify the Azure Region for your OMS workspace"
        }
    }
    ```

    `omsRegion` Hodnoty musí odpovídat na konkrétní sadu hodnot. Vyberte ten, který je nejblíže k nasazení clusteru.

2. Pokud odešlete žádné aplikace protokoly OMS, nejdřív ověřte, že `applicationDiagnosticsStorageAccountType` a `applicationDiagnosticsStorageAccountName` jsou zahrnuty jako parametry v šabloně. Pokud nejsou zahrnuty, přidejte je do části proměnné a upravit jejich hodnoty podle potřeby. Použít také jako parametry podle předchozích formátu.

    ```json
    "applicationDiagnosticsStorageAccountType": "Standard_LRS",
    "applicationDiagnosticsStorageAccountName": "[toLower(concat('oms', uniqueString(resourceGroup().id), '3' ))]"
    ```

3. Přidáte řešení Service Fabric OMS do vaší šablony proměnné:

    ```json
    "solution": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "solutionName": "ServiceFabric"
    ```

4. Přidejte následující na konci části vaše prostředky, po kterém je deklarovaná prostředek clusteru Service Fabric:

    ```json
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[parameters('omsWorkspacename')]",
        "type": "Microsoft.OperationalInsights/workspaces",
        "properties": {
            "sku": {
                "name": "Free"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-11-01-preview",
                "name": "[concat(parameters('applicationDiagnosticsStorageAccountName'),parameters('omsWorkspacename'))]",
                "type": "storageinsightconfigs",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]",
                    "[concat('Microsoft.Storage/storageAccounts/', parameters('applicationDiagnosticsStorageAccountName'))]"
                ],
                "properties": {
                    "containers": [ ],
                    "tables": [
                        "WADServiceFabric*EventTable",
                        "WADWindowsEventLogsTable",
                        "WADETWEventTable"
                    ],
                    "storageAccount": {
                        "id": "[resourceId('Microsoft.Storage/storageaccounts/', parameters('applicationDiagnosticsStorageAccountName'))]",
                        "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-06-15').key1]"
                    }
                }
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('solution')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "id": "[resourceId('Microsoft.OperationsManagement/solutions/', variables('solution'))]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('OMSWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('solution')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('solutionName'))]",
            "promotionCode": ""
        }
    }
    ```
    
    > [!NOTE]
    > Pokud jste přidali `applicationDiagnosticsStorageAccountName` jako proměnnou, nezapomeňte upravit každý odkaz na jeho `variables('applicationDiagnosticsStorageAccountName')` místo `parameters('applicationDiagnosticsStorageAccountName')`.

5. Nasazení šablony jako upgrade správce prostředků do clusteru pomocí `New-AzureRmResourceGroupDeployment` rozhraní API v modulu AzureRM prostředí PowerShell. Příkaz příkladu by byl:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName "sfcluster1" -TemplateFile "<path>\template.json" -TemplateParameterFile "<path>\parameters.json"
    ``` 

    Azure Resource Manager zjistí, že tento příkaz aktualizaci stávajícího prostředku. Zpracuje pouze změny mezi šablony řídí stávající nasazení a poskytuje novou šablonu.

## <a name="deploy-oms-by-using-azure-powershell"></a>Nasazení OMS pomocí prostředí Azure PowerShell

Analýzy protokolů OMS prostředku pomocí prostředí PowerShell můžete taky nasadit pomocí `New-AzureRmOperationalInsightsWorkspace` příkaz. Chcete-li použít tuto metodu, ujistěte se, instalaci [prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.1.1). Vytvořit nový pracovní prostor analýzy protokolů OMS a přidejte do ní řešení Service Fabric pomocí tohoto skriptu: 

```PowerShell

$SubscriptionName = "<Name of your subscription>"
$ResourceGroup = "<Resource group name>"
$Location = "<Resource group location>"
$WorkspaceName = "<OMS Log Analytics workspace name>"
$solution = "ServiceFabric"

# Log in to Azure and access the correct subscription
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $SubID 

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true

```

Když jste hotovi, postupujte podle kroků v předchozím oddílu pro připojení k účtu příslušné úložiště analýzy protokolů OMS.

Můžete také přidat další řešení nebo provádět další úpravy vaším pracovním prostorem OMS pomocí prostředí PowerShell. Další informace najdete v tématu [Spravovat analýzy protokolů pomocí prostředí PowerShell](../log-analytics/log-analytics-powershell-workspace-configuration.md).

## <a name="next-steps"></a>Další postup
* [Nasazení agenta OMS](service-fabric-diagnostics-oms-agent.md) na uzly shromáždit čítače výkonu a shromažďovat protokoly pro vaše kontejnery a statistiky docker
* Získat familiarized s [vyhledávání a dotazování protokolu](../log-analytics/log-analytics-log-searches.md) funkcím poskytovaným jako součást analýzy protokolů
* [Pomocí zobrazení návrhu můžete vytvořit vlastní zobrazení v analýzy protokolů](../log-analytics/log-analytics-view-designer.md)
