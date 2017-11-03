---
title: "Schéma předplatné Azure událostí mřížky"
description: "Popisuje vlastnosti odběru pro událost s událostí mřížky Azure."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/17/2017
ms.author: babanisa
ms.openlocfilehash: eff2352066a76010d6d882a7b7e1961870cd2d46
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="event-grid-subscription-schema"></a>Schématu předplatné mřížky události

Pokud chcete vytvořit předplatné událostí mřížky, odeslat požadavek na operaci vytvoření události odběru. Použijte následující formát:

```
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

Můžete například vytvořit odběr událostí pro účet úložiště s názvem `examplestorage` ve skupině prostředků s názvem `examplegroup`, použijte následující formát:

```
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

Článek popisuje vlastnosti a schéma pro text žádosti.
 
## <a name="event-subscription-properties"></a>Vlastnosti odběru událostí

| Vlastnost | Typ | Popis |
| -------- | ---- | ----------- |
| Cílový | Objekt | Objekt, který definuje koncový bod. |
| Filtr | Objekt | Volitelné pole pro filtrování typy událostí. |

### <a name="destination-object"></a>cílový objekt

| Vlastnost | Typ | Popis |
| -------- | ---- | ----------- |
| endpointType | Řetězec | Typ koncového bodu pro předplatné (webhooku nebo HTTP, centra událostí nebo fronty). | 
| Adresa URL koncového bodu | Řetězec |  | 

### <a name="filter-object"></a>objekt filtru

| Vlastnost | Typ | Popis |
| -------- | ---- | ----------- |
| includedEventTypes | Pole | Shoda, když typ události ve zprávě události je přesnou shodou jeden z těchto názvů typu události. Vyvolá chybu, pokud název události neodpovídá typu názvy registrovaných událostí pro zdroj události. Výchozí odpovídá všechny typy událostí. |
| subjectBeginsWith | Řetězec | Shoda předpony filtrovat pole předmět události zprávy. Výchozí nebo prázdný řetězec odpovídá všem. | 
| subjectEndsWith | Řetězec | Přípona match filtrovat pole předmět události zprávy. Výchozí nebo prázdný řetězec odpovídá všem. |
| subjectIsCaseSensitive | Řetězec | Ovládací prvky malá a velká písmena odpovídající pro filtry. |


## <a name="example-subscription-schema"></a>Příklad předplatné schématu

```json
{
  "properties": {
    "destination": {
      "endpointType": "webhook",
      "properties": {
          "endpointUrl": "https://example.azurewebsites.net/api/HttpTriggerCSharp1?code=VXbGWce53l48Mt8wuotr0GPmyJ/nDT4hgdFj9DpBiRt38qqnnm5OFg=="
      }
    },
    "filter": {
      "includedEventTypes": [ "blobCreated", "blobDeleted" ],
      "subjectBeginsWith": "blobServices/default/containers/mycontainer/log",
      "subjectEndsWith": ".jpg",
      "subjectIsCaseSensitive": "true"
    }
  }
}
```

## <a name="next-steps"></a>Další kroky

* Úvod k mřížce událostí, naleznete v části [co je mřížky událostí?](overview.md)