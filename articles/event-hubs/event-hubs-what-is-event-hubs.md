---
title: "Co je služba Azure Event Hubs? | Dokumentace Microsoftu"
description: "Přehled a popis služby Azure Event Hubs"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 4391d750-5bbe-456d-9091-b416127bc754
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: fcc3e2cb76a06607d0bfbbad810968b70d67e4f4


---
# <a name="what-is-azure-event-hubs"></a>Co je služba Azure Event Hubs?
Vysoce škálovatelná služba Azure Event Hubs slouží ke zpracování příchozích dat. Dokáže přijímat miliony událostí za sekundu a umožňuje zpracovávat a analyzovat masivní objemy dat vytvářených zařízeními a aplikacemi připojenými k vaší síti. Služba Event Hubs slouží jako „vstupní brána“ pro kanál události. Jakmile jsou data shromážděna do centra událostí, lze je transformovat a ukládat pomocí libovolného zprostředkovatele datové analýzy v reálném čase nebo adaptérů pro dávkování či ukládání. Event Hubs oddělí vytvoření proudu událostí od spotřeby těchto události, aby spotřebitelé událostí mohli k událostem přistupovat podle svého vlastního plánu. Další informace a technické podrobnosti najdete v tématu [Přehled služby Event Hubs](event-hubs-overview.md).

## <a name="event-hubs-capabilities"></a>Možnosti služby Event Hubs
Služba Event Hubs zpracovává události a telemetrii v masivním měřítku, s nízkou latencí a vysokou spolehlivostí. Tato služba je užitečné zejména pro:

* instrumentaci aplikací
* zpracování činnosti nebo pracovního postupu koncového uživatele
* scénáře související s internetem věcí (IoT)

Mezi další důležité možnosti služby Event Hubs patří sledování chování v mobilních aplikacích, informace o datových přenosech z webových farem, zachycení událostí v hrách na konzolách nebo shromažďování telemetrických údajů z průmyslových strojů či připojených vozidel.

## <a name="next-steps"></a>Další kroky
Podrobné informace o službě Event Hubs nejdete v následujících tématech.

* [Přehled služby Event Hubs](event-hubs-overview.md)
* [Průvodce programováním pro službu Event Hubs](event-hubs-programming-guide.md)
* [Dostupnost služby Event Hubs a nejčastější dotazy na podporu](event-hubs-availability-and-support-faq.md)
* Úvodní [Kurz služby Event Hubs][Kurz služby Event Hubs]
* Úplná [ukázková aplikace, která používá službu Event Hubs][ukázková aplikace, která používá službu Event Hubs]

[Kurz služby Event Hubs]: event-hubs-csharp-ephcs-getstarted.md
[ukázková aplikace, která používá službu Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097



<!--HONumber=Nov16_HO2-->


