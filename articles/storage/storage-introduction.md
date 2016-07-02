<properties
    pageTitle="Úvod do Storage | Microsoft Azure"
    description="Přehled Azure Storage, online úložiště v cloudu od Microsoftu. Naučte se, jak řešení cloudových úložišť co nejlépe využít ve svých aplikacích."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/23/2016"
    ms.author="tamram"/>

# Úvod do Microsoft Azure Storage

## Přehled

Azure Storage je řešení cloudového úložiště pro moderní aplikace, které se opírají o odolnost, dostupnost a škálovatelnost, aby mohly vyhovět potřebám zákazníků. V tomto článku se vývojáři, profesionálové v oblasti IT a osoby odpovědné za klíčová obchodní rozhodnutí můžou dozvědět:

- Co je Azure Storage a jak se jeho výhody dají využít ve vašich cloudových, mobilních, serverových a desktopových aplikacích
- Jaké druhy dat se dají ukládat ve službách Azure Storage: data objektu blob, data tabulky NoSQL, fronty zpráv a sdílené složky
- Jak se spravuje přístup k vašim datům v Azure Storage
- Jak se chrání vaše data v úložišti Storage pomocí redundance a replikace.
- Kam jít dál, abyste mohli vytvořit svoji první aplikaci pro Azure Storage.

Pokud chcete Azure Storage zprovoznit rychle, podívejte se na téma [Zprovoznění aplikace Azure Storage v pěti minutách](storage-getting-started-guide.md).

Podrobné informace o knihovnách a dalších prostředcích pro práci s Azure Storage najdete dole v části [Další kroky](#next-steps).

## Co je Azure Storage?

Cloudová výpočetní technika umožňuje nové scénáře pro aplikace, které pro svá data vyžadují škálovatelné, odolné a vysoce dostupné úložiště – a právě proto Microsoft vyvinul Azure Storage. Vedle toho, že Azure Storage vývojářům umožní tvořit aplikace ve velkém měřítku, které budou podporovat nové scénáře, na důkaz své robustnosti také nabízí základy úložiště pro Virtuální počítače Azure.

Azure Storage je nesmírně škálovatelná služba, která vám umožní ukládat a zpracovávat stovky terabajtů dat pro scénáře s velkým objemem dat, které jsou předpokladem pro vědeckou nebo finanční analýzu a aplikace pracující s médii. Nebo můžete ukládat malá množství dat, například pro firemní webové stránky. Ať už budete potřebovat cokoli, platit budete jen za uložená data. Azure Storage aktuálně poskytuje úložiště pro bilión jedinečných zákaznických objektů a v průměru zpracovává miliony požadavků za sekundu.

Služba Azure Storage je flexibilní, takže můžete vytvořit aplikace pro velkou globální cílovou skupinu a tyto aplikace škálovat podle potřeby – z pohledu množství uložených dat i počtu požadavků na přístup. Platíte jen za to, co používáte a když to používáte.

Azure Storage používá systém automatického rozdělování do oddílů, který automaticky rozděluje datovou zátěž podle aktuálního provozu. To znamená, že pokud se budou nároky na vaši aplikaci zvyšovat, Azure Storage odpovídajícím způsobem automaticky přidělí zdroje.

Služba Azure Storage je přístupná odkudkoli na světě, z jakéhokoli typu aplikace – ať už běží v cloudu, v desktopovém prostředí, na místním serveru nebo na mobilním zařízení. Azure Storage můžete použít v mobilních situacích, kde aplikace ukládá část dat do zařízení a synchronizuje je s kompletními daty uloženými v cloudu.

Azure Storage podporuje klienty běžící na různých operačních systémech (včetně Windows a Linuxu) a různé programovací jazyky (například .NET, Java, Node.js, Python, Ruby, PHP a C++ a mobilní programovací jazyky) a usnadňuje tak vývoj. Azure Storage taky zpřístupňuje datové prostředky přes jednoduchá rozhraní REST API, která jsou dostupná pro každého klienta, který dokáže odesílat a přijímat data přes HTTP/HTTPS.

Azure Premium Storage nabízí podporu vysoce výkonných disků s nízkou latencí pro intenzivní úlohy náročné na oblast vstup/výstup, které běží ve virtuálních počítačích Azure. Se službou Azure Premium Storage můžete několik trvalých datových disků připojit k virtuálnímu počítači a nastavit je tak, aby odpovídaly vašim požadavkům na výkon. Pro maximální výkon vstupů/výstupů se každý datový disk může opřít o SSD disk v Azure Premium Storage. Další informace najdete v tématu [Premium Storage: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](storage-premium-storage.md).

## Seznámení se službami Azure Storage Services

Úložiště Azure nabízí tyto čtyři služby: úložiště Blob, úložiště Table, úložiště Queue, a úložiště File.

- Blob Storage ukládá nestrukturované datové objekty. Objekt blob může být jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace. Blob Storage se taky může říkat Úložiště objektů.
- Table Storage ukládá strukturované datové sady. Table Storage je datové úložiště na bázi NoSQL typu klíč-atribut, které umožňuje rychlý vývoj a přístup k velkým objemům dat.
- Queue Storage poskytuje spolehlivé zasílání zpráv pro zpracování úloh a komunikaci mezi komponentami cloudových služeb.
- File Storage nabízí sdílené úložiště pro starší aplikace, které používají standardní protokol SMB. Virtuální počítače a cloudové služby Azure můžou sdílet souborová data mezi komponentami aplikace přes sdílené složky a lokální aplikace můžou k souborovým datům ve sdílené složce přistupovat přes REST API služby File.

Účet úložiště Azure je zabezpečený účet, se kterým získáte přístup ke službám v Azure Storage. Váš účet úložiště poskytuje jedinečný obor názvů pro vaše prostředky úložiště. Následující obrázek znázorňuje vztahy mezi prostředky úložiště Azure v účtu úložiště:

![Prostředky Azure Storage](./media/storage-introduction/storage-concepts.png)

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

[AZURE.INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## Blob Storage

Úložiště Blob nabízí cenově výhodné a škálovatelné řešení pro uživatele s velkým množstvím nestrukturovaných dat, která se mají uložit v cloudu. Úložiště Blob můžete použít k uložení například těchto druhů obsahu:

- Dokumenty
- Sociální data, jako jsou fotografie, videa, hudba nebo blogy
- Zálohy souborů, počítačů, databází a zařízení
- Obrázky a text pro webové aplikace
- Konfigurační dat pro cloudové aplikace
- Velké objemy dat, jako jsou protokoly a další velké datové sady

Každý objekt blob se organizuje do kontejneru. Kontejnery také nabízejí praktický způsob přiřazení zásad zabezpečení skupinám objektů. Účet úložiště může obsahovat libovolný počet kontejnerů a kontejner může obsahovat libovolný počet objektů blob až do limitu kapacity úložiště 500 TB.  

Úložiště Blob nabízí tři typy objektů blob – objekty blob bloku, doplňovací objekty blob a objekty blob stránky (disky).

- Objekty blob bloku jsou optimalizované pro streamování a ukládání cloudových objektů a jsou dobrou volbou pro ukládání dokumentů, souborů médií, záloh atd.
- Doplňovací objekty blob jsou podobné objektům blob bloku, ale jsou optimalizované pro doplňovací operace. Doplňovací objekt blob se může aktualizovat jen přidáním nového bloku na konec. Doplňovací objekty blob jsou dobrou volbou pro takové scénáře, jako je například protokolování, kde se nová data potřebují zapisovat jen na konec objektu blob.
- Objekty blob stránky jsou optimalizované pro zastoupení disků IaaS, podporují náhodné zápisy a jejich maximální velikost je 1 TB. Disk IaaS připojení přes síť k virtuálnímu počítači Azure je virtuální pevný disk uložený jako objekt blob.

V případě velkých datových sad, kde stahování nebo ukládání dat do úložiště Blob přes internet není vzhledem k síťovým omezením reálné, můžete zaslat pevný disk společnosti Microsoft, která data exportuje nebo importuje přímo v datovém centru. Další informace najdete v tématu [Přenos dat do Blob Storage pomocí služby Microsoft Azure Import/Export](storage-import-export-service.md).

## Úložiště Table

Moderní aplikace často potřebují datová úložiště s větší škálovatelností a flexibilitou, než potřebovaly starší generace softwaru. Úložiště Table nabízí vysoce dostupné, enormně škálovatelné úložiště, se kterým se vaše aplikace může automaticky škálovat podle požadavků uživatelů. Úložiště Table je úložiště Microsoftu na bázi NoSQL typu klíč/atribut – a na rozdíl od tradičních relačních databází je bez schématu. S datovým úložištěm bez schématu je snadné data přizpůsobovat měnícím se potřebám vaší aplikace. Úložiště Table se snadno používá, takže vývojáři můžou aplikace vytvářet rychle. Přístup k datům je rychlý a cenově výhodný pro všechny typy aplikací.  Využívání úložiště Table Storage obvykle znamená výrazně nižší náklady než tradiční SQL pro podobné objemy dat.

Úložiště Table je úložiště typu klíč-atribut – to znamená, že každá hodnota v tabulce je uložená se typovým názvem vlastnosti. název vlastnosti se může použít pro filtrování a upřesnění kritérií výběru. Kolekce vlastností a jejich hodnot tvoří entitu. Protože úložiště Table nemá schéma, dvě entity ve stejné tabulce můžou obsahovat různé kolekce vlastností a tyto vlastnosti můžou být různých typů.

Úložiště Table Storage můžete používat k ukládání flexibilních datových sad, například uživatelských dat pro webové aplikace, adresářů, informací o zařízení a dalších typů metadat, které vaše služba vyžaduje.  V tabulce můžete uložit libovolný počet entit a účet úložiště může obsahovat libovolný počet tabulek, až do limitu kapacity účtu úložiště.

Stejně jako v případě úložišť Blob a Queue můžou vývojáři spravovat a přistupovat k úložišti Table pomocí standardních protokolů REST, ale úložiště Table navíc podporuje podmnožinu protokolu OData, která zjednodušuje možnosti pokročilého dotazování a umožňuje použití formátů JSON a AtomPub (založeného na XML).

Databáze NoSQL, jako je úložiště Table, nabízejí dnešním internetovým aplikacím oblíbenou alternativu tradičních relačních databází.

## Queue Storage

Při navrhování aplikací pro škálování ve větším měřítku jsou jednotlivé součásti aplikací často nepropojené, aby je bylo možné škálovat nezávisle. Queue Storage poskytuje spolehlivé řešení zasílání zpráv pro asynchronní komunikaci mezi součástmi aplikací bez ohledu na to, jestli běží v cloudu, v desktopovém prostředí, na místním serveru nebo na mobilním zařízení. Queue Storage také podporuje správu asynchronních úloh a pracovní postupy procesů sestavování buildů.

Účet úložiště může obsahovat libovolný počet front. Fronta může obsahovat libovolný počet zpráv až do kapacity účtu úložiště. Jednotlivé zprávy můžou mít velikost až 64 KB.

## File Storage

Azure File Storage nabízí cloudové sdílené složky SMB, takže můžete rychle a bez nákladných přepisů migrovat starší aplikace, které spoléhají na sdílené složky, do Azure. S úložištěm Azure File můžou aplikace běžící v cloudových službách nebo virtuálních počítačích Azure připojit sdílenou složku v cloudu stejným způsobem, jako desktopová aplikace připojí typickou sdílenou složku SMB. Potom může sdílenou složku File Storage připojit a používat libovolný počet aplikací.

Protože je sdílená složka File Storage standardní sdílnou složkou SMB, aplikace běžící v Azure můžou k datům ve sdílené složce přistupovat přes API pro vstup/výstup souborového systému. Vývojáři tedy můžou využít svoje dovednosti a znalosti kódu při migraci stávajících aplikací. Profesionálové v oblasti výpočetní techniky můžou pomocí rutin prostředí PowerShell vytvářet, připojovat a spravovat sdílené složky File Storage v rámci správy aplikací Azure.

Podobně jako ostatní služby úložiště Azure i úložiště File zpřístupňuje rozhraní REST API pro přístup k datům ve sdílené složce. Lokální aplikace můžou zavolat REST API úložiště File a získat přístup k datům ve sdílené složce. Podnik tak má možnost třeba migrovat některé starší aplikace na Azure a ostatní aplikace nechat běžet ze své organizace. Všimněte si, že připojení sdílené složky je možné jen pro aplikace, které běží v Azure. Lokální aplikace má ke sdílené složce možný přístup jen přes REST API.

Distribuované aplikace můžou taky používat úložiště File k ukládání a sdílení užitečných dat aplikace a testovacích nástrojů. Nějaká aplikace může například uložit konfigurační soubory a diagnostická data (protokoly, metriky a výpisy stavu systému) do úložiště File, aby byly dostupné pro několik virtuálních počítačů nebo rolí. Vývojáři a správci můžou do sdílené složky úložiště File uložit nástroje, které potřebují k sestavení nebo správě aplikace. Tato sdílená složka bude přístupná pro všechny komponenty a nebude je potřeba instalovat do každého virtuálního počítače nebo instance role.

## Přístup k prostředkům Blob, Table, Queue a File

Ve výchozím nastavení má k prostředkům v účtu úložiště přístup jen vlastník. Kvůli zabezpečení vašich dat je potřeba každý požadavek směřovaný na prostředky ve vašem účtu ověřit. Ověření používá model sdíleného klíče. Objekty blob se taky dají nakonfigurovat tak, aby podporovaly anonymní ověření.

K vašemu účtu se při vytvoření přiřadí dva privátní přístupové klíče, které se používají pro ověření. Pokud máte k dispozici dva klíče, znamená to, že vaše aplikace bude dostupná i při pravidelném generování nových klíčů, což je při správě bezpečnostních klíčů běžné.

Pokud potřebujete povolit uživatelsky řízený přístup ke svým prostředkům úložiště, můžete vytvořit sdílený přístupový podpis. Sdílený přístupový podpis (SAS) je token, který můžete připojit k URL a který umožní delegovaný přístup k prostředku úložiště. Každý, kdo má tento token, má přístup k prostředku, na který tento token ukazuje, s oprávněními, které tento token vymezuje, po dobu, po kterou je tento token platný. Azure Storage od verze 2015-04-05 podporuje dva druhy sdílených přístupových podpisů: SAS služby a SAS účtu.

SAS služby deleguje přístup k prostředku jen v jedné službě úložiště: službě Blob, Queue, Table nebo File.

SAS účtu deleguje přístup k prostředkům v jedné nebo více službách úložiště. Můžete delegovat přístup k operacím na úrovni služby, které nejsou dostupné se SAS služby. Můžete taky delegovat přístup k operacím čtení, zápis a odstranění pro kontejnery objektů blob, tabulky a sdílené složky, který se nedá vymezit přes SAS služby.

Nakonec můžete nastavit, že kontejner a jeho objekty blob, nebo třeba jen konkrétní objekt blob, jsou veřejně přístupné. Když nějaký kontejner nebo objekt blob označíte jako veřejně přístupný, kdokoli si ho může anonymně přečíst bez nutnosti ověření.  Veřejné kontejnery a objekty blob se hodí pro zpřístupnění prostředků, jako jsou média nebo dokumenty, které jsou hostované na webových stránkách.  Pokud chcete snížit síťovou latenci pro globální cílovou skupinu, můžete data objektů blob, která používají webové stránky, uložit do vyrovnávací paměti pomocí Azure CDN.

Další informace o sdílených přístupových podpisech najdete v tématu [Sdílené přístupové podpisy: vysvětlení modelu SAS](storage-dotnet-shared-access-signature-part-1.md). Další informace o bezpečném přístupu k vašemu účtu úložiště najdete v tématech [Správa anonymního přístupu pro čtení ke kontejnerům a objektům blob](storage-manage-access-to-resources.md) a [Ověření pro služby Azure Storage](https://msdn.microsoft.com/library/azure/dd179428.aspx).

## Replikace pro odolnost a vysokou dostupnost

Data v účtu úložiště Microsoft Azure se vždy replikují, protože se tím zajistí jejich odolnost a vysoká dostupnost a splní se tím nároky [SLA pro úložiště](https://azure.microsoft.com/support/legal/sla/storage/) i při krátkodobém výpadku hardwaru. Při vytvoření účtu úložiště si musíte vybrat jednu z těchto možností replikace:  

- **Místně redundantní úložiště (LRS).** Místně redundantní úložiště udržuje tři kopie dat. LRS se replikuje třikrát v rámci jednoho zařízení v jedné oblasti. LRS chrání vaše data před běžnými výpadky hardwaru, ale ne před výpadkem celého zařízení.  
  
    LRS se nabízí se slevou. Pro maximální odolnost doporučujeme použít geograficky redundantní úložiště popsané dole.


- **Zónově redundantní úložiště (ZRS).** Zónově redundantní úložiště udržuje tři kopie dat. ZRS se replikuje třikrát v rámci dvou nebo tří zařízení buďto v jedné oblasti nebo v rámci dvou oblastí, a nabízí tak větší odolnost LRS. ZRS zajistí, aby vaše data byla odolná v jedné oblasti.  

    ZRS poskytuje větší odolnost než LRS, ale pro maximální odolnost doporučujeme použít geograficky redundantní úložiště popsané dole.  

    > [AZURE.NOTE] ZRS je aktuálně dostupné jen pro objekty blob bloku a podporuje se od verze 2014-02-14.
    > 
    > Pokud vytvoříte účet úložiště a vyberete ZRS, nemůžete později přejít na jiný typ replikace, stejně tak nemůžete z jiného typu replikace přejít na ZRS. 

- **Geograficky redundantní úložiště (GRS)** Při vytváření účtu úložiště se automaticky vybere geograficky redundantní úložiště. GRS udržuje šest kopií dat. S GRS data se replikují třikrát v rámci primární oblasti a třikrát v sekundární oblasti stovky kilometrů od primární oblasti, takže poskytuje nejvyšší úroveň odolnosti. V případě výpadku primární oblasti převezme služby sekundární oblast. GRS zajistí, aby vaše data byla odolná ve dvou oblastech.


- **Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS)** Geograficky redundantní úložiště s přístupem pro čtení replikuje data do sekundárního geografického umístění a v sekundárním umístění poskytne k datům přístup pro čtení. Geograficky redundantní úložiště s přístupem pro čtení vám umožní přistupovat k datům z primárního nebo sekundárního umístění pro případ, že bude jedno umístění nedostupné.

    > [AZURE.IMPORTANT] Pokud jste při vytváření účtu nezvolili replikaci ZRS, můžete způsob replikace později změnit. Pokud ale z LRS přejdete na GRS nebo RA-GRS, může se vám účtovat jednorázový poplatek za přenos dat.
 
Další informace o replikaci úložiště najdete v tématu [Replikace Azure Storage](storage-redundancy.md).

Informace o cenách a sazbách pro jednotlivé způsoby replikace úložiště najdete v tématu [Azure Storage – Ceny](https://azure.microsoft.com/pricing/details/storage/).

Další informace o odolnosti z hlediska architektury v Azure Storage najdete v příspěvku [Studie SOSP - Azure Storage: Služba vysoce dostupného cloudového úložiště se silnou konzistencí](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).


## Přesun dat z Azure Storage a do Azure Storage

Pomocí nástroje příkazového řádku AzCopy můžete kopírovat objekt blob, soubor a tabulková data v rámci svého účtu úložiště nebo mezi různými účty úložiště. Další informace najdete v tématu [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md).

Nástroj AzCopy je postavený na [Knihovně pro přesun dat v Azure](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/), která je aktuálně dostupná v předběžné ukázkové verzi.

Služba Azure Import/Export nabízí způsob, jak importovat data objektů blob do účtu úložiště nebo z něj exportovat data objektů blob z fyzického disku, který uživatel zašle do datového centra Azure. Další informace o službě Import/Export najdete v tématu [Přenos dat do Blob Storage pomocí služby Microsoft Azure Import/Export](storage-import-export-service.md).

## Ceny

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

## API úložiště, knihovny a nástroje

Prostředky Azure Storage jsou dostupné přes jakýkoli jazyk, který umí vytvářet požadavky HTTP/HTTPS. Azure Storage dále nabízí programovací knihovny pro několik oblíbených jazyků. Tyto knihovny zjednodušují spoustu aspektů práce s Azure Storage, protože se starají o drobnosti jako synchronní a asynchronní vyvolání, dávkování operací, řízení výjimek, automatické opakování pokusů, operační chování atd. Knihovny jsou aktuálně dostupné pro následující jazyky a platformy, další se připravují:

### Datové služby Azure Storage

- [REST API služby Azure Storage](http://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Klientská knihovna pro úložiště pro .NET, Windows Phone a Windows Runtime](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Klientská knihovna pro úložiště pro C++](https://github.com/Azure/azure-storage-cpp)
- [Klientská knihovna pro úložiště pro Javu/Android](/develop/java/)
- [Klientská knihovna pro úložiště pro Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
- [Klientská knihovna pro úložiště pro PHP](/develop/php/)
- [Klientská knihovna pro úložiště pro Ruby](/develop/ruby/)
- [Klientská knihovna pro úložiště pro Python](/develop/python/)
- [Rutiny pro úložiště pro PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt269418.aspx)

### Služby správy pro Azure Storage

- [REST API pro poskytovatele prostředků úložiště – referenční informace](https://msdn.microsoft.com/library/azure/mt163683.aspx)
- [Klientská knihovna pro .NET pro poskytovatele prostředků úložiště](https://msdn.microsoft.com/library/azure/mt131037.aspx)
- [Rutiny pro PowerShell 1.0 pro poskytovatele prostředků úložiště](https://msdn.microsoft.com/library/azure/mt607151.aspx)
- [REST API pro správu služeb úložiště (klasické)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### Služby pro přesun dat v Azure Storage

- [REST API pro službu Import/Export úložiště ](https://msdn.microsoft.com/library/azure/dn529096.aspx)
- [Klientská knihovna pro .NET pro přesun dat v úložišti ](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### Nástroje

- [Sady SDK a nástroje Azure](https://azure.microsoft.com/downloads/)
- [Emulátor úložiště Azure](http://www.microsoft.com/download/details.aspx?id=43709)
- [Azure Powershell](../powershell-install-configure.md)
- [Nástroj příkazového řádku AzCopy](http://aka.ms/downloadazcopy)

## Další kroky

Další informace o službě Azure Storage najdete v těchto zdrojích informací:

### Dokumentace

- [Dokumentace k Azure Storage](https://azure.microsoft.com/documentation/services/storage/)

### Pro správce

- [Použití Azure Powershell s Azure Storage](storage-powershell-guide-full.md)
- [Použití Azure CLI s Azure Storage](storage-azure-cli.md)

### Pro vývojáře v rozhraní .NET

- [Začínáme s úložištěm Azure Blob pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md)
- [Začínáme s úložištěm Azure Table pomocí rozhraní .NET](storage-dotnet-how-to-use-tables.md)
- [Začínáme s úložištěm Azure Queue pomocí rozhraní .NET](storage-dotnet-how-to-use-queues.md)
- [Začínáme s úložištěm Azure File ve Windows](storage-dotnet-how-to-use-files.md)

### Pro vývojáře v Javě a Androidu

- [Používání úložiště Blob z Javy](storage-java-how-to-use-blob-storage.md)
- [Používání úložiště Table z Javy](storage-java-how-to-use-table-storage.md)
- [Používání úložiště Queue z Javy](storage-java-how-to-use-queue-storage.md)
- [Používání úložiště File z Javy](storage-java-how-to-use-file-storage.md)

### Pro vývojáře v Node.js

- [Používání úložiště Blob z Node.js](storage-nodejs-how-to-use-blob-storage.md)
- [Používání úložiště Table z Node.js](storage-nodejs-how-to-use-table-storage.md)
- [Používání úložiště Queue z Node.js](storage-nodejs-how-to-use-queues.md)

### Pro vývojáře v PHP

- [Používání úložiště Blob z PHP](storage-php-how-to-use-blobs.md)
- [Používání úložiště Table z PHP](storage-php-how-to-use-table-storage.md)
- [Používání úložiště Queue z PHP](storage-php-how-to-use-queues.md)

### Pro vývojáře v Ruby

- [Používání úložiště Blob z Ruby](storage-ruby-how-to-use-blob-storage.md)
- [Používání úložiště Table z Ruby](storage-ruby-how-to-use-table-storage.md)
- [Používání úložiště Queue z Ruby](storage-ruby-how-to-use-queue-storage.md)

### Pro vývojáře v Pythonu

- [Používání úložiště Blob z Pythonu](storage-python-how-to-use-blob-storage.md)
- [Používání úložiště Table z Pythonu](storage-python-how-to-use-table-storage.md)
- [Používání úložiště Queue z Pythonu](storage-python-how-to-use-queue-storage.md)
- [Používání úložiště File z Pythonu](storage-python-how-to-use-file-storage.md)



<!--HONumber=Jun16_HO2-->


