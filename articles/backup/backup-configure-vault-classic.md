---
title: "Zálohování serveru Windows nebo pracovní stanice do Azure (klasického modelu) | Microsoft Docs"
description: "Zálohování Windows servery nebo klienty do trezoru záloh v Azure. Projděte základy pro ochranu souborů a složek do trezoru zálohování pomocí agenta Azure Backup."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "Trezor záloh; zálohování na Windows server. zálohování systému windows."
ms.assetid: 3b543bfd-8978-4f11-816a-0498fe14a8ba
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: cwatson
ms.openlocfilehash: 16d4581b94501203707099692b37fd9a04a213f1
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/15/2017
---
# <a name="back-up-a-windows-server-or-workstation-to-azure-using-the-classic-portal"></a>Zálohování Windows serveru nebo pracovní stanice pomocí klasického portálu Azure
> [!div class="op_single_selector"]
> * [Portál Classic](backup-configure-vault-classic.md)
> * [Azure Portal](backup-configure-vault.md)
>
>

Tento článek popisuje postupy, které je nutné postupovat při přípravě svého prostředí a zálohování Windows serveru (nebo pracovní stanice) do Azure. Také vysvětluje aspekty pro nasazení řešení pro zálohování. Pokud vás zajímá při prvním pokusu o zálohování Azure, tento článek vás rychle provede procesem.

Azure má dva různé modely nasazení pro vytváření a práci s prostředky: Resource Manager a Klasický model. Tento článek se věnuje použití klasického modelu nasazení. Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.

## <a name="before-you-start"></a>Než začnete
K zálohování serveru nebo klienta do Azure, potřebujete účet Azure. Pokud nemáte, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) si během několika minut.

## <a name="create-a-backup-vault"></a>Vytvoření trezoru záloh
Chcete-li zálohovat soubory a složky ze serveru nebo klienta, vytvořte trezor záloh v zeměpisnou oblast, ve které chcete data uložit.

> [!IMPORTANT]
> Od března 2017 již nelze k vytvoření trezorů služby Backup použít portál Classic.
>
> Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services. Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.<br/> Po **30. listopadu 2017**, nebudete moci použít PowerShell k vytvoření trezory Backup. <br/> **2017 30 listopadu**:
>- Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.
>- Nebudete mít přístup k datům záloh na portálu Classic. Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.
>


## <a name="download-the-vault-credential-file"></a>Stáhněte si soubor s přihlašovacími údaji trezoru
Na místním počítači musí být k ověření pomocí úložiště záloh předtím, než můžete zálohovat data do Azure. Ověřování je dosaženo pomocí *přihlašovací údaje trezoru*. Soubor s přihlašovacími údaji trezoru je stažen prostřednictvím zabezpečeného kanálu z klasického portálu. Privátní klíč certifikátu není zachována v portálu nebo službu.

### <a name="to-download-the-vault-credential-file-to-a-local-machine"></a>Chcete-li stáhnout soubor pověření pro úložiště na místním počítači
1. V levém navigačním podokně klikněte na tlačítko **služeb zotavení**a potom vyberte úložiště záloh, které jste vytvořili.

    ![Dokončení IR](./media/backup-configure-vault-classic/rs-left-nav.png)
2. Na stránce Rychlý Start klikněte na **přihlašovací údaje trezoru Stáhnout**.

   Portálu classic vygeneruje přihlašovací údaje úložiště pomocí kombinace název trezoru a aktuální datum. Soubor s přihlašovacími údaji se používají pouze během pracovního postupu registrace a vyprší po 48 hodin.

   Soubor s přihlašovacími údaji trezoru můžete stáhnout z portálu.
3. Klikněte na tlačítko **Uložit** stáhnout soubor s přihlašovacími údaji trezoru ke složce pro stahování místního účtu. Můžete také vybrat **uložit jako** z **Uložit** nabídky a zadejte umístění pro soubor s přihlašovacími údaji trezoru.

   > [!NOTE]
   > Ujistěte se, že soubor s přihlašovacími údaji trezoru je uložen v umístění, které jsou přístupné z počítače. Pokud je uložen v souboru bloku zpráv sdílenou složku nebo server, ověřte, zda máte oprávnění k přístupu.
   >
   >

## <a name="download-install-and-register-the-backup-agent"></a>Stažení, instalace a registrace agenta zálohování
Po vytvoření úložiště záloh a stáhnout soubor s přihlašovacími údaji trezoru, musí být agent nainstalován na všech počítačích s Windows.

### <a name="to-download-install-and-register-the-agent"></a>Stažení, instalace a registrace agenta
1. Klikněte na tlačítko **služeb zotavení**a potom vyberte úložiště záloh, které chcete zaregistrovat serveru.
2. Na stránce Rychlý Start klikněte na agenta **agenta pro Windows Server nebo klienta System Center Data Protection Manager nebo Windows**. Potom klikněte na **Uložit**.

    ![Uložit agenta](./media/backup-configure-vault-classic/agent.png)
3. Jakmile soubor MARSagentinstaller.exe stáhl, klikněte na tlačítko **spustit** (nebo dvakrát kliknout na **MARSAgentInstaller.exe** z místa uložení).
4. Vyberte instalační složku a složku mezipaměti, které jsou požadovány pro agenta a pak klikněte na tlačítko **Další**. Umístění mezipaměti, které zadáte musí mít volné místo odpovídající nejméně 5 procent zálohovaná data.
5. Můžete se připojit k Internetu výchozí nastavení proxy serveru.             Pokud používáte proxy server pro připojení k Internetu, na stránce konfigurace proxy serveru, vyberte **používat vlastní proxy server nastavení** zaškrtněte políčko a potom zadejte podrobnosti o proxy serveru. Pokud používáte ověřený server proxy, zadejte uživatelské jméno a heslo podrobnosti a pak klikněte na tlačítko **Další**.
6. Klikněte na tlačítko **nainstalovat** k zahájení instalace agenta. Agent zálohování nainstaluje rozhraní .NET Framework 4.5 a prostředí Windows PowerShell (pokud ještě nejsou nainstalované) pro dokončení instalace.
7. Po instalaci agenta, klikněte na tlačítko **přejít k registraci** pokračujte s pracovním postupem.
8. Na stránce úložiště identifikační vyhledejte a vyberte soubor s přihlašovacími údaji trezoru, který jste dříve stáhli.

    Soubor s přihlašovacími údaji trezoru je platný pouze 48 hodin po jeho stažení z portálu. Pokud dojde k chybě na této stránce (například "přihlašovací údaje trezoru, které souboru vypršela platnost."), přihlaste se k portálu a znovu stáhnout soubor s přihlašovacími údaji trezoru.

    Ujistěte se, že soubor s přihlašovacími údaji trezoru je k dispozici v umístění, které můžete získat přístup k instalaci aplikací. Pokud narazíte na chyby týkající se přístupu, zkopírujte soubor s přihlašovacími údaji trezoru do dočasného umístění na stejném počítači a operaci opakujte.

    Pokud dojde k chybě přihlašovacích údajů trezoru jako je například "Neplatné přihlašovací údaje úložiště", soubor je poškozený nebo nemá mít poslední přihlašovací údaje související se službou obnovení. Opakujte operaci po stažení nový soubor s přihlašovacími údaji trezoru z portálu. Této chybě může také dojít, pokud uživatel klikne **přihlašovací údaje trezoru Stáhnout** možnost několikrát za sebou rychlé. V takovém případě je platný pouze poslední soubor přihlašovacích údajů trezoru.
9. Na stránce nastavení šifrování můžete buď vygenerovat přístupové heslo nebo zadat přístupové heslo (s minimálně 16 znaků). Nezapomeňte uložit heslo v zabezpečeném umístění.
10. Klikněte na **Dokončit**. Průvodce registrací serveru zaregistruje pomocí zálohování serveru.

    > [!WARNING]
    > Pokud ztratíte nebo zapomenete heslo, Microsoft vám nemůže pomoci obnovit zálohovaná data. Šifrovací přístupové heslo, které vlastníte, a Microsoft nemá přehled heslo, které používáte. Uložte soubor na bezpečné místo, protože se bude vyžadovat během operace obnovení.
    >
    >

11. Po nastavení šifrovací klíč, ponechte **spusťte Microsoft agenta služeb zotavení Azure** zaškrtnuté políčko a pak klikněte na **Zavřít**.

## <a name="complete-the-initial-backup"></a>Dokončení prvotního zálohy
Prvotní záloha zahrnuje dvě klíčové úlohy:

* Vytváření plánu zálohování
* První zálohování souborů a složek

Po dokončení prvotní zálohování zásady zálohování vytvoří body zálohy, které můžete použít, pokud potřebujete obnovit data. Zásady zálohování tomu podle plánu, který určíte.

### <a name="to-schedule-the-backup"></a>Naplánování zálohování
1. Otevřete Microsoft Azure Backup agent. (Otevře se automaticky Pokud jste nechali **spusťte Microsoft agenta služeb zotavení Azure** zaškrtnutým políčkem při zavření průvodce registrací serveru.) Najdete ho vyhledáním **Microsoft Azure Backup** ve svém počítači.

    ![Spuštění agenta Azure Backup](./media/backup-configure-vault-classic/snap-in-search.png)
2. V agentu Backup, klikněte na tlačítko **plánem zálohování**.

    ![Naplánování zálohování Windows Serveru](./media/backup-configure-vault-classic/schedule-backup-close.png)
3. Na stránce Začínáme v Průvodci plánování zálohování klikněte na **Další**.
4. Na stránce Výběr položek k zálohování klikněte na **Přidat**.
5. Vyberte soubory a složky, které chcete zálohovat, a poté klikněte na **V pořádku**.
6. Klikněte na **Další**.
7. Na stránce **Zadání plánu zálohování** zadejte **plán zálohování** a klikněte na **Další**.

    Můžete naplánovat denní (probíhající maximálně třikrát za den) nebo týdenní zálohování.

    ![Položky k zálohování z Windows Serveru](./media/backup-configure-vault-classic/specify-backup-schedule-close.png)

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

### <a name="enable-network-throttling-optional"></a>Zapnout omezování sítě (volitelné)
Agent zálohování poskytuje, omezení šířky pásma sítě. Omezení využití šířky pásma sítě při přenosu dat ovládací prvky. Tento ovládací prvek může být užitečné, pokud potřebujete zálohovat data v pracovní době, ale nechcete, aby proces zálohování narušoval ostatní internetový provoz. Omezení se vztahuje na zálohování a obnovení aktivity.

**Chcete-li povolit omezení šířky pásma sítě**

1. V agentu Backup, klikněte na tlačítko **změnit vlastnosti**.

    ![Změnit vlastnosti](./media/backup-configure-vault-classic/change-properties.png)
2. Na **omezování** vyberte **povolit využití šířky pásma Internetu u operací zálohování omezení** zaškrtávací políčko.

    ![Omezení šířky pásma sítě](./media/backup-configure-vault-classic/throttling-dialog.png)
3. Po povolení omezení šířky pásma, zadejte povolenou šířky pásma pro přenos zálohovaných dat během **pracovní hodiny** a **nepracovní hodiny**.

    Hodnoty šířky pásma začít ve 512 kilobitů za sekundu (Kbps) a můžete přejít až 1,023 MB za sekundu (MBps). Můžete také určit spuštění a dokončení pro **pracovní hodiny**, a které dny v týdnu, jsou považovány pracovní dny. Hodiny mimo určené práce, kterou jsou považovány za hodiny mimo pracovní hodiny.
4. Klikněte na **OK**.

### <a name="to-back-up-now"></a>Chcete-li zálohovat nyní
1. V agentu Backup, klikněte na tlačítko **zálohovat nyní** dokončit prvotní synchronizaci přes síť.

    ![Zálohovat nyní ve Windows Serveru](./media/backup-configure-vault-classic/backup-now.png)
2. Na stránce Potvrzení zkontrolujte nastavení, které Průvodce Zálohování nyní použije k zálohování počítače. Poté klikněte na **Zálohovat**.
3. Průvodce zavřete kliknutím na **Zavřít**. Pokud to uděláte před dokončením procesu zálohování, průvodce zůstane spuštěný v pozadí.

Po dokončení prvotní zálohy se v konzole Zálohování zobrazí stav **Úloha byla dokončena**.

![Dokončení IR](./media/backup-configure-vault-classic/ircomplete.png)

## <a name="next-steps"></a>Další kroky
* Zaregistrujte si [bezplatný účet Azure](https://azure.microsoft.com/free/).

Další informace o zálohování virtuálních počítačů nebo dalších úloh najdete v tématu:

* [Zálohování virtuálních počítačů IaaS](backup-azure-vms-prepare.md)
* [Zálohování úloh do Azure s Microsoft Azure Backup Server](backup-azure-microsoft-azure-backup.md)
* [Zálohování úloh do Azure s aplikací DPM](backup-azure-dpm-introduction.md)
