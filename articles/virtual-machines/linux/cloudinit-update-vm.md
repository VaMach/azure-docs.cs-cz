---
title: "Aktualizace a nainstalovat balíčky v virtuálního počítače s Linuxem v Azure pomocí cloudu init | Microsoft Docs"
description: "Postup použití cloudu init pro aktualizace a nainstalovat balíčky během vytváření pomocí Azure CLI 2.0 ve virtuální počítač s Linuxem"
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: e45bec2a71f94c66ce3044fb81bd2d7cefdf53a5
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="use-cloud-init-to-update-and-install-packages-in-a-linux-vm-in-azure"></a>Aktualizace a nainstalovat balíčky v virtuálního počítače s Linuxem v Azure pomocí init cloudu
V tomto článku se dozvíte, jak používat [cloudu init](https://cloudinit.readthedocs.io) aktualizovat balíčky na systémem Linux nastaví virtuální počítač (VM) nebo škálování virtuálních počítačů (VMSS) při zřizování čas v Azure. Tyto skripty cloudu init spustit při prvním spuštění počítače po prostředky se zřizují Azure. Další informace o cloudu init fungování nativně ve službě Azure a podporovaných distribucích systému Linux najdete v tématu [init cloudu – přehled](using-cloud-init.md)

## <a name="update-a-vm-with-cloud-init"></a>Aktualizace virtuálního počítače s inicializací cloudu
Z bezpečnostních důvodů můžete nakonfigurovat virtuální počítač na nejnovější aktualizace na při prvním spuštění. Jak cloud init funguje napříč různých distribucích systému Linux, je nutné specifikovat `apt` nebo `yum` pro správce balíčků. Místo toho můžete definovat `package_upgrade` a umožněte proces cloudu init určit příslušné mechanismus pro distro používán. Tento pracovní postup můžete použít stejné cloudové init skripty ve distribucích.

Informace o procesu upgradu v akci, vytvořte soubor ve své aktuální prostředí s názvem *cloud_init_upgrade.txt* a vložte následující konfigurace. V tomto příkladu vytvoření souboru v prostředí cloudu není na místním počítači. Můžete použít libovolný editor, které chcete. Zadáním příkazu `sensible-editor cloud_init_upgrade.txt` soubor vytvořte a zobrazte seznam editorů k dispozici. Zvolte #1 používat **nano** editor. Ujistěte se, že je soubor celou cloudu init zkopírován správně, obzvláště první řádek.  

```yaml
#cloud-config
package_upgrade: true
packages:
- httpd
```

Než nasadíte tuto bitovou kopii, je nutné vytvořit skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#az_group_create) příkaz. Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](/cli/azure/vm#az_vm_create) a určete soubor init cloudu s `--custom-data cloud_init_upgrade.txt` následujícím způsobem:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_upgrade.txt \
  --generate-ssh-keys 
```

SSH na veřejnou IP adresu vašeho virtuálního počítače zobrazené ve výstupu z předchozí příkaz. Zadejte vlastní **publicIpAddress** následujícím způsobem:

```bash
ssh <publicIpAddress>
```

Spusťte nástroj pro správu balíček a kontrola aktualizací. Následující příklad používá `apt-get` na virtuálního počítače s Ubuntu:

```bash
sudo apt-get upgrade
```

Init cloudu – zkontrolovat a nainstalovat aktualizace na spouštěcí, měla by existovat žádné aktualizace pro použití, jak ukazuje následující příklad výstupu:

```bash
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

Můžete také zobrazit, který `httpd` byla nainstalovaná, spuštěním `yum history` a zkontrolujte odkazy na výstup `httpd`. 

## <a name="next-steps"></a>Další postup
Mezi další cloudu init změny konfigurace naleznete v následujících tématech:
 
- [Přidání další uživatele Linux do virtuálního počítače](cloudinit-add-user.md)
- [Spusťte Správce balíčků aktualizovat existující balíčky na při prvním spuštění](cloudinit-update-vm.md)
- [Změňte název místního hostitele virtuálního počítače](cloudinit-update-vm-hostname.md) 
- [Instalace balíčku aplikace, aktualizace konfigurační soubory a vložit klíče](tutorial-automate-vm-deployment.md)