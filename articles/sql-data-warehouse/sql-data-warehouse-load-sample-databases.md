---
title: "Načtení ukázkových dat do SQL Data Warehouse | Microsoft Docs"
description: "Načtení ukázkových dat do SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: e338ecf8-cfee-419b-b7b6-98108d381c62
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 1e0df958a2f18fe1e988168918e5cfd293f84e64
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="load-sample-data-into-sql-data-warehouse"></a>Načtení ukázkových dat do SQL Data Warehouse
Postupujte podle těchto jednoduchých kroků se načíst a dotaz na databázi společnosti Adventure Works ukázka. Tyto skripty nejprve pomocí sqlcmd spustit SQL, která bude vytvářet tabulky a zobrazení. Po vytvoření tabulky skripty pomocí bcp načíst data.  Pokud ještě nemáte sqlcmd a bcp nainstalovaný, postupujte podle následujících odkazech na [nainstalovat bcp] [ install bcp] a [nainstalovat sqlcmd][install sqlcmd].

## <a name="load-sample-data"></a>Načíst ukázková data
1. Stažení [společnosti Adventure Works ukázkové skripty pro SQL Data Warehouse] [ Adventure Works Sample Scripts for SQL Data Warehouse] soubor zip.
2. Extrahujte soubory ze staženého zip do adresáře na místním počítači.
3. Upravte aw_create.bat extrahovaných souborů a nastavte následující proměnné v horní části souboru nalezen.  Ponechejte žádné mezery mezi "=" a parametr.  Níže jsou příklady, jak může vypadat provedené úpravy.
   
    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```
4. Do příkazového řádku systému Windows spusťte upravená aw_create.bat.  Ujistěte se, že jsou v adresáři, kam jste uložili vašeho upravenou verzi aw_create.bat.
   Tento skript bude...
   
   * Vyřaďte společnosti Adventure Works tabulky ani zobrazení, které již existují ve vaší databázi
   * Vytvoření společnosti Adventure Works tabulky a zobrazení
   * Načíst každá tabulka společnosti Adventure Works pomocí bcp
   * Ověření počtu řádků pro každou tabulku společnosti Adventure Works
   * Shromáždit statistiku pro každý sloupec pro každou tabulku společnosti Adventure Works

## <a name="query-sample-data"></a>Dotaz na ukázková data
Jakmile jste načíst ukázková data do SQL Data Warehouse, můžete rychle spustit pár dotazů.  Ke spuštění dotazu, připojit k nově vytvořenou databázi společnosti Adventure Works v Azure SQL DW pomocí sady Visual Studio a rozšíření SSDT, jak je popsáno v [dotazu pomocí sady Visual Studio] [ query with Visual Studio] dokumentu.

Příklad jednoduchého příkazu select se získat informace o zaměstnanci:

```sql
SELECT * FROM DimEmployee;
```

Příklad komplexnější dotaz pomocí konstruktory, jako jsou GROUP BY se podívat na celkovou velikost pro všechny prodeje na každý den:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Příklad SELECT s klauzulí WHERE filtrovat objednávky z do určitého data:

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

SQL Data Warehouse podporuje téměř všechny konstrukce T-SQL, které systém SQL Server podporuje.  Případné rozdíly jsou popsány v našem [migrace kódu] [ migrate code] dokumentaci.

## <a name="next-steps"></a>Další kroky
Teď, když vám, že možnost vyzkoušet několik dotazů s ukázkovými daty, podívejte se na postup [vyvíjet][develop], [načíst][load], nebo [ migrace] [ migrate] do SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[query with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[migrate code]: sql-data-warehouse-migrate-code.md
[install bcp]: sql-data-warehouse-load-with-bcp.md
[install sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works Sample Scripts for SQL Data Warehouse]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
