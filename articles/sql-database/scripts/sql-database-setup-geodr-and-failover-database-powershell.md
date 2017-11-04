---
title: "Prostředí PowerShell příklad aktivní geografické replikace jedním Azure SQL Database | Microsoft Docs"
description: "Azure PowerShell ukázkový skript nastavit aktivní geografickou replikací pro jednu databázi Azure SQL a převzetí služeb při selhání je."
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 22688c1b3c1a993612898da2c3e2b6cca3d03b30
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/13/2017
---
# <a name="use-powershell-to-configure-active-geo-replication-for-a-single-azure-sql-database"></a>Pomocí prostředí PowerShell ke konfiguraci aktivní geografickou replikací pro jednu databázi Azure SQL

Tento ukázkový skript prostředí PowerShell nakonfiguruje aktivní geografickou replikací pro jednu databázi Azure SQL a selhání na sekundární repliku databáze Azure SQL.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-scripts"></a>Ukázkové skripty

[!code-powershell[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover-database/setup-geodr-and-failover-database.ps1?highlight=17-20 "Set up active geo-replication for single database")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $primaryresourcegroupname
Remove-AzureRmResourceGroup -ResourceGroupName $secondaryresourcegroupname
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Nový AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Nový AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu. |
| [Nový AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | Vytvoří elastického fondu v rámci logického serveru. |
| [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) | Aktualizuje vlastnosti databáze nebo přesouvá databázi do, mimo nebo mezi elastické fondy. |
| [Nové AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| Sekundární databáze pro databázi vytvoří a spustí replikaci dat. |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)| Získá jednu nebo více databází. |
| [Set-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| Přepne sekundární databáze, která bude primární k zahájení převzetí služeb při selhání.|
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | Získá odkazy geografická replikace mezi Azure SQL Database a skupinu prostředků nebo SQL Server. |
| [Odebrat AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) | Ukončí replikaci dat mezi SQL Database a zadaný sekundární databázi. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |
|||

## <a name="next-steps"></a>Další kroky

Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Další ukázky skriptu PowerShell databáze SQL najdete v [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).
