---
title: "Odkaz šablony pro nasazení Azure | Microsoft Docs"
description: "Popisuje způsob použití propojených šablon v šablonu Azure Resource Manageru k vytvoření řešení modulární šablony. Ukazuje, jak chcete předat hodnoty parametrů, zadejte soubor parametrů a dynamicky vytvořené adresy URL."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 27d8c4b2-1e24-45fe-88fd-8cf98a6bb2d2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: tomfitz
ms.openlocfilehash: 8b58a83ffd473500dd3f76c09e251f9208527d4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-linked-templates-when-deploying-azure-resources"></a>Použití propojených šablon při nasazování prostředků Azure
Z v rámci jedné šablony Azure Resource Manager, můžete se propojit k jiné šablony, která umožňuje rozložit nasazení do sady s cílem, šablony pro konkrétní účel. Stejně jako u decomposing aplikace do více tříd kódu, poskytuje rozložením výhody z hlediska testování, opakované použití a přehlednosti.  

Z hlavní šablony lze předat parametry do propojené šablony, a tyto parametry můžete přímo namapovat parametry nebo proměnné vystavené volání šablony. Propojené šablony můžete předat také proměnnou výstup zpět na zdrojovou šablonu povolení výměna obousměrný dat mezi šablonami.

## <a name="linking-to-a-template"></a>Propojení do šablony
Můžete vytvořit propojení mezi přidáním nasazení prostředků v rámci hlavní šablony, která odkazuje na šabloně propojené dvě šablony. Můžete nastavit **templateLink** vlastnost na identifikátor URI propojené šablony. Zadáním hodnot parametru pro danou šablonu propojené přímo v šabloně nebo v souboru parametrů. Následující příklad používá **parametry** vlastnost přímo zadat hodnotu parametru.

```json
"resources": [ 
  { 
      "apiVersion": "2017-05-10", 
      "name": "linkedTemplate", 
      "type": "Microsoft.Resources/deployments", 
      "properties": { 
        "mode": "incremental", 
        "templateLink": {
          "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion": "1.0.0.0"
        }, 
        "parameters": { 
          "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
        } 
      } 
  } 
] 
```

Podobně jako ostatní typy prostředků můžete nastavit závislosti mezi propojené šablony a dalším prostředkům. Proto pokud další prostředky vyžadují hodnotu výstup z propojené šablony, jste měli jistotu, že propojené šablony nasazení před sebou. Nebo, pokud propojené šablony závisí na jiné prostředky, můžete zajistit, že jiné prostředky, které jsou nasazeny před propojené šablony. Načtení hodnoty z šablonu propojené s následující syntaxí:

```json
"[reference('linkedTemplate').outputs.exampleProperty.value]"
```

Musí být mít přístup k šabloně propojené služby Správce prostředků. Nelze zadat místní soubor nebo soubor, který je k dispozici ve vaší místní síti propojené šablony. Můžete pouze zadat hodnotu identifikátoru URI, která zahrnuje buď **http** nebo **https**. Jednou z možností je umístit propojené šablony v účtu úložiště, a použijte identifikátor URI pro položku, tak jak je znázorněno v následujícím příkladu:

```json
"templateLink": {
    "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
    "contentVersion": "1.0.0.0",
}
```

I když propojené šablony musí být externě k dispozici, nemusí být obecně dostupné pro veřejnost. Šablony můžete přidat na účet privátní úložiště, které je přístupné pouze majiteli účtu úložiště. Pak vytvořte token sdílený přístupový podpis (SAS) pro povolení přístupu během nasazení. Přidejte tento token SAS URI propojené šablony. Postup nastavení šablony v účtu úložiště a generování tokenu SAS naleznete v tématu [nasazení prostředků pomocí šablony Resource Manageru a prostředí Azure PowerShell](resource-group-template-deploy.md) nebo [nasazení prostředků pomocí šablony Resource Manageru a rozhraní příkazového řádku Azure](resource-group-template-deploy-cli.md). 

Následující příklad ukazuje nadřazené šablony, který odkazuje na jinou šablonu. Propojené šablony přistupuje s tokenem SAS, který se předává v jako parametr.

```json
"parameters": {
    "sasToken": { "type": "securestring" }
},
"resources": [
    {
        "apiVersion": "2017-05-10",
        "name": "linkedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
          "mode": "incremental",
          "templateLink": {
            "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
            "contentVersion": "1.0.0.0"
          }
        }
    }
],
```

I když token, je předaná jako zabezpečený řetězec, je identifikátor URI propojené šablony, včetně tokenu SAS, přihlášení operace nasazení. K omezení rizika, nastavte vypršení platnosti pro daný token.

Správce prostředků zpracovává každé propojené šablony jako samostatné nasazení. V historii nasazení pro skupinu prostředků najdete v části samostatná nasazení pro nadřazené a vnořené šablony.

![historie nasazení](./media/resource-group-linked-templates/linked-deployment-history.png)

## <a name="linking-to-a-parameter-file"></a>Propojování do souboru parametrů
Další příklad používá **parametersLink** vlastnost, která má odkaz na soubor parametru.

```json
"resources": [ 
  { 
     "apiVersion": "2017-05-10", 
     "name": "linkedTemplate", 
     "type": "Microsoft.Resources/deployments", 
     "properties": { 
       "mode": "incremental", 
       "templateLink": {
          "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion":"1.0.0.0"
       }, 
       "parametersLink": { 
          "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
          "contentVersion":"1.0.0.0"
       } 
     } 
  } 
] 
```

Hodnota identifikátoru URI pro parametr propojené soubor nemůže být místní soubor a musí obsahovat buď **http** nebo **https**. Soubor parametrů také možné omezit přístup pomocí tokenu SAS.

## <a name="using-variables-to-link-templates"></a>Použití proměnných propojení šablony
Předchozí příklady nám ukázaly pevně definovaných hodnot adresu URL pro odkazy. šablony. Tento přístup může fungovat pro jednoduchou šablonu, ale nebude fungovat správně při práci s velké sady modulární šablony. Místo toho můžete vytvořit statickou proměnné, která ukládá základní adresu URL pro hlavní šablonu a dynamicky vytvářet adresy URL pro propojené šablony z této základní adresu URL. Výhodou tohoto přístupu je snadno přesunout nebo rozvětvit šablony, protože potřebujete změnit statické proměnné v šabloně hlavní. Hlavní šablonu předá správné identifikátory URI v rámci rozložená šablony.

Následující příklad ukazuje, jak použít základní adresu URL k vytvoření dvou adres URL pro propojených šablon (**sharedTemplateUrl** a **vmTemplate**). 

```json
"variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

Můžete také použít [deployment()](resource-group-template-functions-deployment.md#deployment) získat základní adresu URL pro aktuální šablony a použít k získání adresy URL pro další šablony ve stejném umístění. Tento přístup je užitečný, pokud se změní vaši polohu šablony (možná z důvodu Správa verzí) nebo chcete vyhnout pevného kódování adresy URL v souboru šablony. 

```json
"variables": {
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="complete-example"></a>Úplný příklad
Následující příklad šablony zobrazit zjednodušené uspořádání propojených šablon pro ilustraci několik konceptů v tomto článku. Předpokládá se, že šablony byly přidány do kontejneru v účtu úložiště s veřejného přístupu vypnutý. Propojené šablony předá hodnotu zpět do hlavní šablony **výstupy** části.

**Parent.json** souboru se skládá z:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerSasToken": { "type": "string" }
  },
  "resources": [
    {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
          "contentVersion": "1.0.0.0"
        }
      }
    }
  ],
  "outputs": {
    "result": {
      "type": "string",
      "value": "[reference('linkedTemplate').outputs.result.value]"
    }
  }
}
```

**Helloworld.json** souboru se skládá z:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [],
  "outputs": {
    "result": {
        "value": "Hello World",
        "type" : "string"
    }
  }
}
```

V prostředí PowerShell můžete získat token pro kontejner a nasazení šablon s:

```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzureStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

V Azure CLI 2.0 můžete získat token pro kontejner a nasazení šablon s následujícím kódem:

```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name storagecontosotemplates \
    --query connectionString)
token=$(az storage container generate-sas \
    --name templates \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name parent.json \
    --output tsv \
    --connection-string $connection)
parameter='{"containerSasToken":{"value":"?'$token'"}}'
az group deployment create --resource-group ExampleGroup --template-uri $url?$token --parameters $parameter
```

## <a name="next-steps"></a>Další kroky
* Další informace o definování pořadí nasazení pro vaše prostředky najdete v tématu [definování závislostí v šablonách Azure Resource Manager](resource-group-define-dependencies.md)
* Zjistěte, jak definovat jeden prostředek ale vytvořit mnoho instancí, najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md)

