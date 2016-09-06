<properties
   pageTitle="Vytvoření SQL Data Warehouse na portálu Azure | Microsoft Azure"
   description="Naučte se vytvářet Azure SQL Data Warehouse na portálu Azure"
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
   ms.date="08/25/2016"
   ms.author="barbkess;lodipalm;sonyama"/>

# Vytvoření Azure SQL Data Warehouse

> [AZURE.SELECTOR]
- [portál Azure](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Tento kurz využívá portál Azure k vytvoření služby SQL Data Warehouse, která obsahuje ukázkovou databázi AdventureWorksDW.


## Požadavky

Na začátek budete potřebovat:

- **Účet Azure:** Pokud si chcete vytvořit účet, přečtěte si článek [bezplatné zkušební verzi Azure][] nebo [Kredity Azure pro předplatitele MSDN][].
- **Azure SQL server**: další podrobnosti naleznete v části [Vytvoření logického serveru Azure SQL Database pomocí portálu Azure][].

> [AZURE.NOTE] Vytvoření služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.  Další podrobnosti najdete v tématu [SQL Data Warehouse – ceny][].

## Vytvoření SQL Data Warehouse

1. Přihlaste se k [portálu Azure](https://portal.azure.com).

2. Klikněte na **+ Nový** > **Data + úložiště** > **SQL Data Warehouse**.

    ![Vytvoření](./media/sql-data-warehouse-get-started-provision/create-sample.gif)

3. V okně **SQL Data Warehouse** vyplňte potřebné informace a kliknutím na Vytvořit proveďte vytvoření.

    ![Vytvoření databáze](./media/sql-data-warehouse-get-started-provision/create-database.png)

    - **Server:** Doporučujeme nejdříve vybrat server.  

    - **Název databáze:** Název, který se použije k odkazování na SQL Data Warehouse.  Musí být pro server jedinečný.
    
    - **Výkon:** Doporučujeme začít se 400 [DWU][DWU]. Výkon datového skladu můžete upravit posunutím jezdce doleva nebo doprava, případně pak můžete vertikálně navýšit nebo snížit kapacitu i po jeho vytvoření.  Pokud vás zajímají další informace o jednotkách DWU, projděte si v naší dokumentaci informace o [škálování](./sql-data-warehouse-manage-compute-overview.md) nebo informace na [stránce s cenami][SQL Data Warehouse – ceny]. 

    - **Předplatné:** Vyberte [předplatné], na které se bude tuto službu SQL Data Warehouse fakturovat.

    - **Skupina prostředků**: [Skupiny prostředků][Skupina prostředků] jsou kontejnery, které jsou navržené tak, aby vám pomohly při správě kolekce prostředků Azure. Další informace o [skupinách prostředků](../resource-group-overview.md).

    - **Zvolit zdroj:** Klikněte na **Zvolit zdroj** > **Ukázka**. Azure automaticky vyplní možnost **Vyberte vzorek** s možností AdventureWorksDW.

> [AZURE.NOTE] Výchozí kolace pro SQL Data Warehouse je SQL_Latin1_General_CP1_CI_AS. V případě potřeby jiné kolace je možné použít [T-SQL][] k vytvoření databáze s jinou kolací.

4. Kliknutím na **Vytvořit** si vytvořte svoji službu SQL Data Warehouse.

5. Počkejte několik minut. Když je váš datový sklad připraven, můžete se vrátit do [portálu Azure](https://portal.azure.com). Svoji službu SQL Data Warehouse najdete na řídicím panelu v části s vašimi databázemi SQL nebo ve skupině prostředků, kterou jste použili k jejímu vytvoření. 

    ![zobrazení portálu](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[AZURE.INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)] 

## Další kroky

Službu SQL Data Warehouse máte vytvořenou, takže se teď můžete [připojit](./sql-data-warehouse-connect-overview.md) a můžete začít se zadáváním dotazů.

Pokud budete chtít načíst data do SQL Data Warehouse, najdete další informace v části [s přehledem načítání](./sql-data-warehouse-overview-load.md).

Pokud se pokoušíte migrovat existující databázi do SQL Data Warehouse, podívejte na [přehled migrace](./sql-data-warehouse-overview-migrate.md) nebo použijte [nástroj pro migraci](./sql-data-warehouse-migrate-migration-utility.md).

Pravidla brány firewall lze také konfigurovat pomocí jazyka Transact-SQL. Další informace naleznete v tématu [sp_set_firewall_rule][] a [sp_set_database_firewall_rule][].

Dobrým nápadem je také podívat se na část [Osvědčené postupy][].

<!--Article references-->
[Vytvoření logického serveru Azure SQL Database pomocí portálu Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Vytvoření logického serveru Azure SQL Database pomocí prostředí PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[skupinám prostředků]: ../resource-group-template-deploy-portal.md
[Osvědčené postupy]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md#data-warehouse-units
[předplatné]: ../azure-glossary-cloud-terminology.md#subscription
[skupina prostředků]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md
 
<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[SQL Data Warehouse – ceny]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[bezplatné zkušební verzi Azure]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[Kredity Azure pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F




<!--HONumber=ago16_HO5-->


