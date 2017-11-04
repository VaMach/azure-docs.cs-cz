---
title: "Kopírování dat z aplikace Dynamics CRM a 365 pomocí Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak zkopírovat data z aplikace Dynamics CRM a 365 do úložiště dat podporovaných podřízený pomocí aktivity kopírování v kanál služby Azure Data Factory."
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
ms.openlocfilehash: aab7dfff0b77b5f09a1388c9bac9bdd63ebd8b17
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="copy-data-from-dynamics-365dynamics-crm-using-azure-data-factory"></a>Kopírování dat z Dynamics 365 / Dynamics CRM pomocí Azure Data Factory

Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat z Dynamics 365 / Dynamics CRM. Vychází [zkopírujte aktivity přehled](copy-activity-overview.md) článek, který představuje obecný přehled aktivity kopírování.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [aktivitu kopírování v V1](v1/data-factory-data-movement-activities.md).

## <a name="supported-scenarios"></a>Podporované scénáře

Data můžete zkopírovat z Dynamics 365 / Dynamics CRM do úložiště dat žádné podporované jímky. Seznam úložišť dat jako zdroje nebo jímky nepodporuje aktivitě kopírování najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.

Konkrétně tento Dynamics konektor podporuje následující verze Dynamics a typy ověřování:

| Dynamics verze | Typy ověřování | Ukázky propojené služby |
|:--- |:--- |:--- |
| Dynamics 365 online <br> Dynamics CRM online | Office 365 | [Dynamics Online + ověřování Office 365](#dynamics-365-and-dynamics-crm-online) |
| Dynamics 365 místně s IFD <br> Dynamics CRM 2016 místně s IFD <br> Dynamics CRM 2015 místně s IFD | IFD | [Dynamics místně s IFD + IFD ověřování](#dynamics-365-and-dynamics-crm-on-premises-with-ifd) |

*IFD je zkratka pro internetové nasazení.*

> [!NOTE]
> K používání konektoru Dynamics ukládat heslo v Azure Key Vault a nechat vyžádání aktivitu kopírování ADF odtud při kopírování dat. Zjistit, jak nakonfigurovat v [propojené vlastnosti služby](#linked-service-properties) části.

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
| heslo | Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno. Budete muset uvést heslo do Azure Key Vault a nakonfigurovat heslo jako "AzureKeyVaultSecret". Další informace najdete v tématu [ukládat přihlašovací údaje v Key Vault](store-credentials-in-key-vault.md) článku. | Ano |

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
                "secretName": "mySecret",
                "store":{
                    "linkedServiceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
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
| heslo | Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno. Všimněte si, že budete muset uvést heslo do Azure Key Vault a nakonfigurovat heslo jako "AzureKeyVaultSecret". Další informace najdete v tématu [ukládat přihlašovací údaje v Key Vault](store-credentials-in-key-vault.md) článku. | Ano |

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
                "secretName": "mySecret",
                "store":{
                    "linkedServiceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [datové sady](concepts-datasets-linked-services.md) článku. Tato část obsahuje seznam vlastností nepodporuje Dynamics datovou sadu.

Ke zkopírování dat z Dynamics, nastavte vlastnost typu datové sady, která **DynamicsEntity**. Podporovány jsou následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu datové sady musí být nastavena na: **DynamicsEntity** |Ano |
| entityName | Logický název entity načíst. | Ne (když je zadán zdroj aktivity "dotaz") |

> [!IMPORTANT]
> **V části "struktura" v datové sadě je vyžadována pro Dynamics**, která definuje název a datový typ sloupce pro Dynamics data, která chcete zkopírovat přes. Další informace z [strukturu datové sady](concepts-datasets-linked-services.md#dataset-structure) a [mapování datového typu pro Dynamics](#data-type-mapping-for-dynamics).

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

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [kanály](concepts-pipelines-activities.md) článku. Tato část obsahuje seznam vlastností nepodporuje Dynamics zdroje.

### <a name="dynamics-as-source"></a>Dynamics jako zdroj

Ke zkopírování dat z Dynamics, nastavte typ zdroje v aktivitě kopírování do **DynamicsSource**. Následující vlastnosti jsou podporovány v aktivitě kopírování **zdroj** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavena na: **DynamicsSource**  | Ano |
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

## <a name="data-type-mapping-for-dynamics"></a>Datový typ mapování pro Dynamics

Při kopírování dat z Dynamics, se používají následující mapování Dynamics datových typů k Azure Data Factory dočasné datové typy. V tématu [schéma a data zadejte mapování](copy-activity-schema-and-type-mapping.md) a zjistěte, jak aktivity kopírování mapuje zdroje schéma a data typ jímky.

Nakonfigurujte odpovídající datový typ ADF ve struktuře datové sady na základě vaší zdroje dat Dynamics zadejte pomocí níže mapování tabulky:

| Dynamics datový typ | Typ průběžných dat objektu pro vytváření dat |
|:--- |:--- |
| AttributeTypeCode.BigInt | Dlouhé |
| AttributeTypeCode.Boolean | Logická hodnota |
| AttributeType.Customer | Identifikátor GUID |
| AttributeType.DateTime | Data a času |
| AttributeType.Decimal | Decimal |
| AttributeType.Double | Double |
| AttributeType.EntityName | Řetězec |
| AttributeType.Integer | Int32 |
| AttributeType.Lookup | Identifikátor GUID |
| AttributeType.ManagedProperty | Logická hodnota |
| AttributeType.Memo | Řetězec |
| AttributeType.Money | Decimal |
| AttributeType.Owner | Identifikátor GUID |
| AttributeType.Picklist | Int32 |
| AttributeType.Uniqueidentifier | Identifikátor GUID |
| AttributeType.String | Řetězec |
| AttributeType.State | Int32 |
| AttributeType.Status | Int32 |

> [!NOTE]
> Datový typ Dynamics AttributeType.CalendarRules a AttributeType.PartyList nejsou podporovány.


## <a name="next-steps"></a>Další kroky
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md##supported-data-stores-and-formats).