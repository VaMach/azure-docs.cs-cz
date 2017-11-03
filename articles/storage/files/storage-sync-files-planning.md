---
title: "Plánování nasazení synchronizace souboru Azure (preview) | Microsoft Docs"
description: "Zjistěte, co je potřeba zvážit při plánování nasazení služby Azure Files."
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
ms.openlocfilehash: d626f71aa21cea562ef6c9554c05e6de027e7f4d
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="planning-for-an-azure-file-sync-preview-deployment"></a>Plánování nasazení synchronizace souboru Azure (preview)
Azure File Sync (Preview) umožňuje centralizovat sdílené složky organizace ve službě Soubory Azure bez ztráty flexibility, výkonu a kompatibility místního souborového serveru. Dělá to pomocí transformace serverů Windows na rychlou mezipaměť sdílené složky Azure. Pro místní přístup k datům můžete použít jakýkoli protokol dostupný ve Windows Serveru (včetně SMB, NFS a FTPS) a můžete mít libovolný počet mezipamětí po celém světě.

Tato příručka popisuje, co je potřeba zvážit při nasazování synchronizace souboru Azure. Doporučujeme, abyste si přečetli [plánování nasazení Azure Files](storage-files-planning.md) Průvodce testování. 

## <a name="understanding-azure-file-sync-terminology"></a>Principy terminologie synchronizace souboru Azure
Před přechodem do podrobnosti synchronizace souboru Azure, je důležité, abyste rozuměli technologiím.

### <a name="storage-sync-service"></a>Synchronizační služba úložiště
Synchronizační služba úložiště je nejvyšší úrovně prostředků Azure představující soubor synchronizaci Azure. Úložiště synchronizační služba prostředků je partnerské zařízení prostředků účtu úložiště a dá Wijsova nasadit do skupin odstraňovaného prostředku Azure. Prostředek odlišné nejvyšší úrovně z prostředku účtu úložiště není nutná, protože synchronizační službu úložiště můžete vytvořit synchronizační relace s více účty úložiště prostřednictvím více skupin synchronizace. Předplatné může mít několik úložiště synchronizační služba prostředků nasazení.

### <a name="sync-group"></a>Synchronizace skupiny
Synchronizace skupiny definuje topologie synchronizace pro určitou sadu souborů. Koncové body v rámci skupiny synchronizace se budou uchovávat vzájemně synchronizována. Pokud máte například dvě odlišné skupiny souborů, které chcete spravovat pomocí AFS, by vytvořit dvě synchronizace skupiny a přidejte různými koncovými body pro každý. Synchronizační služba úložiště, může hostovat libovolný počet skupin synchronizace, kolik potřebujete.  

### <a name="registered-server"></a>Zaregistrovaný Server
Objekt zaregistrovaný Server představuje vztah důvěryhodnosti mezi serverem (nebo clusteru) a služby úložiště Sync. Počet serverů do instance úložiště synchronizační služby podle potřeby můžete zaregistrovat. Ale serveru (nebo clusteru) se dají registrovat jenom s jeden synchronizační služby úložiště v daném okamžiku.

### <a name="azure-file-sync-agent"></a>Agent Azure synchronizace souboru
Agent Azure souboru Sync je ke stažení balíčku sdílet tím umožníte a Windows Server mají být synchronizovány s Azure File. Agent Azure souboru Sync se skládá ze tří hlavních součástí: 
- **FileSyncSvc.exe**: na pozadí služby systému Windows je odpovědná za monitorování změn na serveru koncových bodů a inicializaci relace synchronizace do Azure.
- **StorageSync.sys**: systém souborů Azure souboru Sync filtrovat, zodpovědná vrstvení soubory a soubory Azure (když cloudu vrstvení je povoleno).
- **Rutiny prostředí PowerShell pro správu**: rutiny prostředí PowerShell pro interakci s Microsoft.StorageSync poskytovatel prostředků Azure. Ty lze najít v následujících umístěních (ve výchozím nastavení):
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll

### <a name="server-endpoint"></a>Koncový bod serveru
Koncový bod serveru představuje určitého umístění na serveru registrován, například do složky na svazku serveru nebo v kořenovém adresáři svazku. Víc koncových bodů serveru může existovat na stejném svazku, pokud nejsou jejich obory názvů překrývání (např. F:\sync1 a F:\sync2). Můžete nakonfigurovat zásady vrstvení cloudu zvlášť pro každý koncový bod serveru. Pokud přidáte umístění na serveru s existující sadou souborů jako koncový bod serveru do skupiny synchronizace, budou tyto soubory sloučit s další soubory na ostatní koncové body ve skupině synchronizace.

### <a name="cloud-endpoint"></a>Koncového bodu cloudu
Koncový bod cloudu je sdílenou složku Azure File, který je součástí skupiny synchronizace. Celý synchronizacemi sdílenou složku Azure File a sdílenou složku Azure lze pouze členem jeden koncový bod cloudu a proto jednu skupinu synchronizace. Pokud přidáte sdílenou Azure s existující sadou souborů jako koncový bod cloudu do skupiny synchronizace, tyto soubory se sloučí s ostatními soubory už na ostatní koncové body ve skupině synchronizace.

> [!Important]  
> Azure podporuje soubor synchronizaci změn Azure File sdílet přímo, ale mějte na paměti, že veškeré změny provedené na sdílenou složku Azure File nejprve musí být zjištěny úlohou Azure souboru synchronizace změnu detekce, což je pouze initatiated pro koncový bod cloudu každých 24 hodin. Najdete v článku [Azure soubory – nejčastější dotazy](storage-files-faq.md#afs-change-detection) Další informace.

### <a name="cloud-tiering"></a>Vrstvení cloudu 
Cloud vrstvení je volitelná funkce Azure synchronizace souborů, které umožňuje zřídka používají nebo získat přístup k souborům na být rozvrstvena k Azure Files. Když vrstveny. soubor, systém souborů Azure souboru Sync filtrovat (StorageSync.sys) nahradí soubor místně pomocí ukazatele nebo spojovací bod, představující adresu URL k souboru v Azure Files. Vrstvený soubor má atribut "offline" nastavené v systému souborů NTFS, takže aplikace třetích stran můžete identifikovat vrstvené soubory. Když uživatel otevře soubor vrstvené, synchronizace souboru Azure bezproblémově připomíná, že soubor dat z Azure souborů bez uživatele znáte soubor se neukládá místně v systému. Tato funkce se také nazývá správu hierarchických úložišť (HSM).

## <a name="azure-file-sync-interoperability"></a>Interoperabilita synchronizace Azure File 
Tato část obsahuje synchronizace souboru Azure interoperability s funkcí Windows serveru a role a řešeními třetích stran.

### <a name="supported-versions-of-windows-server"></a>Podporované verze systému Windows Server
V současné době jsou podporované verze systému Windows Server pomocí synchronizace souboru Azure:

| Verze | Podporované SKU | Možnosti podporované nasazení |
|---------|----------------|------------------------------|
| Windows Server 2016 | Datacenter a Standard | Úplné (Server s uživatelského rozhraní) |
| Windows Server 2012 R2 | Datacenter a Standard | Úplné (Server s uživatelského rozhraní) |

Budoucí verze systému Windows Server bude přidán jako jejich vydání a starší verze systému Windows, mohou být přidány podle zpětnou vazbu od uživatelů.

> [!Important]  
> Doporučujeme zachovat všechny servery Windows použít s Azure souboru Sync aktuální s nejnovějšími aktualizacemi ze služby Windows Update. 

### <a name="file-system-features"></a>Funkce systému souborů
| Funkce | Podpora stavu | Poznámky |
|---------|----------------|-------|
| Seznamy řízení přístupu (ACL) | Plně podporovány. | Seznamy ACL systému Windows se zachovají pomocí synchronizace souboru Azure a jsou vynucována ve Windows serveru na Server koncové body. Seznamy ACL systému Windows (dosud nejsou) nepodporuje soubory Azure, pokud soubory se k nim přistupuje přímo v cloudu. |
| Pevné odkazy | Přeskočena | |
| Symbolické odkazy | Přeskočena | |
| Přípojné body | Částečně podporována. | Přípojné body můžou být kořenovém koncový bod serveru, ale bude vynechána, pokud obsažené v oboru názvů serveru koncového bodu. |
| Spojovacích bodech | Přeskočena | |
| Body rozboru | Přeskočena | |
| Komprese NTFS | Plně podporovány. | |
| Zhuštěných souborů | Plně podporovány. | Zhuštěné soubory se budou synchronizovat (není blokovaný), ale bude v cloudu jako soubor úplnou synchronizaci. Pokud se obsah souboru se změní v cloudu (nebo na jiném serveru), soubor už nebude zhuštěné, po stažení změn |
| Alternativní datové proudy (reklamy) | Zachovají, ale nejsou synchronizované | |

> [!Note]  
> Jsou podporovány pouze svazky systému souborů NTFS.

### <a name="failover-clustering"></a>Clustering převzetí služeb při selhání
Windows Server Failover Clustering podporuje Azure souboru Sync pro možnost "Souborový Server pro obecné použití" nasazení. Převzetí služeb při selhání není podporováno vytváření clusterů "Souborového serveru Škálovaného na více systémů pro data aplikací" (SOFS) nebo na Clusterované sdílené svazky (CSV).

> [!Note]  
> Agent synchronizace souboru Azure musí být nainstalován na všech uzlech v clusteru převzetí služeb při selhání pro synchronizaci správně fungovat.

### <a name="windows-server-data-deduplication"></a>Odstranění duplicitních dat Windows serveru
Pro svazky bez cloudu vrstvení povolená synchronizace souboru Azure podporuje odstranění duplicitních dat zapínání na svazku. Interoperabilita mezi Azure soubor synchronizaci s cloudem vrstvení povoleno a odstranění duplicitních dat v tuto chvíli není podporována.

### <a name="anti-virus-solutions"></a>Ochrana proti virům řešení
Vzhledem k tomu, že antivirový funguje tak, že kontrola soubory pro známého škodlivého kódu, ochrana proti virům může způsobit odvolání vrstvené souborů. Vzhledem k tomu, že vrstvené soubory mají atribut "offline" nastaven, doporučujeme poradě s dodavatelem softwaru o tom, jak nakonfigurovat své řešení tak, aby přeskočil čtení offline soubory. 

Následující řešení zaručeně podporují přeskočení offline soubory:

- [Symantec Endpoint Protection](https://support.symantec.com/en_US/article.tech173752.html)
- [Zabezpečení koncového bodu McAfee](https://kc.mcafee.com/resources/sites/MCAFEE/content/live/PRODUCT_DOCUMENTATION/26000/PD26799/en_US/ens_1050_help_0-00_en-us.pdf) (najdete v části "Kontrola pouze co je potřeba" na stránce 90)
- [Ochrana proti virům Kaspersky](https://support.kaspersky.com/4684)
- [Sophos Endpoint Protection](https://community.sophos.com/kb/en-us/40102)
- [TrendMicro OfficeScan](https://success.trendmicro.com/solution/1114377-preventing-performance-or-backup-and-restore-issues-when-using-commvault-software-with-osce-11-0#collapseTwo) 

### <a name="backup-solutions"></a>Řešení zálohování
Jako řešení antivirové řešení zálohování může způsobit odvolání vrstvené souborů. Doporučujeme, abyste pomocí řešení cloudového zálohování k zálohování Azure sdílené složky, nikoli pomocí produktu pro zálohování na místě.

### <a name="encryption-solutions"></a>Řešení pro šifrování
Podpora pro šifrování řešení závisí na tom, jak jsou implementované. Synchronizace služby Azure souboru je znám pracovat:

- Šifrování nástrojem BitLocker
- Azure RMS (a starší verze Active Directory RMS)

Synchronizace služby Azure souboru není znám pracovat:

- Systém souborů NTFS šifrované systém souborů (EFS Encrypting File)

Obecně platí synchronizace souboru Azure měli být schopní Interoperabilita s řešeními šifrování, které se nacházejí níže systému souborů, například nástrojem BitLocker a řešení, které jsou implementované ve formátu souboru, například nástroje BitLocker, ale žádné speciální zprostředkovatel komunikace s objekty byl proveden Interoperabilita s řešeními, které se nacházejí výše systému souborů (jako jsou zašifrované systémem souborů NTFS).

### <a name="other-hierarchical-storage-management-hsm-solutions"></a>Jiných řešení pro správu hierarchických úložišť (HSM)
Žádné jiné řešení pro správu hierarchické úložiště se má použít s Azure synchronizace souboru.

## <a name="region-availability"></a>Dostupnost v oblastech
Synchronizace služby Azure souboru je k dispozici pouze v následujících oblastech ve verzi Preview:

| Oblast | Umístění datového centra |
|--------|---------------------|
| Západní USA | Kalifornské, USA |
| Západní Evropa | Nizozemsko |
| Jihovýchodní Asie | Singapur |
| Austrálie – východ | Nové Jižní Morava, Austrálie |

Ve verzi Preview podporujeme jenom synchronizace se službou Azure sdílenou ve stejné oblasti jako službu úložiště synchronizace.

## <a name="azure-file-sync-agent-update-policy"></a>Zásady aktualizace agenta Azure File Sync
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="next-steps"></a>Další kroky
* [Plánování nasazení Azure Files](storage-files-planning.md)
* [Soubory nasazení Azure](storage-files-deployment-guide.md)
* [Nasazení Azure File synchronizace](storage-sync-files-deployment-guide.md)
