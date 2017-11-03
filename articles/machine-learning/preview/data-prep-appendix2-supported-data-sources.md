---
title: "Podporované zdroje dat pomocí Azure Machine Learning Data přípravy | Microsoft Docs"
description: "Tento dokument obsahuje úplný seznam podporovaných zdrojů dat dostupných pro přípravu Azure Machine Learning dat."
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
ms.openlocfilehash: 1ef4c5c33d98cfeb566e8fe23bda9e0d3f041781
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="supported-data-sources-for-this-release"></a>Podporované zdroje dat pro tuto verzi 
Následující dokument popisuje seznam aktuálně podporované zdroje dat v Azure Machine Learning Data přípravy.

Podporované zdroje dat pro tuto verzi jsou následujícím způsobem.

## <a name="types"></a>Typy 
### <a name="directory-versus-file"></a>Adresáře a souboru
*Soubory nebo adresáře*: Zvolte jeden soubor a přečtěte si ho do přípravy Data. Typ souboru je analyzována určit výchozí parametry pro připojení k souboru, který je na zobrazení další obrazovky. Vyberte adresář nebo sadu souborů v adresáři (Nástroje pro výběr souborů je vícenásobného výběru.). Buď přístup výsledky v souborech, který je čten jako jeden toku dat v souborech připojí k sobě navzájem (s hlavičky vynechají v případě potřeby).

Typy souborů jsou následující:
- S oddělovači (.csv, TSV, .txt atd.) 
- Pevná šířka
- Prostý text
- Soubor JSON

### <a name="csv-file"></a>Soubor CSV
Přečte soubor CSV z úložiště.

#### <a name="options"></a>Možnosti
- Oddělovač
- Komentář
- Záhlaví
- Decimal – symbol
- Kódování souborů
- Řádky tak, aby přeskočil

### <a name="tsv-file"></a>Soubor TSV
Přečte soubor TSV hodnotu z úložiště.

#### <a name="options"></a>Možnosti
- Komentář
- Záhlaví
- Kódování souborů
- Řádky tak, aby přeskočil

### <a name="excel-xlsxlsx"></a>Aplikace Excel (.xls/.xlsx)
Přečte soubor aplikace Excel, listů najednou, tak, že zadáte název listu nebo číslo.

#### <a name="options"></a>Možnosti
- Název nebo číslo listu
- Záhlaví
- Řádky tak, aby přeskočil

### <a name="json-file"></a>Soubor JSON
Načtení souboru JSON úložiště. Všimněte si, že soubor je "průmětu" na čtení.

#### <a name="options"></a>Možnosti
Žádný

### <a name="parquet"></a>Parquet
Číst Parquet datovou sadu, buď jeden soubor nebo složku.

Parquet jako formátu můžou mít různé formy v úložišti. Pro menší datové sady se někdy používá .parquet jeden soubor. Různé knihovny Python podporovat čtení nebo zápisu do jednoho .parquet souboru. V současné době Azure Machine Learning Workbench spoléhá na knihovně PyArrow Python pro čtení Parquet během místní interaktivní používání. Podporuje jeden .parquet soubory (tak dlouho, dokud napsané jako takový, nikoli jako součást větší datové sady). Podporuje také Parquet datové sady. 

Datové sady Parquet je kolekce více než jeden soubor .parquet, z nichž každý představuje menší oddíl větší datové sady. Datové sady jsou obvykle obsažené ve složce. Jedná se o výchozí formát výstupu Parquet pro běžné platformy, jako je například Spark a Hive.

>[!NOTE]
>Při čtení Parquet data, která je ve složce s více soubory .parquet, je nejbezpečnější a vyberte adresář pro čtení a značek **datovou sadu Parquet** možnost. Díky tomu PyArrow přečíst celou složku místo jednotlivých souborů. To zajišťuje podporu pro čtení složitější způsoby Parquet ukládání na disku (například složky vytváření oddílů.)

Provádění Škálováním na více systémů využívá Spark Parquet čtení možnosti a podporuje jeden soubory a složky.

#### <a name="options"></a>Možnosti
*Parquet datovou sadu*: Tato možnost určuje, jestli používá Azure Machine Learning Workbench unticked režim nebo režim ticked. Unticked režim rozšíří daný adresář a se pokusí přečíst každý soubor v něm jednotlivě. Ticked režimu zpracovává adresáři jako celou sadu dat a umožňuje PyArrow zjistěte nejlepší způsob, jak interpretovat soubory.


## <a name="locations"></a>Umístění
### <a name="local"></a>Místní
Místní pevný disk nebo namapované síťové umístění úložiště.

### <a name="azure-blob-storage"></a>Azure Blob Storage
Vyžaduje předplatné Azure.

