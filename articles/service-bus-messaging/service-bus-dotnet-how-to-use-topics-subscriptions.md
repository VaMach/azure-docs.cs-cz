---
title: "Začínáme s tématy a odběry služby Azure Service Bus | Dokumentace Microsoftu"
description: "Napíšeme aplikaci konzoly v jazyce C# využívající témata a odběry zasílání zpráv služby Service Bus."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 10/10/2017
ms.author: sethm
ms.openlocfilehash: 3646d14be662af0fdf80790cb53ddc581b33a146
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-service-bus-topics"></a>Začínáme s tématy služby Service Bus

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tento kurz se zabývá následujícími kroky:

1. Pomocí webu Azure Portal vytvoříme obor názvů služby Service Bus.
2. Pomocí webu Azure Portal vytvoříme téma služby Service Bus.
3. Pomocí webu Azure Portal vytvoříme k tomuto tématu odběr služby Service Bus.
4. Napíšeme aplikaci konzoly pro odeslání zprávy do tohoto tématu.
5. Napíšeme aplikaci konzoly pro příjem této zprávy z odběru.

## <a name="prerequisites"></a>Požadavky

1. [Visual Studio 2015 nebo vyšší](http://www.visualstudio.com). V příkladech v tomto kurzu se používá sada Visual Studio 2017.
2. Předplatné Azure.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Vytvoření oboru názvů služby Service Bus pomocí webu Azure Portal

Pokud už máte vytvořený obor názvů pro zasílání zpráv služby Service Bus, přejděte k části [Vytvoření tématu pomocí webu Azure Portal](#2-create-a-topic-using-the-azure-portal).

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-topic-using-the-azure-portal"></a>2. Vytvoření tématu pomocí webu Azure Portal

1. Přihlaste se k webu [Azure Portal][azure-portal].
2. V levém navigačním podokně portálu klikněte na **Service Bus** (pokud položku **Service Bus** nevidíte, klikněte na **Další služby**).
3. Klikněte na obor názvů, ve kterém chcete vytvořit téma. Zobrazí se okno přehledu oboru názvů:
   
    ![Vytvoření tématu][createtopic1]
4. V okně **Obor názvů služby Service Bus** klikněte na **Témata** a pak na **Přidat téma**.
   
    ![Výběr témat][createtopic2]
5. Zadejte název tématu a zrušte zaškrtnutí možnosti **Povolit dělení**. U ostatních možností ponechte jejich výchozí hodnoty.
   
    ![Vyberte Nový][createtopic3]
6. Dole na v okně klikněte na **Vytvořit**.

## <a name="3-create-a-subscription-to-the-topic"></a>3. Vytvoření odběru tématu

1. V podokně prostředků na portálu klikněte na obor názvů, který jste vytvořili v kroku 1, a pak klikněte na téma, které jste vytvořili v kroku 2.
2. V horní části podokna přehledu kliknutím na symbol plus vedle možnosti **Odběr** přidejte odběr tohoto tématu.

    ![Vytvoření odběru][createtopic4]

3. Zadejte název odběru. U ostatních možností ponechte jejich výchozí hodnoty.

## <a name="4-send-messages-to-the-topic"></a>4. Odesílání zpráv do tématu

Pro odesílání zpráv do tématu napíšeme pomocí sady Visual Studio aplikaci konzoly v jazyce C#.

### <a name="create-a-console-application"></a>Vytvoření konzolové aplikace

Spusťte sadu Visual Studio a vytvořte nový projekt **Aplikace konzoly (.NET Framework)**.

### <a name="add-the-service-bus-nuget-package"></a>Přidání balíčku Service Bus NuGet

1. Klikněte pravým tlačítkem na nově vytvořený projekt a vyberte možnost **Spravovat balíčky NuGet**.
2. Klikněte na kartu **Procházet**, vyhledejte **WindowsAzure.ServiceBus** a pak vyberte položku **WindowsAzure.ServiceBus**. Klikněte na **Instalovat** a dokončete instalaci, pak zavřete dialogové okno.
   
    ![Výběr balíčku NuGet][nuget-pkg]

### <a name="write-some-code-to-send-a-message-to-the-topic"></a>Napsání kódu pro odeslání zprávy do tématu

1. Na začátek souboru Program.cs přidejte následující příkaz `using`.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Do metody `Main` přidejte následující kód. Nastavte proměnnou `connectionString` na připojovací řetězec, který jste získali při vytváření oboru názvů, a proměnnou `topicName` nastavte na název, který jste použili při vytváření tématu.
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
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

    namespace tsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var topicName = "<your topic name>";

                var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER to exit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. Spusťte program a podívejte se na web Azure Portal: klikněte na název vašeho tématu v okně **Přehled** oboru názvů. Zobrazí se okno **Základy** tématu. Všimněte si, že v odběrech uvedených v dolní části okna by hodnota **Počet zpráv** měla být u každého odběru 1. Pokaždé, když spustíte aplikaci odesílatele bez načtení zpráv (jak je popsáno v další části), se tato hodnota zvýší o 1. Všimněte si také, že aktuální velikost tématu navyšuje hodnotu **Aktuální** v okně **Základy** pokaždé, když aplikace do daného tématu nebo odběru přidá zprávu.
   
      ![Velikost zpráv][topic-message]

## <a name="5-receive-messages-from-the-subscription"></a>5. Příjem zpráv z odběru

1. Pokud chcete přijímat zprávy, které jste právě odeslali, vytvořte novou aplikaci konzoly a přidejte odkaz na balíček NuGet služby Service Bus, podobně jako předtím u aplikace odesílatele.
2. Na začátek souboru Program.cs přidejte následující příkaz `using`.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Do metody `Main` přidejte následující kód. Nastavte proměnnou `connectionString` na připojovací řetězec, který jste získali při vytváření oboru názvů, a proměnnou `topicName` nastavte na název, který jste použili při vytváření tématu. Nezapomeňte také nahradit položku `<your subscription name>` názvem odběru, který jste vytvořili v kroku 3. 
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
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
   
    namespace GettingStartedWithTopics
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var topicName = "<your topic name>";
   
          var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
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
4. Spusťte program a znovu se podívejte na portál. Všimněte si, že hodnoty **Počet zpráv** a **Aktuální** jsou nyní 0.
   
    ![Délka tématu][topic-message-receive]

Blahopřejeme! Právě jste vytvořili téma a odběr, odeslali zprávu a přijali ji.

## <a name="next-steps"></a>Další kroky

Podívejte se na naše [úložiště GitHub s ukázkami](https://github.com/Azure/azure-service-bus/tree/master/samples), které předvádějí některé pokročilejší funkce zasílání zpráv služby Service Bus.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/nuget-package.png
[topic-message]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message.png
[topic-message-receive]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message-receive.png
[createtopic1]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic1.png
[createtopic2]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic2.png
[createtopic3]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic3.png
[createtopic4]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic4.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
[azure-portal]: https://portal.azure.com
