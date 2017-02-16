---
title: Jak funguje Site Recovery? | Dokumentace Microsoftu
description: "Tento článek přináší přehled architektury Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/02/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: bd8082c46ee36c70e372208d1bd15337acc558a1
ms.openlocfilehash: eb97f66901efa336942dee56d9a8a62ade1f6842


---
# <a name="how-does-azure-site-recovery-work"></a>Jak funguje Azure Site Recovery?

Tento článek vám pomůže porozumět základní architektuře služby Azure Site Recovery a komponentám, které jí umožňují fungovat.

Organizace potřebují strategii BCDR, která určuje, jak aplikace, úlohy a data zůstanou spuštěné a dostupné během plánovaných a neplánovaných výpadků a jak co nejdříve obnovit normální provozní podmínky. Strategie BCDR by měla zajistit bezpečnost a obnovitelnost firemních dat a zajistit, aby v případě, že dojde k havárii, byly zpracovávané úlohy stále k dispozici.

Site Recovery je služba Azure, která přispívá ke strategii BCDR orchestrací replikace místní fyzických serverů a virtuálních počítačů do cloudu (Azure) nebo do sekundárního datového centra. Pokud dojde k výpadkům ve vašem primárním umístění, předáte služby při selhání do sekundárního umístění, aby aplikace a úlohy zůstaly dostupné. Až se obnoví normální provozní podmínky, vrátíte služby po obnovení zpět do primárního umístění. Potřebujete další informace [O službě Site Recovery?](site-recovery-overview.md)

Tento článek popisuje postup nasazení na webu [Azure Portal](https://portal.azure.com). [Portál Azure Classic](https://manage.windowsazure.com/) můžete použít k udržování existujících trezorů Site Recovery, ale ne k vytváření nových.

Případné připomínky můžete připojit v dolní části stránky. Technické dotazy můžete zadávat ve [fóru Služeb zotavení Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="deployment-scenarios"></a>Scénáře nasazení

Site Recovery je možné nasadit pro orchestraci replikace v řadě scénářů:

- **Replikace virtuálních počítačů VMware**: Místní virtuální počítače VMware můžete replikovat buď do Azure, nebo do sekundárního datacentra.
- **Replikace fyzických počítačů:** Fyzické počítače (Windows nebo Linux) můžete replikovat buď do Azure, nebo do sekundárního datacentra. Proces replikace fyzických počítačů je téměř stejný jako u virtuálních počítačů VMware.
- **Replikace virtuálních počítačů Hyper-V:** Virtuální počítače Hyper-V můžete replikovat buď do Azure, nebo do sekundární lokality VMM. Pokud je chcete replikovat do sekundární lokality, musí být spravované v cloudech System Center Virtual Machine Manager (VMM).
- **Migrace virtuálních počítačů:** kromě replikace místních virtuálních počítačů a fyzických serverů do Azure (replikace, převzetí služeb při selhání a navrácení služeb po obnovení) je také možná jejich migrace na virtuální počítače Azure (replikace, převzetí služeb při selhání, žádné navrácení služeb po obnovení). Při migraci máte tyto možnosti:
    - Migrovat úlohy běžící na místních virtuálních počítačích Hyper-V, virtuální počítače VMware a fyzické servery na virtuální počítače Azure.
    - Migrovat [virtuální počítače Azure IaaS](site-recovery-migrate-azure-to-azure.md) mezi oblastmi Azure. V tomto scénáři je aktuálně podporovaná pouze migrace, což znamená, že nemůžete navrátit služby po obnovení.
    - Migrovat [instance Windows AWS](site-recovery-migrate-aws-to-azure.md) do virtuálních počítačů Azure IaaS. V tomto scénáři je aktuálně podporovaná pouze migrace, což znamená, že nemůžete navrátit služby po obnovení.

Site Recovery replikuje aplikace spouštěné na podporovaných virtuálních počítačích a fyzických serverech. Úplný souhrn podporovaných aplikací naleznete v tématu [Jaké úlohy je možné chránit pomocí Azure Site Recovery?](site-recovery-workload.md).

## <a name="replicate-vmware-vmsphysical-servers-to-azure"></a>Replikace virtuálních počítačů VMware a fyzických serverů do Azure

### <a name="components"></a>Komponenty

**Komponenta** | **Podrobnosti**
--- | ---
**Azure** | Pro Azure potřebujete účet Microsoft Azure, účet úložiště Azure a síť Azure.<br/><br/> Účty úložiště a sítě můžou být klasické účty nebo účty Resource Manageru.<br/><br/>  Replikovaná data jsou uložena v účtu úložiště a virtuální počítače Azure se z replikovaných dat vytvoří, jakmile dojde k převzetí místních služeb při selhání. Virtuální počítače Azure se připojí k virtuální síti Azure po svém vytvoření.
**Konfigurační server** | Potřebujete místní konfigurační server pro koordinaci komunikaci mezi místním prostředím a Azure a pro správu replikace dat.
**Procesový server** | Obvykle se instaluje na místní konfigurační server.<br/><br/> Funguje jako replikační brána. Přijímá replikační data z chráněných zdrojových počítačů, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odesílá je do úložiště Azure.<br/><br/> Rovněž se stará o nabízenou instalaci služby Mobility na chráněné počítače a provádí automatické zjišťování virtuálních počítačů VMware.<br/><br/> Jak vaše nasazení poroste, můžete přidávat další samostatné vyhrazené procesní servery pro zpracování zvyšujícího se objemu replikačních přenosů.
**Hlavní cílový server** | Obvykle se instaluje na místní konfigurační server.<br/><br/> Zpracovává replikační data během navracení služeb z Azure po obnovení. Pokud je objem přenosů při navracení služeb po obnovení příliš vysoký, můžete nasadit samostatný hlavní cílový server pro účely navrácení služeb po obnovení.
**Servery VMware** | Servery vCenter a vSphere přidejte do vašeho trezoru služby Recovery Services, abyste mohli replikovat virtuální počítače VMware.<br/><br/> Pokud replikujete fyzické servery, budete pro navrácení služeb po obnovení potřebovat místní infrastrukturu VMware. Služby po obnovení nelze navrátit na fyzický server.
**Replikované počítače** | Na každý počítač, který chcete replikovat, bude nutné nainstalovat službu Mobility. Můžete ji nainstalovat na každý počítač ručně, nebo pomocí nabízené instalace z procesového serveru.

**Obr. 1: Komponenty pro replikaci z VMware do Azure**

![Komponenty](./media/site-recovery-components/arch-enhanced.png)

### <a name="replication-process"></a>Proces replikace

1. Nastavíte nasazení, včetně komponent Azure, a trezor služby Recovery Services. V trezoru zadáte zdroj a cíl replikace, nastavíte konfigurační server, přidáte servery VMware, vytvoříte zásady replikace, nasadíte službu Mobility, povolíte replikaci a spustíte test převzetí služby.
2.  Počítače se začnou replikovat v souladu se zásadami replikace a počáteční kopie dat se replikuje do úložiště Azure.
4. Replikace rozdílových dat do Azure začne po dokončení počáteční replikace. Sledované změny se pro jednotlivé počítače ukládají do souboru .hrl.
    - Počítače, které se replikují, komunikují s konfiguračním serverem kvůli správě replikace na příchozím portu HTTPS 443.
    - Replikující se počítače odesílají data do procesového serveru na příchozím portu HTTPS 9443 (nastavitelné).
    - Konfigurační server orchestruje správu replikace s Azure přes odchozí port HTTPS 443.
    - Procesový server přijímá data ze zdrojového počítače, optimalizuje je a šifruje, a pak je odesílá do úložiště Azure přes odchozí port 443.
    - Když povolíte konzistenci mezi více virtuálními počítači, budou spolu počítače v replikační skupině komunikovat na portu 20004. Konzistence více virtuálních počítačů znamená, že seskupíte víc virtuálních počítačů do replikační skupiny, v rámci které se sdílí body obnovení konzistentní vzhledem k selháním a konzistentní vzhledem k aplikacím, když dojde k převzetí služeb při selhání. To je užitečné, pokud je počítačích spuštěná stejná úloha a je třeba, aby zůstala konzistentní.
5. Provoz se přes internet replikuje do veřejných koncových bodů úložiště Azure. Alternativně můžete použít [veřejný partnerský vztah](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-circuit-peerings#public-peering) Azure ExpressRoute. Přenos replikačních dat přes síť site-to-site VPN z místního serveru do Azure není podporovaný.

**Obr. 2: Replikace z VMware do Azure**

![Rozšířené](./media/site-recovery-components/v2a-architecture-henry.png)

### <a name="failover-and-failback-process"></a>Proces převzetí služeb při selhání a navrácení služeb po obnovení

1. Převzetí služeb při selhání můžete provést pouze z místních virtuálních počítačů VMware a fyzických serverů do Azure. Plánované převzetí služeb není podporované.
2. Můžete převzít službu při selhání jednoho počítače nebo vytvořit [plány zotavení](site-recovery-create-recovery-plans.md) a orchestrovat převzetí služeb více počítačů.
3. V průběhu převzetí služby se v Azure vytvoří replika virtuálního počítače. Po potvrzení procesu převzetí služeb můžete začít používat úlohu na replikovaném virtuálním počítači Azure.
4. Až bude vaše místní lokalita opět dostupná, můžete službu navrátit. Nastavíte infrastrukturu navrácení služeb po obnovení, zahájíte replikaci počítače ze sekundární lokality na primární a spustíte neplánované převzetí služeb při selhání ze sekundární lokality. Jakmile toto navrácení služeb potvrdíte, budou data zpět v místní lokalitě a budete muset opět povolit replikaci do Azure. [Další informace](site-recovery-failback-azure-to-vmware.md)

Pro navrácení služby po obnovení existuje několik požadavků:

- **Navrácení služeb po obnovení (model fyzický-fyzický) není podporováno**: to znamená, že pokud převezmete služby po selhání v případě fyzických serverů na Azure a poté chcete provést navrácení služeb po obnovení, musíte provést navrácení služeb po obnovení virtuálního počítače VMware. Nelze navrátit služby po obnovení v případě fyzického serveru. Pro navrácení služeb po obnovení budete potřebovat virtuální počítač Azure, a pokud jste neprovedli nasazení konfiguračního serveru jako virtuálního počítače VMware, budete muset nastavit samostatný hlavní cílový server jako virtuální počítač VMware. To je potřeba proto, že hlavní cílový server pracuje a spojuje se s úložištěm VMware pro obnovení disků na virtuální počítač VMware.
- **Dočasný procesní server v Azure**: Pokud chcete po převzetí služeb při selhání tyto služby po obnovení vrátit, budete muset pro zpracování replikace z Azure nastavit virtuální počítač Azure nakonfigurovaný jako procesní server. Tento virtuální počítač je možné po navrácení služeb po obnovení odstranit.
- **Připojení k síti VPN:** Pro navrácení služeb po obnovení budete potřebovat připojení k síti VPN (nebo Azure ExpressRoute) nastavené ze sítě Azure k místní lokalitě.
- **Samostatný lokální hlavní cílový server:** Místní hlavní cílový server se stará o navrácení služeb po obnovení. Hlavní cílový server je ve výchozím nastavení nainstalován na serveru pro správu, ale pokud po obnovení vracíte větší objemy přenosů, měli byste pro tento účel nastavit samostatný místní hlavní cílový server.
- **Zásady navrácení služeb po obnovení:** Pro zpětnou replikaci do vaší místní lokality budete potřebovat zásady navrácení služeb. Ty se vytvoří automaticky při vytvoření zásad replikace.

**Obr. 3: Navrácení služeb po obnovení – VMware nebo fyzický server**

![Navrácení služeb po obnovení](./media/site-recovery-components/enhanced-failback.png)


## <a name="replicate-vmware-vmsphysical-servers-to-a-secondary-site"></a>Replikace virtuálních počítačů nebo fyzických serverů do sekundární lokality

### <a name="components"></a>Komponenty

**Komponenta** | **Podrobnosti**
--- | ---
**Azure** | Tento scénář nasadíte pomocí nástroje InMage Scout. K jeho získání budete potřebovat předplatné Azure.<br/><br/> Po vytvoření trezoru služby Recovery Services si stáhnete InMage Scout a nainstalujete nejnovější aktualizace pro nastavení nasazení.
**Procesový server** | Komponentu procesového serveru nasaďte v primární lokalitě. Bude se starat o ukládání do mezipaměti, kompresi a optimalizaci dat.<br/><br/> Také obstará nabízenou instalaci nástroje Unified Agent na počítače, které chcete chránit.
**VMware ESX/ESXi a server vCenter** |  Pro replikaci virtuálních počítačů VMware budete potřebovat infrastrukturu VMware.
**Virtuální počítače / fyzické servery** |  Na virtuální počítače VMware nebo fyzické servery s Windows/Linuxem, které chcete replikovat, nainstalujte Unified Agent.<br/><br/> Agent funguje jako zprostředkovatel komunikace mezi všemi součástmi.
**Konfigurační server** | Nainstalujte ho do sekundární lokality pro účely správy, konfigurace a monitorování vašeho nasazení – buď pomocí webu pro správu, nebo konzole vContinuum.
**Server vContinuum** | Instaluje se do stejné lokality jako konfigurační server.<br/><br/> Poskytuje konzoli pro správu a monitorování chráněného prostředí.
**Hlavní cílový server (sekundární lokalita)** | Na hlavní cílový server se ukládají replikovaná data. Přijímá data z procesního serveru, vytváří repliku počítače v sekundární lokalitě a ukládá body pro uchovávání dat.<br/><br/> Počet hlavních cílových serverů, které potřebujete, závisí na počtu počítačů, které chráníte.<br/><br/> Pokud chcete po obnovení vrátit služby do primární lokality, potřebujete hlavní cílový server i tam. Na tento server je třeba nainstalovat Unified Agent.

### <a name="replication-process"></a>Proces replikace

1. V každé lokalitě si nastavte servery představující jednotlivé součásti (konfigurační, procesní, hlavní cílový) a nainstalujte si Unified Agent na počítače, které chcete replikovat.
2. Po počáteční replikaci odesílá agent na každém počítači na procesní server rozdílové replikační změny.
3. Procesní server tato data optimalizuje a přenese je na hlavní cílový server v sekundární lokalitě. Konfigurační server spravuje replikační proces.

**Obr. 4: Replikace z VMware do VMware**

![Z VMware do VMware](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="replicate-hyper-v-vms-to-azure"></a>Replikace virtuálních počítačů Hyper-V do Azure


### <a name="components"></a>Komponenty

**Komponenta** | **Podrobnosti**
--- | ---
**Azure** | Pro Azure potřebujete účet Microsoft Azure, účet úložiště Azure a síť Azure.<br/><br/> Účty úložiště a sítě můžou být klasické účty nebo účty Resource Manageru.<br/><br/> Replikovaná data jsou uložena v účtu úložiště a virtuální počítače Azure se z replikovaných dat vytvoří, jakmile dojde k převzetí místních služeb při selhání.<br/><br/> Virtuální počítače Azure se připojí k virtuální síti Azure po svém vytvoření.
**Server VMM** | Pokud jsou vaši hostitelé Hyper-V umístěni v cloudech VMM, budete potřebovat logické a VM sítě, aby bylo možné nakonfigurovat [mapování sítě](site-recovery-network-mapping.md). Síť virtuálních počítačů musí být propojena na logickou síť, která je přidružena ke cloudu.
**Hostitel Hyper-V** | Potřebujete jeden nebo více hostitelských serverů Hyper-V.
**Virtuální počítače Hyper-V** | Na hostitelském serveru Hyper-V potřebujete jeden nebo několik virtuálních počítačů. Zprostředkovatel, který běží na hostiteli Hyper-V koordinuje a orchestruje replikaci pomocí služby Site Recovery přes internet. Agent zpracovává replikovaná data dat přes HTTPS 443. Komunikace z poskytovatele i agenta je zabezpečená a šifrovaná. Šifrují se rovněž replikovaná data v úložišti Azure.


## <a name="replication-process"></a>Proces replikace

1. Nastavíte komponenty Azure. Doporučujeme připravit účty úložiště a sítě ještě než začnete s nasazením Site Recovery.
2. Vytvoříte trezor služby Replication Services pro Site Recovery a nakonfigurujete jeho nastavení, včetně těchto:
    - Pokud nespravujete hostitele Hyper-V v cloudu VMM, vytvoříte kontejner lokality Hyper-V a přidáte do něj hostitele Hyper-V.
    - Zdroj a cíl replikace Pokud své hostitele Hyper-V spravujete pomocí VMM, je zdrojem cloud VMM. Pokud ne, je zdrojem lokalita Hyper-V.
    - Instalace zprostředkovatele Azure Site Recovery a agenta Microsoft Azure Recovery Services Pokud máte server VMM, zprostředkovatel bude nainstalován na něj a agent na každého hostitele Hyper-V. Pokud VMM nemáte, nainstalujte zprostředkovatele i agenta na každého hostitele.
    - Vytvoříte zásady replikace pro lokalitu Hyper-V nebo pro cloud VMM. Zásady se použijí pro všechny virtuální počítače na hostitelích v lokalitě nebo v cloudu.
    - Povolíte replikaci pro virtuální počítače Hyper-V. Počáteční replikace proběhne podle nastavených zásad replikace.
4. Pak budou sledovány změny dat a bude probíhat rozdílová replikace do Azure. Sledované změny se pro jednotlivé položky ukládají do souboru .hrl.
5. Nakonec otestujete převzetí služeb při selhání, abyste měli jistotu, že všechno funguje.

### <a name="failover-and-failback-process"></a>Proces převzetí služeb při selhání a navrácení služeb po obnovení

1. Můžete spustit plánované nebo neplánované [převzetí služeb při selhání](site-recovery-failover.md) z místních virtuálních počítačů Hyper-V do Azure. Pokud spustíte plánovanou operaci, dojde k ukončení zdrojových virtuálních počítačů, aby se zcela předešlo možné ztrátě dat.
2. Můžete převzít službu při selhání jednoho počítače nebo vytvořit [plány zotavení](site-recovery-create-recovery-plans.md) a orchestrovat převzetí služeb více počítačů.
4. Po převzetí služeb při selhání byste v Azure měli vidět vytvořené replikované virtuální počítače. Virtuálnímu počítači můžete přiřadit veřejnou IP adresu, když je to třeba.
5. Po potvrzení procesu převzetí služeb můžete začít používat úlohu na replikovaném virtuálním počítači Azure.
6. Až bude vaše místní lokalita opět dostupná, můžete službu navrátit. Zahájíte plánované převzetí služeb při selhání z Azure do primární lokality. Pro plánované převzetí služeb při selhání můžete vybrat navrácení služeb po obnovení do stejného virtuálního počítače nebo do alternativního umístění a synchronizovat změny mezi Azure a místním systémem, aby nedošlo ke ztrátě dat. Jakmile budou místní virtuální počítače vytvořené, můžete převzetí služeb při selhání potvrdit.

**Obr. 5: Replikace z lokality Hyper-V do Azure**

![Komponenty](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Obr. 6: Replikace z Hyper-V v cloudech VMM do Azure**

![Komponenty](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)



## <a name="replicate-hyper-v-vms-to-a-secondary-site"></a>Replikace virtuálních počítačů Hyper-V do sekundární lokality

### <a name="components"></a>Komponenty

**Komponenta** | **Podrobnosti**
--- | ---
**Účet Azure** | Potřebujete účet Microsoft Azure.
**Server VMM** | Doporučujeme jeden server VMM v primární lokalitě a další v sekundární lokalitě, připojené k internetu.<br/><br/> Každý server musí mít alespoň jeden privátní cloud VMM s nastavenou profilovou sadou schopností Hyper-V.<br/><br/> Nainstalujete zprostředkovatele Azure Site Recovery na server VMM. Zprostředkovatel koordinuje a orchestruje replikaci pomocí služby Site Recovery přes internet. Komunikace mezi poskytovatelem a Azure je zabezpečená a šifrovaná.
**Server Hyper-V** |  Potřebujete jeden nebo několik hostitelských serverů Hyper-V v primárních a sekundárních cloudech VMM. Servery by měly být připojené k internetu.<br/><br/> Data se replikují mezi primárním a sekundárním hostitelským serverem Hyper-V přes síť LAN nebo VPN na základě protokolu Kerberos nebo ověření certifikátem.  
**Zdrojové počítače** | Zdrojový hostitelský server Hyper-V musí mít přinejmenším jeden virtuální počítač, který chcete replikovat.

## <a name="replication-process"></a>Proces replikace

1. Připravíte účet Azure.
2. Vytvoříte trezor služby Replication Services pro Site Recovery a nakonfigurujete jeho nastavení, včetně těchto:

    - Zdroj a cíl replikace (primární a sekundární lokality)
    - Instalace zprostředkovatele Azure Site Recovery a agenta Microsoft Azure Recovery Services Zprostředkovatel bude nainstalován na servery VMM a agent na každého hostitele Hyper-V.
    - Vytvoříte zásady replikace pro zdrojový cloud VMM. Zásady se použijí pro všechny virtuální počítače na hostitelích v cloudu.
    - Povolíte replikaci pro virtuální počítače Hyper-V. Počáteční replikace proběhne podle nastavených zásad replikace.
4. Pak budou sledovány změny dat a bude probíhat rozdílová replikace. Sledované změny se pro jednotlivé položky ukládají do souboru .hrl.
5. Nakonec otestujete převzetí služeb při selhání, abyste měli jistotu, že všechno funguje.

**Obrázek 7: Replikace z VMM do VMM**

![Z lokálního prostředí do lokálního prostředí](./media/site-recovery-components/arch-onprem-onprem.png)

### <a name="failover-and-failback-process"></a>Proces převzetí služeb při selhání a navrácení služeb po obnovení

1. Můžete spustit plánované nebo neplánované [převzetí služeb při selhání](site-recovery-failover.md) mezi místními lokalitami. Pokud spustíte plánovanou operaci, dojde k ukončení zdrojových virtuálních počítačů, aby se zcela předešlo možné ztrátě dat.
2. Můžete převzít službu při selhání jednoho počítače nebo vytvořit [plány zotavení](site-recovery-create-recovery-plans.md) a orchestrovat převzetí služeb více počítačů.
4. Pokud proběhlo neplánované převzetí služeb při selhání sekundární lokalitou, po provedení převzetí služeb nebudou počítače v sekundární lokalitě chráněné pomocí replikace. Pokud jste provedli plánované převzetí služeb při selhání, počítače v sekundární lokalitě chráněné budou.
5. Po potvrzení převzetí služeb můžete začít používat úlohu na replikovaném virtuálním počítači.
6. Až bude primární lokalita opět dostupná, zahájíte zpětnou replikaci ze sekundární lokality do primární. Po zpětné replikaci budou virtuální počítače v chráněném stavu, ale sekundární datové centrum bude stále aktivní lokalitou.
7. Chcete-li z primární lokality opět udělat aktivní, zahajte plánované převzetí služeb ze sekundární lokality do primární, následované další zpětnou replikací.


### <a name="hyper-v-replication-workflow"></a>Pracovní postup replikace Hyper-V

**Fáze pracovního postupu** | **Akce**
--- | ---
1. **Povolení ochrany** | Po povolení ochrany pro virtuální počítač Hyper-V se spustí úloha **Povolení ochrany**, která zkontroluje, jestli počítač splňuje požadavky. Úloha vyvolá dvě metody:<br/><br/> [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), která připraví replikaci s konfigurovanými nastaveními.<br/><br/> [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx), která zahájí úplnou replikaci virtuálního počítače.
2. **Počáteční replikace** |  Vytvoří se snímek virtuálního počítače a postupně se jeden po druhém replikují virtuální pevné disky, dokud se všechny nezkopírují do sekundární lokality.<br/><br/> Čas potřebný k dokončení závisí na velikosti virtuálního počítače, šířce pásma sítě a metodě počáteční replikace.<br/><br/> Pokud dojde ke změnám na disku v době, kdy probíhá počáteční replikace, Hyper-V Replica Replication Tracker je zaznamená jako protokoly replikace technologie Hyper-V (.hrl), které jsou umístěny ve stejné složce jako disky.<br/><br/> Každý disk má přidružený soubor .hrl, který se odešle do sekundárního úložiště.<br/><br/> Soubory snímků a protokolů spotřebovávají prostředky disku v době, kdy probíhá počáteční replikace. Po dokončení počáteční replikace se snímek virtuálního počítače odstraní a rozdílové změny na disku v protokolu se synchronizují a sloučí.
3. **Dokončení ochrany** | Po dokončení počáteční replikace nakonfiguruje úloha **Dokončení ochrany** síťová a další postreplikační nastavení tak, aby byl virtuální počítač chráněn.<br/><br/> Pokud replikujete do Azure, možná bude nutné upravit nastavení pro virtuální počítač tak, aby byl připraven k převzetí služeb při selhání.<br/><br/> V tomto bodě můžete spustit test převzetí služeb při selhání a zkontrolovat, zda vše funguje podle očekávání.
4. **Replikace** | Po počáteční replikaci se zahájí rozdílová synchronizace, a to v souladu s nastavením replikace.<br/><br/> **Selhání replikace:** Pokud rozdílová replikace selže a úplná replikace by byla náročná, pokud jde o šířku pásma nebo čas, pak dojde k resynchronizaci. Pokud například soubory .hrl dosáhnou 50 % velikosti disku, pak se u virtuálního počítače stanoví nutnost resynchronizace. Resynchronizace minimalizuje množství dat odesílaných v rámci výpočtů kontrolních součtů zdrojových a cílových virtuálních počítačů tím, že se posílají pouze rozdíly. Po dokončení resynchronizace bude pokračovat rozdílová replikace. Ve výchozím nastavení je automatické spuštění resynchronizace naplánováno na mimopracovní dobu, ale můžete ji u virtuálního počítače spustit i ručně.<br/><br/> **Chyba replikace:** Pokud dojde k chybě replikace, je předdefinován opakovaný pokus. Jde-li o neopravitelnou chybu, například související s ověřením nebo autorizací, nebo je-li v neplatném stavu počítač představující repliku, k opakovanému pokusu nedojde. Pokud se jedná o opravitelnou chybu, jako je chyba sítě nebo nedostatek volného místa na disku či paměti, pak se pokusy opakují, ale postupně se mezi nimi prodlužuje interval (proběhnou po 1, 2, 4, 8 a 10 minutách a pak každých 30 minut).
5. **Plánované/neplánované převzetí služeb při selhání** | Podle potřeby můžete spustit plánované, nebo neplánované převzetí služeb při selhání.<br/><br/> Pokud spustíte plánovanou operaci, dojde k ukončení zdrojových virtuálních počítačů, aby se zcela předešlo možné ztrátě dat.<br/><br/> Virtuální počítače představující repliky přejdou po svém vytvoření do stavu čekání na potvrzení. Převzetí služeb při selhání je třeba dokončit jeho potvrzením.<br/><br/> Jakmile bude primární lokalita funkční, můžete navrátit služby zpět do primární lokality.


**Obr. 8: Pracovní postup Hyper-V**

![pracovní postup](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Další kroky

[Příprava nasazení](site-recovery-best-practices.md)



<!--HONumber=Feb17_HO3-->


