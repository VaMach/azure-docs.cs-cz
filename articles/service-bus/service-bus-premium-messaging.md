<properties
    pageTitle="Přehled cenových úrovní zasílání zpráv Service Bus Premium a Standard | Microsoft Azure"
    description="Zasílání zpráv Service Bus na úrovni Premium a Standard"
    services="service-bus"
    documentationCenter=".net"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="03/16/2016"
    ms.author="darosa;sethm"/>

# Úrovně zasílání zpráv Service Bus Premium a Standard 

Zprostředkované zasílání zpráv Service Bus, které zahrnuje entity zasílání zpráv, jako jsou fronty a témata, v sobě kombinuje funkce pro zasílání zpráv v rámci podniku s bohatou sémantikou publikování a odběru na úrovni cloudu. Zprostředkované zasílání zpráv se používá jako páteřní prvek mnoha sofistikovaných cloudových řešení.

Úroveň *Premium* zasílání zpráv Service Bus řeší běžné požadavky zákazníků z pohledu rozsahu, výkonu a dostupnosti pro klíčové aplikace. Přestože mají tyto dvě úrovně skoro stejné sady funkcí, jsou určené pro použití v odlišných situacích.

Několik nejvýraznějších rozdílů je v tabulce dole.

| Premium                               | Standard                       |
|---------------------------------------|--------------------------------|
| Vysoká propustnost                       | Variabilní propustnost            |
| Předvídatelný výkon               | Variabilní latence               |
| Předvídatelná cena                   | Variabilní průběžná cena  |
| Možnost navýšit a snížit zátěž | –                            |
| Velikost zprávy > 256 KB                  | Velikost zprávy je 256 KB          |

**Zasílání zpráv Azure Service Bus Premium** nabízí izolaci prostředků v rovině CPU a paměti, takže každá úloha zákazníka běží izolovaně. Kontejner prostředků se nazývá *jednotka zasílání zpráv*. Každému prémiovému obor názvů se přiřadí aspoň jedna jednotka zasílání zpráv. Pro každý obor názvů Service Bus Premium můžete koupit 1, 2 nebo 4 jednotky zasílání zpráv. Jedna úloha nebo entita může zabírat několik jednotek zasílání zpráv a počet jednotek zasílání zpráv se dá změnit podle libosti, ale fakturuje se podle 24hodinoví/denní sazby. Výsledkem je předvídatelný a opakovatelný výkon vašeho řešení postaveného na Service Bus.

Vedle toho, že je tento výkon předvídatelnější, je také rychlejší. Zasílání zpráv Service Bus Premium staví na enginu úložiště představeném ve službě [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Se zasíláním zpráv na úrovni Premium je výkon ve špičce mnohem vyšší než na úrovni Standard.

## Technické rozdíly zasílání zpráv na úrovni Premium

Toto je několik rozdílů mezi úrovněmi zasílání zpráv Premium a Standard.

### Dělené entity

Dělené entity se podporují v zasílání zpráv na úrovni Premium, ale nefungují na stejném principu jako při zasílání zpráv Service Bus na úrovních Standard a Basic. Zasílání zpráv na úrovni Premium nepoužívá úložiště dat SQL a není tu tak možnost soupeření o prostředky, které je obvyklé na sdílené platformě. To znamená, že dělení není potřeba. Počet oddílů na úrovni Premium se navíc snížil z 16, které jsou na úrovni Standard, na dva. 2 oddíly zajišťují dostupnost, navíc je to vhodnější počet pro prostředí Permium. Další informace o oddílech najdete v tématu [Segmentované entity zasílání zpráv](service-bus-partitioning.md).

### Expresní entity

Protože zasílání zpráv na úrovni Premium běží v kompletně izolovaném prostředí, nejsou expresní entity potřeba. Obory názvů na úrovni Premium proto nepodporují expresní entity. Další informace o expresní funkci najdete v popisu vlastnosti [Microsoft.ServiceBus.Messaging.QueueDescription.EnableExpress](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enableexpress.aspx).

## Další kroky

Pokud se o přenosu zpráv přes Service Bus chcete dozvědět víc, pročtěte si následující témata.

- [Představení zasílání zpráv Azure Service Bus Premium (příspěvek na blogu)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
- [Představení zasílání zpráv Azure Service Bus Premium (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
- [Přehled přenosu zpráv ve službě Service Bus](service-bus-messaging-overview.md)
- [Přehled architektury služby Azure Service Bus](service-bus-fundamentals-hybrid-solutions.md)
- [Jak používat fronty Service Bus](service-bus-dotnet-how-to-use-queues.md)



<!--HONumber=Jun16_HO2-->


