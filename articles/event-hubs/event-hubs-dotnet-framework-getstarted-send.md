---
title: "Odeslání událostí do Azure Event Hubs pomocí rozhraní .NET Framework | Dokumentace Microsoftu"
description: "Začínáme s odesíláním událostí do služby Event Hubs pomocí rozhraní .NET Framework"
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
ms.sourcegitcommit: c52f7055897ba8e851add431e5ab9c0defdb5bfc
ms.openlocfilehash: 29523e308e038904773582c73c1688f57e3c31e3


---
# <a name="send-events-to-azure-event-hubs-using-the-net-framework"></a>Odeslání událostí do Azure Event Hubs pomocí rozhraní .NET Framework

## <a name="introduction"></a>Úvod
Event Hubs je služba, která zpracovává velké objemy dat událostí (telemetrie) z připojených zařízení a aplikací. Data, která shromáždíte pomocí služby Event Hubs, můžete uložit pomocí úložného clusteru nebo transformovat pomocí zprostředkovatele datové analýzy v reálném čase. Schopnost shromažďovat a zpracovávat velké množství událostí je klíčovou komponentou moderních aplikačních architektur, například internetu věcí (Internet of Things – IoT).

Díky tomuto kurzu se dozvíte, jak pomocí webu [Azure Portal](https://portal.azure.com) vytvořit centrum událostí. Také ukazuje, jak odesílat události do centra událostí pomocí konzolové aplikace napsané v jazyce C# s použitím rozhraní .NET Framework. Pokud chcete přijímat události pomocí rozhraní .NET Framework, přečtěte si článek [Příjem událostí pomocí rozhraní .NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md) nebo klikněte na příslušný přijímající jazyk v obsahu vlevo.

K absolvování tohoto kurzu potřebujete:

* [Microsoft Visual Studio](http://visualstudio.com)
* Aktivní účet Azure. Pokud účet nemáte, můžete si ho bezplatně vytvořit během několika minut. Podrobnosti najdete v tématu [Bezplatná zkušební verze Azure](https://azure.microsoft.com/free/).

## <a name="send-messages-to-event-hubs"></a>Zasílání zpráv do služby Event Hubs
V této části napíšete konzolovou aplikaci pro Windows, která zasílá události do vašeho centra událostí.

1. Pomocí šablony projektu **Konzolová aplikace** vytvořte v sadě Visual Studio nový projekt desktopové aplikace Visual C#. Projekt pojmenujte **Odesílatel**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp1.png)
2. V Průzkumníku řešení klikněte pravým tlačítkem na řešení a potom na **Spravovat balíčky NuGet pro řešení**. 
3. Klikněte na kartu **Procházet** a potom najděte `Microsoft Azure Service Bus`. Zkontrolujte, jestli je v okně **Verze** uvedený název projektu (**Odesílatel**). Klikněte na **Instalovat** a přijměte podmínky použití. 
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp2.png)
   
    Visual Studio stáhne a nainstaluje [balíček NuGet knihovny Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus) a přidá se na něj odkaz.
4. Do horní části souboru **Program.cs** přidejte následující příkazy `using`:
   
    ```csharp
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```
5. K třídě **Program** přidejte následující pole a zástupné hodnoty nahraďte názvem centra událostí, které jste vytvořili v předchozí části, a připojovacím řetězcem na úrovni oboru názvů, který jste si předtím uložili.
   
    ```csharp
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```
6. Přidejte následující metodu do třídy **Program**:
   
    ```csharp
    static void SendingRandomMessages()
    {
        var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
        while (true)
        {
            try
            {
                var message = Guid.NewGuid().ToString();
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                Console.ResetColor();
            }
   
            Thread.Sleep(200);
        }
    }
    ```
   
    Tato metoda neustále odesílá události do vašeho centra událostí se zpožděním 200 ms.
7. Nakonec do metody **Main** přidejte následující řádky:
   
    ```csharp
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```

## <a name="next-steps"></a>Další kroky
Gratulujeme, sestavili jste funkční aplikaci, která vytvoří centrum událostí a odesílá data. Nyní se můžete podívat na některý z následujících scénářů:

* [Příjem událostí pomocí třídy EventProcessorHost](event-hubs-dotnet-framework-getstarted-receive-eph.md)
* [Referenční informace ke třídě EventProcessorHost](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)
* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs overview]: event-hubs-overview.md




<!--HONumber=Feb17_HO1-->


