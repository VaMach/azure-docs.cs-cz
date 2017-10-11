---
title: "Použití zobrazení T-SQL v Azure SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro používání zobrazení jazyka Transact-SQL v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: b5208f32-8f4a-4056-8788-2adbb253d9fd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: d2a03be810bd7f792876607ec735eb578b65a3b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="views-in-sql-data-warehouse"></a>Zobrazení v SQL Data Warehouse
Zobrazení jsou obzvláště užitečná v SQL Data Warehouse. Použitím mnoha různými způsoby ke zlepšení kvality vašeho řešení.  V tomto článku klade důraz několik příkladů, jak zlepšit komunikaci oddělení řešení pomocí zobrazení a také omezení, které je potřeba zvážit.

> [!NOTE]
> Syntaxe pro `CREATE VIEW` není popsané v tomto článku. Podrobnosti najdete [vytvořit zobrazení] [ CREATE VIEW] článku na webu MSDN pro tuto referenční informace.
> 
> 

## <a name="architectural-abstraction"></a>Abstrakce architektury
Velmi běžný vzor aplikací je znovu vytvořit pomocí vytvoření tabulky AS vyberte funkce CTAS () a potom objektem přejmenování vzor, zatímco načítání dat tabulky.

Následující příklad přidá nové datum záznamy do dimenze kalendářního data. Všimněte si, jak je nové tabble, DimDate_New, nejprve vytvořit a pak přejmenovat nahradit původní verzi v tabulce.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Tento přístup může však způsobit tabulky zobrazování a ztrácejí ze zobrazení uživatele, jakož i "tabulka neexistuje" chybové zprávy. Zobrazení umožňuje uživatelům konzistentní prezentační vrstvou a přitom se základní objekty jsou přejmenovat. Tím, že poskytuje uživatelům přístup k datům prostřednictvím zobrazení, znamená, že uživatelé nemusí mít viditelnost základní tabulky. To nabízí konzistentní uživatelské prostředí, a zajistíte, že datový sklad Designer můžete vyvíjet datový model a maximalizovat výkon pomocí funkce CTAS během procesu načítání dat.    

## <a name="performance-optimization"></a>Optimalizace výkonu
Zobrazení, můžete použít také k vynucení výkonu optimalizované spojení mezi tabulkami. Například jako součást kritérií pro spojující minimalizovat přesun dat zobrazení začlenit redundantní distribučního klíče.  Další výhodou zobrazení může být vynutit spojující pomocný parametr nebo specifického dotazu. Použití zobrazení tímto způsobem zaručuje, že vždy spojení optimální způsobem vyloučení uživatelé pamatovat správné konstrukce pro jejich spojení.

## <a name="limitations"></a>Omezení
Zobrazení v SQL Data Warehouse jsou pouze metadata.  V důsledku toho nejsou k dispozici následující možnosti:

* Neexistuje žádná vazba možnost schématu
* Základní tabulky nelze aktualizovat prostřednictvím zobrazení
* Zobrazení nelze vytvořit přes dočasných tabulek
* Neexistuje žádná podpora pro ROZBALENÍ nebo NOEXPAND pomocné parametry
* Nejsou žádná indexované zobrazení v SQL Data Warehouse

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].
Pro `CREATE VIEW` syntaxe naleznete [vytvořit zobrazení][CREATE VIEW].

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CREATE VIEW]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
