---
title: "Zálohování Windows serveru do Azure | Microsoft Docs"
description: "Tento kurz údaje zálohování na místní servery Windows do trezoru služeb zotavení."
services: back up
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
keywords: "Zálohování serveru Windows; Zálohování serveru windows. zálohování a zotavení po havárii"
ms.assetid: 
ms.service: back up
ms.workload: storage-back up-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2017
ms.author: saurabhsensharma;markgal;
ms.custom: 
ms.openlocfilehash: 7caf1dd3fa5ef295c2472cc11deb2895fc2a7111
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/09/2017
---
# <a name="back-up-windows-server-to-azure"></a>Zálohování Windows Serveru do Azure


Azure Backup můžete použít k ochraně systému Windows Server z poškození, útoky a havárií. Azure Backup poskytuje odlehčený nástroj říká agenta nástroje Microsoft Azure Recovery Services (MARS). MARS agent je nainstalován na serveru systému Windows umožňuje chránit soubory a složky a informace o konfiguraci serveru prostřednictvím stav systému Windows Server. Tento kurz vysvětluje, jak můžete pomocí agenta MARS zálohování Windows serveru do Azure. V tomto kurzu se naučíte: 


> [!div class="checklist"]
> * Stáhněte si a nastavte agenta MARS
> * Konfigurace zálohování časy a plán uchovávání informací pro zálohy vašeho serveru
> * Proveďte zálohování ad-hoc


## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k webu Azure Portal na adrese http://portal.azure.com.

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

Předtím, než můžete zálohovat systém Windows Server, musíte vytvořit místo pro zálohování, nebo obnovit body k uložení. A [trezor služeb zotavení](backup-azure-recovery-services-vault-overview.md) je kontejner v Azure, která ukládá zálohy ze systému Windows Server. Použijte následující postup k vytvoření trezoru služeb zotavení na portálu Azure. 

1. V levé nabídce vyberte **další služby** a v seznamu služeb zadejte **služeb zotavení**. Klikněte na tlačítko **trezory služeb zotavení**.

   ![Otevřený trezor služeb zotavení](./media/tutorial-backup-windows-server-to-azure/full-browser-open-rs-vault.png)

2.  V nabídce **Trezory Recovery Services** klikněte na **Přidat**.

   ![Zadejte informace o trezoru](./media/tutorial-backup-windows-server-to-azure/provide-vault-detail-2.png)

3.  V **trezor služeb zotavení** nabídce

    - Typ *myRecoveryServicesVault* v **název**.
    - ID se zobrazí v aktuálním předplatném **předplatné**.
    - Pro **skupiny prostředků**, vyberte **použít existující** a zvolte *myResourceGroup*. Pokud *myResourceGroup* neexistuje, vyberte **vytvořit nový** a typ *myResourceGroup*. 
    - Z **umístění** rozevírací nabídky vyberte *západní Evropa*.
    - Klikněte na tlačítko **vytvořit** pro vytvoření trezoru služeb zotavení.
 
Když je trezor vytvořený, zobrazí se v seznamu trezorů Služeb zotavení.

## <a name="download-recovery-services-agent"></a>Stáhnout agenta služeb zotavení

Agenta nástroje Microsoft Azure Recovery Services (MARS) vytvoří přidružení mezi Windows serverem a trezoru služeb zotavení. Následující postup vysvětluje, jak stáhnout agenta k serveru.

1.  V seznamu trezorů služeb zotavení vyberte **myRecoveryServicesVault** otevřete svůj řídicí panel.

   ![Zadejte informace o trezoru](./media/tutorial-backup-windows-server-to-azure/open-vault-from-list.png)

2.  V nabídce řídícího panelu trezoru, klikněte na tlačítko **zálohování**.

3.  Na **cíl zálohování** nabídky:

    - pro **kde běží vaše úloha?**, vyberte**místní**, 
    - pro **co chcete zálohovat?**, vyberte **soubory a složky** a **stavu systému** 

    ![Zadejte informace o trezoru](./media/tutorial-backup-windows-server-to-azure/backup-goal.png)
    
4.  Klikněte na tlačítko **Příprava infrastruktury** otevřete **připravit infrastrukturu** nabídky.
5.  Na **připravit infrastrukturu** nabídky, klikněte na tlačítko **stáhnout agenta pro Windows Server nebo klienta Windows** ke stažení *MARSAgentInstaller.exe*. 

    ![Příprava infrastruktury](./media/tutorial-backup-windows-server-to-azure/prepare-infrastructure.png)

    Otevře prohlížeč samostatný instalační program a stáhne **MARSAgentInstaller.exe**.
 
6.  Před spuštěním staženého souboru, klikněte **Stáhnout** tlačítka v okně Příprava infrastruktury a stáhněte a uložte **přihlašovací údaje trezoru** souboru. Tento soubor je vyžadován pro připojení agenta MARS se trezor služeb zotavení.

    ![Příprava infrastruktury](./media/tutorial-backup-windows-server-to-azure/download-vault-credentials.png)
 
## <a name="install-and-register-the-agent"></a>Instalace a registrace agenta

1. Vyhledejte a poklikejte stažený **MARSagentinstaller.exe**.
2. **Průvodce instalací agenta služeb zotavení Microsoft Azure** se zobrazí. Při procházení v průvodci zadejte tyto informace při zobrazení výzvy a klikněte na **zaregistrovat**.
    - Umístění složky pro instalaci a mezipaměti.
    - Informace serveru proxy, pokud používáte proxy server pro připojení k Internetu.
    - Vaše uživatelské jméno a heslo údaje Pokud používáte ověřený server proxy.

    ![Příprava infrastruktury](./media/tutorial-backup-windows-server-to-azure/mars-installer.png) 

3. Na konci tohoto průvodce, klikněte na tlačítko **přejít k registraci** a poskytují **přihlašovací údaje trezoru** soubor stažený v předchozím postupu.
 
4. Po zobrazení výzvy zadejte šifrovací přístupové heslo k šifrování záloh ze systému Windows Server. Uložte heslo v zabezpečeném umístění, protože Microsoft nelze obnovit heslo, pokud dojde ke ztrátě.

5. Klikněte na **Dokončit**. 

## <a name="configure-backup-and-retention"></a>Konfigurace zálohování a uchovávání

Agent služeb zotavení Microsoft Azure slouží k plánování, když dojde k zálohování do Azure, v systému Windows Server. Proveďte následující kroky na serveru, kam jste stáhli agenta.

1. Otevřete agenta Služeb zotavení Microsoft Azure. Najdete ho vyhledáním **Microsoft Azure Backup** ve svém počítači.

2.  V konzole agenta služeb zotavení klikněte na **plánem zálohování** pod **podokna akce**.

    ![Příprava infrastruktury](./media/tutorial-backup-windows-server-to-azure/mars-schedule-backup.png)

3. Klikněte na tlačítko **Další** přejděte na **vybrat položky k Back up** stránky.

4. Klikněte na tlačítko **přidat položky** a z dialogového okna, které se otevře, vyberte **stav systému** a soubory nebo složky, které chcete zálohovat. Pak klikněte na **OK**.

5. Klikněte na **Další**.

6. Na **zadat plán zálohování (stav systému)** zadejte čas dne nebo týdne při zálohování muset spustit pro stav systému a klikněte na tlačítko **další** 

7.  Na **výběr zásady uchovávání informací (stav systému)** vyberte zásady uchovávání informací pro záložní kopii pro stav systému a klikněte na tlačítko **další**
8. Podobně vyberte zásadu zálohování, plán a uchování pro vybrané soubory a složky. 
8.  Na **vyberte počáteční zpět do typu** ponechte možnost **automaticky přes síť** vybrané a potom klikněte na **Další**.
9.  Na **potvrzení** stránka, zkontrolujte informace a pak klikněte na tlačítko **Dokončit**.
10. Až průvodce dokončí vytváření plánu zálohování, klikněte na **Zavřít**.

## <a name="perform-an-ad-hoc-back-up"></a>Proveďte zálohování ad-hoc

Plán jste vytvořili při spuštění úlohy zálohování. Nebyly však zazálohovali server. Je osvědčeným postupem obnovení po havárii spustit zálohu na vyžádání zajistit odolnost dat pro váš server.

1.  V konzole agenta služeb zotavení Microsoft Azure, klikněte na **zálohovat nyní**.

    ![Příprava infrastruktury](./media/tutorial-backup-windows-server-to-azure/mars-schedule-backup.png)

2.  Na **zálohovat nyní** průvodce, vyberte jeden z **soubory a složky** nebo **stav systému** , které chcete zálohovat a klikněte na tlačítko **další** 
3. Na **potvrzení** stránka, zkontrolujte nastavení, **zálohovat nyní** průvodce pomocí zálohování serveru. Poté klikněte na **Zálohovat**.
4.  Průvodce zavřete kliknutím na **Zavřít**. Pokud průvodce zavřete před dokončením záložní proces, Průvodce zůstane spuštěný na pozadí.
4.  Po dokončení prvotní zálohování **úloha byla dokončena** stav se zobrazí na **úlohy** podokně konzoly agenta MARS.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste použili portál Azure: 
 
> [!div class="checklist"] 
> * Vytvoření trezoru Služeb zotavení 
> * Stáhnout agenta služeb zotavení Microsoft Azure 
> * Instalace agenta 
> * Konfigurace zálohování pro Windows Server 
> * Proveďte zálohu na vyžádání 

Pokračovat v dalším kurzu chcete obnovit soubory z Azure do systému Windows Server

> [!div class="nextstepaction"] 
> [Obnovit soubory z Azure do systému Windows Server](./tutorial-backup-restore-files-windows-server.md) 

