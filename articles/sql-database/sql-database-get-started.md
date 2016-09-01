<properties
    pageTitle="Kurz k SQL Database: Vytvoření databáze SQL | Microsoft Azure"
    description="Naučte se, jak vytvářet logické servery služby SQL Database, pravidla brány firewall, databáze SQL, vzorová data, jak se propojovat s klientskými nástroji a jak konfigurovat uživatele a pravidla brány firewall pro databázi."
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

# Kurz k SQL Database: Vytvoření databáze SQL během pár minut pomocí portálu Azure

**Izolovaná databáze**

> [AZURE.SELECTOR]
- [portál Azure](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

V tomto kurzu se naučíte, jak pomocí portálu Azure Portal:

- vytvořit logický server služby SQL Database pro hostování databází SQL,
- vytvořit databázi SQL bez dat, s ukázkovými daty nebo s daty ze zálohy databáze SQL,
- vytvořit pravidlo brány firewall na úrovni serveru pro jednu IP adresu nebo rozsah IP adres.

Pomocí těchto odkazů můžete stejné úlohy provádět buď pomocí [C#](sql-database-get-started-csharp.md), nebo v [prostředí PowerShell](sql-database-get-started-powershell.md).

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

<a name="create-logical-server-bk"></a>

[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-create-new-server-portal.md)]

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-portal.md)]

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-server-firewall-portal.md)]

## Další kroky
Nyní jste dokončili kurz SQL Database a vytvořili jste databázi s ukázkovými daty. Jste tedy připraveni na zkoumání použití některých oblíbených nástrojů.

- Pokud jste obeznámeni s Transact-SQL a SQL Server Management Studiem, naučte se, jak [se k databázi SQL připojit a zadávat dotazy pomocí SSMS](sql-database-connect-query-ssms.md).

- Pokud znáte dobře Excel, přečtěte si, jak se [připojit k databázi SQL z Excelu](sql-database-connect-excel.md).

- Jestli jste už připravení na psaní kódu, zvolte si programovací jazyk v článku [Knihovny pro připojení ke službě SQL Database a serveru SQL Server](sql-database-libraries.md)

- Pokud do služby Azure přesouváte databáze z vašeho místního SQL Serveru, najdete další informace v článku [Migrace databáze do služby Azure SQL Database](sql-database-cloud-migrate.md).

- Pokud chcete do nové tabulky načíst data ze souboru CSV pomocí BCP, přečtěte si článek [Načtení dat do databáze SQL ze souboru CSV pomocí BCP](sql-database-load-from-csv-with-bcp.md).


## Další zdroje

[Co je SQL Database?](sql-database-technical-overview.md)




<!---HONumber=Aug16_HO4-->


