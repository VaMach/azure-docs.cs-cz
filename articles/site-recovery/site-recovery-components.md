<properties
    pageTitle="Jak funguje Site Recovery? | Microsoft Azure"
    description="Tento článek přináší přehled architektury Site Recovery."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/13/2016"
    ms.author="raynew"/>


# <a name="how-does-azure-site-recovery-work?"></a>Jak funguje Azure Site Recovery?

Tento článek vám pomůže porozumět architektuře služby Azure Site Recovery a komponentám, které jí umožňují fungovat.

Jakékoli dotazy nebo připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Přehled

Potřebujete strategii pro provozní kontinuitu a zotavení po havárii (BCDR), která zajistí udržování úloh a dat v běžícím a dostupném stavu během plánovaných a neplánovaných výpadků a co nejrychlejší obnovení normálních pracovních podmínek. 

Site Recovery je služba Azure, která se na vaši strategii BCDR podílí. Orchestruje replikaci fyzických lokálních serverů a virtuálních počítačů do cloudu (Azure) nebo do sekundárního datacentra. Pokud dojde k výpadkům ve vašem primárním umístění, předáte služby při selhání do sekundárního umístění, aby aplikace a úlohy zůstaly dostupné. Až se obnoví normální provozní podmínky, vrátíte služby po obnovení zpět do primárního umístění. Přehled služby najdete v tématu [Co je Site Recovery](site-recovery-overview.md).

## <a name="site-recovery-in-the-azure-portal"></a>Site Recovery na portálu Azure

Azure má dva různé [modely nasazení](../resource-manager-deployment-model.md) pro vytváření prostředků a práci s nimi: Azure Resource Manager a klasický model správy služeb. Azure má také dva portály – [portál Azure Classic](https://manage.windowsazure.com/), který podporuje klasický model nasazení, a [portál Azure](https://portal.azure.com) s podporou pro oba modely nasazení.

- Site Recovery je k dispozici na klasickém portálu a na portálu Azure.
- Na portálu Azure Classic je možné podporovat službu Site Recovery s použitím klasického modelu správy služeb.
- Na portálu Azure Portal lze podporovat model nasazení Classic nebo model nasazení prostředků. 

Informace v tomto článku se vztahují na klasické nasazení i na nasazení portálu Azure. Tato tabulka shrnuje rozdíly.

**Replikace** | **Azure Portal** | **Portál Classic**
--- | --- | ---
**Replikace virtuálních počítačů VMware do Azure** | Zjednodušený proces nasazení.<br/><br/> Převzetí služeb virtuálních počítačů při selhání klasickým úložištěm nebo úložištěm na bázi Resource Manageru.<br/><br/> Replikace do klasického úložiště nebo úložiště na bázi Resource Manageru.<br/><br/> Připojení virtuálních počítačů Azure po převzetí služeb při selhání pomocí klasické sítě nebo sítě založené na Resource Manageru.<br/><br/> Použití úložiště LRS nebo GRS. | Převzetí služeb při selhání jen klasickým úložištěm.<br/><br/> Připojení virtuálních počítačů po převzetí služeb při selhání jen pomocí klasických sítí.<br/><br/> Použití úložiště GRS.
**Replikace virtuálních počítačů Hyper-V (bez VMM) do Azure** | Zjednodušený proces nasazení.<br/><br/> Převzetí služeb virtuálních počítačů při selhání klasickým úložištěm nebo úložištěm na bázi Resource Manageru.<br/><br/> Replikace do klasického úložiště nebo úložiště na bázi Resource Manageru.<br/><br/> Připojení virtuálních počítačů Azure po převzetí služeb při selhání pomocí klasické sítě nebo sítě založené na Resource Manageru.
**Replikace virtuálních počítačů Hyper-V (s VMM) do Azure** | Zjednodušený proces nasazení.<br/><br/> Převzetí služeb virtuálních počítačů při selhání klasickým úložištěm nebo úložištěm na bázi Resource Manageru.<br/><br/> Replikace do klasického úložiště nebo úložiště na bázi Resource Manageru.<br/><br/> Připojení virtuálních počítačů Azure po převzetí služeb při selhání pomocí klasické sítě nebo sítě založené na Resource Manageru.<br/><br/> Nutnost nastavit mapování sítě. | Převzetí služeb při selhání jen klasickým úložištěm.<br/><br/> Připojení virtuálních počítačů po převzetí služeb při selhání jen pomocí klasických sítí.
**Replikace virtuálních počítačů Hyper-V (s VMM) do sekundární lokality** | Zjednodušený proces nasazení.<br/><br/> Převzetí služeb virtuálních počítačů při selhání klasickým úložištěm nebo úložištěm na bázi Resource Manageru.<br/><br/> Replikace do klasického úložiště nebo úložiště na bázi Resource Manageru.<br/><br/> Připojení virtuálních počítačů Azure po převzetí služeb při selhání pomocí klasické sítě nebo sítě založené na Resource Manageru.<br/><br/> Nutnost nastavit mapování sítě. | Převzetí služeb při selhání jen klasickým úložištěm.<br/><br/> Připojení virtuálních počítačů po převzetí služeb při selhání jen pomocí klasických sítí.<br/><br/> Možnost nastavit mapování úložiště. <br/><br/> Replikace SAN se nepodporuje.



## <a name="deployment-scenarios"></a>Scénáře nasazení

Site Recovery je možné nasadit pro orchestraci replikace v řadě scénářů:

- **Replikace virtuálních počítačů VMware:** Lokální virtuální počítače VMware můžete replikovat do úložiště Azure nebo sekundárního datacentra.
- **Replikace fyzických počítačů:** Fyzické počítače s Windows nebo Linuxem můžete replikovat do úložiště Azure nebo sekundárního datacentra. Proces pro replikování fyzických počítačů je téměř stejný jako proces pro replikaci virtuálních počítačů VMware.
- **Replikace virtuálních počítačů Hyper-V:** Pokud jsou vaši hostitelé Hyper-V spravováni v cloudech se System Center VMM, můžete virtuální počítače replikovat do úložiště Azure nebo do sekundárního datacentra. Pokud se hostitelé nespravují přes VMM, můžete replikovat jen do Azure. Virtuální počítače Hyper-V nespravované přes VMM můžete replikovat do úložiště Azure.
- **Migrace virtuálních počítačů:** Pomocí Site Recovery můžete [migrovat virtuální počítače Azure IaaS](site-recovery-migrate-azure-to-azure.md) mezi regiony nebo [migrovat instance AWS Windows](site-recovery-migrate-aws-to-azure.md) na virtuální počítače Azure IaaS. Úplná replikace se aktuálně nepodporuje. Můžete replikovat v rámci migrace (převzetí služeb při selhání), ale nemůžete vrátit služby po obnovení. 

Site Recovery může replikovat většinu aplikací běžících na těchto virtuálních počítačích a fyzických serverech. Úplný souhrn podporovaných aplikací naleznete v tématu [Jaké úlohy je možné chránit pomocí Azure Site Recovery?](site-recovery-workload.md).


## <a name="replicate-vmware-virtual-machines-to-azure"></a>Replikace virtuálních počítačů VMware do Azure

![Rozšířené](./media/site-recovery-components/arch-enhanced.png)

**Komponenta** | **Podrobnosti**
--- | ---
**Azure** | **Účet:** Potřebujete účet Azure.<br/><br/> **Úložiště:** K ukládání replikovaných dat potřebujete účet úložiště Azure. Můžete použít klasický účet nebo účet úložiště Resource Manageru. Při nasazení na portálu Azure může být účet LRS nebo GRS. Replikovaná data jsou uložena v úložišti Azure a virtuální počítače Azure se spouštějí, když je třeba převzít služby při selhání.<br/><br/> **Síť:** Potřebujete virtuální síť Azure, ke které se budou virtuální počítače Azure připojovat, když dojde k jejich vytvoření při převzetí služeb při selhání. Na portálu Azure Portal je možné sítě vytvářet v klasickém modelu správy služeb nebo v modelu Resource Manageru.
**Lokální konfigurační server** |  Potřebujete lokální počítač se systémem Windows Server 2012 R2, na kterém poběží konfigurační server a další součásti Site Recovery.<br/><br/> Mělo by jít o vysoce dostupný virtuální počítač VMware.<br/><br/> Součásti na serveru zahrnují:<br/><br/> **Konfigurační server:** Koordinuje komunikaci mezi místním prostředím a Azure a spravuje replikaci a obnovení dat.<br/><br/> **Procesní server:** Funguje jako replikační brána. Přijímá replikační data z chráněných zdrojových počítačů, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odesílá je do úložiště Azure. Rovněž se stará o nabízenou instalaci služby Mobility na chráněné počítače a provádí automatické zjišťování virtuálních počítačů VMware. Jak vaše nasazení poroste, můžete přidávat další samostatné vyhrazené procesní servery pro zpracování zvyšujícího se objemu replikačních přenosů.<br/><br/> **Hlavní cílový server:** Zpracovává replikační data během navracení služeb z Azure po obnovení.
**Lokální virtualizační servery** | Jeden nebo více hostitelů vSphere. Doporučujeme také server vCenter pro správu hostitelů.
**Virtuální počítače k replikaci** | Na každý virtuální počítač VMware, který chcete replikovat do Azure, bude nutné nainstalovat komponentu služby pro mobilitu. Tato služba zaznamenává datové zápisy na počítači a předává je na procesní server. Tuto součást můžete nainstalovat ručně nebo ji může automaticky (v režimu nabízení) nainstalovat procesní server, když aktivujete replikaci pro počítač.

- [Další informace](site-recovery-vmware-to-azure.md#azure-prerequisites) o požadavcích pro nasazení portálu Azure.
- [Další informace](site-recovery-failback-azure-to-vmware.md) o navrácení služeb po obnovení na portálu Azure.


## <a name="replicate-physical-servers-to-azure"></a>Replikace fyzických serverů do Azure

![Rozšířené](./media/site-recovery-components/arch-enhanced.png)

**Komponenta** | **Podrobnosti**
--- | ---
**Azure** | **Účet:** Potřebujete účet Azure.<br/><br/> **Úložiště:** K ukládání replikovaných dat potřebujete účet úložiště Azure. Můžete použít klasický účet nebo účet úložiště Resource Manageru. Při nasazení na portálu Azure může být účet LRS nebo GRS. Replikovaná data jsou uložena v úložišti Azure a virtuální počítače Azure se spouštějí, když je třeba převzít služby při selhání.<br/><br/> **Síť:** Potřebujete virtuální síť Azure, ke které se budou virtuální počítače Azure připojovat, když dojde k jejich vytvoření při převzetí služeb při selhání. Na portálu Azure Portal je možné sítě vytvářet v klasickém modelu správy služeb nebo v modelu Resource Manageru.
**Lokální konfigurační server** |  Potřebujete lokální počítač se systémem Windows Server 2012 R2, na kterém poběží konfigurační server a další součásti Site Recovery.<br/><br/> Server může být virtuální nebo fyzický.<br/><br/> Součásti na serveru zahrnují:<br/><br/> **Konfigurační server:** Koordinuje komunikaci mezi místním prostředím a Azure a spravuje replikaci a obnovení dat.<br/><br/> **Procesní server:** Funguje jako replikační brána. Přijímá replikační data z chráněných zdrojových počítačů, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odesílá je do úložiště Azure. Rovněž se stará o nabízenou instalaci služby Mobility na chráněné počítače a provádí automatické zjišťování virtuálních počítačů VMware. Jak vaše nasazení poroste, můžete přidávat další samostatné vyhrazené procesní servery pro zpracování zvyšujícího se objemu replikačních přenosů.<br/><br/> **Hlavní cílový server:** Zpracovává replikační data během navracení služeb z Azure po obnovení.
**Počítače k replikaci** | Na každý fyzický počítač s Windows nebo Linuxem, který chcete replikovat do Azure, bude nutné nainstalovat komponentu služby pro mobilitu. Tato služba zaznamenává datové zápisy na počítači a předává je na procesní server. Tuto součást můžete nainstalovat ručně nebo ji může automaticky (v režimu nabízení) nainstalovat procesní server, když aktivujete replikaci pro počítač.
**Navrácení služeb po obnovení** | Navrácení služeb po obnovení mezi fyzickými počítači se nepodporuje. To znamená, že pokud služby při selhání fyzických serverů převezmete do Azure a poté chcete provést navrácení služeb po obnovení, musíte je vrátit na virtuální počítač VMware. Nelze navrátit služby po obnovení v případě fyzického serveru. Pro navrácení služeb po obnovení budete potřebovat virtuální počítač Azure, a pokud jste neprovedli nasazení konfiguračního serveru jako virtuálního počítače VMware, budete muset nastavit samostatný hlavní cílový server jako virtuální počítač VMware. To je potřeba proto, že hlavní cílový server pracuje a spojuje se s úložištěm VMware pro obnovení disků na virtuální počítač VMware.


- [Další informace](site-recovery-vmware-to-azure.md#azure-prerequisites) o požadavcích pro nasazení portálu Azure.
- [Další informace](site-recovery-failback-azure-to-vmware.md) o navrácení služeb po obnovení na portálu Azure.


## <a name="replicate-hyper-v-vms-not-managed-by-vmm-to-azure"></a>Replikace virtuálních počítačů Hyper-V nespravovaných přes VMM do Azure

![Z lokality Hyper-V do Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Komponenta** | **Podrobnosti**
--- | ---
**Azure** | **Účet:** Potřebujete účet Azure.<br/><br/> **Úložiště:** K ukládání replikovaných dat potřebujete účet úložiště Azure. Můžete použít klasický účet nebo účet úložiště Resource Manageru. Účet musí být GRS. Replikovaná data jsou uložena v úložišti Azure a virtuální počítače Azure se spouštějí, když je třeba převzít služby při selhání.<br/><br/> **Síť:** Potřebujete virtuální síť Azure, ke které se budou virtuální počítače Azure připojovat, když dojde k jejich vytvoření při převzetí služeb při selhání.
**Hostitelé Hyper-V a virtuální počítače** | Jeden nebo více hostitelů Hyper-V, které spouští jeden nebo více virtuálních počítačů.<br/><br/> Na každém hostiteli se během nasazení nainstaluje zprostředkovatel Site Recovery a agent Recovery Services.

- [Další informace](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites) o požadavcích pro nasazení portálu Azure.
- [Další informace](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites) o požadavcích pro nasazení portálu Classic.



## <a name="replicate-hyper-v-vms-managed-by-vmm-to-azure"></a>Replikace virtuálních počítačů Hyper-V spravovaných přes VMM do Azure


![Z VMM do Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

**Komponenta** | **Podrobnosti**
--- | ---
**Azure** | **Účet:** Potřebujete účet Azure.<br/><br/> **Úložiště:** K ukládání replikovaných dat potřebujete účet úložiště Azure. Můžete použít klasický účet nebo účet úložiště Resource Manageru. Účet musí být GRS. Replikovaná data jsou uložena v úložišti Azure a virtuální počítače Azure se spouštějí, když je třeba převzít služby při selhání.<br/><br/> **Síť:** Potřebujete virtuální síť Azure, ke které se budou virtuální počítače Azure připojovat, když dojde k jejich vytvoření při převzetí služeb při selhání.
**Server VMM** | Musíte mít nejméně jeden lokální server VMM s nejméně jedním privátním cloudem. Během nasazení se na každý server nainstaluje zprostředkovatel Site Recovery.
**Hostitelé Hyper-V a virtuální počítače** | Jeden nebo více hostitelů Hyper-V, které spouští jeden nebo více virtuálních počítačů.<br/><br/> Během nasazení se na každého hostitele nainstaluje agent Recovery Services.


- [Další informace](site-recovery-vmm-to-azure.md#azure-requirements) o požadavcích pro nasazení portálu Azure.
- [Další informace](site-recovery-vmm-to-azure-classic.md#before-you-start) o požadavcích pro nasazení portálu Classic.




## <a name="replicate-vmware-virtual-machines-or-physical-server-to-a-secondary-site"></a>Replikace virtuálních počítačů VMware nebo fyzického serveru do sekundární lokality

![Z VMware do VMware](./media/site-recovery-components/vmware-to-vmware.png)


**Komponenta** | **Podrobnosti**
--- | ---
**Azure** | **Účet:** Tento scénář nasadíte pomocí nástroje InMage Scout. K jeho získání budete potřebovat účet Azure.<br/><br/> Po vytvoření trezoru Site Recovery si stáhnete InMage Scout a nainstalujete nejnovější aktualizace pro nastavení nasazení.
**Primární lokalita** | **Procesní server:** Nastavte si v primární lokalitě součást, kterou představuje procesní server. Ten se bude starat o ukládání do mezipaměti, kompresi a optimalizaci dat. Také obstará nabízenou instalaci nástroje Unified Agent na počítače, které chcete chránit.<br/><br/> **VMware ESX/ESXi a vCenter:** Potřebujete hypervisor VMware ESX/ESXi a volitelně server VMware vCenter.<br/><br/> **Virtuální počítače** | Během nasazení se na počítače, které chcete chránit, nainstaluje Unified Agent. Agent funguje jako zprostředkovatel komunikace mezi všemi součástmi.
**Sekundární lokalita** | **Konfigurační server:** Jde o první součást, kterou nainstalujete, a to na sekundární lokalitu pro účely správy, konfigurace a monitorování vašeho nasazení – buď pomocí webu pro správu, nebo konzole vContinuum. V nasazení je pouze jediný konfigurační server a musí být nainstalován na počítači se systémem Windows Server 2012 R2.<br/><br/> **Server vContinuum (sekundární lokalita):** Je nainstalován ve stejném umístění jako konfigurační server. Poskytuje konzoli pro správu a monitorování chráněného prostředí. Ve výchozí instalaci je server vContinuum prvním hlavním cílovým serverem a je na něm nainstalován Unified Agent.<br/><br/> **Hlavní cílový server:** Hlavní cílový server uchovává replikovaná data. Přijímá data z procesního serveru, vytváří repliku počítače v sekundární lokalitě a ukládá body pro uchovávání dat. Počet hlavních cílových serverů, které potřebujete, závisí na počtu počítačů, které chráníte. Pokud chcete služby po obnovení vrátit do primární lokality, potřebujete hlavní cílový server i tam.


Pokud chcete replikovat virtuální počítače VMware nebo fyzické servery do sekundární lokality, stáhněte si nástroj InMage Scout, který je součástí předplatného Azure Site Recovery. Nástroj si můžete stáhnout z portálu Azure Portal nebo z portálu Azure Classic.

V každé lokalitě si nastavte servery představující jednotlivé součásti (konfigurační, procesní, hlavní cílový) a nainstalujte si Unified Agent na počítače, které chcete replikovat. Po počáteční replikaci odesílá agent na každém počítači na procesní server rozdílové replikační změny. Procesní server tato data optimalizuje a přenese je na hlavní cílový server v sekundární lokalitě. Konfigurační server spravuje replikační proces.


## <a name="replicate-hyper-v-vms-managed-by-vmm-to-a-secondary-site"></a>Replikace virtuálních počítačů Hyper-V spravovaných přes VMM do sekundární lokality

![Z lokálního prostředí do lokálního prostředí](./media/site-recovery-components/arch-onprem-onprem.png)

**Komponenta** | **Podrobnosti**
--- | ---
**Azure** | **Účet:** Potřebujete účet Azure.
**Server VMM** | Doporučujeme jeden server VMM v primární lokalitě a další v sekundární lokalitě. Každý server potřebuje nejméně jeden privátní cloud.<br/><br/> Během nasazení nainstalujete zprostředkovatele Azure Site Recovery na server VMM.
**Hostitelé Hyper-V a virtuální počítače** | Jeden nebo více hostitelů Hyper-V běžících v cloudech s VMM v primární a sekundární lokalitě<br/><br/> Každý hostitel musí mít nejméně jeden virtuální počítač k replikaci.<br/><br/>. Během nasazení se na každého hostitele nainstaluje agent Recovery Services.

- [Zde jsou další informace](site-recovery-vmm-to-vmm.md#azure-prerequisites) o požadavcích na nasazení do portálu Azure.
- [Zde jsou další informace](site-recovery-vmm-to-vmm-classic.md#before-you-start) o požadavcích na nasazení do portálu Azure Classic.


## <a name="replicate-hyper-v-vms-managed-by-vmm-to-a-secondary-site-using-san-replication"></a>Replikace virtuálních počítačů Hyper-V spravovaných přes VMM do sekundární lokality pomocí replikace sítě SAN

![Replikace sítě SAN](./media/site-recovery-components/arch-onprem-onprem-san.png)

Můžete replikovat virtuální počítače Hyper-V, spravované v cloudech VMM, do sekundární lokality pomocí replikace sítě SAN za použití portálu Classic. Tento scénář není na portálu Azure Portal aktuálně podporován.

**Komponenta** | **Podrobnosti**
--- | ---
**Azure** | **Účet:** Potřebujete účet Azure.
**Server VMM** | Doporučujeme jeden server VMM v primární lokalitě a další v sekundární lokalitě. Každý server potřebuje nejméně jeden privátní cloud.<br/><br/> Během nasazení nainstalujete zprostředkovatele Azure Site Recovery na server VMM.
**Síť SAN** | Podporované pole SAN spravované primárním serverem VMM.<br/><br/> Síť SAN by měl sdílet síťovou infrastrukturu s jiným polem SAN v sekundární lokalitě.
**Hostitelé Hyper-V a virtuální počítače** | Jeden nebo více hostitelů Hyper-V běžících v cloudech s VMM v primární a sekundární lokalitě<br/><br/> Každý hostitel musí mít nejméně jeden virtuální počítač k replikaci.<br/><br/>. Během nasazení se na každého hostitele nainstaluje agent Recovery Services.

V tomto scénáři nainstalujete během nasazování Site Recovery zprostředkovatele Azure Site Recovery na servery VMM. Zprostředkovatel koordinuje a orchestruje replikaci pomocí služby Site Recovery přes internet. Data se replikují mezi primárním a sekundárním polem úložiště s použitím synchronní replikace v síti SAN.

[Zde jsou další informace](site-recovery-vmm-san.md#before-you-start) o požadavcích na nasazení.

## <a name="hyper-v-protection-lifecycle"></a>Životní cyklus ochrany Hyper-V

Tento pracovní postup nastiňuje proces ochrany, replikace a převzetí služeb při selhání u virtuálních počítačů Hyper-V.

1. **Povolení ochrany:** Nastavíte trezor Site Recovery, nakonfigurujete nastavení replikace pro cloud VMM nebo web Hyper-V a povolíte ochranu pro virtuální počítače. Spustí se úloha s názvem **Povolení ochrany** a lze ji monitorovat na kartě **Úlohy**. V rámci úlohy se zkontroluje, zda počítač splňuje požadavky, a potom se vyvolá metoda [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), která nastaví replikaci do Azure na základě nastavení, které jste nakonfigurovali. Úloha **Povolení ochrany** také vyvolá metodu [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) pro inicializaci úplné replikace virtuálního počítače.
2. **Počáteční replikace:** Pořídí se snímek virtuálního počítače a virtuální pevné disky se replikují jeden po druhém, dokud se všechny nezkopírují do Azure nebo do sekundárního datacentra. Čas potřebný k dokončení závisí na velikosti virtuálního počítače, šířce pásma sítě a metodě počáteční replikace. Pokud dojde ke změnám na disku v době, kdy probíhá počáteční replikace, Hyper-V Replica Replication Tracker je zaznamená jako protokoly replikace Hyper-V (.hrl), které jsou umístěny ve stejné složce jako disky. Každý disk má přidružený soubor .hrl, který se odešle do sekundárního úložiště. Mějte na paměti, že soubory snímků a protokolů spotřebovávají prostředky disku v době, kdy probíhá počáteční replikace. Po dokončení počáteční replikace se snímek virtuálního počítače odstraní a rozdílové změny na disku v protokolu se synchronizují a sloučí.
3. **Dokončení ochrany:** Po počáteční replikaci nakonfiguruje úloha **Dokončení ochrany** síťová a další postreplikační nastavení tak, aby virtuální počítač byl chráněn. Pokud replikujete do Azure, možná bude nutné upravit nastavení pro virtuální počítač tak, aby byl připraven k převzetí služeb při selhání. V tomto bodě můžete spustit test převzetí služeb při selhání a zkontrolovat, zda vše funguje podle očekávání.
4. **Replikace:** Po počáteční replikaci se zahájí rozdílová synchronizace, a to v souladu s nastavením replikace.
    - **Selhání replikace:** Pokud rozdílová replikace selže a úplná replikace by byla náročná, pokud jde o šířku pásma nebo čas, pak dojde k resynchronizaci. Pokud například soubory .hrl dosáhnou 50 % velikosti disku, pak se u virtuálního počítače stanoví nutnost resynchronizace. Resynchronizace minimalizuje množství dat odesílaných v rámci výpočtů kontrolních součtů zdrojových a cílových virtuálních počítačů tím, že se posílají pouze rozdíly. Po dokončení synchronizace bude pokračovat rozdílová replikace. Ve výchozím nastavení je automatické spuštění resynchronizace naplánováno na mimopracovní dobu, ale můžete ji u virtuálního počítače spustit i ručně.
    - **Chyba replikace:** Pokud dojde k chybě replikace, je předdefinován opakovaný pokus. Jde-li o neopravitelnou chybu, například související s ověřením nebo autorizací, nebo je-li v neplatném stavu počítač představující repliku, k opakovanému pokusu nedojde. Pokud se jedná o opravitelnou chybu, jako je chyba sítě nebo nedostatek volného místa na disku či paměti, pak se pokusy opakují, ale postupně se mezi nimi prodlužuje interval (proběhnou po 1, 2, 4, 8 a 10 minutách a pak každých 30 minut).
4. **Plánované/neplánované převzetí služeb při selhání:** Podle potřeby můžete spustit plánované, nebo neplánované převzetí služeb při selhání. Pokud spustíte plánovanou operaci, dojde k ukončení zdrojových virtuálních počítačů, aby se zcela předešlo možné ztrátě dat. Virtuální počítače představující repliky přejdou po svém vytvoření do stavu čekání na potvrzení. Pro dokončení převzetí služeb při selhání je musíte potvrdit, což ale neplatí při replikaci sítě SAN, protože v takovém případě proběhne potvrzení automaticky. Po nastavení a spuštění primární lokality může proběhnout navrácení služeb po obnovení. Pokud jste nastavili replikaci do Azure, bude zpětná replikace automatická. V opačném případě ji spustíte ručně.


![pracovní postup](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Další kroky

[Příprava nasazení](site-recovery-best-practices.md)



<!--HONumber=Oct16_HO3-->


