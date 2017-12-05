---
title: Obnovit soubory z Azure do Windows serveru | Microsoft Docs
description: "V tomto kurzu podrobnosti obnovení položky z Azure do systému Windows Server."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
keywords: "Zálohování serveru Windows; obnovení souborů systému windows server. zálohování a zotavení po havárii"
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/20/2017
ms.author: saurabhsensharma;markgal;
ms.custom: mvc
ms.openlocfilehash: b5f77ec04ef6d267583a6dc6a4476f118a4d0f74
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="recover-files-from-azure-to-a-windows-server"></a>Obnovit soubory z Azure do systému Windows Server

Zálohování Azure umožňuje obnovení jednotlivých položek ze zálohy systému Windows Server. Obnovení jednotlivých souborů je užitečné, pokud musíte rychlé obnovení náhodně odstraněných souborů. Tento kurz popisuje, jak můžete použít Microsoft Azure Recovery Services agenta (MARS) agenta obnovení položek ze zálohy, které jste již provedli v Azure. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Zahájit obnovení jednotlivých položek 
> * Vyberte bod obnovení 
> * Obnovení položky z bodu obnovení

Tento kurz předpokládá, že jste již provedli postup [zálohování Windows serveru na Azure](backup-configure-vault.md) a mít aspoň jednu zálohu souborů systému Windows Server v Azure.

## <a name="initiate-recovery-of-individual-items"></a>Zahájit obnovení jednotlivých položek

Průvodce užitečné uživatelské rozhraní s názvem Microsoft Azure Backup se instaluje s agenta nástroje Microsoft Azure Recovery Services (MARS). Průvodce Microsoft Azure Backup pracuje s agenta nástroje Microsoft Azure Recovery Services (MARS) k načtení zálohování dat z bodů obnovení uložených v Azure. Použijte průvodce Microsoft Azure Backup k identifikaci soubory nebo složky, které chcete obnovit do systému Windows Server. 

1. Otevřete **Microsoft Azure Backup** modul snap-in. Najdete ho vyhledáním **Microsoft Azure Backup** ve svém počítači.

    ![Zálohování čeká na zpracování](./media/tutorial-backup-restore-files-windows-server/mars.png)

2. V průvodci klikněte na tlačítko **obnovit Data** v **podokna akce** konzole agenta spustit **obnovit Data** průvodce.

    ![Zálohování čeká na zpracování](./media/tutorial-backup-restore-files-windows-server/mars-recover-data.png)

3. Na **Začínáme** vyberte **tento server (název serveru)** a klikněte na tlačítko **Další**.

4. Na **vyberte režimu obnovení** vyberte **jednotlivých souborů a složek** a pak klikněte na **Další** zahájíte proces výběr bodu obnovení.
 
5. Na **vyberte svazek a datum** stránky, vyberte svazek, který obsahuje soubory nebo složky, kterou chcete obnovit a klikněte na **připojit**. Vyberte datum a čas vyberte z rozevírací nabídky, která odpovídá bod obnovení. Data v **tučné** označuje dostupnost alespoň jeden bod obnovení v daný den.

    ![Zálohování čeká na zpracování](./media/tutorial-backup-restore-files-windows-server/mars-select-date.png)
 
    Když kliknete na tlačítko **připojit**, Azure Backup poskytne bodu obnovení k dispozici jako disk. Procházet a obnovit soubory z disku.

## <a name="restore-items-from-a-recovery-point"></a>Obnovení položky z bodu obnovení

1. Po připojení svazku obnovení, klikněte na tlačítko **Procházet** otevřete Průzkumníka Windows a vyhledejte soubory a složky, kterou chcete obnovit. 

    ![Zálohování čeká na zpracování](./media/tutorial-backup-restore-files-windows-server/mars-browse-recover.png)

    Můžete otevřít soubory přímo z obnovení svazku a ověříte soubory.

2. V Průzkumníku Windows zkopírujte soubory nebo složky, kterou chcete obnovit a vložit do libovolného požadovaného umístění na serveru.

    ![Zálohování čeká na zpracování](./media/tutorial-backup-restore-files-windows-server/mars-final.png)

3. Po dokončení obnovení souborů a složek, na **procházení a obnovení souborů** stránky **obnovit Data** průvodce, klikněte na tlačítko **odpojení**. 

    ![Zálohování čeká na zpracování](./media/tutorial-backup-restore-files-windows-server/unmount-and-confirm.png)

4.  Klikněte na tlačítko **Ano** potvrďte, že chcete odpojit svazek.

    Jakmile je snímek nepřipojené, **úloha dokončena** se zobrazí v **úlohy** podokno v konzole agenta.

## <a name="next-steps"></a>Další kroky

Tím dokončíte kurzy o zálohování a obnovení dat Windows serveru do Azure. Další informace o zálohování Azure, najdete v části ukázku v prostředí PowerShell pro zálohování šifrované virtuálních počítačů.

> [!div class="nextstepaction"]
> [Zálohování šifrované virtuálních počítačů](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
