---
title: "Azure DB Cosmos zásady indexování | Microsoft Docs"
description: "Vysvětluje, jak indexování v Azure Cosmos DB. Zjistěte, jak nakonfigurovat a změnit zásady indexování pro automatické indexování a lepší výkon."
keywords: "jak indexování funguje, automatické indexování, indexování databáze"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: d5e8f338-605d-4dff-8a61-7505d5fc46d7
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/17/2017
ms.author: arramac
ms.openlocfilehash: 30a21645831f0cfcb3b52c797dbddfa6b5283960
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-does-azure-cosmos-db-index-data"></a>Jak funguje Azure Cosmos DB data indexu?

Ve výchozím nastavení je indexovaný všechna data v Azure Cosmos DB. A mnoho zákazníků jsou radostí umožníte Azure DB Cosmos automaticky zpracovávat všechny aspekty indexování, Azure Cosmos DB podporuje také zadat vlastní **indexování zásad** pro kolekce během vytváření. Indexování zásady v Azure Cosmos DB jsou pružnější a výkonnější než nabízené na jiných platformách, databáze, sekundární indexy, protože umožňují návrhu a přizpůsobit tvaru indexu, aniž by došlo ke ztrátě flexibilitu schémat. Informace o tom, jak indexování funguje v Azure Cosmos DB, je potřeba pochopit, můžete provést pomocí správy zásady indexování, jemně odstupňovaných kompromisy mezi režijní náklady na úložiště indexů, zápisu a propustnost dotazu a konzistence dotazu.  

V tomto článku jsme prohlédněte zavřít Azure Cosmos DB indexování zásady, jak můžete přizpůsobit zásady indexování a přidružené kompromisy. 

Po přečtení tohoto článku, budete moct odpovězte si na následující otázky:

* Jak můžete přepsat vlastnosti, které chcete zahrnout nebo vyloučit z indexování?
* Konfigurování index pro případné aktualizace
* Jak nakonfigurovat indexování provádět dotazy Order By ani rozsah?
* Jak provést změny zásady indexování kolekce?
* Jak porovnat úložiště a výkon různé zásady indexování?

## <a id="CustomizingIndexingPolicy"></a>Přizpůsobení zásady indexování kolekce
Vývojářům můžete přizpůsobit kompromis mezi úložiště, výkon zápisu nebo dotazů a konzistence dotazu přepsání výchozí zásady indexování na kolekci Azure Cosmos DB a konfigurací následující aspekty.

* **Včetně/vyloučení dokumenty a cesty do nebo z indexu**. Vývojářům můžete zvolit určitých dokumentů vyloučit nebo do indexu zahrnuty v době vložení nebo nahrazení je ke kolekci. Vývojáři mohou také rozhodnout zahrnout nebo vyloučit některé vlastnosti JSON také znám jako cesty (včetně zástupné znaky) k indexování dokumentů, které jsou zahrnuty v indexu.
* **Konfigurace různé typy indexu**. Zahrnuté cesty, vývojáři můžete také určit typ indexu, které vyžadují v kolekci na základě jejich dat a očekávané zatížení dotazu a číselný či řetězce "přesnost" pro každou z cest.
* **Konfigurace režimů aktualizace indexu**. Azure Cosmos DB podporuje tři indexování režimy, které je možné nakonfigurovat přes zásady indexování v kolekci Azure Cosmos DB: konzistentní, Lazy a None. 

Následující fragment kódu .NET ukazuje, jak nastavit vlastní zásady indexování při vytváření kolekce. Zde jsme nastavit zásady s indexem rozsahu pro řetězce a čísla na maximální přesnost. Tato zásada umožňuje nám spouštět dotazy Order By na řetězce.

    DocumentCollection collection = new DocumentCollection { Id = "myCollection" };

    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), collection);   


> [!NOTE]
> Verze rozhraní REST API verze 2015-06-03 pro podporu rozsahu indexů pro řetězce bylo změněno schéma JSON pro zásady indexování. .NET SDK 1.2.0 a Java, Python a Node.js SDK 1.1.0 podporovat nové schéma zásad. Starší sady SDK pomocí rozhraní REST API verze 2015-04-08 a podporují starší schéma zásady indexování.
> 
> Ve výchozím nastavení Azure Cosmos DB indexuje všechny vlastnosti řetězce v rámci dokumenty konzistentně s indexem Hash a číselné vlastnosti s indexem rozsahu.  
> 
> 

### <a name="customizing-the-indexing-policy-using-the-portal"></a>Přizpůsobení zásady indexování pomocí portálu

Můžete změnit zásady indexování kolekce pomocí portálu Azure. Otevřít vaše kolekce účtu Azure Cosmos DB na portálu Azure, vyberte v levém navigačním panelu klikněte na nabídku **nastavení**a potom klikněte na **zásady indexování**. V **zásady indexování** okně změnit zásady indexování a pak klikněte na **OK** uložte provedené změny. 

### <a id="indexing-modes"></a>Režimy indexování databáze
Azure Cosmos DB podporuje tři indexování režimů, které je možné nakonfigurovat přes zásady indexování v kolekci Azure Cosmos DB – konzistentní, Lazy a None.

**Konzistentní**: Pokud zásady Azure Cosmos DB kolekce je určený jako "konzistentní", dotazy na danou kolekci Azure Cosmos DB, postupujte podle kroků stejnou úroveň konzistence jsou zadány pro čtení bod (tj. silné a ohraničenou odolností, založenou relace nebo závěrečné). Index je aktualizována synchronně jako součást aktualizace dokumentu (tj. vložení, nahraďte, aktualizace a odstranění dokumentu v kolekci Azure Cosmos DB).  Konzistentní indexování podporuje konzistentní dotazy za cenu možné snížení zápisu propustnost. Toto snížení je funkce jedinečné cesty, které je třeba zpracovat a "úroveň konzistence". Konzistentní indexování režimu je určená pro "zápisu rychle dotaz okamžitě" úlohy.

**Opožděné**: Povolit přijímání propustnost maximální dokumentu, můžete nakonfigurovat kolekci Azure Cosmos DB opožděné konzistence; význam dotazy jsou nakonec byl konzistentní. Index se asynchronně aktualizuje v případě kolekci Azure Cosmos DB je tichém tj kapacity propustnosti kolekce není plně využívat k obsluze požadavků uživatele. Pro "ingestování teď dotaz později" procesy vyžadující přijímání nerušený dokumentu může být vhodné "opožděné" indexování režimu.

**Žádný**: kolekce označené jako index režim "Žádný" neobsahuje index s ním spojená. To se často používá, pokud se využívá Azure Cosmos DB jako klíč hodnota úložiště a dokumenty jsou dostupné jenom přes jejich vlastnost ID. 

> [!NOTE]
> Konfigurace zásady indexování s "Žádný" má vedlejším účinkem vyřadit všechny existující index. Použijte, pokud jsou přístupové vzorce vyžadují jenom "id" nebo "vlastního odkazu".
> 
> 

Následující tabulka uvádí konzistence pro dotazy na základě indexování režim (konzistentní a Lazy) nakonfigurovaný pro kolekce a úroveň konzistence zadané pro požadavku dotazu. To platí pro dotazy provedené pomocí žádné rozhraní - REST API sady SDK nebo v rámci uložené procedury a triggery. 

|Konzistence|Indexování režim: konzistentní|Indexování režim: opožděné|
|---|---|---|
|Silné|Silné|Nahodilé|
|Omezená neaktuálnost|Omezená neaktuálnost|Nahodilé|
|Relace|Relace|Nahodilé|
|Nahodilé|Nahodilé|Nahodilé|

Azure Cosmos DB vrátí chybu pro dotazy provedená na kolekce se žádné indexování režimu. Dotazy mohou být provedeny stále jako kontroly prostřednictvím explicitní `x-ms-documentdb-enable-scan` hlavičky v rozhraní REST API nebo `EnableScanInQuery` žádosti o možnost pomocí sady .NET SDK. Některé funkce dotazu jako ORDER nejsou podporovány jako kontroly s `EnableScanInQuery`.

V následující tabulce jsou uvedeny konzistence pro dotazy na základě indexování režimu (konzistentní, Lazy a None) Pokud je zadán parametr EnableScanInQuery.

|Konzistence|Indexování režim: konzistentní|Indexování režim: opožděné|Indexování režim: žádné|
|---|---|---|---|
|Silné|Silné|Nahodilé|Silné|
|Omezená neaktuálnost|Omezená neaktuálnost|Nahodilé|Omezená neaktuálnost|
|Relace|Relace|Nahodilé|Relace|
|Nahodilé|Nahodilé|Nahodilé|Nahodilé|

Následující ukázka kódu ukazují, jak vytvořit kolekci Azure Cosmos DB pomocí sady .NET SDK s konzistentní indexování na všechny vložení dokumentu.

     // Default collection creates a hash index for all string fields and a range index for all numeric    
     // fields. Hash indexes are compact and offer efficient performance for equality queries.

     var collection = new DocumentCollection { Id ="defaultCollection" };

     collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

     collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("mydb"), collection);


### <a name="index-paths"></a>Index cesty
Azure Cosmos DB jako stromy modelů dokumentů JSON a index a umožňuje vám umožní ladit zásad pro cesty v rámci stromu. V rámci dokumenty můžete, které cesty musí být zahrnout nebo vyloučit z indexování. To můžete nabízet zápisu lepší výkon a dolní index úložiště pro scénáře po vzorům dotazů jsou známé předem.

Index cesty začínat kořenu (/) a obvykle končit? operátor zástupných znaků, které označuje, že existuje více možných hodnot pro předponu. Například sloužit vybrat * z řady F kde F.familyName = "Rodinu", musí obsahovat cestu index pro /familyName/? v zásadách indexu kolekce.

Index cesty můžete také použít * operátor zástupný znak určit způsob chování pro rekurzivní cest v rámci předponu. Například/datové části / * umožňuje vyloučit všechno v datové části vlastnost z indexu.

Zde jsou obecné vzory pro zadání cesty index:

| Cesta                | Popis nebo používají                                                                                                                                                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| /                   | Výchozí cesta pro kolekci. Rekurzivní a platí pro celý dokument stromu.                                                                                                                                                                                                                                   |
| / prop /?             | Index cesta zapotřebí pro zpracování dotazů takto (s Hash nebo rozsah typy v uvedeném pořadí.):<br><br>Vyberte z kolekce c WHERE c.prop = "hodnota"<br><br>Vyberte z kolekce c WHERE c.prop > 5<br><br>Vyberte z kolekce c Order c.prop                                                                       |
| / prop / *             | Index cesta u všech cest v rámci zadaného popisku. Funguje s následující dotazy<br><br>Vyberte z kolekce c WHERE c.prop = "hodnota"<br><br>Vyberte z kolekce c WHERE c.prop.subprop > 5<br><br>Vyberte z kolekce c WHERE c.prop.subprop.nextprop = "hodnota"<br><br>Vyberte z kolekce c Order c.prop         |
| / props / [] /?         | Cesta index vyžaduje iterace a připojení dotazy pro pole skalárních hodnot jako ["a", "b", "c"]:<br><br>Vyberte označit z značky ve collection.props kde značky = "hodnota"<br><br>Vyberte značku z kolekce c spojení značky v c.props kde značky > 5                                                                         |
| [] /subprop/ /props/? | Cesta index vyžaduje k obsluze iterace a spojení dotazy na pole objektů, jako jsou [{subprop: "a"}, {subprop: "b"}]:<br><br>Vyberte označit z značky ve collection.props kde tag.subprop = "hodnota"<br><br>Vyberte označit z kolekce c spojení značky v c.props kde tag.subprop = "hodnota"                                  |
| / prop/subprop /?     | Index cesta zapotřebí pro zpracování dotazů (s Hash nebo rozsah typy v uvedeném pořadí.):<br><br>Vyberte z kolekce c WHERE c.prop.subprop = "hodnota"<br><br>Vyberte z kolekce c WHERE c.prop.subprop > 5                                                                                                                    |

> [!NOTE]
> Při nastavování vlastním indexu cesty, je nutné zadat výchozí pravidlo indexování pro celý dokument strom odlišené speciální cestu "/ *". 
> 
> 

Následující příklad konfiguruje konkrétní cestu rozsah indexování a hodnotu vlastní přesnost 20 bajtů:

    var collection = new DocumentCollection { Id = "rangeSinglePathCollection" };    

    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = 20 } } 
            });

    // Default for everything else
    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/*" ,
            Indexes = new Collection<Index> {
                new HashIndex(DataType.String) { Precision = 3 }, 
                new RangeIndex(DataType.Number) { Precision = -1 } 
            }
        });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), pathRange);


### <a name="index-data-types-kinds-and-precisions"></a>Index datové typy, typy a přesnosti
Teď, když bylo podívejte se na tom, jak určit cest, podíváme se na možnosti, které jsme můžete použít ke konfiguraci zásady indexování pro cestu. Můžete určit jeden nebo více indexování definice pro každý cestu:

* Datový typ: **řetězec**, **číslo**, **bodu**, **mnohoúhelníku**, nebo **LineString** (může obsahovat pouze jednu položka pro každý datový typ za cesta)
* Index typu: **Hash** (dotazy na rovnost), **rozsah** (rovnosti, rozsah nebo Order By dotazy), nebo **Spatial** (prostorových dotazů) 
* Přesnost: Pro hash index to se liší od 1 do 8 pro řetězce a čísla s výchozí jako 3. Tato hodnota pro rozsah index můžete mít hodnotu -1 (Maximální přesnost) a liší mezi 1-100 (Maximální přesnost) pro řetězec nebo číselné hodnoty.

#### <a name="index-kind"></a>Typ indexu
Azure Cosmos DB podporuje hodnoty Hash a rozsah typy index pro každou cestu (která můžete nakonfigurovat řetězce, čísla nebo obojí).

* **Hodnota hash** podporuje efektivní rovnosti a dotazy spojení. Pro většinu případy použití indexy hash není nutné vyšší přesností než je výchozí hodnota 3 bajtů. Datový typ může být řetězec nebo číslo.
* **Rozsah** podporuje dotazy na rovnost efektivní, dotazy na rozsah (pomocí >, <>, =, < =,! =) a dotazy Order By. Dotazy Order By ve výchozím nastavení také vyžadují maximální index přesnost (-1). Datový typ může být řetězec nebo číslo.

Druh prostorový index Azure Cosmos DB také podporuje pro každé cestu, kterou lze zadat pro datové typy bodu, mnohoúhelníku nebo LineString. Hodnota v zadané cestě musí být platný fragment GeoJSON jako `{"type": "Point", "coordinates": [0.0, 10.0]}`.

* **Prostorové** podporuje efektivní prostorových (v rámci a vzdálenost) dotazy. Datový typ může být bodu, mnohoúhelníku nebo LineString.

> [!NOTE]
> Azure Cosmos DB podporuje automatické indexování bodů, mnohoúhelníky a LineStrings.
> 
> 

Toto jsou typy podporované index a příklady dotazů, které můžou být použita k obsluze:

| Typ indexu | Popis nebo používají                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hodnota hash       | Hodnoty hash přes/prop /? (nebo /) slouží k efektivní slouží následující dotazy:<br><br>Vyberte z kolekce c WHERE c.prop = "hodnota"<br><br>Hodnota hash přes/props / [] /? (nebo / nebo/props /) slouží k efektivní slouží následující dotazy:<br><br>Vyberte označit z kolekce c spojení značky v c.props kde značky = 5                                                                                                                       |
| rozsah      | Rozsah přes/prop /? (nebo /) slouží k efektivní slouží následující dotazy:<br><br>Vyberte z kolekce c WHERE c.prop = "hodnota"<br><br>Vyberte z kolekce c WHERE c.prop > 5<br><br>Vyberte z kolekce c Order c.prop                                                                                                                                                                                                              |
| Spatial     | Rozsah přes/prop /? (nebo /) slouží k efektivní slouží následující dotazy:<br><br>Vyberte z kolekce c<br><br>KDE ST_DISTANCE (c.prop, {"typ": "Bod", "coordinates": [0.0, 10.0]}) < 40<br><br>Vyberte z kolekce c kde ST_WITHIN(c.prop, {"type": "Polygon",...}) – s v bodech povoleno indexování<br><br>Vyberte z kolekce c kde ST_WITHIN({"type": "Point",...}, c.prop) – s indexování na mnohoúhelníky povoleno              |

Ve výchozím nastavení, je vrácena chyba pro dotazy s rozsahu operátory, jako > =, pokud neexistuje žádné rozsah index (všechny přesnost) Chcete-li signál, že kontroly může být potřeba sloužit dotazu. Dotazy na rozsah můžete provést bez rozsah index pomocí hlavičky x-ms-documentdb-enable kontroly v rozhraní REST API nebo možnost EnableScanInQuery žádosti pomocí sady .NET SDK. Pokud jsou všechny ostatní filtry v dotazu Azure Cosmos DB pomocí indexu můžete filtrovat proti, pak bude vrácena žádná chyba.

Použít stejná pravidla pro prostorových dotazů. Ve výchozím nastavení je vrácena chyba prostorových dotazů, pokud neexistuje žádné prostorový index, a neexistují žádné filtry, které se dají obsluhovat z indexu. Mohou být prováděny tak kontroly s využitím x-ms-documentdb-enable kontroly/EnableScanInQuery.

#### <a name="index-precision"></a>Index přesnost
Index přesnost umožňuje kompromis mezi režie index úložiště a výkon dotazů. Pro čísla doporučujeme používat tuto výchozí konfiguraci přesnost-1 ("maximální"). Vzhledem k tomu, že jsou čísla 8 bajtů ve formátu JSON, jde o ekvivalent konfigurace 8 bajtů. Výběr na nižší hodnotu pro přesnost, jako je například 1-7, znamená to, že hodnoty v rámci některé rozsahy namapovány na stejný položka indexu. Proto se zmenší prostor úložiště index, ale při provádění dotazu může být potřeba zpracovat další dokumenty a tedy z tohoto důvodu využívat další propustnost požadované jednotky.

Přesnost konfigurace indexu má více praktické aplikace s rozsahy řetězec. Vzhledem k tomu, že řetězce může být jakékoli libovolné délky, volba přesnost index může mít dopad na výkon dotazů rozsah řetězec a mít vliv na velikost požadovaného místa index úložiště. Řetězec indexů rozsah můžete nakonfigurovat 1-100 nebo -1 ("maximální"). Pokud chcete provádět dotazy Order By pro vlastnosti string, je nutné zadat přesností-1 pro odpovídající cesty.

Prostorové indexy vždy použít výchozí index přesnost pro všechny typy (body, LineStrings a mnohoúhelníky) a nelze přepsat. 

Následující příklad ukazuje, jak zvýšit přesnost pro rozsah indexy v kolekci pomocí sady .NET SDK. 

**Vytvořte kolekci s přesností vlastním indexu**

    var rangeDefault = new DocumentCollection { Id = "rangeCollection" };

    // Override the default policy for Strings to range indexing and "max" (-1) precision
    rangeDefault.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), rangeDefault);   


> [!NOTE]
> Azure Cosmos DB vrátí chybu, pokud dotaz používá Order By, ale nemá indexem rozsahu proti dotazované cestu s maximální přesnost. 
> 
> 

Podobně cesty můžete úplně vyloučit z indexování. Další příklad ukazuje, jak vyloučit celý oddíl dokumentů (také známa jako dílčí stromu) indexování pomocí "*" zástupný znak.

    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*" });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);



## <a name="opting-in-and-opting-out-of-indexing"></a>Vyjádření výslovného souhlasu a zrušení indexování
Můžete zvolit, jestli chcete kolekci automaticky indexuje všechny dokumenty. Ve výchozím nastavení jsou automaticky indexovány všechny dokumenty, ale můžete ho vypnout. Když je vypnutý indexování, dokumenty jsou přístupné pouze prostřednictvím jejich odkazů na sebe sama nebo dotazy pomocí ID.

S automatické indexování, vypnutý, můžete stále selektivně přidat pouze konkrétní dokumentů do indexu. Naopak můžete nechat automatické indexování na a selektivně zvolte vyloučit pouze konkrétní dokumenty. Indexování zapnout nebo vypnout konfigurace jsou užitečné v případě, že máte jenom podmnožinu dokumentů, které je třeba zadat dotaz.

Například následující příklad ukazuje, jak zahrnout dokumentu explicitně pomocí [DocumentDB rozhraní API .NET SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/documentdb-sdk-dotnet) a [RequestOptions.IndexingDirective](http://msdn.microsoft.com/library/microsoft.azure.documents.client.requestoptions.indexingdirective.aspx) vlastnost.

    // If you want to override the default collection behavior to either
    // exclude (or include) a Document from indexing,
    // use the RequestOptions.IndexingDirective property.
    client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new { id = "AndersenFamily", isRegistered = true },
        new RequestOptions { IndexingDirective = IndexingDirective.Include });

## <a name="modifying-the-indexing-policy-of-a-collection"></a>Úprava zásady indexování kolekce
Azure Cosmos DB umožňuje provést změny zásady indexování kolekce za chodu. Změnu indexování zásady na kolekci Azure Cosmos DB může vést k změně ve tvaru index včetně, které lze indexovat cesty, jejich přesnost a také modelu konzistence indexu sám sebe. Změnu zásady indexování, proto vyžaduje efektivně transformaci staré indexu na novou. 

**Transformace indexu online**

![Indexování fungování – Azure Cosmos DB indexu online transformace](./media/indexing-policies/index-transformations.png)

Transformace indexu jsou vytvářeny online, což znamená, že dokumenty indexované podle staré zásad jsou transformovány efektivně na nové zásady **bez ovlivnění dostupnosti zápisu nebo zřízené propustnosti** kolekce. Konzistence operacích čtení a zápisu provedené pomocí rozhraní REST API sady SDK nebo prostřednictvím uložených procedur a aktivačních událostí není ovlivněná během index transformace. To znamená, že je žádné snížení výkonu nebo výpadek pro vaše aplikace při provádění zásady indexování změnit.

Ale po dobu, která index transformaci je průběh, jsou dotazy nakonec byl konzistentní bez ohledu na to indexování konfiguraci režimu (konzistentní nebo Lazy). To také platí pro dotazy od všech rozhraní – rozhraní REST API sady SDK a v rámci uložené procedury a triggery. Stejně jako s Lazy indexování, index je provedena transformace asynchronně na pozadí na replikách pomocí k výměně za chodu prostředky, které jsou k dispozici pro danou repliku. 

Transformace indexu jsou také vytvářeny **na místě** (na místě), tedy Azure DB Cosmos není udržovat dvě kopie indexu a Prohodit původní index se s novým. To znamená, že žádné další místo na disku je požadováno, nebo využívat v kolekcích při provádění transformací index.

Při změně zásady indexování používání změny přesunout z původní index k novému jeden závisí primárně na indexování konfigurace režimu, než ostatní hodnoty jako zahrnout nebo vyloučit cesty, typy index a přesnosti. Pokud vaše staré a nové zásady použít konzistentní indexování, Azure Cosmos DB provede transformaci indexu online. Jiné indexování změny zásad v režimu konzistentní indexování nelze použít, když probíhá transformace.

Ale můžete přesunout na Lazy nebo hodnotu None probíhá indexování režimu při transformaci. 

* Při přesunutí Lazy index zásad změny efektivní okamžitě a Azure Cosmos DB spustí znovu vytvořit index asynchronně. 
* Když přesouváte na hodnotu None, pak index vyřazen efektivní okamžitě. Přesun na hodnotu None je užitečné, když chcete zrušit v průběhu transformaci a spusťte novou s jiný zásady indexování. 

Pokud používáte sady SDK rozhraní .NET, můžete ji indexování změn zásad pomocí nového **ReplaceDocumentCollectionAsync** metoda a sledovat průběh procento index transformace pomocí **IndexTransformationProgress** vlastnost odpověď z **ReadDocumentCollectionAsync** volání. Jiné sady SDK a rozhraní REST API podporují ekvivalentní vlastnosti a metody pro provádění změn v zásadách indexování.

Zde je fragment kódu, který ukazuje, jak upravit indexování zásady kolekce z konzistentní indexování režimu Lazy.

**Upravit zásady indexování z konzistentní k opožděné**

    // Switch to lazy indexing.
    Console.WriteLine("Changing from Default to Lazy IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.Lazy;

    await client.ReplaceDocumentCollectionAsync(collection);


Voláním ReadDocumentCollectionAsync, například můžete zkontrolovat průběh transformaci index, jak je uvedeno níže.

**Sledovat průběh Index transformace**

    long smallWaitTimeMilliseconds = 1000;
    long progress = 0;

    while (progress < 100)
    {
        ResourceResponse<DocumentCollection> collectionReadResponse = await client.ReadDocumentCollectionAsync(
            UriFactory.CreateDocumentCollectionUri("db", "coll"));

        progress = collectionReadResponse.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromMilliseconds(smallWaitTimeMilliseconds));
    }

Přesunutím na hodnotu None indexování režim můžete vyřadit index pro kolekci. Pokud chcete zrušit transformaci v průběhu a okamžitě spustit nový, může to být užitečné provozní nástroj.

**Vyřadit index pro kolekci**

    // Switch to lazy indexing.
    Console.WriteLine("Dropping index by changing to to the None IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.None;

    await client.ReplaceDocumentCollectionAsync(collection);

Pokud by provedete indexování změny zásad Azure Cosmos DB kolekce? Tady jsou nejběžnější případy použití:

* Poskytovat konzistentní výsledky během normálního provozu, ale patří do Opožděné indexování během hromadné importy dat
* Spuštění pomocí nového indexování funkcí v aktuální databázi Cosmos Azure kolekcí, například jako geoprostorové dotazování vyžadují typ prostorového indexu nebo Order By / řetězec rozsah dotazy, které vyžadují řetězec index typu rozsah
* Ruční vyberte vlastnosti a indexovaných časem změnit
* Vyladění indexování přesnost a zlepšit výkon dotazu nebo snižte spotřebovaného úložiště

> [!NOTE]
> Chcete-li upravit zásady indexování pomocí ReplaceDocumentCollectionAsync, musíte verze > = 1.3.0 .NET SDK
> 
> Pro transformaci index úspěšně dokončit Ujistěte se, zda je dostatek volného místa k dispozici na kolekci. Pokud kolekce dosáhne kvóta úložiště, bude pozastavena transformace index. Index transformace se automaticky obnoví, jakmile prostor úložiště je k dispozici, například pokud odstraníte některé dokumenty.
> 
> 

## <a name="performance-tuning"></a>Ladění výkonu
Rozhraní API DocumentDB poskytují informace o metrik výkonu, jako je například úložiště indexu používá a náklady na propustnost (jednotek žádosti) pro všechny operace. Tyto informace slouží k porovnání různé zásady indexování a optimalizace výkonu.

Ke kontrole kvótu úložiště a využití kolekce, spustit žádost HEAD nebo GET pro kolekci prostředků a zkontrolovat x-ms požadavku kvóty a hlavičky x-ms požadavku využití. V sadě SDK .NET [DocumentSizeQuota](http://msdn.microsoft.com/library/dn850325.aspx) a [DocumentSizeUsage](http://msdn.microsoft.com/library/azure/dn850324.aspx) vlastnosti v [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) obsahovat tyto odpovídající hodnoty.

     // Measure the document size usage (which includes the index size) against   
     // different policies.
     ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));  
     Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);


K měření režii indexování na každou operaci zápisu (vytvoření, aktualizace nebo odstranění), zkontrolujte hlavičky x-ms požadavek – zdarma (nebo ekvivalentní [RequestCharge](http://msdn.microsoft.com/library/dn799099.aspx) vlastnost v [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) sady .NET SDK) k měření počtu jednotek žádosti uplatníte tyto operace.

     // Measure the performance (request units) of writes.     
     ResourceResponse<Document> response = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), myDocument);              
     Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);

     // Measure the performance (request units) of queries.    
     IDocumentQuery<dynamic> queryable =  client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"), queryString).AsDocumentQuery();

     double totalRequestCharge = 0;
     while (queryable.HasMoreResults)
     {
        FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>(); 
        Console.WriteLine("Query batch consumed {0} request units",queryResponse.RequestCharge);
        totalRequestCharge += queryResponse.RequestCharge;
     }

     Console.WriteLine("Query consumed {0} request units in total", totalRequestCharge);

## <a name="changes-to-the-indexing-policy-specification"></a>Změny specifikace zásady indexování
Změnu schématu pro zásady indexování byla zavedena v 7. července 2015 se rozhraní REST API verze 2015-06-03. Odpovídající třídy ve verzích sady SDK mají nové implementace tak, aby odpovídala schématu. 

Následující změny byly implementovány ve specifikaci JSON:

* Indexování zásad podporuje rozsah indexů pro řetězce
* Každá cesta může mít několik definic index, jednu pro každý typ dat
* Indexování přesnost podporuje 1 – 8 pro čísla 1 až 100 pro řetězce a -1 (Maximální přesnost)
* Segmenty cesty nevyžadují dvojité uvozovky, abyste se vyhnuli každá cesta zahrnovat. Například můžete přidat cestu pro/název /? místo / "title" /?
* Kořenová cesta představující "všechny cesty" může být reprezentován jako / * (kromě nebo)

Pokud máte kód této kolekce zřizuje s vlastní zásady indexování napsané pomocí .NET SDK nebo starší verze 1.1.0, musíte změnit váš kód pro zpracování tyto změny, aby bylo možné přesunout do SDK verze 1.2.0. Pokud není máte kód, který nakonfiguruje zásady indexování nebo plánujete dál používat starší verze sady SDK, je potřeba žádné změny.

Praktické porovnání tady je jeden příklad vlastní zásady indexování vytvořené pomocí rozhraní REST API verze 2015-06-03, jakož i předchozí verze 2015-04-08.

**Předchozí zásady indexování JSON**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "IncludedPaths":[
          {
             "IndexType":"Hash",
             "Path":"/",
             "NumericPrecision":7,
             "StringPrecision":3
          }
       ],
       "ExcludedPaths":[
          "/\"nonIndexedContent\"/*"
       ]
    }

**Aktuální zásady indexování JSON**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Hash",
                   "dataType":"String",
                   "precision":3
                },
                {
                   "kind":"Hash",
                   "dataType":"Number",
                   "precision":7
                }
             ]
          }
       ],
       "ExcludedPaths":[
          {
             "path":"/nonIndexedContent/*"
          }
       ]
    }

## <a name="next-steps"></a>Další kroky
V odkazech dole pro index ukázky správy zásad a další informace o databázi Cosmos Azure dotazovací jazyk.

1. [Ukázky kódu Správa indexu .NET DocumentDB rozhraní API](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/IndexManagement/Program.cs)
2. [Operace kolekci DocumentDB rozhraní API REST](https://msdn.microsoft.com/library/azure/dn782195.aspx)
3. [Dotaz s SQL](documentdb-sql-query.md)

