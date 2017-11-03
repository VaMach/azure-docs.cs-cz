---
title: "Analýza Azure Batch | Microsoft Docs"
description: "Referenční informace pro analýzu Azure Batch."
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
ms.openlocfilehash: 7d634e1bb595a84b8af339e5bc5a483a7849e7f7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="batch-analytics"></a>Analýza batch
Témata v Batch Analytics obsahovat informace o událostech a výstrahách, které jsou k dispozici pro prostředky služby Batch.

V tématu [protokolování diagnostiky Azure Batch](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) Další informace o povolení a použití Batch diagnostické protokoly.

## <a name="diagnostic-logs"></a>Diagnostické protokoly

Službu Azure Batch vysílá následující události protokolů diagnostiky po dobu životnosti určitých prostředků Batch.

**Služba Protokol událostí**
* [Vytvoření fondu](batch-pool-create-event.md)
* [Spuštění odstranění fondu](batch-pool-delete-start-event.md)
* [Odstranění fondu dokončení](batch-pool-delete-complete-event.md)
* [Počáteční velikosti fondu](batch-pool-resize-start-event.md)
* [Dokončení změny velikosti fondu](batch-pool-resize-complete-event.md)
* [Spuštění úlohy](batch-task-start-event.md)
* [Dokončení úkolů](batch-task-complete-event.md)
* [Selhání úlohy](batch-task-fail-event.md)