<properties
   pageTitle="Připojení k SQL Data Warehouse pomocí sady Visual Studio | Microsoft Azure"
   description="Začněte tím, že se připojíte k SQL Data Warehouse a spustíte pár dotazů."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/13/2016"
   ms.author="sonyama;barbkess"/>

# Připojení k SQL Data Warehouse pomocí sady Visual Studio

> [AZURE.SELECTOR]
- [Visual Studio](sql-data-warehouse-get-started-connect.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md)
- [AAD](sql-data-warehouse-get-started-connect-aad-authentication.md)

Tento návod ukazuje, jak se během pár minut připojit k Azure SQL Data Warehouse pomocí rozšíření SQL Server Data Tools (SSDT) v sadě Visual Studio. Po připojení spustíte jednoduchý dotaz.

## Požadavky

+ Ukázková data AdventureWorksDW v SQL Data Warehouse. Pokud si je chcete vytvořit, podívejte se na téma [Vytvoření SQL Data Warehouse][].
+ Rozšíření SQL Server Data Tools for Visual Studio. Pokyny k instalaci a možnosti najdete v tématu věnovaném [instalaci sady Visual Studio a rozšíření SSDT][].

## Krok 1: Zjištění plně kvalifikovaného názvu SQL serveru Azure

Vaše databáze SQL Data Warehouse je přidružená k Azure SQL Serveru. K připojení ke své databázi potřebujete plně kvalifikovaný název serveru (**název_serveru**. database.windows.net*).

Plně kvalifikovaný název serveru zjistíte následujícím způsobem.

1. Přejděte na [portál Azure][].
2. Klikněte na **Databáze SQL** a klikněte na databázi, ke které se chcete připojit. V tomto příkladě se používá ukázková databáze AdventureWorksDW.
3. Vyhledejte úplný název serveru.

    ![Úplný název serveru][1]

## Krok 2: Připojení k vaší službě SQL Data Warehouse

1. Otevřete Visual Studio 2013 nebo 2015.
2. Otevřete Průzkumník objektů SQL Serveru. Uděláte to tak, že vyberte **Zobrazení** > **Průzkumník objektů systému SQL Server**.

    ![Průzkumník objektů systému SQL Server][2]

3. Klikněte na ikonu **Přidat SQL Server**.

    ![Přidat SQL Server][3]

4. Vyplňte pole v okně pro připojení k serveru.

    ![Připojení k serveru][4]

    - **Název serveru**: Zadejte **název serveru**, který jste si zjistili.
    - **Ověřování**: Vyberte **Ověřování serveru SQL Server** nebo **Integrované ověřování Active Directory**.
    - **Uživatelské jméno** a **Heslo**: Pokud jste výše vybrali možnost Ověřování serveru SQL Server, zadejte uživatelské jméno a heslo.
    - Klikněte na **Připojit**.

5. Pokud chcete SQL server Azure prozkoumat, rozbalte ho. Můžete se podívat, které databáze jsou k tomuto serveru přidružené. Rozbalte položku AdventureWorksDW a podívejte se na tabulky v ukázkové databázi.

    ![Prozkoumejte AdventureWorksDW.][5]

## Krok 3: Spuštění ukázkového dotazu

Teď, když jste si vytvořili připojení k databázi, můžete napsat dotaz.

1. Pravým tlačítkem myši klikněte na databázi v Průzkumníku objektů systému SQL Server.

2. Vyberte **Nový dotaz**. Otevře se nové okno dotazu.

    ![Nový dotaz][6]

3. Zkopírujte tento dotaz TSQL do okna dotazu:

    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. Spusťte dotaz. Pokud to chcete provést, klikněte na zelenou šipku nebo použijte následující klávesovou zkratku: `CTRL`+`SHIFT`+`E`.

    ![Spuštění dotazu][7]

5. Podívejte se na výsledky dotazu. V tomto příkladě má tabulka FactInternetSales 60 398 řádků.

    ![Výsledky dotazu][8]

## Další kroky

Teď, když se můžete připojit a můžete zadávat dotazy, můžete si vyzkoušet [vizualizaci dat pomocí PowerBI][].

Informace o tom, jak si pro prostředí nakonfigurovat ověřování systému Windows, najdete v tématu [Připojení k SQL Database nebo SQL Data Warehouse pomocí ověřování služby Azure Active Directory][].

<!--Arcticles-->
[Vytvoření SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[instalaci sady Visual Studio a rozšíření SSDT]: sql-data-warehouse-install-visual-studio.md
[Připojení k SQL Database nebo SQL Data Warehouse pomocí ověřování služby Azure Active Directory]: ../sql-data-warehouse/sql-data-warehouse-get-started-connect-aad-authentication.md
[vizualizaci dat pomocí PowerBI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[portál Azure]: https://portal.azure.com

<!--Image references-->

[1]: ./media/sql-data-warehouse-get-started-connect/get-server-name.png
[2]: ./media/sql-data-warehouse-get-started-connect/open-ssdt.png
[3]: ./media/sql-data-warehouse-get-started-connect/add-server.png
[4]: ./media/sql-data-warehouse-get-started-connect/connection-dialog.png
[5]: ./media/sql-data-warehouse-get-started-connect/explore-sample.png
[6]: ./media/sql-data-warehouse-get-started-connect/new-query2.png
[7]: ./media/sql-data-warehouse-get-started-connect/run-query.png
[8]: ./media/sql-data-warehouse-get-started-connect/query-results.png



<!--HONumber=Jun16_HO2-->


