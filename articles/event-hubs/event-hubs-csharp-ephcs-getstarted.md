<properties
    pageTitle="Začínáme se službou Event Hubs v jazyce C# | Microsoft Azure"
    description="Tento kurz vám pomůže naučit se základy používání služby Azure Event Hubs s jazykem C# a knihovny EventProcessorHost."
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
    ms.date="05/13/2016"
    ms.author="sethm"/>

# Začínáme se službou Event Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## Úvod

Event Hubs je služba, která zpracovává velké objemy dat událostí (telemetrie) z připojených zařízení a aplikací. Data, která shromáždíte pomocí služby Event Hubs, můžete uložit pomocí úložného clusteru nebo transformovat pomocí zprostředkovatele datové analýzy v reálném čase. Schopnost shromažďovat a zpracovávat velké množství událostí je klíčovou komponentou moderních aplikačních architektur, například internetu věcí (Internet of Things – IoT).

Díky tomuto kurzu se dozvíte, jak pomocí portálu Azure Classic vytvořit centrum událostí (Event Hub). Taky se naučíte, jak v centru událostí shromažďovat zprávy pomocí konzolové aplikace napsané v jazyce C# a jak je paralelně znovu přijímat pomocí knihovny [Event Processor Host][] jazyka C#.

Abyste tento kurz dokončili, potřebujete následující:

+ Microsoft Visual Studio 2013 (nebo novější verzi) nebo Microsoft Visual Studio Express pro Windows. V příkladech v tomto článku pracujeme s aplikací Visual Studio 2015.

+ Aktivní účet Azure. <br/>Pokud účet nemáte, můžete si ho bezplatně vytvořit během několika minut. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank").

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## Spuštění aplikací

Nyní můžete spustit aplikace.

1. Pomocí Visual Studia otevřete dříve vytvořený projekt **Receiver**.

2. Klikněte pravým tlačítkem myši na řešení **Receiver** (Příjemce), pak klikněte na **Přidat** a nakonec na **Existující projekt**.
 
3. Vyhledejte existující soubor Sender.csproj a dvakrát na něj klikněte, aby se přidal k řešení.
 
4. Pravým tlačítkem myši znovu klikněte na řešení **Receiver** a pak klikněte na **Vlastnosti**. Zobrazí se stránka vlastností řešení **Receiver**. 

5. Klikněte na **Spouštěný projekt** a pak klikněte na tlačítko **Více projektů po spuštění**. V poli **Akce** u projektů **Receiver** i **Sender** (Odesílatel) nastavte **Start**.

    ![][19]

6. Klikněte na **Závislosti projektu**. V poli **Projekty** klikněte na **Sender**. Ujistěte se, že je v poli **Závisí na** zaškrtnuto **Receiver** (a případně zaškrtněte).

    ![][20]

7. Klikněte na **OK**, a dialog **Vlastnosti** se zavře.

1.  Stiskněte klávesu F5, aby se ve Visual Studiu spustil projekt **Receiver**. Pak počkejte, dokud se nenačtou příjemci pro všechny oddíly.

    ![][21]

2.  Projekt **Sender** se spustí automaticky. V okně konzoly stiskněte klávesu **Enter**. V okně Receiver se zobrazí události.

    ![][22]

Stiskněte v okně **Sender** kombinaci kláves **Ctrl + C**. Aplikace Sender se ukončí. Pak v okně Receiver stiskněte klávesu **Enter**, a ukončí se i tato aplikace.

## Další kroky

Gratulujeme, sestavili jste funkční aplikaci, která vytvoří centrum událostí a odesílá i přijímá data. Nyní se můžete podívat na některý z následujících scénářů:

- úplná [ukázková aplikace, která používá službu Event Hubs][]
- [horizontální navýšení kapacity zpracování událostí ve službě Event Hubs][] – ukázka
- [řešení zasílání zpráv ve frontě][] pomocí front Service Bus
- [Přehled služby Event Hubs][]

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Portál Azure Classic]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Přehled služby Event Hubs]: event-hubs-overview.md
[ukázková aplikace, která používá službu Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[horizontální navýšení kapacity zpracování událostí ve službě Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[řešení zasílání zpráv ve frontě]: ../service-bus/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 



<!--HONumber=Jun16_HO2-->


