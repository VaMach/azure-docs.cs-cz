---
title: "Přiřazení proměnné v SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro přiřazení proměnné jazyka Transact-SQL v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 81ddc7cf-a6ba-4585-91a3-b6ea50f49227
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 045d5148cd3f12dac63c961ccf7c953d355ed725
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="assign-variables-in-sql-data-warehouse"></a>Přiřazení proměnné v SQL Data Warehouse
Jsou nastaveny proměnné v SQL Data Warehouse pomocí `DECLARE` příkaz nebo `SET` příkaz.

Všechny tyto způsoby perfektně platný nastavte hodnotu proměnné:

## <a name="setting-variables-with-declare"></a>Nastavení proměnné s DECLARE
Inicializace proměnných s DECLARE je jedním z nejpružnější způsobů, jak nastavit hodnotu proměnné v SQL Data Warehouse.

```sql
DECLARE @v  int = 0
;
```

DECLARE můžete také nastavit více než jednu proměnnou najednou. Nemůžete použít `SELECT` nebo `UPDATE` k tomu:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Nelze inicializovat a použití proměnné v příkazu DECLARE stejné. Pro ilustraci bodem následující příklad je **není** povolena jako @p1 jak inicializovat a použít v příkazu DECLARE stejné. To bude mít za následek chybu.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Nastavení hodnot sadou
Sada je velmi běžné metody pro nastavení jednu proměnnou.

Všechny následující příklady jsou platné způsoby nastavení proměnné sadou:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Najednou můžete sadou nastavit pouze jednu proměnnou. Je ale, jak je vidět výše složené operátory povolená.

## <a name="limitations"></a>Omezení
Vyberte nebo aktualizaci nelze použít pro přiřazení proměnné.

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
