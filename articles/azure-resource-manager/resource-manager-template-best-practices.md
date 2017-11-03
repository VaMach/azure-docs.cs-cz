---
title: "Osvědčené postupy pro vytváření šablon Resource Manager | Microsoft Docs"
description: "Pokyny pro zjednodušit vaše šablony Azure Resource Manager."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 31b10deb-0183-47ce-a5ba-6d0ff2ae8ab3
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: tomfitz
ms.openlocfilehash: a23301ba88279af3f7bf4d353ae808e9eeb0900d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="best-practices-for-creating-azure-resource-manager-templates"></a>Osvědčené postupy pro vytváření šablon Azure Resource Manageru
Tyto pokyny vám pomůžou vytvořit šablony Azure Resource Manager, které jsou spolehlivé a snadno se používá. Pokyny jsou pouze návrhy. Nejsou požadavky, pokud není uvedeno jinak. Váš scénář může vyžadovat varianta jednu z následujících přístupy nebo příklady.

## <a name="resource-names"></a>Názvy prostředků
Obecně platí pracujete s tři typy názvy prostředků ve službě Správce prostředků:

* Názvy prostředků, které musí být jedinečný.
* Názvy prostředků, které nemusí být jedinečný, ale můžete rozhodnout pro poskytnutí název, který vám pomůže určit prostředek na základě kontextu.
* Názvy prostředků, které mohou být obecný.

 Informace o omezení přístupu názvem prostředků najdete v tématu [doporučená zásady vytváření názvů pro prostředky Azure](../guidance/guidance-naming-conventions.md).

### <a name="unique-resource-names"></a>Názvy jedinečný prostředků
Je nutné zadat název jedinečný prostředek pro jakýkoli typ prostředku, který má přístup koncový bod data. Některé běžné typy prostředků, které vyžadují jedinečný název patří:

* Úložiště Azure<sup>1</sup> 
* Funkce Web Apps ve službě Azure App Service
* SQL Server
* Azure Key Vault
* Azure Redis Cache
* Azure Batch
* Azure Traffic Manager
* Azure Search
* Azure HDInsight

<sup>1</sup> názvy účtů úložiště musí být také malými písmeny, 24 znaků nebo méně, a nemusí být všechny pomlčky.

Pokud zadáte parametr pro název prostředku, musí se při nasazení prostředku zadejte jedinečný název. Volitelně můžete vytvořit proměnné, která se používá [uniqueString()](resource-group-template-functions-string.md#uniquestring) funkce při generování názvu. 

Můžete také chtít přidat předponu nebo příponu k **uniqueString** výsledek. Úprava jedinečný název můžete další snadno identifikovat typ prostředku z názvu. Můžete například vygenerovat jedinečný název pro účet úložiště pomocí následující proměnnou:

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>Názvy prostředků pro identifikaci
Některé typy prostředků, které můžete chtít názvu, ale jejich názvy nemusí být jedinečný. Pro tyto typy prostředků můžete zadat název, který identifikuje prostředek kontextu a typ prostředku. Zadejte popisný název, který pomáhá identifikovat prostředku v seznamu prostředků. Pokud budete muset použít jiný název prostředku pro jiné nasazení, můžete použít parametr pro název:

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "The name of the VM to create."
        }
    }
}
```

Pokud není nutné předávat název během nasazení, můžete použít proměnné: 

```json
"variables": {
    "vmName": "demoLinuxVM"
}
```

Můžete taky použít hodnotu pevně:

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "demoLinuxVM",
  ...
}
```

### <a name="generic-resource-names"></a>Obecný zdroj názvy
Pro typy prostředků, které většinou přistupovat prostřednictvím jiného prostředku můžete použít obecný název, který je pevně zakódovaná v šabloně. Například můžete nastavit standardní, obecný název pravidla brány firewall na serveru SQL server:

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="parameters"></a>Parametry
Při práci s parametry, může být užitečné následující informace:

* Minimalizujte využití parametrů. Pokud je to možné, použijte proměnnou nebo literálovou hodnotou. Použijte parametry jenom pro tyto scénáře:
   
   * Nastavení, které chcete použít variace podle prostředí (SKU, velikost, kapacity).
   * Názvy prostředků, které chcete určit pro snazší identifikaci.
   * Hodnoty, které používáte k provedení dalších úloh (například správce uživatelské jméno).
   * Tajné klíče (jako jsou hesla).
   * Číslo nebo pole hodnot, který má použít při vytváření více instancí typu prostředku.
* Použijte formát camelCase pro názvy parametrů.
* Zadejte popis všechny parametry v metadatech:

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "The type of the new storage account created to store the VM disks."
           }
       }
   }
   ```

* Definujte výchozí hodnoty pro parametry (s výjimkou hesla a klíče SSH):
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "The type of the new storage account created to store the VM disks."
            }
        }
   }
   ```

* Použití **SecureString** pro všechny hesla a tajné klíče: 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "The value of the secret to store in the vault."
           }
       }
   }
   ```

* Pokud je to možné, nepoužívejte parametr k určení umístění. Místo toho použijte **umístění** vlastnost skupiny prostředků. Pomocí **resourceGroup () .location** výraz pro všechny vaše prostředky, v prostředcích v šabloně nasazených ve stejném umístění jako pro skupinu prostředků:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         ...
     }
   ]
   ```
   
   Pokud typ prostředku je podporován pouze omezený počet umístění, můžete chtít zadat platné umístění přímo v šabloně. Pokud musíte použít **umístění** parametr, že hodnota parametru co nejvíce sdílet s prostředky, které by mohly být ve stejném umístění. Tím se minimalizují počet, který uživatelé vyzváni k zadání informace o umístění.
* Vyhněte se použití parametr nebo proměnná pro verze rozhraní API pro typ prostředku. Vlastnosti prostředku a hodnoty se může lišit podle čísla verze. IntelliSense v editoru kódu nemůže určit správné schéma verze rozhraní API je nastavena na parametr nebo proměnná. Místo toho pevný kódu rozhraní API verze v šabloně.

## <a name="variables"></a>Proměnné
Při práci s proměnnými, může být užitečné následující informace:

* Použití proměnných hodnot, které je nutné použít více než jednou v šabloně. Pokud hodnota je použit pouze jednou, hodnotu pevně usnadňuje šablony čtení.
* Nelze použít [odkaz](resource-group-template-functions-resource.md#reference) fungovat v **proměnné** část šablony. **Odkaz** funkce odvozuje svou hodnotu z prostředku stav modulu runtime. Proměnné jsou však vyřešeny během počáteční analýzy šablony. Konstrukce hodnoty kterém musí **odkaz** funkce přímo v **prostředky** nebo **výstupy** část šablony.
* Zahrnout proměnné pro názvy prostředků, které musí být jedinečný, jak je popsáno v [názvy prostředků](#resource-names).
* Proměnné můžete seskupovat do komplexních objektů. Použití **variable.subentry** formátu k odkazování hodnotu z komplexního objektu. Seskupování proměnných vám mohou pomoci sledovat související proměnné. Také zlepšuje čitelnost šablony. Tady je příklad:
   
   ```json
   "variables": {
       "storage": {
           "name": "[concat(uniqueString(resourceGroup().id),'storage')]",
           "type": "Standard_LRS"
       }
   },
   "resources": [
     {
         "type": "Microsoft.Storage/storageAccounts",
         "name": "[variables('storage').name]",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "sku": {
             "name": "[variables('storage').type]"
         },
         ...
     }
   ]
   ```
   
   > [!NOTE]
   > Komplexní objekt nemůže obsahovat výraz, který odkazuje na hodnotu z komplexního objektu. Definujte proměnnou samostatné pro tento účel.
   > 
   > 
   
     Pokročilé příklady používání komplexních objektů jako proměnné najdete v tématu [sdílet stavu v šablonách Azure Resource Manager](best-practices-resource-manager-state.md).

## <a name="resources"></a>Zdroje
Při práci s prostředky, může být užitečné následující informace:

* Chcete-li jiné přispěvatele pochopit účel prostředku, zadejte **komentáře** pro každý zdroj v šabloně:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used to store the VM disks.",
         ...
     }
   ]
   ```

* Chcete-li přidat metadata k prostředkům můžete použít značky. Metadata použijte k přidání informací o vašich prostředků. Například můžete přidat metadata k zaznamenání fakturačních údajů pro prostředek. Další informace najdete v tématu [použití značek k uspořádání prostředků Azure](resource-group-using-tags.md).
* Pokud používáte *veřejný koncový bod* ve vaší šabloně (například Azure Blob storage veřejný koncový bod), *proveďte pevné kódování* obor názvů. Použití **odkaz** funkce, která se dynamicky načíst obor názvů. Tento postup můžete použít k nasazení šablony do prostředí jiný obor názvů veřejné beze změny ručně koncového bodu v šabloně. Verze rozhraní API se nastaví na stejnou verzi, kterou používáte pro účet úložiště v šabloně:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Pokud účet úložiště je nasazen do stejné šablony, kterou vytváříte, není potřeba zadat obor názvů zprostředkovatele, když odkazujete na prostředek. Toto je zjednodušenou syntaxi:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Pokud máte jiné hodnoty v šabloně, které jsou nakonfigurovány pro použití veřejného obor názvů, změňte tyto hodnoty tak, aby odrážela stejné **odkaz** funkce. Například můžete nastavit **storageUri** vlastnost diagnostického profilu virtuálního počítače:
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   Můžete také použít stávající účet úložiště, který je v jiné skupině prostředků:

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* Přiřaďte veřejné IP adresy k virtuálnímu počítači jenom v případě, že aplikace vyžaduje. Pro připojení k virtuálnímu počítači (VM) pro ladění, nebo pro správu nebo správu účely, použijte příchozích pravidel NAT, bránu virtuální sítě nebo jumpbox.
   
     Další informace o připojení k virtuálním počítačům, najdete v tématu:
   
   * [Spustit virtuální počítače pro architekturu N-vrstvá v Azure](../guidance/guidance-compute-n-tier-vm.md)
   * [Nastavit přístup WinRM pro virtuální počítače ve službě Správce prostředků Azure](../virtual-machines/windows/winrm.md)
   * [Externí přístup k virtuálnímu počítači povolit pomocí portálu Azure](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [Povolit externí přístup k virtuálnímu počítači pomocí prostředí PowerShell](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [Povolit externí přístup k virtuálním počítačům s Linuxem pomocí rozhraní příkazového řádku Azure](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* **DomainNameLabel** vlastnost pro veřejné IP adresy musí být jedinečný. **DomainNameLabel** hodnota musí být v rozmezí 3 až 63 znaků a postupujte podle pravidla určeného tento regulární výraz: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`. Protože **uniqueString** funkce generuje řetězec, který je 13 znaků, **dnsPrefixString** parametr je omezený na 50 znaků:

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* Když přidáte heslo k rozšíření vlastních skriptů, použijte **commandToExecute** vlastnost **protectedSettings** vlastnost:
   
   ```json
   "properties": {
       "publisher": "Microsoft.Azure.Extensions",
       "type": "CustomScript",
       "typeHandlerVersion": "2.0",
       "autoUpgradeMinorVersion": true,
       "settings": {
           "fileUris": [
               "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
           ]
       },
       "protectedSettings": {
           "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
       }
   }
   ```
   
   > [!NOTE]
   > K zajištění, že tajné klíče se šifrují, pokud jsou předávány jako parametry pro virtuální počítače a rozšíření, použijte **protectedSettings** vlastnost relevantní rozšíření.
   > 
   > 

## <a name="outputs"></a>Výstupy
Pokud používáte šablonu pro vytvoření veřejné IP adresy, patří **výstupy** oddíl, který vrátí podrobnosti IP adresy a plně kvalifikovaný název domény (FQDN). Výstupní hodnoty můžete použít k snadnému načtení podrobností o veřejné IP adresy a plně kvalifikované názvy domény po nasazení. Když odkazujete na prostředek, použijte verzi rozhraní API, který jste použili k jeho vytvoření: 

```json
"outputs": {
    "fqdn": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').ipAddress]",
        "type": "string"
    }
}
```

## <a name="single-template-vs-nested-templates"></a>Jediné šabloně vs. vnořené šablony
K nasazení řešení, můžete použít jednu šablonu nebo hlavní šablonu s více vnořených šablon. Vnořené šablony jsou společné pro pokročilejší scénáře. Pomocí šablony vnořené nabízí následující výhody:

* Lze rozčlenit řešení do cílové součásti.
* Vnořené šablon s jinou hlavní šablony můžete znovu použít.

Pokud chcete použít vnořené šablony, můžete pomocí následujících pokynů standardizovat návrhu šablony. Tyto pokyny jsou založené na [vzory pro navrhování šablon Azure Resource Manageru](best-practices-resource-manager-design-templates.md). Doporučujeme, abyste k návrhu, který má následující šablony:

* **Hlavní šablonu** (azuredeploy.json). Používejte pro vstupní parametry.
* **Sdílené prostředky šablony**. Použít k nasazení sdílené prostředky, které používají jiné prostředky (například virtuální sítě a dostupnost sady). Použití **dependsOn** výraz, který se ujistěte, že tato šablona nasazen před další šablony.
* **Volitelné prostředků šablony**. Použijte k nasazení podmíněně prostředky v závislosti na parametr (například jumpbox).
* **Šablony prostředků člen**. Každý typ instance v aplikační vrstvě má svou vlastní konfiguraci. V rámci vrstvy můžete definovat typy jiné instance. (Například nejprve vytvoří cluster a další instance jsou přidány do existujícího clusteru.) Každý typ instance má svou vlastní šablonu nasazení.
* **Skripty**. Široce opakovaně použitelné skripty platí pro každý typ instance (například inicializovat a formát další disky). Vlastní skripty, které vytvoříte za účelem přizpůsobení konkrétní se liší, na základě typu instance.

![Vnořené šablony](./media/resource-manager-template-best-practices/nestedTemplateDesign.png)

Další informace najdete v tématu [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).

## <a name="conditionally-link-to-nested-templates"></a>Podmíněná propojení vnořené šablon
Podmíněná propojení vnořené šablon můžete parametr. Parametr stane součástí identifikátor URI pro šablonu:

```json
"parameters": {
    "newOrExisting": {
        "type": "String",
        "allowedValues": [
            "new",
            "existing"
        ]
    }
},
"variables": {
    "templatelink": "[concat('https://raw.githubusercontent.com/Contoso/Templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
},
"resources": [
    {
        "apiVersion": "2015-01-01",
        "name": "nestedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
            "mode": "incremental",
            "templateLink": {
                "uri": "[variables('templatelink')]",
                "contentVersion": "1.0.0.0"
            },
            "parameters": {
            }
        }
    }
]
```

## <a name="template-format"></a>Formát šablony
Je dobrým zvykem předat šablony prostřednictvím validátor JSON. Validátor můžete odebrat nadbytečné čárkami, kulaté závorky a hranaté závorky, které může způsobit chybu během nasazení. Zkuste [JSONLint](http://jsonlint.com/) nebo linter balíček pro své oblíbené úpravou prostředí (Visual Studio Code, Atom, Sublime Text, Visual Studio).

Je také vhodné pro vaše struktury JSON pro lepší čitelnost. Můžete vytvořit balíček formátovací modul JSON pro vaše místní editor. V sadě Visual Studio k formátování dokumentu, stiskněte **Ctrl + K, Ctrl + D**. V sadě Visual Studio Code stiskněte **Alt + Shift + F**. Pokud vaše místní editor nebude formátu dokumentu, můžete použít [online formátování](https://www.bing.com/search?q=json+formatter).

## <a name="next-steps"></a>Další kroky
* Pokyny na architekturu řešení virtuálních počítačů najdete v tématu [spustit virtuální počítač s Windows v Azure](../guidance/guidance-compute-single-vm.md) a [spuštění virtuálního počítače s Linuxem v Azure](../guidance/guidance-compute-single-vm-linux.md).
* Informace o nastavení účtu úložiště, najdete v části [kontrolní seznam výkonu a škálovatelnosti Azure Storage](../storage/common/storage-performance-checklist.md).
* Další informace o tom, jak mohou organizace pomocí Správce prostředků efektivně spravovat odběry, najdete v části [Azure enterprise vygenerované uživatelské rozhraní: doporučený předplatné zásad správného řízení](resource-manager-subscription-governance.md).

