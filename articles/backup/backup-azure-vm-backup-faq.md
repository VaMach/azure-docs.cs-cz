---
title: "Nejčastější dotazy k Azure Backup pro virtuální počítače | Dokumentace Microsoftu"
description: "Odpovědi na běžné dotazy týkající se fungování zálohování virtuálních počítačů Azure, omezení a toho, co se stane při změnách zásad."
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: azure vm backup, azure vm restore, backup policy
ms.assetid: c4cd7ff6-8206-45a3-adf5-787f64dbd7e1
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/18/2017
ms.author: trinadhk;pullabhk;
ms.openlocfilehash: d3178413e894c095235dde067b369e3554375aa6
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2017
---
# <a name="questions-about-the-azure-vm-backup-service"></a>Dotazy týkající se služby Azure Backup pro virtuální počítače
Tento článek obsahuje odpovědi na běžné dotazy, které vám pomůžou rychle porozumět komponentám služby Azure Backup pro virtuální počítače. Některé odpovědi zahrnují odkazy na články obsahující komplexní informace. Otázky týkající se služby Azure Backup můžete také publikovat na [diskusním fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="configure-backup"></a>Konfigurace zálohování
### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Podporují trezory Recovery Services klasické virtuální počítače nebo virtuální počítače využívající Resource Manager? <br/>
Trezory Recovery Services podporují oba modely.  Do trezoru služby Recovery Services můžete zálohovat klasický virtuální počítač (vytvořený na portálu Classic) nebo virtuální počítač vytvořený pomocí Resource Manageru (na webu Azure Portal).

### <a name="what-configurations-are-not-supported-by-azure-vm-backup"></a>Jaký konfigurace nepodporuje zálohování virtuálních počítačů Azure?
Projděte [podporované operační systémy](backup-azure-arm-vms-prepare.md#supported-operating-system-for-backup) a [zálohování omezení virtuálních počítačů](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm)

### <a name="why-cant-i-see-my-vm-in-configure-backup-wizard"></a>Proč se můj virtuální počítač nezobrazuje v průvodci konfigurací zálohování?
V Průvodci konfigurace zálohování Azure Backup je uveden pouze virtuální počítače, které jsou:
  * Již není chráněná můžete ověřit zálohování stav virtuálního počítače tak, že přejdete do okna virtuálních počítačů a kontrola stavu zálohování z nabídky nastavení. Další informace o [kontrole stavu zálohování virtuálního počítače](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-management-blade).
  * Patří do stejné oblasti jako virtuální počítač.

## <a name="backup"></a>Zálohování
### <a name="will-on-demand-backup-job-follow-same-retention-schedule-as-scheduled-backups"></a>Bude úloha zálohování na vyžádání používat stejný plán uchovávání jako plánovaní zálohování?
Ne. Musíte zadat rozsah uchování pro úlohu zálohování na vyžádání. Ve výchozím nastavení se uchovávají po dobu 30 dnů při aktivaci z portálu. 

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Na některých virtuálních počítačích byla nedávno povolena služba Azure Disk Encryption. Budou moje zálohy stále fungovat?
Musíte službě Azure Backup udělit oprávnění pro přístup ke službě Key Vault. Tato oprávnění můžete zadat v PowerShellu pomocí kroků popsaných v části *Povolení zálohování* v dokumentaci k [PowerShellu](backup-azure-vms-automation.md).

### <a name="i-migrated-disks-of-a-vm-to-managed-disks-will-my-backups-continue-to-work"></a>Disky virtuálního počítače se migrovaly na spravované disky. Budou moje zálohy stále fungovat?
Ano, zálohování funguje bez problémů a není potřeba změnit konfiguraci zálohování. 

### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>Virtuální počítač je vypnutý. Bude na vyžádání nebo naplánované zálohování pracovní?
Ano. I když je počítač je vypnutý zálohování funguje a bod obnovení je označena jako havárií konzistentní. Další podrobnosti najdete v části konzistence dat v [v tomto článku](backup-azure-vms-introduction.md#how-does-azure-back-up-virtual-machines)

## <a name="restore"></a>Obnovení
### <a name="how-do-i-decide-between-restoring-disks-versus-full-vm-restore"></a>Jak se rozhodnout mezi obnovením disků a úplným obnovením virtuálního počítače?
Azure úplné obnovení virtuálního počítače si můžete představit jako možnost rychle vytvořit. Obnovení virtuálních počítačů možnost změny názvy disků, kontejnery používá tyto disky, veřejné IP adresy a síťové názvy. Změna je nutný v zájmu zachování jedinečnost prostředky vytvořené při vytvoření virtuálního počítače. Ale nepřidá virtuálního počítače do skupiny dostupnosti. 

Obnovení disků použijte k:
  * Přizpůsobení virtuálního počítače, která je vytvořena z bodu v konfiguraci čas, jako je změna velikosti
  * Přidání konfigurace, které se nenacházejí v době zálohování 
  * Řízení zásad vytváření názvů pro vytvářené prostředky.
  * Přidání virtuálního počítače do skupiny dostupnosti.
  * Pro ostatní konfigurace, které lze dosáhnout pouze pomocí prostředí PowerShell a deklarativní šablony definice
  
### <a name="can-i-use-backups-of-unmanaged-disk-vm-to-restore-after-i-upgrade-my-disks-to-managed-disks"></a>Můžete použít zálohování nespravované disku virtuálního počítače k obnovení po upgradu Moje disky na spravované disky?
Ano, můžete použít zálohy pořízené před migrací disky z nespravované na spravované. Ve výchozím nastavení úloha obnovení virtuálního počítače vytvoří virtuální počítač s nespravované disky. Obnovení disky funkce slouží k obnovení disků a použít je k vytvoření virtuálního počítače na spravovaného disky. 

## <a name="manage-vm-backups"></a>Správa záloh virtuálních počítačů
### <a name="what-happens-when-i-change-a-backup-policy-on-vms"></a>Co se stane, když že se na virtuálních počítačích změní zásady zálohování?
Nová zásada se aplikuje na virtuální počítače, je následovaný plán a uchovávání nové zásady, které obsahuje. Pokud je rozšířeno uchování, stávajících bodů obnovení jsou označeny k jejich podle nové zásady. Pokud je snížen uchovávání, jsou označeny pro vyřazení v další úlohy čištění a následně odstranit. 
