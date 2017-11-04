---
title: "Použít cloudové init k přizpůsobení virtuálního počítače s Linuxem | Microsoft Docs"
description: "Jak používat cloudové init k přizpůsobení virtuálního počítače s Linuxem během vytváření pomocí Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 10/03/2017
ms.author: iainfou
ms.openlocfilehash: 5559f258f5c29b07edb5e61be4755d67173019e0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-cloud-init-to-customize-a-linux-vm-in-azure"></a>Použít cloudové init k přizpůsobení virtuálního počítače s Linuxem v Azure
V tomto článku se dozvíte, jak používat [cloudu init](https://cloudinit.readthedocs.io) Pokud chcete nastavit název hostitele, balíčky aktualizací a spravovat uživatelské účty ve virtuálním počítači (VM) v Azure. Tyto skripty cloudu init spustit na spuštění při vytvoření virtuálního počítače pomocí Azure CLI 2.0. Podrobnější přehled o tom, jak nainstalovat aplikace, zápis konfigurační soubory a vložit klíče z Key Vault najdete v tématu [v tomto kurzu](tutorial-automate-vm-deployment.md). K provedení těchto kroků můžete také využít [Azure CLI 1.0](using-cloud-init-nodejs.md).


## <a name="cloud-init-overview"></a>Init cloudu – přehled
[Init cloudu](https://cloudinit.readthedocs.io) je často používaný přístup k přizpůsobení virtuálního počítače s Linuxem, jako při prvním spuštění. Init cloudu můžete použít k instalaci balíčků a zapisovat soubory nebo konfigurace zabezpečení a uživatelů. Init cloudu běží během úvodního spouštění, nejsou žádné další kroky nebo požadované agenty použít konfiguraci.

Init cloudu také funguje v různých distribucí. Například nepoužívejte **výstižný get instalace** nebo **yum nainstalovat** nainstalovat balíček. Místo toho můžete definovat seznam balíčků pro instalaci. Init cloudu automaticky používá nástroj pro správu nativní balíčku pro distro, kterou vyberete.

Pracujeme s našimi partnery získat cloudu init zahrnuté a práci v bitové kopie, které poskytují do Azure. Následující tabulka popisuje aktuální dostupnosti cloudu init Image platformy Azure:

| Alias | Vydavatel | Nabídka | Skladová jednotka (SKU) | Verze |
|:--- |:--- |:--- |:--- |:--- |:--- |
| UbuntuLTS |Canonical |UbuntuServer |16.04 LTS |nejnovější |
| UbuntuLTS |Canonical |UbuntuServer |14.04.5-LTS |nejnovější |
| CoreOS |CoreOS |CoreOS |Stable |nejnovější |


## <a name="set-the-hostname-with-cloud-init"></a>Nastavte název hostitele s inicializací cloudu
Init cloudu soubory jsou zapsány v [YAML](http://www.yaml.org). Spuštění skriptu cloudu init při vytváření virtuálního počítače v Azure s [vytvořit virtuální počítač az](/cli/azure/vm#create), zadejte soubor init cloudu s `--custom-data` přepínače. Podívejme se na některé příklady můžete nakonfigurovat pomocí souboru cloudu init. Běžný scénář je nastavit název hostitele virtuálního počítače. Ve výchozím nastavení název hostitele je stejný jako název virtuálního počítače. 

Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *eastus* umístění:

```azurecli
az group create --name myResourceGroup --location eastus
```

V aktuálním prostředí, vytvořte soubor s názvem *cloud_init_hostname.txt* a vložte následující konfigurace. Například vytvoření souboru v prostředí cloudu není na místním počítači. Můžete použít libovolný editor, které chcete. V prostředí cloudu, zadejte `sensible-editor cloud_init_hostname.txt` k vytvoření tohoto souboru a zobrazit seznam dostupných editory. Ujistěte se, že je soubor celou cloudu init zkopírován správně, obzvláště první řádek:

```yaml
#cloud-config
hostname: myhostname
```

Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create) a určete soubor init cloudu s `--custom-data cloud_init_hostname.txt` následujícím způsobem:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVMHostname \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_init_hostname.txt
```

Po vytvoření rozhraní příkazového řádku Azure obsahuje informace o virtuálním počítači. Použití `publicIpAddress` k SSH k virtuálnímu počítači. Zadejte vlastní adresu následujícím způsobem:

```bash
ssh azureuser@publicIpAddress
```

Chcete-li zobrazit název virtuálního počítače, použijte `hostname` příkaz takto:

```bash
hostname
```

Virtuální počítač by měl sestav název hostitele jako tuto hodnotu nastavit v cloudu init souboru, jak ukazuje následující příklad výstupu:

```bash
myhostname
```

## <a name="update-a-vm-with-cloud-init"></a>Aktualizace virtuálního počítače s inicializací cloudu
Z bezpečnostních důvodů můžete nakonfigurovat virtuální počítač na nejnovější aktualizace na při prvním spuštění. Jak cloud init funguje napříč různých distribucích systému Linux, je nutné specifikovat `apt` nebo `yum` pro správce balíčků. Místo toho můžete definovat `package_upgrade` a umožněte proces cloudu init určit příslušné mechanismus pro distro používán. Tento pracovní postup můžete použít stejné cloudové init skripty ve distribucích.

Informace o procesu upgradu v akci, vytvořte soubor init cloudu s názvem *cloud_init_upgrade.txt* a vložte následující konfiguraci:

```yaml
#cloud-config
package_upgrade: true
```

Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create) a určete soubor init cloudu s `--custom-data cloud_init_upgrade.txt` následujícím způsobem:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVMUpgrade \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_init_upgrade.txt
```

SSH na veřejnou IP adresu vašeho virtuálního počítače zobrazené ve výstupu z předchozí příkaz. Zadejte vlastní veřejné IP adresy následujícím způsobem:

```bash
ssh azureuser@publicIpAddress
```

Spusťte nástroj pro správu balíček a kontrola aktualizací. Následující příklad používá `apt-get` na virtuálního počítače s Ubuntu:

```bash
sudo apt-get upgrade
```

Init cloudu – zkontrolovat a nainstalovat aktualizace na spouštěcí, nejsou dostupné žádné aktualizace pro použití, jak ukazuje následující příklad výstupu:

```bash
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="add-a-user-to-a-vm-with-cloud-init"></a>Přidání uživatele do virtuálního počítače s inicializací cloudu
Jedním z první úlohy na žádné nové virtuální počítače Linux je potřeba přidat uživatele pro sami, abyste nepoužívejte *kořenové*. Klíče SSH jsou vhodné pro zabezpečení a použitelnost. Klíče jsou přidány do *~/.ssh/authorized_keys* souboru pomocí tohoto skriptu init cloudu.

Chcete-li přidat uživatele do virtuálního počítače s Linuxem, vytvořte soubor init cloudu s názvem *cloud_init_add_user.txt* a vložte následující konfigurace. Zadejte svůj veřejný klíč (například obsah *~/.ssh/id_rsa.pub*) pro *ssh oprávnění klíče*:

```yaml
#cloud-config
users:
  - name: myadminuser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>
```

Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#create) a určete soubor init cloudu s `--custom-data cloud_init_add_user.txt` následujícím způsobem:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVMUser \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_init_add_user.txt
```

SSH na veřejnou IP adresu vašeho virtuálního počítače zobrazené ve výstupu z předchozí příkaz. Zadejte vlastní veřejné IP adresy následujícím způsobem:

```bash
ssh myadminuser@publicIpAddress
```

Potvrďte uživatelů byla přidána do virtuálního počítače a zadaných skupin zobrazit obsah */etc/skupiny* následujícím způsobem:

```bash
cat /etc/group
```

Následující příklad výstupu zobrazuje uživateli *cloud_init_add_user.txt* soubor byl přidán do virtuálního počítače a do příslušné skupiny:

```bash
root:x:0:
<snip />
sudo:x:27:myadminuser
<snip />
myadminuser:x:1000:
```

## <a name="next-steps"></a>Další kroky
Init cloudu je jedním z standardní způsoby, jak upravit vaše virtuální počítače Linux na spuštění. V Azure můžete taky rozšíření virtuálního počítače upravit virtuálním počítačům s Linuxem na spouštěcí nebo když je spuštěná. Například můžete použít rozšíření virtuálního počítače Azure pro spuštění skriptu na spuštění virtuálního počítače, ne jenom na první spuštění. Další informace o rozšíření virtuálního počítače najdete v tématu [virtuálního počítače rozšíření a funkce](extensions-features.md), nebo příklady o tom, jak používat rozšíření najdete v tématu [spravovat uživatele, SSH a zkontrolujte nebo opravte disky na virtuálních počítačích Azure Linux pomocí rozšíření VMAccess](using-vmaccess-extension.md).