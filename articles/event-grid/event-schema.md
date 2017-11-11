---
title: "Azure schématu události událostí mřížky"
description: "Popisuje vlastnosti, které jsou k dispozici pro události se Azure událostí mřížky"
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 11/07/2017
ms.author: babanisa
ms.openlocfilehash: caa709fdc2a59472ee812bde91f7300396aa5755
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/11/2017
---
# <a name="azure-event-grid-event-schema"></a>Azure schématu události událostí mřížky

Tento článek popisuje vlastnosti a schéma, které jsou k dispozici pro všechny události. Události obsahují sadu pěti řetězec požadované vlastnosti a objekt požadovaná data. Vlastnosti jsou společné pro všechny události z libovolného vydavatele. Datový objekt obsahuje vlastnosti, které jsou specifické pro každý vydavatele. Témata systému tyto vlastnosti jsou specifické pro poskytovatele prostředků, jako je například Azure Storage nebo Azure Event Hubs.

Události se posílají do Azure událostí mřížky ve pole, která může obsahovat více objektů událostí. Pokud existuje jenom jedna událost, má pole o délce 1. Pole může mít celková velikost až 1 MB. Každá událost v poli je omezená na 64 KB.

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
    }
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
    }
  }
]
```
 
## <a name="event-properties"></a>Vlastnosti události

Všechny události obsahovat stejné nejvyšší úrovně následující data:

| Vlastnost | Typ | Popis |
| -------- | ---- | ----------- |
| Téma | Řetězec | Úplné prostředků cesta ke zdroji událostí. Toto pole není možné zapisovat. |
| Předmět | Řetězec | Cesta definované vydavatele události předmět. |
| Typ události | Řetězec | Jeden z typů událostí registrovaných pro tento zdroj událostí. |
| eventTime | Řetězec | Čas, který se vygeneruje událost založené na čas UTC poskytovatele. |
| id | Řetězec | Jedinečný identifikátor pro událost. |
| data | Objekt | Data události specifické pro poskytovatele prostředků. |

Další informace o vlastnosti v objektu data, najdete v části Zdroj události:

* [Předplatná Azure (operace správy)](event-schema-subscriptions.md)
* [Blob Storage](event-schema-blob-storage.md)
* [Služba Event hubs](event-schema-event-hubs.md)
* [Skupiny prostředků (operace správy)](event-schema-resource-groups.md)

Pro vlastní témata určuje vydavatel události datový objekt. Nejvyšší úrovně data by měla obsahovat stejná pole jako standardní událostí definovaných prostředků. Při publikování událostí do vlastní témata, měli byste zvážit modelování předmět události pro usnadnění směrování a filtrování.

## <a name="next-steps"></a>Další kroky

* Úvod do Azure událostí mřížky, najdete v části [co je mřížky událostí?](overview.md)
* Další informace o vytváření předplatného služby Azure událostí mřížky, najdete v části [schématu odběru událostí mřížky](subscription-creation-schema.md).
