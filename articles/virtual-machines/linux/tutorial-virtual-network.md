---
title: "Virtuální sítě Azure a virtuální počítače s Linuxem | Microsoft Docs"
description: "Kurz – Správa virtuálních sítí Azure a virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure CLI"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 0e7f4308290a14e592cf1739fa5b0b3360d7c68b
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="manage-azure-virtual-networks-and-linux-virtual-machines-with-the-azure-cli"></a>Správa virtuálních sítí Azure a virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure CLI

Virtuální počítače Azure pomocí Azure sítě pro vnitřní a vnější síťovou komunikaci. Tento kurz vás provede nasazením dva virtuální počítače a konfigurace Azure sítě pro tyto virtuální počítače. V příkladech v tomto kurzu předpokládá, že virtuální počítače hostují webové aplikace s databáze back-end, ale není v tomto kurzu nasazené aplikace. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření virtuální sítě a podsítě
> * Vytvoření veřejné IP adresy
> * Vytvoření virtuálního počítače s front-endu
> * Zabezpečení provozu sítě
> * Vytvoření virtuálního počítače s back-end

Při dokončení tohoto kurzu, zobrazí se tyto prostředky vytvořené:

![Virtuální síť se dvěma podsítěmi](./media/tutorial-virtual-network/networktutorial.png)

- *myVNet* -virtuální síť, která virtuální počítače používat ke komunikaci s sebe navzájem a Internetem.
- *myFrontendSubnet* -podsítě v *myVNet* používá front-endové prostředky.
- *myPublicIPAddress* -veřejnou IP adresu použitou pro přístup k *myFrontendVM* z Internetu.
- *myFrontentNic* -síťové rozhraní používá *myFrontendVM* ke komunikaci s *myBackendVM*.
- *myFrontendVM* -virtuálního počítače používá ke komunikaci mezi Internetu a *myBackendVM*.
- *myBackendNSG* -skupina zabezpečení sítě, které řídí komunikaci mezi *myFrontendVM* a *myBackendVM*.
- *myBackendSubnet* -podsíť k *myBackendNSG* a používané prostředky back-end.
- *myBackendNic* -síťové rozhraní používá *myBackendVM* ke komunikaci s *myFrontendVM*.
- *myBackendVM* -virtuální počítač, který používá port 22 a 3306 ke komunikaci s *myFrontendVM*.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="vm-networking-overview"></a>Přehled sítě virtuálních počítačů

Virtuální sítě Azure povolit zabezpečené sítě připojení mezi virtuálními počítači, internet a jinými službami Azure, jako je například Azure SQL database. Virtuální sítě jsou rozdělit do logických segmenty označují jako podsítě. Podsítě se používají k řízení toku sítě a jako hranice zabezpečení. Při nasazování virtuálního počítače, obvykle zahrnuje rozhraní virtuální sítě, který je připojen k podsíti.

## <a name="create-a-virtual-network-and-subnet"></a>Vytvoření virtuální sítě a podsítě

V tomto kurzu se vytvoří jedné virtuální sítě se dvěma podsítěmi. Podsíť front-end pro hostování webové aplikace a podsíť back-end pro hostování databázový server.

Než bude možné vytvořit virtuální síť, vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Následující příklad vytvoří skupinu prostředků s názvem *myRGNetwork* v eastus umístění.

```azurecli-interactive 
az group create --name myRGNetwork --location eastus
```

### <a name="create-virtual-network"></a>Vytvoření virtuální sítě

Použití [vytvoření sítě vnet az](/cli/azure/network/vnet#create) příkaz pro vytvoření virtuální sítě. V tomto příkladu je název sítě *mvVNet* a předponu adresy z *10.0.0.0/16*. Podsíť je vytvořen také s názvem *myFrontendSubnet* a předponu *10.0.1.0/24*. Později v tomto kurzu je front-end virtuálního počítače připojené k této podsíti. 

```azurecli-interactive 
az network vnet create \
  --resource-group myRGNetwork \
  --name myVNet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myFrontendSubnet \
  --subnet-prefix 10.0.1.0/24
```

### <a name="create-subnet"></a>Vytvoření podsítě

Novou podsíť je přidaný do virtuální sítě pomocí [az sítě vnet podsíť vytváření](/cli/azure/network/vnet/subnet#create) příkaz. V tomto příkladu je název podsítě *myBackendSubnet* a předponu adresy z *10.0.2.0/24*. Tato podsíť se používá u všech služeb back-end.

```azurecli-interactive 
az network vnet subnet create \
  --resource-group myRGNetwork \
  --vnet-name myVNet \
  --name myBackendSubnet \
  --address-prefix 10.0.2.0/24
```

V tomto okamžiku síť byla vytvořil a rozdělena na dvě podsítě, jeden pro front-endové služby a jinou pro back endové služby. V další části jsou virtuální počítače vytvořené a připojené k tyto podsítě.

## <a name="create-a-public-ip-address"></a>Vytvoření veřejné IP adresy

Veřejná IP adresa umožňuje prostředků Azure bude přístupný na Internetu. Metoda přidělení veřejné IP adresy můžete konfigurovat jako dynamické nebo statické. Ve výchozím nastavení se přidělí dynamicky veřejnou IP adresu. Dynamické IP adresy vydávají, když virtuální počítač je navrácena. To způsobí, že IP adresa, chcete-li změnit během všechny operace, která zahrnuje navrácení virtuálních počítačů.

Statický, které zajišťuje, že IP adresa zůstane i během deallocated stavu přiřazené k virtuálnímu počítači, může být nastavena metoda přidělení. Pokud používáte staticky přidělená adresa IP, nelze zadat IP adresu. Místo toho je přidělen z fondu adres k dispozici.

```azurecli-interactive
az network public-ip create --resource-group myRGNetwork --name myPublicIPAddress
```

Při vytváření virtuálního počítače s [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz, je dynamický metodu přidělení výchozí veřejnou IP adresu. Při vytváření virtuálního počítače pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz, zahrnout `--public-ip-address-allocation static` argument přiřadit statickou veřejnou IP adresu. Tato operace není ukázáno v tomto kurzu, ale v další části se změní dynamicky přidělené IP adresy na staticky přidělené adresy. 

### <a name="change-allocation-method"></a>Změnit přidělení – metoda

Metoda přidělení IP adres se dá změnit pomocí [aktualizace veřejné ip sítě az](/cli/azure/network/public-ip#update) příkaz. V tomto příkladu metoda přidělení IP adresy front-endu virtuálního počítače se změní na statické.

Nejprve zrušit přidělení virtuálního počítače.

```azurecli-interactive 
az vm deallocate --resource-group myRGNetwork --name myFrontendVM
```

Použití [aktualizace veřejné ip sítě az](/cli/azure/network/public-ip#update) příkaz k aktualizaci metoda přidělení. V takovém případě `--allocation-method` je nastavena na *statické*.

```azurecli-interactive 
az network public-ip update --resource-group myRGNetwork --name myPublicIPAddress --allocation-method static
```

Spusťte virtuální počítač.

```azurecli-interactive 
az vm start --resource-group myRGNetwork --name myFrontendVM --no-wait
```

### <a name="no-public-ip-address"></a>Žádné veřejnou IP adresu

Virtuální počítač se často, nemusí být přístupné přes internet. Chcete-li vytvořit virtuální počítač bez veřejnou IP adresu, použijte `--public-ip-address ""` argument s prázdnou sadu dvojité uvozovky. Tato konfigurace je ukázán později v tomto kurzu.

## <a name="create-a-front-end-vm"></a>Vytvoření virtuálního počítače s front-endu

Použití [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz pro vytvoření virtuálního počítače s názvem *myFrontendVM* pomocí *myPublicIPAddress*.

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myFrontendVM \
  --vnet-name myVNet \
  --subnet myFrontendSubnet \
  --nsg myFrontendNSG \
  --public-ip-address myPublicIPAddress \
  --image UbuntuLTS \
  --generate-ssh-keys
```

## <a name="secure-network-traffic"></a>Zabezpečení provozu sítě

Skupina zabezpečení sítě (NSG) obsahuje seznam pravidel zabezpečení, která prostředkům připojeným k virtuálním sítím Azure povolují nebo odpírají síťový provoz. Skupiny Nsg můžou být přidružena k podsítě nebo jednotlivých síťových rozhraní. Když skupinu NSG je spojen s síťovým rozhraním, bude se vztahovat jenom přidružený virtuální počítač. Pokud je skupina zabezpečení sítě přidružená k podsíti, pravidla se vztahují na všechny prostředky, které jsou připojené k příslušné podsíti. 

### <a name="network-security-group-rules"></a>Pravidla skupiny zabezpečení sítě

Pravidla NSG definovat síťové porty, přes které provoz povolený nebo zakázaný. Pravidla mohou obsahovat zdrojové a cílové rozsahy IP adres, tak, aby je řízen provoz mezi konkrétní systémy nebo podsítě. Pravidla NSG také zahrnovat prioritu (mezi 1 – a 4096). Pravidla jsou vyhodnocovány v pořadí podle priority. Pravidlo s prioritou 100 vyhodnotí před pravidlo s prioritou 200.

Všechny skupiny NSG obsahují sadu výchozích pravidel. Výchozí pravidla se nedají odstranit, ale protože je jim přiřazená nejnižší priorita, dají se přepsat pravidly, která vytvoříte.

Výchozí pravidla pro skupiny Nsg jsou:

- **Virtuální síť** – provoz pocházející a ukončování ve virtuální síti je povolena v příchozí a odchozí.
- **Internet** – odchozí provoz je povolený, ale jsou blokovány příchozí přenosy.
- **Nástroj pro vyrovnávání zatížení** – nástroj pro vyrovnávání zatížení povolit Azure testovat stav virtuálních počítačů a instancí rolí. Pokud nepoužíváte skupinu s vyrovnáváním zatížení, můžete přepsat toto pravidlo.

### <a name="create-network-security-groups"></a>Vytvoření skupiny zabezpečení sítě

Lze vytvořit skupinu zabezpečení sítě ve stejnou dobu jako virtuální počítač pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz. Když to uděláte, se NSG k rozhraní sítě virtuálních počítačů a automaticky vytvořit, aby povolovala přenosy na portu je pravidlo NSG *22* z jakéhokoli zdroje. V tomto kurzu front-end NSG se automaticky vytvořené s front-endu virtuálního počítače. Pravidlo NSG se také automaticky vytvořenou pro port 22. 

V některých případech může být užitečné předem vytvořit skupinu NSG, například když by neměly být vytvářeny výchozí pravidla pro SSH, nebo když by mělo být připojeno NSG k podsíti. 

Použití [vytvořit az sítě nsg](/cli/azure/network/nsg#create) příkazu vytvořte skupinu zabezpečení sítě.

```azurecli-interactive 
az network nsg create --resource-group myRGNetwork --name myBackendNSG
```

Místo přidružení skupiny NSG k síťové rozhraní, je přidružená k podsíti. V této konfiguraci dědí žádné virtuální počítače, který je připojen k podsíti pravidla NSG.

Aktualizovat existující podsíť s názvem *myBackendSubnet* se nová skupina NSG.

```azurecli-interactive 
az network vnet subnet update \
  --resource-group myRGNetwork \
  --vnet-name myVNet \
  --name myBackendSubnet \
  --network-security-group myBackendNSG
```

### <a name="secure-incoming-traffic"></a>Zabezpečené příchozí provoz

V okamžiku vytvoření front-endu virtuálního počítače bylo pravidlo NSG vytvořeno povolit příchozí přenosy na portu 22. Toto pravidlo umožňuje připojení SSH pro virtuální počítač. V tomto příkladu má být povoleno přenosy na portu *80*. Tato konfigurace umožňuje webovou aplikaci nelze přistupovat ve virtuálním počítači.

Použití [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) příkaz k vytvoření pravidla pro port *80*.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myFrontendNSG \
  --name http \
  --access allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 80
```

Front-endu virtuálního počítače je k dispozici pouze na portu *22* a port *80*. Všechny ostatní příchozí přenosy je blokováno v skupinu zabezpečení sítě. Může být užitečné k vizualizaci konfigurace pravidel NSG. Konfigurace pravidla NSG se vraťte [az sítě pravidlo seznamu](/cli/azure/network/nsg/rule#list) příkaz. 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myFrontendNSG --output table
```

### <a name="secure-vm-to-vm-traffic"></a>Zabezpečený virtuální počítač k přenosy virtuálních počítačů

Pravidla skupiny zabezpečení sítě můžete také použít mezi virtuálními počítači. V tomto příkladu musí front-end virtuálních počítačů komunikovat s back-end virtuálního počítače na portu *22* a *3306*. Tato konfigurace umožňuje připojení SSH z front-endu virtuálního počítače a také povolit aplikace front-endu virtuálního počítače ke komunikaci s databází MySQL back-end. Všechny ostatní přenosy by se zablokovat mezi virtuálními počítači front-end a back-end.

Použití [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) příkaz k vytvoření pravidla pro port 22. Všimněte si, že `--source-address-prefix` argument určuje hodnotu *10.0.1.0/24*. Tato konfigurace zajistí, že jsou povoleny pouze přenosy z podsítě front-endu prostřednictvím NSG.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myBackendNSG \
  --name SSH \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "22"
```

Teď můžete přidáte pravidlo pro provoz MySQL na portu 3306.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myBackendNSG \
  --name MySQL \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "3306"
```

Navíc vzhledem k tomu, že skupiny Nsg výchozí pravidlo povolující veškerý provoz mezi virtuálními počítači ve stejné virtuální síti, lze vytvořit pravidlo pro skupiny Nsg back-end zablokovat veškerý provoz. Všimněte si zde, že `--priority` je zadána hodnota *300*, která je nižší tohoto pravidla NSG i MySQL. Tato konfigurace zajistí, že provoz SSH a MySQL stále může prostřednictvím NSG.

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myBackendNSG \
  --name denyAll \
  --access Deny \
  --protocol Tcp \
  --direction Inbound \
  --priority 300 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "*"
```

## <a name="create-back-end-vm"></a>Vytvoření virtuálního počítače s back-end

Teď vytvořte virtuální počítač, který je připojen k *myBackendSubnet*. Všimněte si, že `--nsg` argument má hodnotu prázdný dvojité uvozovky. Skupina NSG nemusí být vytvořen s virtuálním Počítačem. Virtuální počítač je připojený k podsíti back-end, který je chráněn s předem vytvořené NSG back-end. Tato skupina NSG se vztahuje na virtuální počítač. Také si zde všimnout, který `--public-ip-address` argument má hodnotu prázdný dvojité uvozovky. Tato konfigurace vytvoří virtuální počítač bez veřejnou IP adresu. 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myBackendVM \
  --vnet-name myVNet \
  --subnet myBackendSubnet \
  --public-ip-address "" \
  --nsg "" \
  --image UbuntuLTS \
  --generate-ssh-keys
```

Virtuální počítač back-end je k dispozici pouze na portu *22* a port *3306* z podsítě front-endu. Všechny ostatní příchozí přenosy je blokováno v skupinu zabezpečení sítě. Může být užitečné k vizualizaci konfigurace pravidel NSG. Konfigurace pravidla NSG se vraťte [az sítě pravidlo seznamu](/cli/azure/network/nsg/rule#list) příkaz. 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myBackendNSG --output table
```

## <a name="next-steps"></a>Další postup

V tomto kurzu jste vytvořili a zabezpečené sítě v souvislosti s virtuálními počítači Azure. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření virtuální sítě a podsítě
> * Vytvoření veřejné IP adresy
> * Vytvoření virtuálního počítače s front-endu
> * Zabezpečení provozu sítě
> * Vytvoření virtuálního počítače s back-end

Přechodu na v dalším kurzu se dozvíte o zabezpečení dat na virtuální počítače pomocí zálohování Azure. 

> [!div class="nextstepaction"]
> [Zálohovat virtuální počítače s Linuxem v Azure](./tutorial-backup-vms.md)
