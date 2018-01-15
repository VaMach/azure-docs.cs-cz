---
title: "Tahák Azure SQL Data Warehouse | Microsoft Docs"
description: "Odkazy osvědčené postupy k vytvoření rychle řešení Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: acomet
manager: jhubbard
editor: 
ms.assetid: 51f1e444-9ef7-4e30-9a88-598946c45196
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 12/14/2017
ms.author: acomet
ms.openlocfilehash: 2d17385ff255ddf7b85baa81600a2af60d015540
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="cheat-sheet-for-azure-sql-data-warehouse"></a>Tahák pro Azure SQL Data Warehouse
Tato stránka by měly pomoci navrhnout pro hlavní použití případů řešení datového skladu. Tato tahák by měla být skvělým podpory v vám dobře slouží k vytvoření špičkových datového skladu, ale důrazně doporučujeme dozvědět další informace o jednotlivých kroků v podrobnostech. Nejprve doporučujeme přečtení tohoto článku skvělé o jaké SQL Data Warehouse  **[a není]**.

Následuje nákresu procesu, který byste měli postupovat při zahájení návrhu SQL Data Warehouse.

![Nákresu]

## <a name="queries-and-operations-across-tables"></a>Dotazy a operace mezi tabulkami

Je skutečně potřeba znát předem nejdůležitější operace a dotazy probíhající v datovém skladu. Architektura vašeho datového skladu by měla prioritu pro tyto operace. Může být běžných příkladů operací:
* Připojení jedno nebo dvě tabulky faktů s dimenzí tabulek, filtrování Tato tabulka pro určitou dobu a připojit výsledky do datového tržiště
* Provedení aktualizací velké nebo malé do vaší fakt prodeje
* Připojení k vaší tabulky jenom data.

Znalost, typ operace umožňuje optimalizovat návrh vaší tabulek.

## <a name="data-migration"></a>Migrace dat

Doporučujeme, abyste na prvním načtením dat  **[do ADLS]**  nebo úložiště objektů Blob Azure. Měli byste použít Polybase načíst data do SQL Data Warehouse v pracovní tabulce. Doporučujeme následující konfiguraci:

| Návrh | Doporučení |
|:--- |:--- |
| Distribuce | Kruhové dotazování. |
| Indexování | Haldy |
| Dělení | Žádné |
| Třída prostředku | largerc nebo xlargerc |

Další informace o  **[migrace dat], [načítání dat]**  s  **[podrobnější pokyny] na načítání**. 

## <a name="distributed-or-replicated-tables"></a>Distribuované nebo replikované tabulky

Doporučujeme následujících strategií v závislosti na její vlastnosti:

| Typ | Skvělé vyhovovat | Sledujte if...|
|:--- |:--- |:--- |
| Replikovat | • Tabulky malé dimenzí v hvězdicového schématu s menší než 2 GB úložiště po kompresi (komprese ~ 5 x) |• Mnoho transakce zápisu pro tabulku (např: vložit, upsert, aktualizace a odstranění)<br></br>• Změňte zřizování často datového skladu jednotky (DWU)<br></br>• Používáte pouze sloupce 2 – 3 a vaše tabulka má mnoho sloupců<br></br>• Indexu replikované tabulky |
| Kruhové dotazování (výchozí) | • Dočasných nebo pracovní tabulka<br></br> • Žádné zřejmé připojení sloupec klíče nebo dobrý candidate |• Pomalý výkon z důvodu přesunu dat. |
| Hash | Tabulky faktů •<br></br>• Velké dimenze tabulky |• Distribučního klíče nelze aktualizovat. |

**Tipy pro:**
* Začněte s kruhové dotazování, ale aspire pro strategie hash distribuce využívat výhod masivní paralelní architektura
* Ujistěte se, že společné klíče hash mají stejný formát dat
* Nemáte distribuovat na varchar formátu
* Tabulky dimenzí s běžné klíč algoritmu hash do tabulky faktů s operací spojování často může být distribuovat algoritmu hash
* Použití  *[sys.dm_pdw_nodes_db_partition_stats]*  k analýze všechny hodnoty v datech
* Použití  *[sys.dm_pdw_request_steps]*  k analýze dat pohybů za dotazy, monitorovat vysílání čas a náhodně operací trvat. Užitečné zkontrolovat strategie distribuce.

Další informace o  **[replikovaných tabulek] a [distribuované tabulky]**.

## <a name="indexing-your-table"></a>Indexování tabulku

Indexování je **skvělé** pro rychlé čtení tabulky. Je jedinečnou sadu technologií, které můžete použít na základě potřeb:

| Typ | Skvělé vyhovovat | Sledujte if...|
|:--- |:--- |:--- |
| Haldy | • Pracovní nebo dočasné tabulky<br></br>• Malé tabulky s malé hledání |• Žádné vyhledávání kontrol plném tabulka |
| Clusterovaný Index | • Až 100-m řádků tabulky<br></br>• Velké tabulky (více než 100-m řádky) s pouze 1 – 2 sloupce výraznou používají. |• Použít na replikované tabulky<br></br>• Složitých dotazů zahrnující více připojení, Group By operace<br></br>• Provést aktualizaci na indexovaného sloupce, jak dlouho trvá paměti |
| Clusterovaný Index Columnstore (KÚS) (výchozí) | • Velké tabulky (více než 100-m řádky) | • Použít na replikované tabulky<br></br>•, Které provedete masivní aktualizovat operací na tabulku<br></br>• Přepsání oddílu tabulku: skupiny řádků napříč uzly jiný distribuční a oddíly, které nezahrnují. |

**Tipy pro:**
* Nad clusterovaný Index můžete přidat do sloupce zatížen pro filtr Neclusterovaný Index. 
* Dávejte pozor, jak spravovat paměti v tabulce s KÚS. Při načítání dat, budete chtít uživatele (nebo dotaz) těžit z třídy velké prostředku. Můžete Ujistěte se, aby se zabránilo oříznutí a vytvoření skupin mnoho malých komprimované řádků
* Optimalizovaná pro výpočetní vrstvě skály s KÚS
* Pro KÚS nízký výkon může dojít v důsledku nízký komprese skupin řádků, můžete znovu sestavit nebo reorganizovat vaší KÚS. Chcete nejméně 100 tisíc řádků na jeden komprimovaný skupiny řádků. Ideální je 1-m řádků v skupiny řádků.
* Na základě frekvence přírůstkových zatížení a velikosti, chcete automatizovat při reorganizovat nebo opětovné sestavení indexů. Čištění pružiny je vždy užitečné.
* Být strategické Pokud chcete trim skupiny řádků: jak velká se o skupiny, otevřete řádek? Množství dat, které očekáváte načíst v následujících dnech?

Další informace o  **[indexy]**.

## <a name="partitioning"></a>Dělení
Vaše tabulka může oddílu, když máte velké fakt tabulky (> 1B řádek tabulky). 99 % případy, klíč oddílu by měla být založena na datum. Pečlivě není přečerpání oddílu, zejména v případě, že máte clusterovaný Columnstore Index.
S pracovních tabulek, které vyžadují ETL, můžete využívat výhod rozdělení do oddílů. Zařídí dat – Správa životního cyklu.
Dejte pozor, abyste overpartition vaše data, zejména u clusterovaný Columnstore Index.

Další informace o  **[oddíly]**.

## <a name="incremental-load"></a>Přírůstkové zatížení

Měli byste si nejdřív ověřit, že přidělíte větší třídy prostředků k načtení dat. Doporučujeme používat Polybase a ADF V2 pro automatizaci kanály ETL do datového skladu SQL.

Pro velké hromadné aktualizace ve vašich historických dat doporučujeme odstranění nejprve obavy data. Pak můžete použít hromadné vložení nová data. Tento krok 2 přístup je efektivnější.

## <a name="maintain-statistics"></a>Udržujte statistiky
Auto-statistics se chystáte brzo obecně dostupná. Do té doby se vyžaduje SQL Data Warehouse ruční údržbu statistiky. Je potřeba aktualizovat statistiku jako **významné** změny dojít k datům. To pomáhá Optimalizace plánu dotazu. Pokud zjistíte, že trvá příliš dlouho udržovat všechny statistické údaje, můžete být užší sloupce, které mají statistiky. Můžete také definovat frekvence aktualizace. Například můžete chtít denně aktualizovat sloupce s datem, do kterých se mohou přidávat nové hodnoty. Získáte tak, že statistiky na sloupce použité ve spojení, sloupce použité v klauzuli WHERE a sloupců v GROUP BY nalezen využívat výhod.

Další informace o  **[statistiky]**.

## <a name="resource-class"></a>Třída prostředků
SQL Data Warehouse používá skupiny prostředků jako způsob přidělení paměti pro dotazy. Pokud potřebujete více paměti pro zlepšení dotazu nebo načtení rychlost, měli byste přidělit vyšší třídy prostředků. Na straně překlopit pomocí větší třídy prostředků ovlivňuje souběžnosti. Chcete vzít v úvahu před přesunutím všichni uživatelé na třídu velké prostředků.

Pokud si všimnete, že dotazy trvá příliš dlouho, zkontrolujte, že uživatelé se nespustí v třídách velké prostředků. Třídy velké prostředků využívat mnoho sloty souběžnosti. Může dojít k jiné dotazy do fronty.

Nakonec pokud používáte úroveň výpočetní optimalizované, prostředků dostane každá třída 2,5 x větší spotřebu paměti než na vrstvě elastické optimalizované.

Další informace o tom, jak pracovat s  **[třídy prostředků a souběžnost]**.

## <a name="lower-your-cost"></a>Snížení nákladů na vaše
Klíčovou funkcí služby SQL Data Warehouse je schopnost pozastavit se, když ji zrovna nepoužíváte, a zastavit tak účtování výpočetních prostředků. Další klíčovou funkcí je schopnost škálovat prostředky. Pozastavení a škálování můžete provádět přes Azure Portal nebo prostřednictvím příkazů prostředí PowerShell.

Automatické škálování teď v době, je vhodné s Azure Functions:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

## <a name="optimize-you-architecture-for-performance"></a>Optimalizace je architektura pro výkon

Doporučujeme, abyste vzhledem k tomu SQL database a Azure Analysis Services v architektuře rozbočovače a koncových. Toto řešení může poskytnout zatížení izolaci mezi skupinami jiného uživatele při také využívat některé pokročilé funkce zabezpečení z databáze SQL a Azure Analysis Services. Toto je způsob, jak uživatelům poskytnout neomezený souběžnosti.

Další informace o  **[typické architektury využití SQL DW]**.

Nasazení v klikněte na vaše koncových v databázích SQL DB z datového skladu SQL:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwSpokeDbTemplate%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>


<!--Image references-->
[Nákresu]:media/sql-data-warehouse-cheat-sheet/picture-flow.png

<!--Article references-->
[načítání dat]:./design-elt-data-loading.md
[podrobnější pokyny]: ./guidance-for-loading-data.md
[indexy]:./sql-data-warehouse-tables-index.md
[oddíly]:./sql-data-warehouse-tables-partition.md
[statistiky]:./sql-data-warehouse-tables-statistics.md
[třídy prostředků a souběžnost]:./sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[typické architektury využití SQL DW]: https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/common-isv-application-patterns-using-azure-sql-data-warehouse/
[a není]:https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/azure-sql-data-warehouse-workload-patterns-and-anti-patterns/
[migrace dat]:https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
[replikovaných tabulek]:https://docs.microsoft.com/azure/sql-data-warehouse/design-guidance-for-replicated-tables
[distribuované tabulky]:https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute
[do ADLS]: https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-store
[sys.dm_pdw_nodes_db_partition_stats]: https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql
[sys.dm_pdw_request_steps]:https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql
