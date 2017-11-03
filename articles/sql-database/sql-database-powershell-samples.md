---
title: "Azure příklady skript prostředí PowerShell pro databázi SQL. | Microsoft Docs"
description: "Skript prostředí Azure PowerShell, příklady vám pomohou vytvořit a spravovat servery Azure SQL Database, elastické fondy, databází a brány firewall."
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: jhubbard
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: overview-samples
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: sql-database
ms.workload: On Demand
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 1d1692cc43a7a5ec50c0689706d93a784a5fed88
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="azure-powershell-samples-for-azure-sql-database"></a>Ukázek Azure PowerShell pro Azure SQL Database

Následující tabulka obsahuje odkazy na ukázkové skripty prostředí Azure PowerShell pro Azure SQL Database.

| |  |
|---|---|
|**Vytvoření jedné databáze a fondu elastické databáze**||
| [Vytvořte jednu databázi a nakonfigurujte pravidlo brány firewall](scripts/sql-database-create-and-configure-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript prostředí PowerShell vytvoří jednu databázi Azure SQL a nakonfiguruje pravidla brány firewall na úrovni serveru. |
| [Vytvoření elastických fondů a přesunutí databází ve fondu](scripts/sql-database-move-database-between-pools-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript prostředí PowerShell vytvoří fondů elastické databáze SQL Azure a přesune databáze ve fondu a změny úrovně výkonu.|
|**Konfigurace geografická replikace a převzetí služeb při selhání**||
| [Konfigurace a převzetí služeb při selhání jedné databáze používá aktivní geografickou replikaci](scripts/sql-database-setup-geodr-and-failover-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript prostředí PowerShell nakonfiguruje aktivní geografickou replikací pro jednu databázi Azure SQL a převezme na sekundární repliku. |
| [Konfigurace a převzetí služeb při selhání ve fondu databáze používá aktivní geografickou replikaci](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript prostředí PowerShell nakonfiguruje aktivní geografickou replikací pro Azure SQL database v elastický fond SQL a převezme na sekundární repliku. |
| [Konfigurace a převzetí služeb při selhání a převzetí služeb při selhání skupiny pro jednu databázi (preview)](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript prostředí PowerShell nakonfiguruje skupinu převzetí služeb při selhání pro instanci serveru Azure SQL Database, přidává databáze ke skupině převzetí služeb při selhání a převezme na sekundární server |
|**Škálování izolovaných databází a fondu elastické databáze**||
| [Škálování jedné databáze](scripts/sql-database-monitor-and-scale-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Toto monitorování skriptu prostředí PowerShell metriky výkonu databáze Azure SQL, škálovatelná pro vyšší úroveň výkonu a vytvoří pravidlo výstrahy na jednom metrik výkonu. |
| [Škálování fondu elastické databáze](scripts/sql-database-monitor-and-scale-pool-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Toto monitorování skriptu prostředí PowerShell metriky výkonu fondu elastické databáze Azure SQL Database škáluje na vyšší úroveň výkonu a vytvoří pravidlo výstrahy na jednom metrik výkonu.  |
| **Auditování a zjišťování hrozeb** |
| [Konfigurace auditování a detekce hrozeb](scripts/sql-database-auditing-and-threat-detection-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript prostředí PowerShell nakonfiguruje zásady detekce hrozeb a auditování pro databázi Azure SQL. |
| **Obnovit, kopírování a importování databáze.**||
| [Obnovení databáze](scripts/sql-database-restore-database-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript prostředí PowerShell obnoví databázi Azure SQL z geograficky redundantní zálohy a obnovení odstraněné databáze Azure SQL nejnovější zálohu. |
| [Zkopírujte databázi do nového serveru](scripts/sql-database-copy-database-to-new-server-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript prostředí PowerShell vytvoří kopii existující databázi Azure SQL v nový server Azure SQL. |
| [Import ze souboru bacpac souboru databáze](scripts/sql-database-import-from-bacpac-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript Powershellu importuje ze souboru bacpac souboru databáze serveru Azure SQL. |
| **Synchronizaci dat mezi databázemi**||
| [Synchronizaci dat mezi databází SQL](scripts/sql-database-sync-data-between-sql-databases.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript prostředí PowerShell nakonfiguruje synchronizaci dat pro synchronizaci mezi více databází Azure SQL. |
| [Synchronizaci dat mezi SQL Database a SQL Server na místě](scripts/sql-database-sync-data-between-azure-onprem.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Tento skript prostředí PowerShell nakonfiguruje synchronizaci dat pro synchronizaci mezi Azure SQL database a místní databázi systému SQL Server. |
|||
|||
