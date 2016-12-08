---
title: "Přehled cenových úrovní zasílání zpráv Service Bus Premium a Standard | Dokumentace Microsoftu"
description: "Zasílání zpráv Service Bus na úrovni Premium a Standard"
services: service-bus-messaging
documentationcenter: .net
author: djrosanova
manager: timlt
editor: 
ms.assetid: e211774d-821c-4d79-8563-57472d746c58
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/02/2016
ms.author: darosa,sethm
translationtype: Human Translation
ms.sourcegitcommit: 9ace119de3676bcda45d524961ebea27ab093415
ms.openlocfilehash: f75dcb6fd74ca2db7944ac76363801f5ee96650d


---
# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Úrovně zasílání zpráv Service Bus Premium a Standard
Zasílání zpráv Service Bus, které zahrnuje entity zasílání zpráv, jako jsou fronty a témata, v sobě kombinuje funkce pro zasílání zpráv v rámci podniku s bohatou sémantikou publikování a odběru na úrovni cloudu. Zasílání zpráv Service Bus se používá jako páteřní prvek mnoha sofistikovaných cloudových řešení.

Úroveň *Premium* zasílání zpráv Service Bus řeší běžné požadavky zákazníků z pohledu rozsahu, výkonu a dostupnosti pro klíčové aplikace. Přestože mají tyto dvě úrovně skoro stejné sady funkcí, jsou určené pro použití v odlišných situacích.

Několik nejvýraznějších rozdílů je v tabulce dole.

| Premium | Standard |
| --- | --- |
| Vysoká propustnost |Variabilní propustnost |
| Předvídatelný výkon |Variabilní latence |
| Předvídatelná cena |Variabilní průběžná cena  |
| Možnost navýšit a snížit zátěž |Není k dispozici |
| Velikost zprávy > 256 KB |Velikost zprávy je 256 KB |

**Zasílání zpráv Azure Service Bus Premium** zajišťuje izolaci prostředků ve vrstvě CPU a paměti, takže každá úloha zákazníka běží izolovaně. Kontejner prostředků se nazývá *jednotka zasílání zpráv*. Každému prémiovému obor názvů se přiřadí aspoň jedna jednotka zasílání zpráv. Pro každý obor názvů Service Bus Premium můžete koupit 1, 2 nebo 4 jednotky zasílání zpráv. Jedna úloha nebo entita může zabírat několik jednotek zasílání zpráv a počet jednotek zasílání zpráv se dá změnit podle libosti, ale fakturuje se podle 24hodinoví/denní sazby. Výsledkem je předvídatelný a opakovatelný výkon vašeho řešení postaveného na Service Bus.

Vedle toho, že je tento výkon předvídatelnější, je také rychlejší. Zasílání zpráv Service Bus Premium staví na enginu úložiště představeném ve službě [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Se zasíláním zpráv na úrovni Premium je výkon ve špičce mnohem vyšší než na úrovni Standard.

## <a name="premium-messaging-technical-differences"></a>Technické rozdíly zasílání zpráv na úrovni Premium
Toto je několik rozdílů mezi úrovněmi zasílání zpráv Premium a Standard.

### <a name="partitioned-queues-and-topics"></a>Dělené fronty a témata
Dělené fronty a témata jsou podporované v zasílání zpráv na úrovni Premium, ale nefungují na stejném principu jako při zasílání zpráv Service Bus na úrovních Standard a Basic. Zasílání zpráv na úrovni Premium nepoužívá úložiště dat SQL a není tu tak možnost soupeření o prostředky, které je obvyklé na sdílené platformě. To znamená, že dělení není potřeba. Počet oddílů na úrovni Premium se navíc snížil z 16, které jsou na úrovni Standard, na 2. Dva oddíly zajišťují dostupnost, navíc je to vhodnější počet pro prostředí runtime úrovně Premium. Další informace o dělení najdete v oddílu [Dělené fronty a témata](service-bus-partitioning.md).

### <a name="express-entities"></a>Expresní entity
Protože zasílání zpráv na úrovni Premium běží v kompletně izolovaném prostředí, nejsou expresní entity v oborech názvů úrovně Premium podporované. Další informace o expresní funkci najdete v popisu vlastnosti [Microsoft.ServiceBus.Messaging.QueueDescription.EnableExpress](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enableexpress.aspx).

## <a name="next-steps"></a>Další kroky
Pokud se o přenosu zpráv přes Service Bus chcete dozvědět víc, pročtěte si následující témata.

* [Představení zasílání zpráv Azure Service Bus Premium (příspěvek na blogu)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Představení zasílání zpráv Azure Service Bus Premium (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Přehled přenosu zpráv ve službě Service Bus](service-bus-messaging-overview.md)
* [Jak používat fronty Service Bus](service-bus-dotnet-get-started-with-queues.md)




<!--HONumber=Nov16_HO3-->


