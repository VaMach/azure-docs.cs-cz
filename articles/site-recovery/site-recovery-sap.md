---
title: "Ochrana nasazení vícevrstvé SAP NetWeaver aplikace pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak k ochraně nasazení SAP NetWeaver aplikace pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: manayar
ms.openlocfilehash: 5a47acab598e113ef7ed968dd3a6429ac3bc1ec3
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="protect-a-multi-tier-sap-netweaver-application-deployment-using-azure-site-recovery"></a>Ochrana nasazení vícevrstvé SAP NetWeaver aplikace pomocí Azure Site Recovery

Většina nasazení SAP velké a střední má určitou formu řešení zotavení po havárii.  Význam robustní a možností intenzivního testování řešení zotavení po havárii zvýšilo další základní obchodní procesy přesunu do aplikace, jako je SAP.  Azure Site Recovery byl otestovaný a integrované se službou aplikace SAP, překračuje možnosti většina řešení zotavení po havárii v místě, na nižší celkové náklady na vlastnictví (TCO) než konkurenční řešení.
S Azure Site Recovery můžete:
* Povolení ochrany místně spuštěných produkčních aplikací SAP NetWeaver a jiných než NetWeaver pomocí replikace komponent do Azure
* Povolení ochrany v Azure spuštěných produkčních aplikací SAP NetWeaver a jiných než NetWeaver pomocí replikace komponent do jiného datového centra Azure
* Zjednodušení migrace na cloud pomocí Site Recovery migrací nasazení SAP do Azure
* Zjednodušení upgradů, testování a vytváření prototypů projektů SAP pomocí vytváření produkčního klonu na vyžádání pro účely testování aplikací SAP.

Tento článek popisuje, jak k ochraně nasazení SAP NetWeaver aplikací pomocí [Azure Site Recovery](site-recovery-overview.md). Tento článek popisuje osvědčené postupy pro ochranu replikace do jiné datové centrum Azure pomocí Azure Site Recovery, podporované scénáře a konfigurace a jak provádět převzetí služeb při selhání, jak testovací nasazení SAP NetWeaver třívrstvá v Azure převzetí služeb při selhání (podrobněji obnovení po havárii) a skutečné převzetí služeb při selhání.


## <a name="prerequisites"></a>Požadavky
Než začnete, ujistěte se, že rozumíte následující:

1. [Replikaci virtuálního počítače do Azure](azure-to-azure-walkthrough-enable-replication.md)
2. Postup [návrh k síti pro obnovení](site-recovery-azure-to-azure-networking-guidance.md)
3. [Provádění převzetí služeb při selhání do Azure](azure-to-azure-walkthrough-test-failover.md)
4. [Provádění převzetí služeb při selhání do Azure](site-recovery-failover.md)
5. Postup [replikace řadiče domény](site-recovery-active-directory.md)
6. Postup [replikaci systému SQL Server](site-recovery-sql.md)

## <a name="supported-scenarios"></a>Podporované scénáře
S Azure Site Recovery můžete implementovat řešení zotavení po havárii pro následující scénáře:
* SAP systémy s operačním systémem v jedné datové centrum Azure replikovat na jiné datové centrum Azure (Azure do Azure DR), jak je navržen [zde](https://aka.ms/asr-a2a-architecture).
* SAP systémy s operačním systémem místními VMWare (nebo fyzický) servery replikující se do zotavení po Havárii lokality v datovém centru Azure (VMware do Azure DR), který vyžaduje další součásti, jako je navržen [zde](https://aka.ms/asr-v2a-architecture).
* SAP systémy s operačním systémem místními technologie Hyper-V replikovat zotavení po Havárii lokality v datovém centru Azure (Hyper-V-do Azure DR), který vyžaduje další součásti, jako je navržen [zde](https://aka.ms/asr-h2a-architecture).

Tento dokument používá prvním případě - Azure-Azure zotavení po Havárii – k předvedení funkcí pro obnovení po havárii SAP Azure Site Recovery. Replikace Azure Site Recovery je nezávislá na aplikace, proces popsaný by měl obsahovat také další scénáře.

### <a name="required-foundation-services"></a>Požadované foundation services
Tento scénář dokumentace všechny byl nasazen s následujícími službami foundation nasazení:
* ExpressRoute nebo Site-to-Site virtuální privátní síť (VPN)
* Alespoň jeden řadič domény služby Active Directory a DNS serveru běží v Azure

Je doporučeno, je před nasazením Azure Site Recovery navázáno infrastruktury výše.


## <a name="typical-sap-application-deployment"></a>Typické nasazení aplikace SAP
Velké SAP zákazníci obvykle nasazovat mezi 6 až 20 jednotlivých aplikací SAP.  Většina těchto aplikací jsou založeny na moduly SAP NetWeaver ABAP nebo Java.  Podpora tyto základní NetWeaver aplikace jsou mnoho modulů menší samostatné konkrétní jiný - NetWeaver SAP a obvykle některé aplikace bez SAP.  

Je důležité pro všechny aplikace SAP, spuštěný v na šířku a k určení režimu nasazení (vrstvy 2 nebo 3 úrovně), verze, opravy, velikosti, změn rychlostí a požadavky na disku trvalost inventáře.

![Vzor nasazení](./media/site-recovery-sap/sap-typical-deployment.png)

Vrstvu trvalosti SAP databáze by měly být chráněné pomocí nativních nástrojů databázového systému, jako je například SQL Server AlwaysOn, Oracle DataGuard nebo HANA systému replikace. Vrstva klienta není také chráněný službou Azure Site Recovery, ale je důležité vzít v úvahu témata, které mají vliv tato vrstva například zpoždění šíření záznamů DNS, zabezpečení a vzdáleného přístupu do datového centra zotavení po Havárii.

Azure Site Recovery je doporučené řešení pro aplikační vrstvu, včetně (A) SCS. Celkové prostředí nasazení SAP součástí jiné aplikace, jako je například aplikace NetWeaver SAP a bez SAP aplikace a také by měly být chráněné službou Azure Site Recovery.

## <a name="replicate-virtual-machines"></a>Replikace virtuálních počítačů
Postupujte podle [v tomto návodu](azure-to-azure-walkthrough-enable-replication.md) k zahájení replikace všech SAP aplikace virtuálních počítačů do datového centra Azure zotavení po Havárii.

Pokud používáte statickou IP adresu, můžete zadat IP, který chcete virtuální počítač v části síťové rozhraní karty v nastavení výpočtů a sítě trvat.

![Cílové IP](./media/site-recovery-sap/sap-static-ip.png)


## <a name="creating-a-recovery-plan"></a>Vytvoření plánu obnovení
Plán obnovení umožňuje pořadí převzetí služeb při selhání v různých vrstvách vícevrstvé aplikace, proto zachování konzistence aplikace. Postupujte podle kroků popsaných [sem](site-recovery-create-recovery-plans.md) při vytváření plánu obnovení vícevrstvých webových aplikací.

### <a name="adding-scripts-to-the-recovery-plan"></a>Probíhá přidávání skriptů do plánu obnovení
Musíte udělat některé operace na virtuálních počítačích Azure post převzetí služeb při selhání a testovací převzetí služeb při selhání pro vaše aplikace fungovat správně. Je možné automatizovat převzetí služeb při selhání operaci post například aktualizaci položky DNS a změna vazby a připojení, přidáním příslušných skriptů v plánu obnovení, jak je popsáno v [v tomto článku](site-recovery-create-recovery-plans.md#add-scripts).

### <a name="dns-update"></a>Aktualizace DNS.
Pokud DNS je nakonfigurován pro dynamické aktualizace DNS, pak po spuštění virtuálních počítačů obvykle aktualizovat DNS s novou IP Adresou. Pokud chcete přidat explicitní kroku aktualizovat DNS s nové IP adresy virtuálních počítačů a pak přidejte tuto [skript pro aktualizaci IP ve službě DNS](https://aka.ms/asr-dns-update) jako akce post na skupiny plánu obnovení.  

## <a name="example-azure-to-azure-deployment"></a>Příklad nasazení Azure do Azure
Scénář je znázorněn na obrázku níže DR Azure do Azure Azure Site Recovery:
* Primární datové centrum se Singapur (Azure jihovýchodní Asie) a zotavení po Havárii datacenter je Hongkong (Azure východní Asie).  V tomto scénáři se tak, že dva virtuální počítače se systémem SQL Server AlwaysOn v synchronním režimu v Singapur poskytuje místní vysokou dostupnost.
* ASC sdílené složky souborů slouží k poskytování HA pro SAP jediné body selhání. ASC sdílené složky souboru nevyžaduje sdíleného disku clusteru a aplikace, jako je SIOS nejsou požadovány.
* Ochrany DR pro vrstvu databázového systému je dosaženo pomocí asynchronní replikaci.
* Tento scénář popisuje "symetrický DR" – termín, který používá k popisu řešení zotavení po Havárii, které je repliku přesný provozních, proto řešení zotavení po Havárii SQL Server má místní vysokou dostupnost. Použití symetrický zotavení po Havárii není povinné pro vrstvu databáze a mnoho zákazníků využít flexibilitu nasazení cloudu rychle vytvářet místní vysoké dostupnosti uzlu po události zotavení po Havárii.
* Diagram znázorňuje vrstvy serveru SAP NetWeaver ASC a aplikaci replikovat pomocí Azure Site Recovery.

![Scénář replikace](./media/site-recovery-sap/sap-replication-scenario.png)

## <a name="doing-a-test-failover"></a>Provádění testovací převzetí služeb
Postupujte podle [v tomto návodu](azure-to-azure-walkthrough-test-failover.md) provedete testovací převzetí služeb.

1.  Přejděte na portál Azure a vyberte svůj trezor služeb zotavení.
2.  Klikněte na plán obnovení, které jsou vytvořené pro aplikace SAP (s).
3.  Klikněte na "Testovací převzetí služeb".
4.  Vyberte bod obnovení a virtuální síť Azure ke spuštění procesu testovací převzetí služeb při selhání.
5.  Jakmile sekundární prostředí zapnutý, můžete provést vaše ověření.
6.  Po dokončení se k ověření, klikněte na 'vyčistit testovací převzetí služeb při selhání a pro vyčištění prostředí převzetí služeb při selhání.

## <a name="doing-a-failover"></a>Převzetím služeb
Postupujte podle [v tomto návodu](site-recovery-failover.md) při dělají převzetí služeb při selhání.

1.  Přejděte na portál Azure a vyberte svůj trezor služeb zotavení.
2.  Kliknutím na vytvořit pro aplikace SAP plánu obnovení.
3.  Klikněte na 'Převzetí služeb při selhání'.
4.  Vyberte bod obnovení se spustit proces převzetí služeb při selhání.

## <a name="next-steps"></a>Další kroky
Další informace o vytváření řešení zotavení po havárii pro nasazení SAP NetWeaver pomocí Azure Site Recovery v [tento dokument White Paper](http://aka.ms/asr-sap). V dokumentu White Paper také popisuje doporučení pro různé architektury SAP, jsou uvedené podporované aplikací a typů virtuálních počítačů pro SAP v Azure a popisuje možné testování plány pro vaše řešení pro zotavení po havárii.

Další informace o [replikace dalších zatížení](site-recovery-workload.md) pomocí Site Recovery.
