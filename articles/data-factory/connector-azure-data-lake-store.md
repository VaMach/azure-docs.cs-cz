---
title: "Kopírování dat do/z Azure Data Lake Store pomocí služby Data Factory | Microsoft Docs"
description: "Zjistěte, jak zkopírovat data z podporované zdrojové úložiště dat do Azure Data Lake Store (nebo) z Data Lake Store k úložištím podporované podřízený pomocí služby Data Factory."
services: data-factory
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 11/01/2017
ms.author: jingwang
ms.openlocfilehash: 1e130fbf36d00a57563419670195fe356e8e5582
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="copy-data-to-or-from-azure-data-lake-store-by-using-azure-data-factory"></a>Kopírovat data do nebo z Azure Data Lake Store pomocí Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-azure-datalake-connector.md)
> * [Verze 2 – Preview](connector-azure-data-lake-store.md)

Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat do a z Azure Data Lake Store. Vychází [zkopírujte aktivity přehled](copy-activity-overview.md) článek, který představuje obecný přehled aktivity kopírování.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [konektor Azure Data Lake Store v V1](v1/data-factory-azure-datalake-connector.md).

## <a name="supported-scenarios"></a>Podporované scénáře

Můžete zkopírovat data z jakékoli úložiště podporované zdroje dat do Azure Data Lake Store nebo kopírování dat z Azure Data Lake Store do úložiště dat žádné podporované jímky. Seznam úložišť dat, které jsou podporovány jako zdroje nebo jímky aktivitě kopírování najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.

Konkrétně tento konektor Azure Data Lake Store podporuje:

- Kopírování souborů pomocí **instanční objekt** ověřování.
- Kopírování souborů jako-je nebo analýza nebo generování souborů pomocí [podporované formáty souborů a komprese kodeky](supported-file-formats-and-compression-codecs.md).

## <a name="get-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování pomocí sady .NET SDK, Python SDK, Azure PowerShell, REST API nebo šablony Azure Resource Manageru. V tématu [kurzu aktivity kopírování](quickstart-create-data-factory-dot-net.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování. 

Následující části obsahují podrobnosti o vlastnosti, které slouží k určení konkrétní entity služby Data Factory k Azure Data lake Store.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Služby Azure Data Lake Store propojené můžete vytvořit pomocí ověřování hlavní služby.

Pokud chcete použít ověřování hlavní služby, zaregistrujte entitu aplikace v Azure Active Directory (Azure AD) a jí udělit přístup k Data Lake Store. Podrobné pokyny najdete v tématu [Service-to-service ověřování](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Poznamenejte si následující hodnoty, které můžete použít k definování propojené služby:

- ID aplikace
- Klíč aplikace
- ID tenanta

>[!TIP]
> Ujistěte se, že udělíte hlavní správné oprávnění služby v Azure Data Lake Store:
>- Jako zdroj, udělte alespoň **čtení + Execute** oprávnění k seznamu a zkopírujte obsah složky, přístup k datům nebo **čtení** oprávnění zkopírovat jeden soubor. Žádný požadavek na řízení úrovně přístupu účtu.
>- Jako jímku, udělte alespoň **zápisu + provést** oprávnění k vytváření podřízených položek ve složce přístup k datům. A pokud používáte Azure Reakcí na základě kterých kopírování (zdroj a jímka mají v cloudu), aby mohli zjistit Data Lake Store oblasti služby Data Factory, udělte alespoň **čtečky** role v účtu řízení přístupu (IAM). Pokud chcete, aby se zabránilo této role IAM [vytvoření služby Azure IR](create-azure-integration-runtime.md#create-azure-ir) s umístění Data Lake Store a přidružení v Data Lake Store propojená služba jako v následujícím příkladu.

Podporovány jsou následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavená na **AzureDataLakeStore**. | Ano |
| dataLakeStoreUri | Informace o účtu Azure Data Lake Store. Tyto informace má jednu z následujících formátů: `https://[accountname].azuredatalakestore.net/webhdfs/v1` nebo `adl://[accountname].azuredatalakestore.net/`. | Ano |
| servicePrincipalId | Zadejte ID aplikace klienta. | Ano |
| servicePrincipalKey | Zadejte klíč aplikace. Toto pole můžete označte jako SecureString. | Ano |
| Klienta | Zadejte informace o klienta (název nebo klienta domény ID) v rámci které se nachází aplikace. Můžete ji načíst podržením ukazatele myši v pravém horním rohu portálu Azure. | Ano |
| subscriptionId | ID předplatného Azure, ke kterému patří účet Data Lake Store. | Vyžaduje se pro sink |
| Název skupiny prostředků | Název skupiny prostředků Azure, ke kterému patří účet Data Lake Store. | Vyžaduje se pro sink |
| connectVia | [Integrace Runtime](concepts-integration-runtime.md) který se má použít pro připojení k úložišti. (Pokud je vaše úložiště dat se nachází v privátní síti), můžete použít modul Runtime integrace Azure nebo Self-hosted integrace Runtime. Pokud není zadaný, použije výchozí Runtime integrace Azure. |Ne |

**Příklad:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady najdete v článku datové sady. Tato část obsahuje seznam vlastností nepodporuje datová sada Azure Data Lake Store.

Chcete-li kopírovat data do/z Azure Data Lake Store, nastavte vlastnost typu datové sady, která **AzureDataLakeStoreFile**. Podporovány jsou následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu datové sady musí být nastavena na: **AzureDataLakeStoreFile** |Ano |
| folderPath | Cesta ke kontejneru a složce v úložišti file. Příklad: rootfolder nebo podsložku / |Ano |
| fileName | Zadejte název souboru do **folderPath** Pokud chcete zkopírovat do nebo z konkrétní soubor. Pokud nezadáte žádnou hodnotu pro tuto vlastnost, datová sada odkazuje na všechny soubory ve složce.<br/><br/>Pokud není zadán název souboru pro datovou sadu výstupů a **preserveHierarchy** není zadané v aktivity podřízený aktivity kopírování automaticky vygeneruje název souboru v následujícím formátu: `Data.[activity run id GUID].[GUID if FlattenHierarchy].[format if configured].[compression if configured]`. Například: `Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz`. |Ne |
| Formát | Pokud chcete **zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu v obou definice vstupní a výstupní datové sady.<br/><br/>Pokud chcete analyzovat nebo vygenerování soubory s konkrétním formátu, jsou podporovány následující typy souboru formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Nastavte **typ** vlastnost pod formát na jednu z těchto hodnot. Další informace najdete v tématu [textovém formátu](supported-file-formats-and-compression-codecs.md#text-format), [formátu Json](supported-file-formats-and-compression-codecs.md#json-format), [Avro formát](supported-file-formats-and-compression-codecs.md#avro-format), [Orc formátu](supported-file-formats-and-compression-codecs.md#orc-format), a [Parquet formát](supported-file-formats-and-compression-codecs.md#parquet-format) oddíly. |Ne (pouze pro scénář binární kopie) |
| Komprese | Zadejte typ a úroveň komprese pro data. Další informace najdete v tématu [podporované formáty souborů a komprese kodeky](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.<br/>Jsou podporované úrovně: **Optimal** a **nejrychlejší**. |Ne |

**Příklad:**

```json
{
    "name": "ADLSDataset",
    "properties": {
        "type": "AzureDataLakeStoreFile",
        "linkedServiceName":{
            "referenceName": "<ADLS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "datalake/myfolder/",
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

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [kanály](concepts-pipelines-activities.md) článku. Tato část obsahuje seznam vlastností, které podporuje Azure Data Lake zdroj a jímka.

### <a name="azure-data-lake-store-as-source"></a>Azure Data Lake Store jako zdroj

Ke zkopírování dat z Azure Data Lake Store, nastavte typ zdroje v aktivitě kopírování do **AzureDataLakeStoreSource**. Následující vlastnosti jsou podporovány v aktivitě kopírování **zdroj** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ zdroje kopie aktivity musí být nastavena na: **AzureDataLakeStoreSource** |Ano |
| Rekurzivní | Označuje, zda je data načíst rekurzivně z dílčí složky nebo pouze do zadané složky.<br/>Povolené hodnoty jsou: **true** (výchozí), **false** | Ne |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyFromADLS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS input dataset name>",
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
                "type": "AzureDataLakeStoreSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-store-as-sink"></a>Azure Data Lake Store jako jímku

Ke zkopírování dat do objektu Blob Azure, nastavte typ jímky v aktivitě kopírování do **AzureDataLakeStoreSink**. Následující vlastnosti jsou podporovány v **podřízený** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ jímky kopie aktivity musí nastavena: **AzureDataLakeStoreSink** |Ano |
| copyBehavior | Definuje chování kopie, pokud je zdroj souborů z úložiště dat na základě souborů.<br/><br/>Povolené hodnoty jsou:<br/><b>-PreserveHierarchy (výchozí)</b>: zachovává hierarchii souborů v cílové složce. Relativní cesta zdrojového souboru do zdrojové složky je stejný jako relativní cestu k souboru cíl k cílové složce.<br/><b>-FlattenHierarchy</b>: všechny soubory ze zdrojové složky jsou v první úroveň cílové složce. Cílové soubory mít název automaticky generovány. <br/><b>-MergeFiles</b>: sloučí všechny soubory ze zdrojové složky pro jeden soubor. Pokud je zadán název souboru nebo objekt Blob, název souboru sloučené by být zadaný název; jinak by automaticky generovaný soubor název. | Ne |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyToADLS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataLakeStoreSink",
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
| Hodnota TRUE |preserveHierarchy | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | cílové složky složku1 je vytvořen s stejná struktura jako zdroj:<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| Hodnota TRUE |flattenHierarchy | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | cíl složku1 je vytvořen s následující strukturou: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File5 |
| Hodnota TRUE |mergeFiles | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | cíl složku1 je vytvořen s následující strukturou: <br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + soubor3 + File4 + soubor 5 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor |
| False |preserveHierarchy | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Vytvoření cílové složky složku1 s následující strukturou<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>Subfolder1 s soubor3, File4 a File5 nejsou zachyceny. |
| False |flattenHierarchy | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Vytvoření cílové složky složku1 s následující strukturou<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automaticky generovaný název File2<br/><br/>Subfolder1 s soubor3, File4 a File5 nejsou zachyceny. |
| False |mergeFiles | Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Soubor3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Vytvoření cílové složky složku1 s následující strukturou<br/><br/>Složku1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 obsah jsou sloučeny do jednoho souboru s názvem automaticky generovaný soubor. automaticky generovaný název File1<br/><br/>Subfolder1 s soubor3, File4 a File5 nejsou zachyceny. |

## <a name="next-steps"></a>Další kroky
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md##supported-data-stores-and-formats).