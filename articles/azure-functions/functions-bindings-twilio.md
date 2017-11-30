---
title: Azure funkce Twilio vazby
description: "Pochopit, jak pomocí Azure Functions Twilio vazby."
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: wesmc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ae97045c27f3ad8b62e7798b2060ea59ccd66ac5
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="twilio-binding-for-azure-functions"></a>Twilio vazby pro Azure Functions

Tento článek vysvětluje postup odesílání textové zprávy pomocí [Twilio](https://www.twilio.com/) vazeb v Azure Functions. Azure funkce podporuje výstup vazby pro Twilio.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="example"></a>Příklad

Podívejte se na konkrétní jazyk příklad:

* [Předkompilované C#](#c-example)
* [Skript jazyka C#](#c-script-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>Příklad jazyka C#

Následující příklad ukazuje [předkompilovaných C# funkce](functions-dotnet-class-library.md) , odešle textovou zprávu při aktivaci pomocí zprávy fronty.

```cs
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static SMSMessage Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new SMSMessage()
    {
        Body = $"Hello {order["name"]}, thanks for your order!",
        To = order["mobileNumber"].ToString()
    };

    return message;
}
```

Tento příklad používá `TwilioSms` atribut s návratovou hodnotu metody. Další možností je použít atribut s `out SMSMessage` parametr nebo `ICollector<SMSMessage>` nebo `IAsyncCollector<SMSMessage>` parametr.

### <a name="c-script-example"></a>Příklad skriptu jazyka C#

Následující příklad ukazuje výstup Twilio vazby ve *function.json* souboru a [funkce skriptu jazyka C#](functions-reference-csharp.md) používající vazby. Funkce, která používá `out` parametr pro odeslání textové zprávy.

Zde je vazba dat v *function.json* souboru:

Příklad function.json:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

Zde je kód skriptu jazyka C#:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    message = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    message.Body = msg;
    message.To = order.mobileNumber;
}
```

Nelze použít výstupní parametry v asynchronní kódu. Tady je asynchronní C# skript kód příklad:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    SMSMessage smsText = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.To = order.mobileNumber;

    await message.AddAsync(smsText);
}
```

### <a name="javascript-example"></a>Příklad v jazyce JavaScript

Následující příklad ukazuje výstup Twilio vazby ve *function.json* souboru a [funkce JavaScript, která](functions-reference-node.md) používající vazby.

Zde je vazba dat v *function.json* souboru:

Příklad function.json:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

Tady je kód jazyka JavaScript:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        to : myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="attributes"></a>Atributy

Pro [předkompilovaných C#](functions-dotnet-class-library.md) používat funkce, [TwilioSms](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) atribut, který je definován v balíčku NuGet [Microsoft.Azure.WebJobs.Extensions.Twilio](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio).

Informace o vlastnostech atributů, které můžete konfigurovat, najdete v tématu [konfigurace](#configuration). Tady je `TwilioSms` atribut příkladu podpis metody:

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(
    AccountSidSetting = "TwilioAccountSid", 
    AuthTokenSetting = "TwilioAuthToken", 
    From = "+1425XXXXXXX" )]
public static SMSMessage Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
    TraceWriter log)
{
    ...
}
 ```

Úplný příklad najdete v tématu [příklad předkompilovaných jazyka C#](#c-example).

## <a name="configuration"></a>Konfigurace

Následující tabulka popisuje vlastnosti konfigurace vazby, které jste nastavili v *function.json* souboru a `TwilioSms` atribut.

|Vlastnost Function.JSON | Vlastnost atributu |Popis|
|---------|---------|----------------------|
|**Typ**|| musí být nastavena na `twilioSms`.|
|**směr**|| musí být nastavena na `out`.|
|**Jméno**|| Název proměnné používá v kódu funkce pro textovou zprávu Twilio SMS. |
|**accountSid**|**AccountSid**| Tato hodnota musí být nastavena na název nastavení aplikace, která obsahuje identifikátor Sid účtu Twilio.|
|**ověřovacího tokenu**|**Ověřovacího tokenu**| Tato hodnota musí být nastavena na název nastavení aplikace, která obsahuje vaše Twilio ověřovací token.|
|**k**|**Komu**| Tato hodnota nastavena na telefonní číslo, který je odeslán textová zpráva.|
|**z**|**Z**| Tato hodnota nastavena na telefonní číslo, který je odeslán textová zpráva z.|
|**text**|**Text**| Tato hodnota slouží k pevného code textovou zprávu SMS, pokud nepotřebujete dynamické nastavení v kódu pro funkce. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Další informace o Azure functions triggerů a vazeb](functions-triggers-bindings.md)


