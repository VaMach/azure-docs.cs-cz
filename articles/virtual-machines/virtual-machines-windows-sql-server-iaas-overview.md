<properties
    pageTitle="Přehled SQL Serveru v Azure Virtual Machines | Microsoft Azure"
    description="Můžete se dozvědět, jak spouštět úplné edice SQL Serveru ve službě Azure Virtual Machines. Můžete získat přímé odkazy na všechny image virtuálních počítačů SQL Serveru a související obsah."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/29/2016"
    ms.author="jroth"/>

# Přehled SQL Serveru v Azure Virtual Machines

Toto téma popisuje možnosti spouštění SQL Serveru ve službě Azure Virtual Machines a uvádí [odkazy na image na portálu](#option-1-deploy-a-sql-vm-per-minute-licensing) a přehled [běžných úkolů](#manage-your-sql-vm).

>[AZURE.NOTE] Pokud jste již obeznámeni s SQL Serverem a chcete jenom zjistit, jak nasadit virtuální počítač SQL Serveru, najdete informace v tématu [Zřízení virtuálního počítače s SQL Serverem na webu Azure Portal](virtual-machines-windows-portal-sql-server-provision.md).

## Přehled
Můžete být správcem databáze, který hledá způsob, jak přesunout místní databázové úlohy SQL Serveru do cloudu. Nebo vývojářem, který zvažuje využívání možností relační databáze SQL Serveru pro aplikaci Azure. Jakou výhodu nabízí provozování úloh SQL Serveru na virtuálních počítačích Azure? Následující souhrnné video vás seznámí s výhodami a získáte z něj celkový technický přehled.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

## Vyhodnocení výhod

Než začnete, zkuste si nejdřív vyhodnotit to, co využíváním SQL Serveru na virtuálních počítačích Azure získáte.

Pokud do Azure přesouváte i jiné úlohy, například podnikovou aplikaci, má smysl přesunout si do Azure také všechny závislé databáze SQL Serveru a dosáhnout tak vyššího výkonu. Hostování SQL Serveru ve virtuálních počítačích Azure ale přináší i další výhody. Budete mít například automaticky přístup k většímu počtu datových center pro dosažení globální působnosti a zotavení po havárii. Úplný seznam scénářů a výhod najdete na [stránce produktu SQL Server na virtuálních počítačích Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/).

> [AZURE.NOTE] Když budete vyhodnocovat využívání SQL Serveru na virtuálních počítačích Azure, projděte si také ostatní možnosti úložiště a SQL v Azure, například [SQL Database](../sql-database/sql-database-technical-overview.md), [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) a [SQL Server Stretch Database](../sql     -server-stretch-database/sql-server-stretch-database-overview.md). Podrobné porovnání najdete v tématu [Volba cloudového řešení SQL Serveru: Azure SQL (PaaS) Database nebo SQL Server na virtuálních počítačích Azure (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

Když se rozhodnete provozovat SQL Server na virtuálních počítačích Azure, je jedním z první rozhodnutí to, jestli použít image virtuálního počítače, jejíž součástí jsou i náklady na licencování SQL Serveru. Jinak je totiž také možné využívat model BYOL (Bring Your Own License) a platit pouze za virtuální počítač. Tyto možnosti jsou popsané v následujících dvou částech článku.

## Možnost 1: Nasazení virtuálního počítače s SQL (licencování za minutu)
Následuje tabulka dostupných imagí SQL Serveru v galerii virtuálních počítačů. Po kliknutí na odkaz se začne vytvářet nový virtuální počítač SQL se zadanou verzí, edicí a operačním systémem. Všechny image zahrnují [náklady na licencování SQL Serveru](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

Podrobné pokyny jsou k dispozici v kurzu [Zřízení virtuálního počítače s SQL Serverem na webu Azure Portal](virtual-machines-windows-portal-sql-server-provision.md). Projděte si také téma [Osvědčené postupy z hlediska výkonu pro virtuální počítače s SQL Serverem](virtual-machines-windows-sql-performance.md), které vysvětluje, jak vybrat odpovídající velikost počítače a další funkce dostupné při zřizování.

|Version|Operační systém|Edition (Edice)|
|---|---|---|
|**SQL 2016**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [Dev](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**SQL 2014 SP1**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL 2014**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL 2012 SP3**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**SQL 2008 R2 SP3**|Windows Server 2008 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**SQL 2008 R2 SP3**|Windows Server 2012|[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|

## Možnost 2: Nasazení virtuálního počítače s SQL Serverem (BYOL)
Druhou možností je využít model BYOL (Bring Your Own License). V tomto scénáři zaplatíte jenom za virtuální počítač bez jakýchkoli dalších poplatků za licencování SQL Serveru. Pokud budete chtít využívat vlastní licenci, použijte níže uvedenou tabulku verzí, edicí a operačních systémů SQL Serveru. Na portálu mají názvy těchto imagí předponu **{BYOL}**.

> [AZURE.IMPORTANT] Abyste mohli používat image virtuálních počítačů BYOL, musíte mít smlouvu Enterprise s [Mobilitou licencí v rámci programu Software Assurance na Azure](https://azure.microsoft.com/pricing/license-mobility/). Budete také potřebovat platnou licenci pro verzi nebo edici SQL Serveru, kterou budete chtít používat. Do **10** dnů od zřízení virtuálního počítače musíte [společnosti Microsoft předat nezbytné informace v souvislosti s BYOL](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf).

Pokyny v [kurzu zřizování](virtual-machines-windows-portal-sql-server-provision.md) platí, musíte ale používat jednu z následujících možných imagí **BYOL**. Projděte si také téma [Osvědčené postupy z hlediska výkonu pro virtuální počítače s SQL Serverem](virtual-machines-windows-sql-performance.md), které vysvětluje, jak vybrat odpovídající velikost počítače a další funkce dostupné při zřizování.

|Version|Operační systém|Edition (Edice)|
|---|---|---|
|**SQL Server 2016**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

## Správa vašeho virtuálního počítače s SQL Serverem
Po zřízení virtuálního počítače s SQL Serverem je možné volitelně provést některé úlohy správy. Z určitého hlediska se SQL Server konfiguruje a spravuje stejně jako místní SQL Server. Některé úlohy jsou ale pro Azure specifické. V následujících částech se na některé z těchto oblastí podíváme podrobněji a najdete v nich také odkazy na další informace.

### Migrace dat

Pokud máte existující databázi, bude ji nutné přesunout do nově zřízeného virtuálního počítače s SQL Serverem. Seznam možností migrace a pokyny najdete v části [Migrace databáze do SQL Serveru ve virtuálním počítači Azure](virtual-machines-windows-migrate-sql.md).

### Nakonfigurování vysoké dostupnosti

Pokud požadujete vysokou dostupnost, doporučujeme nakonfigurovat skupiny dostupnosti SQL Serveru. Znamená to využívat více virtuálních počítačů Azure ve virtuální síti. Na Portálu Azure najdete šablonu, která vám tuto konfiguraci nastaví automaticky. Další informace najdete v tématu [Konfigurace skupiny dostupnosti AlwaysOn ve virtuálních počítačích Azure Resource Manageru](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). Pokud chcete skupiny dostupnosti a přidružený naslouchací proces nakonfigurovat ručně, najdete další informace v tématu [Konfigurace skupin dostupnosti AlwaysOn ve virtuálním počítači Azure](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Další důležité informace o tom, co je potřeba zvážit z hlediska vysoké dostupnosti, najdete v tématu [Vysoká dostupnost a zotavení po havárii pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

### Zálohování dat
Virtuální počítače Azure mohou využívat výhod [automatizovaného zálohování](virtual-machines-windows-sql-automated-backup.md), které pravidelně vytváří zálohy databáze do úložiště blob storage. Tento postup můžete použít také ručně. Další informace najdete v tématu [Použití služby Azure Storage pro zálohování a obnovování SQL Serveru](virtual-machines-windows-use-storage-sql-server-backup-restore.md). Přehled všech možností zálohování a obnovení najdete v tématu [Zálohování a obnovení pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

### Automatické aktualizace
Virtuální počítače Azure mohou používat [Automatické opravy](virtual-machines-windows-sql-automated-patching.md) k naplánování okna údržby pro automatickou instalaci důležitých oken a aktualizace SQL serveru.

### Program Zlepšování softwaru a služeb na základě zkušeností uživatelů (CEIP)
Program Zlepšování softwaru a služeb na základě zkušeností uživatelů (CEIP) je ve výchozím nastavení povolen. Nejedná se úlohu správy (pokud však nechcete program CEIP po zřízení vypnout). Tento program můžete přizpůsobit nebo vypnout připojením k virtuálnímu počítači pomocí vzdálené plochy. Pak spusťte nástroj **pro zasílání zpráv o chybách a vytváření sestav využití SQL Serveru**. Podle pokynů zakažte generování sestav.

## Další kroky
[Projděte si mapy kurzů](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) pro SQL Server na virtuálních počítačích Azure.

Máte nějaký další dotaz? Nejdříve si projděte [Nejčastější dotazy k SQL Serveru na Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-faq.md). Můžete ale také svoje dotazy nebo připomínky přidat do pole, které najdete v dolní části všech témat týkajících se virtuálních počítačů s SQL Serverem, a komunikovat se zástupci společnosti Microsoft a komunitou uživatelů.



<!--HONumber=ago16_HO5-->


