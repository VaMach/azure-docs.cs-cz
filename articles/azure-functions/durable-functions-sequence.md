---
title: "Funkce řetězení trvanlivý funkcí – Azure"
description: "Zjistěte, jak spustit ukázku trvanlivý funkce, která postupně provede sekvenci funkce."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 913805901bf8131e4908be03e9213539a26205ed
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/13/2017
---
# <a name="function-chaining-in-durable-functions---hello-sequence-sample"></a>Funkce řetězení v trvanlivý funkce – ukázka pořadí Hello

Funkce řetězení odkazuje na vzor provádění pořadí funkcí v určitém pořadí. Výstup jednu funkci často potřebuje má být použita pro vstup jinou funkci. Tento článek vysvětluje vzorku, který používá [trvanlivý funkce](durable-functions-overview.md) implementovat řetězení funkce.

## <a name="prerequisites"></a>Požadavky

* Postupujte podle pokynů v [nainstalovat trvanlivý funkce](durable-functions-install.md) nastavit vzorku.

## <a name="the-functions"></a>Funkce

Tento článek vysvětluje v ukázkové aplikace následující funkce:

* `E1_HelloSequence`Funkce: orchestrator, který volá `E1_SayHello` vícekrát v pořadí. Ukládají se výstup z `E1_SayHello` volá a zaznamenává výsledky.
* `E1_SayHello`: Aktivita funkce, která přidá řetězec s text "Hello".

Následující části popisují konfiguraci a kód, který se používá pro vývoj na portálu Azure. Kód pro vývoj v sadě Visual Studio se zobrazí na konci tohoto článku.
 
## <a name="functionjson-file"></a>soubor Function.JSON

Pokud používáte portál Azure pro vývoj, zde je obsah *function.json* soubor pro funkci orchestrator. Většina orchestrator *function.json* soubory vypadat například takto téměř úplně stejné.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/function.json)]

Je důležité si `orchestrationTrigger` typ vazby. Tento typ aktivační události musí používat všechny funkce produktu orchestrator.

> [!WARNING]
> S dodržováním pravidlem "žádné vstupně-výstupních operací" orchestrator funkcí, nechcete používat žádný vstup nebo výstup vazby při použití `orchestrationTrigger` aktivovat vazby.  Pokud další vstupní nebo výstupní vazby jsou potřeba, by měl místo toho používají v rámci `activityTrigger` funkce.

## <a name="c-script"></a>Skript jazyka C#

Tady je zdrojový kód:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/run.csx)]

Musí mít všechny funkce orchestration C# `DurableOrchestrationContext` parametr, který existuje v `Microsoft.Azure.WebJobs.Extensions.DurableTask` sestavení. Pokud používáte skript jazyka C#, sestavení, můžete odkazovat pomocí `#r` zápis. Tento objekt kontextu umožňuje volat jiné *aktivity* funkce a předejte vstupní parametry pomocí jeho [CallActivityAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_) metoda.

Volání kódu `E1_SayHello` třikrát v sekvenci s jiným parametrem hodnoty. Návratovou hodnotu volání je přidán do `outputs` seznam, který je vrácen na konci funkce.

*Function.json* soubor pro aktivitu funkce `E1_SayHello` je podobná `E1_HelloSequence` s tím rozdílem, že se používá `activityTrigger` vazby typu, nikoli `orchestrationTrigger` typ vazby.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_SayHello/function.json)]

> [!NOTE]
> Musíte použít všechny funkce volá funkce orchestration `activityTrigger` vazby.

Implementace `E1_SayHello` je poměrně trivial řetězec formátování operaci.

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_SayHello/run.csx)]

Tato funkce má [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html)parametr, který se používá k získání vstupu, který byl předán voláním funkce orchestrator [CallActivityAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_)>.

## <a name="run-the-sample"></a>Spustit ukázku

Spuštění `E1_HelloSequence` orchestration, odeslat požadavek na následující HTTP POST.

```
POST http://{host}/orchestrators/E1_HelloSequence
```

Například pokud používáte ukázku v aplikaci funkce s názvem "myfunctionapp", nahraďte "myfunctionapp.azurewebsites.net" "{hostitel}".

Výsledkem je odpověď HTTP 202 takto (oříznut jako stručný výtah):

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

V tomto okamžiku orchestration je zařazen do fronty a začne spustit okamžitě. Adresu URL v `Location` záhlaví lze použít ke kontrole stavu spuštění.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

Výsledkem je, stav orchestration. Ho běží a ukončuje se rychle, tak, aby ho zobrazila *dokončeno* stavu s odpovědí, který bude vypadat takto (oříznut jako stručný výtah):

```
HTTP/1.1 200 OK
Content-Length: 179
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":null,"output":["Hello Tokyo!","Hello Seattle!","Hello London!"],"createdTime":"2017-06-29T05:24:57Z","lastUpdatedTime":"2017-06-29T05:24:59Z"}
```

Jak je vidět `runtimeStatus` instance je *dokončeno* a `output` obsahuje serializací JSON výsledek spuštění funkce produktu orchestrator.

> [!NOTE]
> Koncový bod HTTP POST, který spuštění funkce orchestrator je implementovaná v ukázkové aplikace jako HTTP aktivovat funkci s názvem "HttpStart". Můžete implementovat podobné starter logiku pro jiné typy aktivační události, jako je třeba `queueTrigger`, `eventHubTrigger`, nebo `timerTrigger`.

Podívejte se na protokoly spuštění funkce. `E1_HelloSequence` Funkce spuštěno a dokončeno vícekrát z důvodu opětovného přehrání problém popsaný v [přehled](durable-functions-overview.md). Na druhé straně nebyly jenom tři spuštěních z `E1_SayHello` vzhledem k tomu, že tyto funkce spuštěních získat přehrány není.

## <a name="visual-studio-sample-code"></a>Visual Studio ukázkový kód

Tady je orchestration jako jeden soubor jazyka C# v projektu sady Visual Studio:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

## <a name="next-steps"></a>Další kroky

Tato ukázka vám ukázal jednoduché řetězení funkce orchestration. Další příklad ukazuje, jak implementovat fan odesílacího/fan v vzor. 

> [!div class="nextstepaction"]
> [Spustit Fan odesílacího/fan v ukázce](durable-functions-cloud-backup.md)
