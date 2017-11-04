---
title: "Testování dotazů v Azure Stream Analytics | Microsoft Docs"
description: "Postup testování vašich dotazů v úlohy Stream Analytics."
keywords: "Test dotazu, řešení potíží s dotazu"
documentation center: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: samacha
ms.openlocfilehash: 5e7bab0b0c3222ba093a93dc2d15f1e41898e62c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="test-azure-stream-analytics-queries-in-the-azure-portal"></a>Testování dotazů Azure Stream Analytics na portálu Azure

Pomocí služby Azure Stream Analytics můžete otestovat dotazy na portálu Azure bez nutnosti spuštění nebo zastavení úlohy.

## <a name="test-the-input"></a>Testování vstupu

1. Chcete-li otestovat s ukázkovými daty vstupní, klikněte pravým tlačítkem na vstupy a pak vyberte **nahrát ukázková data ze souboru**.

    ![Stream analytics dotaz editor testu dotazu](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

2. Po dokončení nahrávání se klikněte na tlačítko **testování** k testování tento dotaz ukázková data, které jste zadali.

    ![Dotaz služby Stream analytics editor testu ukázková data](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

Výstup tohoto dotazu se zobrazí v prohlížeči s výsledky odkaz ke stažení, budete chtít uložit výstup testu pro pozdější použití. Teď můžete snadno a interaktivně upravte dotaz a otestovat opakovaně a najdete v části Jak výstup změny.

![Stream Analytics query editor ukázkový výstup](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

S více výstupů použitých v dotazu můžete zobrazit výsledky pro obě výstupy samostatně a snadno přepínat mezi nimi.

Jakmile budete spokojeni se na výsledky zobrazené v prohlížeči, můžete uložit dotazu, spusťte úlohu a nechat zpracovat události bez chyby.

## <a name="get-help"></a>Podpora

Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky

* [Úvod do služby Azure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
