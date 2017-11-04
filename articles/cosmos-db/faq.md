---
title: "Nejčastější dotazy k Azure Cosmos DB | Microsoft Docs"
description: "Získejte odpovědi na nejčastější dotazy týkající se Azure Cosmos databáze, databáze globálně distribuované, více modelu služby. Další informace o kapacitě a úrovně výkonu a škálování."
keywords: "Otázky k databázi, často kladené otázky, documentdb, azure, Microsoft azure"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: b68d1831-35f9-443d-a0ac-dad0c89f245b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: mimig
ms.openlocfilehash: 534bf37cc70420dc30fcd5c994ae7d9beb654072
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="azure-cosmos-db-faq"></a>Nejčastější dotazy k Azure Cosmos DB
## <a name="azure-cosmos-db-fundamentals"></a>Základy Azure Cosmos DB
### <a name="what-is-azure-cosmos-db"></a>Co je Azure Cosmos DB?
Azure Cosmos DB je globálně replikované databáze, více modelu služba, která nabízí bohaté dotazování přes data bez schémat, pomáhá poskytovat Konfigurovatelný a spolehlivý výkon a umožňuje rychlý vývoj. Ho všechny dosáhnete prostřednictvím spravovaná platforma, která je podpořenou výkonem a dosahem Microsoft Azure. 

Azure Cosmos DB je správným řešením pro webové, mobilní a herní, a aplikace či aplikace IoT při předvídatelnou propustnost, vysokou dostupnost, nízkou latenci a bez schémat datového modelu jsou klíčové požadavky. Poskytuje flexibilitu schémat a bohaté indexování a zahrnuje podporu transakcí několika dokumentů s integrovaným JavaScriptem. 

Další databáze otázky, odpovědi a pokyny pro nasazení a používání této služby najdete v tématu [stránce dokumentace Azure Cosmos DB](https://azure.microsoft.com/documentation/services/cosmos-db/).

### <a name="what-happened-to-documentdb"></a>Co se stalo s DocumentDB?
Rozhraní API DocumentDB je jedním z podporovaných rozhraní API a datové modely pro Azure Cosmos DB. Kromě toho Azure Cosmos DB podporuje můžete pomocí rozhraní Graph API (Preview), rozhraní API tabulky (Preview) a rozhraní API MongoDB. Další informace najdete v tématu [dotazy zákazníků DocumentDB](#moving-to-cosmos-db).

### <a name="how-do-i-get-to-my-documentdb-account-in-the-azure-portal"></a>Jak získat na můj účet DocumentDB na portálu Azure
Na portálu Azure klikněte na ikonu Azure Cosmos DB v levém podokně. Pokud jste měli účet DocumentDB před, nyní máte účet Azure Cosmos DB beze změny na vaši fakturaci.

### <a name="what-are-the-typical-use-cases-for-azure-cosmos-db"></a>Jaké jsou typické použití případů pro Azure Cosmos DB?
Azure Cosmos DB je dobrou volbou pro nové webové, mobilní a herní, a je důležité, aplikace či aplikace IoT kde automatické škálování, předvídatelný výkon, rychlé pořadí doby odezvy milisekund a schopnost dotazovat přes data bez schémat. Azure Cosmos DB slouží k rychlému vývoji a podpoře nepřetržitých iterací modelů dat aplikace. Aplikace, které spravují uživatelem generovaný obsah a data jsou [běžné případy použití pro Azure Cosmos DB](use-cases.md). 

### <a name="how-does-azure-cosmos-db-offer-predictable-performance"></a>Jak Azure Cosmos DB nabízí předvídatelný výkon?
A [jednotek žádosti](request-units.md) (RU) je mírou propustnosti v Azure Cosmos DB. Propustnost 1 RU odpovídá propustnosti operace GET u 1 KB dokumentu. Všechny operace v Azure DB Cosmos, včetně čtení, zápisu, dotazů SQL a spuštění uložených procedur, má deterministickou RU hodnotu, která je založena na propustnost potřebné k dokončení operace. Namísto přemýšlení o procesoru, vstupně-výstupní operace a paměti a jak každá ovlivňují propustnost aplikace, si můžete představit jako jednu míru RU.

Je možné rezervovat každý kontejner Azure Cosmos DB zajištěnou propustností z hlediska RUs propustnost za sekundu. Pro aplikace jakéhokoli rozsahu můžete srovnávací test jednotlivých požadavků, změřit jejich hodnoty RU a zřídit kontejner pro zpracování celkový počet jednotek žádosti ze všech požadavků. Můžete taky škálovat nebo snižovat propustnost vaší kontejneru potřebám vaší aplikace měnícím. Další informace o jednotkách žádosti a nápovědu pro určení vašeho kontejneru potřebuje, najdete v části [odhadnout požadavky propustnost](request-units.md#estimating-throughput-needs) a zkuste to [propustnost kalkulačky](https://www.documentdb.com/capacityplanner). Termín *kontejneru* zde označují odkazuje na kolekci DocumentDB rozhraní API, rozhraní Graph API grafu, kolekce MongoDB rozhraní API a rozhraní API tabulky tabulku. 

### <a name="how-does-azure-cosmos-db-support-various-data-models-such-as-keyvalue-columnar-document-and-graph"></a>Jak Azure Cosmos DB podporuje různé datové modely, například klíč/hodnota, sloupcové, dokument a graf?

Klíč hodnota (tabulky), sloupcové, dokumentů a data grafu modely jsou všechny nativně podporované kvůli ARS (atomů, záznamy a pořadí) návrhu tohoto Azure DB Cosmos je založený na. Atomů, záznamy a pořadí může být snadno namapované a promítá do různých datových modelů. Rozhraní API pro podmnožinu modely jsou k dispozici pravé nyní (DocumentDB, MongoDB, tabulky a rozhraní Graph API) a ostatní specifické pro další datové modely bude k dispozici v budoucnu.

Azure Cosmos DB má schéma lhostejné indexování modul schopná automaticky indexování všechna data, která ho ingestuje bez nutnosti žádné schéma nebo sekundární indexy od vývojáře. Modul spoléhá na sadu rozvržení logické indexu (obráceným, sloupcové, stromu), které oddělit rozložení úložiště z indexu a zpracování subsystémů dotazů. Cosmos DB má také možnost podporovat sadu přenosu protokolů a rozhraní API extensible způsobem a efektivně převede základní datový model (1) a rozložení logické indexu (2) díky tomu jednoznačně schopný zajistit podporu více datové modely nativně.

### <a name="is-azure-cosmos-db-hipaa-compliant"></a>Je kompatibilní s Azure Cosmos DB HIPAA?
Ano, je kompatibilní se standardem HIPAA Azure Cosmos DB. HIPAA zavádí požadavky na použití, zveřejnění a ochranu jednotlivě identifikovatelných zdravotních informací. Další informace najdete na webu [Centrum zabezpečení Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-azure-cosmos-db"></a>Jaká jsou omezení úložiště databáze Cosmos Azure?
Neexistuje žádné omezení na celkovém množství dat, která kontejner můžete pojmout v Azure Cosmos DB.

### <a name="what-are-the-throughput-limits-of-azure-cosmos-db"></a>Jaká jsou omezení propustnosti databáze Cosmos Azure?
Neexistuje žádné omezení na celkovém množství propustnosti, kterou kontejner může podporovat v Azure Cosmos DB. Klíče Rada je distribuovat vaši úlohu přibližně rovnoměrně mezi dostatečně velký počet klíčů oddílů.

### <a name="how-much-does-azure-cosmos-db-cost"></a>Kolik Azure Cosmos DB stojí?
Podrobnosti najdete [podrobnosti o cenách Azure Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/) stránky. Poplatky za používání služby Azure Cosmos DB určuje počet zřízené kontejnerů, počet hodin kontejnery byly online, a zřízené propustnosti pro každý kontejner. Termín *kontejnery* zde odkazuje na kolekci DocumentDB rozhraní API, grafu rozhraní Graph API, rozhraní API MongoDB shromažďování a Table API tabulky. 

### <a name="is-a-free-account-available"></a>Je k dispozici bezplatný účet?
Ano, můžete zaregistrovat pro účet časově omezené bez poplatků, bez závazků. Přejděte na [zdarma zkuste Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/) nebo Další informace najdete v [zkuste Azure Cosmos DB – nejčastější dotazy](#try-cosmos-db).

Pokud jste Azure ještě nepoužívali, můžete si zaregistrovat [bezplatný účet Azure](https://azure.microsoft.com/free/), kterým získáte 30 dnů a a Dal na vyzkoušení všech služeb Azure. Pokud máte předplatné sady Visual Studio, jste také vhodné pro [volné kredity Azure](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) na libovolnou službu Azure používat. 

Můžete také [emulátoru DB Cosmos Azure](local-emulator.md) pro vývoj a testování vaší aplikace místně pro uvolnění bez vytváření předplatného Azure. Až budete spokojeni s jak funguje aplikaci v emulátoru DB Cosmos Azure, můžete přejít k používání účtu Azure Cosmos DB v cloudu.

### <a name="how-can-i-get-additional-help-with-azure-cosmos-db"></a>Jak můžete získat další pomoc s Azure Cosmos DB?
Pokud potřebujete pomoc, oslovení nám na [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb) nebo [fórum MSDN](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB), nebo zasláním e-mailu můžete naplánovat očima chat s technického týmu Azure Cosmos DB [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com). 

<a id="try-cosmos-db"></a>
## <a name="try-azure-cosmos-db-subscriptions"></a>Zkuste předplatných Azure Cosmos DB

Nyní můžete sledovat v Azure Cosmos DB rozhraní časově omezené bez předplatného zdarma a závazky. Chcete-li zaregistrovat předplatné a zkuste databázi Cosmos Azure, přejděte na [zdarma zkuste Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/). Toto předplatné je nezávislý [bezplatná zkušební verze Azure](https://azure.microsoft.com/free/)a dá se použít kromě bezplatnou zkušební verzi Azure nebo Azure placené předplatné. 

Předplatná Azure Cosmos DB zkuste zobrazovat na portálu Azure vedle dalších předplatná spojená s vaším ID uživatele. 

Tyto podmínky platí pro odběry zkuste Cosmos databázi Azure:

* Jeden kontejner jedno předplatné pro SQL (DocumentDB rozhraní API), Gremlin (rozhraní Graph API) a účty tabulky.
* Až 3 kolekce jedno předplatné pro účty MongoDB.
* Kapacita úložiště 10 GB.
* Globální replikace je k dispozici v následujícím [oblastí Azure](https://azure.microsoft.com/regions/): střed USA, severní Evropy a Asie a Tichomoří – jihovýchod
* Maximální propustnost 5 tis. RU/s.
* Předplatných vyprší po 24 hodinách a je možné rozšířit na celkový 48 hodin.
* Pro účty zkuste Azure DB Cosmos; nelze vytvořit lístků podpory Azure Podpora je však k dispozici pro odběratele s existující plánům podpory. 

## <a name="set-up-azure-cosmos-db"></a>Nastavení Azure Cosmos DB
### <a name="how-do-i-sign-up-for-azure-cosmos-db"></a>Jak přihlásím Azure Cosmos DB?
Azure Cosmos DB je k dispozici na portálu Azure. První zaregistrujte si předplatné Azure. Poté, co jste se přihlásili, můžete přidat DocumentDB rozhraní API, rozhraní Graph API (Preview), rozhraní API tabulky (Preview) nebo rozhraní API MongoDB účet k předplatnému Azure.

### <a name="what-is-a-master-key"></a>Co je hlavní klíč?
Hlavní klíč je token zabezpečení pro přístup ke všem prostředkům účtu. Uživatelé, kteří mají klíč čtení a zápisu přístup ke všem prostředkům v databázovém účtu. Při distribuci hlavního klíče buďte opatrní. Primární hlavní klíč nebo sekundární hlavní klíč jsou k dispozici na **klíče** okno [portál Azure][azure-portal]. Další informace o klíčích najdete v tématu [zobrazení, kopírování a opětovné vytváření přístupových klíčů](manage-account.md#keys).

### <a name="what-are-the-regions-that-preferredlocations-can-be-set-to"></a>Jaké jsou oblasti, které může být nastavena PreferredLocations? 
Hodnota PreferredLocations lze nastavit pro žádného z regionů Azure, ve kterých je k dispozici Cosmos DB. Seznam dostupných oblastí najdete v tématu [oblastí Azure](https://azure.microsoft.com/regions/).

### <a name="is-there-anything-i-should-be-aware-of-when-distributing-data-across-the-world-via-the-azure-datacenters"></a>Je všechno, co I měli vzít v potaz při distribuci dat po celém světě prostřednictvím datových centrech Azure? 
Azure Cosmos DB přes všechny oblasti Azure, jak je uvedeno v nachází [oblastí Azure](https://azure.microsoft.com/regions/) stránky. Vzhledem k tomu, že je základní služby, má každý nový datacenter přítomnosti Azure Cosmos DB. 

Když nastavíte oblast, mějte na paměti, že Azure Cosmos DB respektuje suverénní a government cloudy. To znamená pokud vytvoříte účet v svrchovaných oblasti, nelze replikovat mimo danou svrchovaných oblast. Podobně nelze povolit replikaci do jiných umístění svrchovaných z mimo účtu. 

## <a name="develop-against-the-documentdb-api"></a>Vývoj s DocumentDB rozhraní API

### <a name="how-do-i-start-developing-against-the-documentdb-api"></a>Jak spustit vývoj proti rozhraní API DocumentDB?
Rozhraní API služby Microsoft DocumentDB je k dispozici v [portál Azure][azure-portal]. Nejprve musíte zaregistrovat předplatné Azure. Jakmile si zaregistrujete předplatné Azure, můžete přidat kontejner DocumentDB API k předplatnému Azure. Pokyny k přidání účtu Azure Cosmos DB najdete v tématu [vytvoření účtu Azure Cosmos DB databáze](create-documentdb-dotnet.md#create-account). Pokud jste měli účet DocumentDB v minulosti, máte nyní účet Azure Cosmos DB. 

Pro .NET, Python, Node.js, JavaScript a Javu jsou k dispozici sady [SDK](documentdb-sdk-dotnet.md). Vývojáři mohou použít také [RESTful HTTP API](/rest/api/documentdb/) pracovat s prostředky Azure Cosmos DB z různých platforem a jazyků.

### <a name="can-i-access-some-ready-made-samples-to-get-a-head-start"></a>Může přistupovat ke profesionální některé připravených vzorků?
Ukázky pro rozhraní API DocumentDB [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md), a [Python](documentdb-python-samples.md) sady SDK jsou k dispozici na Githubu.


### <a name="does-the-documentdb-api-database-support-schema-free-data"></a>Podporuje databázi DocumentDB API data bez schémat?
Ano, rozhraní API DocumentDB umožňuje aplikacím ukládat libovolné dokumenty JSON bez schématu definic nebo parametrů. Data jsou okamžitě k dispozici pro dotaz přes rozhraní dotazů SQL databáze Azure Cosmos.  

### <a name="does-the-documentdb-api-support-acid-transactions"></a>Podporuje rozhraní API DocumentDB transakce ACID?
Ano, rozhraní API DocumentDB podporuje transakce mezi dokumenty vyjádřené jako JavaScript uložených procedur a aktivačních událostí. Transakce jsou omezená na jeden oddíl v každé kolekci a spouštěny sémantice ACID jako "všechny nebo nic," izolované od ostatních souběžně spuštěných požadavků kód a uživatele. Pokud jsou výjimky vyvolány prostřednictvím spuštění serverový kód Javascriptové aplikace, celá transakce bude vrácena zpět. Další informace o transakcích najdete v tématu [databáze programu transakce](programming.md#database-program-transactions).

### <a name="what-is-a-collection"></a>Co je kolekce?
Kolekce je skupina dokumentů a jejich přidružené logiky Javascriptové aplikace. Kolekce je fakturovatelná entita, kde [náklady](performance-levels.md) je dáno propustnost a použít úložiště. Kolekce může mít rozsah jeden nebo více oddílů nebo serverů a můžete škálovat zvládaly prakticky neomezené objemy úložišť a propustnosti.

Kolekce jsou také entitami fakturace pro Azure Cosmos DB. Každá kolekce se fakturuje každou hodinu, podle zřízené propustnosti a využitého prostoru úložiště. Další informace najdete v tématu [Azure Cosmos DB ceny](https://azure.microsoft.com/pricing/details/cosmos-db/). 

### <a name="how-do-i-create-a-database"></a>Jak vytvořím databázi?
Databáze můžete vytvořit pomocí [portál Azure](https://portal.azure.com), jak je popsáno v [přidat do kolekce](create-documentdb-dotnet.md#create-collection), jeden z [SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md), nebo [rozhraní REST API](/rest/api/documentdb/). 

### <a name="how-do-i-set-up-users-and-permissions"></a>Jak nastavím uživatele a oprávnění?
Uživatele a oprávnění můžete vytvořit pomocí jedné z [Cosmos DB rozhraní API sady SDK](documentdb-sdk-dotnet.md) nebo [rozhraní REST API](/rest/api/documentdb/).  

### <a name="does-the-documentdb-api-support-sql"></a>Podporuje rozhraní API DocumentDB SQL?
Dotazovací jazyk SQL je vylepšená podmnožina dotazovacích funkcí, kterou podporuje SQL. Dotazovací jazyk Azure Cosmos DB SQL nabízí bohaté hierarchické a relační operátory a rozšiřitelnost prostřednictvím bázi jazyka JavaScript, uživatelsky definované funkce (UDF). Gramatika JSON umožňuje modelování dokumentů JSON ve formě stromů ve s popiskem uzly, které jsou používány Azure Cosmos DB automatických technikách indexování a dialektu dotazování SQL v Azure Cosmos DB. Informace o používání gramatiky SQL najdete v tématu [QueryDocumentDB] [ query] článku.

### <a name="does-the-documentdb-api-support-sql-aggregation-functions"></a>Podporuje DocumentDB rozhraní API funkce agregace SQL?
Rozhraní API DocumentDB podporuje agregace s nízkou latencí v jakémkoli měřítku prostřednictvím agregační funkce `COUNT`, `MIN`, `MAX`, `AVG`, a `SUM` prostřednictvím gramatiku SQL. Další informace najdete v tématu [agregační funkce](documentdb-sql-query.md#Aggregates).

### <a name="how-does-the-documentdb-api-provide-concurrency"></a>Jak rozhraní API DocumentDB zajišťuje souběžnost?
Rozhraní API DocumentDB podporuje optimistické řízení souběžného (přístupu OCC) prostřednictvím značek entit HTTP, neboli Etagů. Každý prostředek DocumentDB API má ETag a značku ETag je nastaven na serveru pokaždé, když je aktualizován dokument. Všechny zprávy odpovědi jsou součástí hlavičku ETag a aktuální hodnotu. Značky etag binárním rozsáhlým použít s hlavičku If-Match umožňuje serveru se rozhodnout, zda mají být aktualizovány prostředku. Hodnota If-Match je hodnota ETag, která má být zkontrolován. Pokud hodnota ETag odpovídá serveru hodnota ETag, prostředek je aktualizována. Pokud už je ETag aktuální, server odmítne operaci s "HTTP 412 Precondition selhání" kód odpovědi. Klient pak znovu načte prostředek získat aktuální hodnota ETag pro prostředek. Kromě toho značky etag binárním rozsáhlým umožňuje s hlavičku If-None-Match určit, jestli je potřeba znovu načtěte prostředku.

Chcete-li použít optimistickou metodu souběžného v rozhraní .NET, použijte [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) třídy. Ukázku .NET naleznete v části [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) v DocumentManagement ukázce na Githubu.

### <a name="how-do-i-perform-transactions-in-the-documentdb-api"></a>Jak se v rozhraní API DocumentDB provádí transakce?
Rozhraní API DocumentDB podporuje transakce integrované do jazyka prostřednictvím JavaScript uložených procedur a aktivačních událostí. Všechny databázové operace ve skriptech se spouští v izolaci snímku. Pokud je kolekci s jedním oddílem, provádění je vymezen na kolekci. Pokud je kolekce rozdělena na oddíly, provádění je vymezen na dokumenty se stejnou hodnotou klíče oddílu v rámci kolekce. Na začátku transakce se pořídí snímek verzí dokumentů (ETagy) a k potvrzení dojde pouze v případě, že skript uspěje. Pokud JavaScript vyvolá chybu, transakce se vrátí zpět. Další informace najdete v tématu [programování v jazyce JavaScript na straně serveru pro databázi Azure Cosmos](programming.md).

### <a name="how-can-i-bulk-insert-documents-into-cosmos-db"></a>Jak můžete I hromadného vložení dokumenty do Cosmos DB?
Vám může hromadného vložení dokumenty do Azure Cosmos DB v některém ze dvou způsobů:

* Nástroj pro migraci dat, jak je popsáno v [nástroj pro migraci databáze pro databázi Azure Cosmos](import-data.md).
* Uložené procedury, jak je popsáno v [programování v jazyce JavaScript na straně serveru pro databázi Azure Cosmos](programming.md).

### <a name="does-the-documentdb-api-support-resource-link-caching"></a>Podporuje DocumentDB API podporu prostředků odkaz ukládání do mezipaměti?
Ano, protože Azure Cosmos DB je služba RESTful, odkazy na zdroje jsou neměnné a mohou být uloženy v mezipaměti. Klienti DocumentDB API můžete zadat hlavičku "If-None-Match" pro čtení pro všechny prostředků jako dokumentu nebo kolekci a pak aktualizujte své místní kopie po změně verze serveru.

### <a name="is-a-local-instance-of-documentdb-api-available"></a>Je k dispozici místní instanci rozhraní API DocumentDB?
Ano. [Emulátoru DB Cosmos Azure](local-emulator.md) poskytuje zachováním emulace služby Cosmos DB. Podporuje funkce, které jsou shodné s Azure Cosmos databáze, včetně podpory pro vytváření a dotazování dokumentů JSON, zřizování a škálování kolekce a provádění uložené procedury a triggery. Můžete vyvíjet a testovat aplikace pomocí emulátoru DB Cosmos Azure a jejich nasazení do Azure v globálním měřítku tím, že změníte koncového bodu připojení pro Azure Cosmos DB jednu konfiguraci.

## <a name="develop-against-the-api-for-mongodb"></a>Vývoj pro rozhraní API pro MongoDB
### <a name="what-is-the-azure-cosmos-db-api-for-mongodb"></a>Co je Azure API DB Cosmos pro MongoDB?
Rozhraní API Azure Cosmos DB pro MongoDB je vrstvu kompatibility, která umožňuje aplikacím snadno a transparentně komunikovat s nativní databázový stroj Azure Cosmos DB pomocí existující, podporované komunity Apache MongoDB rozhraní API a ovladačů. Vývojáři teď můžete použít existující řetězy MongoDB nástroje a dovednosti vytvářet aplikace, které využívá výhod Azure Cosmos DB. Vývojáři těžit z jedinečných funkcích Azure Cosmos DB, mezi které patří automatické indexování, zálohování údržby, smlouvy o úrovni finančně zálohovány služeb (SLA) a tak dále.

### <a name="how-do-i-connect-to-my-api-for-mongodb-database"></a>Připojení k mé rozhraní API pro databázi MongoDB
Nejrychlejší způsob, jak připojit k rozhraní API služby Azure Cosmos DB pro MongoDB má k head přes [portál Azure](https://portal.azure.com). Přejděte na svůj účet a potom v levém navigační nabídce klikněte na **rychlý Start**. Rychlý Start je nejlepší způsob, jak získat fragmenty kódu pro připojení k vaší databázi. 

Azure Cosmos DB vynucuje vysokými nároky na zabezpečení a standardy. Účty Azure Cosmos DB vyžadují ověřování a zabezpečenou komunikaci prostřednictvím protokolu SSL, takže je nutné použít TLSv1.2.

Další informace najdete v tématu [připojit k rozhraní API pro databázi MongoDB](connect-mongodb-account.md).

### <a name="are-there-additional-error-codes-for-an-api-for-mongodb-database"></a>Existují další kódy chyb pro rozhraní API pro databázi MongoDB?
Kromě běžné kódy chyb MongoDB rozhraní API MongoDB má svou vlastní specifické chybové kódy:


| Chyba               | Kód  | Popis  | Řešení  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | Celkový počet jednotek žádosti použití překročilo četnost zřízené požadavek jednotky pro kolekci a byla omezena. | Zvažte škálování propustnost kolekce z portálu Azure nebo opakováním. |
| ExceededMemoryLimit | 16501 | Jako víceklientské služby překročil operaci přidělení paměti klienta. | Snížit rozsah operaci prostřednictvím konkrétnější kritéria dotazu nebo se obraťte na podporu [portál Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). <br><br>Příklad:  *&nbsp; &nbsp; &nbsp; &nbsp;db.getCollection('users').aggregate ([<br> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{$match: {název: "Andy"}}, <br> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{$sort: {stáří: -1} }<br>&nbsp;&nbsp;&nbsp;&nbsp;])*) |

## <a name="develop-with-the-table-api-preview"></a>Vývoj s tabulkou rozhraní API (Preview)

### <a name="terms"></a>Výrazy 
Azure Cosmos DB: Tabulka rozhraní API (Preview) odkazuje premium nabídky pomocí Azure Cosmos DB podpora tabulky oznamují na 2017 sestavení. 

Standardní tabulka SDK je existující tabulce úložiště Azure SDK. 

### <a name="how-can-i-use-the-new-table-api-preview-offering"></a>Jak můžete použít novou nabídku tabulky rozhraní API (Preview)? 
Rozhraní API služby Azure DB Cosmos tabulka je k dispozici v [portál Azure][azure-portal]. Nejprve musíte zaregistrovat předplatné Azure. Poté, co jste se přihlásili, můžete k předplatnému Azure přidat účet rozhraní API služby Azure Cosmos DB tabulky a poté přidejte tabulky k vašemu účtu. 

Během období preview při [sady SDK](../cosmos-db/table-sdk-dotnet.md) jsou k dispozici pro rozhraní .NET, můžete začít používat provedením [tabulky API](../cosmos-db/create-table-dotnet.md) úvodní článek.

### <a name="do-i-need-a-new-sdk-to-use-the-table-api-preview"></a>Je nutné novou sadu SDK používat rozhraní API tabulky (Preview)? 
Ano, [tabulku úložiště Premium Windows Azure (Preview) SDK](https://www.nuget.org/packages/WindowsAzure.Storage-PremiumTable) je k dispozici na NuGet. Další informace jsou k dispozici na [Azure Cosmos DB tabulky .NET API: stažení a poznámky k verzi](https://github.com/Microsoft/azure-docs-pr/cosmos-db/table-sdk-dotnet.md) stránky. 

### <a name="how-do-i-provide-feedback-about-the-sdk-or-bugs"></a>Jak poskytují zpětnou vazbu o SDK nebo chyby?
Váš názor můžete sdílet v některém z následujících způsobů:

* [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api)
* [Fórum MSDN](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB)
* [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb)

### <a name="what-is-the-connection-string-that-i-need-to-use-to-connect-to-the-table-api-preview"></a>Co je připojovací řetězec, který je nutné použít pro připojení k rozhraní API tabulky (Preview)?
Připojovací řetězec je:
```
DefaultEndpointsProtocol=https;AccountName=<AccountNamefromCosmos DB;AccountKey=<FromKeysPaneofCosmosDB>;TableEndpoint=https://<AccountNameFromDocumentDB>.documents.azure.com
```
Na stránce klíče na portálu Azure můžete získat připojovací řetězec. 

### <a name="how-do-i-override-the-config-settings-for-the-request-options-in-the-new-table-api-preview"></a>Jak přepsat nastavení konfigurace pro žádost o možnostech v nové rozhraní API tabulky (Preview)?
Informace o nastavení konfigurace najdete v tématu [možnosti Azure Cosmos DB](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Nastavení můžete změnit jejich přidáním do souboru app.config v sekci appSettings klientské aplikace.

    <appSettings>
        <add key="TableConsistencyLevel" value="Eventual|Strong|Session|BoundedStaleness|ConsistentPrefix"/>
        <add key="TableThroughput" value="<PositiveIntegerValue"/>
        <add key="TableIndexingPolicy" value="<jsonindexdefn>"/>
        <add key="TableUseGatewayMode" value="True|False"/>
        <add key="TablePreferredLocations" value="Location1|Location2|Location3|Location4>"/>....
    </appSettings>


### <a name="are-there-any-changes-for-customers-who-are-using-the-existing-standard-table-sdk"></a>Jsou pro zákazníky, kteří používají existující standardní tabulce SDK všechny změny?
Žádné. Neexistují žádné změny pro existující nebo nové zákazníky, kteří používají existující tabulce standardní sady SDK. 

### <a name="how-do-i-view-table-data-that-is-stored-in-azure-cosmos-db-for-use-with-the-table-api-review"></a>Zobrazení tabulky data, která je uložená v Azure Cosmos DB pro použití s rozhraním API pro tabulku (review) 
Na portálu Azure můžete procházet data. Můžete také použít kódu rozhraní API tabulky (Preview) nebo uvedené v další odpovědí nástroje. 

### <a name="which-tools-work-with-the-table-api-preview"></a>Které nástroje pracovat s rozhraním API pro tabulku (Preview)? 
Můžete použít starší verzi Azure Explorer (0.8.9).

Nástroje a flexibilně trvat připojovací řetězec ve formátu určeném dříve může podporovat nové rozhraní API tabulky (Preview). Seznam nástroje tabulky je uvedený na [Azure Storage Client Tools](../storage/common/storage-explorers.md) stránky. 

### <a name="do-powershell-or-azure-cli-work-with-the-new-table-api-preview"></a>Prostředí PowerShell nebo rozhraní příkazového řádku Azure fungují s novým rozhraním API tabulky (Preview)?
Plánujeme přidat podporu pro prostředí PowerShell a rozhraní příkazového řádku Azure Table API (Preview). 

### <a name="is-the-concurrency-on-operations-controlled"></a>Je na operace řízené souběžnost?
Ano, optimistickou metodu souběžného je k dispozici prostřednictvím použití mechanismu značka ETag. 

### <a name="is-the-odata-query-model-supported-for-entities"></a>Se model dotazů OData podporuje pro entity? 
Ano, podporuje rozhraní API tabulky (Preview) dotazu OData a dotazů LINQ. 

### <a name="can-i-connect-to-the-standard-azure-table-and-the-new-premium-table-api-preview-side-by-side-in-the-same-application"></a>Můžete připojit k standardní tabulky služby Azure a nové premium tabulky rozhraní API (Preview) vedle sebe ve stejné aplikaci? 
Ano, můžete připojit pomocí dvou samostatných instancí CloudTableClient, každý odkazující na vlastní identifikátor URI prostřednictvím připojovací řetězec.

### <a name="how-do-i-migrate-an-existing-azure-table-storage-application-to-this-new-offering"></a>Jak mohu migrovat do této nové nabídky existující aplikaci úložiště Azure Table?
Abyste mohli využívat nové rozhraní API tabulky nabídky na stávající úložiště dat v tabulce, obraťte se [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com). 

### <a name="what-is-the-roadmap-for-this-service-and-when-will-you-offer-other-standard-table-api-functionality"></a>Co je plán pro tuto službu a když vám nabídne další standardní funkce rozhraní API tabulky?
Plánujeme přidat podporu pro tokeny SAS, ServiceContext, statistiky, šifrování, analýzu a další funkce na straně klienta, protože jsme pokračovat směrem k Všeobecné Vám může vaše názory na [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api). 

### <a name="how-is-expansion-of-the-storage-size-done-for-this-service-if-for-example-i-start-with-n-gb-of-data-and-my-data-will-grow-to-1-tb-over-time"></a>Jak je rozšíření velikost úložiště, provádí pro tuto službu, je-li například spustit s  *n*  GB dat a data se v průběhu času rozšiřovat 1 TB? 
Azure Cosmos DB určená k poskytování neomezené úložiště prostřednictvím použití vodorovné škálování. Službu můžete sledovat a efektivně zvýšit úložiště. 

### <a name="how-do-i-monitor-the-table-api-preview-offering"></a>Jak se monitorování nabídku tabulky rozhraní API (Preview)?
Můžete použít rozhraní API tabulky (Preview) **metriky** podokně ke sledování požadavků a využití úložiště. 

### <a name="how-do-i-calculate-the-throughput-i-require"></a>Jak vypočítat propustnost, které můžu vyžadovat?
Odhadu kapacity můžete použít k výpočtu TableThroughput, které je nutné pro operace. Další informace najdete v tématu [jednotky žádosti odhad a úložiště dat](https://www.documentdb.com/capacityplanner). Obecně platí může představovat vaší entity jako JSON a zadejte čísla pro operace. 

### <a name="can-i-use-the-new-table-api-preview-sdk-locally-with-the-emulator"></a>Můžete použít nové rozhraní API tabulky (Preview) SDK místně pomocí emulátoru?
Ano, můžete použít rozhraní API tabulky (Preview) s místní emulátoru, při použití novou sadu SDK. Chcete-li stáhnout nové emulátoru, přejděte na [použití emulátoru DB Cosmos Azure pro místní vývoj a testování](local-emulator.md). Hodnota StorageConnectionString v souboru app.config musí být:

```
DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=https://localhost:8081`. 
```

### <a name="can-my-existing-application-work-with-the-table-api-preview"></a>Můžete Moje existující aplikace pracovat s rozhraním API pro tabulku (Preview)? 
Možnosti útoku na nové rozhraní API tabulky (Preview) je kompatibilní s existující Azure standardní tabulky SDK napříč vytvoření, odstranění, aktualizace a operace dotazů v rozhraní .NET API. Zkontrolujte, zda máte klíč řádku, protože rozhraní API tabulky (Preview) vyžaduje klíč oddílu a klíč řádku. Plánujeme také přidat další podporu SDK jako budeme pokračovat směrem k GA této nabídky služeb.

### <a name="do-i-need-to-migrate-my-existing-azure-table-based-applications-to-the-new-sdk-if-i-do-not-want-to-use-the-table-api-preview-features"></a>Potřebuji k migraci mé existující Azure tabulky aplikace založené na novou sadu SDK, pokud není chcete používat funkce rozhraní API tabulky (Preview)?
Ne, můžete vytvořit a používat existující prostředky standardní tabulky bez přerušení jakéhokoli druhu. Ale pokud nepoužijete nového rozhraní API tabulky (Preview), můžete nemohou využívat automatické index, možnost Další konzistence nebo globální distribuce. 

### <a name="how-do-i-add-replication-of-the-data-in-the-premium-table-api-preview-across-multiple-regions-of-azure"></a>Jak přidat replikaci dat na úrovni premium tabulky rozhraní API (Preview) nad několika oblastmi Azure?
Můžete použít portál Azure Cosmos DB [nastavení globální replikace](tutorial-global-distribution-documentdb.md#portal) přidávání oblastí, které jsou vhodné pro vaši aplikaci. K vývoji globálně distribuované aplikace, měli byste také přidat aplikaci s PreferredLocation informací nastavenou na místní oblast pro zajištění nízkou latenci pro čtení. 

### <a name="how-do-i-change-the-primary-write-region-for-the-account-in-the-premium-table-api-preview"></a>Změna zápisu primární oblast pro účet na úrovni premium tabulky rozhraní API (Preview)
V podokně portálu Azure Cosmos DB globální replikace můžete přidat oblast a potom převzetí služeb při selhání požadované oblasti. Pokyny najdete v tématu [vývoj s více oblast Azure Cosmos DB účty](regional-failover.md). 

### <a name="how-do-i-configure-my-preferred-read-regions-for-low-latency-when-i-distribute-my-data"></a>Jak lze nakonfigurovat Moje upřednostňované čtení oblasti pro s nízkou latencí při I distribuci svá data? 
Pomoc při čtení z místního umístění, použijte PreferredLocation klíč v souboru app.config. Pro existující aplikace rozhraní API tabulky (Preview) vrátí chybu, pokud je nastavená LocationMode. Tento kód odeberte, protože premium tabulky rozhraní API (Preview) převezme tyto informace ze souboru app.config. Další informace najdete v tématu [možnosti Azure Cosmos DB](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="how-should-i-think-about-consistency-levels-in-the-table-api-preview"></a>Jak by měla myslíte o úrovně konzistence v tabulce API (Preview)? 
Azure Cosmos DB poskytuje dobře odůvodněnou kompromis mezi konzistencí, dostupností a latencí. Azure Cosmos DB nabízí pět úrovní konzistence pro vývojáře tabulky rozhraní API (Preview), abyste mohli vybrat model správné konzistence na úrovni tabulky a provádět požadavky na jednotlivé při dotazování data. Když se klient připojí, může však určovat úrovní konzistence. Můžete změnit úroveň prostřednictvím nastavení app.config hodnotu TableConsistencyLevel klíče. 

Rozhraní API tabulky (Preview) nabízí nízkou latencí čte s "načíst vlastní zápisy" s konzistence typu relace jako výchozí. Další informace najdete v tématu [úrovně konzistence](consistency-levels.md). 

Ve výchozím nastavení Azure Table storage poskytuje silnou konzistenci v rámci oblasti a Eventual konzistenci sekundárního umístění. 

### <a name="does-azure-cosmos-db-offer-more-consistency-levels-than-standard-tables"></a>Nabízí Azure Cosmos DB další úrovně konzistence než standardní tabulky?
Ano, informace o tom, jak využívat distribuovaná povaha Azure Cosmos DB najdete v tématu [úrovně konzistence](consistency-levels.md). Protože záruky jsou k dispozici pro úrovně konzistence, můžete je používat s jistotou. Další informace najdete v tématu [možnosti Azure Cosmos DB](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="when-global-distribution-is-enabled-how-long-does-it-take-to-replicate-the-data"></a>Pokud je povoleno globální distribuční, jak dlouho trvá replikaci dat?
Jsme potvrďte data spolehlivě v místní oblast a odešlete data do jiných oblastí okamžitě v řádu milisekundách. Tato replikace je závislá pouze na času jejich návratu (požadavku) datového centra. Další informace o funkci globální distribuční databáze Cosmos Azure najdete v tématu [Cosmos databázi Azure: Služba globálně distribuované databáze v Azure](distribute-data-globally.md).

### <a name="can-the-read-request-consistency-level-be-changed"></a>Můžete změnit úroveň konzistence požadavků na čtení?
S Azure Cosmos databáze můžete nastavit úroveň konzistence na úrovni kontejneru (pro tabulku). Pomocí sady SDK můžete změnit úroveň tím, že poskytuje hodnotu pro klíč TableConsistencyLevel v souboru app.config. Možné hodnoty jsou: silného typu s ohraničenou Prošlostí, relace, konzistentní předpony a Eventual. Další informace najdete v tématu [úrovně konzistence přizpůsobitelné dat v Azure Cosmos DB](consistency-levels.md). Klíče cílem je, že nelze nastavit konzistence žádost o úrovni na větší než nastavení pro tabulku. Například nelze nastavit úroveň konzistence v tabulce v Eventual a úroveň konzistence požadavek na silné. 

### <a name="how-does-the-premium-table-api-preview-account-handle-failover-if-a-region-goes-down"></a>Jak prémiový účet tabulky rozhraní API (Preview) zpracovávat převzetí služeb při selhání případě oblast výpadku? 
Premium vychází z globálně distribuované platformu Azure Cosmos DB jazyků tabulky rozhraní API (Preview). Aby se zajistilo, že vaše aplikace může tolerovat výpadků datacenter, povolte alespoň jeden další oblast pro účet na portálu Azure Cosmos DB [vývoj s více oblast Azure Cosmos DB účty](regional-failover.md). Můžete nastavit prioritu oblasti pomocí portálu [vývoj s více oblast Azure Cosmos DB účty](regional-failover.md). 

Můžete přidat jako v mnoha oblastech jako pro účet a řídit, kde ji můžete převzetí služeb při selhání tím, že poskytuje prioritu převzetí služeb při selhání. Samozřejmě používat databázi, budete muset zadat aplikaci existuje příliš. Pokud tak učiníte, nebudou vaši zákazníci se setkávají výpadku. Klient je sada SDK automaticky domovských stránek. To znamená může zjistit oblast, která je mimo provoz a automaticky převzetí služeb při selhání pro novou oblast.

### <a name="is-the-premium-table-api-preview-enabled-for-backups"></a>Je povolen premium tabulky rozhraní API (Preview) pro zálohování?
Ano, premium tabulky rozhraní API (Preview) vychází z jazyků platformy Azure Cosmos databáze pro zálohování. Zálohy jsou vytvářeny automaticky. Další informace najdete v tématu [Online zálohování a obnovení s Azure Cosmos DB](online-backup-and-restore.md).

 
### <a name="does-the-table-api-preview-index-all-attributes-of-an-entity-by-default"></a>Rozhraní API tabulky (Preview) všechny atributy indexu entity ve výchozím nastavení?
Ano, jsou všechny atributy entity indexovaný ve výchozím nastavení. Další informace najdete v tématu [Cosmos databázi Azure: indexování zásady](indexing-policies.md). 

### <a name="does-this-mean-i-do-not-have-to-create-multiple-indexes-to-satisfy-the-queries"></a>Neodpovídá, znamená to není nutné vytvořit více indexů, aby pokryl dotazy? 
Ano, Azure Cosmos DB poskytuje automatické indexování všechny atributy bez jakékoli definice schématu. Tato automatizace uvolní vývojáři zaměřit se na aplikace a nikoli na vytvoření indexu a správu. Další informace najdete v tématu [Cosmos databázi Azure: indexování zásady](indexing-policies.md).

### <a name="can-i-change-the-indexing-policy"></a>Můžete změnit zásady indexování?
Ano, můžete změnit zásady indexování tím, že poskytuje definici indexu. Další informace najdete v tématu [možnosti Azure Cosmos DB](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Budete muset správně kódování a vyhnuli nastavení. 

Ve formátu json řetězce v souboru app.config:
```
{
  "indexingMode": "consistent",
  "automatic": true,
  "includedPaths": [
    {
      "path": "/somepath",
      "indexes": [
        {
          "kind": "Range",
          "dataType": "Number",
          "precision": -1
        },
        {
          "kind": "Range",
          "dataType": "String",
          "precision": -1
        } 
      ]
    }
  ],
  "excludedPaths": 
[
 {
      "path": "/anotherpath"
 }
]
}
```

### <a name="azure-cosmos-db-as-a-platform-seems-to-have-lot-of-capabilities-such-as-sorting-aggregates-hierarchy-and-other-functionality-will-you-be-adding-these-capabilities-to-the-table-api"></a>Azure DB Cosmos jako platformu zdá se, že máte spoustu možností, jako je například řazení, agregace, hierarchie a další funkce. Můžete přidávat tyto funkce rozhraní API tabulky? 
Tabulka rozhraní API ve verzi preview, nabízí stejné funkce dotazu jako Azure Table storage. Azure Cosmos DB také podporuje řazení, agregace, geoprostorové dotazu, hierarchie a širokou škálu integrované funkce. Poskytujeme další funkce v rozhraní API tabulky v aktualizaci budoucí služby. Další informace najdete v tématu [dotazy SQL pro rozhraní API služby Azure Cosmos databáze DocumentDB](../documentdb/documentdb-sql-query.md).
 
### <a name="when-should-i-change-tablethroughput-for-the-table-api-preview"></a>Kdy je třeba změnit TableThroughput pro rozhraní API tabulky (Preview)?
Měli byste změnit TableThroughput, pokud platí některá z následujících podmínek:
* Provádění extrakce, transformace a načítání (ETL) dat, nebo chcete nahrát velké množství dat v krátkém čase. 
* Budete potřebovat další propustnost z kontejneru na back-end. Například můžete zjistit, že používané propustnost je větší než zřízené propustnosti a jste jsou získávání omezené. Další informace najdete v tématu [sadu propustnost pro Azure Cosmos DB kontejnery](set-throughput.md).

### <a name="can-i-scale-up-or-scale-down-the-throughput-of-my-table-api-preview-table"></a>Je možné škálovat nebo snižovat propustnost tabulka tabulky rozhraní API (Preview)? 
Ano, můžete na portálu Azure Cosmos DB škálování podokně škálování propustnost. Další informace najdete v tématu [sadu propustnost](set-throughput.md).

### <a name="is-a-default-tablethroughput-set-for-newly-provisioned-tables"></a>Je výchozí pro nově zřízeného tabulky nastavit TableThroughput?
Ano, pokud TableThroughput prostřednictvím app.config nepotlačí a nepoužívejte kontejner předem vytvořené v Azure Cosmos DB, služba vytvoří tabulku s propustností 400.
 
### <a name="is-there-any-change-of-pricing-for-existing-customers-of-the-standard-table-api"></a>Je k dispozici žádné změně ceny pro stávající zákazníky služby standardní rozhraní API tabulky?
Žádné. Neexistuje žádná změna v ceny pro stávající zákazníky služby standardní API tabulky. 

### <a name="how-is-the-price-calculated-for-the-table-api-preview"></a>Výpočtu ceny pro rozhraní API tabulky (Preview) 
Cena závisí na přidělené TableThroughput. 

### <a name="how-do-i-handle-any-throttling-on-the-tables-in-table-api-preview-offering"></a>Jak pracovat, žádné omezení pro tabulky v tabulce rozhraní API (Preview) nabízí? 
Pokud rychlost požadavků překročí kapacitu zřízené propustnosti pro základní kontejner, obdržíte chybu a sady SDK bude opakovat volání použitím zásady opakování.

### <a name="why-do-i-need-to-choose-a-throughput-apart-from-partitionkey-and-rowkey-to-take-advantage-of-the-premium-table-api-preview-offering-of-azure-cosmos-db"></a>Proč je muset zvolit propustnost kromě PartitionKey a RowKey využít nabídky tabulky rozhraní API (Preview) premium databáze Cosmos Azure?
Azure Cosmos DB nastaví výchozí propustnost pro váš kontejner, pokud nezadáte, jeden v souboru app.config. 

Azure Cosmos DB poskytuje záruku výkonu a latencí a s horní meze operaci. Tato záruka je možné, pokud modul můžete vynutit zásady správného řízení na operace klienta. Nastavení TableThroughput zajišťuje získat zaručenou propustnosti a latence, protože platforma si vyhrazuje tato kapacita a zaručí provozní úspěch. 

Pomocí specifikace propustnost pružně můžete využívat sezónnosti vaší aplikace, vyhovovaly propustnost a ušetřili náklady.

### <a name="azure-storage-sdk-has-been-very-inexpensive-for-me-because-i-pay-only-to-store-the-data-and-i-rarely-query-the-new-azure-cosmos-db-offering-seems-to-be-charging-me-even-though-i-have-not-performed-a-single-transaction-or-stored-anything-can-you-please-explain"></a>Azure SDK úložiště bylo pro mě nejlepší, velmi málo, protože I platit pouze pro ukládání dat a můžu zřídka dotazu. Nová nabídka Azure Cosmos DB zdá se, že se poplatků mi, i když I nebyly provést jedné transakci nebo nic uložené. Můžete je vysvětlete?

Azure Cosmos DB je navržený jako globálně distribuované, na základě smlouvy SLA systému se záruky dostupnosti, latence a propustnosti. Při rezervaci propustnost v Azure Cosmos DB tak, aby zajistil, na rozdíl od jiných systémů propustnost. Azure Cosmos DB poskytuje další funkce, které zákazníci požadovali, jako je například sekundární indexy a globální distribuci.  

### <a name="i-never-get-a-quota-full-notification-indicating-that-a-partition-is-full-when-i-ingest-data-into-table-storage-with-the-table-api-preview-i-do-get-this-message-is-this-offering-limiting-me-and-forcing-me-to-change-my-existing-application"></a>Nikdy zobrazí oznámení "kvóta úplné" (což znamená, že oddíl je úplná) při načítání dat do úložiště tabulek. S rozhraním API tabulky (Preview) se zobrazí tato zpráva. Tato nabídka je omezení mi a vynucení mi změnit mé existující aplikace?

Azure Cosmos DB je systém na základě smlouvy o úrovni služeb, který poskytuje neomezené škálování záruky latence, propustnost, dostupnosti a konzistence. K zajištění výkonu zaručenou premium, ujistěte se, že velikost dat a index jsou spravovat a škálovatelné. 10 GB limitu počet entit nebo počet položek na klíč oddílu je zajistit, že můžeme poskytnout vynikající výkon vyhledávání a dotazů. Aby se zajistilo, že vaše aplikace škáluje dobře i pro Azure Storage, doporučujeme, aby vám *není* vytvořit aktivní oddíl ukládání všechny informace v jednom oddílu a dotazování ho. 

### <a name="so-partitionkey-and-rowkey-are-still-required-with-the-new-table-api-preview"></a>Proto PartitionKey a RowKey se stále vyžadují s novým rozhraním API tabulky (Preview)? 
Ano. Protože plochy rozhraní API tabulky (Preview) je podobná úložiště Table SDK, klíč oddílu poskytuje účinný způsob, jak distribuovat data. Klíč řádku je jedinečný v rámci oddílu. Klíč řádku musí být přítomen a nemůže mít hodnotu null jako standardní sady SDK. Délka RowKey je 255 bajtů a délka PartitionKey je 100 bajtů (dokdy se zvýší na 1 KB). 

### <a name="what-are-the-error-messages-for-the-table-api-preview"></a>Jaké jsou chybové zprávy pro rozhraní API tabulky (Preview)?
Vzhledem k tomu, že tato předběžná verze je kompatibilní s standardní tabulky, většina chyby bude mapovat chyby z standardní tabulky. 

### <a name="why-do-i-get-throttled-when-i-try-to-create-lot-of-tables-one-after-another-in-the-table-api-preview"></a>Proč I získat omezeny při pokusu vytvořit mnoho tabulek jedna po druhé v tabulce API (Preview)?
Azure Cosmos DB je systém na základě smlouvy o úrovni služeb, který poskytuje latence, propustnost, dostupnosti a konzistence záruky. Protože je zřízená systému, si vyhrazuje prostředky k zajištění těchto požadavků. Rychlé rychlost vytváření tabulek je zjištěna a omezení. Doporučujeme podívat rychlost vytváření tabulek a snížit na méně než 5 za minutu. Mějte na paměti, že rozhraní API tabulky (Preview) je zřízená systém. V okamžiku, zřídíte začne platit pro ni. 

## <a name="develop-against-the-graph-api-preview"></a>Vývoj proti Graph API (Preview)
### <a name="how-can-i-apply-the-functionality-of-graph-api-preview-to-azure-cosmos-db"></a>Jak můžete použít funkci rozhraní Graph API (Preview) pro Azure Cosmos DB?
Rozšíření knihovny můžete použít funkci rozhraní Graph API (Preview). V této knihovně se nazývá grafy Microsoft Azure a je k dispozici na NuGet. 

### <a name="it-looks-like-you-support-the-gremlin-graph-traversal-language-do-you-plan-to-add-more-forms-of-query"></a>Zdá se, podporu jazyka traversal Gremlin grafu. Máte v plánu přidat další formuláře dotazu?
Ano, plánujeme přidat v budoucnu další mechanismy pro dotaz. 

### <a name="how-can-i-use-the-new-graph-api-preview-offering"></a>Jak můžete použít novou nabídku rozhraní Graph API (Preview)? 
Chcete-li začít, proveďte [rozhraní Graph API](../cosmos-db/create-graph-dotnet.md) úvodní článek.

<a id="moving-to-cosmos-db"></a>
## <a name="questions-from-documentdb-customers"></a>Dotazy zákazníků DocumentDB
### <a name="why-are-you-moving-to-azure-cosmos-db"></a>Proč se vám přesun k databázi Cosmos Azure? 

Azure Cosmos DB je další velký nárůst ve globálně distribuované databáze cloudu v odpovídajícím měřítku. Jako zákazník DocumentDB nyní máte přístup k systému revoluční a možnosti, které nabízí Azure Cosmos DB.

Azure Cosmos DB spustit jako "Projektu Florencii" v 2010 tak, aby adres problémové body, jimž vývojáři při vytváření aplikace ve velkém měřítku uvnitř společnosti Microsoft. Na výzvy vytváření globálně distribuované aplikace nejsou jedinečné společnosti Microsoft, proto jsme provedli první generace této technologie k dispozici v 2015 pro vývojáře Azure ve formě Azure DocumentDB. 

Od tohoto okamžiku jsme přidali nové funkce a zavedla významná nové funkce. Azure Cosmos DB je výsledek. Jako součást této verze, DocumentDB zákazníků s jejich daty, automaticky a bezproblémově budou Azure Cosmos DB zákazníků. Tyto funkce jsou v oblasti základní databázový stroj, jakož i globální distribuci, elastickou škálovatelnost a špičkový, komplexní SLA. Konkrétně jsme vyvinuly databázový stroj Azure Cosmos DB efektivně mapovat oblíbených datové modely, typ systémy a rozhraní API základní datový model Azure Cosmos DB. 

Aktuální projevem vývojáře přístupem Tato práce je nová podpora pro [Gremlin](../cosmos-db/graph-introduction.md) a [tabulky úložiště rozhraní API](../cosmos-db/table-introduction.md). A právě začátek. Plánujeme přidat další oblíbených rozhraní API a novější datové modely v čase, s další vylepšení výkonu a úložiště v globálním měřítku. 

Je důležité zmínit, DocumentDB [SQL dialekt](../documentdb/documentdb-sql-query.md) byla vždy jen jednou z mnoha rozhraní API, které může podporovat základní Azure DB Cosmos. Pro vývojáře, kteří používají plně spravovaná služba, například Azure Cosmos DB je rozhraní pouze ke službě rozhraní API, který je zveřejněný prostřednictvím služby. Nic skutečně změní pro stávající zákazníky služby DocumentDB. V Azure DB Cosmos získáte přesně stejné SQL rozhraní API, které nabízí DocumentDB. A teď (a v budoucnu) můžete přístup k dalším funkcím dříve nedostupné 

Jiné projevem naše trvalá práce je rozšířené základem pro globální a elastické škálovatelnost propustnost a úložiště. Provedli jsme několik vylepšení základní subsystém globální distribuce. Jednou z mnoha takové vývojáře směřujících funkcí je model konzistentní předpony konzistence, takže je celkový počet pět modely dobře definovaný konzistence. Vydá jsme řadu zajímavějšího funkcí, jako se pro dospělé. 

### <a name="what-do-i-need-to-do-to-ensure-that-my-documentdb-resources-continue-to-run-on-azure-cosmos-db"></a>Co je třeba provést k zajištění, že moje prostředky DocumentDB dále spustit v Azure Cosmos DB?

Budete muset na všech beze změn. Vaše prostředky DocumentDB jsou teď prostředky Azure Cosmos DB a bez přerušení v rámci služby se při přesunutí došlo k chybě.

### <a name="what-changes-do-i-need-to-make-for-my-app-to-work-with-azure-cosmos-db"></a>Jaké změny jsou třeba chcete, aby pro aplikace pro práci s Azure Cosmos DB?

Neexistují žádné změny, aby. Názvy tříd, obory názvů a NuGet balíček nezměnily. Jako vždy doporučujeme, aby byl váš sady SDK aktuální využívat nejnovější funkce a vylepšení. 

### <a name="whats-changed-in-the-azure-portal"></a>Co se změnilo na portálu Azure?

DocumentDB je již nadále nebude zobrazovat na portálu jako služby Azure. Místo ní je nová ikona Azure Cosmos DB, jak je znázorněno na následujícím obrázku. Všechny kolekce jsou k dispozici, protože se nacházely před a je možné škálovat propustnosti, změna úrovně konzistence a sledování smluv SLA. Vylepšené možnosti dat Explorer (Preview). Teď můžete zobrazit a upravit dokumenty, vytvářet a spouštět dotazy a pracovat s uložené procedury, triggery a UDF z jedné stránky, jak je znázorněno na následujícím obrázku: 

![V okně Azure Cosmos DB kolekce](./media/faq/cosmos-db-data-explorer.png)

### <a name="are-there-changes-to-pricing"></a>Existují změny ceny?

Ne, náklady na provozování vaší aplikace v Azure Cosmos DB je stejný jako byl před.

### <a name="are-there-changes-to-the-slas"></a>Existují změny smluv SLA?

Ne, smlouvy SLA pro dostupnost, konzistence, latence a propustnosti jsou beze změny a se pořád zobrazí na portálu. Další informace najdete v tématu [SLA pro Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/).
   
![Aplikace úkolů s ukázkovými daty](./media/faq/azure-cosmosdb-portal-metrics-slas.png)


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
