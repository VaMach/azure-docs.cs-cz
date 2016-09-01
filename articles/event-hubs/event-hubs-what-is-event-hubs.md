<properties
    pageTitle="Co je služba Azure Event Hubs? | Microsoft Azure"
    description="Přehled a popis služby Azure Event Hubs"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="sethm"/>

# Co je služba Azure Event Hubs?

Vysoce škálovatelná služba Azure Event Hubs slouží ke zpracování příchozích dat. Dokáže přijímat miliony událostí za sekundu a umožňuje zpracovávat a analyzovat masivní objemy dat vytvářených zařízeními a aplikacemi připojenými k vaší síti. Služba Event Hubs slouží jako „vstupní brána“ pro kanál události. Jakmile jsou data shromážděna do centra událostí, lze je transformovat a ukládat pomocí libovolného zprostředkovatele datové analýzy v reálném čase nebo adaptérů pro dávkování či ukládání. Event Hubs oddělí vytvoření proudu událostí od spotřeby těchto události, aby spotřebitelé událostí mohli k událostem přistupovat podle svého vlastního plánu. Další informace a technické podrobnosti najdete v tématu [Přehled služby Event Hubs](event-hubs-overview.md).

## Možnosti služby Event Hubs

Služba Event Hubs zpracovává události a telemetrii v masivním měřítku, s nízkou latencí a vysokou spolehlivostí. Tato služba je užitečné zejména pro:

- instrumentaci aplikací
- zpracování činnosti nebo pracovního postupu koncového uživatele
- scénáře související s internetem věcí (IoT)

Mezi další důležité možnosti služby Event Hubs patří sledování chování v mobilních aplikacích, informace o datových přenosech z webových farem, zachycení událostí v hrách na konzolách nebo shromažďování telemetrických údajů z průmyslových strojů či připojených vozidel.

Na rozdíl od [front a témat sběrnice Service Bus](../service-bus/service-bus-messaging-overview.md) se služba Event Hubs zaměřuje na zpracování datového proudu zpráv ve velkém měřítku. Možnosti služby Event Hubs se například liší od témat sběrnice Service Bus tím, že Event Hubs výrazně upřednostňuje scénáře vysoké propustnosti a zpracování událostí. V důsledku toho služba Event Hubs neimplementuje některé možnosti zpracování zpráv, které jsou k dispozici pro [témata](../service-bus/service-bus-fundamentals-hybrid-solutions.md#topics). Pokud tyto funkce potřebujete, témata zůstávají optimální volbou.

## Další kroky

Podrobné informace o službě Event Hubs nejdete v následujících tématech.

- [Přehled služby Event Hubs](event-hubs-overview.md)
- [Průvodce programováním pro službu Event Hubs](event-hubs-programming-guide.md)
- [Dostupnost služby Event Hubs a nejčastější dotazy na podporu](event-hubs-availability-and-support-faq.md)
- Úvodní [Kurz služby Event Hubs][]
- Úplná [ukázková aplikace, která používá službu Event Hubs][]

[Kurz služby Event Hubs]: event-hubs-csharp-ephcs-getstarted.md
[ukázková aplikace, která používá službu Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097



<!--HONumber=Aug16_HO4-->


