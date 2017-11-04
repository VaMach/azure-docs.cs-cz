---
title: "Postup vytvoření úlohy zpracování dat analytics pro Stream Analytics | Microsoft Docs"
description: "Vytvoření úlohy zpracování dat analytics pro Stream Analytics | učení segmentu cesty."
keywords: "zpracování analýzy dat"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: e825fbcf-69e9-443f-b402-3b7a4568f415
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 05fdf1e20efd129cdfc27e1d37bc9e124edf5dcd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Postup vytvoření úlohy zpracování dat analytics pro Stream Analytics
Nejvyšší úrovně prostředků v Azure Stream Analytics je úlohy Stream Analytics.  Obsahuje jeden nebo více zdrojů vstupních dat, dotaz vyjadřující transformaci dat a jeden nebo více cílů výstup, které výsledky se zapisují do. Společně tyto umožní uživatelům provádět analýzy dat, zpracování pro streamování dat scénáře.

Chcete-li začít používat Stream Analytics, začněte tím, že vytváření nové úlohy Stream Analytics.  Všimněte si, že tato akce nemá žádné fakturace dopady až po zahájení úlohy.

1. Přihlaste se na online [portál Azure classic](http://manage.windowsazure.com) nebo [portál Azure](https://portal.azure.com/).
2. Na portálu: **klikněte na tlačítko Nový**, pak klikněte na tlačítko **datové služby** nebo **Data Analytics** v závislosti na portál a pak klikněte na tlačítko **Azure Stream Analytics**nebo **Stream Analytics** a potom **rychle vytvořit**.
   
   ![Průvodce úlohy zpracování analýzy dat](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  
   
   ![Vytvoření analýzy dat, zpracování úlohy](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  
3. Zadejte požadovanou konfiguraci pro úlohu služby Stream Analytics.
   
   * V **název úlohy** pole, zadejte název pro identifikaci úlohu služby Stream Analytics. Když **název úlohy** byl ověřen, do pole Název úlohy se zobrazí zelená značka zaškrtnutí. **Název úlohy** může obsahovat pouze alfanumerické znaky a '-' znak a musí být v rozmezí 3 až 63 znaků.
   * Použití **oblast** na portálu Azure nebo **umístění** na portálu Azure k určení zeměpisného umístění, ve které chcete spustit úlohu.
   * Pokud používáte portál Azure, vyberte nebo vytvořte účet úložiště, který chcete použít jako **místní monitorování účtu úložiště**. Tento účet úložiště se používá k ukládání dat monitorování pro všechny úlohy služby Stream Analytics, které jsou spuštěné v této oblasti.
   * Pokud používáte portál Azure, zadejte nový nebo existující **skupiny prostředků** pro uložení související prostředky pro vaši aplikaci.
4. Po nakonfigurování nastavení nové úlohy Stream Analytics, klikněte na tlačítko **vytvořit úlohy Stream Analytics**. Může trvat několik minut pro úlohu služby Stream Analytics, který se má vytvořit. Chcete-li zkontrolovat stav, můžete sledovat průběh v centru oznámení.
   
   ![Analýza dat, zpracování centrem oznámení úlohy](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  
   
   ![Vytvoření úlohy úlohy Azure portálu Data analytics zpracování](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. Nová úloha se zobrazí stav **vytvořená**. Všimněte si, že **spustit** tlačítko k dispozici. Před spuštěním úlohy, nakonfigurujte vstupu úlohy, dotaz a výstup.
   
   ![Úlohy zpracování analýzy dat stavu](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  
   
   ![Azure portálu analýzy dat zpracování stavu úlohy](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod do služby Azure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

