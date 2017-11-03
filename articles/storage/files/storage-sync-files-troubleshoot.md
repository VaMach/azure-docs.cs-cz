---
title: "Řešení potíží s synchronizace souboru Azure (preview) | Microsoft Docs"
description: "Řešení běžných problémů s Azure souboru Sync"
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 6bc5f2da2b8628671037b9257db746e73cd3afad
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="troubleshoot-azure-file-sync-preview"></a>Řešení potíží s synchronizace souboru Azure (preview)
Azure File Sync (Preview) umožňuje centralizovat sdílené složky organizace ve službě Soubory Azure bez ztráty flexibility, výkonu a kompatibility místního souborového serveru. Dělá to pomocí transformace serverů Windows na rychlou mezipaměť sdílené složky Azure. Pro místní přístup k datům můžete použít jakýkoli protokol dostupný ve Windows Serveru (včetně SMB, NFS a FTPS) a můžete mít libovolný počet mezipamětí po celém světě.

Tento článek slouží můžete odstraňovat potíže a řešit problémy s nasazením Azure synchronizace souboru. Pokud neexistuje Tento průvodce ukazuje, jak shromažďovat důležité protokoly ze systému, které pomáhají při hlubší rozbor problémy. Pokud nevidíte odpověď na svoji otázku zde, neváhejte a oslovení nám těmito cestami (v narůstajícím pořadí):

1. V sekci komentáře tohoto článku.
2. [Fórum pro Azure Storage](https://social.msdn.microsoft.com/Forums/home?forum=windowsazuredata)
3. [Soubory Azure UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) 
4. Microsoft Support: Chcete-li vytvořit nové případu podpory, přejděte na "Nápověda a podpora" kartě na portálu Azure a klikněte na možnost "Nová žádost o podporu".

## <a name="agent-installation-and-server-registration"></a>Instalace a server registrace agenta
<a id="agent-installation-failures"></a>**Řešení potíží s chybami instalace agenta**  
Pokud instalace agenta Azure souboru synchronizace selhává, spusťte následující příkaz z příkazového řádku se zvýšenými povolení protokolování během instalace agenta:

```
StorageSyncAgent.msi /l*v Installer.log
```

Jakmile se instalace nezdaří, zkontrolujte installer.log zjistit příčinu. 

> [!Note]  
> Instalace agenta se nezdaří, pokud si zvolíte používání služby Microsoft Update a není spuštěná služba Windows Update.

<a id="server-registration-missing"></a>**Server není uveden v části registrované servery na portálu Azure**  
Pokud server není uveden v části registrované servery pro synchronizační služby úložiště, proveďte následující kroky:
1. Přihlášení k serveru, který chcete zaregistrovat.
2. Otevřete Průzkumníka souborů a přejděte do instalačního adresáře agenta synchronizace úložiště (výchozí umístění je `C:\Program Files\Azure\StorageSyncAgent`). 
3. Spusťte ServerRegistration.exe a postupujte podle pokynů Průvodce registrace serveru se službou Sync úložiště.

<a id="server-already-registered"></a>**Registrace serveru zobrazí následující zprávu po instalaci agenta Azure Sync souboru: "Tento server je již zaregistrována"**  
![Snímek obrazovky dialogového okna registrace serveru s chybou "serveru je již zaregistrován" zpráv](media/storage-sync-files-troubleshoot/server-registration-1.png)

Tato zpráva se zobrazí, pokud server byl dříve registrován u služby synchronizace úložiště. Zrušení registrace serveru u služby synchronizace aktuální úložiště a zaregistrujte s novou synchronizační službu úložiště, postupujte podle kroků pro [zrušit registraci serveru s Azure souboru Sync](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

Pokud tento server není uveden v části servery zaregistrované v úložišti služby synchronizace, spusťte následující příkazy prostředí PowerShell na serveru, který chcete zrušit registraci:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Reset-StorageSyncServer
```

> [!Note]  
> Pokud je server součástí clusteru, je volitelný `Reset-StorageSyncServer -CleanClusterRegistration` parametr, který odstraní také registrace v clusteru.

<a id="web-site-not-trusted"></a>**Při registraci serveru získat mnoho "webový server není důvěryhodný" odpovědí, proč?**  
K této chybě dojde, protože **rozšířeného zabezpečení aplikace Internet Explorer** je povolena při registraci serveru. Další informace o tom, jak zakázat správně **rozšířeného zabezpečení aplikace Internet Explorer** zásady, najdete v části [Příprava serverů Windows pro použití s Azure souboru Sync](storage-sync-files-deployment-guide.md#prepare-windows-servers-for-use-with-azure-file-sync) a [jak nasadit Azure File Synchronizace (preview)](storage-sync-files-deployment-guide.md).

## <a name="sync-group-management"></a>Synchronizace skupiny správy
<a id="cloud-endpoint-using-share"></a>**Vytvoření koncového bodu cloudu se nezdaří s následující chybou: "Zadaný Azure sdílení souborů je již používán jinou CloudEndpoint"**  
K této chybě dojde, pokud sdílenou složku Azure File je již používán jiným koncovým bodem cloudu. 

Pokud tato chyba se zobrazují a sdílenou složku Azure File není aktuálně používán koncového bodu cloudu, proveďte následující kroky zrušte Azure souboru synchronizovat metadata pro sdílenou složku Azure File:

> [!Warning]  
> Odstranění metadata na sdílenou složku Azure File, který je aktuálně používán koncového bodu cloudu způsobí selhání operací synchronizace souboru Azure. 

1. Přejděte do sdílené složky Azure File na portálu Azure.  
2. Klikněte pravým tlačítkem na sdílenou složku Azure File a vyberte **upravit metadata**
3. Klikněte pravým tlačítkem na SyncService a vyberte **odstranit**.

<a id="cloud-endpoint-authfailed"></a>**Vytvoření koncového bodu cloudu selže s chybou: AuthorizationFailed**  
K tomuto problému dochází, pokud váš uživatelský účet nemá dostatečná práva k vytvoření koncového bodu cloudu. 

Pokud chcete vytvořit koncový bod cloudu, váš uživatelský účet musí mít následující oprávnění Authorization Microsoft:  
* Přečtěte si: Získání definice role
* Zápisu: Vytvořit nebo aktualizovat vlastní definici role
* Přečtěte si: Získat přiřazení role
* Zápisu: Vytvořit přiřazení role

Následující předdefinované role má příslušná oprávnění Authorization Microsoft:  
* Vlastník
* Správce přístupu uživatelů

Pokud chcete zjistit, zda vaše uživatelská role účet má příslušná oprávnění, postupujte takto:  
* Klikněte na tlačítko **skupiny prostředků** na portálu Azure
* Vyberte skupinu prostředků, kde je umístěn účet úložiště a klikněte na tlačítko **(IAM) řízení přístupu**
* Klikněte **role** (např. vlastník, Přispěvatel) pro váš uživatelský účet.
* V **poskytovatele prostředků** seznamu, vyberte **Authorization Microsoft** 
* **Přiřazení role** by měl mít **čtení** a **oprávnění k zápisu**
* **Definice role** by měl mít **čtení** a **oprávnění k zápisu**

<a id="cloud-endpoint-deleteinternalerror"></a>**Odstranění koncového bodu selže s chybou: MgmtInternalError**  
Tento problém může dojít, pokud byl účet sdílenou složku nebo úložiště Azure File smazán před odstraněním koncového bodu cloudu. Tento problém bude opraven v budoucí aktualizaci a lze je odstranit koncový bod cloudu.

Chcete-li zabránit výskytu tohoto problému, odstraňte koncového bodu cloudu před odstraněním účet sdílenou složku nebo úložiště Azure File.

## <a name="sync"></a>Sync
<a id="afs-change-detection"></a>**Vytvoření souboru přímo v mé sdílenou složku Azure File prostřednictvím protokolu SMB nebo prostřednictvím portálu. Jak dlouho, dokud je soubor synchronizované s servery ve skupině synchronizace?**  
[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="broken-sync"></a>**Řešení potíží s synchronizace nefunguje na serveru**  
Pokud se synchronizace selhává na serveru, postupujte takto:
- Ověřte, že koncový bod serveru existuje na portálu Azure pro adresář, který chcete synchronizovat do Azure sdílené složky:
    
    ![Snímek obrazovky skupiny synchronizace s Cloud a koncový bod serveru na portálu Azure](media/storage-sync-files-troubleshoot/sync-troubleshoot-1.png)

- Zkontrolujte protokoly událostí funkčnosti a diagnostiku pomocí prohlížeče událostí najdete v `Applications and Services\Microsoft\FileSync\Agent`.
- Potvrďte, že má server připojení k Internetu.
- Ověření synchronizace souboru Azure je spuštěná na serveru otevřením modulu snap-in konzoly MMC služby a ověřte, zda že je spuštěna Služba agenta synchronizace úložiště (FileSyncSvc).

<a id="replica-not-ready"></a>**Synchronizace selže s chybou: 0x80c8300f - replika není připraven k provedení požadované operace.**  
Tato chyba se očekává, pokud chcete vytvořit koncový bod cloudu a použít sdílenou složku Azure File, který obsahuje data. Po dokončení detekce změn na sdílenou složku Azure File (může trvat až 24 hodin) synchronizace by měla začít pracovat správně.

<a id="broken-sync-files"></a>**Řešení potíží s jednotlivé soubory selhání pro synchronizaci**  
Pokud jednotlivé soubory se nedaří provést synchronizaci, proveďte následující kroky:
- Zkontrolujte protokoly událostí funkčnosti a Diagnostika v rámci `Applications and Services\Microsoft\FileSync\Agent` v prohlížeči událostí
- Ověřte, že neexistují žádné otevřené popisovače souboru
    - Poznámka: Synchronizace služby Azure soubor bude pravidelně pořízení snímků služby Stínová kopie svazku pro synchronizaci souborů s otevřenými popisovači

## <a name="cloud-tiering"></a>Vrstvení cloudu 
<a id="files-fail-tiering"></a>**Řešení potíží se soubory, které se nepodařilo vrstvy**  
Pokud se soubory se nepodaří vrstvy k Azure Files, proveďte následující kroky:

- Ověřte, že soubory, které existují v Azure sdílené složky
    - Poznámka: Soubor musí být synchronizovat do Azure sdílené složky předtím, než může být rozvrstvena
- Zkontrolujte provozní a Diagnostika protokoly událostí, umístěná `Applications and Services\Microsoft\FileSync\Agent` v prohlížeči událostí
- Potvrďte, že má server připojení k Internetu 
- Ověřte, zda že jsou spuštěné ovladače filtru synchronizace souboru Azure (StorageSync.sys & StorageSyncGuard.sys)
    - Otevřete příkazový řádek se zvýšenými oprávněními, spusťte `fltmc` a ověřte, jsou uvedeny StorageSync.sys a StorageSyncGuard.sys filtru ovladače systému souborů

<a id="files-fail-recall"></a>**Řešení potíží se soubory, které se nepodařilo zrušit**  
Pokud soubory se nepodařilo zrušit, proveďte následující kroky:
- Zkontrolujte provozní a Diagnostika protokoly událostí, umístěná `Applications and Services\Microsoft\FileSync\Agent` v prohlížeči událostí
- Ověřte, zda že existuje soubory do sdílené složky Azure
- Potvrďte, že má server připojení k Internetu 
- Ověřte, zda že jsou spuštěné ovladače filtru synchronizace souboru Azure (StorageSync.sys & StorageSyncGuard.sys)
    - Otevřete příkazový řádek se zvýšenými oprávněními, spusťte `fltmc` a ověřte, jsou uvedeny StorageSync.sys a StorageSyncGuard.sys filtru ovladače systému souborů

<a id="files-unexpectedly-recalled"></a>**Řešení potíží s soubory se neočekávaně vrácené na serveru**  
Ochrana proti virům, zálohování a jiné aplikace, které čtou velkého počtu souborů způsobí, že navrácení žádoucí, pokud jejich respektují atribut offline a přeskočit, je obsah těchto souborů. Přeskočení offline soubory pro tyto produkty, které ji podporují pomáhá zabránit nežádoucí navrácení při provádění operací, jako jsou antivirové kontroly nebo úlohy zálohování.

Požádejte dodavatele softwaru o tom, jak nakonfigurovat své řešení tak, aby přeskočil čtení offline soubory.

Další nežádoucí navrácení může dojít v jiných scénářích jako procházení souborů v Průzkumníku souborů. Otevření složky s vrstvami cloudu souborů v Průzkumníku souborů na serveru může způsobit nežádoucí navrácení, více, pokud je na serveru povoleno antivirový.

## <a name="general-troubleshooting"></a>Obecné řešení potíží
Pokud dojde k potížím se synchronizací souboru Azure na serveru, spusťte provedením následujících akcí:
- Zkontrolujte protokoly událostí diagnostiky a Operational v prohlížeči událostí
    - Synchronizace, vrstvení, a pro vyvolání problémy se protokolují v protokolech událostí diagnostiky a funkční v části`Applications and Services\Microsoft\FileSync\Agent`
    - Problémy správu serveru (například nastavení konfigurace) přihlášeni diagnostiky a provozní protokoly událostí v části`Applications and Services\Microsoft\FileSync\Management`
- Ověřte, zda že je na serveru spuštěna služba synchronizace souboru Azure
    - Otevřete modul snap-in konzoly MMC služby a ověřte, zda že je spuštěna Služba agenta synchronizace úložiště (FileSyncSvc)
- Ověřte, zda že jsou spuštěné ovladače filtru synchronizace souboru Azure (StorageSync.sys & StorageSyncGuard.sys)
    - Otevřete příkazový řádek se zvýšenými oprávněními, spusťte fltmc a ověřte, zda že jsou uvedeny StorageSync.sys a StorageSyncGuard.sys filtru ovladače systému souborů

Pokud není problém vyřešen po provedení výše uvedených kroků, spusťte nástroj AFSDiag provedením následujících kroků:
1. Vytvořte adresář, který se použije k uložení AFSDiag výstup (například c:\output).
2. Otevřete okno prostředí PowerShell se zvýšenými oprávněními a spusťte následující příkazy (stiskněte klávesu enter po každém z nich):

    ```PowerShell
    cd "c:\Program Files\Azure\StorageSyncAgent"
    Import-Module .\afsdiag.ps1
    Debug-Afs c:\output # Note: use the path created in step 1
    ```

3. Úroveň trasování synchronizace souboru Azure jádra režimu zadejte hodnotu 1 (Pokud je určena k vytvoření více podrobného trasování) a stisknutí klávesy enter.
4. Úroveň trasování synchronizace souboru Azure uživatele režimu zadejte hodnotu 1 (Pokud je určena k vytvoření více podrobného trasování) a stisknutí klávesy enter.
5. Reprodukujte problém a stiskněte klávesu D po dokončení.
6. Soubor ZIP obsahující protokolů a trasovací soubory bude v zadanému výstupnímu adresáři.

## <a name="see-also"></a>Viz také
- [Nejčastější dotazy k Azure soubory](storage-files-faq.md)
- [Řešení potíží s Azure soubory v systému Windows](storage-troubleshoot-windows-file-connection-problems.md)
- [Poradce při potížích Azure soubory v systému Linux](storage-troubleshoot-linux-file-connection-problems.md)
