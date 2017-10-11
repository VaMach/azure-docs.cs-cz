---
title: "Tom, jak odinstalovat nástroj úlohy elastické databáze"
description: "Postup odinstalace nástroje úlohy elastické databáze"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: bfc9d820-edbd-4fca-bfbf-1f339cfcc448
ms.service: sql-database
ms.workload: sql-database
ms.custom: scale out apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: ae7f0bce452a0a86f6f1e4d9b0c93a0fa1727f21
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="uninstall-elastic-database-jobs-components"></a>Odinstalace součástí úlohy elastické databáze
**Elastické databáze úlohy** součásti lze odinstalovat pomocí portálu nebo prostředí PowerShell.

## <a name="uninstall-elastic-database-jobs-components-using-the-azure-portal"></a>Odinstalace součástí úlohy elastické databáze pomocí portálu Azure
1. Otevřete web [Azure Portal](https://portal.azure.com/).
2. Přejděte na předplatné, které obsahuje **úlohy elastické databáze** součásti, konkrétně předplatné, ve které elastické databáze součásti úlohy nebyly nainstalovány.
3. Klikněte na tlačítko **Procházet** a klikněte na tlačítko **skupiny prostředků**.
4. Vyberte skupinu prostředků s názvem "__ElasticDatabaseJob".
5. Odstraňte skupinu prostředků.

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a>Odinstalace součástí úlohy elastické databáze pomocí prostředí PowerShell
1. Spusťte příkazové okno Microsoft Azure PowerShell a přejděte na nástroje podadresář ve složce Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x: typ **cd nástroje**.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x* > disk cd nástroje
2. Spusťte.\UninstallElasticDatabaseJobs.ps1 skript prostředí PowerShell.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools > odblokovat soubor.\UninstallElasticDatabaseJobs.ps1 PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools >.\UninstallElasticDatabaseJobs.ps1

Nebo jednoduše, spusťte následující skript, za předpokladu, že výchozí hodnoty, kde je použít pro instalaci součástí:

        $ResourceGroupName = "__ElasticDatabaseJob"
        Switch-AzureMode AzureResourceManager

        $resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
        if(!$resourceGroup)
        {
            Write-Host "The Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
            return
        }

        Write-Host "Removing the Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
        Remove-AzureResourceGroup -Name $ResourceGroupName -Force
        Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job compoennts are now uninstalled."

## <a name="next-steps"></a>Další kroky
Pokud chcete znovu nainstalovat úlohy elastické databáze, přečtěte si téma [nainstalovat službu úlohy elastické databáze](sql-database-elastic-jobs-service-installation.md)

Přehled úlohy elastické databáze najdete v tématu [přehled úlohy elastické databáze](sql-database-elastic-jobs-overview.md).

<!--Image references-->


