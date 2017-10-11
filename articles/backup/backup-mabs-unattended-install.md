---
title: Tichou instalaci serveru Azure Backup v2 | Microsoft Docs
description: "Použití skriptu prostředí PowerShell k bezobslužné instalaci serveru Azure Backup v2. Tento typ instalace je také označován bezobslužné instalace."
services: backup
documentationcenter: " "
author: markgalioto
manager: carmonm
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/30/2017
ms.author: markgal;masaran
ms.openlocfilehash: 91778a67f9ef523aa87b7918197e0d0ded0f5702
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="run-an-unattended-installation-of-azure-backup-server-v2"></a>Spusťte bezobslužnou instalaci serveru Azure Backup v2

Postup spuštění bezobslužné instalace serveru Azure Backup v2. 

Tyto kroky neplatí, pokud instalujete Azure Backup Server v1.

## <a name="install-backup-server-v2"></a>Nainstalujte Backup Server v2

1. Na serveru, který je hostitelem serveru Azure Backup v2 vytvořte textový soubor. (Soubor můžete vytvořit v poznámkovém bloku nebo v jiném textovém editoru.) Uložte soubor jako MABSSetup.ini. 

2. Vložte následující kód v souboru MABSSetup.ini. Nahraďte text v závorkách (\< \>) s hodnotami ze svého prostředí. Tento text je příklad:

  ```
  [OPTIONS]
  UserName=administrator
  CompanyName=<Microsoft Corporation>
  SQLMachineName=localhost
  SQLInstanceName=<SQL instance name>
  SQLMachineUserName=administrator
  SQLMachinePassword=<admin password>
  SQLMachineDomainName=<machine domain>
  ReportingMachineName=localhost
  ReportingInstanceName=<reporting instance name>
  SqlAccountPassword=<admin password>
  ReportingMachineUserName=<username>
  ReportingMachinePassword=<reporting admin password>
  ReportingMachineDomainName=<domain>
  VaultCredentialFilePath=<vault credential full path and complete name>
  SecurityPassphrase=<passphrase>
  PassphraseSaveLocation=<passphrase save location>
  UseExistingSQL=<1/0 use or do not use existing SQL>
  ```

3. Uložte soubor. Na příkazovém řádku se zvýšenými oprávněními na serveru pro instalaci, zadejte tento příkaz:

  ```
  start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
  ```

Pro instalaci můžete použít tyto příznaky:</br>
**/f**: cesta k souboru INI</br>
**/l**: cesta protokolu</br>
**/i**: Instalační cesta</br>
**/x**: Odinstalujte cesta</br>

## <a name="next-steps"></a>Další kroky
Po instalaci serveru zálohování, zjistěte, jak připravit server nebo začít chránit zatížení.

- [Příprava úlohy zálohování serveru](backup-azure-microsoft-azure-backup.md)
- [Pomocí zálohování serveru zálohovat VMware server](backup-azure-backup-server-vmware.md)
- [Použít zálohování serveru k zálohování systému SQL Server](backup-azure-sql-mabs.md)
- [Přidat moderní úložiště zálohování k zálohování serveru](backup-mabs-add-storage.md)
