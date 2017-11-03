---
title: "Navrácení služeb po obnovení v Azure Site Recovery pro virtuální počítače Hyper-v | Microsoft Docs"
description: "Azure Site Recovery koordinuje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Další informace o navrácení služeb po obnovení z Azure do místního datového centra."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 7f478a61ee448d2d18b3ac7bc0a579b6e341c30d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="failback-in-site-recovery-for-hyper-v-virtual-machines"></a>Navrácení služeb po obnovení ve službě Site Recovery pro virtuální počítače Hyper-V

Tento článek popisuje, jak navrácení služeb po obnovení virtuální počítače chráněné službou Site Recovery.

## <a name="prerequisites"></a>Požadavky
1. Ověřte, zda je připojen serveru primární lokality VMM server nebo Hyper-V.
2. Měli jste provedli **potvrdit** na virtuálním počítači.

## <a name="why-is-there-no-button-called-failback"></a>Proč je k dispozici žádné tlačítko názvem navrácení služeb po obnovení?
Na portálu neexistuje žádné explicitní gesto názvem navrácení služeb po obnovení. Navrácení služeb po obnovení je krok, kde jste se vraťte k primární lokalitě. Podle definice převzetí služeb při selhání je, když jste převzetí služeb při selhání virtuálních počítačů ze primary(on-premises) lokality pro obnovení (Azure) a navrácení služeb po obnovení je, když jste převzetí služeb při selhání virtuálního počítače z obnovení zpět na primární.

Když iniciujete převzetí služeb při selhání, v okně informuje o směr úlohy. V případě směru z Azure do místní, je navrácení služeb po obnovení.

## <a name="why-is-there-only-a-planned-failover-gesture-to-failback"></a>Proč je k dispozici pouze gesto plánované převzetí služeb při selhání pro navrácení služeb po obnovení?
Azure je prostředí s vysokou dostupností a virtuální počítače vždy budou k dispozici. Navrácení služeb po obnovení je plánované aktivity, kde se rozhodnete pro malé výpadek tak, aby úlohy můžete začít znovu spouštět místně. Toto předpokládá, že nedošlo ke ztrátě dat. Proto je k dispozici pouze gesto plánované převzetí služeb při selhání, který bude vypnout virtuálních počítačů v Azure, stáhněte nejnovější změny a ujistěte se, že nedošlo ke ztrátě dat.

## <a name="do-i-need-a-process-server-in-azure-to-failback-to-hyper-v"></a>Potřebuji procesní server v Azure k navrácení služeb po obnovení do Hyper-v?
Ne, je potřeba server procesu, pouze pokud chráníte virtuální počítače VMware. Žádné další součásti je potřeba nasadit při ochraně nebo navrácení služeb po obnovení virtuálních počítačů technologie Hyper-v.

## <a name="initiate-failback"></a>Zahájit navrácení služeb po obnovení
Po převzetí služeb při selhání z primárního na sekundární umístění replikované virtuální počítače nejsou chráněny službou Site Recovery a sekundární umístění teď funguje jako aktivní umístění. Podle následujících pokynů k selhání zpět na původní primární lokality. Tento postup popisuje, jak spustit plánované převzetí služeb při selhání pro plán obnovení. Případně můžete spustit převzetí služeb při selhání pro jeden virtuální počítač na **virtuální počítače** kartě.

1. Vyberte **plány obnovení** > *recoveryplan_name*. Klikněte na tlačítko **převzetí služeb při selhání** > **plánované převzetí služeb při selhání**.
2. Na ** potvrďte plánované převzetí služeb při selhání ** vyberte zdrojové a cílové umístění. Všimněte si, že směr převzetí služeb při selhání. Pokud převzetí služeb při selhání z primárního fungovala jako očekávat a všechny virtuální počítače jsou v sekundárním umístění, které toto je pouze pro informaci.
3. Pokud po obnovení zpět z Azure vyberte nastavení v **synchronizace dat**:

   * **Synchronizace dat před převzetí služeb při selhání (pouze synchronizovat rozdílové změny)**– tato možnost minimalizuje prostoje pro virtuální počítače jako synchronizuje bez jejich vypínání. Ji provede následující akce:
     * Fáze 1: Vytváří snímek virtuálního počítače v Azure a zkopíruje jej na hostitele Hyper-V na místě. Bude počítač dál běží v Azure.
     * Fáze 2: Vypne virtuální počítač v Azure tak, aby žádné nové změny dojít k dispozici. Závěrečné sady rozdílového změny přenesou na místním serveru a místní virtuální počítač je spuštění.

    - **Synchronizace dat během pouze převzetí služeb při selhání (úplná ke stažení)**– tuto možnost použijte, pokud jste jste už běží v Azure po dlouhou dobu. Tato možnost je rychlejší, protože Očekáváme, že došlo ke změně většinu disku a nechceme při výpočtu kontrolního součtu. Provede stahování disku. Je také užitečné při místní virtuální počítač byl odstraněn.

    >[!NOTE]
    >Doporučujeme tuto možnost použijte, pokud jste byla spuštěna Azure nějakou dobu (v měsíci nebo více) nebo místní virtuální počítač je odstraněný. Tato možnost nebude provádět výpočty kontrolního součtu.


4. Pokud je povolené šifrování dat pro cloud, v **šifrovací klíč** vyberte certifikát, který byl vydán, pokud povolíte šifrování dat během instalace zprostředkovatele na serveru VMM.
5. Zahájit převzetí služeb při selhání. Můžete sledovat průběh převzetí služeb při selhání **úlohy** kartě.
6. Pokud jste vybrali možnost pro synchronizaci dat před převzetí služeb při selhání, po dokončení synchronizace počátečního data a jste připraveni vypnout virtuální počítače v Azure, klikněte na tlačítko **úlohy** název úlohy plánované převzetí služeb při selhání **dokončení převzetí služeb při selhání**. To vypne počítač Azure, přenáší nejnovější změny na místním virtuálním počítači a spuštění virtuálního počítače na místní.
7. Můžete teď se přihlásit k ověření na virtuální počítač je k dispozici podle očekávání.
8. Virtuální počítač je ve stavu čekání na potvrzení. Klikněte na tlačítko **potvrzení** potvrzení převzetí služeb při selhání.
9. Nyní, aby bylo možné dokončit navrácení služeb po obnovení, klikněte na tlačítko **zpětnou replikaci** ke spuštění ochrany virtuálního počítače v primární lokalitě.

## <a name="failback-to-an-alternate-location"></a>Navrácení služeb po obnovení do alternativního umístění
Pokud jste nasadili ochrany mezi [web Hyper-V a Azure](site-recovery-hyper-v-site-to-azure.md) budete muset možnost navrácení služeb po obnovení z Azure do alternativní místní umístění. To je užitečné, pokud je potřeba nastavit novou místní hardware. Zde je postup ho.

1. Pokud instalujete nový hardware nainstalujte Windows Server 2012 R2 a roli Hyper-V na serveru.
2. Vytvořte virtuální síťový přepínač se stejným názvem, který měl na původním serveru.
3. Vyberte **chráněné položky** -> **skupiny ochrany**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> chcete navrácení služeb po obnovení a vyberte **plánované převzetí služeb při selhání**.
4. V **potvrďte plánované převzetí služeb při selhání** vyberte **vytvořit místní virtuální počítač Pokud neexistuje**.
5. V **název hostitele** vyberte nový server hostitele technologie Hyper-V, na kterém chcete umístit virtuální počítač.
6. Synchronizace dat doporučujeme vybrat možnost **synchronizovat data před převzetí**. To minimalizuje prostoje pro virtuální počítače jako synchronizuje bez jejich vypínání. Ji provede následující akce:

   * Fáze 1: Vytváří snímek virtuálního počítače v Azure a zkopíruje jej na hostitele Hyper-V na místě. Bude počítač dál běží v Azure.
   * Fáze 2: Vypne virtuální počítač v Azure tak, aby žádné nové změny dojít k dispozici. Poslední sadu změn jsou přeneseny na místním serveru a na místním virtuálním počítači spuštění.
7. Kliknutím na značku zaškrtnutí zahájíte převzetí služeb při selhání (navrácení služeb po obnovení).
8. Po dokončení počáteční synchronizace a jste připraveni vypnout virtuální počítač v Azure, klikněte na tlačítko **úlohy** > <planned failover job> > **dokončení převzetí služeb při selhání**. Tím se vypne počítač Azure, přenáší nejnovější změny na místním virtuálním počítači a spustí ho.
9. Můžete se přihlásit na místním virtuálním počítači a ověří, zda že vše funguje podle očekávání. Pak klikněte na tlačítko **potvrdit** ukončíte převzetí služeb při selhání.
10. Klikněte na tlačítko **zpětnou replikaci** ke spuštění ochrany na místním virtuálním počítači.

    > [!NOTE]
    > Pokud zrušíte úlohu navrácení služeb po obnovení, když je v kroku synchronizace dat, místní virtuální počítač bude v poškozeném stavu. Je to proto, že synchronizace dat zkopíruje nejnovější data z disků virtuálního počítače Azure do místní datové disky, a až po dokončení synchronizace, diskových dat nemusí být v konzistentním stavu. Pokud On místní virtuální počítač se spustí po synchronizaci dat se zruší, nemusí spustit. Znovu spustíte převzetí služeb při selhání na dokončení synchronizace dat.

## <a name="time-taken-to-failback"></a>Čas potřebný k navrácení služeb po obnovení
Čas potřebný k dokončení synchronizace dat a spuštění virtuálního počítače závisí na různých faktorech. Umožňuje lépe pochopit, doba vám vysvětlíme, co se stane, že během synchronizace dat.

Synchronizace dat pořídí snímek disky virtuálního počítače a spustí kontrola blok po bloku a vypočítá jeho kontrolního součtu. Toto počítané kontrolního součtu posílá místní k porovnání s místními kontrolního součtu stejné bloku. V případě, že kontrolní součty shodují, nebudou převedeny datového bloku. Pokud neodpovídá, se přenáší datového bloku místně. Tato doba přenosu závisí na dostupné šířky pásma. Rychlost kontrolního součtu je několik GB za minutu. 

Pro urychlení stahování dat, můžete nakonfigurovat agenta MARS používat více podprocesů, které parallalize stahování. Odkazovat [dokumentu zde](https://support.microsoft.com/en-us/help/3056159/how-to-manage-on-premises-to-azure-protection-network-bandwidth-usage) na tom, jak změnit stažení vláken v agentovi.


## <a name="next-steps"></a>Další kroky

Po dokončení navrácení služeb po obnovení úlohy **potvrdit** virtuálního počítače. Potvrzení odstraní virtuální počítač Azure a jeho disky a připraví virtuálního počítače měly by být znovu.

Po **potvrdit**, můžete spustit *zpětnou replikaci*. Tato akce spustí ochranu virtuálního počítače z místního zpět do Azure. Poznámka: Toto bude replikovat změny, pouze vzhledem k tomu, že virtuální počítač byl vypnut v Azure a odešle rozdílové změny jenom.
