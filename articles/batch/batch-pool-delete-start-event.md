---
title: "Události fondu počáteční delete Azure Batch | Microsoft Docs"
description: "Referenční informace pro odstranění fondu Batch, spusťte událost."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: f8a5241dce422e5c826ab428da6d7bc93284a1cf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="pool-delete-start-event"></a>Událost spuštění odstranění fondu

 Tato událost je vygenerované při operaci odstranění fondu bylo zahájeno. Vzhledem k tomu, že odstranění fondu je asynchronní událostí, můžete očekávat, že událost complete odstranění fondu pro vypuštění po dokončení operace odstranění.

 Následující příklad ukazuje tělo události spuštění odstranění fondu.

```
{
    "id": "myPool1"
}
```

|Element|Typ|Poznámky|
|-------------|----------|-----------|
|id|Řetězec|Id fondu.|