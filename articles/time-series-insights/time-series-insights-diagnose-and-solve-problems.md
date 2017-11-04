---
title: "Diagnostika a řešení problémů | Microsoft Docs"
description: "Tento kurz se zaměřuje jak diagnostikovat a řešit problémy ve vašem prostředí Statistika časové řady"
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/24/2017
ms.author: venkatja
ms.openlocfilehash: 4e10a009eb67706d927ece5692134d802094cdf9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a>Diagnostika a řešení problémů ve vašem prostředí Statistika časové řady

## <a name="i-dont-see-my-data"></a>Data se nezobrazí
Tady jsou některé důvody, proč nemusí zobrazit vaše data ve vašem prostředí v [portálu Statistika řady čas Azure](https://insights.timeseries.azure.com).

### <a name="your-event-source-doesnt-have-data-in-json-format"></a>Váš zdroj událostí nemá data ve formátu JSON
Statistiky Azure řady čas podporuje pouze data JSON ještě dnes. Ukázky JSON naleznete v části [podporované JSON tvarů](time-series-insights-send-events.md#supported-json-shapes).

### <a name="when-you-registered-your-event-source-you-didnt-provide-the-key-that-has-the-required-permission"></a>Při registraci zdroje událostí neposkytli klíč, který má požadovaná oprávnění
* Pro Centrum IoT, budete muset zadat klíč, který má **služba připojit** oprávnění.

   ![Povolení pro připojení služby IoT hub](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   Jak je vidět na předchozím obrázku, buď zásady **iothubowner** a **služby** by pracovat, protože mají **služba připojit** oprávnění.
* Pro centra událostí, budete muset zadat klíč, který má **naslouchání** oprávnění.

   ![Oprávnění naslouchání centra událostí](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   Jak je vidět na předchozím obrázku, buď zásady **číst** a **spravovat** by pracovat, protože mají **naslouchání** oprávnění.

### <a name="the-provided-consumer-group-is-not-exclusive-to-time-series-insights"></a>Skupina zadaná příjemce není výhradně pro přehledy časové řady
Centrum IoT nebo centra událostí během registrace jsme vyžadují zadání skupiny uživatelů, který se má použít pro čtení vaše data. Tato skupina uživatelů nesmí sdílet. Když ho sdílíte, základní centra událostí automaticky odpojí jeden čtečky náhodně.

## <a name="i-see-my-data-but-theres-a-lag"></a>Vidět data, ale existuje prodleva
Tady jsou z důvodů, proč může se zobrazit částečná data ve vašem prostředí v [portálu Statistika časové řady](https://insights.timeseries.azure.com).

### <a name="your-environment-is-getting-throttled"></a>Prostředí je získávání omezen.
Omezení limit vynucený na základě typu SKU v prostředí a kapacity. Všechny zdroje událostí v prostředí sdílet tento kapacitu. Pokud zdroj události pro vaše Centrum IoT nebo Centrum událostí je předání dat nad rámec Vynucené limity, zobrazí omezení a prodleva.

Následující diagram znázorňuje časové řady Statistika prostředí, které má SKU S1 a kapacitou 3. Můžete ho příjem příchozích dat 3 miliony událostí za den.

![Aktuální kapacita SKU prostředí](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Předpokládejme, že toto prostředí je příjem zpráv z centra událostí míru příjem příchozích dat vidět na následujícím obrázku:

![Příklad příchozího rychlost pro centra událostí](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

Jak je vidět v diagramu, denní míra příjem příchozích dat je ~ 67,000 zprávy. Tato míra překládá zhruba na 46 zprávy každou minutu. Pokud každou zprávu centra událostí je průmětu do jedné události časové řady statistiky, se zobrazí toto prostředí žádné omezení. Pokud každá zpráva centra událostí se sloučí na 100 Statistika řady čas události, pak 4,600 události by měl konzumaci každou minutu. S1 SKU prostředí, které má kapacitou 3 můžete pouze 2100 události příchozího každou minutu (1 milion událostí za den = 700 události za minutu ve 3 jednotky = 2100 události za minutu). Proto najdete v části funkce lag kvůli omezování. 

Do hloubky pochopili, jak vyrovnání logiku funguje, najdete v části [podporované JSON tvarů](time-series-insights-send-events.md#supported-json-shapes).

#### <a name="recommended-steps"></a>Doporučené kroky
Pokud chcete vyřešit je zpoždění, zvýšit kapacitu SKU vašeho prostředí. Další informace najdete v tématu [postup škálování prostředí časové řady Insights](time-series-insights-how-to-scale-your-environment.md).

### <a name="youre-pushing-historical-data-and-causing-slow-ingress"></a>Jste předání historických dat a způsobuje pomalé příjem příchozích dat
Pokud se připojujete existujícího zdroje událostí, je pravděpodobné, že Centrum IoT nebo Centrum událostí už obsahuje data v ní. Proto prostředí spustí, stahování dat od začátku dobu uchování zpráv zdroj události. 

Toto chování je výchozí chování a nelze přepsat. Můžete použít omezení, a že může trvat dobu aktualizovány na příjem historická data.

#### <a name="recommended-steps"></a>Doporučené kroky
Pokud chcete vyřešit je zpoždění, proveďte následující kroky:
1. Zvětšete kapacitu SKU pro maximální povolenou hodnotu (v tomto případě 10). Po kapacitu vzroste, spustí proces příjem příchozích dat, zachytávání až mnohem rychlejší. Můžete vizualizovat, jak rychle jste jste zachytávání prostřednictvím grafu dostupnosti v [portálu Statistika časové řady](https://insights.timeseries.azure.com). Budou účtovat zvýšené kapacity.
2. Jakmile je zpoždění je zachycena, snížit kapacita SKU zpět do vaší normální příchozí míra.

## <a name="my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Zdroj události *název vlastnosti časové razítko* nastavení nebude fungovat.
Ujistěte se, že název a hodnotu odpovídají následující pravidla:
* Název vlastnosti časové razítko je _malá a velká písmena_.
* Hodnota vlastnosti časové razítko, které pochází ze zdroje událostí, jako řetězec formátu JSON by měl mít formát _rrrr-MM-ddTHH. FFFFFFFK_. Příkladem takových řetězec je "2008-04-12T12:53Z".
