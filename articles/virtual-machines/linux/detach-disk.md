---
title: "Odpojit datový disk z virtuálního počítače s Linuxem – Azure | Microsoft Docs"
description: "Naučte se odpojit datový disk z virtuálního počítače v Azure pomocí rozhraní příkazového řádku 2.0 nebo portálu Azure."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.openlocfilehash: de0222d897ed2cf94be98501c39385ac88f866fc
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/02/2017
---
# <a name="how-to-detach-a-data-disk-from-a-linux-virtual-machine"></a>Postup odpojit datový disk z virtuálního počítače systému Linux

Když už nepotřebujete datový disk připojený k virtuálnímu počítači, můžete ho jednoduše odpojit. Odebere disk z virtuálního počítače, ale neodstraní z úložiště. 

> [!WARNING]
> Pokud se odpojit disk není automaticky odstraněn. Pokud jste přihlášení k odběru služby storage úrovně Premium, můžete nadále toho vám být účtovány poplatky za úložiště pro disk. Další informace najdete v části [ceny a fakturace při použití služby Premium Storage](../windows/premium-storage.md#pricing-and-billing). 
> 
> 

Pokud znovu chcete použít stávající data na disku, můžete ho znovu připojit ke stejnému nebo jinému virtuálnímu počítači.  

## <a name="detach-a-data-disk-using-cli-20"></a>Odpojit datový disk pomocí rozhraní příkazového řádku 2.0

```azurecli
az vm disk detach -g myResourceGroup --vm-name myVm -n myDataDisk
```

Disk zůstává v úložišti, ale už není připojený k virtuálnímu počítači.


## <a name="detach-a-data-disk-using-the-portal"></a>Odpojení datového disku pomocí portálu
1. V centru portálu vyberte **virtuální počítače**.
2. Vyberte virtuální počítač, který má datový disk, kterou chcete odpojit a klikněte na tlačítko **Zastavit** se zrušit přidělení virtuálního počítače.
3. V okně virtuálního počítače vyberte **disky**.
4. V horní části **disky** vyberte **upravit**.
5. V **disky** okno na pravé straně datový disk, který chcete odpojit, klikněte na tlačítko ![obrázek tlačítka odpojení](./media/detach-disk/detach.png) odpojit tlačítko.
5. Po odebrání disku nahoře v okně klikněte na tlačítko Uložit.
6. V okně virtuálního počítače klikněte na **přehled** a klikněte **spustit** tlačítka v horní části okna restartujte virtuální počítač.

Disk zůstává v úložišti, ale už není připojený k virtuálnímu počítači.








## <a name="next-steps"></a>Další kroky
Pokud chcete použít datový disk, jste právě [připojte ji k jiným virtuálním Počítačem](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

