---
title: "Postup spuštění streamování úlohy v Stream Analytics | Microsoft Docs"
description: "Jak spustit úlohu streamování v Azure Stream Analytics | učení segmentu cesty."
keywords: "Úloha streamování"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 9d46950f-2b69-49ce-a567-df558c5dd820
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: fb084f1c6b53e2582849e71271e8114a22dcf05c
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a>Jak spustit úlohu streamování v Azure Stream Analytics
Když úloha vstup, dotaz a výstup všech nebyly zadány, že můžete spustit úlohu služby Stream Analytics.

Při spuštění vaší úlohy:

1. Na portálu Azure Classic na řídicím panelu úloh, klikněte na tlačítko **spustit** v dolní části stránky.
   
   ![Spuštění úlohy tlačítko](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  
   
   Na portálu Azure klikněte na tlačítko **spustit** v horní části stránku úlohy.
   
   ![Úloha spuštění Azure portálu tlačítko](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  
2. Zadejte **spustit výstup** hodnotu, která určí, když tato úloha spustí vytváření výstupu. Výchozí nastavení pro úlohy, které nebyly dříve spuštěna je **čas spuštění úlohy**, což znamená, že úloha okamžitě spustí zpracování dat. Můžete také zadat **vlastní** čas v minulosti (pro použití historických dat) nebo později (a zdržet zpracování dokud datum v budoucnosti). Pro případech, kdy úloha má dříve spuštění a zastavení, možnost **naposledy Zastaveno** je k dispozici, aby bylo možné pokračovat v úloze při posledním výstup a nedošlo ke ztrátě dat.  

> [!NOTE]
> Pokud používáte oddíly, naposledy zastaveno představuje minimální čas poslední výstup pro všechny oddíly.
   
   ![Počáteční čas úlohy streamování](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  
   
   ![Azure portálu spustit úlohu streamování čas](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  
3. Potvrďte výběr. Stav úlohy se změní na *počáteční* a krátce přesune do *systémem* po úlohu spustil. Můžete sledovat průběh **spustit** operace v **centra oznámení**:
   
   ![průběh úlohy streamování](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  
   
   ![Portál Azure průběh úlohy streamování](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod do služby Azure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

