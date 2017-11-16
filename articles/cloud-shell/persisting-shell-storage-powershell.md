---
title: "Zachovat soubory v prostředí PowerShell v prostředí cloudu Azure (Preview) | Microsoft Docs"
description: "Návod, jak Azure Cloud prostředí potrvají soubory."
services: azure
documentationcenter: 
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: damaerte
ms.openlocfilehash: 66d0e20d670e49cdfe64614d1fc6f5739fde6155
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
[!include [features-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-powershell-in-azure-cloud-shell-preview-works"></a>Jak funguje prostředí PowerShell v prostředí cloudu Azure (Preview)
Prostředí PowerShell v prostředí cloudu (Preview) potrvá soubory prostřednictvím následující metody: 
* Připojení zadané sdílené složky jako `clouddrive` ve vaší `$Home` adresář pro přímé sdílení souborů interakce.

## <a name="list-cloud-drive-file-shares"></a>Zobrazit seznam sdílených složek souboru Cloudovou jednotku
`Get-CloudDrive` Příkaz načte informace o sdílené složky souboru aktuálně připojené jednotka cloudu v prostředí cloudu. <br>
![Get-CloudDrive systémem](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

## <a name="unmount-cloud-drive"></a>Odpojení Cloudovou jednotku
Sdílení souborů, které je připojené k prostředí cloudu kdykoli můžete odpojit. Pokud byl odebrán sdílené složky, budete vyzváni k vytvořit a připojit novou sdílenou složku na další relace.

`Dismount-CloudDrive` Příkaz odpojí sdílené složky z aktuální účet úložiště. Odpojení Cloudovou jednotku ukončí aktuální relaci. Uživatel se vyzve k vytvořit a připojit novou sdílenou během další relace.
![Spuštění CloudDrive odpojení](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!include [features-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Další kroky
[Rychlý start pro PowerShell](quickstart-powershell.md) <br>
[Další informace o Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Další informace o značkách úložiště](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>