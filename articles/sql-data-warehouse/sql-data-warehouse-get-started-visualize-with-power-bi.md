<properties
   pageTitle="Vizualizace dat SQL Data Warehouse pomocí Power BI – Microsoft Azure"
   description="Vizualizace dat SQL Data Warehouse pomocí Power BI"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor="" />

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="lodipalm;barbkess;sonyama" />

# Vizualizace dat pomocí Power BI

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

V tomto kurzu si ukážeme, jak se pomocí Power BI připojit k SQL Data Warehouse a vytvořit pár základních vizualizací.

> [AZURE.VIDEO azure-sql-data-warehouse-sample-data-and-powerbi]

## Požadavky

Pro jednotlivé kroky v tomto kurzu budete potřebovat:

- SQL Data Warehouse s předem načtenou databází AdventureWorksDW. Ke zřízení této konfigurace použijte článek [Vytvoření SQL Data Warehouse][] a zvolte načtení ukázkových dat. Pokud už Data Warehouse máte, ale nemáte ukázková data, můžete [ukázková data načíst ručně][].


## 1. Připojení k databázi

Pokud chcete otevřít Power BI a připojit se ke své databázi AdventureWorksDW, postupujte takto:

1. Přihlaste se k [Azure Portal][].
2. Klikněte na **Databáze SQL** a zvolte databázi AdventureWorks služby SQL Data Warehouse.

    ![Vyhledání databáze][1]

3. Klikněte na tlačítko Otevřít v Power BI.

    ![Tlačítko Power BI][2]

4. Teď by se vám měla zobrazit stránka pro připojení k SQL Data Warehouse s webovou adresou vaší databáze. Klikněte na Další.

    ![Připojení Power BI][3]

6. Zadejte své uživatelské jméno a heslo pro SQL Server Azure a budete plně připojení k vaší databázi SQL Data Warehouse.

    ![Přihlášení k Power BI][4]

7. Až se zaregistrujete v Power BI, klikněte v levém okně na datovou sadu AdventureWorksDW. Tím se otevře databáze.

    ![Otevření databáze AdventureWorksDW v Power BI][5]



## 2. Vytvoření sestavy

Teď můžete pomocí Power BI analyzovat ukázková data databáze AdventureWorksDW. K provedení analýzy má databáze AdventureWorksDW zobrazení s názvem AggregateSales. Toto zobrazení obsahuje několik klíčových metrik pro analýzu prodeje společnosti.

1. Pokud budete chtít vytvořit mapu částek prodeje podle PSČ, klikněte v podokně polí napravo na zobrazení AggregateSales a tím ho rozbalte. Kliknutím vyberte sloupce PostalCode a SalesAmount.

    ![Výběr sloupce AggregateSales v Power BI][6]

    Power BI automaticky rozpozná, že se jedná o zeměpisné údaje, a umístí vám je na mapu.

    ![Mapa Power BI][7]

2. Tento krok vytvoří pruhový graf, který zobrazí částku prodeje na příjem zákazníka. Toto můžete vytvořit v rozbaleném zobrazení AggregateSales. Klikněte na pole SalesAmount. Přetáhněte pole Customer Income (Příjem zákazníka) nalevo do části Axis (Osa).

    ![Výběr osy v Power BI][8]

    Pruhový graf jsme přesunuli vlevo.

    ![Pruhový graf v Power BI][9]

3. Tento krok vytvoří spojnicový graf zobrazující prodejní částku k datu objednávky. Toto můžete vytvořit v rozbaleném zobrazení AggregateSales. Klikněte na SalesAmount a OrderDate. Ve sloupci Visualizations (Vizualizace) klikněte na ikonu spojnicového grafu (je to první ikona na druhém řádku pod vizualizacemi).

    ![Výběr spojnicového grafu v Power BI][10]

    Teď máte sestavu zobrazující tři různé vizualizace dat.

    ![Spojnicový graf v Power BI][11]

Kdykoli můžete rozdělanou práci uložit tak, že kliknete na **Soubor** a vyberete **Uložit**.

## Další kroky
Vyzkoušeli jste si tedy práci s ukázkovými daty a teď se podívejte, jak na [vývoj][], [načítání][] nebo [migraci][]. Nebo se podívejte na [web Power BI][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migraci]: sql-data-warehouse-overview-migrate.md
[vývoj]: sql-data-warehouse-overview-develop.md
[načítání]: sql-data-warehouse-overview-load.md
[ukázková data načíst ručně]: sql-data-warehouse-load-sample-databases.md
[připojení k SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Vytvoření SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure Portal]: https://portal.azure.com/
[web Power BI]: http://www.powerbi.com/



<!---HONumber=Aug16_HO4-->


