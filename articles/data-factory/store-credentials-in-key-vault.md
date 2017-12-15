---
title: "Ukládat přihlašovací údaje v Azure Key Vault | Microsoft Docs"
description: "Zjistěte, jak ukládat přihlašovací údaje pro úložiště dat používá v Azure trezoru klíčů, který Azure Data Factory můžete automaticky načíst za běhu."
services: data-factory
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2017
ms.author: jingwang
ms.openlocfilehash: 145c2bc0556010389e78e523fde6fd4b9063f930
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="store-credential-in-azure-key-vault"></a>Uložení přihlašovacích údajů v Azure Key Vault

Můžete uložit přihlašovací údaje k úložišti dat [Azure Key Vault](../key-vault/key-vault-whatis.md). Při provádění aktivity, která používá úložiště dat, načte Azure Data Factory přihlašovací údaje.

V současné době [Dynamics konektor](connector-dynamics-crm-office-365.md), [konektor služby Salesforce](connector-salesforce.md) a několik nově povolit konektory podporu této funkce. Očekávají, že další přicházející později. Můžete zkontrolovat každého tématu konektoru na podrobnosti. Pro tajný pole, které tuto funkci podporovat, zobrazí se poznámka v popis informací "*můžete označit toto pole jako SecureString bezpečně uložit v ADF, nebo uložit heslo v Azure Key Vault a nechat vyžádání aktivitu kopírování z ní Při kopírování dat - Další informace z přihlašovací údaje k úložišti v Key Vault.* "

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [dokumentace pro objekt pro vytváření dat version1](v1/data-factory-introduction.md).

## <a name="prerequisites"></a>Požadavky

Tato funkce závisí na identitu služby data factory. Zjistěte, jak to funguje z [identita služby Data factory](data-factory-service-identity.md) a zajistěte, aby objekt pro vytváření dat mají přidružený jeden.

## <a name="steps"></a>Kroky

Chcete-li přihlašovací údaje uložené v Azure Key Vault, budete muset:

1. [Načtení identita služby data factory](data-factory-service-identity.md#retrieve-service-identity) hodnotu "Služba IDENTITY ID aplikace" generované společně s vaší objekt pro vytváření.
2. Udělte přístup identity služby Azure Key Vault. V trezoru klíčů -> přístup k řízení -> Přidat -> hledání tuto aplikaci služby identity ID přidat aspoň **čtečky** oprávnění. To umožňuje toto určený objekt pro vytváření pro přístup k tajný klíč v trezoru klíčů.
3. Vytvoření propojené služby odkazující na Azure Key Vault. Odkazovat na [propojená služba Azure Key Vault](#azure-key-vault-linked-service).
4. Vytvořte propojenou službu úložiště dat, ve které odkaz odpovídající tajného klíče uložené v trezoru. Odkazovat na [odkazovat přihlašovací údaje uložené v trezoru klíčů](#reference-credential-stored-in-key-vault).

## <a name="azure-key-vault-linked-service"></a>Služba Azure Key Vault propojené

Pro Azure Key Vault propojené služby jsou podporovány následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu musí být nastavena na: **AzureKeyVault**. | Ano |
| BaseUrl | Zadejte adresu URL Azure Key Vault. | Ano |

**Příklad:**

```json
{
    "name": "AzureKeyVaultLinkedService",
    "properties": {
        "type": "AzureKeyVault",
        "typeProperties": {
            "baseUrl": "https://<azureKeyVaultName>.vault.azure.net"
        }
    }
}
```

## <a name="reference-credential-stored-in-key-vault"></a>Odkaz na přihlašovací údaje uložené v trezoru klíčů

Když konfigurujete pole v propojené službě odkazující na tajný klíč trezoru klíčů, jsou podporovány následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| type | Vlastnost typu pole musí být nastavena na: **AzureKeyVaultSecret**. | Ano |
| secretName | Název tajný klíč v azure trezoru klíčů. | Ano |
| secretVersion | Verze tajný klíč v azure trezoru klíčů.<br/>Pokud není zadaný, vždy používá nejnovější verze tajný klíč.<br/>-Li zadána, pak se přilepí na danou verzi.| Ne |
| úložiště | Odkazuje na služby Azure Key Vault propojené, který použijete k uložení pověření. | Ano |

**Příklad: (viz část "password")**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "typeProperties": {
            "deploymentType": "<>",
            "organizationName": "<>",
            "authenticationType": "<>",
            "username": "<>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="next-steps"></a>Další kroky
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats).