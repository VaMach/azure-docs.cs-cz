---
title: "Správa instancí trvanlivý funkcí – Azure"
description: "Naučte se spravovat instancí v rozšíření trvanlivý funkce pro Azure Functions."
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
ms.openlocfilehash: a938e5949896ad3bfa91903106d56ccdf827c725
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="manage-instances-in-durable-functions-azure-functions"></a>Správa instancí trvanlivý funkcí (Azure Functions)

[Trvanlivý funkce](durable-functions-overview.md) orchestration instance může být spuštěna, byla ukončena, dotaz a odeslat oznámení události. Správa všech instancí se provádí pomocí [orchestration klienta vazby](durable-functions-bindings.md). Tento článek přejde na podrobné informace o každé instance operace správy.

## <a name="starting-instances"></a>Výchozí instance

[StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) metodu [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) spustí novou instanci funkce produktu orchestrator. Instance této třídy lze získat pomocí `orchestrationClient` vazby. Interně tato to metoda enqueues zprávu do fronty ovládací prvek, který potom aktivuje spuštění funkce se zadaným názvem, který používá `orchestrationTrigger` aktivovat vazby.

Parametry, které chcete [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) jsou následující:

* **Název**: název funkce orchestrator při plánování.
* **Vstupní**: žádné JSON serializovatelný data, která mají být předány jako vstup do funkce produktu orchestrator.
* **Identifikátor InstanceId**: (volitelné) Jedinečný ID instance. Pokud není zadaný, vygeneruje se ID náhodných instance.

Zde je jednoduchý příklad C#:

```csharp
[FunctionName("HelloWorldManualStart")]
public static async Task Run(
    [ManualTrigger] string input,
    [OrchestrationClient] DurableOrchestrationClient starter,
    TraceWriter log)
{
    string instanceId = await starter.StartNewAsync("HelloWorld", input);
    log.Info($"Started orchestration with ID = '{instanceId}'.");
}
```

Pro jazyky rozhraní .NET, funkce výstup vazby lze spustit také nové instance. V takovém případě můžete použít jakýkoli JSON-serializovatelný objekt, který má tři výše uvedených parametrů jako pole. Zvažte například následující funkce Node.js:

```js
module.exports = function (context, input) {
    var id = generateSomeUniqueId();
    context.bindings.starter = [{
        FunctionName: "HelloWorld",
        Input: input,
        InstanceId: id
    }];

    context.done(null);
};
```

> [!NOTE]
> Doporučujeme použít identifikátor náhodných pro ID instance. To vám pomůže zajištění jako distribučního stejné zatížení při orchestrator funkce škálování mezi několika virtuálními počítači. Správný čas používat-náhodné instance ID je při ID musí pocházet z externího zdroje nebo při implementaci [singleton orchestrator](durable-functions-singletons.md) vzor.

## <a name="querying-instances"></a>Dotazování instancí

[GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) metodu [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) třída dotazuje se na stav instance orchestration. Jak dlouho trvá `instanceId` jako parametr a vrátí objekt s následujícími vlastnostmi:

* **Název**: název funkce produktu orchestrator.
* **Identifikátor InstanceId**: instance ID orchestration (musí být stejná jako `instanceId` vstupní).
* **CreatedTime**: čas, kdy funkce orchestrator spuštění.
* **LastUpdatedTime**: čas, kdy orchestration poslední kontrolní bod.
* **Vstupní**: vstup funkce jako hodnotu JSON.
* **Výstup**: výstup funkce jako hodnota JSON (Pokud je funkce byla dokončena). Pokud funkce orchestrator se nezdařilo, tato vlastnost bude obsahovat podrobnosti o chybě. Pokud funkci orchestrator bylo ukončeno, tato vlastnost bude obsahovat zadaný důvod pro ukončení (pokud existuje).
* **RuntimeStatus**: jeden z následujících hodnot:
    * **Spuštění**: instance byl spuštěn.
    * **Dokončit**: instance dokončil normálně.
    * **ContinuedAsNew**: instance samotného s novou historie restartování. Toto je přechodný stav.
    * **Se nezdařilo**: instance došlo k chybě.
    * **Byl ukončen**: instance náhle byl ukončen.
    
Tato metoda vrátí hodnotu `null` Pokud instance neexistuje nebo ještě nezačala systémem.

```csharp
[FunctionName("GetStatus")]
public static async Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    var status = await client.GetStatusAsync(instanceId);
    // do something based on the current status.
}
```

> [!NOTE]
> Instance dotazu je aktuálně podporuje jenom pro orchestrator funkcí jazyka C#.

## <a name="terminating-instances"></a>Ukončení instance

Spuštěné instance můžete ukončit pomocí [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) metodu [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) třídy. Jsou dva parametry `instanceId` a `reason` řetězce, který se zapisují do protokolů a stav instance. Ukončenou instance se zastaví také nedosáhne Další `await` bod, nebo se ukončí okamžitě pokud už je na `await`.

```csharp
[FunctionName("TerminateInstance")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "It was time to be done.";
    return client.TerminateAsync(instanceId, reason);
}
```

> [!NOTE]
> Instance ukončení aktuálně je podporována pouze pro orchestrator funkcí jazyka C#.

## <a name="sending-events-to-instances"></a>Odesílání událostí do instance

Oznamování událostí lze odeslat buď do spuštěné instance pomocí [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) metodu [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) třídy. Instance, které může zpracovat tyto události jsou ty, které čekají na volání [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_). 

Parametry, které chcete [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) jsou následující:

* **Identifikátor InstanceId**: jedinečné ID instance.
* **EventName**: název události k odeslání.
* **EventData**: datovou část JSON serializovatelný k odeslání do instance.

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

[FunctionName("RaiseEvent")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    int[] eventData = new int[] { 1, 2, 3 };
    return client.RaiseEventAsync(instanceId, "MyEvent", eventData);
}
```

> [!NOTE]
> Vyvolání událostí je momentálně podporován pouze pro orchestrator funkcí jazyka C#.

> [!WARNING]
> Pokud není žádná instance orchestration se zadaným *instance ID* nebo pokud není instance čekání na zadaný *název události*, zpráva o události se zahodí. Další informace o tomto chování najdete v tématu [potíže Githubu](https://github.com/Azure/azure-functions-durable-extension/issues/29).

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Naučte se používat rozhraní API HTTP pro instanci správy](durable-functions-http-api.md)
