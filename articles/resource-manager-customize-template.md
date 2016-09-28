<properties
    pageTitle="Přizpůsobení vyexportované šablony Resource Manageru | Microsoft Azure"
    description="Přidejte do vyexportované šablony Azure Resource Manageru parametry a znovu ji nasaďte prostřednictvím Azure PowerShellu nebo Azure CLI."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/01/2016"
    ms.author="tomfitz"/>


# Přizpůsobení vyexportované šablony Azure Resource Manageru

Tento článek ukazuje, jak upravit vyexportovanou šablonu, abyste ji mohli předat další hodnoty jako parametry. Vychází z kroků provedených v článku [Export šablony Resource Manageru](resource-manager-export-template.md), není je ale nutné dokončit před prováděním tohoto postupu. V tomto článku najdete požadovanou šablonu a skripty.

## Zobrazení vyexportované šablony

Pokud jste dokončili postup z článku [Export šablony Resource Manageru](resource-manager-export-template.md), otevřete šablonu, kterou jste si stáhli. Šablona má název **template.json**. Pokud máte Visual Studio nebo Visual Code, můžete šablonu upravit v nich. Jinak můžete použít libovolný editor JSON nebo textový editor.

Pokud jste nedokončili postup z předchozího návodu, vytvořte soubor s názvem **template.json** a přidejte do něj následující obsah z vyexportované šablony.

```
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualNetworks_VNET_name": {
            "defaultValue": "VNET",
            "type": "String"
        },
        "storageAccounts_storagetf05092016_name": {
            "defaultValue": "storagetf05092016",
            "type": "String"
        }
    },
    "variables": {},
    "resources": [
        {
            "comments": "Generalized from resource in walkthrough.",
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetworks_VNET_name')]",
            "apiVersion": "2015-06-15",
            "location": "northeurope",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "10.0.0.0/16"
                    ]
                },
                "subnets": [
                    {
                        "name": "default",
                        "properties": {
                            "addressPrefix": "10.0.0.0/24"
                        }
                    }
                ]
            },
            "dependsOn": []
        },
        {
            "comments": "Generalized from resource in walkthrough.",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('storageAccounts_storagetf05092016_name')]",
            "apiVersion": "2015-06-15",
            "location": "northeurope",
            "tags": {},
            "properties": {
                "accountType": "Standard_RAGRS"
            },
            "dependsOn": []
        }
    ]
}
```

Šablona template.json bude fungovat, jak má, pokud budete vytvářet stejný typ účtu úložiště ve stejné oblasti s virtuální sítí, která používá stejnou předponu adresy a stejnou předponu podsítě pro všechna nasazení. Resource Manager ale nabízí možnosti pro ještě flexibilnější nasazení šablon. Během nasazování můžete například chtít zadat typ účtu úložiště, který se má vytvořit, nebo hodnoty, které se mají používat pro předponu adresy virtuální sítě a předponu podsítě.

## Přizpůsobení šablony

V této části do vyexportované šablony přidáte parametry, abyste ji mohli použít při nasazování těchto prostředků do dalších prostředí. Také do šablony přidáte některé funkce, aby se snížila pravděpodobnost, že při nasazování šablony dojde k nějaké chybě. Nebudete už muset metodou pokus-omyl zkoušet vytvořit jedinečný název pro účet úložiště. Šablona vytvoří jedinečný název sama. Omezíte hodnoty, které je možné zadat pro typ účtu úložiště, jenom na platné možnosti.

1. Abyste mohli předat hodnoty, které byste mohli chtít zadat během nasazování, nahraďte oddíl **parameters** následujícími definicemi parametrů. Všimněte si hodnot **allowedValues** pro **storageAccount_accountType**. Pokud nechtěně zadáte neplatnou hodnotu, rozpozná se tato chyba ještě před zahájením nasazování. Ještě je třeba upozornit na to, že zadáváte pouze předponu názvu účtu úložiště a předpona je omezena na 11 znaků. Když předponu omezíte na 11 znaků, zajistíte tím, že úplný název nebude delší než maximální povolený počet znaků pro účet úložiště. Předpona umožňuje pro účty úložiště použít zásady vytváření názvů. To, jak vytvořit jedinečný název v dalším kroku, si ukážeme v dalším kroku.

        "parameters": {
          "storageAccount_prefix": {
            "type": "string",
            "maxLength": 11
          },
          "storageAccount_accountType": {
            "defaultValue": "Standard_RAGRS",
            "type": "string",
            "allowedValues": [
              "Standard_LRS",
              "Standard_ZRS",
              "Standard_GRS",
              "Standard_RAGRS",
              "Premium_LRS"
            ]
          },
          "virtualNetwork_name": {
            "type": "string"
          },
          "addressPrefix": {
            "defaultValue": "10.0.0.0/16",
            "type": "string"
          },
          "subnetName": {
            "defaultValue": "subnet-1",
            "type": "string"
          },
          "subnetAddressPrefix": {
            "defaultValue": "10.0.0.0/24",
            "type": "string"
          }
        },

2. Oddíl **variables** šablony je teď prázdný. Nahraďte tento oddíl následujícím kódem. V oddílu **variables** můžete jako autor šablony vytvořit hodnoty, které zjednodušují syntaxi pro zbytek šablony. Proměnná **StorageAccount_name** zřetězí předponu z parametru do jedinečného řetězce, který se generuje na základě identifikátoru skupiny prostředků.

        "variables": {
          "storageAccount_name": "[concat(parameters('storageAccount_prefix'), uniqueString(resourceGroup().id))]"
        },

3. Abyste mohli použít parametry a proměnnou v definicích prostředků, nahraďte oddíl **resources** následujícími definicemi. Všimněte si, že v definicích prostředků se toho ve skutečnosti mnoho nezměnilo – pouze hodnota, která je přiřazená k vlastnosti prostředků. Vlastnosti jsou úplně stejné jako vlastnosti z vyexportované šablony. Jednoduše místo pevně definovaných hodnot přiřazujete hodnotám parametrů vlastnosti. Umístění prostředků je nastavené tak, aby se používalo stejné umístění jako pro skupinu prostředků, a to prostřednictvím výrazu **resourceGroup().location**. Na proměnnou, kterou jste vytvořili pro název účtu úložiště, se odkazuje prostřednictvím výrazu **variables**.

        "resources": [
          {
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetwork_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "properties": {
              "addressSpace": {
                "addressPrefixes": [
                  "[parameters('addressPrefix')]"
                ]
              },
              "subnets": [
                {
                  "name": "[parameters('subnetName')]",
                  "properties": {
                    "addressPrefix": "[parameters('subnetAddressPrefix')]"
                  }
                }
              ]
            },
            "dependsOn": []
          },
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccount_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "tags": {},
            "properties": {
                "accountType": "[parameters('storageAccount_accountType')]"
            },
            "dependsOn": []
          }
        ]

## Oprava souboru parametrů

Stažený soubor parametrů už nemusí odpovídat parametrům ve vaší šabloně. Soubor parametrů nemusíte používat, ale může vám zjednodušit proces při opětovném nasazování prostředí. Výchozí hodnoty, které jsou definované v šabloně, použijete pro spoustu parametrů, takže vám v souboru parametrů stačí pouze dvě hodnoty.

Nahraďte obsah souboru parameters.json tímto kódem:

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccount_prefix": {
      "value": "storage"
    },
    "virtualNetwork_name": {
      "value": "VNET"
    }
  }
}
```

Aktualizovaný soubor parametrů obsahuje hodnoty pouze pro parametry, které nemají výchozí hodnotu. Pokud chcete zadat hodnotu, která se liší od výchozí hodnoty, můžete zadat hodnoty pro další parametry.

## Nasazení šablony

Přizpůsobenou šablonu a soubory parametrů můžete nasadit buď pomocí Azure PowerShellu, nebo pomocí rozhraní příkazového řádku Azure (CLI). V případě potřeby buď [Azure PowerShell](powershell-install-configure.md), nebo [Azure CLI](xplat-cli-install.md) nainstalujte. Můžete použít skripty, které jste stáhli se šablonou, když jste si exportovali původní šablonu, nebo můžete pro nasazení šablony napsat vlastní skript.
V tomto článku si ukážeme obě možnosti.

2. Pokud chcete šablonu nasadit pomocí vlastního skriptu, použijte jednu z následujících možností.

     Pokud používáte PowerShell, spusťte:

        # login
        Add-AzureRmAccount

        # create a new resource group
        New-AzureRmResourceGroup -Name ExportGroup -Location "West Europe"

        # deploy the template to the resource group
        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExportGroup -TemplateFile {path-to-file}\template.json -TemplateParameterFile {path-to-file}\parameters.json

     Pokud používáte Azure CLI, spusťte:

        azure login

        azure group create -n ExportGroup -l "West Europe"

        azure group deployment create -f {path-to-file}\azuredeploy.json -e {path-to-file}\parameters.json -g ExportGroup -n ExampleDeployment

3. Pokud jste si stáhli vyexportovanou šablonu a skripty, najděte soubor **deploy.ps1** (pokud používáte PowerShell) nebo soubor **deploy.sh** (pokud používáte Azure CLI).

     Pokud používáte PowerShell, spusťte:

        Get-Item deploy.ps1 | Unblock-File

        .\deploy.ps1

     Pokud používáte Azure CLI, spusťte:

        .\deploy.sh

## Další kroky

- [Názorný průvodce šablonou Resource Manageru](resource-manager-template-walkthrough.md) umožňuje na základě toho, co jste se naučili v tomto článku, vytvořit šablonu pro složitější řešení. Pomůže vám lépe pochopit, jaké prostředky jsou k dispozici a jak určit hodnoty, které se mají zadat.
- Informace o tom, jak vyexportovat šablonu prostřednictvím PowerShellu, najdete v tématu [Použití Azure PowerShellu s Azure Resource Managerem](powershell-azure-resource-manager.md).
- Informace o tom, jak vyexportovat šablonu prostřednictvím rozhraní příkazového řádku Azure CLI, najdete v tématu věnovaném [Použití rozhraní příkazového řádku Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](xplat-cli-azure-resource-manager.md).
- Další informace o tom, jak je šablona strukturovaná, najdete v tématu o [vytváření šablon Azure Resource Manageru](resource-group-authoring-templates.md).



<!--HONumber=Sep16_HO3-->


