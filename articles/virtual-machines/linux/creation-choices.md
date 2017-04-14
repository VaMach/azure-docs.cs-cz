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
ms.date: 01/03/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: d7ff1317cdf6ccfe6b4e5035878fc4e096fcc0f9
ms.lasthandoff: 04/03/2017


---
# <a name="different-ways-to-create-a-linux-vm"></a>Různé způsoby vytváření virtuálních počítačů s Linuxem
V Azure máte flexibilitu vytvoření virtuálního počítače s Linuxem pomocí nástrojů a pracovních postupů, které vám vyhovují. Tento článek shrnuje tyto rozdíly a příklady vytvoření virtuálních počítačů s Linuxem, včetně Azure CLI 2.0. Můžete si také prohlédnout možnosti vytvoření včetně [Azure CLI 1.0](creation-choices-nodejs.md).

Rozhraní [Azure CLI 2.0](/cli/azure/install-az-cli2) je dostupné napříč platformami jako balíček npm, balíček distribuce nebo kontejner Docker. Nainstalujte sestavení nejvhodnější pro vaše prostředí a přihlaste se k účtu Azure pomocí příkazu [az login](/cli/azure/#login).

V následujících příkladech se používá Azure CLI 2.0. V příslušných článcích najdete další podrobnosti o použitých příkazech. Můžete také použít příklady vytváření instancí s Linuxem pomocí [Azure CLI 1.0](creation-choices-nodejs.md).

* [Vytvoření virtuálního počítače s Linuxem pomocí Azure CLI 2.0](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Tento příklad vytvoří skupinu prostředků s názvem `myResourceGroup` pomocí příkazu [az group create](/cli/azure/group#create): 
-    
    ```azurecli
    az group create --name myResourceGroup --location westus
    ```
    
  * Tento příklad pomocí příkazu [az vm create](/cli/azure/vm#create) vytvoří virtuální počítač s názvem `myVM` s použitím nejnovějšího image Debian s Azure Managed Disks a veřejným klíčem `id_rsa.pub`:

    ```azurecli
    az vm create \
    --image credativ:Debian:8:latest \
     --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
az vm disk attach –g myResourceGroup –-vm-name myVM –-disk myDataDisk  –-new --size-gb 5    --public-ip-address-dns-name myPublicDNS \
    --resource-group myResourceGroup \
    --location westus \
    --name myVM
    ```

    * Pokud chcete použít nespravované disky, přidejte k výše uvedenému příkazu příznak `--use-unmanaged-disks`. Vytvoří se pro vás účet úložiště. Další informace najdete v tématu [Přehled služby Azure Managed Disks](../../storage/storage-managed-disks-overview.md).

* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Následující příklad vytvoří virtuální počítač pomocí šablony uložené na GitHubu příkazem [az group deployment create](/cli/azure/group/deployment#create):
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
    
* [Vytvoření kompletního linuxového prostředí pomocí Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Zahrnuje vytvoření nástroje pro vyrovnávání zatížení a více virtuálních počítačů ve skupině dostupnosti.

* [Přidání disku do virtuálního počítače s Linuxem](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Následující příklad pomocí příkazu [az vm disk attach-new](/cli/azure/vm/disk#attach-new) přidá do existujícího virtuálního počítače s názvem `myVM` disk o velikosti 50 GB:
  
    ```azurecli
    az vm disk attach –g myResourceGroup –-vm-name myVM –-disk myDataDisk  \
    –-new --size-gb 50
    ```

## <a name="azure-portal"></a>portál Azure
[Azure Portal](https://portal.azure.com) umožňuje rychle vytvořit virtuální počítač, protože není nutné nic instalovat na váš systém. Vytvoření virtuálního počítače pomocí Portálu Azure:

* [Vytvoření virtuálního počítače s Linuxem pomocí webu Azure Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 
* [Připojení disku pomocí webu Azure Portal](../windows/attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="operating-system-and-image-choices"></a>Operační systém a volba image
Při vytváření virtuálního počítače zvolíte image podle operačního systému, který chcete používat. Azure a příslušní partneři nabízí celou řadu imagí, přičemž některé už obsahují předinstalované aplikace a nástroje. Nebo nahrajte některou z vlastních imagí (viz [následující oddíl](#use-your-own-image)).

### <a name="azure-images"></a>Image dostupné v Azure
Pomocí příkazů [az vm image](/cli/azure/vm/image) zobrazíte dostupné image podle vydavatele, distribuce nebo sestavení.

Seznam dostupných vydavatelů:

```azurecli
az vm image list-publishers --location WestUS
```

Seznam dostupných produktů (nabídek) pro daného vydavatele:

```azurecli
az vm image list-offers --publisher Canonical --location WestUS
```

Seznam dostupných SKU (vydání distribuce) pro danou nabídku:

```azurecli
az vm image list-skus --publisher Canonical --offer UbuntuServer --location WestUS
```

Seznam všech dostupných imagí pro dané vydání:

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location WestUS
```

Další příklady vyhledávání a použití dostupných imagí najdete v tématu [Vyhledání a výběr imagí virtuálních počítačů Azure pomocí Azure CLI](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Příkaz **az vm create** má aliasy, které můžete použít pro rychlý přístup k častějším distribucím a jejich nejnovějším vydáním. Použití aliasů je často rychlejší, než zadávání vydavatele, nabídky, SKU a verze při každém vytváření virtuálního počítače:

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

* [Distribuce schválené pro Azure](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Informace pro neschválené distribuce](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Jak zachytit virtuální počítač s Linuxem do šablony Resource Manageru](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  
  * Příklady příkazů **az vm** pro rychlé zachycení existujícího virtuálního počítače s využitím nespravovaných disků:
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm capture --resource-group myResourceGroup --name myVM --vhd-name-prefix myCapturedVM
    ```

## <a name="next-steps"></a>Další kroky
* Vytvořte virtuální počítač s Linuxem z webu [Azure Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), přes [rozhraní příkazového řádku](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo pomocí [šablony Azure Resource Manageru](../windows/cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Po vytvoření virtuálního počítače s Linuxem můžete [přidat datový disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Rychlé kroky pro [resetování hesla nebo klíčů SSH a správu uživatelů](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

