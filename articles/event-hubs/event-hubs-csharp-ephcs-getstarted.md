---
title: "Začínáme se službou Event Hubs v jazyce C# | Dokumentace Microsoftu"
description: "Tento kurz vám pomůže naučit se základy používání služby Azure Event Hubs s jazykem C# a knihovny EventProcessorHost."
services: event-hubs
documentationcenter: 
author: jtaubensee
manager: timlt
editor: 
ms.assetid: 2ec2378a-34f7-44c3-b976-cc444c98c338
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/02/2016
ms.author: jotaub;sethm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 35a7e4693ef979dfb947714f2fe5ce5599991189


---
# <a name="get-started-with-event-hubs"></a>Začínáme se službou Event Hubs
[!INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Úvod
Event Hubs je služba, která zpracovává velké objemy dat událostí (telemetrie) z připojených zařízení a aplikací. Data, která shromáždíte pomocí služby Event Hubs, můžete uložit pomocí úložného clusteru nebo transformovat pomocí zprostředkovatele datové analýzy v reálném čase. Schopnost shromažďovat a zpracovávat velké množství událostí je klíčovou komponentou moderních aplikačních architektur, například internetu věcí (Internet of Things – IoT).

Díky tomuto kurzu se dozvíte, jak pomocí portálu Azure Classic vytvořit centrum událostí (Event Hub). Taky se naučíte, jak v centru událostí shromažďovat zprávy pomocí konzolové aplikace napsané v jazyce C# a jak je paralelně znovu přijímat pomocí knihovny [Event Processor Host][Event Processor Host] jazyka C#.

K absolvování tohoto kurzu potřebujete:

* [Microsoft Visual Studio](http://visualstudio.com)
* Aktivní účet Azure. Pokud účet nemáte, můžete si ho bezplatně vytvořit během několika minut. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/free/).

[!INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[!INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[!INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Spuštění aplikací
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
8. Stiskněte klávesu F5, aby se ve Visual Studiu spustil projekt **Receiver**. Pak počkejte, dokud se nenačtou příjemci pro všechny oddíly.
   
   ![][21]
9. Projekt **Sender** se spustí automaticky. V okně konzoly stiskněte klávesu **Enter**. V okně Receiver se zobrazí události.
   
   ![][22]

Stiskněte v okně **Sender** kombinaci kláves **Ctrl + C**. Aplikace Sender se ukončí. Pak v okně Receiver stiskněte klávesu **Enter**, a ukončí se i tato aplikace.

## <a name="next-steps"></a>Další kroky
Gratulujeme, sestavili jste funkční aplikaci, která vytvoří centrum událostí a odesílá i přijímá data. Nyní se můžete podívat na některý z následujících scénářů:

* Úplná [ukázková aplikace, která používá službu Event Hubs][ukázková aplikace, která používá službu Event Hubs]
* Ukázka [horizontálního navýšení kapacity zpracování událostí pomocí služby Event Hubs][horizontální navýšení kapacity zpracování událostí pomocí služby Event Hubs]
* [Přehled služby Event Hubs][Přehled služby Event Hubs]

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
[horizontální navýšení kapacity zpracování událostí pomocí služby Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[řešení zasílání zpráv ve frontě]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md




<!--HONumber=Nov16_HO2-->


