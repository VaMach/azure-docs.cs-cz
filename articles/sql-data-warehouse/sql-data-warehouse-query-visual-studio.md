---
title: Dotazování Azure SQL Data Warehouse (Visual Studio) | Microsoft Docs
description: Dotazování SQL Data Warehouse pomocí sady Visual Studio.
services: sql-data-warehouse
documentationcenter: NA
author: sonyam
manager: barbkess
editor: ''

ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 06/16/2016
ms.author: sonyama;barbkess

---
# Dotazování Azure SQL Data Warehouse (Visual Studio)
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> 
> 

Použijte sadu Visual Studio k dotazování datového skladu SQL Azure během několika minut. Tato metoda používá rozšíření serveru SQL Server Data Tools (SSDT) v sadě Visual Studio. 

## Požadavky
Chcete-li použít tento kurz, potřebujete:

* Existující SQL Data Warehouse. Pokud chcete jeden vytvořit, podívejte se na téma [Vytvoření SQL Data Warehouse][Vytvoření SQL Data Warehouse].
* SSDT pro Visual Studio. Pokud máte aplikaci Visual Studio, pravděpodobně již databázi máte. Pokyny k instalaci a možnosti najdete v tématu věnovaném [instalaci sady Visual Studio a rozšíření SSDT][instalaci sady Visual Studio a rozšíření SSDT].
* Plně kvalifikovaný název serveru SQL. Ten zjistíte v části [Připojení k SQL Data Warehouse][Připojení k SQL Data Warehouse].

## 1. Připojení k vaší službě SQL Data Warehouse
1. Otevřete Visual Studio 2013 nebo 2015.
2. Otevřete Průzkumník objektů SQL Serveru. Uděláte to tak, že vyberte **Zobrazení** > **Průzkumník objektů systému SQL Server**.
   
    ![Průzkumník objektů systému SQL Server][1]
3. Klikněte na ikonu **Přidat SQL Server**.
   
    ![Přidat SQL Server][2]
4. Vyplňte pole v okně pro připojení k serveru.
   
    ![Připojení k serveru][3]
   
   * **Název serveru**: Zadejte **název serveru**, který jste si zjistili.
   * **Ověřování**: Vyberte **Ověřování serveru SQL Server** nebo **Integrované ověřování Active Directory**.
   * **Uživatelské jméno** a **Heslo**: Pokud jste výše vybrali možnost Ověřování serveru SQL Server, zadejte uživatelské jméno a heslo.
   * Klikněte na **Připojit**.
5. Pokud chcete SQL server Azure prozkoumat, rozbalte ho. Můžete se podívat, které databáze jsou k tomuto serveru přidružené. Rozbalte položku AdventureWorksDW a podívejte se na tabulky v ukázkové databázi.
   
    ![Prozkoumejte AdventureWorksDW.][4]

## 2. Spuštění ukázkového dotazu
Teď, když jste si vytvořili připojení k databázi, můžete napsat dotaz.

1. Pravým tlačítkem myši klikněte na databázi v Průzkumníku objektů systému SQL Server.
2. Vyberte **Nový dotaz**. Otevře se nové okno dotazu.
   
    ![Nový dotaz][5]
3. Zkopírujte tento dotaz TSQL do okna dotazu:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Spusťte dotaz. Pokud to chcete provést, klikněte na zelenou šipku nebo použijte následující klávesovou zkratku: `CTRL`+`SHIFT`+`E`.
   
    ![Spuštění dotazu][6]
5. Podívejte se na výsledky dotazu. V tomto příkladě má tabulka FactInternetSales 60 398 řádků.
   
    ![Výsledky dotazu][7]

## Další kroky
Teď, když se můžete připojit a můžete zadávat dotazy, můžete si vyzkoušet [vizualizaci dat pomocí PowerBI][vizualizaci dat pomocí PowerBI].

Informace o tom, jak nakonfigurovat prostředí pro ověřování služby Azure Active Directory naleznete v části [Ověřování pro SQL Data Warehouse][Ověřování pro SQL Data Warehouse].

<!--Arcticles-->
[Připojení k SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Vytvoření SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[instalaci sady Visual Studio a rozšíření SSDT]: sql-data-warehouse-install-visual-studio.md
[Ověřování pro SQL Data Warehouse]: sql-data-warehouse-authentication.md
[vizualizaci dat pomocí PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[portál Azure]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png



<!---HONumber=Aug16_HO4-->


