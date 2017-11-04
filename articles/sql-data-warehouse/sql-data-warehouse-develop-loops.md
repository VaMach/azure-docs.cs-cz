---
title: "Využít smyčky T-SQL v Azure SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro smyčky Transact-SQL a nahraďte kurzory v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f3384b81-b943-431b-bc73-90e47e4c195f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 40a872ff310f48bfd543ac184fe7301b85b50258
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="loops-in-sql-data-warehouse"></a>Smyčky v SQL Data Warehouse
Podporuje SQL Data Warehouse [při][při] smyčky pro opakovaně provádění blok příkazu. To bude pokračovat pro tak dlouho, dokud zadané podmínky splněny, nebo dokud se kód konkrétně ukončí, pomocí smyčky `BREAK` – klíčové slovo. Smyčky jsou obzvláště užitečná pro nahrazení kurzory definované v kódu SQL. Naštěstí téměř všechny kurzory, které jsou zapsány v kódu SQL jsou rychloposuv vpřed číst pouze řady. Proto [při] smyčky jsou skvělý alternativní, pokud se přistihnete museli nahradit jednu.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Využití smyčky a nahrazení kurzory v SQL Data Warehouse
Ale, než začnete v head nejdřív měli byste požádat sami následující otázku: "Tento kurzor lze znovu zapsat používat sady na základě operace?". V mnoha případech odpověď bude Ano a je často nejlepším přístupem. Operace set na základě často provádí výrazně rychlejší než přístup iterativní, řádek po řádku.

Rychloposuv vpřed kurzory jen pro čtení můžete snadno nahradit opakování konstrukce. Níže je jednoduchý příklad. Tento příklad kódu aktualizuje statistiku pro každou tabulku v databázi. Iterování přes tabulky v smyčky jsme jsou mohou ke spuštění každého příkazu v pořadí.

Nejprve vytvořte dočasnou tabulku obsahující řádek jedinečné číslo, které používají k identifikaci jednotlivých příkazy:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Druhý inicializace proměnné potřebná k provedení smyčka:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Nyní smyčku příkazy provádění jeden současně:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Nakonec vyřaďte dočasné tabulky vytvořené v prvním kroku

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[při]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
