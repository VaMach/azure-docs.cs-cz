---
title: "Zálohování Azure SQL Data Warehouse – snímky geograficky redundantní | Microsoft Docs"
description: "Další informace o zálohování integrovanou databází SQL Data Warehouse, které vám umožní obnovit do bodu obnovení nebo jiné zeměpisné oblasti Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: 
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: b5aff094-05b2-4578-acf3-ec456656febd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: 54c0149a769e654139bbdf709802d49127f041ac
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="sql-data-warehouse-backups"></a>Zálohování SQL Data Warehouse
SQL Data Warehouse nabízí místní i zeměpisné záloh v rámci možnosti zálohování datového skladu. Mezi ně patří Azure Storage Blob snímky a geograficky redundantní úložiště. Pomocí zálohování datového skladu obnovit váš datový sklad na bod obnovení v primární oblasti nebo obnovit do jiné zeměpisné oblasti. Tento článek vysvětluje, jaké jsou specifikace záloh v SQL Data Warehouse.

## <a name="what-is-a-data-warehouse-backup"></a>Co je zálohu datového skladu?
Zálohu datového skladu jsou data, která můžete použít k obnovení datového skladu v určený čas.  Vzhledem k tomu, že datový sklad SQL je distribuovaného systému, zálohu datového skladu se skládá z mnoho souborů, které jsou uložené v Azure BLOB. 

Zálohování databáze jsou nedílnou součást vámi vyžádaných jakékoli obchodní strategie pro obnovení kontinuity a po havárii, protože se data chránit před náhodnou poškození nebo odstranění. Další informace najdete v tématu [obchodní kontinuity přehled](../sql-database/sql-database-business-continuity.md).

## <a name="data-redundancy"></a>Redundance dat
SQL Data Warehouse chrání vaše data ukládáním dat v místně redundantní (LRS) Azure Premium Storage. Tato funkce Azure Storage ukládá několik synchronních kopií dat v místní datové centrum tak zajistila transparentní ochrana dat, pokud existují lokálních selhání. Redundanci dat zajišťuje, že vaše data přežijí problémy infrastruktury, jako je selhání disku. Redundanci dat zajišťuje kontinuity podnikových procesů s odolné proti chybám a vysoce dostupné infrastruktuře.

Další informace o:

* Azure Premium storage, najdete v části [Úvod do Azure Premium Storage](../storage/common/storage-premium-storage.md).
* Místně redundantní úložiště, najdete v části [replikace Azure Storage](../storage/common/storage-redundancy.md#locally-redundant-storage).

## <a name="azure-storage-blob-snapshots"></a>Azure Storage Blob snímky
Jako výhodou používání Azure Premium Storage SQL Data Warehouse používá snímky Azure Storage Blob pro zálohování do datového skladu místně. Datový sklad můžete obnovit do bodu obnovení snímku. Snímky spuštění minimálně každých 8 hodin a jsou k dispozici sedm dní.  

Další informace o:

* Snímky objektů blob v Azure, najdete v části [vytvoření snímku blob](../storage/blobs/storage-blob-snapshots.md).

## <a name="geo-redundant-backups"></a>Geograficky redundantní zálohy
Každých 24 hodin, SQL Data Warehouse ukládá úplnou datového skladu do standardního úložiště. Úplné datového skladu je vytvořit tak, aby odpovídaly čas poslední snímek. Standardní úložiště patří k účtu geograficky redundantní úložiště s přístupem pro čtení (RA-GRS). Funkce úložiště Azure RA-GRS replikuje záložní soubory do [spárované datového centra](../best-practices-availability-paired-regions.md). Geografická replikace zajistí, že datový sklad můžete obnovit v případě, že snímky nelze získat přístup v primární oblasti. 

Tato funkce je ve výchozím. Pokud nechcete použít geograficky redundantní zálohy, které můžete [chodit] (https://docs.microsoft.com/powershell/resourcemanager/Azurerm.sql/v2.1.0/Set-AzureRmSqlDatabaseGeoBackupPolicy?redirectedfrom=msdn). 

> [!NOTE]
> V úložišti Azure termín *replikace* odkazuje na kopírování souborů z jednoho umístění do druhého. SQL *replikace databáze* odkazuje na zadáte-li více sekundárních databází synchronizovány s primární databáze. 
> 
> 

> [!NOTE]
> Nelze zamítnutí geograficky redundantní zálohy DWU 9000 a DWU 18000. 
>
> 

Další informace o:

* Geograficky redundantní úložiště, najdete v části [replikace Azure Storage](../storage/common/storage-redundancy.md).
* RA-GRS úložiště, najdete v části [geograficky redundantní úložiště s přístupem pro čtení](../storage/common/storage-redundancy.md#read-access-geo-redundant-storage).

## <a name="data-warehouse-backup-schedule-and-retention-period"></a>Datový sklad plán zálohování a období uchovávání dat
SQL Data Warehouse vytvoří snímky na online datových skladů každých čtyř do osmi hodin a udržuje jednotlivých snímků sedm dní. Online databáze můžete obnovit k jednomu z bodů obnovení v posledních sedmi dnech. 

Pokud chcete zobrazit při spuštění poslední snímek, spusťte tento dotaz na online SQL Data Warehouse. 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

Pokud je potřeba zachovat snímek po dobu delší než 7 dní, můžete obnovit bod obnovení na nový datový sklad. Po dokončení obnovení SQL Data Warehouse spustí vytváření snímků na nový datový sklad. Pokud neprovedete změny do nového datového skladu, snímky zůstane prázdný a proto je minimální náklady na snímku. Může také pozastavit databázi do SQL Data Warehouse zabránit ve vytváření snímků.

### <a name="what-happens-to-my-backup-retention-while-my-data-warehouse-is-paused"></a>Co se stane Moje uchovávání záloh, zatímco Moje datového skladu byla pozastavena?
SQL Data Warehouse nevytváří žádné snímky a nevyprší snímky, zatímco datového skladu je pozastavena. Stáří snímku nedojde ke změně při datového skladu je pozastavena. Počet dnů, po který datový sklad je online, není dny kalendáře vychází uchování snímku.

Například pokud snímku spustí říjen 1 na 16: 00 a datového skladu je pozastavena. října 3 na 16: 00, snímku je dva dny. Vždy, když přejde do režimu online datový sklad je snímek dva dny. Pokud datový sklad přechodu do režimu online. října 5 na 16: 00, snímku je dva dny a zůstane další pět dní.

Když datového skladu přejde do režimu online, SQL Data Warehouse obnoví nových snímků a vyprší platnost snímků, pokud mají víc než sedm dnů s daty.

### <a name="how-long-is-the-retention-period-for-a-dropped-data-warehouse"></a>Jak dlouho je doba uchování vynechaných datového skladu?
Když datový sklad je vynechána, datový sklad a snímky jsou uložit sedm dní a pak odebrat. Datový sklad můžete obnovit všechny body obnovení uložené.

> [!IMPORTANT]
> Pokud odstraníte logické instance systému SQL server, všechny databáze patřící k instanci budou také odstraněny a nelze jej obnovit. Nelze obnovit odstraněné serveru.
> 
> 

## <a name="data-warehouse-backup-costs"></a>Zálohování náklady datového skladu
Celková cena vaší primární datového skladu a sedm dní snímků objektů Blob v Azure se zaokrouhlí na nejbližší TB. Například pokud váš datový sklad je 1,5 TB a snímky používat 100 GB, se vám účtuje 2 TB dat tempem Azure Premium Storage. 

> [!NOTE]
> Každý snímek je původně prázdné a zvětšování při provádění změn do primárního datového skladu. Všechny snímky zvýšit velikost jako změny datového skladu. Proto náklady na úložiště pro snímky růst podle rychlost změny.
> 
> 

Pokud používáte geograficky redundantní úložiště, obdržíte samostatné úložiště poplatků. Geograficky redundantní úložiště se fakturuje standardní sazbou přístup pro čtení geograficky redundantní úložiště (RA-GRS).

Další informace o cenách služby SQL Data Warehouse najdete v tématu [SQL Data Warehouse – ceny](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="using-database-backups"></a>Pomocí zálohování databáze
Primárního použití pro SQL data warehouse zálohy je obnovení datového skladu k jednomu z bodů obnovení v rámci dobu uchování.  

* Chcete-li obnovit SQL data warehouse, najdete v části [obnovení SQL data warehouse](sql-data-warehouse-restore-database-overview.md).

## <a name="related-topics"></a>Související témata
### <a name="scenarios"></a>Scénáře
* Přehled kontinuity obchodních, najdete v části [obchodní kontinuity přehled](../sql-database/sql-database-business-continuity.md)

<!-- ### Tasks -->

* Chcete-li obnovit datového skladu, přečtěte si téma [obnovení SQL data warehouse](sql-data-warehouse-restore-database-overview.md)

<!-- ### Tutorials -->

