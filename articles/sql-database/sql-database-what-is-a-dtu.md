<properties
    pageTitle="SQL Database: Co je DTU? | Microsoft Azure"
    description="Vysvětlení jednotky transakce Azure SQL Database."
    keywords="možnosti databáze, výkon databáze"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="NA"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# Vysvětlení jednotek databázové transakce (DTU) a elastických jednotek databázové transakce (eDTU)

Tento článek vysvětluje jednotky databázové transakce (DTU) a elastické jednotky databázové transakce (eDTU). Popisuje také, co se stane při dosažení maximálního počtu DTU nebo eDTU.  

## Co jsou jednotky databázové transakce (DTU)

DTU je jednotka měření pro prostředky, u kterých je zaručena dostupnost pro samostatnou databázi Azure SQL na konkrétní úrovni výkonu v rámci [úrovně služeb pro samostatné databáze](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels). Jednotka DTU představuje kombinaci měření vstupu/výstupu pro procesor, paměť a data a vstupu/výstupu protokolu transakcí v poměru určeném úlohou srovnávacího testu OLTP, která je navržena jako typická pro úlohy OLTP v reálném světě. Zdvojnásobení jednotek DTU zvýšením úrovně výkonu databáze odpovídá zdvojnásobení sady prostředků, které jsou pro příslušnou databázi k dispozici. Například databáze Premium P11 se 1 750 DTU nabízí 350x více DTU výpočetního výkonu než databáze Basic s 5 DTU. Chcete-li lépe pochopit metodiku používanou úlohou srovnávacího testu OLTP k určení kombinace pro jednotku DTU, přečtěte si článek [Přehled srovnávacích testů SQL Database](sql-database-benchmark-overview.md).

![Úvod do služby SQL Database: DTU izolované databáze podle úrovní](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

[Změnit úrovně služeb](sql-database-scale-up.md) můžete kdykoli s minimální odstávkou aplikace (obecně v průměru méně než čtyři sekundy). Mnoha firmám a aplikacím stačí vytváření databází a nastavování výkonu izolovaných databází na vyžádání, zejména v případě, že jsou vzorce používání relativně předvídatelné. Ale pokud vaše vzorce používání předvídatelné nejsou, může být správa nákladů a údržba obchodního modelu velmi těžká. Pro tento scénář použijete elastický fond s určitým počtem jednotek eDTU.

## Co jsou elastické jednotky databázové transakce (eDTU)

eDTU je jednotka měření sady prostředků (DTU), které mohou být sdíleny v rámci sady databází na serveru Azure SQL – nazývá se [elastický fond](sql-database-elastic-pool.png). Elastické fondy poskytují jednoduché a nákladově efektivní řešení správy výkonnostních cílů pro více databází, jejichž vzory používání se značně liší a nelze je předvídat. Další informace najdete v popisu [elastických fondů a úrovní služeb](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus).

![Úvod do služby SQL Database: eDTU podle vrstvy a úrovně](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Fond obdrží stanovený počet eDTU za stanovenou cenu. V rámci fondu disponují jednotlivé databáze flexibilní možností automatického škálování v rámci stanovených parametrů. V případě velkého zatížení může databáze spotřebovávat více eDTU, aby splnila požadavky. Databáze při nízkém zatížení spotřebovávají méně eDTU a databáze bez zatížení nespotřebovávají žádné eDTU. Zřizováním prostředků pro celý fond, a nikoli pro jednotlivé databáze, se úkoly správy zjednodušují. Kromě toho můžete předem odhadnout náklady na fond.

Další eDTU lze do existujícího fondu přidat bez jakéhokoli výpadku databáze a bez jakéhokoli vlivu na databáze v elastickém fondu. Podobně platí, že pokud již přidané eDTU nejsou potřebné, lze je z existujícího fondu kdykoli odebrat. Databáze můžete do fondu přidávat nebo je z něj odebírat. Pokud databáze podle předpokladu nedostatečně využívá prostředky, odeberte ji.

## Jak lze určit počet DTU potřebných pro určitou úlohu?

Pokud máte v úmyslu migrovat existující úlohu místního počítače nebo virtuálního počítače SQL Server do Azure SQL Database, můžete k odhadnutí potřebného počtu DTU použít [Kalkulačku DTU](http://dtucalculator.azurewebsites.net/). Pro existující úlohy Azure SQL Database můžete použít [SQL Database Query Performance Insight](sql-database-query-performance.md), abyste pochopili spotřebu databázových prostředků (TU) a lépe porozuměli možnostem optimalizace úloh. Můžete také použít DMV [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) a získat informace o spotřebě prostředků za poslední hodinu. Můžete také zadávat dotazy na zobrazení katalogu [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx). Získáte stejná data za posledních 14 dní, průměry za pětiminutová období ale mohou být méně přesné.

## Jak poznám, že by pro mě používání elastického fondu prostředků bylo výhodné?

Fondy jsou vhodné pro velký počet databází s konkrétními vzory využití. Pro danou databázi je tento vzor charakterizován nízkou mírou průměrného využití s relativně málo častými nárůsty využití. SQL Database automaticky vyhodnotí historické údaje používání prostředků databází na existujícím serveru SQL Database a doporučí odpovídající konfigurace fondu na webu Azure Portal. Další informace najdete v článku o [vhodnosti použití fondu elastické databáze](sql-database-elastic-pool-guidance.md).

## Co se stane, když dosáhnu maximálního počtu DTU

Úrovně výkonu se kalibrují a řídí tak, aby poskytovaly zdroje potřebné ke spuštění vaší úlohy databáze až po maximální limity, které jsou pro vybranou úroveň služeb či úroveň výkonu povoleny. Pokud úloha dosahuje limity pro procesory / datový vstup/výstup / vstup/výstup protokolu, budete i nadále využívat prostředky na maximální povolené úrovni; je ale pravděpodobné, že se u vašich dotazů zvýší latence. Tyto limity nevedou k žádným chybám, ale mohou zpomalit zpracování úlohy, pokud se zpomalení nezvýší natolik, že začne docházet k vypršení časového limitu dotazů. Pokud dosahujete limitů pro maximální povolený počet souběžných relací/požadavků uživatelů (pracovních vláken), zobrazují se explicitní chyby. Informace o limitech jiných prostředků než procesory, paměť, datový vstup/výstup či vstup výstup protokolu transakcí najdete v článku [Limity prostředků Azure SQL Database](sql-database-resource-limits.md).

## Další kroky

- Informace o DTU a eDTU, které jsou k dispozici pro samostatné databáze a pro elastické fondy, najdete v článku [Úroveň služeb](sql-database-service-tiers.md).
- Informace o limitech jiných prostředků než procesory, paměť, datový vstup/výstup či vstup výstup protokolu transakcí najdete v článku [Limity prostředků Azure SQL Database](sql-database-resource-limits.md).
- Chcete-li pochopit svou spotřebu (DTU), přečtěte si článek [SQL Database Query Performance Insight](sql-database-query-performance.md).
- Chcete-li lépe pochopit metodiku používanou úlohou srovnávacího testu OLTP k určení kombinace pro jednotku DTU, přečtěte si článek [Přehled srovnávacích testů SQL Database](sql-database-benchmark-overview.md).


<!--HONumber=sep16_HO2-->


