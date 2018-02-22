---
title: "Kurz ke skupinám dostupnosti pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Seznamte se se skupinami dostupnosti pro virtuální počítače s Linuxem v Azure."
documentationcenter: 
services: virtual-machines-linux
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: tutorial
ms.date: 10/05/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 504c4a666d1abd7a495d6759d62815f53f0b54fa
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="how-to-use-availability-sets"></a>Jak používat skupiny dostupnosti


V tomto kurzu zjistíte, jak zvýšit dostupnost a spolehlivost svých řešení využívajících virtuální počítače v Azure pomocí schopnosti označované jako skupiny dostupnosti. Skupiny dostupnosti zajišťují distribuci virtuálních počítačů nasazených v Azure napříč několika izolovanými hardwarovými clustery. To zajišťuje, že pokud dojde k selhání hardwaru nebo softwaru v rámci Azure, ovlivní to pouze podmnožinu vašich virtuálních počítačů a vaše celkové řešení zůstane dostupné a funkční.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření skupiny dostupnosti
> * Vytvoření virtuálního počítače ve skupině dostupnosti
> * Kontrola dostupných velikostí virtuálních počítačů


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a místně používat rozhraní příkazového řádku, musíte mít Azure CLI verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="availability-set-overview"></a>Přehled skupiny dostupnosti

Skupina dostupnosti je schopnost logického seskupení, pomocí které můžete v Azure zajistit, že prostředky virtuálních počítačů, které do ní umístíte, jsou při nasazení v rámci datacentra Azure od sebe navzájem izolované. Azure zajišťuje spouštění virtuálních počítačů, které umístíte do skupiny dostupnosti, napříč několika fyzickými servery, výpočetními stojany, jednotkami úložiště a síťovými přepínači. Pokud dojde k selhání hardwaru nebo softwaru Azure, ovlivní to pouze podmnožinu vašich virtuálních počítačů a vaše celková aplikace zůstane v provozu a bude i nadále dostupná pro zákazníky. Skupiny dostupnosti představují základní schopnost při sestavování spolehlivých cloudových řešení.

Zvažte typické řešení založené na virtuálních počítačích, kdy máte čtyři front-end webové servery a používáte dva back-end virtuální počítače hostující databázi. V případě Azure byste před nasazením virtuálních počítačů měli definovat dvě skupiny dostupnosti: jednu skupinu dostupnosti pro webovou vrstvu a jednu skupinu dostupnosti pro databázovou vrstvu. Při vytváření nového virtuálního počítače pak můžete zadat skupinu dostupnosti jako parametr příkazu az vm create a Azure automaticky zajistí izolaci virtuálních počítačů vytvořených v rámci skupiny dostupnosti napříč více fyzickými hardwarovými prostředky. Pokud dojde k problému s fyzickým hardwarem, na kterém běží virtuální počítače s webovým nebo databázovým serverem, máte jistotu, že ostatní instance virtuálních počítačů s webovým serverem a databází zůstanou spuštěné, protože jsou na jiném hardwaru.

Skupiny dostupnosti použijte v případě, že chcete v Azure nasazovat spolehlivá řešení založená na virtuálních počítačích.


## <a name="create-an-availability-set"></a>Vytvoření skupiny dostupnosti

Skupinu dostupnosti můžete vytvořit pomocí příkazu [az vm availability-set create](/cli/azure/vm/availability-set#az_vm_availability_set_create). V tomto příkladu nastavíme počet aktualizačních domén i domén selhání na *2* pro skupinu dostupnosti *myAvailabilitySet* ve skupině prostředků *myResourceGroupAvailability*.

Vytvořte skupinu prostředků.

```azurecli-interactive 
az group create --name myResourceGroupAvailability --location eastus
```


```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupAvailability \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

Skupiny dostupnosti umožňují izolovat prostředky napříč doménami selhání a aktualizačními doménami. **Doména selhání** představuje izolovanou kolekci prostředků serveru, sítě a úložiště. V předchozím příkladu udáváme, že po nasazení virtuálních počítačů chceme skupinu dostupnosti distribuovat napříč alespoň dvěma doménami selhání. Udáváme také, že chceme skupinu dostupnosti distribuovat napříč dvěma **aktualizačními doménami**.  Dvě aktualizační domény zajistí izolaci prostředků virtuálních počítačů při tom, když Azure provádí aktualizace softwaru, a tím zabrání současné aktualizaci veškerého softwaru běžícího pod virtuálními počítači.


## <a name="create-vms-inside-an-availability-set"></a>Vytvoření virtuálních počítačů ve skupině dostupnosti

Virtuální počítače se musí vytvořit ve skupině dostupnosti, aby se zajistila jejich správná distribuce napříč hardwarem. Existující virtuální počítač po vytvoření není možné přidat do skupiny dostupnosti. 

Při vytváření virtuálního počítače pomocí příkazu [az vm create](/cli/azure/vm#az_vm_create) zadáte skupinu dostupnosti pomocí parametru `--availability-set`, ve kterém zadáte název skupiny dostupnosti.

```azurecli-interactive 
for i in `seq 1 2`; do
   az vm create \
     --resource-group myResourceGroupAvailability \
     --name myVM$i \
     --availability-set myAvailabilitySet \
     --size Standard_DS1_v2  \
     --image Canonical:UbuntuServer:14.04.4-LTS:latest \
     --admin-username azureuser \
     --generate-ssh-keys \
     --no-wait
done 
```

Teď máme v nově vytvořené skupině dostupnosti dva virtuální počítače. Vzhledem k tomu, že jsou ve stejné skupině dostupnosti, Azure zajistí distribuci virtuálních počítačů a všech jejich prostředků (včetně datových disků) napříč izolovaným fyzickým hardwarem. Tato distribuce pomáhá zajistit mnohem vyšší dostupnost celkového řešení využívajícího virtuální počítače.

Pokud se na skupinu dostupnosti podíváte na portálu v části Skupiny dostupnosti > myResourceGroupAvailability > myAvailabilitySet, měli byste vidět, jak se virtuální počítače distribuují napříč dvěma doménami selhání a aktualizačnímu doménami.

![Skupina dostupnosti na portálu](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Kontrola dostupných velikostí virtuálních počítačů 

Později můžete do skupiny dostupnosti přidat další virtuální počítače, potřebujete však vědět, jaké velikosti virtuálních počítačů jsou na konkrétním hardwaru k dispozici.  Pomocí příkazu [az vm availability-set list-sizes](/cli/azure/availability-set#az_availability_set_list_sizes) vypíšete všechny dostupné velikosti na hardwarovém clusteru pro skupinu dostupnosti.

```azurecli-interactive 
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table  
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření skupiny dostupnosti
> * Vytvoření virtuálního počítače ve skupině dostupnosti
> * Kontrola dostupných velikostí virtuálních počítačů

Přejděte k dalšímu kurzu, kde se seznámíte se škálovacími sadami virtuálních počítačů.

> [!div class="nextstepaction"]
> [Vytvoření škálovací sady virtuálních počítačů](tutorial-create-vmss.md)

