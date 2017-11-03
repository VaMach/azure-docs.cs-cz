---
title: "Připojení virtuálního počítače po převzetí služeb při selhání pro sekundární lokalitu s Azure Site Recovery | Microsoft Docs"
description: "Sítě pokyny pro připojení k virtuální počítače po převzetí služeb při selhání pro sekundární lokalitu s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 67d73590-185c-49b2-a097-597bf54747a9
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: raynew
ms.openlocfilehash: 7b27fc568c77b44ab2366d297ca9e7685439143e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="vm-connectivity-after-failover-to-a-secondary-site"></a>Připojení virtuálních počítačů po převzetí služeb při selhání do sekundární lokality

Po zkontrolování požadavcích pro nasazení, přečtěte si tento článek k plánování sítě při replikaci virtuálních počítačů technologie Hyper-V (VM) spravované v cloudech System Center Virtual Machine Manager (VMM), sekundární lokality pomocí [Azure Site Recovery](site-recovery-overview.md) na portálu Azure. 

Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Přehled

Při plánování replikace a převzetí služeb při selhání, mezi klíčové otázky je postup připojení do repliky po převzetí služeb při selhání. Existuje několik možností: 

- **Použít jinou IP adresu**: můžete vybrat použít jinou IP adresu pro replikované virtuální počítač. V tomto scénáři virtuální počítač získá nové adresy IP po převzetí služeb při selhání, a je nutná aktualizace DNS.
- **Zachovat stejnou IP adresu**: můžete chtít použít stejnou IP adresu pro repliku virtuálního počítače. Zachovat stejné IP adresy zjednodušuje obnovení snížením problémy související se sítí po převzetí služeb při selhání. 

## <a name="retaining-ip-addresses"></a>Zachování IP adresy

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




