---
title: "Přehled architektury zpracování zpráv služby Azure Service Bus | Dokumentace Microsoftu"
description: "Popisuje architekturu zpracování zpráv služby Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: baf94c2d-0e58-4d5d-a588-767f996ccf7f
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: 83456d775c5ff2a2476ba46e9c78a8dc1bb482e8
ms.contentlocale: cs-cz
ms.lasthandoff: 08/28/2017

---
# <a name="service-bus-architecture"></a>Architektura služby Service Bus
Tento článek popisuje architekturu zpracování zpráv služby Azure Service Bus.

## <a name="service-bus-scale-units"></a>Jednotky škálování služby Service Bus
Service Bus se organizuje podle *jednotek škálování*. Jednotka škálování je jednotka nasazení a obsahuje všechny komponenty potřebné k tomu, aby služba běžela. Každá oblast nasadí jednu nebo víc jednotek škálování služby Service Bus.

K jednotce škálování je mapovaný obor názvů služby Service Bus. Jednotka škálování zpracovává všechny typy entit služby Service Bus (fronty, témata, odběry). Jednotka škálování služby Service Bus má tyto součásti:

* **Sada uzlů brány.** Uzly brány ověřují příchozí požadavky. Každý uzel brány má veřejnou IP adresu.
* **Sada zprostředkovatelských uzlů pro přenos zpráv.** Zprostředkovatelské uzly pro přenos zpráv zpracovávají požadavky týkající se entit přenos zpráv.
* **Jedno úložiště brány.** Úložiště brány uchovává data pro každou entitu definovanou v této jednotce škálování. Úložiště brány se implementuje nad databázi SQL Azure.
* **Několik úložišť pro přenos zpráv.** Úložiště pro přenos zpráv uchovává zprávy všech front, témat a odběrů definovaných v této jednotce škálování. Taky obsahuje všechna data odběru. Pokud není zapnutá možnost [dělení entit zasílání zpráv na oddíly](service-bus-partitioning.md), mapuje se fronta nebo téma do jednoho úložiště pro přenos zpráv. Odběry jsou uložené ve stejném úložišti pro přenos zpráv jako jejich nadřazené téma. Kromě [Zasílání zpráv na úrovni Premium](service-bus-premium-messaging.md) služby Service Bus se úložiště pro zasílání zpráv implementují nad databáze Azure SQL.

## <a name="containers"></a>Kontejnery
Každé entitě přenosu zpráv je přiřazený specifický kontejner. Kontejner je logická konstrukce, která používá právě jedno úložiště pro přenos zpráv k uložení všech relevantních dat pro tento kontejner. Každý kontejner je přiřazený ke zprostředkovatelskému uzlu přenosu zpráv. Typicky je víc kontejnerů než zprostředkovatelských uzlů přenosu zpráv. Každý zprostředkovatelský uzel služeb načte několik kontejnerů. Distribuce kontejnerů do zprostředkovatelského uzlu přenosu zpráv je organizovaná tak, aby všechny zprostředkovatelské uzly přenosu zpráv byly rovnoměrně zatížené. Pokud se vzorec zatížení změní (například jeden z kontejnerů začne být velmi vytížený) nebo pokud je jeden zprostředkovatelský uzel přenosu zpráv dočasně nedostupný, kontejnery se předistribuují mezi zbývající zprostředkovatelské uzly přenosu zpráv.

## <a name="processing-of-incoming-messaging-requests"></a>Zpracování příchozích událostí přenosu zpráv
Když klient odešle požadavek do služby Service Bus, nástroj pro vyrovnávání zatížení Azure ho přesměruje do jakéhokoli z dostupných zprostředkovatelských uzlů zasílání zpráv. Uzel brány ověří požadavek. Pokud se požadavek týká entity přenosu zpráv (fronty, témata, odběru), uzel brány entitu vyhledá v úložišti brány a zjistí, ve kterém úložišti pro přenos zpráv se entita nachází. Potom vyhledá, který zprostředkovatelský uzel přenosu zpráv se aktuálně stará o tento kontejner, a odešle žádost do tohoto zprostředkovatelského uzlu přenosu zpráv. Zprostředkovatelský uzel přenosu zpráv zpracuje požadavek a aktualizuje stav entity v úložišti kontejneru. Zprostředkovatelský uzel přenosu zpráv pak odešle odpověď zpět do uzlu brány, která odešle odpovídající odpověď zpět klientovi, který vyslal původní požadavek.

![Zpracování Příchozích událostí přenosu zpráv](./media/service-bus-architecture/ic690644.png)

## <a name="next-steps"></a>Další kroky
Teď, když znáte přehled architektury služby Service Bus, navštivte následující odkazy, abyste získali další informace:

* [Přehled přenosu zpráv ve službě Service Bus](service-bus-messaging-overview.md)
* [Základy služby Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Řešení zasílání zpráv ve frontě pomocí front Service Bus](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)



