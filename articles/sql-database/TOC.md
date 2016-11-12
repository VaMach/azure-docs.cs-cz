# Přehled
## [Co je SQL Database?](sql-database-technical-overview.md)
## Funkce
### [Úrovně služeb](sql-database-service-tiers.md)
### [Co je DTU?](sql-database-what-is-a-dtu.md)
### [Přehled srovnávacích testů DTU](sql-database-benchmark-overview.md)
### [Brána firewall a pravidla brány firewall služby SQL Database](sql-database-firewall-configure.md)
### [Nástroje pro správu](sql-database-manage-overview.md)
## Požadavky a omezení
### [Srovnání SQL Database a SQL na virtuálním počítači](sql-database-paas-vs-sql-server-iaas.md)
### [Rozdíly v jazyce T-SQL](sql-database-transact-sql-information.md)
### [Omezení prostředků](sql-database-resource-limits.md)
### [Obecná omezení](sql-database-general-limitations.md)
## [Ceny](https://azure.microsoft.com/pricing/details/sql-database/)
## [Co je nového?](https://azure.microsoft.com/updates/?service=sql-database)
## [SQL Database – nejčastější dotazy](sql-database-faq.md)

## Výhody
### [Učení a adaptace](sql-database-learn-and-adapt.md)
### [Průběžné škálování](sql-database-scale-on-the-fly.md)
### [Vytváření víceklientských aplikací](sql-database-build-multi-tenant-apps.md)
### [Zabezpečení a ochrana](sql-database-helps-secures-and-protects.md)
### [Fungování ve vašem prostředí](sql-database-works-in-your-environment.md)

## Scénáře
### Vytvoření a správa serverů, fondů, databází a bran firewall
#### Vytvoření a správa fondů elastické databáze
#### [Kdy použít fond elastické databáze](sql-database-elastic-pool-guidance.md)
#### [Pokyny pro zabezpečení](sql-database-security-guidelines.md)
#### [Fondy elastické databáze](sql-database-elastic-pool.md)
#### [Automation](sql-database-manage-automation.md)
#### Úprava úrovně služby a úrovně výkonu
##### [Azure [Portal](sql-database-scale-up.md)
##### [PowerShell](sql-database-scale-up-powershell.md)
### Vytvoření a správa databází s horizontálním navýšením kapacity
#### [Přehled](sql-database-elastic-scale-introduction.md)
#### [Vytváření škálovatelných cloudových databází](sql-database-elastic-database-client-library.md)
#### [Horizontální navýšení kapacity pomocí správce mapování horizontálních oddílů](sql-database-elastic-scale-shard-map-management.md)
#### [Migrace existujících databází pro horizontální navýšení kapacity](sql-database-elastic-convert-to-use-elastic-tools.md)
#### [Přesun dat mezi cloudovými databázemi s horizontálním navýšením kapacity](sql-database-elastic-scale-overview-split-and-merge.md)
#### [Použití rozhraní Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
#### [Práce s Dapperem](sql-database-elastic-scale-working-with-dapper.md)
#### [Čítače výkonu pro správce mapování horizontálních oddílů](sql-database-elastic-database-perf-counters.md)
#### Úlohy a dotazy napříč databázemi
##### [Dotazování napříč databázemi](sql-database-elastic-query-overview.md)
##### [Dotazování napříč databázemi s různými schématy](sql-database-elastic-query-vertical-partitioning.md)
##### [Generování sestav napříč databázemi](sql-database-elastic-query-horizontal-partitioning.md)
##### [Úlohy napříč databázemi](sql-database-elastic-jobs-overview.md)
##### [Směrování závislé na datech](sql-database-elastic-scale-data-dependent-routing.md)
##### [Dotazování více horizontálních oddílů](sql-database-elastic-scale-multishard-querying.md)
##### [Víceklientské zabezpečení na úrovni řádků](sql-database-elastic-tools-multi-tenant-row-level-security.md)
##### [Distribuované transakce v cloudových databázích](sql-database-elastic-transactions-overview.md)
####[Glosář nástrojů elastické databáze](sql-database-elastic-scale-glossary.md)
#### [Nejčastější dotazy](sql-database-elastic-scale-faq.md)
### Vytvoření a správa přístupu a oprávnění
#### [Přehled](sql-database-security.md)
#### [Pokyny pro zabezpečení](sql-database-security-guidelines.md)
#### [Správa přihlašování](sql-database-manage-logins.md)
#### [Azure Security Center pro službu Azure SQL Database](https://azure.microsoft.com/documentation/articles/security-center-sql-database/)
#### [SQL Security Center](https://msdn.microsoft.com/library/azure/bb510589)
### [Databáze a vývoj aplikací](sql-database-develop-overview.md)
### Migrace databáze
#### [Migrace databáze SQL Serveru](sql-database-cloud-migrate.md)
### Zajištění kontinuity podnikových procesů
#### [Přehled](sql-database-business-continuity.md)
#### [Zálohování databází](sql-database-automated-backups.md) 
#### [Obnovení databází pomocí záloh](sql-database-recovery-using-backups.md)
#### [Zotavení z výpadku datového centra](sql-database-disaster-recovery.md)
#### [Požadavky na ověřování pro zotavení po havárii](sql-database-geo-replication-security-config.md)
#### [Scénáře návrhu pro kontinuitu podnikových procesů](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
#### [Strategie zotavení po havárii s elastickými fondy](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
#### [Postupné upgrady](sql-database-manage-application-rolling-upgrade.md)
### Monitorování a ladění databází
#### [Izolované databáze](sql-database-single-database-monitor.md)
#### [Pokyny pro izolovanou databázi](sql-database-performance-guidance.md)
#### [Přehledy úloh na webu Azure Portal](sql-database-performance.md)

## Implementace zákazníků
### [Software Daxko/CSI](sql-database-implementation-daxko.md)
### [GEP](sql-database-implementation-gep.md)
### [SnelStart](sql-database-implementation-snelstart.md)
### [Umbraco](sql-database-implementation-umbraco.md)

# Začínáme
## Vytvoření serverů, fondů, databází a bran firewall
### [Azure Portal](sql-database-get-started.md)
### [PowerShell](sql-database-get-started-powershell.md)
### [C#](sql-database-get-started-csharp.md)
## Dotazování dat
### [SQL Server Management Studio](sql-database-connect-query-ssms.md)
## Správa serverů, fondů, databází a bran firewall
### [Azure Portal](sql-database-manage-portal.md)
### [SQL Server Management Studio](sql-database-manage-azure-ssms.md)
### [PowerShell](sql-database-manage-powershell.md)
## [Vytvoření a správa přístupu a oprávnění](sql-database-get-started-security.md)
## Zabezpečení a ochrana dat
### [Auditování](sql-database-auditing-get-started.md)
### [Detekce hrozeb](sql-database-threat-detection-get-started.md)
### Dynamické maskování dat
#### [Azure Portal](sql-database-dynamic-data-masking-get-started-portal.md)
## Vytvoření a správa databází s horizontálním navýšením kapacity
### [Elastické škálování](sql-database-elastic-scale-get-started.md)
### [Elastické úlohy](sql-database-elastic-jobs-getting-started.md)
### Elastické dotazy
### [Sestavy napříč databázemi](sql-database-elastic-query-getting-started.md)
### [Dotazy napříč databázemi](sql-database-elastic-query-getting-started-vertical.md)
## [Optimalizace v paměti](sql-database-in-memory.md)
## [Přesouvání databází](sql-database-troubleshoot-moving-data.md)
## [Synchronizace dat](sql-database-get-started-sql-data-sync.md)
##Monitorování a ladění databází
### [Přehled služby SQL Database Advisor](sql-database-advisor.md)
### [Přehledy úloh na webu Azure Portal](sql-database-performance.md)
## [Rychlé zprovoznění řešení](sql-database-solution-quick-starts.md)
# Postup
## [Auditování](sql-database-auditing-get-started.md)
## Vytvoření
### Skupina prostředků
#### [PowerShell](sql-database-manage-powershell.md#create-a-resource-group)
### Servery
#### [Azure Portal](sql-database-get-started.md)
#### [C#](sql-database-get-started-csharp.md)
#### [PowerShell](sql-database-manage-powershell.md#create-a-sql-database-server)
### Fondy elastické databáze
#### [Azure Portal](sql-database-elastic-pool-create-portal.md)
#### [C#](sql-database-elastic-pool-create-csharp.md)
#### [PowerShell](sql-database-elastic-pool-create-powershell.md)
### Databáze
#### Izolované databáze
##### [Azure Portal](sql-database-get-started.md)
##### [C#](sql-database-get-started-csharp.md)
##### [T-SQL](sql-database-manage-azure-ssms.md#create-and-manage-azure-sql-databases)
##### [PowerShell](sql-database-get-started-powershell.md)
#### [SQL Server Management Studio](sql-database-manage-azure-ssms.md#create-and-manage-azure-sql-databases)
#### Horizontálně dělené databáze
##### [Použití správce mapování horizontálních oddílů](sql-database-elastic-scale-shard-map-management.md)
##### [Konfigurace zabezpečení dělení a slučování](sql-database-elastic-scale-split-merge-security-configuration.md)
##### [Práce s Dapperem](sql-database-elastic-scale-working-with-dapper.md)
##### [Použití rozhraní Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
##### [Směrování závislé na datech](sql-database-elastic-scale-data-dependent-routing.md)
##### [Víceklientské zabezpečení na úrovni řádků](sql-database-elastic-tools-multi-tenant-row-level-security.md)
### Pravidla brány firewall
#### Server
##### [Azure Portal](sql-database-configure-firewall-settings.md)
##### [PowerShell](sql-database-configure-firewall-settings-powershell.md)
##### [REST API](sql-database-configure-firewall-settings-rest.md)
##### [T-SQL](sql-database-configure-firewall-settings-tsql.md#server-level-firewall-rules)
#### Databáze
##### [T-SQL](sql-database-configure-firewall-settings-tsql.md#database-level-firewall-rules)
### Úlohy
#### [Instalace služby](sql-database-elastic-jobs-service-installation.md)
#### [Azure Portal](sql-database-elastic-jobs-create-and-manage.md)
#### [PowerShell](sql-database-elastic-jobs-powershell.md)
### Přihlášení
#### [T-SQL](sql-database-manage-azure-ssms.md#create-and-manage-logins.md)
## Vývoj
### [Přehled](https://msdn.microsoft.com/library/mt763826.aspx)
### Scénáře
#### [Víceklientské aplikace SaaS](sql-database-design-patterns-multi-tenancy-saas-applications.md)
#### Dočasné tabulky
##### [Dočasné tabulky](sql-database-temporal-tables.md)
##### [Zásady uchovávání informací](sql-database-temporal-tables-retention-policy.md)
#### [Data JSON](sql-database-json-features.md)
#### [V paměti](sql-database-in-memory.md)
###Začínáme
#### [Knihovny pro připojení](sql-database-libraries.md)
#### Připojení aplikace
##### [.NET](sql-database-develop-dotnet-simple.md)
##### [Java](sql-database-develop-java-simple.md)
##### [Node.js](sql-database-develop-nodejs-simple.md)
##### [PHP](sql-database-develop-php-simple.md)
##### [Python](sql-database-develop-python-simple.md)
##### [Ruby](sql-database-develop-ruby-simple.md)
##### [Excel](sql-database-connect-excel.md)
#### [Připojení pomocí sady Visual Studio](sql-database-connect-query.md)
### Postup
#### Vytvoření serverů
##### [PowerShell](sql-database-get-started-powershell.md)
##### [C#](sql-database-get-started-csharp.md)
##### Vytvoření elastických fondů
###### [PowerShell](sql-database-elastic-pool-create-powershell.md)
###### [C#](sql-database-elastic-pool-create-csharp.md)
#### Vytvoření databází
##### [PowerShell](sql-database-get-started-powershell.md)
##### [C#](sql-database-get-started-csharp.md)
##### Vytvoření pravidel brány firewall
###### [PowerShell](sql-database-configure-firewall-settings-powershell.md)
###### [REST API](sql-database-configure-firewall-settings-rest.md)
#### Správa serverů, fondů, databází a bran firewall
##### [PowerShell](sql-database-manage-powershell.md)
##### Správa elastických fondů
###### [PowerShell](sql-database-elastic-pool-manage-powershell.md)
###### [C#](sql-database-elastic-pool-manage-csharp.md)
##### [Změna úrovní služby a úrovně výkonu](sql-database-scale-up-powershell.md)
#### Přesun dat
##### [Export databáze do souboru BACPAC](sql-database-export-powershell.md)
##### [Import databáze ze souboru BACPAC](sql-database-import-powershell.md)
##### [Kopírování databáze do jiného umístění Azure](sql-database-copy-powershell.md)
#### [Získání požadovaných hodnot pro ověření aplikace](sql-database-client-id-keys.md)
#### [Elastické úlohy](sql-database-elastic-jobs-powershell.md)
#### Obnovení a zotavení databáze
##### Obnovení odstraněné databáze
###### [PowerShell](sql-database-restore-deleted-database-powershell.md)
##### Obnovení databáze k určitému bodu v čase
###### [PowerShell](sql-database-point-in-time-restore-powershell.md)
##### Geografické obnovení
###### [PowerShell](sql-database-geo-restore-powershell.md)
#### Replikace dat pomocí aktivní geografické replikace
##### Konfigurace
###### [PowerShell](sql-database-geo-replication-powershell.md)
##### Převzetí služeb při selhání
###### [PowerShell](sql-database-geo-replication-failover-powershell.md)
#### [Použití portů nad 1433 pro technologii ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md)
#### [Práce s chybovými zprávami](sql-database-develop-error-messages.md)
#### [Použití dávkování](sql-database-use-batching-to-improve-performance.md)
### Referenční informace
#### [Transact-SQL](https://msdn.microsoft.com/library/azure/bb510741.aspx)
#### [Poskytovatel dat rozhraní .NET Framework pro SQL Server (koncepty)](https://msdn.microsoft.com/library/kb9s9ks0.aspx)
#### [Poskytovatel dat rozhraní .NET Framework pro SQL Server (referenční materiály k rozhraní API)](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)
#### SQL PowerShell
##### [Rutiny služby Azure SQL Database (Správa prostředků)](https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
##### [Rutiny služby Azure SQL Database (Správa služby)](https://msdn.microsoft.com/library/azure/dn546723(v=azure.300\).aspx)
##### [Rutiny SQL Serveru](https://msdn.microsoft.com/library/mt740629.aspx)
#### Rozhraní REST API služby SQL Database
##### [Rozhraní REST API (správa prostředků)](https://msdn.microsoft.com/library/azure/mt420159)
##### [Rozhraní REST API (správa služeb)](https://msdn.microsoft.com/library/azure/dn505719.aspx)
#### Knihovna pro správu služby SQL Database
##### [Referenční informace ke knihovně pro správu SQL Database](https://msdn.microsoft.com/library/azure/mt349017.aspx)
##### [Získání balíčku knihovny pro správu SQL Database](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)
#### Entity Framework
##### [Získání balíčku Entity Framework](https://www.nuget.org/packages/EntityFramework/)
#### [Ovladače SQL Serveru](https://msdn.microsoft.com/library/mt654049.aspx)
##### [ADO.NET](https://msdn.microsoft.com/library/mt657768.aspx)
##### [JDBC](https://msdn.microsoft.com/library/mt484311.aspx)
##### [Node.js](https://msdn.microsoft.com/library/mt652093.aspx)
##### [ODBC](https://msdn.microsoft.com/library/mt654048.aspx)
##### [PHP](https://msdn.microsoft.com/library/dn865013.aspx)
##### [Python](https://msdn.microsoft.com/library/mt652092.aspx)
##### [Ruby](https://msdn.microsoft.com/library/mt691981.aspx)
#### [Azure SDK (ke stažení)](https://www.visualstudio.com/vs/azure-tools/)
#### [Azure SDK (dokumentace)](https://azure.microsoft.com/documentation/articles/dotnet-sdk/)
### Zdroje a prostředky
#### [Nástroje SQL Serveru](https://msdn.microsoft.com/library/mt238365.aspx)
#### [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
#### [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
#### [BCP](https://msdn.microsoft.com/library/ms162802.aspx)
#### [SQLCMD](https://msdn.microsoft.com/library/ms162773.aspx)
#### [SqlPackage](https://msdn.microsoft.com/hh550080.aspx)
## Odstranění
### Databáze
#### [PowerShell](sql-database-manage-powershell.md#delete-a-sql-database)
### Server
#### [PowerShell](sql-database-manage-powershell.md#delete-a-sql-database-server)
## Detekce hrozeb
### [Detekce hrozeb](sql-database-threat-detection-get-started.md)
### [Brána firewall](sql-database-firewall-configure.md)
## Šifrování dat
### Funkce Always Encrypted
#### [Přehled funkce Always Encrypted](sql-database-always-encrypted.md)
#### [Azure Key Vault s funkcí Always Encrypted](sql-database-always-encrypted-azure-key-vault.md)
### [Transparentní šifrování dat](https://msdn.microsoft.com/library/azure/dn948096)
### [Šifrování sloupců](https://msdn.microsoft.com/library/azure/ms179331)
## Spravovat
###  Authentication
#### Ověřování pomocí SQL
#### [Ověřování pomocí Azure Active Directory](sql-database-aad-authentication.md)
#### [Multi-Factor Authentication](sql-database-ssms-mfa-authentication.md)
### Servery
### Elastické fondy
#### [Azure Portal](sql-database-elastic-pool-manage-portal.md)
#### [PowerShell](sql-database-elastic-pool-manage-powershell.md)
#### [T-SQL](sql-database-elastic-pool-manage-tsql.md)
#### [C#](sql-database-elastic-pool-manage-csharp.md)
### Databáze
#### Izolované databáze
##### [Azure Portal](sql-database-manage-portal.md)
##### [T-SQL](sql-database-manage-azure-ssms.md#create-and-manage-azure-sql-databases)
##### [PowerShell](sql-database-manage-powershell.md#create-a-sql-database-blank)
#### Změna úrovní služby a úrovně výkonu
#### [Azure [Portal](sql-database-scale-up.md)
#### [PowerShell](sql-database-manage-powershell.md#change-the-performance-level-of-a-sql-database)
#### Horizontálně dělené databáze
##### [Migrace stávajících škálovaných databází pro horizontální navýšení kapacity](sql-database-elastic-convert-to-use-elastic-tools.md)
##### [Správa přihlašovacích údajů](sql-database-elastic-scale-manage-credentials.md)
##### [Přesun dat mezi cloudovými databázemi s horizontálním navýšením kapacity](sql-database-elastic-scale-overview-split-and-merge.md)
##### [Nasazení služby dělení a slučování](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
##### [Přidání horizontálního oddílu](sql-database-elastic-scale-add-a-shard.md)
##### [Oprava problémů s mapováním horizontálních oddílů pomocí třídy RecoveryManager](sql-database-elastic-database-recovery-manager.md)
### Pravidla brány firewall
#### Server
##### [Azure Portal](sql-database-configure-firewall-settings.md)
##### [PowerShell](sql-database-configure-firewall-settings-powershell.md)
##### [REST API](sql-database-configure-firewall-settings-rest.md)
##### [T-SQL](sql-database-configure-firewall-settings-tsql.md#server-level-firewall-rules)
#### Databáze
##### [T-SQL](sql-database-configure-firewall-settings-tsql.md#database-level-firewall-rules)
### Úlohy
#### [Instalace služby](sql-database-elastic-jobs-service-installation.md)
#### [Azure Portal](sql-database-elastic-jobs-create-and-manage.md)
#### [PowerShell](sql-database-elastic-jobs-powershell.md)
#### [Upgrade klientské knihovny](sql-database-elastic-scale-upgrade-client-library.md)
### Přihlášení
#### [Správa přihlašování](sql-database-manage-logins.md)
## Maskování dat
### Dynamické maskování dat
#### [Azure Portal](sql-database-dynamic-data-masking-get-started-portal.md)
#### [Klienti nižší úrovně](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)
## Migrace
### Určení kompatibility
#### [Nástroj SQL Package](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md)
### Oprava problémů s kompatibilitou
#### [SQL Server Data Tools](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
#### [Průvodce migrací služby SQL Azure](sql-database-cloud-migrate-fix-compatibility-issues.md)
### [Použití průvodce migrací sady SQL Server Management Studio](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
### [Použití transakční replikace](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
### Export databáze do souboru BACPAC
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
#### [Nástroj SQL Package](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)
#### [PowerShell](sql-database-export-powershell.md)
### Import databáze ze souboru BACPAC
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
#### [Nástroj SQL Package](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
#### [Azure Portal](sql-database-import.md)
#### [PowerShell](sql-database-import-powershell.md)

## Monitorování a ladění
### [Izolované databáze](sql-database-performance-guidance.md)
### Elastické fondy
#### [Azure Portal](sql-database-elastic-pool-manage-portal.md)
#### [PowerShell](sql-database-elastic-pool-manage-powershell.md)
#### [T-SQL](sql-database-elastic-pool-manage-tsql.md)
#### [C#](sql-database-elastic-pool-manage-csharp.md)
### [Query Performance Insight](sql-database-query-performance.md)
### SQL Database Advisor
#### [Azure Portal](sql-database-advisor-portal.md)
### Změna úrovní služby a úrovně výkonu
#### [Azure Portal](sql-database-scale-up.md)
#### [PowerShell](sql-database-scale-up-powershell.md)
### [Tipy pro ladění výkonu](sql-database-troubleshoot-performance.md)
### OLTP v paměti
#### [Implementace OLTP v paměti](sql-database-in-memory-oltp-migration.md)
#### [Monitorování úložiště OLTP v paměti](sql-database-in-memory-oltp-monitoring.md)
### Úložiště dotazů
#### [Monitorování výkonu pomocí úložiště dotazů](https://msdn.microsoft.com/library/dn817826.aspx)
#### [Scénáře použití úložiště dotazů](https://msdn.microsoft.com/library/mt614796.aspx)
#### [Provoz úložiště dotazů](sql-database-operate-query-store.md)
### [Úrovně kompatibility](sql-database-compatibility-level-query-performance-130.md)
### [Auditování událostí](sql-database-auditing-get-started.md)
### [Čítače výkonu pro správce mapování horizontálních oddílů](sql-database-elastic-database-perf-counters.md)
### Rozšířené události
#### [Rozšířené události](sql-database-xevent-db-diff-from-svr.md)
#### [Cílový kód souboru události](sql-database-xevent-code-event-file.md)
#### [Cílový kód cyklické vyrovnávací paměti](sql-database-xevent-code-ring-buffer.md)
### Zobrazení dynamických zpráv
#### [Zobrazení dynamické správy](sql-database-monitoring-with-dmvs.md)
#### [Zobrazení dynamických zpráv](sql-database-manage-azure-ssms#monitor-sql-database-using-dynamic-management-views


## Přesun dat
### Kopírování databáze SQL
#### [Přehled](sql-database-copy.md)
#### [Azure Portal](sql-database-copy-portal.md)
#### [PowerShell](sql-database-copy-powershell.md)
#### [T-SQL](sql-database-copy-transact-sql.md)
### Export databáze do souboru BACPAC
#### [Azure Portal](sql-database-export.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
#### [PowerShell](sql-database-export-powershell.md)
### Import databáze ze souboru BACPAC
#### [Azure Portal](sql-database-import.md)
#### [PowerShell](sql-database-import-powershell.md)
### [Synchronizace dat](sql-database-get-started-sql-data-sync.md)
### [Načtení ze souboru CSV pomocí BCP](sql-database-load-from-csv-with-bcp.md)

## Dotaz
### [SQL Server Management Studio](sql-database-connect-query-ssms.md)
### [Dotazování více horizontálních oddílů](sql-database-elastic-scale-multishard-querying.md)
### Dotazy napříč databázemi
#### [Dotazování napříč databázemi s různými schématy](sql-database-elastic-query-vertical-partitioning.md)
#### [Generování sestav napříč databázemi](sql-database-elastic-query-horizontal-partitioning.md)
#### [Distribuované transakce v cloudových databázích](sql-database-elastic-transactions-overview.md)
#### [Upgrade klientské knihovny](sql-database-elastic-scale-upgrade-client-library.md)
#### [Dotazování více horizontálních oddílů](sql-database-elastic-scale-multishard-querying.md)

## Obnovení
### Obnovení odstraněné databáze
### [Azure Portal](sql-database-restore-deleted-database-portal.md)
### [PowerShell](sql-database-restore-deleted-database-powershell.md)
### Obnovení k určitému bodu v čase
#### [Azure Portal](sql-database-point-in-time-restore-portal.md)
#### [PowerShell](sql-database-point-in-time-restore-powershell.md)
### Geografické obnovení
#### [Azure Portal](sql-database-geo-restore-portal.md)
#### [PowerShell](sql-database-geo-restore-powershell.md)
#### [Izolovaná tabulka](sql-database-cloud-migrate-restore-single-table-azure-backup.md)
### [Zotavení z výpadku datového centra](sql-database-disaster-recovery.md)
### [Postup zotavení po havárii](sql-database-disaster-recovery-drills.md)

## Replikace
### [Přehled aktivní geografické replikace](sql-database-geo-replication-overview.md)
### Konfigurace
#### [PowerShell](sql-database-geo-replication-powershell.md)
#### [T-SQL](sql-database-geo-replication-transact-sql.md)
### Převzetí služeb při selhání
#### [Azure Portal](sql-database-geo-replication-failover-portal.md)
#### [PowerShell](sql-database-geo-replication-failover-powershell.md)
#### [T-SQL](sql-database-geo-replication-failover-transact-sql.md)

## Řešení potíží
### Připojení
#### [Problémy s připojením](sql-database-troubleshoot-common-connection-issues.md)
#### [Chyba přechodného připojení](sql-database-troubleshoot-connection.md)
#### [Diagnostika a prevence](sql-database-connectivity-issues.md)
### [Oprávnění](sql-database-troubleshoot-permissions.md)

# Referenční informace
## [T-SQL](https://msdn.microsoft.com/library/azure/bb510741.aspx)
## SQL PowerShell
### [Rutiny služby Azure SQL Database (Správa prostředků)](https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
### [Rutiny SQL Serveru](https://msdn.microsoft.com/library/mt740629.aspx)
## Rozhraní REST API služby SQL Database
### [Rozhraní REST API (správa prostředků)](https://msdn.microsoft.com/library/azure/mt420159)
## Knihovna pro správu služby SQL Database
### [Referenční informace ke knihovně pro správu SQL Database](https://msdn.microsoft.com/library/azure/mt349017.aspx)
### [Získání balíčku knihovny pro správu SQL Database](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)
## [Ovladače SQL Serveru](https://msdn.microsoft.com/library/mt654049.aspx)
### [ADO.NET](https://msdn.microsoft.com/library/mt657768.aspx)
### [JDBC](https://msdn.microsoft.com/library/mt484311.aspx)
### [Node.js](https://msdn.microsoft.com/library/mt652093.aspx)
### [ODBC](https://msdn.microsoft.com/library/mt654048.aspx)
### [PHP](https://msdn.microsoft.com/library/dn865013.aspx)
### [Python](https://msdn.microsoft.com/library/mt652092.aspx)
### [Ruby](https://msdn.microsoft.com/library/mt691981.aspx)

# Zdroje a prostředky
## [Nástroje SQL Serveru](https://msdn.microsoft.com/library/mt238365.aspx)
## [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
## [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
## [BCP](https://msdn.microsoft.com/library/ms162802.aspx)
## [SQLCMD](https://msdn.microsoft.com/library/ms162773.aspx)
## [SqlPackage](https://msdn.microsoft.com/hh550080.aspx)

<!--HONumber=Nov16_HO2-->


