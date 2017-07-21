---
title: "Různé způsoby vytvoření virtuálního počítače s Linuxem v Azure | Microsoft Azure"
description: "Další informace o různých způsobech vytvoření virtuálního počítače s Linuxem na platformě Azure, včetně odkazů na nástroje a kurzy pro jednotlivé metody."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: b2f93579eb1c7a69d0dbd1b0ef112aed9b2168c3
ms.contentlocale: cs-cz
ms.lasthandoff: 05/31/2017


---
# Různé způsoby vytváření virtuálních počítačů s Linuxem
<a id="different-ways-to-create-a-linux-vm" class="xliff"></a>
V Azure máte flexibilitu vytvoření virtuálního počítače s Linuxem pomocí nástrojů a pracovních postupů, které vám vyhovují. Tento článek shrnuje tyto rozdíly a příklady vytvoření virtuálních počítačů s Linuxem, včetně Azure CLI 2.0. Můžete si také prohlédnout možnosti vytvoření včetně [Azure CLI 1.0](creation-choices-nodejs.md).

Rozhraní [Azure CLI 2.0](/cli/azure/install-az-cli2) je dostupné napříč platformami jako balíček npm, balíček distribuce nebo kontejner Docker. Nainstalujte sestavení nejvhodnější pro vaše prostředí a přihlaste se k účtu Azure pomocí příkazu [az login](/cli/azure/#login).

* [Vytvoření virtuálního počítače s Linuxem pomocí Azure CLI 2.0](quick-create-cli.md)
  
  * Pomocí příkazu [az group create](/cli/azure/group#create) vytvořte skupinu prostředků *myResourceGroup*: 
   
    ```azurecli
    az group create --name myResourceGroup --location eastus
    ```
    
  * Pomocí příkazu [az vm create](/cli/azure/vm#create) vytvořte virtuální počítač *myVM* s použitím nejnovější image *UbuntuLTS* a vygenerujte klíče SSH (pokud už ve složce *~/.ssh* neexistují):

    ```azurecli
    az vm create \
        --resource-group myResourceGroup \
        --name myVM \
        --image UbuntuLTS \
        --generate-ssh-keys
    ```

* [Vytvoření virtuálního počítače s Linuxem pomocí šablony Azure](create-ssh-secured-vm-from-template.md)
  
  * Následující příklad pomocí příkazu [az group deployment create](/cli/azure/group/deployment#create) vytvoří virtuální počítač ze šablony uložené na GitHubu:
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
* [Vytvoření a přizpůsobení virtuálního počítače s Linuxem pomocí cloud-init](tutorial-automate-vm-deployment.md)

* [Vytvoření vysoce dostupné aplikace s vyrovnáváním zatížení na více virtuálních počítačích s Linuxem](tutorial-load-balancer.md)


## portál Azure
<a id="azure-portal" class="xliff"></a>
[Azure Portal](https://portal.azure.com) umožňuje rychle vytvořit virtuální počítač, protože není nutné nic instalovat na váš systém. Vytvoření virtuálního počítače pomocí portálu Azure Portal:

* [Vytvoření virtuálního počítače s Linuxem pomocí webu Azure Portal](quick-create-portal.md) 


## Operační systém a volba image
<a id="operating-system-and-image-choices" class="xliff"></a>
Při vytváření virtuálního počítače zvolíte image podle operačního systému, který chcete používat. Azure a příslušní partneři nabízí celou řadu imagí, přičemž některé už obsahují předinstalované aplikace a nástroje. Nebo nahrajte některou z vlastních imagí (viz [následující oddíl](#use-your-own-image)).

### Image dostupné v Azure
<a id="azure-images" class="xliff"></a>
Pomocí příkazů [az vm image](/cli/azure/vm/image) zobrazíte dostupné image podle vydavatele, distribuce nebo sestavení.

Seznam dostupných vydavatelů:

```azurecli
az vm image list-publishers --location eastus
```

Seznam dostupných produktů (nabídek) pro daného vydavatele:

```azurecli
az vm image list-offers --publisher Canonical --location eastus
```

Seznam dostupných SKU (vydání distribuce) pro danou nabídku:

```azurecli
az vm image list-skus --publisher Canonical --offer UbuntuServer --location eastus
```

Seznam všech dostupných imagí pro dané vydání:

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location eastus
```

Další příklady vyhledávání a použití dostupných imagí najdete v tématu [Vyhledání a výběr imagí virtuálních počítačů Azure pomocí Azure CLI](cli-ps-findimage.md).

Příkaz [az vm create](/cli/azure/vm#create) má aliasy, které můžete použít pro rychlý přístup k častějším distribucím a jejich nejnovějším vydáním. Použití aliasů je často rychlejší, než zadávání vydavatele, nabídky, SKU a verze při každém vytváření virtuálního počítače:

| Alias | Vydavatel | Nabídka | Skladová jednotka (SKU) | Verze |
|:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |nejnovější |
| CoreOS |CoreOS |CoreOS |Stable |nejnovější |
| Debian |credativ |Debian |8 |nejnovější |
| openSUSE |SUSE |openSUSE |13.2 |nejnovější |
| RHEL |Redhat |RHEL |7.2 |nejnovější |
| SLES |SLES |SLES |12-SP1 |nejnovější |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |nejnovější |

### Použití vlastní image
<a id="use-your-own-image" class="xliff"></a>
Pokud budete potřebovat image se zvláštními úpravami, můžete zachytit stávající virtuální počítač a použít image založenou na tomto virtuálním počítači. Můžete také nahrát místně vytvořenou image. Další informace o podporovaných distribucích a o tom, jak používat vlastní image, najdete v následujících článcích:

* [Distribuce schválené pro Azure](endorsed-distros.md)
* [Informace pro neschválené distribuce](create-upload-generic.md)
* [Vytvoření image z existujícího virtuálního počítače Azure](tutorial-custom-images.md)
  
  * Příklady příkazů pro vytvoření image z existujícího virtuálního počítače Azure:
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm image create --resource-group myResourceGroup --source myVM --name myImage
    ```

## Další kroky
<a id="next-steps" class="xliff"></a>
* Vytvořte virtuální počítač s Linuxem pomocí [rozhraní příkazového řádku](quick-create-cli.md), z [portálu](quick-create-portal.md) nebo pomocí [šablony Azure Resource Manageru](../windows/cli-deploy-templates.md).
* Po vytvoření virtuálního počítače s Linuxem [si přečtěte o discích a úložišti Azure](tutorial-manage-disks.md).
* Rychlý postup k [resetování hesla nebo klíčů SSH a správě uživatelů](using-vmaccess-extension.md).

