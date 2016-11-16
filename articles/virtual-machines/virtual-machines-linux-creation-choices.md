---
title: "Různé způsoby vytvoření virtuálního počítače s Linuxem | Dokumentace Microsoftu"
description: "Další informace o různých způsobech vytvoření virtuálního počítače s Linuxem na platformě Azure, včetně odkazů na nástroje a kurzy pro jednotlivé metody."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/27/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 8156467ac97445576517ed7280307f6ca94ff0bf


---
# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a>Různé způsoby vytvoření virtuálního počítače s Linuxem v Azure
V Azure máte flexibilitu vytvoření virtuálního počítače s Linuxem pomocí nástrojů a pracovních postupů, které vám vyhovují. Tento článek shrnuje tyto rozdíly a příklady vytvoření virtuálních počítačů s Linuxem.

## <a name="azure-cli"></a>Azure CLI
Rozhraní Azure CLI je k dispozici napříč platformami pomocí balíčku npm, balíčků distribuce nebo kontejneru Docker. Můžete si přečíst další informace o tom, [jak nainstalovat a nakonfigurovat Azure CLI](../xplat-cli-install.md). V následujících kurzech najdete příklady použití Azure CLI. V každém článku si přečtěte další podrobnosti o zobrazených příkazech rychlého startu CLI:

* [Vytvoření virtuálního počítače s Linuxem z Azure CLI pro vývoj a testování](virtual-machines-linux-quick-create-cli.md)
  
  * Následující příklad vytvoří virtuální počítač s CoreOS pomocí veřejného klíče s názvem `azure_id_rsa.pub`:
    
    ```azurecli
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
      --image-urn CoreOS
    ```
* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
  
  * Následující příklad vytvoří virtuální počítač pomocí šablony uložené na GitHubu:
    
    ```azurecli
    azure group create --name myResourceGroup --location WestUS 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```
* [Vytvoření kompletního linuxového prostředí pomocí Azure CLI](virtual-machines-linux-create-cli-complete.md)
  
  * Zahrnuje vytvoření nástroje pro vyrovnávání zatížení a více virtuálních počítačů ve skupině dostupnosti.
* [Přidání disku do virtuálního počítače s Linuxem](virtual-machines-linux-add-disk.md)
  
  * Následující příklad přidá disk o velikosti 5 GB do existujícího virtuálního počítače s názvem `TestVM`:
    
    ```azurecli
    azure vm disk attach-new --resource-group myResourceGroup  --vm-name myVM \
      --size-in-GB 5
    ```

## <a name="azure-portal"></a>portál Azure
[Azure Portal](https://portal.azure.com) umožňuje rychle vytvořit virtuální počítač, protože není nutné nic instalovat na váš systém. Vytvoření virtuálního počítače pomocí Portálu Azure:

* [Vytvoření virtuálního počítače s Linuxem pomocí webu Azure Portal](virtual-machines-linux-quick-create-portal.md) 
* [Připojení disku pomocí webu Azure Portal](virtual-machines-linux-attach-disk-portal.md)

## <a name="operating-system-and-image-choices"></a>Operační systém a volba image
Při vytváření virtuálního počítače zvolíte image podle operačního systému, který chcete používat. Azure a příslušní partneři nabízí celou řadu imagí, přičemž některé už obsahují předinstalované aplikace a nástroje. Nebo nahrajte některou z vlastních imagí (viz [následující oddíl](#use-your-own-image)).

### <a name="azure-images"></a>Image dostupné v Azure
Pomocí příkazů rozhraní příkazového řádku `azure vm image` zobrazíte dostupné image podle vydavatele, vydání distribuce a sestavení.

Seznam dostupných vydavatelů zobrazíte takto:

```azurecli
azure vm image list-publishers --location WestUS
```

Seznam dostupných produktů (nabídek) pro daného vydavatele zobrazíte takto:

```azurecli
azure vm image list-offers --location WestUS --publisher Canonical
```

Seznam dostupných SKU (vydání distribuce) pro danou nabídku zobrazíte takto:

```azurecli
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Seznam všech dostupných imagí pro dané vydání zobrazíte takto:

```azurecli
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Další příklady vyhledávání a použití dostupných imagí najdete v tématu [Vyhledání a výběr imagí virtuálních počítačů Azure pomocí Azure CLI](virtual-machines-linux-cli-ps-findimage.md).

Příkazy `azure vm quick-create` a `azure vm create` mají aliasy, které můžete použít pro rychlý přístup k častějším distribucím a jejich nejnovějším vydáním. Použití aliasů je často rychlejší, než zadávání vydavatele, nabídky, SKU a verze při každém vytváření virtuálního počítače:

| Alias | Vydavatel | Nabídka | Skladová jednotka (SKU) | Verze |
|:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |nejnovější |
| CoreOS |CoreOS |CoreOS |Stable |nejnovější |
| Debian |credativ |Debian |8 |nejnovější |
| openSUSE |SUSE |openSUSE |13.2 |nejnovější |
| RHEL |Redhat |RHEL |7.2 |nejnovější |
| SLES |SLES |SLES |12-SP1 |nejnovější |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |nejnovější |

### <a name="use-your-own-image"></a>Použití vlastní image
Pokud budete potřebovat image se zvláštními úpravami, můžete *zachytit* stávající virtuální počítač a použít image založenou na tomto virtuálním počítači. Můžete také nahrát místně vytvořenou image. Další informace o podporovaných distribucích a o tom, jak používat vlastní image, najdete v následujících článcích:

* [Distribuce schválené pro Azure](virtual-machines-linux-endorsed-distros.md)
* [Informace pro neschválené distribuce](virtual-machines-linux-create-upload-generic.md)
* [Jak zachytit virtuální počítač s Linuxem do šablony Resource Manageru](virtual-machines-linux-capture-image.md)
  
  * Příkazy rychlého startu pro zachycení stávajícího virtuálního počítače:
    
    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --vm-name myVM
    azure vm generalize --resource-group myResourceGroup --vm-name myVM
    azure vm capture --resource-group myResourceGroup --vm-name myVM --vhd-name-prefix myCapturedVM
    ```

## <a name="next-steps"></a>Další kroky
* Vytvořte virtuální počítač s Linuxem z webu [Azure Portal](virtual-machines-linux-quick-create-portal.md), přes [rozhraní příkazového řádku](virtual-machines-linux-quick-create-cli.md) nebo pomocí [šablony Azure Resource Manageru](virtual-machines-linux-cli-deploy-templates.md).
* Po vytvoření virtuálního počítače s Linuxem můžete [přidat datový disk](virtual-machines-linux-add-disk.md).
* Rychlé kroky pro [resetování hesla nebo klíčů SSH a správu uživatelů](virtual-machines-linux-using-vmaccess-extension.md)




<!--HONumber=Nov16_HO2-->


