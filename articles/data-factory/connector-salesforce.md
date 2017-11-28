---
title: "Kopírování dat z/do služby Salesforce pomocí Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak zkopírovat data ze služby Salesforce k úložištím dat. podporované podřízený (nebo) z podporované zdrojové úložiště dat do služby Salesforce pomocí aktivity kopírování v kanál služby Azure Data Factory."
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
ms.date: 11/24/2017
ms.author: jingwang
ms.openlocfilehash: d0db2bd3a7e4d93a8d0690fcb4535c4552cef7ab
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/27/2017
---
# <a name="copy-data-fromto-salesforce-using-azure-data-factory"></a>Kopírování dat z/do služby Salesforce pomocí Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-salesforce-connector.md)
> * [Verze 2 – Preview](connector-salesforce.md)

Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat z a do služby Salesforce. Vychází [zkopírujte aktivity přehled](copy-activity-overview.md) článek, který představuje obecný přehled aktivity kopírování.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [konektor služby Salesforce v V1](v1/data-factory-salesforce-connector.md).

## <a name="supported-capabilities"></a>Podporované možnosti

Data ze služby Salesforce můžete zkopírovat do úložiště dat žádné podporované jímka, nebo nebo zkopírování dat z jakékoli úložiště podporované zdroje dat do služby Salesforce. Seznam úložišť dat, které jsou podporovány jako zdroje nebo jímky aktivitě kopírování najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.

Konkrétně tento konektor služby Salesforce podporuje:

- V následujících edicích Salesforce: **Developer Edition, edice Professional, Enterprise Edition nebo neomezená Edition**.
- Kopírování dat z/do služby Salesforce **výroby, izolovaný prostor a vlastní domény**.

## <a name="prerequisites"></a>Požadavky

* Rozhraní API oprávnění musí být v Salesforce povoleno. V tématu [povolení přístup pomocí rozhraní API v Salesforce sadou oprávnění?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)

## <a name="salesforce-request-limits"></a>Omezení žádostí o služby Salesforce

Služba Salesforce má limity pro celkový počet žádostí o rozhraní API a souběžných žádostí o rozhraní API. Je třeba počítat s následujícím:

- Pokud počet souběžných požadavků překročí limit, dojde k omezení šířky pásma a zobrazí náhodné chyby.
- Pokud celkový počet požadavků překračuje limit, zablokuje se účtu Salesforce ke 24 hodin.

V obou případech se může také zobrazí chyba "REQUEST_LIMIT_EXCEEDED". Najdete v části "API omezení počtu požadavků" v [Salesforce vývojáře omezení](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) článku.

## <a name="getting-started"></a>Začínáme
Vytvoření kanálu s aktivitou kopírování pomocí sady .NET SDK, Python SDK, Azure PowerShell, REST API nebo šablony Azure Resource Manageru. V tématu [kurzu aktivity kopírování](quickstart-create-data-factory-dot-net.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.

Následující části obsahují podrobnosti o vlastnosti, které se používají k definování entit služby Data Factory, které jsou specifické pro konektor služby Salesforce.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Pro služby Salesforce propojené služby jsou podporovány následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type |Vlastnost typu musí být nastavena na: **Salesforce**. |Ano |
| environmentUrl | Zadejte adresu URL služby Salesforce instanci. <br> -Výchozí hodnota je `"https://login.salesforce.com"`. <br> -Ke zkopírování dat z izolovaného prostoru, zadejte `"https://test.salesforce.com"`. <br> -Ke zkopírování dat z vlastní domény, zadejte, například `"https://[domain].my.salesforce.com"`. |Ne |
| uživatelské jméno |Zadejte uživatelské jméno pro uživatelský účet. |Ano |
| heslo |Zadejte heslo pro uživatelský účet.<br/><br/>Můžete označit toto pole jako SecureString bezpečně uložit v ADF nebo uložení hesla v Azure Key Vault a nechat ADF kopírování vyžádání aktivitu z ní při kopírování dat – Další informace z [ukládat přihlašovací údaje v Key Vault](store-credentials-in-key-vault.md). |Ano |
| securityToken |Zadejte token zabezpečení pro uživatelský účet. V tématu [získal token zabezpečení](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) pokyny o tom, jak resetování nebo získat token zabezpečení. Obecné informace o tokeny zabezpečení najdete v tématu [zabezpečení a rozhraní API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).<br/><br/>Můžete označit toto pole jako SecureString bezpečně uložit v ADF nebo ukládání tokenu zabezpečení v Azure Key Vault a nechat ADF kopírování vyžádání aktivitu z ní při kopírování dat – Další informace z [ukládat přihlašovací údaje v Key Vault](store-credentials-in-key-vault.md). |Ano |
| connectVia | [Integrace Runtime](concepts-integration-runtime.md) který se má použít pro připojení k úložišti. Pokud není zadaný, použije výchozí Runtime integrace Azure. | Ne Ano pro sink zdroje |

>[!IMPORTANT]
>Můžete kopírovat data do služby Salesforce, explicitně [vytvoření služby Azure IR](create-azure-integration-runtime.md#create-azure-ir) s umístěním téměř Salesforce a přidružení v propojené službě jako v následujícím příkladu.

**Příklad: ukládání přihlašovacích údajů ve službě ADF**

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            },
            "securityToken": {
                "type": "SecureString",
                "value": "<security token>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Příklad: ukládání přihlašovacích údajů v Azure Key Vault**

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of password in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            },
            "securityToken": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of security token in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
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

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [datové sady](concepts-datasets-linked-services.md) článku. Tato část obsahuje seznam vlastností nepodporuje datová sada služby Salesforce.

Ke zkopírování dat z/do služby Salesforce, nastavte vlastnost typu datové sady, která **SalesforceObject**. Podporovány jsou následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavena na: **SalesforceObject**  | Ano |
| objectApiName | Název objektu služby Salesforce k načtení dat z. | Ne Ano pro sink zdroje |

> [!IMPORTANT]
> Část "__c" název rozhraní API je potřeba pro všechny vlastní objekt.

![Název objektu pro vytváření – připojení Salesforce – rozhraní API](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

**Příklad:**

```json
{
    "name": "SalesforceDataset",
    "properties": {
        "type": "SalesforceObject",
        "linkedServiceName": {
            "referenceName": "<Salesforce linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "objectApiName": "MyTable__c"
        }
    }
}
```

>[!NOTE]
>Pro zpětné kompatibility, při kopírování dat ze služby Salesforce použití předchozí datové sady typu "RelationalTable" bude pokračovat v práci, zatímco jsou navrhované přepnout na nový typ "SalesforceObject".

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu datové sady musí být nastavena na: **RelationalTable** | Ano |
| tableName | Název tabulky v Salesforce. | Ne (když je zadán zdroj aktivity "dotaz") |

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [kanály](concepts-pipelines-activities.md) článku. Tato část obsahuje seznam vlastností nepodporuje Salesforce zdroj a jímka.

### <a name="salesforce-as-source"></a>Salesforce jako zdroj

Ke kopírování dat ze služby Salesforce, nastavte typ zdroje v aktivitě kopírování do **SalesforceSource**. Následující vlastnosti jsou podporovány v aktivitě kopírování **zdroj** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ zdroje kopie aktivity musí být nastavena na: **SalesforceSource** | Ano |
| query |Čtení dat pomocí vlastního dotazu. Můžete použít dotaz SQL 92 nebo [Salesforce objektu dotazu jazyka (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) dotazu. Například: `select * from MyTable__c`. | Ne (když je určena "tableName" v datové sadě) |

> [!IMPORTANT]
> Část "__c" název rozhraní API je potřeba pro všechny vlastní objekt.

![Název objektu pro vytváření – připojení Salesforce – rozhraní API](media/copy-data-from-salesforce/data-factory-salesforce-api-name-2.png)

**Příklad:**

```json
"activities":[
    {
        "name": "CopyFromSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Salesforce input dataset name>",
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
                "type": "SalesforceSource",
                "query": "SELECT Col_Currency__c, Col_Date__c, Col_Email__c FROM AllDataType__c"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

>[!NOTE]
>Pro zpětné kompatibility, při kopírování dat ze služby Salesforce pomocí předchozí zdroj kopie zadejte "RelationalSource" bude pokračovat v práci, zatímco jsou navrhované přepnout na nový typ "SalesforceSource".

### <a name="salesforce-as-sink"></a>Salesforce jako jímku

Ke zkopírování dat do služby Salesforce, nastavte typ jímky v aktivitě kopírování do **SalesforceSink**. Následující vlastnosti jsou podporovány v aktivitě kopírování **podřízený** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ jímky kopie aktivity musí nastavena: **SalesforceSink** | Ano |
| WriteBehavior | Chování zápisu pro danou operaci.<br/>Povolené hodnoty jsou: **vložit**, a **Upsert**. | Ne (výchozí hodnota je Insert) |
| externalIdFieldName | Název pole externí ID pro upsert operaci. Zadané pole musí být definován jako "Externí Id pole" v objektu Salesforce a nemůže mít odpovídající vstupních dat hodnoty NULL. | Ano pro "Upsert" |
| writeBatchSize | Počet řádků dat zapsaných na Salesforce v každé dávce. | Ne (výchozí hodnota je 5000) |
| ignoreNullValues | Určuje, zda Ignorovat hodnoty null ze vstupu dat během operace zápisu.<br/>Povolené hodnoty jsou: **true**, a **false**.<br>- **Hodnota TRUE,**: ponechejte data v cílovém objektu, pokud při provádění operace upsert a update a vložte definován výchozí hodnota při provádění operace insert.<br/>- **false**: aktualizovat data v cílovém objektu na hodnotu NULL, při provádění operace upsert a update a vložit hodnotu NULL při provádění operace insert. | Ne (výchozí hodnota je false) |

### <a name="example-salesforce-sink-in-copy-activity"></a>Příklad: Salesforce jímky v aktivitě kopírování

```json
"activities":[
    {
        "name": "CopyToSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Salesforce output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SalesforceSink",
                "writeBehavior": "Upsert",
                "externalIdFieldName": "CustomerId__c",
                "writeBatchSize": 10000,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="query-tips"></a>Typy dotazů

### <a name="retrieving-data-from-salesforce-report"></a>Načítání dat ze sestavy služby Salesforce

Ze sestavy služby Salesforce můžete data načíst zadáním dotazu jako `{call "<report name>"}`. Příklad: `"query": "{call \"TestReport\"}"`.

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>Načítání odstranit záznamy z funkce Koš služby Salesforce

Chcete-li prohledávat logicky odstraněné záznamy z funkce Koš služby Salesforce, můžete zadat **"IsDeleted = 1"** v dotazu. Například:

* Chcete-li prohledávat pouze odstraněné záznamy, zadejte "vybrat * z MyTable__c **kde IsDeleted = 1**"
* Chcete-li prohledávat všechny záznamy, včetně existující a Odstraněná, zadejte "vybrat * z MyTable__c **kde IsDeleted = 0 nebo IsDeleted = 1**"

### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>Načítání dat pomocí where klauzule ve sloupci data a času

Při zadejte SOQL nebo SQL dotaz, věnujte pozornost rozdíl formátu data a času. Například:

* **Ukázka SOQL**:`SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= @{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-ddTHH:mm:ssZ')} AND LastModifiedDate < @{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-ddTHH:mm:ssZ')}`
* **Ukázka SQL**:`SELECT * FROM Account WHERE LastModifiedDate >= {ts'@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}'} AND LastModifiedDate < {ts'@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'}"`

## <a name="data-type-mapping-for-salesforce"></a>Datový typ mapování pro Salesforce

Při kopírování dat ze služby Salesforce, se používají následující mapování ze služby Salesforce datové typy k Azure Data Factory dočasné datové typy. V tématu [schéma a data zadejte mapování](copy-activity-schema-and-type-mapping.md) a zjistěte, jak aktivity kopírování mapuje zdroje schéma a data typ jímky.

| Salesforce datový typ | Typ průběžných dat objektu pro vytváření dat |
|:--- |:--- |
| Automatické číslování |Řetězec |
| Zaškrtávací políčko |Logická hodnota |
| Měna |Double |
| Datum |Data a času |
| Datum a čas |Data a času |
| E-mail |Řetězec |
| ID |Řetězec |
| Relace hledání |Řetězec |
| Vybrat víc rozevíracího seznamu |Řetězec |
| Číslo |Double |
| Procento |Double |
| Telefon |Řetězec |
| Rozevírací seznam |Řetězec |
| Text |Řetězec |
| Textová oblast |Řetězec |
| Textová oblast (Long) |Řetězec |
| (Rich) textová oblast |Řetězec |
| Text (šifrované) |Řetězec |
| ADRESA URL |Řetězec |

## <a name="next-steps"></a>Další kroky
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats).