---
title: Nasadit soubor synchronizaci Azure (preview) | Microsoft Docs
description: "Zjistěte, jak nasadit Azure souboru Sync, od začátku do konce."
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
ms.openlocfilehash: 42a0e7a3816e0f1d96951feac210e5770add4fe1
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/13/2017
---
# <a name="deploy-azure-file-sync-preview"></a>Nasadit soubor synchronizaci Azure (preview)
Pomocí synchronizace souboru Azure (preview) můžete centralizovat vaší organizace sdílené složky v souborech Azure, zatímco flexibilitu, výkonu a kompatibility pro místní souborový server. Synchronizace služby Azure souboru transformuje na rychlé mezipaměti Azure sdílené složky systému Windows Server. Můžete použít libovolný protokol, který je k dispozici v systému Windows Server pro přístup k datům místně, včetně protokolu SMB, systém souborů NFS a FTPS. Může mít libovolný počet mezipamětí, jako je třeba po celém světě.

Důrazně doporučujeme, abyste si přečetli [plánování nasazení Azure Files](storage-files-planning.md) a [plánování nasazení Azure souboru Sync](storage-sync-files-planning.md) ještě před dokončením kroků popsaných v tomto článku.

## <a name="prerequisites"></a>Požadavky
* Účet úložiště Azure a Azure soubor sdílet ve stejné oblasti, kterou chcete nasadit Azure synchronizace souboru. Další informace naleznete v tématu:
    - [Dostupnost v oblastech](storage-sync-files-planning.md#region-availability) pro synchronizaci souborů Azure.
    - [Vytvoření účtu úložiště](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) podrobný popis toho, jak vytvořit účet úložiště.
    - [Vytvoření sdílené složky](storage-how-to-create-file-share.md) podrobný popis toho, jak vytvořit sdílenou složku.
* Alespoň jeden podporované instance systému Windows Server nebo cluster Windows serveru pro synchronizaci se službou Azure synchronizace souboru. Další informace o podporovaných verzích systému Windows Server najdete v tématu [interoperabilita se systémem Windows Server](storage-sync-files-planning.md#azure-file-sync-interoperability).

## <a name="deploy-the-storage-sync-service"></a>Nasazení služby Sync úložiště 
Synchronizační služba úložiště je prostředek Azure nejvyšší úrovně pro synchronizaci souborů Azure. Chcete-li nasadit službu úložiště synchronizace, přejděte na [portál Azure](https://portal.azure.com/)a poté vyhledejte soubor synchronizaci Azure. Ve výsledcích hledání vyberte **synchronizace souboru Azure (preview)**a potom vyberte **vytvořit** otevřete **nasazení synchronizace úložiště** kartě.

V podokně, které se otevře zadejte následující informace:

- **Název**: jedinečný název (na jedno předplatné) pro službu synchronizace úložiště.
- **Předplatné**: předplatné, ve kterém chcete vytvořit synchronizační služby úložiště. V závislosti na konfiguraci strategie vaší organizace můžete mít přístup k jedné nebo více odběrů. Předplatné Azure je nejzákladnější kontejner pro fakturaci pro jednotlivých cloudových služeb (například soubory Azure).
- **Skupina prostředků**: Skupina prostředků je logické skupiny prostředků Azure, jako je například účet úložiště nebo synchronizační služby úložiště. Můžete vytvořit novou skupinu prostředků nebo použít existující skupinu prostředků pro synchronizaci souborů Azure. (Doporučujeme používat skupiny prostředků jako kontejnery izolovat prostředky logicky pro vaši organizaci, jako je například seskupení HR prostředky nebo prostředky pro konkrétní projekt.)
- **Umístění**: oblast, ve které chcete nasadit Azure synchronizace souboru. Pouze podporované oblasti jsou k dispozici v tomto seznamu.

Po dokončení vyberte **vytvořit** k nasazení služby Sync úložiště.

## <a name="prepare-windows-server-to-use-with-azure-file-sync"></a>Příprava systému Windows Server pro použití s Azure souboru Sync
Pro každý server, který chcete používat se synchronizací souboru Azure, včetně uzlů serveru v clusteru s podporou převzetí služeb při selhání proveďte následující kroky:

1. Zakázat **Rozšířená konfigurace zabezpečení aplikace Internet Explorer**. To je potřeba jenom pro registraci počáteční server. Můžete ji můžete znovu povolit po registraci serveru.
    1. Otevřete správce serveru.
    2. Klikněte na tlačítko **místní Server**:  
        !["Místní Server" na levé straně uživatelského rozhraní správce serveru](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
    3. Na **vlastnosti** subpane, vyberte odkaz **konfigurace rozšířeného zabezpečení aplikace Internet Explorer**.  
        ![V podokně "Konfigurace rozšířeného zabezpečení" v uživatelském rozhraní správce serveru](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
    4. V **konfigurace rozšířeného zabezpečení aplikace Internet Explorer** dialogové okno, vyberte **vypnout** pro **správci** a **uživatelé**:  
        ![Vybrané pop okno Konfigurace rozšířeného zabezpečení aplikace Internet Explorer s "Vypnuto"](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

2. Ujistěte se, že používáte alespoň 5.1 prostředí PowerShell.\* (5.1 prostředí PowerShell je výchozím nastavení v systému Windows Server 2016). Můžete ověřit, že používáte 5.1 prostředí PowerShell. \* pohledem na hodnotu **PSVersion** vlastnost **$PSVersionTable** objektu:

    ```PowerShell
    $PSVersionTable.PSVersion
    ```

    Pokud vaše PSVersion hodnota je menší než 5.1. \*jako bude v případě u většiny instalací systému Windows Server 2012 R2, budete moci snadno upgradovat stahuje a instaluje [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616). Příslušný balíček ke stažení a instalaci pro Windows Server 2012 R2 je **Win8.1AndW2K12R2 KB\*\*\*\*\*\*\*-x64.msu**.

3. [Instalace a konfigurace prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Doporučujeme používat nejnovější verzi modulů prostředí Azure PowerShell.

## <a name="install-the-azure-file-sync-agent"></a>Nainstalujte agenta synchronizace souboru Azure
Agent Azure souboru Sync je ke stažení balíčku, který umožňuje systému Windows Server bude synchronizován se sdílenou složku Azure. Si můžete stáhnout agenta z [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Po dokončení stahování dvakrát klikněte na balíček Instalační služby MSI zahájíte instalaci agenta synchronizace souboru Azure.

> [!Important]  
> Pokud máte v úmyslu používat synchronizaci souboru Azure s clusterem s podporou převzetí služeb při selhání, musí být na všech uzlech v clusteru nainstalován agent synchronizace souboru Azure.

Instalační balíček agenta synchronizace souboru Azure nainstalovat poměrně rychle a bez příliš mnoho dalších výzev. Doporučujeme, abyste provedli následující:
- Ponechte výchozí instalační cesta (C:\Program Files\Azure\StorageSyncAgent), můžete zjednodušit správu řešení potíží a serveru.
- Povolte Microsoft Update a průběžně aktualizovat synchronizace souboru Azure. Všechny aktualizace, agent synchronizace souboru Azure, včetně funkce aktualizace a opravy hotfix, dojít z webu Microsoft Update. Doporučujeme nainstalovat nejnovější aktualizace pro synchronizaci souborů Azure. Další informace najdete v tématu [zásady synchronizace souboru Azure aktualizace](storage-sync-files-planning.md#azure-file-sync-agent-update-policy).

Po dokončení instalace agenta Azure souboru Sync rozhraní registrace serveru se automaticky otevře. Informace o tom a zaregistrujte tento server se synchronizací souboru Azure, najdete v další části.

## <a name="register-windows-server-with-storage-sync-service"></a>Windows Server zaregistrovat úložiště synchronizační služby
Registrace Windows serveru se službou Sync úložiště vytváří vztah důvěryhodnosti mezi serverem (nebo clusteru) a služby úložiště Sync. Uživatelské rozhraní serveru registrace by měla otevřít automaticky po instalaci agenta synchronizace souboru Azure. Pokud tomu tak není, můžete otevřít jej ručně z jeho umístění souboru: C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe. Když se otevře rozhraní registraci serveru, vyberte **přihlášení** zahájíte.

Po přihlášení, budete vyzváni k zadání následujících informací:

![Snímek obrazovky uživatelského rozhraní registrace serveru](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Předplatné Azure**: odběr, který obsahuje službu synchronizace úložiště (v tématu [nasazení služby Sync úložiště](#deploy-the-storage-sync-service)). 
- **Skupina prostředků**: skupinu prostředků, která obsahuje synchronizační služby úložiště.
- **Služba synchronizace úložiště**: název služby Sync úložiště, pro který chcete zaregistrovat.

Jakmile vyberete příslušné informace, vyberte **zaregistrovat** dokončit registraci serveru. Jako součást procesu registrace budete vyzváni k další sign-in.

## <a name="create-a-sync-group"></a>Vytvořte skupinu synchronizace
Synchronizace skupiny definuje topologie synchronizace pro určitou sadu souborů. Koncové body v rámci synchronizace skupiny jsou synchronizovány mezi sebou. Synchronizace skupiny musí obsahovat alespoň jeden koncového bodu cloudu, který představuje sdílenou složku Azure, a jeden koncový bod serveru, který představuje cestu v systému Windows Server. Chcete-li vytvořit skupinu synchronizace v [portál Azure](https://portal.azure.com/), přejděte do vaší služby synchronizace úložiště a potom vyberte **+ skupiny synchronizace**:

![Vytvořit novou skupinu synchronizace na portálu Azure](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

V podokně, které se otevře zadejte následující informace a vytvořte skupinu synchronizace s koncového bodu cloudu:

- **Název skupiny pro synchronizaci**: název skupiny synchronizace, který se má vytvořit. Tento název musí být jedinečný v rámci služby synchronizace úložiště, ale může být jakýkoli název, který je logické za vás.
- **Předplatné**: předplatné, které jste nasadili synchronizační služby úložiště v [nasazení služby Sync úložiště](#deploy-the-storage-sync-service).
- **Účet úložiště**: Pokud jste vybrali **vyberte účet úložiště**, zobrazí se další podokno, ve kterém můžete vybrat účet úložiště, který má sdílenou složku Azure, které chcete synchronizovat s.
- **Sdílenou složku Azure**: název sdílené složky Azure file, pro který chcete synchronizovat.

Chcete-li přidat koncový bod serveru, přejděte do nově vytvořenou skupinu synchronizace a potom vyberte **přidat koncový bod serveru**.

![Přidat nový koncový bod serveru v podokně skupiny synchronizace](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

V **přidat koncový bod serveru** podokně zadejte následující informace pro vytvoření koncového bodu serveru:

- **Zaregistrovat Server**: název serveru nebo clusteru, kde chcete vytvořit koncový bod serveru.
- **Cesta**: cestu v serveru Windows možné synchronizovat jako součást skupiny synchronizace.
- **Cloud Tiering**: přepínač k povolení nebo zakázání cloudu vrstev. S cloudem vrstvení, zřídka používají nebo získat přístup k souborů může být rozvrstvena k Azure Files.
- **Volné místo na svazku**: množství volného místa vyhradit na svazku, na kterém je umístěn serveru koncového bodu. Například pokud volné místo na svazku nastavená na 50 % ve svazku, který má jeden koncový bod serveru, zhruba poloviční množství dat, je vrstvené k Azure Files. Bez ohledu na tom, jestli cloud vrstvení je povoleno, Azure sdílené složky má vždy úplnou kopii dat ve skupině synchronizace.

Chcete-li přidat koncový bod serveru, vyberte **vytvořit**. Soubory jsou teď byly synchronizovány ve sdílenou složku Azure a Windows Server. 

> [!Important]  
> Můžete provádět změny žádný koncový bod cloudu nebo koncový bod serveru ve skupině synchronizace a na dalších koncových bodů ve skupině synchronizace proběhla vaše soubory. Pokud provedete změnu koncového bodu cloudu (Azure sdílené složky) přímo, nutné změny nejprve mají být zjišťované úlohou detekce změn v souboru synchronizace Azure. Úloha zjištění změn je zahájena pro koncový bod cloudu pouze jednou za 24 hodin. Další informace najdete v tématu [nejčastější dotazy k Azure Files](storage-files-faq.md#afs-change-detection).

## <a name="next-steps"></a>Další kroky
- [Přidat nebo odebrat koncový bod serveru synchronizace souboru Azure](storage-sync-files-server-endpoint.md)
- [Zaregistrovat nebo zrušit registraci serveru s Azure souboru Sync](storage-sync-files-server-registration.md)
