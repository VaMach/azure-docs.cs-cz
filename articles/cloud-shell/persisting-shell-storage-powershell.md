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
ms.date: 01/30/2018
ms.author: damaerte
ms.openlocfilehash: 74488b85ec524e4ad4c06a639a16ddbfd54b3154
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
[!INCLUDE [features-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-powershell-in-azure-cloud-shell-preview-works"></a>Jak funguje prostředí PowerShell v prostředí cloudu Azure (Preview)
Prostředí PowerShell v prostředí cloudu (Preview) potrvá soubory prostřednictvím následující metody: 
* Připojení vaší zadanou sdílenou složku Azure jako `clouddrive` ve vaší `$Home` adresář pro přímé sdílení souborů interakce.

## <a name="list-cloud-drive-azure-file-shares"></a>Zobrazit seznam sdílených složek souboru jednotka Azure Cloud
`Get-CloudDrive` Příkaz načte informace o sdílené složky Azure file aktuálně připojené jednotka cloudu v prostředí cloudu. <br>
![Get-CloudDrive systémem](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

## <a name="unmount-cloud-drive"></a>Odpojení Cloudovou jednotku
Sdílenou složku Azure připojeném do prostředí cloudu kdykoli můžete odpojit. Pokud byl odebrán sdílenou složkou Azure, budete vyzváni k vytvořit a připojit novou sdílenou složku Azure file na další relace.

`Dismount-CloudDrive` Příkaz odpojí sdílenou složku Azure z aktuální účet úložiště. Odpojení Cloudovou jednotku ukončí aktuální relaci. Uživatel se vyzve k vytvořit a připojit novou sdílenou složku Azure file během další relace.
![Spuštění CloudDrive odpojení](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!INCLUDE [features-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Další postup
[Rychlý start pro PowerShell](quickstart-powershell.md) <br>
[Další informace o Azure soubory](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Další informace o značkách úložiště](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>