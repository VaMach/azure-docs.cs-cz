<properties
    pageTitle="Začínáme s frontami služby Service Bus | Microsoft Azure"
    description="Postup při psaní konzolové aplikace v jazyce C# pro zasílání zpráv ve službě Service Bus"
    services="service-bus"
    documentationCenter=".net"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="06/07/2016"
    ms.author="jotaub;sethm"/>

# Začínáme s frontami služby Service Bus

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## Co všechno zvládneme

V tomto kurzu provedeme tyto kroky:

1. Pomocí webu Azure Portal vytvoříme obor názvů služby Service Bus.

2. Pomocí webu Azure Portal vytvoříme frontu zasílání zpráv služby Service Bus.

3. Napíšeme konzolovou aplikaci pro odeslání zprávy.

4. Napíšeme konzolovou aplikaci pro příjem zpráv.

## Požadavky

1. [Visual Studio 2013 nebo Visual Studio 2015](http://www.visualstudio.com). V příklady v tomto kurzu se používá Visual Studio 2015.

2. Předplatné Azure.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## 1. Vytvoření oboru názvů služby Service Bus pomocí webu Azure Portal

Pokud už máte obor názvů služby Service Bus vytvořený, přejděte do části [Vytvoření fronty pomocí webu Azure Portal](#2-create-a-queue-using-the-azure-portal).

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## 2. Vytvoření fronty pomocí webu Azure Portal

Pokud už máte frontu služby Service Bus vytvořenou, přejděte do části [Zasílání zpráv do fronty](#3-send-messages-to-the-queue).

[AZURE.INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## 3. Zasílání zpráv do fronty

Abychom mohli do fronty odesílat zprávy, napíšeme ve Visual Studiu konzolovou aplikaci v jazyce C#.

### Vytvoření konzolové aplikace

1. Spusťte Visual Studio a vytvořte novou konzolovou aplikaci.

### Přidání balíčku Service Bus NuGet

1. Klikněte pravým tlačítkem na nově vytvořený projekt a vyberte možnost **Spravovat balíčky NuGet**.

2. Klikněte na kartu **Procházet**, vyhledejte „Microsoft Azure Service Bus“ a potom vyberte položku **Microsoft Azure Service Bus**. Klikněte na **Instalovat** a dokončete instalaci, pak zavřete dialogové okno.

    ![Výběr balíčku NuGet][nuget-pkg]

### Napsání kódu pro zaslání zprávy do fronty

1. Na začátek souboru Program.cs přidejte následující příkaz using.

    ```
    using Microsoft.ServiceBus.Messaging;
    ```
    
2. Do metody `Main` přidejte následující kód, nastavte proměnnou **connectionString** na připojovací řetězec, který jste získali při vytváření oboru názvů, a v proměnné **queueName** nastavte název fronty, který jste použili při vytváření fronty.

    ```
    var connectionString = "<Your connection string>";
    var queueName = "<Your queue name>";
  
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");
    client.Send(message);
    ```

    Soubor Program.cs by měl vypadat takhle.

    ```
    using System;
    using Microsoft.ServiceBus.Messaging;

    namespace GettingStartedWithQueues
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "<Your connection string>";
                var queueName = "<Your queue name>";

                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");

                client.Send(message);
            }
        }
    }
    ```
  
3. Spusťte program a podívejte se na portál Azure Classic. Položka **Délka fronty** by teď měla mít hodnotu 1.
    
      ![Délka fronty][queue-length-send]
    
## 4. Přijetí zpráv z fronty

1. Vytvořte novou konzolovou aplikaci a přidejte odkaz na balíček NuGet služby Service Bus, podobně jako předtím u odesílací aplikace.

2. Na začátek souboru Program.cs přidejte následující příkaz `using`.
  
    ```
    using Microsoft.ServiceBus.Messaging;
    ```
  
3. Do metody `Main` přidejte následující kód, nastavte proměnnou **connectionString** na připojovací řetězec, který jste získali při vytváření oboru názvů, a v proměnné **queueName** nastavte název fronty, který jste použili při vytváření fronty.

    ```
    var connectionString = "";
    var queueName = "samplequeue";
  
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
  
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
  
    Console.ReadLine();
    ```

    Soubor Program.cs by měl vypadat takhle:

    ```
    using System;
    using Microsoft.ServiceBus.Messaging;
  
    namespace GettingStartedWithQueues
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "";
          var queueName = "samplequeue";
  
          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
  
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });
  
          Console.ReadLine();
        }
      }
    }
    ```
  
4. Spusťte program a podívejte se na portál. Položka **Délka fronty** by teď měla mít hodnotu 0.

    ![Délka fronty][queue-length-receive]
  
Blahopřejeme! Vytvořili jste frontu, zaslali jste zprávu a přijali jste zprávu.

## Další kroky

Podívejte se na naše [úložiště GitHub s ukázkami](https://github.com/Azure-Samples/azure-servicebus-messaging-samples), které předvádějí některé pokročilejší funkce zasílání zpráv služby Azure Service Bus.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-length-send]: ./media/service-bus-dotnet-get-started-with-queues/queue-length-send.png
[queue-length-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-length-receive.png


<!--Reference style links - using these makes the source content way more readable than using inline links-->

[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples


<!----HONumber=Aug16_HO4-->


