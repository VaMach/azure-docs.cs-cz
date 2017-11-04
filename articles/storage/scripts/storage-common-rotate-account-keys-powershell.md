---
title: "Azure ukázkový skript prostředí PowerShell - přístupový klíč účtu úložiště otáčení | Microsoft Docs"
description: "Vytvoření účtu úložiště Azure potom načíst a otočit jeden z jeho přístupových klíčů k účtu."
services: storage
documentationcenter: na
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/13/2017
ms.author: robinsh
ms.openlocfilehash: fe7e12991cb59eb32e173b0552eb155bf66c6c76
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>Vytvoření účtu úložiště a otočit jeho přístupových klíčů k účtu

Tento skript vytvoří účet úložiště Azure, zobrazí nový účet úložiště primární přístupový klíč a pak obnovuje (otočí) klíče.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.ps1 "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, účet úložiště a všechny související prostředky.

```powershell
Remove-AzureRmResourceGroup -Name rotatekeystestrg
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření účtu úložiště a načtení a otočit jeden z jeho přístupové klíče. Každou položku v tabulce odkazy na dokumentaci specifické pro příkaz.

| Příkaz | Poznámky |
|---|---|
| [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation) | Získá všechny umístění a podporovaných poskytovatelích prostředků pro každé umístění. |
| [Nový AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Vytvoří skupinu prostředků Azure. |
| [Nové AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) | Vytvoří účet úložiště. |
| [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) | Získá přístupové klíče pro účet úložiště Azure. |
| [Nové AzureRmStorageAccountKey](/powershell/module/azurerm.storage/new-azurermstorageaccountkey) | Znovu generuje přístupový klíč pro účet úložiště Azure. |

## <a name="next-steps"></a>Další kroky

Další informace o modulu Azure PowerShell najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).

Ukázky skriptu PowerShell další úložiště naleznete v [ukázky pro úložiště objektů Blob v Azure PowerShell](../blobs/storage-samples-blobs-powershell.md).
