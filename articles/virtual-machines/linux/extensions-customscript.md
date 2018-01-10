---
title: "Spustit vlastní skripty na virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Automatizovat úkoly konfigurace virtuálního počítače s Linuxem pomocí rozšíření vlastních skriptů"
services: virtual-machines-linux
documentationcenter: 
author: danielsollondon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cf17ab2b-8d7e-4078-b6df-955c6d5071c2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: danis
ms.openlocfilehash: 53a241f12373acdb5d40575915d8d6c2f3c86b9a
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2018
---
# <a name="using-the-azure-custom-script-extension-with-linux-virtual-machines"></a>Virtuální počítače s Linuxem pomocí rozšíření Azure vlastních skriptů
Rozšíření vlastních skriptů stahuje a spouští skripty na virtuálních počítačích Azure. Toto rozšíření je užitečné pro konfiguraci po nasazení, instalaci softwaru nebo jakékoli jiné úlohy konfigurace nebo správy. Skripty můžete stáhnout z úložiště Azure nebo jiné dostupné umístění v Internetu nebo zadat čas spuštění rozšíření. Rozšíření vlastních skriptů se integruje s šablon Azure Resource Manageru a můžete také spustit pomocí rozhraní příkazového řádku Azure, PowerShell, portálu Azure nebo REST API pro virtuální počítač Azure.

Tento dokument podrobnosti o tom, jak používat rozšíření vlastních skriptů z rozhraní příkazového řádku Azure a šablonu Azure Resource Manager a také podrobnosti o řešení potíží v systémech Linux.

## <a name="extension-configuration"></a>Konfigurace rozšíření
Konfigurace rozšíření vlastních skriptů určuje takové věci, jako umístění skriptu a příkaz ke spuštění. Tato konfigurace může být uložen v konfiguračních souborů, zadané na příkazovém řádku, nebo šablonu Azure Resource Manager. Citlivá data se uloží v chráněném konfigurace, který je šifrovaný a dešifrovat jenom ve virtuálním počítači. Chráněné konfigurace je užitečné při provádění příkazu zahrnuje tajné klíče, jako například heslo.

### <a name="public-configuration"></a>Veřejná konfigurace
Schéma:

**Poznámka:** -názvy těchto vlastností jsou velká a malá písmena. Používejte názvy, jak vidíte níže, abyste předešli problémům s nasazení.

* **commandToExecute**: (vyžaduje se, řetězce) k provedení skriptu bodu položka
* **fileUris**: (volitelné, pole řetězců) adresy URL pro soubory ke stažení.
* **časové razítko** (volitelné, celé číslo) můžete do tohoto pole pouze pro aktivaci spusťte skript tím, že změníte hodnotu tohoto pole.

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Chráněné konfigurace
Schéma:

**Poznámka:** -názvy těchto vlastností jsou velká a malá písmena. Používejte názvy, jak vidíte níže, abyste předešli problémům s nasazení.

* **commandToExecute**: (volitelné, string) k provedení skriptu bodu položku. Toto pole použijte místo toho, pokud váš příkaz obsahuje tajné klíče, jako jsou hesla.
* **storageAccountName**: (volitelné, string) název účtu úložiště. Pokud chcete zadat pověření pro úložiště, musí být všechny fileUris adresy URL pro objekty BLOB Azure.
* **storageAccountKey**: (volitelné, string) přístupový klíč účtu úložiště.

```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure CLI
Pokud používáte Azure CLI ke spuštění rozšíření vlastních skriptů, vytvořte konfigurační soubor nebo soubory obsahující alespoň identifikátor uri souboru a spuštění příkazu skriptu.

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

Volitelně můžete nastavení zadané v příkazu jako řetězec formátu JSON. To umožňuje konfiguraci zadat během provádění a bez jiný konfigurační soubor.

```azurecli
az vm extension set '
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
```

### <a name="azure-cli-examples"></a>Příklady rozhraní příkazového řádku Azure

**Příklad 1** -veřejné konfigurace pomocí souboru skriptu.

```json
{
  "fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],
  "commandToExecute": "./config-music.sh"
}
```

Azure CLI příkaz:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

**Příklad 2** -veřejné konfigurace s žádný soubor skriptu.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI příkaz:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

**Příklad 3** – veřejné konfigurační soubor se používá k určení souboru skriptu URI a chráněné konfigurační soubor se používá k určení příkaz, který má být proveden.

Veřejné konfigurační soubor:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"]
}
```

Chráněné konfigurační soubor:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Azure CLI příkaz:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json --protected-settings ./protected-config.json
```

## <a name="resource-manager-template"></a>Šablona Resource Manageru
Rozšíření vlastních skriptů Azure můžete spustit v době nasazení virtuálního počítače pomocí šablony Resource Manageru. Uděláte to tak, přidejte do šablony nasazení správně formátovaný JSON.

### <a name="resource-manager-examples"></a>Příklady Resource Manager
**Příklad 1** -veřejné konfigurace.

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

**Příklad 2** -spuštění příkazu v chráněné konfigurace.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

Najdete v článku .net Core Hudba úložiště ukázkové kompletní příklad - [Hudba úložiště ukázkové](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="troubleshooting"></a>Řešení potíží
Při spuštění rozšíření vlastních skriptů, skript se vytvoří nebo stáhli do adresáře podobně jako v následujícím příkladu. Výstup příkazu je také uložen do tohoto adresáře `stdout` a `stderr` souboru.

```bash
/var/lib/waagent/custom-script/download/0/
```

Přípona skriptu Azure vytvoří protokol, který je zde uveden.

```bash
/var/log/azure/custom-script/handler.log
```

Provádění stav rozšíření vlastních skriptů je také možné načíst pomocí rozhraní příkazového řádku Azure.

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

Výstup vypadá následující text:

```azurecli
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Další kroky
Informace o skriptu rozšíření jiných virtuálních počítačů, v tématu [přehled Azure rozšíření skriptů pro Linux](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

