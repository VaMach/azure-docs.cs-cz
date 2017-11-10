---
title: "Kopírování dat do/z Azure SQL Database pomocí služby Data Factory | Microsoft Docs"
description: "Zjistěte, jak zkopírovat data z podporované zdrojové úložiště dat do Azure SQL Database (nebo) z databáze SQL na úložiště dat podporovaných podřízený pomocí služby Data Factory."
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
ms.date: 08/30/2017
ms.author: jingwang
ms.openlocfilehash: a5d2994eb1203274454fc31c3ee9bf7a21562f75
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="copy-data-to-or-from-azure-sql-database-by-using-azure-data-factory"></a>Kopírovat data do nebo z Azure SQL Database pomocí Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-azure-sql-connector.md)
> * [Verze 2 – Preview](connector-azure-sql-database.md)

Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat z a do Azure SQL Database. Vychází [zkopírujte aktivity přehled](copy-activity-overview.md) článek, který představuje obecný přehled aktivity kopírování.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [konektor Azure SQL Database v V1](v1/data-factory-azure-sql-connector.md).

## <a name="supported-capabilities"></a>Podporované možnosti

Můžete zkopírovat data z/do Azure SQL Database do úložiště dat žádné podporované podřízený nebo zkopírování dat z jakékoli úložiště podporované zdroje dat do Azure SQL Database. Seznam úložišť dat, které jsou podporovány jako zdroje nebo jímky aktivitě kopírování najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.

Konkrétně tento konektor Azure SQL Database podporuje:

- Kopírování dat pomocí ověřování SQL.
- Jako zdroj načítání dat pomocí dotazu SQL nebo uloženou proceduru.
- Jako jímku přidávání dat do cílové tabulky nebo volání uložené procedury s vlastní logikou během kopírování.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování pomocí sady .NET SDK, Python SDK, Azure PowerShell, REST API nebo šablony Azure Resource Manageru. V tématu [kurzu aktivity kopírování](quickstart-create-data-factory-dot-net.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.

Následující části obsahují podrobnosti o vlastnosti, které se používají k definování konkrétní entity služby Data Factory pro konektor Azure SQL Database.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Pro databáze SQL Azure, propojené služby jsou podporovány následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavena na: **azuresqldatabase.** | Ano |
| připojovací řetězec |Zadejte informace potřebné pro připojení k instanci databáze SQL Azure pro vlastnost connectionString. Podporováno je pouze základní ověřování. Toto pole můžete označte jako SecureString. |Ano |
| connectVia | [Integrace Runtime](concepts-integration-runtime.md) který se má použít pro připojení k úložišti. (Pokud je vaše úložiště dat se nachází v privátní síti), můžete použít modul Runtime integrace Azure nebo Self-hosted integrace Runtime. Pokud není zadaný, použije výchozí Runtime integrace Azure. |Ne |

> [!IMPORTANT]
> Konfigurace [Firewall databáze Azure SQL](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) databázový server, který [povolit službám Azure přístup k serveru](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Kromě toho pokud data kopírujete do Azure SQL Database z mimo Azure, včetně zdrojů dat místně pomocí služby data factory vlastním hostováním integrace modulu Runtime, nakonfigurujte odpovídající rozsah IP adres pro počítač, který odesílá data do Azure SQL Databáze.

**Příklad:**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
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

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady najdete v článku datové sady. Tato část obsahuje seznam vlastností, které podporuje Azure SQL Database datovou sadu.

Ke zkopírování dat z/do Azure SQL Database, nastavte vlastnost typu datové sady, která **AzureSqlTable**. Podporovány jsou následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu datové sady musí být nastavena na: **AzureSqlTable** | Ano |
| tableName |Název tabulky nebo zobrazení instance databáze SQL Azure, kterou propojená služba odkazuje. | Ano |

**Příklad:**

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [kanály](concepts-pipelines-activities.md) článku. Tato část obsahuje seznam vlastností, které podporuje Azure SQL Database zdroj a jímka.

### <a name="azure-sql-database-as-source"></a>Azure SQL Database jako zdroj

Ke zkopírování dat z Azure SQL Database, nastavte typ zdroje v aktivitě kopírování do **SqlSource**. Následující vlastnosti jsou podporovány v aktivitě kopírování **zdroj** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ zdroje kopie aktivity musí být nastavena na: **SqlSource** | Ano |
| sqlReaderQuery |Čtení dat pomocí vlastního dotazu SQL. Příklad: `select * from MyTable`. |Ne |
| sqlReaderStoredProcedureName |Název uložené procedury, který čte data ze zdrojové tabulky. Poslední příkaz jazyka SQL musí být příkaz SELECT v uložené proceduře. |Ne |
| storedProcedureParameters |Parametry pro uloženou proceduru.<br/>Povolené hodnoty jsou: páry název/hodnota. Názvy a malá a velká písmena parametry musí odpovídat názvům a malá a velká písmena parametry uložené procedury. |Ne |

**Všimněte si body:**

- Pokud **sqlReaderQuery** je zadán pro SqlSource, aktivitě kopírování spouští tento dotaz zdrojů databáze SQL Azure a získat data. Alternativně můžete zadat uložené procedury zadáním **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud uložená procedura přebírá parametry).
- Pokud nezadáte "sqlReaderQuery" nebo "sqlReaderStoredProcedureName", sloupce definované v části "struktura" sady dat JSON se používají k vytvoření dotazu (`select column1, column2 from mytable`) ke spouštění na databázi SQL Azure. Pokud definice datové sady nemá strukturu"", jsou vybrány všechny sloupce z tabulky.
- Při použití **sqlReaderStoredProcedureName**, stále je třeba zadat fiktivní **tableName** vlastnost v datové sadě JSON.

**Příklad: pomocí jazyka SQL**

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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
                "type": "SqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Příklad: pomocí uložené procedury**

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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
                "type": "SqlSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Uložená procedura definice:**

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="azure-sql-database-as-sink"></a>Azure SQL Database jako jímku

Ke zkopírování dat do Azure SQL Database, nastavte typ jímky v aktivitě kopírování do **SqlSink**. Následující vlastnosti jsou podporovány v aktivitě kopírování **podřízený** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ jímky kopie aktivity musí nastavena: **SqlSink** | Ano |
| writeBatchSize |Vloží data do tabulky SQL, když velikost vyrovnávací paměti dosáhne writeBatchSize.<br/>Povolené hodnoty jsou: celé číslo (počet řádků). |Ne (výchozí hodnota je 10000) |
| writeBatchTimeout |Počkejte, než čas na dokončení předtím, než vyprší časový limit operace dávkové vložení.<br/>Povolené hodnoty jsou: časový interval. Příklad: "00: 30:00" (30 minut). |Ne |
| sqlWriterStoredProcedureName |Název uložené procedury upserts (aktualizace nebo vložení) dat do cílové tabulky. |Ne |
| storedProcedureParameters |Parametry pro uloženou proceduru.<br/>Povolené hodnoty jsou: páry název/hodnota. Názvy a malá a velká písmena parametry musí odpovídat názvům a malá a velká písmena parametry uložené procedury. |Ne |
| sqlWriterTableType |Zadejte název typu tabulky má být použit v uložené proceduře. Aktivita kopírování zpřístupní přesouvání dat v dočasné tabulce s tímto typem tabulky. Uložená procedura kód pak sloučit data kopírovány s existujícími daty. |Ne |
| preCopyScript |Zadejte pro aktivitu kopírování ke spuštění před zápis dat do Azure SQL Database v každé spuštění příkazu jazyka SQL. Tato vlastnost slouží k vyčištění předem načtená data. |Ne |

> [!TIP]
> Při kopírování dat do Azure SQL Database, připojí aktivitě kopírování dat do tabulky jímky ve výchozím nastavení. Chcete-li provedení UPSERT nebo jiné obchodní logiky, použijte uloženou proceduru v SqlSink. Podrobné informace z [volání uložené procedury pro SQL jímky](#invoking-stored-procedure-for-sql-sink).

**Příklad 1: připojování dat**

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**Příklad 2: volání uložené procedury během kopírování pro upsert**

Podrobné informace z [volání uložené procedury pro SQL jímky](#invoking-stored-procedure-for-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "sqlWriterTableType": "CopyTestTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="identity-columns-in-the-target-database"></a>Sloupce identity v cílové databázi

Tato část poskytuje příklad pro kopírování dat ze zdrojové tabulky bez sloupec identity do cílové tabulky se sloupcem identity.

**Zdrojová tabulka:**

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```

**Cílové tabulky:**

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

Všimněte si, že cílová tabulka obsahuje sloupec identity.

**Definice JSON datové sady zdroje**

```json
{
    "name": "SampleSource",
    "properties": {
        "type": " AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTbl"
        }
    }
}
```

**Cílový definici JSON datové sady**

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "TargetTbl"
        }
    }
}
```

Všimněte si, že jako zdrojové a cílové tabulky jiné schéma (cíl má sloupec s identitou). V tomto scénáři budete muset zadat **struktura** vlastnost v definici datové sady cíl, který neobsahuje sloupec identity.

## <a name="invoke-stored-procedure-from-sql-sink"></a>Volání uložené procedury jímku SQL

Při kopírování dat do Azure SQL Database, uživatel zadaný uložené procedury může nakonfigurované a volána s další parametry.

Uložené procedury lze použít, když předdefinované kopie mechanismy neodesílají účel. Používá se obvykle při upsert (insert a update) nebo další zpracování (slučování sloupců, vyhledávání dalších hodnot, vložení do více tabulek, atd.) je třeba provést před posledním vkládání zdrojová data v cílové tabulce.

Následující příklad ukazuje, jak pomocí uložené procedury provádět upsert do tabulky v databázi SQL Azure. Za předpokladu, že vstupní data a podřízený "Marketing" tabulka má tři sloupce: ProfileID, stavu a kategorie. Proveďte podle sloupce "ProfileID" upsert a platí jen pro určité kategorie.

**Výstupní datové sady**

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

Definování v části "SqlSink" v aktivitě kopírování následujícím způsobem.

```json
"sink": {
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing",
    "storedProcedureParameters": {
        "category": {
            "value": "ProductA"
        }
    }
}
```

V databázi zadejte uložená procedura se stejným názvem jako "SqlWriterStoredProcedureName". Zpracovává vstupní data ze zadaného zdroje a sloučení do výstupní tabulky. Všimněte si, že název parametru uložené procedury by měla být stejná jako "tableName" definované v datové sadě.

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
AS
BEGIN
  MERGE [dbo].[Marketing] AS target
  USING @Marketing AS source
  ON (target.ProfileID = source.ProfileID and target.Category = @category)
  WHEN MATCHED THEN
      UPDATE SET State = source.State
  WHEN NOT MATCHED THEN
      INSERT (ProfileID, State, Category)
      VALUES (source.ProfileID, source.State, source.Category)
END
```

V databázi zadejte typ tabulky se stejným názvem jako sqlWriterTableType. Všimněte si, že schéma typu tabulky musí být stejná jako schéma vrácený vstupní data.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL，
)
```

Uložené procedury funkce využívá [zavolat parametry](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-azure-sql-database"></a>Mapování datového typu pro databázi SQL Azure

Při kopírování dat z/do Azure SQL Database, se používají následující mapování z Azure SQL Database datové typy k Azure Data Factory dočasné datové typy. V tématu [schéma a data zadejte mapování](copy-activity-schema-and-type-mapping.md) a zjistěte, jak aktivity kopírování mapuje zdroje schéma a data typ jímky.

| Azure SQL Database datový typ | Typ průběžných dat objektu pro vytváření dat |
|:--- |:--- |
| bigint |Int64 |
| Binární |Byte] |
| Bit |Logická hodnota |
| Char |Řetězec, Char] |
| Datum |Data a času |
| Data a času |Data a času |
| datetime2 |Data a času |
| Datový typ DateTimeOffset |Datový typ DateTimeOffset |
| Decimal |Decimal |
| Atribut FILESTREAM (varbinary(max)) |Byte] |
| Plovoucí desetinná čárka |Double |
| Bitové kopie |Byte] |
| celá čísla |Int32 |
| peníze |Decimal |
| nchar |Řetězec, Char] |
| ntext |Řetězec, Char] |
| číselné |Decimal |
| nvarchar |Řetězec, Char] |
| skutečné |Jeden |
| ROWVERSION |Byte] |
| smalldatetime |Data a času |
| smallint |Int16 |
| Smallmoney |Decimal |
| SQL_VARIANT |Objekt * |
| Text |Řetězec, Char] |
| time |Časový interval |
| časové razítko |Byte] |
| tinyint |Bajtů |
| Typ UniqueIdentifier |Identifikátor GUID |
| varbinary |Byte] |
| varchar |Řetězec, Char] |
| xml |XML |

## <a name="next-steps"></a>Další kroky
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md##supported-data-stores-and-formats).