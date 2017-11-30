---
title: "Knihovny tříd rozhraní .NET pomocí Azure Functions | Microsoft Docs"
description: "Naučte se vytvářet knihovny tříd rozhraní .NET pro použití s Azure Functions"
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: 9f5db0c2-a88e-4fa8-9b59-37a7096fc828
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/10/2017
ms.author: glenga
ms.openlocfilehash: 6f6f89d62f1442198f80247cc5c433aa0c54030b
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="using-net-class-libraries-with-azure-functions"></a>Knihovny tříd rozhraní .NET pomocí Azure Functions

Kromě souborů skriptů Azure Functions podporuje publikování knihovny tříd jako implementace pro jednu nebo více funkcí. Doporučujeme vám, že používáte [Azure funkce 2017 nástroje sady Visual Studio](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).

## <a name="prerequisites"></a>Požadavky 

Tento článek má následující požadavky:

- [Visual Studio 2017 verze 15.3](https://www.visualstudio.com/vs/), nebo novější.
- Nainstalujte **Azure development** zatížení.

## <a name="functions-class-library-project"></a>Funkce projektu knihovny tříd

Ze sady Visual Studio vytvořte nový projekt Azure Functions. Nová šablona projektu vytvoří soubory *host.json* a *local.settings.json*. Můžete [přizpůsobit nastavení modulu runtime Azure Functions v host.json](functions-host-json.md). 

Soubor *local.settings.json* ukládá nastavení aplikace, řetězce připojení a nastavení nástroje základní funkce Azure. Další informace o jeho strukturu, najdete v části [kód a testovat místně na Azure functions](functions-run-local.md#local-settings-file).

### <a name="functionname-attribute"></a>Atribut %{FunctionName/

Atribut [ `FunctionNameAttribute` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/FunctionNameAttribute.cs) určí metodu jako vstupní bod funkce. Musíte ho použít se přesně jedna aktivační událost a 0 nebo více vstup a výstup vazby.

### <a name="conversion-to-functionjson"></a>Převod na function.json

Při sestavování projektu Azure Functions *function.json* soubor je vytvořen v adresáři funkce. Název adresáře je stejný jako funkce názvů, které `[FunctionName]` Určuje atribut. *Function.json* soubor obsahuje triggerů a vazeb a odkazuje na sestavení souboru projektu.

Tento převod provádí balíček NuGet [Microsoft\.NET\.Sdk\.funkce](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions). Zdroj je k dispozici v úložišti GitHub [azure\-funkce\-vs\-sestavení\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).

## <a name="triggers-and-bindings"></a>Triggery a vazby 

Následující tabulka uvádí triggerů a vazeb, které jsou k dispozici v projektu knihovny tříd Azure Functions. Všechny atributy jsou v oboru názvů `Microsoft.Azure.WebJobs`.

| Vazba | Atribut | Balíček NuGet |
|------   | ------    | ------        |
| [Aktivační událost úložiště objektů BLOB, vstup a výstup](#blob-storage) | [BlobAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | [Úložiště objektů blob] |
| [Aktivační událost cosmos DB](#cosmos-db) | [CosmosDBTriggerAttribute] | [Microsoft.Azure.WebJobs.Extensions.DocumentDB] | 
| [Cosmos DB vstup a výstup](#cosmos-db) | [DocumentDBAttribute] | [Microsoft.Azure.WebJobs.Extensions.DocumentDB] |
| [Aktivační událost rozbočovače a výstup](#event-hub) | [EventHubTriggerAttribute], [EventHubAttribute] | [Microsoft.Azure.WebJobs.ServiceBus] |
| [Externí soubor vstup a výstup](#api-hub) | [ApiHubFileAttribute] | [Microsoft.Azure.WebJobs.Extensions.ApiHub] |
| [Aktivace protokolu HTTP a webhooku](#http) | [HttpTriggerAttribute] | [Microsoft.Azure.WebJobs.Extensions.Http] |
| [Mobile Apps vstup a výstup](#mobile-apps) | [MobileTableAttribute] | [Microsoft.Azure.WebJobs.Extensions.MobileApps] | 
| [Výstup centra oznámení](#nh) | [NotificationHubAttribute] | [Microsoft.Azure.WebJobs.Extensions.NotificationHubs] | 
| [Aktivace fronty úložiště a výstup](#queue) | [QueueAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | 
| [Sendgrid vám umožňuje výstup](#sendgrid) | [SendGridAttribute] | [Microsoft.Azure.WebJobs.Extensions.SendGrid] | 
| [Aktivace služby Service Bus a výstup](#service-bus) | [ServiceBusAttribute], [ServiceBusAccountAttribute] | [Microsoft.Azure.WebJobs.ServiceBus]
| [Tabulka úložiště vstup a výstup](#table) | [TableAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | 
| [Trigger časovače](#timer) | [TimerTriggerAttribute] | [Microsoft.Azure.WebJobs.Extensions] | 
| [Výstup Twilio](#twilio) | [TwilioSmsAttribute] | [Microsoft.Azure.WebJobs.Extensions.Twilio] | 

<a name="blob-storage"></a>

### <a name="blob-storage-trigger-input-bindings-and-output-bindings"></a>Aktivační událost úložiště objektů BLOB, vazby vstup a výstup vazby

Azure podporuje aktivaci funkce vstup a výstup vazby pro úložiště objektů Blob v Azure. Další informace o výrazy vazby a metadata, najdete v části [vazby úložiště objektů Blob v Azure funkce](functions-bindings-storage-blob.md).

Aktivační události objektu blob je definovaná pomocí `[BlobTrigger]` atribut. Můžete použít atribut `[StorageAccount]` můžete definovat název nastavení aplikace, který obsahuje připojovací řetězec k účtu úložiště, který je používán celé funkce nebo třída.

```csharp
[StorageAccount("AzureWebJobsStorage")]
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Objekt BLOB vstup a výstup jsou definovány pomocí `[Blob]` atribut spolu s `FileAccess` parametr označující číst nebo zapisovat. Následující příklad používá objekt blob aktivační události a objektů blob výstup vazby.

```csharp
[FunctionName("ResizeImage")]
[StorageAccount("AzureWebJobsStorage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall, 
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
{
    var imageBuilder = ImageResizer.ImageBuilder.Current;
    var size = imageDimensionsTable[ImageSize.Small];

    imageBuilder.Build(image, imageSmall,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);

    image.Position = 0;
    size = imageDimensionsTable[ImageSize.Medium];

    imageBuilder.Build(image, imageMedium,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);
}

public enum ImageSize { ExtraSmall, Small, Medium }

private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
    { ImageSize.ExtraSmall, (320, 200) },
    { ImageSize.Small,      (640, 400) },
    { ImageSize.Medium,     (800, 600) }
};
```        

<a name="cosmos-db"></a>

### <a name="cosmos-db-trigger-input-bindings-and-output-bindings"></a>Cosmos DB aktivační událost, vazby vstup a výstup vazby

Azure Functions podporuje aktivační události a vstup a výstup vazby pro Cosmos DB. Další informace o funkcích vazby Cosmos DB najdete v tématu [Azure funkce Cosmos DB vazby](functions-bindings-documentdb.md).

Chcete-li aktivovat z dokumentu Cosmos DB, použijte atribut `[CosmosDBTrigger]` v balíčku NuGet [Microsoft.Azure.WebJobs.Extensions.DocumentDB]. Následující příklad aktivačních událostí z konkrétní `database` a `collection`. Nastavení `myCosmosDB` obsahuje připojení k instanci databáze Cosmos. 

```csharp
[FunctionName("DocumentUpdates")]
public static void Run(
    [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
IReadOnlyList<Document> documents, TraceWriter log)
{
        log.Info("Documents modified " + documents.Count);
        log.Info("First document Id " + documents[0].Id);
}
```

Chcete-li vytvořit vazbu na dokument Cosmos DB, použijte atribut `[DocumentDB]` v balíčku NuGet [Microsoft.Azure.WebJobs.Extensions.DocumentDB]. V následujícím příkladu má aktivační procedury fronty a DocumentDB API, výstup vazby.

```csharp
[FunctionName("QueueToDocDB")]        
public static void Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, 
    [DocumentDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
{
    document = new { Text = myQueueItem, id = Guid.NewGuid() };
}

```

<a name="event-hub"></a>

### <a name="event-hubs-trigger-and-output"></a>Aktivační událost rozbočovače a výstup

Azure Functions podporuje aktivaci a výstupní vazeb pro služby Event Hubs. Další informace najdete v tématu [centra událostí Azure funkce vazby](functions-bindings-event-hubs.md).

Typy `[Microsoft.Azure.WebJobs.ServiceBus.EventHubTriggerAttribute]` a `[Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute]` jsou definovány v balíčku NuGet [Microsoft.Azure.WebJobs.ServiceBus]. 

Následující příklad používá aktivační procedury Centrum událostí:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

V následujícím příkladu má centra událostí výstupu pomocí návratovou hodnotu metody jako výstup:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnection")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

<a name="api-hub"></a>

### <a name="external-file-input-and-output"></a>Externí soubor vstup a výstup

Azure Functions podporuje aktivační událost, vstup a výstup vazby pro externích souborů, jako je například Google Drive, Dropbox a OneDrive. Další informace najdete v tématu [externí soubor funkce Azure vazby](functions-bindings-external-file.md). Atributy `[ExternalFileTrigger]` a `[ExternalFile]` jsou definovány v balíčku NuGet [Microsoft.Azure.WebJobs.Extensions.ApiHub].

Následující příklad jazyka C# ukazuje externí soubor vstup a výstup vazby. Kód zkopíruje vstupní soubor do výstupního souboru.

```csharp
[StorageAccount("MyStorageConnection")]
[FunctionName("ExternalFile")]
[return: ApiHubFile("MyFileConnection", "samples-workitems/{queueTrigger}-Copy", FileAccess.Write)]
public static string Run([QueueTrigger("myqueue-items")] string myQueueItem, 
    [ApiHubFile("MyFileConnection", "samples-workitems/{queueTrigger}", FileAccess.Read)] string myInputFile, 
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    return myInputFile;
}
```

<a name="http"></a>

### <a name="http-and-webhooks"></a>HTTP a webhooky

Použití `HttpTrigger` definici na triggeru protokolu HTTP nebo webhooku atributu. Tento atribut je definována v balíček NuGet [Microsoft.Azure.WebJobs.Extensions.Http]. Můžete přizpůsobit oprávnění na úrovni, typ webhooku, trasy a metody. V následujícím příkladu definuje aktivační procedury HTTP s anonymní ověřování a _genericJson_ webhooku typu.

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a name="mobile-apps"></a>

### <a name="mobile-apps-input-and-output"></a>Mobile Apps vstup a výstup

Azure Functions podporuje vstup a výstup vazby pro Mobile Apps. Další informace najdete v tématu [Azure funkce Mobile Apps vazby](functions-bindings-mobile-apps.md). Atribut `[MobileTable]` je definována v balíček NuGet [Microsoft.Azure.WebJobs.Extensions.MobileApps].

Následující příklad ukazuje Mobile Apps výstup vazby:

```csharp
[FunctionName("MobileAppsOutput")]        
[return: MobileTable(ApiKeySetting = "MyMobileAppKey", TableName = "MyTable", MobileAppUriSetting = "MyMobileAppUri")]
public static object Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, TraceWriter log)
{
    return new { Text = $"I'm running in a C# function! {myQueueItem}" };
}
```

<a name="nh"></a>

### <a name="notification-hubs-output"></a>Výstup centra oznámení

Azure Functions podporuje vazbu výstup pro centra oznámení. Další informace najdete v tématu [centra oznámení Azure funkce výstup vazby](functions-bindings-notification-hubs.md). Atribut `[NotificationHub]` je definována v balíček NuGet [Microsoft.Azure.WebJobs.Extensions.NotificationHubs].

<a name="queue"></a>

### <a name="queue-storage-trigger-and-output"></a>Aktivace fronty úložiště a výstup

Azure Functions podporuje aktivaci a výstupní vazby pro Azure fronty. Další informace najdete v tématu [Azure funkce Queue Storage vazby](functions-bindings-storage-queue.md).

Následující příklad ukazuje, jak používat návratový typ funkce s výstupní fronty, vazbu, pomocí `[Queue]` atribut. 

```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    // HTTP trigger with queue output binding
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  TraceWriter log)
    {
        log.Info($"C# function processed: {input.Text}");
        return input.Text;
    }
}

```

Chcete-li definovat aktivační procedury fronty, použijte `[QueueTrigger]` atribut.
```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    // Queue trigger
    [FunctionName("QueueTrigger")]
    [StorageAccount("AzureWebJobsStorage")]
    public static void QueueTrigger([QueueTrigger("myqueue-items")] string myQueueItem, TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}

```


<a name="sendgrid"></a>

### <a name="sendgrid-output"></a>Sendgrid vám umožňuje výstup

Azure Functions podporuje sendgrid vám umožňuje výstup, vazba pro odesílání e-mailu prostřednictvím kódu programu. Další informace najdete v tématu [sendgrid vám umožňuje funkce Azure vazby](functions-bindings-sendgrid.md).

Atribut `[SendGrid]` je definována v balíček NuGet [Microsoft.Azure.WebJobs.Extensions.SendGrid]. Vazba sendgrid vám umožňuje vyžaduje aplikaci nastavení s názvem `AzureWebJobsSendGridApiKey`, který obsahuje klíč sendgrid vám umožňuje rozhraní API. Toto je výchozí název nastavení pro klíč sendgrid vám umožňuje rozhraní API. Pokud je potřeba mít více než jeden Sendgridu klíčů nebo zvolte název jiné nastavení, můžete nastavit pomocí tento název `ApiKey` vlastnost `SendGrid` atribut vazby, jako v následujícím příkladu:

    [SendGrid(ApiKey = "MyCustomSendGridKeyName")]

Následuje příklad použití aktivační procedury fronty Service Bus a pomocí vazby sendgrid vám umožňuje výstup `SendGridMessage`:

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid] out SendGridMessage message)
{
    message = new SendGridMessage();
    message.AddTo(email.To);
    message.AddContent("text/html", email.Body);
    message.SetFrom(new EmailAddress(email.From));
    message.SetSubject(email.Subject);
}

public class OutgoingEmail
{
    public string To { get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```
Všimněte si, že tento příklad vyžaduje klíč rozhraní API sendgrid vám umožňuje jako úložiště v aplikaci nastavení s názvem `AzureWebJobsSendGridApiKey`.

<a name="service-bus"></a>

### <a name="service-bus-trigger-and-output"></a>Aktivace služby Service Bus a výstup

Azure Functions podporuje aktivaci a výstupní vazby pro fronty sběrnice a témata. Další informace o konfiguraci vazby najdete v tématu [Azure funkce Service Bus vazby](functions-bindings-service-bus.md).

Atributy `[ServiceBusTrigger]` a `[ServiceBus]` jsou definovány v balíčku NuGet [Microsoft.Azure.WebJobs.ServiceBus]. 

Následuje příklad aktivační procedury fronty sběrnice:

```csharp
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run([ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

Následuje příklad výstupu Service Bus, vazbu, pomocí návratový typ metody jako výstup:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, TraceWriter log)
{
    log.Info($"C# function processed: {input.Text}");
    return input.Text;
}
```        

<a name="table"></a>

### <a name="table-storage-input-and-output"></a>Tabulka úložiště vstup a výstup

Azure Functions podporuje vstup a výstup vazby pro Azure Table storage. Další informace najdete v tématu [vazby úložiště Azure Table funkce](functions-bindings-storage-table.md).

V následujícím příkladu je třída s dvě funkce, ukázka tabulky úložiště výstupní a vstupní vazby. 

```csharp
[StorageAccount("AzureWebJobsStorage")]
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableOutput")]
    [return: Table("MyTable")]
    public static MyPoco TableOutput([HttpTrigger] dynamic input, TraceWriter log)
    {
        log.Info($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }

    // use the metadata parameter "queueTrigger" to bind the queue payload
    [FunctionName("TableInput")]
    public static void TableInput([QueueTrigger("table-items")] string input, [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, TraceWriter log)
    {
        log.Info($"C# function processed: {poco.Text}");
    }
}

```

<a name="timer"></a>

### <a name="timer-trigger"></a>Trigger časovače

Azure Functions nabízí vazbu aktivační událost časovače, který vám umožní spustit funkce kódu podle definovaného plánu. Další informace o funkcích vazby naleznete v tématu [naplánovat provádění kódu s Azure Functions](functions-bindings-timer.md).

Spotřeba plánu, můžete definovat plány se [výraz CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression). Pokud používáte plánu služby App Service, můžete taky řetězec časový interval. 

V následujícím příkladu definuje aktivační událost časovače, která se spouští každých pět minut:

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

<a name="twilio"></a>

### <a name="twilio-output"></a>Výstup Twilio

Azure Functions podporuje Twilio výstupní vazby k povolení funkcí k posílání textových zpráv SMS. Další informace najdete v tématu [poslat SMS zprávy z Azure Functions pomocí Twilio výstup vazby](functions-bindings-twilio.md). 

Atribut `[TwilioSms]` je definována v balíčku [Microsoft.Azure.WebJobs.Extensions.Twilio].

Následující příklad jazyka C# používá fronty aktivační události a Twilio výstupní vazby:

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static SMSMessage Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order, TraceWriter log)
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

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Další informace o Azure functions triggerů a vazeb](functions-triggers-bindings.md)

<!-- NuGet packages --> 
[Microsoft.Azure.WebJobs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.DocumentDB]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB/1.1.0-beta4
[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.MobileApps]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps/1.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs/1.1.0-beta1
[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.SendGrid]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.Http]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http/1.0.0-beta1
[Microsoft.Azure.WebJobs.Extensions.BotFramework]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.BotFramework/1.0.15-beta
[Microsoft.Azure.WebJobs.Extensions.ApiHub]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ApiHub/1.0.0-beta4
[Microsoft.Azure.WebJobs.Extensions.Twilio]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio/1.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions/2.1.0-beta1


<!-- Links to source --> 
[DocumentDBAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs
[CosmosDBTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/Trigger/CosmosDBTriggerAttribute.cs
[EventHubAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs
[EventHubTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs
[MobileTableAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs
[NotificationHubAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs 
[ServiceBusAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs
[ServiceBusAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs
[QueueAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs
[StorageAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs
[BlobAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs
[TableAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs
[TwilioSmsAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs
[SendGridAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs
[HttpTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs
[ApiHubFileAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.ApiHub/ApiHubFileAttribute.cs
[TimerTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs
