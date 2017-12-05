---
title: "Zálohování virtuálních počítačů Azure v Azure škálovaně | Microsoft Docs"
description: "Tento kurz údaje zálohování Azure více virtuálních počítačů do trezoru služeb zotavení."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "zálohování virtuálních počítačů; zálohování virtuálního počítače; zálohování a zotavení po havárii"
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/06/2017
ms.author: trinadhk;jimpark;markgal;
ms.custom: mvc
ms.openlocfilehash: 01609c00c6f0585eff4843932b9eb7a090a59c19
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="back-up-azure-virtual-machines-in-azure-at-scale"></a>Zálohování Azure virtuálních počítačů v Azure ve velkém měřítku

V tomto kurzu podrobné informace o tom, jak zálohovat virtuální počítače Azure do trezoru služeb zotavení. Většina práce pro zálohování virtuálních počítačů je přípravy. Předtím, než můžete zálohovat až (nebo chránit) virtuálního počítače, musíte provést [požadavky](backup-azure-arm-vms-prepare.md) při přípravě svého prostředí pro ochranu virtuálních počítačů. 

> [!IMPORTANT]
> Tento kurz předpokládá, že jste již vytvořili skupinu prostředků a virtuální počítač Azure.

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

A [trezor služeb zotavení](backup-azure-recovery-services-vault-overview.md) je kontejner, který obsahuje body obnovení zálohovaných položek. Trezor služeb zotavení je prostředek služby Azure, kterou můžete nasadit a spravovat v rámci skupiny prostředků Azure. V tomto kurzu vytvoříte trezor služeb zotavení ve stejné skupině prostředků jako virtuální počítač chráněn.


Při prvním použití zálohování Azure, je nutné zaregistrovat službu Azure Recovery zprostředkovatele s vaším předplatným. Pokud je už zaregistrovaný zprostředkovatel s vaším předplatným, přejděte k dalšímu kroku.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices
```

Vytvořte trezor služby Recovery Services pomocí rutiny **New-AzureRmRecoveryServicesVault**. Nezapomeňte zadat název skupiny prostředků a umístění použít při konfiguraci virtuálního počítače, který chcete zálohovat. 

```powershell
New-AzureRmRecoveryServicesVault -Name myRSvault -ResourceGroupName "myResourceGroup" -Location "EastUS"
```

Mnoho rutin Azure Backup vyžadují objekt trezoru služeb zotavení jako vstup. Z tohoto důvodu je vhodné pro uložení objektu trezoru služeb zotavení zálohování v proměnné. Potom pomocí **Set-AzureRmRecoveryServicesBackupProperties** nastavit **- BackupStorageRedundancy** možnost k [geograficky redundantní úložiště (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage). 

```powershell
$vault1 = Get-AzureRmRecoveryServicesVault –Name myRSVault
Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
```

## <a name="back-up-azure-virtual-machines"></a>Zálohování virtuálních počítačů Azure

Než můžete spustit prvotní zálohování, musíte nastavit kontext úložiště. Kontext trezoru je typ dat chráněných v trezoru. Při vytváření trezoru služeb zotavení dodává s výchozí ochrana a zásady uchovávání informací. Výchozí zásady ochrany aktivuje úlohu zálohování každý den v zadanou dobu. Výchozí zásady uchovávání informací zachová denního bodu obnovení po dobu 30 dnů. V tomto kurzu přijměte výchozí zásady. 

Použití  **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  nastavit kontext úložiště. Po nastavení trezoru rámci platí pro všechny následující rutiny. 

```powershell
Get-AzureRmRecoveryServicesVault -Name myRSVault | Set-AzureRmRecoveryServicesVaultContext
```

Použití  **[zálohování AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)**  se spustit úlohu zálohování. Úloha zálohování vytváří bod obnovení. Pokud je prvotní zálohování, bod obnovení je úplné zálohování. Následné zálohy vytvořit přírůstkové kopie.

```powershell
$namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered -FriendlyName "V2VM"
$item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType AzureVM
$job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
```

## <a name="delete-the-recovery-services-vault"></a>Odstranit trezor služeb zotavení

Pokud chcete odstranit trezor služeb zotavení, musíte nejprve odstranit všechny body obnovení v trezoru a potom zrušit registraci k trezoru. Následující příkazy podrobnosti tyto kroky. 


```powershell
$Cont = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered
$PI = Get-AzureRmRecoveryServicesBackupItem -Container $Cont[0] -WorkloadType AzureVm
Disable-AzureRmRecoveryServicesBackupProtection -RemoveRecoveryPoints $PI[0]
Unregister-AzureRmRecoveryServicesBackupContainer -Container $namedContainer
Remove-AzureRmRecoveryServicesVault -Vault $vault1
```

## <a name="troubleshooting-errors"></a>Řešení potíží s chybami
Pokud narazíte na problémy při zálohování virtuálního počítače, přečtěte si téma [zálohovat virtuální počítače Azure řešení potíží s článku](backup-azure-vms-troubleshoot.md) nápovědu.

## <a name="next-steps"></a>Další kroky
Teď, když chráníte virtuální počítače, najdete v následujících článcích Další informace o úlohy správy a obnovení virtuálních počítačů z bodu obnovení.

* Chcete-li upravit zásady zálohování, přečtěte si téma [AzureRM.RecoveryServices.Backup použití rutiny pro zálohování virtuálních počítačů](backup-azure-vms-automation.md#create-a-protection-policy).
* [Správa a monitorování virtuálních počítačů](backup-azure-manage-vms.md)
* [Obnovení virtuálních počítačů](backup-azure-arm-restore-vms.md)
