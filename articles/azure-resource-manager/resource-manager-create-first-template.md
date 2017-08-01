---
title: "Vytvoření první šablony Azure Resource Manageru | Dokumentace Microsoftu"
description: "Podrobný průvodce vytvořením první šablony Azure Resource Manageru. Ukáže vám, jak vytvořit šablonu pomocí referenčních informací k šablonám pro účet úložiště."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 04/18/2017
ms.topic: get-started-article
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: 80fd9d79652e4f0d9c4c524e3a762bcc3462bb53
ms.contentlocale: cs-cz
ms.lasthandoff: 06/01/2017

---

# <a name="create-your-first-azure-resource-manager-template"></a>Vytvoření první šablony Azure Resource Manageru
Toto téma vás provede jednotlivými kroky k vytvoření první šablony Azure Resource Manageru. Šablony Resource Manageru jsou soubory JSON, které definují, jaké prostředky je pro řešení potřeba nasadit. Abyste porozuměli konceptům spojeným s nasazením a správou řešení Azure, podívejte se na téma [Přehled Azure Resource Manageru](resource-group-overview.md). Pokud máte existující prostředky a chcete pro ně získat šablonu, podívejte se na téma [Export šablony Azure Resource Manageru z existujících prostředků](resource-manager-export-template.md).

K vytváření a revidování šablon potřebujete editor JSON. [Visual Studio Code](https://code.visualstudio.com/) je jednoduchý, opensourcový editor kódu pro různé platformy. Podporuje vytváření a úpravy šablon Resource Manageru prostřednictvím rozšíření. Toto téma předpokládá, že používáte VS Code, pokud však máte jiný editor JSON (například sadu Visual Studio), můžete použít ten.

## <a name="get-vs-code-and-extension"></a>Získání nástroje VS Code a rozšíření
1. V případě potřeby nainstalujte VS Code z adresy [https://code.visualstudio.com/](https://code.visualstudio.com/).

2. Nainstalujte rozšíření [Nástroje Azure Resource Manageru](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) tak, že otevřete okno rychlého otevření (Ctrl + P) a spustíte: 

   ```
   ext install msazurermtools.azurerm-vscode-tools
   ```

3. Až budete vyzváni, ukončete a znovu spusťte VS Code, aby se rozšíření povolilo.

## <a name="create-blank-template"></a>Vytvoření prázdné šablony

Začněme s prázdnou šablonou, která obsahuje pouze základní části šablony.

1. Vytvořte soubor. 

2. Zkopírujte a vložte do souboru následující syntaxi JSON:

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {  },
     "variables": {  },
     "resources": [  ],
     "outputs": {  }
   }
   ```

3. Uložte soubor jako **azuredeploy.json**. 

## <a name="add-storage-account"></a>Přidání účtu úložiště
1. Účet úložiště pro nasazení definujete tak, že účet úložiště přidáte do části **resources** (Prostředky) vaší šablony. Hodnoty dostupné pro účet úložiště najdete na stránce s [referenčními informacemi k šablonám pro účty úložiště](/azure/templates/microsoft.storage/storageaccounts). Zkopírujte uvedený kód JSON pro účet úložiště. 

3. Vložte tento kód JSON do části **resources** (Prostředky) vaší šablony, jak je znázorněno v následujícím příkladu: 

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {  },
     "variables": {  },
     "resources": [
       {
         "name": "string",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-12-01",
         "sku": {
           "name": "string"
         },
         "kind": "string",
         "location": "string",
         "tags": {},
         "properties": {
           "customDomain": {
             "name": "string",
             "useSubDomain": boolean
           },
           "encryption": {
             "services": {
               "blob": {
                 "enabled": boolean
               }
             },
             "keySource": "Microsoft.Storage"
           },
           "accessTier": "string"
         }
       }
     ],
     "outputs": {  }
   }
   ```

  VS Code může znamenat, že 2016-12-01 není platnou verzí rozhraní API. Pokud používáte číslo verze z referenční dokumentace šablony, můžete toto upozornění ignorovat. Toto upozornění se zobrazí, když se schéma neaktualizovalo pomocí nejnovějšího čísla verze od poskytovatele prostředků. 
  
  Předchozí příklad obsahuje mnoho zástupných hodnot a některé vlastnosti, které možná pro váš účet úložiště nepotřebujete.

## <a name="set-values-for-storage-account"></a>Zadání hodnot pro účet úložiště

Nyní jste připraveni nastavit hodnoty pro váš účet úložiště. 

1. Znovu se podívejte na stránku s [referenčními informacemi k šablonám pro účty úložiště](/azure/templates/microsoft.storage/storageaccounts), odkud jste zkopírovali kód JSON. Najdete tam několik tabulek s popisem vlastností a nabídkou dostupných hodnot. 

2. Všimněte si, že v rámci elementu **properties** (Vlastnosti) jsou vlastnosti **customDomain** (Vlastní doména), **encryption** (Šifrování) a **accessTier** (Úroveň přístupu) uvedené jako nepovinné. Tyto hodnoty můžou být důležité pro váš scénář, ale pro zjednodušení tohoto příkladu je odebereme.

   ```json
   "resources": [
     {
       "name": "string",
       "type": "Microsoft.Storage/storageAccounts",
       "apiVersion": "2016-12-01",
       "sku": {
         "name": "string"
       },
       "kind": "string",
       "location": "string",
       "tags": {},
       "properties": {
       }
     }
   ],
   ```

3. Aktuálně je element **kind** (Druh) nastavený na zástupnou hodnotu string (Řetězec). VS Code obsahuje mnoho funkcí, které vám pomůžou pochopit, jaké hodnoty máte v šabloně použít. Všimněte si, že nástroj VS Code označuje tuto hodnotu jako neplatnou. Pokud na „string“ najedete myší, VS Code navrhne pro **kind** (Druh) platné hodnoty `Storage` a `BlobStorage`. 

   ![zobrazení navrhovaných hodnot v nástroji VS Code](./media/resource-manager-create-first-template/vs-code-show-values.png)

   Chcete-li zobrazit dostupné hodnoty, smažte znaky mezi uvozovkami a stiskněte kombinaci kláves **Ctrl + Mezerník**. Z dostupných možností vyberte **Storage**.
  
   ![zobrazení technologie IntelliSense](./media/resource-manager-create-first-template/intellisense.png)

   Pokud nepoužíváte VS Code, podívejte se na stránku s referenčními informacemi k šablonám pro účty úložiště. Všimněte si, že v popisu jsou uvedené stejné platné hodnoty. Nastavte element na **Storage**.

   ```json
   "kind": "Storage",
   ```

Vaše šablona teď vypadá nějak takto:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {  },
  "variables": {  },
  "resources": [
    {
      "name": "string",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "string"
      },
      "kind": "Storage",
      "location": "string",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="add-template-function"></a>Přidání funkce šablony

Funkce v rámci šablony slouží k zjednodušení syntaxe šablony a k načítání hodnot, které jsou dostupné pouze během nasazování šablony. Kompletní sadu funkcí šablon najdete v tématu [Funkce šablon Azure Resource Manageru](resource-group-template-functions.md).

Pokud chcete určit, že se účet úložiště nasadí do stejného umístění jako skupina prostředků, nastavte vlastnost **location** (Umístění) na:

```json
"location": "[resourceGroup().location]",
```

VS Code vám opět pomůže návrhem dostupných funkcí. 

![zobrazení funkcí](./media/resource-manager-create-first-template/show-functions.png)

Všimněte si, že je funkce obalena do hranatých závorek. Funkce [resourceGroup](resource-group-template-functions-resource.md#resourcegroup) vrací objekt s vlastností `location`. Skupina prostředků obsahuje všechny související prostředky pro vaše řešení. Vlastnost location (Umístění) můžete pevně nastavit třeba na hodnotu Central US (Střed USA), ale pokud byste chtěli šablonu znovu nasadit do jiného umístění, museli byste ji ručně změnit. Pomocí funkce `resourceGroup` můžete šablonu snadno nasadit do jiné skupiny prostředků v jiném umístění.

Vaše šablona teď vypadá nějak takto:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {  },
  "variables": {  },
  "resources": [
    {
      "name": "string",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "string"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="add-parameters-and-variables"></a>Přidání parametrů a proměnných
V šabloně zbývá nastavit už jen dvě hodnoty – **name** (Název) a **sku.name** (Název SKU). Pro tyto vlastnosti přidáte parametry, které vám umožní přizpůsobit tyto hodnoty během nasazení. 

Pro názvy účtů úložiště platí několik omezení, proto je jejich nastavení obtížné. Název musí být dlouhý 3 až 24 znaků, obsahovat pouze číslice a malá písmena a být jedinečný. Místo toho, abyste se snažili uhodnout jedinečnou hodnotu splňující tato omezení, použijte funkci [uniqueString](resource-group-template-functions-string.md#uniquestring), která vygeneruje hodnotu hash. Aby byla tato hodnota hash srozumitelnější, přidejte předponu, která vám pomůže identifikovat ji po nasazení jako účet úložiště. 

1. Pokud chcete předat předponu názvu, která odpovídá vašim zásadám vytváření názvů, přejděte do části **parameters** (Parametry) vaší šablony. Přidejte do šablony parametr, který přijímá předponu pro název účtu úložiště:

   ```json
   "parameters": {
     "storageNamePrefix": {
       "type": "string",
       "maxLength": 11,
       "defaultValue": "storage",
       "metadata": {
         "description": "The value to use for starting the storage account name."
       }
     }
   },
   ```

  Maximální délka předpony je 11 znaků, protože funkce `uniqueString` vrací 13 znaků a celý název nesmí přesáhnout 24 znaků. Pokud při nasazení nepředáte parametru žádnou hodnotu, použije se výchozí hodnota.

2. Přejděte do části **variables** (Proměnné) vaší šablony. Chcete-li vytvořit název z předpony a jedinečného řetězce, přidejte následující proměnnou:

   ```json
   "variables": {
     "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
   },
   ```

3. V části **resources** (Prostředky) nastavte název účtu úložiště na tuto proměnnou.

   ```json
   "name": "[variables('storageName')]",
   ```

3. Chcete-li povolit předávání různých SKU pro účet úložiště, přejděte do části **parameters** (Parametry). Za parametr pro předponu názvu úložiště přidejte parametr, který určuje povolené hodnoty SKU a výchozí hodnotu. Povolené hodnoty najdete na stránce s referenčními informacemi k šablonám nebo v nástroji VS Code. V následujícím příkladu zahrnujete pro SKU všechny povolené hodnoty. Povolené hodnoty však můžete omezit jenom na typy SKU, které chcete prostřednictvím této šablony nasadit.

   ```json
   "parameters": {
     "storageNamePrefix": {
       "type": "string",
       "maxLength": 11,
       "defaultValue": "storage",
       "metadata": {
         "description": "The value to use for starting the storage account name."
       }
     },
     "storageSKU": {
       "type": "string",
       "allowedValues": [
         "Standard_LRS",
         "Standard_ZRS",
         "Standard_GRS",
         "Standard_RAGRS",
         "Premium_LRS"
       ],
       "defaultValue": "Standard_LRS",
       "metadata": {
         "description": "The type of replication to use for the storage account."
       }
     }
   },
   ```

3. Změňte vlastnost SKU tak, aby používala hodnotu z parametru:

   ```json
   "sku": {
     "name": "[parameters('storageSKU')]"
   },
   ```    

4. Uložte soubor.

## <a name="final-template"></a>Finální šablona

Po dokončení kroků v tomto článku teď vaše šablona vypadá nějak takto:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    },
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "The type of replication to use for the storage account."
      }
    }
  },
  "variables": {
    "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="next-steps"></a>Další kroky
* Šablona je nyní hotová a jste připraveni k jejímu nasazení ve vašem předplatném. Informace o nasazení najdete v tématu [Nasazení prostředků do Azure](resource-manager-quickstart-deploy.md).
* Další informace o struktuře šablon najdete v tématu o [vytváření šablon Azure Resource Manageru](resource-group-authoring-templates.md).
* Hotové šablony pro mnoho různých typů řešení najdete na stránce [Šablony Azure pro rychlý start](https://azure.microsoft.com/documentation/templates/).

