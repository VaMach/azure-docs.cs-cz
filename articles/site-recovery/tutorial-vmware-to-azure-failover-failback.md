---
title: "Převzetí služeb při selhání a selhání zálohovat virtuální počítače VMware a fyzické servery, které se replikují do Azure pomocí Site Recovery | Microsoft Docs"
description: "Naučte se vrátí zpátky na místní lokalitu s Azure Site Recovery a selhání virtuální počítače VMware a fyzické servery do Azure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 28a14a9b28dfe9c2014add9b9f691bce6ba91a4c
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="fail-over-and-fail-back-vmware-vms-and-physical-servers-replicated-to-azure"></a>Převzetí služeb při selhání a selhání zálohovat virtuální počítače VMware a fyzické servery replikovat do Azure

Tento kurz popisuje, jak k převzetí služeb virtuálního počítače VMware do Azure. Poté, co jste při selhání, můžete zpět do místního serveru nezdaří případě, že je k dispozici. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Ověřte, zda vlastnosti virtuálního počítače VMware a zjistěte, v souladu s požadavky na Azure
> * Spuštění převzetí služeb při selhání do Azure
> * Vytvořit procesový server a hlavní cílový server navrácení služeb po obnovení
> * Znovu nastavte ochranu virtuálních počítačů Azure k místní lokalitě
> * Převzetí služeb při selhání z Azure na místě
> * Znovu nastavte ochranu místní virtuální počítače, spusťte znovu replikaci do Azure

Toto je pátý kurz v řadě. Tento kurz předpokládá, že jste už dokončili úkoly v předchozí kurzy.

1. [Příprava Azure](tutorial-prepare-azure.md)
2. [Příprava VMware v místním prostředí](tutorial-prepare-on-premises-vmware.md)
3. [Nastavení zotavení po havárii](tutorial-vmware-to-azure.md)
4. [Spuštění postupu zotavení po havárii](tutorial-dr-drill-azure.md)

## <a name="preparing-for-failover-and-failback"></a>Příprava na převzetí služeb při selhání a navrácení služeb po obnovení

Ujistěte se, že ve virtuálním počítači nejsou žádné snímky. Místní virtuální počítač je vypnutý během vytvoření.
To pomáhá zajistit konzistenci dat během replikace. Nikdy nezapínat na virtuálním počítači po dokončení vytvoření. Použijte stejný hlavní cílový server k nastavení opětné replikační skupiny. Pokud používáte jiný hlavní cílový server k nastavení opětné skupiny replikace, nemůže server poskytovat společný bod v čase.

Převzetí služeb při selhání a navrácení služeb po obnovení má čtyři fáze:

1. **Převzetí služeb při selhání do Azure**: převzetí služeb při selhání počítačů z místní lokality do Azure.
2. **Znovu nastavte ochranu virtuálních počítačů Azure**: znovu nastavte ochranu virtuálních počítačů Azure, tak, aby jejich spuštění replikovat virtuální počítače VMware na místě.
3. **Převzetí služeb při selhání místně**: spuštění převzetí služeb při selhání zpátky do Azure.
4. **Opětovné ochrany místní virtuální počítače**: po dat se nezdařila zpět, znovu aktivujte ochranu místní virtuální počítače, které při selhání zpátky do tak, aby jejich spuštění replikaci do Azure.

## <a name="verify-vm-properties"></a>Ověřte vlastnosti virtuálního počítače

Ověřte vlastnosti virtuálního počítače a ujistěte se, že virtuální počítač splňuje [požadavky pro Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. V **chráněné položky**, klikněte na tlačítko **replikované položky** > virtuálních počítačů.

2. V **replikované položky** podokně je souhrn informace o virtuálních počítačů, stav, a nejnovější dostupný bod obnovení. Klikněte na tlačítko **vlastnosti** zobrazíte další podrobnosti.

3. V **výpočty a síť**, můžete upravit název Azure, skupinu prostředků, cílovou velikost [skupinu dostupnosti](../virtual-machines/windows/tutorial-availability-sets.md), a [spravovat nastavení disku](#managed-disk-considerations)

4. Můžete zobrazit a upravit nastavení sítě, včetně sítě a podsítě, ve kterém virtuální počítač Azure budou umístěné po převzetí služeb při selhání a IP adresu, která bude přiřazena k němu.

5. V **disky**, zobrazí se informace o operačním systému a datové disky na virtuálním počítači.

## <a name="run-a-failover-to-azure"></a>Spuštění převzetí služeb při selhání do Azure

1. V **nastavení** > **replikované položky** klikněte na virtuální počítač > **převzetí služeb při selhání**.

2. V **převzetí služeb při selhání** vyberte **bod obnovení** k převzetí služeb při selhání. Můžete použít jednu z následujících možností:
   - **Nejnovější** (výchozí): tuto možnost napřed zpracuje všechna data přenášená k Site Recovery. Poskytuje nejnižší plánovaný bod obnovení (plánovaného bodu obnovení), protože virtuální počítač Azure vytvořené po převzetí služeb při selhání se všechna data, která se replikují do Site Recovery, když bylo spuštěno převzetí služeb při selhání.
   - **Nejnovější zpracované**: tuto možnost převezme virtuálního počítače do nejnovějšího bodu obnovení, které jsou zpracovávány Site Recovery. Tato možnost poskytuje nízkou RTO (plánovanou dobu obnovení), protože je žádný čas strávený zpracováváním nezpracovaná data.
   - **Nejnovější aplikace konzistentní**: tuto možnost převezme virtuálního počítače do bodu nejnovější konzistentní obnovení zpracovaných službou Site Recovery.
   - **Vlastní**: Zadejte bod obnovení.

3. Vyberte **vypnout počítač před zahájením převzetí služeb při selhání** pokusí provést vypnutí zdrojových virtuálních počítačích než převzetí služeb při selhání. Převzetí služeb při selhání pokračovat i v případě, že vypnutí selže. Můžete sledovat průběh převzetí služeb při selhání **úlohy** stránky.

4. Pokud jste připravili pro připojení k virtuálnímu počítači Azure, připojte k ověření po převzetí služeb při selhání.

5. Po ověření, **potvrdit** převzetí služeb při selhání. Tím se odstraní všechny body obnovení k dispozici.

> [!WARNING]
> **Nemáte zrušení převzetí služeb při selhání v průběhu**: před zahájením převzetí služeb při selhání replikace virtuálního počítače je zastavena.
> Pokud zrušíte převzetí služeb při selhání v průběhu, zastaví převzetí služeb při selhání, ale nebude znovu replikaci virtuálního počítače.

V některých scénářích převzetí služeb při selhání vyžaduje další zpracování, který trvá přibližně osm až deset minut na dokončení. Možná jste si všimli již testovací převzetí služeb při selhání dobu fyzických serverů, počítačů, virtuální počítače VMware, které nemají povoluje služby DHCP a virtuální počítače VMware, který není k dispozici následující ovladače pro spouštění VMware Linux: miniport storvsc, vmbus, storflt, intelide, atapi.

## <a name="create-a-process-server-in-azure"></a>Vytvořte procesový server v Azure

Procesový server přijímá data z virtuálního počítače Azure a odešle ji do místního webu. Síť s nízkou latencí je vyžadován mezi procesovým serverem a chráněných virtuálních počítačů.

- Pro testovací účely Pokud máte připojení k Azure ExpressRoute, můžete na místním serveru proces, který je automaticky nainstalován na konfiguračním serveru.
- Pokud máte připojení k síti VPN nebo používáte navrácení služeb po obnovení v produkčním prostředí, musíte vytvořit virtuální počítač Azure jako na základě Azure procesový server navrácení služeb po obnovení.
- Chcete-li nastavit procesní server v Azure, postupujte podle pokynů v [v tomto článku](site-recovery-vmware-setup-azure-ps-resource-manager.md).

## <a name="configure-the-master-target-server"></a>Nakonfigurujte hlavní cílový server

Ve výchozím nastavení se hlavní cílový server spouští na místní konfigurační server. Pro účely tohoto kurzu používáme výchozí stránku předlohy. Hlavní cílový server přijímá data navrácení služeb po obnovení.

Pokud je virtuální počítač na hostiteli ESXi, který je spravován systémem vCenter server, hlavní cílový server musí mít přístup k úložišti dat Virtuálního počítače (VMDK), k zápisu replikovaná data na disky virtuálních počítačů. Ujistěte se, že je úložiště dat virtuálních počítačů připojený na hostiteli se hlavní cíl s přístupem pro čtení a zápis.

Pokud je virtuální počítač na ESXi, který není spravován systémem vCenter server, vytvoří služba Site Recovery při vytvoření nového virtuálního počítače. Virtuální počítač se vytvoří na hostiteli ESX, ve kterém vytvoříte na hlavním cíli.
V úložišti dat, která je přístupná pomocí musí být pevný disk virtuálního počítače hostitele, na kterém je spuštěný hlavní cílový server.

Pokud virtuální počítač nepoužívá vCenter, musíte provést zjišťování hostitele, na kterém je spuštěný hlavní cílový server, než můžete znovu nastavte ochranu na počítač. To platí pro příliš selhání zpět fyzických serverů. Jinou možnost, pokud existuje místní virtuální počítač, je před provedením navrácení služeb po obnovení odstranit. Navrácení služeb po obnovení pak vytvoří nový virtuální počítač na stejném hostiteli jako hostitele ESX hlavní cíl. Pokud žádnou zpět do alternativního umístění, data je obnovena na stejné úložiště dat a na stejného hostitele ESX, který používá místní hlavní cílový server.

Řešení Storage vMotion nelze použít na hlavním cílovém serveru. Pokud tak učiníte, navrácení služeb po obnovení nebude fungovat, protože nejsou dostupné disky. Ze seznamu řešení vMotion vylučte hlavních cílových serverů.

## <a name="reprotect-azure-vms"></a>Znovu nastavte ochranu virtuálních počítačů Azure

Tento postup předpokládá, že místní počítač není k dispozici a že opětovnou ochranu do alternativního umístění.

1. V **nastavení** > **replikované položky**, klikněte pravým tlačítkem na virtuální počítač, který byl převzetí služeb při selhání > **znovu nastavit ochranu**.
2. V **znovu nastavit ochranu**, ověřte, že **Azure k místnímu**, je vybrána.
3. Zadejte místní hlavní cílový server a procesový server.

4. V **úložiště**, vyberte hlavní cíl úložiště, do které chcete obnovit disky na místě. Tuto možnost použijte, když místní virtuální počítač byl odstraněn, a budete muset vytvořit nové disky. Tato nastavení se ignoruje, pokud disky již existují, ale je třeba zadat hodnotu.
5. Vyberte jednotky pro uchovávání dat hlavního cíle. Zásady navrácení služeb po obnovení je automaticky vybrán.
6. Klikněte na tlačítko **OK** zahájíte nové provedení ochrany. Úlohu začne replikovat virtuální počítač z Azure do místní lokality. Průběh můžete sledovat na **úlohy** kartě.

> [!NOTE]
> Pokud chcete obnovit virtuální počítač Azure na stávající místní virtuální počítač, připojte úložiště na místním virtuálním počítači s přístupem pro čtení a zápis, v hostiteli ESXi hlavní cílový server.


## <a name="run-a-failover-from-azure-to-on-premises"></a>Spuštění převzetí služeb při selhání z Azure do místní

K replikaci zpět na místní, se používá navrácení služeb po obnovení zásad. Tato zásada se automaticky vytvoří, když jste vytvořili zásadu replikace pro replikaci do Azure:

- Zásady se automaticky přidruží konfigurační server.
- Zásady nemůže být upraven.
- Zásady hodnoty jsou:
    - Prahová hodnota plánovaného bodu obnovení = 15 minut
    - Uchování bodu obnovení = 24 hodin
    - Frekvence snímkování konzistentní aplikace vzhledem = 60 minut

Spusťte převzetí služeb při selhání následujícím způsobem:

1. Na **replikované položky** stránky, klikněte pravým tlačítkem myši na počítač > **neplánované převzetí služeb při selhání**.
2. V **potvrzení převzetí služeb při selhání**, ověřte, zda je směr převzetí služeb při selhání z Azure.

3. Vyberte bod obnovení, který chcete použít převzetí služeb při selhání. Bod obnovení konzistentních s aplikací dojde před nejnovější bod v čase a může to způsobit ztrátu dat. Site Recovery při převzetí služeb při selhání spuštění, ukončí virtuálních počítačích Azure a místní virtuální počítač se spustí. Nebudou proto být výpadky, vyberte příslušnou dobu.
4. Pravým tlačítkem na počítač a klikněte na tlačítko **potvrdit**. Tím se spustí úlohu, která odebere virtuálních počítačích Azure.
5. Ověřte, že virtuální počítače Azure mají byla vypnuta podle očekávání.


## <a name="reprotect-on-premises-machines-to-azure"></a>Znovu nastavte ochranu lokálních počítačů do Azure

Data by teď měly být zpět na místní lokality, ale není replikaci do Azure. Můžete spustit replikaci do Azure znovu následujícím způsobem:

1. V trezoru > **nastavení** > **replikované položky**, vyberte selhání zálohování virtuálních počítačů, které selhaly zpět a klikněte na tlačítko **znovu nastavit ochranu**.
2. Vyberte server proces, který se používá k odeslání replikovaných dat do Azure a klikněte na tlačítko **OK**.

Po dokončení vytvoření replikuje virtuální počítač zpět do Azure a podle potřeby můžete spustit převzetí služeb při selhání.
