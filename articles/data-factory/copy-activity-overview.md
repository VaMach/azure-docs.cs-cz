---
title: "Aktivita kopírování v Azure Data Factory | Microsoft Docs"
description: "Další informace o aktivitě kopírování v Azure Data Factory, můžete použít ke zkopírování dat z podporované zdrojové úložiště dat do úložiště dat podporovaných jímky."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: jingwang
ms.openlocfilehash: 9629a86e72e0d5943f0e50e04268720fbedf6c35
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="copy-activity-in-azure-data-factory"></a>Aktivita kopírování v Azure Data Factory

## <a name="overview"></a>Přehled

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-data-movement-activities.md)
> * [Verze 2 – Preview](copy-activity-overview.md)

V Azure Data Factory můžete aktivitu kopírování ke kopírování dat mezi dat úložiště nacházejí na místních i v cloudu. Po zkopírování dat, může být další transformovat a analyzovat. Aktivita kopírování můžete taky publikovat transformaci a analýzu výsledky pro business intelligence (BI) a využití aplikace.

![Role aktivity kopírování](media/copy-activity-overview/copy-activity.png)

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [aktivitu kopírování v V1](v1/data-factory-data-movement-activities.md).

Aktivita kopírování se spustí na [integrace Runtime](concepts-integration-runtime.md). Pro různé datové kopie scénář můžete využít různé příchuť integrace modulu runtime:

* Při kopírování dat mezi daty ukládá obě jsou veřejně přístupné, aktivity kopírování může být pověřený **Runtime integrace Azure**, což je zabezpečený, spolehlivou a škálovatelnou, a [globálně dostupnou](concepts-integration-runtime.md#integration-runtime-location).
* Při kopírování dat z/do úložiště dat umístěným na místních nebo v síti pomocí řízení přístupu (například Azure Virtual Network), budete muset nastavit **hostovanou na vlastním integrované Runtime** na základě kterých kopírování dat.

Integrace Runtime musí být přidružen každý zdroj a jímka úložiště dat. Další informace o tom, aktivita kopírování [Určuje, které IR používat](concepts-integration-runtime.md#determining-which-ir-to-use).

Aktivita kopírování projde těchto fází ke zkopírování dat ze zdroje do jímky. Služba, která pohání aktivity kopírování:

1. Čte data z úložiště dat zdroje.
2. Provede serializaci nebo deserializaci kompresi nebo dekompresi, mapování sloupců, atd. Dělá tyto operace na základě konfigurací vstupní datové sady, výstupní datovou sadu a kopie aktivity.
3. Zapíše data do úložiště dat podřízený/cíl.

![Přehled aktivit kopírování](media/copy-activity-overview/copy-activity-overview.png)

## <a name="supported-data-stores-and-formats"></a>Podporované datové úložiště a formáty

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores.md)]

### <a name="supported-file-formats"></a>Podporované formáty souborů

Můžete použít aktivitu kopírování **zkopírujte soubory jako-je** mezi dvěma úložišti dat na základě souborů, ve kterých se efektivně zkopíruje případ data bez serializaci nebo deserializaci.

Aktivita kopírování také podporuje čtení a zápis do souborů v zadané formáty: **Text JSON, Avro, ORC a Parquet**a kompresní kodek **GZip, Deflate, BZip2 a ZipDeflate** jsou podporovány. V tématu [podporované formáty souborů a komprese](supported-file-formats-and-compression-codecs.md) s podrobnostmi.

Například můžete provést následující kopie aktivity:

* Kopírování dat na místním serveru SQL a zapisovat do Azure Data Lake Store ve formátu ORC.
* Zkopírujte soubory ve formátu textu (CSV) z místní systém souborů a zápis do objektu Blob Azure ve formátu Avro.
* Kopírování komprimované soubory ze systému souborů na místě a pak dekomprimovat krajině do Azure Data Lake Store.
* Kopírování dat z Azure Blob ve formátu GZip komprimované textu (CSV) a zapsat do databáze SQL Azure.

## <a name="configuration"></a>Konfigurace

Pomocí aktivity kopírování v Azure Data Factory, budete muset:

1. **Vytvoření propojených služeb pro úložiště dat zdroj a jímka úložišti.** Informace naleznete v sekci "Vazbu vlastnosti služby" v článku konektoru na tom, jak nakonfigurovat a podporovaných vlastností. Můžete najít v seznamu podporovaných konektor v [podporované úložiště dat a formáty](#supported-data-stores-and-formats) části.
2. **Vytvoření datových sad pro zdrojový a podřízený.** Odkazovat na zdroj a jímka části "Vlastnosti datové sady" konektor články v tom, jak nakonfigurovat a podporovaných vlastností.
3. **Vytvoření kanálu s aktivitou kopírování.** V další části najdete příklad.  

### <a name="syntax"></a>Syntaxe

Následující šablony aktivity kopírování obsahuje úplný seznam podporovaných vlastností. Zadejte ty, které vyhovovat vašemu scénáři.

```json
"activities":[
    {
        "name": "CopyActivityTemplate",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<source dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<sink dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>",
                <properties>
            },
            "sink": {
                "type": "<sink type>"
                <properties>
            },
            "translator":
            {
                "type": "TabularTranslator",
                "columnMappings": "<column mapping>"
            },
            "cloudDataMovementUnits": <number>,
            "parallelCopies": <number>,
            "enableStaging": true/false,
            "stagingSettings": {
                <properties>
            },
            "enableSkipIncompatibleRow": true/false,
            "redirectIncompatibleRowSettings": {
                <properties>
            }
        }
    }
]
```

### <a name="syntax-details"></a>Syntaxe podrobnosti

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu aktivity kopírování musí být nastavena na: **kopie** | Ano |
| Vstupy | Zadejte datovou sadu, která jste vytvořili, které body ke zdrojovým datům. Aktivita kopírování podporuje pouze jeden vstup. | Ano |
| Výstupy | Zadejte datovou sadu jste vytvořili, které body k datům jímky. Aktivita kopírování podporuje pouze jediného výstupu. | Ano |
| typeProperties | Skupina vlastností konfigurace aktivity kopírování. | Ano |
| Zdroj | Zadejte typ zdroje kopie a odpovídající vlastnosti o tom, jak načíst data.<br/><br/>Další informace v článku konektor uvedené v části "Zkopírovat vlastnosti aktivity" [podporované úložiště dat a formáty](#supported-data-stores-and-formats). | Ano |
| podřízený | Zadejte typ jímky kopie a odpovídající vlastnosti o tom, jak zapsat data.<br/><br/>Další informace v článku konektor uvedené v části "Zkopírovat vlastnosti aktivity" [podporované úložiště dat a formáty](#supported-data-stores-and-formats). | Ano |
| Překladač | Zadejte mapování explicitní sloupce ze zdroje na jímky. Použije, pokud výchozí chování kopírování se nedá vyřídit vašim potřebám.<br/><br/>Další informace z podrobností o [schéma a data mapování typu](copy-activity-schema-and-type-mapping.md). | Ne |
| cloudDataMovementUnits | Zadejte powerfulness z [Runtime integrace Azure](concepts-integration-runtime.md) na základě kterých kopírování dat.<br/><br/>Další informace z podrobností o [jednotky přesun dat v cloudu](copy-activity-performance.md). | Ne |
| parallelCopies | Určení stupně paralelního zpracování, který chcete aktivitu kopírování použít při čtení dat ze zdroje a zápis dat do jímky.<br/><br/>Další informace z podrobností o [paralelní kopie](copy-activity-performance.md#parallel-copy). | Ne |
| enableStaging<br/>stagingSettings | Zvolte Příprava dočasné dat v úložišti objektů blob aa místo přímo kopírování dat ze zdroje na jímky.<br/><br/>Další užitečné scénáře a podrobnosti o konfiguraci z [připravený kopie](copy-activity-performance.md#staged-copy). | Ne |
| enableSkipIncompatibleRow<br/>redirectIncompatibleRowSettings| Vyberte, jak pracovat s nekompatibilní řádků při kopírování dat ze zdroje na jímky.<br/><br/>Další informace z podrobností o [odolnost proti chybám](copy-activity-fault-tolerance.md). | Ne |

## <a name="monitoring"></a>Monitorování

Podrobnosti o provádění aktivitě kopírování a výkonové charakteristiky se vrátí při aktivitě kopírování výsledek spuštění -> část výstupu. Níže je vyčerpání seznamu. Naučte se monitorovat aktivity při spuštění z [rychlý start části sledování](quickstart-create-data-factory-dot-net.md#monitor-a-pipeline-run). Můžete porovnat výkon a konfiguraci váš scénář aktivity kopírování [referenční dokumentace výkonu](copy-activity-performance.md#performance-reference) z interní testování.

| Název vlastnosti  | Popis | Jednotka |
|:--- |:--- |:--- |
| DataRead | Velikost dat číst ze zdroje. | Hodnotu Int64 v bajtech |
| DataWritten | Velikost dat zapsána do jímky | Hodnotu Int64 v bajtech |
| rowsCopied | Počet řádků, které jsou kopírovány (neplatí pro binární kopie). | Hodnotu Int64 (žádné unit) |
| rowsSkipped | Počet nekompatibilní řádků přeskočen. Tuto funkci můžete zapnout pomocí sady "enableSkipIncompatibleRow" na hodnotu true. | Hodnotu Int64 (žádné unit) |
| Propustnost | Poměr přenášených dat | Plovoucí číslo bodu v KB/s |
| copyDuration | Doba trvání kopie | Hodnota Int32 v sekundách |
| sqlDwPolyBase | Pokud se při kopírování dat do SQL Data Warehouse PolyBase používá. | Logická hodnota |
| redshiftUnload | Pokud uvolnění se používá při kopírování dat z Redshift. | Logická hodnota |
| hdfsDistcp | Pokud DistCp se používá při kopírování dat z HDFS. | Logická hodnota |
| effectiveIntegrationRuntime | Zobrazit, který Runtime(s) integrace se používá k posílení aktivity při spuštění ve formátu `<IR name> (<region if it's Azure IR>)`. | Text (řetězec) |
| usedCloudDataMovementUnits | Efektivní cloudové jednotky přesun dat během kopírování. | Hodnota Int32 |
| redirectRowPath | Cesta k protokolu přeskočené nekompatibilní řádků v úložišti objektů blob, které jste nakonfigurovali v části "redirectIncompatibleRowSettings". Najdete v následujícím příkladu. | Text (řetězec) |
| billedDuration | Doba trvání pro přesun dat se účtují. | Hodnota Int32 v sekundách |

```json
"output": {
    "dataRead": 1024,
    "dataWritten": 2048,
    "rowsCopies": 100,
    "rowsSkipped": 2,
    "throughput": 1024.0,
    "copyDuration": 3600,
    "redirectRowPath": "https://<account>.blob.core.windows.net/<path>/<pipelineRunID>/",
    "redshiftUnload": true,
    "sqlDwPolyBase": true,
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)",
    "usedCloudDataMovementUnits": 8,
    "billedDuration": 28800
}
```

## <a name="schema-and-data-type-mapping"></a>Schéma a mapování datového typu

Najdete v článku [schéma a data mapování typu](copy-activity-schema-and-type-mapping.md), který popisuje, jak aktivity kopírování mapuje zdroje dat, aby se jímky.

## <a name="fault-tolerance"></a>Odolnost proti chybám

Ve výchozím nastavení aktivity kopírování zastaví kopírování dat a vrátí chybu, pokud se setká s nekompatibilní data mezi zdroj a jímka. Můžete nakonfigurovat výslovně přeskočit a protokolu nekompatibilní řádky a kopírovat pouze ty kompatibilní data tak, aby kopie bylo úspěšné. Najdete v článku [aktivity kopírování odolnost proti chybám](copy-activity-fault-tolerance.md) na další podrobnosti.

## <a name="performance-and-tuning"></a>Výkon a ladění

Najdete v článku [výkonu kopie aktivity a vyladění průvodce](copy-activity-performance.md), který popisuje klíčové faktory ovlivňující výkon přesun dat (aktivita kopírování) v Azure Data Factory. Také uvádí pozorovaný výkon při interním testování a popisuje různé způsoby za účelem optimalizace výkonu kopie aktivity.

## <a name="next-steps"></a>Další kroky
Naleznete v následujících – elementy QuickStart, kurzy a ukázky:

- [Kopírování dat z jednoho umístění do jiného umístění v stejné Azure Blob Storage](quickstart-create-data-factory-dot-net.md)
- [Kopírování dat z Azure Blob Storage do Azure SQL Database](tutorial-copy-data-dot-net.md)
- [Kopírování dat z místního SQL serveru do Azure](tutorial-hybrid-copy-powershell.md)
