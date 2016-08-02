<properties
    pageTitle="Připojení k SQL Database – SQL Server Management Studio | Microsoft Azure"
    description="Zjistěte, jak se připojit k SQL Database na Azure pomocí služby SQL Server Management Studio (SSMS). Potom spusťte ukázkový dotaz pomocí jazyka Transact-SQL (T-SQL)."
    metaCanonical=""
    keywords="connect to sql database,sql server management studio"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="" />

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/09/2016"
    ms.author="sstein;carlrab" />

# Připojení k SQL Database přes SQL Server Management Studio a provedení ukázkového dotazu T-SQL

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Tento článek ukazuje, jak se připojit k databázi Azure SQL pomocí nejnovější verze služby SQL Server Management Studio (SSMS) a provést jednoduchý dotaz s příkazy jazyka Transact-SQL (T-SQL).

[AZURE.INCLUDE [Sign in](../../includes/azure-getting-started-portal-login.md)]

[AZURE.INCLUDE [SSMS Install](../../includes/sql-server-management-studio-install.md)]

[AZURE.INCLUDE [SSMS Connect](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]

Informace o pravidlech brány firewall najdete v tématu [Postupy: Konfigurace nastavení brány firewall (Azure SQL Database)](sql-database-configure-firewall-settings.md).

## Spuštění ukázkových dotazů

Po připojení k logickému serveru se můžete připojit k databázi a spustit ukázkový dotaz. 

1. V **Průzkumníku objektů** přejděte na databázi na serveru, ke které máte oprávnění, například k ukázkové databázi **AdventureWorks**.
2. Klikněte na databázi pravým tlačítkem myši a pak klikněte na **Nový dotaz**.

    ![Nový dotaz Připojení k serveru služby SQL Database: SQL Server Management Studio](./media/sql-database-connect-query-ssms/4-run-query.png)

3. Zkopírujte následující kód a vložte ho do okna dotazu.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. Klikněte na tlačítko **Provést**.  Na následujícím snímku obrazovky vidíte úspěšný dotaz.

    ![Úspěch Připojení k serveru služby SQL Database: SQL Server Management Studio](./media/sql-database-connect-query-ssms/5-success.png)

## Další kroky

Pomocí příkazů T-SQL můžete vytvářet a spravovat databáze v Azure v podstatě stejným způsobem jako pomocí SQL Serveru. Pokud umíte používat T-SQL s SQL Serverem, najdete v tématu [Informace o příkazech jazyka Transact-SQL služby Azure SQL Database)](sql-database-transact-sql-information.md) souhrnné informace o rozdílech.

Pokud s T-SQL teprve začínáte, projděte si [Kurz: Psaní příkazů Transact-SQL](https://msdn.microsoft.com/library/ms365303.aspx) a [Referenční informace k Transact-SQL (databázový stroj)](https://msdn.microsoft.com/library/bb510741.aspx).

Informace o tom, jak začít s vytvářením uživatelů a správců databází, najdete v tématu [Začínáme se zabezpečení služby Azure SQL Database](sql-database-get-started-security.md).



<!--HONumber=Jun16_HO2-->


