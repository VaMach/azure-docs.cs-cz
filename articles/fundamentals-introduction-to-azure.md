---
title: "Úvod do Microsoft Azure | Microsoft Docs"
description: "Nové do služby Microsoft Azure? Získat základní přehled služeb nabízí s příklady, jak jsou užitečné."
services: " "
documentationcenter: .net
author: rboucher
manager: carolz
editor: 
ms.assetid: 6f47f711-2208-4c21-8c1d-826a54c05c29
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2015
ms.author: robb
ms.openlocfilehash: 2d2cc04887d90b9f005bfe5310a9ce2fb2f7e217
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="introducing-microsoft-azure"></a>Úvod do Microsoft Azure
Microsoft Azure je platforma aplikace společnosti Microsoft pro veřejný cloud.  Cílem tohoto článku je poskytují základ pro pochopení základy Azure, i v případě, že bez znalosti o cloud computing.

**Jak si tento článek**

Azure roste vždy, takže můžete snadno získat přetížený.  Začněte základní služby, které jsou uvedeny jako první v tomto článku a potom přejděte k další služby. Který neznamená právě další služby nelze použít samy o sobě, ale základní služby tvoří základní aplikace běžící v Azure.

**Váš názor**

Vaše zpětná vazba je důležité. Tento článek měl dát efektivní Přehled služby Azure. Pokud ne, řekněte nám v sekci komentáře v dolní části stránky. Poskytněte některé podrobnosti o očekává zobrazíte a jak ke zlepšení článek.  

## <a name="the-components-of-azure"></a>Součástí Azure
Azure skupiny služeb do kategorií v portálu pro správu a v různých vizuální pomůcky, jako [co je Azure Infografice](https://azure.microsoft.com/documentation/infographics/azure/) . Portálu pro správu je použít ke správě služby většinu (ale ne všechny) v Azure.

Tento článek se použije **jiné organizaci** mluvit o služeb na základě podobné funkce a volání na důležité dílčí služby, které jsou součástí větší ty.  

![Azure součásti](./media/fundamentals-introduction-to-azure/AzureComponentsIntroNew780.png)   
 *Obrázek: Azure poskytuje přístupné z Internetu aplikačních služeb běžících v datových centrech Azure.*

## <a name="management-portal"></a>Portál pro správu
Azure je webové rozhraní s názvem [portálu pro správu](http://manage.windowsazure.com) umožňuje správcům přístup a spravovat většinu, ale všechny Azure funkce.  Společnost Microsoft obvykle vydává novější uživatelského rozhraní portálu ve verzi beta před vyřazením z provozu starší. Je volána novější jeden ["Portál Azure Preview"](https://portal.azure.com/).

Je to obvykle dlouhé překrývají oba Portály jsou aktivní. Zatímco základní služby se zobrazí v obou portálů, ne všechny funkce mohou být k dispozici v obou. Novější služby může zobrazí v novější portálu první a starší služby a funkce může existovat pouze v ta starší.  Zpráva tady je, že Pokud nenajdete něco v portálu starší kontrolují novější jednu a naopak.

## <a name="compute"></a>Compute
Jedním z nejzákladnější věcí, které nemá cloudové platformy je spuštění aplikace. Každý výpočetní modely má svou vlastní úlohu.

Můžete použít tyto technologie samostatně, nebo podle potřeby vytvořit správné základ pro vaši aplikaci kombinovat. Přístupů, které zvolíte, závisí na jaké problémy, které se snažíte vyřešit.

### <a name="azure-virtual-machines"></a>Azure Virtual Machines
![Virtuální počítače Azure ROBBCSIART_TEST](./media/fundamentals-introduction-to-azure/mscsiart_VirtualMachinesIntroNew_12345.png)   
*Obrázek: Virtuální počítače Azure získáte plnou kontrolu nad instancí virtuálního počítače v cloudu.*

Umožňuje vytvořit virtuální počítač na vyžádání, zda ze standardní bitové kopie nebo z jednoho zadáte, může být velmi užitečná. Tento přístup, obvykle označuje jako infrastruktura jako služba (IaaS), je, co poskytuje virtuální počítače Azure. Obrázek 2 ukazuje kombinaci způsob spuštění virtuálního počítače (VM) a jak vytvořit jeden z disku VHD.  

Chcete-li vytvořit virtuální počítač, zadáte které virtuálního pevného disku na použití a velikost Virtuálního počítače.  Potom platíte čas, kdy je virtuální počítač spuštěný. Platíte minutu a pouze tehdy, když je spuštěná, přestože je minimální úložiště poplatků za pořízení virtuálního pevného disku k dispozici. Azure nabízí stock virtuální pevné disky (nazývané "Image") obsahující spouštěcí operačního systému pro spuštění z galerie. Mezi ně patří společnosti Microsoft a partnera možnosti, například Windows Server a Linux, SQL Server, Oracle a mnoho dalších. Můžete vytvořit virtuální pevné disky a bitové kopie a potom je odeslat sami. Můžete nahrát i virtuální pevné disky, které obsahují pouze data a poté přistupovat k nim z spuštěné virtuální počítače.

Bez ohledu na virtuální pevný disk pochází z, můžete uložit trvale veškeré změny provedené spuštěného virtuálního počítače. Při příštím vytvoření virtuálního počítače z tohoto disku VHD, co se vyzvedávat tam, kde jste přestali. Virtuální pevné disky, které zálohují virtuální počítače jsou uloženy do objektů BLOB Azure Storage, které se věnuje později.  To znamená, že získají redundance, aby bylo, že virtuální počítače nebudou zmizí z důvodu selhání hardwaru a disku. Také je možné zkopírovat změněné virtuálního pevného disku z Azure a potom ji spustit místně.

Vaše aplikace běží v rámci jednoho nebo více virtuálních počítačů, v závislosti na tom, jak ho před vytvořila nebo se rozhodnete ji vytvořit od začátku.

Tento poměrně obecné přístup do cloudu computing můžete používat k adresování mnoho různých problémů.

**Virtuální počítač scénáře**

1. **Vývoj/testování** – můžete je použít k vytvoření cenově nenáročná vývojová a testovací platforma, která můžete vypnout po dokončení použití. Také můžete vytvořit a spuštění aplikace, které používají ať jazyky a knihovny se vám líbí. Tyto aplikace použít některou z možností správy dat, které poskytuje Azure, a můžete také používat SQL Server nebo jiný databázového systému spuštěné v jedné nebo více virtuálních počítačů.
2. **Přesunutí aplikace do Azure (navýšení a shift)** -"Navýšení a shift" odkazuje na mnohem přesunutí vaší aplikace, jako byste používali vysokozdvižném přesunout velkého objektu.  "Navýšení" virtuální pevný disk z vašeho místního datového centra a "posunutí" jej do Azure a spusťte jej.  Většinou máte nějakou práci odebrání závislostí na jiných systémech. Pokud jsou moc, můžete zvolit možnost 3 místo.  
3. **Rozšíření vašeho datového centra** -použití Azure spuštěné virtuální počítače jako rozšíření vašeho datového centra místní SharePoint nebo jinými aplikacemi. Za tímto účelem je možné vytvořit doménách systému Windows v cloudu pomocí služby Active Directory ve virtuálních počítačích Azure. Virtuální sítě Azure (uvedených později) můžete použít ke svázání vaší místní sítí a sítí v Azure společně.

### <a name="web-apps"></a>Web Apps
![Službě Azure Web Apps ROBBCSIART_TEST](./media/fundamentals-introduction-to-azure/mscsiart_AzureWebsitesIntroNew_12345.png)   
 *Obrázek: Azure Web Apps spustí webovou aplikaci v cloudu bez nutnosti ke správě základní webového serveru.*

Mezi nejběžnější činností, které uživatelé provádějí v cloudu běží webů a webových aplikací. To umožňuje virtuální počítače Azure, ale je stále vám zůstane je zodpovědností správě jednoho nebo více virtuálních počítačů a základní operační systémy. Cloudové služby webové role můžete to provést, ale nasazení a údržbu, je stále trvá administrativy.  Co dělat, když chcete web kde někdo jiný kód má na starosti správu činností, které pro vás?

Toto je přesně co nabízí webové aplikace. Tento model výpočetní nabízí prostředí spravované webové pomocí portálu pro správu Azure, jakož i rozhraní API. Existující aplikaci Web můžete přesunout do webové aplikace beze změny, nebo můžete vytvořit novou přímo v cloudu. Jakmile je spuštěn web, můžete přidat nebo odebrat instancí dynamicky, spoléhat na Azure Web Apps načíst vyrovnávat požadavky mezi nimi. Aplikace Azure nabízí sdílené možnost, kde webu běží ve virtuálním počítači s jinými weby, a standardní možnost, která umožňuje serveru pro spuštění ve vlastním virtuálním počítači. Standardní možnost také umožňuje zvýšit velikost vaše instance (computing power) v případě potřeby.

Pro vývoj webové aplikace podporuje rozhraní .NET, PHP, Node.js, Java a Python spolu s SQL Database a MySQL (z ClearDB, partnera společnosti Microsoft) pro relační úložiště. Také poskytuje integrovanou podporu pro několik oblíbených aplikací, včetně WordPress, Joomla nebo Drupal. Cílem je zajistit nízkonákladové, škálovatelné a široce užitečné platforma pro vytváření webů a webových aplikací ve veřejném cloudu.

**Scénáře webové aplikace**

Webové aplikace slouží jako vhodný pro společnosti, vývojáři, webové návrhu úřady. Pro společnosti jedná o snadno spravovat, škálovatelná, vysoce zabezpečených a vysoce dostupné řešení pro spouštění přítomnosti weby. Pokud potřebujete nastavit web, je nejlepší začít s Azure Web Apps a pokračovat ke Cloudovým službám, jakmile budete potřebovat funkce, která není k dispozici. Viz konec v části Další odkazy, které vám můžou pomoct při některou z možností "Výpočet".

### <a name="cloud-services"></a>Cloud Services
![Cloudové služby Azure](./media/fundamentals-introduction-to-azure/CloudServicesIntroNew.png)   
*Obrázek: Azure Cloud Services poskytuje místo, kde můžete spustit na platforma jako služba (PaaS) prostředí vysoce škálovatelné vlastní kód*

Předpokládejme, že chcete vytvořit cloudovou aplikaci, která může podporovat velký počet současně připojených uživatelů, nevyžaduje mnoho správy a nikdy ocitne mimo provoz. Je navázané dodavatelem například, který se rozhodla zapojení Software jako služba (SaaS) ve verzi jednu z vašich aplikací v cloudu. Nebo může být Startup vytváření aplikace příjemce, které očekáváte, že se rychle zvětšovat. Pokud vytváříte v Azure, které model spouštění mám použít?

Azure Web Apps umožňuje vytváření tento druh webové aplikace, ale existují některá omezení. Nemáte přístup pro správu, například, což znamená, že libovolný software nelze nainstalovat. Virtuální počítače Azure nabízí pružné, včetně přístup pro správu a určitě můžete ho vytvořit velmi škálovatelné aplikaci, ale budete mít mnoho aspektů spolehlivosti a správy zpracování sami. Jakým způsobem je možnost, která vám dává kontrolu potřebujete ale také obstará většinu práce potřebné pro spolehlivosti a správy.

Toto je přesně poskytuje Azure Cloud Services. Tato technologie je výslovně navržený pro podporu škálovatelný, spolehlivý a nízká Správce aplikací ale je příkladem co má označovaného jako platforma jako služba (PaaS). Chcete-li použít, vytvořte aplikaci pomocí technologie, kterou zvolíte, například C#, Java, PHP, Python, Node.js nebo něco jiného. Váš kód pak provede ve virtuálních počítačích (označované jako instance) s verzí systému Windows Server.

Ale tyto virtuální počítače se liší od těch, které vytvoříte pomocí Azure Virtual Machines. Pro jednou z věcí, Azure samotné spravuje je to třeba k instalaci opravy operačního systému a automaticky zavedením nové opravit bitové kopie. To znamená, že vaše aplikace by neměl uchování stavu v instancích role web nebo worker; Místo toho by měly být udržovány v jedné z možností správy dat Azure popsané v další části. Azure také monitoruje těchto virtuálních počítačů, všechny restartování tohoto selhání. Cloudové služby pro automatické vytvoření více nebo méně instancí v reakci na poptávku můžete nastavit. To umožňuje zpracovat vyšší míra využívání a pak vertikálně zpět, takže nejsou platícího co nejvíc po menší využití.

Máte dvě role lze vybírat při vytváření instance, obě založené na Windows serveru. Hlavní rozdíl mezi nimi je, že instance webové role spouští IIS, zatímco instanci role pracovního procesu nemá. Jak se spravují stejným způsobem, ale a je běžné pro aplikace pro použití obou. Například instanci webové role může přijímat požadavky od uživatelů a potom předat instanci role pracovního procesu pro zpracování. Vaše aplikace škálovat nahoru nebo dolů, můžete požádat, aby Azure vytvořit více instancí role buď nebo vypnout existující instance. A podobně jako na virtuálních počítačích Azure, se vám účtována jen za čas, každá instance role web nebo pracovní proces běží.

**Cloud Services scénáře**

Cloudové služby jsou ideální pro podporu masivní škálování, když jste třeba větší kontrolu nad platformu, než poskytuje Azure Web Apps, ale nepotřebujete kontrolu nad příslušný operační systém.

#### <a name="choosing-a-compute-model"></a>Výběr modelu výpočetní
Stránce [porovnání Azure Web Apps, cloudové služby a virtuální počítače](app-service/choose-web-site-cloud-service-vm.md) poskytuje podrobnější informace o tom, jak zvolit výpočetní model.

## <a name="data-management"></a>Správa dat
Aplikace potřebují data, a různé druhy aplikací mít různé druhy dat. Z toho důvodu Azure poskytuje několik různých způsobů, jak ukládat a spravovat data. Azure poskytuje mnoho možností úložiště, ale všechny jsou navrženy pro velmi odolné úložiště.  U některé z těchto možností jsou vždy 3 kopií vašich dat, které byly synchronizovány ve datovém centru Azure – 6, pokud povolíte Azure a k zálohování k jinému datovému centru alespoň 300 miles rychle použít geografická redundance.     

### <a name="in-virtual-machines"></a>Ve virtuálních počítačích
Umožňuje spouštění systému SQL Server nebo jiný databázového systému ve virtuálním počítači vytvořit pomocí Azure Virtual Machines má zkopírovali. Uvědomte si, že tato možnost není omezen na relační systémy; jste také můžete spustit NoSQL technologie jako MongoDB a Cassandra. Vlastní databáze je nainstalován systém přehledné it replikují co jsme slouží jako v našich vlastních datových centrech – ale také vyžaduje zpracování správu tohoto databázového systému.  V další možnosti Azure zpracovává více nebo všechny správy za vás.

Stav virtuálního počítače a všechny další datový disk vytvoříte nebo odeslat znovu, jsou zajišťované úložiště objektů blob (které v souvislosti se později).  

### <a name="azure-sql-database"></a>Azure SQL Database
![Úložiště Azure SQL Database](./media/fundamentals-introduction-to-azure/StorageAzureSQLDatabaseIntroNew.png)   

*Obrázek: Poskytuje Azure SQL Database spravovaná služba relační databáze v cloudu.*

Pro relační úložiště Azure poskytuje funkci SQL Database. Nenechte si pojmenování podvést vás. To se liší od typických SQL Database poskytované SQL Server běžící na Windows Server.  

Dříve se označovaly jako SQL Azure, poskytuje Azure SQL Database všechny klíčové funkce relační databáze systém správy, včetně jednotlivé transakce, přístup k datům souběžných více uživatelů s integritu dat, dotazů ANSI SQL a známý programovací model. Jako SQL Server, SQL Database lze přistupovat pomocí rozhraní Entity Framework, ADO.NET, JDBC a dalších známých dat přístupu technologie. Také podporuje většinu jazyka T-SQL společně s nástroje SQL Server, jako je například SQL Server Management Studio. Kdokoliv obeznámeni s systému SQL Server (nebo jiné relační databáze), pomocí databáze SQL je přehledné.

Databáze SQL není právě, ale databázového systému v cloudu it je PaaS služby. Stále řízení dat a kdo k němu přístup, ale SQL Database má na starosti správu grunt práce, jako je například Správa hardwaru infrastruktury a automaticky udržuje software databáze a operační systém aktuální. Databáze SQL také poskytuje vysokou dostupnost, automatické zálohování, v okamžiku obnovit možnosti a můžete replikovat kopie v zeměpisných oblastech.  

**Scénáře pro databázi SQL.**

Pokud vytváříte aplikaci Azure (pomocí kteréhokoli modelu výpočetní), který potřebuje relační úložiště, databáze SQL může být vhodný. Aplikace běží mimo cloudu můžete také tuto službu, ale, je mnoho dalších scénářů. Například data uložená v databázi SQL je přístupná z různých klientských systémů, včetně stolní počítače, notebooky, tablety a telefony. A protože poskytuje integrovanou vysokou dostupnost prostřednictvím replikace, použití SQL Database může pomoci minimalizovat prostoje.

### <a name="tables"></a>Tabulky
![Tabulky úložiště Azure](./media/fundamentals-introduction-to-azure/StorageTablesIntroNew.png)  

*Obrázek: Tabulky Azure poskytuje ploché NoSQL způsob, jak ukládat data.*

Tato funkce se někdy nazývá různé podmínky jako jeho součástí větší funkci "Azure Storage". Pokud se zobrazí "tabulky", "Azure tabulky" nebo "úložiště tabulky", je všechny samé.  

A nemusíte Nezaměňovat s názvem: Tato technologie neposkytuje relační úložiště. Ve skutečnosti je příklad NoSQL přístupu volá úložiště dvojic klíč/hodnota. Tabulky Azure umožní aplikaci uložit vlastnosti různé typy, jako je například řetězce, celá čísla a kalendářní data. Aplikace pak může načíst skupinu vlastností tím, že poskytuje jedinečný klíč pro tuto skupinu. Při komplexních operací jako spojení nepodporuje, tabulky nabízí rychlý přístup k datům typu. Jsou k velmi škálovatelné s mnohem terabajt dat udržet jednu tabulku. A odpovídající jejich jednoduchost, tabulky jsou obvykle levnější než relační úložiště databáze SQL.

**Scénáře pro tabulky**

Předpokládejme, že chcete vytvořit aplikaci Azure, který potřebuje rychlý přístup k typu dat, může být velké množství ji, ale nemusí provádět komplexní dotazy SQL pro tento datový. Představte si například, že vytváříte aplikace příjemce, který potřebuje k ukládání informací o zákaznících profil pro každého uživatele. Aplikace bude velmi Oblíbené, takže je potřeba povolit pro velké množství dat, ale není příliš s tato data kromě ukládání, pak je načítání jednoduché způsoby. Toto je přesně druh scénář, kde tabulky Azure má smysl.

### <a name="blobs"></a>Objekty blob
![Objekty BLOB úložiště Azure](./media/fundamentals-introduction-to-azure/StorageBlobsIntroNew.png)    
*Obrázek: Objektů BLOB Azure poskytuje nestrukturovaných binární data.*  

Azure BLOB (znovu "Úložiště objektů Blob" a právě "úložiště objektů BLOB" jsou samé) je určen k ukládání nestrukturovaných binární data. Stejně jako tabulky poskytuje nenákladné úložiště objektů BLOB a jediného objektu blob může být větší než 1TB (jeden terabajt). Aplikace Azure můžete používat taky jednotky Azure, které umožňují poskytovat trvalé úložiště pro systém souborů Windows připojené v instanci Azure BLOB. Aplikace se zobrazí obyčejnou soubory systému Windows, ale ve skutečnosti uložen obsah do objektu BLOB.

Úložiště objektů blob je používá mnoho dalších Azure funkcí (včetně virtuálních počítačů), tak příliš ho může úlohy určitě zpracovat.

**Scénáře pro objekty BLOB**

Aplikace, která ukládá videa, obrovské soubory nebo jiné binární informace můžete použít pro jednoduché, levných úložiště objektů BLOB. Objekty BLOB také běžně se používají ve spojení s jinými službami jako Content Delivery Network, který se věnuje později.  

### <a name="import--export"></a>Import/export
![Služba Azure Import exportu](./media/fundamentals-introduction-to-azure/ImportExportIntroNew.png)  

*Obrázek: Azure Import / Export nabízí možnost zaslat fyzický pevný disk nebo z Azure pro data zrychluje a zlevňuje hromadně importovat nebo exportovat.*  

Někdy budete chtít přesunout velké množství dat do Azure. Který by trvat dlouhou dobu, možná dny a použít spoustu šířky pásma. V těchto případech můžete použít Azure Import/Export, které umožňuje dodávat šifrované nástrojem Bitlocker 3,5" pevné disky SATA přímo do datových center Azure, kde společnost Microsoft bude přenést data do úložiště objektů blob pro vás.  Po dokončení nahrávání se dodává Microsoft jednotky pro vás.  Můžete také požádat, aby velkých objemů dat z úložiště objektů Blob vyexportovat do pevné disky a odeslat vám prostřednictvím e-mailu.

**Scénáře pro Import / Export**

* **Velké migrace dat** -pokaždé, když máte velké objemy dat (terabajtů), který chcete nahrát do Azure, službu Import/Export je často mnohem rychlejší a případně levnější než přenos přes internet. Jakmile jsou data do objektů BLOB, můžete ji zpracovat do jiných formulářů, jako je například úložiště Table nebo databázi SQL.
* **Obnovení dat archivovat** -Import/Export můžete mít Microsoft přenos velkých objemů dat uložené v Azure Blob Storage do zařízení úložiště které odešlete a pak mít toto zařízení doručit zpět do umístění, kterou si přejete. Protože to bude trvat delší dobu, není vhodný pro zotavení po havárii. Je nejvhodnější pro archivovaná data, která nepotřebujete rychlý přístup k.

### <a name="file-service"></a>Služba File
![Služba Azure souborů](./media/fundamentals-introduction-to-azure/FileServiceIntroNew.png)    
*Obrázek: Azure Souborová služba poskytuje SMB \\ \\server\share cesty pro aplikace běžící v cloudu.*

Na místě, se běžně používají velkých objemů úložiště souborů, které jsou dostupné prostřednictvím protokolu Server Message Block (SMB) \\ \\Server\share formátu. Azure má teď služba, která umožňuje používat tento protokol v cloudu. Aplikace běžící v Azure slouží ke sdílení souborů mezi virtuálními počítači pomocí rozhraní API jako ReadFile a WriteFile známé soubor systému. Kromě toho soubory lze také přistupovat přes rozhraní REST, která umožňuje přístup k sdílené složky z místní při také nastavení virtuální sítě současně. Soubory Azure je postavený na služby objektů blob, takže zdědil stejné dostupnosti, odolnost, škálovatelnosti a geografická redundance integrovaný do úložiště Azure.

**Scénáře pro soubory Azure**

* **Migrace existujících aplikací do cloudu** -jeho snazší migrovat místní aplikace do cloudu, které používají sdílené složky ke sdílení dat mezi částí aplikace. Každý virtuální počítač se připojuje ke sdílené složce a pak můžete číst a zapisovat soubory stejně, jako je by proti soubor místní sdílené složky.
* **Nastavení sdílených aplikací** -běžný vzor pro distribuované aplikace je tak, aby měl konfigurační soubory v centrálním umístění, kde jsou dostupné z mnoha různých virtuálních počítačů. Tyto konfigurační soubory můžete uložený ve sdílené složce Azure File a číst všechny instance aplikace. Nastavení můžete taky spravovat prostřednictvím rozhraní REST, což umožňuje po celém světě přístup ke konfiguračním souborům.
* **Diagnostické sdílenou složku** – můžete uložit a sdílet diagnostické souborech, jako jsou protokoly, metriky a výpisů stavu systému. Tyto soubory k dispozici prostřednictvím protokolu SMB i REST rozhraní umožní aplikacím používat celou řadu nástrojů pro analýzu pro zpracování a analýzu diagnostická data.
* **Dev/testovací/Debug** – když vývojáři nebo správci pracují na virtuální počítače v cloudu, často potřebují sadu nástrojů nebo pomůcek. Instalace a distribuce tyto nástroje na každém virtuálním počítači je časově náročná. Soubory Azure umožňuje vývojáře nebo správce ukládat své oblíbené nástroje do sdílené složky a připojit se k nim z jakéhokoli virtuálního počítače.

## <a name="networking"></a>Sítě
Azure spouští dnes v mnoha datových centrech šíří po celém světě. Při spuštění aplikace nebo ukládání dat, můžete vybrat jednu nebo více těchto datových center používat. Taky můžou připojovat k těchto datových centrech různými způsoby použití služeb níže.

### <a name="virtual-network"></a>Virtual Network
![VirtualNetwork](./media/fundamentals-introduction-to-azure/VirtualNetworkIntroNew.png)   

*Obrázek: Virtuální sítě poskytuje privátní sítě v cloudu, takže různé služby může komunikovat, nebo k místním prostředkům Pokud nastavíte síť VPN mezi různými místy připojení.*  

Jeden užitečný způsob použití veřejného cloudu je považováno za rozšíření vlastního datového centra.

Protože virtuální počítače můžete vytvořit na vyžádání, pak je odstranit (a zastavit platícího) Pokud jste již nepotřebujete, můžete mít computing power jenom v případě, že jej. A vzhledem k tomu, že virtuální počítače Azure umožňuje vytvářet virtuální počítače se systémem SharePoint, služby Active Directory a dalších známých místní software, tento přístup může fungovat s aplikací, které už máte.

Chcete-li to opravdu užitečné, ale uživatelé by měla být schopni s nimi zacházet tyto aplikace, jako by byla spuštěna ve svém vlastním datovém centru. Toto je přesně co Azure Virtual Network umožňují. Pomocí zařízení brány sítě VPN, Správce může nastavit virtuální privátní sítě (VPN) mezi místní sítí a vaše virtuální počítače, které jsou nasazeny do virtuální sítě v Azure. Protože je vlastní adresy IP v4 přiřadit cloud virtuálních počítačů, se zobrazí na vlastní síť. Uživatelé ve vaší organizaci můžete přístup k aplikacím, obsahují tyto virtuální počítače, jako by byla spuštěna místně.

Další informace o plánování a vytvoření virtuální sítě, který vám vyhovuje, najdete v části [virtuální sítě](virtual-network/virtual-networks-overview.md).

### <a name="express-route"></a>ExpressRoute
![ExpressRoute](./media/fundamentals-introduction-to-azure/ExpressRouteIntroNew.png)   

*Obrázek: ExpressRoute používá virtuální síť Azure, ale směruje připojení přes rychlejší vyhrazené linky namísto veřejného Internetu.*  

Pokud potřebujete další šířku pásma nebo zabezpečení než virtuální síť Azure připojení můžete zadat, můžou se podívat do ExpressRoute. V některých případech ExpressRoute můžete také ušetřit peníze. Je třeba do virtuální sítě v Azure, ale propojení mezi Azure a váš web používá vyhrazené připojení, které nepřekračuje prostřednictvím veřejného Internetu. Chcete-li použít tuto službu, musíte mít smlouvu s poskytovatele síťové služby, nebo poskytovatele exchange.

Plánování a jeho nastavení ExpressRoute připojení vyžaduje více času, takže můžete chtít spustit prostřednictvím sítě site-to-site VPN a potom migrovat na připojení ExpressRoute.

Další informace o ExpressRoute najdete v tématu [technický přehled ExpressRoute](expressroute/expressroute-introduction.md).

### <a name="traffic-manager"></a>Traffic Manager
![TrafficManager](./media/fundamentals-introduction-to-azure/TrafficManagerIntroNew.png)   

*Obrázek: Azure Traffic Manager umožňuje směrovat přenos globální k službě na základě inteligentního pravidel.*

Pokud vaše aplikace Azure běží v několik datových center, můžete použít Azure Traffic Manager inteligentně směrovat požadavky od uživatelů napříč instancí aplikace. Můžete také odeslání dat na služby nejsou spuštěny v Azure, dokud jsou přístupné z Internetu.  

Aplikaci Azure s uživateli v právě jedné součásti na světě mohou být spuštěny v pouze jednoho datového centra Azure. Aplikace s uživateli na různých po celém světě, je ale pravděpodobnější ke spuštění v několika datových centrech, může být i všechny. V této druhé situaci čelit problému: jak inteligentně nasměrujete uživatelům instancí aplikace? Ve většině případů, budete ho zřejmě chtít každého uživatele pro přístup k datacenter nejblíže k mu, protože je pravděpodobně získáte jí nejlepší doba odezvy. Ale co když je dané instance aplikace přetížené nebo není k dispozici? V takovém případě je dobrý směrovat své žádosti automaticky k jinému datovému centru. Toto je přesně co se provádí pomocí Azure Traffic Manager.

Vlastník aplikace definuje pravidla, které určují, jak se mají směrovat požadavky od uživatelů k datovým centrům a spoléhá na Traffic Manager slouží k provádění těchto pravidel. Například uživatelé mohou být normálně přesměrovány na nejbližší datové centrum Azure, ale získat posílá jiný, pokud doba odezvy z jejich výchozí datového centra překročí doba odezvy z jiných datových center. Pro globálně distribuované aplikace s mnoha uživateli s integrovanou službu pro zpracování problémy, jako to je užitečné.

Správce provozu používá Directory Name Service (DNS) uživatelům trasy, aby koncové body služby, ale dál provoz nepřekračuje prostřednictvím Správce provozu, po připojení. Díky tomu Traffic Manager nebudou problémové místo, které může zpomalit vaší komunikace služby.

## <a name="developer-services"></a>Vývojářské služby
Azure nabízí řadu nástrojů, který pomáhá vývojářům a IT profesionály, vytvářet a spravovat aplikace v cloudu.  

### <a name="azure-sdk"></a>Azure SDK
Zpět v 2008 úplně první předběžné verze Azure podporované jenom .NET – vývoj. V současné době však můžete vytvořit aplikace Azure v prakticky žádný jazyk. Společnost Microsoft poskytuje aktuálně jazykové sady SDK pro .NET, Java, PHP, Node.js, Ruby nebo Python. Je také obecné Azure SDK, která poskytuje základní podporu pro žádný jazyk, jako je například C++.  

Tyto sady SDK můžete vytvářet, nasazovat a spravovat aplikace na platformě Azure. Jsou k dispozici buď z [www.microsoftazure.com](https://azure.microsoft.com/downloads/) nebo Githubu a lze použít s Visual Studio a Eclipse. Azure nabízí i nástroje příkazového řádku, které mohou vývojáři použít s jakékoli editor nebo vývojovém prostředí, včetně nástrojů pro nasazení aplikací do Azure ze systémů Linux a Macintosh.

Společně umožňují vytvářet Azure aplikace, tyto sady SDK také poskytují klientské knihovny, které vám pomůžou vytvořit softwaru, která používá služby Azure. Může například sestavit aplikaci, která čte a zapisuje objektů Azure BLOB, nebo vytvořte nástroj, který nasadí Azure aplikací prostřednictvím rozhraní pro správu Azure.

### <a name="visual-studio-team-services"></a>Visual Studio Team Services
Visual Studio Team Services je marketing název pokrývajících číslo služeb, které pomáhají k vývoji aplikací ve službě Azure.

Aby nedocházelo k záměně - neposkytuje hostované nebo webové verzi sady Visual Studio. Stále potřebujete vaše místní spuštěná kopie sady Visual Studio. Ale nabízí celou řadu nástrojů, které může být velmi užitečné.

Nepodporuje, obsahuje systému správy hostované zdrojového názvem Team Foundation Service, který nabízí verzí a sledování pracovních položek.  Pokud dáváte přednost, který, můžete použít i Git pro správu verzí. A můžete měnit, které můžete použít projektu správy zdrojového kódu. Můžete vytvořit neomezená privátní týmové projekty přístupné z kdekoliv na světě.  

Visual Studio Team Services poskytuje službu, testování zatížení. Můžete provést zátěžové testy vytvořit v sadě Visual Studio na virtuálních počítačích v cloudu. Zadejte celkový počet uživatelů, kterou chcete zátěžový test pomocí a Visual Studio Team Services automaticky určit, kolik agenti jsou potřeba, začne pracovat požadované virtuální počítače a spustit zátěžové testy. Pokud jste odběratel MSDN, získáváte tisíce volné minut uživatele zatížení testování každý měsíc.

Visual Studio Team Services také poskytuje podporu pro agilní vývoj s funkcí jako sestavení průběžnou integraci, kanbanové karty a virtuální týmové místnosti.

**Visual Studio Team Services scénáře**

Visual Studio Team Services je vhodný pro společnosti, kteří potřebují spolupracovat po celém světě a nebudete už mít infrastrukturu Uděláte to tak. Můžete získat instalační program v minutách, zvolte systému správy zdrojů a začít psaní kódu a sestavování daný den.  Nástroje team poskytují místo pro koordinaci a spolupráce a další nástroje poskytují analýzy potřebné při testování a ladění aplikace rychle.

Ale organizace, které už máte v místním systému můžete otestovat nových projektů na Visual Studio Team Services a zkontrolujte, pokud je efektivnější.   

### <a name="application-insights"></a>Application Insights
![Application Insights](./media/fundamentals-introduction-to-azure/ApplicationInsights.png)  

*Obrázek: Application Insights monitorování výkonu a využití vaší živé aplikace web nebo zařízení.*

Po publikování aplikace – jestli běží na mobilních zařízeních, stolní počítače nebo webových prohlížečů - Application Insights se dozvíte, jak je její výkon a co uživatelé dělají s ním. Ponechá počet havárií a pomalé odezvy, výstrahy, je-li údaje mezi nepřijatelné prahové hodnoty a vám pomohou diagnostikuje problémy.

Při vývoji novou funkci, naplánujte měřit její úspěšnost s uživateli. Analýzou vzorce používání pochopit, co nejlépe vyhovuje vašim zákazníkům a vylepšení v každé cyklu vývoje aplikace.

I když je hostovaná v Azure, Application Insights funguje pro celou řadu rostoucí aplikací, jak zapnout a vypnout Azure. J2EE a ASP.NET webová aplikace vztahují, a také iOS, Android a Windows a OSX aplikace. Telemetrie je odeslána ze sady SDK vytvořené pomocí aplikace pro analyzovat a zobrazení ve službě Application Insights v Azure.

Pokud chcete více specializované analýzy, exportu datový proud telemetrie do databáze, nebo k Power BI nebo jiných nástrojů.

**Přehled scénářů aplikace**

Při vývoji aplikace. Může být webové aplikace nebo aplikace na zařízení nebo aplikace na zařízení s webové back-end.

* Po publikování vylaďte výkon vaší aplikace, nebo když je v zátěžové testování.  Application Insights agreguje telemetrických dat ze všech nainstalovaných instancí a nabídne vám grafy odezvy, žádost a počty výjimka, závislosti odezvy a další ukazatele výkonu. Ti optimalizaci výkonu vaší aplikace. Můžete vložit kód pro nahlášení více konkrétní data, pokud je to nutné.
* Najít a diagnostikovat problémy ve vaší živé aplikaci. Výstrahy můžete získat e-mailem, pokud ukazatele výkonu křížové přijatelné prahové hodnoty. Můžete prozkoumat konkrétní uživatelské relace, například zobrazíte požadavek, který způsobil výjimku.
* Sledovat využití pro vyhodnocení úspěch každé nové funkce. Když navrhujete nový uživatelský scénář, naplánujte měření kolik se používá, a zda uživatelé dosáhli svých cílů očekávané. Application Insights získáte základní informace o využití dat, jako je například zobrazení webové stránky, a můžete vložit kód ke sledování uživatelské prostředí podrobněji.

### <a name="automation"></a>Automation
Žádná Neradi ztrácet čas provádění stejných procesů ruční opakovaně. Azure Automation nabízí způsob, jak k vytváření, monitorování, správu a nasazení prostředků v prostředí Azure.  

Automatizace používá "sady runbook", který používá pracovní postupy prostředí Windows PowerShell (oproti právě regulární PowerShell) v pozadí. Sady Runbook mají provést, bez zásahu uživatele. Pracovní postupy prostředí PowerShell umožňuje stav skript uložit kontrolní body na cestě. Pokud dojde k selhání, nebudete muset spustit skript od začátku. Od posledního kontrolního bodu, můžete ji restartovat. Tím ušetříte velké množství pracovních pokusu o vytvoření skriptu zpracovat všechny možné selhání.

**Scénáře automatizace**

Služby Azure Automation je vhodné použít k automatizaci ruční, dlouhotrvajících, problematických a často se opakujících úloh v Azure.

### <a name="api-management"></a>API Management
Vytváření a publikování aplikace programátory rozhraní (API) na Internetu je běžný způsob poskytování služeb pro aplikace. Pokud tyto služby jsou resellable (například počasí data), organizace může povolit jiných třetích stran pro tyto stejné služby za úplatu přístup. Při změně měřítka další partnerům, budete muset obvykle optimalizovat a řízení přístupu.  Někteří partneři může potřebovat i data do jiného formátu.

Azure API Management usnadňuje organizacím bezpečně a škálované publikovat rozhraní API a partnery, zaměstnanci a jiných společností. Poskytuje jiný koncový bod rozhraní API a funguje jako proxy pro volání skutečný koncový bod při současném poskytování služby, jako je ukládání do mezipaměti, transformaci, omezení, řízení přístupu a analýzy agregace.

**Scénáře správy rozhraní API**

Řekněme, že má vaše společnost skupině zařízení, že všechny potřeba centrální službu a získat data – například zpětné volání přesouvání společnost, která má zařízení v každé vůz na cestách.  Společnost jistě budou chtít nastavit sledování vlastní dodávky tak, aby ho spolehlivě předpovědi a doručení časy aktualizace systému. Může vědět, kolik dodávky má a plánování správně.  Každý vůz bude nutné zařízení, která volá zpět do centrálního umístění s jeho umístění a rychlost daty a případně více.

Zákazník přesouvání společnosti by pravděpodobně také těžit z získávání tato umísťovací data.  Zákazník může použít k vědět, jak daleko je nutné produkty cestují, kde se zablokuje, kolik jejich platícího podél určité trasy (Pokud je v kombinaci s jejich placené dodávat). Pokud společnost přesouvání agreguje tato data již, může pro něj platí mnoho zákazníků.  Ale přesouvání společnosti musí poskytnout způsob, jak poskytnout zákazníkům data. Po poskytnou přístup zákazníkům, se nemusí mít kontrolu nad jak často je dotazován data. Budou muset zadat pravidla o tom, kdo má přístup k jaká data. Všechna tato pravidla se musel jde integrovat přímo do jejich externí rozhraní API. Toto je, kde může pomoci API Management.  

## <a name="identity-and-access"></a>Identita a přístup
Práce s identitou je součástí většinu aplikací. Zjištěním je uživatel umožňuje aplikaci rozhodnout, jak by měla spolupracovat s tímto uživatelem. Azure poskytuje služby k usnadnění sledování identity a také integrovat do úložiště identit, které už používáte.

### <a name="active-directory"></a>Active Directory
Podobně jako většina adresářové služby Azure Active Directory ukládá informace týkající se uživatelů a organizací, ke které patří. Umožňuje uživatelům přihlášení a potom jim poskytuje s tokeny, které můžou poskytnout aplikací k prokázání své identity. Také umožňuje synchronizaci uživatelských údajů s Windows Server Active Directory spuštěna místně v místní síti. Při mechanismy a formáty dat používá Azure Active Directory nejsou identické s těmi, která používá v systému Windows Server Active Directory, jsou velmi podobné funkce, které provádí.

Je důležité si uvědomit, že Azure Active Directory je určen především pro použití cloudové aplikace. Může sloužit aplikace spuštěné v Azure, například nebo na jiných platformách cloudu. Použije se také ve společnosti Microsoft vlastní cloudové aplikace, jako jsou v Office 365. Pokud chcete rozšířit vašeho datacentra do cloudu pomocí Azure Virtual Machines a virtuální síť Azure, ale Azure Active Directory není správná volba. Místo toho budete chtít spustit Windows Server Active Directory ve virtuálních počítačích.

Umožníte přístup k informacím, které obsahuje aplikace Azure Active Directory poskytuje Azure Active Directory Graph volat rozhraní RESTful API. Toto rozhraní API umožňuje aplikacím na všechny objekty adresáře přístup platformy a vztahů mezi nimi.  Oprávnění aplikace může například použít toto rozhraní API Další informace o uživatele, skupiny, které patří do a další informace. Aplikace můžete také zobrazit vztahy mezi jejich uživatelé sociálního grafu – povolení, je více inteligentně pracovat připojení mezi lidí.

Další funkce této služby Azure Active Directory, řízení přístupu, usnadňuje aplikace tak, aby přijímal informací o identitě ze sítě Facebook, Google, Windows Live ID a jiných poskytovatelů identit oblíbených. Místo nutnosti aplikace pochopit formáty různých dat a protokolech používaných každý z těchto poskytovatelů, řízení přístupu překládá všechny z nich do jednoho běžný formát. Umožňuje také aplikace přijmout přihlášení z jedné nebo více domén služby Active Directory. Dodavatele poskytuje aplikaci SaaS může například použít Azure Active Directory řízení přístupu uživatelům v každé z jeho zákazníci jednotného přihlašování k aplikaci.

Adresářové služby jsou základní podpora místního computing. By neměl být překvapivé jste, že mají také důležité v cloudu.

### <a name="multi-factor-authentication"></a>Multi-factor Authentication
![Azure Multi-Factor Authentication](./media/fundamentals-introduction-to-azure/MFAIntroNew.png)   

*Obrázek: Multi-Factor Authentication poskytuje funkce pro vaše aplikace k ověření více než jednu formu identifikace*

Zabezpečení je vždy důležité. Vícefaktorové ověřování (MFA) pomáhá zajistit, že přístup pouze uživatelé, sami své účty. Vícefaktorové ověřování (také označované jako dvojúrovňové ověřování nebo "2FA") vyžaduje, že uživatelé zadali dvěma z těchto tří způsobů ověření identity pro uživatelská přihlášení a transakce.

* Něco znáte (obvykle heslo)
* Něco co uživatel má (důvěryhodné zařízení, která není duplikovaná snadno, například telefon)
* Něco že se (biometrika)

Proto když se uživatel přihlásí, můžete vyžadovat je také ověřit svou identitu pomocí mobilní aplikace, telefonního hovoru nebo textové zprávy v kombinaci s své heslo. Ve výchozím nastavení Azure Active Directory podporuje použití hesel jako jediné metody ověřování pro přihlášení uživatele. Pomocí sady SDK vícefaktorového ověřování můžete MFA společně s Azure AD nebo pomocí vlastní aplikace a adresáře. Také můžete jej spolu s místními aplikacemi pomocí aplikace Multi-Factor Authentication Server.

**Scénáře vícefaktorového ověřování**

Ochrana přihlášení na citlivé účty, jako jsou banky přihlášení a kde neoprávněným položka může mít vysokou finanční nebo duševní vlastnost náklady přístup ke zdrojovému kódu.   

## <a name="mobile"></a>Mobilní
Pokud vytváříte aplikace pro mobilní zařízení, může pomoct Azure ukládání dat v cloudu, ověřování uživatelů a odesílat nabízená oznámení bez nutnosti psaní značnou část vlastního kódu.

Pokud určitě můžete vytvořit back-end mobilní aplikace pomocí virtuálních počítačů, cloudových služeb nebo webových aplikací, strávíte mnohem méně času zápis základní součásti služby pomocí služby Azure.

### <a name="mobile-apps"></a>Mobile Apps
![Mobile Apps](./media/fundamentals-introduction-to-azure/MobileServicesIntroNew.png)

*Obrázek: Mobilních aplikací poskytuje funkce, které jsou běžně požadovaných aplikací, které rozhraní s mobilními zařízeními.*

Azure Mobile Apps poskytuje mnoho užitečné funkce, které můžete ušetřit čas při vytváření back-end mobilní aplikace. Umožňuje provést jednoduché zřizování a správy dat uložených v databázi SQL. V kódu na straně serveru můžete snadno použít možnosti úložiště další data jako úložiště objektů blob nebo MongoDB. Mobile Apps poskytuje podporu pro oznámení, když v některých případech můžete místo toho použít Notification Hubs podle níže uvedeného popisu.  Služba má také REST API, které mobilní aplikace můžete volat ke své práci. Mobilní aplikace také poskytuje schopnost ověřovat uživatele prostřednictvím společnosti Microsoft a služby Active Directory a jiných poskytovatelů identit dobře známé jako je Facebook, Twitter a Google.   

Můžete použít jiné služby Azure, jako je Service Bus a rolí pracovního procesu a připojení k místním systémům. 3. stran doplňky z obchodu Azure (např. sendgrid vám umožňuje se e-mailu) můžete využívat i dodatečných funkcí.

Nativní klientské knihovny pro Android, iOS, HTML/JavaScript, Windows Phone a Windows Store usnadňují vývoj aplikací na všechny hlavní mobilní platformy. Rozhraní REST API umožňuje používat funkce data a ověřování Mobile Services s aplikací na různých platformách. Jedné mobilní služby můžete zálohovat více klientských aplikací, můžete poskytovat konzistentní uživatelské prostředí na zařízeních.

Protože Azure již podporuje masivní škálování, může zpracovávat provoz, jakmile aplikace k více oblíbených.  Sledování a protokolování jsou podporovány při řešení problémů a správa výkonu.

### <a name="notification-hubs"></a>Notification Hubs
![NotificationHubs](./media/fundamentals-introduction-to-azure/NotificationHubsIntroNew.png)  

*Obrázek: Notification Hubs poskytuje funkce, které jsou běžně požadovaných aplikací, které rozhraní s mobilními zařízeními.*

Zatímco můžete napsat kód udělat oznámení v Azure Mobile Apps, Notification Hubs je optimalizován pro všesměrové vysílání miliony vysoce individuální nabízená oznámení během minut.  Nemusíte si dělat starosti o podrobnosti, třeba mobilní operátor nebo výrobce zařízení. Můžete vybrat jednotlivé nebo milionům uživatelů na základě jednoho volání rozhraní API.

Centra oznámení je navržen pro práci s jakýkoli back-end. Můžete použít vlastní back-end v cloudu systémem kteréhokoli poskytovatele služeb, nebo místní back-end mobilní aplikace Azure.

**Scénáře centra oznámení** Pokud jste zapisovali mobilní hru kde přehrávače trvalo zapne, budete muset oznámit player 2, které player 1 dokončení jeho vypněte. Pokud to je všechno, co je potřeba, můžete použít právě mobilních aplikací. Pokud jste měli 100 000 uživatelů, ale vaše hře a chcete odeslat době citlivé nabídka bezplatné všem uživatelům, Notification Hubs je vhodnější.

Můžete odeslat novinek, sporting události a produktu oznámení oznámení milionům uživatelů s nízkou latencí. Podniky uvědomí svým zaměstnancům o nový čas citlivé komunikace, např. prodejní tipy, tak, že zaměstnanci nemusí neustále zkontrolujte e-mailu nebo jiné aplikace při udržení informovanosti. Můžete také odeslat jednoho-jednorázových hesel potřebných pro službu Multi-Factor authentication.

## <a name="back-up"></a>Zálohování
K zálohování a obnovení dat musí každý enterprise. Azure slouží k zálohování a obnovení aplikaci v cloudu nebo místně. Azure nabízí různé možnosti, můžete v závislosti na typu zálohování.

### <a name="site-recovery"></a>Site Recovery
Azure Site Recovery (dříve správce obnovení technologie Hyper-V) můžete chránit důležité aplikace ve spolupráci replikaci a obnovení na webech. Site Recovery poskytuje možnost k ochraně aplikací založených na technologii Hyper-v, VMWare nebo sítě SAN vlastní sekundární lokality, lokality hostitele nebo do Azure a vyhnout se nákladům a složitým vytváření a Správa vlastního sekundárního umístění. Azure šifruje data a komunikace a máte možnost povolit šifrování pro data v rest příliš.

Neustále monitoruje stav vašim službám a pomáhá automatizovat řádné obnovení služeb v případě výpadku lokality na primární datové centrum. Virtuální počítače se dají iniciovat organizovaně, což podporuje rychlé obnovení služby, a to i v případě složitých několikaúrovňových úloh.

Site Recovery spolupracuje s existujících technologií, jako jsou repliky technologie Hyper-V, System Center a SQL serveru Always On. Podívejte se na [přehled Azure Site Recovery](site-recovery/site-recovery-overview.md) další podrobnosti.

### <a name="azure-backup"></a>Azure Backup
![Azure Backup](./media/fundamentals-introduction-to-azure/AzureBackupIntroNew.png)  

*Obrázek: Azure Backup zálohuje data z místních serverů, Windows do cloudu.*  

Azure Backup zálohuje data z místních serverů se systémem Windows Server v cloudu. Můžete spravovat své zálohy přímo z nástroje Zálohování v systému Windows Server 2012, Windows Server 2012 Essentials nebo System Center 2012 – Data Protection Manager. Alternativně můžete použít speciální agenta zálohování.

Data je bezpečnější, protože zálohování se šifrují před samotným přenosem a uložené v šifrované Azure a chráněn certifikát, který můžete nahrávat na server. Služba používá stejný ochrany dat redundantní a vysoce dostupné nalezen ve službě Azure Storage.  Můžete zálohovat soubory a složky v pravidelných intervalech nebo okamžitě, úplné nebo přírůstkové zálohování. Po zálohování do cloudu, mohou oprávnění uživatelé snadno obnovit zálohy na libovolný server. Nabízí také zásady uchovávání informací konfigurovat data, komprese dat a přenosu dat omezení tak náklady na úložiště a přenosu dat můžete spravovat.

**Scénáře pro zálohování Azure**

Pokud jste už používá Windows Server nebo System Center, zálohování Azure je přirozené řešení pro zálohování své servery systému souborů, virtuální počítače a databáze systému SQL Server.  Funguje s šifrovaný zhuštěný a komprimovaných souborů. Existují určitá omezení, takže byste měli [Zkontrolujte předpoklady Azure Backup](http://technet.microsoft.com/library/dn296608.aspx) první.

## <a name="messaging-and-integration"></a>Zasílání zpráv a integrace
Bez ohledu na to, co je to kód často potřebuje komunikovat s jiným kódem.  V některých situacích všechno, co je potřeba je základní zařazených do fronty zasílání zpráv. V ostatních případech se vyžadují složitější interakce. Azure poskytuje několik různých způsobů k řešení těchto problémů. Obrázek 5 ukazuje volby.

### <a name="queues"></a>Fronty
![Předávání přes Azure Service Bus](./media/fundamentals-introduction-to-azure/QueuesIntroNew.png)

*Obrázek: Fronty povolit přijít spojení mezi částí aplikace a usnadnit tak škálování.*  

Služba Řízení front je jednoduchý: jednu aplikaci umístí zprávu do fronty a zprávy je nakonec pro čtení, jiná aplikace. Pokud aplikace potřebuje pouze této přehledné služby, fronty Azure může být nejlepším řešením.

Kvůli způsobu, kterým Azure vzrostla v čase fronty úložiště Azure a fronty služby Service Bus poskytuje podobné služby Řízení front služby. Z důvodů, proč byste měli použít jeden z nich jsou popsané v dokumentu poměrně technické [fronty Azure a fronty služby Service Bus - porovnání a na rozdíl od aktualizovaného](http://msdn.microsoft.com/library/azure/hh767287.aspx).  V mnoha scénářích buď pracovat.

**Scénáře fronty**

Jedno běžné použití front je dnes umožníte komunikaci s instancí role pracovního procesu v rámci stejné cloudové služby aplikace instanci webové role.

Předpokládejme například, že vytvoříte Azure aplikace pro sdílení videa. Aplikace se skládá z PHP kód spuštěný ve webové roli, která umožňuje uživatelům nahrávání a sledovat videa, společně s rolí pracovního procesu implementována v jazyce C#, který převádí nahrané video do různých formátech.

Při instanci webové role získá nové video od uživatele, ho můžete uložit do objektu BLOB na video a pak odešle zprávu do role pracovního procesu prostřednictvím fronty oznamující, kde se toto nové video. Pracovní role instance-it není důležité, které bude jeden potom číst zprávy z fronty a provádět požadované video překlady na pozadí.

Strukturování aplikace tímto způsobem umožňuje asynchronní zpracování, a také způsobí, že aplikace snazší škálování, vzhledem k tomu, že počet instance webových rolí a instancí rolí pracovního procesu můžete upravit nezávisle. Můžete taky velikost fronty jako trigger škálovat počet rolí pracovního procesu nahoru či dolů. Příliš vysoké, a můžete přidat více rolí. Pokud získá nižší, můžete snížit počet rolí ušetřit peníze.  

Tento stejný vzor mezi mnoha různých částí aplikace můžete použít i v případě, že nemáte používají webové a pracovní role.  Umožňuje škálovat části na obou stranách fronty nahoru či dolů jako vyžádání a vyžaduje doba zpracování.

### <a name="service-bus"></a>Service Bus
Jestli spustit i v cloudu, ve vašem datovém centru na mobilní zařízení, nebo jinde, musí být aplikace komunikovat. Cílem Azure Service Bus je umožnit aplikacím prakticky odkudkoli vyměňovat data.

Kromě fronty (1: 1) dříve popisované Service Bus taky poskytuje další metody komunikace.

#### <a name="service-bus-relay"></a>Service Bus Relay
![Předávání přes Azure Service Bus](./media/fundamentals-introduction-to-azure/ServiceBusRelayIntroNew.png)

*Obrázek: Předávání přes Service Bus umožňuje komunikaci mezi aplikacemi na různých stranách bránou firewall.*

Service Bus umožňuje přímé komunikaci prostřednictvím své službě předávání zajištění zabezpečení způsob, jak komunikovat přes brány firewall. Předávací služby Service Bus umožňují aplikacím komunikovat výměnou zpráv prostřednictvím koncového bodu hostované v cloudu, nikoli místně.

**Scénáře předávání přes Service Bus**

Aplikace, které komunikují přes službu Service Bus může být Azure aplikace nebo software spuštěný na některé jiné cloudové platformě. Může být také aplikace, které běží mimo cloud, ale. Například představit letecká společnost, která implementuje rezervace služby v počítačích ve vlastním datovém centru. Letecká společnost musí vystavit tyto služby pro mnoho klientů, včetně vrácení se změnami veřejné terminály v letištích, rezervace agenta terminály a možná i telefony zákazníků. Service Bus může použít k tomu, vytváření volně párované interakcí mezi různými aplikacemi.

#### <a name="service-bus-topics-and-subscriptions"></a>Témata a odběry týkající se Service Bus
![Témata služby Azure Service Bus](./media/fundamentals-introduction-to-azure/ServiceBusTopicsSubsIntroNew.png)   
 *Obrázek: Témata služby Service Bus umožňuje více aplikacemi při odesílání zprávy a jiné aplikace při přihlášení k odběru pro příjem zpráv, které splňují určitá kritéria.*

Service Bus poskytuje mechanismus publikovat a odebírat volá témat a odběrů. S publish-subscribe aplikace mohou zasílat zprávy do tématu, zatímco jiné aplikace můžete vytvářet odběry k tomuto tématu. To umožňuje na více komunikace mezi sadu aplikací, když necháte stejnou zprávu přečíst více příjemců.

**Témata služby Service Bus a odběrů scénáře**

Kdykoli nastavujete kde existuje mnoho zpráv, které jsou všechny důležité, ale různých systémů pro příjem dat stačí pro naslouchání na různé podmnožiny tyto komunikace témata sběrnice a jsou dobrou volbou, odběry.

### <a name="biztalk-services"></a>BizTalk Services
![BizTalk Services](./media/fundamentals-introduction-to-azure/BizTalkServicesIntroNew.png)   
 *Obrázek: Služba BizTalk Services nabízí možnost transformace formáty XML zpráv v cloudu.*

Někdy je nutné připojit systémy, které komunikují pomocí různých formátech zasílání zpráv. Je běžné pro firmy k jiné databázi schémata a XML zasílání zpráv formáty, i když, což je běžný standard je k dispozici. Místo zapisovat velké množství vlastní kód, můžete použít místní BizTalk Server integrovat různými systémy.  Služba Azure BizTalk Services poskytuje stejný typ služby, ale v cloudu. Může platit pouze co používáte a nestarat se o škálování, jako by bylo nutné místně.

**BizTalk Services scénáře**

Interakce Business-to-Business (B2B) běžně vyžadují tento typ posunutí.  Například společnost vytváření letadla musí pořadí částí z jeho různé části poskytovatele. Bude mít mnoho dodavatelů částí.  Tyto objednávky dobré automatizovat přejdete přímo ze systémů počítačů letadle do systémů dodavatelů.  Ani obchodní chce změnit jejich základní systémy a formáty zpráv a je velmi nepravděpodobné, že tyto formáty jsou stejné. BizTalk Services můžete provést zprávy a převod mezi nové formáty obou směrech. Buď letadle dodavatele můžete práci převede nebo můžete různých dodavatelů, v závislosti na tom, kdo chce větší kontrolu a množství překlad potřeby.     

## <a name="compute-assistance"></a>Výpočetní pomoc
Azure poskytuje pomoc pro služby, které není potřeba spustit vždy.  

### <a name="scheduler"></a>Scheduler
![Azure Scheduler](./media/fundamentals-introduction-to-azure/SchedulerIntroNew.png)   
*Obrázek: Azure Scheduler poskytuje způsob, jak plánování úloh v určitém čase pro určitou dobu.*

Aplikace v některých případech nutné spustit v určitém čase. V Azure můžete ušetřit peníze s tímto typem aplikace místo aplikace právě běžet 24 x 7 čekání data ke zpracování. Azure Scheduler umožňuje naplánovat, kdy aplikace by neměl být spuštěný na základě intervalu, času nebo kalendáře. Je spolehlivé a ověří, že proces běží i v případě, že tam jsou chyby center sítě, počítače a data. REST API Scheduleru používáte ke správě těchto akcí.

Při výskytu naplánované varování Plánovač odešle zprávy protokolu HTTP nebo HTTPS na konkrétní nebo můžete vložit zprávu do fronty úložiště.  Proto musíte mít aplikaci tak mít dostupný koncový bod, nebo ji sledovat frontu úložiště. Potom po získá zprávu, může provádět jakékoli akce je naprogramovaný tak, aby.

**Scénáře plánovače**

* Opakující se akce aplikace: jako příklad služby může pravidelně získání dat z twitteru a shromáždit data do regulární informačního kanálu.
* Každodenní údržba: protokolu zpracování nebo vyřazování, provádění záloh a dalších občas plánování úkolů.
* Úlohy, které spustí v noci.
* Webové aplikace úlohy, například každodenní vyřazování protokolů, zálohování a další úlohy údržby. Správce může rozhodnout zálohovat jeho databáze v 1: 00 každý den pro další 9 měsíců, například.

Rozhraní API Scheduleru umožňuje vytvářet, aktualizovat, odstranit, zobrazení a správa kolekcí úloh a naplánované úlohy prostřednictvím kódu programu.

## <a name="performance"></a>Výkon
Výkon, je důležité pro aplikaci. Aplikace se mají přístup ke stejným datům opakovaně. Jedním ze způsobů ke zlepšení výkonu je ponechat si kopii dat blíže k aplikaci, minimalizovat čas potřebný k jejich načtení. Azure poskytuje různé služby tohoto postupu.

### <a name="azure-caching"></a>Ukládání do mezipaměti Azure
![Ukládání do mezipaměti Azure](./media/fundamentals-introduction-to-azure/AzureCacheIntroNew.png)   
 **Obrázek: Aplikací Azure můžete data v paměti do mezipaměti a i rozdělit do mnoha rolí pracovního procesu**

Přístup k datům uloženým v některém z Azure, je Správa dat služby SQL Database, tabulky nebo objekty BLOB-je velmi rychlé. Ještě přístup k datům, které jsou uložené v paměti je i rychlejší. Z toho důvodu udržování kopie v paměti často používaná data může zlepšit výkon aplikace. Ukládání do mezipaměti v paměti Azure můžete použít k tomu.

Cloudové služby aplikace můžete ukládat data do mezipaměti, a pak načíst přímo bez nutnosti přístupu trvalého úložiště. Mezipaměť je možné udržovat uvnitř vaší aplikace virtuálních počítačů nebo poskytovaný virtuální počítače vyhrazený pro ukládání do mezipaměti. V obou případech mohou být distribuovány do mezipaměti, s daty obsahuje šíření mezi více virtuálních počítačů v datovém centru Azure.

Azure má několik různých mezipaměti technologie, které mají zapuštěno v čase. V pořadí, že byly zavedeny, je sdílený, v roli, spravovat a Redis cache. Sdílené ukládání do mezipaměti je technologie starší a byste je neměli vytvářet nové implementace s ním. Spravované mezipaměti má stejné funkce mezipaměť hostovaná v instanci Role, ale služba se spravuje mimo Azure Management Portal. Redis Cache je ve verzi preview. Implementace Redis má největší počet funkcí a doporučuje se při psaní nový kód pro ukládání do mezipaměti.

**Scénáře Azure Cache**

Aplikace, která opakovaně čte katalog produktů může využívat tento druh ukládání do mezipaměti, třeba od data potřebuje bude k dispozici rychleji. Technologie také podporuje uzamčení, nechá ji použije se pro čtení a zápis, jakož i data jen pro čtení. A aplikace ASP.NET můžete použít službu pro ukládání dat relace s právě změně konfigurace.

### <a name="content-delivery-network"></a>Content Delivery Network
![Azure CDN](./media/fundamentals-introduction-to-azure/CDNIntroNew.png)   
 **Obrázek: Kopie objektu blob můžete uložit do mezipaměti v lokalitách po celém světě.**

Předpokládejme, že potřebujete k ukládání dat objektů blob, který bude mít přístup uživatelé po celém světě. Možná je video nejnovější poháru shody, například aktualizace ovladačů nebo oblíbených elektronických knih. Uložení kopie dat v několika datových centrech Azure pomůže, ale pokud existují velký počet uživatelů, není pravděpodobně dostatek. Ještě lepší výkon můžete použít Azure CDN.

CDN má desítek lokality po celém světě, každý může ukládání kopií objektů Azure BLOB. Při prvním uživatele v některé části na světě přístupu konkrétním objektu blob, informace, které obsahuje zkopírován z datového centra Azure do místního úložiště CDN v tomto geography. Poté použije přistupuje z příslušné části na světě kopírovat objekt blob uložené v mezipaměti v CDN-nebudete muset přejít zcela na nejbližší datové centrum Azure. Výsledkem je rychlejší přístup k často používaná data uživatelé kdekoli v celém světě.

**Scénáře CDN**

Je běžné používat CDN pomocí služby Media Services k poskytování video po celém světě. Video je obvykle velké a vyžaduje velké šířky pásma.  Služba Media Services je věnovala jinde na této stránce.

## <a name="big-data-and-big-compute"></a>Velké objemy dat a Big Compute
### <a name="hdinsight-hadoop"></a>HDInsight (Hadoop)
![HDInsight](./media/fundamentals-introduction-to-azure/HDInsightIntroNew.png)   
 **Obrázek: HDInsight pomáhá s hromadné zpracování obrovské objemy dat.**

Po celá léta hromadné analýzy dat bylo na relační data uložená v datovém skladu vytvořené s relační databázového systému. Tento druh obchodní analýza je stále důležité a bude po dlouhou dobu pochází. Ale co když data chcete analyzovat je tak velká, že relačních databází právě nemůže zpracovat ho? A Předpokládejme, že není relační data? Může to být, že v protokolech serveru v datacentru, například nebo data historické události ze senzorů nebo něco jiného. V takových případech musíte, která se označuje jako problém velkých objemů dat. Budete potřebovat další přístup.

Dominantní technologie dnes pro analýzu velkých objemů dat je Hadoop. Apache otevřete projekt source, tato technologie ukládá data pomocí Hadoop Distributed File System (HDFS) a pak umožňuje vývojářům vytvářet úlohy MapReduce k analýze dat. HDFS šíří dat napříč několika servery, pak bloky spustí úlohu MapReduce na každé z nich, když necháte velkých objemů dat se zpracovávají paralelně.

HDInsight je název služby Apache Hadoop založené Azure. HDInsight umožňuje HDFS ukládání dat v clusteru a distribuovat ji do víc virtuálních počítačů. Šíří logiku úlohy MapReduce se také v těchto virtuálních počítačů. Podobně jako s Hadoop místní data jsou zpracovaná místně v logiku a data v pracuje se ve stejném virtuálním počítači- a paralelní pro dosažení vyššího výkonu. HDInsight také můžete uložit data v Azure Storage trezoru (ASV), který používá objekty BLOB.  Pomocí ASV umožňuje ušetřit peníze, protože můžete odstranit svému clusteru HDInsight, když není používán, ale stále uchovávat data v cloudu.

HDinsight podporuje jiných součástí ekosystému Hadoop, včetně Hive a Pig. Společnost Microsoft vytvořila také součásti, které usnadňují pracovat s daty vytvořeného v prostředí HDInsight pomocí tradičních nástrojů BI, například adaptér HiveODBC a Průzkumníku dat, které pracují v aplikaci Excel.

### <a name="high-performance-computing-big-compute"></a>High-Performance Computing (Big Compute)
Jedním z nejvíce atraktivní způsobů, jak používat cloudové platformy je spustit vysokovýkonného výpočetního prostředí (HPC) a dalších aplikacích "Big Compute". Mezi příklady patří specializované engineering aplikacích vytvořených pro standardní rozhraní MPI (Message Passing) a také takzvané paralelně zpracovatelné aplikace, tyto modely finančních rizik.

Podstatu Big Compute provádí kódu na mnoho počítačů současně. V Azure to znamená systémem mnoho virtuálních počítačů současně, všechny práce paralelně vyřešit nějaký problém. Tato funkce vyžaduje nějakým způsobem k prostředkům a naplánovat aplikací, tj., distribuci práci přes tyto instance. Volné HPC Pack společnosti Microsoft a jiných řešení výpočetního clusteru můžete provést i v Azure, pořízení výhod služeb Azure výpočty a infrastruktura pro přidání kapacity na vyžádání do výpočetní cluster místní nebo spouštět aplikace Big Compute zcela v cloudu.

Azure poskytuje rozsah VM instance velikosti s různými konfiguracemi procesoru jádra, paměti, kapacity disku a dalších vlastností pro splnění požadavků různých aplikací. Nedávno přináší práce instancích A8 a A9 i pro mnoho výpočetních zatížení s intenzivním a paralelních aplikací MPI, protože mají vysokou rychlostí, vícejádrovými procesory a velké objemy paměti. V některých konfiguracích instance využít síť s nízkou latencí a vysokou propustností aplikace v cloudu, která zahrnuje do paměti vzdáleného přímý přístup do (počítače RDMA) technologie pro maximální efektivity paralelních aplikací MPI.

Azure také nabízí Big Compute aplikace vývojářům a partnerům úplnou sadu kapacita výpočetních operací, služeb, možností architektur a vývojářských nástrojů. Azure podporuje vlastní pracovní postupy Big Compute zahrnující pracovní postupy specializované dat a úlohy a úlohy plánování vzorů, které je možné škálovat tisíce výpočetní jader.

## <a name="media"></a>Média
![Azure Media Services](./media/fundamentals-introduction-to-azure/MediaServicesIntroNew.png)   
 **Obrázek: Služba Media Services je platforma pro aplikace, které poskytují videa a dalších média klientům po celém světě.**

Video tvoří velkou část internetové přenosy dnes, a že procento bude i větší zítra. Zatím zajišťuje videa na webu není jednoduché. Existuje velký počet proměnné, například algoritmus kódování a rozlišení obrazovky uživatele. Video se taky obvykle mají shluky požadavků, jako je sobota noci Špička při mnoha lidmi se rozhodnete, že se mají sledovat online film.

S ohledem jeho oblíbenosti je sejfu tipu, že mnoho nové aplikace bude vytvořen použití videa. Zatím všechny z nich bude muset vyřešit některé problémy, které jsou stejné a provádění každé z nich řešení těchto problémů na svůj vlastní díky žádné smysl. Je vhodnější je vytvoření platformu, která poskytuje společné řešení pro mnoho aplikací používat. A sestavování této platformě v cloudu má několik výhod zrušte. Může být široce dostupné na základě průběžnými platbami, a můžete také zpracovat variabilitu požadavků, které často čelí video aplikace.

Tento problém řeší Azure Media Services. Poskytuje sadu cloudu součásti, které usnadňuje životnosti uživatelům vytváření a spouštění aplikací s použitím média videa a dalších.

Jak ukazuje obrázek, služba Media Services poskytuje sada komponent pro aplikace, které pracují s videa a jiné médium. Například obsahuje na média ingestování součást nahrát video ve službě Media Services (kde je uložený v Azure BLOB), kódování součást, která podporuje různé formátů videa a zvuku, ochrana obsahu komponenty, která poskytuje digitální rights management, komponentu pro vkládání reklamy do datový proud videa, součásti pro streamování a další. Partneři společnosti Microsoft můžete také zadat součásti pro platformu a pak v aplikaci Microsoft distribuovat tyto součásti a účtovat jejich jménem pošle.

Aplikace, které používají tuto platformu lze spustit v Azure nebo jinde. Například aplikace pro úklidové video produkční může uživatelům jeho nahrání video ke službě Media Services, pak zpracovat různými způsoby. Alternativně může správy obsahu cloudové služby spuštěné v Azure spoléhají na Media Services ke zpracování a distribuovat video. Všude, kde běží a jakým funguje, každá aplikace zvolí součástí, které je nutné použít, a k nim přistupovat pomocí rozhraní RESTful.

Pokud chcete distribuovat, co ji vytvoří, můžete aplikace použít Azure CDN, jiné CDN, nebo jenom odesílat bits přímo uživatelům. Ale získá existuje, video, které jsou vytvořené pomocí služby Media Services mohou být spotřebovávána různé klientské systémy, včetně systému Windows, Macintosh, HTML 5, iOS, Android, Windows Phone, Flash a Silverlight. Cílem je usnadnit k vytvoření média moderních aplikací.

**Odkazy**

Víc vizuální zobrazení jak funguje služba Media Services, stáhněte si [Azure Media Services plakát][Azure Media Services Poster].

## <a name="commerce"></a>Obchodování
Zvýšení softwaru jako služby je transformace, jak jsme vytvořit aplikace. Je také transformace, jak jsme prodeje aplikace. Vzhledem k tomu, že aplikace SaaS žije v cloudu, má smysl, který by měl vypadat jeho potenciální zákazníky pro řešení online. A tato změna se vztahuje na data, jakož i aplikace. Proč by neměl osoby hledat do cloudu komerčně dostupných datových sad? Oba tyto problémy s vyvinuté společností Microsoft [Azure Marketplace](https://azure.microsoft.com/marketplace/).

![Azure Commerce](./media/fundamentals-introduction-to-azure/CommerceIntroNew.png)   
 **Obrázek: Azure Marketplace a úložiště Azure vám umožní najít a zakoupit aplikace Azure a komerční datové sady a používat jako součást aplikace Azure.**

Rozdíl mezi nimi je Marketplace je mimo Azure Management Portal, že Windows Store je přístupná z uvnitř portálu. Potenciální zákazníky můžete hledat aplikace Azure, které podle jejich potřeb. Zákazníci můžete vyhledat taky komerční datové sady včetně demografické údaje, finanční údaje, zeměpisné údaje a další. Když najít něco, co se jako, že k němu přístup buď od dodavatele, přímo prostřednictvím webových umístění Marketplace nebo úložiště, nebo v některých případech z portálu pro správu. Aplikace můžete také použít rozhraní API služby Bing Search přes Marketplace, bude mít přístup k výsledky vyhledávání na webu.

**Scénáře Commerce**

Sendgrid vám umožňuje je aplikace v úložišti Azure, který umožňuje odeslat e-mailu. Nabízí další funkce, jako je spolehlivé doručení a statistiky.  Můžete zakoupit této aplikace a související služby místo pokusit vytvořit takové infrastruktury sami.  

## <a name="getting-started"></a>Začínáme
Teď, když máte velký obrázek, dalším krokem je zápis vaší první aplikace Azure. Vyberte si jazyk [příslušnou sadu SDK získejte](/downloads/)a pak použijte pro něj. Cloud computing je nový výchozí--začít nyní.

[Azure Media Services Poster]: http://azure.microsoft.com/documentation/infographics/media-services/
