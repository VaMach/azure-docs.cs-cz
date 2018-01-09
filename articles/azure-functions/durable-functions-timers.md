---
title: "Časovače trvanlivý funkcí – Azure"
description: "Zjistěte, jak implementovat trvanlivý časovače v rozšíření trvanlivý funkce pro Azure Functions."
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
ms.openlocfilehash: e29e472860890e3f44af79c42c31ff524acb9276
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/08/2018
---
# <a name="timers-in-durable-functions-azure-functions"></a>Časovače trvanlivý funkcí (Azure Functions)

[Trvanlivý funkce](durable-functions-overview.md) poskytuje *trvanlivý časovače* pro použití v orchestrator funkce pro implementaci zpoždění nebo nastavit časové limity na asynchronní akce. Trvanlivý časovače je třeba použít funkce orchestrator místo `Thread.Sleep` nebo `Task.Delay`.

Vytvoření odolné časovač voláním [CreateTimer](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CreateTimer_) metoda v [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html). Metoda vrátí úlohu, která obnoví u zadaného data a času.

## <a name="timer-limitations"></a>Omezení časovače

Při vytváření, jejíž platnost vyprší ve 4:30, základní enqueues trvanlivý Framework úloh zprávu, která se zobrazí pouze ve 4:30 pm časovač. Při spuštění v plánu spotřeby Azure Functions, nově viditelné časovače zpráva bude zajištěno, že funkce aplikace získá aktivovat na příslušný virtuální počítač.

> [!WARNING]
> * Trvanlivý časovače nelze poslední delší než 7 dní z důvodu omezení v Azure Storage. Pracujeme na [žádost o funkce rozšířit časovače za 7 dní](https://github.com/Azure/azure-functions-durable-extension/issues/14).
> * Vždy používat [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) místo `DateTime.UtcNow` podle příkladů níže při výpočtu relativní konečného termínu trvanlivý časovače.

## <a name="usage-for-delay"></a>Využití pro zpoždění

Následující příklad ukazuje, jak použít časovačů trvanlivý pro zpozdit provádění. V příkladu vydává fakturace oznámení každý den pro deset dní.

```csharp
[FunctionName("BillingIssuer")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    for (int i = 0; i < 10; i++)
    {
        DateTime deadline = context.CurrentUtcDateTime.Add(TimeSpan.FromDays(1));
        await context.CreateTimer(deadline, CancellationToken.None);
        await context.CallFunctionAsync("SendBillingEvent");
    }
}
```

> [!WARNING]
> Vyhněte se nekonečné smyčky funkcí produktu orchestrator. Informace o tom, jak bezpečně a efektivně implementovat scénáře nekonečná smyčka najdete v tématu [Eternal Orchestrations](durable-functions-eternal-orchestrations.md). 

## <a name="usage-for-timeout"></a>Použití časového limitu

Tento příklad ukazuje, jak používat trvanlivý časovače implementovat vypršení časových limitů.

```csharp
[FunctionName("TryGetQuote")]
public static async Task<bool> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    TimeSpan timeout = TimeSpan.FromSeconds(30);
    DateTime deadline = context.CurrentUtcDateTime.Add(timeout);

    using (var cts = new CancellationTokenSource())
    {
        Task activityTask = context.CallFunctionAsync("GetQuote");
        Task timeoutTask = context.CreateTimer(deadline, cts.Token);

        Task winner = await Task.WhenAny(activityTask, timeoutTask);
        if (winner == activityTask)
        {
            // success case
            cts.Cancel();
            return true;
        }
        else
        {
            // timeout case
            return false;
        }
    }
}
```

> [!WARNING]
> Použití `CancellationTokenSource` trvanlivý časovač zrušit, pokud kód nebude počkat na její dokončení. Trvanlivý Framework úlohy nedojde ke změně stavu orchestration "dokončeno", dokud se všechny zbývající úlohy byla dokončena nebo zrušena.

Tento mechanismus není ve skutečnosti ukončit provedení funkce probíhající aktivity. Místo toho jednoduše umožní ignorovat výsledek a přechod na funkce produktu orchestrator. Pokud vaše aplikace funkce používá plánu spotřeby, je stále platit pro všechny čas a paměti používané ve funkci opuštěného aktivity. Ve výchozím nastavení mají funkce, které jsou spuštěné v plánu spotřeby vypršení časového limitu pěti minut. Pokud je tento limit překročen, hostitele Azure Functions zastavit všechny provádění a zabránit nezvladatelné situaci fakturace recyklován. [Funkce vypršení časového limitu je možné konfigurovat](functions-host-json.md#functiontimeout).

Podrobnější příklad, jak implementovat vypršení časových limitů ve funkcích orchestrator, najdete v článku [zásahem ze strany & časové limity - ověření telefonu](durable-functions-phone-verification.md) návod.

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Zjistěte, jak vyvolat a zpracování externích událostí](durable-functions-external-events.md)

