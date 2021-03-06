---
title: "Kopírování dat z SAP ECC pomocí Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak ke zkopírování dat z SAP ECC k úložištím dat. podporované podřízený pomocí aktivity kopírování v kanál služby Azure Data Factory."
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
ms.date: 02/27/2018
ms.author: jingwang
ms.openlocfilehash: efca2c129a1c7b8aca6b879d6d1311c6be157be1
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="copy-data-from-sap-ecc-using-azure-data-factory"></a>Kopírování dat z SAP ECC pomocí Azure Data Factory

Tento článek popisuje, jak pomocí aktivity kopírování v Azure Data Factory ke zkopírování dat z SAP ECC (ústřední součást Enterprise SAP). Vychází [zkopírujte aktivity přehled](copy-activity-overview.md) článek, který představuje obecný přehled aktivity kopírování.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [aktivitu kopírování v V1](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Podporované možnosti

Ze SAP ECC může kopírovat data do úložiště dat žádné podporované jímky. Seznam úložišť dat, které jsou podporovány jako zdroje nebo jímky aktivitě kopírování najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.

Konkrétně tento konektor SAP ECC podporuje:

- Kopírování dat z SAP ECC pro SAP NetWeaver verze 7.0 a vyšší. 
- Kopírování dat ze všech objektů, které jsou vystavené SAP ECC OData služby (například SAP tabulky nebo zobrazení, BAPI, extraktory dat atd.) nebo dat nebo Idoc posílá SAP platformy, které lze přijmout, jako OData pomocí relativní adaptérů.
- Kopírování dat pomocí základního ověřování.

## <a name="prerequisites"></a>Požadavky

Obecně platí SAP ECC zpřístupní entity prostřednictvím služeb OData prostřednictvím brány SAP. Chcete-li použít tento konektor SAP ECC, budete muset:

- **Nastaví bránu SAP**. Pro servery s vyšší než 7.4 SAP NetWeaver verzí je již nainstalována bráně SAP. Jinak je potřeba nainstalovat vestavěného okna brány nebo brána centra před úniku dat SAP ECC prostřednictvím služby OData. Zjistěte, jak nastavit bráně SAP z [Průvodce instalací](https://help.sap.com/saphelp_gateway20sp12/helpdata/en/c3/424a2657aa4cf58df949578a56ba80/frameset.htm).

- **Aktivaci a konfiguraci služby SAP OData**. Můžete si aktivovat služby OData prostřednictvím TCODE SICF v sekundách. Můžete taky nakonfigurovat, které objekty musí být zveřejněny. Zde je ukázka [podrobné pokyny](https://blogs.sap.com/2012/10/26/step-by-step-guide-to-build-an-odata-service-based-on-rfcs-part-1/).

## <a name="getting-started"></a>Začínáme

Vytvoření kanálu s aktivitou kopírování pomocí sady .NET SDK, Python SDK, Azure PowerShell, REST API nebo šablony Azure Resource Manageru. V tématu [kurzu aktivity kopírování](quickstart-create-data-factory-dot-net.md) podrobné pokyny k vytvoření kanálu s aktivitou kopírování.

Následující části obsahují podrobnosti o vlastnosti, které slouží k určení konkrétní entity služby Data Factory ke konektoru SAP ECC.

## <a name="linked-service-properties"></a>Vlastnosti propojené služby

Pro SAP ECC propojené služby jsou podporovány následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavena na: **SapEcc** | Ano |
| Adresa URL | Adresa url služby SAP ECC OData. | Ano |
| uživatelské jméno | Uživatelské jméno používané pro připojení k prostředí SAP ECC. | Ne |
| heslo | Heslo jako prostý text, který používá pro připojení k prostředí SAP ECC. | Ne |
| connectVia | [Integrace Runtime](concepts-integration-runtime.md) který se má použít pro připojení k úložišti. (Pokud je veřejně přístupná data store), můžete použít modul Runtime integrace Self-hosted nebo Runtime integrace Azure. Pokud není zadaný, použije výchozí Runtime integrace Azure. |Ne |

**Příklad:**

```json
{
    "name": "SapECCLinkedService",
    "properties": {
        "type": "SapEcc",
        "typeProperties": {
            "url": "<SAP ECC OData url e.g. http://eccsvrname:8000/sap/opu/odata/sap/zgw100_dd02l_so_srv/>",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        }
    },
    "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
    }
}
```

## <a name="dataset-properties"></a>Vlastnosti datové sady

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování datové sady, najdete v článku [datové sady](concepts-datasets-linked-services.md) článku. Tato část obsahuje seznam vlastností nepodporuje SAP ECC datovou sadu.

Ke zkopírování dat z SAP ECC, nastavte vlastnost typu datové sady, která **SapEccResource**. Podporovány jsou následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| path | Cesta SAP ECC OData entity. | Ano |

**Příklad**

```json
{
    "name": "SapEccDataset",
    "properties": {
        "type": "SapEccResource",
        "typePoperties": {
            "path": "<entity path e.g. dd04tentitySet>"
        },
        "linkedServiceName": {
            "referenceName": "<SAP ECC linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Vlastnosti aktivity kopírování

Úplný seznam oddílů a vlastnosti, které jsou k dispozici pro definování aktivity, najdete v článku [kanály](concepts-pipelines-activities.md) článku. Tato část obsahuje seznam vlastností nepodporuje SAP ECC zdroje.

### <a name="sap-ecc-as-source"></a>SAP ECC jako zdroj

Ke zkopírování dat z SAP ECC, nastavte typ zdroje v aktivitě kopírování do **SapEccSource**. Následující vlastnosti jsou podporovány v aktivitě kopírování **zdroj** části:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typ zdroje kopie aktivity musí být nastavena na: **SapEccSource** | Ano |
| query | Možnosti dotazu OData k filtrování dat Příklad: "$select = název, popis a $top = 10".<br/><br/>SAP ECC konektor zkopíruje data z adresy URL pro kombinované: (adresa url zadaná v propojené službě) / (cesty zadané v datové sadě)? (dotaz zadaný ve zdroji aktivita kopírování). Odkazovat na [OData pro adresy URL součásti](http://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Ano |

**Příklad:**

```json
"activities":[
    {
        "name": "CopyFromSAPECC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP ECC input dataset name>",
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
                "type": "SapEccSource",
                "query": "$top=10"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-ecc"></a>Mapování datového typu pro SAP ECC

Při kopírování dat z SAP ECC, následující mapování se používají OData datových typů pro SAP ECC data do Azure Data Factory dočasné datových typů. V tématu [schéma a data zadejte mapování](copy-activity-schema-and-type-mapping.md) a zjistěte, jak aktivity kopírování mapuje zdroje schéma a data typ jímky.

| Datový typ OData | Typ průběžných dat objektu pro vytváření dat |
|:--- |:--- |:--- |
| Edm.Binary | Řetězec |
| Edm.Boolean | Logická hodnota (Bool) |
| Edm.Byte | Řetězec |
| Edm.DateTime | DateTime |
| Edm.Decimal | Decimal |
| Edm.Double | Dvojitý |
| Edm.Single | Svobodný/svobodná |
| Edm.Guid | Řetězec |
| Edm.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | Řetězec |
| Edm.Time | TimeSpan |
| Edm.DateTimeOffset | DateTimeOffset |

> [!NOTE]
> Nyní nejsou podporovány komplexními datovými typy.

## <a name="next-steps"></a>Další postup
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats).
