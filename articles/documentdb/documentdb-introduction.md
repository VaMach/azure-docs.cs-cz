---
title: "Azure Cosmos DB: Rozhraní DocumentDB API | Dokumentace Microsoftu"
description: "Zjistěte, jak můžete používat službu Azure Cosmos DB k ukládání a dotazování velkého množství dokumentů JSON s nízkou latencí pomocí jazyka SQL a JavaScriptu."
keywords: "json databáze, databáze dokumentu"
services: cosmosdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 686cdd2b-704a-4488-921e-8eefb70d5c63
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/04/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 60cc7b5acf0a8c54d8927e992b2189f49af1db3c
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017


---
# <a name="introduction-to-azure-cosmos-db-documentdb-api"></a>Úvod do služby Azure Cosmos DB: Rozhraní DocumentDB API

[Azure Cosmos DB](../cosmos-db/introduction.md) je globálně distribuovaná databázová služba Microsoftu s více modely pro klíčové aplikace. Azure Cosmos DB poskytuje [globální distribuci na klíč](../documentdb/documentdb-distribute-data-globally.md), [elastické škálování propustnosti a úložiště](../cosmos-db/partition-data.md) po celém světě, latence v řádu milisekund na 99. percentilu, [pět jasně definovaných úrovní konzistence](../documentdb/documentdb-consistency-levels.md) a zaručenou vysokou dostupnost. To vše je podloženo [nejlepšími smlouvami SLA v oboru](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/). Azure Cosmos DB [automaticky indexuje data](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), aniž by vyžadovala zapojení správy schémat a indexů. Zahrnuje více modelů a podporuje modely dokumentů, klíčových hodnot, grafů a sloupcových dat. 

![Rozhraní Azure DocumentDB API](./media/documentdb-introduction/cosmosdb-documentdb.png) 

Díky rozhraní DocumentDB API poskytuje služba Azure Cosmos DB bohaté a známé [schopnosti příkazů jazyka SQL](documentdb-sql-query.md) nad daty JSON bez schématu s konsistentní nízkou latencí dat. Tento článek poskytuje přehled rozhraní DocumentDB API pro službu Azure Cosmos DB a popisuje, jak ho použít k ukládání velkých objemů dat JSON, dotazování těchto dat s latencí v řádu milisekund a snadnému rozšíření schématu. 

## <a name="how-can-i-learn-about-the-documentdb-api"></a>Jak mohu zjistit více o rozhraní DocumentDB API?
Rychlý způsob, jak se dozvědět informace o rozhraní DocumentDB API a vidět ho v akci, představují tyto tři kroky: 

1. Podívejte se na dvouminutové video [Co je služba Azure Cosmos DB?](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/), které představuje výhody používání služby Azure Cosmos DB.
2. Podívejte se na tříminutové video [Vytvoření služby Azure Cosmos DB v Azure](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/), ve kterém se dozvíte, jak se službou Azure Cosmos DB začít pomocí webu Azure Portal.
3. Navštivte [Query Playground](http://www.documentdb.com/sql/demo), kde si můžete projít různé aktivity a dozvědět se o bohatých funkcích pro dotazování dostupných v DocumentDB. Pak zamiřte na kartu Izolovaný prostor (sandbox), kde můžete spustit své vlastní dotazy SQL a experimentovat s DocumentDB.

Poté se vraťte k tomuto článku, kde ji prozkoumáme důkladněji.  

## <a name="what-capabilities-and-key-features-does-azure-cosmos-db-offer"></a>Jaké schopnosti a klíčové funkce nabízí databáze Azure Cosmos?
Azure Cosmos DB prostřednictvím rozhraní DocumentDB API nabízí následující klíčové funkce a výhody:

* **Elasticky škálovatelná propustnost a úložiště:** Kapacitu databáze JSON je možné snadno vertikálně navyšovat nebo snižovat, aby splňovala potřeby vaší aplikace. Data se ukládají na discích SSD (solid-state drive), které nabízí nízkou a předvídatelnou latenci. Azure Cosmos DB podporuje kontejnery pro ukládání dat JSON, kterým se říká kolekce a které je možné škálovat na prakticky neomezené velikosti úložiště a zřízenou propustnost. S růstem vaší aplikace je možné službu Azure Cosmos DB bezproblémově elasticky škálovat s předvídatelným výkonem. 


* **Replikace ve více oblastech:** Azure Cosmos DB transparentně replikuje data do všech oblastí, které jste přidružili k vašemu účtu služby Azure Cosmos DB, a umožňuje vyvíjet aplikace, které vyžadují globální přístup k datům a zároveň poskytují kompromisy mezi konzistencí, dostupností a výkonem, a to vše s odpovídajícími zárukami. Azure Cosmos DB poskytuje transparentní regionální převzetí služeb při selhání se souběžnou instalací rozhraní API a schopností elastického škálování propustnosti a úložiště po celém světě. Další informace najdete v tématu [Globální distribuce dat pomocí služby Azure Cosmos DB](documentdb-distribute-data-globally.md).

* **Ad hoc dotazy se známou syntaxí SQL:** Můžete ukládat heterogenní dokumenty JSON a dotazovat se na ně pomocí známé syntaxe SQL. Azure Cosmos DB využívá vysoce souběžnou technologii indexování strukturovanou do protokolů, která nevyžaduje zamykání a automaticky indexuje veškerý obsah dokumentů. Díky tomu lze používat bohaté dotazy v reálném čase, aniž by bylo potřeba specifikovat parametry schématu, sekundární indexy nebo zobrazení. Další informace najdete v tématu [Dotazování služby Azure Cosmos DB](documentdb-sql-query.md). 
* **Spuštění JavaScriptu uvnitř databáze:** Logiku aplikace je možné vyjádřit jako uložené procedury, triggery a uživatelem definované funkce (UDF) prostřednictvím standardního JavaScriptu. To logice aplikace umožňuje pracovat s daty, aniž by bylo nutné se obávat neshody mezi aplikací a schématem databáze. Rozhraní DocumentDB API poskytuje možnost plného spouštění logiky aplikace JavaScript z transakcí přímo v databázovém stroji. Tato těsná integrace JavaScriptu umožňuje spouštět operace INSERT, REPLACE, DELETE a SELECT z javascriptového programu jako izolované transakce. Další informace najdete v tématu [DocumentDB – Programování na straně serveru](documentdb-programming.md).

* **Přizpůsobitelné úrovně konzistence:** Pro dosažení optimálního poměru mezi konzistencí a výkonem si můžete vybrat z pěti jasně definovaných úrovní konzistence. Pro dotazy a operace čtení nabízí služba Azure Cosmos DB pět různých úrovní konzistence: silná, omezená neaktuálnost, relace, konzistentní předpona a konečný výsledek. Tyto podrobné, dobře definované úrovně konzistence umožňují zvolit vhodný poměr mezi konzistencí, dostupností a latencí. Další informace najdete v tématu popisujícím [využití úrovní konzistence pro maximalizaci dostupnosti a výkonu](documentdb-consistency-levels.md).

* **Plně spravovaná služba:** Není nutné spravovat prostředky databáze a počítačů. DocumentDB je plně spravovaná služba Microsoft Azure – nepotřebujete spravovat virtuální počítače, nasazovat a konfigurovat software, spravovat škálování nebo řešit komplexní upgradování datové vrstvy. Každá databáze je automaticky zálohována a chráněna proti selháním v dané oblasti. Jednoduše si můžete přidat účet služby Azure Cosmos DB a zřídit kapacitu podle potřeby. Díky tomu se můžete soustředit na svou aplikaci, ne na provoz a správu databáze. 

* **Otevřené řešení:** Pomocí existujících dovedností a nástrojů můžete začít rychle. Programování s rozhraním DocumentDB API je jednoduché, srozumitelné a nevyžaduje, abyste se učili pracovat s novými nástroji nebo využívali vlastní rozšíření JSON nebo JavaScriptu. Máte přístup k veškerým funkcím databáze, včetně CRUD, dotazování a zpracování JavaScriptu přes jednoduché rozhraní RESTful HTTP. Rozhraní DocumentDB API pracuje s existujícími formáty, jazyky a standardy a zároveň k nim nabízí hodnotné schopnosti databáze.

* **Automatické indexování:** Azure Cosmos DB ve výchozím nastavení automaticky indexuje všechny dokumenty v databázi a neočekává ani nevyžaduje žádné schéma nebo vytváření sekundárních indexů. Nechcete indexovat všechno? Buďte bez obav, můžete také [výslovně nesouhlasit s používáním cest v souborech JSON](documentdb-indexing-policies.md).

## <a name="data-management"></a>Jak se spravují data pomocí rozhraní DocumentDB API?
Rozhraní DocumentDB API pomáhá spravovat data JSON prostřednictvím jasně definovaných databázových prostředků. Tyto prostředky se pro zachování vysoké dostupnosti replikují a je možné je jedinečně adresovat pomocí logického identifikátoru URI. DocumentDB nabízí pro všechny prostředky jednoduchý programovací model RESTful založený na HTTP. 


Účet databáze Azure Cosmos DB je jedinečný obor názvů, který vám dává přístup ke službě Azure Cosmos DB. Před vytvořením databázového účtu si musíte pořídit předplatné Azure, které vám umožní přístup k různým službám Azure. 

Všechny prostředky ve službě Azure Cosmos DB jsou modelovány a ukládány jako dokumenty JSON. Prostředky se spravují jako položky, kterými jsou dokumenty JSON obsahující metadata, a jako kanály, jimiž jsou kolekce položek. Sady položek se uchovávají ve svých příslušných kanálech.

Obrázek níže znázorňuje vztahy mezi prostředky Azure Cosmos DB:

![Hierarchický vztah mezi prostředky ve službě Azure Cosmos DB][1] 

Databázový účet se skládá ze sady databází, kde každá obsahuje několik kolekcí, z nichž každá pak může obsahovat uložené procedury, triggery, funkce UDF, dokumenty a související přílohy. K databázi jsou také přiřazeni uživatelé. Každý má sadu oprávnění pro přístup k různým kolekcím, uloženým procedurám, triggerům, funkcím UDF, dokumentům a přílohám. Databáze, uživatelé, oprávnění a kolekce jsou systémem definované prostředky s dobře známými schématy – dokumenty, uložené procedury, triggery, funkce UDF a přílohy obsahují libovolný, uživatelem definovaný obsah JSON.  

> [!NOTE]
> Vzhledem k tomu, že rozhraní DocumentDB API bylo dříve dostupné jako služba Azure DocumentDB, můžete pokračovat ve zřizování, monitorování a správě účtů vytvořených prostřednictvím nástrojů nebo rozhraní REST API pro správu prostředků Azure s použitím názvů prostředků Azure DocumentDB nebo Azure Cosmos DB. Když mluvíme o rozhraních Azure DocumentDB API, používáme tyto názvy zaměnitelně. 

## <a name="develop"></a>Jak se vyvíjí aplikace pomocí rozhraní DocumentDB API?

Azure Cosmos DB zveřejňuje prostředky přes rozhraní DocumentDB REST API, které je možné volat v jakémkoli jazyce schopném zasílat požadavky HTTP/HTTPS. Kromě toho pro rozhraní DocumentDB API nabízíme programovací knihovny pro řadu oblíbených jazyků. Tyto klientské knihovny zjednodušují mnoho aspektů práce s tímto rozhraním API. Řeší detaily, jako je například ukládání adres do mezipaměti, správa výjimek, automatické opakované pokusy apod. Knihovny jsou v tuto chvíli k dispozici pro následující jazyky a platformy:  

| Ke stažení | Dokumentace |
| --- | --- |
| [.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) |[Knihovna .NET](https://msdn.microsoft.com/library/azure/dn948556.aspx) |
| [Node.js SDK](http://go.microsoft.com/fwlink/?LinkID=402990) |[Knihovna Node.js](http://azure.github.io/azure-documentdb-node/) |
| [Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) |[Knihovna Java](http://azure.github.io/azure-documentdb-java/) |
| [JavaScript SDK](http://go.microsoft.com/fwlink/?LinkID=402991) |[Knihovna JavaScript](http://azure.github.io/azure-documentdb-js/) |
| neuvedeno |[Sada JavaScript SDK na straně serveru](http://azure.github.io/azure-documentdb-js-server/) |
| [Python SDK](https://pypi.python.org/pypi/pydocumentdb) |[Knihovna Python](http://azure.github.io/azure-documentdb-python/) |
| neuvedeno | [Rozhraní API pro MongoDB](documentdb-protocol-mongodb.md)


Pomocí [emulátoru služby Azure Cosmos DB](documentdb-nosql-local-emulator.md) můžete vyvíjet a testovat aplikace místně s použitím rozhraní DocumentDB API bez vytváření předplatného Azure a bez jakýchkoli nákladů. Jakmile budete spokojeni s fungováním aplikace v emulátoru, můžete přejít na účet služby Azure Cosmos DB v cloudu.

Kromě základních operací vytvoření, čtení, aktualizace a odstranění nabízí rozhraní DocumentDB API také bohaté rozhraní příkazů jazyka SQL k získávání dokumentů JSON a podporu spouštění logiky aplikací JavaScript z transakcí na straně serveru. Rozhraní pro spouštění dotazů a skriptů jsou k dispozici prostřednictvím všech knihoven platforem i rozhraní REST API. 

### <a name="sql-query"></a>Dotaz SQL
Rozhraní DocumentDB API podporuje dotazování dokumentů pomocí jazyka SQL, který je integrován do systému typů JavaScript, a výrazy s podporou relačních, hierarchických a prostorových dotazů. Dotazovací jazyk DocumentDB je jednoduché, ale výkonné rozhraní pro dotazování dokumentů JSON. Tento jazyk podporuje podmnožinu gramatiky ANSI SQL a přidává těsnou integraci s javascriptovými objekty, poli, vytvářením objektů a voláním funkcí. DocumentDB poskytuje svůj model dotazování bez explicitního schématu nebo parametrů indexování od vývojáře.

V rozhraní DocumentDB API je možné zaregistrovat uživatelem definované funkce (UDF) a odkazovat na ně jako součást příkazu jazyka SQL. Tím se gramatika rozšiřuje o podporu vlastní logiky aplikace. Tyto funkce se píší jako javascriptové programy a spouští se uvnitř databáze. 

Sada DocumentDB [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx) navíc pro vývojáře v .NET nabízí poskytovatele dotazů LINQ. 

### <a name="transactions-and-javascript-execution"></a>Transakce a spouštění JavaScriptu
Rozhraní DocumentDB API umožňuje psát logiku aplikace jako pojmenované programy vytvořené zcela v JavaScriptu. Tyto programy se registrují ke kolekci a s dokumenty v dané kolekci můžou provádět databázové operace. JavaScript je možné zaregistrovat ke spouštění jako trigger, uloženou proceduru nebo uživatelem definovanou funkci. Triggery a uložené procedury mohou vytvářet, číst, aktualizovat a odstraňovat dokumenty, zatímco uživatelem definované funkce se mohou spouštět jako součást logiky provádění dotazu bez přístupu pro zápis do kolekce.

Spouštění JavaScriptu v rámci rozhraní DocumentDB API je modelováno podle konceptů podporovaných relačními databázovými systémy. JavaScript zde slouží jako moderní náhrada jazyka Transact-SQL. Všechna logika JavaScriptu se spouští v ambientní transakci ACID s izolací snímku. Pokud během spouštění JavaScript vyvolá výjimku, je celá transakce zrušena.

## <a name="are-there-any-online-courses-on-azure-cosmos-db"></a>Existují nějaké online kurzy pro službu Azure Cosmos DB?

Ano, existuje kurz [Microsoft Virtual Academy](https://mva.microsoft.com/en-US/training-courses/azure-documentdb-planetscale-nosql-16847) pro Azure DocumentDB. 

>[!VIDEO https://mva.microsoft.com/en-US/training-courses-embed/azure-documentdb-planetscale-nosql-16847]
>
>

## <a name="next-steps"></a>Další kroky
Máte už účet Azure? Pak můžete se službou Azure Cosmos DB začít pomocí našich [Rychlých startů](../cosmos-db/create-documentdb-dotnet.md), které vás provedou vytvořením účtu a začátky práce se službou Cosmos DB.

[1]: ./media/documentdb-introduction/json-database-resources1.png


