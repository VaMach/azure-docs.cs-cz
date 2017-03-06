---
title: "Vytvoření virtuálního počítače s Linuxem pomocí Azure CLI 2.0 | Microsoft Azure"
description: "Vytvoření virtuálního počítače s Linuxem pomocí Azure CLI 2.0"
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
ms.date: 01/13/2017
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 892e3c62a2ad4dc4fd0691874d46bb296e379524
ms.openlocfilehash: cc51b04c31c02aabf25c9efb1e9cd975077811a4
ms.lasthandoff: 02/27/2017


---

# <a name="create-a-linux-vm-using-the-azure-cli-20"></a>Vytvoření virtuálního počítače s Linuxem pomocí Azure CLI 2.0
Tento článek ukazuje, jak rychle nasadit virtuální počítač s Linuxem na platformě Azure pomocí příkazu [az vm create](/cli/azure/vm#create) rozhraní Azure CLI 2.0 s použitím spravovaných disků i disků v nativních účtech úložiště. K provedení těchto kroků můžete také využít [Azure CLI 1.0](virtual-machines-linux-quick-create-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

K vytvoření virtuálního počítače potřebujete: 

* účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)),
* nainstalované rozhraní [Azure CLI 2.0](/cli/azure/install-az-cli2),
* být přihlášeni ke svému účtu Azure (zadejte [az login](/cli/azure/#login)).

(Virtuální počítač s Linuxem můžete také nasadit pomocí webu [Azure Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).)

Následující příklad ukazuje nasazení virtuálního počítače se systémem Debian a připojení k němu pomocí klíče SSH (Secure Shell). Vaše argumenty se můžou lišit – pokud chcete jinou image, [můžete ji vyhledat](virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="using-managed-disks"></a>Použití spravovaných disků

Pokud chcete využívat spravované disky Azure, musíte použít oblast, která je podporuje. Nejprve zadejte příkaz [az group create](/cli/azure/group#create), kterým vytvoříte skupinu prostředků obsahující všechny nasazené prostředky:

```azurecli
 az group create -n myResourceGroup -l westus
```

Výstup vypadá takto (pokud chcete výstup zobrazit v jiném formátu, můžete zadat jinou možnost `--output`):

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "westus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```
### <a name="create-your-vm"></a>Vytvoření virtuálního počítače 
Nyní můžete vytvořit virtuální počítač a jeho prostředí. Nezapomeňte nahradit hodnotu `--public-ip-address-dns-name` jedinečnou hodnotou, níže uvedená hodnota již může být obsazená.

```azurecli
az vm create \
--image credativ:Debian:8:latest \
--admin-username azureuser \
--ssh-key-value ~/.ssh/id_rsa.pub \
--public-ip-address-dns-name manageddisks \
--resource-group myResourceGroup \
--location westus \
--name myVM
```


Výstup bude vypadat následovně. Poznamenejte si hodnotu `publicIpAddress` nebo `fqdn` pro přístup k virtuálnímu počítači přes **SSH**.


```json
{
  "fqdn": "manageddisks.westus.cloudapp.azure.com",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "macAddress": "00-0D-3A-32-E9-41",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "104.42.127.53",
  "resourceGroup": "myResourceGroup"
}
```

Přihlaste se k virtuálnímu počítači pomocí veřejné IP adresy nebo plně kvalifikovaného názvu domény (FQDN) vypsaného na výstupu.

```bash
ssh ops@manageddisks.westus.cloudapp.azure.com
```

V závislosti na distribuci, kterou jste zvolili, by se měl zobrazit podobný výstup:

```bash
The authenticity of host 'manageddisks.westus.cloudapp.azure.com (134.42.127.53)' can't be established.
RSA key fingerprint is c9:93:f5:21:9e:33:78:d0:15:5c:b2:1a:23:fa:85:ba.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'manageddisks.westus.cloudapp.azure.com' (RSA) to the list of known hosts.
Enter passphrase for key '/home/ops/.ssh/id_rsa':

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Jan 13 14:44:21 2017 from net-37-117-240-123.cust.vodafonedsl.it
ops@myVM:~$ 
```

V části [Další kroky](#next-steps) najdete další věci, které můžete dělat s novým virtuálním počítačem používajícím spravované disky.

## <a name="using-unmanaged-disks"></a>Použití nespravovaných disků 

Virtuální počítače, které používají nespravované disky úložiště, mají nespravované účty úložiště. Nejprve zadejte příkaz [az group create](/cli/azure/group#create), kterým vytvoříte skupinu prostředků k obsáhnutí všech nasazených prostředků:

```azurecli
az group create --name nativedisks --location westus
```

Výstup vypadá takto (pokud chcete, můžete zvolit jinou možnost `--output`):

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/nativedisks",
  "location": "westus",
  "managedBy": null,
  "name": "nativedisks",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

### <a name="create-your-vm"></a>Vytvoření virtuálního počítače 

Nyní můžete vytvořit virtuální počítač a jeho prostředí. K vytvoření virtuálního počítače s nespravovanými disky použijte příznak `--use-unmanaged-disk`. Vytvoří se také účet nespravovaného úložiště. Nezapomeňte nahradit hodnotu `--public-ip-address-dns-name` jedinečnou hodnotou, níže uvedená hodnota již může být obsazená.

```azurecli
az vm create \
--image credativ:Debian:8:latest \
--admin-username azureuser \
--ssh-key-value ~/.ssh/id_rsa.pub \
--public-ip-address-dns-name nativedisks \
--resource-group nativedisks \
--location westus \
--name myVM \
--use-unmanaged-disk
```

Výstup bude vypadat následovně. Poznamenejte si hodnotu `publicIpAddress` nebo `fqdn` pro přístup k virtuálnímu počítači přes **SSH**.

```json
{
  "fqdn": "nativedisks.westus.cloudapp.azure.com",
  "id": "/subscriptions/<guid>/resourceGroups/nativedisks/providers/Microsoft.Compute/virtualMachines/myVM",
  "macAddress": "00-0D-3A-33-24-3C",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.91.91.195",
  "resourceGroup": "nativedisks"
}
```

Přihlaste se k virtuálnímu počítači pomocí veřejné IP adresy nebo plně kvalifikovaného názvu domény (FQDN) – obojí je vypsáno na výše uvedeném výstupu.

```bash
ssh ops@nativedisks.westus.cloudapp.azure.com
```

V závislosti na distribuci, kterou jste zvolili, by se měl zobrazit podobný výstup:

```
The authenticity of host 'nativedisks.westus.cloudapp.azure.com (13.91.93.195)' can't be established.
RSA key fingerprint is 3f:65:22:b9:07:c9:ef:7f:8c:1b:be:65:1e:86:94:a2.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'nativedisks.westus.cloudapp.azure.com,13.91.93.195' (RSA) to the list of known hosts.
Enter passphrase for key '/home/ops/.ssh/id_rsa':

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
ops@myVM:~$ ls /
bin  boot  dev  etc  home  initrd.img  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  vmlinuz
```

## <a name="next-steps"></a>Další kroky
Příkaz `az vm create` představuje způsob, jak rychle nasadit virtuální počítač, abyste se mohli přihlásit k prostředí Bash a začít pracovat. Použití `az vm create` ale neposkytuje větší možnosti kontroly ani neumožňuje vytvářet složitější prostředí.  Pokud budete chtít nasadit virtuální počítač s Linuxem přizpůsobený vaší infrastruktuře, můžete postupovat podle některého z těchto článků:

* [Vytvoření konkrétního nasazení pomocí šablony Azure Resource Manageru](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Přímé vytvoření vlastního prostředí pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure CLI](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* [Vytvoření virtuálního počítače s Linuxem se zabezpečením SSH na platformě Azure pomocí šablon](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

K [rychlému vytvoření virtuálního počítače s Linuxem jako hostitele Docker můžete použít také ovladač Azure `docker-machine` s různými příkazy](virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Pokud používáte Javu, vyzkoušejte metodu [create()](/java/api/com.microsoft.azure.management.compute._virtual_machine).


