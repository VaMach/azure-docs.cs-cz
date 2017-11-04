---
title: "Připojit spravované datový disk k virtuálnímu počítači Windows - Azure | Microsoft Docs"
description: "Jak připojit nový spravovaný datový disk pro virtuální počítač s Windows v portálu Azure pomocí modelu nasazení Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: cynthn
ms.openlocfilehash: f0cf88a06c5470ef173b22e7213419a6c8760723
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-attach-a-managed-data-disk-to-a-windows-vm-in-the-azure-portal"></a>Tom, jak připojit spravované datový disk k virtuálnímu počítači Windows na portálu Azure

Tento článek ukazuje, jak připojit nový spravovaný datový disk pro virtuální počítače s Windows pomocí portálu Azure. Než to uděláte, projděte si tyto tipy:

* Velikost virtuálního počítače určuje, kolik datových disků můžete připojit. Podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md).
* Pro nový disk nemusíte nejdřív ji vytvořit, protože Azure ji vytvoří, když jeho připojení.

Můžete také [připojit datový disk pomocí prostředí Powershell](attach-disk-ps.md).



## <a name="add-a-data-disk"></a>Přidat datový disk
1. V nabídce na levé straně klikněte na tlačítko **virtuální počítače**.
2. Ze seznamu vyberte virtuální počítač.
3. V okně virtuálního počítače klikněte na **disky**.
   4. Na **disky** okně klikněte na tlačítko **+ přidat datový disk**.
5. V rozevírací nabídky pro nový disk, vyberte **vytvořit prázdný**.
6. V **spravovaných disků na vytvořit** okno, zadejte název pro disk a upravte nastavení podle potřeby. Až budete hotovi, klikněte na tlačítko **vytvořit**.
7. V **disky** okně klikněte na tlačítko Uložit uložte novou konfiguraci disku pro virtuální počítač.
6. Jakmile Azure vytvoří disk a připojí jej k virtuálnímu počítači, na nový disk, je uvedena ve nastavení disku virtuálního počítače v části **datových disků**.


## <a name="initialize-a-new-data-disk"></a>Inicializace nový datový disk

1. Připojte k virtuálnímu počítači.
1. Klikněte na nabídku start uvnitř virtuálního počítače a typ **diskmgmt.msc** a počtu **Enter**. Tím se spustí modul snap-in Správa disků.
2. Správa disků rozpozná, že máte nový, zrušení inicializovaného disku a bude překryvné okno inicializovat Disk.
3. Zkontrolujte, že je vybraný nový disk a klikněte na tlačítko **OK** k chybě při inicializaci ho.
4. Nový disk se nyní zobrazí jako **nepřidělené**. Klepněte pravým tlačítkem myši na disk a vyberte **nový jednoduchý svazek**. **Průvodci vytvořením jednoduchého svazku** spustí.
5. Postupujte dle pokynů průvodce, udržuje všechny výchozí hodnoty, po dokončení vyberte **Dokončit**.
6. Zavřete Správa disků.
7. Zobrazí se automaticky otevírané okno, které potřebujete k formátování nový disk, abyste mohli používat. Klikněte na tlačítko **formát disku**.
8. V **nový disk formát** dialogové okno, zkontrolujte nastavení a pak klikněte na tlačítko **spustit**.
9. Zobrazí se upozornění, které se formátování disky bude vymazat všechna data, klikněte na tlačítko **OK**.
10. Po dokončení Formát klikněte na tlačítko **OK**.

## <a name="use-trim-with-standard-storage"></a>Použít standardní úložiště uvolnění dočasné paměti

Pokud chcete použít standardní úložiště (HDD), měli byste povolit uvolnění dočasné paměti. TRIM zahodí nepoužívané bloky na disku, můžete se účtují pouze pro úložiště, které skutečně používáte. To můžete uložit na náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je. 

Můžete spustit tento příkaz a zkontrolujte nastavení uvolnění dočasné paměti. Otevřete příkazový řádek na virtuální počítač s Windows a zadejte:

```
fsutil behavior query DisableDeleteNotify
```

Pokud příkaz vrátí hodnotu 0, TRIM správně povolené. Pokud vrátí hodnotu 1, spusťte následující příkaz k povolení uvolnění dočasné paměti:
```
fsutil behavior set DisableDeleteNotify 0
```

Po odstranění dat z disku, můžete zajistit TRIM operace vyprázdnění správně spuštěním defragmentační s uvolnění dočasné paměti:

```
defrag.exe <volume:> -l
```

Můžete také zajistit, že celý svazek je oříznut podle formátování svazku.

## <a name="next-steps"></a>Další kroky
Pokud jste aplikaci potřebuje používat D: disk k uložení dat, můžete [změnit písmeno jednotky dočasné disk systému Windows](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
