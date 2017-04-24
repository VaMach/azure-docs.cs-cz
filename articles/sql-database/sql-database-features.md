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
ms.custom: features
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 03/03/2017
ms.author: carlrab; jognanay
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: a1ede93b5aacf0d8a5bcf83f208f72be328ee72f
ms.lasthandoff: 04/15/2017


---
# <a name="azure-sql-database-features"></a>Funkce služby Azure SQL Database

Následující tabulky uvádějí hlavní funkce Azure SQL Database a odpovídajících funkce SQL Serveru. Poskytují také informace o tom, zda každá konkrétní funkce je podporovaná, a odkaz na další informace o funkci na každé platformě. Pokud chcete při migraci z existujícího databázového řešení vzít v úvahu odlišnosti jazyka Transact-SQL, přečtěte si [řešení rozdílů jazyka Transact-SQL během migrace na službu SQL Database](sql-database-transact-sql-information.md).

Pokračujeme v přidávání funkcí do Azure SQL Database. Doporučujeme vám proto navštívit webovou stránku aktualizace služeb pro Azure a použít filtry, které obsahuje:

* Filtrování na [službu SQL Database](https://azure.microsoft.com/updates/?service=sql-database)
* Filtrování na [oznámení všeobecné dostupnosti ](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) pro funkce služby SQL Database

| **Funkce** | **SQL Server** | **Azure SQL Database** | 
| --- | :---: | :---: | 
| Aktivní geografická replikace | Nepodporuje se – viz [Skupiny dostupnosti Always On](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | [Podporuje se](sql-database-geo-replication-overview.md)
| Funkce Always Encrypted | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Podporuje se – viz [Úložiště certifikátů](sql-database-always-encrypted.md) a [Key Vault](sql-database-always-encrypted-azure-key-vault.md)|
| Skupiny dostupnosti AlwaysOn | [Podporuje se](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | Nepodporuje se – viz [Aktivní geografická replikace](sql-database-geo-replication-overview.md) |
| Připojení databáze | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Nepodporuje se |
| Aplikační role | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) |
| Automatické škálování | Nepodporuje se | Podporuje se – viz [Úrovně služeb](sql-database-service-tiers.md) |
| Azure Active Directory | Nepodporuje se | [Podporuje se](sql-database-aad-authentication.md) |
| Azure Data Factory | [Podporuje se](../data-factory/data-factory-introduction.md) | [Podporuje se](../data-factory/data-factory-introduction.md) |
| Auditování | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Podporuje se](sql-database-auditing.md) |
| Soubor BACPAC (export) | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | [Podporuje se](sql-database-export.md) |
| Soubor BACPAC (import) | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | [Podporuje se](sql-database-import.md) |
| BACKUP | [Podporuje se](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Nepodporuje se |
| Integrované funkce | [Podporuje se](https://docs.microsoft.com/sql/t-sql/functions/functions) | Většina – viz [jednotlivé funkce] (https://docs.microsoft.com/sql/t-sql/functions/functions) |
| Change data capture | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Nepodporuje se |
| Sledování změn | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) |
| Příkazy kolace | [Podporuje se](https://docs.microsoft.com/sql/t-sql/statements/collations) | [Podporuje se](https://docs.microsoft.com/sql/t-sql/statements/collations) |
| Indexy Columnstore | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | [Jenom verze Premium Edition](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |
| Common language runtime (CLR) | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Nepodporuje se |
| Databáze s omezením | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) |
| Uživatelé s omezením | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | [Podporuje se](sql-database-manage-logins.md#non-administrator-users) |
| Kontrola klíčových slov toku jazyka | [Podporuje se](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | [Podporuje se](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) |
| Mezidatabázové dotazy | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/cross-database-queries) | Částečně – viz [Elastické dotazy](sql-database-elastic-query-overview.md) |
| Kurzory | [Podporuje se](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | [Podporuje se](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | 
| Komprese dat | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) |
| Zálohování databáze | [Spravované uživatelem](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases) | [Spravované službou SQL Database](sql-database-automated-backups.md) |
| Databázový e-mail | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Nepodporuje se |
| Zrcadlení databáze | [Podporuje se](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Nepodporuje se |
| Nastavení konfigurace databáze | [Podporuje se](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | [Podporuje se](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) |
| Data Quality Services (DQS) | [Podporuje se](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Nepodporuje se |
| Snímky databáze | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Nepodporuje se |
| Typy dat | [Podporuje se](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | [Podporuje se](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) |  
| Příkazy DBCC | [Podporuje se](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | Většina – viz [jednotlivé příkazy](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) |
| Příkazy DDL | [Podporuje se](https://docs.microsoft.com/sql/t-sql/statements/statements) | Většina – viz [jednotlivé příkazy](https://docs.microsoft.com/sql/t-sql/statements/statements)
| Triggery DDS | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | [Jenom databáze](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) |
| Distribuované transakce | [MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Jenom omezené scénáře v rámci služby SQL Database |
| Příkazy DML | [Podporuje se](https://docs.microsoft.com/sql/t-sql/queries/queries) | Většina – viz [jednotlivé příkazy](https://docs.microsoft.com/sql/t-sql/queries/queries) |
| Triggery DML | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/triggers/dml-triggers) | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/triggers/dml-triggers) |
| Zobrazení dynamických zpráv | [Všechny](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | Některé – viz [jednotlivá zobrazení DMV](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) |
| Elastické fondy | Nepodporuje se | [Podporuje se](sql-database-elastic-pool.md) |
| Elastické úlohy | Nepodporuje se – viz [Agent systému SQL Server](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | [Podporuje se](sql-database-elastic-jobs-getting-started.md) | 
| Elastické dotazy | Nepodporuje se – viz [Mezidatabázové dotazy](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/cross-database-queries) | [Podporuje se](sql-database-elastic-query-overview.md) |
| Oznámení událostí | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | [Podporuje se](sql-database-insights-alerts-portal.md) |
| Výrazy | [Podporuje se](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) | [Podporuje se](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |
| Rozšířené události | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Některé – viz [jednotlivá zobrazení DMV](sql-database-xevent-db-diff-from-svr.md) |
| Rozšířené uložené procedury | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Nepodporuje se |
| Soubory a skupiny souborů | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | [Jenom primární](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) |
| FileStream | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Nepodporuje se |
| Fulltextové vyhledávání | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) | [Nepodporované moduly pro dělení na slova třetích stran](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) |
| Funkce | [Podporuje se](https://docs.microsoft.com/sql/t-sql/functions/functions) | Většina – viz [jednotlivé funkce](https://docs.microsoft.com/sql/t-sql/functions/functions) |
| Optimalizace v paměti | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | [Jenom verze Premium Edition](sql-database-in-memory.md) |
| Úlohy | Viz [Agent SQL Server](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Viz [Elastické úlohy](sql-database-elastic-jobs-getting-started.md) |
| Podpora dat JSON | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | [Podporuje se](sql-database-json-features.md) |
| Elementy jazyka | [Podporuje se](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | Většina – viz [jednotlivé elementy](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) |  
| Propojené servery | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Nepodporuje se – viz [Elastický dotaz](sql-database-elastic-query-horizontal-partitioning.md) |
| Přesouvání protokolu | [Podporuje se](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | Nepodporuje se – viz [Aktivní geografická replikace](sql-database-geo-replication-overview.md) |
| Master Data Services (MDS) | [Podporuje se](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Nepodporuje se |
| Minimální protokolování v hromadném importu | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Nepodporuje se |
| Úprava dat systému | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Nepodporuje se |
| Online indexovací operace | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | [Podporuje se – velikost transakce omezená podle úrovně služeb](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) |
| Operátory | [Podporuje se](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | Většina – viz [jednotlivé operátory](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) |
| Obnovení databáze k určitému bodu v čase | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | [Podporuje se](sql-database-recovery-using-backups.md#point-in-time-restore) |
| Polybase | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Nepodporuje se
| Správa na základě zásad | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Nepodporuje se |
| Predikáty | [Podporuje se](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Většina – viz [jednotlivé predikáty](https://docs.microsoft.com/sql/t-sql/queries/predicates)
| Služby R | [Podporuje se](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services)
| Správce prostředků | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Nepodporuje se |
| Příkazy RESTORE | [Podporuje se](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Nepodporuje se | 
| Obnovení databáze ze zálohy | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | [Jenom z integrovaných záloh](sql-database-recovery-using-backups.md) |
| Zabezpečení na úrovni řádků | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) |
| Sémantické vyhledávání | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Nepodporuje se |
| Pořadová čísla | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) |
| Service Broker | [Podporuje se](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Nepodporuje se |
| Nastavení konfigurace serveru | [Podporuje se](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Nepodporuje se – viz [Možnosti konfigurace databáze](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) |
| Příkazy Set | [Podporuje se](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | Většina – viz [jednotlivé příkazy](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) 
| Spatial | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) |
| Agent SQL Server | [Podporuje se](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Nepodporuje se – viz [Elastické úlohy](sql-database-elastic-jobs-getting-started.md) |
| Služba Analysis Services serveru SQL (SSAS) | [Podporuje se](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Nepodporuje se – viz [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| SQL Server Integration Services (SSIS) | [Podporuje se](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Nepodporuje se – viz [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) |
| PowerShell pro SQL Server | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) |
| SQL Server Profiler | [Podporuje se](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Nepodporuje se – viz [Rozšířené události](sql-database-xevent-db-diff-from-svr.md) |
| Replikace systému SQL Server | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Jenom předplatitelé transakční replikace a replikace snímků](sql-database-cloud-migrate.md) |
| SQL Server Reporting Services (SSRS) | [Podporuje se](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Nepodporuje se |
| Uložené procedury | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) |
| Systémové uložené funkce | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Některé – viz [jednotlivé funkce](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) |
| Systémové uložené procedury | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Některé – viz [jednotlivé uložené procedury](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) |
| Systémové tabulky | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Některé – viz [jednotlivé tabulky](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) |
| Zobrazení systémového katalogu | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Některé – viz [jednotlivá zobrazení](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql)
| Dělení tabulky | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Podporuje se – [Jenom primární skupina souborů](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) |
| Dočasné tabulky | [Místní a globální](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#temporary-tables) | [Jenom místní](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#temporary-tables) |
| Dočasné tabulky | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | [Podporuje se](sql-database-temporal-tables.md) |
| Transakce | [Podporuje se](https://docs.microsoft.com/sql/t-sql/language-elements/transactions-transact-sql) | [Podporuje se](https://docs.microsoft.com/sql/t-sql/language-elements/transactions-transact-sql) |
| Proměnné | [Podporuje se](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | [Podporuje se](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | 
| Transparentní šifrování dat (TDE)  | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | [Podporuje se](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) |
| Clustering převzetí služeb při selhání ve Windows Serveru | [Podporuje se](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | Nepodporuje se – viz [Aktivní geografická replikace](sql-database-geo-replication-overview.md) |
| Indexy XML | [Podporuje se](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | [Podporuje se](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) |

## <a name="next-steps"></a>Další kroky

- Informace o službě Azure SQL Database najdete v tématu [Co je SQL Database?](sql-database-technical-overview.md)
- Informace o podpoře a odlišnostech jazyka Transact-SQL najdete v tématu [řešícím odlišnosti jazyka Transact-SQL během migrace na službu SQL Database](sql-database-transact-sql-information.md).

