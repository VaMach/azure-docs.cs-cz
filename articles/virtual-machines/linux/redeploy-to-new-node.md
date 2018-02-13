---
title: "Znovu nasadit virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Jak znovu nasadit virtuální počítače s Linuxem v Azure a zmírnit problémy připojení SSH."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/14/2017
ms.author: iainfou
ms.openlocfilehash: 48b4e5f2429ce2bd8a875b084694f83e467b5575
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="redeploy-linux-virtual-machine-to-new-azure-node"></a>Znovu nasaďte virtuální počítač s Linuxem do nového uzlu Azure
Pokud čelí potíží při řešení potíží s SSH nebo mohou pomoci přístup k aplikaci pro systém Linux virtuálního počítače (VM) v Azure, opětovného nasazení virtuálního počítače. Při opětovném nasazování virtuálního počítače, virtuální počítač přesune do nového uzlu v rámci infrastruktury Azure a pak ho znovu zapne. Možnosti konfigurace a přidružené prostředky zůstanou zachovány. Tento článek ukazuje, jak znovu nasadit virtuální počítač pomocí rozhraní příkazového řádku Azure nebo portálu Azure.

> [!NOTE]
> Po opětovném virtuálního počítače, dojde ke ztrátě dočasným diskovým a dynamické IP adresy přidružené k rozhraní virtuální sítě se aktualizují. 

Můžete znovu nasadit virtuální počítač pomocí jedné z následujících možností. Potřebujete jenom jednu možnost znovu nasadit virtuální počítač:

- [Azure CLI 2.0](#azure-cli-20)
- [Azure CLI 1.0](#azure-cli-10)
- [portál Azure Portal](#using-azure-portal)

## <a name="use-the-azure-cli-20"></a>Použití Azure CLI 2.0
Nainstalujte si nejnovější verzi [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se do vaší Azure účet pomocí [az přihlášení](/cli/azure/#az_login).

Opětovné nasazení virtuálního počítače s [az virtuálního počítače znovu ho zaveďte](/cli/azure/vm#az_vm_redeploy). Následující příklad opětovně nasadí virtuální počítač s názvem *Můjvp* ve skupině prostředků s názvem *myResourceGroup*:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

## <a name="use-the-azure-cli-10"></a>Použití Azure CLI 1.0
Nainstalujte [nejnovější Azure CLI 1.0](../../cli-install-nodejs.md) a přihlaste se k účtu Azure. Ujistěte se, že jste v režimu Resource Manager (`azure config mode arm`).

Následující příklad opětovně nasadí virtuální počítač s názvem *Můjvp* ve skupině prostředků s názvem *myResourceGroup*:

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Další postup
Pokud máte problémy s připojením k virtuálnímu počítači, můžete najít konkrétní pomoc na [řešení potíží s připojení SSH](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [podrobné SSH při řešení potíží](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Pokud nelze získat přístup k aplikaci běžící na vašem virtuálním počítači, můžete si také přečíst [řešení potíží s aplikací](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

