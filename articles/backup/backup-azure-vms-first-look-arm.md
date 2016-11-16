---
title: "První pohled: chraňte virtuální počítače Azure pomocí trezoru Recovery Services | Dokumentace Microsoftu"
description: "Virtuální počítače Azure s trezorem Recovery Services. Ochrana dat pomocí záloh virtuálních počítačů nasazených Resource Managerem, virtuálních počítačů nasazených službou Classic a virtuálních počítačů služby Storage úrovně Premium. Vytvoření a registrace trezoru Recovery Services. Registrace virtuálních počítačů, vytváření zásad a ochrana virtuálních počítačů v Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: cfreeman
editor: 
keyword: backups; vm backup
ms.assetid: 45e773d6-c91f-4501-8876-ae57db517cd1
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 10/13/2016
ms.author: markgal; jimpark
translationtype: Human Translation
ms.sourcegitcommit: e29891dc03f8a864ecacc893fd1cc0d3cc1436cb
ms.openlocfilehash: 3fe4d985c62b8476bd3b3f923fa17e7f364f9352


---
# <a name="first-look-protect-azure-vms-with-a-recovery-services-vault"></a>První pohled: Ochrana virtuálních počítačů s trezorem Recovery Services
> [!div class="op_single_selector"]
> * [Ochrana virtuálních počítačů v trezoru služby Recovery Services](backup-azure-vms-first-look-arm.md)
> * [Ochrana virtuálních počítačů v úložišti Backup Vault](backup-azure-vms-first-look.md)
>
>

Tento kurz vás provede kroky pro vytvoření trezoru Recovery Services a zálohování virtuálních počítačů (VM) Azure. Trezory Recovery Services chrání:

* Virtuální počítače nasazené Azure Resource Managerem
* Klasické virtuální počítače
* Virtuální počítače služby Storage úrovně Standard
* Virtuální počítače služby Storage úrovně Premium
* Virtuální počítače jsou šifrované službou Azure Disk Encryption klíči BEK a KEK

Další informace o ochraně virtuálních počítačů služby Storage úrovně Premium naleznete v tématu [Zálohování a obnovení virtuálních počítačů služby Storage úrovně Premium](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup).

> [!NOTE]
> Tento kurz předpokládá, že už máte virtuální počítač ve svém předplatném Azure a že jste zavedli opatření, která umožní službě zálohování přístup k virtuálnímu počítači.
>
>

[!INCLUDE [learn-about-Azure-Backup-deployment-models](../../includes/backup-deployment-models.md)]

Dokončíte tyto hlavní kroky.  

1. Vytvoření trezoru Recovery Services pro virtuální počítač.
2. Použití webu Azure Portal k výběru scénáře, nastavení zásady a identifikaci položek, které mají být chráněné.
3. Spuštění prvotního zálohování.

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Vytvoření trezoru Recovery Services pro virtuální počítač
Trezor Recovery Services je entita, která ukládá všechny vytvořené zálohy a body obnovení. Trezor Recovery Services obsahuje také zásadu zálohování, která se používá pro chráněné virtuální počítače.

> [!NOTE]
> Zálohování virtuálního počítače je místní proces. Virtuální počítače z jedné oblasti nelze zálohovat do trezoru Recovery Services v jiné oblasti. Pro každou oblast Azure s virtuálními počítači, které se mají zálohovat, tedy musí existovat alespoň jeden trezor Recovery Services.
>
>

Vytvoření trezoru Recovery Services:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).
2. V nabídce centra klikněte na **Procházet** a v seznamu prostředků zadejte **Recovery Services**. Seznam se průběžně filtruje podle zadávaného textu. Klikněte na **Trezor Recovery Services**.

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-vms-first-look-arm/browse-to-rs-vaults.png) <br/>

    Zobrazí se seznam trezorů Recovery Services.
3. V nabídce **Trezory Recovery Services** klikněte na **Přidat**.

    ![Vytvoření trezoru Recovery Services – krok 2](./media/backup-azure-vms-first-look-arm/rs-vault-menu.png)

    Otevře se okno trezoru Recovery Services s výzvou k vyplnění polí **Název**, **Předplatné**, **Skupina prostředků** a **Oblast**.

    ![Vytvoření trezoru Recovery Services – krok 5](./media/backup-azure-vms-first-look-arm/rs-vault-attributes.png)
4. Jako **Název** zadejte popisný název pro identifikaci trezoru. Název musí být jedinečný v rámci předplatného Azure. Zadejte název v rozsahu 2 až 50 znaků. Musí začínat písmenem a může obsahovat pouze písmena, číslice a pomlčky.
5. Kliknutím na **Předplatné** zobrazíte seznam dostupných předplatných. Pokud si nejste jisti, jaké předplatné použít, použijte výchozí (nebo navrhované) předplatné. Více možností je dostupných, jen pokud je váš účet organizace přidružený k více předplatným Azure.
6. Klikněte na **Skupina prostředků** a zobrazte seznam dostupných skupin prostředků, nebo klikněte na **Nová** a vytvořte novou skupinu prostředků. Úplnější informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md)
7. Klikněte na **Oblast** a vyberte zeměpisnou oblast trezoru. Trezor **musí** být ve stejné oblasti jako virtuální počítače, které chcete chránit.

   > [!IMPORTANT]
   > Pokud si nejste jisti oblastí, ve které jsou vaše virtuální počítače, zavřete dialogové okno vytvoření trezoru a na portálu přejděte na seznam virtuálních počítačů. Pokud máte virtuální počítače v několika oblastech, vytvořte trezor Recovery Services v každé z nich. Vytvořte trezor nejprve v první oblasti, poté přejděte k další oblasti. Není potřeba specifikovat účty úložiště pro ukládání zálohovaných dat – trezor Recovery Services a služba Azure Backup se o to postarají automaticky.
   >
   >
8. Klikněte na možnost **Vytvořit**. Vytvoření trezoru Recovery Services může chvíli trvat. Sledujte oznámení o stavu v horní pravé části portálu. Když je trezor vytvořený, zobrazí se v seznamu trezorů Recovery Services.

    ![Seznam trezorů záloh](./media/backup-azure-vms-first-look-arm/rs-list-of-vaults.png)

Teď, když jste vytvořili trezor, se naučte, jak nastavit replikaci úložiště.

### <a name="set-storage-replication"></a>Nastavení replikace úložiště
Možnost replikace úložiště umožňuje výběr mezi geograficky redundantním úložištěm a místně redundantním úložištěm. Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště. Ponechte možnost geograficky redundantní úložiště, pokud se jedná o vaši primární zálohu. Pokud chcete levnější možnost, která není tak trvanlivá, vyberte místně redundantní úložiště. Další informace o možnostech [geograficky redundantního](../storage/storage-redundancy.md#geo-redundant-storage) a [místně redundantního](../storage/storage-redundancy.md#locally-redundant-storage) úložiště naleznete v tématu [Přehled replikace Azure Storage](../storage/storage-redundancy.md).

Chcete-li upravit nastavení replikace úložiště:

1. Vyberte svůj trezor pro otevření řídícího panelu trezoru a okna Nastavení. Pokud se okno **Nastavení** neotevře, klikněte na **Všechna nastavení** v řídicím panelu trezoru.
2. Kliknutím na **Infrastruktura zálohování** > **Konfigurace zálohování** v okně **Nastavení** otevřete okno **Konfigurace zálohování**. V okně **Konfigurace zálohování** zvolte pro svůj trezor možnost replikace úložiště.

    ![Seznam trezorů záloh](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Po výběru možnosti úložiště pro svůj trezor jste připraveni k přidružení virtuálního počítače k trezoru. Chcete-li začít přidružení, měli byste vyhledat a zaregistrovat virtuální počítače Azure.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Výběr cíle zálohování, nastavení zásad a určení položek k ochraně
Před registrací virtuálních počítačů k trezoru spusťte proces vyhledávání, abyste se ujistili, že byly identifikované všechny nové virtuální počítače přidané k předplatnému. Proces se dotáže Azure na seznam virtuálních počítačů v rámci předplatného společně s dalšími informacemi, jako například název cloudové služby a oblast. Na webu Azure Portal výraz scénář vyjadřuje, co chcete vložit do trezoru Recovery Services. Zásada je plán, jak často a kdy jsou pořizovány body obnovení. Zásada také obsahuje rozsah uchování bodů obnovení.

1. Pokud již máte otevřený trezor Recovery Services, pokračujte ke kroku 2. Pokud nemáte otevřený trezor Recovery Services, ale jste na webu Azure Portal, klikněte na **Procházet** v Nabídce centra.

   * V seznamu prostředků zadejte **Recovery Services**.
   * Seznam se průběžně filtruje podle zadávaného textu. Až uvidíte **Trezory Recovery Services**, klikněte na ně.

     ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-vms-first-look-arm/browse-to-rs-vaults.png) <br/>

     Objeví se seznam trezorů Recovery Services.
   * Ze seznamu trezorů Recovery Services vyberte trezor.

     Otevře se řídicí panel vybraného trezoru.

     ![Otevřené okno trezoru](./media/backup-azure-vms-first-look-arm/vault-settings.png)
2. Kliknutím na **Zálohování** v nabídce řídicího panelu trezoru otevřete okno Zálohování.

    ![Otevřené okno Zálohování](./media/backup-azure-vms-first-look-arm/backup-button.png)

    Po otevření okna vyhledá služba zálohování všechny virtuální počítače v rámci předplatného.

    ![Vyhledání virtuálních počítačů](./media/backup-azure-vms-first-look-arm/discovering-new-vms.png)
3. Kliknutím na **Cíl zálohování** v okně Zálohování otevřete okno Cíl zálohování.

    ![Otevřené okno Scénář](./media/backup-azure-vms-first-look-arm/select-backup-goal-one.png)
4. V okně Cíl zálohování nastavte **Kde běží vaše úloha** na Azure a **Co chcete zálohovat** na Virtuální počítač, poté klikněte na **OK**.

    Zavře se okno Cíl zálohování a otevře se okno Zásady zálohování.

    ![Otevřené okno Scénář](./media/backup-azure-vms-first-look-arm/select-backup-goal-two.png)
5. V okně Zásady zálohování vyberte zásadu zálohování, kterou chcete použít pro trezor a klikněte na **OK**.

    ![Výběr zásady zálohování](./media/backup-azure-vms-first-look-arm/setting-rs-backup-policy-new.png)

    Detaily výchozí zásady jsou uvedené v podrobnostech. Pokud chcete vytvořit zásadu, vyberte v rozevírací nabídce **Vytvořit novou**. Rozevírací nabídka také nabízí možnost přepnout čas pořizování snímků na 19:00. Pokyny k definování zásad zálohování naleznete v tématu [Definování zásad zálohování](backup-azure-vms-first-look-arm.md#defining-a-backup-policy). Po kliknutí na **OK** je zásada zálohování přidružená k trezoru.

    Dále vyberte virtuální počítače, které se mají přidružit k trezoru.
6. Vyberte virtuální počítače, které se mají přidružit k určené zásadě a klikněte na **Vybrat**.

    ![Výběr úlohy](./media/backup-azure-vms-first-look-arm/select-vms-to-backup-new.png)

    Pokud nevidíte požadovaný virtuální počítač, zkontrolujte, zda je ve stejné oblasti Azure jako trezor Recovery Services.
7. Teď, když jste definovali všechna nastavení trezoru, klikněte v okně Zálohování na **Povolit zálohování** v dolní části stránky. To nasadí zásadu pro trezor a virtuální počítače.

    ![Povolení zálohování](./media/backup-azure-vms-first-look-arm/enable-backup-settings-new.png)

## <a name="initial-backup"></a>Prvotní zálohování
Nasazení zásady zálohování na virtuální počítač neznamená, že jsou data zálohovaná. Ve výchozím nastavení je prvním plánovaným zálohováním (definovaným v zásadě zálohování) prvotní zálohování. Než proběhne prvotní zálohování, bude Stav poslední zálohy v okně **Úlohy zálohování** ukazovat **Upozornění (nedokončené prvotní zálohování)**.

![Zálohování čeká na zpracování](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Pokud nemá prvotní zálohování brzy začít, doporučujeme spustit **Zálohovat nyní**.

Chcete-li spustit **Zálohovat nyní**:

1. V řídícím panelu trezoru na dlaždici **Zálohování** klikněte na **Virtuální počítače Azure**. <br/>
    ![Ikona nastavení](./media/backup-azure-vms-first-look-arm/rs-vault-in-dashboard-backup-vms.png)

    Otevře se okno **Zálohování položek**
2. V okně **Zálohování položek** klikněte pravým tlačítkem myši na trezor, který chcete zálohovat, a klikněte na **Zálohovat nyní**.

    ![Ikona nastavení](./media/backup-azure-vms-first-look-arm/back-up-now.png)

    Aktivuje se úloha zálohování. <br/>

    ![Aktivovaná úloha zálohování.](./media/backup-azure-vms-first-look-arm/backup-triggered.png)
3. Chcete-li zobrazit, zda bylo prvotní zálohování dokončené, klikněte v řídícím panelu trezoru na dlaždici **Úlohy zálohování** na **Virtuální počítače Azure**.

    ![Dlaždice Úlohy zálohování](./media/backup-azure-vms-first-look-arm/open-backup-jobs.png)

    Otevře se okno Úlohy zálohování
4. V okně Úlohy zálohování vidíte stav všech úloh.

    ![Dlaždice Úlohy zálohování](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view.png)

   > [!NOTE]
   > Jako součást operace zálohování vydá služba Azure Backup rozšířením zálohování v každém virtuálním počítači příkaz k vyprázdnění všech zápisů a pořízení konzistentního snímku.
   >
   >

    Po dokončení úlohy zálohování se stav změní na *Dokončeno*.

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>Instalace agenta virtuálního počítače na virtuální počítač
Tyto informace jsou poskytnuté pro případ potřeby. Pro fungování rozšíření Backup musí být na virtuálním počítači Azure nainstalovaný agent virtuálního počítače Azure. Pokud byl váš virtuální počítač vytvořen z galerie Azure, je na něm agent virtuálního počítače již nainstalován. Virtuální počítače migrované z místních datových center nebudou mít agenta virtuálního počítače nainstalovaného. V takovém případě je potřeba agenta virtuálního počítače nainstalovat. Pokud máte problémy se zálohováním virtuálního počítače Azure, zkontrolujte, zda je agent virtuálního počítače na virtuálním počítači správně nainstalovaný (viz následující tabulka). Pokud vytváříte vlastní virtuální počítač, před jeho zřízením [se ujistěte, že je zaškrtávací políčko **Nainstalovat agenta virtuálního počítače** zaškrtnuté](../virtual-machines/virtual-machines-windows-classic-agents-and-extensions.md).

Další informace o [Agentu virtuálního počítače](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) a [jak ho nainstalovat](../virtual-machines/virtual-machines-windows-classic-manage-extensions.md).

Následující tabulka poskytuje další informace o agentu virtuálního počítače pro virtuální počítače s Windows nebo Linuxem.

| **Operace** | **Windows** | **Linux** |
| --- | --- | --- |
| Instalace agenta virtuálního počítače |<li>Stáhněte si a nainstalujte [MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). K dokončení instalace je potřeba oprávnění správce. <li>[Aktualizujte vlastnost virtuálního počítače](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx), aby indikovala, že je agent nainstalovaný. |<li> Nainstalujte nejnovějšího [agenta systému Linux](https://github.com/Azure/WALinuxAgent) z Githubu. K dokončení instalace je potřeba oprávnění správce. <li> [Aktualizujte vlastnost virtuálního počítače](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx), aby indikovala, že je agent nainstalovaný. |
| Aktualizace agenta virtuálního počítače |Aktualizace agenta virtuálního počítače je stejně jednoduchá, jako přeinstalace [binárních souborů agenta virtuálního počítače](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br>Ujistěte se, že během aktualizace agenta virtuálního počítače neběží žádná operace zálohování. |Postupujte podle pokynů v tématu [Aktualizace agenta virtuálního počítače s Linuxem](../virtual-machines/virtual-machines-linux-update-agent.md). <br>Ujistěte se, že během aktualizace agenta virtuálního počítače neběží žádná operace zálohování. |
| Ověření instalace agenta virtuálního počítače |<li>Ve virtuálním počítači Azure přejděte do složky *C:\WindowsAzure\Packages*. <li>Měl by být přítomný soubor WaAppAgent.exe.<li> Pravým tlačítkem myši klikněte na soubor, přejděte na **Vlastnosti** a poté vyberte kartu **Podrobnosti**. Pole Verze produktu by mělo být 2.6.1198.718 nebo vyšší. |Není dostupné. |

### <a name="backup-extension"></a>Rozšíření zálohování
Když je agent virtuálního počítače nainstalovaný na virtuálním počítači, služba Azure Backup nainstaluje do agenta virtuálního počítače rozšíření zálohování. Služba Azure Backup bezproblémově upgraduje a opravuje rozšíření zálohování bez dalšího zásahu uživatele.

Rozšíření zálohování je službou Backup nainstalováno, jakmile je virtuální počítač spuštěný. Spuštěný virtuální počítač poskytuje největší šanci získání bodu obnovení, který je konzistentní v rámci aplikace. Služba Azure Backup nicméně bude pokračovat v zálohování virtuálního počítače, i když je vypnutý a rozšíření nebylo možné nainstalovat. To se označuje jako Virtuální počítač v režimu offline. V takovém případě bude bod obnovení *konzistentní pro případ chyby*.

## <a name="troubleshooting-information"></a>Informace o řešení potíží
Pokud máte problémy s plněním některých úkolů v tomto článku, obraťte se na [Pokyny při řešení potíží](backup-azure-vms-troubleshoot.md).

## <a name="pricing"></a>Ceny
Služba zálohování virtuálních počítačů Azure Backup se účtuje podle počtu chráněných instancí. Přečtěte si další informace o [cenách služby Backup](https://azure.microsoft.com/pricing/details/backup/).

## <a name="questions"></a>Máte dotazy?
Máte-li nějaké dotazy nebo pokud víte o funkci, kterou byste uvítali, [odešlete nám svůj názor](http://aka.ms/azurebackup_feedback).



<!--HONumber=Nov16_HO2-->


