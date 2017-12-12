---
title: "Prostředí PowerShell příklad monitorování škálování SQL elastického fondu Azure SQL Database | Microsoft Docs"
description: "Azure PowerShell ukázkový skript ke sledování a škálování elastický fond SQL v databázi SQL Azure"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 3c7d655ba8893cdd427b2b77cd57608fcb275b59
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="use-powershell-to-monitor-and-scale-a-sql-elastic-pool-in-azure-sql-database"></a>Ke sledování a škálování elastický fond SQL v Azure SQL Database pomocí prostředí PowerShell

Tento ukázkový skript prostředí PowerShell monitoruje metriky výkonu fondu elastické databáze, škálovatelná pro vyšší úroveň výkonu a vytvoří pravidlo výstrahy na jednom metrik výkonu. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-pool/monitor-and-scale-pool.ps1?highlight=16-17 "Monitor and scale single SQL Database")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků a všechny prostředky, které jsou s ním spojená.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
 [Nový AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Nový AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Vytvoří logického serveru, který je hostitelem databáze nebo elastického fondu. |
| [Nový AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | Vytvoří elastického fondu v rámci logického serveru. |
| [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Vytvoří databázi logického serveru jako jednu, nebo databázi ve fondu. |
| [Get-AzureRmMetric](/powershell/module/azurerm.insights/get-azurermmetric) | Zobrazuje informace o využití velikost pro databázi.|
| [Přidat AzureRMMetricAlertRule](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | Přidá nebo aktualizuje pravidlo výstrahy založené na metriky. |
| [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) | Aktualizace vlastnosti elastického fondu |
| [Přidat AzureRMMetricAlertRule](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | Nastaví pravidlo výstrahy automaticky monitorování Dtu v budoucnu. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |
|||

## <a name="next-steps"></a>Další kroky

Další informace o prostředí Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Další ukázky skriptu PowerShell databáze SQL najdete v [skriptů prostředí PowerShell databáze SQL Azure](../sql-database-powershell-samples.md).
