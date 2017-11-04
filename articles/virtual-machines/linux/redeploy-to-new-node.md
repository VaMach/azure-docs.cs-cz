---
title: "Znovu nasadit virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Jak znovu nasadit virtuální počítače s Linuxem v Azure a zmírnit problémy připojení SSH."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/23/2017
ms.author: iainfou
ms.openlocfilehash: 7bf69b2a3c006faa0dc0144313e5ebb64e941e2d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="redeploy-linux-virtual-machine-to-new-azure-node"></a>Znovu nasaďte virtuální počítač s Linuxem do nového uzlu Azure
Pokud čelí potíží při řešení potíží s SSH nebo mohou pomoci přístup k aplikaci pro systém Linux virtuálního počítače (VM) v Azure, opětovného nasazení virtuálního počítače. Při opětovném nasazování virtuálního počítače, virtuální počítač přesune do nového uzlu v rámci infrastruktury Azure a pak ho znovu zapne. Možnosti konfigurace a přidružené prostředky zůstanou zachovány. Tento článek ukazuje, jak znovu nasadit virtuální počítač pomocí rozhraní příkazového řádku Azure nebo portálu Azure.

> [!NOTE]
> Po opětovném virtuálního počítače, dojde ke ztrátě dočasným diskovým a dynamické IP adresy přidružené k rozhraní virtuální sítě se aktualizují. 

Můžete znovu nasadit virtuální počítač pomocí jedné z následujících možností. Potřebujete jenom jednu možnost znovu nasadit virtuální počítač:

- [Azure CLI 2.0](#azure-cli-20)
- [Azure CLI 1.0](#azure-cli-10)
- [Azure Portal](#using-azure-portal)

## <a name="use-the-azure-cli-20"></a>Použití Azure CLI 2.0
Nainstalujte si nejnovější verzi [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se k Azure účet pomocí [az přihlášení](/cli/azure/#login).

Opětovné nasazení virtuálního počítače s [az virtuálního počítače znovu ho zaveďte](/cli/azure/vm#redeploy). Následující příklad opětovně nasadí virtuální počítač s názvem *Můjvp* ve skupině prostředků s názvem *myResourceGroup*:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

## <a name="use-the-azure-cli-10"></a>Použití Azure CLI 1.0
Nainstalujte [nejnovější Azure CLI 1.0](../../cli-install-nodejs.md), přihlaste se k účtu Azure a ujistěte se, že jste v režimu Resource Manager (`azure config mode arm`).

Následující příklad opětovně nasadí virtuální počítač s názvem *Můjvp* ve skupině prostředků s názvem *myResourceGroup*:

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Další kroky
Pokud máte problémy s připojením k virtuálnímu počítači, můžete najít konkrétní pomoc na [řešení potíží s připojení SSH](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [podrobné SSH při řešení potíží](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Pokud nelze získat přístup k aplikaci běžící na vašem virtuálním počítači, můžete si také přečíst [řešení potíží s aplikací](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

