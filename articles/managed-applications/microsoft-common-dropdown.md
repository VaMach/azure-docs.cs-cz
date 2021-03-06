---
title: "Azure spravované aplikace rozevírací prvek uživatelského rozhraní | Microsoft Docs"
description: "Popisuje element Microsoft.Common.DropDown uživatelského rozhraní pro spravované aplikace Azure"
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: a3cbb7363c04043eb253e91e058753bc271e4e01
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftcommondropdown-ui-element"></a>Element Microsoft.Common.DropDown uživatelského rozhraní
Výběr ovládacího prvku s rozevíracím seznamu. Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](publish-service-catalog-app.md).

## <a name="ui-sample"></a>Ukázka uživatelského rozhraní
![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a>Schéma
```json
{
  "name": "element1",
  "type": "Microsoft.Common.DropDown",
  "label": "Some drop down",
  "defaultValue": "Foo",
  "toolTip": "",
  "constraints": {
    "allowedValues": [
      {
        "label": "Foo",
        "value": "Bar"
      },
      {
        "label": "Baz",
        "value": "Qux"
      }
    ]
  },
  "visible": true
}
```

## <a name="remarks"></a>Poznámky
- Popisek pro `constraints.allowedValues` je zobrazený text pro položku, a jeho hodnota může být výstupní hodnotu elementu při výběru.
- -Li zadána, výchozí hodnota musí být součástí štítek `constraints.allowedValues`. Pokud není zadaný, první položky v `constraints.allowedValues` je vybrána. Výchozí hodnota je **null**.
- `constraints.allowedValues`musí obsahovat alespoň jednu položku.
- Tento element nepodporuje `constraints.required` vlastnost. Emulovat toto chování, přidejte položku s popisek a hodnota `""` (prázdný řetězec) k `constraints.allowedValues`.

## <a name="sample-output"></a>Ukázkový výstup
```json
"Bar"
```

## <a name="next-steps"></a>Další kroky
* Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](overview.md).
* Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](create-uidefinition-overview.md).
* Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](create-uidefinition-elements.md).
