---
title: "Rychlý start Azure – Vytvoření účtu úložiště pomocí PowerShellu | Dokumentace Microsoftu"
description: "Rychle se naučíte, jak vytvořit nový účet úložiště pomocí PowerShellu"
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/29/2017
ms.author: tamram
ms.openlocfilehash: 87f179d2cf3802af3eca58e0adf57b07069bfcc6
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/19/2017
---
# <a name="create-a-storage-account-using-powershell"></a>Vytvoření účtu úložiště pomocí PowerShellu

Modul Azure PowerShell slouží k vytváření a správě prostředků Azure z příkazového řádku PowerShellu nebo ve skriptech. Tato příručka podrobně popisuje použití PowerShellu k vytvoření účtu Azure Storage. 

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

Tento Rychlý start vyžaduje modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu `Get-Module -ListAvailable AzureRM`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k předplatnému Azure pomocí příkazu `Login-AzureRmAccount` a postupujte podle pokynů na obrazovce.

```powershell
Login-AzureRmAccount
```

Pokud nevíte, jaké umístění máte použít, můžete vypsat všechna dostupná umístění. Po zobrazení seznamu vyhledejte umístění, které chcete použít. Tento příklad používá **eastus**. Uložte tuto hodnotu do proměnné a používejte tuto proměnnou, abyste umístění mohli změnit na jednom místě.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků Azure pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

```powershell
# put resource group in a variable so you can use the same group name going forward
#   without hardcoding it repeatedly
$resourceGroup = "contoso-storage-accounts"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-general-purpose-storage-account"></a>Vytvoření účtu úložiště pro obecné účely

Existuje několik typů účtů úložiště v závislosti na plánovaném využití a službě (objekty blob, soubory, tabulky nebo fronty), pro kterou se bude používat. Tato tabulka uvádí různé možnosti.

|**Typ účtu úložiště**|**Standard pro obecné účely**|**Premium pro obecné účely**|**Blob Storage, horká a studená vrstva přístupu**|
|-----|-----|-----|-----|
|**Podporované služby**| Služby Blob, File, Table a služba front | Služba Blob | Služba Blob|
|**Typy podporovaných objektů blob**|Objekty blob bloku, objekty blob stránky a doplňovací objekty blob | Objekty blob stránky | Objekty blob bloku a doplňovací objekty blob|

Pomocí rutiny [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount) vytvořte účet úložiště pro obecné účely, který můžete použít pro všechny čtyři služby. Pojmenujte účet úložiště *contosomvcstandard* a nakonfigurujte ho tak, aby měl povolené místně redundantní úložiště a šifrování objektů blob.

```powershell
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "contosomvcstandard" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud už je nepotřebujete, můžete k odebrání skupiny prostředků a všech souvisejících prostředků použít příkaz [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup). V tomto případě se odebere účet úložiště, který jste vytvořili.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste vytvořili účet úložiště úrovně Standard pro obecné účely. Pokud chcete zjistit, jak pomocí účtu úložiště nahrávat a stahovat objekty blob, pokračujte k rychlému startu pro úložiště objektů blob.
> [!div class="nextstepaction"]
> [Přenos objektů do a z úložiště objektů blob v Azure pomocí PowerShellu](../blobs/storage-quickstart-blobs-powershell.md)