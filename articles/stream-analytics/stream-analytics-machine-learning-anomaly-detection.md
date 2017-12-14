---
title: "Detekce anomálií v příručce využití Azure (Preview) | Microsoft Docs"
description: "Pomocí služby stream analytics a machine learningu zjišťovat anomálie."
services: stream-analytics
documentationcenter: 
author: dubansal
manager: jhubbard
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: dubansal
ms.openlocfilehash: db72b1ca936e69a049d64f939d3399bfd9cdf89c
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="using-the-anomalydetection-operator"></a>Pomocí operátoru ANOMALYDETECTION

> [!IMPORTANT]
> Tato funkce je ve verzi preview. Nedoporučujeme použití v produkčním prostředí.

**ANOMALYDETECTION** operátor lze zjišťovat anomálie v datových proudů událostí.
Například může být pomalé poklesu volná paměť nad dlouhou dobu naznačuje výslednou nevrácené paměti nebo počet žádosti webové služby, které jsou v rozsahu stabilní může výrazně zvýšit nebo snížit.

Pro následující typy anomálií zkontroluje přes zadaná doba trvání:

- Změna úrovně obousměrného
- Pomalé pozitivní trend
- Pomalé negativní trend

Časový interval pro jak daleko zpět v historii z aktuální událost musí být prohlédli je omezen pomocí **LIMIT DURATION** klauzule. **ANOMALYDETECTION** volitelně může být omezen na jenom události, které odpovídají určité vlastnosti nebo podmínky, pomocí **při** klauzule.

Volitelně může zpracovat skupiny událostí odděleně podle je klíč zadaný v **PARTITION BY** klauzule. Školení a předpovědi dojít nezávisle v každém oddílu.

## <a name="syntax"></a>Syntaxe

`ANOMALYDETECTION(<scalar_expression>) OVER ([PARTITION BY <partition key>] LIMIT DURATION(<unit>, <length>) [WHEN boolean_expression])` 


## <a name="example-usage"></a>Příklad použití

`SELECT id, val, ANOMALYDETECTION(val) OVER(PARTITION BY id LIMIT DURATION(hour, 1) WHEN id > 100) FROM input`|


## <a name="arguments"></a>Argumenty

- **skalární_výraz**

  Skalární výraz, přes který by se provedla detekce anomálií. Je výraz typu float nebo bigint, který vrací jedinou hodnotu (skalární). Výraz se zástupnými znaky  **\***  není povolen. **skalární_výraz** nemůže obsahovat jiné analytické funkce nebo externí funkce.

- **PŘES (limit_duration_clause [partition_by_clause] [when_clause])**

- **partition_by_clause** 

  `PARTITION BY <partition key>` Klauzule rozděluje učení a školení napříč samostatné oddíly. Jinými slovy, samostatného modelu se použije na hodnotu `<partition key>` a jenom události s hodnotou, kterou by se používat pro učení a školení v tomto modelu. Například:

  `SELECT sensorId, reading, ANOMALYDETECTION(reading) OVER(PARTITION BY sensorId LIMIT DURATION(hour, 1)) FROM input`

  bude trénování a stanovíte jeho skóre čtení proti jiných odečty stejné senzoru.

- **klauzule limit_duration** doba trvání (\<jednotky\>, \<délka\>)

  Určuje, kolik z historie z aktuální událost považován za **ANOMALYDETECTION** výpočty. Funkce DATEDIFF najdete podrobný popis podporované jednotky a jejich zkratky.

- **when_clause** 

  Určuje podmínku Boolean pro události za v **ANOMALYDETECTION** výpočty.

## <a name="return-types"></a>Návratové typy

Vrátí záznam obsahující všechny tři skóre jako výstup. Vlastnosti související s různými typy anomálií detektory se označují jako:

- BiLevelChangeScore
- SlowPosTrendScore
- SlowNegTrendScore

Chcete-li extrahovat jednotlivé hodnoty mimo záznam, použijte **GetRecordPropertyValue** funkce. Například:

`SELECT id, val FROM input WHERE (GetRecordPropertyValue(ANOMALYDETECTION(val) OVER(LIMIT DURATION(hour, 1)), 'BiLevelChangeScore')) > 3.25` 


Anomálií konkrétní typ je zjištěna, když jeden z těchto anomálií skóre překračuje prahovou hodnotu. Prahová hodnota může být jakékoli plovoucí bodu číslo \>= 0. Prahová hodnota je kompromis mezi velkých a malých písmen a spolehlivosti. Například by nižší prahové hodnotě zkontrolujte detekce více citlivé na změny a generovat více výstrah, zatímco vyšší prahová hodnota může zvýšit detekce méně citlivou a větší jistotu, ale některé anomálií maskování. Přesný prahová hodnota používat závisí na scénáři. Neexistuje žádné horní limit, ale doporučené rozsah je 3,25 5.

## <a name="algorithm"></a>Algoritmus

**ANOMALYDETECTION** používá posuvné okno sémantikou, což znamená, že provádí výpočet na událost, která vstupuje do funkce a vytváří skóre pro tuto událost. Výpočet je založena na Martingales výroby, které fungují na kontrolu, pokud došlo ke změně rozdělení hodnoty události. Pokud ano, byl zjištěn potenciální anomálií. Vrácený skóre je údajem o úroveň spolehlivosti této anomálií. Jako interní optimalizace **ANOMALYDETECTION** vypočítá skóre anomálií událost na základě *d* k *2d* za událostí, kde *d*je velikost okna zadaný detekce.

**ANOMALYDETECTION** očekává vstupní časové řady být uniform. Datového proudu událostí můžete provedeny uniform agregování přes přeskakující nebo posílání okno. Ve scénářích, kdy mezery mezi událostmi vždy menší než okno agregace přeskakující okno stačí aby uniform časové řady. Při mezera může být větší, může být vyplněna mezer opakováním poslední hodnotu pomocí skákající okno. Oba tyto scénáře mohou být zpracována v příkladu, který následuje dále. V současné době `FillInMissingValuesStep` krok nelze přeskočí. Nemusí tento krok bude mít za následek chybu kompilace.

## <a name="performance-guidance"></a>Průvodce výkonem

- Použijte pro úlohy 6 SU. 
- Odesílání událostí minimálně 1 sekunda od sebe.
- Bez oddílů dotazu pomocí **ANOMALYDETECTION** funkce může způsobit výsledky s latencí výpočtu o 25ms v průměru.
- Latence zasáhla oddílů dotazu se mírně liší s počet oddílů, jako počet výpočty je vyšší. Latence je však o stejný jako bez oddílů případ porovnatelný z hlediska celkový počet událostí pro všechny oddíly.
- Při čtení dat v reálném čase, velké množství dat požity rychle. Zpracování tato data je aktuálně výrazně pomalejší. Latence v takových případech byl nalezen zvýšit lineárně s počet datových bodů v okně spíše než interval velikost nebo událost okna samo o sobě. Chcete-li snížit latenci v reálném čase případech, doporučujeme použít menší velikost okna. Případně zvažte úlohu od aktuální čas. Několik příkladů latence v dotazu bez oddílů: 
    - 60 datových bodů v okně detekce může mít za následek latence 10 sekund s propustností 3 MB/s. 
    - Na 600 datových bodů latence dosáhnout přibližně 80 sekund s propustností 0,4 MB/s.

## <a name="example"></a>Příklad

Následující dotaz slouží k vypsání výstrahu v případě zjištění anomálií.
Pokud vstupní datový proud není uniform, může pomoci krok agregace transformují je na uniform časové řady. Tento příklad používá **průměr** ale specifický typ agregace závisí na scénáři uživatele. Kromě toho časové řady má větší než interval agregace mezery, budou existovat žádné události časové řady detekce anomálií aktivační události (podle posuvné okno sémantiku). V důsledku toho se předpokládá jednotnost se přeruší, když dorazí následující události. V takových situacích potřebujeme nějaký způsob vyplnění mezer v časové řady. Jeden z možných přístupů je převést na poslední událost do všech oken směrování, jak je uvedeno níže.

Jak jsme uvedli před, není přeskočit `FillInMissingValuesStep` krok teď. Vynechání tohoto kroku bude mít za následek chybu kompilace.

    WITH AggregationStep AS 
    (
         SELECT
               System.Timestamp as tumblingWindowEnd,

               AVG(value) as avgValue

         FROM input
         GROUP BY TumblingWindow(second, 5)
    ),

    FillInMissingValuesStep AS
    (
          SELECT
                System.Timestamp AS hoppingWindowEnd,

                TopOne() OVER (ORDER BY tumblingWindowEnd DESC) AS lastEvent

         FROM AggregationStep
         GROUP BY HOPPINGWINDOW(second, 300, 5)

    ),

    AnomalyDetectionStep AS
    (

          SELECT
                hoppingWindowEnd,
                lastEvent.tumblingWindowEnd as lastTumblingWindowEnd,
                lastEvent.avgValue as lastEventAvgValue,
                system.timestamp as anomalyDetectionStepTimestamp,

                ANOMALYDETECTION(lastEvent.avgValue) OVER (LIMIT DURATION(hour, 1)) as
                scores

          FROM FillInMissingValuesStep
    )

    SELECT
          alert = 1,
          hoppingWindowEnd,
          lastTumblingWindowEnd,
          lastEventAvgValue,
          anomalyDetectionStepTimestamp,
          scores

    INTO output

    FROM AnomalyDetectionStep

    WHERE

        CAST(GetRecordPropertyValue(scores, 'BiLevelChangeScore') as float) >= 3.25

        OR CAST(GetRecordPropertyValue(scores, 'SlowPosTrendScore') as float) >=
        3.25

       OR CAST(GetRecordPropertyValue(scores, 'SlowNegTrendScore') as float) >=
       3.25

## <a name="references"></a>Odkazy

* [Detekce anomálií – pomocí Machine Learning zjišťovat anomálie v časových řad dat](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/)
* [Strojového učení detekce anomálií rozhraní API](https://docs.microsoft.com/en-gb/azure/machine-learning/machine-learning-apps-anomaly-detection-api)
* [Detekce anomálií časové řady](https://msdn.microsoft.com/library/azure/mt775197.aspx)

## <a name="get-support"></a>Získat podporu
Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky

* [Úvod do služby Azure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

