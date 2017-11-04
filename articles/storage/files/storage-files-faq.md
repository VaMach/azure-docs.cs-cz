---
title: "Časté otázky k Azure Files | Microsoft Docs"
description: "Odpovědi na časté otázky o Azure Files."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/13/2017
ms.author: renash
ms.openlocfilehash: 91c46ea0897f83811cfa5c1a8b67677caff14569
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="frequently-asked-questions-about-azure-files"></a>Časté otázky k Azure soubory
[Soubory Azure](storage-files-introduction.md) nabízí plně spravované sdílené složky v cloudu, které jsou přístupné přes oborový standard [zpráva bloku protokol Server (SMB)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) (také označované jako běžné systém souborů Internetu nebo CIFS). Sdílené složky Azure je možné připojit současně do cloudových i místních nasazení systémů Windows, Linux a macOS. Kromě toho Azure sdílené složky můžete do mezipaměti na serverech Windows se synchronizace souboru Azure (Preview) pro rychlý přístup k téměř kde se používá data.

Tento článek se zabývá některé často kladené otázky týkající se Azure soubory funkcí a funkčnosti, včetně synchronizace souboru Azure. Pokud nevidíte odpověď na svoji otázku zde, neváhejte a oslovení nám těmito cestami (v narůstajícím pořadí):

1. V sekci komentáře tohoto článku.
2. [Fórum pro Azure Storage](https://social.msdn.microsoft.com/Forums/home?forum=windowsazuredata)
3. [Soubory Azure UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) 
4. Microsoft Support: Chcete-li vytvořit nové případu podpory, přejděte na "Nápověda a podpora" kartě na portálu Azure a klikněte na možnost "Nová žádost o podporu".

## <a name="general"></a>Obecné
* <a id="why-files-useful"></a>**Soubory Azure je užitečné**  
   Soubory Azure můžete vytvořit sdílené složky v cloudu, aniž by museli spravovat nároky na fyzický server nebo zařízení nebo zařízení. To znamená, že může trávit méně času použití aktualizací operačního systému a nahrazení chybných disků – vše potřebné monotónní jsme to pro vás. Další informace o scénářích soubory Azure může pomoct s najdete v tématu [proč Azure soubory je užitečné](storage-files-introduction.md#why-azure-files-is-useful).

* <a id="file-access-options"></a>**Jaké jsou různé způsoby, jak přistupovat k souborům v souborech Azure?**  
    Můžete připojit sdílenou složku na místním počítači pomocí protokolu SMB 3.0 nebo pomocí nástrojů, jako [Storage Explorer](http://storageexplorer.com/) přístup k souborům ve sdílené složce. Z vaší aplikace můžete použít knihovny klienta úložiště, rozhraní REST API, Powershellu nebo rozhraní příkazového řádku pro přístup k souborům v Azure sdílené složky.

* <a id="what-is-afs"></a>**Co je Azure souboru synchronizace?**  
    Synchronizace služby Azure souboru můžete centralizovat vaší organizace sdílené složky v souborech Azure bez nutnosti poskytnutí flexibilitu, výkonu a kompatibility pro místní souborový server. Dělá to pomocí transformace serverů Windows na rychlou mezipaměť sdílené složky Azure. Pro místní přístup k datům můžete použít jakýkoli protokol dostupný ve Windows Serveru (včetně SMB, NFS a FTPS) a můžete mít libovolný počet mezipamětí po celém světě.

* <a id="files-versus-blobs"></a>**Proč používat Azure sdílenou versus úložiště objektů Blob v Azure pro svá data?**  
    Soubory Azure a Azure Blob úložiště jak poskytnout způsob, jak ukládat velké objemy dat v cloudu, ale jsou užitečné pro účely mírně lišit. Azure Blob storage je užitečné pro masivním měřítku, nativní cloudové aplikace, které potřebují pro ukládání nestrukturovaných dat. Pokud chcete maximalizovat výkon a škálování, úložiště objektů Blob Azure je jednodušší abstrakce úložiště než systém souborů na hodnotu true. Kromě toho Azure Blob storage může přistupovat pouze prostřednictvím klientské knihovny založené na REST (nebo přímo přes protokol založené na REST).

    Soubory Azure na druhé straně konkrétně snaží být systému souborů, se všemi přehledů souboru, které znáte a rádi z let místní operačních systémů. Podobně jako úložiště objektů Azure Blob soubory Azure nabízí rozhraní REST a klientské knihovny založené na REST, ale na rozdíl od Azure Blob storage, Azure Files také nabízí SMB přístup ke sdíleným složkám souborů Azure. To znamená, že můžete přímo připojit Azure sdílené složky v systému Windows, Linux nebo systému macOS, místně nebo v cloudu virtuálních počítačů, bez nutnosti psaní jakéhokoli kódu nebo připojení k systému souborů žádné speciální ovladače. Kromě toho sdílené složky Azure File může být uložené v mezipaměti místní souborové servery pomocí synchronizace souboru Azure pro rychlý přístup téměř kde se používá data. 
   
    Podrobné informace o rozdílech mezi Azure soubory a úložiště objektů Blob v Azure naleznete v tématu [rozhodování, kdy používat úložiště objektů Azure Blob, soubory Azure nebo Azure disky](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Další informace o Azure Blob storage najdete v tématu [Úvod do úložiště objektů Blob](../blobs/storage-blobs-introduction.md).

* <a id="files-versus-disks"></a>**Proč používat Azure sdílenou a Azure?**  
    K Azure Disk je právě tedy disk. Samostatný disk samostatně, není velmi užitečné - získat hodnotu z disku Azure, budete muset připojit k virtuálnímu počítači běží v Azure. Disky systému Azure můžete použít pro všechno, co a všechno byste použili disk na serveru místní: jako disk s operačním systémem systému záměna prostoru pro operační systém, nebo jako vyhrazeného úložiště pro aplikaci. Jednou zajímavé použití pro disky Azure je vytvořit souborový server v cloudu pro použití v stejně umístí vám může použít sdílenou složku Azure File. Nasazení souborového serveru ve virtuálních počítačích Azure je vysoce výkonné, protože přitahují způsob, jak získat soubor úložiště v Azure, pokud požadujete možnosti nasazení není aktuálně podporovaná Azure soubory (třeba podporu nebo premium úložiště serveru NFS protocol). 

    Na druhé straně s Azure disky jako back-end souborového serveru úložiště obvykle bude výrazně dražší než používá sdílenou složku Azure z několika důvodů. Nejprve kromě platícího ukládání na disk, musí také platíte za náklady spuštěných jeden nebo více virtuálních počítačů Azure. Druhý musí taky spravovat virtuální počítače slouží ke spuštění souborový server, jako je například je zodpovědná za upgrady operačního systému atd. Nakonec pokud požadujete nakonec, že data uložena do mezipaměti místně, je to na můžete nastavit a spravovat toho docílit technologie replikace (například replikaci distribuovaného systému souborů).

    Jeden ze způsobů zajímavé získat nejvhodnější soubory Azure a souborový server hostované ve virtuálních počítačích Azure s Azure disky jako back endové úložné je instalace synchronizace souboru Azure na virtuální počítač hostován souborového serveru cloudu. Když sdílenou složku Azure File je ve stejné oblasti jako souborový server, můžete povolit cloudu vrstvení a nastavte procento volného místa na svazku na maximální (99 %). Tím se zajistí minimální duplicitních dat při budete moci použít jakékoli aplikace, které chcete na souborových serverech tyto nic vyžadování protokolu NFS podporují.

    Informace o jeden způsob, jak nastavit vysoký výkon a vysoce dostupný souborový server v Azure najdete v tématu [nasazení IaaS clustery hostů virtuálních počítačů v Microsoft Azure](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/). Podrobné informace o rozdílech mezi Azure soubory a disky Azure naleznete v tématu [rozhodování, kdy používat úložiště objektů Azure Blob, soubory Azure nebo Azure disky](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Další informace o discích Azure najdete v tématu [přehled disky spravované Azure](../../virtual-machines/windows/managed-disks-overview.md).

* <a id="get-started"></a>**Jak můžu začít pracovat pomocí souborů Azure?**  
    Začínáme s Azure Files je snadné: jednoduše [vytvoření sdílené složky](storage-how-to-create-file-share.md) a připojte v operačním systému upřednostňované: 

    - [Připojení v systému Windows](storage-how-to-use-files-windows.md)
    - [Připojení v systému Linux](storage-how-to-use-files-linux.md)
    - [Připojení v systému macOS](storage-how-to-use-files-mac.md)

    Podrobnější informace o tom, jak nasadit sdílenou složku Azure File nahradit produkční sdílené složky ve vaší organizaci najdete v tématu [plánování nasazení Azure Files](storage-files-planning.md).

* <a id="redundancy-options"></a>**Jaké možnosti redundance úložiště podporuje soubory Azure?**  
    Soubory Azure aktuálně podporuje jenom místně redundantní úložiště (LRS) a geograficky redundantní úložiště (GRS) teď. Jsme chcete podporovat zónově redundantní úložiště (ZRS) a geograficky redundantní úložiště s přístupem pro čtení (RA-GRS) v budoucnu, ale nemají časové osy sdílet v tuto chvíli.

* <a id="tier-options"></a>**Soubory Azure aktuálně podporuje jaké vrstvy úložiště?**  
    Soubory Azure aktuálně podporuje pouze úroveň standardní úložiště. Časové osy ke sdílení pro premium a nástrojů podpory aktuálně nemáme. Všimněte si, že nelze vytvořit sdílené složky Azure File z jen objektu Blob úložiště účty nebo účty úložiště Premium.

* <a id="give-us-feedback"></a>**Opravdu chcete vidět *x* funkce přidané do souborů Azure, můžete ho přidat?**  
    Tým služby Azure Files je absolutně, zajímá konzultaci veškeré zpětnou vazbu, měli byste o naši službu. Prosím hlasovat o žádosti o funkce na [Azure soubory UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files)! Nemůžeme se Těšíme se na delighting jste s mnoha nových funkcí.

## <a name="azure-file-sync"></a>Synchronizace Azure File
* <a id="afs-region-availability"></a>**Které oblasti jsou aktuálně podporovány pro synchronizaci souborů Azure (preview)?**  
    Synchronizace služby Azure souboru je nyní dostupná v západní USA, západní Evropa, Austrálie – východ a jihovýchodní Asie. Jak jsme usilovat obecné dostupnosti přidáme podpory pro další oblasti. Další informace najdete v tématu [dostupnost v oblastech](storage-sync-files-planning.md#region-availability).

* <a id="cross-domain-sync"></a>**Může mít připojený k doméně a servery připojené k doméně ve stejné skupině synchronizace?**  
    Ano, synchronizace skupina může obsahovat koncové body serveru, které mají různé členství ve službě Active Directory, která se zaměřuje na nebude připojený k doméně. Při konfiguraci technicky funguje, nedoporučujeme to jako normální konfigurace jako seznamy ACL, které jsou definovány u souborů a složek na jednom serveru nemusí být možné vynutit jinými servery ve skupině synchronizace. Nejlepších výsledků dosáhnete doporučujeme, abyste synchronizaci mezi buď servery ve stejné doménové struktury služby Active Directory, servery v různých doménových strukturách služby Active Directory s vztahy důvěryhodnosti zavedené nebo není v doméně, ale není kombinaci všechny výše uvedené servery.

* <a id="afs-change-detection"></a>**Vytvoření souboru přímo v mé sdílenou složku Azure File prostřednictvím protokolu SMB nebo prostřednictvím portálu. Jak dlouho, dokud je soubor synchronizované s servery ve skupině synchronizace?** 
    [!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

* <a id="afs-conflict-resolution"></a>**Co se stane, když ke stejnému souboru se změní na dvou serverech přibližně ve stejnou dobu?**  
    Synchronizace služby Azure soubor používá strategie řešení jednoduchého konflikt: jsme zachovat oba změny. Většina nedávno písemnou ponechá původní název souboru. Starší soubor má počítač 'source' a číslo konflikt připojeným k názvu s tuto taxonomii: 
   
    `<FileNameWithoutExtension>-<MachineName>[-#].<ext>`  

    Například první konflikt `CompanyReport.docx` by `CompanyReport-CentralServer.docx` Pokud `CentralServer` je, kde starší zápisu došlo k chybě. Druhý konflikt by musí identifikovat jako `CompanyReport-CentralServer-1.docx`.

* <a id="afs-storage-redundancy"></a>**Geograficky redundantní úložiště (GRS) podporované pro synchronizaci souborů Azure?**  
    Ano, místně redundantní úložiště (LRS) a geograficky redundantní úložiště (GRS) podporuje Azure synchronizace souboru. V případě selhání GRS mezi oblastmi spárované doporučujeme práce novou oblast jako zálohu pouze data. Synchronizace služby Azure souboru se nespustí automaticky synchronizuje s novou primární oblasti. 

* <a id="sizeondisk-versus-size"></a>**Proč není *místo na disku* vlastnost pro odpovídající soubor *velikost* vlastnost po dokončení synchronizace souboru Azure?**  
    Průzkumníka souborů ve Windows zpřístupní dvě vlastnosti **velikost** a **místo na disku** představují velikost souboru. Tyto vlastnosti se mírně liší v význam; **Velikost** představuje dokončení velikost souboru, zatímco **místo na disku** představuje velikost datového proudu souboru. ve skutečnosti uložená na disku. Tyto liší z různých důvodů, například komprese, použít odstranění duplicitních dat, nebo v našem případě, vrstvení se synchronizací souboru Azure v cloudu. Pokud je soubor vrstvené do Azure sdílené složky, bude místo na disku nula, protože datový proud souboru uložené v Azure sdílené složce, není na disku. Je také možné pro soubor, který chcete být částečně vrstvené (nebo částečně odvolané), což znamená, že část souboru, je na disku. To může nastat při částečně číst soubory aplikací, jako jsou přehrávačů multimédií nebo pomocí formátu ZIP nástrojů. 

* <a id="is-my-file-tiered"></a>**Jak poznám, pokud byla vrstvené soubor?**  
    Existuje několik způsobů, jak zkontrolovat, pokud soubor má k souboru Azure zřízeny vrstvené sdílení:
    
    1. **Zkontrolujte atributy souborů v souboru.** Chcete-li to provést, klikněte pravým tlačítkem na soubor, přejděte na **podrobnosti** a přejděte dolů k položce **atributy** vlastnost. Vrstvený soubor bude mít nastavit následující atributy:     
        
        | Atribut písmeno | Atribut | Definice |
        |:----------------:|-----------|------------|
        | A | Archiv | Označuje, že je nutné soubor zálohovat zálohování softwarem. Tento atribut nastavený vždy bez ohledu na to, zda je soubor vrstvené nebo plně uložená na disku. |
        | P | Chování řídkého souboru | Určuje, zda je soubor zhuštěného souboru, které se speciálním typem souborů, které nabízí systému souborů NTFS pro efektivní použití po souboru na disku proud, který je ve většině případů prázdný. Synchronizace služby Azure soubor používá zhuštěných souborů, protože soubor je buď plně vrstvené, což znamená, jeho datový proud souboru je uložena pouze v cloudu, nebo částečně odvolané, znamená že část souboru, je již na disku. Pokud je soubor plně připomenout na disk, synchronizace souboru Azure bude převedena z zhuštěného souboru k běžným sdíleným. |
        | L | Spojovací bod | Určuje, že soubor má bod rozboru, což je speciální ukazatel za účelem použití filtrem systému souborů. Synchronizace služby Azure soubor používá spojovací body umožňují definovat k Azure souboru Sync filtrem systému souborů (StorageSync.sys) v cloudu soubor se uloží. To umožňuje bezproblémový přístup bez vašeho koncového uživatele i nepotřebuje vědět, že je používána synchronizace souboru Azure a jak získat přístup k souboru v Azure sdílené složky. Pokud soubor plně je třeba připomenout, synchronizace souboru Azure bodu rozboru odebere ze souboru. |
        | O | V režimu offline | Označuje, že některé nebo všechny obsah souboru není uložen na disku. Pokud soubor plně je třeba připomenout, odebere synchronizace souboru Azure tento atribut. |

        ![Dialogové okno Vlastnosti pro soubor s kartu s podrobnostmi vybrané](media/storage-files-faq/azure-file-sync-file-attributes.png)
        
        Je také možné najdete v části atributy pro všechny soubory ve složce přidáním **atributy** pole do tabulky zobrazení Exporer souboru. Chcete-li to provést, klikněte pravým tlačítkem na existující sloupec (například velikost), vyberte **Další** a vyberte **atributy** z rozevíracího seznamu.
        
    2. **Použití `fsutil` zkontrolujte spojovacích bodů na soubor.** Jak je uvedeno v předchozí možnost, soubor vrstvené budou mít vždy bod rozboru nebo speciální ukazatel pro nastavení synchronizace souboru Azure filtrem systému souborů (StorageSync.sys). Můžete zkontrolovat, jestli u souboru došlo spojovací bod `fsutil` nástroj na příkazovém řádku se zvýšenými nebo relaci prostředí PowerShell:
    
        ```PowerShell
        fsutil reparsepoint query <your-file-name>
        ```

        Pokud má soubor bod rozboru, měli byste vidět **rozboru hodnota značky: 0x8000001e**. Tato hexadecimální hodnota je hodnota bodu rozboru vlastníkem synchronizace souboru Azure. Výstup bude také obsahovat rozboru, které sdílejí data představující cestu k souboru v souboru Azure.

        > [!Warning]  
        > `fsutil reparsepoint` Nástroj příkaz obsahuje taky možnost Odstranit bod rozboru. Nespouštějte tento příkaz, pokud pokyn technický tým synchronizace souboru Azure – to může způsobit ztrátu dat. 

* <a id="afs-recall-file"></a>**Soubor, který chcete použít byly zřízeny vrstvené... jak ho na disk pro použití místně můžete odvolat?**  
    Nejjednodušší způsob, jak odvolat soubor na disketu je ho otevřete. Synchronizace souboru Azure filtrem systému souborů (StorageSync.sys) bezproblémově stáhněte soubor ze Azure sdílené složky bez nutnosti jakékoli práci. Pro typy souborů, které může být částečně pro čtení, například multimédia nebo soubory zip, otevřením souboru nebudou mít za následek stahování celý soubor.

    Je také možné vynutit soubor, který chcete zrušit pomocí prostředí PowerShell. Například to může být užitečné, pokud chcete odvolat velký počet souborů najednou (například všechny soubory ve složce). Otevřete relaci prostředí PowerShell na uzlu serveru, kde je nainstalován synchronizace souboru Azure a spusťte následující příkazy prostředí PowerShell:
    
    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncFileRecall -Path <file-or-directory-to-be-recalled>
    ```

* <a id="afs-force-tiering"></a>**Jak můžete vynutit soubor nebo adresář, chcete-li být rozvrstvena?**  
    Když je povolené, funkci vrstvení cloudu automaticky vrstvy soubory podle posledního přístupu a změnit časy pro dosažení Procento volné místo svazku, zadaný pro koncový bod cloudu, ale někdy může chcete vynutit soubor k vrstvě ručně. Například to může být užitečné, pokud uložíte velký soubor, který nemáte v úmyslu znovu použít po dlouhou dobu a teď chcete volné místo na svazku pro ostatní soubory či složky. Můžete vynutit vrstvení pomocí následujících příkazů prostředí PowerShell:

    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncCloudTiering -Path <file-or-directory-to-be-tiered>
    ```

* <a id="afs-files-excluded"></a>**Které soubory nebo složky se automaticky vyloučí pomocí synchronizace souboru Azure?**
    Ve výchozím nastavení vyloučí synchronizace souboru Azure následující soubory:
    
    - Desktop.ini
    - Thumbs.DB
    - ehthumbs.DB
    - ~$\*.\*
    - \*LACCDB
    - \*TMP.
    - 635D02A9D91C401B97884B82B3BCDAEA.\*

    Ve výchozím nastavení jsou také vyloučit následující složky:

    - \System volume Information
    - \$RECYKLUJTE. KOŠ
    - \SyncShareState

* <a id="afs-os-support"></a>**Chci používat Azure souboru synchronizaci s Windows Server 2008 R2, s Linuxem, nebo s NAS zařízení - synchronizace souboru Azure podporuje který?**
    V současné době synchronizace souboru Azure podporuje pouze systém Windows Server 2016 a Windows Server 2012 R2. V tuto chvíli nemáme jiných plánů, které jsme můžete sdílet, ale nemohli jsme otevřít a zúčastněné pro další platformy, na základě poptávky zákaznickou podporu. Dejte nám vědět, jaké platformy, které chcete do us pro podporu [Azure soubory UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files).

## <a name="security-authentication-and-access-control"></a>Zabezpečení, ověřování a řízení přístupu
* <a id="ad-support"></a>**Je ověřování na základě služby Active Directory a řízení přístupu podporovanou soubory Azure?**  
    Soubory Azure dva způsoby, jak můžete ke správě řízení přístupu:

    - Pomocí SAS můžete vygenerovat tokeny se specifickými oprávněními, které jsou platné pro zadaný časový interval. Například můžete vygenerovat token s přístupem jen pro čtení pro daný soubor s vypršení platnosti 10 minut. Každý, kdo takový token má po dobu jeho platnosti má přístup jen pro čtení k souboru pro těchto 10 minut. SAS klíče jsou podporovány pouze prostřednictvím rozhraní API REST nebo klient knihovny dnes, je nutné připojit sdílenou složku Azure File prostřednictvím protokolu SMB se klíče účtu úložiště.

    - Synchronizace služby Azure souboru se zachovat a pro všechny koncové body serveru, který je synchronizován se replikovat všechny tyto seznamy ACL (na základě AD nebo místní). Protože Windows Server můžete již ověřit pomocí služby Active Directory, synchronizace souboru Azure může zajistit skvělé míru stop mezera dokud plnou podporu pro ověřování založené na AD a dorazí podporu seznamu ACL.

    Soubory Azure Active Directory přímo v tuto chvíli nepodporuje.

* <a id="encryption-at-rest"></a>**Jak můžete zajistit, že moje sdílenou složku Azure File je šifrovaný na rest?**  
    Šifrování na rest právě probíhá povolené ve výchozím nastavení ve všech oblastech. Pro tyto oblasti nemusíte dělat nic povolit šifrování. Pro další oblasti, přečtěte si [šifrování na straně serveru](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="access-via-browser"></a>**Jak může poskytnout přístup k konkrétní soubor pomocí webového prohlížeče?**  
    Pomocí SAS můžete vygenerovat tokeny se specifickými oprávněními, které jsou platné pro zadaný časový interval. Můžete například vygenerovat token s přístupem pouze pro čtení ke konkrétnímu souboru po určité časové období. Každý, kdo má tuto adresu url můžete přístup k souboru přímo z libovolného webového prohlížeče, ale je platný. Klíče SAS snadno vygenerujete z uživatelského rozhraní, jako je Průzkumník úložišť.

* <a id="file-level-permissions"></a>**Je možné zadat jen pro čtení nebo jen pro zápis oprávnění pro složky ve sdílené složce?**  
    Takovou míru kontroly nad oprávněními nemáte, pokud sdílenou složku připojíte přes SMB. Můžete toho však dosáhnout vytvořením sdíleného přístupového podpisu (SAS) přes REST API nebo knihovny klienta.

* <a id="ip-restrictions"></a>**Můžete implementovat omezení IP adres pro sdílení souborů Azure?**  
    Přístup do sdílené složky Azure File Ano, mohou být omezeny na úrovni účtu úložiště. Další informace najdete v tématu [brány firewall nakonfigurovat úložiště Azure a virtuální sítě](../common/storage-network-security.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="data-compliance-policies"></a>**Co jsou zásady dodržování předpisů dat podporována pro soubory Azure?**  
   Soubory Azure běží nad architektuře úložiště jako ostatní služby úložiště ve službě Azure Storage a platí stejné zásady dodržování předpisů data. Další informace o dodržování předpisů dat úložiště Azure, můžete stáhnout a odkazovat na [dokumentu Microsoft Azure Data Protection](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409) a [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx).

## <a name="on-premises-access"></a>Místní přístup
* <a id="expressroute-not-required"></a>**Je nutné použít Azure ExpressRoute pro připojení k Azure Files nebo můžete používat synchronizaci Azure soubor místně?**  
    Ne, ExpressRoute není potřeba přístup Azure sdílené složky. Pokud jste připojení Azure sdílené složky přímo na místních počítačích všechny které je požadované je, že máte port 445 (odchozí TCP) otevřený pro přístup k Internetu (to je port, který komunikuje protokolu SMB na). Pokud používáte Azure souboru Sync, všechny možnosti, které vyžaduje je port 443 (odchozí TCP) pro přístup k protokolu HTTPS (bez protokolu SMB požadované). ExpressRoute může však použít u obou možností přístupu v případě potřeby.

* <a id="mount-locally"></a>**Jak můžete připojit Azure sdílené složky na místním počítači?**  
    Dokud je otevřený port 445 (odchozí TCP) a váš klient podporuje protokol SMB 3.0 můžete připojit sdílenou složku přes protokol SMB (například používáte Windows 10 nebo Windows Server 2016). Pokud port 445 zablokován podle zásad vaší organizace nebo podle vašeho poskytovatele internetových služeb, můžete pro přístup k Azure sdílené složky synchronizace souboru Azure.

## <a name="backup"></a>Zálohování
* <a id="backup-share"></a>**Jak zálohovat Moje Azure File sdílet?**  
    Můžete použít pravidelné [sdílet snímky (preview)](storage-how-to-use-files-snapshots.md) pro ochranu proti náhodného odstranění. Můžete použít AzCopy, RoboCopy, nebo 3. stran zálohování nástroj, který může zálohovat připojené sdílené složky. 

## <a name="share-snapshots"></a>Sdílet snímky
### <a name="share-snapshots---general"></a>Sdílet snímky – obecné
* <a id="what-are-snaphots"></a>**Co je soubor sdílenou složku snímku?**  
    Azure snímky sdílené složky souborů umožňuje vytvářet verze sdílené složky jen pro čtení. Umožňuje také ke zkopírování starší verze obsahu zpět do stejné složky nebo alternativního umístění v Azure nebo místním další změny. Další informace o sdílené složce snímků, najdete v článku na [sdílet přehled snímku](storage-snapshots-files.md).

* <a id="where-are-snapshots-stored"></a>**Kde jsou uloženy snímky Moje sdílené složky?**  
    Sdílené složky snímků jsou uložené ve stejném účtu úložiště jako sdílené složky.

* <a id="snapshot-perf-impact"></a>**Existují jakékoli ovlivnit výkon?**  
    Sdílené složky snímky nemají žádné nároky na výkon.

* <a id="snapshot-consistency"></a>**Jsou aplikace sdílené složky snímků konzistentní?**  
    Ne. Sdílené složky snímků nejsou aplikace konzistentní. Uživatel bude muset vyprázdnění zápisů z aplikace do sdílené složky před přepnutím sdílenou složku snímku.

* <a id="snapshot-limits"></a>**Je nějak omezený na počet snímků, sdílené složky?**  
    Existuje limit 200 snímků sdílené složky, které můžete zachovat Azure Files. Sdílené složky snímků není započítávat kvóta pro sdílenou složku, je ne za sdílené složky limit celkové místo využívaných všechny snímky sdílené složky. Limity účtu úložiště se stále účtují. Po 200 sdílené složky snímků bude muset starší snímky odstranit, aby bylo možné vytvořit novou sdílenou složku snímků.

### <a name="create-share-snapshots"></a>Vytvoření sdílené složky snímků
* <a id="file-snaphsots"></a>**Můžete vytvořit sdílenou složku snímku jednotlivých souborů?**  
    Sdílené složky snímků jsou vytvořené na úrovni sdílené složky. Můžete obnovit jednotlivé soubory ze sdílené složky snímku souboru, ale nemůžete vytvářet snímky sdílené složky souborů. Ale pokud jste provedli snímku úrovni sdílené složky sdílené složky a chcete vypsat snímky sdílenou složku se změnou určitého souboru, můžete provést tak ze zkušeností "Předchozí verze" na připojenou složku systému Windows. Dejte nám vědět, pokud máte třeba funkce snímku souboru na [Azure soubory UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files).

* <a id="encypted-snapshots"></a>**Můžete vytvořit sdílenou složku snímků sdílení šifrovaných souborů?**  
    Může trvat snímek sdílenou složku Sdílené složky Azure File, který má šifrování v klidovém stavu povoleno. Můžete obnovit soubory ze sdílené složky snímku do šifrované sdílené složky. Pokud je vaše sdílená složka šifrovat, bude také zašifrovaný sdílenou složku snímku.

* <a id="geo-redundant-snaphsots"></a>**Jsou mé sdílené složky snímků geograficky redundantní?**
    Sdílenou složku snímku, bude mít stejnou redundanci jako sdílenou složku Azure File, pro které byly provedeny. Pokud jste vybrali geograficky redundantní úložiště (GRS) pro váš účet sdílenou složku snímku se také být ukládána dodatečně ve spárované oblasti.

### <a name="manage-share-snapshots"></a>Spravovat sdílené složky snímků
* <a id="browse-snapshots-linux"></a>**Můžete procházet Moje snímky sdílenou složku z Linux?**  
    Azure CLI 2.0 slouží k vytváření, výpisu, procházet a obnovení v systému Linux.

* <a id="copy-snapshots-to-other-storage-account"></a>**Můžete zkopírovat snímky sdílenou složku na jiný účet úložiště?**  
    Můžete zkopírovat soubory ze sdílené složky snímků do jiného umístění, ale není snímky sdílené složky sami.

### <a name="restore-data-from-share-snapshots"></a>Obnovení dat ze sdílené složky snímků
* <a id="promote-share-snapshot"></a>**Můžete zvýšit úroveň sdílenou složku snímku do základní složky?**  
    Kopírovat můžete pouze data z sdílenou složku snímku na libovolný požadovaný cíl. Ale nemůžete povýšit sdílenou složku snímku do základní složky.

* <a id="restore-snapshotted-file-to-other-share"></a>**Je možné obnovit data z mé sdílenou složku snímku na jiný účet úložiště?**  
    Ano. Soubory ze sdílené složky snímku je možné zkopírovat do původního nebo alternativního umístění, který obsahuje stejný nebo jiný účet úložiště ve stejné nebo jiné oblasti. Můžete také zkopírovat soubory do místní nebo jiné cloudu.    
  
### <a name="cleanup-share-snapshot"></a>Vyčištění sdílenou složku snímku
* <a id="delete-share-keep-snapshots"></a>**Můžete odstranit mé sdílenou složku ale sdílet snímky?**
    Nebudete moct odstranit vaši sdílenou složku, pokud máte aktivní sdílené složky snímků na sdílené složce. Rozhraní API je k dispozici pro odstranění sdílené složky snímků společně s sdílené složky a stejné z portálu Azure také můžete dosáhnout.

* <a id="delete-share-with-snapshots"></a>**Co se stane Moje sdílené složky snímků, pokud svůj účet úložiště odstranit?**
    Pokud odstraníte účet úložiště, se odstraní také snímky sdílené složky.

## <a name="billing-and-pricing"></a>Fakturaci a cenách
* <a id="vm-file-share-network-traffic"></a>**Počet síťový provoz mezi virtuálním počítači Azure a sdílenou složku Azure jako externí šířku pásma, které jsou zpoplatněné nad rámec předplatného?**  
    Pokud sdílená složka a virtuálních počítačů jsou ve stejné oblasti Azure, provoz mezi nimi je zdarma. Pokud jsou v různých oblastech, provoz mezi nimi bude účtován jako externí šířky pásma.

* <a id="share-snapshot-price"></a>**Kolik sdílí náklady snímky?**
     Při sdílení Preview nebude nic účtováno snímku kapacity. Použít standardní úložiště výstupní a transakce parapetu náklady. Po obecné dostupnosti si kapacitu a transakce na snímek sdílenou složku se bude účtovat poplatek.
     
     Sdílené složky snímky jsou ve své podstatě přírůstkové. Snímek základní sdílená složka je sdílená složka sám sebe. Všechny následné sdílené složce snímky jsou přírůstkové a uloží pouze rozdílový z předchozího snímku sdílené složky. Fakturuje se pouze pro změněný obsah. Pokud máte sdílenou složku se 100 GiB dat, ale jenom 5 GiB se změnila po poslední sdílenou složku snímku, bude snímek sdílení využívá jenom 5 Další GiB a vy nebudete fakturovaná pouze 105 GiB. V tématu [cenová stránky](https://azure.microsoft.com/pricing/details/storage/files/) Další informace o transakci a standardní výstupní poplatky.

## <a name="scale-and-performance"></a>Rozsah a výkon
* <a id="files-scale-limits"></a>**Jaké jsou limity škálování Azure souborů?**  
    Informace o škálovatelnosti a cílech výkonnosti Azure souborů najdete v tématu [Azure Files škálovatelnosti a cílech výkonnosti](storage-files-scale-targets.md).

* <a id="need-larger-share"></a>**Je potřeba větší sdílené složky než soubory Azure aktuálně nabízí... může zvýšit velikost Moje sdílenou složku Azure?**  
    Ne, maximální velikost sdílené složky Azure File je 5 TiB. Toto je momentálně pevný limit, který jsme nelze upravit. Jsme práce na řešení a zvyšte velikost sdílené složky na 100 TiBs, ale nemají časové osy sdílet v tuto chvíli.

* <a id="open-handles-quota"></a>**Kolik klientů mohou přistupovat ke stejnému souboru současně?**  
    Není otevřené popisovače kvótu 2000 na jeden soubor. Jakmile máte 2000 otevřených popisovačů, obdržíte chybu, je dosaženo podílu.

* <a id="zip-slow-performance"></a>**Moje šlo to velmi pomalu při pokusu o rozbalit soubory do Azure Files. Co bych měl/a dělat?**  
    K přenosu velkého počtu souborů do Azure souborů, doporučujeme použít AzCopy (Windows, verze Preview pro Linux a Unix) nebo Azure Powershell jako tyto nástroje jsou optimalizované pro síťové přenosy.

* <a id="slow-perf-windows-81-2012r2"></a>**Moje sdílenou složku Azure File jste po připojení na Windows Server 2012 R2 nebo Windows 8.1 a my je pomalé - proč?**  
    Při připojení Azure sdílené složky na Windows Server 2012 R2 a Windows 8.1, která byla opravit v kumulativní aktualizaci. dubna 2014 pro Windows 8.1 a Windows Server 2012 R2 je známý problém. Zkontrolujte prosím, že všechny instance systému Windows Server 2012 R2 a Windows 8.1 mají tato oprava u optimálního výkonu (samozřejmě vždy doporučujeme trvá všechny opravy Windows pomocí služby Windows Update). Další informace najdete v článku znalostní báze [pomalý výkon při přístupu k Azure Files z Windows 8.1 nebo Server 2012 R2](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Funkce a vzájemná funkční spolupráce s jinými službami
* <a id="cluster-witness"></a>**Můžete použít sdílenou Moje Azure jako *určující sdílenou složku* pro moje clusteru převzetí služeb při selhání Windows serveru?**  
    To není aktuálně podporováno pro sdílení souborů Azure. Další informace o tom, jak nastavit službu Azure Blob storage najdete v tématu [nasazení cloudu určující pro Cluster s podporou převzetí služeb při selhání](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness).

* <a id="containers"></a>**Můžete připojit Azure sdílené složky v instanci Azure Container?** ?  
    Ano, Azure sdílené složky jsou, protože přitahují možností zachovat informace mimo dobu životnosti instance kontejneru. Další informace najdete v tématu [připojení sdílenou složku Azure s Azure kontejner instancí](../../container-instances/container-instances-mounting-azure-files-volume.md).

* <a id="rest-rename"></a>**Je k dispozici v rozhraní REST API operaci přejmenovat?**  
    V tuto chvíli to není možné.

* <a id="nested-shares"></a>**Může mít člověk vnořené sdílené složky, jinými slovy sdílenou složku ve sdílené složce?**  
    Ne. Sdílená složka je virtuální jednotka, kterou můžete připojit, takže vnořené sdílené složky se nepodporují.

* <a id="ibm-mq"></a>**Použití Azure souborů s IBM MQ**  
    Společnost IBM vydala dokument zákazníky IBM MQ Provede při konfiguraci Azure souborů pro jejich službu. Další informace najdete v tématu [Nastavení Manažera fronty více instancí IBM MQ se službou Microsoft Azure File](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service).

## <a name="see-also"></a>Viz také
* [Řešení potíží s Azure soubory v systému Windows](storage-troubleshoot-windows-file-connection-problems.md)
* [Poradce při potížích Azure soubory v systému Linux](storage-troubleshoot-linux-file-connection-problems.md)
* [Řešení potíží s synchronizace souboru Azure (preview)](storage-sync-files-troubleshoot.md)