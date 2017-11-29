---
title: "Azure definice strukturu zásad. | Microsoft Docs"
description: "Popisuje použití zásad definice prostředků zásadami Azure k vytvoření konvence pro prostředky ve vaší organizaci pomocí popisující, když je tato zásada vynucená a jaká opatření se mají provést."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/31/2017
ms.topic: article
ms.service: azure-policy
ms.custom: 
ms.openlocfilehash: 1b8fd12e071bfbd01567803370e510e7e07ccb99
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="azure-policy-definition-structure"></a>Struktura definic Azure Policy

Definice zásad prostředků používané zásad Azure umožňuje vytvořit konvence pro prostředky ve vaší organizaci prostřednictvím popisu, když je tato zásada vynucená a jaká opatření se mají provést. Definováním konvence můžete řídit náklady a snadněji spravovat vaše prostředky. Například můžete zadat, že jsou povoleny pouze určité typy virtuálních počítačů. Nebo můžete vyžadovat, aby všechny prostředky měli konkrétní značku. Zásady jsou zdědí všechny podřízené prostředky. Ano Pokud je zásada pro skupinu prostředků, se vztahuje na všechny prostředky v příslušné skupině prostředků.

JSON použijete k vytvoření definice zásady. Definice zásad obsahuje prvky pro:

* Režim
* parameters
* Zobrazovaný název
* description
* Pravidlo zásad
  * logické vyhodnocení
  * Platnost

Například následujícím kódu JSON zobrazuje zásadu, která omezuje, kdy jsou prostředky nasazené:

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

Všechny ukázky šablony zásad Azure jsou [šablon pro Azure zásad](json-samples.md).

## <a name="mode"></a>Režim

Doporučujeme, abyste nastavili `mode` k `all` tak, aby měl zásady přiřazení vyhodnotit všechny skupiny prostředků a typy. Můžete zobrazit příklad definice zásady, které vynucuje značky na skupinu prostředků na [povolit vlastní image virtuálního počítače ze skupiny prostředků](scripts/allow-custom-vm-image.md).

Pokud ji nastavíte na **všechny**, skupiny prostředků a všechny typy prostředků se vyhodnocují zásady. Portál využívá **všechny** pro všechny zásady. Pokud používáte prostředí PowerShell nebo rozhraní příkazového řádku Azure, budete muset zadat `mode` parametr a nastavte ji na **všechny**.

Použít všechny definice zásady vytvořené pomocí portálu `all` režimu, ale pokud chcete použít PowerShell nebo rozhraní příkazového řádku Azure, budete muset zadat `mode` parametr a nastavte ji na `all`.

Pokud nastavíte režim na `indexed`, přiřazení zásad vyhodnotí pouze na typy prostředků, které podporují značky a umístění.


## <a name="parameters"></a>Parametry

Parametry zjednodušit vaší zásady správy snížením počtu definice zásady. Představte si, že parametry jako pole ve formuláři – `name`, `address`, `city`, `state`. Tyto parametry vždy zůstaly stejné, ale změnit jejich hodnoty podle jednotlivých vyplňování formuláře. Parametry fungovat stejným způsobem jako při vytváření zásad. Zahrnutím parametry v definici zásady můžete znovu použít tuto zásadu pro různé scénáře pomocí různé hodnoty.

Můžete třeba definovat zásady pro vlastnosti prostředku, který omezit umístění, kde můžete nasadit prostředky. Při vytváření zásady, v takovém případě by deklarovat následující parametry:


```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "The list of allowed locations for resources.",
      "displayName": "Allowed locations",
      "strongType": "location"
    }
  }
}
```

Typ parametru může být řetězec nebo pole. Vlastnost metadat slouží k nástroje, například na portálu Azure a zobrazte uživatelské informace.

V rámci vlastnost metadat můžete použít **strongType** zajistit vybrat víc seznam možností v rámci portálu Azure.  Povolené hodnoty pro **strongType** aktuálně patří:

* `"location"`
* `"resourceTypes"`
* `"storageSkus"`
* `"vmSKUs"`

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

Pravidla zásad se skládá z **Pokud** a **pak** bloky. V **Pokud** blok, můžete definovat jednu nebo víc podmínek, které určují, kdy je tato zásada vynucená. Logické operátory můžete použít pro tyto podmínky přesně definovat, tento scénář pro zásady.

V **pak** bloku, definujete o tom, že se stane, když **Pokud** podmínky jsou splněny.

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

Podmínka vyhodnocena jako jestli **pole** splňuje určitá kritéria. Jsou podporované podmínky:

* `"equals": "value"`
* `"like": "value"`
* `"match": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

Při použití **jako** podmínku, můžete zadat zástupný znak (*) v hodnotě.

Při použití **odpovídat** podmínky, zadejte `#` představují číslice, `?` písmeno a libovolný znak představují tento skutečný znak. Příklady najdete v tématu [Image schválené virtuálních počítačů](scripts/allowed-custom-images.md).

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
Zásady podporuje následující typy vliv:

* **Odepřít**: vygeneruje událost v protokolu auditování a požadavek se nezdaří
* **Audit**: vygeneruje událost upozornění v protokolu auditu ale nesplní žádost
* **Připojit**: Přidá definovanou sadu pole k této žádosti
* **AuditIfNotExists**: umožňuje auditování, pokud prostředek neexistuje.
* **DeployIfNotExists**: nasadí prostředku, pokud ještě neexistuje. V současné době tímto účelem je podporována pouze prostřednictvím integrovaných zásad.
* **DenyIfNotExists**: vytvoření existují se odepře, pokud neexistuje

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

S **AuditIfNotExists**, **DeployIfNotExists**, a **DenyIfNotExists**, kterou můžete vyhodnotit existenci podřízených prostředků a použít pravidlo a odpovídající efekt Když tento prostředek neexistuje. Například může vyžadovat, že sledovací proces sítě nasazuje pro všechny virtuální sítě.
Příklad audit, když není nasazený rozšíření virtuálního počítače, naleznete v části [Audit Pokud rozšíření neexistuje](scripts/audit-ext-not-exist.md).


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

## <a name="initiatives"></a>Iniciativami v oblasti

Povolit iniciativy seskupit několik související definice zásad ke zjednodušení přiřazení a správy, protože pracujete s skupinu jako jednu položku. Například můžete seskupit všechny související označování definice zásady v jednom iniciativy. Přiřazovat jednotlivě každou zásadu, můžete použít iniciativa zaměřená.

Následující příklad ukazuje, jak vytvořit initiative pro zpracování dvě značky: `costCenter` a `productName`. Chcete-li použít výchozí hodnota značky používá dvě předdefinované zásady.


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

## <a name="next-steps"></a>Další kroky

- Přečtěte si ukázky šablony zásad Azure v [šablon pro Azure zásad](json-samples.md).
