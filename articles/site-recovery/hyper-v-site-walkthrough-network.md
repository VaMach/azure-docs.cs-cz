---
title: "Plán sítí pro technologii Hyper-V (bez System Center VMM) do Azure s replikací pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje plánování sítě požadované při replikaci virtuálních počítačů technologie Hyper-V (bez VMM) do Azure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5ca12254-975d-48e8-a84d-422825dac9b2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 100b9d8a55c2c163e7a04680f0f7d7963315ee73
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-4-plan-networking-for-hyper-v-to-azure-replication"></a>Krok 4: Plánování sítí pro technologii Hyper-V na Azure replikace

Tento článek shrnuje sítě aspekty plánování, když replikovat místní virtuální počítače Hyper-V (bez System Center VMM) do Azure pomocí [Azure Site Recovery](site-recovery-overview.md) služby.

Případné připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="connecting-to-replica-vms-after-failover"></a>Připojení k virtuální počítače repliky po převzetí služeb při selhání

Při plánování replikace a převzetí služeb při selhání, mezi klíčové otázky je způsob připojení k virtuálnímu počítači Azure po převzetí služeb při selhání. Při navrhování strategie sítě pro virtuální počítače Azure repliky existuje několik možností:

- **Jinou IP adresu**: můžete vybrat použít jiný rozsah IP adres pro replikované síť virtuálního počítače Azure. V tomto scénáři virtuální počítač získá nové adresy IP po převzetí služeb při selhání, a je nutná aktualizace DNS. [Další informace](site-recovery-test-failover-vmm-to-vmm.md#prepare-the-infrastructure-for-test-failover)
- **Stejnou IP adresu**: můžete chtít použít stejného rozsahu IP adres, který ve vaší síti primární místní síť Azure po převzetí služeb při selhání.  Zachovat stejné IP adresy zjednodušuje obnovení snížením problémy související se sítí po převzetí služeb při selhání. Ale pokud replikujete do Azure, musíte po převzetí služeb při selhání aktualizace tras se nové umístění na IP adresy.


## <a name="retain-ip-addresses"></a>Zachovat IP adresy

Site Recovery poskytuje schopnost zachovat pevné IP adresy při přebírání služeb při selhání do Azure, s převzetím služeb podsítě.

S převzetí služeb při selhání podsíť určité podsíti se nachází v lokalitě 1 nebo 2 lokality, ale nikdy v obou lokalitách současně. Aby byla zachována adresní prostor IP adres v případě selhání, můžete prostřednictvím kódu programu uspořádejte infrastruktury směrovač přesunout podsítě z jedné lokality do jiného. Během převzetí služeb při selhání přesuňte podsítí s přidružené chráněných virtuálních počítačů. Hlavní nevýhodou je, že v případě selhání, je nutné přesunout celý podsítě.


### <a name="failover-example"></a>Příklad převzetí služeb při selhání

Podívejme se na příklad pro převzetí služeb při selhání do Azure.

- Ficticious společnosti, společnosti Woodgrove Bank má místní infrastruktury hostování jejich obchodních aplikací. Své mobilní aplikace jsou hostované v Azure.
- Připojení mezi virtuálními počítači Woodgrove Bank v Azure a místními servery jsou poskytovány připojení site-to-site (VPN) mezi místní hraniční sítě a virtuální síť Azure.
- Tuto síť VPN znamená, že virtuální síť společnosti v Azure zobrazí jako rozšíření své místní sítě.
- Woodgrove chce pomocí Site Recovery můžete replikovat místní úlohy do Azure.
 - Woodgrove má jak nakládat s aplikací a konfigurace, které závisí na pevně IP adresy a proto potřeba zachovat IP adresy pro své aplikace po převzetí služeb při selhání do Azure.
 - Woodgrove jeho přiřazení IP adres z rozsahu 172.16.1.0/24, 172.16.2.0/24 na jeho prostředky, které běží v Azure.


Woodgrove, abyste mohli replikovat jejích virtuálních počítačů do Azure a zachovat stávající IP adresy, zde je, co společnosti musí udělat:

1. Vytvoření virtuální sítě Azure. Rozšíření místní sítě, třeba tak, že aplikace bezproblémově přebírány.
2. Azure umožňuje přidat připojení VPN typu site-to-site, kromě připojení point-to-site k virtuálním sítím vytvořeným v Azure.
3. Při nastavování připojení site-to-site, v síť Azure, můžete směrovat provoz do místního umístění (místní síť) pouze v případě, že rozsah IP adres se liší od místní rozsah IP adres.
    - Je to proto, že Azure nepodporuje roztažené podsítě. Takže pokud máte podsíť 192.168.1.0/24 místně, nemůžete přidat 192.168.1.0/24 místní sítě v síti Azure.
    - Toto je očekávané, protože Azure není známo, že neexistují žádné aktivní virtuální počítače v podsíti, a že podsíť se vytváří jenom zotavení po havárii.
    - Abyste mohli správně směrovat síťový provoz mimo síť Azure podsítě v síti a v místní síti nesmí být v konfliktu.

![Před převzetí služeb při selhání podsíť](./media/hyper-v-site-walkthrough-network/network-design7.png)

### <a name="before-failover"></a>Před převzetí služeb při selhání

1. Vytvoření další sítě (například síť obnovení). Jedná se o síť ve který převzal virtuální počítače byly vytvořeny.
2. K zajištění, že IP adresu pro virtuální počítač se uchovávají po převzetí služeb ve vlastnostech virtuálního počítače > **konfigurace**, zadejte stejnou IP adresu, aby měl virtuální počítač na místě a klikněte na **Uložit**.
3. Když je virtuální počítač převzal, přiřadí Azure Site Recovery k němu zadaná IP adresa.

    ![Vlastnosti sítě](./media/hyper-v-site-walkthrough-network/network-design8.png)

4. Po převzetí služeb při selhání je aktivační událost se aktivuje a virtuální počítače jsou vytvořeny v Azure s požadované IP adresu, můžete připojit k síti pomocí [virtuální síť k virtuální síti vonnection](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Tato akce může provádět skriptování.
5. Trasy potřebovat k odpovídajícím způsobem upravit, aby odpovídaly že této 192.168.1.0/24 je nyní přesunuta do Azure.

    ![Po převzetí služeb při selhání podsíť](./media/hyper-v-site-walkthrough-network/network-design9.png)

### <a name="after-failover"></a>Po převzetí služeb při selhání

Pokud nemáte síť Azure, jak je popsáno výše, můžete vytvořit připojení site-to-site VPN mezi primární lokalitou a Azure, po failvoer.

## <a name="change-ip-addresses"></a>Změna IP adresy

To [příspěvku na blogu](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) vysvětluje, jak nastavit Azure síťová infrastruktura, pokud nepotřebujete zachovat IP adres po převzetí služeb při selhání. Popis aplikace spustí, vypadá v tom, jak nastavit místní sítí a Azure a s informacemi o spuštění převzetí služeb při selhání se ukončí.  

## <a name="next-steps"></a>Další kroky

Přejděte na [krok 5: Příprava Azure](hyper-v-site-walkthrough-prepare-azure.md)
