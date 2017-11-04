---
title: "Elastické dotazu konceptech Azure SQL Data Warehouse | Microsoft Docs"
description: "Elastické dotazu konceptech Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: e2dc8f3f-10e3-4589-a4e2-50c67dfcf67f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 09/18/2017
ms.author: elbutter
ms.openlocfilehash: 295cc59fdb23105534b4e7431902eaa720643330
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-elastic-query-with-sql-data-warehouse"></a>Použití elastické dotazu s SQL Data Warehouse



Elastické dotaz s Azure SQL Data Warehouse umožňuje zapisovat Transact-SQL v databázi SQL, které je odesláno vzdáleně instance Azure SQL Data Warehouse pomocí externí tabulky. Pomocí této funkce poskytuje úsporu nákladů a další původce architektury v závislosti na scénáři.

Tato funkce umožňuje dva základní scénáře:

1. Izolace domény
2. Spuštění vzdáleného dotazu

### <a name="domain-isolation"></a>Izolace domény

Izolace domény odkazuje na scénář classic datového tržiště. V některých scénářích jeden chtít poskytují logické domény dat pro příjem dat uživatele, kteří jsou izolované od zbytku datového skladu, pro z mnoha důvodů včetně, ale mimo jiné:

1. Izolaci prostředků – databáze SQL je optimalizovaná tak, aby poskytovat velké základní souběžných uživatelů obsluhující mírně odlišné úloh než velké analytické dotazy, které datového skladu je vyhrazený pro. Izolace zajistí, že správné zatížení zpracovává příslušné nástroje.
2. Izolace zabezpečení – jednotlivé autorizovaným data podmnožina selektivně prostřednictvím určité schémat.
3. Sandboxing - poskytují sadu ukázkových dat jako "playground" a prozkoumejte produkční dotazy atd.

Elastické dotaz může poskytovat umožňuje snadno vyberte podmnožiny dat serveru SQL datového skladu a přesuňte jej do instance databáze SQL. Kromě toho tato izolace nebrání možnost taky povolit spouštění vzdálený dotaz povolení pro scénáře zajímavějšího "mezipaměť".

### <a name="remote-query-execution"></a>Spuštění vzdáleného dotazu

Elastické dotazu umožňuje spuštění vzdáleného dotazu na instanci SQL data warehouse. Tím, že oddělíte horká a studená data mezi těmito dvěma databázemi jeden můžete využít nejlepší z databáze SQL a SQL data warehouse. Uživatele můžete ponechat novější data v databázi SQL, která může obsluhovat sestavy a velkého počtu průměrná podnikoví uživatelé. Ale je potřeba víc dat nebo výpočetní, uživatel může přenést část dotazu do instance datového skladu SQL, kde lze zpracovat rozsáhlé agregace mnohem rychlejší a efektivnější.



## <a name="elastic-query-overview"></a>Přehled elastické dotazu

Elastické dotazu je použít k datům umístěným v rámci SQL datového skladu k dispozici instance databáze SQL. Elastické dotazu umožňuje dotazy z databáze SQL odkazují na tabulky vzdálenou instanci SQL data warehouse. 

Prvním krokem je vytvoření definice zdroj externích dat, který odkazuje na instance SQL datového skladu, který používá existující přihlašovací údaje uživatele v SQL data warehouse. Žádné změny jsou nezbytné ve vzdálené instanci SQL data warehouse. 

> [!IMPORTANT] 
> 
> Musíte mít oprávnění ALTER ANY EXTERNAL DATA SOURCE. Toto oprávnění je součástí oprávnění ALTER DATABASE. Jsou potřeba oprávnění ALTER ANY externí zdroj dat k odkazování na vzdálené zdroje dat.

Dále vytvoříme definici vzdálené externí tabulky v instanci SQL databáze, která odkazuje na vzdálenou tabulku v SQL data warehouse. Použijete-li dotaz, který používá externí tabulku, část dotaz odkazující na externí tabulky posílá instance SQL datového skladu na zpracování. Po dokončení dotazu výsledné sady budou odeslána zpět do volání instance databáze SQL. Stručný kurzu nastavení dotaz elastické mezi SQL database a SQL data warehouse, najdete v článku [konfigurace elastické dotaz s SQL Data Warehouse][Configure Elastic Query with SQL Data Warehouse].

Další informace o elastické dotazu s databází SQL, najdete v článku [Azure SQL Database elastické dotazu přehled][Azure SQL Database elastic query overview].



## <a name="best-practices"></a>Osvědčené postupy

### <a name="general"></a>Obecné

- Při použití spuštění vzdáleného dotazu, ujistěte se, můžete pouze výběr nezbytné sloupce a použití správné filtrů. Nejen nemá toto zvýšení nezbytné výpočetní, ale taky zvyšuje velikost sady výsledků dotazu, a proto množství dat, který potřebujete přesunout mezi dvěma instancemi.
- Zachování dat pro analytické účely v SQL Data Warehouse a SQL Database v clusteru columnstore analytiIcal výkonu.
- Ujistěte se, že jsou zdrojové tabulky do několika oddílů pro přesun dotazu a data.
- Ujistěte se, použít jako mezipaměť instance databáze SQL jsou rozděleny do oddílů podrobnější aktualizace a snadnější správu povolit. 
- V ideálním případě použijte PremiumRS databáze, protože poskytují analytical výhod Clusterové columnstore indexování se zaměřením na úlohy náročné na vstupně-výstupní operace se slevou z databází Premium.
- Po zatížením využívat zatížení nebo datum účinnosti Identifikace sloupce pro upserts v instance databáze SQL k udržení integrity zdroj mezipaměti. 
- V instanci SQL data warehouse pro své SQL databáze vzdálené přihlašovací údaje definované ve zdroji dat. externí, vytvořte samostatné přihlášení a uživatele. 

### <a name="elastic-querying"></a>Elastické dotazování

- Interally v mezipaměti tabulky a externí tabulky existovat jako jiné objekty s instance databáze SQL. Zvažte vytvoření zobrazení v horní části uložené v mezipaměti v tabulce a externí tabulky, které sjednocení obě tabulky a použije filtry pro každou tabulku v bodě hranic.

  Představte si, že nám chcete zachovat poslední rok data v instanci databáze SQL. Máme dvě tabulky **ext. Objednávky**, který odkazuje na datový sklad řadí tabulky, a **dbo. Objednávky** představuje poslední za roky dat v rámci instance databáze SQL. Místo požádat uživatele, můžete rozhodnout, jestli dotaz jednu tabulku nebo jiné, můžeme vytvořit zobrazení v horní části obě tabulky v bodě oddílu posledního roku.

  ```sql
  CREATE VIEW dbo.Orders_Elastic AS
  SELECT 
    [col_a]     
  , [col_b]         
  , ...
  , [col_n]
  FROM
    [dbo].[Orders]
  WHERE 
    YEAR([o_orderdate]) >= '<Most Recent Year>'
  UNION
  SELECT 
    [col_a]     
  , [col_b]         
  , ...
  , [col_n]         
  FROM
    [ext].[Orders]
  WHERE
    YEAR([o_orderdate]) < '<Most Recent Year>'
  ```

  Zobrazení vytváří tak kompilátoru dotazu umožňuje určit, zda je nutné použít instance datového skladu k odpovědi na dotaz uživatele. 

  Je také odesílá, kompilace, spuštění a přesouvání dat spojené s každou elastické dotaz instance datového skladu. Být cognizant, že každý elastické dotaz započítává vaší souběžnosti sloty a používá prostředky.  


- Pokud jeden plánuje k podrobnostem další do sadu výsledků dotazu z instance datového skladu, zvažte vyhodnocování v dočasné tabulky v databázi SQL, výkonu a využití prostředků zbytečné.

### <a name="moving-data"></a>Přesun dat 

- Pokud je to možné zachovat data správy snadněji vyřeší pomocí připojovacího zdrojové tabulky tak, aby byly aktualizace snadno udržovatelný mezi instancemi datového skladu a databáze.
- Přesun dat na úrovni oddílu s vyprázdnění a vyplňte sémantiky Chcete-li minimalizovat náklady na dotaz na úroveň datového skladu a množství dat přesunout zachovat aktuální instanci databáze. 

### <a name="when-to-choose-azure-analysis-services-vs-sql-database"></a>Kdy použít Azure Analysis Services vs databáze SQL

#### <a name="azure-analysis-services"></a>Azure Analysis Services

- Máte v úmyslu používat vaše mezipaměť s BI nástroj, který odešle velké množství malých dotazy
- Třeba subsecond latence dotazu
- Máte zkušenosti ve správě nebo vývoj modely pro službu Analysis Services 

#### <a name="sql-database"></a>SQL Database

- Chcete dotazování na data mezipaměti s SQL
- Potřebujete vzdálené spuštění některých dotazů
- Máte větší požadavky mezipaměti



## <a name="faq"></a>Nejčastější dotazy

Otázka: je možné použít databází v rámci fondu elastické databáze pomocí dotazu elastické?

Odpověď: Ano. Databáze SQL v elastickém fondu pomocí elastické dotazu. 

Otázka: je limitu pro počet databází, které lze použít pro elastické dotaz?

Odpověď: logické servery mají omezení jednotek DTU zavedené zákazníkům zabránit náhodnému nákladů nad plán. Pokud chcete povolit několik databází pro elastický dotaz spolu s instanci SQL Data Warehouse, můžete narazit krytky neočekávaně. Pokud k tomu dojde, odešlete žádost o zvýšení limitu DTU na logickém serveru. Můžete zvýšit kvótu podle [vytvoření lístku podpory](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket) a výběrem *kvóty* jako typ požadavku

Otázka: je možné použít řádek úrovně zabezpečení nebo dynamické Data maskování s elastické dotazu?

Odpověď: Zákazníci, kteří jej nepřejete použít pokročilých funkcí zabezpečení SQL Database lze provést první přesunutím a ukládání dat v databázi SQL. Nemůžete použít aktuálně zabezpečení na úrovni řádků nebo DDM na dat získaných prostřednictvím externí tabulky. 

Otázka: je možné napsaný z instance Moje SQL databáze na instance datového skladu?

A: Tato funkce aktuálně není podporována. Navštivte naše [zpětné vazby stránky] [ Feedback page] k vytvoření nebo hlasování pro tuto funkci Pokud toto je funkce vám rádi viděli v budoucnu. 

Otázka: je možné používat prostorové typy jako geometrie nebo Geografie?

Odpověď: můžete uložit prostorové typy v SQL Data Warehouse jako hodnoty varbinary(max). Při dotazu tyto sloupce pomocí elastické dotazu, můžete je převést na odpovídající typy za běhu.

![prostorové typy](./media/sql-data-warehouse-elastic-query-with-sql-database/geometry-types.png)





<!--Image references-->

<!--Article references-->

[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop/
[Configure Elastic Query with SQL Data Warehouse]: ./tutorial-elastic-query-with-sql-datababase-and-sql-data-warehouse.md
[Feedback Page]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Azure SQL Database elastic query overview]: ../sql-database/sql-database-elastic-query-overview.md

<!--MSDN references-->

<!--Other Web references-->