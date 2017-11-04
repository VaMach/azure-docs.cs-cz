---
title: "Uložené procedury v SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro implementaci uložené procedury v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 9b238789-6efe-4820-bf77-5a5da2afa0e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: e42d80f0ca35f3fbb67389c66d072bc40d8a8d2c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="stored-procedures-in-sql-data-warehouse"></a>Uložené procedury v SQL Data Warehouse
SQL Data Warehouse podporuje mnoho funkcí jazyka Transact-SQL v SQL serveru. Je důležité nejsou specifické funkce, které bude chceme využít také k maximalizovat výkon vašeho řešení s více instancemi.

Zachování škálování a výkonu, služby SQL Data Warehouse jsou však také některé funkce a funkce, které mají rozdíly v chování a ostatní uživatele, které nejsou podporovány.

Tento článek vysvětluje, jak implementovat uložené procedury v rámci SQL Data Warehouse.

## <a name="introducing-stored-procedures"></a>Představení uložené procedury
Uložené procedury jsou vhodné pro zapouzdření kódu SQL; ukládání blízko data v datovém skladu. Zapouzdřením kód do spravovatelných jednotek uložené procedury pomoci vývojářům rozčlenění moduly svá řešení; usnadnění větší znovuvyužití kódu. Každý uložené procedury mohou také přijímat parametry tak, aby byly i flexibilnější.

SQL Data Warehouse poskytuje zjednodušenou a efektivní uložené procedury implementaci. Největší rozdíl oproti systému SQL Server je, že uložená procedura není předem zkompilovaný kód. V datové sklady jsou obecně menší význam s časem kompilace. Je důležité kód uložené procedury je při fungování proti velkých objemů dat správně optimalizované. Cílem je uložit hodiny, minuty a sekundy není milisekundách. Je proto více vhodné zamyslet nad uložené procedury jako kontejnery pro logiku SQL.     

Když SQL Data Warehouse provede uložené procedury jsou analyzovat příkazy SQL, přeložených a optimalizované za běhu. Během tohoto procesu je každý příkaz převeden na distribuované dotazy. Kód SQL, který je ve skutečnosti spustit pro data se liší na dotaz odeslána.

## <a name="nesting-stored-procedures"></a>Vnoření uložené procedury
Uložené procedury při volání jiné uložené procedury nebo pak vnitřní uložená procedura nebo kód volání říká, že je možné provést dynamické sql.

SQL Data Warehouse podporují maximálně 8 vnořených úrovní. To se mírně liší k systému SQL Server. Úroveň vnoření v systému SQL Server je 32.

Volání uložené procedury nejvyšší úrovně se rovná vnořit úroveň 1

```sql
EXEC prc_nesting
```
Pokud uložená procedura také provede další EXEC volání pak to zvýší úroveň vnoření 2

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Pokud se druhý postup pak provede některé dynamické sql, pak to zvýší úroveň vnoření na 3

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Poznámka: SQL Data Warehouse nepodporuje aktuálně@NESTLEVEL. Musíte mít nainstalované sledovat vaše úrovně vnoření. Nepravděpodobné, se setkají limit úrovně vnoření 8, ale v takovém případě budete muset znovu fungovat kódu a "zploštění" jej tak, aby odpovídal v rámci tohoto limitu.

## <a name="insertexecute"></a>PŘÍKAZ INSERT... SPUŠTĚNÍ
SQL Data Warehouse nepovoluje využívat sady výsledků dotazu uložené proceduře pomocí příkazu INSERT. Je však alternativní způsob, který můžete použít.

Naleznete v následujícím článku na [dočasných tabulek] příklad o tom, jak to udělat.

## <a name="limitations"></a>Omezení
Existují některé aspekty Transact-SQL uložené procedury, které nejsou implementované v SQL Data Warehouse.

Jsou:

* dočasně uložených procedur
* číslované uložené procedury
* Rozšířené uložené procedury
* CLR uložené procedury
* možnost šifrování
* možnost replikace
* Parametry s hodnotou tabulky
* parametry jen pro čtení
* výchozí parametry
* kontexty provádění
* Return – příkaz

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].

<!--Image references-->

<!--Article references-->
[dočasných tabulek]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
