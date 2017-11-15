---
title: "Kopírování dat do/z Azure Table Storage pomocí služby Data Factory | Microsoft Docs"
description: "Zjistěte, jak zkopírovat data z podporované zdrojové úložiště Azure Table Storage (nebo) z tabulky úložiště na úložiště podporované podřízený pomocí služby Data Factory."
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
ms.date: 11/07/2017
ms.author: jingwang
ms.openlocfilehash: ca5f8e43b6667aa1c2e3ac38e7ea00b5bd86b72f
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/13/2017
---
# <a name="copy-data-to-or-from-azure-table-using-azure-data-factory"></a>Kopírovat data do nebo z Azure Table pomocí Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-azure-table-connector.md)
> * [Verze 2 – Preview](connector-azure-table-storage.md)

Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat do a z Azure Table. Vychází [zkopírujte aktivity přehled](copy-activity-overview.md) článek, který představuje obecný přehled aktivity kopírování.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [konektor Azure Table Storage v V1](v1/data-factory-azure-table-connector.md).

## <a name="supported-capabilities"></a>Podporované možnosti

Můžete kopírování dat z jakékoli úložiště podporované zdroje dat do Azure Table nebo zkopírování dat z Azure Table do úložiště dat žádné podporované jímky. Seznam úložišť dat, které jsou podporovány jako zdroje nebo jímky aktivitě kopírování najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.

Konkrétně tento konektor Azure Table podporuje kopírování dat pomocí obou **klíč účtu** a **SAS služby** ověřování (sdíleného přístupového podpisu).

## <a name="get-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování pomocí sady .NET SDK, Python SDK, Azure PowerShell, REST API nebo šablony Azure Resource Manageru. V tématu [kurzu aktivity kopírování](quickstart-create-data-factory-dot-net.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.

Následující části obsahují podrobnosti o vlastnosti, které slouží k určení entit služby Data Factory konkrétní Azure Table Storage.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

### <a name="using-account-key"></a>Pomocí klíč účtu

Propojenou službu úložiště Azure můžete vytvořit pomocí klíč účtu, které poskytuje globální přístup k objektu pro vytváření dat do úložiště Azure. Podporovány jsou následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavena na: **azurestorage.** |Ano |
| připojovací řetězec | Zadejte informace potřebné pro připojení k úložišti Azure pro vlastnost connectionString. Toto pole můžete označte jako SecureString. |Ano |
| connectVia | [Integrace Runtime](concepts-integration-runtime.md) který se má použít pro připojení k úložišti. (Pokud je vaše úložiště dat se nachází v privátní síti), můžete použít modul Runtime integrace Azure nebo Self-hosted integrace Runtime. Pokud není zadaný, použije výchozí Runtime integrace Azure. |Ne |

**Příklad:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="using-service-sas-authentication"></a>Pomocí SAS služby ověřování

Propojenou službu úložiště Azure můžete vytvořit také pomocí sdíleného přístupového podpisu (SAS), které poskytuje omezené nebo časově vázaných přístup k prostředkům všechna nebo konkrétní v úložišti služby data factory.

Sdíleného přístupového podpisu (SAS) poskytuje Delegovaný přístup k prostředkům ve vašem účtu úložiště. Umožňuje udělit, že klient omezené oprávnění k objektům v účtu úložiště v zadaném časovém intervalu a zadanou sadu oprávnění, aniž by museli sdílet klíče pro přístup k účtu. SAS je identifikátor URI, který zahrnuje všechny informace potřebné pro ověřený přístup k prostředku úložiště v jeho parametry dotazu. Pro přístup k prostředkům úložiště s SAS, klient pouze musí předat SAS metodu, nebo odpovídající konstruktor. Podrobné informace o tokenu SAS naleznete v tématu [sdílené přístupové podpisy: vysvětlení modelu SAS](../storage/common/storage-dotnet-shared-access-signature-part-1.md)

> [!IMPORTANT]
> Azure Data Factory podporuje nyní pouze **SAS služby** , ale není SAS účtu. V tématu [typy z sdílené přístupové podpisy](../storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) podrobné informace o těchto dvou typů a postup vytvoření. Adresa URL SAS generable z portálu Azure nebo Storage Explorer je SAS účtu, který není podporován.
>

Pokud chcete použít ověřování SAS služby, jsou podporovány následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavena na: **azurestorage.** |Ano |
| sasUri | Zadejte identifikátor URI podpis sdíleného přístupu k prostředkům Azure Storage jako objekt blob, kontejneru nebo tabulky. Toto pole můžete označte jako SecureString. |Ano |
| connectVia | [Integrace Runtime](concepts-integration-runtime.md) který se má použít pro připojení k úložišti. (Pokud je vaše úložiště dat se nachází v privátní síti), můžete použít modul Runtime integrace Azure nebo Self-hosted integrace Runtime. Pokud není zadaný, použije výchozí Runtime integrace Azure. |Ne |

**Příklad:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Při vytváření **identifikátor URI pro SAS**, vzhledem k tomu následující body:

- Nastavit vhodné pro čtení a zápis **oprávnění** na objekty podle použití propojené služby (pro čtení, zápisu, čtení/zápis) ve službě data factory.
- Nastavit **čas vypršení platnosti** správně. Ujistěte se, že přístup k objektům Azure Storage, nemá prošlou platnost do aktivního období kanálu.
- Identifikátor URI by se vytvořit na úrovni pravé tabulky, podle potřeby.

## <a name="dataset-properties"></a>Vlastnosti datové sady

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady najdete v článku datové sady. Tato část obsahuje seznam vlastností, které podporuje Azure Table datovou sadu.

Pokud chcete zkopírovat data do/z Azure Table, nastavte vlastnost typu datové sady, která **AzureTable**. Podporovány jsou následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu datové sady musí být nastavena na: **AzureTable** |Ano |
| tableName |Název tabulky instance Azure tabulku databáze, kterou propojená služba odkazuje. |Ano |

**Příklad:**

```json
{
    "name": "AzureTableDataset",
    "properties":
    {
        "type": "AzureTable",
        "linkedServiceName": {
            "referenceName": "<Azure Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Schéma službou Data Factory

Služba Data Factory pro data bez schémat úložiště, jako je například Azure Table, odvodí schéma v jednom z následujících způsobů:

1. Pokud zadáte strukturu dat pomocí **struktura** vlastnost v definici datové sady, služba Data Factory ctí tato struktura jako schéma. V takovém případě Pokud řádek neobsahuje hodnotu pro sloupec, je pro něj zadat hodnotu null.
2. Pokud nezadáte strukturu dat pomocí **struktura** vlastnost v definici datové sady, Data Factory odvodí, že schéma pomocí prvního řádku v datech. V takovém případě pokud první řádek neobsahuje úplnou schéma, jsou vynechalo některé sloupce ve výsledku operace kopírování.

Osvědčeným postupem pro zdroje dat bez schémat, proto je zadat strukturu dat pomocí **struktura** vlastnost.

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [kanály](concepts-pipelines-activities.md) článku. Tato část obsahuje seznam vlastností, které podporuje Azure Table zdroj a jímka.

### <a name="azure-table-as-source"></a>Tabulky Azure jako zdroj

Ke zkopírování dat z Azure Table, nastavte typ zdroje v aktivitě kopírování do **AzureTableSource**. Následující vlastnosti jsou podporovány v aktivitě kopírování **zdroj** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ zdroje kopie aktivity musí být nastavena na: **AzureTableSource** |Ano |
| azureTableSourceQuery |Čtení dat pomocí dotazu vlastní tabulky Azure. Příklady v další části. |Ne |
| azureTableSourceIgnoreTableNotFound |Označuje, zda swallow výjimky tabulky neexistuje.<br/>Povolené hodnoty jsou: **True**, a **False** (výchozí). |Ne |

### <a name="azuretablesourcequery-examples"></a>Příklady azureTableSourceQuery

Pokud sloupec tabulky Azure je typu řetězec:

```json
"azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', <datetime parameter>)"
```

Pokud sloupec tabulky Azure je typu datum a čas:

```json
"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', <datetime parameter>, <datetime parameter>)"
```

### <a name="azure-table-as-sink"></a>Tabulky Azure jako jímku

Ke zkopírování dat do Azure Table, nastavte typ jímky v aktivitě kopírování do **AzureTableSink**. Následující vlastnosti jsou podporovány v aktivitě kopírování **podřízený** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ jímky kopie aktivity musí nastavena: **AzureTableSink** |Ano |
| azureTableDefaultPartitionKeyValue |Výchozí hodnotu klíče oddílu, mohou být využívána jímky. |Ne |
| azureTablePartitionKeyName |Zadejte název sloupce, jejichž hodnoty se používají jako klíče oddílů. Pokud není zadaný, použije se jako klíč oddílu "AzureTableDefaultPartitionKeyValue". |Ne |
| azureTableRowKeyName |Zadejte název sloupce, jejichž hodnoty sloupce jsou použity jako klíč řádku. Pokud není zadaný, použijte identifikátor GUID pro každý řádek. |Ne |
| azureTableInsertType |Režim vložení dat do tabulky Azure. Tato vlastnost určuje, jestli mají existující řádky v tabulce output s odpovídajícím klíče oddílu a řádku jejich hodnoty nahradit nebo sloučit. <br/><br/>Povolené hodnoty jsou: **sloučení** (výchozí), a **nahradit**. <br/><br> Toto nastavení se vztahuje na úrovni řádků, není úrovni tabulky a ani možnost odstraní řádků do výstupní tabulky, které nejsou k dispozici ve vstupu. Další informace o tom, jak tato nastavení (sloučení a nahraďte) fungují, najdete v části [vložení nebo sloučení Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) a [vložení nebo nahrazení Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) témata. |Ne |
| writeBatchSize |Když je dosaženo writeBatchSize nebo writeBatchTimeout vkládá data do tabulky Azure.<br/>Povolené hodnoty jsou: celé číslo (počet řádků) |Ne (výchozí hodnota je 10000) |
| writeBatchTimeout |Když je dosaženo writeBatchSize nebo writeBatchTimeout vkládá data do tabulky Azure.<br/>Povolené hodnoty jsou: časový interval. Příklad: "00:20:00" (20 minut) |Ne (výchozí hodnota je 90 sec – výchozí hodnota časového limitu klienta úložiště) |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyToAzureTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Table output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureTableSink",
                "azureTablePartitionKeyName": "<column name>",
                "azureTableRowKeyName": "<column name>"
            }
        }
    }
]
```

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName

Zdrojový sloupec namapujte na cílový sloupec pomocí vlastnosti "překladač", abyste mohli používat jako azureTablePartitionKeyName cílový sloupec.

V následujícím příkladu je zdrojový sloupec DivisionID namapována na cílový sloupec DivisionID.

```json
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```

"DivisionID" je zadán jako klíč oddílu.

```json
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID"
}
```

## <a name="data-type-mapping-for-azure-table"></a>Mapování datového typu pro Azure Table

Při kopírování dat z/do tabulky Azure, se používají následující mapování z Azure Table datové typy k Azure Data Factory dočasné datové typy. V tématu [schéma a data zadejte mapování](copy-activity-schema-and-type-mapping.md) a zjistěte, jak aktivity kopírování mapuje zdroje schéma a data typ jímky.

Při přesunu dat do a z Azure Table, následující [mapování definovaná službou Azure Table](https://msdn.microsoft.com/library/azure/dd179338.aspx) se používají ve typy OData tabulky Azure na typ .NET a naopak.

| Azure Table datový typ | Typ průběžných dat objektu pro vytváření dat | Podrobnosti |
|:--- |:--- |:--- |
| Edm.Binary |Byte] |Pole bajtů až 64 KB. |
| Edm.Boolean |BOOL |Logická hodnota. |
| Edm.DateTime |Data a času |Hodnota 64-bit, vyjádřené jako koordinovaný světový čas (UTC). Podporovaný rozsah datum a čas zahájení z 12:00 hodin, 1, 1601. ledna (C.E.), UTC. Rozsah končí u 31. prosince 9999. |
| Edm.Double |Double |Hodnota 64-bit plovoucí bodu. |
| Edm.Guid |Identifikátor GUID |Globálně jedinečný identifikátor 128-bit. |
| Edm.Int32 |Int32 |32bitové celé číslo. |
| Edm.Int64 |Int64 |64bitové celé číslo. |
| Edm.String |Řetězec |Hodnota kódování UTF-16. Řetězcové hodnoty může mít až 64 KB. |

## <a name="next-steps"></a>Další kroky
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats).