---
title: 'SQL Database: Co je DTU? | Dokumentace Microsoftu'
description: "Vysvětlení jednotky transakce Azure SQL Database."
keywords: "možnosti databáze, výkon databáze"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: CarlRabeler
ms.assetid: 89e3e9ce-2eeb-4949-b40f-6fc3bf520538
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 09/06/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 3d619f5d6959594ee9b139d611d7e45390a40c55
ms.openlocfilehash: d9b0eaa48d71f3ecf0a23f3bddb3c777c98afea7


---
# <a name="explaining-database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Vysvětlení jednotek databázové transakce (DTU) a elastických jednotek databázové transakce (eDTU)
Tento článek vysvětluje jednotky databázové transakce (DTU) a elastické jednotky databázové transakce (eDTU). Popisuje také, co se stane při dosažení maximálního počtu DTU nebo eDTU.  

## <a name="what-are-database-transaction-units-dtus"></a>Co jsou jednotky databázové transakce (DTU)
DTU je měrná jednotka prostředků zaručeně dostupných izolované databázi Azure SQL při určitém stupni výkonu v rámci [jedné úrovně databázové služby](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels). Jednotka DTU představuje kombinaci měření vstupu/výstupu pro procesor, paměť a data a vstupu/výstupu protokolu transakcí v poměru určeném úlohou srovnávacího testu OLTP, která je navržena jako typická pro úlohy OLTP v reálném světě. Zdvojnásobení jednotek DTU zvýšením úrovně výkonu databáze odpovídá zdvojnásobení sady prostředků, které jsou pro příslušnou databázi k dispozici. Například databáze Premium P11 se 1 750 DTU nabízí 350x více DTU výpočetního výkonu než databáze Basic s 5 DTU. Chcete-li lépe pochopit metodiku používanou úlohou srovnávacího testu OLTP k určení kombinace pro jednotku DTU, přečtěte si článek [Přehled srovnávacích testů SQL Database](sql-database-benchmark-overview.md).

![Úvod do služby SQL Database: DTU izolované databáze podle úrovní](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

[Změnit úrovně služeb](sql-database-scale-up.md) můžete kdykoli s minimální odstávkou aplikace (obecně v průměru méně než čtyři sekundy). Mnoha firmám a aplikacím stačí vytváření databází a nastavování výkonu na vyžádání, zejména v případě, že jsou vzorce používání relativně předvídatelné. Ale pokud vaše vzorce používání předvídatelné nejsou, může být správa nákladů a údržba obchodního modelu velmi těžká. Pro tento scénář použijete elastický fond s určitým počtem jednotek eDTU.

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Co jsou elastické jednotky databázové transakce (eDTU)
eDTU je jednotka měření sady prostředků (DTU), které mohou být sdíleny v rámci sady databází na serveru Azure SQL – nazývá se [elastický fond](sql-database-elastic-pool.md). Elastické fondy poskytují jednoduché a nákladově efektivní řešení správy výkonnostních cílů pro více databází, jejichž vzory používání se značně liší a nelze je předvídat. Další informace najdete v popisu [elastických fondů a úrovní služeb](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus).

![Úvod do služby SQL Database: eDTU podle vrstvy a úrovně](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Fond obdrží stanovený počet eDTU za stanovenou cenu. V rámci elastického fondu disponují jednotlivé databáze flexibilní možností automatického škálování v rámci nakonfigurovaných mezí. V případě velkého zatížení může databáze spotřebovávat více eDTU, aby splnila požadavky, zatímco při nízkém zatížení databáze spotřebovávají méně eDTU a databáze bez zatížení dokonce nespotřebovávají žádné eDTU. Zřizování prostředků pro celý fond, a nikoli pro jednotlivé databáze, zjednoduše úlohy správy a umožňuje předem odhadnout náklady na fond.

Další eDTU lze do existujícího fondu přidat bez jakéhokoli výpadku databáze a bez jakéhokoli vlivu na databáze ve fondu. Podobně platí, že pokud již přidané eDTU nejsou potřebné, lze je z existujícího fondu kdykoli odebrat. Do fondu můžete přidávat databáze nebo je z něj odebírat, nebo můžete omezit množství eDTU, které databáze může využívat v případě velkého zatížení a vyhradit tak eDTU pro ostatní databáze. Pokud databáze podle předpokladu nedostatečně využívá prostředky, můžete ji odebrat z fondu a nakonfigurovat jako izolovanou databázi s předvídatelným množstvím požadovaných prostředků.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Jak lze určit počet DTU potřebných pro určitou úlohu?
Pokud máte v úmyslu migrovat existující úlohu místního počítače nebo virtuálního počítače SQL Server do Azure SQL Database, můžete k odhadnutí potřebného počtu DTU použít [Kalkulačku DTU](http://dtucalculator.azurewebsites.net/). Pro existující úlohy Azure SQL Database můžete použít [SQL Database Query Performance Insight](sql-database-query-performance.md), abyste pochopili spotřebu databázových prostředků (TU) a lépe porozuměli možnostem optimalizace úloh. Můžete také použít DMV [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) a získat informace o spotřebě prostředků za poslední hodinu. Můžete také zadávat dotazy na zobrazení katalogu [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx). Získáte stejná data za posledních 14 dní, průměry za pětiminutová období ale mohou být méně přesné.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Jak poznám, že by pro mě používání elastického fondu prostředků bylo výhodné?
Fondy jsou vhodné pro velký počet databází s konkrétními vzory využití. Pro danou databázi je tento vzor charakterizován nízkou mírou průměrného využití s relativně málo častými nárůsty využití. SQL Database automaticky vyhodnotí historické údaje používání prostředků databází na existujícím serveru SQL Database a doporučí odpovídající konfigurace fondu na webu Azure Portal. Další informace najdete v tématu [Kdy je vhodné používat elastický fond?](sql-database-elastic-pool-guidance.md)

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Co se stane, když dosáhnu maximálního počtu DTU
Úrovně výkonu se kalibrují a řídí tak, aby poskytovaly zdroje potřebné ke spuštění vaší úlohy databáze až po maximální limity, které jsou pro vybranou úroveň služeb či úroveň výkonu povoleny. Pokud úloha dosahuje limity pro procesory / datový vstup/výstup / vstup/výstup protokolu, budete i nadále využívat prostředky na maximální povolené úrovni; je ale pravděpodobné, že se u vašich dotazů zvýší latence. Tyto limity nevedou k žádným chybám, ale mohou zpomalit zpracování úlohy, pokud se zpomalení nezvýší natolik, že začne docházet k vypršení časového limitu dotazů. Pokud dosahujete limitů pro maximální povolený počet souběžných relací/požadavků uživatelů (pracovních vláken), zobrazují se explicitní chyby. Informace o limitech jiných prostředků než procesory, paměť, datový vstup/výstup či vstup výstup protokolu transakcí najdete v článku [Limity prostředků Azure SQL Database](sql-database-resource-limits.md).

## <a name="next-steps"></a>Další kroky
* Další informace o jednotkách DTU a eDTU dostupných pro izolované databáze a elastické fondy najdete v článku [Úrovně služby](sql-database-service-tiers.md).
* Informace o limitech jiných prostředků než procesory, paměť, datový vstup/výstup či vstup výstup protokolu transakcí najdete v článku [Limity prostředků Azure SQL Database](sql-database-resource-limits.md).
* Chcete-li pochopit svou spotřebu (DTU), přečtěte si článek [SQL Database Query Performance Insight](sql-database-query-performance.md).
* Chcete-li lépe pochopit metodiku používanou úlohou srovnávacího testu OLTP k určení kombinace pro jednotku DTU, přečtěte si článek [Přehled srovnávacích testů SQL Database](sql-database-benchmark-overview.md).



<!--HONumber=Jan17_HO1-->


