---
title: Co je Azure SQL Data Warehouse? | Dokumentace Microsoftu
description: "Distribuovaná databáze podnikové třídy schopná zpracovávat petabajtové objemy relačních a nerelačních dat. Je to první cloudový datový sklad v odvětví, který se umí během pár sekund zvětšit, zmenšit nebo pozastavit."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: bjhubbard
editor: 
ms.assetid: 4006c201-ec71-4982-b8ba-24bba879d7bb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: overview
ms.date: 10/23/2017
ms.author: jrj;barbkess
ms.openlocfilehash: d5ad5b566bd8d40ab6d7a9151af54890fd47cc88
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="what-is-azure-sql-data-warehouse"></a>Co je Azure SQL Data Warehouse?

SQL Data Warehouse je cloudový datový sklad podnikové třídy, který využívá architekturu MPP (Massively Parallel Processing) k rychlému spouštění složitých dotazů nad petabajty dat. Využijte službu SQL Data Warehouse jako klíčovou součást řešení pro velké objemy dat. Importujte do služby SQL Data Warehouse velké objemy dat pomocí dotazů PolyBase T-SQL a následně využijte sílu architektury MPP ke spouštění vysoce výkonných analýz. Postupným integrováním a analyzováním se datový sklad stane pro vaši firmu jediným spolehlivým zdrojem pravdivých informací.  


## <a name="key-component-of-big-data-solution"></a>Klíčová součást řešení pro velké objemy dat
SQL Data Warehouse je klíčovou součástí uceleného řešení pro velké objemy dat v cloudu.

![Řešení datového skladu](media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png) 

V cloudovém datovém řešení se data ingestují do úložišť velkých objemů dat z celé řady zdrojů. Jakmile jsou data v úložišti velkých objemů dat, Hadoop, Spark a algoritmy machine learningu data připraví a natrénují. Když jsou data připravena pro komplexní analýzu, SQL Data Warehouse použije PolyBase k dotazování úložišť velkých objemů dat. PolyBase využívá standardní dotazy T-SQL k načítání dat do služby SQL Data Warehouse.
 
SQL Data Warehouse ukládá data v relačních tabulkách se sloupcovým úložištěm. Tento formát výrazně snižuje náklady na úložiště dat a zvyšuje výkon dotazů. Jakmile jsou data uložená ve službě SQL Data Warehouse, můžete spouštět analýzy ve velkém měřítku. V porovnání s tradičními databázovými systémy se dotazy analýz dokončí během několika sekund namísto minut nebo hodin namísto dnů. 

Výsledky analýz můžou jít do aplikací nebo databází pro generování sestav po celém světě. Obchodní analytici pak můžou získávat přehledy, které jim umožní provádět informovaná obchodní rozhodnutí.

## <a name="optimization-choices"></a>Možnosti optimalizace

SQL Data Warehouse nabízí [úrovně výkonu](performance-tiers.md) navržené pro zajištění flexibility umožňující splnit vaše datové potřeby bez ohledu na rozsah. Máte na výběr mezi datovým skladem optimalizovaným pro zajištění elasticity, nebo výpočetního výkonu. 

- **Úroveň výkonu optimalizovaná pro zajištění elasticity** odděluje výpočetní a úložnou vrstvu architektury. Tato možnost vyniká v úlohách, které můžou naplno využít oddělení výpočetního výkonu a úložiště, díky častému škálování podporujícímu krátká období nejvyšší aktivity. Tato výpočetní vrstva zahrnuje nízké vstupní náklady a škáluje se pro zajištění podpory většiny úloh zákazníků.

- **Úroveň výkonu optimalizovaná pro zajištění výpočetního výkonu** využívá nejnovější hardware Azure a zavádí novou mezipaměť disku NVMe SSD, která udržuje nejčastěji používaná data blízko procesorům, tedy přesně tam, kde je chcete mít. Díky automatickému vrstvení úložiště vyniká tato úroveň výkonu ve složitých dotazech, protože všechny vstupně-výstupní operace zůstávají místně na výpočetní vrstvě. Vylepšené sloupcové úložiště navíc umožňuje ve službě SQL Data Warehouse uchovávat neomezený objem dat. Úroveň výkonu optimalizovaná pro zajištění výpočetního výkonu poskytuje nejvyšší úroveň škálovatelnosti, která umožňuje škálování až na 30 000 výpočetních jednotek datového skladu (cDWU). Tuto úroveň zvolte pro úlohy vyžadující nepřetržitý a neuvěřitelně vysoký výkon.

## <a name="next-steps"></a>Další kroky
Teď, když jste se s SQL Data Warehouse seznámili, můžete zjistit, jak rychle [vytvořit datový sklad SQL Data Warehouse][create a SQL Data Warehouse] a [načíst ukázková data][load sample data]. Pokud s Azure začínáte, může vám být užitečný [Glosář Azure][Azure glossary], kde najdete potřebnou terminologii. Můžete se také podívat na některé z těchto dalších zdrojů ke službě SQL Data Warehouse.  

* [Úspěšné zákaznické implementace]
* [Blogy]
* [Žádosti o funkce]
* [Videa]
* [Blogy zákaznického poradního týmu]
* [Vytvoření lístku podpory]
* [Fórum MSDN]
* [Fórum Stack Overflow]
* [Twitter]

<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Vytvoření lístku podpory]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Úspěšné zákaznické implementace]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Blogy]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogy zákaznického poradního týmu]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Žádosti o funkce]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Fórum MSDN]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Fórum Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videa]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://azure.microsoft.com/en-us/support/legal/sla/
