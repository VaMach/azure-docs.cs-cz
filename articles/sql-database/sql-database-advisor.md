---
title: "Výkon doporučení – Azure SQL Database | Microsoft Docs"
description: "Databáze SQL Azure poskytuje doporučení pro vaše databáze SQL, který může zlepšit výkon aktuální dotaz."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: 1db441ff-58f5-45da-8d38-b54dc2aa6145
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 09/20/2017
ms.author: sstein
ms.openlocfilehash: 9b6c60a14578842f4b3b1a9e4724eab6de3f8815
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="performance-recommendations"></a>Doporučení výkonu

Azure SQL Database zjišťuje a přizpůsobuje s vaší aplikací a poskytuje přizpůsobené doporučení umožňuje maximalizovat výkon vaší databáze SQL. Výkon se nepřetržitě hodnotí analýzou historie využití vaší databáze SQL. Doporučení, které jsou k dispozici jsou založené na vzor jedinečný zatížení databáze a zvýšit jeho výkon.

> [!TIP]
> [Automatické ladění](sql-database-automatic-tuning.md) je doporučený způsob optimalizace výkonu. [Inteligentní Statistika](sql-database-intelligent-insights.md) je doporučeným způsobem monitorování výkonu. 
>

## <a name="create-index-recommendations"></a>Vytvoření doporučení indexu
Azure SQL Database nepřetržitě monitoruje spouštění dotazů a identifikuje indexy, které může zlepšit výkon. Po dostatek spolehlivosti, určité index chybí, nový **vytvořit index** doporučení se vytvoří. Databáze SQL Azure vytvoří spolehlivosti odhadem zvýšení výkonu, které by index uvést prostřednictvím čas. V závislosti na odhadované výkonnější doporučení jsou klasifikovány jako Vysoká, střední nebo Nízká. 

Indexy vytvořené pomocí doporučení jsou vždy označeny jako auto_created indexy. Uvidíte, jaké indexy jsou auto_created prohlížením sys.indexes zobrazení. Automaticky vytvořit indexy neblokovat příkazy ALTER nebo přejmenovat. Pokud se pokusíte vyřadit sloupec, který se má automaticky vytvořit index nad ním, předá příkazu a automaticky vytvořit index je vyřadit pomocí příkazu také. Příkaz ALTER/přejmenování na sloupce, které jsou indexované by blokovat obyčejné indexy.

Po vytvoření doporučení indexu bude Azure SQL Database porovnejte výkon dotazů s základní výkon. Pokud nový index uvést do režimu vylepšení výkonu, doporučení se označilo jako úspěšné a dopad sestava bude k dispozici. V případě, že index nebylo přineste výhody, ji budou automaticky zrušeny. Tímto způsobem Azure SQL Database zajišťuje, že pomocí doporučení budou pouze zlepšit výkon databáze.

Všechny **vytvořit index** doporučení obsahuje regrese zásadu, která nebude povolovat použití doporučení, pokud využití v jednotkách DTU databáze nebo fondu byla vyšší než 80 % v posledních 20 minut nebo pokud úložiště je více než 90 % využití. V takovém případě bude odložit doporučení.

## <a name="drop-index-recommendations"></a>Doporučení k odstranění indexu
Kromě zjišťování chybí index, Azure SQL Database průběžně analyzuje výkon stávající indexy. Pokud se nepoužívá index, bude Azure SQL Database doporučujeme jej vyřadíte. Vyřazení indexu se doporučuje ve dvou případech:
* Index je duplicitní s jiným indexem (stejná indexována a zahrnuté sloupce, schéma oddílu a filtry)
* Index se nepoužívá po delší dobu (93 dny)

Doporučení k odstranění indexu také projít ověření po implementaci. Pokud lepší výkon dopad sestavy je k dispozici. V případě, že se detekuje snížení výkonu, budou vráceny doporučení.


## <a name="parameterize-queries-recommendations"></a>Parametrizace doporučení dotazy
**Parametrizace dotazy** doporučení se zobrazí, když máte jeden nebo více dotazů, které jsou neustále probíhá překompilovat, ale end až s stejný plán spuštění dotazu. Tato podmínka otevře možnost použít vynucené Parametrizace, což vám umožní plány dotazů do mezipaměti a opakovaně v budoucí zvýšení výkonu a snížení využití prostředků. 

Každý dotaz původně vydaný pro SQL Server musí být zkompilovány Generovat plán spuštění. Každý generovaného plán je přidán do mezipaměti plánu a dalších spuštěních stejný dotaz můžete opakovaně použít tento plán z mezipaměti, což eliminuje potřebu další kompilace. 

Aplikace, které odesílají dotazy, které obsahují hodnoty parametry, může vést k zatížení, které pro každý dotaz s jiným parametrem hodnotami plán spuštění kompiluje znovu. V mnoha případech stejné dotazy s jiným parametrem hodnoty generovat stejný provádění plánů, ale tyto plány jsou stále samostatně přidaných do mezipaměti v plánu. Nutnosti rekompilace plány provádění používat prostředky databáze, zvýšit dotaz doba trvání a přetečení mezipaměti plánu způsobuje plány k vyloučení z mezipaměti. Toto chování systému SQL Server může být změněna nastavením možnost vynucené Parametrizace v databázi. 

Chcete-li odhadnout dopad toto doporučení, jsou k dispozici srovnání skutečné využití procesoru a předpokládané využití procesoru (jako v případě, že bylo použito doporučení). Kromě úspory využití procesoru zkracuje doba trvání vašeho dotazu pro čas strávený v kompilaci. Také bude mnohem menší nároky na mezipaměti plánu, povolení Většina plánů zůstanou v mezipaměti a znovu použít. Toto doporučení můžete použít snadno a rychle kliknutím na **použít** příkaz. 

Když použijete toto doporučení, zapne vynucené Parametrizace minut ve vaší databázi a začne monitorování procesu, který přibližně trvá po dobu 24 hodin. Po uplynutí této doby bude moci zobrazit sestavy ověření, který ukazuje využití procesoru vaší databáze, 24 hodin před a po použití doporučení. Poradce pro funkci SQL Database má bezpečnostní mechanismus, který automaticky vrátí použité doporučení v případě, že byla zjištěna snížení výkonu.

## <a name="fix-schema-issues-recommendations"></a>Opravte problémy doporučení schématu
**Opravte problémy schématu** doporučení se zobrazí, pokud služba SQL Database oznámení anomálií v počtu chyby související s schématu SQL aktivit ve vaší databázi SQL Azure. Toto doporučení se obvykle zobrazují, když databáze dojde více schématu související chyby (neplatný název sloupce, neplatný název objektu atd.) v rámci hodiny.

"Schématu problémy" jsou třídou chyby syntaxe v systému SQL Server, které dojít v případě, že definice schématu databáze a definice dotazu SQL nejsou zarovnány. Například jeden ze sloupců očekávanou dotazu může být chybějící v cílové tabulce, nebo naopak. 

"Opravit problém schématu" doporučení se zobrazí, když služba Azure SQL Database oznámení anomálií v počtu chyby související s schématu SQL aktivit ve vaší databázi SQL Azure. Následující tabulka obsahuje chyby, které souvisí s problémy schématu:

| Kód chyby SQL | Zpráva |
| --- | --- |
| 201 |Procedura nebo funkce,*'očekává parametr'*', která nebyla zadána. |
| 207 |Neplatný název sloupce ' *'. |
| 208 |Neplatný název objektu ' *'. |
| 213 |Název sloupce nebo počet zadaných hodnot neodpovídá definici tabulky. |
| 2812 |Nelze nalézt uloženou proceduru ' *'. |
| 8144 |Procedura nebo funkce * má příliš mnoho zadaných argumentů. |

## <a name="next-steps"></a>Další kroky
Sledovat vaše doporučení a pokračuje v používání jejich Upřesnit výkonu. Databázové úlohy jsou dynamické a průběžně změnu. Poradce pro SQL Database i nadále monitorovat a poskytovat doporučení, které může zlepšit výkon vaší databáze. 

* V tématu [Azure SQL Database automatické ladění](sql-database-automatic-tuning.md) pro automatické ladění indexy databáze a plány provádění dotazů.
* V tématu [inteligentního Statistika SQL Azure](sql-database-intelligent-insights.md) automaticky monitorování výkonu databáze s automatizované diagnostiky a kořenový provést analýzu příčiny potíží s výkonem.
* V tématu [doporučení výkonu na portálu Azure](sql-database-advisor-portal.md) pokyny o tom, jak používat doporučení výkonu na portálu Azure.
* V tématu [Query Performance Insight](sql-database-query-performance.md) a další informace o zobrazení dopad na výkon nejčastějších dotazů.


