---
title: "Kopírování dat z/do systému souborů pomocí Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak ke zkopírování dat z a podporovaných podřízený datová úložiště v systému souborů (nebo) z podporované zdrojové úložiště dat do systému souborů pomocí Azure Data Factory."
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
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: ff8fb061a5a5108e574860fa26d0d983ef5ffe99
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="copy-data-to-or-from-a-file-system-by-using-azure-data-factory"></a>Kopírovat data do nebo ze systému souborů pomocí Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-onprem-file-system-connector.md)
> * [Verze 2 – Preview](connector-file-system.md)

Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat z a do systému souborů. Vychází [zkopírujte aktivity přehled](copy-activity-overview.md) článek, který představuje obecný přehled aktivity kopírování.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [konektor systému souborů v V1](v1/data-factory-onprem-file-system-connector.md).

## <a name="supported-capabilities"></a>Podporované možnosti

Můžete zkopírovat data do úložiště dat žádné podporované podřízený ze systému souborů nebo zkopírování dat z jakékoli úložiště podporované zdroje dat do systému souborů. Seznam úložišť dat, které jsou podporovány jako zdroje nebo jímky aktivitě kopírování najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.

Konkrétně tento konektor systém souborů podporuje:

- Kopírování souborů z/do místního počítače nebo síťové sdílené složky. Chcete-li použít sdílenou složku systému Linux, nainstalovat [Samba](https://www.samba.org/) na Linux server.
- Kopírování souborů pomocí **Windows** ověřování.
- Kopírování souborů jako-je nebo analýza nebo generování souborů pomocí [podporované formáty souborů a komprese kodeky](supported-file-formats-and-compression-codecs.md).

## <a name="prerequisites"></a>Požadavky

Ke zkopírování dat z/do systému souborů, které nejsou veřejně přístupný, musíte nastavit Self-hosted integrace Runtime. V tématu [Self-hosted integrace Runtime](create-self-hosted-integration-runtime.md) článku.

## <a name="getting-started"></a>Začínáme

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Následující části obsahují podrobnosti o vlastnosti, které slouží k určení entit služby Data Factory, které jsou specifické pro systém souborů.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Pro soubor systému propojené služby jsou podporovány následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavena na: **souborovém serveru**. | Ano |
| hostitel | Určuje cestu kořenové složky, kterou chcete zkopírovat. Použít řídicí znak "\" pro speciální znaky v řetězci. V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady. | Ano |
| ID uživatele | Zadejte ID uživatele, který má přístup k serveru. | Ano |
| heslo | Zadejte heslo pro uživatele (ID uživatele). Toto pole označit jako SecureString bezpečně uložit v datové továrně nebo [odkazovat tajného klíče uložené v Azure Key Vault](store-credentials-in-key-vault.md). | Ano |
| connectVia | [Integrace Runtime](concepts-integration-runtime.md) který se má použít pro připojení k úložišti. (Pokud je veřejně přístupná data store), můžete použít modul Runtime integrace Self-hosted nebo Runtime integrace Azure. Pokud není zadaný, použije výchozí Runtime integrace Azure. |Ne |

### <a name="sample-linked-service-and-dataset-definitions"></a>Ukázkové propojené služby a definice datové sady

| Scénář | "hostitel" v definici propojené služby | "folderPath" v definici datové sady |
|:--- |:--- |:--- |
| Místní složka na počítači Runtime integrace: <br/><br/>Příklady: D:\\ \* nebo D:\folder\subfolder\\* |D:\\\\ |. \\ \\ nebo složky\\\\podsložky |
| Vzdálené sdílené složce: <br/><br/>Příklady: \\ \\myserver\\sdílet\\ \* nebo \\ \\myserver\\sdílet\\složky\\podsložky\\* |\\\\\\\\myserver\\\\share |. \\ \\ nebo složky\\\\podsložky |

**Příklad:**

```json
{
    "name": "FileLinkedService",
    "properties": {
        "type": "FileServer",
        "typeProperties": {
            "host": "<host>",
            "userid": "<domain>\\<user>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
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

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady najdete v článku datové sady. Tato část obsahuje seznam vlastností nepodporuje datovou sadu systému souborů.

Ke zkopírování dat z/do systému souborů, nastavte vlastnost typu datové sady, která **sdílení souborů**. Podporovány jsou následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu datové sady musí být nastavena na: **sdílení souborů** |Ano |
| folderPath | Cesta ke složce. V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady. |Ano |
| fileName | Zadejte název souboru do **folderPath** Pokud chcete zkopírovat do nebo z konkrétní soubor. Pokud nezadáte žádnou hodnotu pro tuto vlastnost, datová sada odkazuje na všechny soubory ve složce jako zdroj a automaticky vygeneruje název souboru.<br/><br/>**Automatické generování fileName pro sink:** Pokud není zadán název souboru pro datovou sadu výstupů a **preserveHierarchy** není zadané v aktivity podřízený aktivity kopírování vygeneruje název souboru s použitím následujících vzoru názvů: <br/>- `Data_[activity run id]_[GUID].[format].[compression if configured]`. Příklad: `Data_0a405f8a-93ff-4c6f-b3be-f69616f1df7a_0d143eda-d5b8-44df-82ec-95c50895ff80.txt.gz` <br/>- nebo `[Table name].[format].[compression if configured]` relační zdroje, když není zadaný dotaz. Příklad: MySourceTable.orc. |Ne |
| fileFilter | Zadejte filtr pro umožňuje vybrat podmnožinu souborů v folderPath, nikoli všech souborů. Platí, pouze pokud není zadán název souboru. <br/><br/>Povolené zástupné znaky jsou: `*` (více znaků) a `?` (jeden znak).<br/>– Příklad 1:`"fileFilter": "*.log"`<br/>-Příklad 2:`"fileFilter": 2017-09-??.txt"` |Ne |
| Formát | Pokud chcete **zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu v obou definice vstupní a výstupní datové sady.<br/><br/>Pokud chcete analyzovat nebo vygenerování soubory s konkrétním formátu, jsou podporovány následující typy souboru formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Nastavte **typ** vlastnost pod formát na jednu z těchto hodnot. Další informace najdete v tématu [textovém formátu](supported-file-formats-and-compression-codecs.md#text-format), [formátu Json](supported-file-formats-and-compression-codecs.md#json-format), [Avro formát](supported-file-formats-and-compression-codecs.md#avro-format), [Orc formátu](supported-file-formats-and-compression-codecs.md#orc-format), a [Parquet formát](supported-file-formats-and-compression-codecs.md#parquet-format) oddíly. |Ne (pouze pro scénář binární kopie) |
| Komprese | Zadejte typ a úroveň komprese pro data. Další informace najdete v tématu [podporované formáty souborů a komprese kodeky](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.<br/>Jsou podporované úrovně: **Optimal** a **nejrychlejší**. |Ne |


**Příklad:**

```json
{
    "name": "FileSystemDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<file system linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "myfile.csv.gz",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Vlastnosti aktivity kopírování

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [kanály](concepts-pipelines-activities.md) článku. Tato část obsahuje seznam vlastností podporovaný zdroj systému souborů a jímky.

### <a name="file-system-as-source"></a>Systém souborů jako zdroj

Ke kopírování dat ze systému souborů, nastavte typ zdroje v aktivitě kopírování do **FileSystemSource**. Následující vlastnosti jsou podporovány v aktivitě kopírování **zdroj** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ zdroje kopie aktivity musí být nastavena na: **FileSystemSource** |Ano |
| Rekurzivní | Určuje, jestli se data z podřízené složky nebo pouze do zadané složky Číst rekurzivně. Poznámka: když rekurzivní nastavena na hodnotu true a jímka je na základě souborů úložiště, prázdné složky nebo dílčí-folder nebudou zkopírovat nebo vytvořit v jímky.<br/>Povolené hodnoty jsou: **true** (výchozí), **false** | Ne |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyFromFileSystem",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<file system input dataset name>",
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
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="file-system-as-sink"></a>Systém souborů jako jímku

Ke zkopírování dat do systému souborů, nastavte typ jímky v aktivitě kopírování do **FileSystemSink**. Následující vlastnosti jsou podporovány v **podřízený** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ jímky kopie aktivity musí nastavena: **FileSystemSink** |Ano |
| copyBehavior | Definuje chování kopie, pokud je zdroj souborů z úložiště dat na základě souborů.<br/><br/>Povolené hodnoty jsou:<br/><b>-PreserveHierarchy (výchozí)</b>: zachovává hierarchii souborů v cílové složce. Relativní cesta zdrojového souboru do zdrojové složky je stejný jako relativní cestu k souboru cíl k cílové složce.<br/><b>-FlattenHierarchy</b>: všechny soubory ze zdrojové složky jsou v první úroveň cílové složce. Cílové soubory mít název automaticky generovány. <br/><b>-MergeFiles</b>: sloučí všechny soubory ze zdrojové složky pro jeden soubor. Pokud je zadán název souboru nebo objekt Blob, název souboru sloučené by být zadaný název; jinak by automaticky generovaný soubor název. | Ne |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyToFileSystem",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<file system output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "FileSystemSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="recursive-and-copybehavior-examples"></a>Příklady rekurzivní a copyBehavior

Tato část popisuje jejich výsledné chování pro různé kombinace hodnot rekurzivní a copyBehavior operace kopírování.

| Rekurzivní | copyBehavior | Struktura složky zdroje | Výsledný cíl |
|:--- |:--- |:--- |:--- |
| true (pravda) |preserveHierarchy | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | cílové složky složku1 je vytvořen s stejná struktura jako zdroj:<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| true (pravda) |flattenHierarchy | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | cíl složku1 je vytvořen s následující strukturou: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File5 |
| true (pravda) |mergeFiles | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | cíl složku1 je vytvořen s následující strukturou: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + soubor3 + File4 + soubor 5 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor |
| nepravda |preserveHierarchy | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Vytvoření cílové složky složku1 s následující strukturou<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>Subfolder1 s soubor3, File4 a File5 nejsou zachyceny. |
| nepravda |flattenHierarchy | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Vytvoření cílové složky složku1 s následující strukturou<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2<br/><br/>Subfolder1 s soubor3, File4 a File5 nejsou zachyceny. |
| nepravda |mergeFiles | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Vytvoření cílové složky složku1 s následující strukturou<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor. automaticky generovaný název File1<br/><br/>Subfolder1 s soubor3, File4 a File5 nejsou zachyceny. |

## <a name="next-steps"></a>Další postup
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md##supported-data-stores-and-formats).