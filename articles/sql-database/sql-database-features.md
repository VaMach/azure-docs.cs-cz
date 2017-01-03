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
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/28/2016
ms.author: carlrab; jognanay
translationtype: Human Translation
ms.sourcegitcommit: 75bf523679c8d8ad6fbe4a8aa8a561d03008e59b
ms.openlocfilehash: c7cb4b872723e07b720c87ecef9d9d8f136c25eb


---
# <a name="azure-sql-database-features"></a>Funkce služby Azure SQL Database
V tomto tématu je k dispozici přehled logických serverů a databází Azure SQL Database a matice na podporu funkcí s odkazy na všechny uvedené funkce. 

## <a name="what-is-an-azure-sql-database-logical-server"></a>Co je logický server Azure SQL Database?
Logický server Azure SQL Database funguje jako centrální bod pro správu více databází. Ve službě SQL Database je server logická konstrukce, která se liší od instance SQL Serveru, kterou můžete znát z místního prostředí. Služba SQL Database zejména neposkytuje žádnou záruku ohledně umístění databází ve vztahu k jejich logickým serverům a nezveřejňuje žádné funkce ani možnosti přístupu na úrovni instance. Další informace o logických SQL serverech Azure najdete v tématu [Logické servery](sql-database-server-overview.md). 

## <a name="what-is-an-azure-sql-database"></a>Co je služba Azure SQL Database?
Každá databáze ve službě Azure SQL Database je přidružená k logickému serveru. Databáze může být:

- Izolovaná databáze s [vlastní sadou prostředků](sql-database-what-is-a-dtu.md#what-are-database-transaction-units-dtus) (DTU)
- Součástí [fondu databází](sql-database-elastic-pool.md), které [sdílejí sadu prostředků](sql-database-what-is-a-dtu.md#what-are-elastic-database-transaction-units-edtus) (eDTU)
- Součástí [sady horizontálně dělených databází s horizontálním navýšením kapacity](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling); tyto databáze můžou být buď izolované, nebo součástí fondu
- Součástí sady databází, které se podílejí na [vzoru návrhu SaaS pro více klientů](sql-database-design-patterns-multi-tenancy-saas-applications.md); tyto databáze můžou být buď izolované, nebo součástí fondu (nebo obojí) 

Další informace o databázích SQL Azure najdete v tématu [Databáze SQL](sql-database-overview.md).

## <a name="what-features-are-supported"></a>Jaké funkce jsou podporovány?

Následující tabulky uvádějí hlavní funkce služby Azure SQL Database a systému SQL Server, specifikují možnosti podpory a obsahují odkaz na další informace o funkci jednotlivých platforem. Funkce jazyka Transact-SQL zobrazíte pomocí následujícího odkazu v tabulce pro kategorii funkce. Přečtěte si také téma [Odlišnosti jazyka Transact-SQL ve službě Azure SQL Database](sql-database-transact-sql-information.md), kde najdete další informace o důvodech nedostatečné podpory určitých typů zařízení.

Pokračujeme v přidávání funkcí do V12. Doporučujeme vám proto navštívit webovou stránku aktualizace služeb pro Azure a použít filtry, které obsahuje:

* Filtrování na [službu SQL Database](https://azure.microsoft.com/updates/?service=sql-database)
* Filtrování na [oznámení všeobecné dostupnosti ](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) pro funkce služby SQL Database

> [!TIP]
> Pokud chcete otestovat stávající databázi na kompatibilitu s funkcí Azure SQL Database, získáte informace v části [Ověření kompatibility služby Azure SQL Database](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md).
>

| **Funkce** | **SQL Server** | **Azure SQL Database** | 
| --- | :---: | :---: | 
| Aktivní geografická replikace | Nepodporuje se – viz [Skupiny dostupnosti AlwaysOn](https://msdn.microsoft.com/library/hh510230.aspx) | [Podporuje se](sql-database-geo-replication-overview.md)
| Funkce Always Encrypted | [Podporuje se](https://msdn.microsoft.com/library/mt163865.aspx) | [Podporuje se](sql-database-always-encrypted.md) |
| Skupiny dostupnosti AlwaysOn | [Podporuje se](https://msdn.microsoft.com/library/hh510230.aspx) | Nepodporuje se – viz [Aktivní geografická replikace](sql-database-geo-replication-overview.md) |
| Připojení databáze | [Podporuje se](https://msdn.microsoft.com/library/ms190209.aspx) | Nepodporuje se |
| Aplikační role | [Podporuje se](https://msdn.microsoft.com/library/ms190998.aspx) | [Podporuje se](https://msdn.microsoft.com/library/ms190998.aspx) |
| Automatické škálování | Nepodporuje se | [Podporuje se](sql-database-scale-up.md) |
| Azure Active Directory | Nepodporuje se | [Podporuje se](sql-database-aad-authentication.md) |
| Azure Data Factory | Nepodporuje se – viz [SQL Server Integration Services (SSIS)](https://msdn.microsoft.com/library/ms141026.aspx) | [Podporuje se](https://azure.microsoft.com/services/data-factory/) |
| Auditování | [Podporuje se](https://msdn.microsoft.com/library/cc280386.aspx) | [Podporuje se](sql-database-auditing-get-started.md) |
| Soubor BACPAC (export) | [Podporuje se](https://msdn.microsoft.com/library/hh213241.aspx) | [Podporuje se](sql-database-export.md) |
| Soubor BACPAC (import) | [Podporuje se](https://msdn.microsoft.com/library/hh710052.aspx) | [Podporuje se](sql-database-import.md) |
| Příkazy BACKUP a RESTORE | [Podporuje se](https://msdn.microsoft.com/library/ff848768.aspx) | Nepodporuje se |
| Integrované funkce | [Podporuje se](https://msdn.microsoft.com/library/ms174318.aspx) | [Většina](https://msdn.microsoft.com/library/ms174318.aspx) |
| Change data capture | [Podporuje se](https://msdn.microsoft.com/library/cc645937.aspx) | Nepodporuje se |
| Sledování změn | [Podporuje se](https://msdn.microsoft.com/library/bb933875.aspx) | [Podporuje se](https://msdn.microsoft.com/library/bb933875.aspx) |
| Příkazy kolace | [Podporuje se](https://msdn.microsoft.com/library/ff848763.aspx) | [Podporuje se](https://msdn.microsoft.com/library/ff848763.aspx) |
| Indexy Columnstore | [Podporuje se](https://msdn.microsoft.com/library/gg492088.aspx) | [Jenom verze Premium Edition](https://msdn.microsoft.com/library/gg492088.aspx) |
| Common language runtime (CLR) | [Podporuje se](https://msdn.microsoft.com/library/ms131102.aspx) | Nepodporuje se |
| Databáze s omezením | [Podporuje se](https://msdn.microsoft.com/library/ff929071.aspx) | Integrované |
| Uživatelé s omezením | [Podporuje se](https://msdn.microsoft.com/library/ff929188.aspx) | [Podporuje se](sql-database-manage-logins.md#non-administrator-users) |
| Kontrola klíčových slov toku jazyka | [Podporuje se](https://msdn.microsoft.com/library/ms174290.aspx) | [Podporuje se](https://msdn.microsoft.com/library/ms174290.aspx) |
| Mezidatabázové dotazy | [Podporuje se](https://msdn.microsoft.com/library/dn584627.aspx) | [Elastické dotazy](sql-database-elastic-query-overview.md) |
| Kurzory | [Podporuje se](https://msdn.microsoft.com/library/ms181441.aspx) | [Podporuje se](https://msdn.microsoft.com/library/ms181441.aspx) | 
| Komprese dat | [Podporuje se](https://msdn.microsoft.com/library/cc280449.aspx) | [Podporuje se](https://msdn.microsoft.com/library/cc280449.aspx) |
| Zálohování databáze | [Vystavené pro uživatele](https://msdn.microsoft.com/library/ms187048.aspx) | [Integrované](sql-database-automated-backups.md) |
| Databázový e-mail | [Podporuje se](https://msdn.microsoft.com/library/ms189635.aspx) | Nepodporuje se |
| Zrcadlení databáze | [Podporuje se](https://msdn.microsoft.com/library/ms189852.aspx) | Nepodporuje se |
| Možnosti konfigurace databáze | [Podporuje se](https://msdn.microsoft.com/library/mt629158.aspx) | [Podporuje se](https://msdn.microsoft.com/library/mt629158.aspx) |
| Data Quality Services (DQS) | [Podporuje se](https://msdn.microsoft.com/library/ff877925.aspx) | Nepodporuje se |
| Snímky databáze | [Podporuje se](https://msdn.microsoft.com/library/ms175158.aspx) | Nepodporuje se |
| Typy dat | [Podporuje se](https://msdn.microsoft.com/library/ms187752.aspx) | [Podporuje se](https://msdn.microsoft.com/library/ms187752.aspx) |  
| Příkazy DBCC | [Všechny](https://msdn.microsoft.com/library/ms188796.aspx) | [Některé](https://msdn.microsoft.com/library/ms188796.aspx) |
| Příkazy DDL | [Podporuje se](https://msdn.microsoft.com/library/ff848799.aspx) | [Většina](https://msdn.microsoft.com/library/ff848799.aspx)
| Triggery DDS | [Podporuje se](https://msdn.microsoft.com/library/ms175941.aspx) | [Jenom databáze](https://msdn.microsoft.com/library/ms175941.aspx) |
| Distribuované transakce | [MS DTC](https://msdn.microsoft.com/library/ms131665.aspx) | Jenom omezené scénáře v rámci služby SQL Database |
| Příkazy DML | [Podporuje se](https://msdn.microsoft.com/library/ff848766.aspx) | [Většina](https://msdn.microsoft.com/library/ff848766.aspx) |
| Triggery DML | [Podporuje se](https://msdn.microsoft.com/library/ms178110.aspx) | [Podporuje se](https://msdn.microsoft.com/library/ms178110.aspx) |
| Zobrazení dynamických zpráv | [Všechny](https://msdn.microsoft.com/library/ms188754.aspx) | [Některé](https://msdn.microsoft.com/library/ms188754.aspx) |
| elastické fondy | Nepodporuje se | [Podporuje se](sql-database-elastic-pool.md) |
| Elastické úlohy | Nepodporuje se – viz [Agent systému SQL Server](https://msdn.microsoft.com/library/ms189237.aspx) | [Podporuje se](sql-database-elastic-jobs-getting-started.md) | 
| Elastické dotazy | Nepodporuje se – viz [Mezidatabázové dotazy](https://msdn.microsoft.com/library/dn584627.aspx) | [Podporuje se](sql-database-elastic-query-overview.md) |
| Oznámení událostí | [Podporuje se](https://msdn.microsoft.com/library/ms186376.aspx) | [Podporuje se](sql-database-insights-alerts-portal.md) |
| Výrazy | [Podporuje se](https://msdn.microsoft.com/library/ms190286.aspx) | [Podporuje se](https://msdn.microsoft.com/library/ms190286.aspx) |
| Rozšířené události | [Podporuje se](https://msdn.microsoft.com/library/bb630282.aspx) | [Některé](sql-database-xevent-db-diff-from-svr.md) |
| Rozšířené uložené procedury | [Podporuje se](https://msdn.microsoft.com/library/ms164627.aspx) | Nepodporuje se |
| Skupiny souborů | [Podporuje se](https://msdn.microsoft.com/library/ms189563.aspx#Anchor_2) | [Jenom primární](https://msdn.microsoft.com/library/ms189563.aspx#Anchor_2) |
| FileStream | [Podporuje se](https://msdn.microsoft.com/library/gg471497.aspx) | Nepodporuje se |
| Fulltextové vyhledávání | [Podporuje se](https://msdn.microsoft.com/library/ms142571.aspx) | [Nepodporované moduly pro dělení na slova třetích stran](https://msdn.microsoft.com/library/ms142571.aspx) |
| Funkce | [Podporuje se](https://msdn.microsoft.com/library/ms174318.aspx) | [Většina](https://msdn.microsoft.com/library/ms174318.aspx) |
| Optimalizace v paměti | [Podporuje se](https://msdn.microsoft.com/library/dn133186.aspx) | [Jenom verze Premium Edition](https://msdn.microsoft.com/library/dn133186.aspx) |
| Úlohy | [Agent SQL Server](https://msdn.microsoft.com/library/ms189237.aspx) | [Podporuje se](sql-database-elastic-jobs-getting-started.md) |
| Podpora dat JSON | [Podporuje se](https://msdn.microsoft.com/library/dn921897.aspx) | [Podporuje se](sql-database-json-features.md) |
| Elementy jazyka | [Podporuje se](https://msdn.microsoft.com/library/ff848807.aspx) | [Většina](https://msdn.microsoft.com/library/ff848807.aspx) |  
| Propojené servery | [Podporuje se](https://msdn.microsoft.com/library/ms188279.aspx) | Nepodporuje se – viz [Elastický dotaz](sql-database-elastic-query-horizontal-partitioning.md) |
| Přesouvání protokolu | [Podporuje se](https://msdn.microsoft.com/library/ms187103.aspx) | Nepodporuje se – viz [Aktivní geografická replikace](sql-database-geo-replication-overview.md) |
| Příkazy pro správu | [Podporuje se](https://msdn.microsoft.com/library/ms190286.aspx)| [Nepodporuje se](https://msdn.microsoft.com/library/ms190286.aspx) |
| Master Data Services (MDS) | [Podporuje se](https://msdn.microsoft.com/library/ff487003.aspx) | Nepodporuje se |
| Minimální protokolování v hromadném importu | [Podporuje se](https://msdn.microsoft.com/library/ms190422.aspx) | Nepodporuje se |
| Úprava dat systému | [Podporuje se](https://msdn.microsoft.com/library/ms178028.aspx) | Nepodporuje se |
| Online indexovací operace | [Podporuje se](https://msdn.microsoft.com/library/ms177442.aspx) | [Velikost transakce omezená podle úrovně služeb](https://msdn.microsoft.com/library/ms177442.aspx) |
| Operátory | [Podporuje se](https://msdn.microsoft.com/library/ms174986.aspx) | [Většina](https://msdn.microsoft.com/library/ms174986.aspx) |
| Obnovení databáze k určitému bodu v čase | [Podporuje se](https://msdn.microsoft.com/library/ms179451.aspx) | [Podporuje se](sql-database-recovery-using-backups.md#point-in-time-restore) |
| Polybase | [Podporuje se](https://msdn.microsoft.com/library/mt143171.aspx) | [Nepodporuje se]
| Správa na základě zásad | [Podporuje se](https://msdn.microsoft.com/library/bb510667.aspx) | Nepodporuje se |
| Predikáty | [Podporuje se](https://msdn.microsoft.com/library/ms189523.aspx) | [Většina](https://msdn.microsoft.com/library/ms189523.aspx)
| Správce prostředků | [Podporuje se](https://msdn.microsoft.com/library/bb933866.aspx) | [Integrované](sql-database-service-tiers.md) |
| Obnovení databáze ze zálohy | [Podporuje se](https://msdn.microsoft.com/library/ms187048.aspx#anchor_6) | [Jenom z integrovaných záloh](sql-database-recovery-using-backups.md) |
| Zabezpečení na úrovni řádků | [Podporuje se](https://msdn.microsoft.com/library/dn765131.aspx) | [Podporuje se](https://msdn.microsoft.com/library/dn765131.aspx) |
| Příkazy zabezpečení | [Podporuje se](https://msdn.microsoft.com/library/ff848791.aspx) | [Některé](https://msdn.microsoft.com/library/ff848791.aspx) |
| Sémantické vyhledávání | [Podporuje se](https://msdn.microsoft.com/library/gg492075.aspx) | Nepodporuje se |
| Pořadová čísla | [Podporuje se](https://msdn.microsoft.com/library/ff878058.aspx) | [Podporuje se](https://msdn.microsoft.com/library/ff878058.aspx) |
| Service Broker | [Podporuje se](https://msdn.microsoft.com/library/bb522893.aspx) | Nepodporuje se |
| Možnosti konfigurace serveru | [Podporuje se](https://msdn.microsoft.com/library/ms189631.aspx) | Nepodporuje se – viz [Možnosti konfigurace databáze](https://msdn.microsoft.com/library/mt629158.aspx) |
| Příkazy Set | [Podporuje se](https://msdn.microsoft.com/library/ms190356.aspx) | [Většina](https://msdn.microsoft.com/library/ms190356.aspx) 
| Spatial | [Podporuje se](https://msdn.microsoft.com/library/bb933790.aspx) | [Podporuje se](https://msdn.microsoft.com/library/bb933790.aspx) |
| Agent SQL Server | [Podporuje se](https://msdn.microsoft.com/library/ms189237.aspx) | Nepodporuje se – viz [Elastické úlohy](sql-database-elastic-jobs-getting-started.md) |
| Služba Analysis Services serveru SQL (SSAS) | [Podporuje se](https://msdn.microsoft.com/library/bb522607.aspx) | Nepodporuje se – viz [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| SQL Server Integration Services (SSIS) | [Podporuje se](https://msdn.microsoft.com/library/ms141026.aspx) | Nepodporuje se – viz [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) |
| PowerShell pro SQL Server | [Podporuje se](https://msdn.microsoft.com/library/hh245198.aspx) | [Podporuje se](https://msdn.microsoft.com/library/hh245198.aspx) |
| SQL Server Profiler | [Podporuje se](https://msdn.microsoft.com/library/ms181091.aspx) | Nepodporuje se – viz [Rozšířené události](https://msdn.microsoft.com/library/ms181091.aspx) |
| Replikace systému SQL Server | [Podporuje se](https://msdn.microsoft.com/library/ms151198.aspx) | [Jenom předplatitelé transakční replikace a replikace snímků](sql-database-cloud-migrate-compatible-using-transactional-replication.md) |
| SQL Server Reporting Services (SSRS) | [Podporuje se](https://msdn.microsoft.com/library/ms159106.aspx) | Nepodporuje se |
| Uložené procedury | [Podporuje se](https://msdn.microsoft.com/library/ms190782.aspx) | [Podporuje se](https://msdn.microsoft.com/library/ms190782.aspx) |
| Systémové uložené funkce | [Podporuje se](https://msdn.microsoft.com/library/ff848780.aspx) | [Některé](https://msdn.microsoft.com/library/ff848780.aspx) |
| Systémové uložené procedury | [Podporuje se](https://msdn.microsoft.com/library/ms187961.aspx) | [Některé](https://msdn.microsoft.com/library/ms187961.aspx) |
| Systémové tabulky | [Podporuje se](https://msdn.microsoft.com/library/ms179932.aspx) | [Některé](https://msdn.microsoft.com/library/ms179932.aspx) |
| Systémová zobrazení | [Podporuje se](https://msdn.microsoft.com/library/ms177862.aspx) | [Některé](https://msdn.microsoft.com/library/ms177862.aspx)
| Dělení tabulky | [Podporuje se](https://msdn.microsoft.com/library/ms190787.aspx) | [Jenom primární skupinu souborů](https://msdn.microsoft.com/library/ms190787.aspx) |
| Dočasné tabulky | [Místní a globální](https://msdn.microsoft.com/library/ms174979.aspx#Anchor_4) | [Jenom místní](https://msdn.microsoft.com/library/ms174979.aspx#Anchor_4) |
| Dočasné tabulky | [Podporuje se](https://msdn.microsoft.com/library/dn935015.aspx) | [Podporuje se](sql-database-temporal-tables.md) |
| Transakční příkazy | [Podporuje se](https://msdn.microsoft.com/library/ms174377.aspx) | [Podporuje se](https://msdn.microsoft.com/library/ms174377.aspx) |
| Proměnné | [Podporuje se](https://msdn.microsoft.com/library/ff848809.aspx) | | [Podporuje se](https://msdn.microsoft.com/library/ff848809.aspx) | 
| Transparentní šifrování dat (TDE)  | [Podporuje se](https://msdn.microsoft.com/library/bb934049.aspx) | [Podporuje se](https://msdn.microsoft.com/dn948096.aspx) |
| Clustering převzetí služeb při selhání ve Windows Serveru | [Podporuje se](https://msdn.microsoft.com/library/hh270278.aspx) | Nepodporuje se – viz [Aktivní geografická replikace](sql-database-geo-replication-overview.md) |
| Indexy XML | [Podporuje se](http://msdn.microsoft.com/library/bb934097.aspx) | [Podporuje se](http://msdn.microsoft.com/library/bb934097.aspx) |
| Příkazy XML | [Podporuje se](https://msdn.microsoft.com/library/ff848798.aspx) | [Podporuje se](https://msdn.microsoft.com/library/ff848798.aspx) |

## <a name="next-steps"></a>Další kroky

- Informace o službě Azure SQL Database najdete v tématu [Co je SQL Database?](sql-database-technical-overview.md)
- Přehled logických SQL serverů Azure najdete v tématu [Přehled logického serveru SQL Database](sql-database-server-overview.md).
- Přehled databází SQL Azure najdete v tématu [Přehled služby SQL Database](sql-database-overview.md).
- Informace o podpoře a odlišnostech jazyka Transact-SQL najdete v tématu [Odlišnosti jazyka Transact-SQL ve službě Azure SQL Database](sql-database-transact-sql-information.md).
- Informace o konkrétních kvótách a omezeních prostředků závisí na vaší **úrovni služeb**. Přehled úrovní služeb najdete v tématu [Úrovně služeb SQL Database](sql-database-service-tiers.md).
- Pokyny pro zabezpečení najdete v tématu [Omezení a pokyny pro zabezpečení služby Azure SQL Database](sql-database-security-guidelines.md).
- Informace o dostupnosti a podpoře ovladačů pro službu SQL Database najdete v tématu [Knihovny pro připojení ke službě SQL Database a serveru SQL Server](sql-database-libraries.md).



<!--HONumber=Dec16_HO3-->


