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
ms.openlocfilehash: 1372a9e05cb47f6c68240bffccd46b0fbebb5464
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="questions-about-the-azure-vm-backup-service"></a>Dotazy týkající se služby Azure Backup pro virtuální počítače
Tento článek obsahuje odpovědi na běžné dotazy, které vám pomůžou rychle porozumět komponentám služby Azure Backup pro virtuální počítače. Některé odpovědi zahrnují odkazy na články obsahující komplexní informace. Otázky týkající se služby Azure Backup můžete také publikovat na [diskusním fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="configure-backup"></a>Konfigurace zálohování
### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Podporují trezory Recovery Services klasické virtuální počítače nebo virtuální počítače využívající Resource Manager? <br/>
Trezory Recovery Services podporují oba modely.  Do trezoru služby Recovery Services můžete zálohovat klasický virtuální počítač (vytvořený na portálu Classic) nebo virtuální počítač vytvořený pomocí Resource Manageru (na webu Azure Portal).

### <a name="what-configurations-are-not-supported-by-azure-vm-backup-"></a>Jaký konfigurace zálohování virtuálních počítačů Azure nepodporuje?
Přečtěte si témata [Podporované operační systémy](backup-azure-arm-vms-prepare.md#supported-operating-system-for-backup) a [Omezení zálohování virtuálních počítačů](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm).

### <a name="why-cant-i-see-my-vm-in-configure-backup-wizard"></a>Proč se můj virtuální počítač nezobrazuje v průvodci konfigurací zálohování?
V průvodci konfigurací zálohování služba Azure Backup uvádí pouze virtuální počítače, které:
* Ještě nejsou chráněné – Stav zálohování virtuálního počítače můžete ověřit tak, že přejdete do okna virtuálního počítače a zkontrolujete Stav zálohování z nabídky okna Nastavení. Další informace o [kontrole stavu zálohování virtuálního počítače](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-management-blade).
* Patří do stejné oblasti jako virtuální počítač.

## <a name="backup"></a>Zálohování
### <a name="will-on-demand-backup-job-follow-same-retention-schedule-as-scheduled-backups"></a>Bude úloha zálohování na vyžádání používat stejný plán uchovávání jako plánovaní zálohování?
Ne. Pro úlohu zálohování na vyžádání musíte zadat rozsah uchovávání. Ve výchozím nastavení se bude uchovávat po dobu 30 dnů, pokud ji aktivujete z portálu. 

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Na některých virtuálních počítačích byla nedávno povolena služba Azure Disk Encryption. Budou moje zálohy stále fungovat?
Musíte službě Azure Backup udělit oprávnění pro přístup ke službě Key Vault. Tato oprávnění můžete zadat v PowerShellu pomocí kroků popsaných v části *Povolení zálohování* v dokumentaci k [PowerShellu](backup-azure-vms-automation.md).

### <a name="i-migrated-disks-of-a-vm-to-managed-disks-will-my-backups-continue-to-work"></a>Disky virtuálního počítače se migrovaly na spravované disky. Budou moje zálohy stále fungovat?
Ano, zálohy budou fungovat bez problému a není nutné znovu konfigurovat zálohování. 

## <a name="restore"></a>Obnovení
### <a name="how-do-i-decide-between-restoring-disks-versus-full-vm-restore"></a>Jak se rozhodnout mezi obnovením disků a úplným obnovením virtuálního počítače?
Úplné obnovení virtuálního počítače Azure si můžete představit jako určitou možnost rychlého vytvoření pro obnovený virtuální počítač. Možnost obnovení virtuálního počítače změní názvy disků, kontejnery, které disky používají, veřejné IP adresy a názvy síťových rozhraní pro zajištění jedinečnosti prostředků vytvářených v rámci vytváření virtuálního počítače. Kromě toho virtuální počítač nepřidá do skupiny dostupnosti. 

Obnovení disků použijte k:
* Přizpůsobení virtuálního počítače vytvářeného z konfigurace k určitému bodu v čase, například ke změně velikosti na jinou, než je nastavena v konfiguraci zálohování.
* Přidání konfigurací, které nebyly k dispozici v době zálohování. 
* Řízení zásad vytváření názvů pro vytvářené prostředky.
* Přidání virtuálního počítače do skupiny dostupnosti.
* Použití konfigurace, které lze dosáhnout jenom pomocí PowerShellu nebo definice deklarativní šablony.

## <a name="manage-vm-backups"></a>Správa záloh virtuálních počítačů
### <a name="what-happens-when-i-change-a-backup-policy-on-vms"></a>Co se stane, když že se na virtuálních počítačích změní zásady zálohování?
Pokud se na virtuálních počítačích použije nová zásada, plán a uchovávání se budou řídit touto novou zásadou. Pokud se doba uchovávání prodlouží, existující body obnovení se označí k zachování pro novou zásadu. Pokud se doba uchovávání zkrátí, označí se k vyřazení v rámci další úlohy čištění a budou odstraněny. 
