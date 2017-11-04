---
title: "Dynamické SQL v SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro používání dynamických SQL v Azure SQL Data Warehouse pro vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: a948c2c3-3cd1-4373-90a9-79e59414b778
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 29228676373aee8dbc7b1b2a7d92ffc978333804
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="dynamic-sql-in-sql-data-warehouse"></a>Dynamické SQL v SQL Data Warehouse
Při vývoji aplikace kód pro SQL Data Warehouse možná budete muset použít dynamické sql které nám pomáhají doručovat flexibilní, obecné a modulární řešení. SQL Data Warehouse nepodporuje datové typy objektů blob v tuto chvíli. To může omezit velikost vašeho řetězce jako objekt blob typy patří typy varchar(max) a nvarchar(max). Pokud jste použili tyto typy v kódu aplikace při sestavování velké řetězce, musíte přerušit kód do bloků, a místo toho použít příkaz EXEC.

Jednoduchý příklad:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Pokud je řetězec krátké můžete použít [sp_executesql] [ sp_executesql] jako normální.

> [!NOTE]
> Příkazy provést, protože dynamické SQL budou platit stále všech ověřovacích pravidel TSQL.
> 
> 

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->
