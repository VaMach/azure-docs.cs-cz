---
title: "Azure skript prostředí PowerShell ukázkový – vytvoření naplánovaného zálohování pro webovou aplikaci | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – vytvoření naplánovaného zálohování pro webovou aplikaci"
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 10/30/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: bfc9dc5defa5644c5ede0acab1d4eae4d93edc94
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="create-a-scheduled-backup-for-a-web-app"></a>Vytvoření naplánovaného zálohování pro webovou aplikaci

Tento ukázkový skript vytvoří webovou aplikaci ve službě App Service se jeho souvisejících prostředků a potom vytvoří pro ni plánované zálohování. 

V případě potřeby nainstalujte prostředí Azure PowerShell pomocí instrukce v nalezen [prostředí Azure PowerShell průvodce](/powershell/azure/overview)a poté spusťte `Login-AzureRmAccount` vytvořit připojení s Azure. 

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/app-service/backup-scheduled/backup-scheduled.ps1?highlight=1-4 "Create a scheduled backup for a web app")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Po spuštění ukázka skriptu, následující příkaz lze použít k odebrání skupiny prostředků, webové aplikace a všechny související prostředky.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Nový AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Nové AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) | Vytvoří účet úložiště. |
| [Nové AzureStorageContainer](/powershell/module/azure.storage/new-azurestoragecontainer) | Vytvoří kontejner úložiště Azure. |
| [Nové AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) | Generuje token SAS pro kontejner úložiště Azure. |
| [Nové AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Vytvoří plán služby App Service. |
| [Nové AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Vytvoří webovou aplikaci. |
| [Upravit AzureRmWebAppBackupConfiguration](/powershell/module/azurerm.websites/edit-azurermwebappbackupconfiguration) | Upraví konfiguraci zálohování pro webovou aplikaci. |
| [Get-AzureRmWebAppBackupList](/powershell/module/azurerm.websites/get-azurermwebappbackuplist) | Získá seznam záloh pro webovou aplikaci. |
| [Get-AzureRmWebAppBackupConfiguration](/powershell/module/azurerm.websites/get-azurermwebappbackupconfiguration) | Získá konfiguraci zálohování pro webovou aplikaci. |

## <a name="next-steps"></a>Další kroky

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Další ukázky prostředí Azure Powershell pro Azure App Service Web Apps naleznete v [prostředí Azure PowerShell ukázky](../app-service-powershell-samples.md).
