<properties
    pageTitle="Připojení k SQL Database pomocí dotazu jazyka C# | Microsoft Azure"
    description="Napište v jazyce C# program, který vám umožní připojit se k databázi SQL a zadávat dotazy na data, která obsahuje. Informace o IP adresách, připojovacích řetězcích, zabezpečeném přihlašování a sadě Visual Studio zdarma."
    services="sql-database"
    keywords="databázový dotaz c#, dotaz c#, připojení k databázi, SQL C#"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="stevestein"/>



# Připojení k SQL Database pomocí sady Visual Studio

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Přečtěte si, jak se připojit k Azure SQL Database v sadě Visual Studio. 

## Požadavky


Abyste se mohli připojit k databázi SQL pomocí sady Visual Studio, potřebujete následující položky: 


- Databázi SQL, k níž se chcete připojit. V tomto článku se používá ukázková databáze **AdventureWorks**. Ukázkovou databázi AdventureWorks můžete získat podle popisu v článku [Vytvoření ukázkovou databázi](sql-database-get-started.md).


- Visual Studio 2013 s aktualizací Update 4 (nebo vyšší). Microsoft teď nabízí *zdarma* Visual Studio Community. 
 - [Visual Studio Community, stažení](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [Další možnosti získání bezplatné verze sady Visual Studio](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)




## Spuštění sady Visual Studio z webu Azure Portal


1. Přihlaste se k [portálu Azure](https://portal.azure.com/).

2. Klikněte na možnost **Další služby** > **Databáze SQL**
3. Otevřete okno databáze **AdventureWorks** po vyhledání databáze *AdventureWorks* a kliknutí na ni.

6. Klikněte na tlačítko **Nástroje** v horní části okna databáze:

    ![Nový dotaz Připojení k serveru služby SQL Database: SQL Server Management Studio](./media/sql-database-connect-query/tools.png)

7. Klikněte na tlačítko **Otevřít ve Visual Studiu** (pokud budete potřebovat sadu Visual Studio, klikněte na odkaz ke stažení):

    ![Nový dotaz Připojení k serveru služby SQL Database: SQL Server Management Studio](./media/sql-database-connect-query/open-in-vs.png)


8. Visual Studio se spustí a v okně **Připojit k serveru** již bude nastaveno připojení k serveru a databázi, které jste vybrali na portálu.  (Po kliknutí na **Možnosti** můžete ověřit, že je nastaveno připojení ke správné databázi.) Zadejte heslo správce serveru a klikněte na **Připojit**.


    ![Nový dotaz Připojení k serveru služby SQL Database: SQL Server Management Studio](./media/sql-database-connect-query/connect.png)


8. Pokud nemáte nastavené pravidlo brány firewall pro IP adresu počítače, může se zde zobrazit zpráva *Připojení nelze provést*. Chcete-li vytvořit pravidlo brány firewall, najdete informace v tématu [Konfigurace pravidla brány firewall na úrovni serveru SQL Azure Database](sql-database-configure-firewall-settings.md).


9. Po úspěšném připojení se otevře okno **Průzkumník objektů systému SQL Server** s připojením k vaší databázi.

    ![Nový dotaz Připojení k serveru služby SQL Database: SQL Server Management Studio](./media/sql-database-connect-query/sql-server-object-explorer.png)


## Spuštění ukázkového dotazu

Po připojení k databázi nyní následující kroky ilustrují, jak spustit jednoduchý dotaz:

2. Klikněte na databázi pravým tlačítkem myši a pak klikněte na **Nový dotaz**.

    ![Nový dotaz Připojení k serveru služby SQL Database: SQL Server Management Studio](./media/sql-database-connect-query/new-query.png)

3. Zkopírujte následující kód a vložte ho do okna dotazu.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. Spusťte dotaz kliknutím na tlačítko **Spustit**:

    ![Úspěch Připojení k serveru SQL Database: SVisual Studio](./media/sql-database-connect-query/run-query.png)

## Další kroky

- K otevírání databází SQL v sadě Visual Studio se používají nástroje SQL Server Data Tools. Podrobnější informace najdete v tématu [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686.aspx).
- Chcete-li se připojit k databázi SQL s použitím kódu, najdete informace v části [Připojení k SQL Database s použitím rozhraní .NET (C#)](sql-database-develop-dotnet-simple.md).






<!--HONumber=ago16_HO5-->


