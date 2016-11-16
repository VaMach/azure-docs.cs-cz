---
title: "Dotazování Azure SQL Data Warehouse (Visual Studio) | Dokumentace Microsoftu"
description: "Dotazování SQL Data Warehouse pomocí sady Visual Studio."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: daace889-95e5-4826-b2fc-047eac9d6d95
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5e97fefdf3cc13f2fd3e060da901e90ef5ef9a29


---
# <a name="query-azure-sql-data-warehouse-visual-studio"></a>Dotazování Azure SQL Data Warehouse (Visual Studio)
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Použijte sadu Visual Studio k dotazování datového skladu SQL Azure během několika minut. Tato metoda používá rozšíření serveru SQL Server Data Tools (SSDT) v sadě Visual Studio. 

## <a name="prerequisites"></a>Požadavky
Chcete-li použít tento kurz, potřebujete:

* Existující SQL Data Warehouse. Postup jeho vytvoření najdete v tématu [Vytvoření SQL Data Warehouse][Vytvoření SQL Data Warehouse].
* SSDT pro Visual Studio. Pokud máte aplikaci Visual Studio, pravděpodobně již databázi máte. Pokyny k instalaci a možnosti najdete v tématu věnovaném [instalaci sady Visual Studio a rozšíření SSDT][Instalace sady Visual Studio a rozšíření SSDT].
* Plně kvalifikovaný název serveru SQL. Ten zjistíte v části [Připojení k SQL Data Warehouse][Připojení k SQL Data Warehouse].

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. Připojení k vaší službě SQL Data Warehouse
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

## <a name="2-run-a-sample-query"></a>2. Spuštění ukázkového dotazu
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
5. Podívejte se na výsledky dotazu. V tomto příkladě má tabulka FactInternetSales 60 398 řádků.
   
    ![Výsledky dotazu][7]

## <a name="next-steps"></a>Další kroky
Teď, když se můžete připojit a můžete zadávat dotazy, můžete si vyzkoušet [vizualizaci dat pomocí PowerBI][Vizualizace dat pomocí PowerBI].

Informace o tom, jak nakonfigurovat prostředí pro ověřování služby Azure Active Directory najdete v části [Ověřování pro SQL Data Warehouse][Ověřování pro SQL Data Warehouse].

<!--Arcticles-->
[Připojení k SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Vytvoření SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Instalace sady Visual Studio a rozšíření SSDT]: sql-data-warehouse-install-visual-studio.md
[Ověřování pro SQL Data Warehouse]: sql-data-warehouse-authentication.md
[Vizualizace dat pomocí PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure Portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png



<!--HONumber=Nov16_HO2-->


