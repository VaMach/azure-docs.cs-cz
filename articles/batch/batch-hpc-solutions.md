---
title: "Řešení pro Batch a prostředí HPC v cloudu – Azure | Dokumentace Microsoftu"
description: "Další informace o scénářích a možnostech řešení pro dávky (batch) a vysokovýkonné výpočetní prostředí (HPC a Big Compute) v Azure"
services: batch, virtual-machines, cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
ms.assetid: aab5401d-2baf-4cf2-bf20-ad224de33888
ms.service: batch
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: d30c09d23a84200ba81df133c9de5b11910ffb61
ms.contentlocale: cs-cz
ms.lasthandoff: 08/21/2017

---
# <a name="batch-and-hpc-solutions-for-large-scale-computing-workloads"></a>Řešení pro Batch a prostředí HPC pro rozsáhlé výpočetní úlohy

Azure nabízí efektivní a škálovatelná cloudová řešení pro dávky (batch) a vysokovýkonné výpočetní prostředí (HPC) – taky známé jako *Big Compute* (provádění výpočtů ve velkém měřítku). Přečtěte si zde o úlohách řešení Big Compute a službách Azure, které je podporují, nebo přejděte přímo na [scénáře řešení](#scenarios) dále v tomto článku. Tento článek je zaměřen především na lidi, kteří mají na starosti technická rozhodnutí ve firmě, manažery pro IT a nezávislé dodavatele softwaru, ale i ostatní vývojáři a profesionálové v oblasti IT ho mohou využít k bližšímu seznámení s těmito řešeními. 

Organizace musí řešit rozsáhlé výpočetní problémy: technický návrh a analýza, vykreslování obrázků, komplexní modelování, simulace typu Monte Carlo, výpočty finančních rizik a další. Azure organizacím pomáhá s řešením těchto problémů s potřebnými prostředky, rozsahem a časovým plánem. Díky službě Azure organizace můžou:

* Vytvářet hybridní řešení, která rozšiřují místní cluster prostředí HPC, aby se mohla snížit jeho zátěž odesláním náročnějších úloh na cloud
* Spouštět nástroje clusteru prostředí HPC a úlohy jen prostřednictvím služby Azure
* Používat spravované a škálovatelné služby Azure, jako je [Batch](https://azure.microsoft.com/documentation/services/batch/), ke spouštění výpočetně náročných úloh bez nutnosti nasazení a správy výpočetní infrastruktury

Azure taky vývojářům a partnerům nabízí úplnou sadu funkcí, možností architektur a vývojářských nástrojů, které umožňují vytvářet vlastní rozsáhlé pracovní postupy Big Compute. To je však nad rámec tohoto článku. Je pro vás taky připraven rostoucí partnerský ekosystém, který vám pomůže v cloudu Azure vytvářet produktivní úlohy Big Compute.

## <a name="batch-and-hpc-applications"></a>Batch a aplikace prostředí HPC
Na rozdíl od webových aplikací a mnoha obchodních aplikací mají batche a aplikace prostředí HPC definovaný začátek a konec a mohou se spouštět podle plánu nebo na vyžádání. Běžet mohou i několik hodin nebo i déle. Většina spadá do dvou hlavních kategorií: *vnitřně paralelní* (někdy nazývané „trapně paralelní“, protože problémy, které řeší, se přímo nabízejí k tomu, aby byly řešeny paralelně na více počítačích nebo procesorech) a *úzce párované*. Další informace o těchto typech aplikací najdete v následující tabulce. Některá řešení služby Azure se hodí více k jednomu a některá k druhému typu.

> [!NOTE]
> U řešení pro Batch a prostředí HPC se spuštěná instance aplikace obvykle nazývá *úloha* (job). Každá úloha se pak může rozdělit na *úkoly* (tasks). A clusterované výpočetní prostředky pro aplikaci se často nazývají *výpočetní uzly*.
> 
> 

| Typ | Vlastnosti | Příklady |
| --- | --- | --- |
| **Vnitřně paralelní**<br/><br/>![Vnitřně paralelní][parallel] |• Jednotlivé počítače spouštějí aplikační logiku nezávisle na sobě.<br/><br/> • Přidávání počítačů umožňuje aplikaci škálovat a snižovat dobu výpočtu.<br/><br/>• Aplikace se skládá ze samostatných spustitelných souborů nebo je rozdělená do skupiny služeb, které vyvolává klient (architektura orientovaná na služby nebo aplikace SOA). |• Modelování finančních rizik<br/><br/>• Vykreslování a zpracovávání obrázků<br/><br/>• Kódování a překódování multimédií<br/><br/>• Simulace typu Monte Carlo<br/><br/>• Testování softwaru |
| **Úzce párované**<br/><br/>![Úzce párované][coupled] |• Aplikace vyžaduje, aby výpočetní uzly spolupracovaly nebo si vyměňovaly přechodné výsledky.<br/><br/>• Výpočetní uzly mohou komunikovat pomocí rozhraní Message Passing Interface (MPI), což je běžný komunikační protokol pro paralelní výpočty.<br/><br/>• Aplikace je citlivá na latenci sítě a šířku pásma.<br/><br/>• Výkon aplikace lze zvýšit pomocí vysokorychlostních síťových technologií, jako je například InfiniBand a přímý přístup do paměti vzdáleného počítače (RDMA). |• Modelování zásob plynu a ropy<br/><br/>• Technický návrh a analýza, například výpočet dynamiky kapaliny<br/><br/>• Fyzické simulace, například autohavárie nebo jaderné reakce<br/><br/>• Předpovědi počasí |

### <a name="considerations-for-running-batch-and-hpc-applications-in-the-cloud"></a>Důležité informace ohledně spouštění aplikací Batch a aplikací prostředí HPC v cloudu
Mnoho aplikací navržených ke spouštění v clusterech místního prostředí HPC můžete snadno migrovat do služby Azure nebo hybridního prostředí (pro více míst). Můžou zde ale existovat jistá omezení a požadavky, například:

* **Dostupnost cloudových prostředků** – V závislosti na typu cloudových výpočetních prostředků, které použijete, se možná nebudete moci spolehnout na nepřetržitou dostupnost počítače po celou dobu běhu úlohy. Běžná technika pro zvládání možných přechodných chyb, téměř nezbytná při používání cloudových prostředků, je používání stavu a kontrolních bodů průběhu.
* **Přístup k datům** – V podnikových clusterech běžně dostupné techniky přístupu k datům, jako je NFS, mohou v cloudu vyžadovat zvláštní konfiguraci. Nebo může být nutné pro cloud použít jiné metody a praktiky přístupu k datům.
* **Přesun dat** – Pro aplikace, které zpracovávají velké objemy dat, je třeba vytvořit strategie přesunu dat do cloudového úložiště a k výpočetním prostředkům. K tomu může být nutné použít vysokorychlostní síťové služby pro spojení mezi místy, jako je například [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/). Taky je třeba vzít v potaz omezení ukládání nebo přístupu k datům týkající se práva, norem nebo zásad.
* **Licencování** – s dodavateli komerčních aplikací se vždy poraďte ohledně licencování a dalších omezení při spouštění v cloudu. Ne všichni dodavatelé nabízejí licencování formou průběžných plateb. Možná pro své řešení budete muset navrhnout licenční server v cloudu nebo se připojit k místnímu licenčnímu serveru.

### <a name="big-compute-or-big-data"></a>Big Compute nebo Big Data?
Rozdíl mezi aplikacemi pro Big Compute (velký objem výpočtů) a Big Data (velký objem dat) není vždy zcela zřejmý. Některé aplikace mohou mít vlastnosti obou. Obojí zahrnuje spouštění rozsáhlých výpočtů, obvykle v clusterech počítačů. Přístupy k řešení a podpůrné nástroje se ale můžou lišit.

• **Big Compute** obvykle zahrnuje aplikace, které jsou závislé na výkonu procesoru a paměti, například aplikace pro technické simulace, modelování finančních rizik a digitální vykreslování. Infrastruktura pro řešení Big Compute může obsahovat počítače se specializovanými vícejádrovými procesory (navrženými k provádění syrových výpočtů) a specializovaný vysokorychlostní síťový hardware k propojení počítačů.

• **Big Data** řeší problémy analýzy dat, které zahrnují velké objemy dat, která není možné spravovat na jednom počítači nebo v jednom systému správy databáze. Jedná se například o velké objemy webových protokolů nebo jiná data business intelligence. Big Data se obvykle více než o výkon procesoru opírají o kapacitu disku a vstupně-výstupní výkon. Pro správu clusteru a třídění dat existují také specializované nástroje Big Data, jako například Apache Hadoop. (Informace o řešení Azure HDInsight a dalších řešeních Azure Hadoop najdete v článku [Hadoop](https://azure.microsoft.com/solutions/hadoop/).)

## <a name="compute-management-and-job-scheduling"></a>Správa výpočtů a plánování úloh
Spuštěné aplikace pro Batch a prostředí HPC často zahrnují služby *správce clusteru* a *plánovače úloh*, které pomáhají spravovat clusterované výpočetní prostředky a přidělovat je aplikacím, které spouštějí úlohy. Tyto funkce můžou být řešené pomocí samostatných nástrojů nebo integrovaného nástroje nebo služby.

* **Správce clusteru** – zřizuje, uvolňuje a spravuje výpočetní prostředky (nebo výpočetní uzly). Správce clusteru může automatizovat instalaci image operačního systému a aplikací na výpočetní uzly, škálovat výpočetní prostředky podle požadavků a sledovat výkon uzlů.
* **Plánovač úloh** – Určuje prostředky (například procesory nebo paměť), které aplikace potřebuje, a podmínky, při jejichž splnění bude spuštěná. Plánovač úloh organizuje frontu úloh a přiděluje jim prostředky na základě přiřazené priority nebo dalších vlastností.

U nástrojů pro clustering a plánování úloh, které jsou navržené pro clustery založené na systémech Windows nebo Linux, by měla být migrace do Azure bezproblémová . Například [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029), bezplatné řešení výpočetního clusteru pro úlohy v prostředí HPC systému Windows a Linux od společnosti Microsoft, nabízí několik možností pro spuštění ve službě Azure. Můžete také sestavit clustery s Linuxem ke spouštění opensourcových nástrojů, jako jsou Torque a SLURM. Do Azure můžete také přinést komerční řešení, jako jsou [TIBCO DataSynapse GridServer](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/), [IBM Spectrum Symphony a Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/) a [Univa Grid Engine](http://www.univa.com/products/grid-engine).

Jak uvidíte v následujících částech, výhod služeb Azure můžete taky využít ke správě výpočetních prostředků a plánování úloh bez tradičních nástrojů pro správu clusteru nebo dodatečně k nim.

## <a name="scenarios"></a>Scénáře
Prohlédněte si tři běžné scénáře spouštění úloh Big Compute v Azure, které používají existující řešení clusteru v prostředí HPC, služby Azure nebo kombinaci obojího. Uvedeny jsou klíčové faktory pro výběr jednotlivých scénářů, nejedná se ale o vyčerpávající seznam. Další informace o dostupných službách Azure, které můžete použít v rámci vašeho řešení, najdete dále v článku.

| Scénář | Proč zvolit? |
| --- | --- | --- |
| **Rozšíření clusteru prostředí HPC do služby Azure**<br/><br/>[![Cluster burst][burst_cluster]](./media/batch-hpc-solutions/burst_cluster.png) <br/><br/> Další informace:<br/>• [Instance pracovního procesu Burst to Azure se sadou HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)<br/><br/>• [Nastavení hybridního výpočetního clusteru pomocí sady HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)<br/><br/>• [Dávka Burst to Azure se sadou HPC Pack](https://technet.microsoft.com/library/mt612877.aspx)<br/><br/> |• Zkombinujte svou sadu [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) nebo jiný místní cluster s dalšími prostředky služby Azure v rámci hybridního řešení.<br/><br/>• Rozšiřte své úlohy Big Compute, aby se spouštěly na instancích virtuálních počítačů jako PaaS (platforma jako služba). Aktuálně jde použít jen u systému Windows Server.<br/><br/>• Přistupujte k místnímu licenčnímu serveru nebo úložišti dat pomocí volitelné virtuální sítě Azure. |
| **Vytvoření clusteru prostředí HPC zcela ve službě Azure**<br/><br/>[![Cluster in IaaS][iaas_cluster]](./media/batch-hpc-solutions/iaas_cluster.png)<br/><br/>Další informace:<br/>• [Řešení clusteru prostředí HPC ve službě Azure](big-compute-resources.md)<br/><br/> |• Rychle a konzistentně nasazujte své aplikace a nástroje clusteru na standardní nebo vlastní virtuální počítače modelu IaaS (infrastruktura jako služba) systému Windows nebo Linux.<br/><br/>• Spouštějte různé úlohy Big Compute pomocí řešení plánování úloh podle vlastního výběru.<br/><br/>• Použijte další služby Azure, například sítě a úložiště, k vytvoření kompletního řešení založeného na cloudu. |
| **Škálování paralelní aplikace do služby Azure**<br/><br/>[![Azure Batch][batch_proc]](./media/batch-hpc-solutions/batch_proc.png)<br/><br/>Další informace:<br/>• [Základy služby Azure Batch](batch-technical-overview.md)<br/><br/>• [Začínáme s knihovnou Azure Batch pro .NET](batch-dotnet-get-started.md) |• Použijte při vyvíjení službu [Azure Batch](https://azure.microsoft.com/documentation/services/batch/) a horizontálně navyšujte kapacitu u různých úloh Big Compute, které budete spouštět na fondech virtuálních počítačů s Windows nebo Linuxem.<br/><br/>• Využijte službu platformy Azure ke správě nasazení a automatického škálování virtuálních počítačů, plánování úloh, zotavení po havárii, přesunu dat, správě závislostí a nasazení aplikace. |

## <a name="azure-services-for-big-compute"></a>Služby Azure pro Big Compute
Zde se dozvíte další informace o výpočtech, datech, sítích a souvisejících službách, které můžete kombinovat s řešeními a pracovními postupy Big Compute. Podrobné pokyny ke službám Azure naleznete v [dokumentaci](https://azure.microsoft.com/documentation/) služeb Azure. [Scénáře](#scenarios) popsané dříve v tomto článku ukazují jenom některé z mnoha možností použití těchto služeb.

> [!NOTE]
> Azure pravidelně zavádí nové služby, které mohou být pro váš scénář užitečné. Pokud máte dotazy, obraťte se na [partnera Azure](https://pinpoint.microsoft.com/en-US/search?keyword=azure) nebo e-mail *bigcompute@microsoft.com*.
> 
> 

### <a name="compute-services"></a>Výpočetní služby
Výpočetní služby Azure jsou jádrem řešení Big Compute. Různé výpočetní služby jsou vhodné pro různé scénáře. Tyto služby na základní úrovni nabízejí různé režimy pro aplikace, které běží na výpočetních instancích založených na virtuálních počítačích, které Azure poskytuje pomocí technologie Windows Server Hyper-V. Na těchto instancích mohou běžet standardní i vlastní nástroje a operační systémy Windows a Linux. Azure vám dává na výběr z různých [velikostí instancí](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) s různými konfiguracemi jader procesoru, paměti, kapacity disku a dalších vlastností. V závislosti na vašich potřebách můžete škálovat instance až na tisíce jader a poté vertikálně snížit kapacitu, až budete potřebovat méně prostředků.

> [!NOTE]
> Využijte [instance Azure pro náročné výpočty, například řadu H-series](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), a zlepšete výkon a škálovatelnost úloh v prostředí HPC. Tyto instance podporují paralelní aplikace MPI vyžadující nízkou latenci a vysokou propustnost aplikační sítě. Pro rozšíření scénářů výpočtů a vizualizací jsou k dispozici také virtuální počítače řady [N-series](../virtual-machines/windows/sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) s grafickými procesory NVIDIA.  
> 
> 

| Služba | Popis |
| --- | --- |
| **[Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)**<br/><br/> |• Poskytuje výpočetní infrastrukturu jako službu (IaaS) pomocí technologie Microsoft Hyper-V.<br/><br/>• Umožňuje pružně zřizovat a spravovat trvalé cloudové počítače ze standardního image systému Windows Server nebo Linux, image a datového disku, který poskytnete, nebo ze služby [Azure Marketplace](https://azure.microsoft.com/marketplace/)<br/><br/>• Může se nasadit a spravovat jako [sada škálování virtuálních počítačů](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/), což umožňuje vytváření rozsáhlých služeb poskytovaných identickými virtuálními počítači a automatické škálování pro automatické zvýšení nebo snížení kapacity.<br/><br/>• Umožňuje spouštět nástroje a aplikace místního výpočetního clusteru plně v cloudu.<br/><br/> |
| **[Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/)**<br/><br/> |• Umožňuje spouštět aplikace Big Compute na instancích rolí pracovního procesu, což jsou virtuální počítače se systémem Windows Server kompletně spravované službou Azure.<br/><br/>• Umožňuje běh škálovatelných a spolehlivých aplikací s nízkou správní režií, které běží na základě modelu PaaS (platforma jako služba).<br/><br/>• Může vyžadovat další nástroje nebo vývoj, aby byla možná integrace s řešeními místního clusteru prostředí HPC. |
| **[Batch](https://azure.microsoft.com/documentation/services/batch/)**<br/><br/> |• Umožňuje spouštět rozsáhlé paralelní a dávkové úlohy prostřednictvím plně spravované služby.<br/><br/>• Poskytuje plánování úloh a automatické škálování spravovaného fondu virtuálních počítačů.<br/><br/>• Umožňuje vývojářům vytvářet a spouštět aplikace jako službu nebo povolit existující aplikace pro cloud.<br/> |

### <a name="storage-services"></a>Služby Storage
Řešení Big Compute obvykle pracuje se sadou vstupních dat a generuje data pro své výsledky. Mezi služby úložiště Azure, které se používají v řešeních pro Big Compute, patří:

* [Blob, Table, a Queue Storage](https://azure.microsoft.com/documentation/services/storage/) – umožňují spravovat velké objemy nestrukturovaných dat, dat typu NoSQL a zpráv pro pracovní postup, v uvedeném pořadí. Úložiště objektů blob můžete například použít pro velké sady technických dat nebo vstupní image či soubory médií, které vaše aplikace zpracovává. Fronty (queues) můžete v řešení použít k asynchronní komunikaci. Přečtěte si článek [Úvod do Microsoft Azure Storage](../storage/common/storage-introduction.md).
* [Azure File Storage](https://azure.microsoft.com/services/storage/files/) – sdílí společné soubory a data v Azure pomocí standardního protokol SMB, který je nutný pro některá řešení clusteru prostředí HPC.
* [Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) – Poskytuje pro cloud hyperškálovatelný systém Apache Hadoop Distributed File System, který je užitečný pro dávkovou (batch) a interaktivní analýzu v reálném čase.

### <a name="data-and-analysis-services"></a>Datové a analytické služby
Některé scénáře Big Compute zahrnují rozsáhlé datové toky nebo generují data, která potřebují další zpracování nebo analýzu. Azure nabízí několik datových a analytických služeb, včetně:

* [Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) – vytváří pracovní postupy (kanály) řízené daty, které slučují, agregují a transformují data z místních, cloudových a internetových úložišť.
* [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/) – poskytuje klíčové funkce systému správy relační databáze serveru Microsoft SQL Server prostřednictvím spravované služby.
* [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/) – nasazuje a zřizuje v cloudu clustery Apache Hadoop založené na systému Windows Server nebo Linux, které slouží ke správě, analýze a generování sestav velkých objemů dat.
* [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) – pomáhá vám vytvářet, testovat, provozovat a spravovat řešení prediktivní analýzy prostřednictvím plně spravované služby.

### <a name="additional-services"></a>Další služby
K připojení vašeho řešení Big Compute k místním prostředkům nebo prostředkům v jiných prostředích můžou být potřeba další služby Azure. Příklady obsahují:

* [Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/) – Vytváří v Azure logicky oddělený oddíl pro připojení prostředků Azure mezi sebou nebo k vašemu místnímu datovému centru. S virtuální sítí mezi různými místy mohou aplikace Big Compute přistupovat k místním datům, službám Active Directory a licenčním serverům.
* [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) – Vytváří privátní připojení mezi datovými centry společnosti Microsoft a infrastrukturou, která se nachází v místním umístění nebo v prostředí ve společném umístění. ExpressRoute poskytuje vyšší úroveň zabezpečení, větší spolehlivost, vyšší rychlost a nižší latence než typická připojení přes internet.
* [Service Bus](https://azure.microsoft.com/documentation/services/service-bus/) – poskytuje pro aplikace několik mechanismů, které umožňují komunikovat nebo vyměňovat data, ať už jsou umístěna ve službě Azure, na jiné cloudové platformě nebo v datovém centru.

## <a name="next-steps"></a>Další kroky
* Přečtěte si technické pokyny k vytvoření vlastního řešení v článku o [technických prostředcích pro Batch a prostředí HPC](big-compute-resources.md).
* Prodiskutujte své možnosti Azure s našimi partnery, mezi které patří Cycle Computing, Rescale nebo UberCloud.
* Přečtěte si informace o řešeních pro Azure Big Compute, o které se podělili [Towers Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222), [Altair](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/), [ANSYS](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/) a [d3VIEW](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088).
* Podívejte se na aktuální novinky na [blogu týmu pro Microsoft HPC a Batch](http://blogs.technet.com/b/windowshpc/) a [blogu Azure](https://azure.microsoft.com/blog/tag/hpc/).

<!--Image references-->
[parallel]: ./media/batch-hpc-solutions/parallel.png
[coupled]: ./media/batch-hpc-solutions/coupled.png
[iaas_cluster]: ./media/batch-hpc-solutions/iaas_cluster.png
[burst_cluster]: ./media/batch-hpc-solutions/burst_cluster.png
[batch_proc]: ./media/batch-hpc-solutions/batch_proc.png

