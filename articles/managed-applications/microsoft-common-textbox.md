---
title: "Azure spravované aplikace textového elementu uživatelského rozhraní | Microsoft Docs"
description: "Popisuje element Microsoft.Common.TextBox uživatelského rozhraní pro spravované aplikace Azure"
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
ms.openlocfilehash: d3fae42ae202fe720761382e1020fa8bd8c62b44
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftcommontextbox-ui-element"></a>Element Microsoft.Common.TextBox uživatelského rozhraní
Ovládací prvek, který lze upravit neformátovaný text. Pomocí tohoto prvku při [vytváření spravovaných aplikací Azure](publish-service-catalog-app.md).

## <a name="ui-sample"></a>Ukázka uživatelského rozhraní
![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a>Schéma
```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "foobar",
  "toolTip": "Halp!",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "visible": true
}
```

## <a name="remarks"></a>Poznámky
- Pokud `constraints.required` je nastaven na **true**, pak textového pole musí obsahovat hodnotu úspěšně ověřit. Výchozí hodnota je **false**.
- `constraints.regex`je vzor regulárního výrazu jazyka JavaScript. -Li zadána, hodnota textového pole musí odpovídat vzorku úspěšně ověřit. Výchozí hodnota je **null**.
- `constraints.validationMessage`je řetězec k zobrazení při ověřování se nezdaří, jeho hodnotu. Pokud není zadaný, se používají zpráv integrované ověření textového pole. Výchozí hodnota je **null**.
- Je možné zadat hodnotu pro `constraints.regex` při `constraints.required` je nastaven na **false**. V tomto scénáři se hodnota nevyžaduje pro textové pole úspěšně ověřit. Pokud je zadaná, musí se shodovat regulární výraz.

## <a name="sample-output"></a>Ukázkový výstup

```json
"foobar"
```

## <a name="next-steps"></a>Další kroky
* Úvod do spravovaných aplikací, najdete v části [Azure spravovaných aplikací – přehled](overview.md).
* Úvod do vytváření definic uživatelského rozhraní, najdete v části [Začínáme s CreateUiDefinition](create-uidefinition-overview.md).
* Popis společných vlastností v prvky uživatelského rozhraní najdete v tématu [CreateUiDefinition elementy](create-uidefinition-elements.md).
