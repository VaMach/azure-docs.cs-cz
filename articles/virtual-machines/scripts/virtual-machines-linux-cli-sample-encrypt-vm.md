---
title: "Rozhraní příkazového řádku Azure ukázkový skript – zašifrovat virtuální počítač s Linuxem | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure ukázkový skript – zašifrovat virtuální počítač s Linuxem"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/02/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 5f5c57a9c5a20e6e6a514b5b4c9d2e040d504983
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="encrypt-a-linux-virtual-machine-in-azure"></a>Zašifrovat virtuální počítač s Linuxem v Azure

Tento skript vytvoří zabezpečené Azure Key Vault, šifrovacích klíčů, objektu služby Azure Active Directory a Linux virtuálního počítače (VM). Virtuální počítač se pak šifrují pomocí šifrovacího klíče z Key Vault a hlavní pověření služby.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků Azure Key Vault, instančního objektu, virtuální počítač a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření az keyvault](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_create) | Vytvoří Azure Key Vault k uložení zabezpečení dat, jako jsou šifrovací klíče. |
| [Vytvořte klíč keyvault az](https://docs.microsoft.com/cli/azure/keyvault/key#az_keyvault_key_create) | Vytvoří šifrovací klíč v Key Vault. |
| [AZ ad sp vytvořit pro rbac](https://docs.microsoft.com/cli/azure/ad/sp#az_ad_sp_create_for_rbac) | Vytvoří Azure Active Directory instanční objekt bezpečně ověřování a řízení přístupu k šifrovací klíče. |
| [AZ keyvault set zásad](https://docs.microsoft.com/cli/azure/keyvault#az_keyvault_set_policy) | Nastaví oprávnění pro Key Vault udělit přístup k hlavní službě pro šifrovací klíče. |
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a NSG. Tento příkaz také Určuje bitovou kopii virtuálního počítače, který se má použít a pověření pro správu.  |
| [Povolit šifrování az virtuálních počítačů](https://docs.microsoft.com/cli/azure/vm/encryption#az_vm_encryption_enable) | Povoluje šifrování na virtuálním počítači pomocí pověření hlavní služby a šifrovací klíč. |
| [Zobrazit šifrování az virtuálních počítačů](https://docs.microsoft.com/cli/azure/vm/encryption#az_vm_encryption_show) | Zobrazuje stav proces šifrování virtuálních počítačů. |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
