---
title: "Zásady prostředků Azure | Microsoft Docs"
description: "Popisuje, jak pomocí Azure Resource Manager zásad na Ujistěte se, že jsou nastaveny vlastnosti konzistentní prostředku během nasazení. Zásady můžete použít na předplatné nebo prostředek skupiny."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: abde0f73-c0fe-4e6d-a1ee-32a6fce52a2d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/09/2017
ms.author: tomfitz
ms.openlocfilehash: 6faa8b4d259eddb1b26345d8900e829f6ca4dba8
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="resource-policy-overview"></a>Přehled zásad prostředků
Zásady prostředků umožňují vytvořit konvence pro prostředky ve vaší organizaci. Definováním konvence můžete řídit náklady a snadněji spravovat vaše prostředky. Například můžete zadat, že jsou povoleny pouze určité typy virtuálních počítačů. Nebo můžete vyžadovat, aby všechny prostředky měli konkrétní značku. Zásady jsou zdědí všechny podřízené prostředky. Ano Pokud je zásada pro skupinu prostředků, se vztahuje na všechny prostředky v příslušné skupině prostředků.

Existují dvě koncepty týkající se zásad:

* Definice zásad - popisují když je tato zásada vynucená a jaká opatření se mají provést
* přiřazení zásady - použít definice zásady oboru (předplatné nebo skupinu prostředků)

Toto téma se zaměřuje na definice zásady. Informace o přiřazení zásad najdete v tématu [portálu Azure použijte přiřadit a spravovat zásady prostředků](resource-manager-policy-portal.md) nebo [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md).

Při vytváření nebo aktualizaci prostředků (PUT a oprava operations), se vyhodnocují zásady.

## <a name="how-is-it-different-from-rbac"></a>Jak se liší od RBAC?
Existuje několik hlavní rozdíly mezi zásadami a řízení přístupu na základě role (RBAC). RBAC se zaměřuje na **uživatele** akce na různých místech. Například jste přidáni do role Přispěvatel pro skupinu prostředků na požadovaný rozsah, umožní vám provádět změny do této skupiny prostředků. Zásady se zaměřuje na **prostředků** vlastnosti během nasazení. Například prostřednictvím zásad, můžete řídit typy prostředků, které mohou být zřízeny. Nebo můžete omezit umístění, ve kterých se dá zřídit prostředky. Na rozdíl od RBAC, je povolit výchozí zásady a explicitní odepřít systému. 

Pokud chcete používat zásady, musí být ověřeny pomocí RBAC. Konkrétně, musí váš účet:

* `Microsoft.Authorization/policydefinitions/write`oprávnění k definování zásad
* `Microsoft.Authorization/policyassignments/write`oprávnění k přiřazení zásady 

Tato oprávnění nejsou součástí **Přispěvatel** role.

## <a name="built-in-policies"></a>Předdefinované zásady

Azure poskytuje definice předdefinované zásady, které může snížit počet zásad, které budete muset definovat. Před pokračováním definice zásady, byste měli zvážit, zda předdefinovaných zásad již obsahuje definice, které potřebujete. Definice předdefinovaných zásad jsou:

* Povolených umístění
* Typy povolené prostředků
* Povolené účet úložiště SKU
* Povolená SKU virtuálního počítače
* Použít značky a výchozí hodnota
* Vynutit značky a hodnota
* Není povoleno typy prostředků
* Vyžaduje systém SQL Server verze 12.0
* Vyžadovat šifrování účtu úložiště

## <a name="policy-definition-structure"></a>Definice strukturu zásad.
JSON použijete k vytvoření definice zásady. Definice zásad obsahuje prvky pro:

* Režim
* parameters
* Zobrazovaný název
* description
* Pravidlo zásad
  * logické vyhodnocení
  * Platnost

Následující příklad ukazuje zásadu, která omezuje, kde jsou nasazené prostředky:

```json
{
  "properties": {
    "mode": "all",
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## <a name="mode"></a>Režim

Doporučujeme, abyste nastavili `mode` k `all`. Pokud ji nastavíte na **všechny**, skupiny prostředků a všechny typy prostředků se vyhodnocují zásady. Portál využívá **všechny** pro všechny zásady. Pokud používáte prostředí PowerShell nebo rozhraní příkazového řádku Azure, budete muset zadat `mode` parametr a nastavte ji na **všechny**.
 
Dříve zásad byla vyhodnocena jako na pouze typy prostředků, které podporuje značky a umístění. `indexed` Režimu pokračuje toto chování. Pokud použijete **všechny** režimu, zásady se také vyhodnocují na typy prostředků, které nepodporují značky a umístění. [Podsíť virtuální sítě](https://github.com/Azure/azure-policy-samples/tree/master/samples/Network/enforce-nsg-on-subnet) je příkladem nově přidané typu. Kromě toho se vyhodnocují skupiny prostředků, pokud režim je nastaven na **všechny**. Například můžete [vynutit značky na skupinu prostředků](https://github.com/Azure/azure-policy-samples/tree/master/samples/ResourceGroup/enforce-resourceGroup-tags). 

## <a name="parameters"></a>Parametry
Pomocí parametrů pomáhá zjednodušit vaší zásady správy, protože snižuje počet definice zásady. Můžete definovat zásady pro vlastnost prostředku (například omezení umístění, kde můžete nasadit prostředky) a zahrnout parametry v definici. Pak můžete znovu použít, aby definice zásady pro různé scénáře předávání různé hodnoty (například určení jednu sadu umístění pro předplatné) při přiřazování zásady.

Parametry deklarovat, při vytváření definice zásad.

```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "The list of allowed locations for resources.",
      "displayName": "Allowed locations"
    }
  }
}
```

Typ parametru může být řetězec nebo pole. Vlastnost metadat slouží k nástroje, například portál Azure a zobrazte uživatelské informace. 

V pravidle zásady můžete odkazovat na parametry s následující syntaxí: 

```json
{ 
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="display-name-and-description"></a>Zobrazovaný název a popis

Můžete použít **displayName** a **popis** identifikovat definice zásady a poskytují kontext pro při použití.

## <a name="policy-rule"></a>Pravidlo zásad

Pravidla zásad se skládá z **Pokud** a **pak** bloky. V **Pokud** blok, můžete definovat jednu nebo víc podmínek, které určují, kdy je tato zásada vynucená. Logické operátory můžete použít pro tyto podmínky přesně definovat, tento scénář pro zásady. V **pak** bloku, definujete o tom, že se stane, když **Pokud** podmínky jsou splněny.

```json
{
  "if": {
    <condition> | <logical operator>
  },
  "then": {
    "effect": "deny | audit | append"
  }
}
```

### <a name="logical-operators"></a>Logické operátory
Jsou podporované logické operátory:

* `"not": {condition  or operator}`
* `"allOf": [{condition or operator},{condition or operator}]`
* `"anyOf": [{condition or operator},{condition or operator}]`

**Není** syntaxe Invertuje výběr výsledek podmínku. **AllOf** syntaxe (podobně jako logické **a** operaci) vyžaduje všechny podmínek. **AnyOf** syntaxe (podobně jako logické **nebo** operaci) vyžaduje jeden nebo více podmínek.

Logické operátory lze vnořit. Následující příklad ukazuje **není** operace, která je vnořená v rámci **allOf** operaci. 

```json
"if": {
  "allOf": [
    {
      "not": {
        "field": "tags",
        "containsKey": "application"
      }
    },
    {
      "field": "type",
      "equals": "Microsoft.Storage/storageAccounts"
    }
  ]
},
```

### <a name="conditions"></a>Podmínky
Je podmínka vyhodnocena jako jestli **pole** splňuje určitá kritéria. Jsou podporované podmínky:

* `"equals": "value"`
* `"like": "value"`
* `"match": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

Při použití **jako** podmínku, můžete zadat zástupný znak (*) v hodnotě.

Při použití **odpovídat** podmínky, zadejte `#` představují číslice, `?` písmeno a libovolný znak představují tento skutečný znak. Příklady najdete v tématu [prostředků pomocí zásad pro názvy a text](resource-manager-policy-naming-convention.md).

### <a name="fields"></a>Pole
Podmínky se vytváří pomocí pole. Pole představuje vlastnosti v datová část požadavku prostředku, který se používá k popisu stavu prostředku.  

Podporovány jsou následující pole:

* `name`
* `kind`
* `type`
* `location`
* `tags`
* `tags.*` 
* Vlastnost aliasy – seznam najdete v tématu [aliasy](#aliases).

### <a name="effect"></a>Efekt
Zásady podporuje tři typy efekt – `deny`, `audit`, `append`, `AuditIfNotExists`, a `DeployIfNotExists`. 

* **Odepřít** vygeneruje událost v protokolu auditování a požadavek se nezdaří
* **Audit** vygeneruje událost upozornění v protokolu auditu ale nesplní žádost
* **Připojit** přidá definovanou sadu pole k této žádosti 
* **AuditIfNotExists** -povolit auditování, pokud prostředek neexistuje.
* **DeployIfNotExists** – prostředek nasazení, pokud ještě neexistuje. V současné době tímto účelem je podporována pouze prostřednictvím integrovaných zásad.

Pro **připojit**, je nutné zadat následující podrobnosti:

```json
"effect": "append",
"details": [
  {
    "field": "field name",
    "value": "value of the field"
  }
]
```

Hodnota může být řetězec nebo objekt formátu JSON. 

S **AuditIfNotExists** a **DeployIfNotExists**, kterou můžete vyhodnotit existenci podřízených prostředků a pravidlo použít, když tento prostředek neexistuje. Například může vyžadovat, že sledovací proces sítě nasazuje pro všechny virtuální sítě.

Příklad audit, když není nasazený rozšíření virtuálního počítače, naleznete v části [rozšíření virtuálního počítače auditu](https://github.com/Azure/azure-policy-samples/blob/master/samples/Compute/audit-vm-extension/azurepolicy.json).

## <a name="aliases"></a>Aliasy

Vlastnost aliasy používáte pro přístup k vlastnosti specifické pro typ prostředku. Aliasy umožňují omezit, jaké hodnoty nebo podmínky jsou povoleny pro vlastnost prostředku. Každý alias mapuje cesty v různých verzích rozhraní API pro typ daného prostředku. Modul zásad během hodnocení zásad, získá vlastnost cesty pro tuto verzi rozhraní API.

**Microsoft.Cache/Redis**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Cache/Redis/enableNonSslPort | Jestli port serveru Redis bez ssl (6379) je povoleno nastavení. |
| Microsoft.Cache/Redis/shardCount | Nastavte počet horizontálních oddílů má být vytvořena na clusteru Cache ve verzi Premium.  |
| Microsoft.Cache/Redis/sku.capacity | Nastavení velikosti mezipaměti Redis k nasazení.  |
| Microsoft.Cache/Redis/sku.family | Nastavte řada SKU používat. |
| Microsoft.Cache/Redis/sku.name | Nastavte typ Redis Cache k nasazení. |

**Microsoft.Cdn/profiles**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.CDN/profiles/sku.name | Nastavte název cenovou úroveň. |

**Microsoft.Compute/disks**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Compute/imageOffer | Nastavte nabídku marketplace image použitá k vytvoření virtuálního počítače nebo image platformy. |
| Microsoft.Compute/imagePublisher | Nastavte vydavatele image platformy nebo marketplace image použitá k vytvoření virtuálního počítače. |
| Microsoft.Compute/imageSku | Nastavte SKU image platformy nebo marketplace image použitá k vytvoření virtuálního počítače. |
| Microsoft.Compute/imageVersion | Nastavte verzi image platformy nebo marketplace image použitá k vytvoření virtuálního počítače. |


**Microsoft.Compute/virtualMachines**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Compute/imageId | Nastavte identifikátor image použitá k vytvoření virtuálního počítače. |
| Microsoft.Compute/imageOffer | Nastavte nabídku marketplace image použitá k vytvoření virtuálního počítače nebo image platformy. |
| Microsoft.Compute/imagePublisher | Nastavte vydavatele image platformy nebo marketplace image použitá k vytvoření virtuálního počítače. |
| Microsoft.Compute/imageSku | Nastavte SKU image platformy nebo marketplace image použitá k vytvoření virtuálního počítače. |
| Microsoft.Compute/imageVersion | Nastavte verzi image platformy nebo marketplace image použitá k vytvoření virtuálního počítače. |
| Microsoft.Compute/licenseType | Nastavení, která obrázku nebo je licencovaný místní disk. Tato hodnota se používá pouze pro bitové kopie, které obsahují operační systém Windows Server.  |
| Microsoft.Compute/virtualMachines/imageOffer | Nastavte nabídku marketplace image použitá k vytvoření virtuálního počítače nebo image platformy. |
| Microsoft.Compute/virtualMachines/imagePublisher | Nastavte vydavatele image platformy nebo marketplace image použitá k vytvoření virtuálního počítače. |
| Microsoft.Compute/virtualMachines/imageSku | Nastavte SKU image platformy nebo marketplace image použitá k vytvoření virtuálního počítače. |
| Microsoft.Compute/virtualMachines/imageVersion | Nastavte verzi image platformy nebo marketplace image použitá k vytvoření virtuálního počítače. |
| Microsoft.Compute/virtualMachines/osDisk.Uri | Nastavte identifikátor URI virtuálního pevného disku. |
| Microsoft.Compute/virtualMachines/sku.name | Nastavení velikosti virtuálního počítače. |

**Microsoft.Compute/virtualMachines/extensions**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Compute/virtualMachines/extensions/publisher | Nastavte název rozšíření vydavatele. |
| Microsoft.Compute/virtualMachines/extensions/type | Nastavte typ rozšíření. |
| Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion | Nastavte verzi rozšíření. |

**Microsoft.Compute/virtualMachineScaleSets**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Compute/imageId | Nastavte identifikátor image použitá k vytvoření virtuálního počítače. |
| Microsoft.Compute/imageOffer | Nastavte nabídku marketplace image použitá k vytvoření virtuálního počítače nebo image platformy. |
| Microsoft.Compute/imagePublisher | Nastavte vydavatele image platformy nebo marketplace image použitá k vytvoření virtuálního počítače. |
| Microsoft.Compute/imageSku | Nastavte SKU image platformy nebo marketplace image použitá k vytvoření virtuálního počítače. |
| Microsoft.Compute/imageVersion | Nastavte verzi image platformy nebo marketplace image použitá k vytvoření virtuálního počítače. |
| Microsoft.Compute/licenseType | Nastavení, která obrázku nebo je licencovaný místní disk. Tato hodnota se používá pouze pro bitové kopie, které obsahují operační systém Windows Server. |
| Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix | Nastavte předpony názvu počítače pro všechny virtuální počítače v sadě škálování. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl | Nastavte identifikátor URI objektu blob bitové kopie uživatele. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers | Nastavte adresy URL kontejneru, které se používají k uložení disku operačního systému pro škálovací sadu. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.name | Nastavení velikosti virtuálních počítačů v sadě škálování. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.tier | Nastavte úroveň virtuálních počítačů v sadě škálování. |
  
**Microsoft.Network/applicationGateways**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Network/applicationGateways/sku.name | Nastavení velikosti brány. |

**Microsoft.Network/virtualNetworkGateways**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Network/virtualNetworkGateways/gatewayType | Nastavte typ tuto bránu virtuální sítě. |
| Microsoft.Network/virtualNetworkGateways/sku.name | Nastavte název SKU brány. |

**Microsoft.Sql/servers**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Sql/servers/version | Nastavte verzi serveru. |

**Microsoft.Sql/databases**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Sql/servers/databases/edition | Nastavte edici databáze. |
| Microsoft.Sql/servers/databases/elasticPoolName | Sada název fondu elastické databáze je v. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveId | Nastavte úroveň cíle ID databáze nakonfigurované služby. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveName | Nastavte název cíle na úrovni služby nakonfigurované databáze.  |

**Microsoft.Sql/elasticpools**

| Alias | Popis |
| ----- | ----------- |
| servery nebo elasticpools | Microsoft.Sql/servers/elasticPools/dtu | Nastavte celkový sdílené DTU fondu elastické databáze. |
| servery nebo elasticpools | Microsoft.Sql/servers/elasticPools/edition | Nastavte edici elastického fondu. |

**Microsoft.Storage/storageAccounts.**

| Alias | Popis |
| ----- | ----------- |
| Microsoft.Storage/storageAccounts/accessTier | Nastavte úroveň přístupu, které se používá pro fakturaci. |
| Microsoft.Storage/storageAccounts/accountType | Název SKU nastavte. |
| Microsoft.Storage/storageAccounts/enableBlobEncryption | Nastavte, jestli služba šifruje data, jak je uložen v úložišti služby objektů blob. |
| Microsoft.Storage/storageAccounts/enableFileEncryption | Nastavte, jestli služba šifruje data, jak je uložen v úložišti služby souboru. |
| Microsoft.Storage/storageAccounts/sku.name | Název SKU nastavte. |
| Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly | Nastavte, aby umožňovala pouze provoz https pro službu úložiště. |

## <a name="policy-sets"></a>Zásady skupiny

Zásady skupiny umožňují seskupit několik definice související zásad. Sadě zásad zjednodušuje přiřazení a správy vzhledem k tomu, že pracujete s skupiny jako jednu položku. Například můžete seskupit všechny související označování zásady v sadě jedna zásada. Přiřazovat jednotlivě každou zásadu, můžete použít sadu zásad.
 
Následující příklad ukazuje, jak vytvořit zásady pro nastavení pro zpracování dvě značky (costCenter a productName). Používá dvě předdefinované zásady pro použití výchozí hodnota značky a vynucování hodnota značky. Sadě zásad deklaruje dva parametry, costCenterValue a productNameValue pro – opětovné použití. Odkazuje na dva definice předdefinovaných zásad vícekrát s odlišnými parametry. Pro každý parametr fixní hodnotou, můžete buď zadat, jak je uvedeno pro tagName nebo nastavit parametr ze zásad, jak je uvedeno pro tagValue.

```json
{
    "properties": {
        "displayName": "Billing Tags Policy",
        "policyType": "Custom",
        "description": "Specify cost Center tag and product name tag",
        "parameters": {
            "costCenterValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for Cost Center tag"
                }
            },
            "productNameValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for product Name tag"
                }
            }
        },
        "policyDefinitions": [
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            }
        ]
    },
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/policySetDefinitions/billingTagsPolicy",
    "type": "Microsoft.Authorization/policySetDefinitions",
    "name": "billingTagsPolicy"
}
```

Přidat zásadu nastavit **New-AzureRMPolicySetDefinition** příkaz prostředí PowerShell.

Pro operace REST, použijte **2017-06-01-preview** verze rozhraní API, jak je znázorněno v následujícím příkladu:

```
PUT /subscriptions/<subId>/providers/Microsoft.Authorization/policySetDefinitions/billingTagsPolicySet?api-version=2017-06-01-preview
```

## <a name="next-steps"></a>Další kroky
* Po definování pravidla zásad, přiřaďte ho do oboru. K přiřazení zásad prostřednictvím portálu, najdete v části [portálu Azure použijte přiřadit a spravovat zásady prostředků](resource-manager-policy-portal.md). K přiřazení zásad pomocí rozhraní REST API, Powershellu nebo příkazového řádku Azure CLI, najdete v části [přiřadit a spravovat zásady prostřednictvím skriptu](resource-manager-policy-create-assign.md).
* Například zásady, najdete v části [úložiště GitHub zásad prostředků Azure](https://github.com/Azure/azure-policy-samples).
* Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).
* Schéma zásad je publikována v [http://schema.management.azure.com/schemas/2016-12-01/policyDefinition.json](http://schema.management.azure.com/schemas/2016-12-01/policyDefinition.json). 

