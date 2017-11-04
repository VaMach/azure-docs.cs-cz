---
title: "Převzetí služeb při selhání a selhání zálohování virtuálních počítačů Hyper-V replikovat do sekundárního datového centra pomocí Site Recovery | Microsoft Docs"
description: "Zjistěte, jak převzetí služeb při selhání Hyper-V virtuální počítače na sekundární místní lokalitu a navrácení služeb po obnovení primární lokality, službou Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 44a662fa-2e7a-4996-86df-fdd6d6f5dedf
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/16/2017
ms.author: raynew
ms.openlocfilehash: 8f139070de99c4249207d048d445e86dd41e9060
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-your-secondary-on-premises-site"></a>Převzetí služeb při selhání a selhání zálohování virtuálních počítačů Hyper-V replikovat na sekundární místní lokalitu

[Azure Site Recovery](site-recovery-overview.md) služby spravuje a orchestruje replikaci, převzetí služeb při selhání a navrácení služeb po obnovení místního počítače a virtuální počítače Azure (VM).

Tento kurz popisuje, jak k převzetí služeb virtuálního počítače technologie Hyper-V spravované v cloudu System Center Virtual Machine Manager (VMM), pro sekundární lokalita VMM. Poté, co jste při selhání, můžete zpět do místního serveru nezdaří případě, že je k dispozici. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Selhání virtuálního počítače technologie Hyper-V z primárního cloudu VMM do sekundární cloudu VMM
> * Znovu nastavte ochranu ze sekundární lokality na primární a navrácení služeb po obnovení
> * Volitelně můžete spustit replikaci z primárního na sekundární znovu

## <a name="overview"></a>Přehled

Převzetí služeb při selhání a navrácení služeb po obnovení má tři fáze:

1. **Převzetí služeb při selhání sekundární lokality**: převzetí služeb při selhání počítače z primární lokality, sekundární.
2. **Po obnovení vrátit sekundární lokality**: replikace virtuálních počítačů z sekundární pro primární a spusťte plánované převzetí služeb při selhání pro navrácení služeb po obnovení.
3. Po plánovaném převzetí služeb při selhání můžete spusťte, replikace z primární lokality do sekundární znovu.


## <a name="fail-over-to-a-secondary-site"></a>Převzetí služeb při selhání do sekundární lokality

### <a name="failover-prerequisites"></a>Požadavky převzetí služeb při selhání

Ujistěte se, když jste dokončili [postupu zotavení po havárii](tutorial-dr-drill-secondary.md) zkontrolujte, zda vše funguje podle očekávání.


### <a name="run-a-failover-from-primary-to-secondary"></a>Spustit převzetí služeb při selhání z primárního na sekundární

Pro virtuální počítače Hyper-V můžete spustit standardním nebo plánované převzetí služeb při selhání.

- Použijte regulární převzetí služeb při selhání pro neočekávaných výpadků. Spustíte-li toto převzetí služeb při selhání, Site Recovery vytvoří virtuální počítač v sekundární lokalitě a zajišťuje nahoru. Můžete spustit převzetí služeb při selhání pro konkrétní bod obnovení. Může dojít ke ztrátě dat v závislosti na bod obnovení, které používáte.
- Plánované převzetí služeb při selhání můžete použít, údržbě nebo při očekávaný výpadek. Tato možnost poskytuje nulovou ztrátou data. Když se aktivuje plánované převzetí služeb při selhání, jsou zdrojové virtuální počítače vypnout. Nesynchronizovaná data synchronizována a aktivaci převzetí služeb při selhání. 
- 
Tento postup popisuje, jak spustit regulární převzetí služeb při selhání.


1. V **nastavení** > **replikované položky** klikněte na virtuální počítač > **převzetí služeb při selhání**.
2. V **převzetí služeb při selhání** vyberte **bod obnovení** k převzetí služeb při selhání. Můžete použít jednu z následujících možností:
    - **Nejnovější** (výchozí): tuto možnost napřed zpracuje všechna data přenášená k Site Recovery. Poskytuje nejnižší plánovaný bod obnovení (plánovaného bodu obnovení), protože virtuální počítač repliky vytvořené po převzetí služeb při selhání se všechna data, která se replikují do Site Recovery, když bylo spuštěno převzetí služeb při selhání.
    - **Nejnovější zpracované**: tuto možnost převezme virtuálního počítače do nejnovějšího bodu obnovení, které jsou zpracovávány Site Recovery. Tato možnost poskytuje nízkou RTO (plánovanou dobu obnovení), protože je žádný čas strávený zpracováváním nezpracovaná data.
    - **Nejnovější aplikace konzistentní**: tuto možnost převezme virtuálního počítače do bodu nejnovější konzistentní obnovení zpracovaných službou Site Recovery. 
3. Šifrovací klíč není relevantní v tomto scénáři.
4. Vyberte **vypnout počítač před zahájením převzetí služeb při selhání** Pokud chcete, aby Site Recovery se pokusit o proveďte vypnutí zdrojové virtuální počítače před spuštěním převzetí služeb při selhání. Site Recovery se také pokusí synchronizovat místní data, která nebyla dosud odeslána do sekundární lokality, než převzetí služeb při selhání. Všimněte si, že převzetí služeb při selhání pokračovat i v případě, že vypnutí selže. Můžete sledovat průběh převzetí služeb při selhání **úlohy** stránky.
5. Teď by měla být moci zobrazit virtuální počítač v sekundární cloudu VMM.
6. Po ověření, virtuální počítač, **potvrdit** převzetí služeb při selhání. Tím se odstraní všechny body obnovení k dispozici.

> [!WARNING]
> **Nemáte zrušení převzetí služeb při selhání v průběhu**: před zahájením převzetí služeb při selhání replikace virtuálního počítače je zastavena. Pokud zrušíte převzetí služeb při selhání v průběhu, zastaví převzetí služeb při selhání, ale nebude znovu replikaci virtuálního počítače.  


## <a name="reprotect-and-fail-back-from-secondary-to-primary"></a>Znovu nastavte ochranu a obnovení vrátit sekundární pro primární

### <a name="prerequisites-for-failback"></a>Předpoklady pro navrácení služeb po obnovení

Dokončit navrácení služeb po obnovení, ujistěte se, že primární a sekundární servery VMM připojeni k Site Recovery.


### <a name="reprotect-and-fail-back"></a>Znovu nastavte ochranu a navrácení služeb po obnovení

Zahájení replikace ze sekundární lokality na primární a selhání zpátky do primární lokality. Po virtuální počítače jsou spuštěné v primární lokalitě znovu, můžete replikovat je do sekundární lokality znovu.  

1. V **nastavení** > **replikované položky** klikněte na virtuální počítač a povolte **zpětnou replikaci**. Virtuální počítač začne replikovat zpět do primární lokality.
2. Klikněte na virtuální počítač > **plánované převzetí služeb při selhání**.
3. V **potvrďte plánované převzetí služeb při selhání**ověřte směr převzetí služeb při selhání (z sekundární cloudu VMM) a vyberte zdrojové a cílové umístění. 
4. V **synchronizace dat**, zadejte, jak chcete synchronizovat:
    - **Synchronizace dat před převzetí služeb při selhání (synchronizovat jenom rozdílové změny)**– tato možnost minimalizuje výpadek virtuálních počítačů, protože synchronizuje bez vypnutí virtuálního počítače. Tady je co umožňuje:
        - Pořídí snímek virtuálního počítače repliky a zkopíruje ho primárním hostitelem technologie Hyper-V. Replika virtuálních počítačů neustále běží.
        - Vypne repliku virtuálního počítače, tak, aby žádné nové změny dojít k dispozici. Závěrečné sady rozdílové změny přenesou do primární lokality a spuštění virtuálního počítače v primární lokalitě.
    - **Synchronizace dat během pouze převzetí služeb při selhání (úplná ke stažení)**– tuto možnost použijte, pokud jste si už běží v sekundární lokalitě dlouhou dobu. Tato možnost je rychlejší, protože očekávané více změny na disku a nebudete věnovat času kontrolního součtu. Tato možnost provede stahování disku. Je také užitečné při primární virtuální počítač byl odstraněn.
5. Šifrovací klíč není relevantní v tomto scénáři.
6. Zahájit převzetí služeb při selhání. Můžete sledovat průběh převzetí služeb při selhání **úlohy** kartě.
7. Pokud jste vybrali k synchronizaci dat před převzetí služeb při selhání, po synchronizaci počáteční data se provádí a jste připraveni vypnout repliku virtuálního počítače v sekundární lokalitě, klikněte na tlačítko **úlohy** > název úlohy plánované převzetí služeb při selhání >  **Dokončete převzetí služeb při selhání**. Tato sekundární virtuální počítač vypne, přenáší nejnovější změny do primární lokality a spustí primárního virtuálního počítače.
8. V primárním cloudu VMM zkontrolujte, zda virtuální počítač je k dispozici.
9. Primární virtuální počítač je nyní ve stavu čekající potvrzení. Klikněte na tlačítko **potvrzení**, potvrzení převzetí služeb při selhání.
10. Pokud chcete, aby zahájení replikace primárního virtuálního počítače zpátky do sekundární lokality znovu povolte **zpětnou replikaci**.


> [!NOTE]
> Zpětná replikace replikuje pouze změny, které nastaly od repliky virtuálních počítačů se vypnulo a posílají se jenom rozdílové změny.

