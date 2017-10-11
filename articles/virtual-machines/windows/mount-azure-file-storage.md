---
title: "Připojení Azure úložiště file z virtuálního počítače Windows Azure | Microsoft Docs"
description: "Uložte soubor v cloudu s Azure file storage a připojte svou cloudovou sdílenou z Azure virtuálního počítače (VM)."
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: 6ffb2d2da1e2439df6f5da543411e3c2c68d3435
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-file-shares-with-windows-vms"></a>Použít sdílené složky Azure s virtuálními počítači Windows 

Sdílené složky Azure můžete použít jako způsob, jak ukládat a přistupovat k souborům z virtuálního počítače. Můžete například uložit skript nebo konfiguračního souboru aplikace, které chcete všechny virtuální počítače do sdílené složky. V tomto tématu jsme ukážeme, jak vytvořit a připojit sdílenou složku Azure a jak pro nahrávání a stahování souborů.

## <a name="connect-to-a-file-share-from-a-vm"></a>Připojení ke sdílené složce z virtuálního počítače

V této části se předpokládá, že už máte sdílenou složku, kterou chcete připojit k. Pokud potřebujete vytvořit, přečtěte si téma [vytvoření sdílené složky](#create-a-file-share) dál v tomto tématu.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce vlevo klikněte na tlačítko **účty úložiště**.
3. Vyberte svůj účet úložiště
4. V **přehled** v části **služby**, vyberte **soubory**.
5. Vyberte sdílenou složku.
6. Klikněte na tlačítko **Connect** otevřete stránku, která ukazuje syntaxi příkazového řádku pro připojení sdílené složky z Windows nebo Linux.
7. Zvýraznění syntaxe příkazu a vložte jej do programu Poznámkový blok nebo jiném místě kde můžete snadno k němu přístup. 
8. Upravit syntaxe odebrat úvodního ** > ** a nahraďte *[písmeno jednotky]* písmeno jednotky (například **/Y:**) kde byste chtěli připojit sdílenou složku.
8. Připojte k virtuálnímu počítači a otevřete příkazový řádek.
9. Vložte v syntaxi upravená připojení a stiskněte tlačítko **Enter**.
10. Po vytvoření připojení, se zobrazí zpráva **příkaz úspěšně dokončil.**
11. Zkontrolujte připojení a to zadáním písmeno jednotky přepnout na tuto jednotku a pak zadejte **dir** zobrazí se obsah sdílené složky.



## <a name="create-a-file-share"></a>Vytvoření sdílené složky 
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce vlevo klikněte na tlačítko **účty úložiště**.
3. Vyberte svůj účet úložiště
4. V **přehled** v části **služby**, vyberte **soubory**.
5. Na stránce služby souborů, klikněte na tlačítko **+ sdílená** vytvořit svou první sdílenou. \
6. Zadejte název sdílené složky souborů. Názvy sdílených složek můžete použít, malá písmena, číslice a jeden pomlčky. Název nemůže začínat pomlčkou a nemůže používat více po sobě jdoucí pomlčky. 
7. Vyplňte omezení na tom, jak velký soubor může být, až 5120 GB.
8. Klikněte na tlačítko **OK** nasazení sdílené složky.
   
## <a name="upload-files"></a>Nahrání souborů
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce vlevo klikněte na tlačítko **účty úložiště**.
3. Vyberte svůj účet úložiště
4. V **přehled** v části **služby**, vyberte **soubory**.
5. Vyberte sdílenou složku.
6. Klikněte na tlačítko **nahrát** otevřete **nahrání souborů** stránky.
7. Klikněte na ikonu složky a přejděte do místního systému souborů pro soubor k odeslání.   
8. Klikněte na tlačítko **nahrát** nahrát soubor do sdílené složky.

## <a name="download-files"></a>Stažení souborů
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce vlevo klikněte na tlačítko **účty úložiště**.
3. Vyberte svůj účet úložiště
4. V **přehled** v části **služby**, vyberte **soubory**.
5. Vyberte sdílenou složku.
6. Klikněte pravým tlačítkem na soubor a vyberte **Stáhnout** se stáhne do místního počítače.
   

## <a name="next-steps"></a>Další kroky

Můžete také vytvořit a spravovat sdílené složky pomocí prostředí PowerShell. Další informace najdete v tématu [Začínáme s Azure File storage ve Windows](../../storage/files/storage-dotnet-how-to-use-files.md).
