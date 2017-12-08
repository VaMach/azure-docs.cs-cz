---
title: "Pomocí aplikace DPM zálohovat úlohy na portálu Azure | Microsoft Docs"
description: "Úvod k zálohování serverů aplikace DPM pomocí služby zálohování Azure"
services: backup
documentationcenter: 
author: adigan
manager: nkolli
editor: 
keywords: "System Center Data Protection Manager, nástroje data protection manager, zálohy aplikace dpm"
ms.assetid: c8c322cf-f5eb-422c-a34c-04a4801bfec7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: adigan;giridham;jimpark;markgal;trinadhk
ms.openlocfilehash: 41eed9c44a226817da9ee5f324e62902bc23754c
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/23/2017
---
# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Příprava zálohování úloh do Azure pomocí DPM
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Server Azure Backup (klasické)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (klasické)](backup-azure-dpm-introduction-classic.md)
>
>

Tento článek obsahuje úvod do Microsoft Azure Backup používá k ochraně vašich serverů System Center Data Protection Manager (DPM) a úloh. Načtením ji budete porozumíte:

* Jak funguje Azure zálohování serveru
* Požadavky pro dosažení smooth zálohování prostředí
* Typické došlo k chybám a řešení problémů s nimi
* Podporované scénáře

> [!NOTE]
> Azure má dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek obsahuje informace a postupy pro obnovení virtuální počítače nasazené pomocí modelu Resource Manager.
>
>

[System Center DPM](https://docs.microsoft.com/en-us/system-center/dpm/dpm-overview) zálohuje data souborů a aplikací. Další informace o podporovaných úlohách najdete [zde](https://docs.microsoft.com/en-us/system-center/dpm/dpm-protection-matrix). Data zálohovaná na DPM můžete uložit na pásce, na disku, nebo zálohovat do Azure se zálohováním Microsoft Azure. Aplikace DPM komunikuje s Azure Backup následujícím způsobem:

* **Aplikace DPM nasazená jako fyzický server nebo místní virtuální počítač** – Pokud aplikace DPM je nasazená jako fyzický server nebo jako virtuální počítač technologie Hyper-V místní data můžete zálohovat do trezoru služeb zotavení kromě disku a pásky Zálohování.
* **Aplikace DPM nasazená jako virtuální počítač Azure** – ze System Center 2012 R2 s aktualizací 3, DPM dá nasadit jako virtuální počítač Azure. Pokud je aplikace DPM nasazená jako virtuální počítač Azure, které můžete zálohovat data na disky Azure připojené k virtuálnímu počítači DPM Azure nebo může přenést úložiště data prostřednictvím jejich zálohování až do trezoru služeb zotavení.

## <a name="why-backup-from-dpm-to-azure"></a>Proč zálohovat z aplikace DPM do Azure?
Mezi výhody firmy pomocí služby Azure Backup k zálohování serverů aplikace DPM patří:

* Pro místní nasazení aplikace DPM můžete použít Azure jako alternativu k dlouhodobé nasazení na pásku.
* Pro nasazení aplikace DPM v Azure Azure Backup k přesměrování zpracování úloh úložiště z disku Azure vám umožní škálování uložením starších data v trezoru služeb zotavení a nových dat na disku.

## <a name="prerequisites"></a>Požadavky
Příprava Azure Backup k zálohování dat aplikace DPM následujícím způsobem:

1. **Vytvoření trezoru služeb zotavení** – vytvoření trezoru na portálu Azure.
2. **Stažení přihlašovacích údajů trezoru** – stáhnout přihlašovací údaje, které použijete k registraci serveru aplikace DPM do trezoru služeb zotavení.
3. **Nainstalovat agenta Azure Backup** – z Azure Backup nainstalujte agenta na každém serveru DPM.
4. **Registrace serveru** – registrace serveru DPM do trezoru služeb zotavení.

## <a name="key-definitions"></a>Klíčové definice
Zde jsou některé klíčové definice pro zálohování Azure pro aplikaci DPM:

1. **Přihlašovací údaje trezoru** – přihlašovací údaje úložiště jsou potřebné k ověření počítače pro odesílání zálohovaných dat do identifikovaného trezoru ve službě Azure Backup. Ho můžete stáhnout z trezoru a je platný pro 48hrs.
2. **Přístupové heslo** – heslo se používá k šifrování záloh do cloudu. Uložte soubor na bezpečné místo, jako je vyžadována během operace obnovení.
3. **Zabezpečení kódu PIN** – Pokud jste povolili [nastavení zabezpečení](https://docs.microsoft.com/en-us/azure/backup/backup-azure-security-feature) úložiště, je potřeba zabezpečení PIN kódu pro provádění kritické operace zálohování. Tato služba Multi-Factor authentication přidá další vrstvu zabezpečení. 
4. **Složka pro obnovení** – je slovní spojení, které jsou zálohy z cloudu dočasně stáhnout do během obnovení cloudu. Jeho velikost musí být přibližně rovná velikosti zálohování položek, které chcete obnovit souběžně.


### <a name="1-create-a-recovery-services-vault"></a>1. Vytvoření trezoru služby Recovery Services
Vytvoření trezoru Recovery Services:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).
2. V nabídce centra klikněte na **Procházet** a v seznamu prostředků zadejte **Služby zotavení**. Během zadávání se seznam bude filtrovat podle zadávaného textu. Klikněte na **Trezor Služeb zotavení**.

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-dpm-introduction/open-recovery-services-vault.png)

    Zobrazí se seznam trezorů služeb zotavení.
3. V nabídce **Trezory Recovery Services** klikněte na **Přidat**.

    ![Vytvoření trezoru Recovery Services – krok 2](./media/backup-azure-dpm-introduction/rs-vault-menu.png)

    Otevře se okno trezoru Recovery Services s výzvou k vyplnění polí **Název**, **Předplatné**, **Skupina prostředků** a **Oblast**.

    ![Vytvoření trezoru Recovery Services – krok 5](./media/backup-azure-dpm-introduction/rs-vault-attributes.png)
4. Jako **Název** zadejte popisný název pro identifikaci trezoru. Název musí být jedinečný v rámci předplatného Azure. Zadejte název v rozsahu 2 až 50 znaků. Musí začínat písmenem a může obsahovat pouze písmena, číslice a pomlčky.
5. Kliknutím na **Předplatné** zobrazíte seznam dostupných předplatných. Pokud si nejste jisti, jaké předplatné použít, použijte výchozí (nebo navrhované) předplatné. Více možností bude dostupných pouze pokud je váš účet organizace přidružený k více předplatným Azure.
6. Kliknutím na **Skupina prostředků** zobrazíte seznam dostupných skupin prostředků, nebo klikněte na **Nová**, chcete-li vytvořit novou skupinu prostředků. Úplnější informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md)
7. Klikněte na **Oblast** a vyberte zeměpisnou oblast trezoru.
8. Klikněte na možnost **Vytvořit**. Vytvoření trezoru Služeb zotavení může chvíli trvat. Sledujte oznámení o stavu v horní pravé části portálu.
   Po vytvoření svůj trezor otevře na portálu.

### <a name="set-storage-replication"></a>Nastavení replikace úložiště
Možnost replikace úložiště umožňuje výběr mezi geograficky redundantním úložištěm a místně redundantním úložištěm. Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště. Ponechte možnost geograficky redundantní úložiště, pokud se jedná o vaši primární zálohu. Zvolte místně redundantní úložiště, pokud chcete levnější možnost, která není tak trvanlivá. Další informace o možnostech [geograficky redundantního](../storage/common/storage-redundancy.md#geo-redundant-storage) a [místně redundantního](../storage/common/storage-redundancy.md#locally-redundant-storage) úložiště naleznete v tématu [Přehled replikace Azure Storage](../storage/common/storage-redundancy.md).

Chcete-li upravit nastavení replikace úložiště:

1. Vyberte svůj trezor pro otevření řídícího panelu trezoru a okna Nastavení. Pokud se okno **Nastavení** neotevře, klikněte na **Všechna nastavení** v řídicím panelu trezoru.
2. Kliknutím na **Infrastruktura zálohování** > **Konfigurace zálohování** v okně **Nastavení** otevřete okno **Konfigurace zálohování**. V okně **Konfigurace zálohování** zvolte pro svůj trezor možnost replikace úložiště.

    ![Seznam trezorů záloh](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Po výběru možnosti úložiště pro svůj trezor jste připraveni k přidružení virtuálního počítače k trezoru. Chcete-li začít přidružení, měli byste vyhledat a zaregistrovat virtuální počítače Azure.

### <a name="2-download-vault-credentials"></a>2. Stažení přihlašovacích údajů trezoru
Soubor s přihlašovacími údaji trezoru je certifikát vytvořený portál pro každý trezor záloh. Portál poté odešle veřejný klíč do Access Control Service (ACS). Privátní klíč certifikátu je k dispozici pro uživatele v rámci pracovního postupu, které je zadána jako vstup v pracovním postupu registrace počítače. To ověřuje počítač pro odesílání zálohovaných dat do identifikovaného trezoru ve službě Azure Backup.

Přihlašovací údajů úložiště se používají pouze během pracovního postupu registrace. Je zodpovědností uživatele zajistit, že není ohrožená soubor s přihlašovacími údaji. Padne-li soubor s přihlašovacími údaji do rukou neautorizovaného uživatele, může být použitý k registraci dalších počítačů ke stejnému trezoru. Jako zálohovaná data šifrovaná pomocí hesla, který patří k zákazníka, však nemůže být ohrožena stávajících zálohovaných dat. Pro snížení tohoto rizika, je nastaveno vypršení platnosti v 48hrs přihlašovací údaje trezoru. Si můžete stáhnout přihlašovací údaje trezoru služeb zotavení libovolný počet časy – ale během pracovního postupu registrace lze použít pouze nejnovější soubor přihlašovacích údajů trezoru.

Soubor s přihlašovacími údaji trezoru je stažen prostřednictvím zabezpečeného kanálu z portálu Azure. Služba Azure Backup nezaznamená privátní klíč certifikátu, a privátní klíč není trvalý v portálu nebo službu. Pomocí následujících kroků stáhněte soubor s přihlašovacími údaji trezoru do místního počítače.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).
2. Otevřený trezor služeb zotavení, do které počítač na který chcete zaregistrovat aplikaci DPM.
3. Otevře se okno nastavení ve výchozím nastavení. Pokud je zavřený, klikněte na **nastavení** na řídicím panelu trezoru otevřete okno nastavení. V okně nastavení klikněte na **vlastnosti**.

    ![Otevřené okno trezoru](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
4. Na stránce vlastnosti, klikněte na **Stáhnout** pod **zálohování pověření**. Na portálu vygeneruje soubor pověření pro úložiště, který je k dispozici ke stažení.

    ![Ke stažení](./media/backup-azure-dpm-introduction/vault-credentials.png)

Na portálu vygeneruje přihlašovací údaje úložiště pomocí kombinace název trezoru a aktuální datum. Klikněte na tlačítko **Uložit** stáhnout přihlašovací údaje úložiště do složky se staženými soubory místní účet, nebo vyberte možnost Uložit jako v nabídce Uložit a zadejte umístění pro přihlašovací údaje trezoru. To bude trvat až několik minut pro soubor, který má být vygenerován.

### <a name="note"></a>Poznámka
* Ujistěte se, že soubor s přihlašovacími údaji uloží v umístění, které jsou přístupné z počítače. Pokud je uložený v souboru sdílené složky nebo SMB, zkontrolujte oprávnění k přístupu.
* Soubor s přihlašovacími údaji se používají pouze během pracovního postupu registrace.
* Soubor s přihlašovacími údaji trezoru vyprší po 48hrs a lze stáhnout z portálu.

### <a name="3-install-backup-agent"></a>3. Instalace agenta zálohování
Po vytvoření trezoru zálohování Azure, je třeba nainstalovat agenta na všechny vaše Windows počítače (Windows Server, klient systému Windows, server System Center Data Protection Manager nebo počítače serveru Azure Backup), které povoluje zálohování dat a aplikací do Azure.

1. Otevřený trezor služeb zotavení, do které počítač na který chcete zaregistrovat aplikaci DPM.
2. Otevře se okno nastavení ve výchozím nastavení. Pokud je zavřený, klikněte na **nastavení** otevřete okno nastavení. V okně nastavení klikněte na **vlastnosti**.

    ![Otevřené okno trezoru](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
3. Na stránce nastavení, klikněte na **Stáhnout** pod **Azure Backup Agent**.

    ![Ke stažení](./media/backup-azure-dpm-introduction/azure-backup-agent.png)

   Jakmile je stáhnout agenta, poklikejte na MARSAgentInstaller.exe spustíte instalaci agenta Azure Backup agent. Vyberte instalační složku a pomocné složky, které jsou potřebné pro agenta. Zadané umístění mezipaměti musí mít volné místo, který je nejméně 5 % zálohovaná data.
4. Pokud používáte proxy server pro připojení k Internetu, v **konfiguraci proxy serveru** obrazovky, zadejte podrobnosti o proxy serveru. Pokud používáte ověřený server proxy, zadejte uživatelské jméno a heslo podrobnosti na této obrazovce.
5. Agent Azure Backup nainstaluje rozhraní .NET Framework 4.5 a prostředí Windows PowerShell (Pokud není k dispozici již) pro dokončení instalace.
6. Po instalaci agenta **Zavřít** okna.

   ![Zavřít](../../includes/media/backup-install-agent/dpm_FinishInstallation.png)
7. K **registrace serveru DPM** do trezoru, v **správy** kartě, klikněte na **Online**. Pak vyberte **zaregistrovat**. Otevře se Průvodce instalací zaregistrovat.
8. Pokud používáte proxy server pro připojení k Internetu, v **konfiguraci proxy serveru** obrazovky, zadejte podrobnosti o proxy serveru. Pokud používáte ověřený server proxy, zadejte uživatelské jméno a heslo podrobnosti na této obrazovce.

    ![Konfigurace proxy serveru](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Proxy.png)
9. Na obrazovce přihlašovací údaje trezoru vyhledejte a vyberte soubor s přihlašovacími údaji trezoru, který byl dříve staženy.

    ![Přihlašovací údaje trezoru](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Credentials.jpg)

    Soubor s přihlašovacími údaji je platná pouze pro 48 hodin (po jeho stažení z portálu). Pokud narazíte na chyby na této obrazovce (například "přihlašovací údaje trezoru, které souboru vypršela platnost."), přihlášení k portálu Azure a stahování, které přihlašovací údaje trezoru soubor znovu.

    Ujistěte se, že je soubor s přihlašovacími údaji k dispozici v umístění, která je přístupná instalační program aplikace. Pokud narazíte na přístup související chyby, zkopírujte soubor s přihlašovacími údaji do dočasného umístění v tomto počítači a operaci opakujte.

    Pokud dojde k chybě neplatný trezoru přihlašovací údaje (například "Neplatný trezoru zadané údaje") soubor je buď poškozený nebo nemá mít poslední přihlašovací údaje související se službou obnovení. Opakujte operaci po stažení nový soubor s přihlašovacími údaji trezoru z portálu. Tato chyba je zpravidla se zobrazí, pokud uživatel klikne na **přihlašovací údaje trezoru Stáhnout** možnost na portálu Azure rychle po sobě. V takovém případě je platný pouze druhý soubor přihlašovacích údajů trezoru.
10. Abyste mohli řídit použití šířky pásma sítě během pracovní a nepracovní dobu ve **nastavení omezení šířky pásma** obrazovky, můžete nastavit omezení využití šířky pásma a definovat pracovní a nepracovní hodiny.

    ![Nastavení omezení](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Throttling.png)
11. V **nastavení složky obnovení** obrazovky, procházet pro složku, kam se soubory stáhli z Azure se dočasně připravený.

    ![Obnovení nastavení složky](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_RecoveryFolder.png)
12. V **nastavení šifrování** obrazovky, můžete buď vygenerovat přístupové heslo nebo zadat přístupové heslo (minimálně 16 znaků). Nezapomeňte uložit heslo v zabezpečeném umístění.

    ![Šifrování](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Encryption.png)

    > [!WARNING]
    > Pokud heslo ztratíte nebo zapomenete; Microsoft vám nemůže pomoci obnovit zálohovaná data. Koncový uživatel vlastní šifrovací přístupové heslo a Microsoft nemá přehled přístupové heslo používané koncovým uživatelem. Uložte soubor na bezpečné místo, jako je vyžadována během operace obnovení.
    >
    >
13. Po kliknutí **zaregistrovat** tlačítko, počítač se úspěšně registrovaný k úložišti a je nyní připraven pro spuštění zálohování na Microsoft Azure.
14. Při použití Data Protection Manager, můžete upravit nastavení určené během pracovního postupu registrace kliknutím **konfigurace** možnost výběrem **Online** pod **správy** kartě.

## <a name="requirements-and-limitations"></a>Požadavky (a omezení)
* Aplikace DPM může být spuštěná jako fyzický server nebo virtuální počítač technologie Hyper-V nainstalovaná v System Center 2012 SP1 nebo System Center 2012 R2. Můžete také používat jako virtuální počítač Azure používá aspoň na System Center 2012 R2 s kumulativní aktualizace 3 pro DPM 2012 R2 nebo virtuálního počítače s Windows v prostředí VMWare alespoň systémem System Center 2012 R2 s kumulativní aktualizací 5.
* Pokud spouštíte aplikaci DPM s nástrojem System Center 2012 SP1 musíte nainstalovat aktualizaci vrátit up 2 pro System Center Data Protection Manager SP1. To je potřeba, před instalací programu Azure Backup Agent.
* Server aplikace DPM by měl mít prostředí Windows PowerShell a rozhraní .net Framework 4.5 nainstalované.
* Aplikace DPM můžete zálohovat většinu úloh do služby Azure Backup. Úplný seznam, co je podporováno najdete podporovat Azure Backup položky dole.
* Data uložená ve službě Azure Backup nelze obnovit pomocí možnosti "Kopírovat na pásku".
* Budete potřebovat účet Azure s povolenou funkcí zálohování Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Přečtěte si informace o [cenách zálohování Azure](https://azure.microsoft.com/pricing/details/backup/).
* Použití zálohování Azure vyžaduje Azure Backup Agent k instalaci na serverech, které chcete zálohovat. Každý server musí mít nejméně 5 % velikost dat, která se zálohuje, k dispozici jako místní volný úložný prostor. Například zálohování 100 GB dat vyžaduje minimálně 5 GB volného místa v pomocné umístění.
* Data budou uložena v úložišti Azure trezoru. Neexistuje žádné omezení množství dat, které jste můžete zálohovat Azure Backup trezoru, ale velikost zdroje dat (třeba virtuální počítač nebo databáze) by neměl být delší než 54400 GB.

Tyto typy souborů jsou podporovány pro zálohování na Azure:

* Šifrované (pouze úplné zálohy)
* Komprimované (je podporováno přírůstkové zálohování)
* Zhuštěné (je podporováno přírůstkové zálohování)
* Komprimované a zhuštěné (zpracovány jako zhuštěné)

A tyto nejsou podporovány:

* Servery v systémech souborů s rozlišením velkých nejsou podporovány.
* Pevné odkazy (vynecháno)
* Body rozboru (vynecháno)
* Zašifrované a komprimované (vynecháno)
* Šifrované a zhuštěné (vynecháno)
* Komprimovaný datový proud
* Zhuštěný datový proud

> [!NOTE]
> Z v System Center DPM 2012 s aktualizací SP1 a vyšší můžete zálohovat do úlohy, které jsou chráněné pomocí DPM do Azure pomocí služby Microsoft Azure Backup.
>
>
