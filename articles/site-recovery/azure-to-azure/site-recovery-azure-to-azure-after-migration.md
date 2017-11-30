---
title: "Příprava počítačů k nastavení zotavení po havárii mezi oblastmi Azure po migraci na Azure pomocí Site Recovery | Microsoft Docs"
description: "Tento článek popisuje postup přípravy počítače, které chcete nastavit zotavení po havárii mezi oblastmi Azure po migraci na Azure pomocí Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: ponatara
ms.openlocfilehash: 7658bedc0bd5c4a289f3271504a006ba54c783b6
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="replicate-azure-vms-to-another-region-after-migration-to-azure-by-using-azure-site-recovery"></a>Replikace virtuálních počítačů Azure do jiné oblasti po migraci na Azure pomocí Azure Site Recovery

>[!NOTE]
> Azure Site Recovery replikaci pro virtuální počítače Azure (VM) je aktuálně ve verzi preview.

## <a name="overview"></a>Přehled

Tento článek vám pomůže připravit virtuální počítače Azure pro replikaci mezi dvěma oblastmi Azure po tyto počítače migraci z místního prostředí do Azure pomocí Azure Site Recovery.

## <a name="disaster-recovery-and-compliance"></a>Zotavení po havárii a dodržování předpisů
V současné době jsou více podniky přesunutí zatížení do Azure. S podniky přesunutí důležité místní úlohy v produkčním prostředí do Azure, nastavení zotavení po havárii pro tyto úlohy je povinné pro dodržování předpisů a zajistí ochranu proti tak možnost narušení v oblasti Azure.

## <a name="steps-for-preparing-migrated-machines-for-replication"></a>Kroky pro přípravu migrované počítače pro replikaci
Příprava migrovat počítačů pro nastavení replikace jiné oblasti Azure:

1. Dokončení migrace.
2. V případě potřeby nainstalujte agenta Azure.
3. Odeberte službu Mobility.  
4. Restartujte virtuální počítač.

Tyto kroky jsou podrobně popsány v další v následujících částech.

### <a name="step-1-migrate-workloads-running-on-hyper-v-vms-vmware-vms-and-physical-servers-to-run-on-azure-vms"></a>Krok 1: Migrace úloh běžících na virtuálních počítačů Hyper-V, virtuálních počítačů VMware a fyzické servery spouštět na virtuálních počítačích Azure

Nastavení replikace a migrovat místní technologie Hyper-V, VMware a fyzické úloh do Azure, postupujte podle kroků v [virtuální počítače Azure IaaS migraci mezi oblastmi Azure s Azure Site Recovery](site-recovery-migrate-azure-to-azure.md) článku. 

Po migraci nemusíte potvrďte, nebo odstraňte převzetí služeb při selhání. Místo toho vyberte **dokončení migrace** možnost pro každý počítač, který chcete migrovat:
1. V části **Replikované položky** klikněte pravým tlačítkem na virtuální počítač a klikněte na **Dokončit migraci**. Klikněte na tlačítko **OK** k dokončení kroku. Můžete sledovat průběh ve vlastnostech virtuálního počítače pomocí monitorování úlohy dokončení migrace v **úlohy Site Recovery**.
2. **Dokončení migrace** akci dokončí proces migrace, odebere replikaci pro počítač a zastaví fakturace Site Recovery pro tento počítač.

### <a name="step-2-install-the-azure-vm-agent-on-the-virtual-machine"></a>Krok 2: Instalace agenta virtuálního počítače Azure na virtuálním počítači
Azure [agenta virtuálního počítače](../../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux) musí být nainstalován na virtuálním počítači pro obnovení lokality rozšíření pro práci a k ochraně virtuálního počítače.

>[!IMPORTANT]
>Počínaje verzí 9.7.0.0, na virtuální počítače s Windows, instalační program služby Mobility nainstaluje nejnovější dostupný agent virtuálního počítače Azure. Virtuální počítač na migraci, splňuje instalace agenta požadované pro používání všech rozšíření virtuálního počítače, včetně přípony Site Recovery. Agent virtuálního počítače Azure je nutné ručně nainstalovat jenom v případě, že je služba Mobility na migrované počítači nainstalovaná verze 9,6 nebo dřívější.

Následující tabulka obsahuje další informace o instalaci agenta virtuálního počítače a ověřuje, zda byl nainstalován:

| **Operace** | **Windows** | **Linux** |
| --- | --- | --- |
| Instalace agenta virtuálního počítače |Stáhněte si a nainstalujte [MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Budete potřebovat oprávnění správce k dokončení instalace. |Nainstalujte si nejnovější verzi [agenta systému Linux](../../virtual-machines/linux/agent-user-guide.md). Budete potřebovat oprávnění správce k dokončení instalace. Doporučujeme nainstalovat agenta z distribuční úložiště. Jsme *nedoporučujeme* instalace agenta virtuálního počítače s Linuxem přímo z Githubu.  |
| Ověření instalace agenta virtuálního počítače |1. Přejděte do složky C:\WindowsAzure\Packages ve virtuálním počítači Azure. Měli byste vidět přítomný soubor WaAppAgent.exe. <br>2. Pravým tlačítkem myši klikněte na soubor, přejděte na **Vlastnosti** a poté vyberte kartu **Podrobnosti**. **Verze produktu** pole by mělo být 2.6.1198.718 nebo vyšší. |Není k dispozici |


### <a name="step-3-remove-the-mobility-service-from-the-migrated-virtual-machine"></a>Krok 3: Odebrání služba Mobility migrovaného virtuálního počítače

Pokud jste migrovali místní počítače VMware nebo fyzických serverů v systému Windows nebo Linux, musíte ručně odebrat nebo odinstalujte službu Mobility z migrovaného virtuálního počítače.

>[!IMPORTANT]
>Tento krok se nevyžaduje pro migraci virtuálních počítačů Hyper-V do Azure.

#### <a name="uninstall-the-mobility-service-on-a-windows-server-vm"></a>Odinstalujte službu Mobility na virtuální počítač Windows serveru
Odinstalujte službu Mobility na počítači s Windows serverem, použijte jednu z následujících metod.

##### <a name="uninstall-by-using-the-windows-ui"></a>Odinstalovat aplikaci pomocí uživatelského rozhraní systému Windows
1. V Ovládacích panelech vyberte **programy**.
2. Vyberte **Microsoft Azure Site Recovery Mobility služby nebo hlavní cílový server**a potom vyberte **odinstalovat**.

##### <a name="uninstall-at-a-command-prompt"></a>Odinstalujte na příkazovém řádku
1. Otevřete okno příkazového řádku jako správce.
2. Odinstalujte službu Mobility, spusťte následující příkaz:

   ```
   MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
   ```

#### <a name="uninstall-the-mobility-service-on-a-linux-computer"></a>Odinstalujte službu Mobility na počítač se systémem Linux
1. Na serveru systému Linux, přihlaste se jako **kořenové** uživatele.
2. V terminálu přejděte na /user/local/ASR.
3. Odinstalujte službu Mobility, spusťte následující příkaz:

   ```
   uninstall.sh -Y
   ```

### <a name="step-4-restart-the-vm"></a>Krok 4: Restartování virtuálního počítače

Po dokončení odinstalace služby Mobility, restartujte virtuální počítač předtím, než nastavení replikace na jiné oblasti Azure.


## <a name="next-steps"></a>Další kroky
- Začněte chránit vaše úlohy [replikovat virtuální počítače Azure](azure-to-azure-quickstart.md).
- Další informace o [sítě pokyny pro replikaci virtuálních počítačů Azure](site-recovery-azure-to-azure-networking-guidance.md).
