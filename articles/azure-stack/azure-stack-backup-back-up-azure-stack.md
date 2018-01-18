---
title: "Zálohování Azure zásobníku | Microsoft Docs"
description: "Proveďte zálohu na vyžádání v zásobníku Azure se zálohováním na místě."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: 955b286967ca2bc8450e8988ec16c6a5c352aa8a
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="back-up-azure-stack"></a>Zálohování Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Proveďte zálohu na vyžádání v zásobníku Azure se zálohováním na místě. Pokud potřebujete aktivovat službu zálohování infrastruktury, najdete v části [povolit zálohování pro zásobník Azure z portálu pro správu](azure-stack-backup-enable-backup-console.md).

> [!Note]  
>  Nástroje Azure zásobníku obsahovat **Start-AzSBackup** rutiny. Pokyny k instalaci nástroje najdete v tématu [zprovoznění pomocí prostředí PowerShell v zásobníku Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart).

## <a name="start-azure-stack-backup"></a>Spustit zálohování Azure zásobníku

Otevřete prostředí Windows PowerShell se v prostředí správy operátor řádku se zvýšenými oprávněními a spusťte následující příkazy:

```powershell
    cd C:\tools\AzureStack-Tools-master\Connect
    Import-Module .\AzureStack.Connect.psm1

    cd C:\tools\AzureStack-Tools-master\Infrastructure
    Import-Module .\AzureStack.Infra.psm1 
    
    Start-AzSBackup -Location $location.Name
```

## <a name="confirm-backup-completed-in-the-administration-portal"></a>Potvrďte zálohování v portálu pro správu bylo dokončeno

1. Otevřete na portálu správy Azure zásobníku na [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Vyberte **další služby** > **infrastruktura zálohování**. Zvolte **konfigurace** v **infrastruktura zálohování** okno.
3. Najít **název** a **datum dokončení** zálohy v **dostupné zálohy** seznamu.
4. Ověřte **stavu** je **úspěšné**.

Můžete také zkontrolovat, zálohování udělat z portálu pro správu. Přejděte na`\MASBackup\<datetime>\<backupid>\BackupInfo.xml`

## <a name="next-steps"></a>Další postup

- Další informace o pracovním postupu pro obnovení z před událostí ztráty dat. V tématu [zotavit závažné ztráty dat](azure-stack-backup-recover-data.md).
