---
title: "Úvod do DocumentDB, databáze JSON | Microsoft Docs"
description: "Informace o Azure DocumentDB, databázi NoSQL JSON Tato dokumentová databáze je navržena pro velký objem dat, elastickou škálovatelnost a vysokou dostupnost."
keywords: "json databáze, databáze dokumentu"
services: documentdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 686cdd2b-704a-4488-921e-8eefb70d5c63
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/16/2016
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: 7f5e33b7f80e3c1e1e3e66b3cab879a5bc30e823
ms.openlocfilehash: 18f2c1876fab914073c8aea67f67724f281a65aa


---
# <a name="introduction-to-documentdb-a-nosql-json-database"></a>Úvod do DocumentDB: Databáze NoSQL JSON
## <a name="what-is-documentdb"></a>Co je DocumentDB?
DocumentDB je plně spravovaná databázová služba NoSQL navržená pro vysoký a předvídatelný výkon, vysokou dostupnost, elastické škálování, globální distribuci a jednoduchý vývoj. Protože je DocumentDB databáze NoSQL bez schématu, poskytuje bohaté a známé schopnosti příkazů jazyka SQL s konsistentní nízkou latencí dat JSON – zajišťuje tak, že 99 % čtení je obslouženo do 10 milisekund a 99 % zápisů je obslouženo do 15 milisekund. Tyto jedinečné výhody činí z DocumentDB ideální řešení pro web, mobilní a herní zařízení, IoT a mnoho dalších aplikací s potřebou snadného škálování a globální replikace.

## <a name="how-can-i-learn-about-documentdb"></a>Jak mohu zjistit více o DocumentDB?
Rychlý způsob, jak se dozvědět informace o DocumentDB a vidět ji v provozu, představují tyto tři kroky: 

1. Podívejte se na dvouminutové video [Co je DocumentDB?](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/), které představuje výhody DocumentDB.
2. Podívejte se na tříminutové video [Vytvoření DocumentDB v Azure](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/), ve kterém se dozvíte, jak s DocumentDB začít přes Portál Azure.
3. Navštivte [Query Playground](http://www.documentdb.com/sql/demo), kde si můžete projít různé aktivity a dozvědět se o bohatých funkcích pro dotazování dostupných v DocumentDB. Pak zamiřte na kartu Izolovaný prostor (sandbox), kde můžete spustit své vlastní dotazy SQL a experimentovat s DocumentDB.

Poté se vraťte k tomuto článku, kde ji prozkoumáme důkladněji.  

## <a name="what-capabilities-and-key-features-does-documentdb-offer"></a>Jaké schopnosti a klíčové funkce nabízí DocumentDB?
Azure DocumentDB nabízí následující klíčové funkce a výhody:

* **Elasticky škálovatelná propustnost a úložiště:** Databázi JSON DocumentDB je možné snadno škálovat nahoru i dolů, aby splňovala potřeby vaší aplikace. Data se ukládají na discích SSD (solid-state drive), které nabízí nízkou a předvídatelnou latenci. DocumentDB podporuje kontejnery pro ukládání dat JSON, kterým se říká kolekce a které je možné škálovat na prakticky neomezené velikosti úložiště a zřízenou propustnost. S růstem vaší aplikace je možné DocumentDB bezproblémově elasticky škálovat s předvídatelným výkonem. 
* **Replikace více oblastí:** DocumentDB transparentně replikuje data do všech oblastí, které jste přidružili k vašemu účtu DocumentDB a umožňuje vám vytvářet aplikace, které vyžadují globální přístup k datům a zároveň poskytují kompromisy mezi konzistencí, dostupností a výkonem, a to vše s odpovídajícími zárukami. DocumentDB poskytuje transparentní regionální převzetí služeb při selhání se souběžnou instalací rozhraní API a schopností pružného škálování propustnosti a úložišť po celém světě. Další informace naleznete v části [Globální distribuce dat pomocí DocumentDB](documentdb-distribute-data-globally.md).
* **Ad hoc dotazy se známou syntaxí SQL:** Do DocumentDB můžete ukládat heterogenní dokumenty JSON a dotazovat se na ně známou syntaxí SQL. DocumentDB využívá vysoce souběžnou technologii indexování strukturovanou do protokolů, která nevyžaduje zamykání a automaticky indexuje všechen obsah dokumentů. Díky tomu lze používat bohaté dotazy v reálném čase, aniž by bylo potřeba specifikovat parametry schématu, sekundární indexy nebo zobrazení. Další informace najdete v tématu [Dotazování DocumentDB](documentdb-sql-query.md). 
* **Spuštění JavaScriptu uvnitř databáze:** Logiku aplikace je možné vyjádřit jako uložené procedury, triggery a uživatelem definované funkce (UDF) prostřednictvím standardního JavaScriptu. To logice aplikace umožňuje pracovat s daty, aniž by bylo nutné se obávat neshody mezi aplikací a schématem databáze. DocumentDB poskytuje možnost plného spouštění logiky aplikace JavaScript z transakcí přímo v databázovém stroji. Tato těsná integrace JavaScriptu umožňuje spouštět operace INSERT, REPLACE, DELETE a SELECT z javascriptového programu jako izolované transakce. Další informace najdete v tématu [DocumentDB – Programování na straně serveru](documentdb-programming.md).
* **Přizpůsobitelné úrovně konzistence:** Pro dosažení optimálního poměru mezi konzistencí a výkonem si můžete vybrat ze čtyř dobře definovaných úrovní konzistence. Pro dotazy a operace čtení DocumentDB nabízí čtyři úrovně konzistence: silnou, s ohraničenou odolností, založenou na relaci a případnou. Tyto podrobné, dobře definované úrovně konzistence umožňují zvolit vhodný poměr mezi konzistencí, dostupností a latencí. Další informace najdete v tématu o [využití úrovní konzistence pro maximalizaci dostupnosti a výkonu v DocumentDB](documentdb-consistency-levels.md).
* **Plně spravovaná služba:** Není nutné spravovat prostředky databáze a počítačů. DocumentDB je plně spravovaná služba Microsoft Azure – nepotřebujete spravovat virtuální počítače, nasazovat a konfigurovat software, spravovat škálování nebo řešit komplexní upgradování datové vrstvy. Každá databáze je automaticky zálohována a chráněna proti selháním v dané oblasti. Snadno si můžete přidat účet DocumentDB a zřídit kapacitu podle potřeby. Díky tomu se můžete soustředit na svou aplikaci, ne na provoz a správu databáze. 
* **Otevřené řešení:** Pomocí existujících dovedností a nástrojů můžete začít rychle. Programování s DocumentDB je jednoduché, srozumitelné a nevyžaduje, abyste se naučili pracovat s novými nástroji nebo využívali vlastní rozšíření technologií JSON nebo JavaScript. Máte přístup k veškerým funkcím databáze, včetně CRUD, dotazování a zpracování JavaScriptu přes jednoduché rozhraní RESTful HTTP. DocumentDB pracuje s existujícími formáty, jazyky a standardy a zároveň k nim nabízí hodnotné schopnosti databáze.
* **Automatické indexování:** DocumentDB ve výchozím nastavení automaticky indexuje všechny dokumenty v databázi a neočekává ani nevyžaduje žádné schéma nebo vytváření sekundárních indexů. Nechcete indexovat všechno? Buďte bez obav, můžete také [výslovně nesouhlasit s používáním cest v souborech JSON](documentdb-indexing-policies.md).

## <a name="a-namedata-managementahow-does-documentdb-manage-data"></a><a name="data-management"></a>Jak DocumentDB spravuje data?
Azure DocumentDB spravuje data JSON prostřednictvím dobře definovaných databázových prostředků. Tyto prostředky se pro zachování vysoké dostupnosti replikují a je možné je jedinečně adresovat pomocí logického identifikátoru URI. DocumentDB nabízí pro všechny prostředky jednoduchý programovací model RESTful založený na HTTP. 

Databázový účet DocumentDB je jedinečný obor názvů, který vám dává přístup k Azure DocumentDB. Před vytvořením databázového účtu si musíte pořídit předplatné Azure, které vám umožní přístup k různým službám Azure. 

Všechny prostředky v DocumentDB jsou modelovány a ukládány jako dokumenty JSON. Prostředky se spravují jako položky, kterými jsou dokumenty JSON obsahující metadata, a jako kanály, jimiž jsou kolekce položek. Sady položek se uchovávají ve svých příslušných kanálech.

Obrázek níže znázorňuje vztahy mezi prostředky DocumentDB:

![Hierarchický vztah mezi prostředky v DocumentDB, databázi NoSQL JSON][1] 

Databázový účet se skládá ze sady databází, kde každá obsahuje několik kolekcí, z nichž každá pak může obsahovat uložené procedury, triggery, funkce UDF, dokumenty a související přílohy. K databázi jsou také přiřazeni uživatelé. Každý má sadu oprávnění pro přístup k různým kolekcím, uloženým procedurám, triggerům, funkcím UDF, dokumentům a přílohám. Databáze, uživatelé, oprávnění a kolekce jsou systémem definované prostředky s dobře známými schématy – dokumenty, uložené procedury, triggery, funkce UDF a přílohy obsahují libovolný, uživatelem definovaný obsah JSON.  

## <a name="a-namedevelopa-how-can-i-develop-apps-with-documentdb"></a><a name="develop"></a>Jak mohu vyvíjet aplikace s DocumentDB?
Azure DocumentDB zveřejňuje prostředky přes rozhraní REST API, které je možné volat v jakémkoli jazyce schopném zasílat požadavky HTTP/HTTPS. DocumentDB dále nabízí programovací knihovny pro několik oblíbených jazyků. Tyto knihovny zjednodušují mnoho aspektů práce s Azure DocumentDB. Řeší podrobnosti, jako například ukládání adres do mezipaměti, správu výjimek, automatické opakované pokusy apod. Knihovny jsou v tuto chvíli k dispozici pro následující jazyky a platformy:  

| Ke stažení | Dokumentace |
| --- | --- |
| [.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) |[Knihovna .NET](https://msdn.microsoft.com/library/azure/dn948556.aspx) |
| [Node.js SDK](http://go.microsoft.com/fwlink/?LinkID=402990) |[Knihovna Node.js](http://azure.github.io/azure-documentdb-node/) |
| [Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) |[Knihovna Java](http://azure.github.io/azure-documentdb-java/) |
| [JavaScript SDK](http://go.microsoft.com/fwlink/?LinkID=402991) |[Knihovna JavaScript](http://azure.github.io/azure-documentdb-js/) |
| neuvedeno |[Sada JavaScript SDK na straně serveru](http://azure.github.io/azure-documentdb-js-server/) |
| [Python SDK](https://pypi.python.org/pypi/pydocumentdb) |[Knihovna Python](http://azure.github.io/azure-documentdb-python/) |

Pomocí [emulátoru Azure DocumentDB](documentdb-nosql-local-emulator.md) můžete vyvíjet a testovat aplikace místně bez vytváření předplatného Azure a bez jakýchkoli nákladů. Jakmile budete spokojeni s fungováním aplikace v emulátoru DocumentDB, můžete přejít na účet Azure DocumentDB v cloudu.

Kromě základních operací vytvoření, čtení, aktualizace a odstranění nabízí DocumentDB také bohaté rozhraní SQL k získávání dokumentů JSON a podporu spouštění logiky aplikací JavaScript z transakcí na straně serveru. Rozhraní pro spouštění dotazů a skriptů jsou k dispozici prostřednictvím všech knihoven platforem i rozhraní REST API. 

### <a name="sql-query"></a>Dotaz SQL
Azure DocumentDB podporuje dotazování dokumentů pomocí jazyka SQL, který je integrován do systému typů JavaScript, a výrazy s podporou relačních, hierarchických a prostorových dotazů. Dotazovací jazyk DocumentDB je jednoduché, ale výkonné rozhraní pro dotazování dokumentů JSON. Tento jazyk podporuje podmnožinu gramatiky ANSI SQL a přidává těsnou integraci s javascriptovými objekty, poli, vytvářením objektů a voláním funkcí. DocumentDB poskytuje svůj model dotazování bez explicitního schématu nebo parametrů indexování od vývojáře.

Do DocumentDB je možné zaregistrovat uživatelem definované funkce (UDF) a odkazovat na ně jako součást dotazu SQL. Tím se gramatika rozšiřuje o podporu vlastní logiky aplikace. Tyto funkce se píší jako javascriptové programy a spouští se uvnitř databáze. 

Pro vývojáře .NET DocumentDB nabízí i poskytovatele dotazů LINQ jako součást sady [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx). 

### <a name="transactions-and-javascript-execution"></a>Transakce a spouštění JavaScriptu
DocumentDB umožňuje psát logiku aplikace jako pojmenované programy vytvořené zcela v JavaScriptu. Tyto programy se registrují ke kolekci a s dokumenty v dané kolekci můžou provádět databázové operace. JavaScript je možné zaregistrovat ke spouštění jako trigger, uloženou proceduru nebo uživatelem definovanou funkci. Triggery a uložené procedury mohou vytvářet, číst, aktualizovat a odstraňovat dokumenty, zatímco uživatelem definované funkce se mohou spouštět jako součást logiky provádění dotazu bez přístupu pro zápis do kolekce.

Spouštění JavaScriptu v rámci DocumentDB je modelováno podle konceptů podporovaných relačními databázovými systémy. JavaScript zde slouží jako moderní náhrada Transact-SQL. Všechna logika JavaScriptu se spouští v ambientní transakci ACID s izolací snímku. Pokud během spouštění JavaScript vyvolá výjimku, je celá transakce zrušena.

## <a name="next-steps"></a>Další kroky
Máte už účet Azure? Pak můžete začít s DocumentDB na webu [Azure Portal](https://portal.azure.com/#gallery/Microsoft.DocumentDB) [vytvořením databázového účtu DocumentDB](documentdb-create-account.md).

Nemáte účet Azure? Můžete:

* Zaregistrovat se k [bezplatné zkušební verzi Azure](https://azure.microsoft.com/free/), s níž získáte 30 dnů a 200 USD na vyzkoušení všech služeb Azure. 
* Pokud máte předplatné MSDN, jste oprávněni získat [bezplatný kredit Azure v hodnotě 150 USD měsíčně](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), který můžete využít pro libovolnou službu Azure. 
* Stáhněte si [Emulátor Azure DocumentDB](documentdb-nosql-local-emulator.md) pro místní vývoj aplikací.

Jakmile pak budete připraveni se dozvědět více, nahlédněte do [schématu kurzů](https://azure.microsoft.com/documentation/learning-paths/documentdb/), kde si můžete projít všechny pro vás dostupné materiály. 

[1]: ./media/documentdb-introduction/json-database-resources1.png




<!--HONumber=Jan17_HO2-->


