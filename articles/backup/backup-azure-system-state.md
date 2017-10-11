---
title: "Zálohování stavu systému Windows Azure | Microsoft Docs"
description: "Postup zálohování stavu systému Windows Server nebo Windows počítačů do Azure."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: carmonm
editor: 
keywords: "postup zálohování; zálohování souborů a složek"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: saurse;markgal
ms.openlocfilehash: 6fbd96935f444d8b0c6d068ebd0d28e612f19816
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="back-up-windows-system-state-in-resource-manager-deployment"></a>Zálohování stavu systému Windows v nasazení Resource Manager
Tento článek vysvětluje, jak zálohovat stav systému Windows Server do Azure. Tento kurz vás má provést základy.

Chcete-li se dozvědět více o Azure Backup, přečtěte si tento [přehled](backup-introduction-to-azure-backup.md).

Pokud předplatné Azure nemáte, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/), který vám umožní přístup ke službám Azure.

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru služby Recovery Services
Chcete-li zálohovat svoje soubory a složky, musíte vytvořit trezor Služeb zotavení v oblasti, kde chcete data ukládat. Musíte také určit způsob replikace úložiště.

### <a name="to-create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení
1. Pokud jste to ještě neudělali, přihlaste se k [portálu Azure](https://portal.azure.com/) pomocí svého předplatného Azure.
2. V nabídce centra klikněte na **Procházet**, v seznamu prostředků zadejte **Recovery Services** a klikněte na **Trezory služby Recovery Services**.

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Pokud předplatné obsahuje trezory služby Recovery Services, jsou tyto trezory uvedené v seznamu.
3. V nabídce **Trezory Recovery Services** klikněte na **Přidat**.

    ![Vytvoření trezoru Recovery Services – krok 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    Otevře se okno trezoru Recovery Services s výzvou k vyplnění polí **Název**, **Předplatné**, **Skupina prostředků** a **Oblast**.

    ![Vytvoření trezoru Recovery Services – krok 3](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. Jako **Název** zadejte popisný název pro identifikaci trezoru. Název musí být jedinečný v rámci předplatného Azure. Zadejte název v rozsahu 2 až 50 znaků. Musí začínat písmenem a může obsahovat pouze písmena, číslice a pomlčky.

5. V části **Předplatné** z rozevírací nabídky vyberte předplatné Azure. Pokud používáte jenom jedno předplatné, zobrazí se toto předplatné a můžete přejít k dalšímu kroku. Pokud si nejste jisti, jaké předplatné použít, použijte výchozí (nebo navrhované) předplatné. Více možností je dostupných, jen pokud je váš účet organizace přidružený k více předplatným Azure.

6. V části **Skupina prostředků**:

    * vyberte **Vytvořit novou**, pokud chcete vytvořit skupinu prostředků.
    Nebo
    * vyberte **Použít existující** a kliknutím na rozevírací nabídku zobrazte seznam dostupných skupin prostředků.

  Kompletní informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).

7. Klikněte na **Oblast** a vyberte zeměpisnou oblast trezoru. Tato volba určuje geografickou oblast, kam jsou zasílaná vaše zálohovaná data.

8. V dolní části okna trezoru služby Recovery Services klikněte na **Vytvořit**.

    Vytvoření trezoru služby Recovery Services může trvat několik minut. Sledujte oznámení o stavu v pravé horní části portálu. Když je trezor vytvořený, zobrazí se v seznamu trezorů Služeb zotavení. Pokud se trezor nezobrazí ani po několika minutách, klikněte na **Obnovit**.

    ![Kliknutí na tlačítko Obnovit](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    Jakmile se trezor zobrazí v seznamu trezorů služby Recovery Services, jste připraveni nastavit redundanci úložiště.

### <a name="set-storage-redundancy-for-the-vault"></a>Nastavení redundance úložiště pro trezor
Při vytváření trezoru služby Recovery Services se ujistěte, že je redundance úložiště nakonfigurována požadovaným způsobem.

1. V okně **Trezory služby Recovery Services** klikněte na nový trezor.

    ![Výběr nového trezoru ze seznamu trezorů služby Recovery Services](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    Po výběru trezoru se okno **Trezory služby Recovery Services** zúží a otevře se okno Nastavení (*s názvem trezoru v horní části*) a okno s podrobnostmi o trezoru.

    ![Zobrazení konfigurace úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. V okně Nastavení nového trezoru pomocí vertikálního posuvníku přejděte dolů do části Správa a klikněte na **Infrastruktura zálohování**.
    Otevře se okno Infrastruktura zálohování.
3. V okně Infrastruktura zálohování klikněte na **Konfigurace zálohování**. Otevře se okno **Konfigurace zálohování**.

    ![Nastavení konfigurace úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. Zvolte vhodnou možnost replikace pro svůj trezor.

    ![volby konfigurace úložiště](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště. Pokud používáte Azure jako primární koncový bod úložiště záloh, pokračujte v používání **geograficky redundantního** úložiště. Pokud Azure nepoužíváte jako primární koncový bod úložiště záloh, vyberte **Místně redundantní** – snížíte tím náklady na úložiště Azure. Další informace o možnostech [geograficky redundantního](../storage/common/storage-redundancy.md#geo-redundant-storage) a [místně redundantního](../storage/common/storage-redundancy.md#locally-redundant-storage) úložiště najdete v tomto [přehledu redundance úložiště](../storage/common/storage-redundancy.md).

Teď, když jste vytvořili trezor, můžete ho nakonfigurujte pro zálohování stavu systému Windows.

## <a name="configure-the-vault"></a>Konfigurace trezoru
1. V okně trezoru služby Recovery Services (pro trezor, který jste právě vytvořili) klikněte v části Začínáme na **Zálohovat** a potom v okně **Začínáme se zálohováním** vyberte **Cíl zálohování**.

    ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    Otevře se okno **Cíl zálohování**.

    ![Otevřete okno cíle zálohování](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. V rozevírací nabídce **Kde běží vaše úlohy?** vyberte **Místní**.

    Možnost **Místní** jste vybrali proto, že počítačem s Windows Serverem nebo Windows je fyzický počítač, který není v Azure.

3. Z **co chcete zálohovat?** nabídce vyberte možnost **stav systému**a klikněte na tlačítko **OK**.

    ![Konfigurace souborů a složek](./media/backup-azure-system-state/backup-goal-system-state.png)

    Po kliknutí na OK se vedle položky **Cíl zálohování** zobrazí zaškrtnutí a otevře se okno **Připravit infrastrukturu**.

    ![Cíl zálohování je nakonfigurovaný, teď se připraví infrastruktura](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. V okně **Připravit infrastrukturu** klikněte na **Stáhnout agenta pro Windows Server nebo klienta Windows**.

    ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    Pokud používáte Windows Server Essential, vyberte ke stažení agenta pro Windows Server Essential. Místní nabídka zobrazí výzvu ke spuštění nebo uložení MARSAgentInstaller.exe.

    ![Dialogové okno MARSAgentInstaller](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. V místní nabídce stahování klikněte na **Uložit**.

    Ve výchozím nastavení se soubor **MARSagentinstaller.exe** uloží do složky Stažené soubory. Po dokončení instalačního programu se zobrazí automaticky otevírané okno s dotazem, jestli chcete spustit instalační program nebo otevřít složku.

    ![Příprava infrastruktury](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    Agenta ještě nemusíte instalovat. Agenta můžete nainstalovat po stažení přihlašovacích údajů trezoru.

6. V okně **Připravit infrastrukturu** klikněte na **Stáhnout**.

    ![stažení přihlašovacích údajů trezoru](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    Přihlašovací údaje trezoru se stáhnou do složky Stažené soubory. Po dokončení stahování přihlašovacích údajů trezoru se zobrazí automaticky otevírané okno s dotazem, jestli chcete přihlašovací údaje otevřít nebo uložit. Klikněte na **Uložit**. Pokud omylem kliknete **Otevřít**, nechte dialogové okno, které se pokusí otevřít přihlašovací údaje trezoru, zobrazit chybu. Přihlašovací údaje trezoru nejde otevřít. Přejděte k dalšímu kroku. Přihlašovací údaje trezoru jsou ve složce Stažené soubory.   

    ![dokončené stahování přihlašovacích údajů trezoru](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-the-agent"></a>Instalace a registrace agenta

> [!NOTE]
> Povolení zálohování prostřednictvím webu Azure Portal ještě není dostupné. Agent služeb zotavení Microsoft Azure pomocí zálohování stavu systému Windows Server.
>

1. Ve složce Stažené soubory (nebo ve složce, kterou jste vybrali pro stahování) vyhledejte soubor **MARSagentinstaller.exe** a dvakrát na něj klikněte.

    Instalační program v průběhu extrakce, instalace a registrace agenta Recovery Services zobrazuje řadu zpráv.

    ![spuštění přihlašovacích údajů instalačního programu agenta Recovery Services](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. Dokončete Průvodce instalací agenta Služeb zotavení Microsoft Azure. K dokončení průvodce budete muset:

   * Vybrat umístění instalace a složky mezipaměti.
   * Zadat informace o serveru proxy, pokud pro připojování k internetu používáte server proxy.
   * Zadat svoje uživatelské jméno a heslo, pokud používáte ověřený server proxy.
   * Zadat stažené přihlašovací údaje trezoru.
   * Uložit šifrovací heslo na bezpečné místo.

     > [!NOTE]
     > Pokud heslo ztratíte nebo zapomenete, Microsoft vám nemůže pomoci obnovit zálohovaná data. Uložte soubor na bezpečné místo. Je požadováno pro obnovení zálohy.
     >
     >

Agent je nyní nainstalovaný a váš počítač je registrovaný k trezoru. Jste připraveni nakonfigurovat a naplánovat zálohování.

## <a name="back-up-windows-server-system-state-preview"></a>Zálohování stavu systému Windows Server (Preview)
Prvotní záloha zahrnuje tři úkoly:

* Povolit zálohování stavu systému pomocí agenta Azure Backup
* Naplánování zálohování
* První zálohování souborů a složek

K dokončení prvotního zálohování použijte agenta Microsoft Azure Recovery Services.

### <a name="to-enable-system-state-backup-using-the-azure-backup-agent"></a>K povolení zálohování stavu systému pomocí agenta Azure Backup

1. V relaci prostředí PowerShell pomocí následujícího příkazu zastavte modul zálohování Azure.

  ```
  PS C:\> Net stop obengine
  ```

2. Otevřete registr systému Windows.

  ```
  PS C:\> regedit.exe
  ```

3. Přidejte následující klíč registru se zadanou hodnotou DWord.

  | Cesta k registru | Klíč registru | Přidejte hodnotu DWord |
  |---------------|--------------|-------------|
  | HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider | TurnOffSSBFeature | 2 |

4. Restartujte modul zálohování spuštěním následujícího příkazu v příkazovém řádku se zvýšenými oprávněními.

  ```
  PS C:\> Net start obengine
  ```

### <a name="to-schedule-the-backup-job"></a>Naplánování úlohy zálohování

1. Otevřete agenta Služeb zotavení Microsoft Azure. Najdete ho vyhledáním **Microsoft Azure Backup** ve svém počítači.

    ![Spuštění agenta Služeb zotavení Azure](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)

2. V agentu Služeb zotavení klikněte na **Naplánovat zálohování**.

    ![Naplánování zálohování Windows Serveru](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. Na stránce Začínáme v Průvodci plánování zálohování klikněte na **Další**.

4. Na stránce Výběr položek k zálohování klikněte na **Přidat**.

5. Vyberte **stav systému** a pak klikněte na **OK**.

6. Klikněte na **Další**.

7. Plán zálohování stavu systému a jejich uchovávání se automaticky nastaví na zálohování každou neděli ve 21:00:00 místního času a dobu uchování je nastaven na 60 dnů.

   > [!NOTE]
   > Zálohování a uchovávání zásad stavu systému je automaticky nakonfigurovaná. Pokud zálohujete soubory a složky kromě stavu systému Windows Server, zadejte jenom zálohování a uchovávání zásady pro soubor zálohy z průvodce. 
   >

8. Na stránce Potvrzení zkontrolujte informace a poté klikněte na **Dokončit**.

9. Až průvodce dokončí vytváření plánu zálohování, klikněte na **Zavřít**.

### <a name="to-back-up-windows-server-system-state-for-the-first-time"></a>Zálohování stavu systému Windows Server poprvé

1. Ujistěte se, že neexistují žádné čekající aktualizace pro systém Windows Server, které vyžadují restart počítače.

2. Chcete-li dokončit prvotní synchronizaci přes síť, v agentu Služeb zotavení klikněte na **Zálohovat nyní**.

    ![Zálohovat nyní ve Windows Serveru](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

3. Na stránce Potvrzení zkontrolujte nastavení, které Průvodce Zálohování nyní použije k zálohování počítače. Poté klikněte na **Zálohovat**.

4. Průvodce zavřete kliknutím na **Zavřít**. Pokud průvodce zavřete před dokončením procesu zálohování, průvodce zůstane spuštěný na pozadí.

5. Pokud zálohujete soubory a složky na serveru, kromě stav systému Windows Server, Průvodce zálohování nyní pouze zálohovat soubory. K provedení zálohování do ad hoc stavu systému, použijte následující příkaz Powershellu:

    ```
    PS C:\> Start-OBSystemStateBackup
    ```

  Po dokončení prvotní zálohy se v konzole Zálohování zobrazí stav **Úloha byla dokončena**.

  ![Dokončení IR](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

Následující otázky a odpovědi poskytovat doplňující informace.

### <a name="what-is-the-staging-volume"></a>Co je pracovní svazek?

Svazek pracovní představuje zprostředkující umístění, kde nativně dostupné, Windows Server Backup zpracuje zálohování stavu systému. Azure Backup agent pak komprimuje a šifruje zprostředkující záloha a odešle ji přes zabezpečený protokol HTTPS nakonfigurovaná trezoru služeb zotavení. **Důrazně doporučujeme že vytvořit pracovní svazek na svazku bez operačního systému Windows. Pokud zjistíte problémy s zálohování stavu systému, kontrola umístění svazku pracovní je prvním krokem řešení potíží.** 

### <a name="how-can-i-change-the-staging-volume-path-specified-in-the-azure-backup-agent"></a>Jak je možné změnit cestu svazku pracovní zadaný v agenta Azure Backup?

Pracovní svazek umístěn ve složce mezipaměti ve výchozím nastavení. 

1. Chcete-li změnit toto umístění, použijte následující příkaz (v příkazovém řádku se zvýšenými):
  ```
  PS C:\> Net stop obengine
  ```

2. Aktualizujte následující položky registru s cestou do nové složky pracovní svazku.

  |Cesta k registru|Klíč registru|Hodnota|
  |-------------|------------|-----|
  |HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider | SSBStagingPath | nový pracovní umístění svazku |

Cesta pracovní velká a malá písmena a musí být přesně stejnou malá a velká písmena jako co na serveru existuje. 

3. Jakmile změníte cestu svazku pracovní, restartujte modul zálohování:
  ```
  PS C:\> Net start obengine
  ```
4. Chcete-li vyzvednutí změněné cestu, otevřete agenta služeb zotavení Microsoft Azure a spustit ad hoc zálohování stavu systému.

### <a name="why-is-the-system-state-default-retention-set-to-60-days"></a>Proč je uchování výchozí stavu systému nastaven na 60 dnů?

Životnost zálohu stavu systému je stejné jako "životnosti objektů označených jako neplatné" nastavení pro roli systému Windows Server Active Directory. Výchozí hodnota pro položku životnosti objektů označených jako neplatné je 60 dnů. Tuto hodnotu můžete nastavit u objektu konfigurace služby Directory (NTDS).

### <a name="how-do-i-change-the-default-backup-and-retention-policy-for-system-state"></a>Jak změnit výchozí zálohování a zásadami uchovávání informací pro stav systému?

Chcete-li změnit výchozí zálohování a zásadami uchovávání informací pro stav systému:
1. Zastavte modul zálohování. Spusťte následující příkaz z příkazového řádku se zvýšenými oprávněními.

  ```
  PS C:\> Net stop obengine
  ```

2. Přidat nebo aktualizovat následující položky klíče registru HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider.

  |Název registru|Popis|Hodnota|
  |-------------|-----------|-----|
  |SSBScheduleTime|Slouží ke konfiguraci čas zálohování. Výchozí hodnota je 21: 00 místního času.|DWord: Formátu hh: mm (decimální): pro příklad. 2130 21:30:00 místního času|
  |SSBScheduleDays|Slouží ke konfiguraci dní, kdy musí být prováděno zálohování stavu systému v určeném čase. Jednotlivé číslic Zadejte dny v týdnu. představuje neděli 0, 1 je pondělí, a tak dále. Výchozí den pro zálohování je neděli.|DWord: dny v týdnu, chcete-li spustit zálohování (decimální): Příklad 1 230 plány zálohování v pondělí, úterý, středu a neděle.|
  |SSBRetentionDays|Slouží ke konfiguraci počet dní do uchovávat zálohu. Výchozí hodnota je 60. Maximální povolená hodnota je 180.|DWord: Uchovávat zálohu (decimální): dnů.|

3. Použijte následující příkaz k restartování stroje zálohování.
    ```
    PS C:\> Net start obengine
    ```

4. Otevřete agenta služeb zotavení Microsoft.

5. Klikněte na tlačítko **plánem zálohování** a pak klikněte na **Další** až se změny projeví.

6. Klikněte na tlačítko **Dokončit** aby se změny projevily.


## <a name="questions"></a>Máte dotazy?
Máte-li nějaké dotazy nebo pokud víte o funkci, kterou byste uvítali, [odešlete nám svůj názor](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Další kroky
* Zdroj dalších informací o [zálohování počítačů se systémem Windows](backup-configure-vault.md).
* Teď, když jste zálohovali své soubory a složky, můžete [spravovat svoje trezory a servery](backup-azure-manage-windows-server.md).
* Potřebujete-li obnovit zálohu, použijte tento článek k [obnovení souborů na počítač se systémem Windows](backup-azure-restore-windows-server.md).
