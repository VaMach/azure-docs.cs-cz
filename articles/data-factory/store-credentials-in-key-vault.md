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
ms.date: 09/26/2017
ms.author: jingwang
ms.openlocfilehash: 193d7c77f01384106b3e0d932d02ba6cdff9e750
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="store-credential-in-azure-key-vault"></a>Uložení přihlašovacích údajů v Azure Key Vault

Můžete uložit přihlašovací údaje k úložišti dat [Azure Key Vault](../key-vault/key-vault-whatis.md). Při provádění aktivity, která používá úložiště dat, načte Azure Data Factory přihlašovací údaje. V současné době pouze [Dynamics konektor](connector-dynamics-crm-office-365.md) tuto funkci podporovat.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [documenttion pro vytváření dat version1](v1/data-factory-introduction.md).

## <a name="steps"></a>Kroky

Při vytváření objekt pro vytváření dat, lze vytvořit identitu služby společně s vytvoření objektu pro vytváření. Identita služby je spravované aplikaci zaregistrovat do Azure Directory aktivity a představuje této konkrétní datové továrně.

- Při vytváření objektu pro vytváření dat prostřednictvím **portál Azure nebo PowerShell**, identita služby vždy se vytvoří automaticky od verze public preview.
- Při vytváření objektu pro vytváření dat prostřednictvím **SDK**, identita služby se vytvoří pouze v případě, že zadáte "Identity = nové FactoryIdentity()" v objektu factory pro vytváření. Viz příklad z [.NET rychlý start - vytváření dat](quickstart-create-data-factory-dot-net.md#create-a-data-factory).
- Při vytváření objektu pro vytváření dat prostřednictvím **REST API**, identita služby se vytvoří pouze v případě, že zadáte části "identity" v textu žádosti. Viz příklad z [REST rychlý start - vytvořit objekt pro vytváření dat](quickstart-create-data-factory-rest-api.md#create-a-data-factory).

Chcete-li přihlašovací údaje uložené v Azure Key Vault, budete muset:

1. Zkopírujte "IDENTITY ID aplikace služby" generované spolu s datovou továrnu. Odkazovat na [načíst identitu služby](#retrieve-service-identity).
2. Udělte přístup identity služby Azure Key Vault. V trezoru klíčů -> přístup k řízení -> Přidat -> hledání toto ID služby Identita aplikace přidat oprávnění čtečky. To umožňuje toto určený objekt pro vytváření pro přístup k tajný klíč v trezoru klíčů.
3. Vytvoření propojené služby odkazující na Azure Key Vault. Odkazovat na [propojená služba Azure Key Vault](#azure-key-vault-linked-service).
4. Vytvořte propojenou službu úložiště dat, ve které odkaz odpovídající tajného klíče uložené v trezoru. Odkazovat na [odkazovat přihlašovací údaje uložené v trezoru klíčů](#reference-credential-stored-in-key-vault).

## <a name="retrieve-service-identity"></a>Načtení identita služby

Můžete načíst identitu služby z portálu Azure nebo prostřednictvím kódu programu. Následující části vysvětlují, některá ukázky.

>[!TIP]
> Pokud nevidíte identita služby [generovat identita služby](#generate-service-identity) aktualizací vašeho objektu pro vytváření.

### <a name="using-azure-portal"></a>Pomocí webu Azure Portal

-> Informace o identitě služby z portálu Azure můžete najít datovou továrnu -> Nastavení -> vlastnosti:

- ID IDENTITY SLUŽBY
- SLUŽBA IDENTITY KLIENTA
- **ID aplikace IDENTITY služby** > zkopírovat tato hodnota k udělení přístupu v Key Vault

![Načtení identita služby](media/store-credentials-in-key-vault/retrieve-service-identity-portal.png)

### <a name="using-powershell"></a>Pomocí prostředí PowerShell

Identita služby ID objektu zabezpečení a ID klienta bude vrácen, pokud objekt pro vytváření konkrétních dat získáte následujícím způsobem:

```powershell
PS C:\WINDOWS\system32> (Get-AzureRmDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName>).Identity

PrincipalId                          TenantId
-----------                          --------
765ad4ab-XXXX-XXXX-XXXX-51ed985819dc 72f988bf-XXXX-XXXX-XXXX-2d7cd011db47
```

Zkopírujte ID objektu zabezpečení a pak spusťte následující příkaz služby Azure Active Directory s ID objektu zabezpečení jako parametr získat **ApplicationId**, který můžete použít k udělení přístupu v Key Vault:

```powershell
PS C:\WINDOWS\system32> Get-AzureRmADServicePrincipal -ObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc

ServicePrincipalNames : {76f668b3-XXXX-XXXX-XXXX-1b3348c75e02, https://identity.azure.net/P86P8g6nt1QxfPJx22om8MOooMf/Ag0Qf/nnREppHkU=}
ApplicationId         : 76f668b3-XXXX-XXXX-XXXX-1b3348c75e02
DisplayName           : ADFV2DemoFactory
Id                    : 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
Type                  : ServicePrincipal
```

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

## <a name="generate-service-identity"></a>Generovat identita služby

Pokud můžete najít objekt pro vytváření dat nemá identity služby přidružené následující [načíst identitu služby](#retrieve-service-identity) instrukce, můžete vygenerovat jeden při aktualizaci objektu pro vytváření dat s iniciátor identity prostřednictvím kódu programu.

> [!NOTE]
> - **Identita služby nelze změnit**. Aktualizace služby data factory, který už máte identity služby nebude mít žádný vliv, identita služby zůstanou beze změny.
> - **Identita služby nelze odstranit,**. Pokud aktualizujete objekt pro vytváření dat, které už máte identitu služby bez zadání parametru "identity" v objektu factory nebo bez zadání oddíl "identity" v textu požadavku REST, zobrazí se chyba.

Následující části vysvětlují, některá ukázky na generování identita služby pro vaše objekt pro vytváření, pokud neexistuje.

### <a name="using-powershell"></a>Pomocí prostředí PowerShell

Volání **Set-AzureRmDataFactoryV2** příkaz znovu, pak najdete v části "Identity" polí nově generován:

```powershell
PS C:\WINDOWS\system32> Set-AzureRmDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName> -Location <region>

DataFactoryName   : ADFV2DemoFactory
DataFactoryId     : /subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory
ResourceGroupName : <resourceGroupName>
Location          : East US
Tags              : {}
Identity          : Microsoft.Azure.Management.DataFactory.Models.FactoryIdentity
ProvisioningState : Succeeded
```

### <a name="using-rest-api"></a>Pomocí rozhraní REST API

Volání níže rozhraní API s část "identity" v textu požadavku:

```
PATCH https://management.azure.com/subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<data factory name>?api-version=2017-09-01-preview
```

**Text žádosti**: přidání "identity": {"typ": "SystemAssigned"}.

```json
{
    "name": "<dataFactoryName>",
    "location": "<region>",
    "properties": {},
    "identity": {
        "type": "SystemAssigned"
    }
}
```

**Odpověď**: identita služby se vytvoří automaticky a oddíl "identity" je vyplněný, odpovídajícím způsobem.

```json
{
    "name": "ADFV2DemoFactory",
    "tags": {},
    "properties": {
        "provisioningState": "Succeeded",
        "loggingStorageAccountKey": "**********",
        "createTime": "2017-09-26T04:10:01.1135678Z",
        "version": "2017-09-01-preview"
    },
    "identity": {
        "type": "SystemAssigned",
        "principalId": "765ad4ab-XXXX-XXXX-XXXX-51ed985819dc",
        "tenantId": "72f988bf-XXXX-XXXX-XXXX-2d7cd011db47"
    },
    "id": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory",
    "type": "Microsoft.DataFactory/factories",
    "location": "EastUS"
}
```

### <a name="using-sdk"></a>Pomocí sady SDK

Volání funkce create_or_update objekt pro vytváření dat s identitou = nové FactoryIdentity(). Ukázkový kód pomocí rozhraní .NET:

```csharp
Factory dataFactory = new Factory
{
    Location = <region>,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
```

## <a name="next-steps"></a>Další kroky
Seznam úložišť dat jako zdroje a jímky nepodporuje aktivitu kopírování v Azure Data Factory najdete v tématu [podporovanými úložišti dat](copy-activity-overview.md##supported-data-stores-and-formats).