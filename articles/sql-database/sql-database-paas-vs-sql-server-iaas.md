<properties
    pageTitle="SQL (PaaS) Database vs. SQL Server v cloudu na virtuálních počítačích (IaaS) | Microsoft Azure"
    description="Další informace o tom, která cloudová možnost SQL Serveru nejlépe odpovídá vaší aplikaci: Azure SQL (PaaS) Database nebo SQL Server v cloudu na virtuálních počítačích Azure."
    services="sql-database, virtual-machines"
    keywords="Cloud SQL Server, SQL Server v cloudu, databáze PaaS, cloudový SQL Server, DBaaS"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="cjgronlund"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/20/2016"
    ms.author="carlrab"/>

# Volba cloudového řešení SQL Serveru: Azure SQL (PaaS) Database nebo SQL Server na virtuálních počítačích Azure (IaaS)

Azure nabízí pro hostování úloh SQL Serveru v Microsoft Azure dvě možnosti:

* [Azure SQL Database](https://azure.microsoft.com/services/sql-database/): Databáze SQL nativní pro cloud označovaná také jako databáze PaaS nebo databáze DBaaS, která je optimalizovaná pro vývoj aplikací SaaS. Nabízí kompatibilitu s většinou funkcí SQL Serveru. Další informace o modelu PaaS najdete v článku [Co je to PaaS?](https://azure.microsoft.com/overview/what-is-paas/), a pokud se chcete podívat na video, přejděte na stránku [SAIIK: PaaS vs. IaaS – orientace v rozhodovacím stromu: Azure SQL DB vs. SQL Server na virtuálním počítači](https://channel9.msdn.com/Series/SAIIK-SQL-Server-on-Azure-IaaS-Implementation-Kit/SAIIK-PaaS-vs-IaaS).
* [SQL Server na virtuálních počítačích Azure:](https://azure.microsoft.com/services/virtual-machines/sql-server/) SQL Server nainstalovaný a hostovaný v cloudu na virtuálních počítačích s Windows Serverech běžící v Azure označovaný také jako IaaS.

Zjistěte, jak jednotlivé možnosti zapadají do datové platformy společnosti Microsoft, a získejte pomoc se spárováním té správné volby s požadavky vaší firmy. Ať už je pro vás určující úspora nákladů nebo minimální režie z hlediska správy (které pro vás mají vyšší prioritu než cokoli jiného), tento článek vám může pomoci rozhodnout, jaký přístup přinese vaší firmě nejlepší výsledky z hlediska požadavků, na jejichž splnění vám nejvíc záleží.


## Datová platforma společnosti Microsoft

Jednou z nejdůležitějších věcí, které je potřeba chápat, pokud se porovnávají výhody databází na Azure oproti využívání místních databází SQL Serveru, je to, že použitelné jsou všechny z nich. Datová platforma společnosti Microsoft využívá technologii SQL Serveru a zpřístupňuje ji napříč fyzickými místními počítači, prostředími privátního cloudu, prostředími privátního cloudu hostovanými třetí stranou a veřejným cloudem. To vám umožňuje vyhovět jedinečným a rozmanitým potřebám firmy pomocí kombinace místního nasazení a nasazení hostovaného v cloudu pomocí stejné sady serverových produktů, nástrojů pro vývoj a odborných znalosti ve všech těchto prostředích.

   ![Možnosti SQL Serveru v cloudu: SQL server na IaaS nebo databáze SQL SaaS v cloudu.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Jak je vidět v diagramu, každou nabídku je možné charakterizovat úrovní správy, kterou nad infrastrukturou máte (na ose X), a stupněm nákladové efektivity dosahovaným konsolidací úrovně databáze a automatizace (na ose Y).

Při navrhování aplikace jsou pro hostování součásti aplikace s SQL Serverem k dispozici čtyři základní možnosti:

- SQL Server na nevirtualizovaných fyzických počítačích
- SQL Server v místních virtualizovaných počítačích (privátní cloud)
- SQL Server na virtuálním počítači Azure (veřejný cloud Microsoftu)
- Azure SQL Database (veřejný cloud Microsoftu)

V následujících částech si řekneme něco o SQL Serveru ve veřejném cloudu Microsoftu: Azure SQL Database a SQL Server na virtuálních počítačích Azure. Kromě toho se podíváme na běžné motivační faktory firem pro určení toho, která volba je pro konkrétní aplikaci nejvhodnější.

## Bližší pohled na Azure SQL Database a SQL Server na virtuálních počítačích Azure

**Azure SQL Database** je relační databáze jako služba (DBaaS) hostovaná v cloudu Azure, která spadá do oborových kategorií *SaaS* a *PaaS*. [SQL Database](sql-database-technical-overview.md) je postavená na standardizovaném hardwaru a softwaru, který vlastní, hostuje a spravuje Microsoft. Pomocí SQL Database můžete provádět vývoj přímo na úrovni služby pomocí integrovaných funkcí. Při použití SQL Database platíte průběžnými platbami s možností vertikálního nebo horizontálního navyšování kapacity pro dosažení vyššího výkonu bez přerušení.

**SQL Server na virtuálních počítačích Azure** spadá do oborové kategorie *IaaS* a umožňuje vám spustit SQL Server uvnitř virtuálního počítače v cloudu. Podobně jako SQL Database je i tato databáze postavena na standardizovaném hardwaru, který je vlastněn, hostován a spravován společností Microsoft. Při použití SQL Serveru na virtuálním počítači si můžete přinést vlastní licenci pro SQL Server nebo použít předem licencovanou image SQL Serveru z portálu Azure.

Obecně jsou tyto dvě možnosti SQL optimalizovány pro různé účely:

- **SQL Database** je optimalizována pro snížení celkových nákladů na minimum pro zřizování a správu velkého počtu databází. Snižuje náklady na průběžnou správu, protože nemusíte spravovat žádné virtuální počítače, operační systémy nebo databázový software. To zahrnuje upgrady, vysokou dostupnost a [zálohování](sql-database-automated-backups.md). Obecně platí, že Azure SQL Database může výrazně zvýšit počet databází, které spravuje jeden IT nebo vývojářský zdroj.
- **SQL Server běžící na virtuálních počítačích Azure** je optimalizovaný pro rozšiřování stávajících místních aplikací SQL Serveru do cloudu v hybridním scénáři nebo nasazování stávající aplikace do Azure ve scénáři migrace nebo vývoje/testování. Příkladem hybridního scénáře je uchovávání replik sekundární databáze v Azure prostřednictvím použití [virtuálních sítí Azure](../virtual-network/virtual-networks-overview.md). V případě používání SQL Serveru na virtuálních počítačích Azure máte úplná práva pro správu pro vyhrazenou instanci SQL Serveru a cloudové virtuální počítače. Je ideální volbou v případě, že organizace už má dostupné IT zdroje pro údržbu virtuálních počítačů. V případě SQL Serveru na virtuálních počítačích můžete vytvořit vysoce přizpůsobený systém pro plnění specifických požadavků vaší aplikace na výkon a dostupnost.

Následující tabulka shrnuje hlavní vlastnosti SQL Database a SQL Serveru na virtuálních počítačích Azure:

|       | Databáze SQL | SQL Server na virtuálním počítači Azure|
| -------------- | ------------ | ---------------------- |
| **Nejvhodnější pro:** | Nové aplikace navržené pro cloud, které mají časová omezení z hlediska vývoje a marketingu. | Existující aplikace, které vyžadují rychlou migraci do cloudu s minimálními změnami. Scénáře rychlého vývoje a testování, když si nechcete koupit hardware pro místní neprodukční SQL Server. |
|| Aplikace, které potřebují integrovanou vysokou dostupnost, možnost zotavení po havárii a upgradovací mechanismy. | Aplikace, které nepotřebují integrovanou vysokou dostupnost, možnost zotavení po havárii a upgradovací mechanismy. |
||Týmy, které nechtějí spravovat příslušný operační systém a nastavení konfigurace.| Pokud potřebujete přizpůsobené prostředí IT s úplnými právy pro správu.|
||Databáze velikosti až 1 TB nebo větší databází, které se dají [horizontálně nebo vertikálně dělit na oddíly](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) pomocí modelu škálování na víc systémů.|Databáze větší než 1 TB, které se nedají [horizontálně nebo vertikálně dělit na oddíly](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling).|
||[Sestavování aplikací s modelem SaaS (Software-as-a-Service)](sql-database-design-patterns-multi-tenancy-saas-applications.md).| Sestavování hybridních aplikací|
|||||
|**Prostředky:**|Nechcete využívat zdroje IT oddělení pro zajišťování podpory a údržby základní infrastruktury, ale chcete se soustředit na vrstvu aplikací.|Máte IT zdroje pro podporu a údržbu.|
|**Celkové náklady na vlastnictví:**|Eliminuje náklady na hardware a snižuje náklady na správu.|Eliminuje náklady na hardware.|
|**Kontinuita podnikových procesů:**|Kromě předdefinovaných funkcí infrastruktury s odolností proti chybám poskytuje Azure SQL Database funkce pro zvýšení kontinuity podnikových procesů, jako je [automatické zálohování](sql-database-automated-backups.md), [obnovení bodu v čase](sql-database-recovery-using-backups.md#point-in-time-restore), [geografické obnovení](sql-database-recovery-using-backups.md#geo-restore) a [aktivní geografická replikace](sql-database-active-geo-replication.md). Další informace najdete v tématu [Databáze SQL – kontinuita podnikových procesů (přehled)](sql-database-business-continuity.md).|SQL Server na virtuálních počítačích Azure vám umožní nastavit řešení vysoké dostupnosti a zotavení po havárii pro konkrétní potřeby vaší databáze. Můžete tak mít systém, který je vysoce optimalizovaný pro vaši aplikaci. Sami podle potřeby můžete otestovat a spustit převzetí služeb při selhání. Další informace najdete v tématu [Vysoká dostupnost a zotavení po havárii pro SQL Server v Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).|
|**Hybridní cloud:**|Vaše místní aplikace mohou přistupovat k datům v Azure SQL Database.|V případě SQL Serveru na virtuálních počítačích Azure můžete mít aplikace, které běží částečně v cloudu a částečně místně. Můžete si například rozšířit místní síť a služby Active Directory Domain do cloudu přes [Azure Virtual Network](../virtual-network/virtual-networks-overview.md). Kromě toho můžete uložit místní datové soubory v úložišti Azure pomocí [datových souborů SQL Serveru v Azure](http://msdn.microsoft.com/library/dn385720.aspx). Další informace najdete v tématu [Úvod do hybridního cloudu SQL Serveru 2014](http://msdn.microsoft.com/library/dn606154.aspx).|
||Podporuje [transakční replikaci systému SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) jako odběratel.|Podporuje [transakční replikace systému SQL Server](https://msdn.microsoft.com/library/mt589530.aspx), zotavení po havárii a [repliky AlwaysOn ve virtuálních počítačích Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).|
|||||
|||||

## Motivace firem pro zvolení Azure SQL Database nebo SQL Serveru na virtuálních počítačích Azure

### Náklady

Ať už jste začínající společnost, která nemá mnoho peněz, nebo tým v zavedené společnosti, který pracuje s limitovaným rozpočtem, je omezená výše prostředků často primárním rozhodujícím faktorem při volbě řešení pro hostování firemních databází. V této části si nejdříve řekneme základní informace o fakturaci a licencování v Azure s ohledem na tyto dvě volby relačních databází: SQL Database a SQL Server na virtuálních počítačích Azure. Pak si ukážeme, jak byste měli vypočítat celkové náklady na aplikaci.

#### Základy fakturace a licencování

**SQL Database** se prodává zákazníkům jako služba, ne ve formě licence, kdežto [licencování systému SQL Server na virtuálních počítačích Azure](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md) vyžaduje buď licencování SQL Serveru po minutách, nebo vlastní licenci prostřednictvím programu Software Assurance.

V současné době je **SQL Database** k dispozici v několika cenových úrovních služeb, přičemž všechny z nich se účtují hodinově s pevnou sazbou na základě cenové úrovně služby a výkonu, které jste zvolili. Kromě toho se vám účtuje odchozí přenos přes internet podle běžných [sazeb za přenos dat](https://azure.microsoft.com/pricing/details/data-transfers/). Cenové úrovně služeb Basic, Standard a Premium jsou navržené tak, aby umožňovaly dosahovat předvídatelného výkonu s více úrovněmi výkonu podle požadavků vaší aplikace ve špičce. Podle potřeby můžete mezi cenovými a výkonými úrovněmi služeb přecházet, abyste zohlednili měnící se potřeby vaší aplikace z hlediska propustnosti. Pokud má vaše databáze velký objem transakcí a musí zajišťovat podporu pro více souběžných uživatelů, doporučujeme využívat cenovou úroveň služeb Premium. Nejnovější informace o aktuálně podporovaných cenových úrovních služeb najdete v tématu [Cenové úrovně služby Azure SQL Database](sql-database-service-tiers.md). Můžete taky vytvořit [fondy elastické databáze](sql-database-elastic-pool.md), které umožňují sdílení prostředků výkonu mezi různými instancemi databáze.

V případě **SQL Database** je databázový software konfigurován, opravován a upgradován automaticky společností Microsoft, což snižuje náklady na správu. Kromě toho vám [integrované funkce zálohování](sql-database-automated-backups.md) pomůžou dosáhnout výrazných úspor nákladů, hlavně v případě, že máte velký počet databází.

Pokud máte **SQL Server na virtuálních počítačích Azure**, můžete buď použít image SQL Serveru poskytovanou platformou (která zahrnuje licenci), nebo použít svoji vlastní licenci na SQL Server. Pokud budete používat image poskytované v rámci Azure, budou provozní náklady záviset na velikosti virtuálního počítače a také na edici SQL Serveru, kterou zvolíte. Bez ohledu na velikost virtuálního počítače nebo edici SQL Serveru platíte cenu za licenci SQL Serveru a Windows Serveru po minutách, a k tomu navíc náklady na úložiště Azure za disky virtuálních počítačů. Možnost fakturace po minutách vám umožňuje používat SQL Server tak dlouho, jak budete potřebovat, bez nutnosti dokupovat další licence na SQL Server. Pokud pro Azure použijete svoji vlastní licenci na SQL Server, bude se vám účtovat jenom cena za Windows Server a úložiště. Další informace o používání vlastní licence (BYOL) najdete v tématu [Mobilita licencí v rámci programu Software Assurance na Azure](https://azure.microsoft.com/pricing/license-mobility/).

#### Výpočet celkových nákladů na aplikaci

Když začnete používat cloudovou platformu, zahrnují primárně náklady na provozování vaší aplikace náklady na vývoj a správu spolu s náklady na služby, které platforma veřejného cloudu vyžaduje.

Tady je podrobný je výpočet nákladů na vaši aplikaci běžící v SQL Database a na SQL Serveru na virtuálních počítačích Azure:

**Při používání Azure SQL Database:**

*Celkové náklady na aplikaci = vysoce minimalizované náklady na správu + náklady na vývoj softwaru + náklady na službu SQL Database*

**Při používání SQL Serveru na virtuálních počítačích Azure:**

*Celkové náklady na aplikaci = minimalizované náklady na vývoj nebo modifikace softwaru + náklady na správu + náklady na licencování SQL Serveru a Windows Serveru + náklady na úložiště Azure*

Další informace o cenách najdete v následujících zdrojích informací a materiálech:

- [SQL Database – ceny](https://azure.microsoft.com/pricing/details/sql-database/)
- [Virtual Machines – ceny](https://azure.microsoft.com/pricing/details/virtual-machines/) pro [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) a [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
- [Cenová kalkulačka funkcí Azure](https://azure.microsoft.com/pricing/calculator/)

> [AZURE.NOTE] SQL Server má malou podmnožinu funkcí, které nejsou platné nebo dostupné při používání SQL Database. Další informace najdete v tématech s [informacemi o obecných omezeních a pokynech pro SQL Database](sql-database-general-limitations.md) a [informacemi o jazyce Transact-SQL služby SQL Database](sql-database-transact-sql-information.md). Pokud do cloudu přesouváte existující řešení SQL Serveru, najdete další informace v tématu [Migrace databáze SQL Serveru do Azure SQL Database](sql-database-cloud-migrate.md). Při migraci stávající místní aplikace SQL Serveru do SQL Database zvažte možnost aktualizace aplikace, abyste mohli využívat funkce, které nabízejí cloudové služby. Můžete například zvážit hostování vaší aplikační vrstvy pomocí [Azure Web App Service](https://azure.microsoft.com/services/app-service/web/) nebo [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/), a zvýšit si tak efektivitu nákladů.

### Správa

Pro mnoho firem je rozhodnutí o přechodu na využívání cloudové služby nejen otázkou nízkých nákladů, ale také snížení složitosti správy. V případě **SQL Database** Microsoft spravuje příslušný hardware, automaticky replikuje všechna data pro zajištění vysoké dostupnosti, konfiguruje a upgraduje databázový software, spravuje vyrovnávání zatížení a provádí transparentní převzetí služeb při selhání, pokud dojde k selhání serveru. Svoji databázi můžete dál spravovat, ale nebudete už muset spravovat databázový stroj, serverový operační systém ani hardware.  Příklady položek, které můžete dál spravovat: databáze a přihlašovací údaje, ladění indexů nebo dotazů a auditování a zabezpečení.

Na druhé straně může mít interní specialisty a tím pádem si chtít zachovat kontrolu jak nad umístěním databáze, tak nad umístění disku. V případě **SQL Serveru běžícího na virtuálních počítačích Azure** máte plnou kontrolu nad operačním systémem a konfigurací instancí SQL Serveru. V případě virtuálních počítačů je na vás, kdy budete chtít aktualizovat nebo upgradovat operační systém a software databáze a kdy se má nainstalovat další software, jako jsou antivirové nástroje a nástroje pro zálohování. Kromě toho můžete nastavovat velikost virtuálního počítače, počet disků a jejich konfigurace úložiště.  Azure například umožňuje podle potřeby měnit velikost virtuálního počítače. Informace najdete v tématu věnovaném [velikostem virtuálních počítačů a cloudových služeb pro Azure](../virtual-machines/virtual-machines-linux-sizes.md).

### Smlouvy o úrovni služeb (SLA)

Pro řadu IT oddělení je nejvyšší prioritou plnit povinnosti z hlediska garantované doby provozuschopnosti vyplývající ze Smlouvy o úrovni služeb (SLA). V této části se podíváme na to, jaká smlouva SLA se vztahuje na každou z možností hostování databáze.

Pro **SQL Database** s cenovou úrovní služeb Basic, Standard a Premium poskytuje Microsoft smlouvu SLA s dostupností 99,99 %. Nejnovější informace najdete v tématu věnovaném [Smlouvám o úrovni služeb (SLA)](https://azure.microsoft.com/support/legal/sla/sql-database/). Nejnovější informace o cenových úrovních služby SQL Database a podporovaných plánech kontinuity podnikových procesů najdete v tématu [Úrovně služeb](sql-database-service-tiers.md).

Pro **SQL Server běžící na virtuálních počítačích Azure** poskytuje Microsoft smlouvu SLA s dostupností 99,95 %, která se vztahuje pouze na virtuální počítač. Tato smlouva SLA nepokrývá procesy (například SQL Server) běžící na virtuálním počítači a vyžaduje, abyste v rámci skupiny dostupnosti hostovali minimálně dvě instance virtuálních počítačů. Nejnovější informace najdete v tématu věnovaném smlouvám [SLA k virtuálním počítačům](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Pro vysokou dostupnost (HA – High Availability) databází v rámci virtuálních počítačů musíte nakonfigurovat jednu z podporovaných možností vysoké dostupnosti SQL Serveru, jako jsou například [skupiny dostupnosti AlwaysOn](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx).

### <a name="market"></a>Rychlost uvedení na trh

**Databáze SQL** je tím správným řešením pro aplikace navržené pro cloud, pokud je důležitým faktorem produktivita vývojářů a rychlé uvedení na trh. V případě programových funkcí podobných DBA jde o ideální řešení pro cloudové architekty a vývojáře, protože snižuje potřebu správy příslušného operačního systému a databáze. Můžete například pomocí [REST API](http://msdn.microsoft.com/library/azure/dn505719.aspx) a [rutin prostředí PowerShell](http://msdn.microsoft.com/library/azure/dn546726.aspx) automatizovat a spravovat operace správy pro tisíce databází. Funkce, jako jsou [fondy elastických databází](sql-database-elastic-pool.md), vám umožní zaměřit se na aplikační vrstvu a zajistit rychlejší dodání vašeho řešení na trh.

**SQL Server běžící na virtuálních počítačích Azure** je ideálním řešením v případě, že vaše existující nebo nové aplikace vyžadují přístup ke všem funkcím instance SQL Serveru a jejich řízení. Je také vhodné, pokud chcete migrovat existující místní aplikace a databáze do Azure tak, jak jsou. Vzhledem k tomu, že nemusíte měnit prezentační, aplikační ani datové vrstvy, ušetříte čas a finanční prostředky z rozpočtu, které byste jinak museli vynaložit na vytvoření nové architektury existujícího řešení. Místo toho se můžete soustředit na migraci všech svých řešení do Azure a provedení některých optimalizací výkonu, která mohou být platformou Azure vyžadována. Další informace najdete v tématu [Osvědčené postupy z hlediska výkonu pro SQL Server ve službě Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sql-performance.md).

## Souhrn

Tento článek se věnoval SQL Database a SQL Serveru na virtuálních počítačích Azure a běžným motivačním faktorům firem, které by mohly mít vliv na vaše rozhodnutí. Následuje souhrn návrhů, které doporučujeme zvážit:

**Databázi SQL Azure** zvolte, pokud:

- Vytváříte nové cloudové aplikace nebo chcete migrovat existující řešení SQL Serveru, abyste mohli využívat výhod v podobě úspor nákladů a optimalizace výkonu, které cloudové služby nabízejí. Tento přístup přináší výhody v podobě plně spravované cloudové služby, pomáhá zkrátit čas nutný k uvedení produktu na trh a může zajistit dlouhodobou optimalizaci nákladů.

- Budete chtít, aby Microsoft prováděl běžné operace správy vašich databází a vyžadovat pro databáze smlouvy SLA s vyšší dostupností.



**SQL Server na virtuálních počítačích Azure** zvolte, pokud:

- Máte stávající místní aplikace a chcete přestat udržovat svůj vlastní hardware nebo uvažujete o hybridních řešeních. Tento přístup vám umožňuje rychleji dosáhnout vysoké kapacity databáze při současném usnadnění připojení k místním aplikacím přes zabezpečené tunelové propojení.

- Máte existující IT zdroje, potřebujete úplná práva pro správu SQL Serveru a vyžadujete úplnou kompatibilitu s místním SQL Serverem. Tento přístup vám umožní minimalizovat náklady na vývoj nebo úpravy existujících aplikací a flexibilně spouštět většinu aplikací. Kromě toho to zajišťuje úplnou kontrolu nad virtuálním počítačem, operačním systémem a konfigurací databáze.


## Další kroky
- Podívejte se na téma [Kurz k SQL Database: vytvoření databáze SQL během pár minut pomocí portálu Azure](sql-database-get-started.md), které vám pomůže začít s SQL Database.
- Viz [SQL Database – ceny] (https://azure.microsoft.com/pricing/details/sql-database/).
- Informace o tom, jak začít s SQL Serverem na virtuálních počítačích Azure, najdete v tématu věnovaném [zřízení virtuálního počítače s SQL Serverem v Azure](../virtual-machines/virtual-machines-windows-portal-sql-server-provision.md).
- Projděte si článek [SQL Server na virtuálním počítači Azure: Mapa kurzů](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/).



<!---HONumber=Aug16_HO4-->


