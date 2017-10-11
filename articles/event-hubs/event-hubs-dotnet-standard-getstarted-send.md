---
title: "Odesílání událostí do centra událostí Azure pomocí rozhraní .NET Standard | Microsoft Docs"
description: "Začínáme odesílání událostí do centra událostí v rozhraní .NET Standard"
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
ms.openlocfilehash: 8af9d70965c1c9ad8c49b7d2bb04244fc207058d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-sending-messages-to-azure-event-hubs-in-net-standard"></a>Začínáme s Azure Event Hubs v rozhraní .NET standardní zasílání zpráv

> [!NOTE]
> Tato ukázka je dostupná na [Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).

Tento kurz ukazuje, jak psát aplikace konzoly .NET Core odeslaná sadu zpráv do centra událostí. Můžete spustit [Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) řešení jako-, nahrazuje `EhConnectionString` a `EhEntityPath` řetězce hodnotami centra událostí. Nebo můžete provést kroky v tomto kurzu k vytvoření vlastní.

## <a name="prerequisites"></a>Požadavky

* [Sadu Microsoft Visual Studio 2015 nebo 2017](http://www.visualstudio.com). Příklady v tento kurz použijte Visual Studio 2017, ale Visual Studio 2015 je také podporována.
* [.NET core Visual Studio 2015 nebo 2017 nástroje](http://www.microsoft.com/net/core).
* Předplatné Azure.
* Na obor názvů centra událostí.

K odesílání zpráv do centra událostí, budeme používat Visual Studio k zápisu konzolovou aplikaci C#.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Vytvoření oboru názvů Event Hubs a centra událostí

Prvním krokem je použití [portál Azure](https://portal.azure.com) vytvořit obor názvů pro typ rozbočovače události a získat přihlašovací údaje správy, které aplikace potřebuje komunikovat s centrem událostí. Pokud chcete vytvořit obor názvů a centra událostí, postupujte podle pokynů v [v tomto článku](event-hubs-create.md)a poté pokračujte podle následujících pokynů.

## <a name="create-a-console-application"></a>Vytvoření konzolové aplikace

Spusťte Visual Studio. V nabídce **Soubor** klikněte na položku **Nový** a potom klikněte na položku **Projekt**. Vytvoření aplikace konzoly .NET Core.

![Nový projekt][1]

## <a name="add-the-event-hubs-nuget-package"></a>Přidejte balíček NuGet centra událostí

Přidat [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET standardní knihovny balíček NuGet do projektu pomocí následujících kroků: 

1. Klikněte pravým tlačítkem na nově vytvořený projekt a vyberte možnost **Spravovat balíčky NuGet**.
2. Klikněte na tlačítko **Procházet** kartu a potom vyhledejte "Microsoft.Azure.EventHubs" a vyberte **Microsoft.Azure.EventHubs** balíčku. Klikněte na **Instalovat** a dokončete instalaci, pak zavřete dialogové okno.

## <a name="write-some-code-to-send-messages-to-the-event-hub"></a>Napsat kód, který odesílání zpráv do centra událostí

1. Do horní části souboru Program.cs přidejte následující příkazy `using`.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Přidejte konstanty k `Program` třídu pro Event Hubs připojovací řetězec a entity cesta (název centra jednotlivých událostí). Nahraďte zástupné symboly v závorkách správné hodnoty, které byly získány při vytváření centra událostí.

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. Přidat novou metodu s názvem `MainAsync` k `Program` třídy následujícím způsobem:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
        // we are using the connection string from the namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
        {
            EntityPath = EhEntityPath
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
    }
    ```

4. Přidat novou metodu s názvem `SendMessagesToEventHub` k `Program` třídy následujícím způsobem:

    ```csharp
    // Creates an event hub client and sends 100 messages to the event hub.
    private static async Task SendMessagesToEventHub(int numMessagesToSend)
    {
        for (var i = 0; i < numMessagesToSend; i++)
        {
            try
            {
                var message = $"Message {i}";
                Console.WriteLine($"Sending message: {message}");
                await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
            }

            await Task.Delay(10);
        }

        Console.WriteLine($"{numMessagesToSend} messages sent.");
    }
    ```

5. Přidejte následující kód, který `Main` metoda v `Program` třídy.

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   Soubor Program.cs by měl vypadat takhle.

    ```csharp
    namespace SampleSender
    {
        using System;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.Azure.EventHubs;

        public class Program
        {
            private static EventHubClient eventHubClient;
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
                // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
                // we are using the connection string from the namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
                {
                    EntityPath = EhEntityPath
                };

                eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

                await SendMessagesToEventHub(100);

                await eventHubClient.CloseAsync();

                Console.WriteLine("Press ENTER to exit.");
                Console.ReadLine();
            }

            // Creates an event hub client and sends 100 messages to the event hub.
            private static async Task SendMessagesToEventHub(int numMessagesToSend)
            {
                for (var i = 0; i < numMessagesToSend; i++)
                {
                    try
                    {
                        var message = $"Message {i}";
                        Console.WriteLine($"Sending message: {message}");
                        await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
                    }
                    catch (Exception exception)
                    {
                        Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
                    }

                    await Task.Delay(10);
                }

                Console.WriteLine($"{numMessagesToSend} messages sent.");
            }
        }
    }
    ```

6. Spusťte program a zkontrolujte, že nejsou žádné chyby.

Blahopřejeme! Nyní jste odeslali zprávy do centra událostí.

## <a name="next-steps"></a>Další kroky
Další informace o službě Event Hubs najdete na následujících odkazech:

* [Přijímat události ze služby Event Hubs](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
* [Vytvoření centra událostí](event-hubs-create.md)
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcore.png
