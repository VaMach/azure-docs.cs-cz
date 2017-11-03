---
title: "Převzetí služeb při selhání a selhání zálohování virtuálních počítačů Hyper-V replikovat do Azure pomocí Site Recovery | Microsoft Docs"
description: "Naučte se vrátí zpátky na místní lokalitu s Azure Site Recovery a selhání virtuálních počítačů Hyper-V do Azure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72065d01-ed0f-430e-b117-a5885cecd1db
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/15/2017
ms.author: raynew
ms.openlocfilehash: e1cc21661450a983c25b24fe2a6228e26ceecec6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-azure"></a>Převzetí služeb při selhání a selhání zálohování virtuálních počítačů Hyper-V replikovat do Azure

[Azure Site Recovery](site-recovery-overview.md) služby spravuje a orchestruje replikaci, převzetí služeb při selhání a navrácení služeb po obnovení místního počítače a virtuální počítače Azure (VM).

Tento kurz popisuje, jak k převzetí služeb virtuálního počítače technologie Hyper-V do Azure. Poté, co jste při selhání, můžete zpět do místního serveru nezdaří případě, že je k dispozici. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Převzít služby virtuálních počítačů technologie Hyper-V z místní do Azure
> * Selhání zpět z Azure do místní a spusťte znovu replikaci do Azure

## <a name="overview"></a>Přehled
Převzetí služeb při selhání a navrácení služeb po obnovení má dvou fázích:

1. **Převzetí služeb při selhání do Azure**: převzetí služeb při selhání počítačů z místní lokality do Azure.
2. **Po obnovení vrátit Azure k místnímu**: spuštění plánovaného převzetí služeb při selhání z Azure do místní. Po plánovaném převzetí služeb při selhání povolte zpětná replikace, do zahájení replikace z místního do Azure znovu. 


## <a name="fail-over-to-azure"></a>Převzetí služeb při selhání do Azure

### <a name="failover-prerequisites"></a>Požadavky převzetí služeb při selhání

K dokončení převzetí služeb při selhání:

- Ujistěte se, když jste dokončili [postupu zotavení po havárii](tutorial-dr-drill-azure.md) zkontrolujte, zda vše funguje podle očekávání.
- Volitelná Příprava pro připojení k virtuálním počítačům Azure předtím, než jste převzetí služeb při selhání.
- Ověřte vlastnosti virtuálního počítače, před spuštěním převzetí služeb při selhání.

#### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Příprava připojení k virtuálním počítačům Azure po převzetí služeb při selhání

Pokud se chcete připojit k virtuálním počítačům Azure po převzetí služeb při selhání pomocí protokolu RDP, postupujte takto:

##### <a name="azure-vms-running-windows"></a>Virtuální počítače Azure s Windows

1. Chcete-li získat přístup k virtuálním počítačům Azure přes internet, povolte RDP pro virtuální počítač na místě před převzetí služeb při selhání. Ujistěte se, že TCP a UDP pravidla jsou přidány k **veřejné** profilu a že je v povolené RDP **brány Windows Firewall** > **povolené aplikace** pro všechny profily.
2. Pro přístup prostřednictvím sítě site-to-site VPN, povolte RDP na místním počítači. RDP má být povoleno v **brány Windows Firewall** -> **povolené aplikace a funkce** pro **domény a privátní** sítě. Zkontrolujte, jestli zásada SAN operačního systému je nastavena na **OnlineAll**. [Další informace](https://support.microsoft.com/kb/3031135). Měla by existovat žádné Windows aktualizace čekající na vyřízení ve virtuálním počítači při aktivaci převzetí služeb při selhání. Pokud existují, nebudete moct přihlásit k virtuálnímu počítači, dokud se nedokončí aktualizace. 
3. Windows Azure virtuálního počítače po převzetí služeb při selhání, zkontrolujte **spouštění diagnostiky** zobrazíte snímek virtuálního počítače. Pokud se nemůžete připojit, zkontrolujte, zda je virtuální počítač spuštěný a zkontrolujte tyto [tipy pro odstraňování potíží](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


##### <a name="azure-vms-running-linux"></a>Virtuální počítače Azure s Linuxem

1. Na místním počítači před převzetí služeb při selhání zkontrolujte, že služba Secure Shell nastavena na automatické spuštění při spuštění systému. Zkontrolujte, jestli pravidla brány firewall umožňují připojení SSH.
2. Na virtuálním počítači Azure po převzetí služeb při selhání povolte příchozí připojení k portu SSH pro pravidel skupiny zabezpečení sítě na selhání virtuálních počítačů a v podsíti Azure, ke kterému je připojený.  [Přidejte veřejnou IP adresu](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) pro virtuální počítač. Můžete zkontrolovat **spouštění diagnostiky** zobrazíte snímek virtuálního počítače.


#### <a name="verify-vm-properties"></a>Ověřte vlastnosti virtuálního počítače

Ověřte vlastnosti virtuálního počítače a ujistěte se, že virtuální počítač splňuje [požadavky pro Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. V **chráněné položky**, klikněte na tlačítko **replikované položky** > virtuálních počítačů.
2. V **replikované položky** podokně je souhrn informace o virtuálních počítačů, stav, a nejnovější dostupný bod obnovení. Klikněte na tlačítko **vlastnosti** zobrazíte další podrobnosti.
3. V **výpočty a síť**, můžete upravit název Azure, skupinu prostředků, cílovou velikost [skupinu dostupnosti](../virtual-machines/windows/tutorial-availability-sets.md), a [spravovat nastavení disku](#managed-disk-considerations)
4. Můžete zobrazit a upravit nastavení sítě, včetně sítě a podsítě, ve kterém virtuální počítač Azure budou umístěné po převzetí služeb při selhání a IP adresu, která bude přiřazena k němu.
5. V **disky**, zobrazí se informace o operačním systému a datové disky na virtuálním počítači.


### <a name="run-a-failover-from-on-premises-to-azure"></a>Spuštění z místního převzetí služeb při selhání do Azure

Můžete spustit standardním nebo plánované převzetí služeb při selhání pro virtuální počítače Hyper-V

- Použijte regulární převzetí služeb při selhání pro neočekávaných výpadků. Při spuštění této převzetí služeb při selhání vytvoří virtuální počítač Azure Site Recovery a zajišťuje nahoru. Můžete spustit převzetí služeb při selhání pro konkrétní bod obnovení. Může dojít ke ztrátě dat v závislosti na bod obnovení, které používáte.
- Plánované převzetí služeb při selhání můžete použít, údržbě nebo při očekávaný výpadek. Tato možnost poskytuje nulovou ztrátou data. Když se aktivuje plánované převzetí služeb při selhání, jsou zdrojové virtuální počítače vypnout. Nesynchronizovaná data synchronizována a aktivaci převzetí služeb při selhání.

Tento postup popisuje, jak spustit regulární převzetí služeb při selhání.


1. V **nastavení** > **replikované položky** klikněte na virtuální počítač > **převzetí služeb při selhání**.
2. V **převzetí služeb při selhání** vyberte **bod obnovení** k převzetí služeb při selhání. Můžete použít jednu z následujících možností:
    - **Nejnovější** (výchozí): tuto možnost napřed zpracuje všechna data přenášená k Site Recovery. Poskytuje nejnižší plánovaný bod obnovení (plánovaného bodu obnovení), protože virtuální počítač Azure vytvořené po převzetí služeb při selhání se všechna data, která se replikují do Site Recovery, když bylo spuštěno převzetí služeb při selhání.
    - **Nejnovější zpracované**: tuto možnost převezme virtuálního počítače do nejnovějšího bodu obnovení, které jsou zpracovávány Site Recovery. Tato možnost poskytuje nízkou RTO (plánovanou dobu obnovení), protože je žádný čas strávený zpracováváním nezpracovaná data.
    - **Nejnovější aplikace konzistentní**: tuto možnost převezme virtuálního počítače do bodu nejnovější konzistentní obnovení zpracovaných službou Site Recovery. 
    - **Vlastní**: Zadejte bod obnovení.
3. Pokud jste selhání virtuálních počítačů technologie Hyper-V v cloudech System Center VMM do Azure, a je povolené šifrování dat pro cloud, v **šifrovací klíč**, vyberte certifikát, který byl vydán, pokud povolíte šifrování dat během instalace zprostředkovatele na server VMM...
4. Vyberte **vypnout počítač před zahájením převzetí služeb při selhání** Pokud chcete, aby Site Recovery se pokusit o proveďte vypnutí zdrojových virtuálních počítačích než převzetí služeb při selhání. Site Recovery se také pokusí synchronizovat místní data, která nebyla dosud odeslána do Azure, než převzetí služeb při selhání. Všimněte si, že převzetí služeb při selhání pokračovat i v případě, že vypnutí selže. Můžete sledovat průběh převzetí služeb při selhání **úlohy** stránky.
5. Pokud jste připravili pro připojení k virtuálnímu počítači Azure, připojte k ověření po převzetí služeb při selhání.
6. Po ověření, **potvrdit** převzetí služeb při selhání. Tím se odstraní všechny body obnovení k dispozici.

> [!WARNING]
> **Nemáte zrušení převzetí služeb při selhání v průběhu**: před zahájením převzetí služeb při selhání replikace virtuálního počítače je zastavena. Pokud zrušíte převzetí služeb při selhání v průběhu, zastaví převzetí služeb při selhání, ale nebude znovu replikaci virtuálního počítače.  


## <a name="fail-back-from-azure-to-on-premises"></a>Po obnovení vrátit Azure k místnímu

### <a name="prerequisites-for-failback"></a>Předpoklady pro navrácení služeb po obnovení

Dokončit navrácení služeb po obnovení, ujistěte se, že server primární lokality VMM server nebo Hyper-V je připojený k Site Recovery.


### <a name="run-a-failback-and-reprotect-on-premises-vms"></a>Spusťte navrácení služeb po obnovení a znovu aktivujte ochranu virtuálních počítačů na místě

Převzetí služeb při selhání z Azure na místní lokalitu a spustit replikaci virtuálních počítačů z místní lokality do Azure.

1. V **nastavení** > **replikované položky** klikněte na virtuální počítač > **plánované převzetí služeb při selhání**.
2. V **potvrďte plánované převzetí služeb při selhání**, ověřte směr převzetí služeb při selhání (z Azure) a vyberte zdrojové a cílové umístění. 
3. V **synchronizace dat**, zadejte, jak chcete synchronizovat:
    - **Synchronizace dat před převzetí služeb při selhání (synchronizovat jenom rozdílové změny)**– tato možnost minimalizuje výpadek virtuálních počítačů, protože synchronizuje bez vypnutí virtuálního počítače. Tady je co umožňuje:
        1. Pořídí snímek virtuálního počítače Azure a zkopíruje jej na hostitele Hyper-V na místě. Virtuální počítač neustále běží v Azure.
        2. Vypne virtuální počítač Azure, tak, aby žádné nové změny dojít k dispozici. Závěrečné sady rozdílové změny přenesou na místním serveru, a jestli je spuštěná místní virtuální počítač.
    - **Synchronizace dat během pouze převzetí služeb při selhání (úplná ke stažení)**– tuto možnost použijte, pokud jste jste už běží v Azure po dlouhou dobu. Tato možnost je rychlejší, protože očekávané více změny na disku a nebudete věnovat času kontrolního součtu. Tato možnost provede stahování disku. Je také užitečné při místní virtuální počítač byl odstraněn.
4. Pokud jste selhání virtuálních počítačů technologie Hyper-V v cloudech System Center VMM do Azure, a je povolené šifrování dat pro cloud, v **šifrovací klíč**, vyberte certifikát, který byl vydán, pokud povolíte šifrování dat během instalace zprostředkovatele na VMM server.
5. Zahájit převzetí služeb při selhání. Můžete sledovat průběh převzetí služeb při selhání **úlohy** kartě.
6. Pokud jste vybrali k synchronizaci dat před převzetí služeb při selhání, po synchronizaci počáteční data se provádí a jste připraveni vypnout a klikněte na virtuálních počítačích Azure **úlohy** > název úlohy plánované převzetí služeb při selhání > **dokončenípřevzetíslužebpřiselhání**. To vypne virtuální počítač Azure, přenáší nejnovější změny místní a spustí virtuální počítač místně.
7. Přihlaste se k virtuálnímu počítači na místě a zkontrolovat, zda že je k dispozici podle očekávání.
8. Místní virtuální počítač je nyní ve stavu čekající potvrzení. Klikněte na tlačítko **potvrzení**, potvrzení převzetí služeb při selhání. Odstraní virtuální počítače Azure a jeho disky a připraví místní virtuální počítač na zpětnou replikaci.
9. Pokud chcete spustit replikovat místní virtuální počítač Azure, povolte **zpětnou replikaci**.


> [!NOTE]
> Zpětná replikace replikuje pouze změny, ke kterým došlo, protože virtuální počítač Azure se vypnulo a posílají se jenom rozdílové změny.

