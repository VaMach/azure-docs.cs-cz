---
title: "Šifrování služby úložiště Azure pro Data v klidovém stavu | Microsoft Docs"
description: "Použít funkci šifrování služby úložiště Azure pro šifrování Azure Blob Storage na straně služby při ukládání dat a při získávání data ho dešifrovat."
services: storage
documentationcenter: .net
author: tamram
manager: timlt
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: tamram
ms.openlocfilehash: 772c36c8310a4bf30c62def507382fe74427e0d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Šifrování služby Azure Storage pro neaktivní uložená data
Azure Storage Service šifrování (SSE) pro Data v klidovém stavu pomáhá chránit a ochranu dat, aby splňovaly vaše organizace zabezpečení a dodržování předpisů závazky. Pomocí této funkce Azure Storage automaticky šifruje vaše data před uložením do úložiště a dešifruje před načtení. Šifrování, dešifrování a správu klíčů jsou pro uživatele zcela transparentní.

Následující části obsahují podrobné informace o tom, jak pomocí funkce šifrování služby úložiště a také Podporované scénáře a uživatele dojde.

## <a name="overview"></a>Přehled
Úložiště Azure poskytuje komplexní sadu funkcí zabezpečení, které společně umožňují vývojářům vytvářet aplikace zabezpečené. Data byla zabezpečená během přenosu mezi aplikací a Azure pomocí [šifrování na straně klienta](../storage-client-side-encryption.md), HTTPs nebo SMB 3.0. Šifrování služby úložiště poskytuje šifrování v klidovém stavu, zpracování šifrování, dešifrování a správu klíčů způsobem zcela transparentní. Všechna data se šifrují pomocí 256 bitů [šifrování AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednu z nejsilnějších bloku šifer k dispozici.

SSE funguje tak, že šifrování dat, když je zapsán do úložiště Azure a lze použít pro Azure Blob Storage a úložiště File. Funguje pro následující:

* Standardní úložiště: Účtů úložiště pro obecné účely pro objekty BLOB a souborů, úložiště a účty úložiště Blob
* Storage úrovně Premium 
* Všechny redundance úrovně (LRS, ZRS, GRS a RA-GRS)
* Účty úložiště Azure Resource Manager (ale ne classic) 
* Všechny oblasti.

Další informace naleznete v části Nejčastější dotazy.

Pokud chcete povolit nebo zakázat šifrování služby úložiště pro účet úložiště, přihlaste se [portál Azure](https://portal.azure.com) a vyberte účet úložiště. V okně nastavení vyhledat v části služby objektů Blob, jak je vidět na tomto snímku obrazovky a klikněte na šifrování.

![Možnost šifrování portálu snímek obrazovky zobrazující](./media/storage-service-encryption/image1.png)
<br/>*Obrázek 1: Povolení SSE pro služby objektů Blob (krok 1)*

![Možnost šifrování portálu snímek obrazovky zobrazující](./media/storage-service-encryption/image3.png)
<br/>*Obrázek 2: Povolení SSE pro služby souborů (krok 1)*

Po kliknutí na tlačítko Nastavení šifrování, můžete povolit nebo zakázat šifrování služby úložiště.

![Vlastnosti šifrování portálu snímek obrazovky zobrazující](./media/storage-service-encryption/image2.png)
<br/>*Obrázek 3: Povolení SSE pro objekt Blob a souboru služby (krok 2)*

## <a name="encryption-scenarios"></a>Šifrování scénáře
Šifrování služby úložiště se dá nastavit na úrovni účtu úložiště. Jakmile bude povoleno, bude zákazníkům zvolte služby, které k šifrování. Podporuje následující scénáře zákazníka:

* Šifrování úložiště objektů Blob a File Storage v účtech Resource Manager.
* Šifrování objektů Blob a služba souborů v klasické účty úložiště jednou k migraci účtů úložiště Resource Manager.

SSE má následující omezení:

* Šifrování klasické účty úložiště není podporováno.
* Stávající Data - SSE šifruje nově vytvořeného datového pouze po povolení šifrování. Pokud například vytvoříte nový účet úložiště Resource Manager, ale nikdy nezapínat na šifrování a pak nahrajte k tomuto účtu úložiště objektů BLOB nebo archivovaný virtuální pevné disky a potom zapněte SSE, nebude možné tyto objekty BLOB zašifrované, pokud jsou přepsaná nebo zkopírovat.
* Podpora Marketplace - povolit šifrování virtuálních počítačů vytvořena z Marketplace pomocí [portál Azure](https://portal.azure.com), prostředí PowerShell a rozhraní příkazového řádku Azure. Základní image virtuálního pevného disku se zůstat nezašifrovaný; ale bude se šifrovat všech zápisu provést poté, co má spuštěné virtuální počítač.
* Tabulky a fronty data nebudou šifrována.

## <a name="getting-started"></a>Začínáme
### <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Krok 1: [vytvořit nový účet úložiště](../storage-create-storage-account.md).
### <a name="step-2-enable-encryption"></a>Krok 2: Povolit šifrování.
Můžete povolit pomocí šifrování [portál Azure](https://portal.azure.com).

> [!NOTE]
> Pokud chcete prostřednictvím kódu programu povolit nebo zakázat šifrování služby úložiště na účet úložiště, můžete použít [REST API služby Azure Storage prostředků zprostředkovatele](https://msdn.microsoft.com/library/azure/mt163683.aspx), [prostředků zprostředkovatele Klientská knihovna pro úložiště pro .NET ](https://msdn.microsoft.com/library/azure/mt131037.aspx), [Prostředí azure PowerShell](/powershell/azureps-cmdlets-docs), nebo [rozhraní příkazového řádku Azure](../storage-azure-cli.md).
> 
> 

### <a name="step-3-copy-data-to-storage-account"></a>Krok 3: Kopírování dat do účtu úložiště
Pokud povolíte SSE služby objektů Blob, bude se šifrovat všechny objekty BLOB, které jsou k tomuto účtu úložiště. Všechny objekty BLOB již nachází v daném účtu úložiště nebudou šifrována, dokud budou přepsána. Kopírování dat z jeden účet úložiště do s SSE šifrovaná, nebo i povolit SSE a kopírovat z jednoho kontejneru objektů BLOB do jiného pro jistotu, že předchozí data jsou zašifrovaná. K tomu můžete použít některý z následujících nástrojů. Toto je stejné chování pro úložiště File také.

#### <a name="using-azcopy"></a>Pomocí AzCopy
AzCopy je nástroj příkazového řádku systému Windows určené pro kopírování dat do a z úložiště objektů Blob v Microsoft Azure, souboru a tabulky pomocí jednoduchých příkazů optimální výkon. Můžete použít pro kopírování objektů BLOB nebo soubory z jeden účet úložiště do jiné takový, který má SSE povoleno. 

Chcete-li získat další informace, navštivte [přenos dat pomocí nástroje příkazového řádku Azcopy](storage-use-azcopy.md).

#### <a name="using-smb"></a>Pomocí protokolu SMB
Soubory Azure nabízí sdílené složky v cloudu přes standardní protokol SMB. Můžete připojit sdílenou složku z klienta na místní nebo v Azure. Jakmile připojen, lze nástrojů, například Robocopy pro kopírování souborů přes do sdílené složky Azure. Další informace najdete v tématu [tom, jak připojit Azure sdílení souborů v systému Windows](../files/storage-how-to-use-files-windows.md) a [tom, jak připojit sdílenou složku Azure v systému Linux](../files/storage-how-to-use-files-linux.md).


#### <a name="using-the-storage-client-libraries"></a>Použití knihovny klienta úložiště
Data objektů blob nebo soubor můžete zkopírovat do a z úložiště objektů blob nebo mezi účty úložiště pomocí našich širokou škálu knihovny klienta úložiště včetně .NET, C++, Java, Android, Node.js, PHP, Python nebo Ruby.

Chcete-li získat další informace, navštivte naše [Začínáme s Azure Blob storage pomocí rozhraní .NET](../blobs/storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>Pomocí Průzkumníka úložiště
Průzkumníka úložiště můžete vytvořit účty úložiště, odesílání a stahování dat, zobrazení obsahu objektů BLOB a procházení adresářů. Můžete použít jednu z těchto k nahrání objektů BLOB k vašemu účtu úložiště s povolit šifrování. Pomocí některé průzkumníci úložiště můžete také zkopírovat data z existující úložiště objektů blob do jiného kontejneru v účtu úložiště nebo nový účet úložiště, který má SSE povolena.

Chcete-li získat další informace, navštivte [Průzkumníci úložiště Azure](../storage-explorers.md).

### <a name="step-4-query-the-status-of-the-encrypted-data"></a>Krok 4: Dotaz na stav šifrovaná data
Byly nasazeny aktualizovanou verzi knihovny klienta úložiště, který umožňuje dotazování na stav objektu k určení, pokud je zašifrovaná nebo ne. Toto je aktuálně dostupné jen pro úložiště objektů Blob. Podpora pro úložiště souborů je plánovaná. 

Do té doby můžete volat [získat vlastnosti účtu](https://msdn.microsoft.com/library/azure/mt163553.aspx) ověřte, zda má účet úložiště povolit šifrování nebo zobrazit vlastnosti účtu úložiště na portálu Azure.

## <a name="encryption-and-decryption-workflow"></a>Šifrování a dešifrování pracovního postupu
Zde je stručný popis pracovního postupu, šifrování a dešifrování:

* Zákazník povoluje šifrování na účtu úložiště.
* Když zákazník zapisuje nová data (PUT Blob bloku PUT PUT stránky, UMÍSTIT soubor atd) do úložiště objektů Blob nebo soubor; každém zápisu jsou šifrována pomocí 256 bitů [šifrování AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednu z nejsilnějších bloku šifer k dispozici.
* Když zákazník potřebuje přístup k datům (získání objektu Blob atd.), je před vrácením uživatele automaticky dešifrovaný data.
* Pokud šifrování je zakázáno, nové zápisy již nejsou zašifrované a existující šifrovaná data zůstává zašifrovaný, dokud přepsaná uživatelem. Když je šifrování povoleno, bude se šifrovat zápisy do úložiště objektů Blob nebo souboru. Stav data nezmění se uživatel při přepínání mezi povolení nebo zákaz šifrování pro účet úložiště.
* Všechny šifrovací klíče jsou uložené, zašifrovaná a spravován společností Microsoft.

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Nejčastější dotazy o šifrování služby úložiště pro Data v klidovém stavu
**Otázka: je nutné stávající účet úložiště classic. Můžete povolit SSE na něm?**

Odpověď: Ne, SSE je podporována pouze na účty úložiště Resource Manager.

**Otázka: jak můžete šifrovat data ve svůj účet úložiště classic?**

Odpověď: můžete vytvořit nový účet úložiště Resource Manager a kopírování dat pomocí [AzCopy](storage-use-azcopy.md) z existujícího účtu úložiště classic na váš nově vytvořený účet úložiště Resource Manager. 

Pokud provádíte migraci účtu úložiště classic do Resource Manager účtu úložiště, tato operace je okamžitou, změní typ účtu, ale neovlivňuje stávající data. Žádná nová data zapsána zašifruje až po aktivaci šifrování. Další informace najdete v tématu [platformy podporované migrace z IaaS prostředky z Classic do Resource Manager](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/). Upozorňujeme, že to je podporována pouze pro služby objektů Blob a souboru.

**Otázka: je nutné stávající účet úložiště Resource Manager. Můžete povolit SSE na něm?**

Odpověď: Ano, ale jenom nově zapisovat data budou šifrovány. Není přejděte zpět a šifrování dat, který již byl uložen. Ještě nejsou podporované pro náhled souboru úložiště.

**Otázka: nastavit jako k šifrování aktuální data v existující účet úložiště Resource Manager?**

Odpověď: můžete povolit SSE kdykoli v účtu úložiště, Resource Manager. Data, které byly již existuje však nebudou šifrována. Existující data šifrovat, můžete je zkopírovat na jiný název nebo jiný kontejner a pak odeberte nezašifrované verze.

**Otázka: používám storage úrovně Premium; můžete použít SSE?**

Odpověď: Ano SSE je podporována na úložiště úrovně Standard a Premium Storage.  Storage úrovně Premium není podporována pro službu souboru.

**Otázka: Pokud lze vytvořit nový účet úložiště a povolit SSE a pak vytvořit nový virtuální počítač pomocí tohoto účtu úložiště, znamená to, že virtuální počítač je zašifrovaná?**

Odpověď: Ano. Disky vytvořené, které používají nový účet úložiště budou zašifrovaná, tak dlouho, dokud se vytvářely po povolení SSE. Pokud virtuální počítač byl vytvořen pomocí Azure Marketplace, zůstane základní image virtuálního pevného disku nezašifrované; ale bude se šifrovat všech zápisu provést poté, co má spuštěné virtuální počítač.

**Otázka: je možné vytvořit nové účty úložiště s SSE povolit pomocí Azure PowerShell a rozhraní příkazového řádku Azure?**

Odpověď: Ano.

**Otázka: jak mnohem víc Azure Storage náklady Pokud je povoleno SSE?**

Odpověď: není bez dalších nákladů.

**Otázka: kdo spravuje šifrovacích klíčů?**

Odpověď: klíče jsou spravované microsoftem.

**Otázka: je možné použít vlastní šifrovací klíče?**

Odpověď: pracujeme na poskytnutí možnosti pro zákazníky, aby vlastní šifrovací klíče.

**Otázka: je možné odvolat přístup k šifrovacím klíčům?**

Odpověď: není v tuto chvíli; klíče jsou plně spravované microsoftem.

**Otázka: je SSE ve výchozím nastavení povolené, po vytvoření nového účtu úložiště?**

Odpověď: SSE není povoleno ve výchozím nastavení; na portálu Azure můžete ji povolit. Můžete také prostřednictvím kódu programu povolit tuto funkci pomocí rozhraní REST API poskytovatele prostředků úložiště.

**Otázka: jak se to liší od Azure Disk Encryption?**

Odpověď: Tato funkce slouží k šifrování dat v úložišti objektů Blob v Azure. Azure Disk Encryption se používá k šifrování operačního systému a datové disky ve virtuálních počítačů IaaS. Další podrobnosti, navštivte naše [Průvodce zabezpečením úložiště](../storage-security-guide.md).

**Otázka: Co když povolit SSE a potom přejděte a povolit Azure Disk Encryption na disky?**

Odpověď: to bude fungovat bez problémů. Vaše data se budou šifrovat pomocí obou metod.

**Otázka: svůj účet úložiště je nastavený na geo redundantně replikovat. Pokud lze povolit SSE, budou mé redundantní kopie také zašifrovaná?**

Odpověď: Ano, všechny kopie účtu úložiště se šifrují, a podporují všechny možnosti redundance – místně redundantní úložiště (LRS), zóny-Redundant Storage (ZRS), Geo-Redundant Storage (GRS) a přístup pro čtení Geo-Redundant Storage (RA-GRS) –.

**Otázka: I nelze povolit šifrování na svůj účet úložiště.**

Odpověď: je účet správce prostředků úložiště? Klasické účty úložiště nejsou podporovány. 

**Otázka: je SSE povolen pouze v určitých oblastí?**

Odpověď: na SSE je k dispozici ve všech oblastech pro úložiště objektů Blob. Zkontrolujte dostupnost část úložiště souborů. 

**Otázka: jak I contact někdo po jakýchkoli problémů nebo chcete poskytnout zpětnou vazbu?**

A: Obraťte se prosím na [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) pro veškeré problémy související s šifrování služby úložiště.

## <a name="next-steps"></a>Další kroky
Úložiště Azure poskytuje komplexní sadu funkcí zabezpečení, které společně umožňují vývojářům vytvářet aplikace zabezpečené. Další podrobnosti naleznete [Průvodce zabezpečením úložiště](../storage-security-guide.md).

