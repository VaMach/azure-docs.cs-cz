---
title: "Vytvoření aplikace logiky pomocí šablony v Azure | Microsoft Docs"
description: "Použijte šablonu Azure Resource Manager k nasazení aplikace logiky pro definování pracovních postupů."
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7574cc7c-e5a1-4b7c-97f6-0cffb1a5d536
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2016
ms.author: LADocs; mandia
ms.openlocfilehash: 161adeacd6da2b15225c8a4ddae171e19e539967
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-logic-app-using-a-template"></a>Vytvoření aplikace logiky pomocí šablony
Šablony poskytují rychlý způsob, jak použít jiný konektory v rámci aplikace logiky. Aplikace logiky zahrnuje šablon Azure Resource Manageru k vytvoření aplikace logiky, která lze použít k definování pracovních firmy. Můžete definovat prostředky, ke kterým jsou nasazeny a jak definovat parametry, když nasazujete aplikaci logiky. Můžete tuto šablonu použít pro vaše vlastní obchodní scénáře, nebo si přizpůsobit tak, aby vyhovovala vašim požadavkům.

Další informace o vlastnostech aplikace logiky získáte v tématu [rozhraní API pro správu aplikace logiky aplikace pracovního postupu](https://msdn.microsoft.com/library/azure/mt643788.aspx). 

Příklady definice samotné najdete v tématu [definice aplikace logiky Autor](logic-apps-author-definitions.md). 

Další informace o vytváření šablon najdete v tématu [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

Pro úplnou šablonu, najdete v části [šablona aplikace logiky](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).

## <a name="what-you-deploy"></a>Můžete nasadit
Pomocí této šablony můžete nasadit aplikace logiky.

Chcete-li spustit nasazení automaticky, vyberte na následující tlačítko:  

[![Nasazení do Azure](media/logic-apps-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry
[!INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri
     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }

## <a name="resources-to-deploy"></a>Prostředky k nasazení
### <a name="logic-app"></a>Aplikace logiky
Vytvoří aplikaci logiky.

Šablony používá hodnotu parametru pro název aplikace logiky. Nastaví umístění aplikaci logiky do stejného umístění jako pro skupinu prostředků. 

Definice této konkrétní spouští jednou za hodinu a odešle příkaz ping umístění zadané v **testUri** parametr. 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-to-run-deployment"></a>Příkazy pro spuštění nasazení
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup



