---
title: Distribuce tabulek v SQL Data Warehouse | Microsoft Docs
description: "Začínáme s distribuci tabulek v Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 5ed4337f-7262-4ef6-8fd6-1809ce9634fc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 12/06/2017
ms.author: barbkess
ms.openlocfilehash: 82e17e575cdb227af2fabf94f01e94df22994aac
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="distributing-tables-in-sql-data-warehouse"></a>Distribuce tabulek v SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Přehled][Overview]
> * [Datové typy][Data Types]
> * [Distribuce][Distribute]
> * [Index][Index]
> * [Oddíl][Partition]
> * [Statistiky][Statistics]
> * [Dočasné][Temporary]
>
>

Řešení SQL Data Warehouse je distribuovaný databázový systém, postavený na architektuře MPP (Massively Parallel Processing).  SQL Data Warehouse nabízí jedinečnou škálovatelnost, daleko za možnostmi jediného systému, a to díky dělení dat a schopnosti zpracování napříč více uzly.  Rozhodování, jak se bude distribuovat svá data do SQL Data Warehouse je jedním z nejdůležitějších faktorů k dosažení optimálního výkonu.   Klíč pro optimální výkon je minimalizaci přesun dat a pak klíč k minimalizaci přesun dat je výběr správné distribuční strategie.

## <a name="understanding-data-movement"></a>Princip přesunu dat
V systému MPP data z tabulek rozděluje do několika základní databáze.  Většina optimalizované dotazy v systému MPP můžete jednoduše předána ke spouštění na jednotlivé distribuované databáze bez jakéhokoli zásahu mezi ostatní databáze.  Řekněme například, že máte databázi s prodejní data, která obsahuje dvě tabulky, prodej a zákazníků.  Pokud máte dotaz, který potřebuje ke spojení vaší prodeje tabulky pro tabulku zákazníků a budete provádět dělení prodeje a tabulky zákazníka až po zákaznické číslo uvedení každého zákazníka a to v samostatné databáze, bude možné v rámci každou databázi bez znalosti ostatní databáze vyřešit všechny dotazy, které připojení prodeje a zákazníka.  Naproti tomu Pokud vaše data prodeje dělený pořadové číslo a zákaznických údajů číslem zákazníka, pak všechny danou databázi nebude mít odpovídající data pro každého zákazníka a proto pokud jste chtěli připojit vaše data prodeje k zákaznická data, potřebujete získat data pro každého zákazníka a to z jiné databáze.  V tomto příkladu druhý přesun dat potřebovat proběhnout pro přesun dat zákazníka do data z prodeje, takže lze propojit dvě tabulky.  

Přesun dat není vždy chybný věcí, někdy je nezbytné k vyřešení dotazu.  Ale když tento další krok se vyhnout, přirozeně dotazu bude pracovat rychleji.  Přesun dat nejčastěji mohou nastat, pokud jsou připojené k tabulky nebo agregace se provádí.  Často je potřeba udělat i, takže když bude pravděpodobně možné optimalizovat pro jeden scénář, jako jsou spojení, budete potřebovat přesun dat, které vám pomůžou vyřešit v situaci, jako je agregace.  Efektu je přijít na to, což je méně práce.  Ve většině případů je distribuce tabulky faktů velký na běžně připojené k sloupci co nejúčinnější metodu pro snížení většina přesun dat.  Distribuci dat na sloupce spojení je mnohem víc běžnou metodu omezit přesun dat než distribuci dat na sloupce, které se účastní agregace.

## <a name="select-distribution-method"></a>Vyberte možnost distribution – metoda
SQL Data Warehouse na pozadí rozděluje data do 60 databáze.  Každé jednotlivé databáze odkazuje jako **distribuční**.  Při načítání dat do tabulek SQL Data Warehouse musí vědět, jak k rozdělení dat mezi těmito 60 distribuce.  

Metoda distribuční definovanou na úrovni tabulky a aktuálně existují dvě možnosti:

1. **Kruhové dotazování** který distribuci dat rovnoměrně ale náhodně.
2. **Hodnoty hash distribuované** která distribuuje data podle algoritmu hash hodnoty z jednoho sloupce

Ve výchozím nastavení, když nejsou definovány způsobu distribuce dat, tabulka budou distribuována pomocí **kruhové dotazování** na metodu.  Ale protože je stále sofistikovanější v implementaci, budete chtít zvažte použití **hash distribuované** dotaz tabulky minimalizovat přesun dat, která bude zase optimalizace výkonu.

### <a name="round-robin-tables"></a>Kruhové dotazování tabulky
Pomocí metody kruhové dotazování distribuci dat je velmi dobře v tom, jak ho vyznívá.  Podle vašich dat je načtena, každý řádek jednoduše posílá další distribuci.  Tato metoda distribuci dat bude vždy náhodně distribuovat data velmi rovnoměrně napříč všemi z rozdělení.  To znamená, že neexistuje žádné řazení provést během procesu kruhové dotazování, který nahradí vaše data.  Kruhové dotazování distribuční se někdy nazývá náhodných hash z tohoto důvodu.  S tabulkou distribuované kruhového dotazování je potřeba pochopit data.  Z tohoto důvodu kruhového dotazování tabulky často je dobré načítání cíle.

Ve výchozím nastavení pokud je vybrána žádná metoda distribuce, metodu distribuce kruhové dotazování se použije.  Při kruhové dotazování tabulky lze snadno použít, protože data se náhodně distribuuje do systému, které znamená, že systém nemůže zaručit, který distribuční každý řádek je však na.  V důsledku toho systém někdy potřebuje k vyvolání operace přesunu dat lepší uspořádání dat před překlad dotazu.  Tento krok navíc může zpomalit své dotazy.

Zvažte použití kruhové dotazování distribuce pro tabulku v následujících scénářích:

* Pro začátek jednoduché počáteční bod
* Pokud neexistuje zřejmé spojující klíč
* Pokud není k dispozici vhodným kandidátem sloupce pro hodnoty hash distribuce tabulky
* Pokud v tabulce nesdílí společný klíč spojení s jinými tabulkami
* Pokud je méně důležité než jiné spojení v dotazu spojení
* Pokud je tabulka dočasné pracovní tabulky

Obě tyto příklady vytvoří tabulku, kruhové dotazování:

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [!NOTE]
> Pokud kruhové dotazování je, že výchozí typ tabulky se explicitní v DDL je považovat za osvědčený postup, aby byly vymazat jiným záměry rozložení tabulky.
>
>

### <a name="hash-distributed-tables"></a>Hodnoty hash distribuované tabulky
Použití **Hash distribuované** algoritmus distribuovat vaše tabulky můžete zlepšit výkon pro mnoho scénářů snížením přesun dat v době dotazu.  Hodnota hash distribuované tabulky jsou tabulky, které jsou rozděleny mezi distribuované databáze pomocí algoritmu hash na jeden sloupec, který jste vybrali.  Sloupec distribuce je co určuje, jak je rozdělit data do distribuované databáze.  Funkce hash používá sloupec distribuční přiřadit distribuce řádků.  Algoritmus hash a výsledný distribuce je deterministický.  To je, bude má vždy stejnou hodnotu stejného typu dat do stejné distribuce.    

Tento příklad vytvoří tabulku rozdělit na id:

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a>Vybrat distribuční sloupce
Pokud zvolíte **distribuovat algoritmu hash** tabulku, budete muset vybrat jednoho distribučního sloupec.  Když vyberete distribuční sloupce, existují tři hlavní faktory vzít v úvahu.  

Vyberte jeden sloupec, který bude:

1. Není možné aktualizovat
2. Distribuci dat rovnoměrně, zabraňující zkosení dat
3. Minimalizovat přesun dat

### <a name="select-distribution-column-which-will-not-be-updated"></a>Vybrat distribuční sloupec, který nebude aktualizován
Distribuční sloupce nejsou aktualizovat, proto, vyberte sloupec s statické hodnoty.  Pokud sloupec bude potřeba aktualizovat, není obecně správné distribuční candidate.  Pokud je případ, kde je nutné aktualizovat distribuční sloupce, můžete to provést nejprve odstranit řádek a následného vložení nového řádku.

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a>Vybrat distribuční sloupec, který bude rovnoměrně distribuovat data
Vzhledem k tomu, že distribuovaného systému provádí pouze rychlé jako jeho nejpomalejší distribuci, je důležité rozdělte práci rovnoměrně mezi distribucí dosáhnout vyrovnáváním provádění v systému.  Způsob práce je rozdělena na distribuovaného systému je založena na tam, kde data pro každý distribuční platný.  Díky tomu je velmi důležité vybrat sloupci vpravo distribuce pro distribuci dat tak, aby každý distribuční má stejnou práci a bude trvat stejnou dobu pro dokončení jeho část práce.  Při práci a rozděluje do systému, dat jsou rovnoměrně mezi distribucí.  Když data není vyrovnáváním rovnoměrně, říkáme to **data zkosení**.  

Rozdělení dat rovnoměrně a zamezit tak data zkosení, zvažte tyto informace při výběru vaší distribuční sloupce:

1. Vyberte sloupce, který obsahuje velký počet jedinečných hodnot.
2. Vyhněte se distribuci dat na sloupce s několika jedinečných hodnot.
3. Vyhněte se distribuci dat na sloupce s vysoká frekvence hodnot Null.
4. Vyhněte se distribuci dat na sloupců s kalendářními daty.

Vzhledem k tomu, že každá hodnota se rozdělí na 1 60 distribuce a zajistit rovnoměrné rozdělení budete chtít vyberte sloupec, který je vysoce jedinečný a obsahuje více než 60 jedinečné hodnoty.  Pro ilustraci, představte si případ, kdy sloupec má jenom 40 jedinečné hodnoty.  Pokud v tomto sloupci jste vybrali jako distribučního klíče, data pro tuto tabulku by nebude zobrazovat na 40 distribuce maximálně ponechat 20 distribuce se žádná data a žádné zpracování udělat.  Naopak dalších 40 distribuce by měla mít více práce k tomu, pokud data byla rovnoměrně šířit přes 60 distribuce.  Tento scénář je příkladem zkosení data.

V systému MPP každého kroku dotazu čeká všechny distribuce pro dokončení jejich podíl práce.  Pokud do jednoho distribučního provádí další práci než ostatní, pak prostředku jiných distribuce jsou v podstatě ke znehodnocení části právě čekání na zaneprázdněn rozdělení.  Při práci není rovnoměrně rozloženy všechny distribuce, říkáme to **zpracování zkosení**.  Zpracování zkosení způsobí, že dotazy na pomalejší, než pokud zatížení může být rovnoměrně rozloženy distribucí.  Data zkosení povede k zkosení zpracování.

Vyhněte se distribuci na vysoce sloupec jako hodnoty null bude zobrazovat na stejné rozdělení. Distribuce ve sloupci Datum může také způsobit zkosení zpracování, protože všechna data pro konkrétního data bude zobrazovat na stejný distribuční. Pokud několik uživatelů provádí dotazy veškeré filtrování ve stejný den, pak bude to jenom 1 60 distribuce, všechny práce od zadaného data bude pouze na jednom distribučním. V tomto scénáři bude spuštění dotazů pravděpodobně 60 x pomalejší než pokud data byly rovnoměrně rozloženy všechny z rozdělení.

Pokud neexistují žádné sloupce vhodným kandidátem, můžete použít kruhové dotazování jako metodu distribuce.

### <a name="select-distribution-column-which-will-minimize-data-movement"></a>Vybrat distribuční sloupec, který bude minimalizovat přesun dat
Minimalizace přesun dat výběrem sloupci vpravo distribuce je jedním z nejdůležitějších strategie pro optimalizaci výkonu SQL Data Warehouse.  Přesun dat nejčastěji mohou nastat, pokud jsou připojené k tabulky nebo agregace se provádí.  Sloupce použité v `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` a `HAVING` klauzule pro všechny provedl **dobrý** hash kandidáty distribuce.

Na druhé straně sloupců v `WHERE` klauzule proveďte **není** provést pro sloupec kandidátů na dobrý hash vzhledem k tomu, že omezit, které distribuce účast v dotazu, která způsobila zpracování zkosení.  Dobrým příkladem sloupce, který může být tempting distribuovat na, ale často může způsobit zpracování zkosení je sloupec datum.

Obecně platí Pokud máte dvě tabulky faktů velké často zahrnutých ve spojení, bude získáte většina výkon distribucí obě tabulky v jednom ze sloupce spojení.  Pokud máte tabulku, která je nikdy připojený k jiné tabulky faktů velký, podívejte se na sloupce, které jsou často v `GROUP BY` klauzule.

Existuje několik klíčů kritérií, které musí splnit, aby se zabránilo přesun dat během spojení:

1. Příslušné tabulky ve spojení musí být hodnota hash rozdělit na **jeden** sloupců podílejících se připojení.
2. Datové typy sloupce spojení se musí shodovat mezi obě tabulky.
3. Sloupce musí být připojené pomocí operátoru rovná se.
4. Typ spojení nesmí být `CROSS JOIN`.

## <a name="troubleshooting-data-skew"></a>Řešení potíží s zkosení dat
Při distribuci dat tabulky pomocí metody distribuce hash existuje pravděpodobnost, že bude nesouměrně rozdělí některých distribucích nepřiměřeně více dat než jiné. Zkosení nadměrné dat může ovlivnit výkon dotazů, protože konečný výsledek distribuovaného dotazu musí čekat na nejdéle probíhající distribuce ukončíte. V závislosti na stupeň zkosení data možná budete muset vyřešit ji.

### <a name="identifying-skew"></a>Identifikace zkosení
Jednoduchý způsob, jak identifikovat tabulku jako nesouměrně rozdělí je použití `DBCC PDW_SHOWSPACEUSED`.  Toto je velmi rychlý a jednoduchý způsob, jak zobrazit počet řádků tabulky, které jsou uložené v každé z 60 distribuce vaší databáze.  Nezapomeňte, že většina vyrovnáváním výkonu, řádky v tabulce distribuované by měl být rovnoměrně rozloženy všechny distribuce.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Pokud je dotaz na zobrazení dynamické správy Azure SQL Data Warehouse (DMV) však můžete provádět více podrobné analýzy.  Pokud chcete spustit, vytvořte zobrazení [dbo.vTableSizes] [ dbo.vTableSizes] zobrazit pomocí SQL z [tabulky přehled] [ Overview] článku.  Po vytvoření zobrazení, spusťte tento dotaz k identifikaci, které tabulky mít víc než 10 % data zkosení.

```sql
select *
from dbo.vTableSizes
where two_part_name in
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a>Řešení zkosení dat
Ne všechny zkosení je dost pro vlastní opravu.  V některých případech může výkon tabulky v některé dotazy převáží nad škodu dat zkosení.  Rozhodnout, pokud byste měli vyřešit dat zkosení v tabulce, měli byste se seznámit co nejvíce o datové svazky a dotazy v vaše úlohy.   Jedním ze způsobů podívat se na dopad zkosení je postupujte podle kroků v [dotazu monitorování] [ Query Monitoring] článek k monitorování dopad na výkon dotazů zkosení a konkrétně dopad na jak dlouho dotazuje trvat dokončení pro jednotlivé distribuce.

Distribuci dat je řádu rovnováhu mezi minimalizaci zkosení dat a současně minimalizujete její přesun dat hledání. To může být proti cíle a někdy budete chtít zachovat data zkosení kvůli snížení přesun dat. Například pokud sloupci distribuce je často sloupci sdílené v spojování a agregaci, můžete se být minimalizace přesun dat. Výhodou vytvoření přesun minimální dat může převažují nad důsledky toho, že data zkreslit.

Typické způsob řešení zkosení dat je znovu vytvořit tabulku se sloupcem jiný distribuční. Vzhledem k tomu, že neexistuje žádný způsob, jak změnit distribuční sloupce na existující tabulky, způsob, jak změnit rozdělení tabulky se ho znovu vytvořit s [] [funkce CTAS].  Zde jsou dva příklady, jak vyřešit data zkosení:

### <a name="example-1-re-create-the-table-with-a-new-distribution-column"></a>Příklad 1: Opětovným vytvořením tabulky se sloupcem nové distribuce
Tento příklad používá [] – [funkce CTAS] a znovu vytvořte tabulku se sloupcem distribuční různé hodnoty hash.

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

### <a name="example-2-re-create-the-table-using-round-robin-distribution"></a>Příklad 2: Opětovným vytvořením tabulky pomocí distribuční kruhové dotazování
Tento příklad používá [] – [funkce CTAS] a znovu vytvořte tabulku s každým místo hash distribuce. Tato změna způsobí i distribuci dat za cenu přesun dat vyšší.

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## <a name="next-steps"></a>Další kroky
Další informace o návrh tabulky, najdete v článku [distribuovat][Distribute], [Index][Index], [oddílu][Partition], [datové typy][Data Types], [statistiky] [ Statistics] a [dočasných tabulek] [ Temporary] články.

Přehled osvědčených postupů najdete v tématu [SQL Data Warehouse osvědčené postupy][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md
[Query Monitoring]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#table-size-queries

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
