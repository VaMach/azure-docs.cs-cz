---
title: "Načtení dat do SQL Data Warehouse pomocí nástroje Data Platform Studio od společnosti Redgate | Dokumentace Microsoftu"
description: "Naučte se používat Data Platform Studio od společnosti Redgate ve scénářích datového skladu."
services: sql-data-warehouse
documentationcenter: NA
author: twounder
manager: jhubbard
editor: 
ms.assetid: 670aef98-31f7-4436-86c0-cc989a39fe7f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: mausher;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 804b0ba72ca73da1a076bd4dbca8bbca33cb76b1


---
# <a name="load-data-with-redgate-data-platform-studio"></a>Načtení dat pomocí nástroje Redgate Data Platform Studio
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
> * [BCP](sql-data-warehouse-load-with-bcp.md)
> 
> 

V tomto kurzu se dozvíte, jak pomocí nástroje [Data Platform Studio od společnosti Redgate](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DPS) přesunout data z lokálního SQL Serveru do Azure SQL Data Warehouse. Data Platform Studio použije nejvhodnější opravy pro kompatibilitu a další optimalizace, takže je to nejrychlejší způsob, jak začít pracovat se službou SQL Data Warehouse.

> [!NOTE]
> [Redgate](http://www.red-gate.com) je dlouhodobý partner Microsoftu, který poskytuje různé nástroje pro SQL Server. Tato funkce byla v nástroji Data Platform Studio zpřístupněna bezplatně pro komerční i nekomerční použití.
> 
> 

## <a name="before-you-begin"></a>Než začnete
### <a name="create-or-identify-resources"></a>Vytvoření nebo určení prostředků
Před zahájením tohoto kurzu musíte mít tyto prostředky:

* **Lokální databáze SQL Serveru:** Data, která chcete importovat do SQL Data Warehouse, musí pocházet z lokálního SQL Serveru (verze 2008 R2 nebo vyšší). Data Platform Studio nemůže importovat data přímo z Azure SQL Database nebo z textových souborů.
* **Účet Azure Storage:** Data Platform Studio připraví data v Azure Blob Storage před jejich načtením do SQL Data Warehouse. Účet úložiště musí používat model nasazení typu Správce prostředků (výchozí), ne model nasazení Classic. Pokud účet úložiště ještě nemáte, podívejte se, jak si ho vytvořit. 
* **SQL Data Warehouse:** V tomto kurzu se provádí přesun dat z lokálního SQL Serveru SQL do SQL Data Warehouse, takže musíte mít online datový sklad. Pokud ještě datový sklad nemáte, přečtěte si, jak si vytvořit Azure SQL Data Warehouse.

> [!NOTE]
> Pokud jsou účet úložiště a datový sklad vytvořeny ve stejné oblasti, zlepší se tím výkon.
> 
> 

## <a name="step-1-sign-in-to-data-platform-studio-with-your-azure-account"></a>Krok 1: Přihlášení k nástroji Data Platform Studio pomocí vašeho účtu Azure
Otevřete webový prohlížeč a přejděte na web [Data Platform Studio](https://www.dataplatformstudio.com/). Přihlaste se pomocí stejného účtu Azure, který jste použili k vytvoření účtu úložiště a datového skladu. Pokud je vaše e-mailová adresa přidružená k pracovnímu nebo školními účtu i účtu Microsoft, je nutné zvolit účet, který má přístup k vašim prostředkům.

> [!NOTE]
> Pokud Data Platform Studio používáte poprvé, zobrazí se žádost, abyste této aplikaci udělili oprávnění ke správě prostředků Azure.
> 
> 

## <a name="step-2-start-the-import-wizard"></a>Krok 2: Spuštění průvodce importem
Z hlavní obrazovky DPS vyberte odkaz pro import do Azure SQL Data Warehouse – tím spustíte průvodce importem.

![][1]

## <a name="step-3-install-the-data-platform-studio-gateway"></a>Krok 3: Instalace brány pro Data Platform Studio
Pro připojení k lokální databázi SQL Serveru musíte nainstalovat bránu DPS. Brána je klientský agent, který poskytuje přístup k lokálnímu prostředí, extrahuje data a nahrává je na váš účet úložiště. Vaše data nikdy neprocházejí přes servery společnosti Redgate. Instalace brány:

1. Klikněte na odkaz **Vytvořit bránu**.
2. Stáhněte a nainstalujte si bránu pomocí poskytnutého instalačního programu.

![][2]

> [!NOTE]
> Bránu je možné nainstalovat na jakýkoli počítač se síťovým přístupem ke zdrojové databázi SQL Serveru. K databázi SQL Serveru přistupuje na základě ověřování Windows pomocí přihlašovacích údajů aktuálního uživatele.
> 
> 

Po instalaci se stav brány změní na Připojeno a vy můžete vybrat tlačítko Další.

## <a name="step-4-identify-the-source-database"></a>Krok 4: Určení zdrojové databáze
Do textového pole *Zadat název serveru* zadejte název serveru, který je hostitelem databáze, a vyberte **Další**. V rozevírací nabídce pak vyberte databázi, ze které chcete data importovat.

![][3]

DPS ve vybrané databázi vyhledá tabulky k importu. Ve výchozím nastavení DPS importuje všechny tabulky v databázi. Tabulky můžete vybrat nebo jejich výběr zrušit po rozbalení odkazu Všechny tabulky. Výběrem tlačítka Další přejdete dále.

## <a name="step-5-choose-a-storage-account-to-stage-the-data"></a>Krok 5: Volba účtu úložiště pro přípravu dat
DPS vás vyzve k zadání umístění, kde se mají data připravit. Vyberte existující účet úložiště z vašeho předplatného a pak zvolte **Další**.

> [!NOTE]
> DPS vytvoří nový kontejner objektů blob ve zvoleném účtu úložiště a použije odlišnou složku pro každý import.
> 
> 

![][4]

## <a name="step-6-select-a-data-warehouse"></a>Krok 6: Výběr datového skladu
Potom vyberete online databázi [Azure SQL Data Warehouse](http://aka.ms/sqldw), do které chcete data naimportovat. Po výběru databáze musíte zadat přihlašovací údaje pro připojení k databázi a vybrat **Další**.

![][5]

> [!NOTE]
> DPS sloučí zdrojové datové tabulky do datového skladu. DPS vás upozorní, pokud je na základě názvu tabulky nutné přepsat existující tabulku v datovém skladu. Volitelně můžete v datovém skladu ještě před importem odstranit všechny existující objekty, a to zaškrtnutím příslušného políčka.
> 
> 

## <a name="step-7-import-the-data"></a>Krok 7: Import dat
DPS si ověří, že chcete data importovat. Jednoduše klikněte na tlačítko Zahájit import.

![][6]

DPS zobrazuje vizualizaci průběhu extrahování a nahrávání dat z lokálního serveru SQL Server a importu do SQL Data Warehouse.

![][7]

Po dokončení této operace zobrazí DPS souhrn datového importu a sestavu změn souvisejících s opravami kompatibility, které byly provedeny.

![][8]

## <a name="next-steps"></a>Další kroky
Pokud chcete prozkoumat data v SQL Data Warehouse, projděte si tyto zdroje informací:

* [Dotazování Azure SQL Data Warehouse (Visual Studio)][Dotazování Azure SQL Data Warehouse (Visual Studio)]
* [Vizualizace dat pomocí Power BI][Vizualizace dat pomocí Power BI]

Další informace o nástroji Data Platform Studio od společnosti Redgate:

* [Domovská stránka DPS](http://www.dataplatformstudio.com/)
* [Ukázka DPS na Channel 9](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

Přehled jiných způsobů migrace a načítání dat do SQL Data Warehouse:

* [Migrace řešení do SQL Data Warehouse][Migrace řešení do SQL Data Warehouse]
* [Načtení dat do Azure SQL Data Warehouse](sql-data-warehouse-overview-load.md)

Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse](sql-data-warehouse-overview-develop.md).

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Dotazování Azure SQL Data Warehouse (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Vizualizace dat pomocí Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Migrace řešení do SQL Data Warehouse]: ./sql-data-warehouse-overview-migrate.md
[Načtení dat do Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[Přehled vývoje SQL Data Warehouse]: ./sql-data-warehouse-overview-develop.md



<!--HONumber=Nov16_HO2-->


