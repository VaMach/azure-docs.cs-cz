---
title: "Vytvoření SQL Data Warehouse na portálu Azure Portal | Dokumentace Microsoftu"
description: "Naučte se vytvářet Azure SQL Data Warehouse na portálu Azure"
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: 552e496e-d560-419c-9996-6bbc80c521cb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: cea81f16ecc6203f487fdf15310638b123312dde
ms.openlocfilehash: caee6a14bd988f2355c9683519b5f5480428a709


---
# <a name="create-an-azure-sql-data-warehouse"></a>Vytvoření Azure SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Azure Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

Tento kurz využívá portál Azure k vytvoření služby SQL Data Warehouse, která obsahuje ukázkovou databázi AdventureWorksDW.

## <a name="prerequisites"></a>Požadavky
Na začátek budete potřebovat:

* **Účet Azure**: Pokud si chcete vytvořit účet, přejděte na stránku [Bezplatná zkušební verze Azure][Azure Free Trial] nebo [Kredity Azure pro předplatitele MSDN][MSDN Azure Credits].
* **Server SQL Azure:** Další podrobnosti najdete v části [Vytvoření logického serveru Azure SQL Database pomocí webu Azure Portal][Create an Azure SQL Database logical server with the Azure portal].

> [!NOTE]
> Vytvoření služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.  Další podrobnosti najdete v tématu [SQL Data Warehouse – ceny][SQL Data Warehouse pricing].
>
>

## <a name="create-a-sql-data-warehouse"></a>Vytvoření SQL Data Warehouse
1. Přihlaste se k webu [Portál Azure](https://portal.azure.com).
2. Klikněte na **+ Nový** > **Databáze** > **SQL Data Warehouse**.

    ![Vytvoření](./media/sql-data-warehouse-get-started-provision/create-sample.gif)
3. V okně **SQL Data Warehouse** vyplňte potřebné informace a kliknutím na Vytvořit proveďte vytvoření.

    ![Vytvoření databáze](./media/sql-data-warehouse-get-started-provision/create-database.png)

   * **Server:** Doporučujeme nejdříve vybrat server.  
   * **Název databáze:** Název, který se použije k odkazování na SQL Data Warehouse.  Musí být pro server jedinečný.
   * **Výkon:** Doporučujeme začít se 400 [DWU][DWU]. Výkon datového skladu můžete upravit posunutím jezdce doleva nebo doprava, případně pak můžete vertikálně navýšit nebo snížit kapacitu i po jeho vytvoření.  Pokud vás zajímají další informace o DWU, projděte si naši dokumentaci ke [škálování](sql-data-warehouse-manage-compute-overview.md) nebo [stránku s cenami][SQL Data Warehouse pricing].
   * **Předplatné:** Vyberte [předplatné], na které se bude tuto službu SQL Data Warehouse fakturovat.
   * **Skupina prostředků:** [Skupiny prostředků][Resource group] jsou kontejnery, které jsou navržené tak, aby vám pomohly při správě kolekce prostředků Azure. Další informace o [skupinách prostředků](../azure-resource-manager/resource-group-overview.md).
   * **Zvolit zdroj:** Klikněte na **Zvolit zdroj** > **Ukázka**. Azure automaticky vyplní možnost **Vyberte vzorek** s možností AdventureWorksDW.

   > [!NOTE]
   > Výchozí kolace pro SQL Data Warehouse je SQL_Latin1_General_CP1_CI_AS. V případě potřeby jiné kolace je možné pomocí [T-SQL][T-SQL] vytvořit databázi s jinou kolací.
   >
   >

1. Kliknutím na **Vytvořit** si vytvořte svoji službu SQL Data Warehouse.
2. Počkejte několik minut. Když je váš datový sklad připraven, můžete se vrátit do [portálu Azure](https://portal.azure.com). Svoji službu SQL Data Warehouse najdete na řídicím panelu v části s vašimi databázemi SQL nebo ve skupině prostředků, kterou jste použili k jejímu vytvoření.

    ![zobrazení portálu](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a>Další kroky
Službu SQL Data Warehouse máte vytvořenou, takže se teď můžete [připojit](sql-data-warehouse-connect-overview.md) a můžete začít se zadáváním dotazů.

Pokud budete chtít načíst data do SQL Data Warehouse, najdete další informace v části [s přehledem načítání](sql-data-warehouse-overview-load.md).

Pokud se pokoušíte migrovat existující databázi do SQL Data Warehouse, podívejte na [přehled migrace](sql-data-warehouse-overview-migrate.md) nebo použijte [nástroj pro migraci](sql-data-warehouse-migrate-migration-utility.md).

Pravidla brány firewall lze také konfigurovat pomocí jazyka Transact-SQL. Další informace najdete v tématech [sp_set_firewall_rule][sp_set_firewall_rule] a [sp_set_database_firewall_rule][sp_set_database_firewall_rule].

Také může být užitečné podívat se na [Osvědčené postupy][Best practices].

<!--Article references-->
[Create an Azure SQL Database logical server with the Azure portal]: ../sql-database/sql-database-get-started.md#create-logical-server-bk
[Create an Azure SQL Database logical server with PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[resource groups]: ../azure-resource-manager/resource-group-template-deploy-portal.md
[Best practices]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md#data-warehouse-units
[předplatné]: ../azure-glossary-cloud-terminology.md#subscription
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md

<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F



<!--HONumber=Feb17_HO3-->


