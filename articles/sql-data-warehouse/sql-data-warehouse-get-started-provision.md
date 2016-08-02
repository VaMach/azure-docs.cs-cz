<properties
   pageTitle="Vytvoření SQL Data Warehouse na portálu Azure | Microsoft Azure"
   description="Naučte se vytvářet Azure SQL Data Warehouse na portálu Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/05/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Vytvoření Azure SQL Data Warehouse

> [AZURE.SELECTOR]
- [Portál Azure](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

V tomto kurzu použije portál Azure k vytvoření služby SQL Data Warehouse, která obsahuje ukázkovou databázi AdventureWorksDW.


[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]


1. Přihlaste se k [portálu Azure](https://portal.azure.com).

2. Klikněte na **+ Nový** > **Data + úložiště** > **SQL Data Warehouse**.

    ![Vytvoření](./media/sql-data-warehouse-get-started-provision/create-sample.gif)

3. V okně **SQL Data Warehouse** vyplňte potřebné informace a kliknutím na Vytvořit proveďte vytvoření.

    ![Vytvoření databáze](./media/sql-data-warehouse-get-started-provision/create-database.png)

    - **Server:** Doporučujeme nejdříve vybrat server.  Můžete vybrat existující server nebo můžete [vytvořit nový](./sql-data-warehouse-get-started-new-server.md). 

    - **Název databáze:** Název, který se použije k odkazování na SQL Data Warehouse.  Musí být pro server jedinečný.
    
    - **Výkon:** Doporučujeme začít se 400 DWU. Výkon datového skladu můžete upravit posunutím jezdce doleva nebo doprava, případně pak můžete vertikálně navýšit nebo snížit kapacitu i po jeho vytvoření.  Pokud vás zajímají další informace o jednotkách DWU, projděte si v naší dokumentaci informace o [škálování](./sql-data-warehouse-manage-compute-overview.md) nebo informace na [stránce s cenami](https://azure.microsoft.com/en-us/pricing/details/sql-data-warehouse/). 

    - **Předplatné:** Vyberte předplatné, na které se bude tuto službu SQL Data Warehouse fakturovat.

    - **Skupina prostředků:** Skupiny prostředků jsou kontejnery, které jsou navržené tak, aby vám pomohly při správě kolekce prostředků Azure. Další informace o [skupinách prostředků](../azure-portal/resource-group-portal.md).

    - **Zvolit zdroj:** Klikněte na **Zvolit zdroj** > **Ukázka**. K dispozici je teď pouze jedna ukázková databáze. Když tedy vyberete možnost Ukázka, Azure automaticky nastaví pro možnost **Zvolit vzorek** hodnotu AdventureWorksDW.

4. Kliknutím na **Vytvořit** si vytvořte svoji službu SQL Data Warehouse.

5. Za několik minut bude vaše služba SQL Data Warehouse připravená. Po dokončení byste se měli vrátit na [portál Azure](https://portal.azure.com). Svoji službu SQL Data Warehouse najdete na řídicím panelu v části s vašimi databázemi SQL nebo ve skupině prostředků, kterou jste použili k jejímu vytvoření. 

    ![Zobrazení portálu](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[AZURE.INCLUDE [SQL DataBase create server](../../includes/sql-database-create-new-server-firewall-portal.md)] 

## Další kroky

Službu SQL Data Warehouse máte vytvořenou, takže se teď můžete [připojit](./sql-data-warehouse-get-started-connect.md) a můžete začít se zadáváním dotazů.

Pokud budete chtít načíst data do SQL Data Warehouse, najdete další informace v části [s přehledem načítání](./sql-data-warehouse-overview-load.md).

Pokud se pokoušíte migrovat existující databázi do SQL Data Warehouse, podívejte na [přehled migrace](./sql-data-warehouse-overview-migrate.md) nebo použijte [nástroj pro migraci](./sql-data-warehouse-migrate-migration-utility.md).




<!--HONumber=Jun16_HO2-->


