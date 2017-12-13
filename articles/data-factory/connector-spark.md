---
title: "Kopírování dat z Spark pomocí Azure Data Factory | Microsoft Docs"
description: "Postup kopírování dat z Spark do úložiště dat podporovaných podřízený pomocí aktivity kopírování v kanál služby Azure Data Factory."
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
ms.date: 11/30/2017
ms.author: jingwang
ms.openlocfilehash: b422b3a721511a25b976586cd324d65f383ad140
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/12/2017
---
# <a name="copy-data-from-spark-using-azure-data-factory"></a>Kopírování dat z Spark pomocí Azure Data Factory 

Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat z Spark. Vychází [zkopírujte aktivity přehled](copy-activity-overview.md) článek, který představuje obecný přehled aktivity kopírování.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [aktivitu kopírování v V1](v1/data-factory-data-movement-activities.md).


## <a name="supported-capabilities"></a>Podporované možnosti

Data můžete zkopírovat z Spark do úložiště dat žádné podporované jímky. Seznam úložišť dat, které jsou podporovány jako zdroje nebo jímky aktivitě kopírování najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.

Azure Data Factory poskytuje integrované ovladače pro umožnění připojení, proto nemusíte ručně nainstalovat všechny ovladače, používání tohoto konektoru.

## <a name="getting-started"></a>Začínáme

Vytvoření kanálu s aktivitou kopírování pomocí sady .NET SDK, Python SDK, Azure PowerShell, REST API nebo šablony Azure Resource Manageru. V tématu [kurzu aktivity kopírování](quickstart-create-data-factory-dot-net.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.

Následující části obsahují podrobnosti o vlastnosti, které slouží k určení konkrétní entity služby Data Factory ke konektoru Spark.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Pro Spark propojené služby jsou podporovány následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavena na: **Spark** | Ano |
| hostitele | IP adresa nebo název hostitele serveru Spark  | Ano |
| port | Port TCP, který používá Spark server naslouchat pro připojení klientů.  | Ano |
| Typ | Typ serveru Spark. <br/>Povolené hodnoty jsou: **SharkServer**, **SharkServer2**, **SparkThriftServer** | Ne |
| thriftTransportProtocol | Přenosový protokol pro použití v Thrift vrstvy. <br/>Povolené hodnoty jsou: **binární**, **SASL**, ** HTTP ** | Ne |
| authenticationType. | Metodu ověřování pro přístup k serveru Spark. <br/>Povolené hodnoty jsou: **anonymní**, **uživatelské jméno**, **UsernameAndPassword**, **WindowsAzureHDInsightService** | Ano |
| uživatelské jméno | Uživatelské jméno, který používáte pro přístup k serveru Spark.  | Ne |
| heslo | Heslo odpovídající uživatelské jméno, které jste zadali do pole uživatelské jméno, je možné označit toto pole jako SecureString uložit bezpečně v ADF, nebo uložit heslo v Azure Key Vault a nechat vyžádání aktivity kopírování z ní při kopírování dat - lea Další z nout [ukládat přihlašovací údaje v Key Vault](store-credentials-in-key-vault.md). | Ne |
| httpPath | Částečné adresa URL serveru Spark odpovídá.  | Ne |
| enableSsl | Určuje, zda jsou šifrované připojení k serveru pomocí protokolu SSL. Výchozí hodnota je false.  | Ne |
| trustedCertPath | Úplná cesta soubor .pem, který obsahuje certifikáty důvěryhodné certifikační Autority pro ověření serveru při připojení přes protokol SSL. Tuto vlastnost lze nastavit pouze při použití protokolu SSL na vlastním hostováním infračerveného signálu. Výchozí hodnota je soubor cacerts.pem nainstalované s infračerveného signálu.  | Ne |
| useSystemTrustStore | Určuje, jestli se má použít certifikát Certifikační autority z úložiště důvěryhodnosti systému nebo z určeného souboru PEM. Výchozí hodnota je false.  | Ne |
| allowHostNameCNMismatch | Určuje, jestli chcete vyžadovat protokol SSL vydaný certifikační Autoritou název certifikátu shodovat s názvem hostitele serveru při připojení přes protokol SSL. Výchozí hodnota je false.  | Ne |
| allowSelfSignedServerCert | Určuje, jestli se má povolit certifikáty podepsané svým držitelem ze serveru. Výchozí hodnota je false.  | Ne |
| connectVia | [Integrace Runtime](concepts-integration-runtime.md) který se má použít pro připojení k úložišti. (Pokud je veřejně přístupná data store), můžete použít modul Runtime integrace Self-hosted nebo Runtime integrace Azure. Pokud není zadaný, použije výchozí Runtime integrace Azure. |Ne |

**Příklad:**

```json
{
    "name": "SparkLinkedService",
    "properties": {
        "type": "Spark",
        "typeProperties": {
            "host" : "<cluster>.azurehdinsight.net",
            "port" : "<port>",
            "authenticationType" : "WindowsAzureHDInsightService",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            },
            "httpPath" : "gateway/sandbox/spark"
        }
    }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [datové sady](concepts-datasets-linked-services.md) článku. Tato část obsahuje seznam vlastností nepodporuje datovou sadu Spark.

Ke zkopírování dat z Spark, nastavte vlastnost typu datové sady, která **SparkObject**. Není k dispozici žádné další vlastnosti specifické pro typ v tomto typu datové sady.

**Příklad**

```json
{
    "name": "SparkDataset",
    "properties": {
        "type": "SparkObject",
        "linkedServiceName": {
            "referenceName": "<Spark linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [kanály](concepts-pipelines-activities.md) článku. Tato část obsahuje seznam vlastností, které jsou podporovány zdrojem Spark.

### <a name="sparksource-as-source"></a>SparkSource jako zdroj

Ke zkopírování dat z Spark, nastavte typ zdroje v aktivitě kopírování do **SparkSource**. Následující vlastnosti jsou podporovány v aktivitě kopírování **zdroj** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ zdroje kopie aktivity musí být nastavena na: **SparkSource** | Ano |
| query | Čtení dat pomocí vlastního dotazu SQL. Například: `"SELECT * FROM MyTable"`. | Ano |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyFromSpark",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Spark input dataset name>",
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
                "type": "SparkSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Další kroky
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats).
