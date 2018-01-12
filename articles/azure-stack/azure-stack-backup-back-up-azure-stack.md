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
ms.openlocfilehash: daea97c0f5ee6ef855dc50c1ed6c7934aa85a1c4
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2018
---
# <a name="back-up-azure-stack"></a>Zálohování Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Proveďte zálohu na vyžádání v zásobníku Azure se zálohováním na místě. Pokud potřebujete aktivovat službu zálohování infrastruktury, najdete v části [povolit zálohování pro zásobník Azure z portálu pro správu](azure-stack-backup-enable-backup-console.md).

## <a name="start-azure-stack-backup"></a>Spustit zálohování Azure zásobníku

Otevřete prostředí Windows PowerShell s řádku se zvýšenými oprávněními a spusťte následující příkazy:

   ```powershell
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
