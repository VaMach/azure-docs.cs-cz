---
title: "Obchodní kontinuitu a zotavení po havárii (BCDR): spárovat oblasti Azure | Microsoft Docs"
description: "Další informace o Azure regionální párování zajistit, aby aplikace byly odolné při selhání datového centra."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: c2d0a21c-2564-4d42-991a-bc31723f61a4
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2017
ms.author: raynew
ms.openlocfilehash: 27491e34ad9e47aec2f424cfc439fad614f0e435
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="business-continuity-and-disaster-recovery-bcdr-azure-paired-regions"></a>Obchodní kontinuitu a zotavení po havárii (BCDR): spárovat oblasti Azure

## <a name="what-are-paired-regions"></a>Jaké jsou spárovat oblastí?

Azure funguje v různých zeměpisných po celém světě. Azure geography je definovaný oblasti na světě, který obsahuje alespoň jedné oblasti Azure. Oblast Azure je oblasti v rámci geography, který obsahuje jeden nebo více datových centrech.

Každé oblasti Azure je spárován s jinou oblast v rámci stejné geography, společně provedení pár místní. Výjimkou je Brazílie – Jih, což je spárován s oblastí mimo jeho geography.

![AzureGeography](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

Obrázek 1 – Azure regionální pár diagram

| Geography | Spárované oblastí |  |
|:--- |:--- |:--- |
| Asie |Východní Asie |Jihovýchodní Asie |
| Austrálie |Austrálie – východ |Austrálie – jihovýchod |
| Kanada |Kanada – střed |Východní Kanada |
| Čína |Čína – sever |Čína – východ|
| Indie |Střed Indie |Indie – jih |
| Japonsko |Japonsko – východ |Japonsko – západ |
| Korea |Korea – střed |Korea – jih |
| Severní Amerika |Střed USA – sever |Střed USA – jih |
| Severní Amerika |Východ USA |Západní USA |
| Severní Amerika |Východní USA 2 |Střed USA |
| Severní Amerika |Západní USA 2 |Západní střed USA |
| Evropa |Severní Evropa |Západní Evropa |
| Japonsko |Japonsko – východ |Japonsko – západ |
| Brazílie |Brazílie – Jih (1) |Střed USA – jih |
| US Government |USA (Gov) – Iowa |USA (Gov) – Virginia |
| US Government |USA (Gov) – Arizona |USA (Gov) – Texas |
| USA – oddělení obrany |US DoD – východ |US DoD – střed |
| SPOJENÉ KRÁLOVSTVÍ |Spojené království – západ |Spojené království – jih |
| Německo |Německo – střed |Německo – severovýchod |

Tabulka 1 - mapování regionální párů Azure

> (1) Brazílie – jih je jedinečný, protože je spárován s oblastí mimo svůj vlastní geography. Brazílie – jih sekundární oblast je jihu USA, ale jihu USA na sekundární oblast není Brazílie – jih.


Doporučujeme, abyste replikaci úloh mezi místní dvojice k těžit z Azure a zásad izolace a dostupnost. Například aktualizace plánované systému Azure se nasadí postupně (ne ve stejnou dobu) přes spárované oblasti. To znamená, že i v výjimečná událost vadný aktualizace, obou oblastí to nebude mít vliv současně. Navíc k nepravděpodobnému široký výpadku obnovení alespoň jedné oblasti mimo každý pár prioritu.

## <a name="an-example-of-paired-regions"></a>Příklad spárované oblastí
Obrázek 2 níže ukazuje hypotetické aplikace, který používá místní dvojici pro zotavení po havárii. Zelená čísla zvýrazněte mezi oblastmi aktivity tři služby Azure (Azure výpočty, úložiště a databáze) a jak jsou nakonfigurované pro replikaci přes oblasti. Jedinečných výhod nasazení přes spárované oblasti jsou vyznačené oranžové čísly.

![Přehled výhod spárované oblast](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

Obrázek 2 – hypotetický Azure regionální pár

## <a name="cross-region-activities"></a>Mezi oblastmi aktivity
Jak je uvedeno na obrázku 2.

![PaaS](./media/best-practices-availability-paired-regions/1Green.png) **Azure Compute (PaaS)** – je nutné zřídit další výpočetní prostředky předem zajistit prostředky jsou k dispozici v jiné oblasti při havárii. Další informace najdete v tématu [technické pokyny Azure odolnosti](resiliency/resiliency-technical-guidance.md).

![Úložiště](./media/best-practices-availability-paired-regions/2Green.png) **Azure Storage** -geograficky redundantní úložiště (GRS) je nakonfigurován ve výchozím nastavení při vytvoření účtu Azure Storage. S GRS, vaše data se automaticky replikují třikrát v rámci primární oblasti a třikrát v oblasti spárované. Další informace najdete v tématu [možnosti redundance úložiště Azure](storage/common/storage-redundancy.md).

![Azure SQL](./media/best-practices-availability-paired-regions/3Green.png) **databází SQL Azure** – s Azure SQL standardní geografickou replikaci, můžete nakonfigurovat asynchronní replikaci transakcí spárované oblasti. S premium geografickou replikací mohli konfigurovat replikaci pro všechny oblasti na světě; Doporučujeme však, že nasazování těchto prostředků v spárované oblasti pro většinu scénářů zotavení po havárii. Další informace najdete v tématu [geografická replikace ve službě Azure SQL Database](sql-database/sql-database-geo-replication-overview.md).

![Správce prostředků](./media/best-practices-availability-paired-regions/4Green.png) **Azure Resource Manager** -logická izolace součásti pro správu služby Správce prostředků ze své podstaty poskytuje v rámci oblasti. To znamená, že je pravděpodobnější mít vliv na jiné logické selhání v jedné oblasti.

## <a name="benefits-of-paired-regions"></a>Výhody spárované oblastí
Jak je uvedeno na obrázku 2.  

![Izolace](./media/best-practices-availability-paired-regions/5Orange.png)
**fyzické izolace** – Pokud je to možné, Azure upřednostní alespoň 300 miles oddělení mezi datových centrech v oblasti páru, i když to není praktické nebo možné ve všech geografických. Oddělení fyzického datacentra snižuje pravděpodobnost přírodní katastrofy, občanského unrest, výpadky napájení nebo fyzické sítě výpadků ovlivňující obou oblastí najednou. Izolace podléhá omezení v rámci geography (geography velikost, dostupnost infrastruktury sítě nebo napájení, předpisy atd.).  

![Replikace](./media/best-practices-availability-paired-regions/6Orange.png)
**poskytovanou platformou replikace** -některé služby, jako je například geograficky redundantní úložiště poskytují automatické replikace spárované oblasti.

![Obnovení](./media/best-practices-availability-paired-regions/7Orange.png)
**oblasti obnovení pořadí** – v případě, že široký výpadku, obnovení jedné oblasti mimo každý pár prioritu. Aplikace, které jsou nasazeny v oblastech spárované kombinace obsahuje jeden z oblasti obnovit s prioritou. Pokud je aplikace nasazená v oblastech, které nejsou spárovány, obnovení může být zpoždění – v nejhorším případě vybrané oblasti může být poslední dva, která se bude obnovovat.

![Aktualizace](./media/best-practices-availability-paired-regions/8Orange.png)
**sekvenčním aktualizacím** – Azure plánované aktualizace systému se nasazuje pro spárované oblasti postupně (ne ve stejnou dobu) Chcete-li minimalizovat prostoje, účinek chyby a logické selhání v výjimečná událost chybný aktualizace.

![Data](./media/best-practices-availability-paired-regions/9Orange.png)
**Data sídle** – oblasti se nachází v rámci stejné geography jako jeho pár (s výjimkou Brazílie – jih) pro splnění požadavků sídle data pro účely jurisdikce vynucení daň a zákonem.
