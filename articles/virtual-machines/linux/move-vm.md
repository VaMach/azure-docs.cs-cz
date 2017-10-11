---
title: "Přesunout virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Přesuňte virtuální počítač s Linuxem do jiné předplatné nebo prostředek skupiny Azure v modelu nasazení Resource Manager."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d635f0a5-4458-4b95-a5f8-eed4f41eb4d4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 4695a9c934f97f2b2d448c4990e7ad5533e38e9f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Přesunout virtuální počítač s Linuxem do jiné skupiny pro předplatné nebo prostředek
Tento článek vás provede postup přesunutí virtuálního počítače s Linuxem mezi skupinami prostředků nebo předplatných. Přesunutí virtuálního počítače mezi předplatnými může být užitečné, když vytvoříte virtuální počítač v odběru osobní a chcete ho přesunout do předplatného ve vaší společnosti.

> [!IMPORTANT]
>V tuto chvíli nelze přesunout spravované disky. 
>
>Nové ID prostředků jsou vytvořené jako součást přesunutí. Po přesunutí virtuálního počítače je potřeba aktualizovat nástroje a skripty, které pomocí nového ID prostředku. 
> 
> 

## <a name="use-the-azure-cli-to-move-a-vm"></a>Použijte rozhraní příkazového řádku Azure k přesunutí virtuálního počítače
Chcete-li úspěšně přesunout virtuální počítač, musíte přesunout virtuální počítač a všechny její Podpůrné prostředky. Použití **zobrazit skupiny azure** seznam všechny prostředky v skupinu prostředků a jejich ID příkazu. Pomáhá výstup tohoto příkazu do souboru, můžete zkopírovat a vložit ID do novější příkazy.

    azure group show <resourceGroupName>

Chcete-li přesunout virtuální počítač a jeho prostředků do jiné skupině prostředků, použijte **přesunutí prostředku azure** rozhraní příkazového řádku příkaz. Následující příklad ukazuje, jak přesunout virtuální počítač a nejběžnější prostředky, které vyžaduje. Používáme **-i** parametr a předejte jí seznam oddělený čárkami (bez mezer) ID pro prostředky. Chcete-li přesunout.

    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      

    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"

Pokud chcete přesunout virtuální počítač a jeho prostředků do jiného předplatného, přidejte **– ID cílového předplatného & č. 60; destinationSubscriptionID & č. 62;** parametr k určení cílového odběru.

Pokud pracujete z příkazového řádku na počítači se systémem Windows, je nutné přidat  **$**  před názvy proměnných, když je deklarovat. Toto není nutné v systému Linux.

Zobrazí se výzva k potvrzení, že chcete přesunout zadaný prostředek. Typ **Y** potvrďte, že chcete přesunout prostředky.

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Další kroky
Mnoho různých typů prostředků můžete přesouvat mezi skupinami prostředků a předplatná. Další informace najdete v tématu, které se zabývá [přesunutím prostředků do nové skupiny prostředků nebo předplatného](../../resource-group-move-resources.md).    

