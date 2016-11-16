# Přehled
## [Co je SQL Database?](sql-database-technical-overview.md)
## [SQL Database – nejčastější dotazy](sql-database-faq.md)
## Funkce
### [Úrovně služeb](sql-database-service-tiers.md)
### [Jednotky databázové transakce](sql-database-what-is-a-dtu.md)
### [Přehled srovnávacích testů DTU](sql-database-benchmark-overview.md)
### [Nástroje pro správu](sql-database-manage-overview.md)
## Požadavky a omezení
### [Srovnání SQL Database a SQL na virtuálním počítači](sql-database-paas-vs-sql-server-iaas.md)
### [Rozdíly v jazyce T-SQL](sql-database-transact-sql-information.md)
### [Omezení prostředků](sql-database-resource-limits.md)
### [Obecná omezení](sql-database-general-limitations.md)
### [Pokyny pro zabezpečení](sql-database-security-guidelines.md)

## Výhody
### [Učení a adaptace](sql-database-learn-and-adapt.md)
### [Průběžné škálování](sql-database-scale-on-the-fly.md)
### [Vytváření víceklientských aplikací](sql-database-build-multi-tenant-apps.md)
### [Zabezpečení a ochrana](sql-database-helps-secures-and-protects.md)
### [Fungování ve vašem prostředí](sql-database-works-in-your-environment.md)

## Scénáře

### Servery, fondy, databáze a brány firewall
#### [Kdy použít fond elastické databáze](sql-database-elastic-pool-guidance.md)
#### [Fondy elastické databáze](sql-database-elastic-pool.md)
#### [Automation](sql-database-manage-automation.md)

### Databáze s horizontálním navýšením kapacity
#### [Přehled](sql-database-elastic-scale-introduction.md)
#### [Vytvoření škálovatelných cloudových databází](sql-database-elastic-database-client-library.md)
#### [Mezidatabázové úlohy](sql-database-elastic-jobs-overview.md)
#### [Glosář nástrojů elastické databáze](sql-database-elastic-scale-glossary.md)
#### [Nejčastější dotazy](sql-database-elastic-scale-faq.md)

### Přístup a oprávnění
#### [Přehled](sql-database-security.md)
#### [Azure Security Center pro službu Azure SQL Database](../security-center/security-center-sql-database.md?toc=%2fazure%2fsql-database%2ftoc.json)
#### [SQL Security Center](https://msdn.microsoft.com/library/azure/bb510589)

### Kontinuita podnikových procesů
#### [Přehled](sql-database-business-continuity.md)
#### [Zálohování databází](sql-database-automated-backups.md)
#### [Obnovení databází pomocí záloh](sql-database-recovery-using-backups.md)
#### [Požadavky na ověřování pro zotavení po havárii](sql-database-geo-replication-security-config.md)
#### [Scénáře návrhu pro kontinuitu podnikových procesů](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
#### [Strategie zotavení po havárii s elastickými fondy](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
#### [Postupné upgrady](sql-database-manage-application-rolling-upgrade.md)

### [Nasazení databáze](sql-database-develop-overview.md)
### [Migrace databází SQL Serveru](sql-database-cloud-migrate.md)

## Implementace zákazníků
### [Software Daxko/CSI](sql-database-implementation-daxko.md)
### [GEP](sql-database-implementation-gep.md)
### [SnelStart](sql-database-implementation-snelstart.md)
### [Umbraco](sql-database-implementation-umbraco.md)


# Začínáme
## [Rychlé zprovoznění řešení](sql-database-solution-quick-starts.md)
## Vytvoření databáze SQL
### [Azure Portal](sql-database-get-started.md)
### [C#](sql-database-get-started-csharp.md)
### [PowerShell](sql-database-get-started-powershell.md)
## Vytvoření a správa databází s horizontálním navýšením kapacity
### [Elastické škálování](sql-database-elastic-scale-get-started.md)
### [Elastické úlohy](sql-database-elastic-jobs-getting-started.md)
### [Sestavy napříč databázemi](sql-database-elastic-query-getting-started.md)
### [Dotazy napříč databázemi](sql-database-elastic-query-getting-started-vertical.md)
## Monitorování a ladění databází
### [Přehled služby SQL Database Advisor](sql-database-advisor.md)
### [Přehledy úloh na webu Azure Portal](sql-database-performance.md)
## [Vytvoření a správa přístupu a oprávnění](sql-database-get-started-security.md)
## [Optimalizace v paměti](sql-database-in-memory.md)
## [Synchronizace dat](sql-database-get-started-sql-data-sync.md)

# Postup

## Vytvoření a správa
### Servery a databáze
#### [Izolované databáze](sql-database-manage-portal.md)
#### [Azure Portal](sql-database-get-started.md)
#### [C#](sql-database-get-started-csharp.md)
#### [PowerShell](sql-database-manage-powershell.md)
#### [SQL Server Management Studio](sql-database-manage-azure-ssms.md)
### Fondy elastické databáze
#### [Azure Portal](sql-database-elastic-pool-create-portal.md)
#### [PowerShell](sql-database-elastic-pool-create-powershell.md)
#### [C#](sql-database-elastic-pool-create-csharp.md)
#### [T-SQL](sql-database-elastic-pool-manage-tsql.md)
### Horizontálně dělené databáze
#### [Použití správce mapování horizontálních oddílů](sql-database-elastic-scale-shard-map-management.md)
#### [Konfigurace zabezpečení dělení a slučování](sql-database-elastic-scale-split-merge-security-configuration.md)
#### [Práce s Dapperem](sql-database-elastic-scale-working-with-dapper.md)
#### [Použití rozhraní Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
#### [Směrování závislé na datech](sql-database-elastic-scale-data-dependent-routing.md)
#### [Víceklientské zabezpečení na úrovni řádků](sql-database-elastic-tools-multi-tenant-row-level-security.md)
#### [Správa přihlašovacích údajů](sql-database-elastic-scale-manage-credentials.md)
#### [Nasazení služby dělení a slučování](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
#### [Přidání horizontálního oddílu](sql-database-elastic-scale-add-a-shard.md)
#### [Oprava problémů s mapováním horizontálních oddílů pomocí třídy RecoveryManager](sql-database-elastic-database-recovery-manager.md)
###  Ověřování
#### [Ověřování Azure AD](sql-database-aad-authentication.md)
#### [Multi-Factor Authentication](sql-database-ssms-mfa-authentication.md)
### Pravidla brány firewall
#### [Azure Portal](sql-database-configure-firewall-settings.md)
#### [PowerShell](sql-database-configure-firewall-settings-powershell.md)
#### [REST API](sql-database-configure-firewall-settings-rest.md)
#### [T-SQL](sql-database-configure-firewall-settings-tsql.md)
### Úlohy
#### [Instalace služby](sql-database-elastic-jobs-service-installation.md)
#### [Azure Portal](sql-database-elastic-jobs-create-and-manage.md)
#### [PowerShell](sql-database-elastic-jobs-powershell.md)
#### [Upgrade klientské knihovny](sql-database-elastic-scale-upgrade-client-library.md)
### [Přihlášení](sql-database-manage-logins.md)

## Vývoj
### [Přehled](https://msdn.microsoft.com/library/mt763826.aspx)
### [Knihovny pro připojení](sql-database-libraries.md)
### Scénáře
#### [Víceklientské aplikace SaaS](sql-database-design-patterns-multi-tenancy-saas-applications.md)
#### [Data JSON](sql-database-json-features.md)
#### [Dočasné tabulky](sql-database-temporal-tables.md)
#### [Zásady uchovávání informací](sql-database-temporal-tables-retention-policy.md)
### Připojení aplikace
#### [.NET](sql-database-develop-dotnet-simple.md)
#### [Java](sql-database-develop-java-simple.md)
#### [Node.js](sql-database-develop-nodejs-simple.md)
#### [PHP](sql-database-develop-php-simple.md)
#### [Python](sql-database-develop-python-simple.md)
#### [Ruby](sql-database-develop-ruby-simple.md)
#### [Excel](sql-database-connect-excel.md)
#### [Visual Studio](sql-database-connect-query.md)

### [Vytvoření databází](sql-database-get-started-powershell.md)
### Správa elastických fondů
#### [PowerShell](sql-database-elastic-pool-manage-powershell.md)
#### [C#](sql-database-elastic-pool-manage-csharp.md)
### [Získání požadovaných hodnot pro ověření aplikace](sql-database-client-id-keys.md)
### [Použití portů nad 1433 pro technologii ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md)
### [Práce s chybovými zprávami](sql-database-develop-error-messages.md)
### [Použití dávkování](sql-database-use-batching-to-improve-performance.md)

### Referenční informace
#### [.NET (koncepce)](https://msdn.microsoft.com/library/kb9s9ks0.aspx)
#### [.NET (reference k rozhraní API)](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)
#### [Entity Framework](https://www.nuget.org/packages/EntityFramework/)
#### [Azure SDK (ke stažení)](https://www.visualstudio.com/vs/azure-tools/)
#### [Azure SDK (dokumentace)](../dotnet-sdk.md)
#### [Rutiny PowerShell pro správu služeb](https://msdn.microsoft.com/library/azure/dn546723.aspx)
#### REST
##### [REST (správa prostředků)](https://msdn.microsoft.com/library/azure/mt420159)
##### [REST (správa služeb)](https://msdn.microsoft.com/library/azure/dn505719.aspx)

## Detekce hrozeb
### [Detekce hrozeb](sql-database-threat-detection-get-started.md)
### [Brána firewall](sql-database-firewall-configure.md)

## Šifrování dat
### [Always Encrypted – přehled](sql-database-always-encrypted.md)
### [Azure Key Vault s funkcí Always Encrypted](sql-database-always-encrypted-azure-key-vault.md)
### [Transparentní šifrování dat](https://msdn.microsoft.com/library/azure/dn948096)
### [Šifrování sloupců](https://msdn.microsoft.com/library/azure/ms179331)

## Maskování dat
### [Dynamické maskování dat](sql-database-dynamic-data-masking-get-started-portal.md)
### [Klienti nižší úrovně](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)

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
### [Migrace stávajících škálovaných databází pro horizontální navýšení kapacity](sql-database-elastic-convert-to-use-elastic-tools.md)

## Monitorování a ladění
### [Izolované databáze](sql-database-performance-guidance.md)
### [Query Performance Insight](sql-database-query-performance.md)
### [SQL Database Advisor](sql-database-advisor-portal.md)
### [Výkon databáze](sql-database-single-database-monitor.md)
### [Zobrazení dynamické správy](sql-database-monitoring-with-dmvs.md)
### [Úrovně kompatibility](sql-database-compatibility-level-query-performance-130.md)
### [Auditování událostí](sql-database-auditing-get-started.md)
### [Čítače výkonu pro správce mapování horizontálních oddílů](sql-database-elastic-database-perf-counters.md)
### [Tipy pro ladění výkonu](sql-database-troubleshoot-performance.md)
### Změna úrovní služby a úrovně výkonu
#### [Azure Portal](sql-database-scale-up.md)
#### [PowerShell](sql-database-scale-up-powershell.md)
### OLTP v paměti
#### [Implementace OLTP v paměti](sql-database-in-memory-oltp-migration.md)
#### [Monitorování úložiště OLTP v paměti](sql-database-in-memory-oltp-monitoring.md)
### Úložiště dotazů
#### [Monitorování výkonu pomocí úložiště dotazů](https://msdn.microsoft.com/library/dn817826.aspx)
#### [Scénáře použití úložiště dotazů](https://msdn.microsoft.com/library/mt614796.aspx)
#### [Provoz úložiště dotazů](sql-database-operate-query-store.md)
### Rozšířené události
#### [Rozšířené události](sql-database-xevent-db-diff-from-svr.md)
#### [Cílový kód souboru události](sql-database-xevent-code-event-file.md)
#### [Cílový kód cyklické vyrovnávací paměti](sql-database-xevent-code-ring-buffer.md)

## Přesun dat
### [Kopírování databáze SQL](sql-database-copy.md)
#### [Azure Portal](sql-database-copy-portal.md)
#### [PowerShell](sql-database-copy-powershell.md)
#### [T-SQL](sql-database-copy-transact-sql.md)
### Export databáze do souboru BACPAC
#### [Azure Portal](sql-database-export.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
#### [Nástroj SQL Package](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)
#### [PowerShell](sql-database-export-powershell.md)
### Import databáze ze souboru BACPAC
#### [Azure Portal](sql-database-import.md)
#### [PowerShell](sql-database-import-powershell.md)
#### [SQL Server Management Studio](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
#### [Nástroj SQL Package](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
### [Načtení ze souboru CSV pomocí BCP](sql-database-load-from-csv-with-bcp.md)
### [Přesun dat mezi cloudovými databázemi s horizontálním navýšením kapacity](sql-database-elastic-scale-overview-split-and-merge.md)

## Dotaz
### [SQL Server Management Studio](sql-database-connect-query-ssms.md)
### [Dotazování více horizontálních oddílů](sql-database-elastic-scale-multishard-querying.md)
### Mezidatabázové dotazy
#### [Přehled](sql-database-elastic-query-overview.md)
#### [Dotazování napříč databázemi s různými schématy](sql-database-elastic-query-vertical-partitioning.md)
#### [Generování sestav napříč databázemi](sql-database-elastic-query-horizontal-partitioning.md)
#### [Distribuované transakce v cloudových databázích](sql-database-elastic-transactions-overview.md)

## Obnovení
### Obnovení odstraněné databáze
#### [Azure Portal](sql-database-restore-deleted-database-portal.md)
#### [PowerShell](sql-database-restore-deleted-database-powershell.md)
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
### [Problémy s připojením](sql-database-troubleshoot-common-connection-issues.md)
### [Chyba přechodného připojení](sql-database-troubleshoot-connection.md)
### [Diagnostika a prevence](sql-database-connectivity-issues.md)
### [Oprávnění](sql-database-troubleshoot-permissions.md)
### [Přesouvání databází](sql-database-troubleshoot-moving-data.md)

# Referenční informace
## [PowerShell](/powershell/azureps-cmdlets-docs/)
## [PowerShell – Classic](/powershell/servicemanagement/)
## [Java](/java/api/)
## [.NET](/dotnet/api/)
## [T-SQL](https://msdn.microsoft.com/library/azure/bb510741.aspx)
## [Rutiny Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
## [Rutiny SQL Serveru](https://msdn.microsoft.com/library/mt740629.aspx)
## [REST](/rest/api/sql/)

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
## [Ceny](https://azure.microsoft.com/pricing/details/sql-database/)
## [Aktualizace služeb](https://azure.microsoft.com/updates/?service=sql-database)
## [Nástroje SQL Serveru](https://msdn.microsoft.com/library/mt238365.aspx)
## [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
## [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
## [BCP](https://msdn.microsoft.com/library/ms162802.aspx)
## [SQLCMD](https://msdn.microsoft.com/library/ms162773.aspx)
## [SqlPackage](https://msdn.microsoft.com/hh550080.aspx)
## [Fórum](https://social.msdn.microsoft.com/Forums/home?forum=ssdsgetstarted)
## [Videa](https://azure.microsoft.com/documentation/videos/index/?services=sql-database)


<!--HONumber=Nov16_HO2-->


