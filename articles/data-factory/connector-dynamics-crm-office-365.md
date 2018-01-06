---
title: "Kopírování dat z/do Dynamics CRM a 365 pomocí Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak zkopírovat data z aplikace Dynamics CRM a 365 k úložištím dat. podporované podřízený (nebo) z podporované zdrojové úložiště dat do aplikace Dynamics CRM a 365 pomocí aktivity kopírování v kanál služby Azure Data Factory."
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
ms.date: 01/05/2018
ms.author: jingwang
ms.openlocfilehash: b9b7091a8cb1de3eefcce77cbf82eedfcb33c787
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="copy-data-fromto-dynamics-365dynamics-crm-using-azure-data-factory"></a>Kopírování dat z/do Dynamics 365 / Dynamics CRM pomocí Azure Data Factory

Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat z a do Dynamics 365 / Dynamics CRM. Vychází [zkopírujte aktivity přehled](copy-activity-overview.md) článek, který představuje obecný přehled aktivity kopírování.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [aktivitu kopírování v V1](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Podporované možnosti

Můžete zkopírovat data z Dynamics 365 / Dynamics CRM do úložiště dat žádné podporované podřízený nebo zkopírování dat z jakékoli úložiště podporované zdroje dat do Dynamics 365 / Dynamics CRM. Seznam úložišť dat jako zdroje nebo jímky nepodporuje aktivitě kopírování najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.

Tento konektor Dynamics podporuje následující verze Dynamics a typy ověřování (*IFD je zkratka pro internetové nasazení*):

| Dynamics verze | Typy ověřování | Ukázky propojené služby |
|:--- |:--- |:--- |
| Dynamics 365 online <br> Dynamics CRM online | Office365 | [Dynamics Online + ověřování Office 365](#dynamics-365-and-dynamics-crm-online) |
| Dynamics 365 místně s IFD <br> Dynamics CRM 2016 místně s IFD <br> Dynamics CRM 2015 místně s IFD | IFD | [Dynamics místně s IFD + IFD ověřování](#dynamics-365-and-dynamics-crm-on-premises-with-ifd) |

Dynamics 365 konkrétně následující typy aplikací jsou podporované:

- Dynamics 365 pro prodej
- Dynamics 365 zákaznický servis
- Dynamics 365 služby pole
- Dynamics 365 projektu služby Automation
- Dynamics 365 pro Marketing

> [!NOTE]
> K používání konektoru Dynamics uložit heslo v Azure Key Vault a nechat vyžádání aktivitu kopírování z ní při kopírování dat provádění. Zjistit, jak nakonfigurovat v [propojené vlastnosti služby](#linked-service-properties) části.

## <a name="getting-started"></a>Začínáme

Vytvoření kanálu s aktivitou kopírování pomocí sady .NET SDK, Python SDK, Azure PowerShell, REST API nebo šablony Azure Resource Manageru. V tématu [kurzu aktivity kopírování](quickstart-create-data-factory-dot-net.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.

Následující části obsahují podrobnosti o vlastnosti, které se používají k definování entit služby Data Factory, které jsou specifické pro Dynamics.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Pro Dynamics propojené služby jsou podporovány následující vlastnosti:

### <a name="dynamics-365-and-dynamics-crm-online"></a>Dynamics 365 a Dynamics CRM Online

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavena na: **Dynamics**. | Ano |
| deploymentType | Typ nasazení Dynamics instance. Musí být **"Online"** pro Dynamics Online. | Ano |
| Název organizace | Název organizace Dynamics instance. | Ne, musí určovat po více než jedna instance Dynamics přidružené k uživateli. |
| authenticationType. | Typ ověřování pro připojení k serveru Dynamics. Zadejte **"Office 365"** pro Dynamics Online. | Ano |
| uživatelské jméno | Zadejte uživatelské jméno pro připojení k dynamiky. | Ano |
| heslo | Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno. Budete muset uvést heslo do Azure Key Vault a nakonfigurovat heslo jako "AzureKeyVaultSecret". Další informace z [ukládat přihlašovací údaje v Key Vault](store-credentials-in-key-vault.md). | Ano |
| connectVia | [Integrace Runtime](concepts-integration-runtime.md) který se má použít pro připojení k úložišti. Pokud není zadaný, použije výchozí Runtime integrace Azure. | Žádné zdroje, Ano pro sink Pokud zdroj propojené služby nemá reakcí na Incidenty |

>[!IMPORTANT]
>Při kopírování dat **do** Dynamics, výchozí Runtime integrace Azure nelze použít ke spuštění kopírování. V jiné aplikaci word, pokud vaše zdrojová propojené služby nemá zadaný IR, explicitně [vytvoření služby Azure IR](create-azure-integration-runtime.md#create-azure-ir) s umístěním téměř Dynamics a přidružení v propojené službě Dynamics jako v následujícím příkladu.

**Příklad: Dynamics online pomocí ověřování Office 365**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics online linked service using Office365 authentication",
        "typeProperties": {
            "deploymentType": "Online",
            "organizationName": "orga02d9c75",
            "authenticationType": "Office365",
            "username": "test@contoso.onmicrosoft.com",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name in AKV>",
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

### <a name="dynamics-365-and-dynamics-crm-on-premises-with-ifd"></a>Dynamics 365 a Dynamics CRM místně s IFD

*Další vlastnosti srovnáním Dyanmics online jsou "název hostitele" a "port".*

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavena na: **Dynamics**. | Ano |
| deploymentType | Typ nasazení Dynamics instance. Musí být **"OnPremisesWithIfd"** pro Dynamics místně s IFD.| Ano |
| **název hostitele** | Název hostitele serveru Dynamics místně. | Ano |
| **port** | Port serveru Dynamics místně. | Ne, výchozí hodnota je 443 |
| Název organizace | Název organizace Dynamics instance. | Ano |
| authenticationType. | Typ ověřování pro připojení k serveru Dynamics. Zadejte **"Ifd"** pro Dynamics místně s IFD. | Ano |
| uživatelské jméno | Zadejte uživatelské jméno pro připojení k dynamiky. | Ano |
| heslo | Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno. Všimněte si, že budete muset uvést heslo do Azure Key Vault a nakonfigurovat heslo jako "AzureKeyVaultSecret". Další informace z [ukládat přihlašovací údaje v Key Vault](store-credentials-in-key-vault.md). | Ano |
| connectVia | [Integrace Runtime](concepts-integration-runtime.md) který se má použít pro připojení k úložišti. Pokud není zadaný, použije výchozí Runtime integrace Azure. | Ne Ano pro sink zdroje |

>[!IMPORTANT]
>Můžete kopírovat data do Dynamics, explicitně [vytvoření služby Azure IR](create-azure-integration-runtime.md#create-azure-ir) s umístěním téměř Dynamics a přidružení v propojené službě jako v následujícím příkladu.

**Příklad: Dynamics místně s IFD pomocí IFD ověřování**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics on-premises with IFD linked service using IFD authentication",
        "typeProperties": {
            "deploymentType": "OnPremisesWithIFD",
            "hostName": "contosodynamicsserver.contoso.com",
            "port": 443,
            "organizationName": "admsDynamicsTest",
            "authenticationType": "Ifd",
            "username": "test@contoso.onmicrosoft.com",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name in AKV>",
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

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [datové sady](concepts-datasets-linked-services.md) článku. Tato část obsahuje seznam vlastností nepodporuje Dynamics datovou sadu.

Ke zkopírování dat z/do Dynamics, nastavte vlastnost typu datové sady, která **DynamicsEntity**. Podporovány jsou následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu datové sady musí být nastavena na: **DynamicsEntity** |Ano |
| entityName | Logický název entity načíst. | Ne pro zdroj (Pokud je zadán "dotaz" v zdroj aktivity), Ano pro sink |

> [!IMPORTANT]
>- **Při kopírování dat z Dynamics, v části "struktura" je vyžadován** v datové sadě Dynamics, která definuje název a datový typ sloupce pro Dynamics data, která chcete zkopírovat přes. Další informace z [strukturu datové sady](concepts-datasets-linked-services.md#dataset-structure) a [mapování datového typu pro Dynamics](#data-type-mapping-for-dynamics).
>- **Při kopírování dat do Dynamics, v části "struktura" je volitelný** v datové sadě Dynamics. Sloupce, které chcete zkopírovat do se určí na základě schématu datového zdroje. Pokud je zdrojem souboru CSV bez hlavičky, ve vstupní datové sady, zadejte "struktura" s název a datový typ sloupce, která se mapuje na pole v souboru CSV po jednom v pořadí.

**Příklad:**

```json
{
    "name": "DynamicsDataset",
    "properties": {
        "type": "DynamicsEntity",
        "structure": [
            {
                "name": "accountid",
                "type": "Guid"
            },
            {
                "name": "name",
                "type": "String"
            },
            {
                "name": "marketingonly",
                "type": "Boolean"
            },
            {
                "name": "modifiedon",
                "type": "Datetime"
            }
        ],
        "typePoperties": {
            "entityName": "account"
        },
        "linkedServiceName": {
            "referenceName": "<Dynamics linked service name>",
            "type": "linkedservicereference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [kanály](concepts-pipelines-activities.md) článku. Tato část obsahuje seznam vlastností nepodporuje Dynamics zdroj a jímka.

### <a name="dynamics-as-source"></a>Dynamics jako zdroj

Ke zkopírování dat z Dynamics, nastavte typ zdroje v aktivitě kopírování do **DynamicsSource**. Následující vlastnosti jsou podporovány v aktivitě kopírování **zdroj** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ zdroje kopie aktivity musí být nastavena na: **DynamicsSource**  | Ano |
| query  | FetchXML je proprietární dotazovací jazyk, který se používá v aplikaci Microsoft Dynamics (online & místně). Najdete v následujícím příkladu a další informace z [vytvořit dotazy s FeachXML](https://msdn.microsoft.com/en-us/library/gg328332.aspx). | Ne (když je určena "entityName" v datové sadě)  |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyFromDynamics",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Dynamics input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DynamicsSource",
                "query": "<FetchXML Query>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="sample-fetchxml-query"></a>Ukázkový dotaz FetchXML

```xml
<fetch>
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <attribute name="marketingonly" />
    <attribute name="modifiedon" />
    <order attribute="modifiedon" descending="false" />
    <filter type="and">
      <condition attribute ="modifiedon" operator="between">
        <value>2017-03-10 18:40:00z</value>
        <value>2017-03-12 20:40:00z</value>
      </condition>
    </filter>
  </entity>
</fetch>
```

### <a name="dynamics-as-sink"></a>Dynamics jako jímku

Ke zkopírování dat do Dynamics, nastavte typ jímky v aktivitě kopírování do **DynamicsSink**. Následující vlastnosti jsou podporovány v aktivitě kopírování **podřízený** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ jímky kopie aktivity musí nastavena: **DynamicsSink**  | Ano |
| WriteBehavior | Chování zápisu operace.<br/>Povolená hodnota je: **"Upsert"**. | Ano |
| writeBatchSize | Počet řádků dat zapsaných na Dynamics v každé dávce. | Ne (výchozí hodnota je 10) |
| ignoreNullValues | Označuje, zda Ignorovat hodnoty null ze vstupních dat (s výjimkou polí s klíči) během operace zápisu.<br/>Povolené hodnoty jsou: **true**, a **false**.<br>-true: ponechejte data v cílovém objektu, pokud při provádění operace upsert a update a vložte definován výchozí hodnota při provádění operace insert.<br/>-false: aktualizovat data v cílovém objektu na hodnotu NULL, při provádění operace upsert a update a vložit hodnotu NULL při provádění operace insert.  | Ne (výchozí hodnota je false) |

>[!NOTE]
>Výchozí hodnota podřízený writeBatchSize a kopie aktivity [parallelCopies](copy-activity-performance.md#parallel-copy) pro Dynamics sink jsou obě 10, což znamená, že 100 záznamů předán Dynamics současně.

**Příklad:**

```json
"activities":[
    {
        "name": "CopyToDynamics",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Dynamics output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "DynamicsSink",
                "writeBehavior": "Upsert",
                "writeBatchSize": 10,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="data-type-mapping-for-dynamics"></a>Datový typ mapování pro Dynamics

Při kopírování dat z Dynamics, se používají následující mapování Dynamics datových typů k Azure Data Factory dočasné datové typy. V tématu [schéma a data zadejte mapování](copy-activity-schema-and-type-mapping.md) a zjistěte, jak aktivity kopírování mapuje zdroje schéma a data typ jímky.

Nakonfigurujte odpovídající datový typ objektu pro vytváření dat ve struktuře datové sady na základě vaší zdroje dat Dynamics zadejte pomocí níže mapování tabulky:

| Dynamics datový typ | Typ průběžných dat objektu pro vytváření dat | Podporované jako zdroj | Podporované jako jímku |
|:--- |:--- |:--- |:--- |
| AttributeTypeCode.BigInt | Dlouhé | ✓ | ✓ |
| AttributeTypeCode.Boolean | Logická hodnota | ✓ | ✓ |
| AttributeType.Customer | Guid | ✓ |  |
| AttributeType.DateTime | Datum a čas | ✓ | ✓ |
| AttributeType.Decimal | Decimal | ✓ | ✓ |
| AttributeType.Double | Double | ✓ | ✓ |
| AttributeType.EntityName | Řetězec | ✓ | ✓ |
| AttributeType.Integer | Int32 | ✓ | ✓ |
| AttributeType.Lookup | Guid | ✓ |  |
| AttributeType.ManagedProperty | Logická hodnota | ✓ |  |
| AttributeType.Memo | Řetězec | ✓ | ✓ |
| AttributeType.Money | Decimal | ✓ | ✓ |
| AttributeType.Owner | Guid | ✓ | |
| AttributeType.Picklist | Int32 | ✓ | ✓ |
| AttributeType.Uniqueidentifier | Guid | ✓ | ✓ |
| AttributeType.String | Řetězec | ✓ | ✓ |
| AttributeType.State | Int32 | ✓ | ✓ |
| AttributeType.Status | Int32 | ✓ | ✓ |


> [!NOTE]
> Datový typ Dynamics AttributeType.CalendarRules a AttributeType.PartyList nejsou podporovány.

## <a name="next-steps"></a>Další postup
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats).