---
title: "Nasazení více instancí prostředků Azure | Microsoft Docs"
description: "Použití operace kopírování a polí ve šablonu Azure Resource Manager k iteraci v vícekrát při nasazení prostředků."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 94d95810-a87b-460f-8e82-c69d462ac3ca
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2017
ms.author: tomfitz
ms.openlocfilehash: ed8e3081d2b2e07938d7cf3aa5f95f6dde81bc66
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a>Nasazení více instancí prostředek nebo vlastnost v šablonách Azure Resource Manager
Toto téma ukazuje, jak k iteraci v šablony Azure Resource Manager můžete vytvořit více instancí prostředku nebo více instancí vlastnosti prostředku.

Pokud potřebujete přidat logiku do šablony, která umožňuje určit, jestli je prostředek nasazené, najdete v části [podmíněně nasazení prostředků](#conditionally-deploy-resource).

## <a name="resource-iteration"></a>Iterace prostředků
Chcete-li vytvořit více instancí typu prostředku, přidejte `copy` element pro typ prostředku. V elementu kopírování zadejte počet opakování a název pro tento smyčky. Hodnota počtu musí být kladné celé číslo a nesmí být delší než 800. Resource Manager vytvoří prostředky paralelně. Proto není zaručena pořadí, ve které byly vytvořeny. Pokud chcete vytvořit vstupní prostředky v pořadí, v tématu [sériové kopie](#serial-copy). 

Prostředek pro vytvoření vícekrát má následující formát:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        }
    ],
    "outputs": {}
}
```

Všimněte si, že obsahuje název každého prostředku `copyIndex()` funkci, která vrátí na aktuální iteraci smyčky. `copyIndex()`je počítáno od nuly. To, v následujícím příkladu:

```json
"name": "[concat('storage', copyIndex())]",
```

Vytvoří tyto názvy:

* storage0
* storage1
* storage2.

K posunutí hodnotu indexu, může předat hodnotu ve funkci copyIndex(). V elementu kopie je stále zadat počet opakování provést, ale zadaná hodnota je posunut hodnotu copyIndex. To, v následujícím příkladu:

```json
"name": "[concat('storage', copyIndex(1))]",
```

Vytvoří tyto názvy:

* storage1
* storage2
* storage3

Operace kopírování je užitečné při práci s poli, protože můžete iterovat každý prvek v poli. Použití `length` funkce v poli zadat počet opakování, a `copyIndex` načíst aktuální index v poli. To, v následujícím příkladu:

```json
"parameters": { 
  "org": { 
     "type": "array", 
     "defaultValue": [ 
         "contoso", 
         "fabrikam", 
         "coho" 
      ] 
  }
}, 
"resources": [ 
  { 
      "name": "[concat('storage', parameters('org')[copyIndex()])]", 
      "copy": { 
         "name": "storagecopy", 
         "count": "[length(parameters('org'))]" 
      }, 
      ...
  } 
]
```

Vytvoří tyto názvy:

* storagecontoso
* storagefabrikam
* storagecoho

## <a name="serial-copy"></a>Kopírování sériového portu

Při použití elementu kopie vytvořit více instancí na typ prostředku, správce prostředků, ve výchozím nastavení, nasadí tyto instance paralelně. Můžete však zadat, aby byly prostředky nasazené v pořadí. Například při aktualizaci provozním prostředí, můžete tak rozložte aktualizací jenom několik jsou aktualizovány v daném okamžiku.

Resource Manager poskytuje vlastnosti na kopírování elementu, který vám umožní sériově nasazení více instancí. V sadě elementů kopie `mode` k **sériové** a `batchSize` na počet instancí pro nasazení v čase. Sériového portu v režimu Resource Manager vytvoří závislost na dřívější instancí ve smyčce, tak nespustí jeden batch, dokud se nedokončí předchozí dávka.

```json
"copy": {
    "name": "iterator",
    "count": "[parameters('numberToDeploy')]",
    "mode": "serial",
    "batchSize": 2
},
```

Vlastnost režimu také přijímá **paralelní**, což je výchozí hodnota.

K otestování sériové kopírování bez vytváření skutečné prostředků, použijte následující šablony, která nasadí prázdný vnořené šablony:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "numberToDeploy": {
      "type": "int",
      "minValue": 2,
      "defaultValue": 5
    }
  },
  "resources": [
    {
      "apiVersion": "2015-01-01",
      "type": "Microsoft.Resources/deployments",
      "name": "[concat('loop-', copyIndex())]",
      "copy": {
        "name": "iterator",
        "count": "[parameters('numberToDeploy')]",
        "mode": "serial",
        "batchSize": 1
      },
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [],
          "outputs": {
          }
        }
      }
    }
  ],
  "outputs": {
  }
}
```

V historii nasazení Všimněte si, že vnořené nasazení se zpracovávají v pořadí.

![sériové nasazení](./media/resource-group-create-multiple/serial-copy.png)

Následující příklad realističtější scénáři nasadí dvě instance v době virtuálního počítače s Linuxem z vnořené šablony:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "User name for the Virtual Machine."
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Password for the Virtual Machine."
            }
        },
        "dnsLabelPrefix": {
            "type": "string",
            "metadata": {
                "description": "Unique DNS Name for the Public IP used to access the Virtual Machine."
            }
        },
        "ubuntuOSVersion": {
            "type": "string",
            "defaultValue": "16.04.0-LTS",
            "allowedValues": [
                "12.04.5-LTS",
                "14.04.5-LTS",
                "15.10",
                "16.04.0-LTS"
            ],
            "metadata": {
                "description": "The Ubuntu version for the VM. This will pick a fully patched image of this given Ubuntu version."
            }
        }
    },
    "variables": {
        "templatelink": "https://raw.githubusercontent.com/rjmax/Build2017/master/Act1.TemplateEnhancements/Chapter03.LinuxVM.json"
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "[concat('nestedDeployment',copyIndex())]",
            "type": "Microsoft.Resources/deployments",
            "copy": {
                "name": "myCopySet",
                "count": 4,
                "mode": "serial",
                "batchSize": 2
            },
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "adminUsername": {
                        "value": "[parameters('adminUsername')]"
                    },
                    "adminPassword": {
                        "value": "[parameters('adminPassword')]"
                    },
                    "dnsLabelPrefix": {
                        "value": "[parameters('dnsLabelPrefix')]"
                    },
                    "ubuntuOSVersion": {
                        "value": "[parameters('ubuntuOSVersion')]"
                    },
                    "index":{
                        "value": "[copyIndex()]"
                    }
                }
            }
        }
    ]
}
```

## <a name="property-iteration"></a>Vlastnost iterace

Chcete-li vytvořit více hodnot pro vlastnost prostředku, přidejte `copy` pole v prvku vlastnosti. Toto pole obsahuje objekty a každý objekt má následující vlastnosti:

* název – název vlastnosti pro vytvoření více hodnot pro
* počet - počet hodnot k vytvoření
* (vstup) – objekt, který obsahuje hodnoty, které mají přiřadit k vlastnosti  

Následující příklad ukazuje, jak se má použít `copy` pro dataDisks vlastnost na virtuálním počítači:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "copy": [{
          "name": "dataDisks",
          "count": 3,
          "input": {
              "lun": "[copyIndex('dataDisks')]",
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

Všimněte si, že při použití `copyIndex` uvnitř iterace vlastnost, je nutné zadat název iterace. Nemáte k zadání názvu při použití s iterace prostředků.

Správce prostředků rozšíří `copy` pole během nasazení. Název pole bude název vlastnosti. Vstupní hodnoty stát vlastnosti objektu. Nasazené šablony se změní na:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
          {
              "lun": 0,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 1,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 2,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

Iterace prostředků a vlastnosti můžete použít společně. Odkaz na vlastnost iterace podle názvu.

```json
{
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[concat(parameters('vnetname'), copyIndex())]",
    "apiVersion": "2016-06-01",
    "copy":{
        "count": 2,
        "name": "vnetloop"
    },
    "location": "[resourceGroup().location]",
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "copy": [
            {
                "name": "subnets",
                "count": 2,
                "input": {
                    "name": "[concat('subnet-', copyIndex('subnets'))]",
                    "properties": {
                        "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
                    }
                }
            }
        ]
    }
}
```

Ve vlastnostech pro každý prostředek může zahrnovat pouze jeden element kopírování. Chcete-li zadat smyčky iterace pro více než jednu vlastnost, definujte více objektů v poli Kopírovat. Každý objekt je vstupní samostatně. Chcete-li například vytvořit více instancí i `frontendIPConfigurations` vlastnost a `loadBalancingRules` vlastnost zařízení na Vyrovnávání zatížení, zadejte oba objekty v elementu jedna kopie: 

```json
{
    "name": "[variables('loadBalancerName')]",
    "type": "Microsoft.Network/loadBalancers",
    "properties": {
        "copy": [
          {
              "name": "frontendIPConfigurations",
              "count": 2,
              "input": {
                  "name": "[concat('loadBalancerFrontEnd', copyIndex('frontendIPConfigurations', 1))]",
                  "properties": {
                      "publicIPAddress": {
                          "id": "[variables(concat('publicIPAddressID', copyIndex('frontendIPConfigurations', 1)))]"
                      }
                  }
              }
          },
          {
              "name": "loadBalancingRules",
              "count": 2,
              "input": {
                  "name": "[concat('LBRuleForVIP', copyIndex('loadBalancingRules', 1))]",
                  "properties": {
                      "frontendIPConfiguration": {
                          "id": "[variables(concat('frontEndIPConfigID', copyIndex('loadBalancingRules', 1)))]"
                      },
                      "backendAddressPool": {
                          "id": "[variables('lbBackendPoolID')]"
                      },
                      "protocol": "tcp",
                      "frontendPort": "[variables(concat('frontEndPort' copyIndex('loadBalancingRules', 1))]",
                      "backendPort": "[variables(concat('backEndPort' copyIndex('loadBalancingRules', 1))]",
                      "probe": {
                          "id": "[variables('lbProbeID')]"
                      }
                  }
              }
          }
        ],
        ...
    }
}
```

## <a name="depend-on-resources-in-a-loop"></a>Závisí na prostředky ve smyčce
Určíte, že je prostředek nasazeno po jiný prostředek pomocí `dependsOn` elementu. K nasazení na prostředek, který závisí na kolekci prostředků ve smyčce, zadejte název kopírovací smyčkou v elementu dependsOn. Následující příklad ukazuje, jak nasadit tři účty úložiště před nasazením virtuálního počítače. Úplná definice virtuálního počítače se nezobrazí. Všimněte si, že element kopie má název nastaven `storagecopy` a element dependsOn pro virtuální počítače je také nastavena na `storagecopy`.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        },
        {
            "apiVersion": "2015-06-15", 
            "type": "Microsoft.Compute/virtualMachines", 
            "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
            "dependsOn": ["storagecopy"],
            ...
        }
    ],
    "outputs": {}
}
```

## <a name="create-multiple-instances-of-a-child-resource"></a>Vytvoření více instancí podřízených prostředků
Kopírovací smyčkou nelze použít pro podřízený prostředek. Pokud chcete vytvořit několik instancí na prostředek, který je obvykle definovat jako vnořené v rámci jiný prostředek, musíte místo toho vytvořit prostředku jako prostředek nejvyšší úrovně. Můžete definovat relaci s nadřazený prostředek prostřednictvím typ a název vlastnosti.

Předpokládejme například, že definujete obvykle datovou sadu jako podřízený prostředek v rámci služby data factory.

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
    "resources": [
    {
        "type": "datasets",
        "name": "exampleDataSet",
        "dependsOn": [
            "exampleDataFactory"
        ],
        ...
    }
}]
```

Pokud chcete vytvořit více instancí datových sad, přesuňte jej mimo služby data factory. Datová sada musí být na stejné úrovni jako objekt pro vytváření dat, ale je stále prostředek podřízeného objektu pro vytváření dat. Můžete zachovat vztah mezi datovou sadu a objektu pro vytváření dat pomocí vlastnosti typu a název. Vzhledem k tomu, že už se nedá odvodit typ od pozice v šabloně, je nutné zadat plně kvalifikovaný typ ve formátu: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.

K vytvoření vztahu nadřazený podřízený s instancí objektu pro vytváření dat, zadejte název pro datovou sadou, která obsahuje název nadřazené prostředků. Použijte formát: `{parent-resource-name}/{child-resource-name}`.  

Následující příklad ukazuje implementaci:

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
},
{
    "type": "Microsoft.DataFactory/datafactories/datasets",
    "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
    "dependsOn": [
        "exampleDataFactory"
    ],
    "copy": { 
        "name": "datasetcopy", 
        "count": "3" 
    } 
    ...
}]
```

## <a name="conditionally-deploy-resource"></a>Podmíněná nasazení prostředků

Chcete-li určit, jestli je nasazené prostředku, použijte `condition` elementu. Hodnota pro tento element překládá true nebo false. Pokud je hodnota true, prostředek je nasazena. Pokud je hodnota false, prostředek není nasazen. Například k určení, zda nový účet úložiště je nasazena nebo existující účet úložiště se používá, použijte tento příkaz:

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

Příklad použití heslo nebo klíč SSH pro virtuální počítač nejde nasadit, naleznete v části [uživatelské jméno nebo SSH podmínku šablony](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).

## <a name="next-steps"></a>Další kroky
* Pokud chcete další informace o části šablony, najdete v části [vytváření šablon Azure Resource Manager](resource-group-authoring-templates.md).
* Informace o nasazení šablony najdete v tématu [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).

