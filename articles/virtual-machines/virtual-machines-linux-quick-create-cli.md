---
title: "Rychlý start Azure – Vytvoření virtuálního počítače pomocí rozhraní příkazového řádku | Dokumentace Microsoftu"
description: "Rychle se naučíte, jak vytvářet virtuální počítače pomocí Azure CLI."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/10/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: c1d7cfe614ab4e677e7fff989e79eb09acb3feed
ms.lasthandoff: 03/22/2017

---

# <a name="create-a-linux-virtual-machine-with-the-azure-cli"></a>Vytvoření virtuálního počítače s Linuxem pomocí Azure CLI

Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. Tento průvodce podrobně popisuje nasazení virtuálního počítače se systémem Ubuntu 16.04 LTS pomocí Azure CLI.

Než začnete, ujistěte se, že je rozhraní Azure CLI nainstalované. Další informace najdete v tématu [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

## <a name="log-in-to-azure"></a>Přihlaste se k Azure. 

Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v umístění `westeurope`.

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače

Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create). 

Následující příklad vytvoří virtuální počítač `myVM`, a pokud ve výchozím umístění klíčů ještě neexistují klíče SSH, vytvoří je. Chcete-li použít konkrétní sadu klíčů, použijte možnost `--ssh-key-value`.  

```azurecli
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

Po vytvoření virtuálního počítače se v Azure CLI zobrazí podobné informace jako v následujícím příkladu. Poznamenejte si veřejnou IP adresu. Tato adresa se používá pro přístup k virtuálnímu počítači.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westeurope",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="connect-to-virtual-machine"></a>Připojení k virtuálnímu počítači

Pomocí následujícího příkazu vytvořte s virtuálním počítačem relaci SSH. IP adresu nahraďte veřejnou IP adresou vašeho virtuálního počítače.

```bash 
ssh <Public IP Address>
```

## <a name="delete-virtual-machine"></a>Odstranění virtuálního počítače

Pokud už je nepotřebujete, můžete k odebrání skupiny prostředků, virtuálního počítače a všech souvisejících prostředků použít následující příkaz.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

[Kurz vytvoření virtuálního počítače s vysokou dostupností](./virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Prozkoumejte ukázky nasazení virtuálního počítače pomocí rozhraní příkazového řádku](./virtual-machines-linux-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

