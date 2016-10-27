<properties
   pageTitle="Nejčastější dotazy k Azure Backup | Microsoft Azure"
   description="Odpovědi na nejčastější dotazy ohledně služby zálohování, agenta zálohování, zálohování a uchovávání, obnovení, zabezpečení a další časté dotazy k zálohování a zotavení po havárii."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="zálohování a zotavení po havárii; služba zálohování"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="08/29/2016"
     ms.author="trinadhk; giridham; arunak; markgal; jimpark;"/>


# <a name="azure-backup-service--faq"></a>Služba Azure Backup – Nejčastější dotazy

> [AZURE.SELECTOR]
- [Nejčastější dotazy týkající se režimu Classic](backup-azure-backup-faq.md)
- [Nejčastější dotazy týkající se režimu Resource Manager](backup-azure-backup-ibiza-faq.md)

Tento článek je seznam častých dotazů (a příslušných odpovědí) ke službě Azure Backup. Naše komunita odpovídá rychle a pokud je dotaz pokládán často, přidáme ho do tohoto článku. Odpovědi na otázky obvykle poskytují odkaz nebo informace o podpoře. Dotazy k Azure Backup můžete pokládat v části Disqus u tohoto článku nebo u souvisejících článků. Otázky týkající se služby Azure Backup můžete také publikovat na [diskusním fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).


## <a name="what-is-the-list-of-supported-operating-systems-from-which-i-can-back-up-to-azure-using-azure-backup?-<br/>"></a>Jaké jsou podporované operační systémy, ze kterých mohu zálohovat do Azure pomocí Azure Backup? <br/>
Azure Backup podporuje pro zálohování souborů a složek, zálohování aplikací pomocí serveru Azure Backup a SCDPM následující seznam operačních systémů. 

| Operační systém        | Platforma           | Skladová jednotka (SKU)  |
| :------------- |-------------| :-----|
| Windows 8 a nejnovější aktualizace Service Packu      | 64bitová verze | Enterprise, Pro |
| Windows 7 a nejnovější aktualizace Service Packu      | 64bitová verze | Ultimate, Enterprise, Professional, Home Premium, Home Basic, Starter |
| Windows 8.1 a nejnovější aktualizace Service Packu | 64bitová verze      |    Enterprise, Pro |
| Windows 10      | 64bitová verze | Enterprise, Pro, Home |
|Windows Server 2012 R2 a nejnovější aktualizace Service Packu| 64bitová verze| Standard, Datacenter, Foundation|
|Windows Server 2012 a nejnovější aktualizace Service Packu|    64bitová verze| Datacenter, Foundation, Standard|
|Windows Storage Server 2012 R2 a nejnovější aktualizace Service Packu  |64bitová verze|    Standard, Workgroup|
|Windows Storage Server 2012 a nejnovější aktualizace Service Packu |64bitová verze |Standard, Workgroup
|Windows Server 2012 R2 a nejnovější aktualizace Service Packu  |64bitová verze|    Essential|
|Windows Server 2008 R2 SP1 |64bitová verze|    Standard, Enterprise, Datacenter, Foundation|
|Windows Server 2008 SP2    |64bitová verze|    Standard, Enterprise, Datacenter, Foundation|

Pro zálohování virtuálního počítače Azure,

- **Linux**: Azure Backup podporuje [seznam distribucí schválených Azure](../virtual-machines/virtual-machines-linux-endorsed-distros.md), kromě základního OS Linux.  Další vlastní distribuce Linuxu mohou také fungovat, pokud je ve virtuálním počítači dostupný agent virtuálního počítače a pokud je podporovaný Python.
- **Windows Server**: Verze starší než Windows Server 2008 R2 nejsou podporovány.

## <a name="where-can-i-download-the-latest-azure-backup-agent?-<br/>"></a>Kde mohu stáhnout nejnovější verzi agenta Azure Backup? <br/>
Nejnovější verzi agenta pro zálohování Windows Serveru, aplikace System Center DPM nebo klienta Windows si můžete stáhnout [zde](http://aka.ms/azurebackup_agent). Chcete-li zálohovat virtuální počítač, použijte agenta virtuálního počítače (který automaticky nainstaluje správné rozšíření). Agent virtuálního počítače je již nainstalován na virtuálních počítačích vytvořených z galerie Azure.

## <a name="which-version-of-scdpm-server-is-supported?-<br/>"></a>Jaká verze serveru SCDPM je podporovaná? <br/>
Doporučujeme vám nainstalovat [nejnovějšího](http://aka.ms/azurebackup_agent) agenta Azure Backup na nejnovější kumulativní aktualizaci SCDPM (UR11 k srpnu 2016).

## <a name="when-configuring-the-azure-backup-agent,-i-am-prompted-to-enter-the-vault-credentials.-do-vault-credentials-expire?"></a>Při konfiguraci agenta Azure Backup se zobrazí výzva k zadání přihlašovacích údajů trezoru. Mohou přihlašovací údaje trezoru vypršet?
Ano, přihlašovací údaje trezoru vyprší po 48 hodinách. Pokud soubor vyprší, přihlaste se k webu Azure Portal a ze svého trezoru si stáhněte soubory s přihlašovacími údaji trezoru. 

## <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription?-<br/>"></a>Je v rámci předplatného Azure nějak omezený počet trezorů, které lze vytvořit? <br/>
Ano. Od září 2016 můžete vytvořit 25 úložišť záloh na jedno předplatné. Můžete vytvořit až 25 trezorů služby Recovery Services pro každou podporovanou oblast záloh Azure na jedno předplatné. Pokud potřebujete další trezory, vytvořte nové předplatné.

## <a name="are-there-any-limits-on-the-number-of-servers/machines-that-can-be-registered-against-each-vault?-<br/>"></a>Je nějak omezený počet serverů/počítačů, které lze zaregistrovat k trezoru? <br/>
Ano, k jednomu trezoru můžete zaregistrovat až 50 počítačů. Pro virtuální počítače Azure IaaS je limit 200 virtuálních počítačů na jeden trezor. Pokud potřebujete zaregistrovat více počítačů, vytvořte nový trezor.

## <a name="how-do-i-register-my-server-to-another-datacenter?<br/>"></a>Jak mohu zaregistrovat svůj server k jinému datovému centru?<br/>
Zálohovaná data se odesílají do datového centra trezoru, ke kterému je agent registrován. Nejjednodušší způsob, jak změnit datové centrum, je odinstalování agenta a jeho přeinstalování a zaregistrování k novému trezoru, který patří k požadovanému datovému centru.

## <a name="what-happens-if-i-rename-a-windows-server-that-is-backing-up-data-to-azure?<br/>"></a>Co se stane, když přejmenuji server Windows, který zálohuje data do Azure?<br/>
Když server přejmenujete, všechna stávající nastavená zálohování se zastaví.
Je třeba zaregistrovat nový název serveru k trezoru služby Backup. Když vytvoříte novou registraci, první zálohování je úplné zálohování a ne přírůstkové zálohování. Pokud potřebujete obnovit data, která byla dříve zálohovaná do trezoru se starým názvem, můžete tak učinit pomocí možnosti [**Jiný server**](backup-azure-restore-windows-server.md#recover-to-an-alternate-machine) v průvodci **Obnova dat**.

## <a name="what-types-of-drives-can-i-backup-files-and-folders-from?-<br/>"></a>Z jakých typů jednotek mohu zálohovat soubory a složky? <br/>
Následující jednotky/svazky nelze zálohovat:

- Vyměnitelné médium: Jednotka se musí hlásit jako pevná, aby se dala použít jako zdroj položek k zálohování.
- Svazky jen pro čtení: Svazek musí být zapisovatelný, aby fungovala služba Stínová kopie svazku (VSS).
- Offline svazky: Svazek musí být online, aby fungovala služba VSS.
- Sdílené síťové složky: Svazek musí být místní pro server, aby mohl být zálohovaný pomocí online zálohování.
- Svazky chráněné nástrojem Bitlocker: Předtím, než může dojít k zálohování, musí být svazek odemčený.
- Identifikace systému souborů: Pro tuto verzi služby online zálohování je jediným podporovaným systémem souborů NTFS.

## <a name="what-file-and-folder-types-can-i-back-up-from-my-server?<br/>"></a>Jaké typy souborů a složek mohu zálohovat ze svého serveru?<br/>
Jsou podporovány následující typy:

- Šifrované
- Komprimované
- Řídké
- Komprimované a řídké
- Pevné odkazy: Není podporováno, vynecháno
- Bod rozboru: Není podporováno, vynecháno
- Šifrované a komprimované: Není podporováno, vynecháno
- Šifrované a řídké: Není podporováno, vynecháno
- Komprimovaný datový proud: Není podporováno, vynecháno
- Řídký datový proud: Není podporováno, vynecháno

## <a name="what's-the-minimum-size-requirement-for-the-cache-folder?-<br/>"></a>Jaký je požadavek na minimální velikost složky mezipaměti? <br/>
Velikost složky mezipaměti určuje množství dat, která zálohujete. Složka mezipaměti by měla zabírat 5 % požadovaného místa pro úložiště dat.

## <a name="if-my-organization-has-one-vault,-how-can-i-isolate-one-server's-data-from-another-server-when-restoring-data?<br/>"></a>Pokud má moje organizace jeden trezor, jak mohu během obnovování dat izolovat data jednoho serveru od jiného?<br/>
Všechny servery zaregistrované ke stejnému trezoru mohou obnovit data zálohovaná ostatními servery, *které používají stejné heslo* Máte-li servery, jejichž zálohovaná data chcete izolovat od ostatních serverů ve vaší organizaci, použijte pro tyto servery vyhrazené heslo. Například servery lidských zdrojů mohou používat jedno šifrovací heslo, účetní servery jiné a servery úložiště ještě jiné.

## <a name="can-i-“migrate”-my-backup-data-or-vault-between-subscriptions?-<br/>"></a>Mohu „migrovat“ data nebo trezor záloh mezi předplatnými? <br/>
Ne. Trezor je vytvořený na úrovni předplatného a po vytvoření ho nelze přiřadit k jinému předplatnému.

## <a name="does-the-azure-backup-agent-work-on-a-server-that-uses-windows-server-2012-deduplication?-<br/>"></a>Funguje agent Azure Backup na serveru, který používá odstranění duplicit Windows Serveru 2012? <br/>
Ano. Služba agenta během přípravy operace zálohování převádí odstraněná duplicitní data na normální data. Poté optimalizuje data pro zálohování, zašifruje je, a zašifrovaná data odešle do online služby zálohování.

## <a name="if-i-cancel-a-backup-job-once-it-has-started,-is-the-transferred-backup-data-deleted?-<br/>"></a>Pokud zruším úlohu zálohování poté, co již byla spuštěná, dojde k odstranění přenášených dat? <br/>
Ne. Trezor záloh uloží všechna zálohovaná data, která byla přenesená až do okamžiku zrušení. Azure Backup používá mechanismus kontrolních bodů k příležitostnému přidávání kontrolních bodů do zálohovaných dat během zálohování. Díky kontrolním bodům v zálohovaných datech je možné při dalším procesu zálohování ověřit integritu souborů. Další aktivované zálohování pak bude přírůstkové přes data zálohovaná dříve. Přírůstkové zálohování poskytuje lepší využití šířky pásma, takže není nutné přenášet stejná data opakovaně.

V případě zálohování virtuálních počítačů Azure pokud dojde ke zrušení úlohy, přenášená data jsou ignorována a čerstvá záloha přenese přírůstková data z předchozí úspěšné úlohy zálohování. 

## <a name="why-am-i-seeing-the-warning-"azure-backups-have-not-been-configured-for-this-server"-even-though-i-had-scheduled-regular-backups-previously?-<br/>"></a>Proč se zobrazuje upozornění „Zálohování Azure nebyla pro tento server konfigurovaná“ i přesto, že jsem už dříve naplánoval pravidelná zálohování? <br/>
Toto upozornění se zobrazí v případě, že se nastavení plánu zálohování uložené na místním serveru neshodují s nastaveními uloženými v trezoru záloh. Pokud došlo k obnovení serveru nebo nastavení do známého stavu, může dojít ke ztrátě synchronizace plánů zálohování. Pokud se zobrazí toto upozornění, [překonfigurujte zásadu zálohování](backup-azure-manage-windows-server.md) a poté **spusťte Zálohovat nyní**, aby došlo k opětovné synchronizaci místního serveru s Azure. 

## <a name="what-firewall-rules-should-be-configured-for-azure-backup?-<br/>"></a>Jaká pravidla brány firewall mají být nakonfigurována pro Azure Backup? <br/>
Kvůli bezproblémové ochraně dat odesílaných z místních prostředků nebo úlohy do Azure doporučujeme povolit bráně firewall komunikaci s následujícími adresami URL:

- www.msftncsi.com
- \*.Microsoft.com
- \*.WindowsAzure.com
- \*.microsoftonline.com
- \*.windows.net

##<a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-already-backed-by-the-azure-backup-service-using-the-vm-extension?-<br/>"></a>Mohu nainstalovat agenta Azure Backup na virtuální počítač Azure, který už je zálohovaný službou Azure Backup pomocí rozšíření virtuálního počítače? <br/>
Jistě. Azure Backup poskytuje zálohování na úrovni virtuálních počítačů pro virtuální počítače Azure, které používají rozšíření virtuálního počítače. Chcete-li chránit soubory a složky na hostovaném operačním systému Windows, můžete na něj nainstalovat agenta Azure Backup.

## <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-to-back-up-files-and-folders-present-on-temporary-storage-provided-by-the-azure-vm?-<br/>"></a>Mohu nainstalovat agenta Azure Backup na virtuální počítač Azure a použít ho k zálohování souborů a složek umístěných na dočasném úložišti poskytnutém virtuálním počítačem Azure? <br/>
Agenta Azure Backup můžete nainstalovat na hostovaný operační systém Windows a zálohovat soubory a složky na dočasné úložiště. Nicméně počítejte s tím, že zálohy selžou, jakmile dojde k vymazání dat na dočasném úložišti.  Navíc pokud dojde k vymazání dat na dočasném úložišti, budete moci provést obnovení pouze na stálé úložiště.

## <a name="i-have-installed-azure-backup-agent-to-protect-my-files-and-folders.-can-i-now-install-scdpm-to-work-with-azure-backup-agent-to-protect-on-premises-application/vm-workloads-to-azure?-<br/>"></a>Nainstaloval jsem agenta Azure Backup, aby chránil moje soubory a složky. Mohu nyní do Azure nainstalovat SCDPM, aby spolupracoval s Azure Backup při ochraně místních aplikací a úloh virtuálního počítače? <br/>
Chcete-li používat Azure Backup s SCDPM, doporučujeme nejdříve nainstalovat SCDPM a až poté nainstalovat agenta Azure Backup. To zajistí bezproblémovou integraci agenta Azure Backup s SCDPM a umožní ochranu souborů a složek, úloh aplikací a virtuálních počítačů do Azure přímo z konzoly pro správu SCDPM. Instalovat SCDPM až po instalaci agenta Azure Backup pro výše uvedené účely se nedoporučuje ani nepodporuje.

## <a name="what-is-the-length-of-file-path-that-can-be-specified-as-part-of-azure-backup-policy-using-azure-backup-agent?-<br/>"></a>Jaká je maximální délka cesty k souboru, kterou lze zadat jako součást zásady Azure Backup pomocí agenta Azure Backup? <br/>  
Agent Azure Backup se spoléhá na systém souborů NTFS. [Specifikace délky cesty k souboru je omezená rozhraním API systému Windows](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). V případě zálohování souborů, u nichž cesta k souboru přesahuje délku určenou rozhraním API systému Windows, můžete k zálohování zvolit nadřazenou složku nebo diskovou jednotku se soubory, které chcete zálohovat.  

## <a name="what-characters-are-allowed-in-file-path-of-azure-backup-policy-using-azure-backup-agent?-<br>"></a>Jaké znaky jsou povolené v cestě k souboru zásady Azure Backup pomocí agenta Azure Backup? <br>  
 Agent Azure Backup se spoléhá na systém souborů NTFS. Pro specifikaci souboru povoluje [znaky podporované systémem souborů NTFS](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions).  

## <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-(bmr)-backup-for-a-physical-server?-<br/>"></a>Mohu použít server Azure Backup k vytvoření zálohy úplného obnovení (BMR) pro fyzický server? <br/>
Ano.

## <a name="can-i-configure-the-backup-service-to-send-mail-if-a-backup-job-fails?-<br/>"></a>Mohu nakonfigurovat službu Backup, aby v případě selhání úlohy zálohování zaslala email? <br/>
Ano, služba Backup obsahuje několik výstrah založených na událostech, které lze použít ve skriptu prostředí PowerShell. Úplný popis naleznete v tématu [Oznámení o upozorněních](backup-azure-manage-vms.md#alert-notifications).

## <a name="is-there-a-limit-on-the-size-of-each-data-source-being-backed-up?-<br/>"></a>Existuje nějaké omezení velikosti jednotlivých zdrojů zálohovaných dat? <br/>
Zatímco na úrovni trezoru není množství dat, která lze zálohovat, nijak omezeno, u Azure Backup platí omezení (pro všechny praktické účely jsou limity dostatečně vysoké) maximální velikosti zdroje dat. K srpnu 2015 je maximální velikost zdroje dat pro podporované operační systémy:

|Zdroj č. | Operační systém |  Maximální velikost zdroje dat |
| :-------------: |:-------------| :-----|
|1| Windows Server 2012 nebo novější| 54400 GB|
|2| Windows 8 nebo novější| 54400 GB|
|3| Windows Server 2008, Windows Server 2008 R2 | 1700 GB|
|4| Windows 7 | 1700 GB|

Následující tabulka vysvětluje, jak je určená velikost jednotlivých zdrojů dat.

|   Zdroj dat  |   Podrobnosti |
| :-------------: |:-------------|
|Svazek |Množství dat zálohovaných z jednoho svazku serveru nebo klientského počítače.|
|Virtuální počítač s technologií Hyper-V | Součet dat všech virtuálních pevných disků zálohovaného virtuálního počítače.|
|Databáze Microsoft SQL Serveru | Velikost jedné zálohované databáze SQL. |
|Microsoft SharePoint |Součet databází obsahu a konfigurace v rámci zálohované farmy služby SharePoint.|
|Microsoft Exchange |Součet všech databází systému Exchange na zálohovaném serveru Exchange.|
|BMR/Stav systému |Každá jednotlivá kopie BMR nebo stavu systému zálohovaného počítače.|

## <a name="are-there-limits-on-the-number-of-times-a-backup-job-can-be-scheduled-per-day?<br/>"></a>Existují omezení počtu naplánovaných úloh zálohování za den?<br/>
Ano, úlohy zálohování na Windows Serveru nebo na klientu Windows můžete spustit až třikrát za den. Úlohy zálohování na System Center DPM můžete spustit až dvakrát za den. Úlohy zálohování pro virtuální počítače IaaS můžete spustit jednou za den.

## <a name="is-there-a-difference-between-the-scheduling-policy-for-dpm-and-windows-server-(i.e.-on-windows-server-without-dpm)?-<br/>"></a>Je nějaký rozdíl mezi zásadami plánování pro DPM a Windows Server (tj. pro Windows Server bez DPM)? <br/>
Ano. Pomocí DPM můžete zadat denní, týdenní, měsíční nebo roční plány. Windows Server (bez DPM) umožňuje zadat pouze denní nebo týdenní plány.

## <a name="is-there-a-difference-between-the-retention-policy-for-dpm-and-windows-server/client-(i.e.-on-windows-server-without-dpm)?<br/>"></a>Je nějaký rozdíl mezi zásadami uchovávání informací pro DPM a Windows Server nebo klienta (tj. pro Windows Server bez DPM)?<br/>
Ne, DPM i Windows Server nebo klient mohou mít denní, týdenní, měsíční nebo roční zásady uchovávání.

## <a name="can-i-configure-my-retention-policies-selectively-–-i.e.-configure-weekly-and-daily-but-not-yearly-and-monthly?<br/>"></a>Mohu konfigurovat zásady uchovávání informací selektivně – tj. nakonfigurovat týdenní a denní, ale ne roční a měsíční?<br/>
Ano, struktura uchovávání Azure Backup vám poskytuje úplnou flexibilitu při definování zásad uchovávání informací podle vašich požadavků.

## <a name="can-i-“schedule-a-backup”-at-6pm-and-specify-“retention-policies”-at-a-different-time?<br/>"></a>Mohu „naplánovat zálohování“ na 18:00 a pro „zásady uchovávání informací“ zadat jiný čas?<br/>
Ne. Zásady uchovávání informací lze aplikovat pouze na body záloh. Následující obrázek ukazuje zadanou zásadu uchovávání informací pro zálohy pořízené v 00:00 a v 18:00. <br/>

![Plánování zálohování a uchovávání](./media/backup-azure-backup-faq/Schedule.png)
<br/>

## <a name="is-an-incremental-copy-transferred-for-the-retention-policies-scheduled?-<br/>"></a>Je naplánované přenášení přírůstkové kopie pro zásady uchovávání informací? <br/>
Ne, přírůstková kopie se odesílá podle času uvedeného na stránce plánu zálohování. Body, které mohou být uchované, se určují podle zásady uchovávání informací.

## <a name="if-a-backup-is-retained-for-a-long-duration,-does-it-take-more-time-to-recover-an-older-data-point?-<br/>"></a>Pokud se záloha uchovává po dlouhou dobu, trvá pak obnovení staršího datového bodu déle? <br/>
 Ne – obnovení nejstaršího i nejnovějšího bodu trvá stejně dlouho. Každý bod obnovení se chová jako úplný bod.

## <a name="if-each-recovery-point-is-like-a-full-point,-does-it-impact-the-total-billable-backup-storage?<br/>"></a>Jestliže se každý bod obnovení chová jako úplný bod, ovlivní to celkové fakturovatelné úložiště zálohování?<br/>
Typické produkty s dlouhodobými body uchování ukládají zálohovaná data jako úplné body.  Úplné body jsou *neefektivní* z hlediska úložiště, ale jejich obnovení je snadnější a rychlejší. Přírůstkové kopie jsou *efektivní* z hlediska úložiště, ale vyžadují obnovení řetězu dat, což ovlivňuje dobu obnovení. Architektura úložiště Azure Backup nabízí to nejlepší z obou světů – optimální ukládání dat pro rychlé obnovení a nízké poplatky za úložiště. Tento přístup k ukládání dat zajišťuje efektivní využití příchozí i odchozí šířky pásma. Velikost úložiště dat i čas potřebný k obnovení dat jsou omezeny na minimum. Dozvědět se více o efektivitě [přírůstkového zálohování](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/). 

## <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-created?<br/>"></a>Je nějak omezený počet bodů obnovení, které lze vytvořit?<br/>
Ne. Omezení počtu bodů obnovení jsme odstranili. Můžete vytvořit tolik bodů obnovení, kolik si přejete.

## <a name="why-is-the-amount-of-data-transferred-in-backup-not-equal-to-the-amount-of-data-i-backed-up?<br/>"></a>Proč se liší množství přenesených dat v záloze od množství dat, která jsem zálohoval?<br/>
 Všechna data zálohovaná z agenta Azure Backup, SCDPM nebo serveru Azure Backup, projdou před odesláním kompresí a šifrováním. Po kompresi a šifrování jsou data v trezoru záloh o 30-40 % menší.

## <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-by-the-backup-service?<br/>"></a>Existuje způsob, jak nastavit šířku pásma používaného službou Backup?<br/>
 Ano, k úpravě šířky pásma použijte možnost **Změnit vlastnosti** v agentu Backup. Upravte šířku pásma a dobu, kdy tuto šířku pásma používáte. Další informace naleznete v tématu [Omezování sítě](../backup-configure-vault.md#enable-network-throttling).

## <a name="my-internet-bandwidth-is-limited-for-the-amount-of-data-i-need-to-back-up.-is-there-a-way-i-can-move-data-to-a-certain-location-with-a-large-network-pipe-and-push-that-data-into-azure?-<br/>"></a>Moje šířka internetového pásma je omezená pro množství dat, která potřebuji zálohovat. Existuje způsob, jak přenést data do určitého umístění s velkým síťovým kanálem a předat je do Azure? <br/>
Data do Azure můžete zálohovat přes standardní online proces zálohování, nebo můžete využít službu Import/export úložiště Azure pro přenos dat do úložiště objektů blob v Azure. Jiný způsob přenosu zálohovaných dat do úložiště Azure neexistuje. Informace o tom, jak používat službu Import/export úložiště Azure s Azure Backup naleznete v článku [Pracovní postup offline zálohování](backup-azure-backup-import-export.md).

## <a name="how-many-recoveries-can-i-perform-on-the-data-that-is-backed-up-to-azure?<br/>"></a>Kolikrát mohu provést obnovení dat zálohovaných v Azure?<br/>
Počet obnovení z Azure Backup není omezený.

## <a name="do-i-have-to-pay-for-the-egress-traffic-from-azure-data-center-during-recoveries?<br/>"></a>Musím platit za výchozí přenos z datového centra Azure během obnovování?<br/>
 Ne. Vaše obnovení jsou zdarma a výchozí přenos vám není účtován.

## <a name="is-the-data-sent-to-azure-encrypted?-<br/>"></a>Jsou data odesílaná do Azure šifrovaná? <br/>
Ano. Data jsou zašifrována na místním serveru, klientu nebo počítači SCDPM pomocí AES256 a odesílaná jsou přes zabezpečené spojení HTTPS.

## <a name="is-the-backup-data-on-azure-encrypted-as-well?<br/>"></a>Jsou šifrovaná i zálohovaná data v Azure?<br/>
 Ano. Data odeslaná do Azure zůstávají šifrovaná (neaktivní uložená data). Microsoft nikdy nedešifruje zálohovaná data. V případě zálohování virtuálních počítačů Azure se Azure Backup spoléhá na šifrování virtuálního počítače tj. pokud je váš virtuální počítač šifrovaný pomocí služby Azure Disk Encryption nebo jiné technologie šifrování, Azure Backup k zabezpečení dat použije toto šifrování. 

## <a name="what-is-the-minimum-length-of-encryption-key-used-to-encrypt-backup-data?-<br/>"></a>Jaká je minimální délka šifrovacího klíče, který se používá k šifrování zálohovaných dat? <br/>
 Šifrovací klíč by měl obsahovat alespoň 16 znaků.

## <a name="what-happens-if-i-misplace-the-encryption-key?-can-i-recover-the-data-(or)-can-microsoft-recover-the-data?-<br/>"></a>Co se stane, když ztratím šifrovací klíč? Mohu obnovit data (nebo) může Microsoft obnovit data? <br/>
Klíč, pomocí kterého se šifrují zálohovaná data, je přítomen pouze u zákazníka. Microsoft neudržuje jeho kopii v Azure a nemá ke klíči žádný přístup. Pokud zákazník klíč ztratí, Microsoft nemůže zálohovaná data obnovit.

## <a name="how-do-i-change-the-cache-location-specified-for-the-azure-backup-agent?<br/>"></a>Jak změním umístění mezipaměti, zadané pro agenta Azure Backup?<br/>
 Chcete-li změnit umístění mezipaměti, postupujte podle následujícího seznamu.
- Zastavte modul zálohování spuštěním následujícího příkazu v příkazovém řádku se zvýšenými oprávněními:

  ```PS C:\> Net stop obengine```

- Soubory nepřesouvejte. Místo toho zkopírujte složku s místem v mezipaměti na jinou jednotku s dostatkem volného místa. Po potvrzení, že zálohování s novým místem v mezipaměti funguje správně, můžete původní místo v mezipaměti odebrat.

- U následujících položek registru aktualizujte cestu k nové složce s místem v mezipaměti.<br/>

|Cesta k registru | Klíč registru | Hodnota |
| ------ | ------- | ------|
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` | ScratchLocation | *Nové umístění složky mezipaměti* |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` | ScratchLocation | *Nové umístění složky mezipaměti* |

- Restartujte modul Backup spuštěním následujícího příkazu v příkazovém řádku se zvýšenými oprávněními:

  ```PS C:\> Net start obengine```

  Po úspěšném dokončení vytvoření zálohy v novém umístění mezipaměti můžete původní složku mezipaměti odebrat.

## <a name="where-can-i-put-the-cache-folder-for-the-azure-backup-agent-to-work-as-expected?<br/>"></a>Kam mohu dát složku mezipaměti, aby agent Azure Backup fungoval podle očekávání?<br/>
Pro složku mezipaměti nedoporučujeme používat následující umístění:

- Sdílenou síťovou složku nebo vyměnitelné médium: Složka mezipaměti musí být místní pro server, který potřebuje zálohování pomocí online zálohování. Síťová umístění a vyměnitelná média jako jednotky USB nejsou podporovaná.
- Offline svazky: Složka mezipaměti musí být online pro očekávané zálohování pomocí agenta Azure Backup.

## <a name="are-there-any-attributes-of-the-cache-folder-that-are-not-supported?<br/>"></a>Jsou nějaké atributy složky mezipaměti, které nejsou podporované?<br/>
 Složka mezipaměti nepodporuje následující atributy nebo jejich kombinace:

- Šifrované
- S odstraněním duplicit
- Komprimované
- Řídké
- Bod rozboru

Pro správné fungování agenta Azure Backup doporučujeme, aby složka mezipaměti ani metadata virtuálního pevného disku neobsahovaly výše uvedené atributy.



<!--HONumber=Oct16_HO3-->


