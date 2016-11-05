---
title: Architektura služby Service Bus | Microsoft Docs
description: Popisuje architekturu zpracování zpráv a přenosů služby Azure Service Bus.
services: service-bus
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''

ms.service: service-bus
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2016
ms.author: sethm

---
# Architektura služby Service Bus
Tento článek popisuje architekturu zpracování zpráv a přenosů služby Azure Service Bus.

## Jednotky škálování služby Service Bus
Service Bus se organizuje podle *jednotek škálování*. Jednotka škálování je jednotka nasazení a obsahuje všechny komponenty potřebné k tomu, aby služba běžela. Každá oblast nasadí jednu nebo víc jednotek škálování služby Service Bus.

K jednotce škálování je mapovaný obor názvů služby Service Bus. Jednotka škálování zpracovává všechny typy entit služby Service Bus: předání a entity zprostředkovaného zasílání zpráv (fronty, témata, odběry). Jednotka škálování služby Service Bus má tyto součásti:

* **Sada uzlů brány.** Uzly brány ověřují příchozí požadavky a zpracovávají požadavky na předání. Každý uzel brány má veřejnou IP adresu.
* **Sada zprostředkovatelských uzlů pro přenos zpráv.** Zprostředkovatelské uzly pro přenos zpráv zpracovávají požadavky týkající se entit přenos zpráv.
* **Jedno úložiště brány.** Úložiště brány uchovává data pro každou entitu definovanou v této jednotce škálování. Úložiště brány se implementuje nad databázi SQL Azure.
* **Několik úložišť pro přenos zpráv.** Úložiště pro přenos zpráv uchovává zprávy všech front, témat a odběrů definovaných v této jednotce škálování. Taky obsahuje všechna data odběru. Pokud není zapnutá možnost [segmentované entity zasílání zpráv](service-bus-partitioning.md), mapuje se fronta nebo téma do jednoho úložiště pro přenos zpráv. Odběry jsou uložené ve stejném úložišti pro přenos zpráv jako jejich nadřazené téma. Kromě [Zasílání zpráv na úrovni Premium](service-bus-premium-messaging.md) služby Service Bus se úložiště pro zasílání zpráv implementují nad databáze Azure SQL.

## Kontejnery
Každé entitě přenosu zpráv je přiřazený specifický kontejner. Kontejner je logická konstrukce, která používá právě jedno úložiště pro přenos zpráv k uložení všech relevantních dat pro tento kontejner. Každý kontejner je přiřazený ke zprostředkovatelskému uzlu přenosu zpráv. Typicky je víc kontejnerů než zprostředkovatelských uzlů přenosu zpráv. Každý zprostředkovatelský uzel služeb načte několik kontejnerů. Distribuce kontejnerů do zprostředkovatelského uzlu přenosu zpráv je organizovaná tak, aby všechny zprostředkovatelské uzly přenosu zpráv byly rovnoměrně zatížené. Pokud se vzorec zatížení změní (například jeden z kontejnerů začne být velmi vytížený) nebo pokud je jeden zprostředkovatelský uzel přenosu zpráv dočasně nedostupný, kontejnery se předistribuují mezi zbývající zprostředkovatelské uzly přenosu zpráv.

## Zpracování příchozích událostí přenosu zpráv
Když klient odešle požadavek do služby Service Bus, nástroj pro vyrovnávání zatížení Azure ho přesměruje do jakéhokoli z dostupných zprostředkovatelských uzlů zasílání zpráv. Uzel brány ověří požadavek. Pokud se požadavek týká entity přenosu zpráv (fronty, témata, odběru), uzel brány entitu vyhledá v úložišti brány a zjistí, ve kterém úložišti pro přenos zpráv se entita nachází. Potom vyhledá, který zprostředkovatelský uzel přenosu zpráv se aktuálně stará o tento kontejner, a odešle žádost do tohoto zprostředkovatelského uzlu přenosu zpráv. Zprostředkovatelský uzel přenosu zpráv zpracuje požadavek a aktualizuje stav entity v úložišti kontejneru. Zprostředkovatelský uzel přenosu zpráv pak odešle odpověď zpět do uzlu brány, která odešle odpovídající odpověď zpět klientovi, který vyslal původní požadavek.

![Zpracování Příchozích událostí přenosu zpráv](./media/service-bus-architecture/IC690644.png)

## Zpracování příchozích událostí požadavků na předání
Když klient odešle požadavek do služby Service Bus, nástroj pro vyrovnávání zatížení Azure ho přesměruje do jakéhokoli z dostupných zprostředkovatelských uzlů zasílání zpráv. Pokud se jedná o požadavek na poslech, uzel brány vytvoří nové propojení. Pokud se jedná o požadavek na připojení ke konkrétnímu propojení, uzel brány předá požadavek na spojení uzlu brány, který vlastní požadované propojení. Uzel brány, který vlastní požadované propojení, pošle čekajícímu klientovi požadavek na setkání a pokyn, aby klient vytvořil dočasný kanál pro uzel brány, který obdržel požadavek na připojení.

Když se vytvoří předávací spojení, klienti si můžou vyměňovat zprávy přes uzel brány, který se používá pro setkání.

![Zpracování Příchozích událostí požadavků na předání](./media/service-bus-architecture/IC690645.png)

## Další kroky
Teď, když znáte přehled architektury služby Service Bus, ji můžete začít používat. K tomu vám pomůžou tyto odkazy:

* [Přehled přenosu zpráv ve službě Service Bus](service-bus-messaging-overview.md)
* [Základy služby Service Bus](../service-bus/service-bus-fundamentals-hybrid-solutions.md)
* [Řešení přenosu zpráv ve frontě pomocí front Service Bus](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)

<!--HONumber=Sep16_HO4-->


