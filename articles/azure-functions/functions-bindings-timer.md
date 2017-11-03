---
title: "Azure aktivaci časovačem funkce | Microsoft Docs"
description: "Pochopit, jak použít aktivační události časovače v Azure Functions."
services: functions
documentationcenter: na
author: christopheranderson
manager: cfowler
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: d2f013d1-f458-42ae-baf8-1810138118ac
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/27/2017
ms.author: glenga
ms.custom: 
ms.openlocfilehash: 12beb090a95a31c7e83ae03a920016bdfbf474e3
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2017
---
# <a name="azure-functions-timer-trigger"></a>Azure aktivaci časovačem funkce

[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje postup konfigurace a aktivační události časovače kódu v Azure Functions. Azure Functions nabízí vazbu aktivační událost časovače, který vám umožní spustit funkce kódu podle definovaného plánu. 

Aktivační událost časovače podporuje víc instancí Škálováním na více systémů. Ve všech instancích je spuštěna jedna instance funkce konkrétní časovače.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a id="trigger"></a>

## <a name="timer-trigger"></a>Trigger časovače
Aktivační událost časovače funkce používá následující objekt JSON v `bindings` pole function.json:

```json
{
    "schedule": "<CRON expression - see below>",
    "name": "<Name of trigger parameter in function signature>",
    "type": "timerTrigger",
    "direction": "in"
}
```

Hodnota `schedule` je [výraz CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression) obsahující tyto šest polí: 

    {second} {minute} {hour} {day} {month} {day-of-week}
&nbsp;
>[!NOTE]   
>Řada výrazů cron zjistíte online vynechejte `{second}` pole. Pokud zkopírujete z jednoho z nich, bude nutné upravit pro nadbytečné `{second}` pole. Konkrétní příklady najdete v tématu [naplánovat příklady](#examples) níže.

Použít s výrazy CRON výchozí časové pásmo je koordinovaný světový čas (UTC). Pokud chcete, aby vaše výraz CRON založený na jiném časovém pásmu, vytvořte nové nastavení aplikace pro funkce aplikace s názvem `WEBSITE_TIME_ZONE`. Nastavte hodnotu na název požadované časové pásmo, jak je znázorněno [Microsoft časové pásmo Index](https://technet.microsoft.com/library/cc749073(v=ws.10).aspx). 

Například *východní běžný čas* je UTC-05:00. Má vaše časovače aktivovat ještě efektivněji na 10:00 AM EST každý den, použijte následující výraz CRON účty pro časové pásmo UTC:

```json
"schedule": "0 0 15 * * *",
``` 

Alternativně můžete přidat nové nastavení aplikace pro funkce aplikace s názvem `WEBSITE_TIME_ZONE` a nastavte hodnotu na **východní běžný čas**.  Následující výraz CRON pak může použít pro 10:00 AM EST: 

```json
"schedule": "0 0 10 * * *",
``` 


<a name="examples"></a>

## <a name="schedule-examples"></a>Příklady plán
Tady jsou některé ukázky můžete použít pro výrazy CRON `schedule` vlastnost. 

K aktivaci každých pět minut:

```json
"schedule": "0 */5 * * * *"
```

K aktivaci jednou v horní části každou hodinu:

```json
"schedule": "0 0 * * * *",
```

K aktivaci každé dvě hodiny:

```json
"schedule": "0 0 */2 * * *",
```

K aktivaci jednou za hodinu z 9: 00 do 17: 00:

```json
"schedule": "0 0 9-17 * * *",
```

Spouštět v 9:30:00 každý den:

```json
"schedule": "0 30 9 * * *",
```

Spouštět v 9:30:00 každý den v týdnu:

```json
"schedule": "0 30 9 * * 1-5",
```

<a name="usage"></a>

## <a name="trigger-usage"></a>Aktivační události využití
Po vyvolání funkce aktivační událost časovače [časovače objekt](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) je předána do funkce. Následujícím kódu JSON je příklad reprezentací objekt časovače. 

```json
{
    "Schedule":{
    },
    "ScheduleStatus": {
        "Last":"2016-10-04T10:15:00.012699+00:00",
        "Next":"2016-10-04T10:20:00+00:00"
    },
    "IsPastDue":false
}
```

<a name="sample"></a>

## <a name="trigger-sample"></a>Ukázka aktivační události
Předpokládejme, že máte následující aktivační událost časovače `bindings` pole function.json:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Naleznete v ukázce pro specifický jazyk, který čte objekt časovače pro zjištění, zda je spuštěna pozdní.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Ukázka aktivační události v jazyce C# #
```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    if(myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}" );  
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Ukázka aktivační události v jazyce F # #
```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Ukázka aktivační události v Node.js
```JavaScript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);   

    context.done();
};
```

## <a name="next-steps"></a>Další kroky
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

