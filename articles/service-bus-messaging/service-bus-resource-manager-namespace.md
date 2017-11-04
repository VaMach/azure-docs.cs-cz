---
title: "Vytvoření oboru názvů Service Bus pomocí šablony Azure Resource Manager | Microsoft Docs"
description: "Vytvoření oboru názvů Service Bus pomocí šablony Azure Resource Manageru"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: dc0d6482-6344-4cef-8644-d4573639f5e4
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: 0598ee93a38c07aa7b1102cdaf228c2a4b4dcf71
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Vytvoření oboru názvů Service Bus pomocí šablony Azure Resource Manager

Tento článek popisuje, jak používat šablonu Azure Resource Manager, která vytvoří obor názvů sběrnice typu **zasílání zpráv** pomocí standardní SKU. V článku také definuje parametry, které jsou určené pro spuštění nasazení. Tuto šablonu můžete použít pro vlastní nasazení nebo ji upravit, aby splňovala vaše požadavky.

Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru][Authoring Azure Resource Manager templates].

Úplnou šablonu, najdete v článku [šablony oboru názvů Service Bus] [ Service Bus namespace template] na Githubu.

> [!NOTE]
> Následující šablony Azure Resource Manager jsou k dispozici ke stažení a nasazení. 
> 
> * [Vytvoření oboru názvů Service Bus pomocí fronty](service-bus-resource-manager-namespace-queue.md)
> * [Vytvoření oboru názvů Service Bus s téma a odběr](service-bus-resource-manager-namespace-topic.md)
> * [Vytvoření oboru názvů Service Bus pomocí fronty a autorizační pravidla](service-bus-resource-manager-namespace-auth-rule.md)
> * [Vytvoření oboru názvů Service Bus pomocí tématu, předplatné a pravidla](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> Vyhledat nejnovější šablony, najdete [šablon Azure rychlý Start] [ Azure Quickstart Templates] galerie a vyhledejte Service Bus.
> 
> 

## <a name="what-will-you-deploy"></a>Co budete nasazovat?
Pomocí této šablony, v oboru názvů Service Bus s nasazením [Standard nebo Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU.

Pokud chcete nasazení spustit automaticky, klikněte na následující tlačítko:

[![Nasazení do Azure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry
Pomocí Azure Resource Manageru definujete parametry pro hodnoty, které chcete zadat při nasazení šablony. Šablona obsahuje oddíl s názvem `Parameters` obsahující všechny hodnoty parametru. Měli byste parametr pro ty hodnoty, které se liší podle prostředí, ve kterém provádíte nasazení nebo na základě projektu, které nasazujete. Nedefinují parametry pro hodnoty, které zůstanou vždy stejná. Každá hodnota parametru se v šabloně použije k definování nasazovaných prostředků.

Tato šablona definuje následující parametry.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
Název oboru názvů Service Bus k vytvoření.

```json
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebussku"></a>serviceBusSKU
Název služby Service Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) k vytvoření.

```json
"serviceBusSku": { 
    "type": "string", 
    "allowedValues": [ 
        "Standard",
        "Premium" 
    ], 
    "defaultValue": "Standard", 
    "metadata": { 
        "description": "The messaging tier for service Bus namespace" 
    } 

```

Šablona definuje hodnoty, které jsou povoleny pro tento parametr (Standard nebo Premium) a přiřadí výchozí hodnotu (Standard), pokud není zadaná žádná hodnota.

Další informace o cenách služby Service Bus najdete v tématu [Service Bus ceny a fakturace][Service Bus pricing and billing].

### <a name="servicebusapiversion"></a>serviceBusApiVersion
Verze rozhraní API služby Service Bus šablony.

```json
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       } 
```

## <a name="resources-to-deploy"></a>Prostředky k nasazení
### <a name="service-bus-namespace"></a>Obor názvů Service Bus
Vytvoří standardní oboru názvů Service Bus typu **zasílání zpráv**.

```json
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "properties": {
        }
    }
]
```

## <a name="commands-to-run-deployment"></a>Příkazy pro spuštění nasazení
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a>Azure CLI
```azurecli
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a>Další kroky
Teď, když jste vytvoření a nasazení prostředků pomocí Azure Resource Manager, zjistěte, jak tyto zdroje spravovat pomocí čtení těchto článcích:

* [Správa služby Service Bus pomocí prostředí PowerShell](service-bus-manage-with-ps.md)
* [Správa prostředků služby Service Bus pomocí Průzkumníka služby sběrnice](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace template]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Service Bus pricing and billing]: service-bus-pricing-billing.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
