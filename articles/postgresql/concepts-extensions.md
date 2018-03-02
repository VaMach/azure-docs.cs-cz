---
title: "Použití rozšíření PostgreSQL v databázi Azure pro PostgreSQL"
description: "Popisuje možnost rozšířit funkce vaší databáze pomocí rozšíření v databázi Azure pro PostgreSQL."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 0b4150fcd7d32c823173c3e2676e226634346a2b
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql"></a>Rozšíření PostgreSQL v databázi Azure pro PostgreSQL
PostgreSQL poskytuje možnost rozšířit funkce vaší databáze pomocí rozšíření. Rozšíření umožňují pro sdružování více souvisejících objektů SQL společně v jednom balíčku, který může načíst nebo odebrat z databáze pomocí jednoho příkazu. Po načtení v databázi můžete rozšíření fungovat stejně jako integrované funkce. Další informace o rozšíření PostgreSQL najdete v tématu [balení související objekty do rozšíření](https://www.postgresql.org/docs/9.6/static/extend-extensions.html).

## <a name="how-to-use-postgresql-extensions"></a>Jak používat rozšíření PostgreSQL
Rozšíření PostgreSQL musí být nainstalován do databáze, abyste mohli používat. Pokud chcete nainstalovat konkrétní rozšíření, spusťte [vytvoření rozšíření](https://www.postgresql.org/docs/9.6/static/sql-createextension.html) příkaz psql nástroj pro načítání zabalené objektů do databáze.

Azure databázi PostgreSQL aktuálně podporuje podmnožinu klíče rozšíření, jak je uvedeno dále. Rozšíření nad rámec uvedené nejsou podporována; Nelze vytvořit vlastní rozšíření s Azure databáze pro službu PostgreSQL.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>Rozšíření nepodporuje Azure databáze pro PostgreSQL
V následujících tabulkách najdete standardní PostgreSQL rozšíření, které jsou aktuálně podporovány Azure databáze PostgreSQL. Tyto informace jsou k dispozici také dotazováním `pg\_available\_extensions`.

### <a name="data-types-extensions"></a>Datové typy rozšíření

> [!div class="mx-tableFixed"]
| **Rozšíření** | **Popis** |
|---|---|
| [chkpass](https://www.postgresql.org/docs/9.6/static/chkpass.html) | Poskytuje datový typ pro automatické šifrovat hesla. |
| [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | Poskytuje typu string znak velká a malá písmena. |
| [cube](https://www.postgresql.org/docs/9.6/static/cube.html) | Poskytuje datový typ pro multidimenzionálních krychlí. |
| [hstore](https://www.postgresql.org/docs/9.6/static/hstore.html) | Poskytuje datový typ pro ukládání sad párů klíč/hodnota. |
| [isn](https://www.postgresql.org/docs/9.6/static/isn.html) | Poskytuje datové typy mezinárodní produktu číslování standardů. |
| [ltree](https://www.postgresql.org/docs/9.6/static/ltree.html) | Poskytuje datový typ pro hierarchické jako stromové struktury. |

### <a name="functions-extensions"></a>Funkce rozšíření

> [!div class="mx-tableFixed"]
| **Rozšíření** | **Popis** |
|---|---|
| [earthdistance](https://www.postgresql.org/docs/9.6/static/earthdistance.html) | Poskytuje prostředky k výpočtu vzdálenosti dobře kroužek na povrchu země. |
| [fuzzystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | Poskytuje několik funkcí k určení podobnosti a vzdálenost mezi řetězce. |
| [intarray](https://www.postgresql.org/docs/9.6/static/intarray.html) | Poskytuje funkce a operátory pro manipulaci s hodnotou null bez pole celých čísel. |
| [pgcrypto](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Poskytuje kryptografických funkcí. |
| [pg\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Spravuje dělené tabulky čas nebo ID. |
| [pg\_trgm](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | Poskytuje funkce a operátory pro určení podobnosti alfanumerické text na základě shody se trigram. |
| [tablefunc](https://www.postgresql.org/docs/9.6/static/tablefunc.html) | Poskytuje funkce, které pracují s celé tabulky, včetně sestavě křížových tabulek. |
| [uuid-ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | Generuje univerzálně jedinečné identifikátory (identifikátory UUID). |

### <a name="full-text-search-extensions"></a>Fulltextové vyhledávání rozšíření

> [!div class="mx-tableFixed"]
| **Rozšíření** | **Popis** |
|---|---|
| [dict\_int](https://www.postgresql.org/docs/9.6/static/dict-int.html) | Poskytuje textové vyhledávání slovník šablony pro celá čísla. |
| [unaccent](https://www.postgresql.org/docs/9.6/static/unaccent.html) | Slovník hledání textu, který odebere lexemes akcenty (diakritiky znaky). |

### <a name="index-types-extensions"></a>Index typy rozšíření

> [!div class="mx-tableFixed"]
| **Rozšíření** | **Popis** |
|---|---|
| [Struktura b-stromu\_gin](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | Poskytuje ukázkové GIN operátor třídy, které implementují B-stromu jako chování u určitých datových typů. |
| [Struktura b-stromu\_gist](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | Poskytuje GiST index operátor třídy, které implementují B-stromu. |

### <a name="language-extensions"></a>Jazyková rozšíření

> [!div class="mx-tableFixed"]
| **Rozšíření** | **Popis** |
|---|---|
| [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | PL/pgSQL načíst procedurální jazyk. |

### <a name="miscellaneous-extensions"></a>Ostatní rozšíření

> [!div class="mx-tableFixed"]
| **Rozšíření** | **Popis** |
|---|---|
| [pg\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | Poskytuje způsob kontroly, co se děje v mezipaměti sdílené vyrovnávací paměti v reálném čase. |
| [pg\_prewarm](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | Poskytuje způsob, jak načíst data relace do mezipaměti vyrovnávací paměti. |
| [PG\_stat\_příkazy](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Poskytuje prostředky pro sledování spuštění statistiky všechny příkazy SQL, které jsou spuštěny serveru. |
| [pgrowlocks](https://www.postgresql.org/docs/9.6/static/pgrowlocks.html) | Poskytuje způsob pro zobrazení informací o uzamčení úrovni řádků. |
| [pgstattuple](https://www.postgresql.org/docs/9.6/static/pgstattuple.html) | Představuje způsob, jak zobrazuje statistika na úrovni řazené kolekce členů. |
| [postgres\_fdw](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | Obálka cizí dat používá pro přístup k dat uložených v externích serverů PostgreSQL. |

### <a name="postgis-extensions"></a>PostGIS rozšíření

> [!div class="mx-tableFixed"]
| **Rozšíření** | **Popis** |
|---|---|
| [PostGIS](http://www.postgis.net/), postgis\_topologie, postgis\_tiger\_geocoder, postgis\_sfcgal | Prostorová a geografické objekty pro PostgreSQL. |
| Adresa\_standardizer, adresa\_standardizer\_data\_nám | Použít k analýze adresu do základní elementy. Použít pro podporu určování zeměpisných souřadnic adresu normalizaci krok. |
| [pgrouting](http://pgrouting.org/) | Rozšiřuje PostGIS / PostgreSQL geoprostorové databázi, aby zajistil geoprostorové směrování funkce. |

## <a name="next-steps"></a>Další postup
Pokud nevidíte rozšíření, které chcete použít, dejte nám vědět. Hlasovat pro existující požadavky nebo vytvořit nový zpětnou vazbu a požadavky v našem [fóru pro zpětnou vazbu zákazníka](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).
