---
title: "O disky a virtuální pevné disky pro virtuální počítače Microsoft Azure Linux | Microsoft Docs"
description: "Seznamte se se základy disky a virtuální pevné disky pro virtuální počítače s Linuxem v Azure."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 7be8dd52-98f7-4187-9b78-55197915bc9b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.openlocfilehash: be5f09af275142590ec6ade02562e914d5726e08
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="about-disks-and-vhds-for-azure-linux-vms"></a>O disky a virtuální pevné disky pro virtuální počítače Azure s Linuxem
Stejně jako všechny ostatní počítače virtuálních počítačů v Azure používat disky jako místo pro uložení operačního systému, aplikace a data. Všechny virtuální počítače Azure mít aspoň dva disky – disk operačního systému Linux a dočasný disk. Vytváření disku operačního systému z bitové kopie a disku operačního systému a image jsou ve skutečnosti virtuální pevné disky (VHD) uložené v účtu úložiště Azure. Virtuální počítače také může mít jeden nebo více datových disků, které jsou také uloženy jako virtuální pevné disky. 

V tomto článku jsme se v souvislosti se jiný používá pro disky a potom popisují různé typy disků můžete vytvořit a použít. Tento článek je také k dispozici pro [virtuální počítače s Windows](../windows/about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Disky, které jsou používány virtuálními počítači

Podívejme se na tom, jak jsou disky používány virtuálních počítačů.

## <a name="operating-system-disk"></a>Disk operačním systému
Každý virtuální počítač má jeden disk připojené operačního systému. Je zaregistrován jako jednotka SATA a je označeno/dev/sda ve výchozím nastavení. Tento disk má maximální kapacita 2 048 gigabajtů (GB). 

## <a name="temporary-disk"></a>Dočasné disku
Každý virtuální počítač obsahuje dočasné disk. Dočasné disku poskytuje krátkodobé úložiště pro aplikace a procesy a slouží k uložení pouze data, jako jsou soubory stránky nebo odkládacího souboru. Data na dočasné disku mohou být ztraceny při [údržby](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) nebo když jste [znovu nasadit virtuální počítač](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Během restartu standardní virtuální počítač by měl uchovat data na dočasné jednotce.

Na virtuální počítače s Linuxem, disk je obvykle **/dev/sdb** a je naformátován a připojit k **/mnt** Azure Linux Agent. Velikost dočasné disku se liší, na základě velikosti virtuálního počítače. Další informace najdete v tématu [velikosti virtuálních počítačů Linux](../windows/sizes.md).

Další informace o tom, jak Azure používá dočasným diskovým najdete v tématu [pochopení dočasné jednotce ve virtuálních počítačích Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Datový disk
Datový disk je virtuální pevný disk, který je připojen k virtuálnímu počítači na ukládání dat aplikací, nebo data, která je třeba zachovat. Datové disky jsou zaregistrované jako disky SCSI a jsou označeny písmenem, který zvolíte. Každý datový disk má maximální kapacitu 4095 GB. Velikost virtuálního počítače určuje, kolik datových disků můžete připojit k jeho a typ úložiště můžete použít k hostování disky.

> [!NOTE]
> Další podrobnosti o kapacity virtuálních počítačů najdete v tématu [velikosti virtuálních počítačů Linux](../windows/sizes.md).
> 

Azure vytvoří disk operačního systému, když vytvoříte virtuální počítač z bitové kopie. Pokud používáte image, která obsahuje datové disky, Azure vytvoří datových disků také při vytváření virtuálního počítače. V opačném datových disků přidat po vytvoření virtuálního počítače.

Můžete přidat datových disků pro virtuální počítač v každém okamžiku nástrojem **připojení** disk k virtuálnímu počítači. Můžete vytvořit virtuální pevný disk, který jste nahráli nebo můžete zkopírovat do svého účtu úložiště nebo ten, který pro vás vytvoří Azure. Připojením datového disku přiřadí soubor virtuálního pevného disku s virtuálním Počítačem, umístěním zapůjčení na virtuální pevný disk, nelze jej odstranit z úložiště při je stále připojen.

[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="troubleshooting"></a>Řešení potíží
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Další kroky
* [Připojit disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) přidat další úložiště pro virtuální počítač.
* [Konfigurace softwaru diskového pole RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) redundanci.
* [Zachytit virtuální počítač s Linuxem](./classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) , můžete rychle nasadit další virtuální počítače.

