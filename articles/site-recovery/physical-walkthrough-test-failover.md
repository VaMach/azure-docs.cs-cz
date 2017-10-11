---
title: "Spuštění testovací převzetí služeb při selhání pro fyzický server replikaci do Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky, které potřebujete pro spouštění testovací převzetí služeb při selhání pro [fyzické servery replikující se do Azure pomocí služby Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a640e139-3a09-4ad8-8283-8fa92544f4c6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 94aa3bfc700cad3de9fc5516c0c9a4d86ade3fed
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="step-11-run-a-test-failover-of-physical-servers-to-azure"></a>Krok 11: Spuštění testovací převzetí služeb při selhání fyzických serverů do Azure

Tento článek popisuje, jak spustit testovací převzetí služeb z místní fyzických serverů do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.

POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Než začnete

Před spuštěním testu převzetí služeb, které doporučujeme ověřit vlastnosti serveru a proveďte požadované změny potřebujete. má přístup k vlastnostem virtuálních počítačů v **replikované položky**. **Essentials** okno se zobrazí informace o stavu a nastavení počítače.

## <a name="managed-disk-considerations"></a>Důležité informace o spravovaných disků

[Spravované disky](../virtual-machines/windows/managed-disks-overview.md) zjednodušit správu disku pro virtuální počítače Azure, pomocí správy účty úložiště přidružené disky virtuálních počítačů. 

- Pokud povolíte ochranu pro server, data virtuálního počítače se replikuje na účet úložiště. Spravované disky jsou vytvořené a připojené k virtuálnímu počítači jenom v případě, že dojde k převzetí služeb při selhání.
- Spravované disky se dají vytvořit jenom pro virtuální počítače Azure, které jsou nasazeny pomocí modelu Resource Manager.  
- Toto nastavení povoleno, nastaví pouze dostupnosti ve skupinách prostředků, které mají **použijte spravované disky** povoleno lze vybrat. Virtuální počítače s spravované disky musí být ve skupinách dostupnosti s **použijte spravované disky** nastavena na **Ano**. Pokud toto nastavení není povoleno pro virtuální počítače, můžete vybrat jenom skupiny dostupnosti v skupinám prostředků bez spravovaných disků povolená.
- [Další informace](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set) o spravovaných disků a dostupnosti nastaví.
- Pokud účet úložiště, které používáte pro replikaci byla zašifrována pomocí šifrování služby úložiště, spravovaných disků nelze vytvořit během převzetí služeb při selhání. V takovém případě buď nemáte povolit použití spravovaných disků, nebo zakažte ochranu pro virtuální počítač a znovu ho na použití účtu úložiště, který nemá povolené šifrování. [Další informace](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).


## <a name="network-considerations"></a>Důležité informace o síti

Můžete nastavit cílovou IP adresu pro virtuální počítač Azure vytvořené po převzetí služeb při selhání.

- Pokud adresu nezadáte, bude počítač, který převezme služby při selhání, používat DHCP.
- Pokud nastavíte adresu, která není k dispozici na převzetí služeb při selhání, převzetí služeb při selhání nebude fungovat.
- Stejnou cílovou IP adresu lze pro testovací převzetí služeb při selhání, pokud je adresa k dispozici v testovací síti převzetí služeb při selhání.
- Počet síťových adaptérů je závisí na velikosti, kterou zadáte pro cílový virtuální počítač:

     - Pokud počet síťových adaptérů na zdrojovém počítači je stejný jako nebo menší než počet adaptérů povoleno pro velikost cílového počítače a potom cíl bude mít stejný počet adaptérů jako zdroj.
     - Pokud počet adaptérů pro zdrojový virtuální počítač překračuje počet povolený pro cílovou velikost, pak se použije maximální velikost cíle.
     - Pokud má například zdrojový počítač dva síťové adaptéry a velikost cílového počítače podporuje čtyři, bude mít cílový počítač dva adaptéry. Pokud má zdrojový počítač dva adaptéry, ale podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač pouze jeden adaptér.     
   - Pokud má virtuální počítač více síťových adaptérů připojí se všechny ke stejné síti.
   - Pokud virtuální počítač má několik síťových adaptérů se pak stane první z nich uvedené v seznamu *výchozí* síťový adaptér ve virtuálním počítači Azure.
 - [Další informace](vmware-walkthrough-network.md) o IP adresách.



## <a name="view-and-modify-vm-settings"></a>Zobrazení a úprava nastavení virtuálního počítače

Doporučujeme ověřit vlastnosti zdrojového serveru před spuštěním převzetí služeb při selhání.

1. V **chráněné položky**, klikněte na tlačítko **replikované položky**a klikněte na počítač.
2. V **replikované položky** podokně se zobrazí souhrnné informace o počítači, stav a nejnovější dostupné body obnovení. Klikněte na tlačítko **vlastnosti** zobrazíte další podrobnosti.
3. V **výpočty a síť**, můžete:
    - Změňte název virtuálního počítače Azure. Název musí splňovat [požadavky pro Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
    - Zadejte post-převzetí služeb při selhání [skupiny prostředků].
    - Zadejte cílovou velikost virtuálního počítače Azure
    - Vyberte [skupinu dostupnosti](../virtual-machines/windows/tutorial-availability-sets.md).
    - Určete, zda chcete použít [discích spravovaných](#managed-disk-considerations). Vyberte **Ano**, pokud se chcete připojit k počítači k migraci na Azure spravované disky.
    - Zobrazit nebo upravit nastavení sítě, včetně sítě a podsítě, ve kterém virtuální počítač Azure budou umístěné po převzetí služeb při selhání a IP adresu, která bude přiřazena k němu.
4. V **disky**, zobrazí se informace o operačním systému a datové disky na virtuálním počítači.

## <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání

Poté, co jste nastavili vše, spusťte převzetí služeb při selhání a ujistěte se všechno funguje podle očekávání.

- Pokud se chcete připojit k virtuálním počítačům Azure po převzetí služeb při selhání, pomocí protokolu RDP [Příprava připojení](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - Abyste mohli převzetí služeb při selhání plně otestovat, musíte zkopírovat Active Directory a DNS do testovacího prostředí. [Další informace](site-recovery-active-directory.md#test-failover-considerations).
 - Úplné informace o převzetí služeb při selhání, přečtěte si [v tomto článku](site-recovery-test-failover-to-azure.md) článku.
- Vám zajistí rychlý přehled videa, než začnete:

     
     >[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video4-Recovery-Plan-DR-Drill-and-Failover/player]

Nyní spusťte převzetí služeb při selhání:

1. K převzetí služeb při selhání jednom počítači, v **nastavení** > **replikované položky**, klikněte na počítač > **+ testovací převzetí služeb při selhání** ikonu.

    ![Testovací převzetí služeb při selhání](./media/physical-walkthrough-test-failover/test-failover.png)

2. Pokud chcete pro převzetí služeb při selhání použít plán obnovení, klikněte v **Nastavení** > **Plány obnovení** pravým tlačítkem myši na plán > **Testovací převzetí služeb při selhání**. Pokud chcete vytvořit plán obnovení, [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md).  

3. V **testovací převzetí služeb při selhání**, vyberte síť Azure, ke které virtuální počítače Azure připojí po převzetí služeb při selhání.

4. Kliknutím na **OK** zahajte převzetí služeb při selhání. Průběh můžete sledovat kliknutím na počítači otevřete jeho vlastnosti, nebo na **testovací převzetí služeb při selhání** úlohy v název trezoru > **nastavení** > **úlohy** > **úlohy Site Recovery**.

5. Po dokončení převzetí byste také měli vidět repliky virtuálního počítače Azure se zobrazovat na portálu Azure > **virtuální počítače**. Měli byste zajistit, aby měl virtuální počítač odpovídající velikost, byl připojený k odpovídající síti a aby běžel.

6. Pokud jste připravili připojení po převzetí služeb při selhání, měli byste být schopni se k virtuálnímu počítači Azure připojit.

### <a name="delete-test-failover-vms"></a>Odstranit testovací převzetí služeb při selhání virtuálních počítačů

1. Až dokončíte, klikněte na **vyčistit testovací převzetí služeb při selhání** do plánu obnovení nebo počítače.
2. V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s testovací převzetí služeb.
3. Akce čištění odstraní virtuální počítače Azure, které se vytvořily během testovacího převzetí služeb při selhání.

## <a name="summary"></a>Souhrn

Pokud jste úspěšně dokončili testu převzetí služeb, jsou replikace fyzických serverů a jste zkontrolovali, zda mohou přecházet do Azure. Nyní můžete spustit převzetí služeb při selhání v souladu s požadavky vaší organizace. 

Mějte na paměti, že nelze označit aktuálně zpět z Azure na fyzický server. Budete muset navrácení služeb po obnovení virtuálního počítače VMware. To znamená, že potřebujete místní infrastruktury VMware k navrácení služeb po obnovení. [Další informace](site-recovery-failback-azure-to-vmware.md) o selhání zpět do VMware virtuálních počítačích Azure.


## <a name="next-steps"></a>Další kroky

- [Spustit převzetí služeb při selhání](site-recovery-failover.md) podle potřeby.
