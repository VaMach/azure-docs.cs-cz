---
title: "Začínáme s frontami služby Azure Service Bus | Dokumentace Microsoftu"
description: "Napíšeme konzolovou aplikaci C# využívající fronty zasílání zpráv služby Service Bus."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68a34c00-5600-43f6-bbcc-fea599d500da
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 10/16/2017
ms.author: sethm
ms.openlocfilehash: c381489c3cb5b42505d0cdf01618edb370793304
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/17/2017
---
# <a name="get-started-with-service-bus-queues"></a>Začínáme s frontami služby Service Bus
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## <a name="what-will-be-accomplished"></a>Co všechno zvládneme
Tento kurz se zabývá následujícími kroky:

1. Pomocí webu Azure Portal vytvoříme obor názvů služby Service Bus.
2. Pomocí webu Azure Portal vytvoříme frontu služby Service Bus.
3. Napíšeme konzolovou aplikaci pro odeslání zprávy.
4. Napíšeme konzolovou aplikaci pro příjem zpráv odeslaných v předchozím kroku.

## <a name="prerequisites"></a>Požadavky
1. [Visual Studio 2015 nebo vyšší](http://www.visualstudio.com). V příkladech v tomto kurzu se používá sada Visual Studio 2017.
2. Předplatné Azure.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Vytvoření oboru názvů služby Service Bus pomocí webu Azure Portal
Pokud už máte vytvořený obor názvů pro zasílání zpráv služby Service Bus, přejděte k části [Vytvoření fronty pomocí webu Azure Portal](#2-create-a-queue-using-the-azure-portal).

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-the-azure-portal"></a>2. Vytvoření fronty pomocí webu Azure Portal
Pokud už máte frontu služby Service Bus vytvořenou, přejděte k části [Zasílání zpráv do fronty](#3-send-messages-to-the-queue).

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-to-the-queue"></a>3. Zasílání zpráv do fronty
Abychom mohli do fronty odesílat zprávy, napíšeme v sadě Visual Studio konzolovou aplikaci v jazyce C#.

### <a name="create-a-console-application"></a>Vytvoření konzolové aplikace

Spusťte sadu Visual Studio a vytvořte nový projekt **Aplikace konzoly (.NET Framework)**.

### <a name="add-the-service-bus-nuget-package"></a>Přidání balíčku Service Bus NuGet
1. Klikněte pravým tlačítkem na nově vytvořený projekt a vyberte možnost **Spravovat balíčky NuGet**.
2. Klikněte na kartu **Procházet**, vyhledejte **WindowsAzure.ServiceBus** a pak vyberte položku **WindowsAzure.ServiceBus**. Klikněte na **Instalovat** a dokončete instalaci, pak zavřete dialogové okno.
   
    ![Výběr balíčku NuGet][nuget-pkg]

### <a name="write-some-code-to-send-a-message-to-the-queue"></a>Napsání kódu pro zaslání zprávy do fronty
1. Na začátek souboru Program.cs přidejte následující příkaz `using`.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Do metody `Main` přidejte následující kód. Nastavte proměnnou `connectionString` na připojovací řetězec, který jste získali při vytváření oboru názvů, a proměnnou `queueName` nastavte na název, který jste použili při vytváření fronty.
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    Soubor Program.cs by měl vypadat takhle.
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;

    namespace qsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var queueName = "<your queue name>";

                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER to exit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. Spusťte program a podívejte se na web Azure Portal: klikněte na název vaší fronty v okně **Přehled** oboru názvů. Zobrazí se okno **Základy** fronty. Všimněte si, že hodnota **Počet aktivních zpráv** by měla nyní být 1. Pokaždé, když spustíte aplikaci odesílatele bez načtení zpráv, se tato hodnota zvýší o 1. Všimněte si také, že se pokaždé, když aplikace přidá zprávu do fronty, zvětší aktuální velikost fronty.
   
      ![Velikost zpráv][queue-message]

## <a name="4-receive-messages-from-the-queue"></a>4. Přijetí zpráv z fronty

1. Pokud chcete přijímat zprávy, které jste právě odeslali, vytvořte novou konzolovou aplikaci a přidejte odkaz na balíček NuGet služby Service Bus, podobně jako předtím u aplikace odesílatele.
2. Na začátek souboru Program.cs přidejte následující příkaz `using`.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Do metody `Main` přidejte následující kód. Nastavte proměnnou `connectionString` na připojovací řetězec, který jste získali při vytváření oboru názvů, a proměnnou `queueName` nastavte na název, který jste použili při vytváření fronty.
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    Soubor Program.cs by měl vypadat takhle:
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithQueues
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var queueName = "<your queue name>";
   
          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });

          Console.WriteLine("Press ENTER to exit program");   
          Console.ReadLine();
        }
      }
    }
    ```
4. Spusťte program a znovu se podívejte na portál. Všimněte si, že hodnoty **Počet aktivních zpráv** a **Aktuální** jsou nyní 0.
   
    ![Délka fronty][queue-message-receive]

Blahopřejeme! Vytvořili jste frontu, do této fronty jste odeslali zprávu a ze stejné fronty jste přijali zprávu.

## <a name="next-steps"></a>Další kroky

Podívejte se na naše [úložiště GitHub s ukázkami](https://github.com/Azure/azure-service-bus/tree/master/samples), které předvádějí některé pokročilejší funkce zasílání zpráv služby Service Bus.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png

