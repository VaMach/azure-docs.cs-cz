---
title: "První pohled: Ochrana virtuálních počítačů Azure s trezorem zálohování | Dokumentace Microsoftu"
description: "Virtuální počítače Azure s trezorem zálohování. Kurz vysvětluje vytvoření trezoru, registraci virtuálních počítačů, vytvoření zásad a ochranu virtuálních počítačů v Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: cfreeman
editor: 
ms.assetid: 722820dc-b65f-425c-a9e5-c1946e896a87
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/15/2016
ms.author: markgal; jimpark
translationtype: Human Translation
ms.sourcegitcommit: d883cdc007beaf17118c6b6ddbc8345c3bfb5ef2
ms.openlocfilehash: 895eeb27b6050897575c5d6f20f16ea3f99fdcf3


---
# <a name="first-look-backing-up-azure-virtual-machines"></a>První pohled: Zálohování virtuálních počítačů Azure
> [!div class="op_single_selector"]
> * [Ochrana virtuálních počítačů v trezoru služby Recovery Services](backup-azure-vms-first-look-arm.md)
> * [Ochrana virtuálních počítačů Azure s trezorem zálohování](backup-azure-vms-first-look.md)
>
>

Tento kurz vás provede kroky pro zálohování virtuálních počítačů (VM) Azure do trezoru zálohování v Azure. Tento článek popisuje model Classic nebo model nasazení portálu Service Manager pro zálohování virtuálních počítačů. Pokud máte zájem o zálohování virtuálních počítačů do trezoru Recovery Services, který patří do skupiny prostředků, přečtěte si téma [První pohled: Ochrana virtuálních počítačů pomocí trezoru Recovery Services](backup-azure-vms-first-look-arm.md). Pro úspěšné dokončení tohoto kurzu musí být splněny tyto požadavky:

* Vytvořili jste virtuální počítač v rámci svého předplatného Azure.
* Virtuální počítač je připojen k veřejným IP adresám Azure. Další informace naleznete v tématu [Připojení k síti](backup-azure-vms-prepare.md#network-connectivity).

Zálohování virtuálního počítače se skládá z pěti hlavních kroků:  

![1.krok](./media/backup-azure-vms-first-look/step-one.png) Vytvořte trezor záloh nebo určete existující trezor záloh. <br/>
![2.krok](./media/backup-azure-vms-first-look/step-two.png) Použijte portál Azure Classic k vyhledání a registraci virtuálních počítačů. <br/>
![3.krok](./media/backup-azure-vms-first-look/step-three.png) Nainstalujte agenta virtuálního počítače. <br/>
![4.krok](./media/backup-azure-vms-first-look/step-four.png) Vytvořte zásadu pro ochranu virtuálních počítačů. <br/>
![5.krok](./media/backup-azure-vms-first-look/step-five.png) Spusťte zálohování.

![Zobrazení procesu zálohování virtuálního počítače na nejvyšší úrovni](./media/backup-azure-vms-first-look/backupazurevm-classic.png)

> [!NOTE]
> Azure obsahuje dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md). Tento kurz je určen pro použití s virtuálními počítači, které lze vytvořit na portálu Azure Classic. Služba Azure Backup podporuje virtuální počítače založené na správci prostředků. Podrobnosti o zálohování virtuálních počítačů do trezoru Recovery Services naleznete v tématu [První seznámení: Ochrana virtuálních počítačů pomocí trezoru Recovery Services](backup-azure-vms-first-look-arm.md).
>
>

## <a name="step-1---create-a-backup-vault-for-a-vm"></a>Krok 1 – Vytvoření trezoru záloh pro virtuální počítač
Trezor záloh je entita, která ukládá všechny vytvořené zálohy a body obnovení. Trezor záloh obsahuje také zásady zálohování, které se aplikují na zálohované virtuální počítače.

1. Přihlaste se k [portálu Azure Classic](http://manage.windowsazure.com/).
2. V levém dolním rohu webu Azure Portal klikněte na **Nový**.

    ![Možnost Nový](./media/backup-azure-vms-first-look/new-button.png)
3. V průvodci Rychlého vytvoření klikněte na **Datové služby** > **Recovery Services** > **Trezor záloh** > **Rychle vytvořit**.

    ![Vytvoření trezoru záloh](./media/backup-azure-vms-first-look/new-vault-wizard-one-subscription.png)

    Průvodce vás vyzve k zadání **Názvu** a **Oblasti**. Pokud spravujete více než jedno předplatné, zobrazí se dialogové okno pro výběr předplatného.
4. Jako **Název** zadejte popisný název pro identifikaci trezoru. Název musí být jedinečný v rámci předplatného Azure.
5. V rozevírací nabídce **Oblast** vyberte zeměpisnou oblast trezoru. Trezor **musí** být ve stejné oblasti jako virtuální počítače, které chrání.

    Pokud neznáte oblast, ve které je váš virtuální počítač, zavřete průvodce a klikněte na **Virtuální počítače** v seznamu služeb Azure. Sloupec Umístění obsahuje název oblasti. Pokud máte virtuální počítače v několika oblastech, vytvořte trezor záloh v každé oblasti.
6. Pokud se v průvodci neobjeví dialogové okno **Předplatné**, přeskočte na následující krok. Pokud pracujete s více předplatnými, vyberte předplatné, které chcete přidružit k novému trezoru záloh.

    ![Informační zpráva vytvoření trezoru](./media/backup-azure-vms-first-look/backup-vaultcreate.png)
7. Klikněte na **Vytvořit trezor**. Vytvoření trezoru záloh může chvíli trvat. Sledujte oznámení stavu ve spodní části portálu.

    ![Informační zpráva vytvoření trezoru](./media/backup-azure-vms-first-look/create-vault-demo.png)

    Objeví se zpráva s potvrzením, že trezor byl úspěšně vytvořený. Na stránce **Recovery Services** je označené jako **Aktivní**.

    ![Informační zpráva vytvoření trezoru](./media/backup-azure-vms-first-look/create-vault-demo-success.png)
8. V seznamu trezorů na stránce **Recovery Services** vyberte vytvořený trezor pro spuštění stránky **Rychlý start**.

    ![Seznam trezorů záloh](./media/backup-azure-vms-first-look/active-vault-demo.png)
9. Na stránce **Rychlý start** klikněte na **Konfigurovat** pro otevření možnosti replikace úložiště.
    ![Seznam trezorů záloh](./media/backup-azure-vms-first-look/configure-storage.png)
10. V možnosti **Replikace úložiště** zvolte možnost replikace pro svůj trezor.

    ![Seznam trezorů záloh](./media/backup-azure-vms-first-look/backup-vault-storage-options-border.png)

    Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště. Zvolte geograficky redundantní úložiště, pokud se jedná o vaši primární zálohu. Zvolte místně redundantní úložiště, pokud chcete levnější možnost, která není tak trvanlivá. Další informace o možnostech geograficky a místně redundantního úložiště naleznete v tématu [Přehled replikace Azure Storage](../storage/storage-redundancy.md).

Po výběru možnosti úložiště pro svůj trezor jste připraveni k přidružení virtuálního počítače k trezoru. Chcete-li začít přidružení, vyhledejte a zaregistrujte virtuální počítače Azure.

## <a name="step-2---discover-and-register-azure-virtual-machines"></a>Krok 2 – Vyhledání a registrace virtuálních počítačů Azure
Před zaregistrováním virtuálního počítače k trezoru spusťte proces vyhledávání pro identifikaci nových virtuálních počítačů. Ten vrátí seznam virtuálních počítačů v rámci předplatného společně s dalšími informacemi, jako například název cloudové služby a oblast.

1. Přihlaste se k [portálu Azure Classic](http://manage.windowsazure.com/).
2. Na portálu Azure Classic klikněte na **Recovery Services** a otevře se seznam trezorů Recovery Services.
    ![Výběr úlohy](./media/backup-azure-vms-first-look/recovery-services-icon.png)
3. Ze seznamu trezorů vyberte trezor pro zálohování virtuálního počítače.

    Po výběru se trezor otevře na stránce **Rychlý start**.
4. V nabídce trezoru klikněte na **Registrované položky**.

    ![Výběr úlohy](./media/backup-azure-vms-first-look/configure-registered-items.png)
5. Z nabídky **Typ** vyberte **Virtuální počítač Azure**.

    ![Výběr úlohy](./media/backup-azure-vms/discovery-select-workload.png)
6. Klikněte na **Vyhledat** ve spodní části stránky.
    ![Tlačítko Vyhledat](./media/backup-azure-vms/discover-button-only.png)

    Proces vyhledávání může kvůli srovnávání virtuálních počítačů trvat i několik minut. Ve spodní části obrazovky je oznámení, které indikuje, že proces běží.

    ![Vyhledání virtuálních počítačů](./media/backup-azure-vms/discovering-vms.png)

    Po dokončení procesu se oznámení změní.

    ![Vyhledávání dokončeno](./media/backup-azure-vms-first-look/discovery-complete.png)
7. Klikněte na **Registrovat** ve spodní části stránky.
    ![Tlačítko Registrovat](./media/backup-azure-vms-first-look/register-icon.png)
8. V místní nabídce **Registrovat položky** vyberte virtuální počítače, které chcete zaregistrovat.

   > [!TIP]
   > Najednou může být registrováno více virtuálních počítačů.
   >
   >

    Pro každý virtuální počítač, který jste vybrali, se vytvoří úloha.
9. Klikněte na **Zobrazit úlohy** v oznámení pro přechod na stránku **Úlohy**.

    ![Registrace úlohy](./media/backup-azure-vms/register-create-job.png)

    Virtuální počítač se také zobrazí v seznamu registrovaných položek společně se stavem operace registrování.

    ![Stav registrace 1](./media/backup-azure-vms/register-status01.png)

    Po dokončení operace se stav změní, aby reflektoval stav *registrován*.

    ![Stav registrace 2](./media/backup-azure-vms/register-status02.png)

## <a name="step-3---install-the-vm-agent-on-the-virtual-machine"></a>Krok 3 – Na virtuální počítač nainstalujte agenta virtuálního počítače
Pro fungování rozšíření Backup musí být na virtuálním počítači Azure nainstalovaný agent virtuálního počítače Azure. Pokud byl váš virtuální počítač vytvořen z galerie Azure, je na něm agent virtuálního počítače již nainstalován. Můžete přeskočit na [ochranu virtuálních počítačů](backup-azure-vms-first-look.md#step-4---create-the-backup-policy).

Pokud byl virtuální počítač přenesen z místního datového centra, pravděpodobně na něm není agent virtuálního počítače nainstalovaný. Před pokračováním k ochraně virtuálního počítače je potřeba na něj nainstalovat agenta virtuálního počítače. Podrobné pokyny k instalaci agenta virtuálního počítače naleznete v [oddílu Agent virtuálního počítače v článku Zálohování virtuálních počítačů](backup-azure-vms-prepare.md#vm-agent).

## <a name="step-4---create-the-backup-policy"></a>Krok 4 – Vytvoření zásady zálohování
Předtím, než aktivujete úlohu prvotního zálohování, nastavte plán pořizování snímků zálohy. Plán pořizování snímků záloh a doba jejich uchování se nazývá zásada zálohování. Informace o zachování jsou založené na trojgeneračním schématu rotace záloh.

1. Na portálu Azure Classic přejděte v **Recovery Services** do trezoru záloh a klikněte na **Registrované položky**.
2. Z rozevírací nabídky vyberte **Virtuální počítač Azure**

    ![Výběr úlohy v portálu](./media/backup-azure-vms/select-workload.png)
3. Klikněte na **Chránit** ve spodní části stránky.
    ![Klikněte na Chránit](./media/backup-azure-vms-first-look/protect-icon.png)

    Objeví se **Průvodce ochranou položek** a zobrazí *pouze* virtuální počítače, které jsou zaregistrované a nechráněné.

    ![Škálování konfigurace ochrany](./media/backup-azure-vms/protect-at-scale.png)
4. Vyberte virtuální počítače, které chcete ochránit.

    Pokud existují dva nebo více virtuálních počítačů se stejným názvem, použijte k jejich rozlišení Cloudovou službu.
5. V nabídce **Konfigurace ochrany** vyberte existující zásadu nebo vytvořte novou zásadu k ochraně určených virtuálních počítačů.

    Nové trezory záloh mají přidruženou výchozí zásadu. Tato zásada pořizuje každý večer denní snímek, který je uchován po dobu 30 dnů. Ke každé zásadě zálohování může být přidružených více virtuálních počítačů. K virtuálnímu počítači však může být najednou přidružená pouze jedna zásada.

    ![Ochrana pomocí nové zásady](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > Zásada zálohování obsahuje schéma uchovávání pro plánované zálohy. Pokud zvolíte existující zásadu zálohování, nebudete moci v dalším kroku změnit možnosti uchovávání.
   >
   >
6. V **Rozsahu uchovávání** definujte denní, týdenní, měsíční nebo roční rozsah pro konkrétní body zálohy.

    ![Virtuální počítač je zálohovaný s bodem obnovení](./media/backup-azure-vms/long-term-retention.png)

    Zásada uchovávání specifikuje, jak dlouho se záloha má uchovat. Můžete zadat rozdílné zásady v závislosti na tom, kdy dochází k zálohování.
7. Chcete-li zobrazit seznam úloh **Konfigurace ochrany**, klikněte na **Úlohy**.

    ![Úloha konfigurace ochrany](./media/backup-azure-vms/protect-configureprotection.png)

    Nyní, když jste vytvořili zásadu, přejděte k dalšímu kroku a spusťte prvotní zálohování.

## <a name="step-5---initial-backup"></a>Krok 5 – Prvotní zálohování
Jakmile je virtuální počítač chráněný zásadou, můžete si tento vztah prohlédnout na kartě **Chráněné položky**. Před provedením prvotního zálohování bude **Stav ochrany** ukazovat **Chráněno – (čekání na prvotní zálohování)**. Ve výchozím nastavení je první plánovanou zálohou *prvotní záloha*.

![Zálohování čeká na zpracování](./media/backup-azure-vms-first-look/protection-pending-border.png)

Chcete-li nyní spustit prvotní zálohování:

1. Na stránce **Chráněné položky** klikněte na **Zálohovat nyní** v dolní části stránky.
    ![Ikona Zálohovat nyní](./media/backup-azure-vms-first-look/backup-now-icon.png)

    Služba Azure Backup vytvoří úlohu zálohování pro operaci prvotního zálohování.
2. Chcete-li zobrazit seznam úloh, klikněte na kartu **Úlohy**.

    ![Probíhá zálohování](./media/backup-azure-vms-first-look/protect-inprogress.png)

    Po dokončení prvotního zálohování stav virtuálního počítače na kartě **Chráněné položky** je *Chráněný*.

    ![Virtuální počítač je zálohovaný s bodem obnovení](./media/backup-azure-vms/protect-backedupvm.png)

   > [!NOTE]
   > Zálohování virtuálních počítačů je místní proces. Virtuální počítače z jedné oblasti nelze zálohovat do trezoru záloh v jiné oblasti. Pro každou oblast Azure s virtuálními počítači, které je třeba zálohovat, je tedy nutné vytvořit v této oblasti alespoň jeden trezor záloh.
   >
   >

## <a name="next-steps"></a>Další kroky
Když jste teď úspěšně zálohovali virtuální počítač, je několik dalších kroků, které by vás mohly zajímat. Nejlogičtějším krokem je seznámení se s obnovováním dat na virtuálním počítači. Nicméně existují úlohy správy, které vám pomohou pochopit, jak zabezpečit dat a minimalizovat náklady.

* [Správa a monitorování virtuálních počítačů](backup-azure-manage-vms.md)
* [Obnovení virtuálních počítačů](backup-azure-restore-vms.md)
* [Pokyny při řešení potíží](backup-azure-vms-troubleshoot.md)

## <a name="questions"></a>Máte dotazy?
Máte-li nějaké dotazy nebo pokud víte o funkci, kterou byste uvítali, [odešlete nám svůj názor](http://aka.ms/azurebackup_feedback).



<!--HONumber=Nov16_HO4-->


