<properties
    pageTitle="Co je Site Recovery? | Microsoft Azure"
    description="Poskytuje přehled služby Azure Site Recovery a shrnuje scénáře nasazení."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/13/2016"
    ms.author="raynew"/>


#  <a name="what-is-site-recovery?"></a>Co je Site Recovery?

Vítejte v Azure Site Recovery! Tento článek poskytuje rychlý přehled služby Site Recovery a jejího přínosu pro vaši firmu.

Vaše organizace potřebuje strategii pro provozní kontinuitu a zotavení po havárii (BCDR), která zajistí uchování aplikací, úloh a dat zabezpečených a dostupných během plánovaných a neplánovaných výpadků a co nejrychlejší obnovení normální pracovních podmínek. Site Recovery je služba Azure, která přispívá k této strategii.

Site Recovery orchestruje replikaci úloh spuštěných na místních fyzických serverech a virtuálních počítačích. Servery a virtuální počítače můžete replikovat z primárního datového centra do cloudu (Azure) nebo do sekundárního datového centra. Pokud dojde k výpadkům v primární lokalitě, sekundární lokalita převezme služby při selhání, aby aplikace a úlohy zůstaly dostupné. Až se obnoví normální provozní podmínky, vrátíte služby po obnovení zpět do primárního umístění.

## <a name="site-recovery-in-the-azure-portal"></a>Site Recovery na portálu Azure

V Azure existují dva různé [modely nasazení](../resource-manager-deployment-model.md) pro vytváření prostředků a práci s nimi. Model Azure Resource Manager a klasický model správy služeb. Azure má také dva portály – [portál Azure Classic](https://manage.windowsazure.com/), který podporuje klasický model nasazení, a [Azure Portal](https://portal.azure.com) s podporou pro oba modely nasazení.

- Site Recovery je k dispozici na klasickém portálu a na portálu Azure.
- Na portálu Azure Classic můžete podporovat Site Recovery s klasickým modelem správy služeb.
- Na webu Azure Portal můžete podporovat klasický model nasazení nebo model nasazení Resource Manager. 

Informace v tomto článku se vztahují na klasické nasazení i na nasazení portálu Azure. Rozdíly jsou popsány v případě potřeby.


## <a name="why-deploy-site-recovery?"></a>Proč nasadit Site Recovery?

Zde je výčet, co může Site Recovery poskytnout vašemu podniku:

- **Zjednodušení BCDR** – Můžete řídit replikaci, převzetí služeb při selhání a obnovení více úloh z jednoho místa na webu Azure Portal. Site Recovery orchestruje replikaci a převzetí služeb při selhání, ale nezachycuje data aplikací ani o nich nemá žádné informace.
- **Flexibilní replikace** – Pomocí Site Recovery můžete replikovat úlohy běžící v podporovaných virtuálních počítačích Hyper-V a VMware a na fyzických serverech s Windows nebo Linuxem.
- **Provádění snadného testování replikace** – Site Recovery poskytuje testovací převzetí služeb při selhání, což umožňuje nácvik zotavení po havárii, aniž by to mělo dopad na produkční prostředí.
- **Převzetí služeb při selhání a obnovení** – Pro očekávané výpadky je možné spouštět plánovaná převzetí služeb při selhání bez ztráty dat. V případě neočekávaných havárií pak mohou proběhnout neplánovaná převzetí služeb při selhání s minimálními ztrátami dat (v závislosti na četnosti replikací). Po obnovení můžete služby, které byly převzaty při selhání, navrátit na primární lokality. Site Recovery poskytuje plány obnovení, které mohou obsahovat skripty a sešity Azure Automation, což vám umožní přizpůsobit si přebírání služeb při selhání a obnovování vícevrstvých aplikací.
- **Eliminace sekundárního datového centra** – Úlohy můžete replikovat do Azure namísto sekundární lokality. Tím se eliminují náklady a složitost spojené s udržováním sekundárního datového centra. Replikovaná data se ukládají ve službě Azure Storage s veškerou odolností, kterou tato služba nabízí. V případě, že dojde k převzetí služeb při selhání, se vytvoří virtuální počítače s replikovanými daty.
- **Integrace s existujícími technologiemi BCDR** – Site Recovery se integruje s dalšími funkcemi BCDR. Site Recovery je například možné použít k ochraně back-endu systému SQL Server u firemních úloh, včetně nativní podpory pro SQL Server AlwaysOn, pokud jde o přebírání služeb skupin dostupnosti při selhání.

## <a name="what-can-i-replicate?"></a>Co mohu replikovat?

Zde naleznete souhrn toho, co dokáže nástroj Site Recovery replikovat.

![Z lokálního prostředí do lokálního prostředí](./media/site-recovery-overview/asr-overview-graphic.png)

**REPLIKACE** | **REPLIKACE DO** 
---|---
Úlohy běžící na místních virtuálních počítačích VMware | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Sekundární lokalita](site-recovery-vmware-to-vmware.md)
Úlohy běžící na místních virtuálních počítačích Hyper-V spravovaných v cloudech VMM  | [Azure](site-recovery-vmm-to-azure.md)<br/><br/> [Sekundární lokalita](site-recovery-vmm-to-vmm.md) 
Úlohy běžící na místních virtuálních počítačích Hyper-V používajících úložiště pro sítě SAN a spravovaných v cloudech VMM|  [Sekundární lokalita](site-recovery-vmm-san.md)
Úlohy běžící na místních virtuálních počítačích Hyper-V bez nástroje VMM | [Azure](site-recovery-hyper-v-site-to-azure.md)
Úlohy běžící na místních fyzických serverech s Windows nebo Linuxem | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Sekundární lokalita](site-recovery-vmware-to-vmware.md)


## <a name="what-workloads-can-i-protect?"></a>Jaké úlohy mohu ochránit?

Site Recovery povoluje strategii BCDR se sledováním aplikací, aby úlohy a aplikace neustále běžely, i když dojde k výpadku. Site Recovery poskytuje následující:

- **Snímky konzistentní vzhledem k aplikacím** – Počítače se replikují pomocí snímků konzistentních vzhledem k aplikacím s jednou nebo více vrstvami. Kromě zachytávání dat na disku zachycují snímky konzistentní vzhledem k aplikacím i veškerá data v paměti a všechny probíhající transakce.
- **Téměř synchronní replikace** – Site Recovery umožňuje frekvenci replikací pouhých 30 sekund pro Hyper-V a neustálou replikaci pro VMware.
- **Flexibilní plány obnovení** – Můžete vytvářet a upravovat plány obnovení pomocí externích skriptů a ručních akcí. Integrace s runbooky služby Azure Automation umožňuje obnovit celý zásobník aplikací jediným kliknutím.
- **Integrace s SQL Server AlwaysOn** – Správa převzetí služeb skupin dostupnosti při selhání v plánech obnovení Site Recovery
- **Knihovna Automation** – Bohatá knihovna Azure Automation obsahující předpřipravené skripty specifické pro aplikace, které je možné stáhnout a integrovat se Site Recovery
- **Jednoduchá správa sítě** – Pokročilá správa sítě v Site Recovery a Azure, která zjednodušuje požadavky aplikací na síť, včetně rezervace IP adres, konfigurace nástrojů pro vyrovnávání zatížení a integrace služby Azure Traffic Manager pro efektivní přepínání sítí.


## <a name="next-steps"></a>Další kroky

- Více informací o Site Recovery najdete v tématu [Jaké úlohy je možné chránit pomocí Site Recovery?](site-recovery-workload.md).
- Další informace o architektuře Site Recovery najdete v článku [Jak funguje Site Recovery?](site-recovery-components.md).
 



<!--HONumber=Oct16_HO3-->


