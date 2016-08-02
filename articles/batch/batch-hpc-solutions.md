<properties
   pageTitle="Řešení pro Batch a prostředí HPC v cloudu | Microsoft Azure"
   description="Představuje scénáře a možnosti řešení pro dávky (batch) a vysokovýkonné výpočetní prostředí (HPC a Big Compute) ve službě Azure"
   services="batch, virtual-machines, cloud-services"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-compute"
   ms.date="04/21/2016"
   ms.author="danlep"/>

# Řešení pro Batch a prostředí HPC v cloudu Azure

Azure nabízí efektivní a škálovatelná cloudová řešení pro dávky (batch) a vysokovýkonné výpočetní prostředí (HPC) – taky známé jako *Big Compute* (provádění výpočtů ve velkém měřítku). Přečtěte si zde o úlohách řešení Big Compute a službách Azure, které je podporují, nebo přejděte přímo na [scénáře řešení](#scenarios) dále v tomto článku. Tento článek je zaměřen především na lidi, kteří mají na starosti technická rozhodnutí ve firmě, manažery pro IT a nezávislé dodavatele softwaru, ale i ostatní vývojáři a profesionálové v oblasti IT ho mohou využít k bližšímu seznámení s těmito řešeními. 

Organizace musejí řešit rozsáhlé výpočetní problémy, včetně technického návrhu a analýzy, vykreslování obrázků, komplexního modelování, simulací typu Monte Carlo a výpočtů finančních rizik. Služba Azure organizacím pomáhá s řešením těchto problémů a rozhodováním o potřebných prostředcích, rozsahu a časovém plánu. Díky službě Azure organizace můžou:

* Vytvářet hybridní řešení, která rozšiřují místní cluster prostředí HPC, aby se mohla snížit jeho zátěž odesláním náročnějších úloh na cloud

* Spouštět nástroje clusteru prostředí HPC a úlohy jen prostřednictvím služby Azure

* Používat spravované a škálovatelné služby Azure, jako je [Batch](https://azure.microsoft.com/documentation/services/batch/), ke spouštění výpočetně náročných úloh bez nutnosti nasazení a správy výpočetní infrastruktury

Azure taky vývojářům a partnerům nabízí úplnou sadu funkcí, možností architektur a vývojářských nástrojů, které umožňují vytvářet vlastní rozsáhlé pracovní postupy Big Compute. To je však nad rámec tohoto článku. Je pro vás taky připraven rostoucí partnerský ekosystém, který vám pomůže v cloudu Azure vytvářet produktivní úlohy Big Compute.


## Batch a aplikace prostředí HPC

Na rozdíl od webových aplikací a mnoha obchodních aplikací mají batche a aplikace prostředí HPC definovaný začátek a konec a mohou se spouštět podle plánu nebo na vyžádání. Běžet mohou i několik hodin nebo i déle. Většina spadá do dvou hlavních kategorií: *vnitřně paralelní* (někdy nazývané „trapně paralelní“, protože problémy, které řeší, se přímo nabízejí k tomu, aby byly řešeny paralelně na více počítačích nebo procesorech) a *úzce párované*. Další informace o těchto typech aplikací najdete v následující tabulce. Některá řešení služby Azure se hodí více k jednomu a některá k druhému typu.

>[AZURE.NOTE] U řešení pro Batch a prostředí HPC se spuštěná instance aplikace obvykle nazývá *úloha* (job). Každá úloha se pak může rozdělit na *úkoly* (tasks). A clusterované výpočetní prostředky pro aplikaci se často nazývají *výpočetní uzly*.

Typ | Vlastnosti | Příklady
------------- | ----------- | ---------------
**Vnitřně paralelní**<br/><br/>![Vnitřně paralelní][parallel] |• Jednotlivé počítače spouštějí aplikační logiku nezávisle na sobě.<br/><br/> • Přidávání počítačů umožňuje aplikaci škálovat a snižovat dobu výpočtu.<br/><br/>• Aplikace se skládá ze samostatných spustitelných souborů nebo je rozdělená do skupiny služeb, které vyvolává klient (architektura orientovaná na služby nebo aplikace SOA). |• Modelování finančních rizik<br/><br/>• Vykreslování a zpracovávání obrázků<br/><br/>• Kódování a překódování multimédií<br/><br/>• Simulace typu Monte Carlo<br/><br/>• Testování softwaru
**Úzce párované**<br/><br/>![Úzce párované][coupled] |• Aplikace vyžaduje, aby výpočetní uzly spolupracovaly nebo si vyměňovaly přechodné výsledky.<br/><br/>• Výpočetní uzly mohou komunikovat pomocí rozhraní Message Passing Interface (MPI), což je běžný komunikační protokol pro paralelní výpočty.<br/><br/>• Aplikace je citlivá na latenci sítě a šířku pásma.<br/><br/>• Výkon aplikace lze zvýšit pomocí vysokorychlostních síťových technologií, jako je například InfiniBand a přímý přístup do paměti vzdáleného počítače (RDMA). |• Modelování zásob plynu a ropy<br/><br/>• Technický návrh a analýza, například výpočet dynamiky kapaliny<br/><br/>• Fyzické simulace, například autohavárie nebo jaderné reakce<br/><br/>• Předpovědi počasí

### Důležité informace ohledně spouštění aplikací Batch a aplikací prostředí HPC v cloudu

Mnoho aplikací navržených ke spouštění v clusterech místního prostředí HPC můžete snadno migrovat do služby Azure nebo hybridního prostředí (pro více míst). Můžou zde ale existovat jistá omezení a požadavky, například:


* **Dostupnost prostředků cloudu** – v závislosti na typu výpočetních prostředků cloudu, které použijete, se možná nebudete moct spolehnout na nepřetržitou dostupnost stroje po celou dobu průběhu úlohy. Běžná technika pro zvládání možných přechodných chyb, téměř nezbytná při využívání prostředků cloudu, je používání stavu a/nebo kontrolních bodů průběhu. 


* **Přístup k datům** – techniky přístupu k datům, které jsou běžně k dispozici v rámci podnikového síťového clusteru, jako je například NFS, můžou v cloudu vyžadovat zvláštní konfiguraci, nebo může být nutné pro cloud použít jiné metody a praktiky přístupu k datům.

* **Přesun dat** – pro aplikace, které zpracovávají velké objemy dat, je třeba vytvořit strategii přesunu dat do cloudového úložiště a k výpočetním prostředkům. K tomu může být nutné použít vysokorychlostní síťové služby pro spojení mezi místy, jako je například [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/). Taky je třeba vzít v potaz omezení ukládání nebo přístupu k datům týkající se práva, norem nebo zásad.


* **Licencování** – s dodavateli komerčních aplikací se vždy poraďte ohledně licencování a dalších omezení při spouštění v cloudu. Ne všichni dodavatelé nabízejí licencování formou průběžných plateb. Možná pro své řešení budete muset navrhnout licenční server v cloudu nebo se připojit k místnímu licenčnímu serveru.


### Big Compute nebo Big Data?

Rozdíl mezi aplikacemi pro Big Compute (velký objem výpočtů) a Big Data (velký objem dat) není vždy zcela zřejmý. Některé aplikace mohou mít vlastnosti obou. Obojí zahrnuje spouštění rozsáhlých výpočtů, obvykle v clusterech počítačů. Přístupy k řešení a podpůrné nástroje se ale můžou lišit.

• **Big Compute** obvykle zahrnuje aplikace, které jsou závislé na výkonu procesoru a paměti, například aplikace pro technické simulace, modelování finančních rizik a digitální vykreslování. Clustery, na kterých běží řešení Big Compute, můžou obsahovat počítače se specializovanými vícejádrovými procesory (navrženými k provádění syrových výpočtů) a specializovaný vysokorychlostní síťový hardware k propojení počítačů.

• **Big Data** řeší problémy analýzy dat, které zahrnují velké objemy dat, která není možné spravovat na jednom počítači nebo v jednom systému správy databáze. Jedná se například o velké objemy webových protokolů nebo jiná data business intelligence. Big Data se obvykle více než o výkon procesoru opírají o kapacitu disku a vstupně-výstupní výkon. Ke správě clusteru a třídění dat můžou používat specializované nástroje, jako je například Apache Hadoop. (Informace o řešení Azure HDInsight a dalších řešeních Azure Hadoop najdete v článku [Hadoop](https://azure.microsoft.com/solutions/hadoop/).)

## Správa výpočtů a plánování úloh

Spuštěné aplikace pro Batch a prostředí HPC často zahrnují služby *správce clusteru* a *plánovače úloh*, které pomáhají spravovat clusterované výpočetní prostředky a přidělovat je aplikacím, které spouštějí úlohy. Tyto funkce můžou být řešené pomocí samostatných nástrojů nebo integrovaného nástroje nebo služby.

* **Správce clusteru** – zřizuje, uvolňuje a spravuje výpočetní prostředky (nebo výpočetní uzly). Správce clusteru může automatizovat instalaci image operačního systému a aplikací na výpočetní uzly, škálovat výpočetní prostředky podle požadavků a sledovat výkon uzlů.

* **Plánovač úloh** – určuje prostředky (například procesory nebo paměť), které aplikace potřebuje, a podmínky, při jejichž splnění bude spuštěná. Plánovač úloh organizuje frontu úloh a přiděluje jim prostředky na základě přiřazené priority nebo dalších vlastností.

U nástrojů pro clustering a plánování úloh, které jsou navržené pro clustery založené na systémech Windows nebo Linux, by měla být migrace do Azure bezproblémová . Například [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029), bezplatné řešení výpočetního clusteru pro úlohy v prostředí HPC systému Windows a Linux od společnosti Microsoft, nabízí několik možností pro spuštění ve službě Azure. Ve službě Azure můžete taky vytvářet clustery založené na Linuxu a spouštět na nich open source nástroje, jako je například Torque nebo SLURM, nebo komerční nástroje, jako je například [TIBCO DataSynapse GridServer](http://www.tibco.com/products/automation/application-development/grid-computing/gridserver) nebo [Univa Grid Engine](http://www.univa.com/products/grid-engine).

Jak uvidíte v následujících částech, výhod služeb Azure můžete taky využít ke správě výpočetních prostředků a plánování úloh bez tradičních nástrojů pro správu clusteru nebo dodatečně k nim.


## Scénáře

Prohlédněte si tři běžné scénáře spouštění úloh Big Compute ve službě Azure, které využívají existující řešení clusteru v prostředí HPC, služby Azure nebo kombinaci obojího. Uvedeny jsou klíčové faktory pro výběr jednotlivých scénářů, nejedná se ale o vyčerpávající seznam. Další informace o dostupných službách Azure, které můžete použít v rámci vašeho řešení, najdete dále v článku.

  | Scénář | Proč zvolit?
------------- | ----------- | ---------------
**Rozšíření clusteru prostředí HPC do služby Azure**<br/><br/>[![Cluster burst][burst_cluster]](./media/batch-hpc-solutions/burst_cluster.png) <br/><br/> Další informace:<br/>• [Rozšíření do služby Azure pomocí sady Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)<br/><br/>• [Nastavení hybridního výpočetního clusteru pomocí sady Microsoft HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)<br/><br/>|• Zkombinujte svou sadu [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) nebo jiný místní cluster s dalšími prostředky služby Azure v rámci hybridního řešení.<br/><br/>• Rozšiřte své úlohy Big Compute, aby se spouštěly na instancích virtuálních počítačů jako PaaS (platforma jako služba). Aktuálně jde použít jen u systému Windows Server.<br/><br/>• Přistupujte k místnímu licenčnímu serveru nebo úložišti dat pomocí volitelné virtuální sítě služby Azure.|• Máte existující cluster prostředí HPC a potřebujete další prostředky. <br/><br/>• Nechcete si kupovat a spravovat další infrastrukturu clusteru prostředí HPC.<br/><br/>• Máte přechodná náročná období nebo speciální projekty.
**Vytvoření clusteru prostředí HPC zcela ve službě Azure**<br/><br/>[![Cluster in IaaS][iaas_cluster]](./media/batch-hpc-solutions/iaas_cluster.png)<br/><br/>Další informace:<br/>• [Řešení clusteru prostředí HPC ve službě Azure](./big-compute-resources.md)<br/><br/>|• Rychle a konzistentně nasazujte své aplikace a nástroje clusteru na standardní nebo vlastní virtuální počítače modelu IaaS (infrastruktura jako služba) systému Windows nebo Linux.<br/><br/>• Spouštějte různé úlohy Big Compute pomocí řešení plánování úloh podle vlastního výběru.<br/><br/>• Použijte další služby Azure, například sítě a úložiště, k vytvoření kompletního řešení založeného na cloudu. |• Nechcete si kupovat a spravovat další infrastrukturu clusteru prostředí HPC pro Linux nebo Windows.<br/><br/>• Máte přechodná náročná období nebo speciální projekty.<br/><br/>• Potřebujete další cluster pro určité období, ale nechcete investovat do počítačů a prostoru pro jeho nasazení.<br/><br/>• Chcete přesměrovat aplikace náročné na výkon, aby byly plně spuštěny v cloudu jako služba.
**Škálování paralelní aplikace do služby Azure**<br/><br/>[![Azure Batch][batch_proc]](./media/batch-hpc-solutions/batch_proc.png)<br/><br/>Další informace:<br/>• [Základy služby Azure Batch](./batch-technical-overview.md)<br/><br/>• [Začínáme s knihovnou Azure Batch pro .NET](./batch-dotnet-get-started.md)|• Použijte při vyvíjení rozhraní API [Azure Batch](https://azure.microsoft.com/documentation/services/batch/) a horizontálně navyšujte kapacitu u mnoha úloh Big Compute, které budete spouštět na fondech virtuálních počítačů se systémem Windows nebo Linux.<br/><br/>• Využijte službu Azure ke správě nasazení a automatického škálování virtuálních počítačů, plánování úloh, zotavení po havárii, přesunu dat, správě závislosti a nasazení aplikace – bez nutnosti samostatného clusteru prostředí HPC nebo plánovače úloh.|•,Nechcete spravovat výpočetní prostředky ani plánovač úloh, ale chcete se soustředit na běh svých aplikací.<br/><br/>• Chcete přesměrovat aplikace náročné na výkon, aby byly spuštěny v cloudu jako služba.<br/><br/>• Chcete automaticky škálovat své výpočetní prostředky tak, aby odpovídaly vypočítávaným úlohám.


## Služby Azure pro Big Compute

Zde se dozvíte další informace o výpočtech, datech, sítích a souvisejících službách, které můžete kombinovat s řešeními a pracovními postupy Big Compute. Podrobné pokyny ke službám Azure naleznete v [dokumentaci](https://azure.microsoft.com/documentation/) služeb Azure. [Scénáře](#scenarios) popsané dříve v tomto článku ukazují jenom některé z mnoha možností použití těchto služeb.

>[AZURE.NOTE] Azure pravidelně zavádí nové služby, které mohou být pro váš scénář užitečné. Pokud máte dotazy, obraťte se na [partnera Azure](https://pinpoint.microsoft.com/en-US/search?keyword=azure) nebo e-mail *bigcompute@microsoft.com*.

### Výpočetní služby

Výpočetní služby Azure jsou jádrem řešení Big Compute. Různé výpočetní služby jsou vhodné pro různé scénáře. Tyto služby na základní úrovni nabízejí různé režimy pro aplikace, které běží na výpočetních instancích založených na virtuálních počítačích, které Azure poskytuje pomocí technologie Windows Server Hyper-V. Na těchto instancích může běžet celá řada standardních a vlastních nástrojů a operačních systémů Windows a Linux. Azure vám dává na výběr z různých [velikostí instancí](../virtual-machines/virtual-machines-windows-sizes.md) s různými konfiguracemi jader procesoru, paměti, kapacity disku a dalších vlastností. V závislosti na vašich potřebách můžete škálovat instance až na tisíce jader a pak vertikálně snížit kapacitu, až budete potřebovat méně prostředků.

>[AZURE.NOTE] Využijte instancí o velikostech A8-A11 ke zlepšení výkonu některých úloh prostředí HPC, včetně paralelních aplikací MPI, které vyžadují nízkou latenci a vysokou propustnost aplikační sítě. Přečtěte si více [o instancích A8, A9, A10 a A11 náročných na výpočetní výkon](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).  

Služba | Popis
------------- | -----------
**[Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/)**<br/><br/> |• Umožňuje spouštět aplikace Big Compute na instancích rolí pracovního procesu, což jsou virtuální počítače se systémem Windows Server kompletně spravované službou Azure.<br/><br/>• Umožňuje běh škálovatelných a spolehlivých aplikací s nízkou správní režií, které běží na základě modelu PaaS (platforma jako služba).<br/><br/>• Může vyžadovat další nástroje nebo vývoj, aby byla možná integrace s řešeními místního clusteru prostředí HPC.
**[Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)**<br/><br/> |• Poskytuje výpočetní infrastrukturu jako službu (IaaS) pomocí technologie Microsoft Hyper-V.<br/><br/>• Umožňuje pružně zřizovat a spravovat trvalé cloudové počítače ze standardního image systému Windows Server nebo Linux, image a datového disku, který poskytnete, nebo ze služby [Azure Marketplace](https://azure.microsoft.com/marketplace/).<br/><br/>• Může se nasadit a spravovat jako [sada škálování virtuálních počítačů](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/), což umožňuje vytváření rozsáhlých služeb poskytovaných identickými virtuálními počítači a automatické škálování pro automatické zvýšení nebo snížení kapacity.<br/><br/>• Umožňuje spouštět nástroje a aplikace místního výpočetního clusteru plně v cloudu.<br/><br/>
**[Batch](https://azure.microsoft.com/documentation/services/batch/)**<br/><br/> |• Umožňuje spouštět rozsáhlé paralelní a dávkové úlohy prostřednictvím plně spravované služby.<br/><br/>• Poskytuje plánování úloh a automatické škálování spravovaného fondu virtuálních počítačů.<br/><br/>• Umožňuje vývojářům vytvářet a spouštět aplikace jako službu nebo povolit existující aplikace pro cloud.<br/>

### Služby Storage

Řešení Big Compute obvykle pracuje se sadou vstupních dat a generuje data pro své výsledky. Mezi služby úložiště Azure, které se používají v řešeních pro Big Compute, patří:

* [Blob, Table, a Queue Storage](https://azure.microsoft.com/documentation/services/storage/) – umožňují spravovat velké objemy nestrukturovaných dat, dat typu NoSQL a zpráv pro pracovní postup, v uvedeném pořadí. Úložiště objektů blob můžete například použít pro velké sady technických dat nebo vstupní image či soubory médií, které vaše aplikace zpracovává. Fronty (queues) můžete v řešení použít k asynchronní komunikaci. Přečtěte si článek [Úvod do Microsoft Azure Storage](../storage/storage-introduction.md).

* [Azure File Storage](https://azure.microsoft.com/services/storage/files/) – sdílí společné soubory a data v Azure pomocí standardního protokol SMB, který je nutný pro některá řešení clusteru prostředí HPC.

* [Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) – poskytuje pro cloud hyperškálovatelný systém Apache Hadoop Distributed File System, který je zvláště užitečný pro dávkovou (batch) a interaktivní analýzu a analýzu v reálném čase.

### Datové a analytické služby

Některé scénáře Big Compute zahrnují rozsáhlé datové toky nebo generují data, která potřebují další zpracování nebo analýzu. Azure nabízí řešení v podobě mnoha datových a analytických služeb, jako jsou například tyto:

* [Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) – vytváří pracovní postupy (kanály) řízené daty, které slučují, agregují a transformují data z místních, cloudových a internetových úložišť.

* [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/) – poskytuje klíčové funkce systému správy relační databáze serveru Microsoft SQL Server prostřednictvím spravované služby.

* [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/) – nasazuje a zřizuje v cloudu clustery Apache Hadoop založené na systému Windows Server nebo Linux, které slouží ke správě, analýze a generování sestav velkých objemů dat.

* [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) – pomáhá vám vytvářet, testovat, provozovat a spravovat řešení prediktivní analýzy prostřednictvím plně spravované služby.

### Další služby

K připojení vašeho řešení Big Compute k místním prostředkům nebo prostředkům v jiných prostředích můžou být potřeba další služby Azure. Příklady obsahují:

* [Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/) – vytvoří logicky izolovaný oddíl ve službě Azure, který slouží k připojení prostředků Azure k místnímu datovému centru nebo jednomu klientskému počítači prostřednictvím protokolu IPSec. Umožňuje aplikacím Big Compute přístup k místním datům, službám Active Directory a licenčním serverům.

* [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) – vytvoří privátní propojení mezi datovými centry Microsoftu a infrastrukturou, která je v místním prostředí nebo společném umístění. Poskytuje vyšší zabezpečení a rychlost, větší spolehlivost a nižší latenci, než poskytuje typické připojení přes Internet.

* [Service Bus](https://azure.microsoft.com/documentation/services/service-bus/) – poskytuje pro aplikace několik mechanismů, které umožňují komunikovat nebo vyměňovat data, ať už jsou umístěna ve službě Azure, na jiné cloudové platformě nebo v datovém centru.

## Další kroky

* Přečtěte si technické pokyny k vytvoření vlastního řešení v článku o [technických prostředcích pro Batch a prostředí HPC](big-compute-resources.md).

* Prodiskutujte své možnosti služby Azure s našimi partnery, jako jsou například Cycle Computing nebo UberCloud.

* Přečtěte si informace o řešeních pro Azure Big Compute, o které se podělili [Towers Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222), [Altair](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/), a [d3VIEW](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088).

* Podívejte se na aktuální novinky na [blogu týmu pro Microsoft HPC a Batch](http://blogs.technet.com/b/windowshpc/) a [blogu Azure](https://azure.microsoft.com/blog/tag/hpc/).

<!--Image references-->
[paralelní]: ./media/batch-hpc-solutions/parallel.png
[párované]: ./media/batch-hpc-solutions/coupled.png
[iaas_cluster]: ./media/batch-hpc-solutions/iaas_cluster.png
[burst_cluster]: ./media/batch-hpc-solutions/burst_cluster.png
[batch_proc]: ./media/batch-hpc-solutions/batch_proc.png



<!--HONumber=Jun16_HO2-->


