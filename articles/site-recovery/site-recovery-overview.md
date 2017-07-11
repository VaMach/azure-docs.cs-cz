---
title: Co je Azure Site Recovery? | Dokumentace Microsoftu
description: "Poskytuje přehled služby Azure Site Recovery a shrnuje scénáře nasazení."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: e9b97b00-0c92-4970-ae92-5166a4d43b68
ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/25/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: a442d398cf5c54b00dc92ebcbe62597fb3ece30c
ms.contentlocale: cs-cz
ms.lasthandoff: 06/28/2017


---
<a id="what-is-site-recovery" class="xliff"></a>

# Co je Site Recovery?

Vítá vás služba Azure Site Recovery! Tento článek přináší stručný přehled služby.

<a id="business-continuity-and-disaster-recovery-bdcr-with-azure-recovery-services" class="xliff"></a>

## Kontinuita podnikových procesů a zotavení po havárii (BCDR) pomocí služby Azure Recovery Services

Jako organizace potřebujete vyřešit, jak zajistíte bezpečnost vašich dat a provoz aplikací a úloh v případě plánovaných a neplánovaných výpadků.

Služba Azure Recovery Services přispívá ke strategii BCDR:

- **Služba Site Recovery:** Site Recovery pomáhá zajistit kontinuitu podnikových procesů tím, že zajistí provoz aplikací na virtuálních počítačích a dostupnost fyzických serverů v případě, že je lokalita mimo provoz. Site Recovery replikuje úlohy spuštěné na virtuálních počítačích a fyzických serverech, aby zůstaly dostupné v sekundárním umístění v případě, že je primární lokalita nedostupná. Po opětovném zprovoznění primární lokality do ní úlohy obnoví.
- **Služba Backup:** [Azure Backup](https://docs.microsoft.com/azure/backup/) navíc zajišťuje zabezpečení a obnovitelnost vašich dat tím, že je zálohuje do Azure.

Site Recovery může spravovat replikaci pro:

- Virtuální počítače Azure replikované mezi oblastmi Azure.
- Místní virtuální počítače a fyzické servery replikované do Azure nebo sekundární lokality.


<a id="what-does-site-recovery-provide" class="xliff"></a>

## Co Site Recovery poskytuje?

**Funkce** | **Podrobnosti**
--- | ---
**Nasazení jednoduchého řešení BCDR** | Pomocí Site Recovery můžete nastavit a spravovat replikaci, převzetí služeb při selhání a navrácení služeb po obnovení z jednoho místa na webu Azure Portal.
**Replikace virtuálních počítačů Azure** | Můžete nastavit strategii BCDR tak, že se virtuální počítače Azure budou replikovat mezi oblastmi Azure.
**Replikace místních virtuálních počítačů mimo lokalitu** | Můžete replikovat místní virtuální počítače a fyzické servery do Azure nebo sekundární místní lokality. Replikací do Azure se eliminují náklady a složitost spojené s udržováním sekundárního datového centra.
**Replikace jakékoli úlohy** | Můžete replikovat libovolné úlohy spuštěné na podporovaných virtuálních počítačích Azure, místních virtuálních počítačích Hyper-V a VMware a na fyzických serverech s Windows nebo Linuxem.
**Zajištění odolnosti a zabezpečení dat** | Site Recovery orchestruje replikaci bez zachycování dat aplikací. Replikovaná data se ukládají ve službě Azure Storage s odolností, kterou tato služba nabízí. Pokud dojde k převzetí služeb při selhání, vytvoří se virtuální počítače Azure na základě replikovaných dat.
**Splnění RTO a RPO** | Udržujte cíle plánované doby obnovení (RTO) a plánovaných bodů obnovení (RPO) v mezích limitů organizace. Site Recovery poskytuje průběžnou replikaci virtuálních počítačů Azure a VMware a umožňuje frekvenci replikací pouhých 30 sekund. Plánovanou dobu obnovení (RTO) můžete ještě snížit integrací se službou [Azure Traffic Manager](https://azure.microsoft.com/blog/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/).
**Zachování konzistence aplikací při převzetí služeb při selhání** | Můžete nakonfigurovat body obnovení se snímky konzistentními vzhledem k aplikacím. Snímky konzistentní vzhledem k aplikacím zachycují data na disku, veškerá data v paměti a všechny probíhající transakce.
**Testování bez výpadků** | Můžete snadno spouštět testovací převzetí služeb při selhání, což umožňuje nácvik postupu zotavení po havárii, aniž by to mělo dopad na probíhající replikaci.
**Spouštění flexibilních převzetí služeb při selhání** | Pro očekávané výpadky můžete spouštět plánovaná převzetí služeb při selhání bez ztráty dat. V případě neočekávaných havárií pak můžou proběhnout neplánovaná převzetí služeb při selhání s minimálními ztrátami dat (v závislosti na frekvenci replikací). Služby potom můžete po obnovení snadno navrátit do primární lokality, až bude znovu dostupná.
**Vytváření plánů obnovení** | Pomocí plánů obnovení můžete přizpůsobit a nastavit pořadí převzetí služeb při selhání a zotavení vícevrstvých aplikací na několika virtuálních počítačích. V rámci plánů seskupujete počítače a přidáváte skripty a ručně prováděné akce. Plány obnovení je možné integrovat s runbooky služby Azure Automation.
**Integrace s existujícími technologiemi BCDR** | Site Recovery se integruje s dalšími technologiemi BCDR. Site Recovery je například možné použít k ochraně back-endu systému SQL Server u firemních úloh, včetně nativní podpory pro SQL Server AlwaysOn, pokud jde o přebírání služeb skupin dostupnosti při selhání.
**Integrace s knihovnou Automation** | Bohatá knihovna Azure Automation poskytuje skripty specifické pro aplikace a připravené pro produkční prostředí, které je možné stáhnout a integrovat se Site Recovery.
**Správa síťových nastavení** | Site Recovery se integruje s Azure a poskytuje jednoduchou správu aplikační sítě, včetně rezervace IP adres, konfigurace nástrojů pro vyrovnávání zatížení a integrace služby Azure Traffic Manager pro efektivní přepínání sítí.


<a id="what-can-i-replicate" class="xliff"></a>

## Co mohu replikovat?

**Podporuje se** | **Podrobnosti**
--- | ---
**Co můžu replikovat?** | Virtuální počítače Azure mezi oblastmi Azure (ve verzi Preview)<br/><br/>  Místní virtuální počítače VMware a Hyper-V a fyzické servery (s Windows nebo Linuxem) do Azure<br/<br/> Místní virtuální počítače VMware a Hyper-V a fyzické servery do sekundární lokality. Replikace virtuálních počítačů Hyper-V do sekundární lokality je podporována, pouze pokud jsou hostitelé Hyper-V spravováni nástrojem System Center VMM.
**Které oblasti jsou podporovány pro Site Recovery?** | [Podporované oblasti](https://azure.microsoft.com/regions/services/) |
**Které operační systémy replikované počítače potřebují?** | [Požadavky na virtuální počítač Azure](site-recovery-support-matrix-azure-to-azure.md#support-for-replicated-machine-os-versions)<br></br>[Požadavky na virtuální počítač VMware](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)<br/><br/> Pro virtuální počítače Hyper-V se podporuje každý [hostovaný operační systém](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows), který podporuje Azure a Hyper-V.<br/><br/> [Požadavky na fyzický server](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)
**Jaké servery nebo hostitele VMware potřebuji?** | Virtuální počítače VMware můžou být umístěné na [podporovaných hostitelích vSphere nebo serverech vCenter](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
**Jaké úlohy můžu replikovat?** | Můžete replikovat jakoukoli úlohu běžící v podporovaném počítači replikace. Kromě toho tým Site Recovery [pro řadu aplikací](site-recovery-workload.md#workload-summary) provedl testování specifické pro aplikace.


<a id="azure-portal-considerations" class="xliff"></a>

## Důležité informace o webu Azure Portal

* Site Recovery je možné nasadit na webu [Azure Portal](https://portal.azure.com).
* Na portálu Azure Classic můžete spravovat Site Recovery s klasickým modelem správy služeb.
- Portál Classic by se měl používat jenom k údržbě stávajících nasazení Site Recovery. Na portálu Classic nelze vytvářet nové trezory.

<a id="next-steps" class="xliff"></a>

## Další kroky
* Další informace o [podpoře úloh](site-recovery-workload.md)
* Můžete začít s [replikací virtuálních počítačů Azure mezi oblastmi](site-recovery-azure-to-azure.md), [replikací VMware do Azure](vmware-walkthrough-overview.md) nebo [replikací Hyper-V do Azure](hyper-v-site-walkthrough-overview.md).

