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
ms.date: 10/05/2016
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 5614c39d914d5ae6fde2de9c0d9941e7b93fc10f
ms.openlocfilehash: ef9a7da86e7528d3052f89dbe1eaac6fbb90527c


---
# <a name="how-does-azure-site-recovery-work"></a>Jak funguje Azure Site Recovery?
Tento článek vám pomůže porozumět základní architektuře služby Azure Site Recovery a komponentám, které jí umožňují fungovat.

Jakékoli dotazy nebo připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Přehled
Organizace potřebují strategii BCDR, která určuje, jak aplikace, úlohy a data zůstanou spuštěné a dostupné během plánovaných a neplánovaných výpadků a jak co nejdříve obnovit normální provozní podmínky. Strategie BCDR by měla zajistit bezpečnost a obnovitelnost firemních dat a zajistit, aby v případě, že dojde k havárii, byly zpracovávané úlohy stále k dispozici.

Site Recovery je služba Azure, která přispívá ke strategie BCDR orchestrací replikace místní fyzických serverů a virtuálních počítačů do cloudu (Azure) nebo do sekundárního datového centra. Pokud dojde k výpadkům ve vašem primárním umístění, předáte služby při selhání do sekundárního umístění, aby aplikace a úlohy zůstaly dostupné. Až se obnoví normální provozní podmínky, vrátíte služby po obnovení zpět do primárního umístění. Potřebujete další informace [O službě Site Recovery?](site-recovery-overview.md)

## <a name="site-recovery-in-the-azure-portal"></a>Site Recovery na portálu Azure
Azure má dva různé [modely nasazení](../resource-manager-deployment-model.md) pro vytváření prostředků a práci s nimi: Azure Resource Manager a klasický model správy služeb. Azure má také dva portály – [portál Azure Classic](https://manage.windowsazure.com/), který podporuje klasický model nasazení, a [portál Azure](https://portal.azure.com) s podporou pro oba modely nasazení.

Site Recovery je k dispozici na klasickém portálu a na portálu Azure. Na portálu Azure Classic můžete podporovat službu Site Recovery s klasickým modelem správy služeb. Na portálu Azure můžete podporovat klasický model nasazení nebo model nasazení prostředků. [Další informace](site-recovery-overview.md#site-recovery-in-the-azure-portal) o nasazení s portálem Azure.

Informace v tomto článku se vztahují na klasické nasazení i na nasazení portálu Azure. Rozdíly jsou popsány v případě potřeby.

## <a name="deployment-scenarios"></a>Scénáře nasazení
Site Recovery je možné nasadit pro orchestraci replikace v řadě scénářů:

* **Replikace virtuálních počítačů VMware**: Lokální virtuální počítače VMware můžete replikovat do Azure nebo sekundárního datacentra.
* * **Replikace fyzických počítačů**: Fyzické počítače s Windows nebo Linuxem můžete replikovat do Azure nebo sekundárního datacentra. Proces pro replikování fyzických počítačů je téměř stejný jako proces pro replikaci virtuálních počítačů VMware.
* **Replikace virtuálních počítačů Hyper-V (bez VMM)**: Virtuální počítače Hyper-V nespravované přes VMM můžete replikovat do Azure.
* **Replikace virtuálních počítačů Hyper-V spravovaných v cloudech System Center VMM**: Lokální virtuální počítače Hyper-V v cloudech VMM, které běží na hostitelských serverech Hyper-V, můžete replikovat do Azure nebo sekundárního datacentra. Můžete replikovat pomocí standardní repliky Hyper-V nebo replikace sítě SAN.
* **Migrace virtuálních počítačů**: Pomocí Site Recovery můžete [migrovat virtuální počítače Azure IaaS](site-recovery-migrate-azure-to-azure.md) mezi regiony nebo [migrovat instance AWS Windows](site-recovery-migrate-aws-to-azure.md) na virtuální počítače Azure IaaS. Aktuálně je podporována pouze migrace, což znamená, že při selhání můžete převzít služby těchto virtuálních počítačů, ale nemůžete je po obnovení navrátit.

Site Recovery může replikovat většinu aplikací běžících na těchto virtuálních počítačích a fyzických serverech. Úplný souhrn podporovaných aplikací naleznete v tématu [Jaké úlohy je možné chránit pomocí Azure Site Recovery?](site-recovery-workload.md).

## <a name="replicate-to-azure-vmware-virtual-machines-or-physical-windowslinux-servers"></a>Replikace do Azure: Virtuální počítače VMware nebo fyzické servery Windows nebo Linux
Existuje několik způsobů replikace virtuálních počítačů VMware pomocí Site Recovery.

* **Pomocí portálu Azure** – Pokud nasadíte službu Site Recovery v portálu Azure, můžete převzít služby virtuálních počítačů do klasického úložiště správce služeb nebo do Správce prostředků. Replikace virtuálních počítačů VMware v portálu Azure přináší řadu výhod, včetně možnosti replikace do klasického úložiště nebo do úložiště Správce prostředků v Azure. [Další informace](site-recovery-vmware-to-azure.md).
* **Použití klasického portálu**-Site Recovery můžete nasadit na klasickém portálu pomocí vylepšeného prostředí. To by mělo sloužit pro všechna nová nasazení na klasickém portálu. V tomto nasazení můžete předat virtuální počítače jenom do klasického úložiště v Azure a nikoli do úložiště Správce prostředků. [Další informace](site-recovery-vmware-to-azure-classic.md). K dispozici je také [starší verze prostředí](site-recovery-vmware-to-azure-classic-legacy.md) pro nastavení replikace VMware na klasickém portálu. Ta by se neměla používat pro nová nasazení.  Pokud jste už provedli nasazení pomocí starší verze prostředí, [získejte další informace o migraci](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment) do rozšířeného nasazení.

Požadavky na architekturu pro nasazení služby Site Recovery na replikování virtuálních počítačů VMware / fyzických serverů na portálu Azure nebo Azure Classic (rozšířené) jsou podobné s několika rozdíly:

* Pokud provádíte nasazení na portálu Azure, můžete replikovat do úložiště založeného na Správci prostředků a použít sítě Správce prostředků pro připojení virtuálních počítačů Azure po převzetí služeb při selhání.
* Když provádíte nasazení na portálu Azure, je podporováno úložiště LRS i GRS. Na portálu Classic je vyžadováno úložiště GRS.
* Proces nasazení je na portálu Azure jednodušší a srozumitelnější.

Zde je seznam toho, co budete potřebovat:

* **Účet Azure**: Budete potřebovat účet Microsoft Azure.
* **Úložiště Azure**: K ukládání replikovaných dat budete potřebovat účet úložiště Azure. Můžete použít klasický účet nebo účet úložiště Správce prostředků. Při nasazení na portálu Azure může být účet LRS nebo GRS. Replikovaná data jsou uložena v úložišti Azure a virtuální počítače Azure se spouštějí, když je třeba převzít služby při selhání.
* **Síť Azure**: Budete potřebovat virtuální síť Azure, ke které se budou spuštěné virtuální počítače Azure připojovat, když dojde k jejich vytvoření při převzetí služeb při selhání. Na portálu Azure mohou být sítě vytvořeny v klasickém modelu správy služeb nebo v modelu Správce prostředků.
* **Místní konfigurační server**: budete potřebovat místní počítač Windows Server 2012 R2, na kterém běží konfigurační server a další součásti Site Recovery. Pokud provádíte replikaci virtuálních počítačů VMware, mělo by se jednat o vysoce dostupný virtuální počítač VMware. Pokud chcete replikovat fyzické servery, může se jednat o fyzický počítač. Na počítač se nainstalují tyto součásti služby Site Recovery:
  * **Konfigurační server**: Koordinuje komunikaci mezi místním prostředím a Azure a spravuje replikaci a obnovení dat.
  * **Procesní server:** Funguje jako replikační brána. Přijímá replikační data z chráněných zdrojových počítačů, optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování a odesílá je do úložiště Azure. Rovněž se stará o nabízenou instalaci služby Mobility na chráněné počítače a provádí automatické zjišťování virtuálních počítačů VMware. Jak vaše nasazení poroste, můžete přidávat další samostatné vyhrazené procesní servery pro zpracování zvyšujícího se objemu replikačních přenosů.
  * **Hlavní cílový server**: Zpracovává replikační data během navracení služeb z Azure po obnovení.
* **Virtuální počítače VMware**: Na každý počítač, který chcete replikovat do Azure, bude nutné nainstalovat službu Mobility. Tato služba zaznamenává datové zápisy na počítači a předává je na procesní server. Tuto součást můžete nainstalovat ručně nebo ji může automaticky (v režimu nabízení) nainstalovat procesní server, když aktivujete replikaci pro počítač.
* **Hostitelé vSPhere / server vCenter**: Budete potřebovat nejméně jeden hostitelský server vSPhere, na kterém poběží virtuální počítače VMware. Doporučujeme vám, abyste nasadili server vCenter pro správu těchto hostitelů.
* **Navrácení služeb po obnovení**: Tady je vše, co budete potřebovat:
  * **Navrácení služeb po obnovení (model fyzický-fyzický) není podporováno**: to znamená, že pokud převezmete služby po selhání v případě fyzických serverů na Azure a poté chcete provést navrácení služeb po obnovení, musíte provést navrácení služeb po obnovení virtuálního počítače VMware. Nelze navrátit služby po obnovení v případě fyzického serveru. Pro navrácení služeb po obnovení budete potřebovat virtuální počítač Azure, a pokud jste neprovedli nasazení konfiguračního serveru jako virtuálního počítače VMware, budete muset nastavit samostatný hlavní cílový server jako virtuální počítač VMware. To je potřeba proto, že hlavní cílový server pracuje a spojuje se s úložištěm VMware pro obnovení disků na virtuální počítač VMware.
  * * **Dočasný procesní server v Azure**: Pokud chcete po převzetí služeb při selhání tyto služby po obnovení vrátit, budete muset pro zpracování replikace z Azure nastavit virtuální počítač Azure nakonfigurovaný jako procesní server. Tento virtuální počítač je možné po navrácení služeb po obnovení odstranit.
  * **Připojení k síti VPN:** Pro navrácení služeb po obnovení budete potřebovat připojení k síti VPN (nebo Azure ExpressRoute) nastavené ze sítě Azure k místní lokalitě.
  * **Samostatný lokální hlavní cílový server:** Místní hlavní cílový server se stará o navrácení služeb po obnovení. Hlavní cílový server je ve výchozím nastavení nainstalován na serveru pro správu, ale pokud po obnovení vracíte větší objemy přenosů, měli byste pro tento účel nastavit samostatný místní hlavní cílový server.

**Obecná architektura**

![Rozšířené](./media/site-recovery-components/arch-enhanced.png)

**Součásti nasazení**

![Rozšířené](./media/site-recovery-components/arch-enhanced2.png)

**Navrácení služeb po obnovení**

![Rozšířené navrácení služeb po obnovení](./media/site-recovery-components/enhanced-failback.png)

* [Další informace](site-recovery-vmware-to-azure.md#azure-prerequisites) o požadavcích pro nasazení na portálu Azure.
* [Další informace](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) o požadavcích u rozšířeného nasazení na portálu Classic.
* [Další informace](site-recovery-failback-azure-to-vmware.md) o navrácení služeb po obnovení na portálu Azure Portal.
* [Další informace](site-recovery-failback-azure-to-vmware-classic.md) o navrácení služeb po obnovení na portálu Classic.

## <a name="replicate-to-azure-hyper-v-vms-not-managed-by-vmm"></a>Replikace do Azure: virtuální počítače Hyper-V nejsou spravovány nástrojem VMM
Následujícím způsobem můžete do Azure pomocí Site Recovery replikovat virtuální počítače Hyper-V, které nejsou spravovány nástrojem System Center VMM:

* **Pomocí portálu Azure**-Pokud provádíte nasazení Site Recovery na portálu Azure, můžete převzít služby virtuálních počítačů do klasického úložiště nebo do Správce prostředků. [Další informace](site-recovery-hyper-v-site-to-azure.md).
* **Použití portálu Classic**-Službu Site Recovery můžete nasadit na portálu Classic. V tomto nasazení můžete předat virtuální počítače jenom do klasického úložiště v Azure a nikoli do úložiště Správce prostředků. [Další informace](site-recovery-hyper-v-site-to-azure-classic.md).

Architektura je pro obě nasazení podobná s následující výjimkou:

* Pokud provádíte nasazení na portálu Azure, můžete replikovat do úložiště Správce prostředků a použít sítě Správce prostředků pro připojení virtuálních počítačů Azure po převzetí služeb při selhání.
* Proces nasazení je na portálu Azure jednodušší a srozumitelnější.

Zde je seznam toho, co budete potřebovat:

* **Účet Azure**: Budete potřebovat účet Microsoft Azure.
* **Úložiště Azure**: K ukládání replikovaných dat budete potřebovat účet úložiště Azure. Na portálu Azure můžete použít klasický účet nebo účet úložiště Správce prostředků. Na klasickém portálu můžete použít pouze klasický účet. Replikovaná data jsou uložena v úložišti Azure a virtuální počítače Azure jsou vytvářeny, když dojde k převzetí služeb při selhání.
* **Síť Azure**: Budete potřebovat síť Azure, ke které se budou spuštěné virtuální počítače Azure připojovat, když dojde k jejich vytvoření po převzetí služeb při selhání.
* **Hostitel Hyper-v**: budete potřebovat minimálně jeden hostitelský server Windows Server 2012 R2 Hyper-V. Během nasazování Site Recovery nainstalujete na hostitele zprostředkovatele služby Azure Site Recovery a agenta služby Microsoft Azure Recovery Services.
* **Virtuální počítače Hyper-V**: budete potřebovat jeden nebo více virtuálních počítačů na hostitelském serveru Hyper-V. Zprostředkovatel Azure Site Recovery a agent Služeb zotavení Azure na hostiteli Hyper-V během nasazení Site Recovery. Zprostředkovatel koordinuje a orchestruje replikaci pomocí služby Site Recovery přes internet. Agent zpracovává replikovaná data dat přes HTTPS 443. Komunikace z poskytovatele i agenta je zabezpečená a šifrovaná. Šifrují se rovněž replikovaná data v úložišti Azure.

**Obecná architektura**

![Z lokality Hyper-V do Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

* [Další informace](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites) o požadavcích pro nasazení portálu Azure.
* [Další informace](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites) o požadavcích pro nasazení portálu Classic.

## <a name="replicate-to-azure-hyper-v-vms-managed-by-vmm"></a>Replikace do Azure: virtuální počítače Hyper-V jsou spravovány nástrojem VMM
Virtuální počítače v cloudech VMM můžete replikovat do Azure pomocí Site Recovery následujícím způsobem:

* **Pomocí portálu Azure**-Pokud provádíte nasazení Site Recovery na portálu Azure, můžete převzít služby virtuálních počítačů do klasického úložiště nebo do Správce prostředků. [Další informace](site-recovery-vmm-to-azure.md).
* **Použití portálu Classic**-Službu Site Recovery můžete nasadit na portálu Classic. V tomto nasazení můžete předat virtuální počítače jenom do klasického úložiště v Azure a nikoli do úložiště Správce prostředků. [Další informace](site-recovery-vmm-to-azure-classic.md).

Architektura je pro obě nasazení podobná s následující výjimkou:

* Pokud provádíte nasazení na portálu Azure, můžete replikovat do úložiště založeného na Správci prostředků a použít sítě Správce prostředků pro připojení virtuálních počítačů Azure po převzetí služeb při selhání.
* Proces nasazení je na portálu Azure jednodušší a srozumitelnější.

Zde je seznam toho, co budete potřebovat:

* **Účet Azure**: Budete potřebovat účet Microsoft Azure.
* **Úložiště Azure**: K ukládání replikovaných dat budete potřebovat účet úložiště Azure. Na portálu Azure můžete použít klasický účet nebo účet úložiště Správce prostředků. Na klasickém portálu můžete použít pouze klasický účet. Replikovaná data jsou uložena v úložišti Azure a virtuální počítače Azure jsou vytvářeny, když dojde k převzetí služeb při selhání.
* **Síť Azure**: Budete muset nastavit mapování sítě, aby virtuální počítače Azure zůstaly po vytvoření a převzetí služeb při selhání připojené k příslušným sítím.
* **VMM server**: Budete potřebovat jeden nebo více místních VMM serverů běžících na System Center 2012 R2 a nastavení s jedním nebo několika privátními cloudy. Pokud provádíte nasazení do portálu Azure, budete potřebovat nastavit logické sítě a sítě virtuálních počítačů, aby bylo možné nakonfigurovat mapování sítě. Na portálu Classic je toto volitelné.  Síť virtuálních počítačů musí být propojena na logickou síť, která je přidružena ke cloudu.
* **Hostitel Hyper-v**: budete potřebovat jeden nebo více hostitelských serverů Windows Server 2012 R2 Hyper-V v cloudu VMM.
* **Virtuální počítače Hyper-V**: budete potřebovat jeden nebo více virtuálních počítačů na hostitelském serveru Hyper-V.

**Obecná architektura**

![Z VMM do Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

* [Další informace](site-recovery-vmm-to-azure.md#azure-prerequisites) o požadavcích pro nasazení portálu Azure.
* [Další informace](site-recovery-vmm-to-azure-classic.md) o požadavcích pro nasazení portálu Classic.

## <a name="replicate-to-a-secondary-site-vmware-virtual-machines-or-physical-servers"></a>Replikace do sekundární lokality: virtuální počítače VMware nebo fyzické servery
Pokud chcete replikovat virtuální počítače VMware nebo fyzické servery do sekundární lokality, stáhněte si nástroj InMage Scout, který je součástí předplatného Azure Site Recovery. Nástroj se dá stáhnout z portálu Azure nebo z portálu Azure Classic.

V každé lokalitě si nastavte servery představující jednotlivé součásti (konfigurační, procesní, hlavní cílový) a nainstalujte si Unified Agent na počítače, které chcete replikovat. Po počáteční replikaci odesílá agent na každém počítači rozdílové replikační změny na procesní server. Procesní server tato data optimalizuje a přenese je na hlavní cílový server v sekundární lokalitě. Konfigurační server spravuje replikační proces.

Zde je seznam toho, co k tomu potřebujete:

**Účet Azure**: Tento scénář nasadíte pomocí nástroje InMage Scout. K jeho získání budete potřebovat předplatné Azure. Po vytvoření trezoru Site Recovery si stáhnete InMage Scout a nainstalujete nejnovější aktualizace pro nastavení nasazení.
**Procesní server (primární lokalita)**: Nastavte si v primární lokalitě součást, kterou představuje procesní server. Ten se bude starat o ukládání do mezipaměti, kompresi a optimalizaci dat. Také obstará nabízenou instalaci nástroje Unified Agent na počítače, které chcete chránit.
**VMware ESX/ESXi a server vCenter (primární lokalita)**: Pokud chráníte virtuální počítače VMware, budete potřebovat hypervisor VMware EXS/ESXi a volitelně server VMware vCenter pro správu hypervisorů.

* **Virtuální počítače/fyzické servery (primární lokalita)**: Virtuální počítače VMware nebo fyzické servery s Windows nebo Linuxem, které chcete chránit, budou potřebovat, aby byl nainstalován Unified Agent. Unified Agent je také nainstalován na počítačích, které fungují jako hlavní cílový server. Agent funguje jako zprostředkovatel komunikace mezi všemi součástmi.
* * **Konfigurační server (sekundární lokalita)**: Jde o první součást, kterou nainstalujete, a to na sekundární lokalitu pro účely správy, konfigurace a monitorování vašeho nasazení – buď pomocí webu pro správu, nebo konzole vContinuum. V nasazení je pouze jediný konfigurační server a musí být nainstalován na počítači se systémem Windows Server 2012 R2.
* **vContinuum server (sekundární lokalita)**: Je nainstalována ve stejném umístění (sekundární lokalita) jako konfigurační server. Poskytuje konzoli pro správu a monitorování chráněného prostředí. Ve výchozí instalaci je server vContinuum prvním hlavním cílovým serverem a je na něm nainstalován Unified Agent.
* **Hlavní cílový server (sekundární lokalita)**: Hlavní cílový server uchovává replikovaná data. Přijímá data z procesního serveru, vytváří repliku počítače v sekundární lokalitě a ukládá body pro uchovávání dat. Počet hlavních cílových serverů, které potřebujete, závisí na počtu počítačů, které chráníte. Pokud chcete po obnovení služby vrátit do primární lokality, potřebujete hlavní cílový server i tam.

**Obecná architektura**

![Z VMware do VMware](./media/site-recovery-components/vmware-to-vmware.png)

## <a name="replicate-to-a-secondary-site-hyper-v-vms-managed-by-vmm"></a>Replikace do sekundární lokality: virtuální počítače Hyper-V jsou spravovány nástrojem VMM
Pomocí Site Recovery můžete následujícím způsobem replikovat do sekundárního datového centra virtuální počítače Hyper-V, které jsou spravovány nástrojem System Center VMM:

* **Pomocí portálu Azure** – Pokud nasadíte Site Recovery v portálu Azure. [Další informace](site-recovery-hyper-v-site-to-azure.md).
* **Použití portálu Classic**-Službu Site Recovery můžete nasadit na portálu Classic. [Další informace](site-recovery-hyper-v-site-to-azure-classic.md).

Architektura je pro obě nasazení podobná s následující výjimkou:

* Pokud provádíte nasazení na portálu Azure, musíte nastavit mapování sítě. Na portálu Classic je toto volitelné.
* Proces nasazení je na portálu Azure jednodušší a srozumitelnější.
* * Pokud provádíte nasazení na portálu Azure Classic, je k dispozici [mapování úložiště](site-recovery-storage-mapping.md).

Zde je seznam toho, co budete potřebovat:

* **Účet Azure**: Budete potřebovat účet Microsoft Azure.
* **Server VMM**: Doporučujeme mít jeden server VMM v primární lokalitě a další v sekundární lokalitě, přičemž každý by měl obsahovat alespoň jeden privátní cloud VMM. Na serveru by měl běžet přinejmenším System Center 2012 SP1 s nejnovějšími aktualizacemi a měl by být připojen k internetu. Cloudy musí mít nastavenu profilovou sadu schopností Hyper-V. Budete potřebovat nainstalovat zprostředkovatele Azure Site Recovery na server VMM. Zprostředkovatel koordinuje a orchestruje replikaci pomocí služby Site Recovery přes internet. Komunikace mezi poskytovatelem a Azure je zabezpečená a šifrovaná.
* **Server Hyper-V**: Hostitelské servery Hyper-V by se měly nacházet v primárních a sekundárních cloudech VMM. Na hostitelských serverech musí běžet přinejmenším Windows Server 2012, na kterém jsou nainstalovány nejnovější aktualizace, a musí být připojeny k internetu. Data se replikují mezi primárním a sekundárním hostitelským serverem Hyper-V přes síť LAN nebo VPN na základě protokolu Kerberos nebo ověření certifikátem.  
* **Chráněné počítače**: Zdrojový hostitelský server Hyper-V musí mít přinejmenším jeden virtuální počítač, který chcete chránit.

**Obecná architektura**

![Z lokálního prostředí do lokálního prostředí](./media/site-recovery-components/arch-onprem-onprem.png)

* [Zde jsou další informace](site-recovery-vmm-to-vmm.md#azure-prerequisites) o požadavcích na nasazení do portálu Azure.
* * [Zde jsou další informace](site-recovery-vmm-to-vmm-classic.md#before-you-start) o požadavcích na nasazení do portálu Azure Classic.

## <a name="replicate-to-a-secondary-site-with-san-replication-hyper-v-vms-managed-by-vmm"></a>Replikace do sekundární lokality pomocí replikace sítě SAN: virtuální počítače Hyper-V jsou spravovány nástrojem VMM
Můžete replikovat virtuální počítače Hyper-V, spravované v cloudech VMM, do sekundární lokality pomocí replikace sítě SAN za použití portálu Azure Classic. Tento scénář není na novém portálu Azure aktuálně podporován.

V tomto scénáři nainstalujete během nasazování Site Recovery zprostředkovatele Azure Site Recovery na servery VMM. Zprostředkovatel koordinuje a orchestruje replikaci pomocí služby Site Recovery přes internet. Data se replikují mezi primárním a sekundárním polem úložiště s použitím synchronní replikace v síti SAN.

Zde je seznam toho, co budete potřebovat:

**Účet Azure**: budete potřebovat předplatné Azure

* **Pole SAN**: [Podporované pole SAN](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) spravované primárním serverem VMM. Síť SAN sdílí síťovou infrastrukturu s jiným polem SAN v sekundární lokalitě.
* **Server VMM:** Doporučujeme mít jeden server VMM v primární lokalitě a další v sekundární lokalitě, přičemž každý by měl obsahovat alespoň jeden privátní cloud VMM. Na serveru by měl běžet přinejmenším System Center 2012 SP1 s nejnovějšími aktualizacemi a měl by být připojen k internetu. Cloudy musí mít nastavenu profilovou sadu schopností Hyper-V.
* **Server Hyper-V:** Hostitelské servery Hyper-V nacházející se v primárních a sekundárních cloudech VMM. Na hostitelských serverech musí běžet přinejmenším Windows Server 2012, na kterém jsou nainstalovány nejnovější aktualizace, a musí být připojeny k internetu.
* **Chráněné počítače**: Zdrojový hostitelský server Hyper-V musí mít přinejmenším jeden virtuální počítač, který chcete chránit.

**Architektura replikace sítě SAN**

![Replikace sítě SAN](./media/site-recovery-components/arch-onprem-onprem-san.png)

[Zde jsou další informace](site-recovery-vmm-san.md#before-you-start) o požadavcích na nasazení.

### <a name="on-premises"></a>Lokálně
## <a name="hyper-v-protection-lifecycle"></a>Životní cyklus ochrany Hyper-V
Tento pracovní postup nastiňuje proces ochrany, replikace a převzetí služeb při selhání u virtuálních počítačů Hyper-V.

1. **Povolení ochrany:** Nastavíte trezor Site Recovery, nakonfigurujete nastavení replikace pro cloud VMM nebo web Hyper-V a povolíte ochranu pro virtuální počítače. Spustí se úloha s názvem **Povolení ochrany** a lze ji monitorovat na kartě **Úlohy**. V rámci úlohy se zkontroluje, zda počítač splňuje požadavky, a potom se vyvolá metoda [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx), která nastaví replikaci do Azure na základě nastavení, které jste nakonfigurovali. Úloha **Povolení ochrany** také vyvolá metodu [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) pro inicializaci úplné replikace virtuálního počítače.
2. **Počáteční replikace:** Pořídí se snímek virtuálního počítače a virtuální pevné disky se replikují jeden po druhém, dokud se všechny nezkopírují do Azure nebo do sekundárního datacentra. Čas potřebný k dokončení závisí na velikosti virtuálního počítače, šířce pásma sítě a metodě počáteční replikace. Pokud dojde ke změnám na disku v době, kdy probíhá počáteční replikace, Hyper-V Replica Replication Tracker je zaznamená jako protokoly replikace technologie Hyper-V (.hrl), které jsou umístěny ve stejné složce jako disky. Každý disk má přidružený soubor .hrl, který se odešle do sekundárního úložiště. Mějte na paměti, že soubory snímků a protokolů spotřebovávají prostředky disku v době, kdy probíhá počáteční replikace. Po dokončení počáteční replikace se snímek virtuálního počítače odstraní a rozdílové změny na disku v protokolu se synchronizují a sloučí.
3. **Dokončení ochrany:** Po počáteční replikaci nakonfiguruje úloha **Dokončení ochrany** síťová a další postreplikační nastavení tak, aby virtuální počítač byl chráněn. Pokud replikujete do Azure, možná bude nutné upravit nastavení pro virtuální počítač tak, aby byl připraven k převzetí služeb při selhání. V tomto bodě můžete spustit test převzetí služeb při selhání a zkontrolovat, zda vše funguje podle očekávání.
4. **Replikace:** Po počáteční replikaci se zahájí rozdílová synchronizace, a to v souladu s nastavením replikace.
   * **Selhání replikace:** Pokud rozdílová replikace selže a úplná replikace by byla náročná, pokud jde o šířku pásma nebo čas, pak dojde k resynchronizaci. Pokud například soubory .hrl dosáhnou 50 % velikosti disku, pak se u virtuálního počítače stanoví nutnost resynchronizace. Resynchronizace minimalizuje množství dat odesílaných v rámci výpočtů kontrolních součtů zdrojových a cílových virtuálních počítačů tím, že se posílají pouze rozdíly. Po dokončení synchronizace bude pokračovat rozdílová replikace. Ve výchozím nastavení je automatické spuštění resynchronizace naplánováno na mimopracovní dobu, ale můžete ji u virtuálního počítače spustit i ručně.
   * **Chyba replikace:** Pokud dojde k chybě replikace, je předdefinován opakovaný pokus. Jde-li o neopravitelnou chybu, například související s ověřením nebo autorizací, nebo je-li v neplatném stavu počítač představující repliku, k opakovanému pokusu nedojde. Pokud se jedná o opravitelnou chybu, jako je chyba sítě nebo nedostatek volného místa na disku či paměti, pak se pokusy opakují, ale postupně se mezi nimi prodlužuje interval (proběhnou po 1, 2, 4, 8 a 10 minutách a pak každých 30 minut).
5. **Plánované/neplánované převzetí služeb při selhání:** Podle potřeby můžete spustit plánované, nebo neplánované převzetí služeb při selhání. Pokud spustíte plánovanou operaci, dojde k ukončení zdrojových virtuálních počítačů, aby se zcela předešlo možné ztrátě dat. Virtuální počítače představující repliky přejdou po svém vytvoření do stavu čekání na potvrzení. Pro dokončení převzetí služeb při selhání je musíte potvrdit, což ale neplatí při replikaci sítě SAN, protože v takovém případě proběhne potvrzení automaticky. Po nastavení a spuštění primární lokality může proběhnout navrácení služeb po obnovení. Pokud jste nastavili replikaci do Azure, bude zpětná replikace automatická. V opačném případě ji spustíte ručně.

![pracovní postup](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Další kroky
[Příprava nasazení](site-recovery-best-practices.md)



<!--HONumber=Nov16_HO2-->


