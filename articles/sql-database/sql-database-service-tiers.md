---
title: "Služba Azure SQL Database | Microsoft Docs"
description: "Další informace o úrovních služeb pro jeden a zadejte velikost úložiště a úrovně výkonu databáze fondu."
keywords: 
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 08/20/2017
ms.author: carlrab
ms.openlocfilehash: 55f59fddee008eb42b7252d6368a56873a6abd16
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="what-are-azure-sql-database-service-tiers"></a>Co je Azure SQL Database úrovně služeb

[Azure SQL Database](sql-database-technical-overview.md) nabízí **základní**, **standardní**, **Premium**, a **Premium RS** úrovních pro obě služeb[jedna databáze](sql-database-single-database-resources.md) a [elastické fondy](sql-database-elastic-pool.md). Úrovně služeb jsou primárně rozlišené pomocí celou řadu úroveň výkonu a možnosti velikosti úložiště a ceny.  Všechny úrovně služeb poskytují flexibilitu při změně velikosti výkonu úroveň a úložiště.  Izolované databáze i elastické fondy se fakturuje po hodinách podle úrovně služby, úroveň výkonu a velikost úložiště.   

## <a name="choosing-a-service-tier"></a>Výběr úrovně služeb

Volba úrovně služby závisí hlavně na kontinuity podnikových procesů, úložiště a požadavky na výkon.
| | **Basic** | **Standard** |**Premium** |**Premium RS** |
| :-- | --: |--:| --:| --:| 
|Cíl pracovního vytížení|Vývoj a výroby|Vývoj a výroby|Vývoj a výroby|Úlohy, které může tolerovat ztráty dat až 5 minut z důvodu selhání služby|
|Smlouva SLA o provozuschopnosti|99,99 %|99,99 %|99,99 %|Není k dispozici při ve verzi preview|
|Uchování záloh|7 dní|35 dní|35 dní|35 dní|
|Procesor|Nízký|Nízká, střední, vysoká|Střední, vysoká|Střednědobé používání|
|Propustnost vstupně-výstupní operace|Nízký  | Střednědobé používání | O řád vyšší než Standard|Stejné jako Premium|
|Latence vstupně-výstupní operace|Vyšší než Premium|Vyšší než Premium|Nižší než Basic a Standard|Stejné jako Premium|
|Indexu Columnstore a OLTP v paměti|Není k dispozici|Není k dispozici|Podporuje se|Podporuje se|
|||||

## <a name="performance-level-and-storage-size-limits"></a>Omezení velikosti úroveň výkonu a úložiště

Úrovně výkonu jsou vyjádřeny z hlediska jednotky transakcí databáze (Dtu) pro izolované databáze a transakčních jednotkách elastické databáze (Edtu) pro elastické fondy. Další informace o Dtu a Edtu najdete v tématu [co jsou Dtu a Edtu?](sql-database-what-is-a-dtu.md).

### <a name="single-databases"></a>Izolované databáze

|  | **Basic** | **Standard** | **Premium** | **Premium RS**|
| :-- | --: | --: | --: | --: |
| Maximální velikost úložiště velikost * | 2 GB | 1 TB | 4 TB  | 1 TB  |
| Maximální počet jednotek Dtu | 5 | 3000 | 4000 | 1000 |
||||||

### <a name="elastic-pools"></a>Elastické fondy

| | **Basic** | **Standard** | **Premium** | **Premium RS**|
| :-- | --: | --: | --: | --: |
| Maximální velikost úložiště pro databázi *  | 2 GB | 1 TB | 1 TB | 1 TB |
| Maximální velikost úložiště pro fond * | 156 GB | 4 TB | 4 TB | 1 TB |
| Maximální počet jednotek Edtu na databázi | 5 | 3000 | 4000 | 1000 |
| Maximální počet jednotek Edtu na fond | 1600 | 3000 | 4000 | 1000 |
| Maximální počet databází na každý fond | 500  | 500 | 100 | 100 |
||||||

> [!IMPORTANT]
> \* Velikosti úložiště větší než velikost zahrnutého úložiště jsou ve verzi Preview a účtují se za ně další poplatky. Podrobnosti najdete na stránce s [cenami služby SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). 
>
> \* Na úrovni Premium je úložiště větší než 1 TB aktuálně dostupné v následujících oblastech: USA – východ 2, USA – západ, USA (Gov) – Virginia, Západní Evropa, Německo – střed, Jihovýchodní Asie, Japonsko – východ, Austrálie – východ, Kanada – střed a Kanada – východ. Viz [Aktuální omezení pro P11–P15](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 

Podrobnosti o konkrétní úrovně výkonu a možnosti velikosti úložiště k dispozici v tématu [limitů prostředků databáze SQL](sql-database-resource-limits.md).


## <a name="next-steps"></a>Další kroky

- Další informace o [jedna databáze prostředků](sql-database-single-database-resources.md).
- Další informace o elastické fondy najdete v tématu [elastické fondy](sql-database-elastic-pool.md).
- Další informace o [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md)
* Další informace o [Dtu a Edtu](sql-database-what-is-a-dtu.md).
* Další informace o sledování využití v jednotkách DTU najdete v tématu [monitorování a optimalizace výkonu](sql-database-troubleshoot-performance.md).

