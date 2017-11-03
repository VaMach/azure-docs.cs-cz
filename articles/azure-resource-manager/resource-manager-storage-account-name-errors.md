---
title: "Chyby název účtu úložiště Azure | Microsoft Docs"
description: "Popisuje chyby, ke kterým může dojít při zadávání názvu účtu úložiště."
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
ms.openlocfilehash: dc045827fbd38054a334ff22eb30e0db6a31bac8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-storage-account-names"></a>Vyřešte chyby pro názvy účtů úložiště

Tento článek popisuje pojmenování chyb, které se můžete setkat při nasazování účet úložiště.

## <a name="symptom"></a>Příznaky

Pokud váš název účtu úložiště obsahuje zakázané znaky, obdržíte chybu jako:

```
Code=AccountNameInvalid
Message=S!torageckrexph7isnoc is not a valid storage account name. Storage account name must be 
between 3 and 24 characters in length and use numbers and lower-case letters only.
```

Pro účty úložiště je nutné zadat název pro prostředek, který je v rámci Azure jedinečný. Pokud nezadáte jedinečný název, zobrazí chybu jako:

```
Code=StorageAccountAlreadyTaken
Message=The storage account named mystorage is already taken.
```

Pokud nasazení účtu úložiště se stejným názvem jako stávající účet úložiště v rámci vašeho předplatného, ale zadejte jiné umístění, zobrazí se chyba oznamující, že účet úložiště již existuje v jiném umístění. Buď existující účet úložiště odstranit, nebo zadejte stejné umístění jako stávající účet úložiště.

## <a name="cause"></a>Příčina

Názvy účtů úložiště musí být v rozmezí 3 až 24 znaků a používat jenom číslice a malá písmena. Název musí být jedinečný.

## <a name="solution"></a>Řešení

### <a name="solution-1"></a>Řešení 1

Ujistěte se, že název účtu úložiště je jedinečný. Můžete vytvořit jedinečný název zřetězení vaše zásady vytváření názvů s výsledkem [uniqueString](resource-group-template-functions-string.md#uniquestring) funkce.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

### <a name="solution-2"></a>Řešení 2

Ujistěte se, že váš název účtu úložiště není delší než 24 znaků. [UniqueString](resource-group-template-functions-string.md#uniquestring) funkce vrátí 13 znaků. Pokud řetězení předpony nebo přípony k **uniqueString** vést, zadejte hodnotu, která je 11 znaků nebo méně.

```json
"parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    }
}
```

### <a name="solution-3"></a>Řešení 3

Ujistěte se, že váš název účtu úložiště nezahrnuje všechny elká písmena nebo speciální znaky.