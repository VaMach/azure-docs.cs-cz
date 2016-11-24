---
title: "Vytvoření virtuálního počítače s Linuxem pomocí Azure CLI 2.0 (Preview) | Microsoft Azure"
description: "Vytvoření virtuálního počítače s Linuxem pomocí Azure CLI 2.0 (Preview)."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: 
ms.assetid: 82005a05-053d-4f52-b0c2-9ae2e51f7a7e
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/26/2016
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 317d6980d304cc503cc43358c4b91459d4abd1ba
ms.openlocfilehash: 6b08f27a27a31fcc665ab051438a962ddc711767


---

# <a name="create-a-linux-vm-using-the-azure-cli-20-preview"></a>Vytvoření virtuálního počítače s Linuxem pomocí Azure CLI 2.0 (Preview)
Tento článek ukazuje, jak rychle nasadit virtuální počítač s Linuxem na platformě Azure pomocí příkazu [az-vm-create](/cli/azure/vm?branch=master#create) v Azure CLI 2.0 (Preview). 

> [!NOTE] 
> Azure CLI 2.0 Preview je naše multiplatformní rozhraní příkazového řádku příští generace. Vyzkoušejte si jej a dejte nám vědět, co si myslíte na [stránce projektu na GitHubu](https://github.com/Azure/azure-cli).
>
> Naše ostatní dokumentace používá stávající Azure CLI. Informace o vytvoření virtuálního počítače pomocí stávajícího Azure CLI, a ne CLI 2.0 Preview, najdete v tématu [Vytvoření virtuálního počítače pomocí Azure CLI](virtual-machines-linux-quick-create-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

K vytvoření virtuálního počítače potřebujete: 

* účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)),
* nainstalované rozhraní [Azure CLI verze 2.0 (Preview)](https://github.com/Azure/azure-cli#installation),
* být přihlášeni ke svému účtu Azure (zadejte [az login](/cli/azure/#login)).

(Virtuální počítač s Linuxem můžete rychle nasadit také pomocí webu [Azure Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).)

Následující příklad ukazuje, jak nasadit virtuální počítač s Debian a připojit klíč SSH (Secure Shell). Vaše argumenty ale mohou být jiné – pokud chcete jinou image, můžete [ji vyhledat](virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Nejprve zadejte příkaz [az resource group create](/cli/azure/resource/group#create), kterým vytvoříte skupinu prostředků obsahující všechny nasazené prostředky:

```azurecli
az resource group create -n myResourceGroup -l westus
```

Výstup vypadá takto (pokud chcete, můžete zvolit jinou možnost `--output`):

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "westus",
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-your-vm-using-the-latest-debian-image"></a>Vytvoření virtuálního počítače pomocí nejnovější image Debian

Nyní můžete vytvořit virtuální počítač a jeho prostředí. Nezapomeňte nahradit hodnotu `----public-ip-address-dns-name` jedinečnou hodnotou, níže uvedená hodnota již může být obsazená.

```azurecli
az vm create \
--image credativ:Debian:8:latest \
--admin-username ops \
--ssh-key-value ~/.ssh/id_rsa.pub \
--public-ip-address-dns-name mydns \
--resource-group myResourceGroup \
--location westus \
--name myVM
```


Výstup bude vypadat následovně. Poznamenejte si hodnotu `publicIpAddress` nebo `fqdn` pro přístup k virtuálnímu počítači přes **SSH**.


```json
{
  "fqdn": "mydns.westus.cloudapp.azure.com",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "macAddress": "00-0D-3A-32-05-07",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.112.217.29",
  "resourceGroup": "myResourceGroup"
}
```

Přihlaste se do vašeho virtuálního počítače pomocí veřejné IP adresy, která je uvedená ve výstupu. Můžete také využít uvedený plně kvalifikovaný název domény.

```bash
ssh ops@mydns.westus.cloudapp.azure.com
```

V závislosti na distribuci, kterou jste zvolili, by se měl zobrazit podobný výstup:

```
The authenticity of host 'mydns.westus.cloudapp.azure.com (40.112.217.29)' can't be established.
RSA key fingerprint is SHA256:xbVC//lciRvKild64lvup2qIRimr/GB8C43j0tSHWnY.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'mydns.westus.cloudapp.azure.com,40.112.217.29' (RSA) to the list of known hosts.

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
ops@mynewvm:~$ ls /
bin  boot  dev  etc  home  initrd.img  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  vmlinuz
```

## <a name="next-steps"></a>Další kroky
Příkaz `az vm create` představuje způsob, jak rychle nasadit virtuální počítač, abyste se mohli přihlásit k prostředí Bash a začít pracovat. Použití `az vm create` ale neposkytuje větší možnosti kontroly ani neumožňuje vytvářet složitější prostředí.  Pokud budete chtít nasadit virtuální počítač s Linuxem přizpůsobený vaší infrastruktuře, můžete postupovat podle některého z těchto článků:

* [Vytvoření konkrétního nasazení pomocí šablony Azure Resource Manageru](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Vytvoření virtuálního počítače s Linuxem se zabezpečením SSH na platformě Azure pomocí šablon](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

K [rychlému vytvoření virtuálního počítače s Linuxem jako hostitele Docker můžete použít také ovladač Azure `docker-machine` s různými příkazy](virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Pokud používáte Javu, vyzkoušejte metodu [create()](/java/api/com.microsoft.azure.management.compute._virtual_machine).




<!--HONumber=Nov16_HO3-->


