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
    ms.date="10/11/2016"
    ms.author="jroth"/>


# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Přehled SQL Serveru v Azure Virtual Machines

Toto téma popisuje možnosti spouštění SQL Serveru na virtuálních počítačích Azure a uvádí [odkazy na image na portálu](#option-1-create-a-sql-vm-with-per-minute-licensing) a přehled [běžných úloh](#manage-your-sql-vm).

>[AZURE.NOTE] Pokud jste již obeznámeni s SQL Serverem a chcete jenom zjistit, jak nasadit virtuální počítač s SQL Serverem, najdete informace v tématu [Zřízení virtuálního počítače s SQL Serverem na webu Azure Portal](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="overview"></a>Přehled
Pokud jste správce databáze nebo vývojář, nabízejí virtuální počítače Azure způsob, jak lokální úlohy aplikace SQL Serveru přesunout do cloudu. Následující video poskytuje technický přehled virtuálních počítačů Azure pro SQL Server.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

Ve videu se probírají následující oblasti:

|Čas|Oblast|
|---|---|
| 00:21 | Co jsou virtuální počítače Azure? |
| 01:45 | Zabezpečení |
| 02:50 | Připojení |
| 03:30 | Spolehlivost a výkon úložiště |
| 05:20 | Velikost virtuálních počítačů |
| 05:54 | Vysoká dostupnost a smlouva SLA |
| 07:30 | Podpora konfigurace |
| 08:00 | Monitorování |
| 08:32 | Ukázka: Vytvoření virtuálního počítače s SQL Serverem 2016 |

>[AZURE.NOTE] Video se zaměřuje na SQL Server 2016, ale Azure poskytuje image virtuálních počítačů pro mnoho verzí SQL Serveru: 2008, 2012, 2014 a 2016. 

## <a name="understand-your-options"></a>Vysvětlení možností, které máte
Existuje mnoho důvodů, kvůli kterým byste se mohli rozhodnout, že jako hostitele dat budete využívat Azure. Pokud do Azure přesunujete některou aplikaci, dosáhnete lepšího výkonu, když tam přesunete i data. Ale jsou tu i další výhody. Budete mít automaticky přístup k většímu počtu datových center pro dosažení globální působnosti a zotavení po havárii. Data také získají vysoké zabezpečení a odolnost.

SQL Server běžící na virtuálních počítačích Azure je jednou z možností, jak ukládat relační data v Azure. Následující tabulka poskytuje rychlý přehled nabídek SQL v Azure.

|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Nabídka SQL | Popis |
|---:|---|---|
|![SQL Server na Azure Virtual Machines](./media/virtual-machines-windows-sql-server-iaas-overview/sql-server-virtual-machine.png)|[SQL Server na Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/)|SQL Server můžete provozovat v Azure Virtual Machines (na to se zaměřuje toto téma). Virtuální počítač můžete přímo spravovat a databázi je možné spouštět na prodejních verzích SQL Serveru. |
|![SQL Database](./media/virtual-machines-windows-sql-server-iaas-overview/azure-sql-database.png)|[SQL Database](https://azure.microsoft.com/services/sql-database/)|Pomocí služby SQL Database můžete přistupovat k databázi a škálovat ji bez nutnosti spravovat infrastrukturu, na které běží.|
|![SQL Data Warehouse](./media/virtual-machines-windows-sql-server-iaas-overview/azure-sql-data-warehouse.png)|[SQL Data Warehouse](https://azure.microsoft.com/en-us/services/sql-data-warehouse/)|Azure SQL Data Warehouse vám umožní zpracování velkých objemů relačních a nerelačních dat. Poskytuje možnosti škálovatelného datového skladu ve formě služby.|
|![SQL Server Stretch Database](./media/virtual-machines-windows-sql-server-iaas-overview/sql-server-stretch-database.png)|[SQL Server Stretch Database](https://azure.microsoft.com/en-us/services/sql-server-stretch-database/)|Lokální transakční data se dynamicky roztáhnou z Microsoft SQL Serveru 2016 do Azure.|

Vzhledem k těmto různým možnostem je SQL Server běžící na virtuálních počítačích Azure dobrou volbou pro několik scénářů. Například by mohlo být vhodné virtuální počítač Azure nakonfigurovat co nejpodobněji lokálnímu počítači s SQL Serverem. Nebo budete chtít na stejném databázovém serveru provozovat další aplikace a služby. K dispozici jsou dva zdroje informací, které vám můžou pomoci při zvažování dalších faktorů:

 - [SQL Server ve službě Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/) poskytuje přehled nejlepších scénářů použití SQL Serveru na virtuálních počítačích Azure. 
 - [Volba cloudového řešení systému SQL Server: Azure SQL (PaaS) Database nebo SQL Server na virtuálních počítačích Azure (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md) nabízí podrobné porovnání SQL Database a SQL Serveru běžícího na virtuálním počítači.

## <a name="create-a-new-sql-vm"></a>Vytvoření nového virtuálního počítače s SQL Serverem
Následující oddíly poskytují přímé odkazy na image v galerii virtuálních počítačů s SQL Serverem na webu Azure Portal. V závislosti na zvolené imagi můžete platit licenční náklady na SQL Server po minutách, nebo můžete používat vlastní licenci (BYOL).

Podrobné pokyny k tomuto procesu najdete v kurzu [Zřízení virtuálního počítače s SQL Serverem na webu Azure Portal](virtual-machines-windows-portal-sql-server-provision.md). Projděte si také téma [Osvědčené postupy z hlediska výkonu pro virtuální počítače s SQL Serverem](virtual-machines-windows-sql-performance.md), které vysvětluje, jak vybrat odpovídající velikost počítače a další funkce dostupné při zřizování.

## <a name="option-1:-create-a-sql-vm-with-per-minute-licensing"></a>Možnost 1: Vytvoření virtuálního počítače s SQL Serverem a licencováním po minutách
Následuje tabulka dostupných imagí SQL Serveru v galerii virtuálních počítačů. Po kliknutí na odkaz se začne vytvářet nový virtuální počítač SQL se zadanou verzí, edicí a operačním systémem.

|Verze|Operační systém|Edice|
|---|---|---|
|**SQL 2016**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [Dev](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**SQL 2014 SP1**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL 2014**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL 2012 SP3**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**SQL 2008 R2 SP3**|Windows Server 2008 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**SQL 2008 R2 SP3**|Windows Server 2012|[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|

## <a name="option-2:-create-a-sql-vm-with-an-existing-license"></a>Možnost 2: Vytvoření virtuálního počítače s SQL Serverem a stávající licencí
Můžete také používat vlastní licenci (BYOL). V tomto scénáři zaplatíte jenom za virtuální počítač bez jakýchkoli dalších poplatků za licencování SQL Serveru. Pokud budete chtít využívat vlastní licenci, použijte níže uvedenou tabulku verzí, edicí a operačních systémů SQL Serveru. Na portálu mají názvy těchto imagí předponu **{BYOL}**.

|Verze|Operační systém|Edice|
|---|---|---|
|**SQL Server 2016**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

> [AZURE.IMPORTANT] Abyste mohli používat image virtuálních počítačů BYOL, musíte mít smlouvu Enterprise s [Mobilitou licencí v rámci programu Software Assurance na platformě Azure](https://azure.microsoft.com/pricing/license-mobility/). Budete také potřebovat platnou licenci pro verzi nebo edici SQL Serveru, kterou budete chtít používat. Do **10** dnů od zřízení virtuálního počítače musíte [společnosti Microsoft předat nezbytné informace v souvislosti s BYOL](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf).

## <a name="manage-your-sql-vm"></a>Správa vašeho virtuálního počítače s SQL Serverem
Po zřízení virtuálního počítače s SQL Serverem je možné volitelně provést některé úlohy správy. V mnoha aspektech se SQL Server konfiguruje a spravuje úplně stejně jako místní instance SQL Serveru. Některé úlohy jsou ale pro Azure specifické. V následujících částech se na některé z těchto oblastí podíváme podrobněji a najdete v nich také odkazy na další informace.

### <a name="connect-to-the-vm"></a>Připojení k virtuálnímu počítači
Jedním z nejzákladnějších kroků správy je připojení k virtuálnímu počítači SQL Serveru pomocí nástrojů, jako je SQL Server Management Studio (SSMS). Pokyny pro připojení k novému virtuálnímu počítači s SQL Serverem najdete v tématu [Připojení k virtuálnímu počítači s SQL Serverem v Azure](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Migrace dat
Pokud máte existující databázi, bude ji nutné přesunout do nově zřízeného virtuálního počítače s SQL Serverem. Seznam možností migrace a pokyny najdete v části [Migrace databáze do SQL Serveru ve virtuálním počítači Azure](virtual-machines-windows-migrate-sql.md).

### <a name="configure-high-availability"></a>Nakonfigurování vysoké dostupnosti
Pokud požadujete vysokou dostupnost, doporučujeme nakonfigurovat skupiny dostupnosti SQL Serveru. Znamená to využívat více virtuálních počítačů Azure ve virtuální síti. Na Portálu Azure najdete šablonu, která vám tuto konfiguraci nastaví automaticky. Další informace najdete v tématu [Konfigurace skupiny dostupnosti AlwaysOn ve virtuálních počítačích Azure Resource Manageru](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). Pokud chcete skupiny dostupnosti a přidružený naslouchací proces nakonfigurovat ručně, najdete další informace v tématu [Konfigurace skupin dostupnosti AlwaysOn ve virtuálním počítači Azure](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Další důležité informace o tom, co je potřeba zvážit z hlediska vysoké dostupnosti, najdete v tématu [Vysoká dostupnost a zotavení po havárii pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

### <a name="back-up-your-data"></a>Zálohování dat
Virtuální počítače Azure mohou využívat výhod [automatizovaného zálohování](virtual-machines-windows-sql-automated-backup.md), které pravidelně vytváří zálohy databáze do úložiště blob storage. Tento postup můžete použít také ručně. Další informace najdete v tématu [Použití služby Azure Storage pro zálohování a obnovování SQL Serveru](virtual-machines-windows-use-storage-sql-server-backup-restore.md). Přehled všech možností zálohování a obnovení najdete v tématu [Zálohování a obnovení pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

### <a name="automate-updates"></a>Automatické aktualizace
Virtuální počítače Azure mohou používat [Automatické opravy](virtual-machines-windows-sql-automated-patching.md) k naplánování okna údržby pro automatickou instalaci důležitých oken a aktualizace SQL serveru.

### <a name="customer-experience-improvement-program-(ceip)"></a>Program Zlepšování softwaru a služeb na základě zkušeností uživatelů (CEIP)
Program Zlepšování softwaru a služeb na základě zkušeností uživatelů (CEIP) je ve výchozím nastavení povolen. Ten pravidelně odesílá zprávy do Microsoftu s cílem přispět k vylepšování SQL Serveru. S programem CEIP není spojená žádná nutná úloha správy – za předpokladu, že ho nechcete po zřízení zakázat. Tento program můžete přizpůsobit nebo vypnout připojením k virtuálnímu počítači pomocí vzdálené plochy. Pak spusťte nástroj **pro zasílání zpráv o chybách a vytváření sestav využití SQL Serveru**. Podle pokynů zakažte generování sestav. 

Další informace najdete v části věnované programu CEIP v tématu [Přijetí licenčních podmínek](https://msdn.microsoft.com/library/ms143343.aspx). 

## <a name="next-steps"></a>Další kroky
[Projděte si mapy kurzů](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) pro SQL Server na virtuálních počítačích Azure.

Máte nějaký další dotaz? Nejdříve si projděte [Nejčastější dotazy k SQL Serveru na Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-faq.md). Můžete ale také svoje dotazy nebo připomínky přidat do pole, které najdete v dolní části všech témat týkajících se virtuálních počítačů s SQL Serverem, a komunikovat se zástupci společnosti Microsoft a komunitou uživatelů.



<!--HONumber=Oct16_HO3-->


