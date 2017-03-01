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
ms.date: 02/06/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 5adacea6033070e808f9bf5e76bae0f45f7f1199
ms.openlocfilehash: e922590c500517c93f7682b5cca5ef6a2c89de53


---
# <a name="what-is-site-recovery"></a>Co je Site Recovery?
Vítá vás služba Azure Site Recovery! Tento článek přináší stručný přehled služby Site Recovery.

Vaše organizace potřebuje strategii pro provozní kontinuitu a zotavení po havárii (BCDR), která zajistí zabezpečení a dostupnost aplikací a dat během plánovaných i neplánovaných výpadků a co nejrychlejší obnovení normální pracovních podmínek.

Site Recovery přispívá ke strategii BCDR orchestrací replikace místních virtuálních počítačů a fyzických serverů. Servery a virtuální počítače můžete replikovat ze svého primárního místního datového centra do cloudu (Azure) nebo do sekundárního datového centra.

Pokud dojde k výpadkům v primární lokalitě, sekundární lokalita převezme služby při selhání, aby úlohy zůstaly dostupné. Až se obnoví normální provozní podmínky, vrátíte služby po obnovení zpět do primárního umístění.

## <a name="site-recovery-in-the-azure-portal"></a>Site Recovery na portálu Azure
V Azure existují dva různé [modely nasazení](../azure-resource-manager/resource-manager-deployment-model.md) pro vytváření prostředků a práci s nimi. Model Azure Resource Manager a klasický model správy služeb. Azure má také dva portály. [Portál Azure Classic](https://manage.windowsazure.com/) a web [Azure Portal](https://portal.azure.com).

* Site Recovery je možné nasadit na portálu Classic i na webu Azure Portal.
* Na portálu Azure Classic můžete podporovat Site Recovery s klasickým modelem správy služeb.
* Na webu Azure Portal můžete podporovat klasický model nasazení nebo model nasazení Resource Manager.

Informace v tomto článku se vztahují na klasické nasazení i na nasazení portálu Azure. Případné rozdíly jsme popsali.

## <a name="why-deploy-site-recovery"></a>Proč nasadit Site Recovery?
Zde je výčet, co může Site Recovery poskytnout vašemu podniku:

* **Zjednodušení BCDR** – Můžete replikovat, přebírat služby při selhání a obnovovat více úloh z jednoho místa na webu Azure Portal. Site Recovery orchestruje replikace a převzetí služeb při selhání, bez zachycování dat aplikací.
* **Flexibilní replikace** – Můžete replikovat libovolné úlohy běžící v podporovaných virtuálních počítačích Hyper-V a VMware a na fyzických serverech s Windows nebo Linuxem.
* **Eliminace sekundárního datového centra** – Úlohy můžete replikovat do Azure namísto sekundární lokality. Tím se eliminují náklady a složitost spojené s udržováním sekundárního datového centra. Replikovaná data se ukládají ve službě Azure Storage s odolností, kterou tato služba nabízí. Pokud dojde k převzetí služeb při selhání, vytvoří se virtuální počítače Azure s replikovanými daty.
* **Snadné testování replikace** – Je možné snadno spouštět testovací převzetí služeb při selhání, což umožňuje nácvik zotavení po havárii, aniž by to mělo dopad na produkční prostředí.
* **Převzetí služeb při selhání a obnovení** – Pro očekávané výpadky je možné spouštět plánovaná převzetí služeb při selhání bez ztráty dat. V případě neočekávaných havárií pak mohou proběhnout neplánovaná převzetí služeb při selhání s minimálními ztrátami dat (v závislosti na četnosti replikací). Služby potom můžete po obnovení navrátit na primární web, až bude znovu dostupný.
* **Převzetí služeb při selhání několika virtuálních počítačů** – Je možné nastavit plány obnovení, které zahrnují skripty a runbooky služby Azure Automation. Plány obnovení umožňují modelovat a přizpůsobit převzetí služeb při selhání a obnovení vícevrstvých aplikací, které jsou rozdělené do několika virtuálních počítačů.
* **Integrace se stávajícími technologiemi BCDR** – Site Recovery se integruje s dalšími technologiemi BCDR. Site Recovery je například možné použít k ochraně back-endu systému SQL Server u firemních úloh, včetně nativní podpory pro SQL Server AlwaysOn, pokud jde o přebírání služeb skupin dostupnosti při selhání.

## <a name="which-regions-are-supported"></a>Které oblasti jsou podporovány?
Další informace o oblastech, ve kterých je služba Azure Site Recovery dostupná, najdete [zde](https://azure.microsoft.com/en-us/regions/services/). 

## <a name="what-can-i-replicate"></a>Co mohu replikovat?
Zde naleznete souhrn toho, co dokáže nástroj Site Recovery replikovat.

![Z lokálního prostředí do lokálního prostředí](./media/site-recovery-overview/asr-overview-graphic.png)

| **REPLIKACE** | **REPLIKACE DO** |
| --- | --- |
| Místní virtuální počítače VMware |[Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Sekundární lokalita](site-recovery-vmware-to-vmware.md) |
| Místní virtuální počítače Hyper-V spravované v cloudech VMM |[Azure](site-recovery-vmm-to-azure.md)<br/><br/> [Sekundární lokalita](site-recovery-vmm-to-vmm.md) |
| Místní virtuální počítače Hyper-V používající úložiště pro sítě SAN a spravované v cloudech VMM |[Sekundární lokalita](site-recovery-vmm-san.md) |
| Místní virtuální počítače Hyper-V bez nástroje VMM |[Azure](site-recovery-hyper-v-site-to-azure.md) |
| Místní fyzické servery Windows/Linux |[Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Sekundární lokalita](site-recovery-vmware-to-vmware.md) |

## <a name="how-does-site-recovery-protect-workloads"></a>Jak služba Site Recovery chrání úlohy?
Site Recovery poskytuje replikaci se sledováním aplikací, aby úlohy a aplikace neustále běžely, i když dojde k výpadku.

* **Snímky konzistentní vzhledem k aplikacím** – Počítače se replikují pomocí snímků konzistentních vzhledem k aplikacím s jednou nebo více vrstvami. Kromě zachytávání dat na disku zachycují snímky konzistentní vzhledem k aplikacím i veškerá data v paměti a všechny probíhající transakce.
* **Téměř synchronní replikace** – Site Recovery umožňuje frekvenci replikací pouhých 30 sekund pro Hyper-V a neustálou replikaci pro VMware.
* **Flexibilní plány obnovení** – Můžete vytvářet a upravovat plány obnovení pomocí externích skriptů a ručních akcí. Integrace s runbooky služby Azure Automation umožňuje obnovit celý zásobník aplikací jediným kliknutím.
* **Integrace s SQL Server AlwaysOn** – Správa převzetí služeb skupin dostupnosti při selhání pomocí plánů obnovení
* **Knihovna Automation** – Bohatá knihovna Azure Automation obsahující předpřipravené skripty specifické pro aplikace, které je možné stáhnout a integrovat se Site Recovery
* **Jednoduchá správa sítě** – Pokročilá správa sítě v Site Recovery a Azure, která zjednodušuje požadavky aplikací na síť, včetně rezervace IP adres, konfigurace nástrojů pro vyrovnávání zatížení a integrace služby Azure Traffic Manager pro efektivní přepínání sítí.

## <a name="next-steps"></a>Další kroky
* Více informací o Site Recovery najdete v tématu [Jaké úlohy je možné chránit pomocí Site Recovery?](site-recovery-workload.md).
* Další informace o architektuře Site Recovery najdete v článku [Jak funguje Site Recovery?](site-recovery-components.md).



<!--HONumber=Feb17_HO4-->


