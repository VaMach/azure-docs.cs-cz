---
title: "Azure rychlý start - vytvořit účet úložiště pomocí prostředí PowerShell | Microsoft Docs"
description: "Naučte se rychle vytvořit nový účet úložiště pomocí prostředí PowerShell"
services: storage
documentationcenter: 
author: robinsh
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
ms.author: robinsh
ms.openlocfilehash: c9175cce0cb93e73009fb8d751e54f631603d482
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-storage-account-using-powershell"></a>Vytvořit účet úložiště pomocí prostředí PowerShell

Modul Azure PowerShell slouží k vytváření a správě prostředků Azure z příkazového řádku PowerShellu nebo ve skriptech. Tato příručka údaje pomocí prostředí PowerShell k vytvoření účtu úložiště Azure. 

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

Tento Rychlý start vyžaduje modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu `Get-Module -ListAvailable AzureRM`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k předplatnému Azure pomocí příkazu `Login-AzureRmAccount` a postupujte podle pokynů na obrazovce.

```powershell
Login-AzureRmAccount
```

Pokud si nejste jisti umístění, které chcete použít, můžete seznam dostupných umístění. Jakmile se zobrazí v seznamu, najít ten, který chcete použít. Tento příklad použije **eastus**. Uložit jako proměnnou a používat proměnné, můžete ho změnit na jednom místě.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků Azure pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

```powershell
# put resource group in a variable so you can use the same group name going forward
#   without hardcoding it repeatedly
$resourceGroup = "contoso-storage-accounts"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-general-purpose-standard-storage-account"></a>Vytvořit účet standardního úložiště pro obecné účely

Existuje několik typů účtů úložiště, v závislosti na tom, jak se bude používat a pro které služba (objekty BLOB, soubory, tabulky nebo fronty). V této tabulce jsou uvedeny možnosti.

|**Typ účtu úložiště**|**Standard pro obecné účely**|**Premium pro obecné účely**|**Blob Storage, horká a studená vrstva přístupu**|
|-----|-----|-----|-----|
|**Podporované služby**| Tabulka objektů BLOB, soubor, služba fronty | Blob Service | Blob Service|
|**Typy podporovaných objektů blob**|Objekty BLOB bloku, objekty BLOB stránky, doplňovací objekty BLOB | Objekty blob stránky | Objekty blob bloku a doplňovací objekty blob|

Použít [AzureRmStorageAccount nový](/powershell/module/azurerm.storage/New-AzureRmStorageAccount) k vytvoření účtu úložiště pro obecné účely standardní, můžete použít pro všechny čtyři služby. Název účtu úložiště *contosomvcstandard*a nakonfigurujte ho místně redundantní úložiště a objektů blob, zapnuté šifrování.

```powershell
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "contosomvcstandard" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete, můžete použít [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) příkaz, který má odeberte skupinu zdrojů a všechny související prostředky. V takovém případě odebere účet úložiště, který jste vytvořili.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Další kroky

V této úvodní jste vytvořili účet standardního úložiště pro obecné účely. Informace o tom, nahrávání a stahování objekty BLOB se svým účtem úložiště, i nadále rychlý start úložiště objektů Blob.
> [!div class="nextstepaction"]
> [Objekty přenosu do nebo z Azure Blob storage pomocí prostředí PowerShell](../blobs/storage-quickstart-blobs-powershell.md)