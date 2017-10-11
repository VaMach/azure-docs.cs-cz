---
title: Limity kapacity SQL Data Warehouse | Microsoft Docs
description: "Maximální hodnoty pro připojení, databáze, tabulky a dotazy pro SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: e1eac122-baee-4200-a2ed-f38bfa0f67ce
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: reference
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: 52026a58a5b6e26a660f9e1374e67036c67ac525
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sql-data-warehouse-capacity-limits"></a>Limity kapacity SQL Data Warehouse
Následující tabulky obsahují maximální hodnoty povolené pro různé komponenty služby Azure SQL Data Warehouse.

## <a name="workload-management"></a>Správa zatížení
| Kategorie | Popis | Maximální počet |
|:--- |:--- |:--- |
| [Jednotky datového skladu (DWU)][Data Warehouse Units (DWU)] |Maximální počet DWU pro jeden datový sklad SQL |6000 |
| [Jednotky datového skladu (DWU)][Data Warehouse Units (DWU)] |Maximální počet DWU pro jeden server SQL |6000 ve výchozím nastavení<br/><br/> Ve výchozím nastavení má každého serveru SQL server (například myserver.database.windows.net) kvóty DTU 45,000, což umožňuje až 6000 DWU. Tato kvóta je jednoduše bezpečnostní omezení. Můžete zvýšit kvótu podle [vytvoření lístku podpory] [ creating a support ticket] a výběrem *kvóty* jako typ požadavku.  K výpočtu vaší DTU potřebuje, celkový počet DWU potřeby vynásobit 7.5. Vaši aktuální spotřebu DTU z okna serveru SQL můžete zobrazit na portálu. Pozastavené i nepozastavené databáze se započítávají do kvóty DTU. |
| Připojení k databázi |Souběžné otevřených relací |1024<br/><br/>Podporujeme maximálně 1 024 aktivní připojení, z nichž každá může odesílat žádosti k databázi SQL Data Warehouse ve stejnou dobu. Všimněte si, že existují omezení počtu dotazy, které ve skutečnosti můžete současně provést. Při překročení limitu souběžnosti, žádost přejde do vnitřní fronty, kde se čeká na zpracování. |
| Připojení k databázi |Maximální velikost paměti pro připravené příkazy |20 MB |
| [Úlohy správy][Workload management] |Maximální počet souběžných dotazů |32<br/><br/> Ve výchozím nastavení můžete spustit SQL Data Warehouse maximálně 32 souběžných dotazů a fronty zbývající dotazy.<br/><br/>Uživatelé přiřazení k vyšší Třída prostředků, nebo když datový sklad SQL je nakonfigurovaná s nízkou DWU se mohou snížit úroveň souběžnosti. Některé dotazy, jako jsou dotazy DMV, je vždy povoleno spustit. |
| [Databáze tempdb][Tempdb] |Maximální velikost databáze Tempdb |399 GB za od DW100. Proto v databázi Tempdb DWU1000 je velikost 3,99 TB |

## <a name="database-objects"></a>Objekty databáze.
| Kategorie | Popis | Maximální počet |
|:--- |:--- |:--- |
| Databáze |Maximální velikost |240 TB komprimované na disku<br/><br/>Tento prostor je nezávislý na prostor tempdb nebo protokolu, a proto je tento prostor vyhrazený pro trvalé tabulky.  Komprese Clusterové columnstore se odhaduje na 5 X.  Tato komprese umožňuje databázi do dosáhnout přibližně 1 PB všech tabulek po clusterových columnstore (výchozí typ tabulky). |
| Table |Maximální velikost |60 TB komprimované na disku |
| Table |Tabulky na databázi |2 miliardy |
| Table |Sloupců v tabulce |1024 sloupců |
| Table |Počet bajtů za sloupce |Závislé na sloupci [datový typ][data type].  Limit je 8000 pro datové typy char, 4000 pro nvarchar, nebo 2 GB pro maximální počet datových typů. |
| Table |Bajtů na řádek, definovaná velikost |8 060 bajtů<br/><br/>Počet bajtů na řádek je vypočítána stejným způsobem, jako je pro SQL Server s kompresí stránky. Podobně jako SQL Server, SQL Data Warehouse podporuje úložiště přetečení řádek, který umožňuje **proměnnou délkou** na poslat mimo řádek. Když proměnlivou délkou řádky odesílají mimo řádek, uloží se pouze 24 bajtů kořenové hlavní záznam. Další informace najdete v tématu [přetečení řádek dat překročení 8 KB] [ Row-Overflow Data Exceeding 8 KB] článku na webu MSDN. |
| Table |Oddíly na tabulky |15,000<br/><br/>Pro vysoký výkon, doporučujeme minimalizovat počet oddílů musí při zároveň podporovat vaše podnikové požadavky. S růstem počet oddílů režii pro operace jazyka DDL (Data Definition) a manipulace dat jazyk (DML) zvětšování a způsobí snížení výkonu. |
| Table |Znaků na jednu hodnotu hranice oddílu. |4000 |
| Index |Bez Clusterované indexy na jednu tabulku. |999<br/><br/>Platí pro pouze rowstore tabulky. |
| Index |Clusterované indexy na jednu tabulku. |1<br><br/>Platí pro rowstore a columnstore tabulky. |
| Index |Velikost klíče indexu. |900 bajtů.<br/><br/>Platí pro pouze rowstore indexy.<br/><br/>Indexy v varchar sloupce s maximální velikost více než 900 bajtů lze vytvořit, pokud existující data v sloupce, které není delší než 900 bajtů při vytvoření indexu. Však vložit později nebo se nezdaří aktualizace akcí u sloupců, které způsobí celková velikost přesahovat 900 bajtů. |
| Index |Klíčových sloupců na index. |16<br/><br/>Platí pro pouze rowstore indexy. Clusterované indexy columnstore zahrňte všechny sloupce. |
| Statistika |Velikost hodnoty kombinované sloupců. |900 bajtů. |
| Statistika |Sloupce podle statistiku objektu. |32 |
| Statistika |Statistika na sloupců v tabulce vytvoří. |30,000 |
| Uložené procedury |Maximální úrovně vnoření. |8 |
| Zobrazení |Sloupců v zobrazení |1,024 |

## <a name="loads"></a>Načítání
| Kategorie | Popis | Maximální počet |
|:--- |:--- |:--- |
| Polybase zatížení |MB na řádek |1<br/><br/>Polybase zatížení jsou omezeny na načítání řádky obou menší než 1MB a nelze načíst VARCHR(MAX), NVARCHAR(MAX) nebo VARBINARY(MAX).<br/><br/> |

## <a name="queries"></a>Dotazy
| Kategorie | Popis | Maximální počet |
|:--- |:--- |:--- |
| Dotaz |Ve frontě dotazů v případě uživatelských tabulek. |1000 |
| Dotaz |Počet souběžných dotazů na systémová zobrazení. |100 |
| Dotaz |Ve frontě dotazů na systémová zobrazení |1000 |
| Dotaz |Maximální parametry |2098 |
| Batch |Maximální velikost |65,536*4096 |
| Vyberte výsledky |Sloupců na řádek |4 096<br/><br/>Nikdy můžete mít více než 4096 sloupců na řádek v vyberte výsledek. Není zaručeno, že budete mít vždy 4096. Pokud plán dotazu vyžaduje dočasné tabulky, může použít 1024 sloupců na maximální tabulky. |
| VYBERTE |Vnořené poddotazy |32<br/><br/>Nikdy můžete mít víc než 32 vnořené poddotazy v příkazu SELECT. Není zaručeno, že budete mít vždy 32. Například spojení můžou představovat poddotazu na plán dotazu. Počet poddotazy může být také omezena dostupné paměti. |
| VYBERTE |Sloupců na připojení k |1024 sloupců<br/><br/>Nikdy může mít maximálně 1 024 sloupce ve spojení. Není zaručeno, že budete mít vždy 1024. Pokud plán spojení vyžaduje dočasné tabulky s více sloupců než výsledek spojení, platí 1024 limit pro dočasnou tabulku. |
| VYBERTE |Počet bajtů za sloupce GROUP BY. |8060<br/><br/>Sloupce v klauzuli GROUP BY může mít maximálně 8 060 bajtů. |
| VYBERTE |Počet bajtů za sloupců ORDER BY |8 060 bajtů.<br/><br/>Sloupce v klauzuli ORDER by může mít maximálně 8 060 bajtů. |
| Identifikátory a konstant na – příkaz |Počet odkazované identifikátory a konstanty. |65,535<br/><br/>SQL Data Warehouse omezí počet identifikátorů a konstanty, které může obsahovat jeden výraz dotazu. Tento limit, je 65 535. Překročení výsledkem chyba systému SQL Server 8632 číslo. Další informace najdete v tématu [vnitřní chyba: byl dosažen limit služby výrazu][Internal error: An expression services limit has been reached]. |

## <a name="metadata"></a>Metadata
| Zobrazení systému | Maximální počet řádků |
|:--- |:--- |
| Sys.dm_pdw_component_health_alerts |10 000 |
| Sys.dm_pdw_dms_cores |100 |
| Sys.dm_pdw_dms_workers |Celkový počet pracovních procesů DMS pro poslední 1000 požadavků SQL. |
| Sys.dm_pdw_errors |10 000 |
| Sys.dm_pdw_exec_requests |10 000 |
| Sys.dm_pdw_exec_sessions |10 000 |
| Sys.dm_pdw_request_steps |Celkový počet kroků pro poslední žádosti SQL 1000, které jsou uložené v sys.dm_pdw_exec_requests. |
| Sys.dm_pdw_os_event_logs |10 000 |
| Sys.dm_pdw_sql_requests |Poslední 1 000 SQL požadavků, do které jsou uložené v sys.dm_pdw_exec_requests. |

## <a name="next-steps"></a>Další kroky
Další informace najdete v části [Přehled odkaz SQL Data Warehouse][SQL Data Warehouse reference overview].

<!--Image references-->

<!--Article references-->
[Data Warehouse Units (DWU)]: ./sql-data-warehouse-overview-what-is.md
[SQL Data Warehouse reference overview]: ./sql-data-warehouse-overview-reference.md
[Workload management]: ./sql-data-warehouse-develop-concurrency.md
[Tempdb]: ./sql-data-warehouse-tables-temporary.md
[data type]: ./sql-data-warehouse-tables-data-types.md
[creating a support ticket]: /sql-data-warehouse-get-started-create-support-ticket.md

<!--MSDN references-->
[Row-Overflow Data Exceeding 8 KB]: https://msdn.microsoft.com/library/ms186981.aspx
[Internal error: An expression services limit has been reached]: https://support.microsoft.com/kb/913050
