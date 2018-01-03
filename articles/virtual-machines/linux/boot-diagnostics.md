---
title: "Spuštění diagnostiky pro virtuální počítače s Linuxem v Azure | Microsoft dokumentů"
description: "Přehled ladění dvou funkcí pro virtuální počítače s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: Deland-Han
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: delhan
ms.openlocfilehash: 70254d39b5c6326166f7e29fdfc99533835502f9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-linux-virtual-machines-in-azure"></a>Jak používat Diagnostika spouštění k řešení potíží s virtuální počítače s Linuxem v Azure

V Azure je teď dostupná podpora dvou funkcí ladění: Podpora výstupu konzoly a snímku obrazovky pro model nasazení virtuálních počítačů Azure Resource Manager. 

Při přenosu vlastní image do Azure nebo spouštění některé z imagí platformy může virtuální počítač přejít do nespustitelného stavu z mnoha důvodů. Tyto funkce umožňují snadnou diagnostiku a obnovení virtuálních počítačů v případě chyb při spuštění.

U virtuálních počítačů s Linuxem můžete snadno zobrazit výstup protokolu konzoly z portálu:

![portál Azure](./media/boot-diagnostics/screenshot1.png)
 
U virtuálních počítačů s Windows i Linuxem však Azure umožňuje zobrazit také snímek obrazovky virtuálního počítače z hypervisoru:

![Chyba](./media/boot-diagnostics/screenshot2.png)

Obě tyto funkce jsou podporované pro virtuální počítače Azure ve všech oblastech. Mějte na paměti, že může trvat až 10 minut, než se snímky obrazovky a výstup zobrazí v účtu úložiště.

## <a name="common-boot-errors"></a>Běžné chyby spuštění

- [Problémy systému souborů](https://blogs.msdn.microsoft.com/linuxonazure/2016/09/13/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck-inodes/)
- [Problémy jádra](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-manually-fixing-non-boot-issues-related-to-kernel-problems/)
- [FSTAB chyby](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/ )

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Povolení diagnostiky na novém virtuálním počítači
1. Při vytváření nového virtuálního počítače na portálu Preview vyberte z rozevíracího seznamu modelu nasazení **Azure Resource Manager**:
 
    ![Resource Manager](./media/boot-diagnostics/screenshot3.jpg)

2. Nakonfigurujte možnost Monitorování pro výběr účtu úložiště, do kterého chcete tyto diagnostické soubory ukládat.
 
    ![Vytvoření virtuálního počítače](./media/boot-diagnostics/screenshot4.jpg)

3. Pokud provádíte nasazení z šablony Azure Resource Manageru, přejděte do prostředku vašeho virtuálního počítače a připojte část s diagnostickým profilem. Nezapomeňte použít hlavičku verze rozhraní API s hodnotou 2015-06-15.

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. Diagnostický profil umožňuje vybrat účet úložiště pro ukládání protokolů.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

## <a name="update-an-existing-virtual-machine"></a>Aktualizace stávajícího virtuálního počítače

Pokud chcete povolit Diagnostika spouštění prostřednictvím portálu, můžete také aktualizovat existujícího virtuálního počítače přes portál. Vyberte možnost Diagnostika spuštění a klikněte na Uložit. Restartujte virtuální počítač, aby se projevily změny.

![Aktualizace stávajícího virtuálního počítače](./media/boot-diagnostics/screenshot5.png)