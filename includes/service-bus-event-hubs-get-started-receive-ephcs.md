## <a name="receive-messages-with-eventprocessorhost"></a>Přijímání zpráv pomocí třídy EventProcessorHost
[EventProcessorHost][EventProcessorHost] je třída rozhraní .NET, která zjednodušuje přijímání události ze služby Event Hubs tím, že spravuje trvalé kontrolní body a paralelní příjemce událostí ze služby Event Hubs. Pomocí třídy [EventProcessorHost][EventProcessorHost] můžete události rozdělit mezi několik příjemců, i když jsou hostované v různých uzlech. Tento příklad ukazuje způsob použití třídy [EventProcessorHost][EventProcessorHost] pro jednoho příjemce. Ukázka metody [Škálované zpracování událostí][Škálované zpracování událostí] znázorňuje způsob použití třídy [EventProcessorHost][EventProcessorHost] v případě několika příjemců.

K používání třídy [EventProcessorHost][EventProcessorHost] potřebujete [účet služby Azure Storage][účet služby Azure Storage]:

1. Přihlaste se na web [Azure Portal][Azure Portal] a v levém horním rohu obrazovky klikněte na **Nový**.
2. Klikněte na **Data + Úložiště** a poté klikněte na **Účet úložiště**.
   
    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage1.png)
3. V okně **Vytvořit účet úložiště** zadejte název účtu úložiště. Zvolte předplatné Azure, skupinu prostředků a umístění, ve kterém se má prostředek vytvořit. Poté klikněte na **Vytvořit**.
   
    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage2.png)
4. V seznamu účtů úložiště klikněte na nově vytvořený účet úložiště.
5. V okně účtu úložiště klikněte na **Přístupové klávesy**. Zkopírujte hodnotu **key1** pro pozdější použití v tomto kurzu.
   
    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage3.png)
6. Pomocí šablony projektu **Konzolová aplikace** vytvořte v sadě Visual Studio nový projekt desktopové aplikace Visual C#. Projekt nazvěte **Receiver** (Příjemce).
   
    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp1.png)
7. V Průzkumníku řešení klikněte pravým tlačítkem na řešení a potom na **Správa balíčků NuGet pro řešení**.
8. Klikněte na kartu **Procházení** a potom najděte `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Zkontrolujte, jestli je v okně **Verze** uvedený název projektu (**Receiver**). Klikněte na **Instalovat** a přijměte podmínky použití.
   
    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-eph-csharp1.png)
   
    Visual Studio stáhne, nainstaluje a přidá odkaz na [balíček NuGet třídy EventProcessorHost služby Event Hub ve službě Azure Service Bus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost) se všemi jeho závislostmi.
9. Klikněte pravým tlačítkem na projekt **Receiver**, **Přidat** a potom na **Třída**. Pojmenujte novou třídu **SimpleEventProcessor** a potom kliknutím na **Přidat** třídu vytvořte.
   
    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp2.png)
10. Na začátek souboru SimpleEventProcessor.cs přidejte následující příkazy:
    
     ```
     using Microsoft.ServiceBus.Messaging;
     using System.Diagnostics;
     ```
    
     Potom nahraďte tělo třídy následujícím kódem:
    
     ```
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
    
     ```
     using Microsoft.ServiceBus.Messaging;
     ```
    
     Potom nahraďte metodu `Main` ve třídě `Program` následujícím kódem, kde nahradíte název centra událostí a připojovací řetězec na úrovni oboru názvů, který jste si dříve uložili, a účet úložiště spolu s klíčem, který jste si v předchozích částech zkopírovali. 
    
     ```
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
> Tento kurz používá jednu instanci třídy [EventProcessorHost][EventProcessorHost]. Pokud chcete zvýšit propustnost, spusťte několik instancí třídy [EventProcessorHost][EventProcessorHost], jak je znázorněno v ukázce metody [Škálované zpracování událostí][Škálované zpracování událostí]. V těchto případech se spolu různé instance navzájem automaticky koordinují, aby dokázaly vyrovnávat zatížení přijatých událostí. Pokud chcete, aby každý z několika příjemců zpracovával *všechny* události, musíte použít koncept **ConsumerGroup**. Když přijímáte události z různých počítačů, může být užitečné nazvat instance třídy [EventProcessorHost][EventProcessorHost] podle počítačů (nebo rolí), ve kterých jsou nasazené. Další informace o těchto tématech najdete v článcích [Přehled služby Event Hubs][Přehled služby Event Hubs] a [Průvodce programováním pro službu Event Hubs][Průvodce programováním pro službu Event Hubs].
> 
> 

<!-- Links -->
[Přehled služby Event Hubs]: ../articles/event-hubs/event-hubs-overview.md
[Průvodce programováním pro službu Event Hubs]: ../articles/event-hubs/event-hubs-programming-guide.md
[Škálované zpracování událostí]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[účet služby Azure Storage]: ../articles/storage/storage-create-storage-account.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Azure Portal]: https://portal.azure.com

<!--HONumber=Nov16_HO2-->


