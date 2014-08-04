<properties  umbracoNaviHide="0" pageTitle="Application Model" metaKeywords="Azure, Azure, application model, Azure application model, development model, Azure development model, hosted service, Azure hosted service, web role, worker role" description="Learn about the Azure hosted service application model. Understand core concepts, design considerations, defining and configuring your application, and scaling." linkid="dev-net-fundamentals-application-model" urlDisplayName="Application Model" headerExpose="" footerExpose="" disqusComments="1" title="Application Model" authors="" />

# Modely systému Azure pro spouštění aplikací

V rámci systému Azure je k dispozici několik různých modelů pro spouštění aplikací. Každý z nich nabízí jinou sadu služeb, takže to, který z nich si vyberete, bude vždycky záležet jenom na tom, co se snažíte udělat. Tento článek se zaměřuje na tři modely a u každého popisuje technologie a dává příklady, kdy je vhodné je používat.

## Obsah

* [Virtuální počítače](#VMachine)
* [Weby](#WebSites)
* [Cloudové služby](#CloudServices)
* [Co mám používat? Podle čeho se rozhodovat](#WhatShouldIUse)

<h2><a  id="VMachine" ></a>Virtuální počítače</h2>


Model Virtuální počítače Azure umožňuje vývojářům, lidem z IT oddělení a dalším vytvářet a používat virtuální počítače v cloudu. Tato technologie zajišťuje něco, čemu se říká *infrastruktura jako služba (IaaS)*, a může se používat řadou různých způsobů. Na [Obrázku 1](#Fig1) vidíte, z čeho se zhruba skládá.

<a name="Fig1"></a>![01_CreatingVMs](./media/fundamentals-application-models/ExecModels_01_CreatingVMs.png)

**Obrázek 1: Model Virtuální počítače Azure poskytuje přístup
Infrastruktura jako služba (IaaS).**

Jak je vidět na obrázku, virtuální počítače můžete vytvářet buď pomocí portálu pro správu systému Azure, nebo pomocí rozhraní REST API pro správu služeb systému Azure. Na portál pro správu se dá dostat ze všech oblíbených prohlížečů, včetně Internet Exploreru, Mozilla Firefoxu a Chromu. Pro rozhraní REST API poskytuje Microsoft klientské skriptovací nástroje pro systémy Windows, Linux a Macintosh. Pro toto rozhraní se ale může používat i jiný software.

Bez ohledu na to, jaký přístup k platformě použijete, musí se při vytváření nového virtuálního počítače vybrat virtuální pevný disk (VHD) pro image virtuálního počítače. Tyto virtuální pevné disky se ukládají v objektech blob systému Azure.

Začít můžete dvěma způsoby:

1.  Nahrajete do úložiště vlastní virtuální pevný disk (soubor VHD) 2.  Použijte soubory VHD od Microsoftu a jeho partnerů z galerie
    virtuálních počítačů systému Azure nebo z open source webu
    Microsoftu [VMDepot][1].

Mezi soubory VHD v galerii a na webu VMDepot najdete taky čisté image operačních systémů Microsoft a Linux i image s nainstalovanými produkty Microsoft a produkty jiných firem. Možnosti pořád přibývají. Přibývají třeba různé verze, edice a konfigurace těchto produktů:

* Windows Server
* Linux servery, jako například Suse, Ubuntu a CentOS
* SQL Server
* BizTalk Server
* SharePoint Server

Kromě VHD určíte taky velikost nového virtuálního počítače. Kompletní specifikace jednotlivých velikostí najdete [v knihovně Azure][2].

* **Velmi malé** se sdíleným jádrem a 768 MB paměti
* **Malé** s 1 jádrem a 1,75 GB paměti
* **Střední** se 2 jádry a 3,5 GB paměti
* **Velké** se 4 jádry a 7 GB paměti
* **Velmi velké** s 8 jádry a 14 GB paměti
* **A6** se 4 jádry a 28 GB paměti
* **A7** s 8 jádry a 56 GB paměti

Nakonec si zvolíte, v jakém datacentru Azure má váš nový virtuální počítač běžet (například ve Spojených státech, Evropě nebo Asii).

Od okamžiku, kdy začne virtuální počítač běžet, budete platit za každou hodinu, kdy poběží, a přestanete za něho platit, když ho odeberete.
Částka, kterou zaplatíte, nezávisí na tom, jak moc se váš virtuální počítač využívá -- odvíjí se čistě od doby, po jakou se používal. Virtuální počítač, který je úplně nevyužitý, stojí stejně jako ten, který se využívá každou minutu na maximum.

Každý spuštěný virtuální počítač má přiřazený *disk operačního systému (OS)*, který je uložený v objektu blob. Když vytvoříte virtuální počítač pomocí VHD z galerie, VHD se vám zkopíruje na disk OS vašeho virtuálního počítače. Všechny změny, které uděláte v operačním systému vašeho spouštěného virtuálního počítače, se sem uloží. Pokud třeba nainstalujete aplikaci, která něco změní v registru Windows, uloží se tato změna na disk OS vašeho virtuálního počítače. Když pak příště budete vytvářet virtuální počítač z tohoto disku OS, virtuální počítač bude dál běžet v takovém stavu, v jakém jste ho vypínali. Na soubory VHD uložené v galerii aplikuje společnost Microsoft v případě potřeby aktualizace, například opravy operačního systému. U souborů VHD na vašich vlastních discích OS jste ale za instalaci těchto aktualizací zodpovědní vy (i když je ve výchozím nastavení zapnutý Windows Update).

Běžící virtuální počítače se taky dají upravit a pak se dá udělat jejich snímek nástrojem Sysprep. Sysprep zbavuje virtuální počítač specifických informací, jako je název počítače, aby se dala VHD image použít k vytvoření dalších virtuálních počítačů se stejnou obecnou konfigurací. Tyto image jsou uložené na portálu pro správu společně s vašimi nahranými imagemi, takže se dají použít jako výchozí bod pro další virtuální počítače.

V rámci modelu Virtuální počítače se taky monitoruje hardware, na kterém se každý virtuální počítač, který vytvoříte, hostuje. Pokud fyzický server, na kterém virtuální počítač běží, selže, platforma si toho všimne a spustí stejný virtuální počítač na jiném počítači. A pokud máte tu správnou licenci, můžete si změněný soubor VHD zkopírovat ze svého disku OS někam jinam, kde ho budete chtít spouštět, například do vlastního místního datacentra nebo k jinému poskytovateli cloudových služeb.

Kromě disku OS má virtuální počítač ještě taky jeden nebo víc datových disků. I když se vám všechny budou ve virtuálním počítači zobrazovat jako připojená jednotka, obsah každého z nich je ve skutečnosti uložený v objektu blob. Každý zápis na datový disk se trvale uloží do příslušného objektu blob. Tak jako u všech ostatních objektů blob pak Azure tyto objekty blob replikuje v rámci jednoho datacentra a mezi datacentry, aby se zajistila ochrana proti selhání.

Běžící virtuální počítače se dají spravovat pomocí portálu pro správu, prostředí PowerShell a dalších skriptovacích nástrojů nebo přímo přes REST API. (Ve skutečnosti můžete všechno to, co můžete dělat přes portál pro správu, dělat taky programově přes toto API.) Partneři Microsoftu, jako například RightScale a ScaleXtreme, taky poskytují služby správy, které jsou založené na REST API.

Model Virtuální počítače Azure se může využívat nejrůznějšími způsoby. Příklady hlavních scénářů, na které se zaměřuje Microsoft:

* **Virtuální počítače pro vývoj a testování.** Vývojové skupiny běžně
  potřebují pro vytváření aplikací virtuální počítače s konkrétní
  konfigurací. Přes model Virtuální počítače Azure se dají tyto
  virtuální počítače jednoduchým a ekonomickým způsobem vytvářet,
  používat a pak odebrat, když už nejsou potřeba.
* **Spuštění aplikace v cloudu.** U některých aplikací má spouštění na
  veřejném cloudu smysl z ekonomického hlediska. Vezměme si třeba
  aplikaci s velkými výkyvy v poptávce. Samozřejmě se dá kdykoliv
  dokoupit dostatek počítačů pro vaše vlastní datacentrum, aby se v něm
  dala tato aplikace spouštět, ale většina z těchto počítačů pak bude
  nejspíš povětšinou hodně nevyužitá. Když budete tuto aplikaci spouštět
  na platformě Azure, budete si moct připlatit za další virtuální
  počítače, když je budete potřebovat, a když pak špička poptávky
  skončí, zase je povypínat. Nebo řekněme, že se vaše firma rozjíždí a
  potřebuje výpočetní prostředky podle aktuální potřeby rychle a bez
  závazku. I tady může být Azure ta správná volba.
* **Rozšíření vašeho vlastního datacentra do veřejného cloudu.**
  Virtuální síť Azure umožní vaší organizaci vytvořit virtuální síť
  (VNET), díky které se pak bude skupina virtuálních počítačů Azure
  chovat tak, jako kdyby byla součástí vaší vlastní místní sítě. Umožní
  to spouštět aplikace, jako je například SharePoint a další, v systému
  Azure a mohlo by to pro vás být levnější řešení s jednodušším
  nasazováním, než kdybyste tyto aplikace měli spouštět ve vlastním
  datacentru.
* **Zotavení po havárii.** Místo toho, abyste bez přestání platili za
  zálohovací datacentrum, který se jenom zřídka kdy používá, můžete
  využívat systém zotavení po havárii na bázi IaaS. V takovém případě
  pak platíte jenom za výpočetní prostředky, které používáte -- jenom,
  když je opravdu potřebujete. Pokud vám třeba selže vaše primární
  datacentrum, můžete si vytvořit virtuální počítače, na kterých vám v
  systému Azure poběží základní aplikace, abyste mohli fungovat, a až už
  je nebudete potřebovat, zase je vypnete.

To samozřejmě není to jediné, co se dá s virtuálními počítači Azure dělat. Zmiňujeme se tady o tom jenom proto, že je to dobrý příklad toho, jak se dají virtuální počítače v systému Azure taky využít.

### Seskupování virtuálních počítačů: Cloudové služby

Když vytvoříte nový virtuální počítač pomocí modelu Virtuální počítače Azure, můžete si vybrat, jestli má běžet jako samostatný virtuální počítač, nebo ve skupině virtuálních počítačů, které spolu běží v *cloudové službě*. (Tento koncept má podobný název jako PaaS technologie Cloudové služby v systému Azure, ale je to něco jiného, takže pozor na to, ať se vám to neplete. Jsou to dvě různé věci.) Každý samostatný virtuální počítač má přidělenou vlastní veřejnou IP adresu, zatímco pro přístup ke všem virtuálním počítačů v rámci stejné cloudové služby se používá jedna jediná veřejná IP adresa. Na [Obrázku 2](#Fig2) vidíte, jak to asi vypadá.

<a name="Fig2"></a>![02_CloudServices](./media/fundamentals-application-models/ExecModels_02_CloudServices.png)

**Obrázek 2: Každý samostatný virtuální počítač má přidělenou vlastní
veřejnou IP adresu, zatímco pro přístup k virtuálním počítačů seskupeným do cloudové služby se používá jedna jediná veřejná IP adresa.**

Pokud byste například vytvářeli virtuální počítač pro vytvoření a otestování jednoduché aplikace, použili byste k tomu nejspíš samostatný virtuální počítač. Pokud ale vytváříte vícevrstvou aplikaci s webovým rozhraním, databází a možná dokonce i s prostřední vrstvou, propojíte s největší pravděpodobností několik virtuálních počítačů do cloudové služby, jak je vidět na Obrázku 2.

Díky seskupení virtuálních počítačů do cloudové služby budete taky moct používat další možnosti. Azure zajišťuje u virtuálních počítačů ve stejné cloudové službě vyrovnávání zatížení, a to tak, že mezi ně rozděluje požadavky uživatelů. Virtuální počítače, které jsou takto propojené, můžou taky v datacentru Azure mezi sebou komunikovat přímo přes lokální síť.

Virtuální počítače ve stejné cloudové službě se dají taky seskupovat do *sad dostupnosti*. Abychom pochopili, proč je to důležité, představíme si webovou aplikaci, která běží přes několik front-endových virtuálních počítačů. Pokud jsou všechny tyto virtuální počítače nasazené na stejném fyzickém počítači nebo dokonce na několika fyzických počítačích ve stejném racku, může jediné selhání hardwaru způsobit, že budou všechny nedostupné. Pokud ale budou tyto virtuální počítače seskupené do sady dostupnosti, systém Azure je nasadí na různých místech po datacentru tak, aby žádný z nich nesdílel jediný bod selhání.

### Scénář: Používání SQL Serveru pro spouštění aplikací

Abyste si mohli udělat lepší představu o tom, jak model Virtuální počítače Azure funguje, bude užitečné podívat se trochu podrobněji na několik scénářů. Řekněme třeba, že chcete vytvořit spolehlivou a škálovatelnou webovou aplikaci, která poběží v systému Azure. Jeden ze
způsobů, jak se to dá udělat, je spustit logiku aplikace na virtuálních počítačích Azure a pak pro správu dat použít SQL Server. Na [Obrázku 3](#Fig3) vidíte, jak to asi vypadá.

<a name="Fig3"></a>![03_AppUsingSQLServer](./media/fundamentals-application-models/ExecModels_03_AppUsingSQLServer.png)

**Obrázek 3: Aplikace, která běží v rámci modelu Virtuální počítače
Azure, může pro úložiště používat SQL Server.**

V tomto příkladu se oba typy virtuálních počítačů vytváří ze standardních souborů VHD v galerii. Logika aplikace běží na Windows Serveru 2008 R2, takže vývojář z tohoto VHD vytvoří tři virtuální počítače a pak na každý z nich nainstaluje svoji aplikaci. Vzhledem k tomu, že jsou všechny tyto virtuální počítače ve stejné cloudové službě, může nastavit, že se má používat hardwarové vyrovnávání zatížení, aby se mezi ně požadavky rozprostřely. Vývojář taky vytvoří dva virtuální počítače ze souboru VHD v galerii s SQL Serverem 2012. Až se tyto počítače spustí, nakonfiguruje SQL Server v rámci každé instance tak, aby se používalo zrcadlení databáze s automatickým převzetím služeb při selhání. Není to nutné dělat (aplikace může používat taky jenom jednu instanci SQL Serveru), ale zvyšuje se tím spolehlivost.

### Scénář: Spuštění sharepointové farmy

Řekněme, že organizace chce vytvořit sharepointovou farmu, ale nechce ji spouštět ve vlastním datacentru. Může to být proto, že má v místním datacentru nedostatek prostředků, nebo třeba jenom proto, že obchodní jednotka, která potřebuje farmu vytvořit, nechce, aby se o ni staralo interní IT oddělení. V takových případech, může mít smysl používat SharePoint v rámci modelu Virtuální počítače Azure. Na [Obrázku 4](#Fig4) vidíte, jak to asi vypadá.

<a name="Fig4"></a>![04_SharePointFarm](./media/fundamentals-application-models/ExecModels_04_SharePointFarm.png)

**Obrázek 4: Model Virtuální počítače Azure umožňuje mít spuštěnou
sharepointovou farmu v cloudu.**

Sharepointová farma se skládá z několika různých částí a každá z nich běží na virtuálním počítači Azure vytvořeném z jiného virtuálního pevného disku (VHD). Bude potřeba:

* Microsoft SharePoint. V galerii jsou image se zkušební verzí nebo může
  organizace použít vlastní VHD.
* Microsoft SQL Server. SharePoint je závislý na SQL Serveru, takže
  organizace vytvoří virtuální počítače s SQL Serverem 2012 ze
  standardního VHD, který najde v galerii.
* Windows Server Active Directory. SharePoint potřebuje taky Active
  Directory, takže organizace vytvoří řadiče domény v cloudu pomocí
  image s Windows Serverem (taky z galerie). Není to úplně nutné (dají
  se taky použít místní řadiče domény). SharePoint ale dost často
  komunikuje s Active Directory, takže se takto dá zlepšit výkon.

I když to není vidět na obrázku, je tato sharepointová farma pravděpodobně připojená k místní síti přes Virtuální síť Azure. Virtuální počítače (a aplikace na nich) tak budou lidé, kteří tuto síť používají, považovat za místní.

Jak jsme si ukázali na těchto příkladech, virtuální počítače Azure můžete používat k vytváření a spouštění nových aplikací v cloudu, ke spouštění existujících aplikací nebo taky pro jiné účely. Cíl této technologie je stejný, ať už budete virtuální počítače používat k
čemukoliv: Má poskytovat univerzální základ pro veřejný cloud computing.

<h2><a  id="WebSites" ></a>Weby</h2>


Lidé používají webové technologie řadou různých způsobů. Společnost může potřebovat migrovat nebo nastavit internetový web, který by dokázal zvládat miliony návštěv týdně a který by mohl být nasazený v několika datacentrech po celém světě. Organizace, která bude web navrhovat, může ve spolupráci s týmem vývojářů vytvořit vlastní webovou aplikaci, která bude schopná zvládat tisíce uživatelů. Firemní vývojář může potřebovat nastavit aplikaci pro sledování vyúčtování výdajů v cloudu pro ověřené uživatele z podnikového adresáře Active Directory. IT konzultant může použít některou z oblíbených open source aplikací k nastavení systému pro správu obsahu pro malou firmu. Tohle všechno se dá dělat prostřednictvím modelu Virtuální počítače Azure. Vytváření a správa virtuálních počítačů, které jsou zatím ve výchozím stavu, ale vyžaduje určitou zručnost a úsilí. Pokud potřebujete implementovat web nebo webovou aplikaci, je tu jednodušší (a levnější) řešení: přístup, kterému se běžně říká Platforma jako služba (PaaS). Jak je vidět na Obrázku 5, systém Azure pro weby přístup PaaS umožňuje.

<a name="Fig5"></a>![05_Websites](./media/fundamentals-application-models/ExecModels_05_Websites.png)

**Obrázek 5: Řešení Weby Azure podporuje statické weby, oblíbené webové
aplikace a vlastní webové aplikace, které používají nejrůznější technologie.**

Weby Azure staví na řešení Cloudové služby Azure, aby bylo možné vytvořit řešení Platforma jako služba (PaaS) optimalizované pro spouštění webových aplikací. Jak je vidět na obrázku, model Weby běží na skupině jednotlivých virtuálních počítačů, na kterých může být několik webů vytvořených několika uživateli stejně jako standardní virtuální počítače, které patří jednomu konkrétnímu uživateli. Virtuální počítače jsou součástí fondu prostředků, které se spravují v rámci modelu Weby Azure. Díky tomu se dosahuje vysoké spolehlivosti a odolnosti proti chybám. S řešením není problém začít pracovat. V případě modelu Weby Azure můžou uživatelé vybírat z řady aplikací, architektur a šablon a vytvářet si weby během pár sekund. Můžou pak pomocí svých oblíbených vývojových nástrojů (WebMatrix, Visual Studio, jakýkoliv jiný editor) a možností správy zdrojového kódu nastavit kontinuální integraci a vyvíjet jako tým. Aplikace, které využívají MySQL DB, můžou používat službu MySQL, kterou pro Azure zajišťuje ClearDB, partner Microsoftu. Vývojáři můžou pomocí řešení Weby Azure vytvářet velké, škálovatelné webové aplikace. Technologie podporuje vytváření aplikací pomocí platforem ASP.NET, PHP, Node.js a Python. Aplikace můžou například používat přetrvávající relace (sticky session) a stávající webové aplikace se můžou přesunout do této cloudové platformy bez jakékoliv změny. Aplikace postavené na řešení Weby Azure můžou libovolně používat další aspekty Azure, jako například sběrnici, SQL databázi a úložiště objektů blobu. Můžete taky spouštět několik kopií aplikace v různých virtuálních počítačích a řešení Weby pak automaticky bude vyrovnávat zatížení tím, že mezi ně bude požadavky rovnoměrně rozdělovat. A protože se nové
instance Webů vytváří ve virtuálních počítačích, které už existují, spustí se nová instance aplikace velmi rychle. Je to výrazně rychlejší než čekat na vytvoření nového virtuálního počítače. Jak je vidět na [Obrázku 5](#Fig5), můžete do řešení Weby publikovat kód a další webový
obsah několika způsoby. Můžete použít technologii FTP, FTPS nebo technologii WebDeploy Microsoftu. Weby taky podporují publikování kódu ze systémů správy zdrojového kódu, včetně systému Git, GitHub, CodePlex, BitBucket, Dropbox, Mercurial, Team Foundation Server a cloudového
řešení Team Foundation Service.

<h2><a  id="CloudServices" ></a>Cloudové služby</h2>


Řešení Virtuální počítače Azure poskytuje IaaS, zatímco řešení Weby Azure poskytuje webový hosting. Třetí možnost zpracování výpočetních operací, Cloudové služby, zajišťuje přístup *Platforma jako služba (PaaS)*. Tato technologie je navržená tak, aby podporovala aplikace, které jsou škálovatelné a spolehlivé a dají se levně provozovat. Jejím účelem je taky to, aby se mohli vývojáři přestat starat o správu platformy, kterou používají, a mohli se naopak plně soustředit na svoje aplikace. Jak to funguje, vidíte na [Obrázku 6](#Fig6).

<a name="Fig6"></a>![06_CloudServices2](./media/fundamentals-application-models/ExecModels_06_CloudServices2.png)

**Obrázek 6: Cloudové služby Azure zajišťují přístup Platforma jako
služba.**

Stejně tak jako u ostatních možností systému Azure pro výpočetní operace se i pro Cloudové služby využívají virtuální počítače. Tato technologie nabízí dvě mírně odlišné možnosti používání virtuálních počítačů: instance *webových rolí* používají variantu Windows Serveru s IIS a instance *rolí pracovních procesů* používají stejnou variantu Windows Serveru ale bez IIS. Aplikace Cloudových služeb využívá kombinaci těchto dvou možností.

Například jednoduchá aplikace může používat jenom webovou roli, ale složitější aplikace může používat webovou roli pro zpracování příchozích požadavků od uživatelů a potom tyto požadavky předávat roli pracovního procesu ke zpracování. (Pro tuto komunikaci by se mohlo používat řešení Sběrnice a Fronty Azure.)

Jak je vidět na obrázku, všechny virtuální počítače v jedné aplikaci běží ve stejné cloudové službě, jak jsme si říkali, když jsme mluvili o modelu Virtuální počítače Azure. Uživatelé tak můžou aplikaci používat přes jedinou veřejnou IP adresu, přičemž požadavky se díky vyrovnávání zatížení automaticky rozdělují mezi virtuální počítače aplikace. A stejně jako u cloudových služeb vytvořených pomocí Virtuálních počítačů Azure bude platforma nasazovat virtuální počítače v aplikaci Cloudových služeb tak, aby se zabránilo používání jediného bodu selhání hardwaru.

I když aplikace běží ve virtuálních počítačích, je důležité si uvědomit, že Cloudové služby zajišťují přístup PaaS, ne přístup IaaS. Jeden ze způsobů, jak se na to dívat, je tento: V případě přístupu IaaS, například model Virtuální počítače Azure, musíte nejdřív vytvořit a nakonfigurovat prostředí, ve kterém vaše aplikace poběží, a potom do tohoto prostředí nasadit svoji aplikaci. Za správu všech těchto věcí jste zodpovědní vy (například za nasazování nových verzí operačního systému aktualizovaného o opravy v každém virtuálním počítači). V případě přístupu PaaS je to naopak tak, jako kdyby už prostředí existovalo. Stačí jenom nasadit svoji aplikaci. Správu platformy, na které běží, včetně nasazení nových verzí operačního systému, děláte vy.

V případě modelu Cloudové služby nevytváříte virtuální počítače. Místo toho můžete vytvořit konfigurační soubor, který systému Azure říká, kolik od každého typu instance potřebujete, například tři instance webových rolí a dvě instance rolí pracovních procesů, a platforma vám je vytvoří. Vyberete to, jakou velikost mají virtuální počítače mít (možnosti jsou stejné jako v případě modelu Virtuální počítače Azure), ale sami je explicitně nevytváříte. Pokud vaše aplikace potřebuje zvládat větší zátěž, můžete požádat o víc virtuálních počítačů a systém Azure tyto instance vytvoří. Pokud se zátěž sníží, můžete vypnout instance, které se vytvořily navíc, a přestat za ně platit.

Aplikace Cloudových služeb se obvykle uživatelům zpřístupňuje procesem, který má dva kroky. Vývojář nejdřív nahraje aplikaci do pracovní oblasti platformy. Pokud už bude aplikace připravená a vývojář ji bude chtít zprovoznit v produkčním prostředí, zadá na to přes portál pro správu systému Azure požadavek. Toto přepnutí mezi pracovním a produkčním prostředí se dá udělat bez prostojů, takže se dají běžící aplikace upgradovat na novou verzi, aniž by se tím nějak uživatelé vyrušovali v práci.

Řešení Cloudové služby taky zajišťují monitorování. Stejně jako řešení
Virtuální počítače Azure dokáže i toto řešení zjistit, že selhal fyzický server, a virtuální počítače, které na tomto serveru běžely, restartovat na novém počítači. Řešení Cloudové služby ale taky dokáže zjistit selhání virtuálních počítačů a aplikací, nejenom selhání hardwaru. Na rozdíl od řešení Virtuální počítače má řešení Cloudové služby uvnitř každé webové role a role pracovního procesu agenta, takže dokáže v případě, že dojde k selhání, spouštět nové instance virtuálních počítačů a aplikací.

PaaS povaha Cloudových služeb má taky další dopady. Jedním z nejdůležitějších je to, že aplikace postavené na této technologii by měly být napsané tak, aby správně běžely v případě, že by došlo k selhání instance kterékoliv webové role nebo role pracovního procesu. Aplikace Cloudových služeb by proto neměla udržovat stav v systému souborů svých vlastních virtuálních počítačů. Na rozdíl od virtuálních počítačů vytvořených pomocí řešení Virtuální počítače Azure, nejsou zápisy prováděné ve virtuálních počítačích Cloudových služeb trvalé. Neexistuje žádný datový disk Virtuální počítače. Místo toho by měla aplikace Cloudových služeb explicitně zapisovat všechny stavy do SQL databáze, objektů blob, tabulek nebo nějakého jiného externího úložiště. Pokud se aplikace vytvářejí tímto způsobem, můžou se snadněji škálovat a jsou pak odolnější vůči selhání, což je oboje pro model Cloudové služby důležité.

<h2><a  id="WhatShouldIUse" ></a>Co mám používat? Podle čeho se rozhodovat</h2>


Všechny tři modely systému Azure pro spouštění aplikací umožňují vytvářet škálovatelné a spolehlivé aplikace v cloudu. Všechny tři modely jsou si hodně podobné, který z nich byste tedy měli používat? Odpověď závisí na tom, co se snažíte udělat.

Model Virtuální počítače je nejobecnější řešení. Pokud chcete mít nad vším co největší kontrolu nebo pokud potřebujete obecné virtuální počítače, například pro vývoj a testování, je to ta nejlepší volba. Model Virtuální počítače je taky tou nejlepší volbou pro spouštění připravených místních aplikací v cloudu, jak jsme si předtím ukazovali na příkladu SharePointu. A protože virtuální počítače, které pomocí této technologie vytvoříte, můžou vypadat stejně jako vaše místní virtuální počítače, je taky pravděpodobné, že to bude ta nejlepší volba pro zotavení po havárii. Ale něco za něco. S velkým výkonem je spojená taky velká zodpovědnost -- u IaaS se vyžaduje, abyste převzali část práce, která souvisí se správou.

Model Weby je tou správnou volbou, pokud chcete vytvořit jednoduchý web. To platí hlavně tehdy, když je váš web založený na existující aplikaci, jako je Joomla, WordPress nebo Drupal. Model Weby je taky dobrá volba pro vytváření webových aplikací s nízkou úrovní správy i takových, které musí být škálovatelné, nebo pro přesunutí existující webové aplikace IIS do veřejného cloudu. Umožňuje taky rychlé nasazení. Nová instance vaší aplikace může začít běžet téměř okamžitě, zatímco nasazení nového virtuálního počítače buď pomocí modelu Virtuální počítače, nebo pomocí modelu Cloudové služby může trvat několik minut.

Model Cloudové služby byl počáteční model spouštění aplikací, který se v rámci systému Azure poskytoval, a v jeho případě jde výslovně o PaaS přístup. Přístup PaaS se s webovým hostingem poměrně kryje, model Cloudové služby se ale v některých důležitých ohledech od modelu Weby liší, například v tomto:

* Na rozdíl od modelu Weby vám model Cloudové služby poskytuje přístup
  pro správu k virtuálním počítačům vaší aplikace. To vám umožní
  nainstalovat libovolný software, který vaše aplikace potřebuje, což v
  případě modelu Weby možné není.
* Vzhledem k tomu, že model Cloudové služby nabízí jak webové role, tak
  i role pracovních procesů, je to lepší volba, než model Weby pro
  vícevrstvé aplikace, které pro svoji obchodní logiku potřebují
  samostatné virtuální počítače.
* Model Cloudové služby poskytuje samostatné pracovní a produkční
  prostředí, takže aktualizace aplikací probíhají v porovnání s modelem
  Weby o něco hladčeji.
* Na rozdíl od modelu Weby můžete používat síťové technologie, například
  Virtuální síť Azure a Azure Connect, pro připojení místních počítačů k
  aplikacím Cloudových služeb.
* Model Cloudové služby vám umožní připojovat se pomocí Vzdálené plochy
  přímo k virtuálním počítačům aplikace. Něco takového není při
  používání modelu Weby možné.

Vzhledem k tomu, že model Cloudové služby znamená přístup PaaS, nabízí taky několik výhod oproti modelu Virtuální počítače Azure. Automaticky se za vás dělají další úkoly správy, například nasazení aktualizací operačního systému, takže budete mít nejspíš nižší provozní náklady než v případě přístupu IaaS u modelu Virtuální počítače Azure.

Všechny tři modely spouštění aplikací systému Azure mají svoje výhody a nevýhody. Abyste se mohli co nejlíp rozhodnout, musíte pochopit, jak fungují, a vědět, čeho se snažíte dosáhnout. Pak dokážete zvolit ten, který je pro vás nejvhodnější.

Jsou situace, kdy neexistuje jeden nevhodnější model spouštění aplikací. Ideální je pak možnosti kombinovat. Řekněme třeba, že vytváříte aplikaci, kde by se vám líbilo mít výhody správy webových rolí modelu Cloudových služeb, musíte ale taky kvůli kompatibilitě a výkonu používat standardní SQL Server. V takovém případě je nejlepší modely spouštění aplikací kombinovat, jak je vidět na [Obrázku 7](#Fig7).

<a name="Fig7"></a>![07_CombineTechnologies](./media/fundamentals-application-models/ExecModels_07_CombineTechnologies.png)

**Obrázek 7: Jedna aplikace může používat víc modelů spouštění
aplikací.**

Jak vidíte na obrázku, virtuální počítače modelu Cloudových služeb běží v samostatné cloudové službě odděleně od virtuálních počítačů modelu Virtuální počítače. Přesto spolu dokážou docela efektivně komunikovat, takže někdy je nejlepší vytvářet aplikace tímto způsobem.

Azure poskytuje různé modely spouštění aplikací, protože cloudové platformy musí podporovat řadu různých scénářů. Každý, kdo chce efektivně využívat tuto platformu (a pokud jste článek dočetli až sem, pravděpodobně jste to i vy), musí jednotlivé modely pochopit.



[1]: http://vmdepot.msopentech.com/
[2]: http://msdn.microsoft.com/en-us/library/windowsazure/dn197896.aspx