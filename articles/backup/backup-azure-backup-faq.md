
---
title: "Nejčastější dotazy k Azure Backup | Dokumentace Microsoftu"
description: "Odpovědi na nejčastější dotazy týkající se fungování služby, agenta Azure Backup, trezoru služby Recovery Services a omezení zálohování a uchování."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "zálohování a zotavení po havárii; služba zálohování"
ms.assetid: 1011bdd6-7a64-434f-abd7-2783436668d7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 3/10/2017
ms.author: markgal;giridham;arunak;trinadhk;
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 87d500d886feb2dbd61d15d5a980ab2c4018c2f2
ms.lasthandoff: 04/03/2017


---
# <a name="questions-about-the-azure-backup-service"></a>Dotazy týkající se služby Azure Backup
Tento článek obsahuje části s běžnými dotazy (a odpověďmi), které vám pomůžou rychle porozumět komponentám Azure Backup. Některé odpovědi zahrnují odkazy na články obsahující komplexní informace. Můžete klást otázky týkající se Azure Backup kliknutím na **Komentáře** (napravo). Komentáře se zobrazují v dolní části tohoto článku. Pro komentáře je potřeba účet Livefyre. Otázky týkající se služby Azure Backup můžete také publikovat na [diskusním fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

K rychlému procházení částmi tohoto článku použijte odkazy vpravo v části **V tomto článku**.


## <a name="recovery-services-vault"></a>Trezor služby Recovery Services

### <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription-br"></a>Je v rámci předplatného Azure nějak omezený počet trezorů, které lze vytvořit? <br/>
Ano. Od září 2016 můžete vytvořit 25 trezorů služby Recovery Services nebo Backup na jedno předplatné. Můžete vytvořit až 25 trezorů služby Recovery Services pro každou podporovanou oblast služby Azure Backup na jedno předplatné. Pokud potřebujete další trezory, vytvořte další předplatné.

### <a name="are-there-limits-on-the-number-of-serversmachines-that-can-be-registered-against-each-vault-br"></a>Je nějak omezený počet serverů nebo počítačů, které lze zaregistrovat k trezoru? <br/>
Ano, k jednomu trezoru můžete zaregistrovat až 50 počítačů. Pro virtuální počítače Azure IaaS je limit 200 virtuálních počítačů na jeden trezor. Pokud potřebujete zaregistrovat víc počítačů, vytvořte nový trezor.

### <a name="how-do-i-register-my-server-to-another-datacenterbr"></a>Jak mohu zaregistrovat svůj server k jinému datovému centru?<br/>
Zálohovaná data se odesílají do datového centra trezoru, ke kterému je agent registrován. Nejjednodušší způsob, jak změnit datové centrum, je odinstalování agenta a jeho přeinstalování a zaregistrování k novému trezoru, který patří k požadovanému datovému centru.

### <a name="if-my-organization-has-one-vault-how-can-i-isolate-one-servers-data-from-another-server-when-restoring-databr"></a>Pokud má moje organizace jeden trezor, jak mohu během obnovování dat izolovat data jednoho serveru od jiného?<br/>
Všechny servery zaregistrované ke stejnému trezoru mohou obnovit data zálohovaná ostatními servery, *které používají stejné heslo* Máte-li servery, jejichž zálohovaná data chcete izolovat od ostatních serverů ve vaší organizaci, použijte pro tyto servery vyhrazené heslo. Například servery lidských zdrojů mohou používat jedno šifrovací heslo, účetní servery jiné a servery úložiště ještě jiné.

### <a name="whats-the-minimum-size-requirement-for-the-cache-folder-br"></a>Jaký je požadavek na minimální velikost složky mezipaměti? <br/>
Velikost složky mezipaměti určuje množství dat, která zálohujete. Složka mezipaměti by měla zabírat 5 % požadovaného místa pro úložiště dat.

### <a name="can-i-migrate-my-backup-data-or-vault-between-subscriptions-br"></a>Mohu „migrovat“ data nebo trezor záloh mezi předplatnými? <br/>
Ne. Trezor je vytvořený na úrovni předplatného a po vytvoření ho nelze přiřadit k jinému předplatnému.

### <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Trezory služby Recovery Services jsou založené na Resource Manageru. Jsou trezory služby Backup (v klasickém režimu) stále podporovány? <br/>
Všechny existující trezory služby Backup na [portálu Azure Classic](https://manage.windowsazure.com) jsou podporovány i nadále. Portál Azure Classic však již nelze použít k nasazení nových trezorů služby Backup. Společnost Microsoft doporučuje pro všechna nasazení používat trezory služby Recovery Services, protože všechna budoucí vylepšení se budou vztahovat výhradně na trezory služby Recovery Services. Pokud se pokusíte vytvořit trezor služby Backup na portálu Azure Classic, budete přesměrováni na web [Azure Portal](https://portal.azure.com).

### <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Můžu migrovat trezor Backup do trezoru Služeb zotavení? <br/>
Bohužel ne. Obsah trezoru služby Backup nelze migrovat do trezoru služby Recovery Services. Na přidání této funkce pracujeme, zatím ale není dostupná.

### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Podporují trezory Recovery Services klasické virtuální počítače nebo virtuální počítače využívající Resource Manager? <br/>
Trezory Recovery Services podporují oba modely.  Do trezoru služby Recovery Services můžete zálohovat klasický virtuální počítač (vytvořený na portálu Classic) nebo virtuální počítač vytvořený pomocí Resource Manageru (na webu Azure Portal).

### <a name="i-backed-up-my-classic-vms-in-a-backup-vault-can-i-migrate-my-vms-from-classic-mode-to-resource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>Svoje klasické virtuální počítače jsem zálohoval do trezoru služby Backup. Můžu migrovat svoje virtuální počítače z klasického režimu do režimu Resource Manageru a chránit je pomocí trezoru služby Recovery Services?
Při převádění virtuálních počítačů z klasického režimu do režimu Resource Manageru Body neproběhne automatická migrace bodů obnovení klasických virtuálních počítačů do trezoru služby Recovery Services. Při převedení záloh virtuálních počítačů použijte tento postup:

1. V trezoru služby Backup přejděte na kartu **Chráněné položky** a vyberte virtuální počítač. Klikněte na [Zastavit ochranu](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Políčko *Delete associated backup data* (Odstranit přidružená data záloh) ponechte **nezaškrtnuté**.
2. Proveďte migraci virtuálního počítače z klasického režimu do režimu Resource Manageru. Ověřte, že se do režimu Resource Manager migruje také úložiště a informace o síti odpovídající tomuto virtuálnímu počítači.
3. Vytvořte trezor služby Recovery Services a ke konfiguraci zálohování migrovaného virtuálního počítače použijte akci **Zálohování** v horní části řídicího panelu trezoru. Podrobnosti o zálohování virtuálních počítačů do trezoru služby Recovery Services naleznete v tématu [První pohled: Ochrana virtuálních počítačů s trezorem Recovery Services](backup-azure-vms-first-look-arm.md).



## <a name="azure-backup-agent"></a>Agent Azure Backup

### <a name="where-can-i-download-the-latest-azure-backup-agent-br"></a>Kde mohu stáhnout nejnovější verzi agenta Azure Backup? <br/>
Nejnovější verzi agenta pro zálohování Windows Serveru, aplikace System Center DPM nebo klienta Windows si můžete stáhnout [zde](http://aka.ms/azurebackup_agent). Chcete-li zálohovat virtuální počítač, použijte agenta virtuálního počítače (který automaticky nainstaluje správné rozšíření). Agent virtuálního počítače je již nainstalován na virtuálních počítačích vytvořených z galerie Azure.

### <a name="when-configuring-the-azure-backup-agent-i-am-prompted-to-enter-the-vault-credentials-do-vault-credentials-expire"></a>Při konfiguraci agenta Azure Backup se zobrazí výzva k zadání přihlašovacích údajů trezoru. Mohou přihlašovací údaje trezoru vypršet?
Ano, přihlašovací údaje trezoru vyprší po 48 hodinách. Pokud soubor vyprší, přihlaste se k webu Azure Portal a ze svého trezoru si stáhněte soubory s přihlašovacími údaji trezoru.

### <a name="what-happens-if-i-rename-a-windows-server-that-is-backing-up-data-to-azurebr"></a>Co se stane, když přejmenuji server Windows, který zálohuje data do Azure?<br/>
Když server přejmenujete, všechna stávající nastavená zálohování se zastaví.
Zaregistrujte nový název serveru k trezoru služby Backup. Když zaregistrujete nový název trezoru, první zálohování bude provedeno jako *úplné*. Pokud potřebujete obnovit data zálohovaná do trezoru se starým názvem serveru, použijte možnost [**Jiný server**](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine) v průvodci **Obnova dat**.

### <a name="what-types-of-drives-can-i-back-up-files-and-folders-from-br"></a>Z jakých typů jednotek můžu zálohovat soubory a složky? <br/>
Nemůžete zálohovat následující jednotky a svazky:

* Vyměnitelné médium: Všechny zdroje položek k zálohování se musí hlásit jako pevné.
* Svazky jen pro čtení: Svazek musí být zapisovatelný, aby fungovala služba Stínová kopie svazku (VSS).
* Offline svazky: Svazek musí být online, aby fungovala služba VSS.
* Sdílené síťové složky: Svazek musí být místní pro server, aby mohl být zálohovaný pomocí online zálohování.
* Svazky chráněné nástrojem Bitlocker: Předtím, než může dojít k zálohování, musí být svazek odemčený.
* Identifikace systému souborů: Jediným podporovaným systémem souborů je NTFS.

### <a name="what-file-and-folder-types-can-i-back-up-from-my-serverbr"></a>Jaké typy souborů a složek mohu zálohovat ze svého serveru?<br/>
Jsou podporovány následující typy:

* Šifrované
* Komprimované
* Řídké
* Komprimované a řídké
* Pevné odkazy: Není podporováno, vynecháno
* Bod rozboru: Není podporováno, vynecháno
* Šifrované a řídké: Není podporováno, vynecháno
* Komprimovaný datový proud: Není podporováno, vynecháno
* Řídký datový proud: Není podporováno, vynecháno

### <a name="what-is-the-maximum-file-path-length-that-can-be-specified-in-backup-policy-using-azure-backup-agent-br"></a>Jaká je maximální délka cesty k souboru, kterou lze zadat v zásadě služby Backup pomocí agenta Azure Backup? <br/>
Agent Azure Backup se spoléhá na systém souborů NTFS. [Specifikace délky cesty k souboru je omezená rozhraním API systému Windows](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). Pokud mají soubory, které chcete chránit, délku cesty k souboru větší než povoluje rozhraní API systému Windows, zálohujte nadřazenou složku nebo diskovou jednotku.  

### <a name="what-characters-are-allowed-in-file-path-of-azure-backup-policy-using-azure-backup-agent-br"></a>Jaké znaky jsou povolené v cestě k souboru zásady Azure Backup pomocí agenta Azure Backup? <br>
 Agent Azure Backup se spoléhá na systém souborů NTFS. Pro specifikaci souboru povoluje [znaky podporované systémem souborů NTFS](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions).  

### <a name="how-do-i-change-the-cache-location-specified-for-the-azure-backup-agentbr"></a>Jak změním umístění mezipaměti, zadané pro agenta Azure Backup?<br/>
Umístění mezipaměti můžete změnit pomocí následujícího seznamu.

* Zastavte modul zálohování spuštěním následujícího příkazu v příkazovém řádku se zvýšenými oprávněními:

```PS C:\> Net stop obengine```
* Soubory nepřesouvejte. Místo toho zkopírujte složku s místem v mezipaměti na jinou jednotku s dostatkem volného místa. Po potvrzení, že zálohování s novým místem v mezipaměti funguje správně, můžete původní místo v mezipaměti odebrat.
* U následujících položek registru aktualizujte cestu k nové složce s místem v mezipaměti.<br/>

| Cesta k registru | Klíč registru | Hodnota |
| --- | --- | --- |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Nové umístění složky mezipaměti* |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Nové umístění složky mezipaměti* |

* Restartujte modul Backup spuštěním následujícího příkazu v příkazovém řádku se zvýšenými oprávněními:

```PS C:\> Net start obengine```

Po úspěšném dokončení vytvoření zálohy v novém umístění mezipaměti můžete původní složku mezipaměti odebrat.

### <a name="where-can-i-put-the-cache-folder-for-the-azure-backup-agent-to-work-as-expectedbr"></a>Kam mohu dát složku mezipaměti, aby agent Azure Backup fungoval podle očekávání?<br/>
Pro složku mezipaměti nedoporučujeme používat následující umístění:

* Sdílenou síťovou složku nebo vyměnitelné médium: Složka mezipaměti musí být místní pro server, který potřebuje zálohování pomocí online zálohování. Síťová umístění a vyměnitelná média jako jednotky USB nejsou podporovaná.
* Offline svazky: Složka mezipaměti musí být online pro očekávané zálohování pomocí agenta Azure Backup.

### <a name="are-there-any-attributes-of-the-cache-folder-that-are-not-supportedbr"></a>Jsou nějaké atributy složky mezipaměti, které nejsou podporované?<br/>
Složka mezipaměti nepodporuje následující atributy nebo jejich kombinace:

* Šifrované
* S odstraněním duplicit
* Komprimované
* Řídké
* Bod rozboru

Složka mezipaměti ani virtuální pevný disk s metadaty nemají atributy vyžadované pro agenta Azure Backup.


## <a name="virtual-machines"></a>Virtuální počítače

### <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-already-backed-by-the-azure-backup-service-using-the-vm-extension-br"></a>Mohu nainstalovat agenta Azure Backup na virtuální počítač Azure, který už je zálohovaný službou Azure Backup pomocí rozšíření virtuálního počítače? <br/>
Jistě. Azure Backup poskytuje zálohování na úrovni virtuálních počítačů pro virtuální počítače Azure, které používají rozšíření virtuálního počítače. Pokud chcete chránit soubory a složky na hostovaném operačním systému Windows, nainstalujte na něj agenta Azure Backup.

### <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-to-back-up-files-and-folders-present-on-temporary-storage-provided-by-the-azure-vm-br"></a>Mohu nainstalovat agenta Azure Backup na virtuální počítač Azure a použít ho k zálohování souborů a složek umístěných na dočasném úložišti poskytnutém virtuálním počítačem Azure? <br/>
Ano. Nainstalujte agenta Azure Backup na hostovaný operační systém Windows a zálohujte soubory a složky do dočasného úložiště. Jakmile dojde k vymazání dat na dočasném úložišti, úlohy zálohování selžou. Navíc pokud dojde k vymazání dat na dočasném úložišti, budete moci provést obnovení pouze na stálé úložiště.


## <a name="azure-backup-server-and-data-protection-manager"></a>Azure Backup Server a Data Protection Manager

### <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Mohu použít server Azure Backup k vytvoření zálohy úplného obnovení (BMR) pro fyzický server? <br/>
Ano.

### <a name="which-version-of-system-center-data-protection-manager-is-supported-br"></a>Jaká verze aplikace System Center Data Protection Manager je podporována? <br/>
Doporučujeme vám nainstalovat [nejnovějšího](http://aka.ms/azurebackup_agent) agenta Azure Backup na nejnovější kumulativní aktualizaci aplikace System Center Data Protection Manager (DPM). Od srpna 2016 je nejnovější aktualizací Kumulativní aktualizace 11.

### <a name="i-have-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-now-install-system-center-dpm-to-work-with-azure-backup-agent-to-protect-on-premises-applicationvm-workloads-to-azure-br"></a>Nainstaloval jsem agenta Azure Backup, aby chránil moje soubory a složky. Můžu nyní do Azure nainstalovat aplikaci System Center DPM, aby spolupracovala s agentem Azure Backup při ochraně místních aplikací a úloh virtuálního počítače? <br/>
Když chcete používat Azure Backup se sadou System Center Data Protection Manager (DPM), nainstalujte nejprve DPM a až potom agenta Azure Backup. Instalace komponent služby Azure Backup v tomto pořadí zajistí, že agent Azure Backup bude fungovat s DPM. Instalovat agenta Azure Backup dříve než DPM se nedoporučuje ani nepodporuje.


## <a name="how-azure-backup-works"></a>Jak funguje Azure Backup

### <a name="does-the-azure-backup-agent-work-on-a-server-that-uses-windows-server-2012-deduplication-br"></a>Funguje agent Azure Backup na serveru, který používá odstranění duplicit Windows Serveru 2012? <br/>
Ano. Služba agenta během přípravy operace zálohování převádí odstraněná duplicitní data na normální data. Poté optimalizuje data pro zálohování, zašifruje je, a zašifrovaná data odešle do online služby zálohování.

### <a name="if-i-cancel-a-backup-job-once-it-has-started-is-the-transferred-backup-data-deleted-br"></a>Pokud zruším úlohu zálohování poté, co již byla spuštěná, dojde k odstranění přenášených dat? <br/>
Ne. Všechna data přenášená do trezoru před zrušením úlohy zálohování zůstanou v trezoru. Azure Backup používá mechanismus kontrolních bodů k příležitostnému přidávání kontrolních bodů do zálohovaných dat během zálohování. Díky kontrolním bodům v zálohovaných datech je možné při dalším procesu zálohování ověřit integritu souborů. Následující zálohování proběhne jako přírůstkové vzhledem k naposledy zálohovaným datům. Přírůstkové zálohování přenáší jen nová nebo změněná data, což znamená lepší využití přenosové kapacity.

Když ve virtuálním počítači Azure zrušíte úlohu zálohování, budou dosud přenesená data ignorována. Při následujícím přírůstkovém zálohování se přenesou data, která se změnila od poslední úspěšně dokončené úlohy zálohování.

### <a name="if-a-backup-job-fails-can-i-configure-the-backup-service-to-send-e-mail-br"></a>Můžu nakonfigurovat službu Backup, aby v případě selhání úlohy zálohování odeslala email? <br/>
Ano, služba Backup obsahuje několik výstrah založených na událostech, které lze použít ve skriptu prostředí PowerShell. Úplný popis najdete v tématu [Konfigurace oznámení](backup-azure-monitor-vms.md#configure-notifications).

### <a name="are-there-limits-on-when-or-how-many-times-a-backup-job-can-be-scheduledbr"></a>Existují omezení počtu a času naplánovaných úloh zálohování?<br/>
Ano. Úlohy zálohování na pracovních stanicích s Windows Serverem nebo s Windows můžete spustit až třikrát za den. Úlohy zálohování na System Center DPM můžete spustit až dvakrát za den. Úlohy zálohování pro virtuální počítače IaaS můžete spustit jednou za den. Pomocí zásady plánování můžete pro pracovní stanice s Windows Serverem nebo s Windows zadat denní nebo týdenní plány. Pomocí aplikace System Center DPM můžete zadat denní, týdenní, měsíční nebo roční plány.

### <a name="why-is-the-size-of-the-data-transferred-to-the-recovery-services-vault-smaller-than-the-data-i-backed-upbr"></a>Proč je velikost dat přenášených do trezoru služby Recovery Services menší než velikost dat, která jsem zálohoval?<br/>
 Všechna data zálohovaná z agenta Azure Backup, SCDPM nebo serveru Azure Backup, projdou před odesláním kompresí a šifrováním. Po kompresi a šifrování jsou data v trezoru záloh o 30-40 % menší.

 ### <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-by-the-backup-servicebr"></a>Existuje způsob, jak nastavit šířku pásma používaného službou Backup?<br/>
  Ano, k úpravě šířky pásma použijte možnost **Změnit vlastnosti** v agentu Backup. Můžete upravit šířku pásma a dobu, kdy tuto šířku pásma používáte. Podrobné pokyny najdete v tématu **[Povolení omezení využití sítě](backup-configure-vault.md#enable-network-throttling)**.



## <a name="what-can-i-back-up"></a>Co můžu zálohovat

### <a name="which-operating-systems-do-azure-backup-support-br"></a>Které operační systémy podporuje služba Azure Backup? <br/>
Azure Backup podporuje pro zálohování souborů a složek a pro zálohování úloh aplikací chráněných pomocí Azure Backup Serveru a aplikace System Center Data Protection Manager (SCDPM) následující operační systémy.

| Operační systém | Platforma | Skladová jednotka (SKU) |
|:--- | --- |:--- |
| Windows 8 a nejnovější aktualizace Service Packu |64bitová verze |Enterprise, Pro |
| Windows 7 a nejnovější aktualizace Service Packu |64bitová verze |Ultimate, Enterprise, Professional, Home Premium, Home Basic, Starter |
| Windows 8.1 a nejnovější aktualizace Service Packu |64bitová verze |Enterprise, Pro |
| Windows 10 |64bitová verze |Enterprise, Pro, Home |
| Windows Server 2016 |64bitová verze |Standard, Datacenter, Essentials |
| Windows Server 2012 R2 a nejnovější aktualizace Service Packu |64bitová verze |Standard, Datacenter, Foundation |
| Windows Server 2012 a nejnovější aktualizace Service Packu |64bitová verze |Datacenter, Foundation, Standard |
| Windows Storage Server 2012 R2 a nejnovější aktualizace Service Packu |64bitová verze |Standard, Workgroup |
| Windows Storage Server 2012 a nejnovější aktualizace Service Packu |64bitová verze |Standard, Workgroup |
| Windows Server 2012 R2 a nejnovější aktualizace Service Packu |64bitová verze |Essential |
| Windows Server 2008 R2 SP1 |64bitová verze |Standard, Enterprise, Datacenter, Foundation |
| Windows Server 2008 SP2 |64bitová verze |Standard, Enterprise, Datacenter, Foundation |

**Pro zálohování virtuálního počítače Azure:**

* **Linux**: Azure Backup podporuje [seznam distribucí schválených pro Azure](../virtual-machines/linux/endorsed-distros.md), kromě základního OS Linux.  Další vlastní distribuce Linuxu mohou také fungovat, pokud je ve virtuálním počítači dostupný agent virtuálního počítače a pokud je podporovaný Python.
* **Windows Server**: Verze starší než Windows Server 2008 R2 nejsou podporovány.


### <a name="is-there-a-limit-on-the-size-of-each-data-source-being-backed-up-br"></a>Existuje nějaké omezení velikosti jednotlivých zdrojů zálohovaných dat? <br/>
Množství dat, která můžete do trezoru zálohovat, není nijak omezené. Azure Backup omezuje maximální velikost pro zdroj dat, ale tato omezení jsou velká. K srpnu 2015 je maximální velikost zdroje dat pro podporované operační systémy:

| Zdroj č. | Operační systém | Maximální velikost zdroje dat |
|:---:|:--- |:--- |
| 1 |Windows Server 2012 nebo novější |54 400 GB |
| 2 |Windows 8 nebo novější |54 400 GB |
| 3 |Windows Server 2008, Windows Server 2008 R2 |1700 GB |
| 4 |Windows 7 |1700 GB |

Následující tabulka vysvětluje, jak je určená velikost jednotlivých zdrojů dat.

| Zdroj dat | Podrobnosti |
|:---:|:--- |
| Svazek |Množství dat zálohovaných z jednoho svazku serveru nebo klientského počítače. |
| Virtuální počítač s technologií Hyper-V |Součet dat všech virtuálních pevných disků zálohovaného virtuálního počítače. |
| Databáze Microsoft SQL Serveru |Velikost jedné zálohované databáze SQL. |
| Microsoft SharePoint |Součet databází obsahu a konfigurace v rámci zálohované farmy služby SharePoint. |
| Microsoft Exchange |Součet všech databází systému Exchange na zálohovaném serveru Exchange. |
| BMR/Stav systému |Každá jednotlivá kopie BMR nebo stavu systému zálohovaného počítače. |



## <a name="retention-policy-and-recovery-points"></a>Zásady uchovávání informací a body obnovení

### <a name="is-there-a-difference-between-the-retention-policy-for-dpm-and-windows-serverclient-that-is-on-windows-server-without-dpmbr"></a>Je nějaký rozdíl mezi zásadami uchovávání informací pro DPM a Windows Server nebo klienta (tj. pro Windows Server bez DPM)?<br/>
Ne, DPM i Windows Server nebo klient mohou mít denní, týdenní, měsíční nebo roční zásady uchovávání.

### <a name="can-i-configure-my-retention-policies-selectively--ie-configure-weekly-and-daily-but-not-yearly-and-monthlybr"></a>Mohu konfigurovat zásady uchovávání informací selektivně – tj. nakonfigurovat týdenní a denní, ale ne roční a měsíční?<br/>
Ano, struktura uchovávání Azure Backup vám poskytuje úplnou flexibilitu při definování zásad uchovávání informací podle vašich požadavků.

### <a name="can-i-schedule-a-backup-at-6pm-and-specify-retention-policies-at-a-different-timebr"></a>Můžu naplánovat zálohování na 18:00 a pro zásady uchovávání informací zadat jiný čas?<br/>
Ne. Zásady uchovávání informací lze aplikovat pouze na body záloh. Následující obrázek ukazuje zadanou zásadu uchovávání informací pro zálohy pořízené v 00:00 a v 18:00. <br/>

![Plánování zálohování a uchovávání](./media/backup-azure-backup-faq/Schedule.png)
<br/>

### <a name="is-an-incremental-copy-transferred-for-the-retention-policies-scheduled-br"></a>Je naplánované přenášení přírůstkové kopie pro zásady uchovávání informací? <br/>
Ne, přírůstková kopie se odesílá podle času uvedeného na stránce plánu zálohování. Body, které mohou být uchované, se určují podle zásady uchovávání informací.

### <a name="if-a-backup-is-retained-for-a-long-duration-does-it-take-more-time-to-recover-an-older-data-point-br"></a>Pokud se záloha uchovává po dlouhou dobu, trvá pak obnovení staršího datového bodu déle? <br/>
Ne – obnovení nejstaršího i nejnovějšího bodu trvá stejně dlouho. Každý bod obnovení se chová jako úplný bod.

### <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-the-total-billable-backup-storagebr"></a>Jestliže se každý bod obnovení chová jako úplný bod, ovlivní to celkové fakturovatelné úložiště zálohování?<br/>
Typické produkty s dlouhodobými body uchování ukládají zálohovaná data jako úplné body.  Úplné body jsou *neefektivní* z hlediska úložiště, ale jejich obnovení je snadnější a rychlejší. Přírůstkové kopie jsou *efektivní* z hlediska úložiště, ale vyžadují obnovení řetězu dat, což ovlivňuje dobu obnovení. Architektura úložiště Azure Backup nabízí to nejlepší z obou světů – optimální ukládání dat pro rychlé obnovení a nízké poplatky za úložiště. Tento přístup k ukládání dat zajišťuje efektivní využití příchozí i odchozí šířky pásma. Velikost úložiště dat i čas potřebný k obnovení dat jsou omezeny na minimum. Dozvědět se více o efektivitě [přírůstkového zálohování](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/).

### <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-createdbr"></a>Je nějak omezený počet bodů obnovení, které lze vytvořit?<br/>
Na jednu chráněnou instanci je možné vytvořit až 9 999 bodů obnovení. Chráněná instance je počítač, server (fyzický nebo virtuální) nebo úloha nakonfigurovaná pro zálohování dat do Azure. Neexistuje žádné omezení počtu chráněných instancí na trezor záloh. Další informace najdete vysvětlené v částech [Zálohování a uchovávání](./backup-introduction-to-azure-backup.md#backup-and-retention) a [Co je chráněná instance?](./backup-introduction-to-azure-backup.md#what-is-a-protected-instance).

### <a name="how-many-recoveries-can-i-perform-on-the-data-that-is-backed-up-to-azurebr"></a>Kolikrát mohu provést obnovení dat zálohovaných v Azure?<br/>
Počet obnovení z Azure Backup není omezený.

### <a name="when-restoring-data-do-i-pay-for-the-egress-traffic-from-azure-br"></a>Platí se za výchozí přenos z Azure při obnovování dat? <br/>
Ne. Vaše obnovení jsou zdarma a výchozí přenos vám není účtován.

### <a name="i-receive-the-warning-azure-backups-have-not-been-configured-for-this-server-even-though-i-configured-a-backup-policy-br"></a>Zobrazuje se upozornění „Služba Azure Backup nebyla pro tento server nakonfigurovaná“ i přesto, že jsem nakonfiguroval zásadu zálohování. <br/>
Toto upozornění se zobrazí v případě, že se nastavení plánu zálohování uložené na místním serveru neshodují s nastaveními uloženými v trezoru záloh. Pokud došlo k obnovení serveru nebo nastavení do známého stavu, může dojít ke ztrátě synchronizace plánů zálohování. Pokud se zobrazí toto upozornění, [překonfigurujte zásadu zálohování](backup-azure-manage-windows-server.md) a poté **spusťte Zálohovat nyní**, aby došlo k opětovné synchronizaci místního serveru s Azure. 



## <a name="azure-backup-encryption"></a>Šifrování ve službě Azure Backup

### <a name="is-the-data-sent-to-azure-encrypted-br"></a>Jsou data odesílaná do Azure šifrovaná? <br/>
Ano. Data jsou zašifrována na místním serveru, klientu nebo počítači SCDPM pomocí AES256 a odesílaná jsou přes zabezpečené spojení HTTPS.

### <a name="is-the-backup-data-on-azure-encrypted-as-wellbr"></a>Jsou šifrovaná i zálohovaná data v Azure?<br/>
Ano. Data odeslaná do Azure zůstávají šifrovaná (neaktivní uložená data). Microsoft nikdy nedešifruje zálohovaná data. Při zálohování virtuálního počítače Azure spoléhá služba Azure Backup na šifrování virtuálního počítače. Například pokud je váš virtuální počítač šifrovaný pomocí služby Azure Disk Encryption nebo jiné technologie šifrování, Azure Backup k zabezpečení dat použije toto šifrování.

### <a name="what-is-the-minimum-length-of-encryption-key-used-to-encrypt-backup-data-br"></a>Jaká je minimální délka šifrovacího klíče, který se používá k šifrování zálohovaných dat? <br/>
Šifrovací klíč by měl obsahovat alespoň 16 znaků.

### <a name="what-happens-if-i-misplace-the-encryption-key-can-i-recover-the-data-or-can-microsoft-recover-the-data-br"></a>Co se stane, když ztratím šifrovací klíč? Mohu obnovit data (nebo) může Microsoft obnovit data? <br/>
Klíč, pomocí kterého se šifrují zálohovaná data, je přítomen pouze u zákazníka. Microsoft neudržuje jeho kopii v Azure a nemá ke klíči žádný přístup. Pokud zákazník klíč ztratí, Microsoft nemůže zálohovaná data obnovit.

