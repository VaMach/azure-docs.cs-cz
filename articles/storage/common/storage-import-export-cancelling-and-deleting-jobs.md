---
title: "Zrušit a odstranit úlohu Azure Import/Export | Microsoft Docs"
description: "Zjistěte, jak zrušit a odstraňovat úlohy pro službu Microsoft Azure Import/Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: fd3d66f0-1dbb-4c75-9223-307d5abaeefc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 1e989c72fc03697bf6d2e515ff53003703665d1a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>Zrušení a odstranění úlohy Azure Import/Export

 Požádat o zrušit úlohu předtím, než je v `Packaging` stavu, volejte [vlastnosti úlohy aktualizace](/rest/api/storageimportexport/jobs#Jobs_Update) operace a sadu `CancelRequested` element `true`. Úloha je zrušena na základě typu best effort. Pokud jednotky probíhá přenos dat, dat pokračovat v přesunu, i když bylo vyžádáno zrušení.

 Zrušené úlohy je přesunuta do `Completed` stavu a je uložen za 90 dnů od této chvíle se odstraní.

 Chcete-li odstranit úlohu, volejte [odstranit úlohu](/rest/api/storageimportexport/jobs#Jobs_Delete) operace předtím, než má dodaný úlohy (to znamená, když úloha je v `Creating` stav). Můžete také odstranit úlohu, když je `Completed` stavu. Po odstranění úlohy její informace a stav již nejsou přístupné přes rozhraní REST API nebo portálu Azure.

## <a name="next-steps"></a>Další kroky

* [Pomocí REST API služby importu a exportu](storage-import-export-using-the-rest-api.md)
