## Přijímání zpráv pomocí třídy EventProcessorHost

[EventProcessorHost][] je třída rozhraní .NET, která zjednodušuje přijímání události ze služby Event Hubs tím, že spravuje trvalé kontrolní body a paralelní příjemce událostí ze služby Event Hubs. Pomocí třídy [EventProcessorHost][] můžete události rozdělit mezi několik příjemců, i když jsou hostované v různých uzlech. Tento příklad ukazuje způsob použití třídy [EventProcessorHost][] pro jednoho příjemce. Ukázka metody [Škálované zpracování událostí][] znázorňuje způsob použití třídy [EventProcessorHost][] v případě několika příjemců.

Pokud chcete [EventProcessorHost][] používat, musíte mít [Účet služby Azure Storage][]:

1. Přihlaste se na [portál Azure Classic][] a v dolní části obrazovky klikněte na **NOVÝ**.

2. Klikněte na **Data Services**, **Úložiště**, **Rychlé vytvoření** a potom zadejte název svého účtu úložiště. Vyberte požadovanou oblast a potom klikněte na **Vytvořit účet úložiště**.

    ![][11]

3. Klikněte na nově vytvořený účet úložiště a potom klikněte na **Spravovat přístupové klíče**:

    ![][12]

    Primární přístupový klíč si zkopírujte pro pozdější použití v tomto kurzu.

4. Pomocí šablony projektu **Konzolová aplikace** vytvořte v sadě Visual Studio nový projekt desktopové aplikace Visual C#. Projekt nazvěte **Receiver** (Příjemce).

    ![][14]

5. V Průzkumníku řešení klikněte pravým tlačítkem na řešení a potom na **Správa balíčků NuGet pro řešení**.

6. Klikněte na kartu **Procházení** a potom najděte `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Zkontrolujte, jestli je v okně **Verze** uvedený název projektu (**Receiver**). Klikněte na **Instalovat** a přijměte podmínky použití.

    ![][13]

    Visual Studio stáhne, nainstaluje a přidá odkaz na [balíček NuGet třídy EventProcessorHost služby Event Hub ve službě Azure Service Bus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost) se všemi jeho závislostmi.

7. Klikněte pravým tlačítkem na projekt **Receiver**, **Přidat** a potom na **Třída**. Pojmenujte novou třídu **SimpleEventProcessor** a potom kliknutím na **Přidat** třídu vytvořte.

    ![][15]

8. Na začátek souboru SimpleEventProcessor.cs přidejte následující příkazy:

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

9. Ve třídě **Program** přidejte na začátek souboru následující příkaz `using`:

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

> [AZURE.NOTE] Tento kurz používá jednu instanci třídy [EventProcessorHost][]. Pokud chcete zvýšit propustnost, spusťte několik instancí třídy [EventProcessorHost][], jak je znázorněno v ukázce metody [Škálované zpracování událostí][]. V těchto případech se spolu různé instance navzájem automaticky koordinují, aby dokázaly vyrovnávat zatížení přijatých událostí. Pokud chcete, aby každý z několika příjemců zpracovával *všechny* události, musíte použít koncept **ConsumerGroup**. Když přijímáte události z různých počítačů, může být užitečné nazvat instance třídy [EventProcessorHost][] podle počítačů (nebo rolí), ve kterých jsou nasazené. Další informace o těchto tématech najdete v článcích [Přehled služby Event Hubs][] a [Průvodce programováním pro službu Event Hubs][].

<!-- Links -->
[Přehled služby Event Hubs]: event-hubs-overview.md
[Průvodce programováním pro službu Event Hubs]: event-hubs-programming-guide.md
[Škálované zpracování událostí]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Účet služby Azure Storage]: ../storage/storage-create-storage-account-classic-portal.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[portál Azure Classic]: http://manage.windowsazure.com

<!-- Images -->

[11]: ./media/service-bus-event-hubs-getstarted/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-getstarted/create-eph-csharp3.png
[13]: ./media/service-bus-event-hubs-getstarted/create-eph-csharp1.png
[14]: ./media/service-bus-event-hubs-getstarted/create-receiver-csharp1.png
[15]: ./media/service-bus-event-hubs-getstarted/create-receiver-csharp2.png




<!--HONumber=ago16_HO5-->


