---
title: "Kopírování dat ze služby Salesforce pomocí Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak zkopírovat data ze služby Salesforce do úložiště dat podporovaných podřízený pomocí aktivity kopírování v kanál služby Azure Data Factory."
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
ms.openlocfilehash: c819b3e3e715427632e793ce77d31554914d248e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="copy-data-from-salesforce-using-azure-data-factory"></a>Kopírování dat ze služby Salesforce pomocí Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-salesforce-connector.md)
> * [Verze 2 – Preview](connector-salesforce.md)

Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat z databáze služby Salesforce. Vychází [zkopírujte aktivity přehled](copy-activity-overview.md) článek, který představuje obecný přehled aktivity kopírování.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [konektor služby Salesforce v V1](v1/data-factory-salesforce-connector.md).

## <a name="supported-scenarios"></a>Podporované scénáře

Z databáze služby Salesforce můžete zkopírovat data do úložiště dat žádné podporované jímky. Seznam úložišť dat, které jsou podporovány jako zdroje nebo jímky aktivitě kopírování najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.

Konkrétně tento konektor služby Salesforce podporuje následujících edice Salesforce: **Developer Edition, edice Professional, Enterprise Edition nebo neomezená Edition**. A podporuje kopírování dat ze služby Salesforce **výroby, izolovaný prostor a vlastní domény**.

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
| environmentUrl | Zadejte adresu URL služby Salesforce instanci. <br><br> -Výchozí hodnota je `"https://login.salesforce.com"`. <br> -Ke zkopírování dat z izolovaného prostoru, zadejte `"https://test.salesforce.com"`. <br> -Ke zkopírování dat z vlastní domény, zadejte, například `"https://[domain].my.salesforce.com"`. |Ne |
| uživatelské jméno |Zadejte uživatelské jméno pro uživatelský účet. |Ano |
| heslo |Zadejte heslo pro uživatelský účet. |Ano |
| securityToken |Zadejte token zabezpečení pro uživatelský účet. V tématu [získal token zabezpečení](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) pokyny o tom, jak resetování nebo získat token zabezpečení. Obecné informace o tokeny zabezpečení najdete v tématu [zabezpečení a rozhraní API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Ano |

**Příklad:**

```json
{
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
        }
    },
    "name": "SalesforceLinkedService"
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady najdete v článku datové sady. Tato část obsahuje seznam vlastností nepodporuje datová sada služby Salesforce.

Chcete-li kopírovat data ze služby Salesforce, nastavte vlastnost typu datové sady, která **RelationalTable**. Podporovány jsou následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu datové sady musí být nastavena na: **RelationalTable** | Ano |
| tableName | Název tabulky v databázi služby Salesforce. | Ne (když je zadán zdroj aktivity "dotaz") |

> [!IMPORTANT]
> Část "__c" název rozhraní API je potřeba pro všechny vlastní objekt.

![Název objektu pro vytváření – připojení Salesforce – rozhraní API](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

**Příklad:**

```json
{
    "name": "SalesforceDataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Salesforce linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable__c"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [kanály](concepts-pipelines-activities.md) článku. Tato část obsahuje seznam vlastností nepodporuje zdroje služby Salesforce.

### <a name="salesforce-as-source"></a>Salesforce jako zdroj

Ke kopírování dat ze služby Salesforce, nastavte typ zdroje v aktivitě kopírování do **RelationalSource**. Následující vlastnosti jsou podporovány v aktivitě kopírování **zdroj** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ zdroje kopie aktivity musí být nastavena na: **RelationalSource** | Ano |
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
                "type": "RelationalSource",
                "query": "SELECT Col_Currency__c, Col_Date__c, Col_Email__c FROM AllDataType__c"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="query-tips"></a>Typy dotazů

### <a name="retrieving-data-from-salesforce-report"></a>Načítání dat ze sestavy služby Salesforce

Ze sestavy služby Salesforce můžete data načíst zadáním dotazu jako `{call "<report name>"}. Example: `"dotaz": "{volání \"TestReport\"}"'.

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>Načítání odstranit záznamy z funkce Koš služby Salesforce

Chcete-li prohledávat logicky odstraněné záznamy z funkce Koš služby Salesforce, můžete zadat **"IsDeleted = 1"** v dotazu. Například:

* Chcete-li prohledávat pouze odstraněné záznamy, zadejte "vybrat * z MyTable__c **kde IsDeleted = 1**"
* Chcete-li prohledávat všechny záznamy, včetně existující a Odstraněná, zadejte "vybrat * z MyTable__c **kde IsDeleted = 0 nebo IsDeleted = 1**"

### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>Načítání dat pomocí where klauzule ve sloupci data a času

Při zadejte SOQL nebo SQL dotaz, věnujte pozornost rozdíl formátu data a času. Například:

* **Ukázka SOQL**:`$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', <datetime parameter>, <datetime parameter>)`
* **Ukázka SQL**:`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', <datetime parameter>, <datetime parameter>)`

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
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md##supported-data-stores-and-formats).