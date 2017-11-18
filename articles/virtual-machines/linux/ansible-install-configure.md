---
title: "Instalace a konfigurace Ansible pro použití s virtuálními počítači Azure | Microsoft Docs"
description: "Zjistěte, jak nainstalovat a nakonfigurovat Ansible pro správu prostředků Azure na SLES, Ubuntu a CentOS"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: iainfou
ms.openlocfilehash: c5257ef5c635080f5eaca371e1882b13cc37e0fd
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/18/2017
---
# <a name="install-and-configure-ansible-to-manage-virtual-machines-in-azure"></a>Instalace a konfigurace Ansible ke správě virtuálních počítačů v Azure
Tento článek podrobně popisuje postup instalace Ansible a požadované moduly Azure Python SDK pro některé z nejběžnějších distribucích systému Linux. Ansible můžete nainstalovat na jiné distribucích úpravou nainstalované balíčky podle vaší konkrétní platformu. Vytváření prostředků Azure zabezpečeným způsobem, můžete také zjistěte, jak vytvořit a definovat přihlašovací údaje pro Ansible používat. 

Další možnosti instalace a kroky pro další platformy najdete v tématu [Průvodce instalací Ansible](https://docs.ansible.com/ansible/intro_installation.html).


## <a name="install-ansible"></a>Nainstalujte Ansible
Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupAnsible* v *eastus* umístění:

```azurecli
az group create --name myResourceGroupAnsible --location eastus
```

Teď vytvořte virtuální počítač a nainstalujte Ansible pro jednu z následujících distribucích:

- [Ubuntu 16.04 LTS](#ubuntu1604-lts)
- [CentOS 7.3](#centos-73)
- [SLES 12 SP2](#sles-12-sp2)

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS
Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create). Následující příklad vytvoří virtuální počítač s názvem *myVMAnsible*:

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH pro virtuální počítač pomocí `publicIpAddress` uvedeno ve výstupu z virtuálního počítače vytvořit operace:

```bash
ssh azureuser@<publicIpAddress>
```

Na vašem virtuálním počítači nainstalujte požadované balíčky pro moduly Azure Python SDK a Ansible následujícím způsobem:

```bash
## Install pre-requisite packages
sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip

## Install Ansible and Azure SDKs via pip
pip install ansible[azure]
```

Nyní se přesunout na [přihlašovací údaje Azure vytvořit](#create-azure-credentials).


### <a name="centos-73"></a>CentOS 7.3
Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create). Následující příklad vytvoří virtuální počítač s názvem *myVMAnsible*:

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH pro virtuální počítač pomocí `publicIpAddress` uvedeno ve výstupu z virtuálního počítače vytvořit operace:

```bash
ssh azureuser@<publicIpAddress>
```

Na vašem virtuálním počítači nainstalujte požadované balíčky pro moduly Azure Python SDK a Ansible následujícím způsobem:

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Nyní se přesunout na [přihlašovací údaje Azure vytvořit](#create-azure-credentials).


### <a name="sles-12-sp2"></a>SLES 12 SP2
Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create). Následující příklad vytvoří virtuální počítač s názvem *myVMAnsible*:

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image SLES \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH pro virtuální počítač pomocí `publicIpAddress` uvedeno ve výstupu z virtuálního počítače vytvořit operace:

```bash
ssh azureuser@<publicIpAddress>
```

Na vašem virtuálním počítači nainstalujte požadované balíčky pro moduly Azure Python SDK a Ansible následujícím způsobem:

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 python-devel \
    libopenssl-devel libtool python-pip python-setuptools

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Nyní se přesunout na [přihlašovací údaje Azure vytvořit](#create-azure-credentials).


## <a name="create-azure-credentials"></a>Vytvořit přihlašovací údaje Azure
Ansible komunikuje se službou Azure pomocí uživatelského jména a hesla nebo hlavní název služby. Objektu zabezpečení služby Azure je identita zabezpečení, která můžete použít s aplikací, služeb a automatizace nástroje, například Ansible. Můžete řídit a definovat oprávnění, jaké operace objektu služby můžete provádět v Azure. Pokud chcete zvýšit zabezpečení přes právě poskytnutí uživatelského jména a hesla, tento příklad vytvoří základní služby hlavní.

Vytvoření služby hlavní s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) a výstupní přihlašovací údaje, které potřebuje Ansible:

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

Příklad výstupu z předchozích příkazů vypadá takto:

```json
[
  "eec5624a-90f8-4386-8a87-02730b5410d5",
  "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "72f988bf-86f1-41af-91ab-2d7cd011db47"
]
```

K ověření do Azure, musíte také získat svoje ID předplatného Azure s [az účet zobrazit](/cli/azure/account#show):

```azurecli
az account show --query [id] --output tsv
```

V dalším kroku použijete výstup z těchto dvou příkazů.


## <a name="create-ansible-credentials-file"></a>Vytvořte soubor s přihlašovacími údaji Ansible
K zadání přihlašovacích údajů k Ansible, můžete definovat proměnné prostředí nebo vytvoření přihlašovacích údajů místního souboru. Další informace o tom, jak definovat Ansible pověření najdete v tématu [poskytování pověření moduly Azure](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules). 

Vývojové prostředí, vytvářet *pověření* souboru Ansible na hostiteli virtuálního počítače takto:

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

*Pověření* samotný soubor kombinuje ID předplatného s výstupem vytvoření objektu služby. Výstup z předchozí [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) příkaz je stejné pořadí podle potřeby pro *client_id*, *tajný klíč*, a *klienta*. Následující příklad *pověření* soubor znázorňuje tyto hodnoty odpovídající předchozí výstup. Zadejte vlastní hodnoty následujícím způsobem:

```bash
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=eec5624a-90f8-4386-8a87-02730b5410d5
secret=531dcffa-3aff-4488-99bb-4816c395ea3f
tenant=72f988bf-86f1-41af-91ab-2d7cd011db47
```


## <a name="use-ansible-environment-variables"></a>Použití proměnných prostředí Ansible
Pokud budete používat nástroje, například Ansible věž nebo volaných, můžete definovat proměnné prostředí následujícím způsobem. Tyto proměnné kombinovat s výstupem vytvoření služby Hlavní ID předplatného. Výstup z předchozí [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) příkaz je stejné pořadí podle potřeby pro *AZURE_CLIENT_ID*, *AZURE_SECRET*, a *AZURE_TENANT*. 

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=eec5624a-90f8-4386-8a87-02730b5410d5
export AZURE_SECRET=531dcffa-3aff-4488-99bb-4816c395ea3f
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a>Další kroky
Nyní máte Ansible a požadované moduly Azure Python SDK nainstalovat a přihlašovací údaje definované pro Ansible používat. Zjistěte, jak [vytvoření virtuálního počítače s Ansible](ansible-create-vm.md). Můžete si také přečíst postup [kompletní virtuální počítače Azure můžete vytvořit a podpůrné prostředky s Ansible](ansible-create-complete-vm.md).
