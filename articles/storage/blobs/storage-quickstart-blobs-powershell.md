---
title: "Azure rychlý start - objekty přenosu do nebo z Azure Blob storage pomocí prostředí PowerShell | Microsoft Docs"
description: "Naučte se rychle převést objekty do a z Azure Blob storage pomocí prostředí PowerShell"
services: storage
documentationcenter: storage
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
ms.date: 07/19/2017
ms.author: robinsh
ms.openlocfilehash: 1a9941b21b92c70dd0a46ce2e4c75142e1786650
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-azure-powershell"></a>Objekty přenosu do nebo z Azure Blob storage pomocí prostředí Azure PowerShell

Modul Azure PowerShell slouží k vytváření a správě prostředků Azure z příkazového řádku PowerShellu nebo ve skriptech. Tato příručka údaje pomocí prostředí PowerShell pro přenos souborů mezi místního disku a úložiště objektů Blob v Azure.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

Tento Rychlý start vyžaduje modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu `Get-Module -ListAvailable AzureRM`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).

[!INCLUDE [storage-quickstart-tutorial-intro-include-powershell](../../../includes/storage-quickstart-tutorial-intro-include-powershell.md)]

## <a name="create-a-container"></a>Vytvoření kontejneru

Objekty BLOB jsou vždy nahrát do kontejneru. To umožňuje uspořádat skupiny objektů BLOB jako uspořádání souborů ve vašem počítači ve složkách.

Nastavte název kontejneru a pak vytvořte kontejner pomocí [New-AzureStorageContainer](/powershell/module/azure.storage/new-azurestoragecontainer), nastavení oprávnění pro blob umožní veřejný přístup souborů. Název kontejneru v tomto příkladu je *quickstartblobs*.

```powershell
$containerName = "quickstartblobs"
New-AzureStorageContainer -Name $containerName -Context $ctx -Permission blob
```

## <a name="upload-blobs-to-the-container"></a>Odešlete do kontejneru objektů BLOB

Úložiště objektů blob podporuje objekty blob bloku, doplňovací objekty blob a objekty blob stránky. Soubory VHD použité pro zálohování virtuálních počítačů IaaS jsou objekty BLOB stránky. Append – objekty BLOB jsou použitá pro protokolování, například když chcete zapsat do souboru a potom mít přidání další informace. Většina souborů uložených v úložišti objektů Blob jsou objekty BLOB bloku. 

Nahrát soubor do objektu blob bloku, získejte odkaz na kontejner a potom získat odkaz na objekt blob bloku v tomto kontejneru. Až budete mít odkaz na objekt blob, můžete nahrát data do něj s použitím [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent). Tato operace vytvoří objekt blob, pokud ještě neexistuje, nebo ho přepíše, pokud již existuje.

Následující příklady nahrát Image001.jpg a Image002.png z D:\\_TestImages složky na místním disku ke kontejneru, který jste právě vytvořili.

```powershell
# upload a file
Set-AzureStorageBlobContent -File "D:\_TestImages\Image001.jpg" `
  -Container $containerName `
  -Blob "Image001.jpg" `
  -Context $ctx 

# upload another file
Set-AzureStorageBlobContent -File "D:\_TestImages\Image002.png" `
  -Container $containerName `
  -Blob "Image002.png" `
  -Context $ctx
```

Nahrání libovolný počet souborů, jak se vám líbí, než budete pokračovat.

## <a name="list-the-blobs-in-a-container"></a>Zobrazí seznam objektů blob v kontejneru

Získání seznamu objektů BLOB v kontejneru pomocí [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob). Tento příklad ukazuje názvy objektů BLOB nahrát.

```powershell
Get-AzureStorageBlob -Container $ContainerName -Context $ctx | select Name 
```

## <a name="download-blobs"></a>Stáhnout objekty blob

Stáhnete objekty BLOB na místní disk. Pro každý objekt blob ke stažení, nastavte název a volání [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) ke stažení objektu blob.

Tento příklad stahování objektů BLOB do D:\\_TestImages\Downloads na místním disku. 

```powershell
# download first blob
Get-AzureStorageBlobContent -Blob "Image001.jpg" `
  -Container $containerName `
  -Destination "D:\_TestImages\Downloads\" `
  -Context $ctx 

# download another blob
Get-AzureStorageBlobContent -Blob "Image002.png" `
  -Container $containerName `
  -Destination "D:\_TestImages\Downloads\" `
  -Context $ctx 
```

## <a name="data-transfer-with-azcopy"></a>Přenos dat pomocí nástroje AzCopy

[AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) nástroj je další možností pro přenos dat Skriptovatelná vysoce výkonné úložiště Azure. AzCopy můžete použít k přenosu dat do a z úložiště objektů Blob, soubor a tabulku.

Jako zběžný příklad, zde je AzCopy příkaz pro nahrání souboru názvem *myfile.txt* k *mystoragecontainer* kontejneru z v okně prostředí PowerShell.

```PowerShell
./AzCopy `
    /Source:C:\myfolder `
    /Dest:https://mystorageaccount.blob.core.windows.net/mystoragecontainer `
    /DestKey:<storage-account-access-key> `
    /Pattern:"myfile.txt"
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Odeberte všechny prostředky, které jste vytvořili. Nejjednodušším způsobem je odstranit skupinu prostředků. To také odstraní všechny prostředky obsažené v rámci skupiny. V takovém případě odebere účet úložiště a skupině prostředků, sám sebe.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Další kroky

V této úvodní jste zjistili, jak pro přenos souborů mezi místního disku a úložiště objektů Blob v Azure. Další informace o práci s úložišti objektů Blob, pokračujte do úložiště objektů Blob postupy.

> [!div class="nextstepaction"]
> [Postupy operace úložiště objektů BLOB](storage-how-to-use-blobs-powershell.md)

### <a name="microsoft-azure-powershell-storage-cmdlets-reference"></a>Rutiny úložiště Microsoft Azure PowerShell – referenční informace
* [Rutiny prostředí PowerShell úložiště](/powershell/module/azurerm.storage#storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Storage Explorer
* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) je bezplatná samostatná aplikace od Microsoftu, která umožňuje vizuálně pracovat s daty Azure Storage ve Windows, macOS a Linuxu.