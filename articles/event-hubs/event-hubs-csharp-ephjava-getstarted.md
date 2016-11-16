---
title: "Začínáme se službou Event Hubs v jazyce C# | Dokumentace Microsoftu"
description: "Tento kurz vám pomůže začít používat službu Azure Event Hubs a posílat události v jazyce C# a přijímat je v jazyce Java za použití třídy EventProcessorHost."
services: event-hubs
documentationcenter: 
author: jtaubensee
manager: timlt
editor: 
ms.assetid: 059fb733-a397-400e-8e43-0c7ea5930b8b
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/27/2016
ms.author: jotaub;sethm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 51d880650ab8059f3346b5c1272c232b49be33e9


---
# <a name="get-started-with-event-hubs"></a>Začínáme se službou Event Hubs
[!INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Úvod
Event Hubs je služba, která zpracovává velké objemy dat událostí (telemetrie) z připojených zařízení a aplikací. Data, která shromáždíte pomocí služby Event Hubs, můžete uložit pomocí úložného clusteru nebo transformovat pomocí zprostředkovatele datové analýzy v reálném čase. Schopnost shromažďovat a zpracovávat velké množství událostí je klíčovou komponentou moderních aplikačních architektur, například internetu věcí (Internet of Things – IoT).

Díky tomuto kurzu se dozvíte, jak pomocí portálu Azure Classic vytvořit centrum událostí (Event Hub). Také se naučíte, jak v centru událostí shromažďovat zprávy pomocí konzolové aplikace napsané v jazyce C# a jak je paralelně znovu přijímat pomocí knihovny Event Processor Host jazyka Java.

K absolvování tohoto kurzu potřebujete:

* [Microsoft Visual Studio](http://visualstudio.com)
* Aktivní účet Azure. <br/>Pokud účet nemáte, můžete si ho bezplatně vytvořit během několika minut. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank").

[!INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[!INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[!INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Spuštění aplikací
Nyní můžete spustit aplikace.

1. Spusťte projekt **Receiver** (Příjemce).
   
   ![][21]
2. Spusťte projekt **Sender** (Odesílatel).
   
   ![][22]

## <a name="next-steps"></a>Další kroky
Gratulujeme, sestavili jste funkční aplikaci, která vytvoří centrum událostí a odesílá i přijímá data. Nyní se můžete podívat na některý z následujících scénářů:

* Úplná [ukázková aplikace, která používá službu Event Hubs][ukázková aplikace, která používá službu Event Hubs]
* Ukázka [horizontálního navýšení kapacity zpracování událostí pomocí služby Event Hubs][horizontální navýšení kapacity zpracování událostí pomocí služby Event Hubs]
* [Přehled služby Event Hubs][Přehled služby Event Hubs]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[Portál Azure Classic]: https://manage.windowsazure.com/
[Přehled služby Event Hubs]: event-hubs-overview.md
[ukázková aplikace, která používá službu Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[horizontální navýšení kapacity zpracování událostí pomocí služby Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3



<!--HONumber=Nov16_HO2-->


