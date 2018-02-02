---
title: "Azure schématu události událostí mřížky"
description: "Popisuje vlastnosti, které jsou k dispozici pro události se Azure událostí mřížky"
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: babanisa
ms.openlocfilehash: 2b0039c7b90ef6f003641e096521f84885171c26
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="azure-event-grid-event-schema"></a>Azure schématu události událostí mřížky

Tento článek popisuje vlastnosti a schéma, které jsou k dispozici pro všechny události. Události obsahují sadu pěti řetězec požadované vlastnosti a objekt požadovaná data. Vlastnosti jsou společné pro všechny události z libovolného vydavatele. Datový objekt obsahuje vlastnosti, které jsou specifické pro každý vydavatele. Témata systému tyto vlastnosti jsou specifické pro poskytovatele prostředků, jako je například Azure Storage nebo Azure Event Hubs.

Události se posílají do Azure událostí mřížky ve pole, která může obsahovat více objektů událostí. Pokud existuje jenom jedna událost, má pole o délce 1. Pole může mít celková velikost až 1 MB. Každá událost v poli je omezená na 64 KB.

Můžete najít schéma JSON pro událost událostí mřížky a každý Azure vydavatele datová část [úložiště schématu události](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/eventgrid/data-plane).

## <a name="event-schema"></a>Schéma událostí

Následující příklad ukazuje vlastnosti, které jsou používány všechny zdroje událostí:

```json
[
  {
    "topic": string,
    "subject": string,    
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```

Schéma publikována pro událost úložiště objektů Blob v Azure je například:

```json
[
  {
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
    "subject": "/blobServices/default/containers/oc2d2817345i200097container/blobs/oc2d2817345i20002296blob",
    "eventType": "Microsoft.Storage.BlobCreated",
    "eventTime": "2017-06-26T18:41:00.9584103Z",
    "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
      "api": "PutBlockList",
      "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
      "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
      "eTag": "0x8D4BCC2E4835CD0",
      "contentType": "application/octet-stream",
      "contentLength": 524288,
      "blobType": "BlockBlob",
      "url": "https://oc2d2817345i60006.blob.core.windows.net/oc2d2817345i200097container/oc2d2817345i20002296blob",
      "sequencer": "00000000000004420000000000028963",
      "storageDiagnostics": {
        "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
      }
    },
    "dataVersion": "",
    "metadataVersion": "1"
  }
]
```
 
## <a name="event-properties"></a>Vlastnosti události

Všechny události obsahovat stejné nejvyšší úrovně následující data:

| Vlastnost | Typ | Popis |
| -------- | ---- | ----------- |
| Téma | řetězec | Úplné prostředků cesta ke zdroji událostí. Toto pole není možné zapisovat. Událost mřížky poskytuje tuto hodnotu. |
| Předmět | řetězec | Cesta definované vydavatele události předmět. |
| eventType | řetězec | Jeden z typů událostí registrovaných pro tento zdroj událostí. |
| eventTime | řetězec | Čas, který se vygeneruje událost založené na čas UTC poskytovatele. |
| id | řetězec | Jedinečný identifikátor pro událost. |
| data | Objekt | Data události specifické pro poskytovatele prostředků. |
| dataVersion | řetězec | Verze schématu datového objektu. Vydavatel definuje verze schématu. |
| metadataVersion | řetězec | Verze schématu metadat událostí. Událost mřížky definuje schéma vlastnosti nejvyšší úrovně. Událost mřížky poskytuje tuto hodnotu. |

Další informace o vlastnosti v objektu data, najdete v části Zdroj události:

* [Předplatná Azure (operace správy)](event-schema-subscriptions.md)
* [Blob Storage](event-schema-blob-storage.md)
* [Služba Event hubs](event-schema-event-hubs.md)
* [Skupiny prostředků (operace správy)](event-schema-resource-groups.md)

Pro vlastní témata určuje vydavatel události datový objekt. Nejvyšší úrovně data by měla obsahovat stejná pole jako standardní událostí definovaných prostředků. Při publikování událostí do vlastní témata, měli byste zvážit modelování předmět události pro usnadnění směrování a filtrování.

## <a name="next-steps"></a>Další postup

* Úvod do Azure událostí mřížky, najdete v části [co je mřížky událostí?](overview.md)
* Další informace o vytváření předplatného služby Azure událostí mřížky, najdete v části [schématu odběru událostí mřížky](subscription-creation-schema.md).
