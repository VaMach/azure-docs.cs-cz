---
title: "Vytvoření virtuálního počítače s Linuxem pomocí šablony Azure | Dokumentace Microsoftu"
description: "Virtuální počítač s Linuxem můžete na Azure vytvořit pomocí šablony Azure Resource Manageru."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: hero-article
ms.date: 10/24/2016
ms.author: v-livech
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: fa842efd99718be7fa9eaf8aac8030c32cbceeec
ms.openlocfilehash: bd0c9fd0044ce8a5de9004c68b1267b8b66df80a
ms.lasthandoff: 03/01/2017


---
# <a name="how-to-create-a-linux-vm-using-an-azure-resourec-manager-template"></a>Vytvoření virtuálního počítače s Linuxem pomocí šablony Azure Resource Manageru
Tento článek ukazuje, jak rychle nasadit virtuální počítač s Linuxem na platformě Azure pomocí šablony Azure.  Tento článek vyžaduje:

* účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/))
* [rozhraní příkazového řádku Azure](../xplat-cli-install.md) s přihlášením `azure login`.
* Rozhraní příkazového řádku Azure *musí být v *režimu Azure Resource Manager`azure config mode arm`.

Šablonu virtuálního počítače s Linuxem můžete rychle nasadit také pomocí webu [Azure Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="quick-command-summary"></a>Rychlý přehled příkazu
```azurecli
azure group create \
    -n myResourceGroup \
    -l westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Podrobný postup
Šablony umožňují vytvořit virtuální počítače na platformě Azure s nastavením, které si pak při spouštění upravíte, například uživatelská jména a názvy hostitelů. Pro tento článek spouštíme šablonu Azure využívající virtuální počítač Ubuntu společně se skupinu zabezpečení sítě (NSG) s portem 22 otevřeným pro SSH.

Šablony Azure Resource Manageru jsou soubory JSON, které jde použít k jednoduchým jednorázovým úlohám, jako je spuštění virtuálního počítače Ubuntu VM pro účely tohoto článku.  Šablony Azure jde také použít k vytvoření složitých konfigurací Azure pro celé prostředí, jako je sada pro nasazení testovacího, vývojového nebo produkčního prostředí.

## <a name="create-the-linux-vm"></a>Vytvoření virtuálního počítače s Linuxem
Následující příklad kódu ukazuje, jak voláním příkazu `azure group create` vytvořit skupinu prostředků a zároveň nasadit virtuální počítač s Linuxem se zabezpečením SSH pomocí [této šablony Azure Resource Manageru](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Je třeba myslet na to, že v příkladu musíte použít názvy, které jsou jedinečné pro vaše prostředí. V tomto příkladě se jako název skupiny prostředků používá `myResourceGroup` a jako název virtuálního počítače `myVM`.

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Výstup by měl vypadat jako následující výstupní blok:

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Tento příklad nasazuje virtuální počítač pomocí parametru `--template-uri`.  Můžete taky šablonu stáhnout nebo ji vytvořit místně a předat ji pomocí parametru `--template-file`cesta k souboru šablony. Rozhraní Azure CLI vás požádá o parametry vyžadované šablonou.

## <a name="next-steps"></a>Další kroky
V [galerii šablon](https://azure.microsoft.com/documentation/templates/) se podívejte, jaké aplikační architektury nasadit jako další.


