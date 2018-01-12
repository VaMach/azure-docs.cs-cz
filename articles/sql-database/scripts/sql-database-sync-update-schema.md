---
title: "Příklad PowerShell - synchronizaci dat SQL aktualizace schématu synchronizace | Microsoft Docs"
description: "Azure PowerShell ukázkový skript aktualizace schématu synchronizace pro synchronizaci dat SQL"
services: sql-database
documentationcenter: sql-database
author: jognanay
manager: craigg
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 01/10/2018
ms.author: jognanay
ms.reviewer: douglasl
ms.openlocfilehash: 66bf084f585b86979e6521321daf466c571de10c
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="use-powershell-to-update-the-sync-schema-in-an-existing-sync-group"></a>Pomocí prostředí PowerShell aktualizovat schéma synchronizace v existující skupině synchronizace

Tento příklad PowerShell aktualizací schématu synchronizace v existující skupině synchronizace. Když se synchronizuje více tabulek, tento skript umožňuje efektivně aktualizovat schéma synchronizace.

Tento příklad ukazuje použití **UpdateSyncSchema** skript, který je k dispozici na Githubu jako [UpdateSyncSchema.ps1](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-data-sync/UpdateSyncSchema.ps1).

Přehled synchronizaci dat SQL najdete v tématu [synchronizaci dat mezi několika databází cloudu a místně s synchronizaci dat SQL Azure (Preview)](../sql-database-sync-data.md).
## <a name="prerequisites"></a>Požadavky

Tato ukázka vyžaduje prostředí Azure PowerShell verze modulu 4.2 nebo novější. Spustit `Get-Module -ListAvailable AzureRM` najít nainstalovanou verzi. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).
 
Spustit `Login-AzureRmAccount` vytvořit připojení s Azure.

## <a name="examples"></a>Příklady

### <a name="example-1---add-all-tables-to-the-sync-schema"></a>Příklad 1 - přidejte všechny tabulky do schématu synchronizace

V následujícím příkladu se obnoví schéma databáze a přidá všechny platné tabulky v databázi rozbočovače schématu synchronizace.

```powershell
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -RefreshDatabaseSchema $true -AddAllTables $true
```

### <a name="example-2---add-and-remove-tables-and-columns"></a>Příklad 2 – Přidání nebo odebrání tabulek a sloupců

Následující příklad přidá `[dbo].[Table1]` a `[dbo].[Table2].[Column1]` schéma synchronizace a odebere `[dbo].[Table3]`.

```powershell
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -TablesAndColumnsToAdd "[dbo].[Table1],[dbo].[Table2].[Column1]" -TablesAndColumnsToRemove "[dbo].[Table3]"
```

## <a name="script-parameters"></a>Parametry skriptu

**UpdateSyncSchema** skript má následující parametry:

| Parametr | Poznámky |
|---|---|
| $SubscriptionId | Předplatné, kde se má vytvořit skupiny synchronizace. |
| $ResourceGroupName | Skupinu prostředků, kde se má vytvořit skupiny synchronizace.|
| $ServerName | Název serveru databáze rozbočovače.|
| $DatabaseName | Název databáze rozbočovače. |
| $SyncGroupName | Název skupiny synchronizace. |
| $MemberName | Pokud chcete načíst schéma databáze ze synchronizace člen místo z databáze rozbočovače, zadejte název člena. Pokud chcete načíst schéma databáze z centra, ponechte tento parametr prázdné. |
| $TimeoutInSeconds | Vypršel časový limit při skript aktualizuje schématu databáze. Výchozí hodnota je 900 sekund. |
| $RefreshDatabaseSchema | Zadejte, zda skript, který je potřeba aktualizovat schéma databáze. Změnu svého schématu databáze z předchozí konfigurace – například pokud jste přidali novou tabulku nebo znovu sloupec), je potřeba aktualizovat schéma, než je znovu nakonfigurovat. Výchozí hodnota je false. |
| $AddAllTables | Pokud je tato hodnota true, jsou přidány všechny platné tabulek a sloupců schématu synchronizace. Hodnoty $TablesAndColumnsToAdd a $TablesAndColumnsToRemove se ignorují. |
| $TablesAndColumnsToAdd | Zadejte tabulky nebo sloupce, které chcete přidat do schématu synchronizace. Každý název tabulky nebo sloupce musí být plně oddělený s názvem schématu. Příklad: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Více názvů tabulky nebo sloupce můžete zadat a oddělených čárkami (,). |
| $TablesAndColumnsToRemove | Zadejte tabulky nebo sloupce, které chcete odebrat ze schématu synchronizace. Každý název tabulky nebo sloupce musí být plně oddělený s názvem schématu. Příklad: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Více názvů tabulky nebo sloupce můžete zadat a oddělených čárkami (,). |
|||

## <a name="script-explanation"></a>Vysvětlení skriptu

**UpdateSyncSchema** skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci specifické pro příkaz.

| Příkaz | Poznámky |
|---|---|
| [Get-AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncgroup) | Vrátí informace o synchronizace skupiny. |
| [Aktualizace AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncgroup) | Aktualizace skupiny synchronizace. |
| [Get-AzureRmSqlSyncMember](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncmember) | Vrátí informace o synchronizaci člena. |
| [Get-AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncschema) | Vrátí informace o schématu synchronizace. |
| [Aktualizace AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncschema) | Aktualizace schématu synchronizace. |
|||

## <a name="next-steps"></a>Další postup

Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Další ukázky skriptu PowerShell databáze SQL naleznete v [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).

Další informace o synchronizaci dat SQL najdete v tématu:

-   [Synchronizaci dat mezi několika databází cloudu a místně s synchronizaci dat SQL Azure](../sql-database-sync-data.md)
-   [Nastavit synchronizaci dat SQL Azure](../sql-database-get-started-sql-data-sync.md)
-   [Osvědčené postupy pro synchronizaci dat SQL Azure](../sql-database-best-practices-data-sync.md)
-   [Monitorování synchronizaci dat Azure SQL s OMS analýzy protokolů](../sql-database-sync-monitor-oms.md)
-   [Řešení problémů s synchronizaci dat SQL Azure](../sql-database-troubleshoot-data-sync.md)

-   Dokončete příklady prostředí PowerShell, které ukazují, jak nakonfigurovat synchronizaci dat SQL:
    -   [Pomocí prostředí PowerShell k synchronizaci mezi více databází Azure SQL](sql-database-sync-data-between-sql-databases.md)
    -   [Synchronizace mezi databáze SQL Azure a místní databáze SQL serveru pomocí prostředí PowerShell](sql-database-sync-data-between-azure-onprem.md)

-   [Stáhněte si dokumentaci rozhraní API REST synchronizaci dat SQL](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Další informace o databázi SQL najdete v tématu:

-   [Databáze SQL – přehled](../sql-database-technical-overview.md)
-   [Správa životního cyklu databáze](https://msdn.microsoft.com/library/jj907294.aspx)
