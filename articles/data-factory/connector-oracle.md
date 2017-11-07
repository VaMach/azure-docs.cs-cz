---
title: "Kopírování dat do nebo z databáze Oracle pomocí Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak zkopírovat data z podporované zdrojové úložiště do databáze Oracle (nebo) z databáze Oracle do podporovaných podřízený úložišť pomocí služby Data Factory."
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
ms.date: 11/01/2017
ms.author: jingwang
ms.openlocfilehash: 2266bf17dd769102e70c20728ededb304f73beb4
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="copy-data-from-and-to-oracle-using-azure-data-factory"></a>Kopírování dat z a do databáze Oracle pomocí Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-onprem-oracle-connector.md)
> * [Verze 2 – Preview](connector-oracle.md)

Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat z a do databáze Oracle. Vychází [zkopírujte aktivity přehled](copy-activity-overview.md) článek, který představuje obecný přehled aktivity kopírování.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [Oracle konektoru V1](v1/data-factory-onprem-oracle-connector.md).

## <a name="supported-capabilities"></a>Podporované možnosti

Můžete zkopírovat data z databáze Oracle do úložiště dat žádné podporované podřízený nebo zkopírování dat z jakékoli úložiště podporované zdroje dat do databáze Oracle. Seznam úložišť dat, které jsou podporovány jako zdroje nebo jímky aktivitě kopírování najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.

Konkrétně tento konektor Oracle podporuje následující verze databáze Oracle a podporuje ověřování Basic nebo OID.

    - R1 Oracle 12c (12.1)
    - R1 Oracle 11g nebo R2 (11.1, 11.2)
    - R1 Oracle 10g, R2 (10,1, 10.2)
    - Oracle 9i R1, R2 (9.0.1, 9.2)
    - Oracle 8i R3 (8.1.7)

## <a name="prerequisites"></a>Požadavky

Ke zkopírování dat z/do databáze Oracle, který není veřejně přístupný, musíte nastavit Self-hosted integrace Runtime. V tématu [Self-hosted integrace Runtime](create-self-hosted-integration-runtime.md) článku podrobnosti o běhu integrace. Modul Runtime integrace poskytuje integrované ovladače Oracle, proto nemusíte ručně nainstalovat všechny ovladače při kopírování dat z/do databáze Oracle.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování pomocí sady .NET SDK, Python SDK, Azure PowerShell, REST API nebo šablony Azure Resource Manageru. V tématu [kurzu aktivity kopírování](quickstart-create-data-factory-dot-net.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.

Následující části obsahují podrobnosti o vlastnosti, které slouží k určení konkrétní entity služby Data Factory ke konektoru Oracle.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Pro Oracle propojené služby jsou podporovány následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavena na: **Oracle** | Ano |
| připojovací řetězec | Zadejte informace potřebné pro připojení k instanci databáze Oracle. Toto pole můžete označte jako SecureString. | Ano |
| connectVia | [Integrace Runtime](concepts-integration-runtime.md) který se má použít pro připojení k úložišti. (Pokud je veřejně přístupná data store), můžete použít modul Runtime integrace Self-hosted nebo Runtime integrace Azure. Pokud není zadaný, použije výchozí Runtime integrace Azure. |Ne |

**Příklad:**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady najdete v článku datové sady. Tato část obsahuje seznam vlastností nepodporuje datovou sadu Oracle.

Ke zkopírování dat z/do databáze Oracle, nastavte vlastnost typu datové sady, která **OracleTable**. Podporovány jsou následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu datové sady musí být nastavena na: **OracleTable** | Ano |
| tableName |Název tabulky v databázi Oracle, který propojená služba odkazuje na. | Ano |

**Příklad:**

```json
{
    "name": "OracleDataset",
    "properties":
    {
        "type": "OracleTable",
        "linkedServiceName": {
            "referenceName": "<Oracle linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [kanály](concepts-pipelines-activities.md) článku. Tato část obsahuje seznam vlastností nepodporuje Oracle zdroj a jímka.

### <a name="oracle-as-source"></a>Oracle jako zdroj

Ke zkopírování dat z databáze Oracle, nastavte typ zdroje v aktivitě kopírování do **OracleSource**. Následující vlastnosti jsou podporovány v aktivitě kopírování **zdroj** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ zdroje kopie aktivity musí být nastavena na: **OracleSource** | Ano |
| oracleReaderQuery | Čtení dat pomocí vlastního dotazu SQL. Například: `"SELECT * FROM MyTable"`. | Ne |

Pokud nezadáte "oracleReaderQuery", sloupce definované v části "struktura" datové sady se používají k vytvoření dotazu (`select column1, column2 from mytable`) ke spouštění databáze Oracle. Pokud definice datové sady nemá strukturu"", jsou vybrány všechny sloupce z tabulky.

**Příklad:**

```json
"activities":[
    {
        "name": "CopyFromOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Oracle input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="oracle-as-sink"></a>Oracle jako jímku

Pokud chcete zkopírovat data do databáze Oracle, nastavte typ jímky v aktivitě kopírování do **OracleSink**. Následující vlastnosti jsou podporovány v aktivitě kopírování **podřízený** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ zdroje kopie aktivity musí být nastavena na: **OracleSink** | Ano |
| writeBatchSize | Vloží data do tabulky SQL, když velikost vyrovnávací paměti dosáhne writeBatchSize.<br/>Povolené hodnoty jsou: celé číslo (počet řádků). |Ne (výchozí hodnota je 10000) |
| writeBatchTimeout | Počkejte, než čas na dokončení předtím, než vyprší časový limit operace dávkové vložení.<br/>Povolené hodnoty jsou: časový interval. Příklad: 00:30:00 (30 minut). | Ne |
| preCopyScript | Zadejte dotaz SQL pro aktivitu kopírování ke spuštění před zápis dat do Oracle při každém spuštění. Tato vlastnost slouží k vyčištění předem načtená data. | Ne |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyToOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Oracle output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OracleSink"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-oracle"></a>Datový typ mapování pro Oracle

Při kopírování dat z/do databáze Oracle, se používají následující mapování Oracle datových typů k Azure Data Factory dočasné datové typy. V tématu [schéma a data zadejte mapování](copy-activity-schema-and-type-mapping.md) a zjistěte, jak aktivity kopírování mapuje zdroje schéma a data typ jímky.

| Oracle datový typ | Typ průběžných dat objektu pro vytváření dat |
|:--- |:--- |
| BFILE |Byte] |
| OBJEKT BLOB |Byte] |
| CHAR – |Řetězec |
| DATOVÝ TYP CLOB |Řetězec |
| DATUM |Data a času |
| PLOVOUCÍ DESETINNÁ ČÁRKA |Decimal, řetězec (Pokud přesnost > 28) |
| CELÉ ČÍSLO |Decimal, řetězec (Pokud přesnost > 28) |
| DLOUHÁ |Řetězec |
| DLOUHO NEZPRACOVANÁ |Byte] |
| NCHAR |Řetězec |
| NCLOB |Řetězec |
| ČÍSLO |Decimal, řetězec (Pokud přesnost > 28) |
| NVARCHAR2 |Řetězec |
| NEZPRACOVANÁ |Byte] |
| ID ŘÁDKU |Řetězec |
| ČASOVÉ RAZÍTKO |Data a času |
| ČASOVÉ RAZÍTKO S MÍSTNÍM ČASOVÉM PÁSMU |Řetězec |
| ČASOVÉ RAZÍTKO S ČASOVÝM PÁSMEM |Řetězec |
| CELÉ ČÍSLO BEZ ZNAMÉNKA |Číslo |
| VARCHAR2 |Řetězec |
| XML |Řetězec |

> [!NOTE]
> Datový typ INTERVALU rok, měsíc a den na INTERVAL druhý nejsou podporovány.


## <a name="next-steps"></a>Další kroky
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md##supported-data-stores-and-formats).