---
title: "Příručka o vytváření datové služby pro Marketplace | Microsoft Docs"
description: "Podrobné pokyny o tom, jak vytvořit, certifikovat a datové služby pro nasazení zakoupit na webu Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 3a632825-db5b-49ec-98bd-887138798bc4
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: a853b4dbd1952ba4ea8ee68ea3ca98f588bb71a2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="mapping-an-existing-web-service-to-odata-through-csdl"></a>Mapování existující webovou službu na OData prostřednictvím CSDL
> [!IMPORTANT]
> **V tuto chvíli jsme již nejsou registrace všechny nové služby Data vydavatele. Nové dataservices nebude získat schválení pro výpis.** Pokud máte SaaS obchodní aplikace, který chcete publikovat na AppSource můžete najít další informace [zde](https://appsource.microsoft.com/partners). Pokud máte IaaS aplikace nebo služby vývojáře, které chcete publikovat na webu Azure Marketplace můžete najít další informace [zde](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Tento článek nabízí přehled o tom, jak používat CSDL mapovat existující služby kompatibilní službě OData. Vysvětluje, jak vytvořit mapování dokument (CSDL), který transformuje vstupní požadavek od klienta přes volání služby a výstup (data) zpět do klienta prostřednictvím kompatibilní OData informačního kanálu. Microsoft Azure Marketplace zveřejňuje služeb koncovým uživatelům pomocí protokolu OData. Služby, které jsou vystavené poskytovatelů obsahu (Data vlastníky) jsou zveřejněné v různých formulářů, například SOAP, REST, atd.

## <a name="what-is-a-csdl-and-its-structure"></a>Co je CSDL a jeho struktura?
CSDL (koncepční Schema Definition Language) je specifikace definování postupy k popisu webové služby nebo služba databáze v běžné tento problém XML pro Azure Marketplace.

Jednoduchý přehled **požadavku toku:**

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

**Tok dat** je v opačném směru:

  `Client <- Azure Marketplace <- Content Provider’s WebService`

**Obrázek 1** diagramy, jak klient získat data z obsahu zprostředkovatele (služba) prostřednictvím Azure Marketplace.  CSDL se používá mapování/transformační součástí pro zpracování požadavku a předávání dat mezi obsahu poskytovatele služeb a klienta, který.

*Obrázek 1: Podrobné toku z klienta, který k poskytovateli obsahu prostřednictvím Azure Marketplace*

  ![Kreslení](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

Pozadí na Atom, Atom Pub a protokolu OData, na kterém sestavení rozšíření Azure Marketplace, Zkontrolujte prosím: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

Výňatek ze výše odkaz: *"účelem Open Data protokolu (dále jen OData) je poskytnutí protokol založené na REST pro operace CRUD stylu (vytvoření, čtení, aktualizaci a odstraňování) s prostředky, které jsou zveřejněné jako datové služby. "Služba dat" je koncový bod tam, kde se nacházejí data z jedné nebo více "kolekcí" každý s nula nebo více "položky", které se skládají z zveřejněné zadali páry název hodnota. OData je publikovaná společností Microsoft v rámci standardy OASIS (organizace pro rozvoj z strukturovaných informace standardy), aby každý uživatel, který chce můžete vytvořit servery, klienty nebo nástrojů bez licenčních nebo omezení."*

### <a name="three-critical-pieces-that-have-to-be-defined-by-the-csdl-are"></a>Jsou tři důležité údaje, které musí být definovány CSDL:
* **Koncový bod** z služby zprostředkovatele webovou adresu (URI) službu
* **Parametrů dat** předávány jako vstup pro poskytovatele služeb definice parametry odesílány službě poskytovatele obsahu dolů datového typu.
* **Schéma** dat, jako odpověď na službu požaduje schéma dat doručován službou poskytovatele obsahu, včetně kontejneru, kolekce nebo tabulky, proměnné nebo sloupce a datové typy.

Následující diagram ukazuje přehled toku z kde klienta do příkaz OData (volání k poskytovateli obsahu webové službě) k získání výsledků nebo dat zpět.

  ![Kreslení](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a>pomocí následujících kroků:
1. Klient odešle požadavek prostřednictvím volání služby kompletní s vstupní parametry definované v XML tak, aby Azure Marketplace
2. CSDL se používá k ověření volání služby.
   * Naformátovaný služby volání se pak posílají do obsahu zprostředkovatelé služby pomocí Azure Marketplace
3. Webová služba spustí a preforms akce příkaz protokolu Http (tj. získat) je vrácen data do Azure Marketplace, kde je požadovaná data (pokud existuje) zpřístupňuje ve formátu XML do klienta pomocí mapování definované v CSDL.
4. Klient se odesílá data (pokud existuje) ve formátu XML nebo JSON

## <a name="definitions"></a>Definice
### <a name="odata-atom-pub"></a>Protokol pub OData ATOM
Nastavit rozšíření pub ATOM, kde každá položka představuje jeden řádek výsledků. Části obsahu položky je vylepšená tak, aby obsahovala hodnoty řádku – jako párů klíčových hodnot. Další informace naleznete zde: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### <a name="csdl---conceptual-schema-definition-language"></a>CSDL - Conceptual Schema Definition Language
Umožňuje definovat funkce (SPROCs) a entity, které jsou k dispozici prostřednictvím databáze. Další informace, které se nachází tady: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)  

> [!TIP]
> Klikněte **jiné verze** rozevírací seznam a vyberte verzi, pokud se nezobrazí v článku.
> 
> 

### <a name="edm---entry-data-model"></a>EDM - vstupní datový Model
* Přehled: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink]

[OverviewLink]:http://msdn.microsoft.com/library/vstudio/ee382825(v=vs.100).aspx
* Ve verzi Preview: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink]

[PreviewLink]:http://msdn.microsoft.com/library/aa697428(v=vs.80).aspx
* Datové typy: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink]

[DataTypesLink]:http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx

Následují podrobné zleva doprava toku z kde klienta do příkaz OData (volání k poskytovateli obsahu webové službě) k získání výsledků nebo dat zálohování:

  ![Kreslení](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)

## <a name="csdl-basics"></a>Základy CSDL
CSDL (koncepční Schema Definition Language) je specifikace definování postupy k popisu webové služby nebo služba databáze v běžné tento problém XML pro Azure Marketplace. Popisuje důležité CSDL kusy který **umožňuje předávání dat ze zdroje dat do Azure Marketplace.** Hlavní části jsou popsány zde:

* Rozhraní informace, které popisují všechny veřejně dostupné funkce (FunctionImport uzel)
* Datový typ informace pro všechny zprávy requests(input) a zpráva responses(outputs) (EntityContainer nebo objekt EntitySet/EntityType uzlů)
* Vazba informace o přenosu protokol, který je možné použít (záhlaví uzlu)
* Informace o adrese pro vyhledání zadaná služba (BaseURI atribut)

Stručně řečeno CSDL představuje kontraktu nezávislé na platformě a jazyku mezi žadatel služby a poskytovatelem služeb. Pomocí CSDL, můžete klienta najít webové služby a databáze služby a vyvolání svých veřejně dostupné úkolů.

### <a name="relating-a-csdl-to-a-database-or-a-collection"></a>Související CSDL do databáze nebo kolekce
**Specifikace CSDL**

CSDL se gramatika XML pro popis webové služby. Specifikace samotné je rozdělené do 4 důležité elementy: EntitySet, FunctionImport; Obor názvů a typ EntityType.

Aby lépe pochopit, umožňuje tato abstrakce CSDL se týkají tabulku.

Mějte na paměti,

  CSDL představuje kontraktu nezávislé na platformě a jazyku mezi **služby žadatele** a **poskytovatele služeb**. Pomocí CSDL, **klienta** mohou vyhledat **webové služby a databáze služby** a spustit žádné jeho veřejně dostupné **funkce.**

Pro datové služby čtyři části CSDL si lze představit z hlediska databáze, tabulky, sloupce a proceduře úložiště.

Týkající se těchto takto pro datové služby:

* EntityContainer ~ = databáze
* Objekt EntitySet ~ = tabulky
* EntityType ~ = sloupců
* Element FunctionImport ~ = uložené procedury

**Akce HTTP povolena**

* GET – vrátí hodnoty z databáze (vrátí kolekci)
* POST – sloužící k předávání dat do schránky a volitelné návratové hodnoty z databáze (vytvořit novou položku v kolekci, návratový id nebo identifikátor URI)
* ODSTRANĚNÍ – odstranění dat z databáze (odstraní kolekce)
* PUT – aktualizace dat do DB (nahraďte kolekci nebo ji vytvořte)

## <a name="metadatamapping-document"></a>Dokument metadat/mapování
Dokument metadat/mapování se používá k mapování poskytovateli obsahu existujících webových služeb tak, aby mohou být zpřístupněny jako webovou službu OData v systému Azure Marketplace. Je založena na CSDL a implementuje několik rozšíření CSDL, aby vyhovovaly potřebám REST na základě webové služby, které jsou k dispozici prostřednictvím Azure Marketplace. Rozšíření se nacházejí v [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) oboru názvů.

Příklad CSDL: (kopírování a vkládání následujícím příkladu CSDL do editoru XML a změňte tak, aby odpovídaly vaší služby.  Vložte do mapování CSDL kartě DataService při vytváření služby v [publikování portálu Azure Marketplace](https://publish.windowsazure.com)).

**Podmínky:** související CSDL podmínky k [publikování portál](https://publish.windowsazure.com) podmínky uživatelského rozhraní (PPUI).

* Nabízí "Title" v PPUI má vztah k MyWebOffer
* Moje_firma v PPUI má vztah k **zobrazovaný název vydavatele** v [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) uživatelského rozhraní
* Rozhraní API má vztah k webu nebo dat služby (plán v PPUI)

**Hierarchie:** společnost (poskytovateli obsahu) vlastní tyto nabídky, které mají/í, konkrétně service(s), který řádek nahoru rozhraním API.

### <a name="webservice-csdl-example"></a>Příklad CSDL webovou službu
Připojí k službě, která je vystavení koncový bod webové aplikace (např. aplikaci C#)

        <?xml version="1.0" encoding="utf-8"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all the web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition near the bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is the entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines the service call, replace & with the encode value (&amp;).
        In the input name value pairs {param} represents passed in value.
        Or the value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows the CSDL to specifically specify the verb of the service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes the parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how the web service call is exposed to marketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, the {...} point to the parameters defined below.
        Marketplace will read the parameters from this URITemplate and fill the values into the corresponding parameters of the BaseUri or RequestBody (below) during calls to your service.  
        It is okay for @d:BaseUri to include information only for Marketplace consumption, it will not be exposed to end users. i.e. the hardcoded AccountKey in the above BaseURI does not show up in the client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of the RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders to insert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how to pass userid and password via the header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed to end users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with the value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited to appearing as the value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used to specify values to insert into the ATOM feed returned to the end user.  You can specify the element to contain a fixed message by providing a value for the element (this is the default value).  @d:Map is an XPath expression that points into the response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay to also add a d:Map attribute to override this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of the web service call:
        @Name should match exactly (case sensitive) the {…} placeholders in the @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether the parameter is required.
        @d:Regex - optional, attribute to describe the string, limiting unwanted input at the entry of the system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in the Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating the service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in the order listed.
        @d:Match is an Xpath query on the service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is the error code to return if an response matches the error.
        @d:errorMessage is the user friendly message to return when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect to the service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- The EntityContainer defines the output data schema -->
        </EntityContainer>
        <!-- The EntityType @d:Map defines the repeating node (an XPath query) in the response (output data schema). -->
        <!-- If these nodes are outside a namespace, add the prefix in the xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in the ATOM feed, so comply with the XML element naming restrictions (no spaces or other illegal characters).
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query to point at the location to extract the content from your services response.
        The "." is relative to the repeating node in the EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID"    d:IsPrimaryKey="True" Type="Int32"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount"    Type="Double"    Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"    Type="String"    Nullable="false" d:Map="./City"/>
        <Property Name="State"    Type="String"    Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime"    Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [!TIP]
> Zobrazit další příklady CSDL webové služby v článku [příklady mapování existující webové služby OData prostřednictvím CSDLs](marketplace-publishing-data-service-creation-odata-mapping-examples.md)
> 
> 

### <a name="dataservice-csdl-example"></a>Příklad DataService CSDL
Připojí se ke službě, která je vystavení databázové tabulky nebo zobrazení, jak koncový bod následující příklad ukazuje, že dvě rozhraní API pro základní Data na základě CSDL rozhraní API (můžete použít zobrazení, nikoli tabulky).

        <?xml version="1.0"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all the data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of the EntitySet as a Service
        @Name is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); the table (or view) and columns to be returned by the data service.)
            Map is the schema.tabel or schema.view
            dals.TableName is the table Name
            Name is the name identifier for the EntityType and the Name of the service exposed to the client via the UI.
            dals:IsExposed determines if the table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is the schema name of the table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines the column properties and the output of the service.
            dals:ColumnName is the name of the column in the table /view.
            Type is the emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is the maximum length of the output value
            Name is the name of the Property and is exposed to the client facing UI
            dals:IsReturned is the Boolean that determines if the Service exposes this value to the client.
            IsQueryable is the Boolean that determines if the column can be used in a database query
            (For data Services: To improve Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is the numerical position x in the table or the View, where x is from 1 to the number of columns in the table.
            dals:DatabaseDataType is the data type of the column in the database, i.e. SQL data type dals:IsPrimaryKey indicates if the column is the Primary key in the table/view.  (The columns marked ISPrimaryKey are used in the Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a>Viz také
* Pokud vás zajímá učení a seznámit se s konkrétním uzlům a jejich parametrů, přečtěte si tento článek [datové služby OData mapování uzly](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) pro definice a vysvětlení, příklady a kontext případů použití.
* Pokud vás zajímá kontrola příklady, přečtěte si tento článek [Data služby OData mapování příklady](marketplace-publishing-data-service-creation-odata-mapping-examples.md) najdete ukázkový kód a pochopit syntaxe kódu a kontext.
* Pokud chcete vrátit do předepsaných cestu pro publikování datové služby v Azure Marketplace, přečtěte si tento článek [Průvodce publikování dat služby](marketplace-publishing-data-service-creation.md).

