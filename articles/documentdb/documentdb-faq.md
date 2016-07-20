<properties 
    pageTitle="Otázky k databázi DocumentDB – časté otázky | Microsoft Azure" 
    description="Odpovědi na časté otázky k Azure DocumentDB a dokumentové databázové službě NoSQL pro JSON Odpovědi na otázky k databázi a její kapacitě, úrovním výkonu a škálování" 
    keywords="Database questions, frequently asked questions, documentdb, azure, Microsoft azure"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="03/30/2016" 
    ms.author="mimig"/>


#Časté otázky o DocumentDB

## Otázky k databázi a základům Microsoft Azure DocumentDB

### Co je Microsoft Azure DocumentDB? 
Microsoft Azure DocumentDB je vysoce škálovatelná dokumentová databáze NoSQL poskytovaná jako služba, která nabízí bohaté dotazování na data bez schémat, pomáhá poskytovat konfigurovatelný a spolehlivý výkon a umožňuje rychlý vývoj – to vše přes spravovanou platformu podpořenou výkonem a dosahem Microsoft Azure. DocumentDB je správným řešením pro webové, mobilní a herní aplikace či aplikace IoT, kde mezi klíčové požadavky patří předvídatelná propustnost, nízká latence a datový model bez schémat. DocumentDB nabízí flexibilitu schémat a bohaté indexování prostřednictvím nativního datového modelu JSON. K tomu navíc podporuje transakce s více dokumenty s integrovaným JavaScriptem.  
  
Další otázky, odpovědi a pokyny k nasazení databáze a používání této služby najdete na [stránce dokumentace DocumentDB](https://azure.microsoft.com/documentation/services/documentdb/).

### Jaký druh databáze je DocumentDB?
DocumentDB je databáze NoSQL orientovaná na dokumenty, která data ukládá ve formátu JSON.  DocumentDB podporuje vnořené, nezávislé datové struktury, na které je možné zasílat dotazy prostřednictvím bohaté [gramatiky dotazů SQL](documentdb-sql-query.md) DocumentDB. DocumentDB poskytuje vysoký výkon zpracování transakcí JavaScriptu na straně serveru. Používá k tomu [uložené procedury, triggery a uživatelem definované funkce](documentdb-programming.md). Databáze také podporuje úrovně konzistence s přidruženými [úrovněmi výkonu](documentdb-performance-levels.md), které si vývojář může přizpůsobit.
 
### Mají databáze DocumentDB tabulky jako relační databáze (RDBMS)?
Ne, DocumentDB ukládá data v kolekcích dokumentů JSON.  Informace o prostředcích DocumentDB najdete v tématu [Model prostředků a koncepty DocumentDB](documentdb-resources.md). 

### Podporují databáze DocumentDB data bez schémat?
Ano, DocumentDB umožňuje aplikacím ukládat libovolné dokumenty JSON bez definic nebo parametrů schémat. Na data je možné se okamžitě dotazovat přes rozhraní dotazů SQL pro DocumentDB.   

### Podporuje DocumentDB transakce ACID?
Ano, DocumentDB podporuje transakce mezi dokumenty vyjádřené jako javascriptové uložené procedury a triggery. Transakce jsou v každé kolekci vymezeny do jediného oddílu a spouštěny sémantice ACID jako transakce typu „vše, nebo nic“. Jsou izolovány od ostatních souběžně spuštěných kódů a požadavků uživatelů.  Pokud za běhu kód javascriptové aplikace na straně serveru vyvolá výjimku, celá transakce bude vrácena zpět. 

### Jaké jsou typické scénáře použití DocumentDB?  
DocumentDB je dobrá volba pro nové webové, mobilní a herní aplikace či aplikace IoT, ve kterých je důležité automatické škálování, předvídatelný výkon, rychlé doby odezvy v řádech milisekund a schopnost dotazovat se na data bez schémat. DocumentDB slouží k rychlému vývoji a podpoře nepřetržitých iterací modelů dat aplikace. [Běžnými případy použití DocumentDB](documentdb-use-cases.md) jsou aplikace, které spravují uživatelem generovaný obsah a data.  

### Jak DocumentDB zajišťuje předvídatelný výkon?
Mírou propustnosti v DocumentDB je jednotka žádosti (RU). 1 RU odpovídá propustnosti operace GET u 1kB dokumentu. Každá operace v DocumentDB, včetně čtení, zápisu, dotazů SQL a spuštění uložených procedur, má jasně stanovenou hodnotu jednotek žádosti podle propustnosti, která je nutná k jejímu dokončení. Namísto přemýšlení o procesoru, vstupně-výstupních operacích a paměti a o tom, jak tyto faktory ovlivňují propustnost aplikace, můžete uvažovat na bázi jediné veličiny, kterou je jednotka žádosti.

Každou kolekci DocumentDB je možné obsloužit zajištěnou propustností stanovenou jako jednotky žádosti určující propustnost za sekundu. Pro aplikace jakéhokoli rozsahu můžete provést srovnávací test jednotlivých požadavků, změřit tak jejich hodnoty jednotek žádosti a zřídit kolekce, které zpracují celkový součet jednotek žádosti ze všech požadavků. Také je možné propustnost kolekce zvyšovat nebo snižovat podle toho, jak se vyvíjí vaše aplikace. Pro další informace o jednotkách žádosti a nápovědu k určování potřeb kolekce si prosím přečtěte téma [Správa výkonu a kapacity](documentdb-manage.md).

### Odpovídá DocumentDB požadavkům HIPAA?
Ano, DocumentDB požadavkům HIPAA odpovídá. HIPAA zavádí požadavky na použití, zveřejnění a ochranu jednotlivě identifikovatelných zdravotních informací. Další informace najdete na webu [Centrum zabezpečení Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### Jaká jsou omezení úložiště DocumentDB? 
Neexistuje žádné teoretické omezení množství dat, které kolekce může v DocumentDB uchovávat. Pokud byste v jedné kolekci chtěli uložit více než 250 GB dat, [požádejte prosím podporu](documentdb-increase-limits.md) o zvýšení kvóty vašeho účtu. 

### Jaká jsou omezení propustnosti DocumentDB? 
Neexistuje žádné teoretické omezení propustnosti, kterou kolekce může v DocumentDB podporovat, pokud je možné vaši úlohu přibližně rovnoměrně distribuovat na dostatečně velký počet klíčů oddílů. Pokud chcete u jedné kolekce nebo účtu překročit 250 000 jednotek žádosti za sekundu, [požádejte prosím podporu](documentdb-increase-limits.md) o zvýšení kvóty vašeho účtu. 

### Kolik Microsoft Azure DocumentDB stojí?
Podrobnosti najdete na stránce s [podrobnostmi o cenách za DocumentDB](https://azure.microsoft.com/pricing/details/documentdb/). Poplatky za používání DocumentDB se určují podle počtu používaných kolekcí, počtu hodin, po které byly kolekce online, a spotřebovaného úložiště a zřízené propustnosti pro každou z kolekcí. 

### Je k dispozici bezplatný účet?
Pokud jste Azure ještě nepoužívali, můžete si zaregistrovat [bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/), se kterým získáte 30 dnů a 200 USD na vyzkoušení všech služeb Azure. Nebo pokud máte předplatné nástroje Visual Studio, jste oprávněni získat [bezplatný kredit Azure v hodnotě 150 USD měsíčně](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), který můžete využít pro libovolnou službu Azure.  

### Jak lze získat další pomoc s DocumentDB?
Pokud potřebujete pomoc, kontaktujte nás na webu [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-documentdb), [fórech pro vývojáře MSDN v Azure DocumentDB](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB) nebo si naplánujte [osobní chat s technickým týmem DocumentDB](http://www.askdocdb.com/). Pokud se chcete nechat informovat o novinkách v DocumentDB, sledujte nás na [Twitteru](https://twitter.com/DocumentDB).

## Nastavení Microsoft Azure DocumentDB

### Jak se zaregistruji k Microsoft Azure DocumentDB?
Microsoft Azure DocumentDB je k dispozici na [Portálu Azure][azure-portal].  Nejprve si musíte zaregistrovat předplatné Microsoft Azure.  Jakmile si zaregistrujete předplatné, můžete ke svému předplatnému Azure přidat účet DocumentDB. Pokyny, jak přidat účet DocumentDB, najdete v tématu [Vytvoření databázového účtu DocumentDB](documentdb-create-account.md).   

### Co je hlavní klíč?
Hlavní klíč je token zabezpečení pro přístup ke všem prostředkům účtu. Uživatelé, kteří mají klíč, mají ke všem prostředkům v databázovém účtu přístup pro čtení a zápis. Při distribuci hlavního klíče buďte opatrní. Primární hlavní klíč nebo sekundární hlavní klíč jsou k dispozici v okně **Klíče** na [Portálu Azure][azure-portal]. Další informace o klíčích najdete v tématu [Zobrazení, kopírování a obnovení přístupových klíčů](documentdb-manage-account.md#keys).

### Jak vytvořím databázi?
Databáze můžete vytvářet přes [Portál Azure]() (popsáno v tématu [Vytvoření databáze DocumentDB](documentdb-create-database.md)), pomocí jedné ze sad [DocumentDB SDK](documentdb-sdk-dotnet.md) nebo přes rozhraní [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx).  

### Co je kolekce?
Kolekce je kontejner dokumentů JSON a přidružené logiky javascriptové aplikace. Kolekce je fakturovatelná entita, kde se [cena](documentdb-performance-levels.md) určuje podle úrovně výkonu přidružené ke kolekci. Kolekce mohou pokrývat jeden nebo více oddílů nebo serverů a lze je škálovat tak, aby zvládaly prakticky neomezené objemy úložišť a propustnosti.

Kolekce jsou také entitami fakturace pro DocumentDB. Každá kolekce se fakturuje po hodinách podle zřízené propustnosti a využitého prostoru úložiště. Další informace najdete v tématu [DocumentDB – ceny](https://azure.microsoft.com/pricing/details/documentdb/).  

### Jak nastavím uživatele a oprávnění?
Uživatele a oprávnění můžete vytvořit pomocí jedné ze sad [DocumentDB SDK](documentdb-sdk-dotnet.md) nebo přes rozhraní [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx).   

## Otázky k databázi a vývoji s Microsoft Azure DocumentDB

### Jak můžu začít vyvíjet aplikace s DocumentDB?
Pro .NET, Python, Node.js, JavaScript a Javu jsou k dispozici sady [SDK](documentdb-sdk-dotnet.md).  Vývojáři mohou využít i rozhraní [RESTful HTTP API](https://msdn.microsoft.com/library/azure/dn781481.aspx), pomocí kterých je možné pracovat s prostředky DocumentDB z různých platforem a jazyků. 

Ukázky sad SDK DocumentDB pro [.NET](documentdb-dotnet-samples.md), [Javu](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md) a [Python](documentdb-python-samples.md) jsou k dispozici na GitHubu.

### Podporuje DocumentDB jazyk SQL?
Dotazovací jazyk DocumentDB SQL je vylepšená podmnožina dotazovacích funkcí podporovaných jazykem SQL. Dotazovací jazyk DocumentDB SQL nabízí bohaté hierarchické a relační operátory a rozšiřitelnost prostřednictvím uživatelem definovaných funkcí (UDF) založených na JavaScriptu. Gramatika JSON umožňuje modelování dokumentů JSON ve formě stromů ve formě uzlů stromu, které se používají jak v automatických technikách indexování, tak v dialektu dotazování SQL v DocumentDB.  Podrobnosti o tom, jak používat gramatiku SQL, najdete v článku [Dotazování DocumentDB][query].

### Jaké datové typy DocumentDB podporuje?
Primitivní datové typy, které DocumentDB podporuje, jsou shodné s formátem JSON. JSON obsahuje jednoduchý systém typů, který se skládá z řetězců, čísel (dvojitá přesnost IEEE754) a logických hodnot – true, false a null.  Komplexnější datové typy jako DateTime, Guid, Int64 a Geometry je možné ve formátu JSON i v DocumentDB reprezentovat vytvořením vnořených objektů pomocí operátoru { } a polí pomocí operátoru [ ]. 

### Jak DocumentDB zajišťuje souběžnost?
DocumentDB podporuje optimistické řízení souběžného přístupu (OCC) prostřednictvím značek entit HTTP, neboli ETagů. Každý prostředek DocumentDB má ETag a klienti DocumentDB do svých požadavků na zápis zahrnují naposledy přečtenou verzi. Pokud je ETag aktuální, změna se potvrdí. Pokud je hodnota změněna externě, server zápis odmítne s kódem odpovědi HTTP 412 Precondition failure (Selhání předběžné podmínky). Klienti musí přečíst nejnovější verzi prostředku a opakovat požadavek. 

### Jak se v DocumentDB provádí transakce?
DocumentDB podporuje transakce integrované do jazyka prostřednictvím javascriptových uložených procedur a triggerů. Všechny databázové operace ve skriptech se spouští v izolaci snímku vymezené jako kolekce, pokud jde o kolekci s jedním oddílem, nebo jako dokumenty se stejnou hodnotou klíče oddílu v kolekci, pokud je kolekce rozdělena na oddíly. Na začátku transakce se pořídí snímek verzí dokumentů (ETagy) a k potvrzení dojde pouze v případě, že skript uspěje. Pokud JavaScript vyvolá chybu, transakce se vrátí zpět. Další podrobnosti najdete v tématu o [programování na straně serveru v DocumentDB](documentdb-programming.md).

### Jak lze do DocumentDB vložit dokumenty hromadně? 
Existují tři způsoby hromadného vkládání dokumentů do DocumentDB:

- Nástroj pro migraci dat, jak je popsáno v tématu o [importu dat do DocumentDB](documentdb-import-data.md)
- Průzkumník dokumentů na Portálu Azure, jak je popsáno v tématu o [hromadném přidávání dokumentů pomocí Průzkumníka dokumentů](documentdb-view-json-document-explorer.md#BulkAdd)
- Uložené procedury, jak je popsáno v tématu o [programování na straně serveru v DocumentDB](documentdb-programming.md)

### Podporuje DocumentDB ukládání odkazů na prostředky do mezipaměti?
Ano. Vzhledem k tomu, že DocumentDB je služba RESTful, odkazy na prostředky jsou neměnné a lze je uložit do mezipaměti. Klienti DocumentDB mohou zadat pro čtení hlavičku If-None-Match oproti jakémukoli prostředku, například dokumentu nebo kolekci, a aktualizovat své místní kopie jen tehdy, když dojde ke změně verze na serveru. 




[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
 


<!--HONumber=Jun16_HO2-->


