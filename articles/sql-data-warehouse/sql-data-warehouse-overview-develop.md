---
title: "Prostředky pro vývoj datového skladu v Azure | Microsoft Docs"
description: "Vývoj koncepty, rozhodnutí o návrhu, doporučení a kódování techniky pro SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: barbkess
editor: 
ms.assetid: 996e3afc-c21c-4e21-b9df-997f953f6dfd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: develop
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: b85a4f09e561e429aa5bf46ec680014487fb40c7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Rozhodnutí o návrhu a kódování techniky pro SQL Data Warehouse
Prohlédněte si prostřednictvím tyto články vývoj, abyste lépe pochopili rozhodnutí o návrhu klíče, doporučení a kódování techniky pro SQL Data Warehouse.

## <a name="key-design-decisions"></a>Rozhodnutí o návrhu klíče
V následujících článcích zvýrazněte některé klíčové koncepty a rozhodnutí o návrhu, které je třeba porozumět pro vývoj distribuované datového skladu pomocí SQL Data Warehouse:

* [připojení][connections]
* [souběžnosti][concurrency]
* [transakce][transactions]
* [uživatelem definované schémata][user-defined schemas]
* [distribuce tabulky][table distribution]
* [indexy tabulek][table indexes]
* [Tabulka oddílů][table partitions]
* [FUNKCE CTAS][CTAS]
* [statistiky][statistics]

## <a name="development-recommendations-and-coding-techniques"></a>Doporučení pro vývoj a techniky kódování
Tyto články zvýraznit konkrétní kódování techniky, tipy a doporučení pro vývoj SQL Data Warehouse:

* [uložené procedury][stored procedures]
* [popisky][labels]
* [zobrazení][views]
* [dočasné tabulky][temporary tables]
* [dynamické SQL][dynamic SQL]
* [ve smyčce][looping]
* [Seskupit podle možností][group by options]
* [přiřazení proměnné][variable assignment]

## <a name="next-steps"></a>Další kroky
Jakmile jste prostřednictvím články vývoj prohlédněte prostřednictvím [referenční dokumentace jazyka Transact-SQL] [ Transact-SQL reference] stránka Další informace o podporovaných syntaxe pro SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[concurrency]: ./sql-data-warehouse-develop-concurrency.md
[connections]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dynamic SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[group by options]: ./sql-data-warehouse-develop-group-by-options.md
[labels]: ./sql-data-warehouse-develop-label.md
[looping]: ./sql-data-warehouse-develop-loops.md
[statistics]: ./sql-data-warehouse-tables-statistics.md
[stored procedures]: ./sql-data-warehouse-develop-stored-procedures.md
[table distribution]: ./sql-data-warehouse-tables-distribute.md
[table indexes]: ./sql-data-warehouse-tables-index.md
[table partitions]: ./sql-data-warehouse-tables-partition.md
[temporary tables]: ./sql-data-warehouse-tables-temporary.md
[transactions]: ./sql-data-warehouse-develop-transactions.md
[user-defined schemas]: ./sql-data-warehouse-develop-user-defined-schemas.md
[variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[views]: ./sql-data-warehouse-develop-views.md
[Transact-SQL reference]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
