---
title: "Správa služby Soubory Azure z webu Azure Portal | Dokumentace Microsoftu"
description: "Zjistěte, jak pomocí webu Azure Portal spravovat službu Soubory Azure."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/19/2017
ms.author: renash
ms.openlocfilehash: e56f8bf1057a8bc2cfcde841f69022104bafff27
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-azure-files-from-the-azure-portal"></a>Použití služby Soubory Azure z webu Azure Portal
[Azure Portal](https://portal.azure.com) nabízí uživatelské rozhraní pro správu služby Soubory Azure. Z webového prohlížeče můžete provádět následující akce:

* Vytvoření sdílené složky
* Ukládat soubory do sdílené složky a stahovat soubory ze sdílené složky.
* Monitorovat skutečné využití každé sdílené složky.
* Upravit kvótu velikosti sdílené složky.
* Kopírovat příkazy pro připojení a použít je k připojení sdílené složky z klienta pro Windows nebo Linux.

## <a name="create-file-share"></a>Vytvoření sdílené složky
1. Přihlaste se k portálu Azure.
2. V navigační nabídce klikněte na **Účty úložiště** nebo **Účty úložiště (klasické)**.
    
    ![Snímek obrazovky, který ukazuje vytvoření sdílené složky v portálu.](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share1.png)

3. Vyberte svůj účet úložiště

    ![Snímek obrazovky, který ukazuje vytvoření sdílené složky v portálu.](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share2.png)

4. Vyberte službu „Files“.

    ![Snímek obrazovky, který ukazuje vytvoření sdílené složky v portálu.](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share3.png)

5. Klikněte na „Sdílené složky“ a pomocí odkazu vytvořte svou první sdílenou složku.

    ![Snímek obrazovky, který ukazuje vytvoření sdílené složky v portálu.](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share4.png)

6. Zadejte název sdílené složky a její velikost (max. 5120 GB) a vaše první složka se vytvoří. Po vytvoření sdílené složky ji můžete připojit z jakéhokoli souborové systému, který podporuje SMB 2.1 nebo SMB 3.0. Po kliknutí na **Kvóta** sdílené složky můžete změnit velikost souboru až do velikosti 5 120 GB. Pro odhad nákladů na úložiště při použití služby Soubory Azure použijte [cenovou kalkulačku funkcí Azure](https://azure.microsoft.com/pricing/calculator/).

    ![Snímek obrazovky, který ukazuje vytvoření sdílené složky v portálu.](./media/storage-how-to-use-files-portal/use-files-portal-create-file-share5.png)

## <a name="upload-and-download-files"></a>Ukládání a stahování souborů
1. Vyberte jednu sdílenou složku, kterou jste už vytvořili.

    ![Snímek obrazovky, který ukazuje ukládání a stahování souborů z portálu.](./media/storage-how-to-use-files-portal/use-files-portal-upload-file1.png)

2. Klikněte na **Uložit** a otevře se uživatelské prostředí pro ukládání souborů.

    ![Snímek obrazovky, který ukazuje ukládání souborů z portálu.](./media/storage-how-to-use-files-portal/use-files-portal-upload-file2.png)

## <a name="connect-to-file-share"></a>Připojení ke sdílené složce
-  Klikněte na **Připojit** a zobrazí se příkazový řádek pro připojení sdílené složky z Windows a Linuxu. Pokud jste uživatelem Linuxu, další informace o připojení pro jiné distribuce Linuxu najdete v tématu [Jak používat Soubory Azure s Linuxem](../storage-how-to-use-files-linux.md).

    ![Snímek obrazovky, který ukazuje připojení sdílené složky.](./media/storage-how-to-use-files-portal/use-files-portal-connect.png)
-  Můžete zkopírovat příkazy pro připojení sdílené složky ve Windows nebo Linuxu a spustit je z virtuálního počítače Azure nebo místního počítače.

    ![Snímek obrazovky, který ukazuje příkazy pro připojení pro Windows a Linux](./media/storage-how-to-use-files-portal/use-files-portal-show-mount-commands.png)

**Tip:**  
Přístupový klíč účtu úložiště pro připojení najdete tak, že kliknete na **Zobrazit přístupové klíče pro tento účet úložiště** ve spodní části stránky připojení.

## <a name="see-also"></a>Viz také
Další informace o službě Soubory Azure najdete na těchto odkazech.

* [Nejčastější dotazy](../storage-files-faq.md)
* [Řešení potíží ve Windows](storage-troubleshoot-windows-file-connection-problems.md)      
* [Řešení potíží v Linuxu](storage-troubleshoot-linux-file-connection-problems.md)    
