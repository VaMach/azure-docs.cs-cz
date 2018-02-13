---
title: "Azure Service Fabric - nastavení monitorování s OMS Log Analytics | Microsoft Docs"
description: "Zjistěte, jak nastavit OMS vizualizaci a analýzu událostí pro monitorování clusterů Azure Service Fabric."
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
ms.openlocfilehash: 53b06c5a1395f34c96d4011366835a920d5c670b
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
# <a name="set-up-oms-log-analytics-for-a-cluster"></a>Nastavit OMS analýzy protokolů pro cluster

Můžete nastavit pracovní prostor OMS pomocí Azure Resource Manager, prostředí PowerShell, nebo prostřednictvím Azure Marketplace. Pokud udržujete aktualizovanou šablonu Resource Manager vašeho nasazení pro budoucí použití, pomocí stejné šablony nastavení prostředí OMS. Nasazení prostřednictvím Marketplace je jednodušší, pokud již máte cluster nasadit s diagnostikou povolena. V případě nemají předplatné úroveň přístupu účtu, do které nasazujete OMS pomocí prostředí PowerShell nebo nasadit pomocí šablony Resource Manageru.

> [!NOTE]
> Je potřeba mít diagnostiky povolené pro váš cluster zobrazíte clusteru nebo úroveň události platformy být schopni nastavit OMS na úspěšně monitorování vašeho clusteru.

## <a name="deploying-oms-using-azure-marketplace"></a>Nasazení OMS pomocí webu Azure Marketplace

Pokud chcete přidat pracovním prostorem OMS po nasazení clusteru, přejděte na Azure Marketplace (na portálu) a vyhledejte *"Service Fabric Analytics."*

1. Klikněte na **nový** v levé navigační nabídce. 

2. Vyhledejte *služby Fabric Analytics*. Klikněte na prostředek, který se zobrazí.

3. Klikněte na **vytvořit**

    ![Analýza SF OMS v Marketplace.](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. V okně vytváření Service Fabric Analytics klikněte na **vyberte pracovní prostor** pro *pracovním prostorem OMS* pole a potom **vytvořit nový pracovní prostor**. Vyplnit požadované položky – tady Jediným požadavkem je, že předplatné pro cluster Service Fabric a pracovním prostorem OMS musí být stejná. Po ověření vaší položky se spustí pracovní prostor OMS nasazení. To by měl jenom pár minut trvat.

5. Po dokončení klikněte na tlačítko **vytvořit** znovu v dolní části okna Vytvoření Service Fabric Analytics. Ujistěte se, že nový pracovní prostor se zobrazí v části *pracovním prostorem OMS*. Toto řešení přidá do pracovního prostoru, který jste právě vytvořili.

Pokud používáte systém Windows, pokračujte v následujících krocích spojit OMS k účtu úložiště, kde jsou uloženy vaše události clusteru. Povolení této funkce správně pro Linux clustery je stále probíhá. Mezitím pokračujte přidáním agenta OMS do clusteru.  

1. V pracovním prostoru stále musí být připojen k diagnostiky dat pocházejících z clusteru. Přejděte do skupiny prostředků, kterou jste vytvořili řešení Service Fabric analýzy v. Měli byste vidět *ServiceFabric (\<nameOfOMSWorkspace\>)*. Klikněte na řešení, přejděte na stránku s jeho přehled, ze kterých můžete změnit nastavení řešení, nastavení pracovního prostoru a přejděte na portálu OMS.

2. V levém navigační nabídce klikněte na **účtů úložiště protokolů**v části *zdroje dat pracovního prostoru*.

3. Na *protokol účtu úložiště* klikněte na tlačítko **přidat** v horní části přidat váš cluster protokoly do pracovního prostoru.

4. Klikněte na tlačítko do **účet úložiště** přidat odpovídající účet vytvořený v clusteru. Pokud jste použili výchozí název, název účtu úložiště *sfdg\<resourceGroupName\>*. Můžete to ověřit také kontrolou šablony Azure Resource Manageru, které jsou používány k nasazení kontrolou hodnota používaná pro cluster, `applicationDiagnosticsStorageAccountName`. Možná budete také muset přejděte dolů a klikněte na tlačítko **načtěte více** Pokud název nezobrazuje. Klikněte na název účtu úložiště vpravo si vyberte.

5. Potom budete muset zadat *datový typ*, který by měl být nastaven na **události prostředků infrastruktury služby**.

6. *Zdroj* automaticky musí být nastavena na *WADServiceFabric\*EventTable*.

7. Klikněte na tlačítko **OK** připojit pracovního prostoru do protokolů vašeho clusteru.

    ![Přidat protokol účtu úložiště do OMS](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

Účet by měl nyní zobrazují jako součást vaší *protokol účtu úložiště* ve zdrojích dat pracovního prostoru.

Pomocí této nyní jste přidali řešení Service Fabric analýzy v pracovním prostoru analýzy protokolů OMS, který je nyní správně připojené k platformě váš cluster a tabulku protokolu aplikace. Další zdroje můžete přidat do pracovního prostoru stejným způsobem.


## <a name="deploying-oms-using-a-resource-manager-template"></a>Nasazení pomocí šablony Resource Manageru OMS

Při nasazování clusteru pomocí šablony Resource Manageru, šablony měli vytvořit nový pracovní prostor OMS, přidejte řešení prostředků infrastruktury služby k němu a nakonfigurujte ji číst data z tabulky odpovídající úložiště.

[Zde](https://azure.microsoft.com/resources/templates/service-fabric-oms/) je ukázka šablony, která můžete použít a změnit podle požadavků. Další šablony, které poskytují různé možnosti pro nastavení pracovním prostorem OMS naleznete na adrese [Service Fabric a OMS šablony](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

Hlavní změny jsou následující:

1. Přidat `omsWorkspaceName` a `omsRegion` na parametry. To znamená, že přidáním následující fragment kódu do parametrech definovaných v vaše *template.json* souboru. Nebojte se, že podle potřeby upravte výchozí hodnoty. Měli byste také přidat dva nové parametry ve vaší *Parameters.JSON tímto kódem* zadat jejich hodnoty pro nasazení prostředků:
    
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

    `omsRegion` Hodnoty musí odpovídat na konkrétní sadu hodnot. Měli byste vybrat ten, který je nejblíže k nasazení clusteru.

2. Pokud budete k OMS odesílat všechny protokoly aplikací, ujistěte se, že `applicationDiagnosticsStorageAccountType` a `applicationDiagnosticsStorageAccountName` jsou zahrnuty jako parametry v šabloně. Pokud tomu tak není, přidejte je do části proměnné takto a upravit jejich hodnoty podle potřeby. Můžete taky zahrnout je jako parametry, pokud chcete, ve formátu použít výše.

    ```json
    "applicationDiagnosticsStorageAccountType": "Standard_LRS",
    "applicationDiagnosticsStorageAccountName": "[toLower(concat('oms', uniqueString(resourceGroup().id), '3' ))]"
    ```

3. Přidáte řešení Service Fabric OMS do vaší šablony proměnné:

    ```json
    "solution": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "solutionName": "ServiceFabric"
    ```

4. Přidání následujících na konci části vaše prostředky, po kterém je deklarovaná prostředek clusteru Service Fabric.

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

5. Nasazení šablony jako upgrade správce prostředků do clusteru. To se provádí pomocí `New-AzureRmResourceGroupDeployment` rozhraní API v modulu AzureRM prostředí PowerShell. Příkaz příkladu by byl:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName "sfcluster1" -TemplateFile "<path>\template.json" -TemplateParameterFile "<path>\parameters.json"
    ``` 

    Azure Resource Manager bude schopna zjistit, že se jedná o aktualizaci stávajícího prostředku. Zpracována bude pouze změny mezi šablony řídí stávající nasazení a poskytuje novou šablonu.

## <a name="deploying-oms-using-azure-powershell"></a>Nasazení OMS pomocí Azure PowerShell

Můžete také nasadit analýzy protokolů OMS prostředku pomocí prostředí PowerShell. Toho dosáhnete pomocí `New-AzureRmOperationalInsightsWorkspace` příkaz. Chcete-li to provést, zajistěte, aby instalaci [prostředí Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.1.1). Vytvořit nový pracovní prostor analýzy protokolů OMS a přidejte do ní řešení Service Fabric pomocí tohoto skriptu: 

```ps

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

Až to uděláte, když je váš cluster clusteru se systémem Windows, postupujte podle kroků v části výše spojit OMS Log Analytics k účtu odpovídající úložiště.

Můžete také přidat další řešení nebo provádět další úpravy vaším pracovním prostorem OMS pomocí prostředí PowerShell. Další informace o tom, najdete v části [Spravovat analýzy protokolů pomocí prostředí PowerShell](../log-analytics/log-analytics-powershell-workspace-configuration.md)

## <a name="next-steps"></a>Další kroky
* [Nasazení agenta OMS](service-fabric-diagnostics-oms-agent.md) na uzly shromáždit čítače výkonu a shromažďovat protokoly pro vaše kontejnery a statistiky docker
* Získat familiarized s [vyhledávání a dotazování protokolu](../log-analytics/log-analytics-log-searches.md) funkcím poskytovaným jako součást analýzy protokolů
* [Pomocí zobrazení návrhu můžete vytvořit vlastní zobrazení v analýzy protokolů](../log-analytics/log-analytics-view-designer.md)
