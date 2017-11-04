---
title: "Postup nasazení synchronizace souboru Azure (preview) | Microsoft Docs"
description: "Zjistěte, jak nasadit Azure souboru Sync od začátku do konce."
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
ms.openlocfilehash: b31b6ae413f72c626e2601ba860aad44ddaa29cd
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="how-to-deploy-azure-file-sync-preview"></a>Postup nasazení synchronizace souboru Azure (preview)
Azure File Sync (Preview) umožňuje centralizovat sdílené složky organizace ve službě Soubory Azure bez ztráty flexibility, výkonu a kompatibility místního souborového serveru. Dělá to pomocí transformace serverů Windows na rychlou mezipaměť sdílené složky Azure. Pro místní přístup k datům můžete použít jakýkoli protokol dostupný ve Windows Serveru (včetně SMB, NFS a FTPS) a můžete mít libovolný počet mezipamětí po celém světě.

Důrazně doporučujeme čtení [plánování nasazení Azure Files](storage-files-planning.md) a [plánování nasazení Azure souboru Sync](storage-sync-files-planning.md) před podle kroků v této příručce.

## <a name="prerequisites"></a>Požadavky
* Účet úložiště a Azure sdílených složek ve stejné oblasti, kterou chcete nasadit Azure synchronizace souboru. Další informace naleznete v tématu:
    - [Dostupnost v oblastech](storage-sync-files-planning.md#region-availability) Azure synchronizace souborů,
    - [Vytvoření účtu úložiště](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) pro podrobné pokyny o tom, jak vytvořit účet úložiště a
    - [Vytvoření sdílené složky](storage-how-to-create-file-share.md) pro podrobné pokyny o tom, jak vytvořit sdílenou složku.
* Alespoň jeden podporovaný Windows Server nebo cluster Windows serveru pro synchronizaci se službou Azure souboru Sync. V tématu [interoperabilita se systémem Windows Server](storage-sync-files-planning.md#azure-file-sync-interoperability) Další informace o podporovaných verzích systému Windows Server.

## <a name="deploy-the-storage-sync-service"></a>Nasazení služby Sync úložiště 
Synchronizační služba úložiště je nejvyšší úrovně prostředků Azure představující soubor synchronizaci Azure. Chcete-li nasadit službu úložiště synchronizace, přejděte na [portál Azure](https://portal.azure.com/)a vyhledejte soubor synchronizaci Azure. Po výběru "Soubor synchronizace Azure (preview)" ve výsledcích hledání, vyberte "Vytvořit" pop karta otevřete "nasadit úložiště synchronizace".

Výsledný okno požádá o následující informace:

- **Název**: jedinečný název (na jedno předplatné) pro službu synchronizace úložiště.
- **Předplatné**: předplatné, ve kterém k vytvoření synchronizační služby úložiště. V závislosti na konfiguraci strategie vaší organizace můžete mít přístup k jedné nebo více odběrů. Předplatné Azure je nejzákladnější kontejner pro fakturaci pro jednotlivých cloudových služeb (například soubory Azure).
- **Skupina prostředků**: Skupina prostředků je logické skupiny prostředků Azure, jako je například účet úložiště nebo synchronizační služby úložiště. Můžete vytvořit novou skupinu prostředků nebo použít existující skupinu prostředků pro synchronizaci souborů Azure (vám doporučujeme používat skupiny prostředků jako kontejnery používají k izolaci prostředků logicky pro vaši organizaci, jako je například seskupení HR prostředky nebo prostředky pro konkrétní projekt) .
- **Umístění**: oblast, ve kterém byste chtěli nasadit Azure synchronizace souboru. Pouze podporované oblasti jsou k dispozici v tomto seznamu.

Po dokončení "Nasadit úložiště synchronizace" formuláře klikněte na tlačítko "Vytvořit" k nasazení služby Sync úložiště.

## <a name="prepare-windows-servers-for-use-with-azure-file-sync"></a>Příprava serverů Windows pro použití s Azure souboru Sync
Pro někdy serveru, ke které máte v úmyslu používat synchronizaci souboru Azure, včetně uzlů serveru v clusteru s podporou převzetí služeb při selhání, proveďte následující kroky:

Pro každý server, včetně uzlů serveru v clusteru s podporou převzetí služeb při selhání který chcete použít s Azure souboru Sync, proveďte následující kroky:

1. Zakážete konfigurace rozšířeného zabezpečení aplikace Internet Explorer. To je jenom vyžadovaných pro registraci počáteční serveru a může být opětovně povolena po má server byl zaregistrován.
    1. Otevřete správce serveru.
    2. Klikněte na tlačítko **místní Server**:  
        !["Místní Server" na levé straně uživatelského rozhraní správce serveru](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
    3. Vyberte odkaz **konfigurace rozšířeného zabezpečení aplikace Internet Explorer** v podokně dílčí vlastnosti:  
        !["Konfigurace rozšířeného zabezpečení IE" v uživatelském rozhraní správce serveru](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
    4. Vyberte **vypnout** pro správce i uživatele v místním okně Konfigurace rozšířeného zabezpečení aplikace Internet Explorer:  
        ![Vybrané pop okno Konfigurace rozšířeného zabezpečení aplikace Internet Explorer s "Vypnuto"](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

2. Ujistěte se, že používáte alespoň 5.1 prostředí PowerShell.\* (5.1 prostředí PowerShell je výchozím nastavení v systému Windows Server 2016). Můžete ověřit, že používáte 5.1 prostředí PowerShell. \* pohledem na hodnotu vlastnosti PSVersion $PSVersionTable objektu:

    ```PowerShell
    $PSVersionTable.PSVersion
    ```

    - Pokud vaše PSVersion je menší než 5.1. \*jako bude v případě u většiny instalací systému Windows Server 2012 R2, budete moci snadno upgradovat stahuje a instaluje [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616). Příslušný balíček ke stažení a instalaci pro Windows Server 2012 R2 je **Win8.1AndW2K12R2 KB\*\*\*\*\*\*\*-x64.msu**.

3. [Instalace a konfigurace prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Vždy doporučujeme používat nejnovější verzi modulů prostředí Azure PowerShell.

## <a name="install-the-azure-file-sync-agent"></a>Nainstalujte agenta synchronizace souboru Azure
Agent Azure souboru Sync je ke stažení balíčku sdílet tím umožníte a Windows Server mají být synchronizovány s Azure File. Agent si můžete stáhnout z [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Po stažení, dvakrát klikněte na balíček Instalační služby MSI zahájíte instalaci agenta synchronizace souboru Azure.

> [!Important]  
> Pokud máte v úmyslu používat synchronizaci souboru Azure s clusterem s podporou převzetí služeb při selhání, agenta synchronizace souboru Azure bude muset být nainstalován na všech uzlech v clusteru.

Instalační balíček agenta synchronizace souboru Azure nainstalovat poměrně rychle bez příliš mnoho dalších výzev. Doporučujeme následující:
- Ponechejte výchozí cestu instalace `C:\Program Files\Azure\StorageSyncAgent`) můžete zjednodušit správu řešení potíží a serveru.
- Povolení webu Microsoft Update a průběžně aktualizovat synchronizace souboru Azure. Všechny aktualizace, včetně funkce aktualizace a opravy hotfix, které agentovi Azure soubor synchronizaci proběhne z webu Microsoft Update. Doporučujeme vždy trvá nejnovější aktualizace pro synchronizaci souborů Azure. Najdete v tématu [zásady aktualizace synchronizace souboru Azure](storage-sync-files-planning.md#azure-file-sync-agent-update-policy) Další informace.

Při ukončení instalace agenta synchronizace souboru Azure rozhraní registrace serveru bude automatické spouštění. Prosím naleznete v další části o tom, jak zaregistrovat tento server se synchronizací souboru Azure.

## <a name="register-windows-server-with-storage-sync-service"></a>Windows Server zaregistrovat úložiště synchronizační služby
Registrace systému Windows Server se službou Sync úložiště vytváří vztah důvěryhodnosti mezi serverem (nebo clusteru) a služby úložiště Sync. Rozhraní registrace serveru by měl automaticky spouštěná po instalaci agenta synchronizace souboru Azure, ale pokud tomu tak není, můžete otevřít jej ručně z umístění: `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe`. Po registraci rozhraní serveru je otevřený, klikněte na **přihlášení** zahájíte.

Po přihlášení budete vyzváni k zadání následujících informací:

![Snímek obrazovky uživatelského rozhraní registrace serveru](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Předplatné Azure**: předplatné obsahující synchronizační služby úložiště (v tématu [nasazení služby Sync úložiště](#deploy-the-storage-sync-service) výše). 
- **Skupina prostředků**: skupinu prostředků obsahující synchronizační služby úložiště.
- **Služba synchronizace úložiště**: název služby Sync úložiště, pro který chcete zaregistrovat.

Jakmile vyberete příslušné informace z rozevírací nabídky, klikněte na tlačítko **zaregistrovat** dokončit registraci serveru. Jako součást procesu registrace budete vyzváni k další sign-in.

## <a name="create-a-sync-group"></a>Vytvořte skupinu synchronizace
Synchronizace skupiny definuje topologie synchronizace pro určitou sadu souborů. Koncové body v rámci skupiny synchronizace se budou uchovávat vzájemně synchronizována. Synchronizace skupiny musí obsahovat alespoň jeden koncového bodu cloudu, který představuje sdílenou složku Azure, a jeden koncový bod serveru, který představuje cestu na serveru systému Windows. Chcete-li vytvořit skupinu, synchronizace, přejděte na synchronizační služby úložiště v [portál Azure](https://portal.azure.com/)a klikněte na tlačítko **+ skupiny synchronizace**:

![Vytvořit novou skupinu synchronizace na portálu Azure](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

V podokně výsledné požádá o následující informace a vytvořte skupinu synchronizace s koncového bodu cloudu:

- **Název skupiny pro synchronizaci**: název skupiny synchronizace, který se má vytvořit. Tento název musí být jedinečný v rámci služby synchronizace úložiště, ale může být jakýkoli název, který je logické za vás.
- **Předplatné**: předplatné jste nasadili synchronizační služby úložiště v [nasazení služby Sync úložiště](#deploy-the-storage-sync-service) výše.
- **Účet úložiště**: Kliknutím na "Vyberte účet úložiště" pop otevřete podokně aplikace další povolení, vyberte účet úložiště, který obsahuje sdílenou složku Azure File, se kterým chcete synchronizovat.
- **Sdílenou složku Azure**: název sdílené složky Azure File, se kterým chcete synchronizovat.

Chcete-li přidat koncový bod serveru, přejděte na nově vytvořenou skupinu synchronizace a klikněte na tlačítko "Přidat koncový bod serveru".

![Přidat nový koncový bod serveru v podokně skupiny synchronizace](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

V podokně výsledné "přidat koncový bod serveru" vyžaduje následující informace se vytvořit koncový bod serveru:

- **Zaregistrovat Server**: název serveru nebo clusteru, na které má být vytvořen koncový bod serveru.
- **Cesta**: cesta na serveru Windows k synchronizaci jako součást skupiny synchronizace.
- **Cloud Tiering**: přepínač k povolení nebo zakázání cloudu vrstev, které umožňuje zřídka použít nebo k němu přistupovat soubory být rozvrstvena k Azure Files.
- **Volné místo na svazku**: množství volného místa vyhradit na svazku, v němž jsou uložena serveru koncového bodu. Například pokud volné místo na svazku nastavená na 50 % na svazku s jeden koncový bod serveru, zhruba poloviční množství dat bude být rozvrstvena k Azure Files. Všimněte si, že bez ohledu na to, jestli cloud vrstvení je povoleno, Azure sdílené složky má vždy úplnou kopii dat ve skupině synchronizace.

Klikněte na tlačítko "Vytvořit" přidat koncový bod serveru. Vaše soubory se teď sesynchronizovávat napříč Azure sdílené složky a systému Windows Server. 

> [!Important]  
> Můžete provádět změny žádný Cloud nebo koncový bod serveru ve skupině synchronizace a byly synchronizované soubory do dalších koncových bodů ve skupině synchronizace. Pokud provedete změnu koncového bodu cloudu (Azure sdílené složky) přímo, Upozorňujeme, že změny nejprve musí být zjištěny úlohou Azure souboru synchronizace změnu detekce, což je pouze initatiated pro koncový bod cloudu každých 24 hodin. Najdete v článku [Azure soubory – nejčastější dotazy](storage-files-faq.md#afs-change-detection) Další informace.

## <a name="next-steps"></a>Další kroky
- [Přidat nebo odebrat Server koncový bod Azure File synchronizace](storage-sync-files-server-endpoint.md)
- [Registrace nebo zrušení registrace serveru se synchronizací souboru Azure](storage-sync-files-server-registration.md)