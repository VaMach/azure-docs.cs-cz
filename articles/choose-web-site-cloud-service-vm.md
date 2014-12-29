<properties linkid="manage-scenarios-choose-web-app-service" urlDisplayName="Web Options for Azure" pageTitle="Azure Web Sites, Cloud Services and Virtual Machines comparison" metaKeywords="Cloud Services, Virtual Machines, Web Sites" description="Learn when to use Azure Web Sites, Cloud Services, and Virtual Machines for hosting web applications. Review a feature comparison." metaCanonical="" services="web-sites,virtual-machines,cloud-services" documentationCenter="" title=" Cloud Services" authors="jroth" solutions="" manager="paulettm" editor="mollybos" />

# Porovnání modelů spouštění aplikací Weby, Cloudové služby a Virtuální počítače systému Azure

Azure nabízí několik způsobů, jak hostovat vaše webové aplikace, například pomocí modelů spouštění aplikací [Weby][1], [Cloudové služby][2] a [Virtuální počítače][3]. Když si tyto různé možnosti projdete, nemusí vám být úplně jasné, který z nich bude nejlíp vyhovovat vašim potřebám, nebo nemusíte mít jasno v některých konceptech, například to, jaký je rozdíl mezi IaaS a PaaS. Tento článek vám pomůže pochopit možnosti, které máte, a pomůže vám správně se rozhodnout pro váš webový scénář. Všechny tři možnosti vám sice umožní rychle zprovoznit vysoce škálovatelné webové aplikace v systému Azure, jsou mezi nimi ale rozdíly, které vám můžou při rozhodování pomoct.

V mnoha situacích je nejvhodnějším modelem model Weby Azure. Nabízí jednoduché a flexibilní možnosti nasazení a správy a dokáže hostovat objemné weby. Můžete rychle vytvořit nový web pomocí některého z oblíbených softwarových produktů, jako je WordPress, z galerie webových aplikací, nebo můžete přesunout stávající web do modelu Weby Azure. Pomocí sady [Azure WebJobs SDK][4] (aktuálně ve verzi Preview) můžete taky přidat zpracování úloh na pozadí.

Máte taky možnost hostovat webové aplikace v rámci modelu Cloudové služby Azure nebo Virtuální počítače Azure. Tyto možnosti jsou dobrá volba, pokud vaše webová vrstva vyžaduje další úroveň kontroly a přizpůsobení, které tyto možnosti poskytují. Tato zvýšená kontrola je tady ale za cenu větší složitosti při vývoji, správě a nasazení. Na následujícím obrázku se můžete podívat, jaké kompromisy musíte dělat v každém z těchto tří modelů.

![ChoicesDiagram](./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_2.png)

Weby se snadněji nastavují, spravují a monitorují, ale budete mít míň možností konfigurace. Klíčové je toto zjištění: Pokud nemáte závažný důvod umístit front-end svého webu do modelu Cloudových služeb nebo Virtuálních počítačů, použijte model Weby Azure. Ve zbytku dokumentu najdete informace, které potřebujete, abyste se mohli informovaně rozhodnout. To zahrnuje:

* [Scénáře](#scenarios)
* [Shrnutí služeb](#services)
* [Porovnání funkcí](#features)

## <a name="scenarios"></a>Scénáře

### Jsem majitel malé firmy a potřebuju najít levný způsob, jak hostovat náš web, ale tak, aby se počítalo s budoucím růstem firmy

Skvělým řešením pro takovou situaci je řešení Weby Azure, protože ho můžete začít používat zadarmo a další funkce můžete přidat, až je budete potřebovat. Součástí každého bezplatného webu je například doména, kterou poskytuje systém Azure (*vaše_společnost*.azurewebsites.net). Až budete chtít začít používat vlastní doménu, můžete si tuto možnost přidat jenom za 9,80 USD za měsíc (cena platná k lednu 2014). Existuje mnoho dalších služeb a možností škálování, které umožňují vyvíjet web s tím, jak poroste poptávka uživatelů. Díky využívání modelu **Weby Azure** můžete:

* Začít s bezplatnou vrstvou a potom si služby a funkce podle potřeby
  rozšířit
* Pomocí Galerie aplikací rychle nastavit oblíbené webové aplikace, jako
  je WordPress.
* Přidat si do aplikace podle potřeby další služby a funkce Azure
* Zabezpečit si web používáním HTTPS s použitím certifikátu dodaného k
  názvu domény *vaše_společnost*.

### Jsem webový nebo grafický návrhář a chci navrhnout a vytvořit weby pro svoje zákazníky

Pro webové vývojáře má model Weby Azure všechno, co potřebují k vytváření důmyslných webových aplikací. Model Weby nabízí těsnou integraci s nástroji, jako je Visual Studio a SQL databáze. Díky využívání modelu **Weby Azure** vývojáři můžou:

* Používat nástroje příkazového řádku pro [automatizované úkoly][5]
* Pracovat s oblíbenými jazyky, například [.Net][6], [PHP][7],
  [Node.js][8] a [Python][9]
* Vybrat si tři různé úrovně škálování pro škálování na velmi vysoké
  kapacity
* Využít integrace s dalšími službami Azure, jako například [SQL
  databáze][10], [Sběrnice][11] a [Úložiště][12], nebo partnerských
  nabídek z [Azure Store][13], například MySQL a MongoDB.
* Využít integrace s nástroji, jako je Visual Studio, Git, WebMatrix,
  WebDeploy, TFS a FTP

### Migruji si vícevrstvou aplikaci s webovým front-endem do cloudu

Pokud provozujete vícevrstvou aplikaci, například webový server, který komunikuje s databázovým serverem za účelem ukládání a načítání dat webu, máte v systému Azure několik možností. Můžete třeba, co se týče architektury, používat model Weby, Cloudové služby a Virtuální počítače. První model, **Weby**, je dobrá volba pro webovou vrstvu vašeho řešení: Dá se používat s SQL databází Azure a může se tak vytvořit dvouvrstvá architektura. Model Weby taky umožňuje spouštět procesy na pozadí nebo procesy, které běží dlouho, pomocí sady Azure WebJobs SDK Preview. Pokud potřebujete složitější architekturu nebo flexibilnější možnosti škálování, bude lepší volbou model Cloudové služby nebo Virtuální počítače.

Model **Cloudové služby** vám umožňuje:

* Hostovat služby webu, prostřední vrstvy a služby back-end na
  škálovatelných webových rolích a rolích pracovních procesů
* Hostovat jenom služby prostřední vrstvy a back-endu na rolích
  pracovních procesů a front-end nechat v modelu Weby Azure
* Škálovat služby front-endu a back-endu nezávisle na sobě

Model **Virtuální počítače** vám umožňuje:

* Snadněji migrovat vysoce přizpůsobená prostředí jako image virtuálního
  počítače
* Spouštět software nebo služby, které se nedají konfigurovat v rámci
  modelů Weby nebo Cloudové služby

### Moje aplikace potřebuje k běhu vysoce přizpůsobená prostředí systému Windows nebo Linux

Pokud vaše aplikace vyžaduje složitou instalaci nebo konfiguraci softwaru a operačního systému, bude asi nejlepším řešením model Virtuální počítače. Díky využívání modelu **Virtuální počítače** můžete:

* +Použít galerii virtuálních počítačů ke spuštění s operačním systémem,
  jako je například Windows nebo Linux, a pak systém přizpůsobit
  požadavkům vaší aplikace
* Vytvořit a nahrát vlastní image existujícího místního serveru, který
  má běžet na virtuálním počítači v systému Azure

### Můj web používá open source software a chci ho hostovat v systému Azure

Každý z těchto tří modelů vám umožní hostovat open source jazyky a architektury. Model **Cloudové služby** vyžaduje, abyste pomocí úloh při spuštění nechali nainstalovat a nakonfigurovat veškerý požadovaný open source software, který běží na Windows. Pomocí modelu **Virtuální počítače** nainstalujete a nakonfigurujete software na imagi počítače, a ta může být buď s Windows nebo s Linuxem. Pokud je vaše open source architektura v rámci modelu Weby podporovaná, nabízí jednodušší způsob, jak hostovat tyto typy aplikací, protože Weby se můžou automaticky konfigurovat s jazyky a architekturami, které vaše aplikace potřebuje. Model **Weby** vám umožňuje:

* Používat řadu oblíbených open source jazyků, například [.NET][6],
  [PHP][7], [Node.js][8] a [Python][9].
* Nastavit WordPress, Drupal, Umbraco, DNN a řadu dalších webových
  aplikací třetích stran
* Migrovat existující aplikaci nebo vytvořit novou z Galerie aplikací

### Mám podnikovou aplikaci, která se potřebuje připojovat k podnikové síti

Pokud chcete vytvořit podnikovou aplikaci, mohl by váš web vyžadovat přímý přístup ke službám nebo datům v podnikové síti. To je možné v modelech **Weby**, **Cloudové služby** a **Virtuální počítače**. Přístupy, které můžete použít, se navzájem liší. Například v tomto:

* Model Weby se může bezpečně připojovat k místním prostředkům
  prostřednictvím používání Propojovací sběrnice. Umožňuje to službám v
  podnikové síti provádět úlohy jménem webové aplikace, aniž by se
  všechno muselo přesouvat do cloudu nebo by se musela nastavovat
  virtuální síť.
* Modely Cloudové služby a Virtuální počítače můžou využívat výhod
  řešení Virtuální síť. Ve skutečnosti řešení Virtuální síť umožňuje
  počítačům, které běží v systému Azure, připojit se k místní síti.
  Systém Azure se pak stane rozšířením vašeho firemního datacentra.

### Chci hostovat REST API nebo webovou službu pro mobilní klienty

Webové služby, které používají HTTP, umožňují podporovat širokou škálu klientů, včetně mobilních klientů. Architektury, jako je ASP.NET Web API, se integrují s Visual Studiem, aby se daly jednodušším způsobem vytvářet a používat služby REST. Tyto služby jsou dostupné z webového koncového bodu, takže je pro podporu tohoto scénáře možné v systému Azure použít jakoukoliv metodu webového hostingu. Model **Weby** je ale skvělá volba pro hostování rozhraní REST API. Díky využívání modelu Weby můžete:

* Rychle vytvořit weby pro hostování webové služby HTTP v jednom z
  globálně distribuovaných datacenter systému Azureâ
* Migrovat existující služby nebo vytvářet nové, případně s využitím
  rozhraní ASP.NET Web API ve Visual Studiu
* Dosáhnout plnění podmínek smlouvy SLA pro dostupnost s jedinou
  instanci nebo škálovat na větší počet vyhrazených počítačů
* Používat publikovaný web k poskytování rozhraní REST API všem klientům
  HTTP, včetně mobilních klientů

## <a name="services"></a>Shrnutí služeb

Model [Weby Azure][1] umožňuje v systému Azure rychle vytvářet vysoce škálovatelné weby. Pomocí portálu systému Azure nebo nástrojů příkazového řádku můžete nastavit web s oblíbenými jazyky, jako je .NET, PHP, Node.js a Python. Podporované architektury jsou už nasazené a nemusí se v souvislosti s nimi dál nic instalovat. Galerie modelu Weby Azure obsahuje mnoho aplikací třetích stran, jako například Drupal a WordPress, ale taky vývojové architektury, jako Django a CakePHP. Po vytvoření webu můžete buď migrovat existující web, nebo vytvořit úplně nový web. Model Weby eliminuje potřebu spravovat fyzický hardware a nabízí taky několik možností škálování. Můžete přejít ze sdíleného víceklientského modelu na standardní režim, kde vyhrazené počítače zpracovávají příchozí provoz. Model Weby vám taky umožní integraci s dalšími službami Azure, například SQL databáze, sběrnice a úložiště. Pomocí sady [Azure WebJobs SDK][4] Preview můžete taky přidat zpracování
úloh na pozadí. Stručně řečeno, model Weby Azure umožňuje jednodušší
zaměření na vývoj aplikací, protože podporuje širokou škálu jazyků, open source aplikací a metodik nasazení (FTP, Git, Web Deploy nebo TFS). Pokud nemáte nějaké specifické požadavky, které by vyžadovaly model Cloudové služby nebo Virtuální počítače, bude pro vás nejspíš nejlepší volbou model Weby Azure.

Model [Cloudové služby][2] vám umožní vytvářet vysoce dostupné, škálovatelné webové aplikace v prostředí Platforma jako služba (PaaS) se spoustou možností. Na rozdíl od modelu Weby se nejdřív cloudová služba vytvoří ve vývojovém prostředí, jako je Visual Studio, než se nasadí do Azure. Architektury, jako je PHP, vyžadují vlastní kroky nebo úlohy nasazení, které instalují architekturu při spouštění role. Hlavní předností modelu Cloudových služeb je schopnost podporovat složitější vícevrstvé architektury. Jedna cloudová služba se může skládat z front-endové webové role a jedné nebo několika rolí pracovních procesů. Každá vrstva se dá škálovat samostatně. K dispozici je taky zvýšená úroveň kontroly nad infrastrukturou webových aplikací. K počítačům, na kterých běží instance rolí, se můžete například připojovat přes vzdálenou plochu. Pro složitější změny konfigurace počítače a služby IIS, včetně úloh, které vyžadují kontrolu správce, taky můžete použít skripty, které se pak spouští při spuštění role.

Model [Virtuální počítače][3] vám umožní spouštět webové aplikace na virtuálních počítačích v systému Azure. Tato schopnost se taky označuje jako přístup Infrastruktura jako služba (IaaS). Nové počítače se systémem Windows Server nebo Linux můžete vytvářet prostřednictvím portálu anebo nebo můžete nahrát image existujícího virtuálního počítače. Model Virtuální počítače vám dává maximální kontrolu nad operačním systémem, konfigurací a nainstalovaným softwarem a službami. Je to dobrá volba pro rychlou migraci složitějších místních webových aplikací do cloudu, protože počítače se dají přesouvat jako celek. Pomocí modelu Virtuální sítě můžete taky tyto virtuální počítače připojovat k místním podnikovým sítím. Stejně jako v případě modelu Cloudové služby budete mít k těmto počítačům vzdálený přístup a budete moct dělat změny konfigurace na úrovni správy. Na rozdíl od modelů Weby a Cloudové služby ale musíte spravovat image svých virtuálních počítačů a aplikační architekturu úplně odděleně od úrovně infrastruktury. Jedním ze základních příkladů je to, že budete muset pro operační systém použít svoje vlastní opravy.

## <a name="features"></a>Porovnání funkcí

Následující tabulka porovnává možnosti modelů Weby, Cloudové služby a Virtuální počítače a může vám tak pomoct při výběru modelu, který pro vás bude nejlepší. Položky s vysvětlivkami jsou pak dovysvětlené v poznámkách pod tabulkou.

<table  cellspacing="0" border="1">
<tr>
   <th  align="left" valign="middle">Funkce</th>

   <th  align="left" valign="middle">Weby</th>

   <th  align="left" valign="middle">Cloudové služby (webové role)</th>

   <th  align="left" valign="middle">Virtuální počítače</th>

</tr>

<tr>
   <td  valign="middle"><p>Přístup ke službám, například služba sběrnice, úložiště, SQL databáze</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Hostování webu nebo vrstvy webových služeb vícevrstvé architektury</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Hostování prostřední vrstvy vícevrstvé architektury</p>
</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Integrovaná podpora pro MySQL-as-a-service</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X <sup>1</sup>
</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Podpora pro ASP.NET, klasické ASP, Node.js, PHP, Python</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Škálování na větší počet instancí bez opakovaného nasazení</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X <sup>2</sup>
</td>

</tr>

<tr>
   <td  valign="middle"><p>Podpora pro SSL</p>
</td>

   <td  valign="middle">X <sup>3</sup>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Integrace s Visual Studiem</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Vzdálené ladění</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Nasazení kódu s TFS</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Nasazení kódu s GIT, FTP</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Nasazení kódu s Web Deploy</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle"><sup>4</sup>
</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Podpora pro WebMatrix</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Téměř okamžité nasazení</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

   <td  valign="middle" />

</tr>

<tr>
   <td  valign="middle"><p>Instance sdílejí obsah a konfiguraci</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

   <td  valign="middle" />

</tr>

<tr>
   <td  valign="middle"><p>Škálování na větší počítače bez nutnosti opakovaného nasazení</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

   <td  valign="middle" />

</tr>

<tr>
   <td  valign="middle"><p>Několik prostředí nasazení (produkční a pracovní)</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

</tr>

<tr>
   <td  valign="middle"><p>Izolace sítě pomocí řešení Virtuální síť Azure</p>
</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Podpora pro Správce provozu Azure</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Přístup k serverům pomocí vzdálené plochy</p>
</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Schopnost definovat/spouštět úlohy při spuštění</p>
</td>

   <td  valign="middle" />

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Automatická správa aktualizací OS</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

</tr>

<tr>
   <td  valign="middle"><p>Integrované monitorování koncových bodů</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

</tr>

<tr>
   <td  valign="middle"><p>Jednoduché a bezproblémové přepínání mezi platformami (32bitová/64bitová)</p>
</td>

   <td  valign="middle">X</td>

   <td  valign="middle">X</td>

   <td  valign="middle" />

</tr>

</table>

<sup>1</sup> Webové role nebo role pracovních
procesů můžou integrovat MySQL-as-a-servis prostřednictvím nabídek ClearDB, ale ne jako součást pracovního postupu portálu pro správu.

<sup>2</sup> Model Virtuální počítače sice umožňuje škálování na větší počet instancí, ale služby, které na těchto počítačích běží, musí být nastavené, aby se toto škálování na víc instancí dalo zvládnout. Musí být nakonfigurovaný další nástroj pro vyrovnávání zatížení, který by rozděloval požadavky rovnoměrně mezi jednotlivé počítače. Nakonec se musí vytvořit Skupiny vztahů pro všechny počítače, které se účastní stejné role, aby byly chráněné před souběžným restartování při údržbě nebo selhání hardwaru.

<sup>3</sup> V případě modelu Weby se zabezpečení SSL pro vlastní domény podporuje jenom pro standardní režim. Další informace o používání SSL s modelem Weby najdete v tématu [Konfigurace certifikátu SSL pro model Weby Azure][14].

<sup>4</sup> Metodika nasazení Web Deploy je podporovaná pro cloudové služby při nasazování do rolí s jednou instancí. Role produkčního prostředí musí mít několik instancí, aby se splňovaly podmínky smlouvy Azure SLA. Web Deploy tak není vhodným mechanismem nasazení pro cloudové služby v produkčním prostředí.



[1]: http://go.microsoft.com/fwlink/?LinkId=306051
[2]: http://go.microsoft.com/fwlink/?LinkId=306052
[3]: http://go.microsoft.com/fwlink/?LinkID=306053
[4]: http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs
[5]: http://www.windowsazure.com/en-us/documentation/scripts/?services=web-sites
[6]: http://www.windowsazure.com/en-us/develop/net/
[7]: http://www.windowsazure.com/en-us/develop/php/
[8]: http://www.windowsazure.com/en-us/develop/nodejs/
[9]: http://www.windowsazure.com/en-us/develop/python/
[10]: http://www.windowsazure.com/en-us/documentation/services/sql-database/
[11]: http://www.windowsazure.com/en-us/documentation/services/service-bus/
[12]: http://www.windowsazure.com/en-us/documentation/services/storage/
[13]: http://www.windowsazure.com/en-us/gallery/store/
[14]: http://www.windowsazure.com/en-us/develop/net/common-tasks/enable-ssl-web-site/<!--HONumber=2-->
