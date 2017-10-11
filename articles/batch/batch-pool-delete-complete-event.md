---
title: "Odstranit událost complete fondu Azure Batch | Microsoft Docs"
description: "Referenční dokumentace pro fondu Batch odstranit událost complete."
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
ms.openlocfilehash: 890f2ba7fda37060c56177868d6214d517d91831
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="pool-delete-complete-event"></a>Událost complete odstranění fondu

 Tato událost je vygenerované při operaci odstranění fondu byla dokončena.

 Následující příklad ukazuje text událost complete odstranění fondu.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Element|Typ|Poznámky|
|-------------|----------|-----------|
|id|Řetězec|Id fondu.|
|startTime|Data a času|Odstranění fondu čas spuštění.|
|čas ukončení|Data a času|Čas odstranění fondu byla dokončena.|

## <a name="remarks"></a>Poznámky
Další informace o stavu a kódy chyb pro operace změny velikosti fondu najdete v tématu [odstranění fondu z účtu](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).