---
title: "Testovací převzetí služeb při selhání (VMM do nástroje VMM) ve službě Azure Site Recovery | Microsoft Docs"
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
ms.date: 06/05/2017
ms.author: pratshar
ms.openlocfilehash: afc4790d5714ce7145c8f4291a05acc2e9882a9b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="test-failover-vmm-to-vmm-in-site-recovery"></a>Testovací převzetí služeb při selhání (VMM do nástroje VMM) ve službě Site Recovery


Tento článek obsahuje informace a pokyny pro provádění testovací převzetí služeb nebo přechod zotavení po havárii virtuálních počítačů (VM) a fyzické servery, které jsou chráněné službou Azure Site Recovery. System Center Virtual Machine Manager VMM spravovat místní lokality budete používat jako lokality pro obnovení.

Můžete spustit převzetí služeb při selhání ověření strategie replikace nebo provést zotavení po Havárii procházení bez ztráty dat nebo výpadek. Testovací převzetí služeb nemá žádný vliv na probíhající replikace nebo produkční prostředí. Můžete ji spustit virtuální počítač nebo [plán obnovení](site-recovery-create-recovery-plans.md). Když se aktivuje testovací převzetí služeb, budete muset zadat testovací virtuální počítače se budou připojovat k síti. Můžete sledovat průběh převzetí služeb při selhání na **úlohy** stránky.  

Pokud máte jakékoli dotazy nebo připomínky, odešlete je na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prepare-the-infrastructure-for-test-failover"></a>Příprava infrastruktury pro testovací převzetí služeb při selhání
Pokud chcete spustit testovací převzetí služeb pomocí stávající síť, připravte služby Active Directory, DHCP a DNS v síti.

Pokud chcete spustit testovací převzetí služeb pomocí možnosti vytvoření sítě virtuálních počítačů automaticky, přidejte v plánu obnovení, který se chystáte použít pro testovací převzetí služeb manuální krok před skupiny-1. Pak přidejte prostředky infrastruktury k síti automaticky vytvořené před spuštěním testu převzetí služeb.

### <a name="things-to-note"></a>Všimněte
Pokud replikujete do sekundární lokality, typ sítě, který používá počítač repliky se nemusí shodovat s typem logické sítě pro testovací převzetí služeb při selhání, ale některé kombinace nemusí fungovat. Pokud replika používá DHCP a sítí VLAN izolace, síť virtuálních počítačů pro repliku nepotřebuje fond statických adres IP. Proto pro testovací převzetí služeb pomocí virtualizace sítě systému Windows nebude fungovat, protože nejsou k dispozici žádné fondy adres. 

Kromě toho testovací převzetí služeb nebude fungovat, pokud síť repliky používá bez izolace a testovací síti používá virtualizace sítě systému Windows. To je proto síti bez izolace nemá podsítě potřebné pro vytvoření síť virtualizace sítě systému Windows.

Jak jsou repliky virtuální počítače připojené k namapované sítě virtuálních počítačů po převzetí služeb při selhání závisí na konfiguraci sítě virtuálních počítačů v konzole nástroje VMM.

#### <a name="vm-network-configured-with-no-isolation-or-vlan-isolation"></a>Síť virtuálních počítačů konfigurovanou bez izolace nebo izolace sítě VLAN
Pokud DHCP je definována pro síť virtuálních počítačů, je připojený virtuální počítač repliky na ID sítě VLAN prostřednictvím nastavení, které jsou určené pro síťové lokality v přidruženou logickou síť. Virtuální počítač přijímá jeho IP adresu ze serveru DHCP k dispozici. 

Nemusíte definovat fond statických adres IP pro síť virtuálních počítačů cíl. Pokud se používá fond statických adres IP pro síť virtuálních počítačů, je připojený virtuální počítač repliky na ID sítě VLAN prostřednictvím nastavení, které jsou určené pro síťové lokality v přidruženou logickou síť.

Virtuální počítač přijímá jeho IP adresu z fondu, která je definována pro síť virtuálních počítačů. Pokud fond statických adres IP není definován v cílové síti virtuálních počítačů, se nezdaří přidělení IP adresy. Vytvořte fond IP adres na serverech VMM zdroj i cíl, které budete používat pro ochranu a obnovení.

#### <a name="vm-network-with-windows-network-virtualization"></a>Síť virtuálních počítačů s virtualizací sítě systému Windows
Pokud je síť virtuálních počítačů nakonfigurované pomocí virtualizace sítě systému Windows, byste měli definovat fond statických pro virtuální počítač má cílová síť bez ohledu na to, jestli je zdrojové síti virtuálních počítačů nakonfigurované na používání protokolu DHCP nebo fond statických adres IP. 

Pokud definujete DHCP, cílovém serveru VMM funguje jako DHCP server a poskytuje IP adresu z fondu, která je definována pro síť virtuálních počítačů cíl. Pokud se používá fond statických IP adres je definována pro zdrojový server, cílovém serveru VMM přidělí IP adresu z fondu. V obou případech přidělování IP adres se nezdaří, pokud fond statických adres IP není definován.


### <a name="prepare-dhcp"></a>Příprava DHCP
Pokud součástí virtuálních počítačů testovací převzetí služeb při selhání používal protokol DHCP, vytvořit testovací server DHCP v rámci izolované sítě pro účely testovací převzetí služeb při selhání.

### <a name="prepare-active-directory"></a>Příprava služby Active Directory
Pokud chcete spustit testovací převzetí služeb pro aplikace, testování, je třeba kopii produkčního prostředí služby Active Directory v testovacím prostředí. Další informace najdete v článku [testovací převzetí služeb při selhání důležité informace týkající se služby Active Directory](site-recovery-active-directory.md#test-failover-considerations).

### <a name="prepare-dns"></a>Příprava DNS
Připravte DNS server pro testovací převzetí služeb takto:

* **DHCP**: Pokud virtuální počítače používat službu DHCP, IP adresu testu DNS by měly být aktualizovány na testovací server DHCP. Pokud používáte typ sítě virtualizace sítě systému Windows, VMM server funguje jako DHCP server. Proto je třeba aktualizovat na IP adresu DNS v testovací síti převzetí služeb při selhání. V takovém případě virtuální počítače zaregistrovat sami na relevantní server DNS.
* **Statická adresa**: Pokud virtuální počítače používat statickou IP adresu, na IP adresu serveru DNS testovací aktualizoval v testovací síti převzetí služeb při selhání. Možná budete muset aktualizovat DNS na IP adresu testovací virtuální počítače. Pro tento účel můžete použít následující ukázkový skript:

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání
Tento postup popisuje, jak spouštět testovací převzetí služeb při selhání pro plán obnovení. Alternativně můžete spustit převzetí služeb při selhání pro jeden virtuální počítač na **virtuální počítače** kartě.

![Okno test převzetí služeb při selhání](./media/site-recovery-test-failover-vmm-to-vmm/TestFailover.png)

1. Vyberte **plány obnovení** > *recoveryplan_name*. Klikněte na tlačítko **převzetí služeb při selhání** > **testovací převzetí služeb při selhání**.
1. Na **testovací převzetí služeb při selhání** okno, zadejte, jak virtuální počítače by měl být připojeny k sítím po testu převzetí služeb. Další informace najdete v tématu [sítě možnosti](#network-options-in-site-recovery).
1. Sledovat průběh převzetí služeb při selhání **úlohy** kartě.
1. Po dokončení převzetí služeb při selhání ověřte, že virtuální počítače úspěšně spustí.
1. Když jste hotovi, klikněte na tlačítko **vyčistit testovací převzetí služeb při selhání** v plánu obnovení. V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s testovací převzetí služeb. Tento krok odstraní virtuální počítače a sítě, které byly vytvořeny během testovacího převzetí služeb při selhání.


## <a name="network-options-in-site-recovery"></a>Možnosti sítě v Site Recovery

Při spuštění převzetí služeb při selhání, budete vyzváni k výběru nastavení sítě pro testovací repliku počítače. Máte několik možností.  

| **Možnost testovací převzetí služeb při selhání** | **Popis** | **Kontrola převzetí služeb při selhání** | **Podrobnosti** |
| --- | --- | --- | --- |
| **Převzetí služeb při selhání pro sekundární lokalita VMM – bez sítě** |Nemáte vyberte síť virtuálních počítačů. |Převzetí služeb při selhání ověří, že jsou vytvořeny testovacích počítačů.<br/><br/>Na hostiteli, kde existuje virtuální počítač repliky se vytvoří testovací virtuální počítač. Není přidáno do cloudu, kde je umístěn virtuální počítač repliky. |<p>Počítače při selhání není připojen k žádné síti.<br/><br/>Na počítači může být připojen k síti virtuálních počítačů po jeho vytvoření. |
| **Převzetí služeb při selhání pro sekundární lokalita VMM – se sítí** |Vyberte stávající síť VM. |Převzetí služeb při selhání ověří, že byly vytvořeny virtuální počítače. |Na hostiteli, kde existuje virtuální počítač repliky se vytvoří testovací virtuální počítač. Není přidáno do cloudu, kde je umístěn virtuální počítač repliky.<br/><br/>Vytvořte síť virtuálních počítačů, která bude izolovaná od produkční sítě.<br/><br/>Pokud používáte síť založená na síti VLAN, doporučujeme vytvořit samostatnou logickou síť (nepoužívá se v produkčním prostředí) v nástroji VMM pro tento účel. Tato logická síť se používá k vytvoření sítě virtuálních počítačů pro testovací převzetí služeb při selhání.<br/><br/>Logická síť má být přidružen alespoň jeden ze síťových adaptérů ve všech serverech technologie Hyper-V, které jsou hostiteli virtuálních počítačů.<br/><br/>Pro logické sítě VLAN musí být izolované síťové lokality, které přidáte k logické síti.<br/><br/>Pokud používáte logické sítě na základě virtualizace sítě systému Windows, Azure Site Recovery automaticky vytvoří izolované sítě virtuálních počítačů. |
| **Převzetí služeb při selhání pro sekundární lokalita VMM – vytvoření sítě** |Dočasné testovací síti se vytvoří automaticky v závislosti na nastavení, které určíte v **logické sítě** a její související síťové lokality. |Převzetí služeb při selhání ověří, že byly vytvořeny virtuální počítače. |Tuto možnost použijte, pokud plán obnovení používá více než jedna síť virtuálních počítačů. Pokud používáte Windows virtualizace sítě, tato možnost umožňuje automatické vytvoření sítě virtuálních počítačů se stejným nastavením (podsítí a fondy IP adres) v síti virtuálního počítače repliky. Tyto sítě virtuálních počítačů se automaticky vyčistí po dokončení testu převzetí služeb.</p><p>Na hostiteli, kde existuje virtuální počítač repliky se vytvoří testovací virtuální počítač. Není přidáno do cloudu, kde je umístěn virtuální počítač repliky. |

> [!TIP]
> IP adresa zadány pro virtuální počítač během testovacího převzetí služeb při selhání se stejnou IP adresu, která by virtuální počítač přijímat plánovaném nebo neplánovaném převzetí služeb při selhání (za předpokladu, že IP adresa je k dispozici v testovací síti převzetí služeb při selhání). Pokud není k dispozici v testovací síti převzetí služeb při selhání stejnou IP adresu, virtuální počítač přijímá jinou IP adresu, která je k dispozici v testovací síti převzetí služeb při selhání.
>
>


## <a name="test-failover-to-a-production-network-on-a-recovery-site"></a>Testovací převzetí služeb při selhání na produkční síť na obnovení lokality
Doporučujeme, pokud jste to testovací převzetí služeb, abyste zvolili síť, která se liší od síti produkční obnovení lokality, který jste zadali v [mapování sítě](https://docs.microsoft.com/azure/site-recovery/site-recovery-network-mapping). Ale pokud Opravdu chcete ověřit připojení k síti začátku do konce ve virtuálním počítači při selhání, mějte na paměti následující body:

* Ujistěte se, že primární virtuální počítač je vypnutý když provádíte testu převzetí služeb. Pokud to neuděláte, dva virtuální počítače se stejnou identitou bude spuštěna ve stejné síti současně. Tato situace může vést k neočekávanému důsledky.
* Veškeré změny, které provedete testovací převzetí služeb při selhání virtuálního počítače budou ztraceny při vyčistit testovací převzetí služeb při selhání virtuálního počítače. Tyto změny nejsou replikovat zpět na primární virtuální počítač.
* Tento způsob plnění testování vede k výpadku pro produkční aplikace. Požádejte uživatele aplikace není pro použití aplikace v průběhu procházení zotavení po Havárii.  


## <a name="next-steps"></a>Další kroky
Poté, co byl úspěšně spuštěn převzetí služeb při selhání, můžete zkusit [převzetí služeb při selhání](site-recovery-failover.md).
