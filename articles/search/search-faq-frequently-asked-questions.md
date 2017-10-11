---
title: "Nejčastější dotazy (FAQ) o službě Azure Search | Microsoft Docs"
description: "Získejte odpovědi na časté otázky týkající se služby vyhledávání Microsoft Azure"
services: search
author: HeidiSteen
manager: jhubbard
ms.service: search
ms.technology: search
ms.topic: article
ms.date: 08/03/2017
ms.author: heidist
ms.openlocfilehash: 02d5fac8cf9067ec544668f306fe49b805b3d164
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-search---frequently-asked-questions-faq"></a>Služba Azure Search – nejčastější dotazy (FAQ)
 
 Odpovědi na nejčastější dotazy o konceptech, kód a scénáře související s Azure Search.

## <a name="platform"></a>Platforma

### <a name="how-is-azure-search-different-from-full-text-search-in-my-dbms"></a>Jak se liší od fulltextového vyhledávání v mé databázového systému Azure Search?

Služba Azure Search podporuje více zdrojů dat, [lingvistické analýzy pro mnoho jazyků](https://docs.microsoft.com/rest/api/searchservice/language-support), [vlastní analýza dat vstupy zajímavé a neobvyklé](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search), hledání rank ovládacích prvků prostřednictvím [vyhodnocování profily](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)a činnost koncového uživatele funkce jako je například typeahead, počtu zvýraznění a Fasetové navigace. Zahrnuje také další funkce, například synonyma a plnohodnotný dotazovací syntaxe, ale těch, které nejsou obvykle rozlišení funkce.

### <a name="what-is-the-difference-between-azure-search-and-elasticsearch"></a>Jaký je rozdíl mezi Azure Search a Elasticsearch?

Při porovnávání technologie vyhledávání, zákazníci často požádejte pro konkrétní na tom, jak Azure Search porovná s Elasticsearch. Zákazníci, kteří zvolte Azure Search přes Elasticsearch pro jejich vyhledávání projekty aplikací obvykle provést, protože jsme provedli úlohu klíčů snazší nebo potřebují integraci s jinými technologiemi společnosti Microsoft:

+ Služba Azure Search je plně spravovaná Cloudová služba se 99,9 % Smlouvy o úrovni služeb (SLA) při zřizování s dostatečnou redundance (2 repliky pro čtení, 3 repliky pro čtení i zápis).
+ Společnosti Microsoft [přirozeného jazyka procesory](https://docs.microsoft.com/rest/api/searchservice/language-support) nabízejí analysis inguistic hrany.  
+ [Azure Search indexery](search-indexer-overview.md) může procházet celou řadu zdrojů dat Azure pro počáteční a přírůstkové indexování.
+ Pokud budete potřebovat rychlou odezvu na kolísání dotaz nebo indexování svazky, můžete použít [ovládací prvky posuvníku](search-manage.md#scale-up-or-down) ve službě Azure portálu nebo spuštění [skript prostředí PowerShell](search-manage-powershell.md), obcházení horizontálního oddílu správu přímo.  
+ [Vyhodnocování a ladění funkcí](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) poskytují způsob pro ovlivňující vyhledávání rank skóre nad rámec co může poskytnout samostatně na vyhledávací web. 

### <a name="can-i-pause-azure-search-service-and-stop-billing"></a>Můžete pozastavit službu Azure Search a zastavit fakturace?

Nelze pozastavit službu. Úložiště a výpočetní prostředky se přidělují pro výhradní použití při vytváření služby. Není možné k uvolnění a získat tyto prostředky na vyžádání. 

## <a name="indexing-operations"></a>Operace indexování

### <a name="backup-and-restore-or-download-and-move-indexes-or-index-snapshots"></a>Indexy zálohování a obnovení (nebo stahování a přesunutí) nebo snímky index?

Můžete sice [získat definice indexu](https://docs.microsoft.com/rest/api/searchservice/get-index) kdykoli, není žádný index extrakce, snímku nebo funkce zálohování – obnovení pro stahování *naplní* index běží v cloudu na místní systém, nebo ani ji přesunout do jiné služby Azure Search. 

Indexy jsou vytvořené a naplněny z kód, který můžete zapisovat a spustit pouze v Azure Search v cloudu. Zákazníci, kteří chcete přejít na jiné službě indexu obvykle upravit svůj kód používat nový koncový bod a znovu spusťte indexování. Pokud chcete mít možnost k pořízení snímku nebo zálohování index, přetypovat hlas [User Voice](https://feedback.azure.com/forums/263029-azure-search/suggestions/8021610-backup-snapshot-of-index).

### <a name="can-i-index-from-sql-database-replicas-applies-to-azure-sql-database-indexershttpsdocsmicrosoftcomazuresearchsearch-howto-connecting-azure-sql-database-to-azure-search-using-indexers"></a>Mohu indexovat z repliky databáze SQL (platí pro [Azure SQL Database indexery](https://docs.microsoft.com/azure/search/search-howto-connecting-azure-sql-database-to-azure-search-using-indexers))

 Neexistují žádná omezení na stránce použít primární nebo sekundární repliky jako zdroj dat, při vytváření indexu od začátku. Aktualizovat index s přírůstkové aktualizace (podle změněné záznamy) ale vyžaduje primární repliky. Tento požadavek pochází z databáze SQL, které záruky změňte sledování na pouze primární repliky. Pokud se pokusíte pomocí sekundární repliky pro úlohu obnovení indexu, není zaručeno, kterou získáte všechna data.

## <a name="search-operations"></a>Operace hledání

### <a name="can-i-search-across-multiple-indexes"></a>Můžete najít napříč více indexů?

Ne, tato operace není podporována. Hledání je vždy vymezeno na jeden index.

### <a name="can-i-restrict-search-corpus-access-by-user-identity"></a>Můžete omezit vyhledávání svátek přístup podle identity uživatele?

Služba Azure Search nemá modelu na základě rolí zabezpečení pro přístup k datům na uživatele. Ověřování je buď úplná práva nebo jen pro čtení na úrovni služby. Někteří zákazníci byla implementována řešení založená na [ `$filter` parametr vyhledávání](https://docs.microsoft.com/rest/api/searchservice/search-documents), ale v nejlepší je alternativní řešení. Pokud chcete tuto funkci, přetypovat hlas [User Voice](https://feedback.azure.com/forums/263029-azure-search/category/86074-security).

### <a name="why-are-there-zero-matches-on-terms-i-know-to-be-valid"></a>Proč existují nula odpovídá na podmínky, které lze zjistit, aby byl platný?

Nejběžnější případ není zároveň budete vědět, že každý typ dotazu podporuje chování různých vyhledávání a úrovně lingvistické analýzy. Fulltextové vyhledávání, který je převládá zatížení, zahrnuje analytické fáze jazyka, která dělí podmínky dolů kořenové forms. Tento aspekt analýza dotazu vrhá širší net přes odpovídající položky, protože tokenizovaná termín odpovídá větší počet variant.

Pokud vyvolání [jiné typy dotazů](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) (zástupný znak vyhledávání, přibližné vyhledávání, vyhledávání blízkých výrazů, návrhy, mimo jiné), neexistuje žádný lingvistické analýzy. Podmínky jsou přidány do stromu dotazu jako-je. 

### <a name="why-is-the-search-rank-a-constant-or-equal-score-of-10-for-every-hit"></a>Proč je pořadí hledání konstantní nebo rovna skóre 1.0 pro každý podle?

Ve výchozím nastavení, jsou hodnoceny na základě výsledků vyhledávání [statistické vlastnosti odpovídajících podmínky](search-lucene-query-architecture.md#stage-4-scoring)a seřazené sady nejvyšších po nejnižší ve výsledku. Ale některé typy (zástupný znak, předpony, regulární výraz) dotazů vždy přispívat konstantní skóre na celkové skóre dokumentu. Toto chování je záměrné. Vyhledávání systému Azure vynucuje konstantní skóre umožňující odpovídá nalezen prostřednictvím rozšíření dotazu mají být zahrnuty ve výsledcích, aniž by to ovlivnilo hodnocení. 

Předpokládejme například, že vytváří odpovídá na "prohlídka", "tourettes" a "tourmaline" vstup "prohlídka *" v hledání pomocí zástupných znaků. Vzhledem k povaze těchto výsledků, neexistuje žádný způsob, jak to bude přiměřeně infer – které podmínky jsou cenné více než jiné. Z tohoto důvodu jsme ignorovat termín frekvence, při vyhodnocování výsledky v dotazech typy zástupný znak, předpony a regulární výraz. Výsledky hledání podle částečné vstup, se získají skóre konstantní předejdete odchylka směrem potenciálně neočekávané odpovídá.

## <a name="design-patterns"></a>Vzory návrhu

### <a name="what-is-the-best-approach-for-implementing-localized-search"></a>Co je nejlepší metodou pro implementaci lokalizované hledání?

Většina zákazníků zvolte vyhrazené pole v kolekci, pokud jde o podpoře různá národní prostředí (jazyky) ve stejném indexu. Pole specifické pro národní prostředí umožňují přiřadit odpovídající analyzátor. Například přiřazování Microsoft francouzština Analyzer pole obsahující francouzštině řetězce. Zjednodušuje také filtrování. Pokud víte, že dotaz se zahájí na stránce fr-fr, může omezit výsledky vyhledávání na toto pole. Nebo vytvořte [vyhodnocování profil](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) poskytnout další relativní váhu pole.

## <a name="next-steps"></a>Další kroky

Je váš dotaz o chybějící funkce nebo funkce? V žádosti o funkci [webu User Voice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="see-also"></a>Viz také

 [StackOverflow: Služba Azure Search](https://stackoverflow.com/questions/tagged/azure-search)   
 [Jak úplné textové vyhledávání funguje ve službě Azure Search](search-lucene-query-architecture.md)  
 [Co je Azure Search?](search-what-is-azure-search.md)

 