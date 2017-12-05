---
title: "Zálohování virtuálních počítačů Azure ve velkém měřítku | Microsoft Docs"
description: "Současně zálohovat více virtuálních počítačů do Azure"
services: backup
keywords: "zálohování virtuálních počítačů; zálohování virtuálního počítače; zálohování virtuálních počítačů; zálohování virtuálních počítačů; zálohování virtuálních počítačů Azure; zálohování a zotavení po havárii"
author: markgalioto
ms.author: markgal
ms.date: 09/16/2017
ms.topic: tutorial
ms.service: backup
ms.custom: mvc
ms.openlocfilehash: 74ccf95b559b690eb53c2f4df14513dab5a94677
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="use-azure-portal-to-back-up-multiple-virtual-machines"></a>Zálohovat více virtuálních počítačů pomocí portálu Azure

Při zálohování dat v Azure, jsou data uložena v prostředek služby Azure, názvem trezoru služeb zotavení. Je k dispozici z nabídky nastavení služeb nejvíce Azure prostředek trezoru služeb zotavení. Výhodou vytvoření trezoru služeb zotavení, integrované do nabídky nastavení služeb nejvíce Azure lze velmi snadno zálohovat data. Jednotlivě práce každé databáze nebo virtuální počítač ve vašem podniku je však zdlouhavé. Co dělat, když chcete zálohovat data pro všechny virtuální počítače v jednom oddělení nebo na jednom místě? Je snadné zálohovat více virtuálních počítačů vytvořením zásady zálohování a použití zásad na požadovaný virtuální počítače. Tento kurz vysvětluje postup:

> [!div class="checklist"]
> * Vytvoření trezoru Služeb zotavení
> * Definování zásad zálohování
> * Použít zásady zálohování pro ochranu více virtuálních počítačů
> * Spustit úlohu zálohování na vyžádání pro chráněné virtuální počítače

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal

Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

Trezor služeb zotavení obsahuje zálohovaná data a zásady zálohování u chráněných virtuálních počítačů. Zálohování virtuálních počítačů je místní proces. Do trezoru služeb zotavení v jiném umístění nelze zálohovat virtuální počítač z jednoho umístění. Ano pro každé umístění Azure, s virtuálními počítači zálohovat, musí existovat alespoň jeden trezor služeb zotavení v tomto umístění.

1. V levé nabídce vyberte **další služby** a v seznamu služeb zadejte *služeb zotavení*. Při psaní se seznam prostředků bude filtrovat. Až uvidíte trezory služeb zotavení v seznamu, vyberte ho a otevřete nabídku trezory služeb zotavení.

    ![Otevřít nabídku trezoru služeb zotavení](./media/tutorial-backup-vm-at-scale/full-browser-open-rs-vault.png) <br/>

2. V **trezory služeb zotavení** nabídky, klikněte na tlačítko **přidat** otevřete nabídku trezoru služeb zotavení.

    ![Otevřít trezor nabídky](./media/tutorial-backup-vm-at-scale/provide-vault-detail-2.png)

3. V služeb zotavení trezoru nabídce 

    - Typ *myRecoveryServicesVault* v **název**,
    - ID se zobrazí v aktuálním předplatném **předplatné**. Pokud máte další odběry, může zvolit jiné předplatné pro nový trezor.
    - Pro **skupiny prostředků** vyberte **použít existující** a zvolte *myResourceGroup*. Pokud *myResourceGroup* neexistuje, vyberte **vytvořit nový** a typ *myResourceGroup*.
    - Z **umístění** rozevírací nabídky vyberte *západní Evropa*.
    - Klikněte na tlačítko **vytvořit** pro vytvoření trezoru služeb zotavení.

Trezor služeb zotavení musí být ve stejném umístění jako virtuální počítače, který je chráněn. Pokud máte virtuální počítače v několika oblastech, vytvořte trezor služeb zotavení v každé oblasti. V tomto kurzu vytvoří trezoru služeb zotavení v *západní Evropa* vzhledem k tomu, který je tam, kde *Můjvp* (vytvoření virtuální počítače vytvořené pomocí rychlé spuštění).

Vytvoření trezoru služby Recovery Services může trvat několik minut. Sledujte oznámení o stavu v pravé horní části portálu. Když je trezor vytvořený, zobrazí se v seznamu trezorů Služeb zotavení.

Při vytváření trezoru služeb zotavení, má trezoru ve výchozím nastavení geograficky redundantní úložiště. K zajištění ochrany dat, geograficky redundantní úložiště replikuje data vícekrát rámci dvou oblastí Azure.

## <a name="set-backup-policy-to-protect-vms"></a>Nastavte zásady zálohování pro ochranu virtuálních počítačů

Po vytvoření trezoru služeb zotavení, dalším krokem je konfigurace úložiště pro typ dat a k nastavení zásad zálohování. Zásady zálohování je plán, jak často a kdy jsou pořizovány body obnovení. Zásada také obsahuje rozsah uchování bodů obnovení. V tomto kurzu Předpokládejme vaší společnosti sportu komplexní s hotelů, stadium a restaurace a koncese a chráníte data na virtuálních počítačích. Následující kroky slouží k vytvoření zásady zálohování pro finanční údaje.

1. V seznamu trezorů služeb zotavení vyberte **myRecoveryServicesVault** otevřete svůj řídicí panel.

   ![Otevřete nabídku scénář](./media/tutorial-backup-vm-at-scale/open-vault-from-list.png)

2. V nabídce řídícího panelu trezoru, klikněte na tlačítko **zálohování** otevřete nabídku zálohování.

3. V nabídce Cíl zálohování v **kde běží vaše úlohy** rozevírací nabídky vyberte *Azure*. Z **co chcete zálohovat** rozevíracího seznamu, vyberte *virtuálního počítače*a klikněte na tlačítko **zálohování**.

    Tyto akce připravte trezor služeb zotavení pro interakci s virtuálním počítačem. Trezory služeb zotavení mít výchozí zásadu, která vytvoří bod obnovení každý den a uchovává body obnovení po dobu 30 dnů.

    ![Otevřete nabídku scénář](./media/tutorial-backup-vm-at-scale/backup-goal.png)

4. Chcete-li vytvořit nové zásady, v nabídce zásady zálohování z **vyberte zásady zálohování** rozevírací nabídky vyberte *vytvořit nový*.

    ![Výběr úlohy](./media/tutorial-backup-vm-at-scale/create-new-policy.png)

5. V **zálohování zásad** nabídce pro **název zásady** typ *finanční*. Zadejte zásady zálohování pro následující změny: 
    - Pro **četnost záloh** nastavit časové pásmo pro *střední čas*. Protože je komplexní sportu Texas, vlastník chce načasování jako místní. Ponechte četnost zálohování nastavit na každý den ve 3:30:00.
    - Pro **uchování denního bodu zálohy**, nastavení období na 90 dní.
    - Pro **uchování týdenního bodu zálohy**, použijte *pondělí* bod obnovení a uchovávány 52 týdnů.
    - Pro **uchování měsíčního bodu zálohy**pomocí bodu od neděle první den v měsíci a ukládat je 36 měsíců.
    - Zrušte výběr **uchování ročního bodu zálohy** možnost. Vedoucí finanční nechce uchovávat data delší než 36 měsíců.
    - Kliknutím na **OK** vytvořte zásadu zálohování.

    ![Výběr úlohy](./media/tutorial-backup-vm-at-scale/set-new-policy.png) 

    Po vytvoření zásady zálohování, přidružte zásady s virtuálními počítači.

6. V **vybrat virtuální počítače** dialogovém okně vyberte *Můjvp* a klikněte na tlačítko **OK** k nasazení zásady zálohování virtuálních počítačů. 

    Zobrazí všechny virtuální počítače, které jsou ve stejném umístění a ještě nejsou přidružené k zásadě zálohování. *myVMH1* a *myVMR1* vybraných pro přidružen *finanční* zásad.

    ![Výběr úlohy](./media/tutorial-backup-vm-at-scale/choose-vm-to-protect.png) 

    Po dokončení nasazení, dostanete oznámení, že nasazení byla úspěšně dokončena.

## <a name="initial-backup"></a>Prvotní zálohování

Jste povolili zálohování pro trezory služeb zotavení, ale nebyl vytvořen prvotní zálohování. Osvědčený postup obnovení po havárii pro aktivaci první zálohy je tak, aby vaše data jsou chráněna. 

Spustit úlohu zálohování na vyžádání:

1. Na řídicím panelu trezoru, klikněte na tlačítko **3** pod **zálohování položek**, otevřete nabídku zálohování položek.

    ![Ikona nastavení](./media/tutorial-backup-vm-at-scale/tutorial-vm-back-up-now.png)

    **Zálohování položek** otevře se nabídka.

2. Na **zálohování položek** nabídky, klikněte na tlačítko **virtuální počítač Azure** otevřete seznam virtuálních počítačů, které jsou přidružené k trezoru.

    ![Ikona nastavení](./media/tutorial-backup-vm-at-scale/three-virtual-machines.png)

    Otevře se seznam **Zálohování položek**.

    ![Aktivovaná úloha zálohování.](./media/tutorial-backup-vm-at-scale/initial-backup-context-menu.png)

3. V seznamu **Zálohování položek** klikněte na **...** (tři tečky) a otevřete místní nabídku.

4. V místní nabídce vyberte **zálohovat nyní**.

    ![Místní nabídka](./media/tutorial-backup-vm-at-scale/context-menu.png)

    Otevře se nabídka zálohovat nyní.

5. V nabídce Zálohovat nyní zadáním posledního dne uchování bodu obnovení, a klikněte na **zálohování**.

    ![nastavte poslední den uchování bodu obnovení vytvořeného pomocí možnosti Zálohovat nyní](./media/tutorial-backup-vm-at-scale/backup-now-short.png)

    Oznámení nasazení vás budou informovat o aktivaci úlohy zálohování a možnosti sledovat průběh úlohy na stránce Úlohy zálohování. V závislosti na velikosti virtuálního počítače vytváření prvotní zálohování může chvíli trvat.

    Po dokončení počáteční úlohu zálohování, zobrazí se jeho stav v nabídce úlohy zálohování. Úloha zálohování na vyžádání vytvořit bod obnovení počáteční pro *Můjvp*. Pokud chcete zálohovat jiné virtuální počítače, opakujte tyto kroky pro každý virtuální počítač. 

    ![Dlaždice Úlohy zálohování](./media/tutorial-backup-vm-at-scale/initial-backup-complete.png)
  
## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud máte v úmyslu pokračovat v pro práci s další kurzy, neprovádí vyčištění prostředky vytvořené v tomto kurzu. Pokud neplánujete, chcete-li pokračovat, pomocí následujících kroků můžete odstranit všechny prostředky, které jsou vytvořené v tomto kurzu na webu Azure portal.

1. Na **myRecoveryServicesVault** řídicí panel, klikněte na tlačítko **3** pod **zálohování položek**, otevřete nabídku zálohování položek.

    ![Ikona nastavení](./media/tutorial-backup-vm-at-scale/tutorial-vm-back-up-now.png)


2. Na **zálohování položek** nabídky, klikněte na tlačítko **virtuální počítač Azure** otevřete seznam virtuálních počítačů, které jsou přidružené k trezoru.

    ![Ikona nastavení](./media/tutorial-backup-vm-at-scale/three-virtual-machines.png)

    Otevře se seznam **Zálohování položek**.

3. V **zálohování položek** nabídky, klikněte na tlačítko se třemi tečkami otevřete v místní nabídce.

    ![Ikona nastavení](./media/tutorial-backup-vm-at-scale/context-menu-to-delete-vm.png)

4. V místní nabídce vyberte **zastavení zálohování** otevřete nabídku zastavení zálohování. 

    ![Ikona nastavení](./media/tutorial-backup-vm-at-scale/context-menu-for-delete.png)

5. V **zastavení zálohování** nabídce v horní nabídce rozevíracího seznamu vyberte a zvolte **odstranit Data zálohování**.

6. V **zadejte název zálohované položky** dialogové okno, typ *Můjvp*.
 
7. Po ověření položky zálohování (zaškrtnout se zobrazí), **zastavení zálohování** tlačítko je k dispozici. Klikněte na tlačítko **zastavení zálohování** zásady zastavení a odstranění bodů obnovení. 

    ![Klikněte na tlačítko Zastavit zálohy trezoru](./media/tutorial-backup-vm-at-scale/provide-reason-for-delete.png).

8. V **myRecoveryServicesVault** nabídky, klikněte na tlačítko **odstranit**.

    ![Klikněte na tlačítko Zastavit zálohy trezoru](./media/tutorial-backup-vm-at-scale/deleting-the-vault.png)

    Po odstranění trezoru můžete vrátit do seznamu trezorů služeb zotavení.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste použili portál Azure:

> [!div class="checklist"]
> * Vytvoření trezoru Služeb zotavení
> * Nastavení trezoru k ochraně virtuálních počítačů
> * Vytvořit vlastní zásady pro zálohování a uchovávání
> * Přiřaďte zásady k ochraně více virtuálních počítačů
> * Spustit na vyžádání zpět do virtuálních počítačů

Pokračujte v dalším kurzu k obnovení virtuálního počítače Azure z disku. 

> [!div class="nextstepaction"]
> [Obnovení virtuálních počítačů pomocí rozhraní příkazového řádku](./tutorial-restore-disk.md)
