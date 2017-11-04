---
title: "Příručka o vytváření datové služby pro Marketplace | Microsoft Docs"
description: "Podrobné pokyny o tom, jak vytvořit, certifikovat a datové služby pro nasazení zakoupit na webu Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 107baab2-5828-4158-abdf-59a580c80d37
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 8ff76ea21ba684ae2a2afcb74d66b4912d7be053
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="understanding-the-nodes-schema-for-mapping-an-existing-web-service-to-odata-through-csdl"></a>Principy schématu uzly pro mapování existující webovou službu OData prostřednictvím CSDL
> [!IMPORTANT]
> **V tuto chvíli jsme již nejsou registrace všechny nové služby Data vydavatele. Nové dataservices nebude získat schválení pro výpis.** Pokud máte SaaS obchodní aplikace, který chcete publikovat na AppSource můžete najít další informace [zde](https://appsource.microsoft.com/partners). Pokud máte IaaS aplikace nebo služby vývojáře, které chcete publikovat na webu Azure Marketplace můžete najít další informace [zde](https://azure.microsoft.com/marketplace/programs/certified/).
>
>

Tento dokument vám pomůže vysvětlení uzel struktury pro mapování na CSDL protokol OData. Je důležité si uvědomit, že je dobře strukturu uzlu formátu XML. Kořenové, nadřazených a podřízených schématu tak lze použít při navrhování vaší mapování OData.

## <a name="ignored-elements"></a>Ignoruje elementy
Níže jsou nejvyšší úrovni CSDL prvky (z uzlů XML), které se bude používat back-end Azure Marketplace během importu metadat webovou službu. Mohou existovat, ale budou ignorovány.

| Element | Rozsah |
| --- | --- |
| Pomocí elementu. |Uzel, podřízené uzly a všech atributů |
| Documentation Element |Uzel, podřízené uzly a všech atributů |
| Typ ComplexType |Uzel, podřízené uzly a všech atributů |
| Element přidružení |Uzel, podřízené uzly a všech atributů |
| Rozšířené vlastnosti |Uzel, podřízené uzly a všech atributů |
| EntityContainer |Následující atributy se ignorují: *rozšiřuje* a *AssociationSet* |
| Schéma |Následující atributy se ignorují: *Namespace* |
| Element FunctionImport |Následující atributy se ignorují: *režimu* (ln se předpokládá výchozí hodnota) |
| Typ entity |Následující uzly dílčí ignorují: *klíč* a *PropertyRef* |

Následující část popisuje změny (Přidat a ignoruje elementy) na různých uzlech CSDL XML podrobně.

## <a name="functionimport-node"></a>Element FunctionImport uzlu
Element FunctionImport uzel představuje jeden URL (vstupní bod), která zpřístupňuje službu pro koncového uživatele. Uzel umožňuje popisující, jak je řešit adresu URL, které jsou dostupné pro koncového uživatele a jak tyto parametry jsou k dispozici.

Podrobnosti o tomto uzlu se nacházejí v [sem][MSDNFunctionImportLink](https://msdn.microsoft.com/library/cc716710.aspx)

Následují další atributy (nebo přidání atributů), jsou zveřejněné podle uzlu FunctionImport:

**d:BaseUri** -šablony identifikátoru URI pro prostředek REST, který je zveřejněný na webu Marketplace. Marketplace použije šablona k vytvoření dotazy pro webovou službu REST. Šablony URI obsahuje zástupné symboly pro parametry ve formátu {parameterName}, kde název parametru je název parametru. Příklad: apiVersion = {apiVersion}.
Než se objeví jako parametry URI nebo jako součást cesta k identifikátoru URI jsou povolené parametry. V případě vzhled v cestě jsou vždy povinné (nemohou být označeny jako s možnou hodnotou Null). *Příklad:*`d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**Název** -název importované funkce.  Nemůže být stejná jako jiné názvy definované v CSDL.  Příklad: Název = "GetModelUsageFile"

**Objekt EntitySet** *(volitelné)* – Pokud je funkce vrátí kolekci typů entit, hodnota **EntitySet** musí být sada entit, ke které patří do kolekce. Jinak **EntitySet** nesmí se používat atribut. *Příklad:*`EntitySet="GetUsageStatisticsEntitySet"`

**Vlastnost ReturnType** *(volitelné)* – Určuje typ elementů vrácený identifikátor URI.  Tento atribut nepoužívejte, pokud funkce nevrací hodnotu. Tady jsou podporované typy:

* **Kolekce (<Entity type name>)**: Určuje kolekci typů entit definované. Název je v uzlu EntityType atribut názvu. Příkladem je kolekce (WXC. HourlyResult).
* **Nezpracovaná (<mime type>)**: Určuje, nezpracované dokumentu nebo objekt blob, který je vrácen uživateli. Příklad je Raw(image/jpeg) Další příklady:

  * ReturnType="Raw(text/plain)"
  * Vlastnost ReturnType = "kolekce (sage. DeleteAllUsageFilesEntity) "*

**d:paging** -Určuje, jak je stránkování zpracovávat prostředek REST. Hodnoty parametru se používají v rámci složené odvětví, například stránky = {$page} & vlastnost itemsperpage = {$size} jsou dostupné možnosti:

* **Žádné:** je k dispozici žádné stránkování
* **Přeskočit:** stránkování je vyjádřit pomocí logických "Přeskočit" a "vzít" (nahoře). Přeskočit skáče přes M elementy a proveďte vrátí další prvky N. Hodnota parametru: $skip
* **Proveďte:** proveďte vrátí další elementy N. Hodnota parametru: $take
* **PageSize:** stránkování je vyjádřit pomocí logické stránky a velikost (počet položek na stránku). Stránka představuje aktuální stránku, která je vrácena. Hodnota parametru: $page
* **Velikost:** velikost představuje počet vrácených pro jednotlivé stránky položek. Hodnota parametru: $size

**d:AllowedHttpMethods** *(volitelné)* -Určuje, které operace se zpracovává souborem prostředek REST. Také omezuje přijatý operaci se zadanou hodnotou.  Výchozí = POST.  *Příklad:* `d:AllowedHttpMethods="GET"` jsou dostupné možnosti:

* **GET:** obvykle používá k vrácení dat
* **POST:** obvykle používají k vkládání nových dat
* **PUT:** obvykle používá k aktualizaci dat
* **ODSTRANĚNÍ:** používá k odstranění dat

Další podřízené uzly (nevztahuje dokumentace CSDL) v rámci uzlu FunctionImport jsou:

**d:RequestBody** *(volitelné)* -textu požadavku se používá k označení, že žádost očekává, že text k odeslání. Parametry lze zadat v textu požadavku. Jsou vyjádřeny v rámci složené závorky, například {parameterName}. Tyto parametry nebyly mapovány ze vstupu uživatele do textu, které se přenese do služby poskytovatele obsahu. RequestBody element má atribut s názvem httpMethod. Atribut umožňuje dvou hodnot:

* **POST:** použita, pokud je požadavek HTTP POST
* **GET:** použita, pokud je požadavek HTTP GET

    Příklad:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**d:namespaces** a **d:Namespace** -tento uzel popisuje obory názvů, které jsou definovány v souboru XML, který je vrácené importem funkce (URI koncového bodu). XML, který je vrácen službě back-end může obsahovat libovolný počet oborů názvů k odlišení obsah, který je vrácen. **Všechny tyto obory názvů, pokud se používá v d:Map nebo d:Match dotazů XPath musí být uvedeny.** Uzel d:Namespaces obsahuje sadu nebo seznam d:Namespace uzlů. Každý z nich uvádí jeden obor názvů použít v odpovědi služby back-end. Tady jsou atribut d:Namespace uzlu:

* **d:prefix:** předponu pro obor názvů, jak je vidět v XML výsledky vrácené službou, např. f:FirstName, f:LastName, kde f je předpona.
* **d:URI:** úplný identifikátor URI oboru názvů v dokumentu výsledek. Reprezentuje hodnotu, která předpona je přeložit na za běhu.

**d:ErrorHandling** *(volitelné)* -tento uzel obsahuje podmínky pro zpracování chyb. Každý stav, bude ověřeno výsledek vrácený službou poskytovatele obsahu. Pokud podmínku odpovídá navrhované kód chyby protokolu HTTP je pro koncového uživatele vrátí chybovou zprávu.

**d:ErrorHandling** *(volitelné)* a **d:Condition** *(volitelné)* -uzel podmínek obsahuje jednu podmínku, která je vrácena se změnami výsledek vrácený pomocí obsahu poskytovatele služeb. Následují **požadované** atributy:

* **d:match:** výraz XPath, který ověřuje, zda je přítomen v poskytovateli obsahu daného uzlu nebo hodnota výstupu XML. Výraz XPath je spustit proti výstup a by měla vrátit hodnotu true, pokud je podmínka vyhodnocena shoda nebo hodnotu NEPRAVDA, jinak hodnota.
* **d:HttpStatusCode:** odpovídá stavový kód protokolu HTTP, který má být vrácen Marketplace v případě, že podmínka. Marketplace signalizes chyby pro uživatele prostřednictvím stavové kódy HTTP. Seznam stavové kódy HTTP jsou k dispozici na http://en.wikipedia.org/wiki/HTTP_status_code
* **d:errorMessage:** chybová zpráva, která je vrácena – s kódem stavu HTTP – pro koncového uživatele. To by měl být popisný chybovou zprávu, která neobsahuje žádné tajných klíčů.

**d:Title** *(volitelné)* -umožňuje popisující název funkce. Hodnota pro název pochází z

* Volitelné mapy atribut (xpath), která určuje, kde najít název v odpovědi vrácené z žádost o služby.
* - Nebo - title zadaný jako hodnota uzlu.

**d:Rights** *(volitelné)* -(například autorským) práv spojených s funkcí. Hodnota pro práva pochází od:

* Volitelné mapy atribut (xpath), která určuje, kde najít práva v odpovědi vrácené z žádost o služby.
* - Nebo - práva zadaný jako hodnota uzlu.

**d:Description** *(volitelné)* – krátkodobých popis funkce. Hodnota pro popis pochází z

* Volitelné mapy atribut (xpath), která určuje, kde najít popis v odpovědi vrácené z žádost o služby.
* - Nebo – popis zadaný jako hodnota uzlu.

**d:EmitSelfLink** - *. viz výše příklad "FunctionImport 'stránkování, prostřednictvím vrácená data"*

**d:EncodeParameterValue** -volitelné rozšíření OData

**d:QueryResourceCost** -volitelné rozšíření OData

**d:map** -volitelné rozšíření OData

**d:Headers** -volitelné rozšíření OData

**d:Headers** -volitelné rozšíření OData

**d:value** -volitelné rozšíření OData

**d:HttpStatusCode** -volitelné rozšíření OData

**d:errorMessage** -volitelné rozšíření OData

## <a name="parameter-node"></a>Uzel parametru
Tento uzel představuje jeden parametr, který je zveřejněný v rámci šablony URI / text, který byl zadaný v elementu FunctionImport uzlu žádosti.

Na stránce dokumentu velmi užitečné informace o uzlu "Prvek parametru" se nachází zde [sem](http://msdn.microsoft.com/library/ee473431.aspx) (použití **další verze** rozevírací seznam a vyberte jinou verzi, v případě potřeby můžete zobrazit dokumentaci). *Příklad:*`<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| Atribut parametru | Je vyžadován | Hodnota |
| --- | --- | --- |
| Name (Název) |Ano |Název parametru. Velká a malá písmena!  Malá a velká písmena BaseUri. **Příklad:**`<Property Name="IsDormant" Type="Byte" />` |
| Typ |Ano |Typ parametru. Hodnota musí být **EDMSimpleType** nebo komplexní typ, který je v rámci oboru modelu. Další informace najdete v části "6 typy podporované parametr nebo vlastnost".  (Rozlišuje velká a malá písmena! První znak je velké písmeno, rest jsou malá písmena.)  Také najdete [konceptuálního modelu typy (CSDL)][MSDNParameterLink](http://msdn.microsoft.com/library/bb399548.aspx). **Příklad:**`<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Režim |Ne |**V**, Out nebo InOut, v závislosti na tom, zda je parametr vstupní, výstupní nebo vstupní a výstupní parametr. ("V" je k dispozici jen v Azure Marketplace.) **Příklad:**`<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| Hodnota maxLength |Ne |Maximální povolená délka parametru. **Příklad:**`<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| přesnost |Ne |Přesnost parametr. **Příklad:**`<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| Měřítko |Ne |Měřítko parametr. **Příklad:**`<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

Tady jsou atributy, které jsou přidané do specifikace CSDL:

| Atribut parametru | Popis |
| --- | --- |
| **d:Regex** *(volitelné)* |Příkaz regulární výraz používaný k ověření vstupní hodnota pro parametr. Pokud vstupní hodnota se neshoduje se příkaz hodnota byl odmítnut. To umožňuje zadejte také možné hodnoty, například ^ [0-9] +? $ a Povolit jenom číslice. **Příklad:** ' < název parametru = "name" režimu = "V" typ = "Řetězec" d: null = "false" d:Regex = "^ [a-zA-Z] * $" d:Description = "A název, který nemůže obsahovat žádné mezery nebo jiné než alfanumerické znaky jiné než anglické" d:SampleValues = "Jirka |
| **d:enum** *(volitelné)* |Svislicí oddělený seznam hodnot pro parametr platný. Typ hodnoty musí odpovídat definovaného typu parametru. Příklad:, angličtina |
| **d: Null** *(volitelné)* |Umožňuje definovat, zda parametr může mít hodnotu null. Výchozí hodnota je: true. Parametry, které jsou zveřejněné jako část této cesty v šabloně URI však nemůže mít hodnotu null. Pokud je atribut nastaven na hodnotu false pro tyto parametry – vstup uživatele přepsána. **Příklad:**`<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(volitelné)* |Ukázková hodnota zobrazuje jako poznámka pro klienta v uživatelském rozhraní.  Je možné přidat několik hodnot pomocí seznamu kanálu oddělené, tj. |

## <a name="entitytype-node"></a>Typ entity uzlu
Tento uzel představuje jeden z typů, které jsou vráceny z Marketplace pro koncového uživatele. Také obsahuje mapování z výstupu, který je vrácen službou poskytovatele obsahu do hodnoty, které jsou vráceny pro koncového uživatele.

Podrobnosti o tomto uzlu se nacházejí v [sem](http://msdn.microsoft.com/library/bb399206.aspx) (použití **další verze** rozevírací seznam a vyberte jinou verzi, v případě potřeby můžete zobrazit dokumentaci.)

| Název atributu | Je vyžadován | Hodnota |
| --- | --- | --- |
| Name (Název) |Ano |Název tohoto typu entity. **Příklad:**`<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| Základní typ BaseType |Ne |Název jiného typu entity, která je základní typ typu entity, který je definovaný. **Příklad:**`<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

Tady jsou atributy, které jsou přidané do specifikace CSDL:

**d:map** -výraz XPath spustit pro výstup služby. Tady se předpokládá je, že výstup služby obsahuje sadu elementů, které se opakují, jako jsou ATOM kanálu, kde je sada uzlů položky, které se opakují. Každá z těchto opakující se uzly obsahuje jeden záznam. Výraz XPath je potom zadat tak, aby odkazoval na uzlu jednotlivých opakující se ve výsledku služby poskytovatele obsahu, který obsahuje hodnoty pro jednotlivé záznamy. Příklad výstupu ze služby:

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

Výraz XPath by být /foo/panel, protože každý panelu uzlu je opakovaný uzel ve výstupu a obsahuje skutečný obsah, který je vrácen pro koncového uživatele.

**Klíč** -tento atribut je ignorován v Marketplace. REST na základě webové služby, obecně Nevystavujte primární klíč.

## <a name="property-node"></a>Vlastnosti uzlu
Tento uzel obsahuje jednu vlastnost záznamu.

Podrobnosti o tomto uzlu se nacházejí v [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (pomocí **další verze** rozevírací seznam a vyberte jinou verzi, v případě potřeby můžete zobrazit dokumentaci.) *Příklad:*`<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"     Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| AttributeName | Požaduje se | Hodnota |
| --- | --- | --- |
| Name (Název) |Ano |Název vlastnosti |
| Typ |Ano |Typ hodnoty vlastnosti. Typ hodnoty vlastnosti musí být **EDMSimpleType** nebo komplexní typ (označená plně kvalifikovaný název), který je v rozsahu modelu. Další informace najdete v tématu typy konceptuální Model (CSDL). |
| S možnou hodnotou Null |Ne |**Hodnota TRUE,** (výchozí hodnota) nebo **False** v závislosti na tom, jestli vlastnost může mít hodnotu null. Poznámka: V této verzi CSDL indikován [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) obor názvů, vlastnost komplexního typu musí mít Nullable = "False". |
| Výchozí hodnota |Ne |Výchozí hodnota vlastnosti. |
| Hodnota maxLength |Ne |Maximální délka hodnoty vlastnosti. |
| Řetězci FixedLength |Ne |**Hodnota TRUE,** nebo **False** v závislosti na tom, jestli se uloží hodnotu vlastnosti jako řetězec délky fiexed. |
| přesnost |Ne |Odkazuje na maximální počet číslic, které se uchovají v číselnou hodnotu. |
| Měřítko |Ne |Maximální počet desetinných míst, které se uchovají v číselná hodnota. |
| Kódování Unicode |Ne |**Hodnota TRUE,** nebo **False** v závislosti na tom, jestli hodnota vlastnosti se ukládají jako řetězec znaků Unicode. |
| Kolace |Ne |Řetězec, který určuje pořadí řazení, který se má použít v datovém zdroji. |
| Režim ConcurrencyMode |Ne |**Žádný** (výchozí hodnota) nebo **pevné**. Pokud je hodnota nastavená **pevné**, bude použita hodnota vlastnosti v optimistickou metodu souběžného kontroly. |

Zde jsou další atributy, které byly přidány do specifikace rozhraní CSDL:

**d:map** -výraz XPath spustit pro službu výstup a extrahuje jednu vlastnost výstupu. Zadaný výraz XPath je relativní vzhledem ke opakovaných uzlu, který byl zvolen v uzlu EntityType XPath. Také je možné zadat absolutní XPath umožňující včetně statické prostředků v jednotlivých uzlů výstupu, jako je například autorským příkaz, který je jednou nacházejí pouze ve službě původní výstup, ale měl by být ve všech řádků ve výstupu OData. Příklad ze služby:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

Výraz XPath tady by ./bar/baz0 získat uzlu baz0 ze služby poskytovatele obsahu.

**d:CharMaxLength** – pro typ řetězce, můžete zadat maximální délka. Viz příklad DataService CSDL

**d:IsPrimaryKey** -Určuje, zda sloupec primárního klíče v tabulce nebo zobrazení. Podívejte se na příklad DataService CSDL.

**d:isExposed** -Určuje, pokud je vystaven schématu tabulky (obvykle true). Viz příklad DataService CSDL

**d:IsView** *(volitelné)* – hodnota true, pokud to je založené na zobrazení a nikoli na tabulku.  Viz příklad DataService CSDL

**d:Tableschema** – viz příklad DataService CSDL

**d:ColumnName** -je název sloupce v tabulce nebo zobrazení.  Viz příklad DataService CSDL

**d:IsReturned** -je logická hodnota, která určuje, pokud služba zpřístupní této hodnoty na klientovi.  Viz příklad DataService CSDL

**d:IsQueryable** -je logická hodnota, která určuje, pokud sloupec lze použít v dotazu databáze.   Viz příklad DataService CSDL

**d:OrdinalPosition** -je sloupce číselné pozice výskytu, x, v tabulce nebo zobrazení, kde x je od 1 do počet sloupců v tabulce.  Viz příklad DataService CSDL

**d:DatabaseDataType** -je datový typ sloupce v databázi, tj. datový typ SQL. Viz příklad DataService CSDL

## <a name="supported-parametersproperty-types"></a>Typy podporované parametry nebo vlastnost
Dále jsou podporované typy vlastností a parametry. (Malá a velká písmena)

| Primitivní typy | Popis |
| --- | --- |
| Hodnotu Null |Představuje chybějící hodnoty |
| Logická hodnota |Představuje matematickém konceptu s hodnotou binární logiky |
| Bajtů |Celé číslo bez znaménka 8bitové hodnoty |
| Data a času |Představuje data a času s hodnotami v rozsahu od půlnoci 12:00:00, 1, 1753. ledna prostřednictvím 11:59:59 P.M, prosince 9999 A.D. |
| Decimal |Představuje číselných hodnot s pevnou přesnost a měřítko. Tento typ, ale popíše číselnou hodnotu v rozsahu od 10 záporné ^ 255 + 1 pro kladné 10 ^ 255 -1 |
| Double |Představuje bod plovoucí desetinné číslo s přesností na 15 číslic, která představuje hodnoty s přibližnou rozsah rozmezí 2, 23E-308 odchylkou až 1, 79E +308. **Použití Decimal z důvodu problému Exel exportu** |
| Jeden |Představuje bod plovoucí desetinné číslo s přesností 7 míst, která představuje hodnoty s přibližnou rozsah rozmezí 1, 18E-38 odchylkou až 3, 40E +38 |
| Identifikátor GUID |Reprezentuje hodnotu jedinečný identifikátor 16 bajtů (128 bitů) |
| Int16 |Reprezentuje hodnotu podepsaný 16bitové celé číslo |
| Int32 |Reprezentuje hodnotu podepsaný 32bitové celé číslo |
| Int64 |Reprezentuje hodnotu podepsaný 64bitové celé číslo |
| Řetězec |Představuje pevné nebo proměnná délky textová data |

## <a name="see-also"></a>Viz také
* Pokud vás zajímá porozumět celkový proces mapování OData a účel, přečtěte si tento článek [mapování dat služby OData](marketplace-publishing-data-service-creation-odata-mapping.md) ke kontrole definice struktury a pokynů.
* Pokud vás zajímá kontrola příklady, přečtěte si tento článek [Data služby OData mapování příklady](marketplace-publishing-data-service-creation-odata-mapping-examples.md) najdete ukázkový kód a pochopit syntaxe kódu a kontext.
* Pokud chcete vrátit do předepsaných cestu pro publikování datové služby v Azure Marketplace, přečtěte si tento článek [Průvodce publikování dat služby](marketplace-publishing-data-service-creation.md).
