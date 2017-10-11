---
title: "Použití popisků na nástrojích dotazy v SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro používání popisky na nástrojích dotazy v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 44988de8-04c1-4fed-92be-e1935661a4e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 9e75bbe528a427724a623305fbd45e2277e9d0af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a>Použití popisků k nástroji dotazů v SQL Data Warehouse
SQL Data Warehouse podporuje koncept názvem popisky dotazu. Před přechodem do jakékoli hloubka umožňuje podívejte se na příklad jednoho:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Tento poslední řádek značky řetězec popisek Moje dotazu pro dotaz. To je zvláště užitečné, protože popisek je dotaz může prostřednictvím zobrazení dynamické správy. To poskytuje nám mechanismus sledovat problém dotazy a také k identifikaci průběh prostřednictvím spustit ETL.

Dobrý zásady vytváření názvů pomáhá skutečně sem. Například něco jako ' projektu: postup: příkaz: komentář se pomohou k jednoznačné identifikaci dotazu v mezi všechny kód ve správě zdrojového kódu.

Pokud chcete hledat podle popisku můžete použít následující dotaz, který používá zobrazení dynamické správy:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Je nezbytné při dotazování zabalení hranaté závorky a dvojité uvozovky kolem popisek aplikace word. Popisek je vyhrazené slovo a způsobilo chybu, pokud nebyla oddělené.
> 
> 

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
