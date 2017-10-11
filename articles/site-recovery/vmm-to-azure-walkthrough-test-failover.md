---
title: "Spuštění testovací převzetí služeb při selhání pro replikaci technologie Hyper-V (s nástrojem System Center VMM) do Azure | Microsoft Docs"
description: "Shrnuje kroky, které potřebujete k spuštěná testovací převzetí služeb pro virtuální počítače Hyper-v cloudech VMM replikovat do Azure pomocí služby Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 7b562a23-7ba7-48ee-905d-c22b4b5d6466
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/25/2017
ms.author: raynew
ms.openlocfilehash: 4688fc4bc74a9e0e04487cfbe965006070fd9a7b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="step-11-run-a-test-failover-for-hyper-v-replication-with-vmm-to-azure"></a>Krok 11: Spuštění testovací převzetí služeb při selhání pro replikaci technologie Hyper-V (s nástrojem VMM) do Azure

Po můžete [povolení replikace pro virtuální počítače Hyper-V](vmm-to-azure-walkthrough-enable-replication.md), použijte tento článek spustit testovací převzetí služeb z místní virtuální počítače Hyper-V spravované v cloudech System Center Virtual Machine Manager (VMM) do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.

POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Než začnete

Před spuštěním testu převzetí služeb, které doporučujeme ověřit vlastnosti virtuálního počítače a proveďte požadované změny potřebujete. má přístup k vlastnostem virtuálních počítačů v **replikované položky**. **Essentials** okno zobrazuje informace o nastavení počítače a stav.

## <a name="managed-disk-considerations"></a>Důležité informace o spravovaných disků

[Spravované disky](../virtual-machines/windows/managed-disks-overview.md) zjednodušit správu disku pro virtuální počítače Azure, pomocí správy účty úložiště přidružené disky virtuálních počítačů. 

- Spravované disky jsou vytvořené a připojené k virtuálnímu počítači jenom v případě, že dojde k převzetí služeb při selhání do Azure. Pokud povolíte ochranu, replikuje data z virtuálních počítačů místní účty úložiště.
- Spravované disky se dají vytvořit jenom pro virtuální počítače, které jsou nasazeny pomocí modelu nasazení Resource manager.
- Navrácení služeb po obnovení z Azure do místního prostředí Hyper-V se aktuálně nepodporuje pro počítače s spravované disky. Byste měli nastavit pouze **použijte spravované disky** k **Ano** Pokud provádíte migraci pouze (selhání do Azure bez navrácení služeb po obnovení)
- Toto nastavení povoleno, nastaví pouze dostupnosti ve skupinách prostředků, které mají **použijte spravované disky** povoleno lze vybrat. Virtuální počítače s spravované disky musí být ve skupinách dostupnosti s **použijte spravované disky** nastavena na **Ano**. Pokud toto nastavení není povoleno pro virtuální počítače, můžete vybrat jenom skupiny dostupnosti v skupinám prostředků bez spravovaných disků povolená. [Další informace](../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set).
- - Pokud účet úložiště, které používáte pro replikaci byla zašifrována pomocí šifrování služby úložiště, spravovaných disků nelze vytvořit během převzetí služeb při selhání. V takovém případě buď nemáte povolit použití spravovaných disků, nebo zakažte ochranu pro virtuální počítač a znovu ho na použití účtu úložiště, který nemá povolené šifrování. [Další informace](../virtual-machines/windows/managed-disks-overview.md#managed-disks-and-encryption).

 
## <a name="network-considerations"></a>Důležité informace o síti
    
- Můžete nastavit cílovou IP adresu, který se má použít pro virtuální počítač Azure po převzetí služeb při selhání. Pokud adresu nezadáte, bude počítač, který převezme služby při selhání, používat DHCP. Pokud nastavíte adresu, která není k dispozici pro převzetí služeb při selhání, převzetí služeb při selhání se nezdaří. Stejnou cílovou IP adresu je možné použít pro testovací převzetí služeb při selhání, pokud je adresa k dispozici v testovací síti převzetí služeb při selhání.
- Počet síťových adaptérů závisí na velikosti, kterou zadáte pro cílový virtuální počítač, a to následujícím způsobem:
    - Pokud je počet síťových adaptérů na zdrojovém počítači menší nebo roven počtu adaptérů, které jsou povolené pro velikost cílového počítače, pak bude mít cíl stejný počet adaptérů jako zdroj.
    - Pokud počet adaptérů pro zdrojový virtuální počítač překračuje počet povolený pro cílovou velikost, použije se maximální velikost cíle.
    - Pokud má například zdrojový počítač dva síťové adaptéry a velikost cílového počítače podporuje čtyři, bude mít cílový počítač dva adaptéry. Pokud má zdrojový počítač dva adaptéry, ale podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač pouze jeden adaptér.     
- Pokud má virtuální počítač více síťových adaptérů, připojí se všechny ke stejné síti.
- Pokud virtuální počítač má několik síťových adaptérů se pak stane první z nich uvedené v seznamu *výchozí* síťový adaptér ve virtuálním počítači Azure.


## <a name="view-and-manage-vm-settings"></a>Zobrazit a spravovat nastavení virtuálního počítače

Doporučujeme ověřit vlastnosti zdrojového počítače před spuštěním převzetí služeb při selhání.

1. V **chráněné položky**, klikněte na tlačítko **replikované položky**a klikněte na virtuální počítač.

    ![Povolení replikace](./media/vmm-to-azure-walkthrough-test-failover/test-failover1.png)
2. V **replikované položky** podokně, zobrazí se souhrn informací, stav a nejnovější dostupné body obnovení virtuálního počítače. Klikněte na tlačítko **vlastnosti** zobrazíte další podrobnosti.

    ![Povolení replikace](./media/vmm-to-azure-walkthrough-test-failover/test-failover2.png)
3. V **výpočty a síť**, můžete:
    - Změňte název virtuálního počítače Azure. Název musí splňovat [požadavky pro Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
    - Zadejte post-převzetí služeb při selhání [skupiny prostředků].
    - Zadejte cílovou velikost virtuálního počítače Azure
    - Vyberte [skupinu dostupnosti](../virtual-machines/windows/tutorial-availability-sets.md).
    - Určete, zda chcete použít [discích spravovaných](#managed-disk-considerations). Vyberte **Ano**, pokud se chcete připojit k počítači k migraci na Azure spravované disky.
    - Zobrazit nebo upravit nastavení sítě, včetně sítě a podsítě, ve kterém virtuální počítač Azure budou umístěné po převzetí služeb při selhání a IP adresu, která bude přiřazena k němu.

    ![Povolení replikace](./media/vmm-to-azure-walkthrough-test-failover/test-failover4.png)
4. V **disky**, zobrazí se informace o operačním systému a datové disky na virtuálním počítači.


## <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání

Nyní spusťte převzetí služeb při selhání a ujistěte se, zda že vše funguje podle očekávání.

- Pokud se chcete připojit k virtuálním počítačům Azure po převzetí služeb při selhání, pomocí protokolu RDP [Příprava připojení](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - Abyste mohli převzetí služeb při selhání plně otestovat, musíte zkopírovat Active Directory a DNS do testovacího prostředí. [Další informace](site-recovery-active-directory.md#test-failover-considerations).
 - Úplné informace o převzetí služeb při selhání, přečtěte si [v tomto článku](site-recovery-test-failover-to-azure.md) článku.
 
 Nyní spusťte převzetí služeb při selhání:

1. K převzetí služeb při selhání jednom počítači, v **replikované položky**, klikněte na virtuální počítač > **+ testovací převzetí služeb při selhání** ikonu.
2. Pokud chcete pro převzetí služeb při selhání použít plán obnovení, klikněte v části **Plány obnovení** pravým tlačítkem myši na plán > **Otestovat převzetí služeb při selhání**. Pokud chcete vytvořit plán obnovení, [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md).
3. V **testovací převzetí služeb při selhání**, vyberte síť Azure, ke které virtuální počítače Azure připojí po převzetí služeb při selhání.
4. Kliknutím na **OK** zahajte převzetí služeb při selhání. Průběh můžete sledovat kliknutím na virtuálním počítači otevřete jeho vlastnosti, nebo na **testovací převzetí služeb při selhání** úlohy v název trezoru > **úlohy** > **úlohy Site Recovery**.
5. Po dokončení převzetí služeb při selhání by se vám také měl zobrazit počítač Azure repliky na portálu Azure Portal > **Virtuální počítače**. Měli byste zajistit, aby měl virtuální počítač odpovídající velikost, byl připojený k odpovídající síti a aby běžel.
6. Pokud jste připravili připojení po převzetí služeb při selhání, měli byste být schopni se k virtuálnímu počítači Azure připojit.
7. Až dokončíte, klikněte na **vyčistit testovací převzetí služeb při selhání** v plánu obnovení. V části **Poznámky** si zaznamenejte a uložte jakékoli připomínky související s testovacím převzetím služeb při selhání. Tím odstraníte virtuální počítače, které se vytvořily během testu.



## <a name="next-steps"></a>Další kroky

- [Další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a jak je spouštět.
- [Přečtěte si informace o navrácení služeb po obnovení](site-recovery-failback-from-azure-to-hyper-v.md)navrácení služeb po obnovení a replikovat virtuální počítače Azure zpět na primární místní cloudu VMM.

