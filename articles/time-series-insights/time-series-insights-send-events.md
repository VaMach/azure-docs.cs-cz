---
title: "Odesílání událostí do prostředí Azure Time Series Insights | Dokumentace Microsoftu"
description: "Tento kurz se zaměřuje na odesílání událostí do prostředí Time Series Insights."
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: venkatja
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 9f2d3b57a42efb7b04566278d3267b3cdbed713a
ms.contentlocale: cs-cz
ms.lasthandoff: 07/01/2017

---
# Odesílání událostí do prostředí Time Series Insights prostřednictvím centra událostí
<a id="send-events-to-a-time-series-insights-environment-via-event-hub" class="xliff"></a>

Tento kurz vysvětluje, jak vytvořit a nakonfigurovat centrum událostí a jak spustit ukázkovou aplikaci odesílající události. Pokud máte existující centrum událostí s událostmi ve formátu JSON, přeskočte tento kurz a zobrazte své prostředí v [Time Series Insights](https://insights.timeseries.azure.com).

## Konfigurace centra událostí
<a id="configure-an-event-hub" class="xliff"></a>
1. Pokud chcete vytvořit centrum událostí, postupujte podle pokynů uvedených v [dokumentaci](https://docs.microsoft.com/azure/event-hubs/event-hubs-create) ke službě Event Hubs.

2. Ujistěte se, že vytváříte skupinu příjemců, kterou používá výhradně váš zdroj událostí Time Series Insights.

  > [!IMPORTANT]
  > Zajistěte, aby tuto skupinu příjemců nepoužívala žádná jiná služba (například úloha služby Stream Analytics nebo jiné prostředí Time Series Insights). Pokud skupinu příjemců používají další služby, bude negativně ovlivněna operace čtení pro toto prostředí i ostatní služby. Pokud jako skupinu příjemců používáte $Default, může potenciálně dojít k jejímu opakovanému použití jinými čtenáři.

  ![Výběr skupiny příjemců centra událostí](media/send-events/consumer-group.png)

3. V centru událostí vytvořte zásadu MySendPolicy, která v ukázce csharp slouží k odesílání událostí.

  ![Vyberte Zásady sdíleného přístupu a klikněte na tlačítko Přidat.](media/send-events/shared-access-policy.png)  

  ![Přidání nové zásady sdíleného přístupu](media/send-events/shared-access-policy-2.png)  

## Vytvoření zdroje událostí Time Series Insights
<a id="create-time-series-insights-event-source" class="xliff"></a>
1. Pokud jste ještě nevytvořili zdroj událostí, postupujte podle pokynů uvedených [tady](time-series-insights-add-event-source.md) a vytvořte ho.

2. Zadejte „deviceTimestamp“ jako název vlastnosti časového razítka – tato vlastnost se v ukázce csharp používá jako časové razítko. V názvu vlastnosti časového razítka se rozlišují malá a velká písmena a při odesílání hodnot do centra událostí ve formátu JSON musí mít formát __yyyy-MM-ddTHH:mm:ss.FFFFFFFK__. Pokud v události tato vlastnost chybí, použije se čas zařazení do fronty centra událostí.

  ![Vytvoření zdroje událostí](media/send-events/event-source-1.png)

## Vzorový kód pro odesílání událostí
<a id="sample-code-to-push-events" class="xliff"></a>
1. Přejděte do zásady centra událostí MySendPolicy a zkopírujte připojovací řetězec s klíčem zásady.

  ![Zkopírování připojovacího řetězce zásady MySendPolicy](media/send-events/sample-code-connection-string.png)

2. Spusťte následující kód, který za každé ze tří zařízení odešle 600 událostí. Nahraďte `eventHubConnectionString` vaším připojovacím řetězcem.

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.Rdx.DataGenerator
{
    internal class Program
    {
        private static void Main(string[] args)
        {
            var from = new DateTime(2017, 4, 20, 15, 0, 0, DateTimeKind.Utc);
            Random r = new Random();
            const int numberOfEvents = 600;

            var deviceIds = new[] { "device1", "device2", "device3" };

            var events = new List<string>();
            for (int i = 0; i < numberOfEvents; ++i)
            {
                for (int device = 0; device < deviceIds.Length; ++device)
                {
                    // Generate event and serialize as JSON object:
                    // { "deviceTimestamp": "utc timestamp", "deviceId": "guid", "value": 123.456 }
                    events.Add(
                        String.Format(
                            CultureInfo.InvariantCulture,
                            @"{{ ""deviceTimestamp"": ""{0}"", ""deviceId"": ""{1}"", ""value"": {2} }}",
                            (from + TimeSpan.FromSeconds(i * 30)).ToString("o"),
                            deviceIds[device],
                            r.NextDouble()));
                }
            }

            // Create event hub connection.
            var eventHubConnectionString = @"Endpoint=sb://...";
            var eventHubClient = EventHubClient.CreateFromConnectionString(eventHubConnectionString);

            using (var ms = new MemoryStream())
            using (var sw = new StreamWriter(ms))
            {
                // Wrap events into JSON array:
                sw.Write("[");
                for (int i = 0; i < events.Count; ++i)
                {
                    if (i > 0)
                    {
                        sw.Write(',');
                    }
                    sw.Write(events[i]);
                }
                sw.Write("]");

                sw.Flush();
                ms.Position = 0;

                // Send JSON to event hub.
                EventData eventData = new EventData(ms);
                eventHubClient.Send(eventData);
            }
        }
    }
}

```
## Podporované tvary JSON
<a id="supported-json-shapes" class="xliff"></a>
### Ukázka 1
<a id="sample-1" class="xliff"></a>

#### Vstup
<a id="input" class="xliff"></a>

Jednoduchý objekt JSON.

```json
{
    "id":"device1",
    "timestamp":"2016-01-08T01:08:00Z"
}
```
#### Výstup – 1 událost
<a id="output---1-event" class="xliff"></a>

|id|časové razítko|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|

### Ukázka 2
<a id="sample-2" class="xliff"></a>

#### Vstup
<a id="input" class="xliff"></a>
Pole JSON se dvěma objekty JSON. Oba objekty JSON se převedou na událost.
```json
[
    {
        "id":"device1",
        "timestamp":"2016-01-08T01:08:00Z"
    },
    {
        "id":"device2",
        "timestamp":"2016-01-17T01:17:00Z"
    }
]
```
#### Výstup – 2 události
<a id="output---2-events" class="xliff"></a>

|id|časové razítko|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|
|device2|2016-01-08T01:17:00Z|
### Ukázka 3
<a id="sample-3" class="xliff"></a>

#### Vstup
<a id="input" class="xliff"></a>

Objekt JSON s vnořeným polem JSON, které obsahuje dva objekty JSON.
```json
{
    "location":"WestUs",
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z"
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z"
        }
    ]
}

```
#### Výstup – 2 události
<a id="output---2-events" class="xliff"></a>
Všimněte si, že vlastnost „location“ (umístění) se kopíruje do obou událostí.

|location|events.id|events.timestamp|
|--------|---------------|----------------------|
|WestUs|device1|2016-01-08T01:08:00Z|
|WestUs|device2|2016-01-08T01:17:00Z|

### Ukázka 4
<a id="sample-4" class="xliff"></a>

#### Vstup
<a id="input" class="xliff"></a>

Objekt JSON s vnořeným polem JSON, které obsahuje dva objekty JSON. Tento vstup ukazuje, že globální vlastnosti může reprezentovat komplexní objekt JSON.

```json
{
    "location":"WestUs",
    "manufacturer":{
        "name":"manufacturer1",
        "location":"EastUs"
    },
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z",
            "data":{
                "type":"pressure",
                "units":"psi",
                "value":108.09
            }
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z",
            "data":{
                "type":"vibration",
                "units":"abs G",
                "value":217.09
            }
        }
    ]
}
```
#### Výstup – 2 události
<a id="output---2-events" class="xliff"></a>

|location|manufacturer.name|manufacturer.location|events.id|events.timestamp|events.data.type|events.data.units|events.data.value|
|---|---|---|---|---|---|---|---|
|WestUs|manufacturer1|EastUs|device1|2016-01-08T01:08:00Z|pressure|psi|108.09|
|WestUs|manufacturer1|EastUs|device2|2016-01-08T01:17:00Z|vibration|abs G|217.09|

## Další kroky
<a id="next-steps" class="xliff"></a>

* Zobrazení prostředí na [portálu Time Series Insights](https://insights.timeseries.azure.com)

