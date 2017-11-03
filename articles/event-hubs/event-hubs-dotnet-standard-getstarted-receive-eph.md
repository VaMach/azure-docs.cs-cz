---
title: "Přijímat události z Azure Event Hubs pomocí rozhraní .NET Standard | Microsoft Docs"
description: "Začínáme příjem zpráv pomocí knihovny EventProcessorHost ve standardní rozhraní .NET"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2017
ms.author: sethm
ms.openlocfilehash: cc62792dad0284f9514664795fdfb32e94a85943
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-receiving-messages-with-the-event-processor-host-in-net-standard"></a>Začínáme v rozhraní .NET standardní přijímání zpráv pomocí třídy Eventprocessorhost

> [!NOTE]
> Tato ukázka je dostupná na [Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).

Tento kurz ukazuje, jak psát aplikace konzoly .NET Core, která přijímá zprávy z centra událostí pomocí **EventProcessorHost**. Můžete spustit [Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) řešení jako-se, řetězce nahrazení hodnoty události rozbočovače a úložiště účtu. Nebo můžete provést kroky v tomto kurzu k vytvoření vlastní.

## <a name="prerequisites"></a>Požadavky

* [Sadu Microsoft Visual Studio 2015 nebo 2017](http://www.visualstudio.com). Příklady v tento kurz použijte Visual Studio 2017, ale Visual Studio 2015 je také podporována.
* [.NET core Visual Studio 2015 nebo 2017 nástroje](http://www.microsoft.com/net/core).
* Předplatné Azure.
* Na obor názvů Azure Event Hubs.
* Účet úložiště Azure.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Vytvoření oboru názvů Event Hubs a centra událostí  

Prvním krokem je použití [portál Azure](https://portal.azure.com) vytvořit obor názvů pro daný typ služby Event Hubs a získat přihlašovací údaje správy, které aplikace potřebuje komunikovat s centrem událostí. Pokud chcete vytvořit obor názvů a event hub, postupujte podle pokynů v [v tomto článku](event-hubs-create.md)a poté pokračujte podle následujících pokynů.  

## <a name="create-an-azure-storage-account"></a>Vytvoření účtu úložiště Azure  

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).  
2. V levém navigačním podokně portálu klikněte na **nový**, klikněte na tlačítko **úložiště**a potom klikněte na **účet úložiště**.  
3. Vyplňte pole v okně účtu úložiště a pak klikněte na tlačítko **vytvořit**.

    ![Vytvořit účet úložiště][1]

4. Jakmile se zobrazí **úspěšné nasazení** zprávy, klikněte na název nového účtu úložiště. V **Essentials** okně klikněte na tlačítko **objekty BLOB**. Když **služba objektů Blob** okno otevře, klikněte na tlačítko **+ kontejner** v horní části. Zadejte název kontejneru a pak zavřete **služba objektů Blob** okno.  
5. Klikněte na tlačítko **přístupové klíče** v levém okně a zkopírujte název kontejneru úložiště, účet úložiště a hodnota **key1**. Uložte tyto hodnoty do programu Poznámkový blok nebo některé dočasné umístění.  

## <a name="create-a-console-application"></a>Vytvoření konzolové aplikace

Spusťte Visual Studio. V nabídce **Soubor** klikněte na položku **Nový** a potom klikněte na položku **Projekt**. Vytvoření aplikace konzoly .NET Core.

![Nový projekt][2]

## <a name="add-the-event-hubs-nuget-package"></a>Přidejte balíček NuGet centra událostí

Přidat [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) a [ `Microsoft.Azure.EventHubs.Processor` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET standardní knihovna NuGet balíčky do projektu pomocí následujících kroků: 

1. Klikněte pravým tlačítkem na nově vytvořený projekt a vyberte možnost **Spravovat balíčky NuGet**.
2. Klikněte na tlačítko **Procházet** kartu a potom vyhledejte "Microsoft.Azure.EventHubs" a vyberte **Microsoft.Azure.EventHubs** balíčku. Klikněte na **Instalovat** a dokončete instalaci, pak zavřete dialogové okno.
3. Opakujte kroky 1 a 2 a nainstalujte **Microsoft.Azure.EventHubs.Processor** balíčku.

## <a name="implement-the-ieventprocessor-interface"></a>Implementace rozhraní IEventProcessor

1. V Průzkumníku řešení klikněte pravým tlačítkem na projekt, klikněte na tlačítko **přidat**a potom klikněte na **třída**. Pojmenujte novou třídu **SimpleEventProcessor**.

2. Otevřete na začátek souboru SimpleEventProcessor.cs a přidejte následující `using` příkazů do horní části souboru.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. Implementace `IEventProcessor` rozhraní. Nahradí celý obsah `SimpleEventProcessor` třídy následujícím kódem:

    ```csharp
    public class SimpleEventProcessor : IEventProcessor
    {
        public Task CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine($"Processor Shutting Down. Partition '{context.PartitionId}', Reason: '{reason}'.");
            return Task.CompletedTask;
        }

        public Task OpenAsync(PartitionContext context)
        {
            Console.WriteLine($"SimpleEventProcessor initialized. Partition: '{context.PartitionId}'");
            return Task.CompletedTask;
        }

        public Task ProcessErrorAsync(PartitionContext context, Exception error)
        {
            Console.WriteLine($"Error on Partition: {context.PartitionId}, Error: {error.Message}");
            return Task.CompletedTask;
        }

        public Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (var eventData in messages)
            {
                var data = Encoding.UTF8.GetString(eventData.Body.Array, eventData.Body.Offset, eventData.Body.Count);
                Console.WriteLine($"Message received. Partition: '{context.PartitionId}', Data: '{data}'");
            }

            return context.CheckpointAsync();
        }
    }
    ```

## <a name="write-a-main-console-method-that-uses-the-simpleeventprocessor-class-to-receive-messages"></a>Napíše metoda hlavní konzoly, která používá třídu SimpleEventProcessor pro příjem zpráv

1. Do horní části souboru Program.cs přidejte následující příkazy `using`.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. Přidejte konstanty k `Program` třídu pro událost rozbočovače připojovací řetězec, název centra událostí, název kontejneru účtu úložiště, název účtu úložiště a klíč účtu úložiště. Přidejte následující kód, jejich příslušné hodnoty nahraďte zástupné symboly.

    ```csharp
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. Přidat novou metodu s názvem `MainAsync` k `Program` třídy následujícím způsobem:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        Console.WriteLine("Registering EventProcessor...");

        var eventProcessorHost = new EventProcessorHost(
            EhEntityPath,
            PartitionReceiver.DefaultConsumerGroupName,
            EhConnectionString,
            StorageConnectionString,
            StorageContainerName);

        // Registers the Event Processor Host and starts receiving messages
        await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

        Console.WriteLine("Receiving. Press ENTER to stop worker.");
        Console.ReadLine();

        // Disposes of the Event Processor Host
        await eventProcessorHost.UnregisterEventProcessorAsync();
    }
    ```

3. Přidejte následující řádek kódu `Main` metoda:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    Soubor Program.cs by měl vypadat takhle:

    ```csharp
    namespace SampleEphReceiver
    {

        public class Program
        {
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";
            private const string StorageContainerName = "{Storage account container name}";
            private const string StorageAccountName = "{Storage account name}";
            private const string StorageAccountKey = "{Storage account key}";

            private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                Console.WriteLine("Registering EventProcessor...");

                var eventProcessorHost = new EventProcessorHost(
                    EhEntityPath,
                    PartitionReceiver.DefaultConsumerGroupName,
                    EhConnectionString,
                    StorageConnectionString,
                    StorageContainerName);

                // Registers the Event Processor Host and starts receiving messages
                await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

                Console.WriteLine("Receiving. Press ENTER to stop worker.");
                Console.ReadLine();

                // Disposes of the Event Processor Host
                await eventProcessorHost.UnregisterEventProcessorAsync();
            }
        }
    }
    ```

4. Spusťte program a zkontrolujte, že nejsou žádné chyby.

Blahopřejeme! Nyní máte přijímá zprávy z centra událostí pomocí Event Processor Host.

## <a name="next-steps"></a>Další kroky
Další informace o službě Event Hubs najdete na následujících odkazech:

* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
* [Vytvoření centra událostí](event-hubs-create.md)
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcore.png
