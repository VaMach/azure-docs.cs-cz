---
title: "Diagnostika a řešení problémů ve službě Azure časové řady Insights | Microsoft Docs"
description: "Tento článek popisuje, jak k určení, řešení potíží a řešení běžných problémů, které mohou nastat ve vašem prostředí Statistika Azure časové řady."
author: venkatgct
ms.author: venkatja
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 11/15/2017
ms.openlocfilehash: 4216b245fd480003cfa4a34452f87efade964f8d
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a>Diagnostika a řešení problémů ve vašem prostředí Statistika časové řady

## <a name="problem-1-no-data-is-shown"></a>Problém 1: Žádná data se zobrazí.
Existuje několik běžných důvodů, proč nemusí zobrazit vaše data v [Azure časové řady Statistika Explorer](https://insights.timeseries.azure.com):

### <a name="possible-cause-a-event-source-data-is-not-in-json-format"></a>Možná příčina A: události zdroje dat není ve formátu JSON
Statistiky Azure řady čas podporuje pouze data JSON. Ukázky JSON naleznete v části [podporované JSON tvarů](time-series-insights-send-events.md#supported-json-shapes).

### <a name="possible-cause-b-event-source-key-is-missing-a-required-permission"></a>Klíč zdroj události B: možnou příčinou je chybějící požadované oprávnění
* Pro Centrum IoT, budete muset zadat klíč, který má **služba připojit** oprávnění.

   ![Povolení pro připojení služby IoT hub](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   Jak je vidět na předchozím obrázku, buď zásady **iothubowner** a **služby** by pracovat, protože mají **služba připojit** oprávnění.
   
* Pro centra událostí, budete muset zadat klíč, který má **naslouchání** oprávnění.

   ![Oprávnění naslouchání centra událostí](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   Jak je vidět na předchozím obrázku, buď zásady **číst** a **spravovat** by pracovat, protože mají **naslouchání** oprávnění.

### <a name="possible-cause-c-the-consumer-group-provided-is-not-exclusive-to-time-series-insights"></a>Možná příčina C: skupiny příjemců zadaný není výhradně pro přehledy časové řady
Během registrace am Centrum IoT nebo centra událostí zadejte skupinu příjemců, který se má použít pro čtení dat. Tato skupina uživatelů musí **není** sdílet. Pokud je sdílená skupiny příjemců, základní centra událostí automaticky odpojí jeden čtečky náhodně. Zadejte skupiny jedinečný příjemců přehledů časové řady číst z.

## <a name="problem-2-some-data-is-shown-but-some-is-missing"></a>Problém 2: Některá data se zobrazí, ale některé chybí
Pokud se zobrazí data částečně, ale data zaostává, existuje několik možností vzít v úvahu:

### <a name="possible-cause-a-your-environment-is-getting-throttled"></a>Možná příčina A: prostředí je získávání omezen.
Omezení limit vynucený na základě typu SKU v prostředí a kapacity. Všechny zdroje událostí v prostředí sdílet tento kapacitu. Pokud zdroj události pro vaše Centrum IoT nebo Centrum událostí je předání dat nad rámec Vynucené limity, zobrazí omezení a prodleva.

Následující diagram znázorňuje časové řady Statistika prostředí, které má SKU S1 a kapacitou 3. Můžete ho příjem příchozích dat 3 miliony událostí za den.

![Aktuální kapacita SKU prostředí](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Předpokládejme například, že toto prostředí je příjem zpráv z centra událostí. Sledujte rychlost příjem příchozích dat vidět na následujícím obrázku:

![Příklad příchozího rychlost pro centra událostí](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

Jak je vidět v diagramu, denní míra příjem příchozích dat je ~ 67,000 zprávy. Tato míra překládá zhruba na 46 zprávy každou minutu. Pokud každou zprávu centra událostí je průmětu do jedné události časové řady statistiky, se zobrazí toto prostředí žádné omezení. Pokud každá zpráva centra událostí se sloučí na 100 Statistika řady čas události, pak 4,600 události by měl konzumaci každou minutu. S1 SKU prostředí, které má kapacitou 3 můžete pouze 2100 události příchozího každou minutu (1 milion událostí za den = 700 události za minutu ve 3 jednotky = 2100 události za minutu). Proto najdete v části funkce lag kvůli omezování. 

Do hloubky pochopili, jak vyrovnání logiku funguje, najdete v části [podporované JSON tvarů](time-series-insights-send-events.md#supported-json-shapes).

### <a name="recommended-resolution-steps-for-excessive-throttling"></a>Doporučené řešení kroky pro omezení nadměrné
Pokud chcete vyřešit je zpoždění, zvýšit kapacitu SKU vašeho prostředí. Další informace najdete v tématu [postup škálování prostředí časové řady Insights](time-series-insights-how-to-scale-your-environment.md).

### <a name="possible-cause-b-initial-ingestion-of-historical-data-is-causing-slow-ingress"></a>Možná příčina B: počáteční přijímání historických dat způsobuje pomalé příjem příchozích dat
Pokud se připojujete existujícího zdroje událostí, je pravděpodobné, že Centrum IoT nebo Centrum událostí už obsahuje data v ní. Prostředí spustí stahování dat od začátku dobu uchování zpráv zdroj události.

Toto chování je výchozí chování a nelze přepsat. Můžete použít omezení, a že může trvat dobu aktualizovány na příjem historická data.

#### <a name="recommended-resolution-steps-of-large-initial-ingestion"></a>Doporučené řešení kroky velký počáteční přijímání
Pokud chcete vyřešit je zpoždění, proveďte následující kroky:
1. Zvětšete kapacitu SKU pro maximální povolenou hodnotu (v tomto případě 10). Po kapacitu vzroste, spustí proces příjem příchozích dat, zachytávání až mnohem rychlejší. Můžete vizualizovat, jak rychle jste jste zachytávání prostřednictvím grafu dostupnosti v [explorer časové řady Insights](https://insights.timeseries.azure.com). Budou účtovat zvýšené kapacity.
2. Jakmile je zpoždění je zachycena, snížit kapacita SKU zpět do vaší normální příchozí míra.

## <a name="problem-3-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Problém 3: Mé zdroje událostí *název vlastnosti časové razítko* nastavení nebude fungovat.
Ujistěte se, že název a hodnotu odpovídají následující pravidla:
* Název vlastnosti časové razítko je _malá a velká písmena_.
* Hodnota vlastnosti časové razítko, které pochází ze zdroje událostí, jako řetězec formátu JSON by měl mít formát _rrrr-MM-ddTHH. FFFFFFFK_. Příkladem takových řetězec je "2008-04-12T12:53Z".

## <a name="next-steps"></a>Další kroky
- Další pomoc získáte zahájit konverzaci na [fórum MSDN](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) nebo [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-timeseries-insights). 
- Můžete také použít [podporu Azure](https://azure.microsoft.com/support/options/) možnosti odborné pomoci řešení.
