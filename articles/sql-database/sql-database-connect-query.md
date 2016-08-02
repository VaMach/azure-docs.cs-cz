<properties
    pageTitle="Připojení k SQL Database pomocí dotazu jazyka C# | Microsoft Azure"
    description="Napište v jazyce C# program, který vám umožní připojit se k databázi SQL a zadávat dotazy na data, která obsahuje. Informace o IP adresách, připojovacích řetězcích, zabezpečeném přihlašování a sadě Visual Studio zdarma."
    services="sql-database"
    keywords="c# database query, c# query, connect to database, SQL C#"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="04/25/2016"
    ms.author="annemill"/>


# Připojení k SQL Database pomocí sady Visual Studio

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Přečtěte si, jak se připojit k Azure SQL Database v sadě Visual Studio. 

## Požadavky


Abyste se mohli připojit k SQL Database pomocí sady Visual Studio, musíte mít: 


- Účet a předplatné Azure. Můžete si zaregistrovat i [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).


- Ukázkovou databázi **AdventureWorksLT** služby Azure SQL Database.
 - [Vytvoření ukázkové databáze](sql-database-get-started.md) během pár minut.


- Visual Studio 2013 s aktualizací Update 4 (nebo vyšší). Microsoft teď nabízí *zdarma* Visual Studio Community. 
 - [Visual Studio Community, stažení](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [Další možnosti získání bezplatné verze sady Visual Studio](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)
 - Nebo se můžete podívat, jak tento [krok](#InstallVSForFree) dále v tomto tématu popisuje, jak vás [portál Azure](https://portal.azure.com/) provede instalací sady Visual Studio.


<a name="InstallVSForFree" id="InstallVSForFree"></a>

&nbsp;

## Krok 1: Instalace bezplatné verze Visual Studio Community


Pokud potřebujete instalaci sady Visual Studio, můžete:

- Nainstalovat zdarma verzi Visual Studio Community, a to tak, že přejdete v prohlížeči na webové stránky produktu Visual Studio se soubory, které jsou zdarma ke stažení, a dalšími možnostmi, nebo
- Nechat se [portálem Azure](https://portal.azure.com/) navést na webovou stránku se soubory ke stažení, která je popsána dále.


### Visual Studio prostřednictvím portálu Azure


1. Přihlaste se k [portálu Azure](https://portal.azure.com/) (http://portal.azure.com/).

2. Klikněte na **PROCHÁZET* VŠE** > **Databáze SQL**. Otevře se okno umožňující hledat databáze.

3. Do textového pole filtru v horní začněte psát název vaší databáze **AdventureWorksLT**.

4. Pokud uvidíte řádek pro vaši databázi na serveru, klikněte na něj. Otevře se okno pro vaši databázi.

5. Abyste měli zobrazení přehlednější, klikněte na každém z předchozích oken na ovládací prvek pro minimalizaci.

6. V horní části okna databáze klikněte na tlačítko **Otevřít v sadě Visual Studio**. Otevře se nové okno o sadě Visual Studio s odkazy na umístění instalace pro Visual Studio.

    ![Tlačítko Otevřít v sadě Visual Studio][20-OpenInVisualStudioButton]

7. Klikněte na odkaz **Verze Community (zdarma)** nebo na podobný odkaz. Přidá se nová webová stránka.

8. Pomocí odkazů na nové webové stránce nainstalujte sadu Visual Studio.

9. Po nainstalování sady Visual Studio klikněte v okně **Otevřít v sadě Visual Studio** na tlačítko **Otevřít v sadě Visual Studio**. Otevře se Visual Studio.

10. Pro podokno **Průzkumníka objektů systému SQL Serveru** vás sada Visual Studio vyzve k vyplnění polí připojovacího řetězce v dialogu.
 - Zvolte **Ověřování SQL Serveru**, nikoli **Ověřování systému Windows**.
 - Nezapomeňte zadat svoji databázi **AdventureWorksLT** (**Možnosti** > **Vlastnosti připojení** v dialogovém okně).

11. V **Průzkumníku objektů SQL Serveru** rozbalte uzel vaší databáze.


## Krok 2: Spuštění ukázkových dotazů

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

    ![Úspěch Připojení k serveru SQL Database: SVisual Studio](./media/sql-database-connect-query-ssms/5-success.png)

## Další kroky

[Připojení k SQL Database s použitím rozhraní .NET (C#)](sql-database-develop-dotnet-simple.md) 


<!-- Image references. -->

[20-OpenInVisualStudioButton]: ./media/sql-database-connect-query/connqry-free-vs-e.png




<!--HONumber=Jun16_HO2-->


