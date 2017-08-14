Cloudová řešení Azure jsou založená na virtuálních počítačích (emulaci hardwaru fyzických počítačů), které umožňují agilní balení softwarových nasazení a lepší konsolidaci prostředků než fyzický hardware. Kontejnery a ekosystém [Docker](https://www.docker.com) výrazným způsobem rozšířily možnosti vývoje, dodávek a správy distribuovaného softwaru. Kód aplikace v kontejneru je oddělený od hostitelského virtuálního počítače a dalších kontejnerů ve stejném virtuálním počítači. Tato izolace poskytuje flexibilnější možnosti vývoje a nasazení.

Azure nabízí tyto výhody Dockeru:

* [Řada](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) [různých](../articles/virtual-machines/linux/dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) způsobů, jak vytvořit hostitele Docker pro kontejnery tak, aby to odpovídalo vaší situaci.
* [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) vytváří clustery hostitelů kontejnerů s využitím orchestrátorů, jako je **Marathon** a **Swarm**.
* [Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md) a [šablony skupin prostředků](../articles/resource-group-authoring-templates.md) zjednodušují nasazování a aktualizace komplexních distribuovaných aplikací.
* Integrace se širokou škálou vlastních nebo i opensourcových nástrojů pro správu konfigurace.

A vzhledem k tomu, že můžete programově vytvářet virtuální počítače a kontejnery Linuxu v Azure, můžete taky využívat nástroje pro *orchestraci* kontejnerů a virtuálních počítačů k vytváření skupin virtuálních počítačů a k nasazování aplikací do kontejnerů Linuxu a nově i [kontejnerů Windows](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview).

Tento článek nejen popisuje tyto koncepty na základní úrovni, ale obsahuje taky spousty odkazů na další informace, kurzy a produkty, které souvisejí s využitím kontejnerů a clusterů v Azure. Pokud toto všechno už víte a chcete jenom přístup k odkazům, najdete je u [nástrojů pro práci s kontejnery](#tools-for-working-with-azure-vms-and-containers).

## <a name="the-difference-between-virtual-machines-and-containers"></a>Rozdíl mezi virtuálními počítači a kontejnery
Virtuální počítače běží uvnitř izolovaného prostředí virtualizace hardwaru, které poskytuje [hypervisor](http://en.wikipedia.org/wiki/Hypervisor). Služba [Virtual Machines](https://azure.microsoft.com/services/virtual-machines/) v Azure to všechno zpracuje za vás: Vy vytvoříte službu Virtual Machines tak, že zvolíte a nakonfigurujete operační systém, nebo tak, že nahrajete vlastní image virtuálního počítače. Služba Virtual Machines nabízí technologii prověřenou časem a „zocelenou bojem“ a navíc je dostupná celá řada nástrojů, které umožňují spravovat operační systémy a aplikace, které obsahují.  Aplikace ve virtuálním počítači jsou před hostitelským operačním systémem skryté. Z hlediska aplikace nebo uživatele na virtuálním počítači se virtuální počítač jeví jako autonomní fyzický počítač.

[Kontejnery Linuxu](http://en.wikipedia.org/wiki/LXC) a kontejnery vytvořené a hostované pomocí nástrojů Dockeru k zajištění izolace nepoužívají hypervisor. V případě kontejnerů hostitel s kontejnerem využívá procesy a funkce izolace systému souborů linuxového jádra k tomu, aby kontejneru a jeho aplikacím zpřístupnil jenom některé funkce jádra a vlastní izolovaný systém souborů. Z pohledu aplikace běžící uvnitř kontejneru se kontejner jeví jako jedinečná instance operačního systému. Aplikace v kontejneru nevidí žádné procesy ani jiné prostředky vně svého kontejneru.

V kontejneru Dockeru se využívá mnohem méně prostředků než ve virtuálním počítači. Kontejnery Dockeru používají izolaci aplikace a spouštěcí model, který nesdílí jádro hostitele Dockeru. Kontejner má mnohem nižší nároky na místo na disku, protože neobsahuje celý operační systém. Čas spuštění je podstatně kratší a požadované místo na disku je výrazně menší než v případě virtuálního počítače.
Kontejnery Windows poskytují aplikacím, které běží ve Windows, stejné výhody jako kontejnery Linux. Kontejnery Windows podporují formát image Dockeru a rozhraní API Dockeru, ale je možné je spravovat taky pomocí PowerShellu. Pro kontejnery Windows jsou dostupné dva kontejnerové moduly runtime, Windows Server Containers a Hyper-V Containers. Modul Hyper-V Containers poskytuje další úroveň izolace, protože každý kontejner hostuje v superoptimalizovaném virtuálním počítači. Další informace o používání kontejnerů Windows najdete v tématu věnovaném [kontejnerům Windows](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview). Pokud chcete začít používat kontejnery Windows v Azure, naučte se, jak [nasadit cluster Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md).

## <a name="what-are-containers-good-for"></a>K čemu jsou kontejnery dobré?

Kontejnery můžou zlepšit:

* Rychlost vývoje kódu aplikace a jeho sdílení v širokém měřítku
* Rychlost a jistotu, s jakou můžete aplikaci testovat
* Rychlost a jistotu, s jakou můžete aplikaci nasadit

Kontejnery běží na hostiteli, kterým je operační systém, a v Azure, to znamená virtuální počítač Azure. I když se vám představa kontejnerů už zamlouvá, budete pořád potřebovat infrastrukturu virtuálních počítačů, která kontejnery hostuje. Výhodou ale je, že kontejnerům je jedno, ve kterém virtuálním počítači jsou spuštěné (přestože třeba to, jestli konkrétní kontejner potřebuje ke spuštění prostředí Linuxu nebo Windows, důležité je).


## <a name="what-are-containers-good-for"></a>K čemu jsou kontejnery dobré?
Hodí se ke spoustě věcí, ale umožňují (stejně jako [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) a [Azure Service Fabric](../articles/service-fabric/service-fabric-overview.md)) vytváření distribuovaných jednoúčelových aplikací zaměřených na mikroslužby, jejichž návrh je založený na několika menších sestavitelných částech, a ne na větších komponentách s větší provázaností.

To platí hlavně ve veřejných cloudových prostředích, jako je Azure, ve kterých si virtuální počítače pronajímáte, kdy a kde chcete. Nejenom že získáte izolaci, rychlý výboj a nástroje pro orchestraci, ale můžete taky efektivněji rozhodovat a infrastruktuře aplikací.

Představte si třeba, že máte nasazení, které se skládá z 9 virtuálních počítačů Azure s velkou velikostí pro distribuovanou aplikaci s vysokou dostupností. Pokud by bylo možné nasadit komponenty této aplikace v kontejnerech, mohli byste použít jenom 4 virtuální počítače a nasadit komponenty aplikace do 20 kontejnerů pro zajištění redundance a vyrovnávání zatížení.

To je samozřejmě jenom příklad, ale pokud něco takového je možné ve vašem scénáři využít, můžete špičky využití pokrýt větším počtem kontejnerů (místo virtuálních počítačů Azure) a využít zbývající celkové zatížení procesoru mnohem efektivněji než dřív.

Navíc existuje řada scénářů, ve kterých přístup založený na mikroslužbách nefunguje. Vy sami budete nejlíp vědět, jestli vám mikroslužby a kontejnery pomůžou.

### <a name="container-benefits-for-developers"></a>Výhody kontejnerů pro vývojáře
Z obecného hlediska je jasné, že kontejnerová technologie představuje krok vpřed. Přináší ale taky konkrétnější výhody. Vezměme si třeba kontejnery Docker. Toto téma se nebude Dockeru věnovat podrobně (pokud vás to zajímá, přečtěte si [Co je Docker?](https://www.docker.com/whatisdocker/) nebo článek na [Wikipedii](http://wikipedia.org/wiki/Docker_%28software%29)), ale Docker a jeho ekosystém nabízejí mimořádné výhody jak vývojářům, tak odborníkům na IT.

Vývojáři si kontejnery Docker rychle oblíbí hlavně proto, že usnadňují využití kontejnerů Linuxu a Windows:

* Můžou pomocí jednoduchých přírůstkových příkazů vytvořit pevnou image, která se snadno nasazuje, a automatizovat vytváření těchto imagí pomocí souboru Docker.
* Tyto image můžou snadno sdílet pomocí jednoduchých příkazů typu push a pull ve stylu [gitu](https://git-scm.com/) ve [veřejných](https://registry.hub.docker.com/) nebo [privátních registrech Dockeru](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Můžou se zaměřit na izolované aplikační komponenty, ne na počítače.
* Můžou využívat širokou škálu nástrojů, které zvládají kontejnery Docker a různé základní image.

### <a name="container-benefits-for-operations-and-it-professionals"></a>Výhody kontejnerů pro provozní pracovníky a odborníky na IT
Provozní pracovníci a odborníci na IT můžou taky těžit z kombinace kontejnerů a virtuálních počítačů.

* Služby v kontejneru jsou izolované od hostitelského spouštěcího prostředí virtuálního počítače.
* Kód v kontejneru je ověřitelně identický.
* Služby v kontejneru je možné spouštět, zastavovat a rychle přesouvat mezi vývojovým, testovacím a produkčním prostředím.

Takovéto funkce (a je jich víc) jsou mimořádně zajímavé pro už zavedené firmy, ve kterých se profesionální IT organizace snaží o optimální přiřazení prostředků (včetně čistého výpočetního výkonu) k požadovaným úlohám, nejenom proto, aby vůbec zůstali ve hře, ale také aby se zvýšila spokojenost zákazníků a možnosti jejich oslovení. Malé firmy, nezávislí dodavatelé softwaru a startupy mají přesně stejný požadavek, ale pravděpodobně ho jinak formulují.

## <a name="what-are-virtual-machines-good-for"></a>K čemu jsou dobré virtuální počítače?
Virtuální počítače jsou základem cloud computingu, a to se nemění. Pokud se virtuální počítače spouští pomaleji, mají větší nároky na místo na disku a nemapují se přímo na architekturu mikroslužeb, poskytují velmi významné výhody:

1. Standardně poskytují mnohem větší výchozí ochranu zabezpečení pro hostitelský počítač.
2. Podporují všechny hlavní operační systémy a konfigurace aplikací.
3. Dlouhodobě mají ekosystémy nástrojů pro příkazy a ovládání.
4. Poskytuje spouštěcí prostředí pro hostování kontejnerů.

Tento poslední bod je důležitý, protože aplikace v kontejneru nadále vyžaduje konkrétní operační systém a typ procesoru, v závislosti na typu volání, která používá. Je důležité si pamatovat, že kontejnery se instalují do virtuálních počítačů, protože obsahují aplikace, které chcete nasadit. Kontejnery nejsou náhradou za virtuální počítače nebo operační systémy.

## <a name="high-level-feature-comparison-of-vms-and-containers"></a>Porovnání základních funkcí virtuálních počítačů a kontejnerů
Následující tabulka popisuje na velmi obecné úrovni rozdíly funkcí (bez velkého dalšího úsilí) mezi kontejnery Linux a virtuálními počítači. Všimněte si, že některé funkce můžou být více nebo méně žádoucí v závislosti na potřebách vaší aplikace. A jako u veškerého softwaru i tady platí, že dalším úsilím zajistíte vyšší podporu funkcí, hlavně v oblasti zabezpečení.

| Funkce | Virtuální počítače | Kontejnery |
|:--- | --- | --- |
| „Výchozí“ podpora zabezpečení |Do značné míry |Do menší míry |
| Vyžadovaná paměť na disku |Kompletní operační systém plus aplikace |Jenom požadavky aplikací |
| Čas potřebný ke spuštění |Podstatně delší: Spuštění operačního systému plus načítání aplikací |Podstatně kratší: Spouští se jenom aplikace, protože jádro už je spuštěné |
| Přenositelnost |Přenositelné po příslušné přípravě |Přenositelné v rámci formátu image; obvykle menší |
| Automatizace imagí |Výrazně se liší v závislosti na operačním systému a aplikacích |[Registr Dockeru](https://registry.hub.docker.com/), další |

## <a name="creating-and-managing-groups-of-vms-and-containers"></a>Vytváření a správa skupin virtuálních počítačů a kontejnerů
V této chvíli si pravděpodobně každý architekt, vývojář nebo specialista na provoz IT myslí: „Tohle všechno dokážu automatizovat sám. Není to nic jiného než datové centrum jako služba!“

Máte pravdu, může to tak být a existuje spousta systémů, z nichž řadu už možná používáte, které dokážou spravovat skupiny virtuálních počítačů Azure a vkládat vlastní kód pomocí skriptů, často pomocí [CustomScriptingExtension pro Windows](https://msdn.microsoft.com/library/azure/dn781373.aspx) nebo [CustomScriptingExtension pro Linux](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/). Můžete automatizovat nasazení Azure pomocí PowerShellu nebo skriptů rozhraní příkazového řádku Azure (a už jste to pravděpodobně i udělali).

Tyto funkce se potom často přenášejí do nástrojů, jako je [Puppet](https://puppetlabs.com/) a [Chef](https://www.chef.io/), které automatizují vytváření a konfiguraci virtuálních počítačů ve velkém měřítku. (Tady je pár odkazů na [použití těchto nástrojů v Azure](#tools-for-working-with-containers).)

### <a name="azure-resource-group-templates"></a>Šablony skupin prostředků Azure
Nedávno bylo vydané rozhraní REST API pro [správu prostředků Azure](../articles/resource-manager-deployment-model.md) a aktualizovaly se nástroje PowerShellu a rozhraní příkazového řádku Azure umožňující jeho snadné využití. Pomocí [šablon Azure Resource Manageru](../articles/resource-group-authoring-templates.md) s rozhraním API pro správu prostředků Azure můžete nasazovat, upravovat nebo opakovaně nasazovat celé topologie aplikací a využít přitom:

* [Šablony na webu Azure Portal](https://github.com/Azure/azure-quickstart-templates) (Tip: Použijte tlačítko DeployToAzure.)
* [Rozhraní příkazového řádku Azure](../articles/virtual-machines/linux/cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Moduly Azure PowerShellu](../articles/virtual-machines/linux/cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="deployment-and-management-of-entire-groups-of-azure-vms-and-containers"></a>Nasazení a správa celých skupin kontejnerů a virtuálních počítačů Azure
Existuje několik oblíbených systémů, které umožňují nasadit celé skupiny virtuálních počítačů a nainstalovat na ně Docker (nebo jiné hostitelské systémy kontejnerů Linux) jako na automatizovatelnou skupinu. Přímé odkazy najdete níž v části věnované [kontejnerům a nástrojům](#containers-and-vm-technologies). Systémů, které tuto operaci ve větší nebo menší míře zvládají, je několika a jejich seznam není úplný. V závislosti na vašich dovednostech a scénářích pro vás můžou nebo nemusejí být užitečné.

Docker má vlastní sadu nástrojů pro vytváření virtuálních počítačů ([docker-machine](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) a taky nástroj pro správu clusteru s vyrovnáváním zatížení ([swarm](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). [Rozšíření Azure Docker VM](https://github.com/Azure/azure-docker-extension/blob/master/README.md) navíc nabízí výchozí podporu pro příkaz [`docker-compose`](https://docs.docker.com/compose/), který umožňuje nasadit nakonfigurované kontejnery aplikace přes několik kontejnerů.

Můžete taky vyzkoušet [systém DCOS (Data Center Operating System) od Mesosphere](http://docs.mesosphere.com). DCOS využívá opensourcové „jádro distribuovaných systémů“ [mesos](http://mesos.apache.org/), které umožňuje využívat datové centrum jako jednu adresovatelnou službu. DCOS nabízí integrované balíčky pro několik důležitých systémů, jako je [Spark](http://spark.apache.org/) a [Kafka](http://kafka.apache.org/) (a další), a taky integrované služby, jako je [Marathon](https://mesosphere.github.io/marathon/) (systém pro řízení kontejnerů) a [Chronos](https://mesos.github.io/chronos/) (distribuovaný plánovač). Mesos vychází z poznatků zjištěných Twitterem, AirBnb a dalšími webovými společnostmi. **Swarm** jde taky využít jako orchestrační modul.

[Kubernetes](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/) je opensourcový systém pro správu skupin kontejnerů a virtuálních počítačů, který vychází z poznatků a zkušeností Googlu. Dokonce je možné využít i [Kubernetes a Weave k zajištění síťové podpory](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave).

[Deis](http://deis.com/) je opensourcová platforma jako služba (PaaS), která usnadňuje nasazení a správu aplikací na vlastních serverech. Deis vychází z Dockeru a CoreOS a poskytuje službu jednoduchou službu PaaS s pracovními postupy, které jsou inspirované Heroku. Můžete snadno [vytvořit skupinu virtuálních počítačů Azure se 3 uzly, nainstalovat Deis](../articles/virtual-machines/linux/deis-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) do Azure a potom [nainstalovat aplikaci Hello World typu Go](../articles/virtual-machines/linux/deis-cluster.md#deploy-and-scale-a-hello-world-application).

[CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html), linuxová distribuce s optimalizovanými nároky na místo, podporou Dockeru a vlastním kontejnerovým systémem [rkt](https://github.com/coreos/rkt), taky nabízí nástroj pro správu skupin kontejnerů s názvem [fleet](https://coreos.com/fleet/docs/latest/).

Ubuntu, další velice oblíbená distribuce Linuxu, zajišťuje dobrou podporu Dockeru a podporuje taky [linuxové clustery (typu LXC)](https://help.ubuntu.com/lts/serverguide/lxc.html).

## <a name="tools-for-working-with-azure-vms-and-containers"></a>Nástroje pro práci s kontejnery a virtuálními počítači Azure
Práce s kontejnery a virtuálními počítači Azure vyžaduje nástroje. Tato část obsahuje seznam jenom několika nejužitečnějších nebo nejdůležitějších koncepcí a nástrojů pro práci s kontejnery a skupinami a nástrojů pro konfiguraci a orchestraci, které se spolu s nimi využívají.

> [!NOTE]
> Tato oblast se velice rychle rozvíjí. Snažíme se, aby toto téma i všechny odkazy byly aktuální, ale zdá se, že to snad ani nejde. Témata, která vás zajímají, si proto vyhledejte!
>
>

### <a name="containers-and-vm-technologies"></a>Technologie virtuálních počítačů a kontejnerů
Některé technologie kontejnerů Linux:

* [Docker](https://www.docker.com)
* [LXC](https://linuxcontainers.org/)
* [CoreOS a rkt](https://github.com/coreos/rkt)
* [Open Container Project](http://opencontainers.org/)
* [RancherOS](http://rancher.com/rancher-os/)

Odkazy pro kontejnery Windows:

* [Kontejnery Windows](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)

Odkazy na Docker pro Visual Studio:

* [Visual Studio Tools for Docker](https://docs.microsoft.com/en-us/dotnet/core/docker/visual-studio-tools-for-docker)

Nástroje Dockeru:

* [Démon Dockeru](https://docs.docker.com/installation/#installation)
* Klienti Docker
  * [Klient Docker pro Windows v Chocolatey](https://chocolatey.org/packages/docker)
  * [Pokyny k instalaci Dockeru](https://docs.docker.com/installation/#installation)

Docker v Microsoft Azure:

* [Rozšíření Docker VM pro Linux v Azure](../articles/virtual-machines/linux/dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Uživatelská příručka k rozšíření Azure Docker VM](https://github.com/Azure/azure-docker-extension/blob/master/README.md)
* [Použití rozšíření Docker VM z rozhraní příkazového řádku Azure (Azure CLI)](../articles/virtual-machines/linux/classic/cli-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Použití rozšíření Docker VM z webu Azure Portal](../articles/virtual-machines/linux/classic/portal-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Jak použít počítač s Dockerem v Azure](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Jak použít Docker se Swarmem v Azure](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Začínáme s prostředím Docker a Compose v Azure](../articles/virtual-machines/linux/docker-compose-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Použití šablony skupiny prostředků Azure k rychlému vytvoření hostitele Docker v Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)
* [Integrovaná podpora pro `compose`](https://github.com/Azure/azure-docker-extension#11-public-configuration-keys) pro aplikace v kontejneru
* [Implementace privátního registru Docker v Azure](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Linuxové distribuce a příklady Azure:

* [CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html)

Konfigurace, správa clusterů a orchestrace kontejnerů:

* [Fleet v systému CoreOS](https://coreos.com/fleet/docs/latest/)
* Deis

  * [Vytvoření skupiny virtuálních počítačů Azure se 3 uzly, instalace Deis do Azure a spuštění aplikace Hello World typu Go](../articles/virtual-machines/linux/deis-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Kubernetes

  * [Kompletní pokyny k automatizovanému nasazení clusteru Kubernetes s využitím CoreOS a Weave](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
  * [Vizualizátor Kubernetes](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)
* [Mesos](http://mesos.apache.org/)

  * [DCOS (Data Center Operating System) od Mesosphere](https://docs.mesosphere.com/1.7/overview/design/azure-container-service/)
* [Jenkins](https://jenkins.io/) a [Hudson](http://hudson-ci.org/)

  * [Modul plug-in Jenkins agenta virtuálního počítače pro Azure](https://wiki.jenkins.io/display/JENKINS/Azure+VM+Agents+plugin)
  * [Úložiště GitHub: Plug-In Jenkins Storage pro Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)
  * [Jiný výrobce: Plug-in Hudson Slave pro Azure](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
  * [Jiný výrobce: Plug-in Hudson Storage pro Azure](https://github.com/hudson3-plugins/windows-azure-storage-plugin)
* [Azure Automation](https://azure.microsoft.com/services/automation/)

  * [Video: Postup využití Azure Automation s linuxovými virtuálními počítači](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)
* Powershell DSC pro Linux

  * [Blog: Jak využít Powershell DSC pro Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
  * [GitHub: DockerClientDSC](https://github.com/anweiss/DockerClientDSC)

## <a name="next-steps"></a>Další kroky
Podívejte se na témata [Docker](https://www.docker.com) a [Kontejnery Windows](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview).

<!--Anchors-->
[microservices]: http://martinfowler.com/articles/microservices.html
[microservice]: http://martinfowler.com/articles/microservices.html
<!--Image references-->
