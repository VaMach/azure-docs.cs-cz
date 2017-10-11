---
title: "Obnovení dat do systému Windows Server nebo klienta Windows z Azure pomocí modelu nasazení classic | Microsoft Docs"
description: "Zjistěte, jak obnovit ze systému Windows Server nebo klienta Windows."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 85585dfc-c764-4e8c-8f0e-40b969640ac2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 300b2b17b44e21ed446fd63d572a2461e2fc1343
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-the-classic-deployment-model"></a>Obnovení souborů na serveru Windows nebo klientském počítači s využitím klasického modelu nasazení
> [!div class="op_single_selector"]
> * [Portál Classic](backup-azure-restore-windows-server-classic.md)
> * [Azure Portal](backup-azure-restore-windows-server.md)
>
>

Tento článek vysvětluje, jak obnovit data z úložiště záloh a obnovení serveru nebo počítači. Počínaje března 2017, můžete již nemohou vytvářet záloh na portálu classic.

> [!IMPORTANT]
> Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services. Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.<br/> Od **15. října 2017** už nebude možné pomocí PowerShellu vytvářet trezory služby Backup. <br/> **Od 1. listopadu 2017**:
>- Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.
>- Nebudete mít přístup k datům záloh na portálu Classic. Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.
>

Chcete-li obnovit data, použijte Průvodce obnovení dat v agentovi nástroje Microsoft Azure Recovery Services (MARS). Při obnovování dat, je možné:

* Obnovení dat na stejný počítač, ve kterém byly provedeny zálohy.
* Obnovte data do alternativní počítače.

V lednu 2017 společnost Microsoft vydala Preview aktualizace agenta MARS. Společně s oprav chyb tato aktualizace umožňuje rychlé obnovení, která umožňuje připojit obnovení zapisovatelného snímku bodu jako svazek obnovení. Pak můžete zkoumat obnovení svazku a zkopírujte soubory do místního počítače a následné obnovení souborů.

> [!NOTE]
> [Ledna 2017 Azure Backup aktualizace](https://support.microsoft.com/en-us/help/3216528?preview) je povinný, pokud chcete obnovit data pomocí funkce Rychlé obnovení. Zálohovaná data musí být chráněna v trezorů v národní prostředí, které jsou uvedené v článku podpory. Obrátit [ledna 2017 Azure Backup aktualizace](https://support.microsoft.com/en-us/help/3216528?preview) nejnovější seznam národních prostředí, které podporují rychlé obnovení. Rychlé obnovení je **není** aktuálně k dispozici ve všech národních prostředí.
>

Rychlé obnovení je k dispozici pro použití v trezory služeb zotavení v portálu Azure a trezory Backup na portálu classic. Pokud chcete použít rychlé obnovení, stažení aktualizace MARS a postupujte podle pokynů, které zmínili, rychlé obnovení.


## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a>Obnovit data do stejného počítače pomocí funkce Rychlé obnovení

Pokud jste omylem odstranit soubor a chcete obnovit do stejného počítače (ze kterého dochází k zálohování), následující kroky vám pomohou obnovit data.

1. Otevřete **Microsoft Azure Backup** přichycení v. Pokud si nejste jisti, kde byl nainstalován modul snap-in, hledání se počítač nebo server pro **Microsoft Azure Backup**.

    Desktopová aplikace by se ve výsledcích hledání.

2. Klikněte na tlačítko **obnovit Data** spusťte průvodce.

    ![Obnovení dat](./media/backup-azure-restore-windows-server/recover.png)

3. Na **Začínáme** , chcete-li obnovit data na stejném serveru nebo počítače, vyberte **tento server (`<server name>`)** a klikněte na tlačítko **Další**.

    ![Vyberte tuto možnost serveru k obnovení dat na stejný počítač](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. Na **vyberte režimu obnovení** podokně vyberte **jednotlivých souborů a složek** a pak klikněte na **Další**.

    ![Procházet soubory](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. Na **vyberte svazek a datum** podokně, vyberte svazek, který obsahuje soubory nebo složky, kterou chcete obnovit.

    V kalendáři vyberte bod obnovení. Můžete obnovit z libovolného obnovení bodu v čase. Data v **tučné** označuje dostupnost alespoň jeden bod obnovení. Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte konkrétní bod obnovení z **čas** rozevírací nabídce.

    ![Svazek a datum](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. Po zadání bodu obnovení pro obnovení, klikněte na tlačítko **připojit**.

    Zálohování Azure připojí bod místní obnovení a používá je jako svazek obnovení.

7. Na **procházení a obnovit soubory** podokně klikněte na tlačítko **Procházet** otevřete Průzkumníka Windows a soubory a složky, které chcete najít.

    ![Možnosti obnovení](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. V Průzkumníku Windows zkopírujte soubory nebo složky, kterou chcete obnovit a vložit do libovolného umístění místní k serveru nebo počítači. Můžete otevřít nebo stream soubory přímo ze svazku obnovení a ověřte, že se obnoví správné verze.

    ![Zkopírujte a vložte souborů a složek z připojeného svazku do místního umístění](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. Po dokončení obnovení souborů a složek, na **procházení a obnovení souborů** podokně klikněte na tlačítko **odpojení**. Pak klikněte na tlačítko **Ano** potvrďte, že chcete odpojit svazek.

    ![Odpojte Image svazku a potvrďte](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > Pokud neklikejte na odpojení, zůstane svazek obnovení připojené šest hodin od okamžiku, kdy byla připojena. Žádná operace zálohování se spustí připojený svazek. Naplánované spuštění v době, kdy je svazek připojený, zálohování se spustí po obnovení svazku je odpojené.
    >


## <a name="recover-data-to-the-same-machine"></a>Obnovení dat na stejný počítač
Pokud jste omylem odstranit soubor a chcete obnovit do stejného počítače (ze kterého dochází k zálohování), následující kroky vám pomohou obnovit data.

1. Otevřete **Microsoft Azure Backup** přichycení v.
2. Klikněte na tlačítko **obnovit Data** inicializace pracovního postupu.

    ![Obnovení dat](./media/backup-azure-restore-windows-server-classic/recover.png)
3. Vyberte  **tento server (*yourmachinename*) ** možnost obnovení zálohy souboru ve stejném počítači.

    ![Stejný počítač](./media/backup-azure-restore-windows-server-classic/samemachine.png)
4. Zvolit **Procházet soubory** nebo **vyhledávání souborů**.

    Ponechte výchozí možnost, pokud máte v plánu obnovení jeden nebo více souborů, jejichž cesta je známá. Pokud nejste jisti o struktura složek, ale chcete hledat soubor, vyberte **vyhledávání souborů** možnost. Pro účely této části se budeme pokračovat v výchozí možnost.

    ![Procházet soubory](./media/backup-azure-restore-windows-server-classic/browseandsearch.png)
5. Vyberte svazek, ze kterého chcete soubor obnovit.

    Můžete obnovit z libovolného bodu v čase. Data, které jsou v **tučné** v ovládacím prvku kalendář označuje dostupnost bodu obnovení. Jakmile je vybrat datum, na základě plán zálohování (a úspěch operace zálohování), můžete vybrat bod v čase, ze **čas** rozevírací nabídku.

    ![Svazek a datum](./media/backup-azure-restore-windows-server-classic/volanddate.png)
6. Vyberte položky, které chcete obnovit. Můžete vybrat víc složek nebo souborů, které chcete obnovit.

    ![Výběr souborů](./media/backup-azure-restore-windows-server-classic/selectfiles.png)
7. Zadejte parametry obnovení.

    ![Možnosti obnovení](./media/backup-azure-restore-windows-server-classic/recoveroptions.png)

   * Máte možnost obnovení do původního umístění (ve kterém soubor nebo složku, budou přepsána) nebo do jiného umístění ve stejném počítači.
   * Pokud v cílovém umístění existuje soubor nebo složku, kterou chcete obnovit, můžete vytvořit kopie (dvě verze stejného souboru), přepisovat soubory v cílovém umístění nebo přeskočit obnovení souborů, které existují v cíl.
   * Důrazně doporučujeme ponechat výchozí možnost obnovení seznamy ACL v souborech, které se obnovuje.
8. Jakmile jsou k dispozici tyto vstupy, klikněte na možnost **Další**. Pracovní postup obnovení, který obnoví soubory k tomuto počítači, bude zahájena.

## <a name="recover-to-an-alternate-machine"></a>Obnovit na alternativní počítače
Pokud dojde ke ztrátě celý server, můžete stále obnovit data z Azure Backup na jiný počítač. Následující kroky popisují pracovního postupu.  

Zahrnuje technologiím použitým v těchto kroků:

* *Zdrojový počítač* – původní počítač, ze kterého bylo provedeno zálohování a který není aktuálně k dispozici.
* *Cílový počítač* – počítače, do níž se obnovuje data.
* *Ukázka trezoru* – úložiště záloh, ke kterému *zdrojový počítač* a *cílový počítač* jsou registrované. <br/>

> [!NOTE]
> Zálohy vytvořené z počítače nelze obnovit v počítači, který běží starší verze operačního systému. Například pokud zálohy jsou převzaty z počítače s Windows 7, může být obnovena do systému Windows 8 nebo novější verze počítače. Naopak jsou však nemá hodnotu true.
>
>

1. Otevřete **Microsoft Azure Backup** přichycení v na *cílový počítač*.
2. Ujistěte se, že *cílový počítač* a *zdrojový počítač* jsou registrované na stejné úložiště záloh.
3. Klikněte na tlačítko **obnovit Data** inicializace pracovního postupu.

    ![Obnovení dat](./media/backup-azure-restore-windows-server-classic/recover.png)
4. Vyberte **jiný server**

    ![Jiný Server](./media/backup-azure-restore-windows-server-classic/anotherserver.png)
5. Zadejte soubor s přihlašovacími údaji trezoru, která odpovídá *ukázka trezoru*. Pokud soubor s přihlašovacími údaji trezoru je neplatný (nebo vypršela platnost), stáhněte si nový soubor přihlašovacích údajů trezoru z *ukázka trezoru* na portálu Azure classic. Jakmile je zadaný soubor s přihlašovacími údaji trezoru, se zobrazí úložiště záloh proti soubor s přihlašovacími údaji trezoru.
6. Vyberte *zdrojový počítač* ze seznamu zobrazených počítačů.

    ![Seznam počítačů](./media/backup-azure-restore-windows-server-classic/machinelist.png)
7. Vyberte buď **vyhledávání souborů** nebo **Procházet soubory** možnost. Pro účely této části se budeme používat **vyhledávání souborů** možnost.

    ![Search](./media/backup-azure-restore-windows-server-classic/search.png)
8. Vyberte svazek a datum na další obrazovce. Vyhledávání pro název složka či soubor, který chcete obnovit.

    ![Hledání položek](./media/backup-azure-restore-windows-server-classic/searchitems.png)
9. Vyberte umístění, kde je nutné obnovit soubory.

    ![Obnovení umístění](./media/backup-azure-restore-windows-server-classic/restorelocation.png)
10. Zadejte šifrovací heslo, které jste zadali během *zdrojový počítač* registraci *ukázka trezoru*.

    ![Šifrování](./media/backup-azure-restore-windows-server-classic/encryption.png)
11. Jakmile je k dispozici vstup, klikněte na **obnovit**, která aktivuje obnovení do cílového umístění zadané zálohované soubory.

## <a name="use-instant-restore-to-restore-data-to-an-alternate-machine"></a>K obnovení dat alternativní počítači použít rychlé obnovení
Pokud dojde ke ztrátě celý server, můžete stále obnovit data z Azure Backup na jiný počítač. Následující kroky popisují pracovního postupu.

Zahrnuje technologiím použitým v těchto kroků:

* *Zdrojový počítač* – původní počítač, ze kterého bylo provedeno zálohování a který není aktuálně k dispozici.
* *Cílový počítač* – počítače, do níž se obnovuje data.
* *Ukázka trezoru* – trezor služeb zotavení, ke kterému *zdrojový počítač* a *cílový počítač* jsou registrované. <br/>

> [!NOTE]
> Zálohování nelze obnovit do cílového počítače, který používá starší verzi operačního systému. Například převzat ze systému Windows 7, počítač může být obnovena záloha v systému Windows 8 nebo novější, počítače. Zálohy z počítače se systémem Windows 8 není možné obnovit do počítače Windows 7.
>
>

1. Otevřete **Microsoft Azure Backup** přichycení v na *cílový počítač*.

2. Ujistěte se, *cílový počítač* a *zdrojový počítač* jsou registrované ke stejnému trezoru služeb zotavení.

3. Klikněte na tlačítko **obnovit Data** otevřete **Průvodce obnovení dat**.

    ![Obnovení dat](./media/backup-azure-restore-windows-server/recover.png)

4. Na **Začínáme** podokně, vyberte **jiný server**

    ![Jiný Server](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. Zadejte soubor s přihlašovacími údaji trezoru, která odpovídá *ukázka trezoru*a klikněte na tlačítko **Další**.

    Pokud soubor s přihlašovacími údaji trezoru je neplatný (nebo vypršela platnost), stáhněte si nový soubor přihlašovacích údajů trezoru z *ukázka trezoru* na portálu Azure. Po zadání přihlašovacích údajů platné úložiště, zobrazí se název odpovídajícího úložiště záloh.

6. Na **vyberte zálohování serveru** podokně, vyberte *zdrojový počítač* ze seznamu zobrazených počítačů a zadejte heslo. Pak klikněte na tlačítko **Další**.

    ![Seznam počítačů](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. Na **vyberte režimu obnovení** podokně, vyberte **jednotlivých souborů a složek** a klikněte na tlačítko **Další**.

    ![Search](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. Na **vyberte svazek a datum** podokně, vyberte svazek, který obsahuje soubory nebo složky, kterou chcete obnovit.

    V kalendáři vyberte bod obnovení. Můžete obnovit z libovolného obnovení bodu v čase. Data v **tučné** označuje dostupnost alespoň jeden bod obnovení. Jakmile vyberete datum, pokud jsou k dispozici více bodů obnovení, vyberte konkrétní bod obnovení z **čas** rozevírací nabídce.

    ![Hledání položek](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. Klikněte na tlačítko **připojit** místně připojit bod obnovení jako svazek obnovení na vaše *cílový počítač*.

10. Na **procházení a obnovit soubory** podokně klikněte na tlačítko **Procházet** otevřete Průzkumníka Windows a soubory a složky, které chcete najít.

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. V Průzkumníku Windows, zkopírujte soubory nebo složky ze svazku obnovení a vložte je do vašeho *cílový počítač* umístění. Můžete otevřít nebo stream soubory přímo ze svazku obnovení a ověřte, že se obnoví správné verze.

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. Po dokončení obnovení souborů a složek, na **procházení a obnovení souborů** podokně klikněte na tlačítko **odpojení**. Pak klikněte na tlačítko **Ano** potvrďte, že chcete odpojit svazek.

    ![Šifrování](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > Pokud neklikejte na odpojení, zůstane svazek obnovení připojené šest hodin od okamžiku, kdy byla připojena. Žádná operace zálohování se spustí připojený svazek. Naplánované spuštění v době, kdy je svazek připojený, zálohování se spustí po obnovení svazku je odpojené.
    >


## <a name="next-steps"></a>Další kroky
* [Azure Backup – nejčastější dotazy](backup-azure-backup-faq.md)
* Přejděte [fórum Azure Backup](http://go.microsoft.com/fwlink/p/?LinkId=290933).

## <a name="learn-more"></a>Další informace
* [Přehled služby Azure Backup](http://go.microsoft.com/fwlink/p/?LinkId=222425)
* [Zálohování virtuálních počítačů Azure](backup-azure-vms-introduction.md)
* [Zálohování se úlohy Microsoft](backup-azure-dpm-introduction.md)
