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
ms.date: 04/03/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 303cb9950f46916fbdd58762acd1608c925c1328
ms.openlocfilehash: 67b4af9f3ef7fb078d6d125e0d3257ea85e288b0
ms.lasthandoff: 04/04/2017

---

# <a name="create-a-linux-virtual-machine-with-the-azure-cli"></a>Vytvoření virtuálního počítače s Linuxem pomocí Azure CLI

Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. Tento průvodce podrobně popisuje nasazení virtuálního počítače se systémem Ubuntu 16.04 LTS pomocí Azure CLI. Po nasazení serveru se připojíme pomocí SSH k virtuálnímu počítači, aby bylo možné nainstalovat NGINX. 

Než začnete, ujistěte se, že je rozhraní Azure CLI nainstalované. Další informace najdete v tématu [Průvodce instalací Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

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

Po vytvoření virtuálního počítače se v Azure CLI zobrazí podobné informace jako v následujícím příkladu. Poznamenejte si `publicIpAddress`. Tato adresa se používá pro přístup k virtuálnímu počítači.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westeurope",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="open-port-80-for-web-traffic"></a>Otevření portu 80 pro webový provoz 

Ve výchozím nastavení jsou na virtuální počítače s Linuxem, které jsou nasazené v Azure, povolená pouze připojení SSH. Pokud bude tento virtuální počítač webovým serverem, budete muset otevřít port 80 z internetu.  K otevření požadovaného portu se vyžaduje jeden příkaz.  
 
 ```azurecli 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="ssh-into-your-vm"></a>Připojení SSH k virtuálnímu počítači

Pomocí následujícího příkazu vytvořte s virtuálním počítačem relaci SSH. Nahraďte `<publicIpAddress>` správnou veřejnou IP adresou vašeho virtuálního počítače.  V našem příkladu byla IP adresa `40.68.254.142`.

```bash 
ssh <publicIpAddress>
```

## <a name="install-nginx"></a>Instalace serveru NGINX

Pomocí následujícího skriptu bash provedete aktualizaci zdrojů balíčku a nainstalujete nejnovější balíček NGINX. 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-ngix-welcome-page"></a>Zobrazení úvodní stránky serveru NGINX

S nainstalovaným serverem NGINX na virtuálním počítači a nyní otevřeným portem 80 z internetu můžete použít libovolný webový prohlížeč a zobrazit výchozí úvodní stránku serveru NGINX. Nezapomeňte pro návštěvu výchozí stránky použít adresu `publicIpAddress` popsanou výše. 

![Výchozí web NGINX](./media/quick-create-cli/nginx.png) 


## <a name="delete-virtual-machine"></a>Odstranění virtuálního počítače

Pokud už je nepotřebujete, můžete k odebrání skupiny prostředků, virtuálního počítače a všech souvisejících prostředků použít následující příkaz.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Další kroky

[Kurz vytvoření virtuálního počítače s vysokou dostupností](create-cli-complete.md)

[Prozkoumejte ukázky nasazení virtuálního počítače pomocí rozhraní příkazového řádku](cli-samples.md)

