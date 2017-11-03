# <a name="compute"></a>Compute
Azure umožňuje nasadit a monitorovat kódu aplikace běžících v rámci datového centra společnosti Microsoft. Když vytvoříte aplikaci a spusťte ho na Azure, kód a konfigurace společně se nazývá Azure hostovaná služba. Hostované služby se dají snadno spravovat, škálovat nahoru a dolů, překonfigurujte a aktualizovat nové verze kódu vaší aplikace. Tento článek zaměřuje na Azure hostovaná služba aplikačního modelu.<a id="compare" name="compare"></a>

## Obsah<a id="_GoBack" name="_GoBack"></a>
* [Výhody modelu aplikace Azure][Azure Application Model Benefits]
* [Hostovaná služba základní koncepty][Hosted Service Core Concepts]
* [Aspekty návrhu hostované služby][Hosted Service Design Considerations]
* [Návrh aplikace pro škálování][Designing your Application for Scale]
* [Hostovaná služba definice a konfigurace][Hosted Service Definition and Configuration]
* [Soubor definice služby][The Service Definition File]
* [Konfigurační soubor služby][The Service Configuration File]
* [Vytvoření a nasazení v hostované službě][Creating and Deploying a Hosted Service]
* [Odkazy][References]

## <a id="benefits"></a>Výhody modelu aplikace azure
Když nasadíte aplikaci jako hostované služby, Azure vytvoří jednu nebo více virtuálních počítačů (VM) obsahující kódu vaší aplikace a spustí virtuální počítače na fyzické počítače, které se nacházejí v jedné z datových center Azure. Jako požadavky na aplikace hostované zadejte datové centrum, nástroj pro vyrovnávání zatížení distribuuje tyto žádosti stejnou měrou na virtuálních počítačích. Když je vaše aplikace hostována v Azure, získá tři klíčové výhody:

* **Vysoká dostupnost.** Vysoká dostupnost znamená, že Azure zajistí, aby vaše aplikace běží co nejvíce a schopné reagovat na požadavky klientů. Pokud vaše aplikace ukončí (z důvodu neošetřené výjimky, např.), pak Azure to zjistí a automaticky znovu spusťte aplikaci. Pokud vaše aplikace běží na počítač v prostředí nějaký druh selhání hardwaru, pak Azure bude také to zjistit a automaticky vytvořit nový virtuální počítač na jiný fyzický počítač pracovní a spusťte z vašeho kódu. Poznámka: Aby vaše aplikace a získat smlouvy o úrovni služeb společnosti Microsoft k dispozici 99,95 %, musíte mít aspoň dva virtuální počítače se systémem kódu aplikace. To umožňuje jeden virtuální počítač ke zpracování požadavků klientů při Azure přesune kódu z selhání virtuálního počítače do nové, dobrý virtuálního počítače.
* **Škálovatelnost.** Azure vám umožní snadno a dynamicky měnit počet virtuálních počítačů s kódu aplikace pro zpracování skutečné zátěže je uskutečnění ve vaší aplikaci. To umožňuje upravit aplikaci pro zatížení, které vaši zákazníci jsou umístění na něm při platícího pouze pro virtuální počítače, musíte, když je budete potřebovat. Pokud chcete změnit počet virtuálních počítačů, odpoví Azure v rámci minut, což umožňuje dynamicky měnit počet virtuálních počítačů spuštěných tak často, podle potřeby.
* **Možnosti správy.** Protože Azure je platforma jako služba (PaaS) nabídky, spravuje infrastrukturu (samotného hardwaru, elektřiny a sítě) vyžaduje, aby tyto počítače se systémem. Azure také spravuje platformy, zajištění aktuální operační systém s všechny na správný opravy a aktualizace zabezpečení, a také aktualizace součástí, jako je rozhraní .NET Framework a Internet Information Server. Protože všechny virtuální počítače se systémem Windows Server 2008, Azure poskytuje další funkce, například monitorování diagnostiky, podpora vzdálené plochy, brány firewall a konfigurace úložiště certifikátů. Všechny tyto funkce jsou k dispozici na žádné peníze navíc. Ve skutečnosti při spuštění aplikace v Azure, licence operační systém (OS) systému Windows Server 2008 je součástí sady. Vzhledem k tomu, že všechny virtuální počítače se systémem Windows Server 2008, kód, který běží na systému Windows Server 2008 funguje správně, při spuštění v Azure.

## <a id="concepts"></a>Hostované služby základní koncepty
Pokud je vaše aplikace je nasazená jako služby hostované v Azure, běží jako jeden nebo více *role.* A *role* jednoduše odkazuje na soubory aplikace a konfigurace. Můžete definovat jednu nebo více rolí pro vaši aplikaci, každý s vlastní sadou soubory aplikace a konfigurace. Pro každou roli v aplikaci, můžete zadat počet virtuálních počítačů, nebo *instance rolí*, ke spuštění. Následující obrázek zobrazuje dva jednoduché příklady aplikace modelován jako hostovanou službu pomocí rolí a instancí rolí.

##### <a name="figure-1-a-single-role-with-three-instances-vms-running-in-an-azure-data-center"></a>Obrázek 1: Jedné role pomocí tří instancí (VM) spuštěné v datové centrum Azure
![Bitové kopie][0]

##### <a name="figure-2-two-roles-each-with-two-instances-vms-running-in-an-azure-data-center"></a>Obrázek 2: Dvě role, každý s dvě instance (VM), spuštěné v datové centrum Azure
![Bitové kopie][1]

Instance role obvykle zpracování žádosti internetových klientů zadávání datového centra prostřednictvím co je volána *vstupní koncový bod*. Jedné role může mít 0 nebo více vstupních koncových bodů. Každý koncový bod určuje protokol (HTTP, HTTPS nebo TCP) a portu. Je běžné při konfiguraci role má dva vstupní koncové body: HTTP naslouchá na portu 80 a HTTPS naslouchání na portu 443. Následující obrázek znázorňuje příklad dva různé role s jinou vstupních koncových bodů, které jsou požadavky klientů na ně odkazovat.

![Bitové kopie][2]

Pokud vytvoříte hostovanou službu v Azure, je přiřazen veřejně adresovatelné IP adresu, který můžou klienti používat k přístupu. Při vytváření hostované služby je nutné vybrat také předponu adresy URL, který je namapovaný na tuto IP adresu. Tato předpona musí být jedinečný, jako jsou v podstatě rezervování *předponu*. adresa URL cloudapp.net tak, že nikdo jiný ji může mít. Klienti komunikují se instance role pomocí adresy URL. Obvykle nebudete distribuovat nebo publikování Azure *předponu*. cloudapp.net adresy URL. Místo toho bude zakoupit název DNS od vašeho registrátora DNS výběru a nakonfigurujte název DNS přesměrovávat žádosti klientů na adresu URL Azure. Další podrobnosti najdete v tématu [konfigurace vlastního názvu domény v Azure][Configuring a Custom Domain Name in Azure].

## <a id="considerations"></a>Hostované služby aspekty návrhu
Při navrhování aplikace na spouštění v prostředí cloudu, existuje několik hledisek myslet například latence, vysokou dostupnost a škálovatelnost.

Umístění kódu aplikace je důležitý faktor při spuštění služby hostované v Azure. Je běžné pro nasazení aplikace do datových center, které jsou nejblíž k vaši klienti mohli snížit latenci a získat možné nejlepší výkon.
Však můžete zvolit datového centra blíže vaší společnosti nebo co nejblíže ke svým datům, pokud máte některé příslušnosti nebo právních pochybnostmi k datům a kde se nachází. Může být hostitelem kódu aplikace jsou šesti datových centrech po celém světě. Následující tabulka uvádí dostupná umístění:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">

<tbody>

<tr>

<td style="width: 100px;">
**Země nebo oblast**

</td>

<td style="width: 200px;">
**Dílčí oblastí**

</td>
</tr>

<tr>

<td>
Spojené státy

</td>

<td>
Jižní střed & střed – sever

</td>
</tr>

<tr>

<td>
Evropa

</td>

<td>
Severní & – západ

</td>
</tr>

<tr>

<td>
Asie

</td>

<td>
– Jihovýchod & – východ

</td>
</tr>
</tbody>
</table>
Při vytváření hostované služby, vyberte podoblast označující umístění, ve kterém chcete kódu provést.

K dosažení vysoké dostupnosti a škálovatelnosti, je důležité, aby se data aplikace zachovala centrální úložiště pro více instancí role dostupné. Usnadní to, Azure nabízí několik možností úložiště jako objekty BLOB, tabulek a databáze SQL. Najdete v tématu [nabídky úložiště dat v Azure] [ Data Storage Offerings in Azure] článku Další informace o těchto technologií úložiště. Následující obrázek znázorňuje, jak nástroj pro vyrovnávání zatížení uvnitř datového centra Azure distribuuje požadavky klientů na které mají přístup ke stejnému úložišti dat instance jinou roli.

![Bitové kopie][3]

Obvykle chcete najít kódu aplikace a data ve stejném datovém centru jako to umožňuje, aby s nízkou latencí (lepší výkon) Pokud kód aplikace přistupuje k datům. Kromě toho vám není účtován šířky pásma při přesunu dat ve stejném datovém centru.

## <a id="scale"></a>Návrhu aplikace pro škálování
V některých případech můžete chtít trvat jednu aplikaci (jako jednoduchý webový server) a mějte ho hostované v Azure. Ale často, vaše aplikace může obsahovat několik rolí společně. Například následující obrázek existují dvě instance role webu, tři instancí role pořadí zpracování a jednu instanci role generátor sestavy. Tyto role fungují společně a kód pro všechny z nich se dají spojených oprav a nasadit jako na jednu jednotku do Azure.

![Bitové kopie][4]

Hlavním důvodem k rozdělení aplikace do jiné role každý systémem vlastní sadu instancí role (VM) je role nezávisle škálovat. Například během sváteční sezóny mnoho zákazníků může být zakoupení produkty z vaší společnosti, proto můžete chtít zvýšit počet instancí role spuštěna vaše role webu a také počet instancí role vaší rolí pořadí zpracování. Po sváteční sezóny může získat spoustu produkty vrátí, proto musíte může stále velké množství instancí webu, ale méně instancí pořadí zpracování. Pro zbytek roku musíte pouze několik webu a pořadí zpracování instancí. Ve všech to může být nutné jenom jeden výskyt generátor sestavy. Možnost nasazení na základě rolí v Azure umožňuje snadno zakomponovat aplikace k vašim obchodním potřebám.

Je běžné mít role, kterou instancí v rámci vaší hostované služby vzájemně komunikovat. Například role webu přijímá zákazníka pořadí, ale pak snižování zátěže pořadí zpracování pro instance rolí pořadí zpracování. Nejlepší způsob, jak předat Formulář pracovní jednu sadu instancí role na jinou sadu instancí je pomocí služby Řízení front technologie poskytovaný platformou Azure, buď služba fronty nebo fronty služby Service Bus. Použití fronty je zásadní součástí článku. Fronty umožňuje škálovat jeho role nezávisle umožňuje vyrovnávání zátěže proti náklady na hostované služby. Počet zpráv ve frontě se zvyšuje v čase, pak můžete postupně škálovat počet instancí role pořadí zpracování. Pokud v čase se sníží počet zpráv ve frontě, pak můžete vertikálně snížit kapacitu počet instancí role pořadí zpracování. Tímto způsobem, jenom platíte pro instance vyžadovaná ke zpracování skutečné pracovní vytížení.

Fronty taky poskytuje spolehlivost. Při zvětšení velikosti dolů počet instancí role pořadí zpracování, Azure rozhodne, které instance ukončit. Může rozhodnout o ukončení instanci, která se právě zpracovává zprávu fronty. Ale protože zpracování zprávy úspěšně nedokončí, zpráva se zobrazí znovu k jiné pořadí zpracování role instanci, která vybere a zpracovává je. Z důvodu viditelnost zprávu fronty jsou zprávy zaručit nakonec zpracovat. Fronty taky funguje jako nástroj pro vyrovnávání zatížení efektivně distribucí jeho zprávy a pokuste se instance všech rolí, které z něj požádat o zprávy.

Pro instance rolí webu, můžete sledovat provoz přicházející do nich a rozhodnout, škálovat počet je nahoru nebo dolů, také. Fronty můžete škálovat počet instancí role webu nezávisle na instancí rolí pořadí zpracování. Toto je velmi mocné a vám přináší značnou flexibilitu. Samozřejmě pokud se aplikace skládá z dalších rolí, můžete přidat další fronty jako přenos ke komunikaci mezi nimi, aby bylo možné využívat stejné škálování a náklady výhody.

## <a id="defandcfg"></a>Hostované služby definice a konfigurace
Nasazení v hostované službě do Azure, musíte mít také souboru definice služby a konfigurační soubor služby. Oba tyto soubory jsou soubory formátu XML, které umožňují vám umožní deklarativně možnosti nasazení pro hostovanou službu. Soubor definice služby popisuje všech rolí, které tvoří vaší hostované služby a jak komunikují. Konfigurační soubor služby popisuje se počet instancí pro každou roli a nastavení lze nakonfigurovat jednotlivé instance role.

## <a id="def"></a>Soubor definice služby
Jak jsem už zmínili dřív, definice služby (CSDEF) soubor je soubor XML, který popisuje různé role, které tvoří vaši kompletní aplikaci. Dokončení schématu pro soubor XML naleznete zde: [] – [http://msdn.microsoft.com/library/windowsazure/ee758711.aspx].
CSDEF soubor obsahuje element WebRole nebo WorkerRole pro každou roli, které chcete v aplikaci. Nasazení role jako webové role (pomocí elementu WebRole) znamená, že kód bude spuštěna v instanci role obsahující systém Windows Server 2008 a Internet Information Server (IIS).
Nasazení role jako roli pracovního procesu (pomocí elementu WorkerRole) znamená, že role instance budou mít systém Windows Server 2008 na něm (služba IIS nebude nainstalována).

Určitě můžete vytvořit a nasadit role pracovního procesu, který používá některé další mechanismus pro naslouchání příchozích webových požadavků (například kódu lze vytvořit a použít .NET HttpListener). Vzhledem k tomu, že všechny instance rolí se systémem Windows Server 2008, kód můžete provádět žádné operace, které jsou obvykle dostupné pro aplikace běžící v systému Windows Server
2008.

Pro každou roli znamenat požadovaná velikost virtuálního počítače, které by měl používat instance dané role. Následující tabulka uvádí různé dnes dostupné velikosti virtuálních počítačů a atributy každého:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">

<tbody>

<tr align="left" valign="top">

<td>
**Velikost virtuálního počítače**

</td>

<td>
**VYUŽITÍ PROCESORU**

</td>

<td>
**PAMĚŤ RAM**

</td>

<td>
**Disk**

</td>

<td>
**Sítě ve špičce vstupně-výstupních operací**

</td>
</tr>

<tr align="left" valign="top">

<td>
**Extra malé**

</td>

<td>
1 x 1.0 GHz

</td>

<td>
768 MB

</td>

<td>
20GB

</td>

<td>
\~5 MB/s

</td>
</tr>

<tr align="left" valign="top">

<td>
**Malá**

</td>

<td>
1 × 1,6 GHz

</td>

<td>
1,75 GB

</td>

<td>
225GB

</td>

<td>
\~100 MB/s

</td>
</tr>

<tr align="left" valign="top">

<td>
**Střední**

</td>

<td>
2 x 1,6 GHz

</td>

<td>
3,5 GB

</td>

<td>
490GB

</td>

<td>
\~200 MB/s

</td>
</tr>

<tr align="left" valign="top">

<td>
**Velká**

</td>

<td>
4 x 1,6 GHz

</td>

<td>
7 GB

</td>

<td>
1TB

</td>

<td>
\~400 MB/s

</td>
</tr>

<tr align="left" valign="top">

<td>
**Velmi velká**

</td>

<td>
8 x 1,6 GHz

</td>

<td>
14 GB

</td>

<td>
2TB

</td>

<td>
\~800 MB/s

</td>
</tr>
</tbody>
</table>
Účtujeme každou hodinu pro každý virtuální počítač používáte jako instanci role a se účtují poplatky pro všechna data, že instance role odeslat mimo datové centrum. Vám není účtován data zadáním datového centra. Další informace najdete v tématu [Azure – ceny] [Azure – ceny]. Obecně je vhodné používat velký počet instancí role malé oproti několik instancí velký, aby vaše aplikace je odolnější vůči selhání. Po všech máte méně instancí role, více katastrofální a selhání v jednom z nich je vaše celkové aplikace. Taky jak je uvedeno nahoře, je nutné nasadit aspoň dvě instance pro každou roli při získání smlouvu o úrovni služeb 99,95 %, které společnost Microsoft poskytuje.

Soubor definice (CSDEF) služby je také, kde zadáte mnoho atributů o každou roli v aplikaci. Tady jsou některé další užitečné položky k dispozici:

* **Certifikáty**. Používáte certifikáty pro šifrování dat nebo pokud webová služba podporuje protokol SSL. Všechny certifikáty musí být nahrán do Azure. Další informace najdete v tématu [Správa certifikátů v Azure][Managing Certificates in Azure]. Toto nastavení XML nainstaluje předtím nahrála certifikátů do úložiště certifikátů instanci role, aby byla použitelná pro kód aplikace.
* **Názvy nastavení konfigurace**. Pro hodnoty, které chcete, aby vaše aplikace čtení během spuštěné v instanci role. Skutečná hodnota nastavení konfigurace je nastavena v souboru konfigurace (CSCFG) služby, který můžete kdykoli aktualizovat bez nutnosti znovu nasaďte vašeho kódu. Ve skutečnosti může kód aplikace takovým způsobem, ke zjištění hodnoty změněné konfigurace, aniž by docházelo k výpadkům.
* **Vstupní koncové body**. Sem zadejte žádné HTTP, HTTPS nebo TCP koncové body (s porty), který chcete vystavit k vnějším světem prostřednictvím vaší *předponu*. cloadapp.net adresy URL. Když Azure nasadí vaši roli, nakonfiguruje bránu firewall na instanci role automaticky.
* **Vnitřních koncových bodů**. Sem zadejte jakékoli HTTP nebo TCP koncových bodů, které chcete zpřístupněny další instance rolí, které jsou nasazeny jako součást vaší aplikace. Povolit všechny instance role v rámci aplikace komunikovat vnitřních koncových bodů, ale nejsou dostupné pro všechny instance rolí, které jsou mimo aplikaci.
* **Naimportovat moduly**. Tyto instance role volitelně nainstalovat součásti užitečné. Součásti existovat diagnostiky sledování, vzdálené plochy a Azure připojit (tím instanci role pro přístup k místním prostředkům prostřednictvím zabezpečeného kanálu).
* **Místní úložiště**. To přiděluje podadresáři na instanci role pro vaši aplikaci používat. Je popsán v podrobněji [nabídky úložiště dat v Azure] [ Data Storage Offerings in Azure] článku.
* **Spuštění úlohy**. Spuštění úlohy poskytují způsob, jak nainstalovat požadované součásti na instanci role jako spuštění. Úlohy můžete spustit jako správce v případě potřeby zvýšenými oprávněními.

## <a id="cfg"></a>Konfigurační soubor služby
Soubor konfigurace (CSCFG) služby je soubor XML, který popisuje nastavení, která lze změnit bez opětovného nasazení aplikace. Dokončení schématu pro soubor XML naleznete zde: [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx][http://msdn.microsoft.com/library/windowsazure/ee758710.aspx].
Soubor .CSCFG obsahuje element Role pro každou roli v aplikaci. Tady jsou některé položky, které můžete zadat v soubor .CSCFG:

* **Verze operačního systému**. Tento atribut umožňuje vyberte operační systém (OS) verzi, kterou chcete použít pro všechny instance role spuštění kódu aplikace. Tento operační systém se označuje jako *operačního systému hosta*, a každá nová verze zahrnuje nejnovější opravy zabezpečení a aktualizace, které jsou k dispozici v době vydání hostovaný operační systém. Pokud nastavíte hodnotu atribut osVersion "\*", pak Azure automaticky aktualizuje hostovaný operační systém na každém z instance role dostupná nové verze operačního systému hosta. Můžete však zamítnutí automatických aktualizací, vyberte konkrétní verze operačního systému hosta. Například atribut osVersion nastavení na hodnotu "WA-GUEST-OS-2.8\_201109-01" způsobí, že všechny instance role získat, co je popsáno na této webové stránce: [http://msdn.microsoft.com/library/hh560567.aspx] [http://msdn.microsoft.com/library/hh560567.aspx]. Další informace o verzích operačního systému hosta najdete v tématu [Správa upgrady operačního systému Azure hosté].
* **Instance**. Tento element hodnota označuje počet instancí role, které chcete zřízené spuštění kódu pro konkrétní roli. Vzhledem k tomu, že nový soubor .CSCFG můžete nahrát do Azure (bez opětovného nasazení aplikace), je trivially jednoduchý a změňte hodnotu pro tento element nahrát nový soubor .CSCFG dynamicky zvyšte nebo snižte počet instancí role spuštění kódu aplikace . To umožňuje snadno škálovat aplikaci nahoru nebo dolů plnění vytížení požadavky při řízení také kolik vám účtovat pro spuštění instancí role.
* **Konfigurace nastavení hodnoty**. Tento element označuje hodnoty pro nastavení (podle definice v souboru CSDEF). Vaše role může číst tyto hodnoty, když je spuštěná. Tyto hodnoty nastavení konfigurace se obvykle používají pro připojovací řetězce k databázi SQL nebo do úložiště Azure, ale mohou být použity pro jakýkoli účel, které očekáváte.

## <a id="hostedservices"></a>Vytvoření a nasazení v hostované službě
Vytváření hostované služby vyžaduje nejprve přejít k [portálu pro správu Azure] a zřídit hostovanou službu tak, že zadáte předponu DNS a data center můžete nakonec chcete spuštěné v kódu. Poté ve vašem vývojovém prostředí vytvoření souboru definice (CSDEF) služby, sestavení kódu aplikace a balíčku (zip), všechny tyto soubory do souboru balíčku (CSPKG) služby. Také je nutné připravit soubor konfigurace (CSCFG) vaší služby. Pokud chcete nasadit vaši roli, nahrajte CSPKG a CSCFG soubory pomocí Azure Service Management API. Po nasazení Azure, bude instance role zřídit v datovém centru (podle konfigurační data), extrahujte kódu aplikace z balíčku, zkopírujte jej do instance rolí a spuštění instance. Váš kód je teď spuštěná.

Následující obrázek znázorňuje CSPKG a CSCFG soubory, které vytvoříte ve svém vývojovém počítači. Soubor CSPKG obsahuje soubor CSDEF a kód pro dvě role. Po odeslání souborů CSPKG a CSCFG s rozhraním API pro správu služby Azure, Azure vytvoří instance rolí v datovém centru. V tomto příkladu soubor .CSCFG ukázalo, že Azure má vytvořit tři instancí role \#1 až dvou instancí Role \#2.

![Bitové kopie][5]

Další informace o nasazení, upgrade a změny konfigurace rolí, najdete [nasazení a aktualizace aplikací Azure] [ Deploying and Updating Azure Applications] článku.<a id="Ref" name="Ref"></a>

## <a id="references"></a>Odkazy
* [Vytváření hostované služby pro Azure.][Creating a Hosted Service for Azure]
* [Správa hostované služby v Azure][Managing Hosted Services in Azure]
* [Migrace aplikací do Azure][Migrating Applications to Azure]
* [Konfigurace aplikace Azure][Configuring an Azure Application]

<div style="width: 700px; border-top: solid; margin-top: 5px; padding-top: 5px; border-top-width: 1px;">

<p>Zapsaných správcem Jana Richter (Wintellect)</p>

</div>

[Azure Application Model Benefits]: #benefits
[Hosted Service Core Concepts]: #concepts
[Hosted Service Design Considerations]: #considerations
[Designing your Application for Scale]: #scale
[Hosted Service Definition and Configuration]: #defandcfg
[The Service Definition File]: #def
[The Service Configuration File]: #cfg
[Creating and Deploying a Hosted Service]: #hostedservices
[References]: #references
[0]: ./media/application-model/application-model-3.jpg
[1]: ./media/application-model/application-model-4.jpg
[2]: ./media/application-model/application-model-5.jpg
[Configuring a Custom Domain Name in Azure]: http://www.windowsazure.com/develop/net/common-tasks/custom-dns/
[Data Storage Offerings in Azure]: http://www.windowsazure.com/develop/net/fundamentals/cloud-storage/
[3]: ./media/application-model/application-model-6.jpg
[4]: ./media/application-model/application-model-7.jpg

[Azure Pricing]: http://www.windowsazure.com/pricing/calculator/
[Managing Certificates in Azure]: http://msdn.microsoft.com/library/windowsazure/gg981929.aspx
[http://msdn.microsoft.com/library/windowsazure/ee758710.aspx]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[http://msdn.microsoft.com/library/hh560567.aspx]: http://msdn.microsoft.com/library/hh560567.aspx
[Správa upgrady operačního systému Azure hosté]: http://msdn.microsoft.com/library/ee924680.aspx
[portálu pro správu Azure]: http://manage.windowsazure.com/
[5]: ./media/application-model/application-model-8.jpg
[Deploying and Updating Azure Applications]: http://www.windowsazure.com/develop/net/fundamentals/deploying-applications/
[Creating a Hosted Service for Azure]: http://msdn.microsoft.com/library/gg432967.aspx
[Managing Hosted Services in Azure]: http://msdn.microsoft.com/library/gg433038.aspx
[Migrating Applications to Azure]: http://msdn.microsoft.com/library/gg186051.aspx
[Configuring an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405486.aspx
