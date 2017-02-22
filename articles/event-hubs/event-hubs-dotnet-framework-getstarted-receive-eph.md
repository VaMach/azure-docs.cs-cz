---
title: "Příjem událostí z Azure Event Hubs pomocí rozhraní .NET Framework | Dokumentace Microsoftu"
description: "V tomto kurzu se naučíte přijímat události z Azure Event Hubs pomocí rozhraní .NET Framework."
services: event-hubs
documentationcenter: 
author: jtaubensee
manager: timlt
editor: 
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/30/2017
ms.author: jotaub
translationtype: Human Translation
ms.sourcegitcommit: fe331199333d492dbc42c9125c9da96a44066ee1
ms.openlocfilehash: 4a3656d09f216e5c895bac5d7d680b44d76f4df8


---
# <a name="get-started-with-event-hubs-using-the-net-framework"></a>Začínáme se službou Event Hubs pomocí rozhraní .NET Framework

## <a name="receive-messages-with-eventprocessorhost"></a>Přijímání zpráv pomocí třídy EventProcessorHost
[EventProcessorHost][EventProcessorHost] je třída rozhraní .NET, která zjednodušuje přijímání události ze služby Event Hubs tím, že spravuje trvalé kontrolní body a paralelní příjemce událostí ze služby Event Hubs. Pomocí třídy [EventProcessorHost][EventProcessorHost] můžete události rozdělit mezi několik příjemců, i když jsou hostované v různých uzlech. Tento příklad ukazuje způsob použití třídy [EventProcessorHost][EventProcessorHost] pro jednoho příjemce. Ukázka metody [Horizontální navýšení kapacity zpracování událostí][Scale out Event Processing with Event Hubs] znázorňuje způsob použití třídy [EventProcessorHost][EventProcessorHost] v případě několika příjemců.

Pokud chcete používat třídu [EventProcessorHost][EventProcessorHost], musíte mít [Účet služby Azure Storage][Azure Storage account]:

1. Přihlaste se na web [Azure Portal][Azure portal] a v levém horním rohu obrazovky klikněte na **Nový**.
2. Klikněte na **Storage** a poté klikněte na **Účet úložiště**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage1.png)
3. V okně **Vytvořit účet úložiště** zadejte název účtu úložiště. Zvolte předplatné Azure, skupinu prostředků a umístění, ve kterém se má prostředek vytvořit. Poté klikněte na **Vytvořit**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)
4. V seznamu účtů úložiště klikněte na nově vytvořený účet úložiště.
5. V okně účtu úložiště klikněte na **Přístupové klávesy**. Zkopírujte hodnotu **key1** pro pozdější použití v tomto kurzu.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)
6. Pomocí šablony projektu **Konzolová aplikace** vytvořte v sadě Visual Studio nový projekt desktopové aplikace Visual C#. Projekt nazvěte **Receiver** (Příjemce).
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)
7. V Průzkumníku řešení klikněte pravým tlačítkem na řešení a potom na **Správa balíčků NuGet pro řešení**.
8. Klikněte na kartu **Procházení** a potom najděte `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Zkontrolujte, jestli je v okně **Verze** uvedený název projektu (**Receiver**). Klikněte na **Instalovat** a přijměte podmínky použití.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    Visual Studio stáhne, nainstaluje a přidá odkaz na [balíček NuGet třídy EventProcessorHost služby Event Hub ve službě Azure Service Bus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost) se všemi jeho závislostmi.
9. Klikněte pravým tlačítkem na projekt **Receiver**, **Přidat** a potom na **Třída**. Pojmenujte novou třídu **SimpleEventProcessor** a potom kliknutím na **Přidat** třídu vytvořte.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
10. Na začátek souboru SimpleEventProcessor.cs přidejte následující příkazy:
    
     ```csharp
     using Microsoft.ServiceBus.Messaging;
     using System.Diagnostics;
     ```
    
     Potom nahraďte tělo třídy následujícím kódem:
    
     ```csharp
     class SimpleEventProcessor : IEventProcessor
     {
         Stopwatch checkpointStopWatch;
    
         async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
         {
             Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
             if (reason == CloseReason.Shutdown)
             {
                 await context.CheckpointAsync();
             }
         }
    
         Task IEventProcessor.OpenAsync(PartitionContext context)
         {
             Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
             this.checkpointStopWatch = new Stopwatch();
             this.checkpointStopWatch.Start();
             return Task.FromResult<object>(null);
         }
    
         async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
         {
             foreach (EventData eventData in messages)
             {
                 string data = Encoding.UTF8.GetString(eventData.GetBytes());
    
                 Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                     context.Lease.PartitionId, data));
             }
    
             //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
             if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
             {
                 await context.CheckpointAsync();
                 this.checkpointStopWatch.Restart();
             }
         }
     }
     ```
    
     Tuto třídu bude volat třída **EventProcessorHost** kvůli zpracování událostí přijatých z centra událostí. Všimněte si, že třída `SimpleEventProcessor` používá stopky, aby pravidelně volala metodu kontrolního bodu v kontextu třídy **EventProcessorHost**. Tím je zajištěno, že příjemce v případě restartování neztratí víc než pět minut práce potřebné ke zpracování.
11. Ve třídě **Program** přidejte na začátek souboru následující příkaz `using`:
    
     ```csharp
     using Microsoft.ServiceBus.Messaging;
     ```
    
     Potom nahraďte metodu `Main` ve třídě `Program` následujícím kódem, kde nahradíte název centra událostí a připojovací řetězec na úrovni oboru názvů, který jste si dříve uložili, a účet úložiště spolu s klíčem, který jste si v předchozích částech zkopírovali. 
    
     ```csharp
     static void Main(string[] args)
     {
       string eventHubConnectionString = "{Event Hub connection string}";
       string eventHubName = "{Event Hub name}";
       string storageAccountName = "{storage account name}";
       string storageAccountKey = "{storage account key}";
       string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);
    
       string eventProcessorHostName = Guid.NewGuid().ToString();
       EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
       Console.WriteLine("Registering EventProcessor...");
       var options = new EventProcessorOptions();
       options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
       eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();
    
       Console.WriteLine("Receiving. Press enter key to stop worker.");
       Console.ReadLine();
       eventProcessorHost.UnregisterEventProcessorAsync().Wait();
     }
     ```

> [!NOTE]
> Tento kurz používá jednu instanci třídy [EventProcessorHost][EventProcessorHost]. Pokud chcete zvýšit propustnost, spusťte několik instancí třídy [EventProcessorHost][EventProcessorHost], jak je znázorněno v ukázce metody [Horizontální navýšení kapacity zpracování událostí][Horizontální navýšení kapacity zpracování událostí]. V těchto případech se spolu různé instance navzájem automaticky koordinují, aby dokázaly vyrovnávat zatížení přijatých událostí. Pokud chcete, aby každý z několika příjemců zpracovával *všechny* události, musíte použít koncept **ConsumerGroup**. Když přijímáte události z různých počítačů, může být užitečné nazvat instance třídy [EventProcessorHost][EventProcessorHost] podle počítačů (nebo rolí), ve kterých jsou nasazené. Další informace o těchto tématech najdete v tématech [Přehled služby Event Hubs][Event Hubs Overview] a [Průvodce programováním pro službu Event Hubs][Event Hubs Programming Guide].
> 
> 

<!-- Links -->
[Event Hubs Overview]: event-hubs-overview.md
[Event Hubs Programming Guide]: event-hubs-programming-guide.md
[Azure Storage account]: ../storage/storage-create-storage-account.md
[EventProcessorHost]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Další kroky
Sestavili jste funkční aplikaci, která vytvoří centrum událostí a odesílá i přijímá data. Nyní se můžete dozvědět víc návštěvou následujících odkazů:

* [Event Processor Host](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)
* [Přehled služby Event Hubs][Event Hubs overview]
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs overview]: event-hubs-overview.md
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3


<!--HONumber=Feb17_HO1-->


