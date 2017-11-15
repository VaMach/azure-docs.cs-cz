---
title: " Odstranit trezor služeb zotavení v Azure | Microsoft Docs "
description: "Postup odstranění trezoru služby Azure Backup a služeb zotavení. Trezor záloh je možné volat trezoru cloudu Azure nebo Azure recovery vault. Řešení potíží, když nelze odstranit úložiště záloh v portálu classic nebo portálu Azure."
services: service-name
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 5fa08157-2612-4020-bd90-f9e3c3bc1806
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/10/2017
ms.author: markgal;trinadhk
ms.openlocfilehash: b07b9e01a5a8d8a5189b130fb5a9baeef7a43f4f
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/11/2017
---
# <a name="delete-a-recovery-services-vault"></a>Odstranění trezoru služby Recovery Services
Služba Azure Backup má dva typy trezorů – trezor Backup a trezor Recovery Services. První byl trezor Backup. Po něm přišel trezor Recovery Services na podporu rozšířeného modelu nasazení Resource Manager. Z důvodu rozšířené možnosti a závislosti informace, které musí být uložen v úložišti odstranění trezoru zálohování nebo obnovení služby může být matoucí. Tento článek vysvětluje, jak odstranit trezorů v portálu classic a portálu Azure.  

| **Typ nasazení** | **Azure Portal** | **Název trezoru** |
| --- | --- | --- |
| Classic |Classic |Úložiště záloh |
| Resource Manager |Azure |Trezor služby Recovery Services |

> [!NOTE]
> Trezory Backup nemohou chránit řešení s modelem nasazení Resource Manager. Můžete však trezoru služeb zotavení k ochraně classically nasazené serverů a virtuálních počítačů.  
>

> [!IMPORTANT]
> Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services. Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.<br/> Po **30. listopadu 2017**, nebudete moci použít PowerShell k vytvoření trezory Backup. <br/> **2017 30 listopadu**:
>- Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.
>- Nebudete mít přístup k datům záloh na portálu Classic. Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.
>

V tomto článku používáme termín, úložiště, který bude odkazovat na obecný formulář úložiště záloh nebo trezor služeb zotavení. Když je to nutné k rozlišení mezi trezory používáme formální název, trezor záloh nebo trezor služeb zotavení.

## <a name="deleting-a-recovery-services-vault"></a>Odstranění trezoru služby Recovery Services
Odstranění trezoru služeb zotavení je jednoduchý proces - *zadaný Trezor neobsahuje žádné prostředky*. Než budete moct odstranit trezor služeb zotavení, musíte odebrat nebo odstranit všechny prostředky v trezoru. Pokud se pokusíte odstranit trezoru, který obsahuje prostředky, dojde k chybě jako na následujícím obrázku:

![Chyba při odstraňování trezoru](./media/backup-azure-delete-vault/vault-deletion-error.png) <br/>

Dokud jste vymazali prostředky z trezoru, kliknutím na tlačítko **opakujte** vytváří ke stejné chybě. Pokud jste se zablokuje na tato chybová zpráva, klikněte na tlačítko **zrušit** a pomocí následujících kroků můžete odstranit prostředky v trezoru.

### <a name="removing-the-items-from-a-vault-protecting-a-vm"></a>Odebírání položek z trezoru ochranu virtuálního počítače
Pokud již máte otevřete trezor služeb zotavení, pokračujte druhém kroku.

1. Otevřete portál Azure a na řídicím panelu otevřete úložišti, které chcete odstranit.

   Pokud nemáte trezor služeb zotavení připnuli k řídicímu panelu, v nabídce centra klikněte na tlačítko **více služeb** a v seznamu prostředků zadejte **služeb zotavení**. Seznam se průběžně filtruje podle zadávaného textu. Klikněte na tlačítko **trezory služeb zotavení**.

   ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   Zobrazí se seznam trezorů služeb zotavení. V seznamu vyberte trezor, které chcete odstranit.

   ![Vyberte možnost trezoru ze seznamu](./media/backup-azure-work-with-vaults/choose-vault-to-delete.png)
2. V zobrazení trezoru, podívejte se na **Essentials** podokně. Pokud chcete odstranit trezor, nesmí být žádné chráněné položky. Pokud se zobrazí číslo větší než nula, v části buď **zálohování položek** nebo **zálohování serverů pro správu**, musíte odebrat tyto položky před odstraněním trezoru.

    ![Podívejte se na chráněných položkách v podokně Essentials](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

    Virtuální počítače a soubory a složky jsou považovány za zálohování položek a jsou uvedeny v **zálohování položek** plochu podokna Essentials. DPM server je uveden v **serveru pro správu zálohování** plochu podokna Essentials. **Replikované položky** se týkají služby Azure Site Recovery.
3. Pokud chcete začít, odebrání chráněné položky z trezoru, zjistí položky v trezoru. V řídícím panelu trezoru klikněte na **nastavení**a potom klikněte na **zálohování položek** k otevření tohoto okna.

    ![Vyberte možnost trezoru ze seznamu](./media/backup-azure-delete-vault/open-settings-and-backup-items.png)

    **Zálohování položek** okno obsahuje samostatné seznamy, v závislosti na typu položky: virtuální počítače Azure nebo složek souborů (viz obrázek). Výchozí typ položky seznamu zobrazí je virtuálních počítačích Azure. Chcete-li zobrazit seznam položek souborů složek v trezoru, vyberte **složek souborů** z rozevírací nabídky.
4. Před odstraněním položky z trezoru ochranu virtuálního počítače, musíte zastavit úlohu zálohování položky a odstranit data bodu obnovení. Pro každou položku v trezoru postupujte takto:

    a. Na **položky zálohování** , klikněte pravým tlačítkem položku a v místní nabídce vyberte **zastavení zálohování**.

    ![zastavení úlohy zálohování](./media/backup-azure-delete-vault/stop-the-backup-process.png)

    Otevře se okno zastavení zálohování.

    b. Na **zastavení zálohování** okně z **zvolte možnost** nabídce vyberte možnost **odstranit záložní Data** > zadejte název položky > a klikněte na tlačítko **zastavení zálohování**.

    Zadejte název položky, chcete-li ověřit, že chcete odstranit. **Zastavení zálohování** tlačítko aktivuje po ověření položky. Pokud nevidíte dialogovém okně zadejte název položky zálohování, jste zvolili **zachovat zálohovaná Data** možnost.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Volitelně můžete zadat důvod, proč se odstranit data a přidání komentářů. Po kliknutí na tlačítko **zastavení zálohování**, povolit odstranit úlohu provést před pokusem o odstranění trezoru. Pokud chcete ověřit, zda byla úloha dokončena, zkontrolujte zprávy Azure ![odstranit záložní data](./media/backup-azure-delete-vault/messages.png). <br/>
    Po dokončení úlohy se zobrazí zpráva byla zastavena procesu zálohování a zálohování dat, pro danou položku, byla odstraněna.

    c. Po odstranění položky v seznamu na **zálohování položek** nabídky, klikněte na tlačítko **aktualizovat** zobrazit zbývající položky v trezoru.

      ![Odstranit záložní data](./media/backup-azure-delete-vault/empty-items-list.png)

      Pokud v seznamu nejsou žádné položky, přejděte na **Essentials** podokně v okně Zálohování trezoru. Nesmí být žádné **zálohování položek**, **zálohování serverů pro správu**, nebo **replikované položky** uvedené. Pokud stále se zobrazí položky v trezoru, vraťte se ke kroku tři a zvolte jiný položky typu seznamu.  
5. Když na panelu nástrojů trezoru nejsou žádné další položky, klikněte na tlačítko **odstranit**.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/delete-vault.png)
6. Chcete-li ověřit, že chcete trezor odstranit, klikněte na tlačítko **Ano**.

    Odstranění trezoru a vrátí portálu **nový** nabídky služby.

## <a name="what-if-i-stopped-the-backup-process-but-retained-the-data"></a>Co když jsem zastavit proces zálohování ale uchovávají data?
Pokud je ale omylem zastavit proces zálohování *uchovávají* data, je nutné odstranit zálohovaná data před odstraněním trezoru. Odstranit záložní data:

1. Na **položky zálohování** okno, klikněte pravým tlačítkem položku a v místní nabídce klikněte na **odstranit záložní data**.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    **Odstranit Data zálohování** otevře se okno.
2. Na **odstranit Data zálohování** okno, zadejte název položky a klikněte na **odstranit**.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/delete-retained-vault.png)

    Jakmile jste odstranili data, vraťte se ke kroku 4c a pokračujte v procesu.

## <a name="delete-a-vault-used-to-protect-a-dpm-server"></a>Odstranění úložiště používá k ochraně serveru aplikace DPM
Před odstraněním trezoru použít také k ochraně serveru aplikace DPM, musíte vymazat všechny body obnovení, které byly vytvořeny a potom zrušit registraci serveru z trezoru.

Chcete-li odstranit data související s skupinu ochrany:

1. V konzole správce aplikace DPM klikněte na **ochrany** > vyberte skupinu ochrany > vyberte člena skupiny ochrany > a na pásu karet nástroje klikněte na tlačítko **odebrat**.

  Vyberte člena skupiny ochrany k aktivaci **odebrat** tlačítko pásu karet nástroje. V příkladu je člen **dummyvm9**. Chcete-li vybrat několik členů skupiny ochrany, podržte stisknutou klávesu Ctrl při kliknutí na členy.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/az-portal-delete-protection-group.png)

    **Zastavit ochranu** otevře se dialogové okno.
2. V **zastavit ochranu** dialogovém okně, vyberte **odstranit chráněná data**a klikněte na tlačítko **zastavit ochranu**.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/delete-dpm-protection-group.png)

    Pokud chcete odstranit trezor, musíte vymazat nebo trezor chráněných dat odstranit. V závislosti na počtu bodů obnovení a data ve skupině ochrany může trvat kdekoli za několik sekund až několik minut odstranit data. **Zastavit ochranu** dialogové okno se zobrazí stav po dokončení úlohy.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/success-deleting-protection-group.png)
3. Dál tento proces pro všechny členy všech skupin ochrany.

    Odeberte všechny chráněné skupiny dat a ochrany.
4. Po odstranění všechny členy ze skupiny ochrany, přejděte na portál Azure. Otevřete řídícím panelu trezoru a ověřte, zda žádná **zálohování položek**, **zálohování serverů pro správu**, nebo **replikované položky**. Na panelu nástrojů trezoru, klikněte na tlačítko **odstranit**.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/delete-vault.png)

    Pokud jsou servery pro správu zálohování registrovaný k úložišti, nelze odstranit trezor i v případě, že neexistují žádná data v trezoru. Pokud jste odstranili servery pro správu zálohování přidruženého k úložišti, ale nejsou uvedeny v servery **Essentials** podokně, najdete v části [najít servery pro správu zálohování registrovaný k úložišti](backup-azure-delete-vault.md#find-the-backup-management-servers-registered-to-the-vault).
5. Chcete-li ověřit, že chcete trezor odstranit, klikněte na tlačítko **Ano**.

    Odstranění trezoru a vrátí portálu **nový** nabídky služby.

## <a name="delete-a-vault-used-to-protect-a-production-server"></a>Odstranění úložiště používá k ochraně provozním serveru
Před odstraněním trezoru použít také k ochraně provozním serveru, musíte odstranit, nebo zrušte registraci serveru z trezoru.

Chcete-li odstranit přidruženého k úložišti provozním serveru:

1. Na portálu Azure otevřete řídícím panelu trezoru a klikněte na tlačítko **nastavení** > **infrastruktura zálohování** > **produkční servery**.

    ![Otevřete okno provozních serverech](./media/backup-azure-delete-vault/delete-production-server.png)

    **Produkční servery** okno otevře a obsahuje seznam všech provozních serverů v trezoru.

    ![Seznam provozních serverech](./media/backup-azure-delete-vault/list-of-production-servers.png)
2. Na **produkční servery** , klikněte pravým tlačítkem na server a klikněte na tlačítko **odstranit**.

    ![Odstranit provozním serveru ](./media/backup-azure-delete-vault/delete-server-on-production-server-blade.png)

    **Odstranit** otevře se okno.

    ![Odstranit provozním serveru ](./media/backup-azure-delete-vault/delete-blade.png)
3. Na **odstranit** okně potvrďte název serveru a klikněte na tlačítko **odstranit**. Je třeba správně zadat název serveru, a aktivovat **odstranit** tlačítko.

    Po odstranění trezoru, obdržíte zprávu s oznámením, že trezor byl odstraněn. Po odstranění všech serverech v trezoru, přejděte zpět do podokna Essentials na řídicím panelu trezoru.
4. Na řídicím panelu trezoru, zkontrolujte, že žádné **zálohování položek**, **zálohování serverů pro správu**, nebo **replikované položky**. Na panelu nástrojů trezoru, klikněte na tlačítko **odstranit**.
5. Chcete-li ověřit, že chcete trezor odstranit, klikněte na tlačítko **Ano**.

    Odstranění trezoru a vrátí portálu **nový** nabídky služby.

## <a name="delete-a-backup-vault-in-classic-portal"></a>Odstranit trezor záloh na portálu classic
Následující pokyny jsou pro odstranění úložiště záloh na portálu classic. Před odstraněním úložiště záloh, musíte odstranit bodů obnovení, nebo zálohovat položky a odstraňte zaregistrované servery. Registrované servery jsou systému Windows Server, pracovní stanice nebo virtuálních počítačů, které byly registrovaný k úložišti.

1. Otevřete [portálu Classic](https://manage.windowsazure.com).

2. Ze seznamu trezorů záloh vyberte trezor, který chcete odstranit.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/classic-portal-delete-vault-open-vault.png)

    Otevře se řídícím panelu trezoru. Podívejte se na počet virtuálních počítačů Windows servery nebo Azure přidruženého k úložišti. Podívejte se taky na celkové úložiště využívat v Azure. Zastavit všechny úlohy zálohování a odstranění všech dat před odstraněním trezoru.

3. Klikněte **chráněné položky** a pak klikněte **zastavit ochranu**

    ![Odstranit záložní data](./media/backup-azure-delete-vault/classic-portal-delete-vault-stop-protect.png)

    **Zastavit ochranu svůj trezor** otevře se dialogové okno.
4. V **zastavit ochranu svůj trezor** dialogové okno, zkontrolujte **odstranit související zálohovaná data** a klikněte na tlačítko ![zaškrtnutí](./media/backup-azure-delete-vault/checkmark.png). <br/>
    Volitelně můžete vybrat důvod zastavení ochrany a zadejte komentář.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/classic-portal-delete-vault-verify-stop-protect.png)

    Po odstranění položky v trezoru, bude prázdný trezoru.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/classic-portal-delete-vault-post-delete-data.png)
5. V seznamu karty, klikněte na **registrované položky**. **Typ** rozevírací nabídky, umožní vám vybrat typ serveru registrovaný k úložišti. Typ může být Windows Server nebo virtuální počítač Azure. V následujícím příkladu, vyberte virtuální počítač, který je registrovaný k úložišti a klikněte na tlačítko **Unregister**.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/classic-portal-unregister.png)

  Pokud chcete odstranit registrace pro Windows Server z **typ** rozevírací nabídky vyberte **systému Windows Server**, klikněte na tlačítko ![zaškrtnutí](./media/backup-azure-delete-vault/checkmark.png) aktualizovat obrazovku, a pak klikněte na **odstranit**. <br/>

  ![Vyberte Server systému Windows](./media/backup-azure-delete-vault/select-windows-server.png)

6. V seznamu karty, klikněte na **řídicí panel** otevření této karty. Ověřte, že neexistují žádné registrované servery nebo Azure virtuální počítače chráněné v cloudu. Také ověřte, že neexistují žádná data v úložišti. Klikněte na tlačítko **odstranit** odstranění trezoru.

    ![Odstranit záložní data](./media/backup-azure-delete-vault/classic-portal-list-of-tabs-dashboard.png)

    Otevře se na potvrzovací obrazovce a odstranit zálohy trezoru. Vyberte možnost Proč při odstraňování trezor a klikněte na ![zaškrtnutí](./media/backup-azure-delete-vault/checkmark.png). <br/>

    ![Odstranit záložní data](./media/backup-azure-delete-vault/classic-portal-delete-vault-confirmation-1.png)

    Portál se odstraní a vy se vrátíte k řídicímu panelu portálu Classic.

### <a name="find-the-backup-management-servers-registered-to-the-vault"></a>Najít servery pro správu zálohování registrovaný k úložišti
Pokud máte více serverů registrovaný k trezoru, může být obtížné mějte na paměti, je. Zobrazit servery registrovaný k úložišti a odstranit:

1. Otevřete řídícím panelu trezoru.
2. V **Essentials** podokně klikněte na tlačítko **nastavení** k otevření tohoto okna.

    ![Otevřete okno nastavení](./media/backup-azure-delete-vault/backup-vault-click-settings.png)
3. Na **okno nastavení**, klikněte na tlačítko **infrastruktura zálohování**.
4. Na **infrastruktura zálohování** okně klikněte na tlačítko **servery pro správu zálohování**. Otevře se okno zálohování serverů pro správu.

    ![seznam serverů pro správu zálohování](./media/backup-azure-delete-vault/list-of-backup-management-servers.png)
5. Odstranění serveru ze seznamu, klikněte pravým tlačítkem na název serveru a pak klikněte na **odstranit**.
    **Odstranit** otevře se okno.
6. Na **odstranit** okno, zadejte název serveru. Pokud je dlouhý název, můžete zkopírovat a vložit ze seznamu serverů pro správu zálohování. Pak klikněte na tlačítko **odstranit**.  
