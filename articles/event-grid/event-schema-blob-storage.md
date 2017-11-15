---
title: "Azure schématu události úložiště objektů blob událostí mřížky"
description: "Popisuje vlastnosti, které jsou k dispozici pro události úložiště objektů blob s Azure událostí mřížky"
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: bdc64733b75fd809cf0245986aa96370343c1a34
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/11/2017
---
# <a name="azure-event-grid-event-schema-for-blob-storage"></a>Azure schématu události událostí mřížky pro úložiště objektů Blob

Tento článek poskytuje vlastnosti a schématu pro události úložiště objektů blob. Úvod do schémata událostí, naleznete v části [schématu události mřížky událostí Azure](event-schema.md).

## <a name="available-event-types"></a>Typy událostí k dispozici

Úložiště objektů BLOB vysílá následující typy událostí:

| Typ události | Popis |
| ---------- | ----------- |
| Microsoft.Storage.BlobCreated | Vyvolá, když je vytvořen objekt blob. |
| Microsoft.Storage.BlobDeleted | Vyvolá, když je odstraněn objekt blob. |

## <a name="example-event"></a>Příklad událostí

Následující příklad ukazuje schématu objektu blob vytvořit událost: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "0x8D4BCC2E4835CD0",
    "contentType": "text/plain",
    "contentLength": 524288,
    "blobType": "BlockBlob",
    "url": "https://example.blob.core.windows.net/testcontainer/testfile.txt",
    "sequencer": "00000000000004420000000000028963",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  }
}]
```

Schéma pro události odstranit objekt blob je podobný: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobDeleted",
  "eventTime": "2017-11-07T20:09:22.5674003Z",
  "id": "4c2359fe-001e-00ba-0e04-58586806d298",
  "data": {
    "api": "DeleteBlob",
    "requestId": "4c2359fe-001e-00ba-0e04-585868000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://example.blob.core.windows.net/testcontainer/testfile.txt",
    "sequencer": "0000000000000281000000000002F5CA",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  }
}]
```
 
## <a name="event-properties"></a>Vlastnosti události

Událost má následující dat nejvyšší úrovně:

| Vlastnost | Typ | Popis |
| -------- | ---- | ----------- |
| Téma | Řetězec | Úplné prostředků cesta ke zdroji událostí. Toto pole není možné zapisovat. |
| Předmět | Řetězec | Cesta definované vydavatele události předmět. |
| Typ události | Řetězec | Jeden z typů událostí registrovaných pro tento zdroj událostí. |
| eventTime | Řetězec | Čas, který se vygeneruje událost založené na čas UTC poskytovatele. |
| id | Řetězec | Jedinečný identifikátor pro událost. |
| data | Objekt | Data události úložiště objektů BLOB. |

Datový objekt má následující vlastnosti:

| Vlastnost | Typ | Popis |
| -------- | ---- | ----------- |
| rozhraní api | Řetězec | Operace, která spustí událost. |
| clientRequestId | Řetězec | Hodnota klientem generovaná, neprůhledné se omezena na 1 KB znaků. Pokud jste povolili protokolování analytika úložiště, zaznamenává se v protokolech analýzy. |
| ID žádosti | Řetězec | Jedinečný identifikátor pro požadavek. Použijte pro řešení potíží s žádosti. |
| Značka ETag | Řetězec | Hodnota, která můžete použít k provádění operací podmíněně. |
| Typ obsahu | Řetězec | Typ obsahu zadaný pro tento objekt blob. |
| contentLength | celé číslo | Velikost objektu blob v bajtech. |
| BlobType | Řetězec | Typ objektu blob. |
| Adresa URL | Řetězec | Cesta k objektu blob. |
| aplikace Sequencer | Řetězec | Hodnota uživatele řídí, která můžete použít ke sledování požadavků. |
| storageDiagnostics | Objekt | Informace o diagnostiku storage. |
 
## <a name="next-steps"></a>Další kroky

* Úvod do Azure událostí mřížky, najdete v části [co je mřížky událostí?](overview.md)
* Další informace o vytváření předplatného služby Azure událostí mřížky, najdete v části [schématu odběru událostí mřížky](subscription-creation-schema.md).
* Úvod k práci s událostmi úložiště objektů blob, najdete v části [události úložiště objektů Blob trasy - rozhraní příkazového řádku Azure](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json). 
