---
title: "Postup zálohování souborů a složek z Windows do Azure s Azure Backup pomocí modelu nasazení Resource Manageru | Dokumentace Microsoftu"
description: "Postup zálohování dat Windows Serveru pomocí vytvoření trezoru, nainstalování agenta Služeb zotavení, a zálohování vašich souborů a složek do Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: cfreeman
editor: 
keywords: "postup zálohování; jak zálohovat"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/27/2016
ms.author: markgal;
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 900967975694a688b6d5054cb351746819b65f16


---
# <a name="first-look-back-up-files-and-folders-with-azure-backup-using-the-resource-manager-deployment-model"></a>První pohled: zálohování souborů a složek s Azure Backup pomocí modelu nasazení Resource Manageru
Tento článek vysvětluje, jak postupovat při zálohování souborů a složek z Windows Serveru (nebo z klienta Windows) do Azure s Azure Backup pomocí Resource Manageru. Tento kurz vás má provést základy. Chcete-li začít používat Azure Backup, jste na správném místě.

Chcete-li se dozvědět více o Azure Backup, přečtěte si tento [přehled](backup-introduction-to-azure-backup.md).

Pro zálohování souborů a složek do Azure postupujte následovně:

![Krok 1](./media/backup-try-azure-backup-in-10-mins/step-1.png) Získejte předplatné Azure (pokud ho ještě nemáte).<br>
![Krok 2](./media/backup-try-azure-backup-in-10-mins/step-2.png) Vytvořte trezor služby Recovery Services.<br>
![Krok 3](./media/backup-try-azure-backup-in-10-mins/step-3.png) Stáhněte potřebné soubory.<br>
![Krok 4](./media/backup-try-azure-backup-in-10-mins/step-4.png) Nainstalujte a zaregistrujte agenta Recovery Services.<br>
![Krok 5](./media/backup-try-azure-backup-in-10-mins/step-5.png) Zazálohujte soubory a složky.

![Jak zálohovat počítač se systémem Windows s Azure Backup](./media/backup-try-azure-backup-in-10-mins/backup-process.png)

## <a name="step-1-get-an-azure-subscription"></a>Krok 1 – Získání předplatného Azure.
Pokud předplatné Azure nemáte, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/), který vám umožní přístup ke službám Azure.

## <a name="step-2-create-a-recovery-services-vault"></a>Krok 2 – Vytvoření trezoru Služeb zotavení.
Chcete-li zálohovat svoje soubory a složky, musíte vytvořit trezor Služeb zotavení v oblasti, kde chcete data ukládat. Musíte také určit způsob replikace úložiště.

### <a name="to-create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení
1. Pokud jste to ještě neudělali, přihlaste se k [portálu Azure](https://portal.azure.com/) pomocí svého předplatného Azure.
2. V nabídce centra klikněte na **Procházet**, v seznamu prostředků zadejte **Služby zotavení** a klikněte na **Trezory služeb zotavení**.
   
    ![Vytvoření trezoru Služeb zotavení – krok 1](./media/backup-try-azure-backup-in-10-mins/browse-to-rs-vaults.png) <br/>
3. V nabídce **Trezory Služeb zotavení** klikněte na **Přidat**.
   
    ![Vytvoření trezoru Recovery Services – krok 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)
   
    Otevře se okno trezoru Recovery Services s výzvou k vyplnění polí **Název**, **Předplatné**, **Skupina prostředků** a **Oblast**.
   
    ![Vytvoření trezoru Recovery Services – krok 5](./media/backup-try-azure-backup-in-10-mins/rs-vault-attributes.png)
4. Jako **Název** zadejte popisný název pro identifikaci trezoru.
5. Kliknutím na **Předplatné** zobrazíte seznam dostupných předplatných.
6. Kliknutím na **Skupina prostředků** zobrazíte seznam dostupných skupin prostředků, nebo klikněte na **Nová**, chcete-li vytvořit novou skupinu prostředků.
7. Klikněte na **Oblast** a vyberte zeměpisnou oblast trezoru. Tato volba určuje geografickou oblast, kam jsou zasílaná vaše zálohovaná data.
8. Klikněte na možnost **Vytvořit**.
   
    Pokud po vytvoření svůj trezor nevidíte v seznamu, klikněte na **Obnovit**. Až se seznam aktualizuje, klikněte na název trezoru.

### <a name="to-determine-storage-redundancy"></a>Určení redundance úložiště
Při prvním vytvoření trezoru Služeb zotavení určíte, jak má být úložiště replikované.

1. Kliknutím na nový trezor otevřete řídící panel.
2. V okně **Nastavení**, které se otevře automaticky s řídícím panelem trezoru, klikněte na **Infrastruktura zálohování**.
3. V okně Infrastruktura zálohování klikněte na **Nastavení zálohování** pro zobrazení **Typu replikace úložiště**.
   
    ![Vytvoření trezoru Služeb zotavení – krok 5](./media/backup-try-azure-backup-in-10-mins/backup-infrastructure.png)
4. Zvolte vhodnou možnost replikace pro svůj trezor.
   
    ![Seznam trezorů služeb zotavení](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)
   
    Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště. Pokud používáte Azure jako primární koncový bod úložiště záloh, pokračujte v používání geograficky redundantního úložiště. Pokud Azure není váš primární koncový bod úložiště záloh, vyberte místně redundantní úložiště, což sníží náklady na ukládání dat v Azure. Další informace o možnostech [geograficky redundantního](../storage/storage-redundancy.md#geo-redundant-storage) a [místně redundantního](../storage/storage-redundancy.md#locally-redundant-storage) úložiště naleznete v tomto [přehledu](../storage/storage-redundancy.md).

Nyní, když jste vytvořili trezor, připravíte infrastrukturu pro zálohování souborů a složek stažením agenta Služeb zotavení Microsoft Azure a přihlašovacích údajů trezoru. 

## <a name="step-3-download-files"></a>Krok 3 – Stažení souborů
1. V řídícím panelu trezoru Služeb zotavení klikněte na **Nastavení**.
   
    ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/settings-button.png)
2. V okně Nastavení klikněte na **Začínáme > Zálohování**.
   
    ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/getting-started-backup.png)
3. V okně Zálohování klikněte na **Cíl zálohování**.
   
    ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/backup-goal.png)
4. Z nabídky Kde je spuštěná vaše úloha? vyberte **Místní**.
5. Z nabídky Co chcete zálohovat? vyberte **Soubory a složky** a klikněte na **OK**.

### <a name="download-the-recovery-services-agent"></a>Stažení agenta Služeb zotavení
1. V okně **Příprava infrastruktury** klikněte na **Stáhnout agenta pro Windows Server nebo klienta Windows**.
   
    ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/prepare-infrastructure-short.png)
2. V automaticky otevřeném okně stahování klikněte na **Uložit**. Ve výchozím nastavení se soubor **MARSagentinstaller.exe** uloží do složky Stažené soubory.

### <a name="download-vault-credentials"></a>Stažení přihlašovacích údajů trezoru
1. V okně Příprava infrastruktury klikněte na **Stáhnout > Uložit**.
   
    ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/prepare-infrastructure-download.png)

## <a name="step-4-install-and-register-the-agent"></a>Krok 4 – Instalace a registrace agenta
> [!NOTE]
> Povolení zálohování prostřednictvím portálu Azure bude možné již brzy. Prozatím použijte pro zálohování svých souborů a složek místního agenta Služeb zotavení Microsoft Azure.
> 
> 

1. Ve složce Stažené soubory (nebo ve složce, kterou jste vybrali pro stahování) vyhledejte soubor **MARSagentinstaller.exe** a poklikejte na něj.
2. Dokončete Průvodce instalací agenta Služeb zotavení Microsoft Azure. K dokončení průvodce budete muset:
   
   * Vybrat umístění instalace a složky mezipaměti.
   * Zadat informace o serveru proxy, pokud pro připojování k internetu používáte server proxy.
   * Zadat svoje uživatelské jméno a heslo, pokud používáte ověřený server proxy.
   * Zadat stažené přihlašovací údaje trezoru.
   * Uložit šifrovací heslo na bezpečné místo.
     
     > [!NOTE]
     > Pokud heslo ztratíte nebo zapomenete, Microsoft vám nemůže pomoci obnovit zálohovaná data. Uložte prosím soubor na bezpečné místo. Je požadováno pro obnovení zálohy.
     > 
     > 

Agent je nyní nainstalovaný a váš počítač je registrovaný k trezoru. Jste připraveni nakonfigurovat a naplánovat zálohování.

## <a name="step-5-back-up-your-files-and-folders"></a>Krok 5 – Zálohování souborů a složek
Prvotní záloha zahrnuje dvě klíčové úlohy:

* Naplánování zálohování
* První zálohování souborů a složek

K dokončení prvotního zálohy použijte agenta Služeb zotavení Microsoft Azure.

### <a name="to-schedule-the-backup"></a>Naplánování zálohování
1. Otevřete agenta Služeb zotavení Microsoft Azure. Najdete ho vyhledáním **Microsoft Azure Backup** ve svém počítači.
   
    ![Spuštění agenta Služeb zotavení Azure](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)
2. V agentu Služeb zotavení klikněte na **Naplánovat zálohování**.
   
    ![Naplánování zálohování Windows Serveru](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)
3. Na stránce Začínáme v Průvodci plánování zálohování klikněte na **Další**.
4. Na stránce Výběr položek k zálohování klikněte na **Přidat**.
5. Vyberte soubory a složky, které chcete zálohovat, a poté klikněte na **V pořádku**.
6. Klikněte na **Další**.
7. Na stránce **Zadání plánu zálohování** zadejte **plán zálohování** a klikněte na **Další**.
   
    Můžete naplánovat denní (probíhající maximálně třikrát za den) nebo týdenní zálohování.
   
    ![Položky k zálohování z Windows Serveru](./media/backup-try-azure-backup-in-10-mins/specify-backup-schedule-close.png)
   
   > [!NOTE]
   > Další informace o tom, jak zadat plán zálohování, naleznete v článku [Use Azure Backup to replace your tape infrastructure](backup-azure-backup-cloud-as-tape.md) (Použití služby Azure Backup k nahrazení páskové infrastruktury).
   > 
   > 
8. Na stránce **Výběr zásady uchovávání informací** vyberte **zásadu uchovávání informací** pro záložní kopii.
   
    Zásada uchovávání informací určuje dobu, po kterou bude záloha uložená. Místo zadání „ploché zásady“ pro všechny body zálohy můžete zadat různé zásady uchovávání informací v závislosti na tom, kdy dochází k zálohování. Podle svých potřeb můžete upravit denní, týdenní, měsíční a roční zásady uchovávání informací.
9. Na stránce Výběr typu prvotní zálohy vyberte typ prvotní zálohy. Ponechejte vybranou možnost **Automaticky přes síť** a poté klikněte na **Další**.
   
    Zálohovat můžete automaticky přes síť nebo offline. Zbývající část tohoto článku popisuje proces automatického zálohování. Pokud upřednostňujete offline zálohování, přečtěte si článek [Pracovní postup offline zálohování v Azure Backup](backup-azure-backup-import-export.md), kde naleznete další informace.
10. Na stránce Potvrzení zkontrolujte informace a poté klikněte na **Dokončit**.
11. Až průvodce dokončí vytváření plánu zálohování, klikněte na **Zavřít**.

### <a name="to-back-up-files-and-folders-for-the-first-time"></a>První zálohování souborů a složek
1. Chcete-li dokončit prvotní synchronizaci přes síť, v agentu Služeb zotavení klikněte na **Zálohovat nyní**.
   
    ![Zálohovat nyní ve Windows Serveru](./media/backup-try-azure-backup-in-10-mins/backup-now.png)
2. Na stránce Potvrzení zkontrolujte nastavení, které Průvodce Zálohování nyní použije k zálohování počítače. Poté klikněte na **Zálohovat**.
3. Průvodce zavřete kliknutím na **Zavřít**. Pokud to uděláte před dokončením procesu zálohování, průvodce zůstane spuštěný v pozadí.

Po dokončení prvotní zálohy se v konzole Zálohování zobrazí stav **Úloha byla dokončena**.

![Dokončení IR](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="questions"></a>Máte dotazy?
Máte-li nějaké dotazy nebo pokud víte o funkci, kterou byste uvítali, [odešlete nám svůj názor](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Další kroky
* Zdroj dalších informací o [zálohování počítačů se systémem Windows](backup-configure-vault.md).
* Teď, když jste zálohovali své soubory a složky, můžete [spravovat svoje trezory a servery](backup-azure-manage-windows-server.md).
* Potřebujete-li obnovit zálohu, použijte tento článek k [obnovení souborů na počítač se systémem Windows](backup-azure-restore-windows-server.md).




<!--HONumber=Nov16_HO2-->


