# Přehled
## [Co je SQL Database?](sql-database-technical-overview.md)
### [Úrovně služeb](sql-database-service-tiers.md)
### [Jednotky DTU a eDTU](sql-database-what-is-a-dtu.md)
### [Přehled srovnávacích testů DTU](sql-database-benchmark-overview.md)
### [Omezení prostředků](sql-database-resource-limits.md)
### [Funkce](sql-database-features.md)
### [SQL Database – nejčastější dotazy](sql-database-faq.md)
## Porovnání
### [Srovnání SQL Database a SQL na virtuálním počítači](sql-database-paas-vs-sql-server-iaas.md)
### [Rozdíly v jazyce T-SQL](sql-database-transact-sql-information.md)
### [Srovnání SQL a NoSQL](../documentdb/documentdb-nosql-vs-sql.md)
## [Nástroje pro SQL Database](sql-database-manage-overview.md)
## [Kurzy pro SQL Database](sql-database-explore-tutorials.md)
## Zabezpečení
### [Přehled zabezpečení](sql-database-security-overview.md)
### [Azure Security Center pro službu Azure SQL Database](https://azure.microsoft.com/documentation/articles/security-center-sql-database/)
### [SQL Security Center](https://msdn.microsoft.com/library/azure/bb510589)
# Začínáme
## Servery a databáze
### Informace
#### [Servery](sql-database-server-overview.md)
#### [Izolované databáze](sql-database-overview.md)
#### [Více databází](sql-database-elastic-scale-introduction.md)
##### Mapování tenantů
###### [Knihovna elastických klientů](sql-database-elastic-database-client-library.md)
###### [Správce mapování horizontálních oddílů](sql-database-elastic-scale-shard-map-management.md)
###### [Směrování závislé na datech](sql-database-elastic-scale-data-dependent-routing.md)
###### [Správa přihlašovacích údajů](sql-database-elastic-scale-manage-credentials.md)
###### [Dotazování více horizontálních oddílů](sql-database-elastic-scale-multishard-querying.md)
##### Elastické fondy (fondy prostředků)
###### [Co je elastický fond?](sql-database-elastic-pool.md)
###### [Kdy použít elastický fond](sql-database-elastic-pool-guidance.md)
###### [Ceny elastických fondů](sql-database-elastic-pool-price.md)
##### Horizontálně dělené databáze
###### [Elastické nástroje – glosář](sql-database-elastic-scale-glossary.md)
###### [Přesouvání dat mezi horizontálními oddíly](sql-database-elastic-scale-overview-split-and-merge.md)
###### [Elastické nástroje – nejčastější dotazy](sql-database-elastic-scale-faq.md)
##### Elastický dotaz (mezidatabázové dotazy)
###### [Co je elastický dotaz?](sql-database-elastic-query-overview.md)
##### Elastické transakce (distribuované transakce)
###### [Transakce v cloudových databázích](sql-database-elastic-transactions-overview.md)
##### Elastické úlohy (mezidatabázové úlohy)
###### [Co je elastická úloha?](sql-database-elastic-jobs-overview.md)
#### [Připojení k SQL Database s použitím Azure RemoteApp](sql-database-ssms-remoteapp.md)
#### [Správa SQL Database pomocí služby Azure Automation](sql-database-manage-automation.md)
### Správný postup
#### [Vytvoření izolované databáze prostřednictvím portálu Azure Portal](sql-database-get-started.md)
#### [Vytvoření izolované databáze prostřednictvím prostředí PowerShell](sql-database-get-started-powershell.md)
#### [Vytvoření izolované databáze s použitím jazyka C#](sql-database-get-started-csharp.md)
#### [Vytvoření horizontálně dělené aplikace](sql-database-elastic-scale-get-started.md)
#### [Přesouvání dat mezi horizontálními oddíly](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
#### [Začínáme s elastickými úlohami](sql-database-elastic-jobs-getting-started.md)
#### [Začínáme s elastickými dotazy](sql-database-elastic-query-getting-started-vertical.md)
#### [Vytváření sestav s použitím elastického dotazu](sql-database-elastic-query-getting-started.md)
#### [Dotazování databází s různými schématy](sql-database-elastic-query-vertical-partitioning.md)
#### [Vytváření sestav napříč databázemi s horizontálním navýšením kapacity](sql-database-elastic-query-horizontal-partitioning.md)
## Migrace a přesun dat
### Informace
#### [Migrace databáze](sql-database-cloud-migrate.md)
#### [Transakční replikace](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
#### [Synchronizace dat](sql-database-get-started-sql-data-sync.md)
#### [Kopírování databáze SQL](sql-database-copy.md)
## Pravidla brány firewall, ověřování a autorizace
### Informace
#### [Řízení přístupu](sql-database-control-access.md)
#### [Brána firewall](sql-database-firewall-configure.md)
#### [Správa přihlašování](sql-database-manage-logins.md)
### Správný postup
#### [Ověřování a autorizace SQL](sql-database-control-access-sql-authentication-get-started.md)
#### [Autorizace a ověřování Azure AD](sql-database-control-access-aad-authentication-get-started.md)
## Zabezpečení a ochrana dat
### Informace
#### Auditování
##### [Auditování](sql-database-auditing-get-started.md)
##### [Podpora klientů nižší úrovně a změny koncových bodů IP pro auditování](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md)
#### [Detekce hrozeb](sql-database-threat-detection-get-started.md)
#### Šifrování dat
##### [Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md)
##### [Transparentní šifrování dat](https://msdn.microsoft.com/library/azure/dn948096)
##### [Šifrování sloupců](https://msdn.microsoft.com/library/azure/ms179331)
#### Maskování dat
##### Dynamické maskování dat
###### [Azure Portal](sql-database-dynamic-data-masking-get-started.md)
### Správný postup
#### [Dynamické maskování dat s využitím webu Azure Portal](sql-database-dynamic-data-masking-get-started.md)
##### [Funkce Always Encrypted s použitím úložiště certifikátů systému Windows](sql-database-always-encrypted.md)
## Kontinuita podnikových procesů
### Informace
#### [Přehled](sql-database-business-continuity.md)
#### [Zálohování databází](sql-database-automated-backups.md)
#### [Dlouhodobé uchovávání](sql-database-long-term-retention.md)
#### [Obnovení databází pomocí záloh](sql-database-recovery-using-backups.md)
#### [Zotavení izolované tabulky](sql-database-cloud-migrate-restore-single-table-azure-backup.md)
#### [Zotavení z výpadku datového centra](sql-database-disaster-recovery.md)
#### [Požadavky na ověřování pro zotavení po havárii](sql-database-geo-replication-security-config.md)
#### [Scénáře návrhu pro kontinuitu podnikových procesů](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
#### [Strategie zotavení po havárii s elastickými fondy](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
#### [Postupné upgrady](sql-database-manage-application-rolling-upgrade.md)
#### [Postup zotavení po havárii](sql-database-disaster-recovery-drills.md)
#### [Přehled aktivní geografické replikace](sql-database-geo-replication-overview.md)
### Správný postup
#### [Azure Portal: Zálohování a obnovení](sql-database-get-started-backup-recovery.md)
#### [PowerShell: Zálohování a obnovení](sql-database-get-started-backup-recovery-powershell.md)
## Vývoj aplikací
### Informace
#### [Přehled vývoje databázových aplikací](sql-database-develop-overview.md)
#### [Knihovny pro připojení](sql-database-libraries.md)
#### [Víceklientské aplikace SaaS](sql-database-design-patterns-multi-tenancy-saas-applications.md)
#### [Škálování víceklientských aplikací SaaS se zabezpečením na úrovni řádku](sql-database-elastic-tools-multi-tenant-row-level-security.md)
#### [Použití portů nad 1433 pro technologii ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md)
#### [Získání požadovaných hodnot pro ověření aplikace](sql-database-client-id-keys.md)
### Správný postup
#### Připojení aplikace
##### [.NET](sql-database-develop-dotnet-simple.md)
##### [C a C++](sql-database-develop-cplusplus-simple.md)
##### [Java](sql-database-develop-java-simple.md)
##### [Node.js](sql-database-develop-nodejs-simple.md)
##### [PHP](sql-database-develop-php-simple.md)
##### [Python](sql-database-develop-python-simple.md)
##### [Ruby](sql-database-develop-ruby-simple.md)
##### [Excel](sql-database-connect-excel.md)
#### [Připojení pomocí sady Visual Studio](sql-database-connect-query.md)
#### [Sestavení klientské aplikace](https://www.microsoft.com/sql-server/developer-get-started)
#### [Práce s chybovými zprávami](sql-database-develop-error-messages.md)
#### [Použití rozhraní Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
#### [Použití klientské knihovny s Dapperem](sql-database-elastic-scale-working-with-dapper.md)
### Implementace zákazníků
#### [Software Daxko/CSI](sql-database-implementation-daxko.md)
#### [GEP](sql-database-implementation-gep.md)
#### [SnelStart](sql-database-implementation-snelstart.md)
#### [Umbraco](sql-database-implementation-umbraco.md)
## Vývoj databází
### Informace
#### Dočasné tabulky
##### [Dočasné tabulky](sql-database-temporal-tables.md)
##### [Zásady uchovávání informací](sql-database-temporal-tables-retention-policy.md)
#### [Data JSON](sql-database-json-features.md)
#### [Optimalizace v paměti](sql-database-in-memory.md)
### Správný postup
#### [Vývoj pro SQL Server](https://msdn.microsoft.com/library/ms179422.aspx)
#### [Implementace OLTP v paměti](sql-database-in-memory-oltp-migration.md)
## Sledování a ladění
### Informace
#### [Izolované databáze](sql-database-single-database-monitor.md)
#### [Přehled služby SQL Database Advisor](sql-database-advisor.md)
#### [Pokyny pro izolovanou databázi](sql-database-performance-guidance.md)
#### [Informace o výkonu: Azure Portal](sql-database-performance.md)
#### [Použití dávkování](sql-database-use-batching-to-improve-performance.md)
#### [Rozšířené události](sql-database-xevent-db-diff-from-svr.md)
## SQL Database V11
### [Edice Web a Business](sql-database-web-business-sunset-faq.md)
### [Service Tier Advisor](sql-database-service-tier-advisor.md)
### [Nástroj pro vyhodnocení elastických fondů](sql-database-elastic-pool-database-assessment-powershell.md)
### [Upgrade na V12](sql-database-v12-plan-prepare-upgrade.md)
#### [Upgrade s využitím webu Azure Portal](sql-database-upgrade-server-portal.md)
#### [Upgrade s využitím PowerShellu](sql-database-upgrade-server-powershell.md)
# Postup
## Vytvoření a správa
### [Správa SQL Database prostřednictvím portálu Azure Portal](sql-database-manage-portal.md)
### [Správa SQL Database prostřednictvím prostředí PowerShell](sql-database-manage-powershell.md)
### [Správa SQL Database s použitím aplikace SSMS](sql-database-manage-azure-ssms.md)
### Servery
#### [Vytváření serverů](sql-database-create-servers.md)
#### [Zobrazení nebo aktualizace nastavení serveru](sql-database-view-update-server-settings.md)
### Izolované databáze
#### [Vytváření izolovaných databází](sql-database-create-databases.md)
#### [Zobrazení nebo aktualizace nastavení databáze](sql-database-view-update-database-settings.md)
### Pravidla brány firewall
#### [Vytváření pravidel brány firewall prostřednictvím portálu Azure Portal](sql-database-configure-firewall-settings.md)
#### [Vytváření pravidel brány firewall prostřednictvím prostředí PowerShell](sql-database-configure-firewall-settings-powershell.md)
#### [Vytváření pravidel brány prostřednictvím rozhraní API REST](sql-database-configure-firewall-settings-rest.md)
#### [Vytváření pravidel brány s použitím jazyka T-SQL](sql-database-configure-firewall-settings-tsql.md)
### Více databází
#### [Upgrade klientské knihovny v klientských aplikacích](sql-database-elastic-scale-upgrade-client-library.md)
#### Horizontálně dělené databáze
##### [Konfigurace zabezpečení](sql-database-elastic-scale-split-merge-security-configuration.md)
##### [Přidání horizontálního oddílu](sql-database-elastic-scale-add-a-shard.md)
##### [Odstraňování problémů s horizontálními oddíly mapy](sql-database-elastic-database-recovery-manager.md)
##### [Migrace stávajících databází s horizontálním navýšením kapacity na horizontálně dělené databáze](sql-database-elastic-convert-to-use-elastic-tools.md)
##### [Vytváření čítačů výkonu pro správce mapování horizontálních oddílů](sql-database-elastic-database-perf-counters.md)
#### Elastické úlohy
##### [Jak nainstalovat službu elastických úloh?](sql-database-elastic-jobs-service-installation.md)
##### [Vytváření a správa elastických úloh s využitím PowerShellu](sql-database-elastic-jobs-powershell.md) 
##### [Vytváření a správa elastických úloh prostřednictvím portálu Azure Portal](sql-database-elastic-jobs-create-and-manage.md)
##### [Jak odinstalovat elastické úlohy?](sql-database-elastic-jobs-uninstall.md)
#### Elastické fondy
##### [Vytvoření s použitím webu Azure Portal](sql-database-elastic-pool-create-portal.md)
##### [Vytvoření s použitím PowerShellu](sql-database-elastic-pool-create-powershell.md)
##### [Vytvoření s použitím C#](sql-database-elastic-pool-create-csharp.md)
##### [Správa s využitím webu Azure Portal](sql-database-elastic-pool-manage-portal.md)
##### [Správa s využitím PowerShellu](sql-database-elastic-pool-manage-powershell.md)
##### [Správa s využitím C#](sql-database-elastic-pool-manage-csharp.md)
##### [Správa s využitím T-SQL](sql-database-elastic-pool-manage-tsql.md)
##  Ověřování a autorizace
### [Ověřování Azure AD](sql-database-aad-authentication.md)
### [Multi-Factor Authentication](sql-database-ssms-mfa-authentication.md)
## Šifrování dat
### [Transparentní šifrování dat](https://msdn.microsoft.com/library/azure/dn948096)
### [Šifrování sloupců](https://msdn.microsoft.com/library/azure/ms179331)
## Migrace databází
### Určení kompatibility
#### [Určování kompatibility pomocí nástroje SQL Package](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
#### [Určování kompatibility s použitím aplikace SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
### Oprava problémů s kompatibilitou
#### [Oprava problémů s kompatibilitou s použitím rozšíření SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
#### [Oprava problémů s kompatibilitou s použitím aplikace SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
#### [Oprava problémů s kompatibilitou s použitím SMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
### [Migrace s použitím Průvodce migrací aplikace SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
## Monitorování a ladění
### [Query Performance Insight](sql-database-query-performance.md)
### [SQL Database Advisor](sql-database-advisor-portal.md)
### [Zobrazení dynamické správy](sql-database-monitoring-with-dmvs.md)
### [Úrovně kompatibility](sql-database-compatibility-level-query-performance-130.md)
### [Tipy pro ladění výkonu](sql-database-troubleshoot-performance.md)
### Změna úrovní služby a úrovně výkonu
#### [Změna úrovní služeb prostřednictvím portálu Azure Portal](sql-database-scale-up.md)
#### [Změna úrovní služeb prostřednictvím prostředí PowerShell](sql-database-scale-up-powershell.md)
### [Vytváření výstrah](sql-database-insights-alerts-portal.md)
#### [Monitorování úložiště OLTP v paměti](sql-database-in-memory-oltp-monitoring.md)
### Úložiště dotazů
#### [Monitorování výkonu pomocí úložiště dotazů](https://msdn.microsoft.com/library/dn817826.aspx)
#### [Scénáře použití úložiště dotazů](https://msdn.microsoft.com/library/mt614796.aspx)
#### [Provoz úložiště dotazů](sql-database-operate-query-store.md)
### Rozšířené události
#### [Cílový kód souboru události](sql-database-xevent-code-event-file.md)
#### [Cílový kód cyklické vyrovnávací paměti](sql-database-xevent-code-ring-buffer.md)
## Přesun dat
### Kopírování databáze SQL
#### [Kopírování s použitím webu Azure Portal](sql-database-copy-portal.md)
#### [Kopírování s použitím PowerShellu](sql-database-copy-powershell.md)
#### [Kopírování s použitím T-SQL](sql-database-copy-transact-sql.md)
### Export databáze do souboru BACPAC
#### [Export prostřednictvím portálu Azure Portal](sql-database-export.md)
#### [Export prostřednictvím aplikace SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
#### [Export pomocí nástroje SQL Package](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)
#### [Export prostřednictvím prostředí PowerShell](sql-database-export-powershell.md)
### Import databáze ze souboru BACPAC
#### [Import prostřednictvím portálu Azure Portal](sql-database-import.md)
#### [Import prostřednictvím prostředí PowerShell](sql-database-import-powershell.md)
#### [Import prostřednictvím aplikace SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
#### [Import pomocí nástroje SQL Package](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
### [Načtení ze souboru CSV pomocí BCP](sql-database-load-from-csv-with-bcp.md)
## Dotaz
### [Dotazování prostřednictvím aplikace SSMS](sql-database-connect-query-ssms.md)
## Zálohování a obnovení
### Dlouhodobé uchovávání záloh
#### [Konfigurace dlouhodobého uchovávání záloh](sql-database-configure-long-term-retention.md)
#### [Zobrazení záloh v trezoru služby Recovery Services](sql-database-view-backups-in-vault.md)
#### [Obnovení z dlouhodobě uchovávaných záloh](sql-database-restore-from-long-term-retention.md)
#### [Odstranění z dlouhodobě uchovávaných záloh](sql-database-long-term-retention-delete.md)
### Obnovení odstraněné databáze
#### [Obnovení odstraněných položek prostřednictvím portálu Azure Portal](sql-database-restore-deleted-database-portal.md)
#### [Obnovení odstraněných položek prostřednictvím prostředí PowerShell](sql-database-restore-deleted-database-powershell.md)
### Obnovení k určitému bodu v čase
#### [Obnovení k určitému bodu v čase](sql-database-point-in-time-restore.md)
#### [Zobrazení nejstaršího bodu obnovení](sql-database-view-oldest-restore-point.md)
### [Obnovení z geograficky redundantní zálohy](sql-database-geo-restore.md)
## Aktivní geografická replikace
### [Konfigurace prostřednictvím portálu Azure Portal](sql-database-geo-replication-portal.md)
### [Konfigurace prostřednictvím prostředí PowerShell](sql-database-geo-replication-powershell.md)
### [Konfigurace s použitím jazyka T-SQL](sql-database-geo-replication-transact-sql.md)
### [Převzetí služeb při selhání prostřednictvím portálu Azure Portal](sql-database-geo-replication-failover-portal.md)
### [Převzetí služeb při selhání prostřednictvím prostředí PowerShell](sql-database-geo-replication-failover-powershell.md)
### [Převzetí služeb při selhání s použitím jazyka T-SQL](sql-database-geo-replication-failover-transact-sql.md)
## Řešení potíží
### [Problémy s připojením](sql-database-troubleshoot-common-connection-issues.md)
### [Chyba přechodného připojení](sql-database-troubleshoot-connection.md)
### [Diagnostika a prevence](sql-database-connectivity-issues.md)
### [Oprávnění](sql-database-troubleshoot-permissions.md)
### [Přesouvání databází](sql-database-troubleshoot-moving-data.md)
# Referenční informace
## [PowerShell](/powershell/resourcemanager/azurerm.sql/v2.3.0/azurerm.sql)
## [PowerShell (elastická databáze)](/powershell/elasticdatabasejobs/v0.8.33/elasticdatabasejobs)
## [Azure CLI 2.0](/cli/azure/appservice)
## [.NET](/dotnet/api/microsoft.azure.management.sql.models)
## [Java](/java/api/com.microsoft.azure.management.sql)
## [Node.js](https://msdn.microsoft.com/library/mt652093.aspx)
## [Python](https://msdn.microsoft.com/library/mt652092.aspx)
## [Ruby](https://msdn.microsoft.com/library/mt691981.aspx)
## [PHP](https://msdn.microsoft.com/library/dn865013.aspx)
## [T-SQL](https://msdn.microsoft.com/library/bb510741.aspx)
## [REST](https://msdn.microsoft.com/library/azure/mt163571.aspx)

# Související
## Knihovna pro správu služby SQL Database
### [Získání balíčku knihovny pro správu SQL Database](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)
## [Ovladače SQL Serveru](https://msdn.microsoft.com/library/mt654049.aspx)
### [ADO.NET](https://msdn.microsoft.com/library/mt657768.aspx)
### [JDBC](https://msdn.microsoft.com/library/mt484311.aspx)
### [ODBC](https://msdn.microsoft.com/library/mt654048.aspx)

# Zdroje a prostředky
## [Ceny](https://azure.microsoft.com/pricing/details/sql-database/)
## [Fórum MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/sql-azure)
## [Videa](https://azure.microsoft.com/documentation/videos/index/?services=sql-database)
## [Aktualizace služeb](https://azure.microsoft.com/updates/?service=sql-database)
## [Nástroje SQL Serveru](https://msdn.microsoft.com/library/mt238365.aspx)
## [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
## [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
## [BCP](https://msdn.microsoft.com/library/ms162802.aspx)
## [SQLCMD](https://msdn.microsoft.com/library/ms162773.aspx)
## [SqlPackage](https://msdn.microsoft.com/hh550080.aspx)
