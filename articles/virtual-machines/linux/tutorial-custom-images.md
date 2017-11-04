---
title: "Vytvoření vlastní Image virtuálních počítačů pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Kurz – vytvoření vlastní image virtuálního počítače pomocí rozhraní příkazového řádku Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/21/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 23212bc947dc523561acdf9d1ca784d1bab2c84c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-the-cli"></a>Vytvořit vlastní image virtuálního počítače Azure pomocí rozhraní příkazového řádku

Vlastní Image jsou jako obrázky marketplace, ale můžete vytvořit sami. Vlastní Image slouží k zavedení konfigurace, třeba předběžného načítání aplikace, konfigurace aplikace a další konfigurace operačního systému. V tomto kurzu vytvoříte svoji vlastní image virtuálního počítače v Azure. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Zrušení zřízení a generalize virtuální počítače
> * Vytvoření vlastní image
> * Vytvoření virtuálního počítače z vlastní image
> * Seznam všechny bitové kopie v rámci vašeho předplatného
> * Odstranit bitovou kopii


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Než začnete

Následující postup podrobnosti jak využít existující virtuální počítač a zapnout ho do opakovaně použitelné vlastní obrázek, který můžete použít k vytvoření nové instance virtuálního počítače.

Pokud chcete provést v příkladu v tomto kurzu, musí mít existující virtuální počítač. V případě potřeby to [ukázka skriptu](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) můžete vytvořit za vás. Při absolvování kurzu, nahraďte skupinu prostředků a virtuálních počítačů názvy, kde je potřeba.

## <a name="create-a-custom-image"></a>Vytvoření vlastní image

Chcete-li vytvořit bitovou kopii virtuálního počítače, je nutné připravit virtuální počítač zrušení zřízení, rušení přidělení a pak označením zdrojového virtuálního počítače jako zobecněn. Jakmile je virtuální počítač připravený, můžete vytvořit bitovou kopii.

### <a name="deprovision-the-vm"></a>Zrušení zřízení virtuálního počítače 

Zrušení zřízení umožňuje zobecnit virtuální počítač odebráním informace specifické pro počítač. Tato generalizace umožňuje nasadit mnoho virtuálních počítačů z jedné bitové kopie. Během zrušení zřízení, název hostitele je obnovena na *localhost.localdomain*. Klíče hostitele SSH, konfigurace názvový server, kořenové heslo a uložené v mezipaměti zapůjčení DHCP se také odstraní.

Chcete-li zrušit jejich zřízení virtuálního počítače, použijte agenta virtuálního počítače Azure (příkaz waagent). Agent virtuálního počítače Azure je nainstalovaný na virtuálním počítači a spravuje zřizování a interakci s Kontroleru prostředků infrastruktury Azure. Další informace najdete v tématu [Azure Linux Agent uživatelská příručka](agent-user-guide.md).

Připojení k virtuálnímu počítači pomocí protokolu SSH a spusťte příkaz pro zrušení zřízení virtuálního počítače. Pomocí `+user` argument, poslední účet zřízení uživatele a všechna přidružená data budou také odstraněny. Nahraďte IP adresu příklad veřejnou IP adresu vašeho virtuálního počítače.

SSH k virtuálnímu počítači.
```bash
ssh azureuser@52.174.34.95
```
Zrušení zřízení virtuálního počítače.

```bash
sudo waagent -deprovision+user -force
```
Ukončení relace SSH.

```bash
exit
```

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>Deallocate a označit virtuální počítač jako zobecněn

Pokud chcete vytvořit bitovou kopii, virtuální počítač musí být navrácena. Zrušit přidělení virtuálního počítače pomocí [az OM deallocate](/cli//azure/vm#deallocate). 
   
```azurecli-interactive 
az vm deallocate --resource-group myResourceGroup --name myVM
```

Nakonec nastavit stav virtuálního počítače jako zobecněn s [az virtuálních počítačů zobecní](/cli//azure/vm#generalize) tak platformy Azure zná zobecněný virtuální počítač. Bitovou kopii lze vytvořit pouze z zobecněný virtuální počítač.
   
```azurecli-interactive 
az vm generalize --resource-group myResourceGroup --name myVM
```

### <a name="create-the-image"></a>Vytvoření bitové kopie

Nyní můžete vytvořit image virtuálního počítače pomocí [vytvoření bitové kopie az](/cli//azure/image#create). Následující příklad vytvoří bitovou kopii s názvem *myImage* z virtuálního počítače s názvem *Můjvp*.
   
```azurecli-interactive 
az image create \
    --resource-group myResourceGroup \
    --name myImage \
    --source myVM
```
 
## <a name="create-vms-from-the-image"></a>Vytvořit virtuální počítače z image

Teď, když máte image, můžete vytvořit jeden nebo více nové virtuální počítače z image pomocí [vytvořit virtuální počítač az](/cli/azure/vm#create). Následující příklad vytvoří virtuální počítač s názvem *myVMfromImage* z bitové kopie s názvem *myImage*.

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVMfromImage \
    --image myImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

## <a name="image-management"></a>Správy bitových kopií 

Tady jsou některé příklady běžné úlohy správy bitové kopie a postup dokončete pomocí rozhraní příkazového řádku Azure.

Zobrazí seznam všech Image podle názvu ve formátu tabulky.

```azurecli-interactive 
az image list \
  --resource-group myResourceGroup
```

Odstraňte bitovou kopii. Tento příklad odstraní image s názvem *myOldImage* z *myResourceGroup*.

```azurecli-interactive 
az image delete \
    --name myOldImage \
    --resource-group myResourceGroup
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili vlastní image virtuálního počítače. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Zrušení zřízení a generalize virtuální počítače
> * Vytvoření vlastní image
> * Vytvoření virtuálního počítače z vlastní image
> * Seznam všechny bitové kopie v rámci vašeho předplatného
> * Odstranit bitovou kopii

Přechodu na v dalším kurzu se dozvíte o vysoce dostupných virtuálních počítačů.

> [!div class="nextstepaction"]
> [Vytvořit vysoce dostupné virtuální počítače](tutorial-availability-sets.md).

