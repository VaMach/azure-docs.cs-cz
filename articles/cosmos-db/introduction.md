---
title: "Úvod do služby Azure Cosmos DB | Dokumentace Microsoftu"
description: "Přečtěte si něco o službě Azure Cosmos DB. Tato globálně distribuovaná databáze s více modely je navržena s ohledem na nízkou latenci, elastickou škálovatelnost a vysokou dostupnost."
services: cosmosdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: a855183f-34d4-49cc-9609-1478e465c3b7
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/10/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 1591a7935b10e22164d26a09c4e923101b517227
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017


---

# <a name="welcome-to-azure-cosmos-db"></a>Vítá vás Azure Cosmos DB

Azure Cosmos DB je globálně distribuovaná databáze Microsoftu pro více modelů. Stačí kliknout na tlačítko a Azure Cosmos DB vám umožní elasticky a nezávisle škálovat propustnost a úložiště v řadě geografických oblastí Azure. Nabízí záruku propustnosti, latence, dostupnosti a konzistence s kompletními [smlouvami o úrovni služeb](https://aka.ms/acdbsla) (SLA), které někdy nenabízejí žádné jiné databázové služby.

![Azure Cosmos DB je celosvětově distribuovaná databázová služba od Microsoftu, která nabízí elastické horizontální navýšení kapacity, zaručenou nízkou latenci, pět modelů konzistence a kompletní zaručené smlouvy SLA](./media/introduction/azure-cosmos-db.png)

Obsahuje databázový stroj optimalizovaný pro zápis, řízený prostředky, který netrvá dogmaticky na schématu a nativně podporuje více datových modelů: hodnota klíče, dokumenty, grafy a sloupce. Podporuje také spousty rozhraní API pro přístup k datům včetně následujících: [MongoDB](../documentdb/documentdb-protocol-mongodb.md), [DocumentDB SQL](../documentdb/documentdb-introduction.md), [Gremlin](graph-introduction.md) (Preview) a [Azure Tables](table-introduction.md) (Preview). Tato rozhraní postupně přibývají. 

Služba Azure Cosmos DB vznikla koncem roku 2010 a pomáhá řešit bolavá místa vývojářům, kteří v Microsoftu pracují s rozsáhlými aplikacemi. Vzhledem k tomu, že vytváření globálně distribuovaných aplikací není problém, který se týká jenom Microsoftu, zpřístupnili jsme tuto službu externě všem vývojářům Azure ve formě Azure DocumentDB. Azure Cosmos DB je dalším velkým pokrokem při vývoji DocumentDB a my ji teď zpřístupňujeme i vám. V rámci této verze služby Azure Cosmos DB se zákazníci DocumentDB (i se svými daty) stávají automaticky zákazníky Azure Cosmos DB. Tento přechod je plynulý a tito uživatelé teď mají přístup k většímu počtu nových funkcí, které Azure Cosmos DB nabízí. 

## <a name="capability-comparison"></a>Porovnání schopností

Azure Cosmos DB nabízí ty nejlepší možnosti relačních a nerelačních databází.

| Možnosti | Relační databáze    | Nerelační databáze (NoSQL) |     Azure Cosmos DB |
| --- | --- | --- | --- |
| Globální distribuce | x | x | ✓ Na klíč, více než 30 oblastí, souběžná instalace |
| Horizontální škálování | x | ✓ | ✓ Nezávislé škálování úložiště a propustnost | 
| Záruky latence | x | ✓ | ✓ <10 ms pro čtení, <15 ms pro zápis při p99 | 
| Vysoká dostupnost | x | ✓ | ✓ Stálé připojení, kompromisy PACELC, automatické a ruční převzetí služeb při selhání |
| Datový model + API | Relační + SQL | Více modelů + OSS API | Více modelů + SQL + OSS API (další už brzy) |
| Smlouvy SLA | ✓ | x | ✓ Komplexní SLA pro latenci, propustnost, konzistenci, dostupnost |

## <a name="key-capabilities"></a>Klíčové funkce
Azure Cosmos DB jakožto globálně distribuovaná databázová služba poskytuje následující možnosti, které pomáhají vytvářet škálovatelné, globálně distribuované aplikace s rychlou odezvou:

* [**Globální distribuce na klíč**](#global-distribution)
    * Aplikace je uživatelům ihned k dispozici, a to kdekoli. A vaše data můžou být taky.
    * Nemusíte si dělat starosti s hardwarem, přidáním uzlů, virtuálních počítačů nebo jader. Stačí ukázat a kliknout a data jsou, kde potřebujete. 

* [**Více datových modelů a oblíbená rozhraní API pro přístup k datům a dotazování na ně**](#data-models)
    * Podpora více datových modelů včetně hodnoty klíče, dokumentu, grafu a sloupců.
    * Rozšiřitelná rozhraní API pro Node.js, Java, .NET, .NET Core, Python a MongoDB.
    * SQL a Gremlin pro dotazy. 

* [**Elastické škálování propustnosti a úložiště na vyžádání, celosvětově**](#horizontal-scale)
    * Snadno škálujte propustnost v přírůstcích po [sekundách](../documentdb/documentdb-request-units.md) a [minutách](https://aka.ms/acdbrupm), přičemž to můžete kdykoli změnit. 
    * Škálujte úložiště [transparentně a automaticky](partition-data.md), abyste pokryli požadavky na velikost nyní a navždy.

* [**Vytváření aplikací s rychlou odezvou a vysokou důležitostí pro chod firmy**](#low-latency) 
    * Získejte přístup ke svým datům s latencemi v řádu milisekund (percentil 99), kdekoli na světě. 

* [**Zajištění neustálé dostupnosti**](#high-availability)
    * 99,99% dostupnost v jedné oblasti.
    * Nasaďte je do libovolného počtu [oblastí Azure](https://azure.microsoft.com/regions) pro zajištění vyšší dostupnosti.
    * [Simulujte selhání](../documentdb/documentdb-regional-failovers.md) jedné nebo více oblastí se zárukou nulových ztrát dat. 

* [**Zápis globálně distribuovaných aplikací správným způsobem**](#consistency)
    * [Pět modelů konzistence](../documentdb/documentdb-consistency-levels.md) nabízí silnou konzistenci pravděpodobnosti SQL až pravděpodobnosti NoSQL a cokoli mezi tím. 
  
* [**Záruka vrácení peněz**](#sla) 
    * Data budou k dispozici rychle, jinak dostanete zpátky peníze. 
    * [Smlouvy o úrovni služeb](https://aka.ms/acdbsla) jsou zárukou dostupnosti, latence, propustnosti a konzistence. 

* [**Není potřeba správa databázového schématu / indexu**](#schema-free)
    * Už si nemusíte dělat starosti se zajištěním synchronizace databázového schématu a indexů ve schématu aplikace. Schéma není zapotřebí. 

* [**Nízké náklady na vlastnictví**](#tco)
    * Toto řešení je pět až desetkrát [méně nákladné](https://aka.ms/documentdb-tco-paper) než nespravované řešení.
    * Třikrát levnější než DynamoDB.

<a id="global-distribution"></a>

## <a name="global-distribution"></a>Globální distribuce
Kontejnery Azure Cosmos DB se distribuují podél dvou dimenzí: 

1. V rámci dané oblasti jsou všechny prostředky horizontálně dělené do oddílů s využitím oddílů prostředků (místní distribuce). 
2. Každý oddíl prostředků se také replikuje napříč zeměpisnými oblastmi (globální distribuce). 

![Diagram znázorňující globální distribuci Azure Cosmos DB](./media/introduction/azure-cosmos-db-global-distribution.png) 

Když je nutné škálovat vaše potřeby v oblasti úložiště a propustnosti, Cosmos DB transparentně provádí operace správy oddílů ve všech oblastech. Bez ohledu na škálování, distribuci nebo selhání poskytuje Cosmos DB nadále jeden systémový obraz globálně distribuovaných prostředků. 

Globální distribuce prostředků v rámci služby Cosmos DB je [na klíč](../documentdb/documentdb-distribute-data-globally.md). Se svým databázovým účtem můžete kdykoli propojit libovolný počet zeměpisných oblastí. Stačí k tomu kliknout na pár tlačítek (nebo programově volat rozhraní API). 

Bez ohledu na objem dat nebo počet oblastí zaručuje Cosmos DB, že každá nově přidružená oblast začne na 99. percentilu zpracovávat požadavky klientů do hodiny. K tomu slouží paralelní nastavení zdrojových a kopírovaných dat ze všech zdrojových oddílů prostředků s nově přidruženou oblastí. Zákazníci také mají možnost odebrat stávající oblast nebo převést oblast, která byla dříve přidružená k jejich databázovému účtu, do offline režimu.

<a id="data-models"></a>
## <a name="multi-model-multi-api-support"></a>Podpora více modelů, více API
 Azure Cosmos DB nativně podporuje více datových modelů, včetně dokumentů, hodnoty klíče a řady sloupců. Základní model obsahu databázového stroje Cosmos DB je založen na sekvenci záznamu atomů (ARS). Atomy sestávají z malých sad primitivních typů, jako je řetězec, logický operátor a číslo. Záznamy jsou struktury, které se skládají z těchto typů. Sekvence jsou pole sestávající z atomů, záznamů nebo sekvencí. 

![Azure Cosmos DB s více modely a více rozhraními API](./media/introduction/azure-cosmos-db-multimodel.png) 
 
 Databázový stroj dokáže efektivně překládat a projektovat různé datové modely do datového modelu založeného na ARS. Základní datový model Cosmos DB je nativně přístupný z dynamicky zapsaných programovacích jazyků a dá se vystavit „tak, jak je“ jako JSON. 
 
 Služba také podporuje známá databázová rozhraní API pro přístup k datům a dotazování na ně. Databázový stroj Cosmos DB v současné době podporuje rozhraní API [DocumentDB SQL](../documentdb/documentdb-introduction.md), [MongoDB](../documentdb/documentdb-protocol-mongodb.md), [Azure Tables](table-introduction.md) (Preview) a [Gremlin](graph-introduction.md) (Preview). Můžete dále sestavovat aplikace pomocí známých rozhraní API OSS a získat všechny výhody v praxi testované a plně spravované globálně distribuované databázové služby. 

<a id="horizontal-scale"></a>
## <a name="horizontal-scaling-of-storage-and-throughput"></a>Horizontální škálování úložiště a propustnosti
Všechna data v kontejneru Cosmos DB (například kolekce dokumentů, tabulka nebo graf) jsou horizontálně rozdělena do oddílů a transparentně je spravují oddíly prostředků. Oddíl prostředků je konzistentní a vysoce dostupný kontejner dat rozdělený do oddílů podle [klíče dělení do oddílů specifikovaného zákazníkem](partition-data.md). Poskytuje jednu systémovou image pro sadu prostředků, které spravuje, a je základní jednotkou škálovatelnosti a distribuce. Služba Cosmos DB je navržena tak, aby umožňovala elastické škálování propustnosti na základě vzorců přenosu v aplikaci napříč různými zeměpisnými oblastmi na podporu kolísajících úloh, které se liší podle zeměpisné oblasti a času. Služba spravuje oddíly transparentně bez narušení dostupnosti, konzistence, latence nebo propustnosti kontejneru Cosmos DB.  
 
![Služba Azure Cosmos DB je škálovatelná horizontálně](./media/introduction/azure-cosmos-db-partitioning.png) 

Propustnost kontejneru Azure Cosmos DB můžete elasticky škálovat programovým zřizováním propustnosti pomocí [jednotek požadavků za sekundu (RU/s)](../documentdb/documentdb-request-units.md). Interně služba transparentně spravuje oddíly prostředků pro zajištění propustnosti na daném kontejneru. Cosmos DB zajišťuje, že propustnost je k dispozici napříč všemi oblastmi spojenými s daným kontejnerem. Nová propustnost začíná platit do pěti sekund od změny konfigurované hodnoty propustnosti. 

Propustnost můžete v kontejneru Cosmos DB zřizovat v přírůstcích po jedné sekundě a [po jedné minutě (RU/m)](request-units-per-minute.md). Zřízená propustnost v přírůstcích po jedné minutě slouží k zvládání nečekaných nárůstů úloh, které nastávají v přírůstcích po jedné sekundě. 

<a id="low-latency"></a>
## <a name="low-latency-guarantees-at-the-99th-percentile"></a>Záruky nízké latence na 99. percentilu
V rámci smluv SLA zaručuje Cosmos DB svým zákazníkům koncovou nízkou latentnost na 99. percentilu. Pro typickou 1kB položku zaručuje Cosmos DB koncovou latenci čtení pod 10 ms a indexovaných zápisů pod 15 ms na 99. percentilu v rámci stejné oblasti Azure. Střední latence jsou výrazně nižší (pod 5 ms).  Cosmos DB umožňuje díky horní hranici zpracování požadavků v každé databázové transakci klientům jasně rozlišovat mezi transakcemi s vysokou latencí vs. nedostupnou databází.

<a id="high-availability"></a>
## <a name="transparent-multi-homing-and-9999-high-availability"></a>Transparentní souběžná instalace a 99,99% vysoká dostupnost
Oblastem spojeným s vaším databázovým účtem Azure Cosmos DB můžete dynamicky přidružit priority. Priority se používají ke směrování požadavků do konkrétních oblastí v případě regionálních selhání. Pokud nastane nepravděpodobná situace regionálního selhání, Cosmos DB automaticky převezme služby při selhání v pořadí podle priorit.

Pokud chcete otestovat koncovou dostupnost aplikace, můžete [ručně vyvolat převzetí služeb při selhání](../documentdb/documentdb-regional-failovers.md) (rychlost je omezená na dvě operace během hodiny). Cosmos DB zaručuje v případě ručních regionálních převzetí služeb při selhání nulové ztráty dat. V případě regionálního selhání zaručuje Cosmos DB horní omezení ztráty dat během systémem vyvolaného automatického převzetí služeb při selhání. Aplikaci nemusíte po regionálním převzetí služeb při selhání znovu nasazovat a smlouvy SLA o dostupnosti spravuje databáze Azure Cosmos DB. 

Při tomto scénáři umožňuje Cosmos DB interakci s prostředky s využitím logických koncových bodů (nezávislých na oblasti) nebo fyzických koncových bodů (závislých na oblasti). Logické využití zaručuje, že je možné aplikaci v případě převzetí služeb při selhání instalovat souběžně. Druhá možnost zajišťuje přesnější kontrolu přesměrování čtení a zápisů aplikace do konkrétních oblastí. Cosmos DB zaručuje smlouvu SLA s 99,99% dostupností pro každý databázový účet. Záruky dostupnosti nejsou závislé na škálování (zřízená propustnost a úložiště), počtu oblastí nebo geografické vzdálenosti mezi oblastmi spojenými s danou databází. 

<a id="consistency"></a>
## <a name="multiple-well-defined-consistency-models"></a>Více řádně definovaných modelů konzistence
Komerčně distribuované databáze spadají do dvou kategorií: databáze, které vůbec nenabízejí řádně definované osvědčené volby konzistence, a databáze, které nabízejí dvě extrémně programovatelné volby (silná vs. nahodilá konzistence). Databáze uvedené jako první zatěžují vývojáře aplikací podrobnostmi protokolů replikace a očekávají od nich náročné kompromisy mezi konzistencí, dostupností, latencí a propustností. Druhá možnost klade tlak na volbu jednoho ze dvou extrémů. Navzdory spoustě průzkumů a návrhů více než 50 modelů konzistence nemohla komunita pro distribuované databáze komercionalizovat úrovně konzistence nad rámec silné a nahodilé konzistence. 

Cosmos DB umožňuje volbu mezi [pěti správně definovanými modely konzistence](../documentdb/documentdb-consistency-levels.md) podél spektra konzistence – silná, omezená neaktuálnost, [relace](http://dl.acm.org/citation.cfm?id=383631), předpona konzistence a nahodilé. 

![Azure Cosmos DB nabízí více správně definovaných (uvolněných) modelů konzistence, z nichž je možné vybírat.](media/introduction/azure-cosmos-db-consistency-levels.png)

Následující tabulka uvádí konkrétní záruky, které poskytují jednotlivé úrovně konzistence.
 
**Úrovně konzistence a záruky**

| Úrovně konzistence    | Záruky |
| --- | --- |
| Silné | Linearizovatelnost |
| Omezená neaktuálnost    | Konzistentní předpona Prodleva čtení mezi zápisy podle předpon k nebo intervalu t |
| Relace    | Konzistentní předpona Monotónní čtení, monotónní zápisy, čtení zápisů, zápisy po čtení |
| Konzistentní předpona    | Vrácené aktualizace jsou předponou všech aktualizací bez mezer |
| Nahodilé    | Čtení mimo pořadí |

Můžete nakonfigurovat výchozí úroveň konzistence účtu Cosmos DB (a později přepsat konzistenci v konkrétním požadavku pro čtení). Výchozí úroveň konzistence se interně vztahuje na data v rámci sad oddílů, kterými můžou být rozpětí oblasti. 


<a id="sla"></a>
## <a name="guaranteed-service-level-agreements"></a>Zaručené smlouvy o úrovni služeb

Cosmos DB je první spravovaná databázová služba, která nabízí 99,99% [záruku pokrytí smlouvou SLA](https://aka.ms/acdbsla) dostupnosti, propustnosti, nízké latence a konzistence.
* Dostupnost: smlouva SLA o 99,99% dostupnosti doby provozu pro každou operaci v rámci datového a kontrolního plánu.
* Propustnost: 99,99 % požadavků je úspěšně dokončeno. 
* Latence: 99,99 % z latencí menších než 10 ms na 99. percentilu
* Konzistence: 100 % požadavků na čtení vyhoví záruce konzistence pro úroveň konzistence, kterou požadujete.


<a id="schema-free"></a>
## <a name="schema-free"></a>Bez schématu

Relační i NoSQL databáze vynucují práci se správou schématu a indexu, vytváření verzí a migrací – což je všechno mimořádně náročné v globálně distribuované instalaci. Ale nedělejte si starosti – Cosmos DB umožňuje tento problém odstranit. S Cosmos DB není nutné spravovat schémata a indexy, zabývat se verzemi schématu nebo si dělat starosti s prostojem aplikace v průběhu migrace schémat. Databázový stroj Cosmos DB je zcela nezávislý na schématu – automaticky indexuje všechna data, která přijímá, bez vyžadování jakéhokoli schématu nebo indexů a zajišťuje bleskově rychlé dotazy. 

<a id="tco"></a>
## <a name="low-cost-of-ownership"></a>Nízké náklady na vlastnictví

 Při zohlednění všech celkových nákladů na vlastnictví můžou být spravované cloudové služby jako Azure Cosmos DB pětkrát až desetkrát nákladově efektivnější než jejich protějšky OSS, které běží místně nebo na virtuálních počítačích. A Azure Cosmos DB je dvakrát až třikrát levnější než DynamoDB pro vysokoobjemové úlohy. Více se dozvíte v [dokumentu white paper s informacemi o celkových nákladech na vlastnictví](https://aka.ms/documentdb-tco-paper). 

## <a name="next-steps"></a>Další kroky
Začínáme se službou Azure Cosmos DB s využitím jedné ze čtyř šablon Rychlý start:

* [Začínáme s rozhraním API DocumentDB služby Azure Cosmos DB](create-documentdb-dotnet.md)
* [Začínáme s rozhraním API MongoDB služby Azure Cosmos DB](create-mongodb-nodejs.md)
* [Začínáme s rozhraním API Graph služby Azure Cosmos DB](create-graph-dotnet.md)
* [Začínáme s rozhraním API Table služby Azure Cosmos DB](create-table-dotnet.md)

