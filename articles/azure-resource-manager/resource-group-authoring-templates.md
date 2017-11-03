---
title: "Struktura šablony Azure Resource Manager a syntaxe | Microsoft Docs"
description: "Popisuje strukturu a vlastnosti šablon Azure Resource Manager pomocí deklarativní syntaxe JSON."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/14/2017
ms.author: tomfitz
ms.openlocfilehash: dc9b64062d7f68c83aa090eec96744819a5ca423
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a>Pochopit strukturu a syntaxe šablon Azure Resource Manager
Toto téma popisuje strukturu šablony Azure Resource Manager. Představuje různé části šablony a vlastnosti, které jsou k dispozici v těchto částech. Šablona se skládá z JSON a výrazy, které můžete použít k vytvoření hodnot pro vaše nasazení. Podrobný kurz k vytvoření šablony, najdete v části [vytvoření vaší první šablony Azure Resource Manager](resource-manager-create-first-template.md).

## <a name="template-format"></a>Formát šablony
Ve své nejjednodušší struktuře šablonu obsahuje následující prvky:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "resources": [  ],
    "outputs": {  }
}
```

| Název elementu | Požaduje se | Popis |
|:--- |:--- |:--- |
| $schema |Ano |Umístění souboru schématu JSON, který popisuje verzi jazyka šablony. Použijte adresu URL v předchozím příkladu. |
| contentVersion |Ano |Verze šablony (jako je například 1.0.0.0). Můžete zadat jakoukoli hodnotu pro tento element. Při nasazení prostředků pomocí šablony, tuto hodnotu lze zajistit, aby se používal správnou šablonu. |
| parameters |Ne |Hodnoty, které jsou k dispozici při nasazení pro přizpůsobení nasazení prostředků. |
| proměnné |Ne |Hodnoty, které se používá jako fragmenty JSON v šabloně, které zjednodušují výrazy jazyka šablony. |
| Prostředky |Ano |Typy prostředků, které jsou nasazené nebo aktualizovány v skupinu prostředků. |
| Výstupy |Ne |Hodnoty, které se vrátí po nasazení. |

Každý prvek obsahuje vlastnosti, které můžete zadat. Následující příklad obsahuje úplnou syntaxí šablony:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  
        "<parameter-name>" : {
            "type" : "<type-of-parameter-value>",
            "defaultValue": "<default-value-of-parameter>",
            "allowedValues": [ "<array-of-allowed-values>" ],
            "minValue": <minimum-value-for-int>,
            "maxValue": <maximum-value-for-int>,
            "minLength": <minimum-length-for-string-or-array>,
            "maxLength": <maximum-length-for-string-or-array-parameters>,
            "metadata": {
                "description": "<description-of-the parameter>" 
            }
        }
    },
    "variables": {  
        "<variable-name>": "<variable-value>",
        "<variable-name>": { 
            <variable-complex-type-value> 
        }
    },
    "resources": [
      {
          "condition": "<boolean-value-whether-to-deploy>",
          "apiVersion": "<api-version-of-resource>",
          "type": "<resource-provider-namespace/resource-type-name>",
          "name": "<name-of-the-resource>",
          "location": "<location-of-resource>",
          "tags": {
              "<tag-name1>": "<tag-value1>",
              "<tag-name2>": "<tag-value2>"
          },
          "comments": "<your-reference-notes>",
          "copy": {
              "name": "<name-of-copy-loop>",
              "count": "<number-of-iterations>",
              "mode": "<serial-or-parallel>",
              "batchSize": "<number-to-deploy-serially>"
          },
          "dependsOn": [
              "<array-of-related-resource-names>"
          ],
          "properties": {
              "<settings-for-the-resource>",
              "copy": [
                  {
                      "name": ,
                      "count": ,
                      "input": {}
                  }
              ]
          },
          "resources": [
              "<array-of-child-resources>"
          ]
      }
    ],
    "outputs": {
        "<outputName>" : {
            "type" : "<type-of-output-value>",
            "value": "<output-value-expression>"
        }
    }
}
```

Jsme zkontrolujte v částech šablony podrobněji později v tomto tématu.

## <a name="expressions-and-functions"></a>Výrazy a funkce
Základní syntaxe šablony je JSON. K dispozici v rámci šablony JSON hodnoty rozšířit však výrazy a funkce.  Výrazy jsou zapsané v JSON textové literály jejichž první a poslední znaky jsou hranaté závorky: `[` a `]`, v uvedeném pořadí. Hodnota výrazu vyhodnotí při nasazení šablony. Při zápisu jako řetězcový literál, může být výsledkem vyhodnocení výrazu jiného typu formátu JSON, jako je například pole nebo celé číslo, v závislosti na skutečný výraz.  Tak, aby měl řetězcový literál začínat závorky `[`, ale je interpretován jako výraz, můžete přidat další znak pravé závorky zahájíte řetězec s `[[`.

Obvykle používají výrazy s funkcí k provádění operací pro konfiguraci nasazení. Jenom jako v jazyce JavaScript, volání funkce jsou formátovány jako `functionName(arg1,arg2,arg3)`. Vlastnosti odkazovat pomocí operátorů dot a [index].

Následující příklad ukazuje, jak použít několik funkcí, při vytváření hodnoty:

```json
"variables": {
    "location": "[resourceGroup().location]",
    "usernameAndPassword": "[concat(parameters('username'), ':', parameters('password'))]",
    "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
}
```

Úplný seznam funkcí šablony najdete v tématu [funkce šablon Azure Resource Manager](resource-group-template-functions.md). 

## <a name="parameters"></a>Parametry
V sekci parametrů šablony zadejte hodnoty, které můžete zadat při nasazování prostředky. Tyto hodnoty parametrů umožňují přizpůsobit nasazení zadáním hodnoty, které jsou přizpůsobené pro konkrétní prostředí (například vývoj, testování a provozním). Není nutné zadat parametry v šabloně, ale bez parametrů šablony vždy nasazení stejné prostředky se stejnými názvy, umístění a vlastnosti.

Můžete definovat parametry s následující strukturou:

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": "<default-value-of-parameter>",
        "allowedValues": [ "<array-of-allowed-values>" ],
        "minValue": <minimum-value-for-int>,
        "maxValue": <maximum-value-for-int>,
        "minLength": <minimum-length-for-string-or-array>,
        "maxLength": <maximum-length-for-string-or-array-parameters>,
        "metadata": {
            "description": "<description-of-the parameter>" 
        }
    }
}
```

| Název elementu | Požaduje se | Popis |
|:--- |:--- |:--- |
| Název parametru |Ano |Název parametru. Musí být platný identifikátor jazyka JavaScript. |
| type |Ano |Typ hodnoty parametru. Zobrazit seznam povolených typů za touto tabulkou. |
| Výchozí hodnota |Ne |Výchozí hodnota pro parametr, pokud není zadána žádná hodnota pro parametr. |
| allowedValues |Ne |Povolené hodnoty pro parametr a ujistěte se, že je zadaná hodnota pravé pole. |
| MinValue |Ne |Minimální hodnota pro parametry typu int, tato hodnota je (včetně). |
| MaxValue |Ne |Maximální hodnota parametry typu int, tato hodnota je (včetně). |
| minLength |Ne |Minimální délka řetězce, secureString a parametry typu pole, tato hodnota je (včetně). |
| Hodnota maxLength |Ne |Maximální délka řetězce, secureString a parametry typu pole, tato hodnota je (včetně). |
| description |Ne |Popis parametru, který se zobrazí uživatelům prostřednictvím portálu. |

Povolené typy a hodnoty jsou:

* **řetězec**
* **secureString**
* **celá čísla**
* **BOOL**
* **objekt** 
* **secureObject**
* **pole**

Pro zadání parametru, jako je volitelné, zadejte hodnotu defaultValue (může být prázdný řetězec). 

Pokud zadáte název parametru v šabloně, který odpovídá parametru v příkazu k nasazení šablony, je potenciální nejednoznačnosti hodnoty, které zadáte. Správce prostředků řeší tento nedorozuměním přidáním operátory **FromTemplate** pro parametr šablony. Například, pokud zahrnete parametr s názvem **ResourceGroupName** v šabloně, je v konfliktu s **ResourceGroupName** parametr ve [New-AzureRmResourceGroupDeployment ](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) rutiny. Během nasazení, budete vyzváni, zadejte hodnotu pro **ResourceGroupNameFromTemplate**. Obecně platí neměli byste toto nedorozuměním není pojmenováním parametry se stejným názvem jako parametry použité pro operace nasazení.

> [!NOTE]
> Všechna hesla, klíče a jiné tajné klíče by měl používat **secureString** typu. Pokud předáte citlivá data v objektu JSON, použít **secureObject** typu. Parametry šablony s secureString nebo secureObject typů nelze přečíst po nasazení prostředků. 
> 
> Například následující položku v historii nasazení zobrazuje hodnotu pro řetězce a objekt, ale ne pro secureString a secureObject.
>
> ![Zobrazit hodnoty nasazení](./media/resource-group-authoring-templates/show-parameters.png)  
>

Následující příklad ukazuje, jak definovat parametry:

```json
"parameters": {
    "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
    },
    "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
    },
    "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
    },
    "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
    }
}
```

Pro vstupní hodnoty parametrů během nasazování naleznete v tématu [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md). 

## <a name="variables"></a>Proměnné
V sekci proměnných můžete vytvořit hodnoty, které lze použít v celé vaší šablony. Není potřeba definovat proměnné, ale jejich často zjednodušit vaše šablony snížením složité výrazy.

Můžete definovat proměnné s následující strukturou:

```json
"variables": {
    "<variable-name>": "<variable-value>",
    "<variable-name>": { 
        <variable-complex-type-value> 
    }
}
```

Následující příklad ukazuje, jak k definování proměnné, který je sestavený ze dvou hodnot parametrů:

```json
"variables": {
    "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
}
```

Další příklad ukazuje, proměnné, která je komplexního typu JSON a proměnné, které se vytvářejí na základě jiné proměnné:

```json
"parameters": {
    "environmentName": {
        "type": "string",
        "allowedValues": [
          "test",
          "prod"
        ]
    }
},
"variables": {
    "environmentSettings": {
        "test": {
            "instancesSize": "Small",
            "instancesCount": 1
        },
        "prod": {
            "instancesSize": "Large",
            "instancesCount": 4
        }
    },
    "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
    "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
    "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
}
```

## <a name="resources"></a>Zdroje
V části prostředky definujete prostředky, které jsou nasazené a aktualizovat. V této části můžete získat složité, protože je potřeba pochopit, typy, které nasazujete zadejte správné hodnoty. Pro konkrétní prostředky hodnoty (apiVersion, typ a vlastnosti), které je nutné nastavit, najdete v části [definování zdrojů v šablonách Azure Resource Manager](/azure/templates/). 

Můžete definovat prostředky s následující strukturou:

```json
"resources": [
  {
      "condition": "<boolean-value-whether-to-deploy>",
      "apiVersion": "<api-version-of-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "name": "<name-of-the-resource>",
      "location": "<location-of-resource>",
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "comments": "<your-reference-notes>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": "<number-of-iterations>",
          "mode": "<serial-or-parallel>",
          "batchSize": "<number-to-deploy-serially>"
      },
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| Název elementu | Požaduje se | Popis |
|:--- |:--- |:--- |
| Podmínka | Ne | Logická hodnota, která určuje, jestli je nasazené prostředku. |
| apiVersion |Ano |Verze rozhraní REST API pro vytvoření prostředku. |
| type |Ano |Typ prostředku. Tato hodnota je kombinací obor názvů zprostředkovatele prostředků a typ prostředku (například **Microsoft.Storage/storageAccounts**). |
| jméno |Ano |Název prostředku. Název musí splňovat omezení součást URI definované v RFC3986. Kromě toho služby Azure, které zveřejňují názvu prostředku třetí stranou ověřit název, který má Ujistěte se, zda není pokus o zfalšovat jiné identity. |
| location |Je to různé. |Podporované geografické umístění zadaného prostředku. Můžete vybrat některý z dostupných umístění, ale obvykle má smysl vyberte ten, který je blízko vaši uživatelé. Obvykle také má smysl umístit prostředky, které vzájemně spolupracovat ve stejné oblasti. Většina typů prostředků vyžadují umístění, ale některé typy (například přiřazení role) nevyžadují umístění. V tématu [nastavit umístění prostředku v šablonách Azure Resource Manager](resource-manager-template-location.md). |
| tags |Ne |Značky, které jsou přidružené k prostředku. V tématu [označit prostředky v šablonách Azure Resource Manager](resource-manager-template-tags.md). |
| Komentáře |Ne |Poznámky pro dokumentaci prostředky ve vaší šabloně |
| Kopírování |Ne |V případě potřeby více než jednu instanci počet zdrojů pro vytvoření. Paralelní je výchozí režim. Zadejte sériové režim, když nechcete, aby všechny nebo prostředky do nasazení ve stejnou dobu. Další informace najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md). |
| dependsOn |Ne |Prostředky, které musí být nasazené, než je nasazený tento prostředek. Správce prostředků vyhodnotí závislosti mezi prostředky a nasadí je ve správném pořadí. Pokud nejsou na sobě navzájem závislé prostředky, jsou nasazeny současně. Hodnota může být čárkami oddělený seznam prostředek názvy nebo jedinečné identifikátory prostředků. Zobrazit seznam pouze těch prostředků, které jsou nasazeny v této šabloně. Prostředky, které nejsou v této šabloně definovány již musí existovat. Vyhněte se přidání nepotřebné závislostí, jak mohou zpomalit nasazení a vytvoření cyklické závislosti. Pokyny v závislosti na nastavení najdete v tématu [definování závislostí v šablonách Azure Resource Manager](resource-group-define-dependencies.md). |
| properties |Ne |Nastavení konfigurace specifických prostředků. Hodnoty pro vlastnosti jsou stejné jako hodnoty, které zadáte v textu požadavku REST API operaci (metoda PUT) k vytvoření prostředku. Můžete také zadat pole kopie vytvořit více instancí vlastnosti. Další informace najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md). |
| Prostředky |Ne |Podřízené prostředky, které jsou závislé na prostředku definovaný. Zadejte pouze typy prostředků, které jsou povoleny schématem nadřazený prostředek. Plně kvalifikovaný typ prostředku podřízené obsahuje nadřazený typ prostředku, jako například **Microsoft.Web/sites/extensions**. Závislost na nadřazeném prostředku není implicitní. Je nutné explicitně zadat tuto závislost. |

V části prostředky obsahuje pole prostředky pro nasazení. V rámci každého prostředku můžete také definovat pole podřízené prostředky. Proto vaše oddílu prostředků může mít struktura jako:

```json
"resources": [
  {
      "name": "resourceA",
  },
  {
      "name": "resourceB",
      "resources": [
        {
            "name": "firstChildResourceB",
        },
        {   
            "name": "secondChildResourceB",
        }
      ]
  },
  {
      "name": "resourceC",
  }
]
```      

Další informace o definování podřízené prostředky najdete v tématu [nastavte název a typ pro podřízený prostředek v šabloně Resource Manager](resource-manager-template-child-resource.md).

**Podmínku** element určuje, zda je nasazení na prostředek. Hodnota pro tento element překládá true nebo false. Například k určení, jestli je nasazené nový účet úložiště, použijte tento příkaz:

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

Příklad použití nový nebo existující prostředek, naleznete v části [nový nebo stávající šablonu podmínku](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).

Chcete-li určit, zda je virtuální počítač nasadit pomocí hesla nebo klíče SSH, definovat dvě verze virtuálního počítače šablony a použít **podmínku** k odlišení využití. Předání parametru, který určuje scénář, který chcete nasadit.

```json
{
    "condition": "[equals(parameters('passwordOrSshKey'),'password')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'password')]",
    "properties": {
        "osProfile": {
            "computerName": "[variables('vmName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
        },
        ...
    },
    ...
},
{
    "condition": "[equals(parameters('passwordOrSshKey'),'sshKey')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'ssh')]",
    "properties": {
        "osProfile": {
            "linuxConfiguration": {
                "disablePasswordAuthentication": "true",
                "ssh": {
                    "publicKeys": [
                        {
                            "path": "[variables('sshKeyPath')]",
                            "keyData": "[parameters('adminSshKey')]"
                        }
                    ]
                }
            }
        },
        ...
    },
    ...
}
``` 

Příklad použití heslo nebo klíč SSH pro virtuální počítač nejde nasadit, naleznete v části [uživatelské jméno nebo SSH podmínku šablony](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).

## <a name="outputs"></a>Výstupy
V části výstupy zadejte hodnoty, které jsou vráceny z nasazení. Například může vrátit identifikátor URI pro přístup k prostředkům nasazené.

Následující příklad ukazuje strukturu definici výstup:

```json
"outputs": {
    "<outputName>" : {
        "type" : "<type-of-output-value>",
        "value": "<output-value-expression>"
    }
}
```

| Název elementu | Požaduje se | Popis |
|:--- |:--- |:--- |
| outputName |Ano |Název výstupní hodnotu. Musí být platný identifikátor jazyka JavaScript. |
| type |Ano |Typ hodnoty výstup. Výstupní hodnoty podporují stejné typy jako vstupní parametry šablony. |
| hodnota |Ano |Výraz jazyka šablony, který se vyhodnotí a vrátí jako výstupní hodnotu. |

Následující příklad ukazuje, hodnotu, která je vrácena v části výstupy.

```json
"outputs": {
    "siteUri" : {
        "type" : "string",
        "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
    }
}
```

Další informace o práci s výstupem najdete v tématu [sdílení stavu v šablonách Azure Resource Manager](best-practices-resource-manager-state.md).

## <a name="template-limits"></a>Omezení šablony

Omezení velikosti vaší šablony 1 MB a každý soubor parametrů na 64 KB. Omezení 1 MB platí pro konečného stavu šablony po rozšířila s definic iterativní prostředků a hodnoty pro parametry a proměnné. 

Také jste omezeni na:

* 256 parametry
* 256 proměnné
* 800 prostředky (včetně počet kopií)
* 64 výstupní hodnoty
* 24,576 znaků výraz šablony

Některá omezení šablony můžete překročit pomocí vnořené šablony. Další informace najdete v tématu [použití propojených šablon při nasazování prostředků Azure](resource-group-linked-templates.md). Abyste snížili počet parametrů, proměnné nebo výstupů, můžete sloučit několik hodnot do objektu. Další informace najdete v tématu [objektů jako parametry](resource-manager-objects-as-parameters.md).

## <a name="next-steps"></a>Další kroky
* Hotové šablony pro mnoho různých typů řešení najdete na stránce [Šablony Azure pro rychlý start](https://azure.microsoft.com/documentation/templates/).
* Podrobnosti o funkcích, které můžete použít z v rámci šablon najdete v tématu [funkce šablon Azure Resource Manager](resource-group-template-functions.md).
* Pokud chcete kombinovat několik šablon během nasazení, přečtěte si téma [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).
* Musíte používat prostředky, které existují v jiné skupině prostředků. Tento scénář je běžný, při práci s účty úložiště a virtuální sítě, které jsou sdíleny více skupin prostředků. Další informace najdete v tématu [resourceId funkce](resource-group-template-functions-resource.md#resourceid).
