---
title: "Zálohování Azure – zálohování Offline nebo prvotní synchronizaci pomocí služby Azure Import/Export | Microsoft Docs"
description: "Zjistěte, jak Azure Backup umožňuje odesílat data mimo síť využívající službu Azure Import/Export. Tento článek vysvětluje, offline synchronizace replik indexů data prvotní zálohy pomocí služby Azure Import exportovat."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: ada19c12-3e60-457b-8a6e-cf21b9553b97
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 4/20/2017
ms.author: saurse;nkolli;trinadhk
ms.openlocfilehash: 074d21269206b243f8b0e8747811544132805229
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="offline-backup-workflow-in-azure-backup"></a>Pracovní postup offline zálohování ve službě Azure Backup
Zálohování Azure má několik předdefinovaných efektivitu, která ukládají náklady na síť a úložiště během počáteční úplné zálohy dat do Azure. Počáteční úplné zálohování obvykle přenos velkých objemů dat a vyžaduje větší šířku pásma sítě ve srovnání s následné zálohy, které přenášejí pouze rozdílů/přírůstková. Zálohování Azure komprimaci prvotní zálohy. Proces offline synchronizace replik indexů Azure Backup můžete použít disky k nahrání komprimovaná data prvotní zálohy do offline režimu Azure.  

Proces offline synchronizace replik indexů Azure Backup je úzce integrovaná s [služba Azure Import/Export](../storage/common/storage-import-export-service.md) , umožňuje přenos dat do Azure pomocí disků. Pokud máte terabajtů (TBs) počáteční zálohovací data, která je potřeba přenést přes síť s vysokou latencí a malou šířkou pásma, můžete k dodání kopie prvotní zálohy na jeden nebo více pevných disků na datovém centru Azure pracovní postup offline synchronizace replik indexů. Tento článek obsahuje přehled kroky, které dokončit tento pracovní postup.

## <a name="overview"></a>Přehled
Pomocí funkce offline synchronizace replik indexů Azure Backup a Azure Import/Export je jednoduše nahrát data do offline režimu do Azure pomocí disků. Namísto použití přenosu počáteční úplnou kopii přes síť, zálohování data se zapisují do *pracovního umístění*. Po dokončení kopírování do pracovního umístění pomocí nástroje Azure Import/Export tato data se zapisují na jeden nebo více SATA jednotky, v závislosti na množství dat. Tyto disky jsou nakonec odeslaná do nejbližší datové centrum Azure.

[Srpna 2016 aktualizace služby Azure Backup (a novější)](http://go.microsoft.com/fwlink/?LinkID=229525) zahrnuje *nástroj pro přípravu Azure Disk*, s názvem AzureOfflineBackupDiskPrep, který:

* Vám pomůže připravit vaše jednotky pro Import úložiště Azure pomocí nástroje Azure Import/Export.
* Automaticky vytvoří úlohu Import úložiště Azure pro službu Azure Import/Export na [portál Azure classic](https://manage.windowsazure.com) oproti vytvoření stejné ručně pomocí starší verze služby Azure Backup.

Po dokončení nahrávání dat zálohování do Azure, Azure Backup zkopíruje data záloh do trezoru záloh a přírůstkové zálohování.

> [!NOTE]
> Chcete-li použít nástroj pro přípravu Azure Disk, zkontrolujte, zda máte nainstalovanou aktualizaci srpna 2016 služby Azure Backup (nebo novější) a provedení všech kroků pracovního postupu s ním. Pokud používáte starší verzi služby Azure Backup, můžete pomocí nástroje Azure Import/Export podle popisu v pozdějších částech tohoto článku připravit jednotky SATA.
>
>

## <a name="prerequisites"></a>Požadavky
* [Seznamte se s pracovním postupem Azure Import/Export](../storage/common/storage-import-export-service.md).
* Před zahájením pracovní postup, ověřte následující:
  * Byla vytvořena trezoru zálohování Azure.
  * Stažené přihlašovací údaje trezoru.
  * Na Windows Server nebo Windows klienta nebo serveru System Center Data Protection Manager se nainstaloval agenta Azure Backup a počítač je zaregistrován v trezoru zálohování Azure.
* [Stahování souboru nastavení publikování Azure](https://manage.windowsazure.com/publishsettings) na počítači, ze kterého plánujete zálohování dat.
* Příprava pracovní umístění, což může být na sdílené síťové složce nebo další jednotku na počítači. Pracovní umístění je přechodným úložištěm a používá se dočasně během tento pracovní postup. Zajistěte, aby pracovní umístění dostatek místa na disku pro uložení počáteční kopii. Například pokud chcete zálohovat 500 GB souborový server, zajistěte, aby byl v pracovní oblasti alespoň 500 GB. (Menší velikost se používá v důsledku komprese.)
* Ujistěte se, zda používáte podporované jednotku. Pouze 2,5 SSD nebo 2.5 nebo 3,5 SATA II/III interní pevné disky jsou podporovány pro použití se službou importu a exportu. Pevné disky můžete použít až do 10 TB. Zkontrolujte [dokumentace ke službě Azure Import/Export](../storage/common/storage-import-export-service.md#hard-disk-drives) pro nejnovější sadu disků, které služba podporuje.
* Povolte nástroj BitLocker v počítači, ke kterému je připojený zapisovače jednotky SATA.
* [Stáhněte si nástroj Azure Import/Export](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) k počítači, ke kterému je připojený zapisovače jednotky SATA. Tento krok není požadováno, pokud jste stáhli a nainstalovali aktualizace srpna 2016 služby Azure Backup (nebo novější).

## <a name="workflow"></a>Pracovní postupy
Informace v této části vám pomůže dokončit pracovní postup offline zálohování tak, aby vaše data mohou doručit do datového centra Azure a nahrané do úložiště Azure. Pokud máte dotazy týkající se importu služby či jiného aspektu procesu, najdete v článku [Přehled služby importu](../storage/common/storage-import-export-service.md) dokumentace odkazuje dříve.

### <a name="initiate-offline-backup"></a>Spustit zálohování offline
1. Pokud naplánujete zálohování, uvidíte následující obrazovku (v systému Windows Server, klient systému Windows nebo System Center Data Protection Manager).

    ![Import obrazovky](./media/backup-azure-backup-import-export/offlineBackupscreenInputs.png)

    Tady je odpovídající obrazovky v System Center Data Protection Manager: <br/>
    ![Aplikace DPM import obrazovky](./media/backup-azure-backup-import-export/dpmoffline.png)

    Popis vstupních hodnot je následující:

    * **Pracovní umístění**: dočasné úložiště umístění, do které se zapíše kopie prvotní zálohy. To může být na sdílené síťové složky nebo místního počítače. Pokud počítač kopie a zdrojového počítače liší, doporučujeme, že zadáváte cestu plné síti pracovní umístění.
    * **Azure název úlohy importu**: jedinečný název, který Import úložiště Azure service a Azure Backup sledovat přenos dat odesílaných na discích do Azure.
    * **Nastavení Azure publikování**: soubor ve formátu XML, který obsahuje informace o profilu vašeho předplatného. Obsahuje taky zabezpečené přihlašovací údaje, které jsou spojeny s předplatným. Můžete [stáhněte soubor](https://manage.windowsazure.com/publishsettings). Zadejte místní cestu k souboru nastavení publikování.
    * **ID předplatného Azure**: ID předplatného Azure pro předplatné, kde chcete zahájit úlohy importu v Azure. Pokud máte víc předplatných Azure, použijte Identifikátor odběru, který chcete přidružit k úloze importu.
    * **Účet služby Azure Storage**: classic typ účtu úložiště v rámci zadaného předplatného Azure, který bude přidružen úlohy importu v Azure.
    * **Kontejner úložiště Azure**: název cílový objekt blob úložiště v účtu úložiště Azure, kde tato úloha importu.

    > [!NOTE]
    > Pokud jste si zaregistrovali serveru trezoru služeb zotavení Azure z [portál Azure](https://portal.azure.com) pro zálohování a nejsou ve Cloud Solution Provider (CSP) předplatné, můžete stále vytvořit classic typ účtu úložiště z Azure portál a použít jej pro pracovní postup offline zálohování.
    >
    >

     Tyto informace uložte, protože je nutné znovu zadat v následujících krocích. Pouze *pracovního umístění* se vyžaduje, pokud jste použili nástroj pro přípravu Azure Disk připravit disky.    
2. Dokončení pracovního postupu a potom vyberte **zálohovat nyní** v konzole pro správu Azure Backup k inicializaci offline zálohování kopírováním. Prvotní zálohování je zapsána do pracovní oblasti v rámci tohoto kroku.

    ![Zálohovat nyní](./media/backup-azure-backup-import-export/backupnow.png)

    K dokončení odpovídající pracovní postup v System Center Data Protection Manager, klikněte pravým tlačítkem myši **skupiny ochrany**a potom zvolte **vytvořit bod obnovení** možnost. Potom vyberte **Online ochrany** možnost.

    ![Aplikace DPM zálohovat nyní](./media/backup-azure-backup-import-export/dpmbackupnow.png)

    Po dokončení operace je připravená k použití pro přípravu disku pracovní umístění.

    ![Postup zálohování](./media/backup-azure-backup-import-export/opbackupnow.png)

### <a name="prepare-a-sata-drive-and-create-an-azure-import-job-by-using-the-azure-disk-preparation-tool"></a>Příprava jednotky SATA a vytvořte úlohy importu v Azure pomocí nástroje pro přípravu Azure disku
Nástroj pro přípravu Azure disku je k dispozici v instalační adresář agenta služeb zotavení (aktualizovat srpna 2016 a novější) v následující cestě.

   *\Microsoft* *azure* *obnovení* *služby* * Agent\Utils\*

1. Přejděte do adresáře a kopírování **AzureOfflineBackupDiskPrep** adresář pro kopírování počítače, ke kterému jsou připojené jednotky, abyste byli připraveni. Zajistěte následující s ohledem na počítači kopie:

    * Kopírování počítače mají přístup ke pracovní umístění pro pracovní postup offline synchronizace replik indexů pomocí stejné síťové cestě, která byla součástí **zahájit zálohování offline** pracovního postupu.
    * Nástroj BitLocker je povolen v počítači.
    * Počítač můžete přístup k portálu Azure.

    V případě potřeby kopie počítač může být stejný jako zdrojový počítač.
2. Otevření příkazového řádku se zvýšenými oprávněními v počítači kopírování s adresářem nástroj pro přípravu Azure Disk jako aktuální adresář a spusťte následující příkaz:

    `*.\AzureOfflineBackupDiskPrep.exe*   s:<*Staging Location Path*>   [p:<*Path to PublishSettingsFile*>]`

    | Parametr | Popis |
    | --- | --- |
    | s:&lt;*cesta pracovní umístění*&gt; |Povinné vstup, který slouží k zadání cesty na pracovní umístění, které jste zadali v **zahájit zálohování offline** pracovního postupu. |
    | p:&lt;*cestu k PublishSettingsFile*&gt; |Volitelné vstup, který slouží k zadat cestu k **nastavením publikování v Azure** soubor, který jste zadali v **zahájit zálohování offline** pracovního postupu. |

    > [!NOTE]
    > &lt;Cestu k PublishSettingFile&gt; hodnota je povinný, když počítač kopie a zdrojový počítač se liší.
    >
    >

    Při spuštění příkazu požadavky nástroj Výběr úlohy importu v Azure, která odpovídá jednotky, které je nutné připravit. Pokud jenom jeden import úlohy je přidružen zadané pracovní umístění, zobrazí obrazovky jako ten, který následuje.

    ![Azure vstup nástroj Příprava disku](./media/backup-azure-backup-import-export/azureDiskPreparationToolDriveInput.png) <br/>
3. Zadejte písmeno jednotky bez koncové dvojtečkou připojeného disku, který chcete připravit pro přenos do Azure. Potvrzení pro formátování disku po zobrazení výzvy zadejte.

    Nástroj pak začne Příprava disku spolu s zálohovaná data. Budete muset připojit další disky, a po zobrazení výzvy nástroj v případě, že zadaný disk nemá dostatek místa pro zálohovaná data. <br/>

    Na konci úspěšné provedení nástroj jeden nebo více disků, které jste zadali připravené pro přesouvání do Azure. Kromě toho úlohy importu s názvem jste zadali při **zahájit zálohování offline** pracovní postup je vytvořen na portálu Azure classic. Nakonec nástroj zobrazí adresa přesouvání datové centrum Azure, kde je třeba dodat disky a odkaz na portálu Azure classic k úloze importu.

    ![Příprava Azure disku dokončení](./media/backup-azure-backup-import-export/azureDiskPreparationToolSuccess.png)<br/>

4. Dodávat disky na adresu, které poskytuje nástroje a zachovejte sledování pro budoucí použití.<br/>

5. Při přechodu na odkaz, který zobrazí nástroj, uvidíte účet úložiště Azure, který jste zadali v **zahájit zálohování offline** pracovního postupu. Zde můžete zobrazit úlohy importu nově vytvořený na **IMPORTU a EXPORTU** kartě účtu úložiště.

    ![Úlohy vytvořené importu](./media/backup-azure-backup-import-export/ImportJobCreated.png)<br/>

6. Klikněte na tlačítko **informace o PŘESOUVÁNÍ** v dolní části stránky a aktualizovat svoje kontaktní údaje, jak je znázorněno v následujícím obrazovky. Společnost Microsoft používá tyto informace pro odeslání vaše disky vám po dokončení úlohy importu.

    ![Kontaktní údaje](./media/backup-azure-backup-import-export/contactInfoAddition.PNG)<br/>

7. Zadejte podrobnosti přesouvání na další obrazovce. Zadejte **doručení poskytovatel** a **sledování Číslo** podrobnosti, které odpovídají disky, které odeslaná do datovém centru Azure.

    ![Informace o přesouvání](./media/backup-azure-backup-import-export/shippingInfoAddition.PNG)<br/>

### <a name="complete-the-workflow"></a>Dokončení pracovního postupu
Po dokončení úlohy importu dat prvotní zálohy je k dispozici ve vašem účtu úložiště. Agenta služeb zotavení pak zkopíruje data z tohoto účtu do trezoru zálohování nebo obnovení služeb obsah trezoru, podle toho, která se vztahuje. V dalším naplánovaném čase zálohování aplikace Azure Backup agent provádí přírůstkové zálohování přes kopie prvotní zálohy.

> [!NOTE]
> Následující části platí pro uživatele starších verzí služby Azure Backup, kteří nemají přístup k nástroj pro přípravu Azure disku.
>
>

### <a name="prepare-a-sata-drive"></a>Příprava jednotky SATA
1. Stažení [nástroj Microsoft Azure Import/Export](http://go.microsoft.com/fwlink/?linkid=301900&clcid=0x409) k počítači kopie. Ujistěte se, že pracovní umístění je přístupné z počítače, ve kterém chcete spustit další sadu příkazů. V případě potřeby kopie počítač může být stejný jako zdrojový počítač.

2. Rozbalte soubor WAImportExport.zip. Spusťte nástroj WAImportExport, který zformátuje jednotky SATA, zapíše data záloh do jednotky SATA a zašifruje. Než spustíte následující příkaz, ujistěte se, povolení nástroje BitLocker v počítači. <br/>

    `*.\WAImportExport.exe PrepImport /j:<*JournalFile*>.jrn /id: <*SessionId*> /sk:<*StorageAccountKey*> /BlobType:**PageBlob** /t:<*TargetDriveLetter*> /format /encrypt /srcdir:<*staging location*> /dstdir: <*DestinationBlobVirtualDirectory*>/*`

    > [!NOTE]
    > Pokud máte nainstalovanou aktualizaci srpna 2016 služby Azure Backup (nebo novější), ujistěte se, že pracovní umístění, které jste zadali, je stejný jako ten, na **zálohovat nyní** obrazovky a obsahuje soubory AIB a základní objektů Blob.
    >
    >

| Parametr | Popis |
| --- | --- |
| /j: <*JournalFile*> |Cesta k souboru deníku. Každé jednotky, musí mít přesně jeden soubor deníku. Soubor deníku nesmí být na cílové jednotce. Přípona souboru deníku je .jrn a je vytvořen jako součást spuštění tohoto příkazu. |
| /ID: <*SessionId*> |ID relace identifikuje relaci kopírování. Slouží k zajištění přesných obnovení kopírování dojde k přerušení relace. Soubory, které jste zkopírovali v relaci kopie jsou uloženy v adresáři s názvem po ID relace na cílové jednotce. |
| /Sk: <*StorageAccountKey*> |Klíč účtu pro účet úložiště, do kterého se data importovat. Klíč musí být stejný jako bylo zadáno při vytváření skupiny zálohování zásady/ochrany. |
| / BlobType |Typ objektu blob. Tento pracovní postup úspěšné pouze v případě **PageBlob** je zadán. Toto je výchozí možnost a by se měla uvádět v tomto příkazu. |
| / t: <*TargetDriveLetter*> |Písmeno jednotky bez koncové dvojtečkou pevný disk cílového pro aktuální relaci kopírování. |
| / Format |Možnost formátování disku. Zadejte tento parametr, pokud jednotka musí být ve formátu; jinak vynechejte. Předtím, než nástroj formáty jednotku, zobrazí výzvu k potvrzení z konzoly. Chcete-li potlačit potvrzení, zadejte parametr /silentmode. |
| / šifrování |Možnost šifrování jednotky. Když jednotka nebyla dosud pomocí Bitlockeru šifrovat a musí být šifrované nástrojem, zadejte tento parametr. Pokud jednotka již byla zašifrována pomocí nástroje BitLocker, tento parametr vynecháte, zadejte parametr /bk a zadejte existující klíč nástroje BitLocker. Pokud zadáte parametr/Format, je třeba zadat také / šifrování parametru. |
| /srcdir: <*SourceDirectory*> |Zdrojový adresář, který obsahuje soubory budou zkopírovány do cílové jednotky. Zkontrolujte, zda má zadaný název adresáře úplné místo relativní cesty. |
| /dstdir: <*DestinationBlobVirtualDirectory*> |Cesta k cílové virtuální adresář ve vašem účtu úložiště Azure. Nezapomeňte použít názvy platný kontejnerů, když zadáte cílové virtuální adresáře nebo objekty BLOB. Uvědomte si, že názvy kontejnerů musí být malými písmeny.  Tento název kontejneru musí být ten, který jste zadali při vytváření skupiny zálohování zásady/ochrany. |

> [!NOTE]
> Soubor deníku se vytvoří ve složce WAImportExport, která zaznamená celý informace o pracovním postupu. Tento soubor musíte při vytváření úlohy importu v portálu Azure.
>
>

  ![Výstup prostředí PowerShell](./media/backup-azure-backup-import-export/psoutput.png)

### <a name="create-an-import-job-in-the-azure-portal"></a>Vytvoření úlohy importu v portálu Azure
1. Přejděte na svůj účet úložiště [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **importu a exportu**a potom **vytvoření úlohy importu** v podokně úloh.

    ![Import a export na portálu Azure](./media/backup-azure-backup-import-export/azureportal.png)

2. V kroku 1 Průvodce znamenat, že je vše připraveno jednotce a, abyste měli deníku souboru disku, které jsou k dispozici.

3. V kroku 2 průvodce zadejte kontaktní informace osoby, která je zodpovědná za tuto úlohu importu.

4. V kroku 3 odešlete soubory deníku jednotky, které jste získali v předchozí části.

5. V kroku 4 zadejte popisný název úlohy importu, kterou jste zadali při vytváření skupiny zálohování zásady/ochrany. Název, který zadáte, může obsahovat jenom malá písmena, číslice, pomlčky a podtržítka, musí začínat písmenem a nesmí obsahovat mezery. Název, který zvolíte, se používá ke sledování úloh v době, kdy jsou v průběhu a po jejich dokončení.

6. V dalším kroku vyberte oblast datacenter ze seznamu. Oblasti datacenter označuje datacenter a adresu, na kterou je nutné dodat vašeho balíčku.

    ![Vyberte oblast, datacenter](./media/backup-azure-backup-import-export/dc.png)

7. V kroku 5 vyberte vaše návratový poskytovatel ze seznamu a zadejte číslo svého operátora účtu. Společnost Microsoft používá tento účet pro odeslání jednotky vám po dokončení úlohy importu.

8. Odešlete disk a zadejte číslo sledování sledovat stav dodávky. Po doručení disku v datovém centru, je zkopírován do účtu úložiště a stav bude aktualizován.

    ![Stavu dokončení](./media/backup-azure-backup-import-export/complete.png)

### <a name="complete-the-workflow"></a>Dokončení pracovního postupu
Po počáteční zálohovací data je k dispozici ve vašem účtu úložiště, agenta služeb zotavení Microsoft Azure zkopíruje obsah data z tohoto účtu do trezoru služby Backup nebo trezor služeb zotavení, podle toho, která se vztahuje. V další čas plánu zálohování aplikace Azure Backup agent provádí přírůstkové zálohování přes kopie prvotní zálohy.

## <a name="next-steps"></a>Další kroky
* Všechny dotazy týkající se Azure Import/Export pracovního postupu najdete v části [používat službu Microsoft Azure Import/Export k přenosu dat do úložiště objektů Blob](../storage/common/storage-import-export-service.md).
* Informace naleznete v sekci zálohování offline ze služby Azure Backup [– nejčastější dotazy](backup-azure-backup-faq.md) pro všechny dotazy týkající se pracovní postup.
