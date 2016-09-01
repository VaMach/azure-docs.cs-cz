<properties
    pageTitle="Začínáme se službou Event Hubs v jazyce C# | Microsoft Azure"
    description="Tento kurz vám pomůže začít používat službu Azure Event Hubs a posílat události v jazyce C# a přijímat je v jazyce Java za použití třídy EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="06/16/2016"
    ms.author="jtaubensee"/>

# Začínáme se službou Event Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## Úvod

Event Hubs je služba, která zpracovává velké objemy dat událostí (telemetrie) z připojených zařízení a aplikací. Data, která shromáždíte pomocí služby Event Hubs, můžete uložit pomocí úložného clusteru nebo transformovat pomocí zprostředkovatele datové analýzy v reálném čase. Schopnost shromažďovat a zpracovávat velké množství událostí je klíčovou komponentou moderních aplikačních architektur, například internetu věcí (Internet of Things – IoT).

Díky tomuto kurzu se dozvíte, jak pomocí portálu Azure Classic vytvořit centrum událostí (Event Hub). Také se naučíte, jak v centru událostí shromažďovat zprávy pomocí konzolové aplikace napsané v jazyce C# a jak je paralelně znovu přijímat pomocí knihovny Event Processor Host jazyka Java.

Abyste tento kurz dokončili, potřebujete následující:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Aktivní účet Azure. <br/>Pokud účet nemáte, můžete si ho bezplatně vytvořit během několika minut. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank").

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## Spuštění aplikací

Nyní můžete spustit aplikace.

1.  Spusťte projekt **Receiver** (Příjemce).

    ![][21]

2.  Spusťte projekt **Sender** (Odesílatel).

    ![][22]

## Další kroky

Gratulujeme, sestavili jste funkční aplikaci, která vytvoří centrum událostí a odesílá i přijímá data. Nyní se můžete podívat na některý z následujících scénářů:

- úplná [ukázková aplikace, která používá službu Event Hubs][]
- [horizontální navýšení kapacity zpracování událostí ve službě Event Hubs][] – ukázka
- [řešení zasílání zpráv ve frontě][] pomocí front Service Bus
- [Přehled služby Event Hubs][]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[klasický portál Azure]: https://manage.windowsazure.com/
[Přehled služby Event Hubs]: event-hubs-overview.md
[ukázková aplikace, která používá službu Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[horizontální navýšení kapacity zpracování událostí ve službě Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[řešení zasílání zpráv ve frontě]: ../service-bus/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 



<!---HONumber=Aug16_HO4-->


