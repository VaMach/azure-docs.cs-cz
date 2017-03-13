---
title: Co je Site Recovery? | Dokumentace Microsoftu
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
ms.date: 02/26/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: e4bb13a73f6338d2d844a0561edc65063c685d59
ms.openlocfilehash: e554a0ba87efb0272e092a121ba96edc9d9eb011
ms.lasthandoff: 03/02/2017


---
# <a name="what-is-site-recovery"></a>Co je Site Recovery?

Vítá vás služba Azure Site Recovery! Tento článek poskytuje přehled služby a odkazy na další informace.

Obchodní výpadky jsou způsobené přírodními událostmi a provozními chybami. Vaše organizace potřebuje strategii pro provozní kontinuitu a zotavení po havárii (BCDR), která zajistí zabezpečení dat a dostupnost aplikací během plánovaných i neplánovaných výpadků a co nejrychlejší obnovení normální pracovních podmínek.

Služba Azure Recovery Services přispívá ke strategii BCDR. Služba [Azure Backup](https://docs.microsoft.com/en-us/azure/backup/) zajišťuje bezpečnost a obnovitelnost vašich dat. Site Recovery replikuje, přebírá služby při selhání a obnovuje úlohy, aby zůstaly dostupné i v případě selhání.

## <a name="what-does-site-recovery-provide"></a>Co Site Recovery poskytuje?

- **Zotavení po havárii v cloudu** – Úlohy běžící ve virtuálních počítačích a na fyzických serverech můžete replikovat do Azure namísto sekundární lokality. Tím se eliminují náklady a složitost spojené s udržováním sekundárního datového centra.
- **Flexibilní replikace pro hybridní prostředí** – Můžete replikovat libovolné úlohy běžící v podporovaných místních virtuálních počítačích Hyper-V a VMware a na fyzických serverech s Windows nebo Linuxem.
- **Migrace** – Pomocí Site Recovery můžete migrovat místní instance AWS na virtuální počítače Azure nebo migrovat virtuální počítače Azure mezi oblastmi Azure.
- **Zjednodušená strategie BCDR** – Replikaci můžete nasadit z jednoho místa na webu Azure Portal.  Můžete spouštět jednoduché převzetí služeb při selhání a navrácení služeb po obnovení u jednoho nebo několika počítačů.
- **Odolnost** – Site Recovery orchestruje replikace a převzetí služeb při selhání bez zachycování dat aplikací.
Replikovaná data se ukládají ve službě Azure Storage s odolností, kterou tato služba nabízí. Pokud dojde k převzetí služeb při selhání, vytvoří se virtuální počítače Azure na základě replikovaných dat.
- **Výkon replikace** – Site Recovery umožňuje frekvenci replikací pouhých 30 sekund pro Hyper-V a průběžnou replikaci pro VMware. Můžete nastavit prahové hodnoty cíle bodu obnovení (RPO) a řídit, jak často se body obnovení dat vytváří, a díky automatizovanému procesu obnovení služby Site Recovery a integraci se službou [Azure Traffic Manager](https://azure.microsoft.com/en-us/blog/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) můžete snížit plánovanou dobu obnovení (RTO).
- **Konzistentnost aplikací** – Počítače se replikují pomocí snímků konzistentních vzhledem k aplikacím. Kromě zachytávání dat na disku zachycují snímky konzistentní vzhledem k aplikacím i veškerá data v paměti a všechny probíhající transakce.
- **Testování bez výpadků** – Můžete snadno spouštět testovací převzetí služeb při selhání, což umožňuje nácvik zotavení po havárii, aniž by to mělo dopad na produkční prostředí.
- **Flexibilní převzetí služeb při selhání a obnovení** – Pro očekávané výpadky můžete spouštět plánovaná převzetí služeb při selhání bez ztráty dat. V případě neočekávaných havárií pak mohou proběhnout neplánovaná převzetí služeb při selhání s minimálními ztrátami dat (v závislosti na četnosti replikací). Služby potom můžete po obnovení snadno navrátit do primární lokality, až bude znovu dostupná.
- **Vlastní plány obnovení** – Plány obnovení umožňují modelovat a přizpůsobit převzetí služeb při selhání a obnovení vícevrstvých aplikací, které jsou rozdělené do několika virtuálních počítačů. V rámci plánů řadíte skupiny a přidáváte skripty a ručně prováděné akce. Plány obnovení je možné integrovat s runbooky služby Azure Automation.
- **Vícevrstvé aplikace** – Můžete vytvářet plány obnovení pro navazující převzetí služeb při selhání a obnovení vícevrstvých aplikací. V rámci plánu obnovení můžete počítače seskupovat v různých úrovních (například databáze, web, aplikace) a upravovat, jakým způsobem u každé skupiny probíhá převzetí služeb při selhání a spouštění.
* **Integrace se stávajícími technologiemi BCDR** – Site Recovery se integruje s dalšími technologiemi BCDR. Site Recovery je například možné použít k ochraně back-endu systému SQL Server u firemních úloh, včetně nativní podpory pro SQL Server AlwaysOn, pokud jde o přebírání služeb skupin dostupnosti při selhání.
* **Integrace s knihovnou Automation** – Bohatá knihovna Azure Automation obsahuje předpřipravené skripty specifické pro aplikace, které je možné stáhnout a integrovat se Site Recovery.
* **Jednoduchá správa sítě** – Pokročilá správa sítě v Site Recovery a Azure, která zjednodušuje požadavky aplikací na síť, včetně rezervace IP adres, konfigurace nástrojů pro vyrovnávání zatížení a integrace služby Azure Traffic Manager pro efektivní přepínání sítí.

## <a name="which-azure-portal"></a>Který portál Azure?

* Site Recovery je možné nasadit na novějším webu [Azure Portal](https://portal.azure.com) i na [portálu Azure Classic](https://manage.windowsazure.com/).
* Na portálu Azure Classic můžete podporovat Site Recovery s klasickým modelem správy služeb.
* Na webu Azure Portal můžete podporovat klasický model nasazení nebo novější [model nasazení Resource Manager](../azure-resource-manager/resource-manager-deployment-model.md).
- Portál Classic by se měl používat jenom k údržbě stávajících nasazení Site Recovery. Na portálu Classic nelze vytvářet nové trezory.

## <a name="whats-supported"></a>Co je podporováno?

**Podporuje se** | **Podrobnosti**
--- | ---
**Které oblasti jsou podporovány pro Site Recovery?** | [Podporované oblasti](https://azure.microsoft.com/en-us/regions/services/) |
**Co můžu replikovat?** | Místní virtuální počítače VMware a Hyper-V a fyzické servery s Windows nebo Linuxem.
**Které operační systémy replikované počítače potřebují?** | [Podporované operační systémy](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) pro virtuální počítače VMware<br/><br/> Pro virtuální počítače Hyper-V se podporuje každý [hostovaný operační systém](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows), který podporuje Azure a Hyper-V.<br/><br/> [Operační systémy](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) pro fyzické servery
**Kam můžu replikovat?** | Do úložiště Azure nebo do sekundárního datacentra<br/><br/> Pro Hyper-V je možné do sekundárního datacentra replikovat pouze virtuální počítače na hostitelích Hyper-V spravovaných v cloudech System Center VMM.
**Jaké servery nebo hostitele VMware potřebuji?** | Virtuální počítače VMware, které chcete replikovat, můžou být spravované [podporovanými hostiteli vSphere nebo servery vCenter](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
**Jaké úlohy můžu replikovat?** | Můžete replikovat jakoukoli úlohu běžící v podporovaném počítači replikace. Kromě toho tým Site Recovery [pro řadu aplikací](site-recovery-workload.md#workload-summary) provedl testování specifické pro aplikace.

## <a name="next-steps"></a>Další kroky
* Další informace o [podpoře úloh](site-recovery-workload.md)
* Další informace o [Architektuře a komponentách Site Recovery](site-recovery-components.md)

