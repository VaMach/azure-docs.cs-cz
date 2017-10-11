---
title: "Otevřete porty, které se virtuální počítač s Linuxem pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Zjistěte, jak otevřít port / create koncového bodu virtuálním počítačům s Linuxem pomocí modelu nasazení Azure resource manager a Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: eef9842b-495a-46cf-99a6-74e49807e74e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: d176187fe465264b5f433260de5178b48ca9dd4a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="open-ports-and-endpoints-to-a-linux-vm-with-the-azure-cli"></a>Otevřete porty a koncové body pro virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure
Otevření portu nebo vytvořte koncového bodu, virtuální počítač (VM) v Azure vytvořením filtr sítě na podsíť nebo rozhraní sítě virtuálních počítačů. Tyto filtry, které řídí příchozí a odchozí přenosy, můžete umístit na skupinu zabezpečení sítě, který je připojen k prostředku, který přijímá provoz. Použijeme Běžným příkladem webové přenosy na portu 80. Tento článek ukazuje, jak otevřít port pro virtuální počítač s Azure CLI 2.0. K provedení těchto kroků můžete také využít [Azure CLI 1.0](nsg-quickstart-nodejs.md).


## <a name="quick-commands"></a>Rychlé příkazy
Chcete-li vytvořit skupinu zabezpečení sítě a pravidel, je třeba nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).

V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty. Zahrnout názvy parametrů příklad *myResourceGroup*, *myNetworkSecurityGroup*, a *myVnet*.

Vytvořit skupinu zabezpečení sítě s [vytvořit az sítě nsg](/cli/azure/network/nsg#create). Následující příklad vytvoří skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup* v *eastus* umístění:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Přidat pravidlo s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create) povolit provoz protokolu HTTP na svůj webový server (nebo upravit vlastní scénáře, jako je připojení SSH přístupu nebo databáze). Následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRule* umožňující komunikaci TCP na portu 80:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```

Přidružení skupiny zabezpečení sítě Virtuálního počítače síťovým rozhraním (NIC) [aktualizace seskupování sítě az](/cli/azure/network/nic#update). Následující příklad přiřadí stávající síťové karty s názvem *myNic* ke skupině zabezpečení sítě s názvem *myNetworkSecurityGroup*:

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

Alternativně můžete přiřadit skupině zabezpečení sítě podsíť virtuální sítě s [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update) spíše než jenom k síťové rozhraní na jeden virtuální počítač. Následující příklad přiřadí existující podsíť s názvem *mySubnet* v *myVnet* virtuální síť s skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a>Další informace o skupinách zabezpečení sítě
Rychlé příkazy umožňují zprovoznění s provoz do virtuálního počítače. Skupiny zabezpečení sítě zadejte mnoho funkcí a členitosti pro řízení přístupu k prostředkům. Další informace o [vytvoření skupiny zabezpečení sítě a seznamu ACL pravidla zde](tutorial-virtual-network.md#secure-network-traffic).

Pro vysokou dostupnost webové aplikace měli byste umístit virtuální počítače za pro vyrovnávání zatížení Azure. Nástroje pro vyrovnávání zatížení distribuuje provoz do virtuálních počítačů s skupinu zabezpečení sítě, která poskytuje filtrování provozu. Další informace najdete v tématu [jak načíst vyvážit virtuální počítače s Linuxem v Azure k vytvoření vysoce dostupné aplikace](tutorial-load-balancer.md).

## <a name="next-steps"></a>Další kroky
V tomto příkladu jste vytvořili jednoduché pravidlo umožňující přenos HTTP. Můžete najít informace o vytváření podrobnější prostředí v těchto článcích:

* [Přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md)
* [Co je skupina zabezpečení sítě (NSG)?](../../virtual-network/virtual-networks-nsg.md)
