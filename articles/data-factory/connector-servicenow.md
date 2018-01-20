---
title: "Kopírování dat z ServiceNow pomocí Azure Data Factory (Beta) | Microsoft Docs"
description: "Zjistěte, jak zkopírovat data z ServiceNow k úložištím dat. podporované podřízený pomocí aktivity kopírování v kanál služby Azure Data Factory."
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
ms.openlocfilehash: d1e4d3a2d8edf061c5f16da62287359bd6039c69
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="copy-data-from-servicenow-using-azure-data-factory-beta"></a>Kopírování dat z ServiceNow pomocí Azure Data Factory (Beta)

Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat z ServiceNow. Vychází [zkopírujte aktivity přehled](copy-activity-overview.md) článek, který představuje obecný přehled aktivity kopírování.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [aktivitu kopírování v V1](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Tento konektor je aktuálně ve verzi Beta. Můžete si vyzkoušet a sdělte nám svůj názor. Nepoužívejte ji v produkčním prostředí.

## <a name="supported-capabilities"></a>Podporované možnosti

Data můžete zkopírovat z ServiceNow do úložiště dat žádné podporované jímky. Seznam úložišť dat, které jsou podporovány jako zdroje nebo jímky aktivitě kopírování najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.

Azure Data Factory poskytuje integrované ovladače pro umožnění připojení, proto nemusíte ručně nainstalovat všechny ovladače, používání tohoto konektoru.

## <a name="getting-started"></a>Začínáme

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

Následující části obsahují podrobnosti o vlastnosti, které slouží k určení konkrétní entity služby Data Factory ke konektoru ServiceNow.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Pro ServiceNow propojené služby jsou podporovány následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavena na: **ServiceNow** | Ano |
| endpoint | Koncový bod serveru ServiceNow. (to znamená, http://ServiceNowData.com)  | Ano |
| authenticationType. | Typ ověřování používat. <br/>Povolené hodnoty jsou: **základní**, **OAuth2** | Ano |
| uživatelské jméno | Uživatelské jméno používané pro připojení k serveru ServiceNow pro ověřování Basic a OAuth2.  | Ne |
| heslo | Heslo odpovídající uživatelské jméno pro ověřování Basic a OAuth2. Můžete zvolit označit toto pole jako SecureString bezpečně uložit v ADF nebo uložení hesla v Azure Key Vault a nechat aktivitě kopírování načítat z ní při kopírování dat – Další informace z [ukládat přihlašovací údaje v Key Vault](store-credentials-in-key-vault.md). | Ne |
| clientId | ID klienta pro ověřování OAuth2.  | Ne |
| clientSecret | Tajný klíč klienta pro ověřování OAuth2. Můžete zvolit označit toto pole jako SecureString bezpečně uložit v ADF nebo uložení hesla v Azure Key Vault a nechat aktivitě kopírování načítat z ní při kopírování dat – Další informace z [ukládat přihlašovací údaje v Key Vault](store-credentials-in-key-vault.md). | Ne |
| useEncryptedEndpoints | Určuje, zda jsou koncové body zdroje dat jsou šifrované pomocí protokolu HTTPS. Výchozí hodnota je true.  | Ne |
| useHostVerification | Určuje, jestli chcete vyžadovat názvu hostitele v certifikátu serveru, aby odpovídal názvu hostitele serveru při připojení přes protokol SSL. Výchozí hodnota je true.  | Ne |
| usePeerVerification | Určuje, jestli pro ověření totožnosti serveru při připojení přes protokol SSL. Výchozí hodnota je true.  | Ne |

**Příklad:**

```json
{
    "name": "ServiceNowLinkedService",
    "properties": {
        "type": "ServiceNow",
        "typeProperties": {
            "endpoint" : "http://ServiceNowData.com",
            "authenticationType" : "Basic",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [datové sady](concepts-datasets-linked-services.md) článku. Tato část obsahuje seznam vlastností nepodporuje ServiceNow datovou sadu.

Ke zkopírování dat z ServiceNow, nastavte vlastnost typu datové sady, která **ServiceNowObject**. Není k dispozici žádné další vlastnosti specifické pro typ v tomto typu datové sady.

**Příklad**

```json
{
    "name": "ServiceNowDataset",
    "properties": {
        "type": "ServiceNowObject",
        "linkedServiceName": {
            "referenceName": "<ServiceNow linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Zkopírovat vlastnosti aktivit

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [kanály](concepts-pipelines-activities.md) článku. Tato část obsahuje seznam vlastností nepodporuje ServiceNow zdroje.

### <a name="servicenowsource-as-source"></a>ServiceNowSource jako zdroj

Ke zkopírování dat z ServiceNow, nastavte typ zdroje v aktivitě kopírování do **ServiceNowSource**. Následující vlastnosti jsou podporovány v aktivitě kopírování **zdroj** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ zdroje kopie aktivity musí být nastavena na: **ServiceNowSource** | Ano |
| query | Čtení dat pomocí vlastního dotazu SQL. Například: `"SELECT * FROM alm.asset"`. | Ano |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyFromServiceNow",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ServiceNow input dataset name>",
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
                "type": "ServiceNowSource",
                "query": "SELECT * FROM alm.asset"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Další postup
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats).
