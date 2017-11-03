---
title: "Tom, jak psát dotazy v Stream Analytics | Microsoft Docs"
description: "Zápis dotazů v Stream Analytics a dotazování dat | učení segmentu cesty."
keywords: "tom, jak psát dotazy, dotaz na data, napsat dotaz, zápis dotazů"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 0e9cdadd-0ee0-4bee-b65b-4a06fb863c95
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 215b774c20d80a67b1cefa2634131bd44860c692
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-write-queries-in-stream-analytics"></a>Tom, jak psát dotazy v Stream Analytics
Zápis dotazů pro zpracování logiky v Azure Stream Analytics datového proudu je implementovaný jako "stojící dotaz", který je definován před úloha spustí a u dat provést, protože nedosáhne úlohy. Transformaci dat je vyjádřenou v jazyce SQL jako dotaz, který je z velké části podmnožinou T-SQL s některými přidat jazyková rozšíření jako [Oddílová](https://msdn.microsoft.com/library/azure/dn835019.aspx) použije pro vyjádření dočasné sémantiku.

## <a name="writing-queries"></a>Zápis dotazů:
1. V úloze Stream Analytics na portálu Azure, klikněte na tlačítko **dotazu**.
   
    ![Vybrat dotaz](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  
   
    Na portálu Azure klikněte na tlačítko **dotazu**.
   
    ![Vyberte Náhled dotazu](./media/stream-analytics-write-queries/query-preview-portal.png)  
2. Nové úlohy mají šablonu dotazu pro vám pomůžou začít. Šablonu dotazu provádí "předávací" dotaz, který projekty všech polí ze vstupu události do výstupu.  
   
   * Pokud jste definovali alespoň jeden vstup a výstup pro úlohu, můžete nahraďte zástupný symbol "[YourOutputAlias]" a "[YourInputAlias]" pole s aliasy vstupu a výstupu, který chcete použít jako první. Kromě toho můžete stále vytvářet a testovat dotaz na portálu Azure Classic bez definování vstupů a výstupů v úloze.
   * Pokud chcete provést další zpracování než jednoduchý průchozí, můžete upravit definice dotazu. Začít s vytváření dotazů, podívejte se na některé běžné dotazu zaznamenání vzory [zde](stream-analytics-stream-analytics-query-patterns.md).  
   
   ![Dotaz na data okna](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="to-validate-query-data-is-working"></a>Ověřit data dotazu funguje:
Můžete otestovat, zda dotaz chová podle očekávání spuštěním v prohlížeči přes jeden nebo více místní soubory JSON obsahující testovací data. To nebude spustit úlohu nebo mají vliv na všechny fakturace.

> [!NOTE]
> Aktuálně testování dotazů v prohlížeči není podporována na portálu Azure.  
> 
> 

1. Ujistěte se, že nejsou žádné chyby v dotazu (jinak tlačítko Testovat bude zakázáno) a potom klikněte na tlačítko Test.  
   
   ![Dotaz na data testu](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  
2. Zobrazí se výzva k zadání souborů pro každou z vstupů v dotazu odkazovat. V tomto příkladu je dotaz šablony ponechané jako-se, takže dialogu je dotaz na vstup s názvem "yourinputalias".
   
   ![Dotaz na Data testu](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  
3. Vyhledejte soubor testu. Několik ukázkových souborů, které jsou k dispozici na [githubu](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) a můžete také načíst ukázková data z vlastního datového proudu vstupy dat prostřednictvím funkce ukázková Data na kartě vstupy.
   
   ![Vstupní dotaz](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  
4. Po zavření dialogu, dotaz se spustí přes testovací data a zobrazí výsledky v dolní části stránky dotazu.
   
   ![Souhrn dotazu](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod do služby Azure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

