---
title: "Převzetí služeb při selhání ve službě Site Recovery | Microsoft Docs"
description: "Azure Site Recovery koordinuje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Další informace o převzetí služeb při selhání do Azure nebo do sekundárního datacentra."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/25/2017
ms.author: pratshar
ms.openlocfilehash: 9c00cf88fa8b754c92cfd0f01be61a596d04d7c6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="failover-in-site-recovery"></a>Převzetí služeb při selhání v Site Recovery
Tento článek popisuje, jak k převzetí služeb při selhání virtuálního počítače a fyzické servery, které se chránit pomocí Site Recovery.

## <a name="prerequisites"></a>Požadavky
1. Před provedením převzetí služeb při selhání, proveďte [testovací převzetí služeb při selhání](site-recovery-test-failover-to-azure.md) zajistit, že vše funguje podle očekávání.
1. [Příprava sítě](site-recovery-network-design.md) v cílovém umístění, před provedením převzetí služeb při selhání.  

Následující tabulku použijte k vědět o možnosti převzetí služeb při selhání poskytuje Azure Site Recovery. Tyto možnosti jsou také uvedeny pro převzetí služeb při selhání různé scénáře.

| Scénář | Požadavek na obnovení aplikace | Pracovní postup pro Hyper-V | Pracovní postup pro VMware
|---|--|--|--|
|Plánované převzetí služeb při selhání kvůli výpadku nadcházející datacenter| Nulové ztrátě dat pro aplikaci, když probíhá plánované aktivity| Pro Hyper-V automatické obnovení systému replikuje data frekvencí kopie specifikovaných uživatelem. Plánované převzetí služeb při selhání se používá k přepsání četnost a replikaci poslední změny, než je zahájeno převzetí služeb při selhání. <br/> <br/> 1.    Plán údržby podle proces správy změn vaší firmy. <br/><br/> 2. upozorní uživatele na nadcházející výpadku. <br/><br/> 3. Aplikace uživatelsky orientovaný převést do režimu offline.<br/><br/>4 zahájit plánovaná převzetí služeb při selhání pomocí portálu pro automatické obnovení systému. Místní virtuální počítač je automaticky vypnutí.<br/><br/>Účinné používání únikem = 0 <br/><br/>Deník bodů obnovení je také součástí intervalem pro uchovávání dat pro uživatele, který chce použít starší bod obnovení. (24 hodin uchování pro Hyper-V).| Automatické obnovení systému pro VMware, replikuje data průběžně pomocí distribučního místa seznamu CRL. Převzetí služeb při selhání umožňuje uživateli možnost převzetí služeb při selhání na nejnovější data (včetně ukončení aplikace post)<br/><br/> 1. Plán údržby podle procesu správy změn <br/><br/>2. upozorní uživatele na nadcházející výpadek <br/><br/>3.  Aplikace uživatelsky orientovaný převést do režimu offline. <br/><br/>4.  Zahájení plánované převzetí služeb při selhání pomocí portálu automatické obnovení systému do nejnovějšího bodu po aplikace je offline. Pomocí možnosti "Neplánované převzetí služeb při selhání" na portálu a vyberte nejnovější bod pro převzetí služeb při selhání. Místní virtuální počítač je automaticky vypnutí.<br/><br/>Účinné používání únikem = 0 <br/><br/>Deník bodů obnovení v okně uchovávání se poskytuje pro zákazníka, který chce používat starší bod obnovení. (72 hodin doba uchování VMware).
|Převzetí služeb při selhání z důvodu datacenter neplánované výpadky (přirozený nebo po havárii IT) | Minimálními ztrátami dat pro aplikaci | 1 zahájit plán BCP organizace <br/><br/>2. Spustit neplánované převzetí služeb při selhání pomocí portálu automatické obnovení systému nejnovější nebo bod z okna uchování (deníku).| 1. Zahájit plán BCP organizace. <br/><br/>2.  Spustit neplánované převzetí služeb při selhání pomocí portálu automatické obnovení systému nejnovější nebo bod z okna uchování (deníku).


## <a name="run-a-failover"></a>Spuštění převzetí služeb při selhání
Tento postup popisuje, jak spustit převzetí služeb při selhání pro [plán obnovení](site-recovery-create-recovery-plans.md). Případně můžete spustit převzetí služeb při selhání pro jeden virtuální počítač nebo z fyzického serveru **replikované položky** stránky


![Převzetí služeb při selhání](./media/site-recovery-failover/Failover.png)

1. Vyberte **plány obnovení** > *recoveryplan_name*. Klikněte na tlačítko **převzetí služeb při selhání**
2. Na **převzetí služeb při selhání** obrazovku, vyberte **bod obnovení** k převzetí služeb při selhání. Můžete použít jednu z následujících možností:
    1.  **Nejnovější** (výchozí): tuto možnost spustí úlohu na první zpracování všech dat, které byl odeslán do služby Site Recovery. Zpracování dat vytvoří bod obnovení pro každý virtuální počítač. Tento bod obnovení se používá pro virtuální počítač během převzetí služeb při selhání. Tato možnost poskytuje nejnižší plánovaný bod obnovení (plánovaného bodu obnovení) jako virtuální počítač vytvořené po převzetí služeb při selhání se všechna data, která má nebyla replikována do služby Site Recovery, pokud bylo spuštěno převzetí služeb při selhání.
    1.  **Nejnovější zpracované**: tuto možnost převezme všechny virtuální počítače v plánu obnovení do nejnovějšího bodu obnovení, který již byl zpracován službou Site Recovery. Při provádění převzetí služeb při selhání virtuálního počítače, časové razítko posledního bodu obnovení zpracování je také zobrazit. Při provádění převzetí služeb při selhání plánu obnovení, můžete přejít na virtuální počítač zvlášť a podívejte se na **body obnovení nejnovější** dlaždicí a získejte tyto informace. Jak je žádný čas strávený zpracování nezpracovaných dat, tato možnost nabízí nízkou možnost převzetí služeb při selhání RTO (plánovanou dobu obnovení).
    1.  **Nejnovější aplikace konzistentní**: tuto možnost převezme všechny virtuální počítače v plánu obnovení do nejnovějšího bodu obnovení konzistentních s aplikací, který již byl zpracován službou Site Recovery. Při provádění převzetí služeb při selhání virtuálního počítače, je také zobrazit časové razítko bodu nejnovější obnovení konzistentních s aplikací. Při provádění převzetí služeb při selhání plánu obnovení, můžete přejít na virtuální počítač zvlášť a podívejte se na **body obnovení nejnovější** dlaždicí a získejte tyto informace.
    1.  **Nejnovější více virtuálních počítačů zpracovat**: Tato možnost je dostupná jenom pro plány obnovení, které mají alespoň jeden virtuální počítač s konzistencí pro víc virtuálních počítačů na. Virtuální počítače, které jsou součástí replikační skupiny převzetí nejnovější běžné konzistentní obnovení více virtuálních počítačů bodu. Ostatní virtuální počítače převzetí služeb při selhání jejich nejnovějším zpracovaných bodem obnovení.  
    1.  **Nejnovější více virtuálních počítačů konzistentní**: Tato možnost je dostupná jenom pro plány obnovení, které mají alespoň jeden virtuální počítač s ON konzistence více virtuálních počítačů. Virtuální počítače, které jsou součástí replikační skupiny převzetí nejnovější běžné konzistentní s aplikací obnovení více virtuálních počítačů bodu. Ostatní virtuální počítače převzetí služeb při selhání na jejich nejnovější bod obnovení konzistentních s aplikací.
    1.  **Vlastní**: Pokud byste testovací převzetí služeb při selhání virtuálního počítače, pak můžete použít tuto možnost, převzetí služeb při selhání do bodu konkrétní obnovení.

    > [!NOTE]
    > Vyberte bod obnovení možnost je dostupná jenom při selhání do Azure.
    >
    >


1. Pokud některé virtuální počítače v plánu obnovení byly převzetí služeb při selhání při předchozím spuštění a teď virtuální počítače nejsou aktivní na zdrojovém i cílovém umístění, můžete použít **změnit směr** možnost rozhodnout, směr, ve kterém musí dojít převzetí služeb při selhání.
1. Pokud jste selhání do Azure a je povolené šifrování dat pro cloud (platí jenom v případě, že uživatelé chrání virtuální počítače Hyper-v ze serveru VMM) v **šifrovací klíč** vyberte certifikát, který byl vydán, pokud povolíte šifrování dat během instalace na serveru VMM.
1. Vyberte **vypnout počítač před zahájením převzetí služeb při selhání** Pokud chcete, aby Site Recovery se pokusit o proveďte vypnutí zdrojových virtuálních počítačích než převzetí služeb při selhání. Převzetí služeb při selhání pokračovat i v případě, že vypnutí selže.  

    > [!NOTE]
    > Pokud chráněné virtuální počítače Hyper-v, pokusí se také možnost vypnutí synchronizovat místní data, která nebyla dosud odeslána do služby před spuštěním převzetí služeb při selhání.
    >
    >

1. Můžete sledovat průběh převzetí služeb při selhání **úlohy** stránky. I v případě, že během neplánované převzetí služeb při selhání dojít k chybám, plán obnovení běží, dokud se nedokončí.
1. Po převzetí služeb při selhání ověřte tak, že přihlášení k ní virtuální počítač. Pokud chcete přejít jiný bod obnovení pro virtuální počítač, pak můžete použít **změnit bod obnovení** možnost.
1. Jakmile budete spokojeni s neúspěšný přes virtuální počítač, můžete **potvrdit** převzetí služeb při selhání. Potvrzení odstraní všechny body obnovení k dispozici ve službě a **změnit bod obnovení** možnost nadále již nebudou dostupné.

## <a name="planned-failover"></a>Plánované převzetí služeb při selhání
Virtuální počítače/fyzické servery, které jsou chráněné pomocí Site Recovery také podpora **plánované převzetí služeb při selhání**. Plánované převzetí služeb při selhání je nulové data ztrátě převzetí služeb při selhání možnost. Když se aktivuje plánované převzetí služeb při selhání, nejprve vypnout zdrojový virtuální počítač, synchronizaci dat ještě k synchronizaci a pak se aktivuje převzetí služeb při selhání.

> [!NOTE]
> Pokud jste převzetí služeb při selhání technologie Hyper-v virtuální počítače z jednoho místní lokality do jiné lokality v místě, vraťte zpět na primární místního serveru máte první **zpětně replikovat** virtuální počítač zpět do primární lokality a potom aktivovat převzetí služeb při selhání. Pokud se primární virtuální počítač není k dispozici, pak před spuštěním na **zpětně replikovat** máte virtuální počítač obnovit ze zálohy.   
>
>

## <a name="failover-job"></a>Převzetí služeb při selhání úlohy

![Převzetí služeb při selhání](./media/site-recovery-failover/FailoverJob.png)

Když se aktivuje převzetí služeb při selhání, zahrnuje následující kroky:

1. Kontrola předpokladů: Tento krok zajistí, že jsou splněny všechny podmínky požadované pro převzetí služeb při selhání
1. Převzetí služeb při selhání: Tento krok zpracovává data a umožňuje připraven, aby virtuální počítač Azure můžete vytvořit mimo ho. Pokud jste vybrali **nejnovější** bodu obnovení, tento krok vytvoří bod obnovení z dat, který byl odeslán do služby.
1. Začátek: Tento krok vytvoří virtuální počítač Azure pomocí dat, která zpracuje v předchozím kroku.

> [!WARNING]
> **Nemáte zrušit v průběhu převzetí služeb při selhání**: před zahájením převzetí služeb při selhání replikace pro virtuální počítač je zastavena. Pokud jste **zrušit** v průběhu úlohy zastaví převzetí služeb při selhání, ale virtuální počítač nespustí replikaci. Replikace se nedá spustit znovu.
>
>

## <a name="time-taken-for-failover-to-azure"></a>Čas potřebný pro převzetí služeb při selhání do Azure

Převzetí služeb při selhání virtuálních počítačů v určitých případech vyžaduje velmi přechodný krok, který obvykle trvá přibližně 8 až 10 minut na dokončení. Čas potřebný k převzetí služeb při selhání bude vyšší než obvykle v následujících případech:

* Virtuální počítače VMware pomocí služby mobility verze starší než 9.8
* Fyzické servery 
* Virtuální počítače s Linuxem VMware
* Technologie Hyper-V virtuální počítače chráněné jako fyzických serverů
* Virtuální počítače VMware, kde nejsou zadány jako spouštěcí ovladače následující ovladače 
    * miniport storvsc 
    * VMBus 
    * storflt 
    * Intelide 
    * ATAPI
* Virtuální počítače VMware, které nemají služba DHCP povolena bez ohledu na to, jestli jsou pomocí protokolu DHCP nebo statické IP adresy

Ve všech ostatních případech tento zprostředkující krok není povinný a čas potřebný převzetí služeb při selhání je výrazně nižší. 





## <a name="using-scripts-in-failover"></a>Pomocí skriptů v převzetí služeb při selhání
Můžete automatizovat některé akce přitom převzetí služeb při selhání. Můžete použít skripty nebo [runbooky služby Azure automation](site-recovery-runbook-automation.md) v [plány obnovení](site-recovery-create-recovery-plans.md) to provést.

## <a name="other-considerations"></a>Další důležité informace
* **Písmeno jednotky** – Chcete-li zachovat písmeno na virtuální počítače po převzetí služeb při selhání můžete nastavit **zásada SAN** pro virtuální počítač **OnlineAll**. [Přečtěte si další informace](https://support.microsoft.com/en-us/help/3031135/how-to-preserve-the-drive-letter-for-protected-virtual-machines-that-are-failed-over-or-migrated-to-azure).



## <a name="next-steps"></a>Další kroky

> [!WARNING]
> Jakmile budete mít převzetí služeb při selhání virtuálního počítače a místního datového centra je k dispozici, měli byste [ **znovu nastavte ochranu** ](site-recovery-how-to-reprotect.md) virtuálních počítačů VMware zpět do místního datového centra.

Použití [ **plánované převzetí služeb při selhání** ](site-recovery-failback-from-azure-to-hyper-v.md) možnost k **navrácení služeb po obnovení** technologie Hyper-v virtuální počítače zpět do místní z Azure.

Pokud se nezdařilo přes Hyper-v virtuální počítač do jiného místního datového centra spravované serverem VMM a je k dispozici primární datové centrum, potom použijte **zpětná replikace** možnost spustit replikaci zpět na primární datové centrum.
