---
title: "Plánování sítě pro replikaci technologie Hyper-V do sekundární lokalita VMM s Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje plánování sítě při replikaci virtuálních počítačů Hyper-V do sekundární lokality System Center VMM s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 095f2d73-994e-4fc0-96f2-e03a7d72e492
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: raynew
ms.openlocfilehash: a1f3f6e6cba074647195e2b0cbcdc7b4f3dec475
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-3-plan-networking-for-hyper-v-vm-replication-to-a-secondary-vmm-site"></a>Krok 3: Plánování sítě pro replikaci virtuálního počítače technologie Hyper-V do sekundární lokality VMM

Po zkontrolování požadavcích pro nasazení, přečtěte si tento článek k plánování sítě při replikaci virtuálních počítačů technologie Hyper-V (VM) spravované v cloudech System Center Virtual Machine Manager (VMM), sekundární lokality pomocí [Azure Site Recovery](site-recovery-overview.md) na portálu Azure. 

Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="network-mapping-overview"></a>Přehled mapování sítě

Mapování sítě se používá při replikaci virtuálních počítačů Hyper-V (spravované v nástroji VMM) do sekundárního datacentra. Mapování sítě zajišťuje mapování mezi sítěmi virtuálních počítačů na zdrojovém serveru VMM a sítě virtuálních počítačů na cílovém serveru VMM. Mapování provede následující akce:

- **Síťové připojení**– připojí virtuální počítače na odpovídající sítě po převzetí služeb při selhání. Virtuální počítač repliky se připojí k cílové síti, který je namapovaný na zdrojovou síť.
- **Optimální umístění**– optimálně umístí virtuální počítače replik na hostitelských serverech technologie Hyper-V. Virtuální počítače repliky jsou umístěny na hostitelích, kteří mohou přistupovat k namapovanou síť virtuálních počítačů.
- **Žádné mapování sítě**– Pokud nenakonfigurujete mapování sítě, virtuální počítače replik se nepřipojí k žádné síti virtuálních počítačů po převzetí služeb při selhání.


### <a name="example"></a>Příklad

Tady je příklad pro ilustraci tento mechanismus. Podívejme se na organizaci s dvěma umístěními v New Yorku a Chicagu.

**Umístění** | **Server VMM** | **Sítě virtuálních počítačů** | **Mapovat na**
---|---|---|---
New York | VMM NewYork| VMNetwork1 NewYork | Mapovat do Chicaga VMNetwork1
 |  | VMNetwork2 NewYork | Není mapováno
Chicago | VMM Chicago| VMNetwork1 Chicago | Mapovat na VMNetwork1 NewYork
 | | VMNetwork1 Chicago | Není mapováno

V tomto příkladu:

- Když virtuální počítač repliky se vytvoří pro virtuální počítač, který je připojen k VMNetwork1 NewYork, bude připojen k VMNetwork1 Chicagu.
- Když virtuální počítač repliky se vytvoří pro VMNetwork2 NewYork nebo VMNetwork2 Chicagu, nebude připojen k žádné síti.

Zde je, jak jsou cloudy VMM nastavit v našem příkladu organizace a logické sítě přidružené ke cloudům.

#### <a name="cloud-protection-settings"></a>Nastavení ochrany cloudu

**Chráněném cloudu** | **Ochrana cloudu** | **Logické sítě (New Yorku)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>Není k dispozici</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>
SilverCloud2 | <p>Není k dispozici</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>

#### <a name="logical-and-vm-network-settings"></a>Nastavení sítě logické a virtuální počítač

**Umístění** | **Logické sítě** | **Přidružené sítě virtuálních počítačů**
---|---|---
New York | LogicalNetwork1 NewYork | VMNetwork1 NewYork
Chicago | LogicalNetwork1 Chicago | VMNetwork1 Chicago
 | LogicalNetwork2Chicago | VMNetwork2 Chicago

#### <a name="target-network-settings"></a>Nastavení cílové sítě

Na základě tohoto nastavení, když vyberete Cílová síť virtuálních počítačů, v následující tabulce jsou možnosti, které budou k dispozici.

**Výběr** | **Chráněném cloudu** | **Ochrana cloudu** | **Cílová síť k dispozici**
---|---|---|---
VMNetwork1 Chicago | SilverCloud1 | SilverCloud2 | Dostupné
 | GoldCloud1 | GoldCloud2 | Dostupné
VMNetwork2 Chicago | SilverCloud1 | SilverCloud2 | Není k dispozici
 | GoldCloud1 | GoldCloud2 | Dostupné


Pokud má cílová síť více podsítí a jedna z těchto podsítí má stejný název jako podsíť, na kterém se nachází zdrojový virtuální počítač, pak virtuální počítač repliky se připojí k této cílové podsíti po převzetí služeb při selhání. Pokud neexistuje žádná cílová podsíť s odpovídajícím názvem, připojí se virtuální počítač k první podsíti v síti.


#### <a name="failback-behavior"></a>Chování navrácení služeb po obnovení

Pokud chcete zobrazit, co se stane, že v případě navrácení služeb po obnovení (zpětná replikace), Předpokládejme, že VMNetwork1 NewYork je namapována na VMNetwork1-Chicagu s následujícím nastavením.


**Virtuální počítač** | **Připojení k síti virtuálních počítačů**
---|---
VM1 | VMNetwork1 sítě
Virtuálního počítače 2 (repliky VM1) | VMNetwork1 Chicago

S těmito nastaveními pojďme si shrnout, co se stane, že v několika možných scénářích.

**Scénář** | **Výsledek**
---|---
Žádná změna v vlastnosti sítě virtuálních počítačů 2 po převzetí služeb při selhání. | Zůstane připojené ke zdrojové síti virtuálních počítačů 1.
Vlastnosti sítě virtuálních počítačů 2 se změní po převzetí služeb při selhání a není připojen. | 1 virtuální počítač není připojen.
Vlastnosti sítě virtuálních počítačů 2 se změní po převzetí služeb při selhání a je připojený k VMNetwork2 Chicagu. | Pokud není namapované VMNetwork2 Chicagu, bude odpojen virtuálních počítačů 1.
Mapování sítě VMNetwork1 Chicagu se změní. | 1 virtuální počítač bude připojený k síti nyní mapováno na VMNetwork1 Chicagu.



## <a name="prepare-for-network-mapping"></a>Příprava mapování sítě

1. Na serverech VMM zdrojové a cílové měli byste mít logické sítě přidružené ke cloudům zdrojové a cílové. 
2. Ve zdrojové a cílové servery měli byste mít síť virtuálních počítačů připojených k logické síti.
3. Virtuální počítače na hostitele Hyper-V ve zdrojovém umístění musí být propojena na zdrojové síti virtuálních počítačů. Pokud používáte pouze jeden server VMM, můžete nakonfigurovat mapování sítě virtuálních počítačů na stejném serveru.

Zde je, co se stane, když nastavíte mapování sítě během nasazování Site Recovery:

- Když nastavit mapování sítě a vyberte cílovou síť virtuálních počítačů, cloudy VMM zdroje, které používají zdrojové síti virtuálních počítačů se zobrazí, společně s sítě virtuálních počítačů dostupných cílových v cloudech, cíl.
- - Při mapování správně nakonfigurovaná a je povolená replikace, zdrojového virtuálního počítače se připojí k jeho zdrojové síti virtuálních počítačů a jeho repliky v cílovém umístění, budou připojené k jeho namapovanou síť virtuálních počítačů.
- Pokud má cílová síť více podsítí a jedna z těchto podsítí má stejný název jako podsíť, na kterém se nachází zdrojový virtuální počítač, pak virtuální počítač repliky se připojí k této cílové podsíti po převzetí služeb při selhání. Pokud není žádná cílová podsíť s odpovídajícím názvem, virtuální počítač se připojí k první podsíti v síti.

## <a name="connect-to-vms-after-failover"></a>Po převzetí služeb při selhání připojit k virtuální počítače

Při plánování replikace a převzetí služeb při selhání, mezi klíčové otázky je postup připojení do repliky po převzetí služeb při selhání. Existuje několik možností: 

- **Použít jinou IP adresu**: můžete vybrat použít jinou IP adresu pro replikované virtuální počítač. V tomto scénáři virtuální počítač získá nové adresy IP po převzetí služeb při selhání, a je nutná aktualizace DNS.
- **Zachovat stejnou IP adresu**: můžete chtít použít stejnou IP adresu pro repliku virtuálního počítače. Zachovat stejné IP adresy zjednodušuje obnovení snížením problémy související se sítí po převzetí služeb při selhání. 

## <a name="retain-ip-addresses"></a>Zachovat IP adresy

Pokud chcete zachovat IP adresy z primární lokality po převzetí služeb při selhání pro sekundární lokalitu, můžete selhání úplné podsítě a aktualizovat trasy k označení nové umístění na IP adresy nebo alternativní nasazení podsíť roztažené mezi primárním serverem a obnovení lokality.

### <a name="stretched-subnet"></a>Roztažené podsítě

V podsíť roztažené podsíť je k dispozici současně v primární a sekundární lokality. Pokud přesunete do sekundární lokality serveru a jeho konfigurace IP adresy (vrstvy 3), sítě bude směrovat přenosy do nového umístění automaticky. 

Z hlediska vrstvy 2 (Datová vrstva odkaz) budete potřebovat síťové zařízení, která můžete spravovat roztažené sítě VLAN. Kromě toho roztáhnout sítě VLAN, potenciální domény selhání rozšiřuje do obou lokalit, v podstatě stane jediným bodem selhání. Přestože se pravděpodobně, k tomu mohlo dojít, že všesměrového vysílání storm spuštěná a nemůže být izolované. 


### <a name="subnet-failover"></a>Podsíť převzetí služeb při selhání

Můžete spustit převzetí služeb při selhání podsíť využívat výhody roztažené podsíť bez ve skutečnosti roztažení ho. V tomto řešení bude podsíť k dispozici v lokalitě zdroje nebo cíle, ale ne v obou současně. Pokud chcete zachovat v případě selhání adresní prostor IP adres, můžete prostřednictvím kódu programu uspořádat infrastruktury směrovač přesunout podsítě z jedné lokality do jiného. Poté, co když dojde k převzetí služeb při selhání by podsítě přesuňte přidružený virtuálních počítačů. Hlavní nevýhodou je, že v případě selhání, je nutné přesunout celý podsítě.

### <a name="example"></a>Příklad

Tady je příklad podsíť dokončení převzetí služeb při selhání. Aplikace běžící v podsíti 192.168.1.0/24 má primární lokalita. Všechny virtuální počítače v této podsíti na převzetí služeb při selhání, jsou převzetí služeb při selhání pro sekundární lokalitu a zachovat jejich IP adresy. Směrování je potřeba upravit tak, aby odrážela skutečnost, že všechny virtuální počítač virtuální počítače patřící do podsítě 192.168.1.0/24 mají nyní přesunuta do sekundární lokality.

Následující obrázky znázorňují podsítě, před a po převzetí služeb při selhání:

- Před převzetí služeb při selhání 192.168.0.1/24 podsíť je aktivní ve zdrojové lokalitě, stane aktivní v sekundární lokalitě po převzetí služeb při selhání.
- Směrování mezi primární lokality a lokality obnovení, třetí lokality primární lokality a třetí lokalit a obnovení bude muset být odpovídajícím způsobem upravit.

**Před převzetí služeb při selhání**

![Před převzetí služeb při selhání](./media/vmm-to-vmm-walkthrough-network/network-design2.png)

**Po převzetí služeb při selhání**

![Po převzetí služeb při selhání](./media/vmm-to-vmm-walkthrough-network/network-design3.png)

Po převzetí služeb při selhání stane se toto:

- Site Recovery přidělí IP adresu pro každé síťové rozhraní na virtuálním počítači, z fondu statických adres IP v příslušné síti, pro každou instanci služby VMM.
- Pokud fond IP adres v sekundární lokalitě je stejný jako ve zdrojové lokalitě, Site Recovery přiděluje stejnou IP adresu (ze zdrojového virtuálního počítače) v replice virtuálního počítače. IP adresa je vyhrazená v nástroji VMM, ale není nastaven jako IP adresu převzetí služeb při selhání na hostitele Hyper-V. Těsně před převzetí služeb při selhání je sada adres IP převzetí služeb při selhání na hostiteli technologie Hyper-v.
- Pokud není k dispozici stejnou IP adresu, Site Recovery přiděluje další dostupnou IP adresu z fondu.
- Pokud virtuální počítače používat službu DHCP, není Site Recovery spravovat IP adresy. Je třeba zkontrolovat, že server DHCP v sekundární lokalitě můžete přidělit adresu ze stejného rozsahu jako zdrojová lokalita.

### <a name="validate-the-ip-address"></a>Ověřte adresu IP

Po povolení ochrany pro virtuální počítač, můžete využít následující ukázkový skript k ověření adresy přiřazené k virtuálnímu počítači. Stejnou IP adresu bude nastaven jako IP adresu převzetí služeb při selhání a přiřazené k virtuálnímu počítači v době převzetí služeb při selhání:

    ```
    $vm = Get-SCVirtualMachine -Name <VM_NAME>
    $na = $vm[0].VirtualNetworkAdapters>
    $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
    $ip.address 
    ```

## <a name="changing-ip-addresses"></a>Změna IP adresy

V tomto scénáři se změní IP adresy virtuálních počítačů, které převzetí služeb při selhání. Nevýhodou tohoto řešení je údržby vyžaduje. Obvykle DNS bude aktualizován po spuštění replikované virtuální počítače. Záznamy DNS může být nutné změnit nebo fluster v síťového a aktualizované položky mezipaměti. To může vést k výpadkům. Výpadek lze zmírnit následujícím způsobem:

- Použijte nízké hodnoty TTL pro aplikace v síti intranet.
- Pomocí následujícího skriptu v plánu obnovení Site Recovery, chcete-li aktualizovat server DNS zajistit včasné aktualizace. Skript není nutný, pokud používáte dynamickou registraci DNS.

    ```
    param(
    string]$Zone,
    [string]$name,
    [string]$IP
    )
    $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
    $newrecord = $record.clone()
    $newrecord.RecordData[0].IPv4Address  =  $IP
    Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord
    ```
    
### <a name="example"></a>Příklad 

Podívejme se na scénář, ve které se chystáte použít jinou IP adres mezi primárním serverem a obnovení lokality. V tomto příkladu máme různých IP adres napříč primárních a sekundárních lokalit a; je přístupná s třetí lokality, ze které aplikace hostované na serveru primární nebo obnovení.

- Před převzetí služeb při selhání aplikace jsou 192.168.1.0/24 hostované podsítí v primární lokalitě a jsou nakonfigurované jako v podsíti 172.16.1.0/24 v sekundární lokalitě po převzetí služeb při selhání.
- Trasy připojení nebo síť VPN byla správně nakonfigurována, aby všechny tři servery navzájem přístup.
- Po převzetí služeb při selhání se obnoví v podsíti obnovení aplikace. V tomto scénáři je potřeba převzít celou podsíť, a je nutné provést žádné změny a změňte konfiguraci sítě VPN nebo síťové trasy. Převzetí služeb při selhání a některé aktualizace služby DNS, ujistěte se, aby aplikace zůstaly dostupné.
- Pokud chcete povolit dynamické aktualizace DNS nakonfigurovaný, bude registrovat virtuální počítače se při spuštění po převzetí služeb při selhání na novou IP adresu.

**Před převzetí služeb při selhání**

![Různé IP - před převzetí služeb při selhání](./media/vmm-to-vmm-walkthrough-network/network-design10.png)

**Po převzetí služeb při selhání**

![Různé IP - po převzetí služeb při selhání](./media/vmm-to-vmm-walkthrough-network/network-design11.png)



## <a name="next-steps"></a>Další kroky

Přejděte na [krok 4: Příprava VMM a technologie Hyper-V](vmm-to-vmm-walkthrough-vmm-hyper-v.md).


