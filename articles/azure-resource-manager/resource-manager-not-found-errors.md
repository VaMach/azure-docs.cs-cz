---
title: "Chyby nenalezení prostředků Azure | Microsoft Docs"
description: "Popisuje, jak vyřešit chyby, když prostředek nebyl nalezen."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: c76c965c43ca8217faa9488c01975ce09a21daaf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-not-found-errors-for-azure-resources"></a>Vyřešte chyby nenalezení pro prostředky Azure

Tento článek popisuje chyb, které může dojít, pokud prostředek nelze nalézt během nasazení. 

## <a name="symptom"></a>Příznaky

Pokud vaše šablona zahrnuje název prostředku, které nelze vyřešit, obdržíte chybu podobně jako:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

Pokud pokus o použití [odkaz](resource-group-template-functions-resource.md#reference) nebo [listKeys](resource-group-template-functions-resource.md#listkeys) funkce s prostředky, které nelze vyřešit, zobrazí se následující chyba:

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

## <a name="cause"></a>Příčina

Správce prostředků je potřeba načíst vlastnosti pro prostředek, ale nemůže identifikovat prostředků ve vašem předplatném.

## <a name="solution"></a>Řešení

### <a name="solution-1"></a>Řešení 1

Pokud se pokoušíte nasadit chybějící prostředek v šabloně, zkontrolujte, jestli je potřeba přidat závislost. Správce prostředků optimalizuje nasazení tak, že vytvoříte prostředky paralelně, pokud je to možné. Pokud po jiný prostředek musí být nasazený jeden prostředek, budete muset použít **dependsOn** element v šabloně pro vytvoření závislosti na jiný prostředek. Například Pokud nasazujete webovou aplikaci, musí existovat plán služby App Service. Pokud jste nezadali, že webová aplikace závisí na plán služby App Service, vytvoří Resource Manager i prostředky ve stejnou dobu. Zobrazí se chyba oznamující, že nelze najít prostředek plánu služby App Service, protože neexistuje ještě při pokusu o nastavení vlastnosti ve webové aplikaci. Tuto chybu můžete zabránit nastavením závislost ve webové aplikaci.

```json
{
  "apiVersion": "2015-08-01",
  "type": "Microsoft.Web/sites",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

Návrhy na řešení potíží s chybami závislostí, najdete v části [Zkontrolujte pořadí nasazení](resource-manager-troubleshoot-tips.md#check-deployment-sequence).

### <a name="solution-2"></a>Řešení 2

Pokud existuje v jiné skupině prostředků než ten, který provádí jeho nasazení na prostředek, použijte [resourceId funkce](resource-group-template-functions-resource.md#resourceid) získat plně kvalifikovaný název prostředku.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

### <a name="solution-3"></a>Řešení 3

Vyhledat výraz, který zahrnuje [odkaz](resource-group-template-functions-resource.md#reference) funkce. Hodnoty, které zadáte lišit v závislosti na tom, zda prostředek je ve stejné šablony, skupinu prostředků a předplatného. Zkontrolujte, že poskytnete požadovaný parametr hodnoty pro váš scénář. Pokud je v jiné skupině prostředků, zadejte ID úplné prostředku. Chcete-li účet úložiště v jiné skupině prostředků, například použijte:

```json
"[reference(resourceId('exampleResourceGroup', 'Microsoft.Storage/storageAccounts', 'myStorage'), '2017-06-01')]"
```