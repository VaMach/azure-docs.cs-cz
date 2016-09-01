<properties
    pageTitle="Připojení Excelu k SQL Database | Microsoft Azure"
    description="Zjistěte, jak připojit Microsoft Excel k databázi SQL Azure v cloudu. Naimportujte si data do Excelu, kde můžete data dále zkoumat a vytvářet z nich sestavy."
    services="sql-database"
    keywords="připojení excelu k sql, import dat do excelu"
    documentationCenter=""
    authors="joseidz"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/05/2016"
    ms.author="joseidz"/>


# Kurz k SQL Database: Připojení Excelu k Azure SQL Database a vytvoření sestavy

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Přečtěte si, jak připojit Excel k databázi SQL v cloudu, abyste mohli naimportovat data a vytvářet tabulky a grafy na základě hodnot v databázi. V tomto kurzu nastavíte připojení mezi Excelem a databázovou tabulkou, uložíte soubor, ve kterém jsou uložená data a informace o připojení pro Excel, a pak z hodnot v databázi vytvoříte kontingenční graf.

Než začnete, budete potřebovat databázi SQL v Azure. Pokud žádnou nemáte, podívejte se na téma [Vytvoření první databáze SQL](sql-database-get-started.md), kde najdete informace o tom, jak si během několika málo minut zprovoznit databázi s ukázkovými daty. V tomto článku naimportujete do Excelu ukázková data z tohoto článku, ale podobným způsobem můžete zpracovat i svoje vlastní data.

Budete také potřebovat Excel. V tomto článku používáme [Microsoft Excel 2016](https://products.office.com/en-US/).

## Připojení Excelu k databázi SQL a vytvoření souboru odc

1.  Když budete chtít Excel připojit k databázi SQL, otevřete ho a vytvořte nový sešit nebo otevřete existující excelový sešit.

2.  V řádku nabídek v horní části stránky klikněte na **Data**, **Z jiných zdrojů** a potom klikněte na **Z SQL Serveru**.

    ![Výběr zdroje dat: Připojení Excelu k databázi SQL](./media/sql-database-connect-excel/excel_data_source.png)

    Otevře se Průvodce datovým připojením.

3.  V dialogovém okně **Připojit k databázovému serveru** zadejte **název serveru** SQL Database, ke kterému se chcete připojit, v následující podobě: <*název_serveru*>**. database.windows.net**. Například **adworkserver.database.windows.net**.

4.  V části **Přihlašovací pověření** klikněte na **Použít následující uživatelské jméno a heslo**, zadejte **uživatelské jméno** a **heslo**, které jste nastavili pro server SQL Database, když jste ho vytvářeli, a potom klikněte na **Další**.

    ![Zadejte název a přihlašovací údaje serveru.](./media/sql-database-connect-excel/connect-to-server.png)

    > [AZURE.TIP] V závislosti na vašem síťovém prostředí nemusíte být schopni se připojit nebo může dojít ke ztrátě připojení (pokud server databáze SQL nepovoluje přenos dat z IP adresy vašeho klienta). Přejděte na [portál Azure](https://portal.azure.com/), klikněte na SQL servery, klikněte na svůj server, v nastavení klikněte na bránu firewall a přidejte IP adresu svého klienta. Podrobnosti najdete v tématu [Jak nakonfigurovat nastavení brány firewall](sql-database-configure-firewall-settings.md).

5. V dialogovém okně **Vybrat databázi a tabulku** vyberte ze seznamu databázi, se kterou chcete pracovat, a potom klikněte na tabulky nebo zobrazení, se kterými chcete pracovat (my jsme zvolili **vGetAllCategories**). Potom klikněte na **Další**.

    ![Vyberte databázi a tabulku.](./media/sql-database-connect-excel/select-database-and-table.png)

    Otevře se dialogové okno **Uložit soubor datového připojení a dokončit**, kde zadáte informace o souboru databázového připojení (*.odc) Office, který používá Excel. Můžete nechat nastavené výchozí hodnoty nebo si vybrané možnosti přizpůsobit.

6. Můžete nechat nastavené výchozí hodnoty, ale důležité je poznamenat si **název souboru**. **Popis**, **popisný název** a **klíčová slova pro vyhledávání** vám a dalším uživatelům pomůžou zapamatovat si, k jaké databázi se připojujete, a najít připojení. Pokud chcete, aby se informace o připojení uložily do souboru odc, aby se mohl aktualizovat, když se k němu připojíte, klikněte na **Vždy aktualizovat data pomocí tohoto souboru** a potom klikněte na **Dokončit**.

    ![Uložení souboru odc](./media/sql-database-connect-excel/save-odc-file.png)

    Zobrazí se dialogové okno **Importovat data**.

## Import dat do Excelu a vytvoření kontingenčního grafu
Máte vytvořené připojení a také soubor s daty a informacemi o připojení, můžete tedy začít s importem dat.

1. V dialogovém okně **Importovat data** klikněte na možnost, kterou chcete použít pro prezentování vašich dat v listu, a potom klikněte na **OK**. Zvolili jsme možnost **Kontingenční graf**. Můžete také zvolit možnost **Nový list** nebo **Přidat tato data do datového modelu**. Další informace o datových modelech najdete v tématu [Vytvoření datového modelu v Excelu](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). Klikněte na **Vlastnosti** a prozkoumejte informace o souboru odc, který jste vytvořili v předchozím kroku. Pak zvolte možnosti pro aktualizaci dat.

    ![Volba formátu pro data v Excelu](./media/sql-database-connect-excel/import-data.png)

    List má teď prázdnou kontingenční tabulku a graf.

8. V části **Pole kontingenční tabulky** zaškrtněte políčka pro všechna pole, která chcete zobrazit.

    ![Nakonfigurujte sestavu databáze.](./media/sql-database-connect-excel/power-pivot-results.png)

> [AZURE.TIP] Pokud chcete k databázi připojit jiné excelové sešity a listy, klikněte na **Data**, klikněte na **Připojení**, klikněte na **Přidat**, ze seznamu zvolte připojení, které jste vytvořili, a pak klikněte na **Otevřít**.
> ![Otevření připojení z jiného sešitu](./media/sql-database-connect-excel/open-from-another-workbook.png)

## Další kroky

- Zjistěte, jak se [připojit k SQL Database přes SQL Server Management Studio](sql-database-connect-query-ssms.md) a provádět pokročilé dotazy a analýzy.
- Další informace o výhodách [elastických fondů](sql-database-elastic-pool.md).
- Zjistěte, jak [vytvořit webovou aplikaci, která se připojuje k SQL Database na back-endu](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).



<!--HONumber=Aug16_HO4-->


