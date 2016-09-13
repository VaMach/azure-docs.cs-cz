<properties
    pageTitle="Kurz k SQL Database: Vytvoření databáze SQL | Microsoft Azure"
    description="Zjistěte, jak nastavit logický server služby SQL Database, pravidlo brány firewall serveru, databáze SQL a ukázková data. Zjistíte také, jak se připojit pomocí nástrojů klienta, konfigurovat uživatele a nastavit pravidlo brány firewall databáze."
    keywords="kurz k sql database, vytvoření databáze sql"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/05/2016"
    ms.author="carlrab"/>


# Kurz k SQL Database: Vytvoření databáze SQL během pár minut pomocí webu Azure Portal

**Izolovaná databáze**

> [AZURE.SELECTOR]
- [portál Azure](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

V tomto kurzu se naučíte, jak pomocí portálu Azure Portal:

- Vytvořit logický server služby SQL Database pro hostování databází SQL,
- vytvořit databázi SQL bez dat, s ukázkovými daty nebo s daty ze zálohy databáze SQL,
- vytvořit pravidlo brány firewall na úrovni serveru pro jednu IP adresu nebo rozsah IP adres.

Stejné úlohy můžete provádět i pomocí jazyka [C#](sql-database-get-started-csharp.md) nebo prostředí [PowerShell](sql-database-get-started-powershell.md).

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

<a name="create-logical-server-bk"></a>

[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-create-new-server-portal.md)]

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-portal.md)]

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-server-firewall-portal.md)]

## Další kroky
Nyní jste dokončili kurz k SQL Database a vytvořili jste databázi s ukázkovými daty. Jste tedy připraveni prozkoumávat pomocí svých oblíbených nástrojů.

- Pokud jste obeznámeni s jazykem Transact-SQL a aplikací SQL Server Management Studio (SSMS), zjistěte, jak [se pomocí SSMS k databázi SQL připojit a zadávat dotazy](sql-database-connect-query-ssms.md).

- Pokud znáte Excel, zjistěte, jak se [připojit k databázi SQL v Azure pomocí aplikace Excel](sql-database-connect-excel.md).

- Jste-li připraveni na psaní kódu, zvolte si programovací jazyk v tématu [Knihovny pro připojení ke službě SQL Database a systému SQL Server](sql-database-libraries.md).

- Chcete-li do Azure přesunout databáze z místního systému SQL Server, více zjistíte v tématu [Migrace databáze do služby SQL Database](sql-database-cloud-migrate.md).

- Chcete-li do nové tabulky načíst data ze souboru CSV pomocí nástroje příkazového řádku BCP, více zjistíte v tématu [Načtení dat do služby SQL Database ze souboru CSV pomocí BCP](sql-database-load-from-csv-with-bcp.md).


## Další zdroje

[Co je SQL Database?](sql-database-technical-overview.md)



<!--HONumber=sep16_HO1-->


