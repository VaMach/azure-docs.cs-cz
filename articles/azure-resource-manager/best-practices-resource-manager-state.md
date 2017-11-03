---
title: "Předejte komplexní hodnoty mezi šablony Azure | Microsoft Docs"
description: "Zobrazí doporučená přístupy k používání komplexních objektů ke sdílení dat o stavu pomocí šablon Azure Resource Manager a propojených šablon."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fd2f5e2d-d56d-4e01-a57d-34f3eaead4a9
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: tomfitz
ms.openlocfilehash: e163f3c2e9a78b057dc2a7a42924c59d0aac3fab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="share-state-to-and-from-azure-resource-manager-templates"></a>Sdílená složka stavu do a z šablon Azure Resource Manageru
Toto téma ukazuje osvědčené postupy pro správu a sdílení stavu v rámci šablony. Parametry a proměnné, které jsou uvedené v tomto tématu jsou příklady typu objektů můžete definovat pohodlně uspořádání požadavků vašeho nasazení. V těchto příkladech můžete implementovat vlastní objekty s hodnoty vlastností, které dávají smysl pro vaše prostředí.

Toto téma je součástí větší dokument White Paper. Číst úplnou dokumentu, stáhněte si [World Třída prostředků Manager šablony požadavky a osvědčené postupy](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).

## <a name="provide-standard-configuration-settings"></a>Zadejte standardní konfigurace nastavení
Místo nabízí šablonu, která poskytuje celkový flexibilitu a obrovském množství variant, je běžné vzor poskytnout výběr známé konfigurace. V důsledku toho mohou uživatelé vybrat standardní velikosti tričko například izolovaného prostoru, malé, střední a velké. Další příklady tričko velikosti jsou nabídky produktů, jako je například community edition nebo enterprise edition. V jiných případech může být určeného pro konkrétní úlohy konfigurace technologii – například mapy snížit nebo žádné sql.

S komplexních objektů můžete vytvářet proměnné, které obsahují kolekcí dat, někdy označovanou jako "kontejnery vlastností" a tato data použít k řízení deklaraci prostředků ve vaší šabloně. Tento přístup poskytuje dobrý, známé konfigurace různou velikost předem konfigurovaných pro zákazníky. Bez známé konfigurace musí uživatelé šablony určit cluster pro změnu velikosti na své vlastní, zohlednit v omezení prostředků platformy a provést matematické k identifikaci výsledné oddíly účty úložiště a dalším prostředkům (z důvodu omezení velikosti a prostředek clusteru). Kromě vytváření lepší podmínky pro zákazníka, několik známé konfigurace se snadněji podporovat a můžete poskytovat vyšší úroveň hustotou.

Následující příklad ukazuje, jak k definování proměnné, které obsahují pro představující kolekce dat na komplexní objekty. Kolekce definovat hodnoty, které se používají pro velikost virtuálního počítače, nastavení sítě, nastavení operačního systému a nastavení dostupnosti.

```json
"variables": {
  "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
  "tshirtSizeSmall": {
    "vmSize": "Standard_A1",
    "diskSize": 1023,
    "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
    "vmCount": 2,
    "storage": {
      "name": "[parameters('storageAccountNamePrefix')]",
      "count": 1,
      "pool": "db",
      "map": [0,0],
      "jumpbox": 0
    }
  },
  "tshirtSizeMedium": {
    "vmSize": "Standard_A3",
    "diskSize": 1023,
    "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
    "vmCount": 2,
    "storage": {
      "name": "[parameters('storageAccountNamePrefix')]",
      "count": 2,
      "pool": "db",
      "map": [0,1],
      "jumpbox": 0
    }
  },
  "tshirtSizeLarge": {
    "vmSize": "Standard_A4",
    "diskSize": 1023,
    "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
    "vmCount": 3,
    "slaveCount": 2,
    "storage": {
      "name": "[parameters('storageAccountNamePrefix')]",
      "count": 2,
      "pool": "db",
      "map": [0,1,1],
      "jumpbox": 0
    }
  },
  "osSettings": {
    "scripts": [
      "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
    ],
    "imageReference": {
      "publisher": "Canonical",
      "offer": "UbuntuServer",
      "sku": "14.04.2-LTS",
      "version": "latest"
    }
  },
  "networkSettings": {
    "vnetName": "[parameters('virtualNetworkName')]",
    "addressPrefix": "10.0.0.0/16",
    "subnets": {
      "dmz": {
        "name": "dmz",
        "prefix": "10.0.0.0/24",
        "vnet": "[parameters('virtualNetworkName')]"
      },
      "data": {
        "name": "data",
        "prefix": "10.0.1.0/24",
        "vnet": "[parameters('virtualNetworkName')]"
      }
    }
  },
  "availabilitySetSettings": {
    "name": "pgsqlAvailabilitySet",
    "fdCount": 3,
    "udCount": 5
  }
}
```

Všimněte si, že **tshirtSize** zřetězí velikost trička zadaný pomocí parametru (**malé**, **střední**, **velké**) na text **tshirtSize**. Pomocí této proměnné načíst proměnnou přidružené komplexní objekt pro tento tričko velikost.

Pak můžete odkazovat tyto proměnné později v šabloně. Možnost k odkazování proměnné s názvem a jejich vlastnosti zjednodušuje syntaxi šablony a umožňuje snadno pochopit kontextu. V následujícím příkladu definuje zdroj nasazení pomocí objekty zobrazené dříve nastavit hodnoty. Například je velikost virtuálního počítače nastavit načtením hodnota `variables('tshirtSize').vmSize` při hodnotu pro velikost disku je načtena z `variables('tshirtSize').diskSize`. Kromě toho je nastaven identifikátor URI pro šablonu propojené s hodnotou pro `variables('tshirtSize').vmTemplate`.

```json
"name": "master-node",
"type": "Microsoft.Resources/deployments",
"apiVersion": "2015-01-01",
"dependsOn": [
    "[concat('Microsoft.Resources/deployments/', 'shared')]"
],
"properties": {
    "mode": "Incremental",
    "templateLink": {
      "uri": "[variables('tshirtSize').vmTemplate]",
      "contentVersion": "1.0.0.0"
    },
    "parameters": {
      "adminPassword": {
        "value": "[parameters('adminPassword')]"
      },
      "replicatorPassword": {
        "value": "[parameters('replicatorPassword')]"
      },
      "osSettings": {
        "value": "[variables('osSettings')]"
      },
      "subnet": {
        "value": "[variables('networkSettings').subnets.data]"
      },
      "commonSettings": {
        "value": {
          "region": "[parameters('region')]",
          "adminUsername": "[parameters('adminUsername')]",
          "namespace": "ms"
        }
      },
      "storageSettings": {
        "value":"[variables('tshirtSize').storage]"
      },
      "machineSettings": {
        "value": {
          "vmSize": "[variables('tshirtSize').vmSize]",
          "diskSize": "[variables('tshirtSize').diskSize]",
          "vmCount": 1,
          "availabilitySet": "[variables('availabilitySetSettings').name]"
        }
      },
      "masterIpAddress": {
        "value": "0"
      },
      "dbType": {
        "value": "MASTER"
      }
    }
  }
}
```

## <a name="pass-state-to-a-template"></a>Předat stavu do šablony
Sdílíte stavu do šablony prostřednictvím parametrů, které poskytují přímo během nasazení.

Následující tabulka uvádí běžně používané parametry v šablonách.

| Name (Název) | Hodnota | Popis |
| --- | --- | --- |
| location |Řetězec z omezené seznam oblastí Azure |Umístění, kde jsou nasazené prostředky. |
| storageAccountNamePrefix |Řetězec |Jedinečný název DNS pro účet úložiště, kde jsou umístěné disky Virtuálního počítače |
| domainName |Řetězec |Název domény veřejně přístupná jumpbox virtuálních počítačů ve formátu: **{domainName}. { umístění}.cloudapp.com** například: **mydomainname.westus.cloudapp.azure.com** |
| adminUsername |Řetězec |Uživatelské jméno pro virtuální počítače |
| adminPassword |Řetězec |Heslo pro virtuální počítače |
| tshirtSize |Řetězec, ze seznamu omezené nabízí tričko velikosti |Velikost jednotky škálování s názvem zřídit. Například "Malá", "střední", "Velké" |
| virtualNetworkName |Řetězec |Název virtuální sítě, které chce příjemce použít. |
| enableJumpbox |Řetězec omezené seznamu (povoleno nebo zakázáno) |Parametr, který určuje, jestli se má povolit jumpbox pro prostředí. Hodnoty: "povoleno", "Zakázat" |

**TshirtSize** parametr použitý v předchozí části je definován jako:

```json
"parameters": {
  "tshirtSize": {
    "type": "string",
    "defaultValue": "Small",
    "allowedValues": [
      "Small",
      "Medium",
      "Large"
    ],
    "metadata": {
      "Description": "T-shirt size of the MongoDB deployment"
    }
  }
}
```

## <a name="pass-state-to-linked-templates"></a>Stav předat propojených šablon
Při připojování k propojených šablon, můžete často použít kombinaci statických a vygeneruje proměnné.

### <a name="static-variables"></a>Statické proměnné
Statické proměnné se často používá k poskytování základní hodnoty, například adresy URL, které se používají v rámci šablony.

V následující výňatek ze šablony `templateBaseUrl` Určuje kořenový adresář pro danou šablonu v Githubu. Na další řádek vytvoří novou proměnnou `sharedTemplateUrl` který zřetězí základní adresu URL s známý název šablony sdílených prostředků. Níže daného řádku proměnnou komplexní objekt se používá k ukládání velikost trička, kde je zřetězen do umístění šablony známé konfigurace a uložené v základní adresu URL `vmTemplate` vlastnost.

Výhodou tohoto přístupu je, pokud se změní umístění šablon, potřebujete jenom statické proměnné na jednom místě, které vyhovují v propojených šablon změnit.

```json
"variables": {
  "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
  "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
  "tshirtSizeSmall": {
    "vmSize": "Standard_A1",
    "diskSize": 1023,
    "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
    "vmCount": 2,
    "slaveCount": 1,
    "storage": {
      "name": "[parameters('storageAccountNamePrefix')]",
      "count": 1,
      "pool": "db",
      "map": [0,0],
      "jumpbox": 0
    }
  }
}
```

### <a name="generated-variables"></a>Vygenerovaný proměnné
Kromě statické proměnné několika proměnných generována dynamicky. Tato část popisuje některé běžné typy generovaného proměnných.

#### <a name="tshirtsize"></a>tshirtSize
Jste obeznámeni s Tato proměnná generovaný z výše uvedených příkladech.

#### <a name="networksettings"></a>networkSettings
V kapacitu, schopností nebo šablona vymezená řešení začátku do konce propojených šablon obvykle vytvořit prostředky, které existují v síti. Jeden Nejjednodušším způsobem je použití komplexní objekt pro uložení nastavení sítě a předat propojených šablon.

Níže najdete příklad komunikaci nastavení sítě.

```json
"networkSettings": {
  "vnetName": "[parameters('virtualNetworkName')]",
  "addressPrefix": "10.0.0.0/16",
  "subnets": {
    "dmz": {
      "name": "dmz",
      "prefix": "10.0.0.0/24",
      "vnet": "[parameters('virtualNetworkName')]"
    },
    "data": {
      "name": "data",
      "prefix": "10.0.1.0/24",
      "vnet": "[parameters('virtualNetworkName')]"
    }
  }
}
```

#### <a name="availabilitysettings"></a>availabilitySettings
Prostředky vytvořené v propojených šablon jsou často umístěny v nastavení dostupnosti. V následujícím příkladu je zadána název sady dostupnosti a také počet doména selhání a aktualizace domény použít.

```json
"availabilitySetSettings": {
  "name": "pgsqlAvailabilitySet",
  "fdCount": 3,
  "udCount": 5
}
```

Pokud potřebujete více skupiny dostupnosti (například jeden pro hlavní uzly) a druhý pro datové uzly, můžete použít název jako předpony, zadejte několik skupin dostupnosti nebo postupujte podle modelu uvedené výše pro vytvoření proměnné pro konkrétní velikost trička.

#### <a name="storagesettings"></a>storageSettings
Podrobnosti o úložiště jsou často sdílet s propojených šablon. V příkladu níže *storageSettings* objektu obsahuje podrobné informace o názvu účtu a kontejner úložiště.

```json
"storageSettings": {
    "vhdStorageAccountName": "[parameters('storageAccountName')]",
    "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
    "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
}
```

#### <a name="ossettings"></a>osSettings
S propojených šablon musíte předat nastavení operačního systému na různé typy uzlů napříč jinou konfiguraci známé typy. Komplexní objekt je snadný způsob, jak ukládat a sdílet informace o operačním systému a také usnadňuje podporují více možností operačního systému pro nasazení.

Následující příklad ukazuje objekt pro *osSettings*:

```json
"osSettings": {
  "imageReference": {
    "publisher": "Canonical",
    "offer": "UbuntuServer",
    "sku": "14.04.2-LTS",
    "version": "latest"
  }
}
```

#### <a name="machinesettings"></a>machineSettings
Proměnnou generovaného *machineSettings* je komplexní objekt obsahující směs základní proměnných pro vytvoření virtuálního počítače. Proměnné zahrnují správce uživatelské jméno a heslo, předpona pro názvy virtuálních počítačů a odkaz na bitovou kopii operačního systému.

```json
"machineSettings": {
    "adminUsername": "[parameters('adminUsername')]",
    "adminPassword": "[parameters('adminPassword')]",
    "machineNamePrefix": "mongodb-",
    "osImageReference": {
        "publisher": "[variables('osFamilySpec').imagePublisher]",
        "offer": "[variables('osFamilySpec').imageOffer]",
        "sku": "[variables('osFamilySpec').imageSKU]",
        "version": "latest"
    }
},
```

Všimněte si, že *osImageReference* načte hodnoty z *osSettings* proměnná definovaná v šabloně hlavní. To znamená, že můžete snadno změnit operační systém pro virtuální počítač – zcela nebo v závislosti na předvoleb šablony příjemce.

#### <a name="vmscripts"></a>vmScripts
*VmScripts* objekt obsahuje podrobnosti o skripty ke stažení a spuštění na instanci virtuálního počítače, včetně odkazů a vnější. Mimo obsahovat odkazy na infrastrukturu.
Odkazy uvnitř zahrnují nainstalovaného softwaru nainstalovaného a konfigurace.

Můžete použít *scriptsToDownload* vlastnost seznam skripty ke stažení do virtuálního počítače. Tento objekt obsahuje taky odkazy na argumenty příkazového řádku pro různé typy akcí. Mezi ně patří provádění výchozí instalace pro všechny jednotlivé uzly, instalaci, která spustí po všechny uzly, jsou nasazené a jakékoli další skripty, které mohou být specifické pro dané šablony.

V tomto příkladu je ze šablony použít k nasazení MongoDB, který vyžaduje arbiter zajistit vysokou dostupnost. *ArbiterNodeInstallCommand* byl přidán do *vmScripts* k instalaci arbiter.

V části proměnné je, kde najít proměnné, které definují konkrétní text pro spuštění skriptu s správné hodnoty.

```json
"vmScripts": {
    "scriptsToDownload": [
        "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
        "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
    ],
    "regularNodeInstallCommand": "[variables('installCommand')]",
    "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
    "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
},
```

## <a name="return-state-from-a-template"></a>Návratový stav ze šablony
Nejenže můžete předat data do šablony, můžete také sdílet data zpět do volání šablony. V **výstupy** část propojené šablony, můžete zadat dvojice klíč/hodnota, které mohou být spotřebovávána zdrojovou šablonu.

Následující příklad ukazuje, jak předat privátní IP adresu vygenerovaných propojené šablony.

```json
"outputs": {
    "masterip": {
        "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
        "type": "string"
     }
}
```

V rámci hlavní šablony můžete tato data s následující syntaxí:

    "[reference('master-node').outputs.masterip.value]"

Můžete použít tento výraz v části výstupy nebo oddílu prostředků hlavní šablony. V sekci proměnných nelze použít ve výrazu, protože závisí na stav modulu runtime. Vrácená hodnota z hlavní šablony, použijte:

```json
"outputs": {
  "masterIpAddress": {
    "value": "[reference('master-node').outputs.masterip.value]",
    "type": "string"
  }
```

Příklad použití části výstupy propojené šablony pro vrácení datových disků pro virtuální počítač, naleznete v části [vytváření více datových disků pro virtuální počítač](resource-group-create-multiple.md).

## <a name="define-authentication-settings-for-virtual-machine"></a>Zadejte nastavení ověřování pro virtuální počítač
Stejného vzoru zobrazovaného dříve nastavení konfigurace můžete použít k určení nastavení ověřování pro virtuální počítač. Vytvoření parametru pro předávání v typ ověřování.

```json
"parameters": {
  "authenticationType": {
    "allowedValues": [
      "password",
      "sshPublicKey"
    ],
    "defaultValue": "password",
    "metadata": {
      "description": "Authentication type"
    },
    "type": "string"
  }
}
```

Přidejte proměnných pro typy různých ověřování a proměnnou pro uložení jaký typ se používá pro toto nasazení na základě hodnoty parametru.

```json
"variables": {
  "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
  "osProfilepassword": {
    "adminPassword": "[parameters('adminPassword')]",
    "adminUsername": "notused",
    "computerName": "[parameters('vmName')]",
    "customData": "[base64(variables('customData'))]"
  },
  "osProfilesshPublicKey": {
    "adminUsername": "notused",
    "computerName": "[parameters('vmName')]",
    "customData": "[base64(variables('customData'))]",
    "linuxConfiguration": {
      "disablePasswordAuthentication": "true",
      "ssh": {
        "publicKeys": [
          {
            "keyData": "[parameters('sshPublicKey')]",
            "path": "/home/notused/.ssh/authorized_keys"
          }
        ]
      }
    }
  }
}
```

Při definování virtuální počítač, můžete nastavit **osProfile** do proměnné, které jste vytvořili.

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  ...
  "osProfile": "[variables('osProfile')]"
}
```

## <a name="next-steps"></a>Další kroky
* Další informace o části šablony najdete v tématu [vytváření šablon Azure Resource Manager](resource-group-authoring-templates.md)
* Funkce, které jsou k dispozici v rámci šablon najdete v sekci [funkce šablon Azure Resource Manager](resource-group-template-functions.md)
