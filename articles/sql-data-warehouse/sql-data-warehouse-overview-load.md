---
title: "Načtení dat do Azure SQL Data Warehouse | Microsoft Docs"
description: "Další běžné scénáře pro data načítání do SQL Data Warehouse. Patří sem pomocí PolyBase, úložiště objektů blob v Azure, plochých souborů a přesouvání disku. Můžete také použít nástroje třetích stran."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 2253bf46-cf72-4de7-85ce-f267494d55fa
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: c4199a387f5cdbd477a5e348e48ba8e8b5900075
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="load-data-into-azure-sql-data-warehouse"></a>Načtení dat do Azure SQL Data Warehouse
Přehled možností scénáře a doporučení pro načítání dat do SQL Data Warehouse.

Nejtěžší součástí načítání dat je obvykle Příprava zatížení data. Azure zjednodušuje načítání pomocí úložiště objektů blob v Azure jako běžné úložiště dat pro mnoho služeb, a pomocí Azure Data Factory k orchestraci přesouvání komunikace a dat mezi službami Azure. Tyto procesy jsou integrované s technologií PolyBase, který používá (MPP) massively parallel processing načíst data paralelně z Azure blob storage do SQL Data Warehouse. 

Podrobné pokyny, které načítají ukázkové databáze, najdete v části [načíst ukázkové databáze][Load sample databases].

## <a name="load-from-azure-blob-storage"></a>Načtení z Azure blob storage
Nejrychlejší způsob, jak importovat data do SQL Data Warehouse je pomocí funkce PolyBase načteme dat z Azure blob storage. PolyBase používá SQL Data Warehouse (MPP) massively parallel processing návrhu načíst data paralelně z Azure blob storage. Pokud chcete používat funkce PolyBase, můžete použít příkazy T-SQL nebo kanál služby Azure Data Factory.

### <a name="1-use-polybase-and-t-sql"></a>1. Použijte PolyBase a T-SQL
Souhrn procesu načtení:

1. Přesun dat do úložiště objektů blob v Azure nebo Azure Data Lake Store a uloží jej v textových souborů.
2. Nakonfigurujte externí objekty v SQL Data Warehouse zadat umístění a formát dat
3. Spusťte příkaz T-SQL chcete načíst data souběžně do nové tabulky databáze.

<!-- 5. Schedule and run a loading job. --> 

Podívejte se kurz [načtení dat z Azure blob storage do SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].

### <a name="2-use-azure-data-factory"></a>2. Použití Azure Data Factory
Pro jednodušší způsob, jak pomocí funkce PolyBase můžete vytvořit kanál Azure Data Factory, využívající funkci PolyBase k načtení dat z Azure blob storage do SQL Data Warehouse. Toto je rychlé konfigurace vzhledem k tomu, že nemusíte definovat objekty, T-SQL. Pokud potřebujete dotazu na externí data bez provedení importu, použijte T-SQL. 

Souhrn procesu načtení:

1. Přesunutí dat do Azure blob storage a uloží jej v textových souborů. Azure Data Factory aktuálně nepodporuje ADLS připojení pomocí funkce PolyBase).
2. Vytvořte kanál Azure Data Factory k ingestování data. Použijte možnost PolyBase.
4. Naplánování a spuštění kanálu.

Podívejte se kurz [načtení dat z Azure blob storage do SQL Data Warehouse (Azure Data Factory)][Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)].

## <a name="load-from-sql-server"></a>Načtení z SQL serveru
Načtení dat z SQL serveru do SQL Data Warehouse můžete Integration Services (SSIS) a přenos plochých souborů nebo disky lodě společnosti Microsoft. Pro čtení k najdete souhrn různými načítání procesy a odkazy na výukové programy.

Plánování migrace úplná data z SQL serveru do SQL Data Warehouse, najdete v článku [Přehled migrace][Migration overview]. 

### <a name="use-integration-services-ssis"></a>Použití integrační služby (SSIS)
Pokud už používáte balíčky Integration Services (SSIS) do systému SQL Server, můžete je aktualizovat balíčky serveru použít SQL Server jako zdroj a SQL Data Warehouse jako cíl. To je rychlé a snadné provést, a je vhodný, pokud se snažíte provést migraci váš proces načítání již používat data v cloudu. Kompromis je že zatížení bude nižší než pomocí PolyBase, protože tento SSIS neprovádí zatížení paralelně.

Souhrn procesu načtení:

1. Zkontrolovat, jestli vaše balíčku integračních služeb tak, aby odkazoval na instanci serveru SQL pro zdroj a databázi SQL Data Warehouse pro cíl.
2. Migrujte schéma do SQL Data Warehouse, pokud ještě není existuje.
3. Změňte mapování ve vašich balíčcích použít pouze typy dat, které jsou podporovány nástrojem SQL Data Warehouse.
4. Naplánování a spuštění balíčku.

Podívejte se kurz [načtení dat z SQL serveru Azure SQL Data Warehouse (SSIS)][Load data from SQL Server to Azure SQL Data Warehouse (SSIS)].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>Pomocí nástroje AZCopy (doporučeno pro < 10 TB dat)
Pokud má velikost dat < 10 TB, může exportu dat z SQL serveru do plochých souborů, zkopírujte soubory do Azure blob storage a pak pomocí funkce PolyBase načíst data do SQL Data Warehouse

Souhrn procesu načtení:

1. Pomocí nástroje příkazového řádku bcp k exportu dat z SQL serveru do plochých souborů.
2. Pomocí nástroje příkazového řádku AZCopy ke zkopírování dat z plochých souborů do úložiště objektů blob Azure.
3. Pomocí funkce PolyBase načteme do SQL Data Warehouse.

Podívejte se kurz [načtení dat z Azure blob storage do SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].

### <a name="use-bcp"></a>Pomocí bcp
Pokud máte malé množství dat můžete načíst přímo do Azure SQL Data Warehouse bcp.

Souhrn procesu načtení:

1. Pomocí nástroje příkazového řádku bcp k exportu dat z SQL serveru do plochých souborů.
2. Pomocí bcp k načtení dat z plochých souborů přímo do SQL Data Warehouse.

Podívejte se kurz [načtení dat z SQL serveru do Azure SQL Data Warehouse (bcp)][Load data from SQL Server to Azure SQL Data Warehouse (bcp)].

### <a name="use-importexport-recommended-for--10-tb-data"></a>Použití importu a exportu (doporučeno pro > 10 TB dat)
Pokud vaše velikost dat je > 10 TB a chcete ho přesunout do Azure, doporučujeme používat naše disku přesouvání služby [importu a exportu][Import/Export]. 

Souhrn procesu načtení

1. Pomocí nástroje příkazového řádku bcp k exportu dat z SQL serveru do plochých souborů na přenositelné disky.
2. Dodávat disky společnosti Microsoft.
3. Microsoft načte data do SQL Data Warehouse

## <a name="load-from-hdinsight"></a>Načtení z prostředí HDInsight
SQL Data Warehouse podporuje načítání dat z HDInsight prostřednictvím PolyBase. Proces je stejný jako načítání dat z Azure Blob Storage – pro připojení k HDInsight k načtení dat pomocí PolyBase. 

### <a name="1-use-polybase-and-t-sql"></a>1. Použijte PolyBase a T-SQL
Souhrn procesu načtení:

1. Přesunutí dat do HDInsight a uloží jej v textových souborů ORC nebo Parquet formátu.
2. Nakonfigurujte externí objekty v SQL Data Warehouse zadat umístění a formát data.
3. Spusťte příkaz T-SQL chcete načíst data souběžně do nové tabulky databáze.

Podívejte se kurz [načtení dat z Azure blob storage do SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].

## <a name="recommendations"></a>Doporučení
Mnoho našich partnerů má načítání řešení. Další informace, podívejte se do seznamu naše [partnery poskytujícími řešení][solution partners]. 

Pokud vaše data pochází z nerelační zdroje a chcete načíst do SQL Data Warehouse je nutné jej před načtením transformovat do řádků a sloupců. Transformovaná data nemusí být uložená v databázi, mohou být uloženy v textových souborů.

Vytvoření statistiky pro nově načtená data. Azure SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik.  Chcete-li získat nejlepší výkon ze své dotazy, je důležité vytvořit statistiku pro všechny sloupce všech tabulek po prvním načtením nebo dojít k významné změny v datech.  Podrobnosti najdete v tématu [statistiky][Statistics].

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj, najdete v článku [přehled vývoje][development overview].

<!--Image references-->

<!--Article references-->
[Load data from Azure blob storage to SQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Load data from SQL Server to Azure SQL Data Warehouse (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Load data from SQL Server to Azure SQL Data Warehouse (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Load sample databases]: ./sql-data-warehouse-load-sample-databases.md
[Migration overview]: ./sql-data-warehouse-overview-migrate.md
[solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Import/Export]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
