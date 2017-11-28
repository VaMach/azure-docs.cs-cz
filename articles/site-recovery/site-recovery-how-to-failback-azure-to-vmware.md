---
title: "Postup zpět z Azure se nepodařilo VMware | Microsoft Docs"
description: "Po převzetí služeb při selhání virtuálních počítačů do Azure můžete zahájit navrácení služeb po obnovení a dovést tak virtuální počítače zpět do místní. Přečtěte si postup pro navrácení služeb po obnovení."
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
ms.date: 11/28/2017
ms.author: ruturajd
ms.openlocfilehash: ad424818f41e6b48e754dd0d39771248a1cd04fb
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="fail-back-from-azure-to-an-on-premises-site"></a>Selhání zpět z Azure do místního serveru

Tento článek popisuje, jak selhání virtuální počítače zpět z virtuálních počítačů Azure k místní lokalitě. Postupujte podle pokynů v tomto článku selhání zálohování vaše virtuální počítače VMware nebo Windows nebo Linuxem fyzických serverů po jste při selhání z místní lokality do Azure pomocí [VMware replikaci virtuálních počítačů a fyzických serverů do Azure s Azure Site Recovery](site-recovery-vmware-to-azure-classic.md) kurzu.

> [!WARNING]
> Až budete mít buď není možné navrácení služeb po obnovení [dokončit migraci](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), přesunout virtuální počítač do jiné skupiny prostředků nebo odstranit virtuální počítač Azure. Pokud zakážete ochrany virtuálního počítače, nemůžete navrácení služeb po obnovení.

> [!NOTE]
> Pokud jste při selhání virtuálních počítačů VMware nemůžete navrácení služeb po obnovení na hostitele Hyper-v.

## <a name="overview-of-failback"></a>Přehled o navrácení služeb po obnovení
Zde je, jak funguje navrácení služeb po obnovení. Po mít při selhání do Azure, můžete se nepodařilo zpět na místní lokalitu v několika fázích:

1. [Znovu nastavte ochranu](site-recovery-how-to-reprotect.md) virtuální počítače na Azure tak, aby jejich spuštění k replikaci pro virtuální počítače VMware ve vaší místní lokalitě. Jako součást tohoto procesu musíte také:
    1. Nastavit místní hlavní cílový: Windows hlavnímu cíli pro virtuální počítače s Windows a [hlavního cíle Linuxu](site-recovery-how-to-install-linux-master-target.md) pro virtuální počítače s Linuxem.
    2. Nastavení [procesový server](site-recovery-vmware-setup-azure-ps-resource-manager.md).
    3. Zahájit [znovu nastavte ochranu](site-recovery-how-to-reprotect.md). Se vypnout virtuální počítač na místě a synchronizaci dat Azure virtuálního počítače s místními disky.
5. Po virtuální počítače v Azure jsou replikující se do místního serveru, spustíte selhání přes z Azure do místní lokality.

Po vašich dat se nezdařila zpět, je znovu nastavte ochranu místní virtuální počítače, které při selhání zpátky do tak, aby jejich spuštění replikovat do Azure.

Pro rychlý přehled najdete v následujícím videu o tom, jak převzetí služeb při selhání z Azure na místní lokalitu.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]

### <a name="fail-back-to-the-original-or-alternate-location"></a>Navrácení služeb po obnovení do původního nebo alternativního umístění

Pokud jste při selhání virtuálního počítače VMware, můžete Pokud můžou selhat zpět do stejného zdrojového místní virtuální počítače stále existuje. V tomto scénáři jsou replikovány pouze změny zpět. Tento scénář se označuje jako obnovení do původního umístění. Pokud na místním virtuálním počítači neexistuje, tento scénář je obnovení do alternativního umístění.

> [!NOTE]
> Můžete pouze navrácení služeb po obnovení na původní server vCenter a konfigurace. Nelze nasadit novou konfigurační server a použití navrácení služeb po obnovení. Navíc nelze přidat nový vCenter k existujícímu serveru konfigurace a navrácení služeb po obnovení do nové vCenter.

#### <a name="original-location-recovery"></a>Obnovení do původního umístění

Pokud žádnou zpět do původního virtuálního počítače, vyžadují se následující podmínky:
* Pokud je virtuální počítač spravovaný vCenter server, hostitele ESX se hlavní cíl měli přístup k úložišti dat virtuálního počítače.
* Pokud virtuální počítač na hostiteli ESX, ale není spravován nástrojem vCenter, musí být pevný disk virtuálního počítače v úložiště dat, které se hlavní cíl hostitelé mají přístup.
* Pokud virtuální počítač na hostiteli ESX a nepoužívá vCenter, by měl předtím, než můžete znovu nastavte ochranu dokončit zjišťování hostitele ESX hlavního cíle. To platí, pokud po obnovení zpět fyzických serverů, příliš.
* Vám může selhat zpět k virtuální síti SAN (vSAN) nebo disk, který podle nezpracované zařízení mapování (RDM), pokud již existují a jsou připojené k virtuálnímu počítači místní disky.

#### <a name="alternate-location-recovery"></a>Obnovení do náhradního umístění
Pokud virtuální počítač na místě před opětovnou ochranu virtuálního počítače neexistuje, tento scénář nazývá obnovení do alternativního umístění. Pracovní postup opětovné ochrany znovu vytvoří na místním virtuálním počítači. Také to způsobí stahování úplná data.

* Pokud žádnou zpět do alternativního umístění, virtuální počítač se obnoví na stejného hostitele ESX, na kterém je nasazený hlavní cílový server. Úložiště dat, který slouží k vytvoření disku bude stejné úložiště, který byl vybrán při opětovné povolení ochrany virtuálního počítače.
* Může selhat zpět jenom na systém souborů virtuálního počítače (VMFS) nebo sítě vSAN úložiště. Pokud máte RDM, opětovné ochrany a navrácení služeb po obnovení nebude fungovat.
* Opětovné ochrany zahrnuje jeden velký počáteční přenos dat, za kterým následuje změny. Tento proces existuje, protože virtuální počítač na místní neexistuje. Kompletní datový musí replikovat zpět. Tato opětovné ochrany bude taky trvat déle než obnovení do původního umístění.
* Nelze nepodaří zpět na základě RDM disky. V úložišti dat VMFS nebo síť vSAN může vytvořit pouze nové disky virtuálního počítače (VMDKs).

Fyzický počítač, při převzetí služeb při selhání do Azure, můžete se nezdařilo zpět pouze jako virtuální počítač VMware (také označované jako P2A2V). Tento tok klesne pod alternativní umístění pro obnovení.

* Fyzický server Windows Server 2008 R2 SP1, pokud ochranu a převzetí služeb při selhání do Azure, nemůže se zpět.
* Ujistěte se, že zjistíte alespoň jeden hlavní cílový server a potřeby hostitelů ESX/ESXi, na které budete muset navrácení služeb po obnovení.

## <a name="have-you-completed-reprotection"></a>Dokončili jste vytvoření?
Než budete pokračovat, proveďte kroky opětovné ochrany tak, aby virtuální počítače jsou ve stavu, replikované a můžete zahájit převzetí služeb při selhání zpátky do místního webu. Další informace najdete v tématu [postupy k nastavení opětné z Azure do místní](site-recovery-how-to-reprotect.md).

## <a name="prerequisites"></a>Požadavky

> [!IMPORTANT]
> Během převzetí služeb při selhání do Azure, nemusí být dostupný na místní lokalitu a proto konfigurační server může být buď zrušení k dispozici nebo vypnutí. Během opětovné ochrany a navrácení služeb po obnovení by měla být na místním serveru configuration spuštěná a v připojeném stavu OK.

* Konfigurační server je vyžadováno místní uděláte navrácení služeb po obnovení. Server by měla být v běžícím stavu a připojení ke službě tak, že je jeho stav v pořádku. Během navrácení služeb po obnovení virtuální počítač, musí existovat v konfigurační databázi serveru, nebo navrácení služeb po obnovení nebude úspěšné. Proto zajistěte podniknout pravidelně naplánované zálohování serveru. Pokud dojde havárii, musíte se k obnovení serveru se stejnou adresou IP pro navrácení služeb po obnovení fungovat.
* Hlavní cílový server by neměl mít všechny snímky před spuštěním opětovné ochrany nebo navrácení služeb po obnovení.

## <a name="steps-to-fail-back"></a>Kroky pro navrácení služeb po obnovení

> [!IMPORTANT]
> Před spuštěním navrácení služeb po obnovení, ujistěte se, že jste dokončili vytvoření virtuálních počítačů. Virtuální počítače musí být v chráněném stavu, a jejich stavu by měl být **OK**. Chcete-li znovu nastavte ochranu virtuálních počítačů, přečtěte si [postupy k nastavení opětné](site-recovery-how-to-reprotect.md).

1. Na stránce replikované položky vyberte virtuální počítač a klikněte pravým tlačítkem ji vyberte **neplánované převzetí služeb při selhání**.
2. V **potvrzení převzetí služeb při selhání**, ověřte směr převzetí služeb při selhání (z Azure) a pak vyberte bod obnovení (nejnovější nebo konzistentní nejnovější aplikace), kterou chcete použít převzetí služeb při selhání. Bod konzistentní aplikace je za do nejnovějšího bodu v čase a způsobí ztrátu dat.
3. Během převzetí služeb při selhání Site Recovery vypne virtuální počítače na platformě Azure. Po můžete zkontrolovat, že tento navrácení služeb po obnovení je dokončená, podle očekávání, můžete zkontrolovat, že virtuální počítače na Azure byla vypnuta.

### <a name="to-what-recovery-point-can-i-fail-back-the-virtual-machines"></a>Jaké bodu obnovení můžete I navrácení služeb po obnovení virtuálních počítačů?

Během navrácení služeb po obnovení máte dvě možnosti pro navrácení služeb po obnovení virtuálního počítače nebo obnovení plánu.

Pokud vyberete nejnovější zpracované bodu v čase, všechny virtuální počítače převezme služby při selhání k jejich nejnovějšímu dostupnému bodu v čase. V případě, že je skupina replikace v rámci plánu obnovení, pak každý virtuální počítač z replikační skupiny se převzetí služeb při selhání jeho nezávislé nejnovějšího bodu v čase.

Virtuální počítač nelze označit zpět, dokud má alespoň jeden bod obnovení. Plán obnovení nelze označit zpět, dokud všechny virtuální počítače mít alespoň jeden bodu obnovení.

> [!NOTE]
> Nejnovější bod obnovení je bod obnovení konzistentní při selhání.

Pokud vyberete bod obnovení konzistentních s aplikací, bude navrácení jeden virtuální počítač obnovit do jeho nejnovější bod obnovení k dispozici konzistentních s aplikací. V případě plán obnovení s replikační skupiny bude každá skupina replikace obnovit do jeho společný bod obnovení.
Všimněte si, že bodů obnovení konzistentních s aplikací mohou být za v čase a může být ztráty dat.

### <a name="what-happens-to-vmware-tools-post-failback"></a>Co se stane VMware nástroje post navrácení služeb po obnovení?

Během převzetí služeb při selhání do Azure nelze používat nástroje VMware na virtuální počítač Azure. V případě virtuálního počítače s Windows zakáže automatické obnovení systému nástroje VMware během převzetí služeb při selhání. V případě virtuální počítač Linux automatické obnovení systému odinstaluje nástroje VMware během převzetí služeb při selhání.

Během navrácení služeb po obnovení virtuálního počítače s Windows jsou nástroje VMware znovu povolit po navrácení služeb po obnovení. Podobně pro virtuální počítač s linuxem nástroje VMware jsou přeinstalovány na počítači během navrácení služeb po obnovení.

## <a name="next-steps"></a>Další kroky

Po dokončení navrácení služeb po obnovení, budete muset potvrdit tento virtuální počítač k zajištění, že se odstraní obnovené virtuálních počítačů v Azure.

### <a name="commit"></a>Potvrzení
Potvrzení je vyžadován k odebrání neúspěšný přes virtuální počítač z Azure.
Klikněte pravým tlačítkem na chráněné položky a pak klikněte na **potvrdit**. Úlohu odebere neúspěšný přes virtuální počítače v Azure.

### <a name="reprotect-from-on-premises-to-azure"></a>Nastavte znovu z místní do Azure

Po dokončení potvrzení virtuálního počítače je zpět na místní lokality, ale nebudou chráněné. Pokud chcete spustit znovu replikovat do Azure, postupujte takto:

1. V **trezoru** > **nastavení** > **replikované položky**, vyberte virtuální počítače, které selhaly zpět a pak klikněte na tlačítko **znovu nastavit ochranu**.
2. Zadejte hodnotu procesového serveru, které se musí použít k odesílání dat zpět do Azure.
3. Klikněte na tlačítko **OK** k zahájení úloh opětovné ochrany.

> [!NOTE]
> Poté, co se místní virtuální počítač se spustí, trvá delší dobu pro agenta k registraci zpět na konfigurační server (až 15 minut). Během této doby opětovné ochrany se nezdaří a vrátí chybovou zprávu s oznámením, že agent není nainstalovaný. Počkejte několik minut a pak zkuste znovu opětovné ochrany.

Po dokončení úloh opětovné ochrany zpět do Azure se replikuje virtuální počítač, a můžete provést převzetí služeb při selhání.

## <a name="common-issues"></a>Běžné problémy
Ujistěte se, že se vCenter je v připojeném stavu před provedením navrácení služeb po obnovení. Odpojení disky a znovu je připojit zpět k virtuálnímu počítači, jinak nebude úspěšná.

### <a name="common-error-codes"></a>Běžné kódy chyb

#### <a name="error-code-8038"></a>Kód chyby 8038

*Nepovedlo se zprovoznit místní virtuální počítač z důvodu chyby*

To se stane, když 
1. Místní virtuální počítač je zapínají na hostitele, který nemá dostatek paměti zřízený.

Chcete-li vyřešit tento problém
1. Můžete zřídit další paměť na hostiteli ESXi.
2. řešení vMotion virtuálního počítače na jiného hostitele ESXi, který má dostatek paměti ke spuštění virtuálního počítače.

