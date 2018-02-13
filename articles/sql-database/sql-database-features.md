---
title: "Přehled funkcí databází SQL Azure | Dokumentace Microsoftu"
description: "Na této stránce je k dispozici přehled logických serverů a databází služby Azure SQL Database a matice na podporu funkcí s odkazy na všechny uvedené funkce."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: d1a46fa4-53d2-4d25-a0a7-92e8f9d70828
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 02/08/2018
ms.author: carlrab
ms.openlocfilehash: 23474539fd7db113ec4d857269056fb5061becca
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/11/2018
---
# <a name="azure-sql-database-features"></a>Funkce služby Azure SQL Database

Azure SQL Database sdílí společný kód základní s SQL serverem a na úrovni databáze podporuje většinu stejné funkce. Hlavní funkce rozdíly mezi Azure SQL Database a SQL Server jsou na úrovni instance. 

Pokračujeme v přidávání funkcí do Azure SQL Database. Doporučujeme vám proto navštívit webovou stránku aktualizace služeb pro Azure a použít filtry, které obsahuje:

* Filtrování na [službu SQL Database](https://azure.microsoft.com/updates/?service=sql-database)
* Filtrování na [oznámení všeobecné dostupnosti ](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) pro funkce služby SQL Database

## <a name="sql-server-and-sql-database-feature-support"></a>Funkce Podpora systému SQL Server a databáze SQL

V následující tabulce jsou uvedeny hlavní funkce systému SQL Server a poskytuje informace o tom, zda každý konkrétní funkce je podporovaná a obsahuje odkazy na další informace o funkci. Pokud chcete při migraci z existujícího databázového řešení vzít v úvahu odlišnosti jazyka Transact-SQL, přečtěte si [řešení rozdílů jazyka Transact-SQL během migrace na službu SQL Database](sql-database-transact-sql-information.md).


| **SQL Server Feature** | **Podporované služby Azure SQL Database** | 
| --- | --- |  
| [Funkce Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Ano – viz [úložiště certifikátů](sql-database-always-encrypted.md) a [Key vault](sql-database-always-encrypted-azure-key-vault.md)|
| [Skupiny dostupnosti AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | Zajištění vysoké dostupnosti je součástí každé databázi. V tématu [vysokou dostupnost a Azure SQL Database](sql-database-high-availability.md). Zotavení po havárii je popsána v [přehled kontinuity podnikových procesů s Azure SQL Database](sql-database-geo-replication-overview.md) |
| [Připojit databázi](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Ne |
| [Aplikační role](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | Ano |
|[Auditování](/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Ano](sql-database-auditing.md)|
| [Automatické ladění](/sql/relational-databases/automatic-tuning/automatic-tuning)| [Ano](sql-database-automatic-tuning.md)|
| [Soubor souboru BACPAC (exportovat)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | Ano – najdete v části [export databáze SQL](sql-database-export.md) |
| [Soubor souboru BACPAC (Importovat)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | Ano – najdete v části [import databáze SQL](sql-database-import.md) |
| [Příkaz BACKUP](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Ne – najdete v části [automatizované zálohování](sql-database-automated-backups.md) |
| [Integrované funkce](https://docs.microsoft.com/sql/t-sql/functions/functions) | Většina - jednotlivých funkcí najdete v části |
| [Funkce Change data capture](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Ne |
| [Sledování změn](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | Ano |
| [Kolace příkazy](https://docs.microsoft.com/sql/t-sql/statements/collations) | Ano |
| [Indexy Columnstore](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | Ano – [pouze ve verzi Premium](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |
| [Modul common language runtime (CLR)](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Ne |
| [Databáze s omezením](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | Ano |
| [Uživatelů s omezením](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | Ano |
| [Řízení toku klíčová slova jazyka](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | Ano |
| [Mezi databázové dotazy](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/cross-database-queries) | Částečně – viz [Elastické dotazy](sql-database-elastic-query-overview.md) |
| [Kurzory](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | Ano | 
| [Komprese dat](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | Ano |
| [Databázového e-mailu](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Ne |
| [Zrcadlení databáze](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Ne |
| [Nastavení konfigurace databáze](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | Ano |
| [Data Quality Services (DQS)](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Ne |
| [Snímky databáze](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Ne |
| [Datové typy](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | Ano |  
| [Příkaz DBCC příkazy](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | Většina - najdete jednotlivé příkazy |
| [Příkazy DDL](https://docs.microsoft.com/sql/t-sql/statements/statements) | Ano |
| [Aktivační události DDL](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | Pouze pro databázi |
| [Distribuované transakce - MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Ne – najdete v části [elastické transakce](sql-database-elastic-transactions-overview.md) |
| [Příkazy DML](https://docs.microsoft.com/sql/t-sql/queries/queries) | Ano |
| [Aktivační události DML](https://docs.microsoft.com/sql/relational-databases/triggers/create-dml-triggers) | Většina - najdete jednotlivé příkazy | 
| [Zobrazení dynamické správy](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | Některé - najdete v části jednotlivých zobrazení dynamické správy |
|[Dynamické maskování dat](/sql/relational-databases/security/dynamic-data-masking)|[Ano](sql-database-dynamic-data-masking-get-started.md)|
| [Oznamování událostí](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | Ne – najdete v části [výstrahy](sql-database-insights-alerts-portal.md) |
| [Výrazy](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |Ano |
| [Rozšířené události](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Některé - najdete v části [rozšířené události v databázi SQL](sql-database-xevent-db-diff-from-svr.md) |
| [Rozšířené uložené procedury](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Ne |
| [Soubory a skupiny souborů](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | Pouze skupiny primární soubor |
| [Filestream](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Ne |
| [Fulltextové vyhledávání](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) | Dělení textu třetích stran nejsou podporovány. |
| [Functions](https://docs.microsoft.com/sql/t-sql/functions/functions) | Většina - jednotlivých funkcí najdete v části |
| [Zpracování grafu](/sql/relational-databases/graphs/sql-graph-overview) | Ano |
| [Optimalizace v paměti](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | Ano – [pouze ve verzi Premium](sql-database-in-memory.md) |
| [Podpora data JSON](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | Ano |
| [Jazykové elementy](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | Většina - najdete jednotlivé elementy |  
| [Odkazované servery](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Ne – najdete v části [elastické dotazu](sql-database-elastic-query-horizontal-partitioning.md) |
| [Přesouvání protokolu](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | Zajištění vysoké dostupnosti je součástí každé databázi. Zotavení po havárii je popsána v [přehled kontinuity podnikových procesů s Azure SQL Database](sql-database-geo-replication-overview.md) |
| [Služby Master Data Services (služby MDS)](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Ne |
| [Minimální úroveň protokolování v hromadného importu](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Ne |
| [Úprava dat systému](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Ne |
| [Online operace indexu](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | Ano |
| [Operátory](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | Většina - najdete jednotlivé operátory |
| [Obnovení bodu v čase databáze](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | Ano – najdete v části [obnovení databáze SQL](sql-database-recovery-using-backups.md#point-in-time-restore) |
| [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Ne |
| [Na základě zásad správy](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Ne |
| [Predikáty](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Ano |
| [Služby R](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services) | Verze Preview; v tématu [co je nového v machine learning](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services)  |
| [Správce zdrojů](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Ne |
| [OBNOVENÍ příkazů](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Ne | 
| [Obnovit databázi ze zálohy](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | Z předdefinovaných záloh jenom - najdete v části [obnovení databáze SQL](sql-database-recovery-using-backups.md) |
| [Zabezpečení na úrovni řádků](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | Ano |
| [Sémantické vyhledávání](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Ne |
| [Pořadová čísla](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | Ano |
| [Služba Service Broker](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Ne |
| [Nastavení konfigurace serveru](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Ne |
| [Set – příkazy](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | Většina - najdete jednotlivé příkazy 
| [Spatial](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | Ano |
| [Agent SQL Server](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Ne – najdete v části [elastické úlohy](sql-database-elastic-jobs-getting-started.md) |
| [SQL Server Analysis Services (SSAS)](https://docs.microsoft.com/sql/analysis-services/analysis-services) | V tématu [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| [Auditování SQL serveru](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | Ne – najdete v části [auditování databáze SQL](sql-database-auditing.md) |
| [SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Ano – najdete v části [navýšení a shift SQL Server Integration Services zatížení do cloudu.](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-lift-shift-ssis-packages-overview) |
| [SQL Server PowerShell](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | Ano |
| [SQL Server Profiler](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Ne – najdete v části [rozšířené události](sql-database-xevent-db-diff-from-svr.md) |
| [Replikace SQL serveru](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Jenom předplatitelé transakční replikace a replikace snímků](sql-database-cloud-migrate.md) |
| [SQL Server Reporting Services (SSRS)](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Ne |
| [Uložené procedury](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | Ano |
| [Systémové uložené funkce](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Některé - jednotlivé funkce najdete v tématu |
| [Systémové uložené procedury](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Některé - najdete v části jednotlivých uložené procedury |
| [Systémové tabulky](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Některé - najdete v části jednotlivé tabulky |
| [Zobrazení katalogu systému](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Některé – viz jednotlivých zobrazení |
| [Dělení tabulky](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Ano – jenom primární skupinu souborů |
| [Dočasné tabulky](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#temporary-tables) | Místní a obor databáze globální dočasné tabulky jenom |
| [Dočasné tabulky](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | Ano |
| [Proměnné](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | Ano | 
| [Transparentní šifrování dat (šifrování TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | Ano |
| [Windows Server Failover Clustering](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | Zajištění vysoké dostupnosti je součástí každé databázi. Zotavení po havárii je popsána v [přehled kontinuity podnikových procesů s Azure SQL Database](sql-database-geo-replication-overview.md) |
| [Indexy XML](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | Ano |

## <a name="next-steps"></a>Další postup

- Informace o službě Azure SQL Database najdete v tématu [Co je SQL Database?](sql-database-technical-overview.md)
- Informace o podpoře a odlišnostech jazyka Transact-SQL najdete v tématu [řešícím odlišnosti jazyka Transact-SQL během migrace na službu SQL Database](sql-database-transact-sql-information.md).
