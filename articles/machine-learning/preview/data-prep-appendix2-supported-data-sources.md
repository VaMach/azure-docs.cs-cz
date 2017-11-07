---
title: "Podporované zdroje dat, které jsou k dispozici přípravy dat Azure Machine Learning | Microsoft Docs"
description: "Tento dokument obsahuje úplný seznam podporovaných zdrojů dat k dispozici pro přípravu dat Azure Machine Learning."
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: db4774de28a17e022de111986f72a1f15ec32beb
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="supported-data-sources-for-azure-machine-learning-data-preparation"></a>Podporované zdroje dat pro přípravu Azure Machine Learning dat. 
Tento článek popisuje aktuálně podporovaných zdrojů dat pro přípravu dat Azure Machine Learning.

Takto vypadají podporovaných zdrojů dat pro tuto verzi.

## <a name="types"></a>Typy 
### <a name="directory-vs-file"></a>Directory oproti souboru
Zvolte jeden soubor a přečtěte si ho do přípravy data. Typ souboru je analyzována určit výchozí parametry pro připojení k souboru na další obrazovce.

Vyberte adresář nebo sadu souborů v adresáři (Nástroje pro výběr souborů je vícenásobného výběru.). S buď přístupem soubory se čtou v jako jednoho datového toku a jsou připojeny k sobě navzájem hlavičky vynechají v případě potřeby.

Typy podporované souboru jsou:
- S oddělovači (.csv tsv, .txt, atd.)
- Pevná šířka
- Prostý text
- Soubor JSON

### <a name="csv-file"></a>Soubor CSV
Načtení souboru čárkami oddělených hodnot úložiště.

#### <a name="options"></a>Možnosti
- Oddělovač
- Komentář
- Záhlaví
- Decimal – symbol
- Kódování souborů
- Řádky tak, aby přeskočil

### <a name="tsv-file"></a>Soubor TSV
Načtení souboru karta oddělených hodnot úložiště.

#### <a name="options"></a>Možnosti
- Komentář
- Záhlaví
- Kódování souborů
- Řádky tak, aby přeskočil

### <a name="excel-xlsxlsx"></a>Aplikace Excel (.xls/.xlsx)
Přečtěte si jeden listu aplikace Excel soubor současně zadáním názvu list nebo číslo.

#### <a name="options"></a>Možnosti
- Název listu nebo číslo
- Záhlaví
- Řádky tak, aby přeskočil

### <a name="json-file"></a>Soubor JSON
Načtení souboru JSON úložiště. Soubor je "průmětu" na čtení.

#### <a name="options"></a>Možnosti
- Žádný

### <a name="parquet"></a>Parquet
Číst Parquet datové sady, buď jeden soubor nebo složku.

Parquet jako formátu můžou mít různé formy v úložišti. Pro menší sady dat se někdy používá .parquet jeden soubor. Různé knihovny Python podporovat čtení nebo zápis do jednoho .parquet souborů. V současné době přípravy dat Azure Machine Learning spoléhá na knihovně PyArrow Python pro čtení Parquet během místní interaktivní používání. Podporuje jeden .parquet soubory (Pokud napsané jako a nikoli jako součást větší datové sady), a také Parquet datových sad.

Datové sady Parquet je kolekce více než jeden soubor .parquet, z nichž každý představuje menší oddílu větší datové sady. Datové sady jsou obvykle obsažené ve složce a jsou výchozí formát výstupu parquet pro platformy, jako je například Spark a Hive.

>[!NOTE]
>Při čtení Parquet data, která je ve složce s více soubory .parquet, je nejbezpečnější a vyberte adresář pro čtení a **Parquet datové sady** možnost. Díky tomu PyArrow přečíst celou složku místo jednotlivých souborů. Tím se zajistí podpora pro čtení složitější způsoby Parquet ukládání na disku, jako je například vytváření oddílů složky.

Provádění Škálováním na více systémů využívá Spark Parquet čtení možnosti a podporuje jeden soubory, jakož i složek, podobně jako místní interaktivní použití.

#### <a name="options"></a>Možnosti
- Parquet datové sady. Tato možnost určuje, zda přípravu dat Azure Machine Learning rozšíří daný adresář a se pokusí přečíst každý soubor jednotlivě (nezaškrtnuté režim), nebo jestli zpracovává adresáři jako celé sady dat (vybraný režim). S vybraný režim PyArrow vybere nejlepší způsob, jak interpretovat soubory.


## <a name="locations"></a>Umístění
### <a name="local"></a>Místní
Místní pevný disk nebo namapované síťové umístění úložiště.

### <a name="azure-blob-storage"></a>Azure Blob Storage
Objekt Blob úložiště Azure, který vyžaduje předplatné Azure.

