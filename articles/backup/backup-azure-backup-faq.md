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
     ms.date="07/01/2016"
     ms.author="trinadhk; giridham; arunak; markgal; jimpark;"/>

# Služba Azure Backup – Nejčastější dotazy

> [AZURE.SELECTOR]
- [Nejčastější dotazy pro klasický režim](backup-azure-backup-faq.md)
- [Nejčastější dotazy pro režim ARM](backup-azure-backup-ibiza-faq.md)

Tento článek je seznam častých dotazů (a příslušných odpovědí) ke službě Azure Backup. Naše komunita odpovídá rychle a pokud je dotaz pokládán často, přidáme ho do tohoto článku. Odpovědi na otázky obvykle poskytují odkaz nebo informace o podpoře. Dotazy k Azure Backup můžete pokládat v části Disqus u tohoto článku nebo u souvisejících článků. Dotazy k Azure Backup také můžete pokládat v [diskuzním fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## Instalace a konfigurace
**Otázka č. 1: Jaké jsou podporované operační systémy, ze kterých mohu zálohovat do Azure pomocí Azure Backup?** <br/>
Odpověď č. 1: Azure Backup podporuje následující seznam operačních systémů.


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

**Otázka č. 2: Kde mohu stáhnout nejnovější verzi agenta Azure Backup?** <br/>
Odpověď č. 2: Nejnovější verzi agenta pro zálohování Windows Serveru, aplikace System Center DPM nebo klienta Windows si můžete stáhnout [zde](http://aka.ms/azurebackup_agent). Chcete-li zálohovat virtuální počítač, použijte agenta virtuálního počítače (který automaticky nainstaluje správné rozšíření). Agent virtuálního počítače je již nainstalován na virtuálních počítačích vytvořených z galerie Azure.

**Otázka č. 3: Jaká verze serveru SCDPM je podporovaná?** <br/>
Odpověď č. 3: Doporučujeme vám nainstalovat [nejnovějšího](http://aka.ms/azurebackup_agent) agenta Azure Backup na nejnovější kumulativní aktualizaci SCDPM (UR6 k červenci 2015).

**Otázka č. 4: Při konfiguraci agenta Azure Backup se zobrazí výzva k zadání **přihlašovacích údajů k trezoru**. Mohou přihlašovací údaje trezoru vypršet?
Odpověď č. 4: Ano, přihlašovací údaje trezoru vyprší po 48 hodinách. Pokud soubor vyprší, přihlaste se k portálu Azure a stáhněte soubory s přihlašovacími údaji trezoru z trezoru služby Backup.

**Otázka č. 5: Je v rámci předplatného Azure nějak omezený počet trezorů záloh, které lze vytvořit?** <br/>
Odpověď č. 5: Ano. K červenci 2015 můžete v rámci jednoho předplatného vytvořit 25 trezorů. Pokud potřebujete další trezory, vytvořte nové předplatné.

**Otázka č. 6: Mám považovat trezor za fakturační položku?** <br/>
Odpověď č. 6: Ačkoli je možné získat pro každý trezor podrobný účet, důrazně doporučujeme považovat za fakturační položku předplatné Azure. To je konzistentní napříč všemi službami a jednodušší pro správu.

**Otázka č. 7: Je nějak omezený počet serverů/počítačů, které lze zaregistrovat k trezoru?** <br/>
Odpověď č. 7: Ano, k jednomu trezoru můžete zaregistrovat až 50 počítačů. Pro virtuální počítače Azure IaaS je limit 200 virtuálních počítačů na jeden trezor. Pokud potřebujete zaregistrovat více počítačů, vytvořte nový trezor.

**Otázka č. 8: Je nějak omezené množství dat, která lze zálohovat ze serveru/klientu Windows nebo serveru SCDPM?** <br/>
Odpověď č. 8: Ne.

**Otázka č. 9: Jak mohu zaregistrovat svůj server k jinému datovému centru?**<br/>
Odpověď č. 9: Zálohovaná data se odesílají do datového centra služby Backup, ke které je zaregistrované. Nejjednodušší způsob, jak změnit datové centrum, je odinstalování agenta a jeho přeinstalování a zaregistrování k novému datovému centru.

**Otázka č. 10: Co se stane, když přejmenuji server Windows, který zálohuje data do Azure?**<br/>
Odpověď č. 10: Když server přejmenujete, všechna stávající nastavená zálohování se zastaví.
Je třeba zaregistrovat nový název serveru k trezoru služby Backup. Když vytvoříte novou registraci, první zálohování je úplné zálohování a ne přírůstkové zálohování. Pokud potřebujete obnovit data, která byla dříve zálohovaná do trezoru se starým názvem, můžete tak učinit pomocí možnosti [**Jiný server**](backup-azure-restore-windows-server.md#recover-to-an-alternate-machine) v průvodci **Obnova dat**.


**Otázka č. 11: Z jakých typů jednotek mohu zálohovat soubory a složky?** <br/>
Odpověď č. 11: Následující jednotky/svazky nelze zálohovat:

- Vyměnitelné médium: Jednotka se musí hlásit jako pevná, aby se dala použít jako zdroj položek k zálohování.
- Svazky jen pro čtení: Svazek musí být zapisovatelný, aby fungovala služba Stínová kopie svazku (VSS).
- Offline svazky: Svazek musí být online, aby fungovala služba VSS.
- Sdílené síťové složky: Svazek musí být místní pro server, aby mohl být zálohovaný pomocí online zálohování.
- Svazky chráněné nástrojem Bitlocker: Předtím, než může dojít k zálohování, musí být svazek odemčený.
- Identifikace systému souborů: Pro tuto verzi služby online zálohování je jediným podporovaným systémem souborů NTFS.

**Otázka č. 12: Jaké typy souborů a složek mohu zálohovat ze svého serveru?**<br/>
Odpověď č. 12: Jsou podporovány následující typy:

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

**Otázka č. 13: Jaký je požadavek na minimální velikost složky mezipaměti?** <br/>
Odpověď č. 13: Velikost složky mezipaměti určuje množství dat, která zálohujete. Složka mezipaměti by měla zabírat 5 % požadovaného místa pro úložiště dat.

**Otázka č. 14: Když má moje organizace jeden trezor zálohování, jak mohu během obnovování dat izolovat data jednoho serveru od jiného?**<br/>
Odpověď č. 14: Všechny servery zaregistrované ke stejnému trezoru mohou obnovit data zálohovaná ostatními servery, *které používají stejné heslo* Máte-li servery, jejichž zálohovaná data chcete izolovat od ostatních serverů ve vaší organizaci, použijte pro tyto servery vyhrazené heslo. Například servery lidských zdrojů mohou používat jedno šifrovací heslo, účetní servery jiné a servery úložiště ještě jiné.

**Otázka č. 15: Mohu „migrovat“ zálohovaná data mezi předplatnými?** <br/>
Odpověď č. 15: Ne.

**Otázka č. 16: Mohu „migrovat“ trezor záloh mezi předplatnými?** <br/>
Odpověď č. 16: Ne. Trezor je vytvořený na úrovni předplatného a po vytvoření ho nelze přiřadit k jinému předplatnému.

**Otázka č. 17: Funguje agent Azure Backup na serveru, který používá odstranění duplicit Windows Serveru 2012?** <br/>
Odpověď č. 17: Ano. Služba agenta během přípravy operace zálohování převádí odstraněná duplicitní data na normální data. Poté optimalizuje data pro zálohování, zašifruje je, a zašifrovaná data odešle do online služby zálohování.

**Otázka č. 18: Pokud zruším úlohu zálohování poté, co již byla spuštěná, dojde k odstranění přenášených dat?** <br/>
Odpověď č. 18: Ne. Trezor záloh uloží všechna zálohovaná data, která byla přenesená až do okamžiku zrušení. Azure Backup používá mechanismus kontrolních bodů k příležitostnému přidávání kontrolních bodů do zálohovaných dat během zálohování. Díky kontrolním bodům v zálohovaných datech je možné při dalším procesu zálohování ověřit integritu souborů. Další aktivované zálohování pak bude přírůstkové přes data zálohovaná dříve. Přírůstkové zálohování poskytuje lepší využití šířky pásma, takže není nutné přenášet stejná data opakovaně.

**Otázka č. 19: Proč se zobrazuje upozornění „Zálohování Azure nebyla pro tento server konfigurovaná“ i přesto, že jsem už dříve naplánoval pravidelná zálohování?** <br/>
Odpověď č. 19: Toto upozornění se zobrazí v případě, že se nastavení plánu zálohování uložené na místním serveru neshoduje s nastavením uloženým v trezoru záloh. Pokud došlo k obnovení serveru nebo nastavení do známého stavu, může dojít ke ztrátě synchronizace plánů zálohování. Pokud se zobrazí toto upozornění, [překonfigurujte zásadu zálohování](backup-azure-manage-windows-server.md) a poté **spusťte Zálohovat nyní**, aby došlo k opětovné synchronizaci místního serveru s Azure. 

**Otázka č. 20: Jaká pravidla brány firewall mají být nakonfigurována pro Azure Backup?** <br/>
Odpověď č. 20: Kvůli bezproblémové ochraně dat odesílaných z místních prostředků nebo úlohy do Azure doporučujeme povolit bráně firewall komunikaci s následujícími adresami URL:

- www.msftncsi.com
- \*.Microsoft.com
- \*.WindowsAzure.com
- \*.microsoftonline.com
- \*.windows.net

**Otázka č. 21: Mohu nainstalovat agenta Azure Backup na virtuální počítač Azure, který už je zálohovaný službou Azure Backup pomocí rozšíření virtuálního počítače?** <br/>
Odpověď č. 21: Jistě. Azure Backup poskytuje zálohování na úrovni virtuálních počítačů pro virtuální počítače Azure, které používají rozšíření virtuálního počítače. Chcete-li chránit soubory a složky na hostovaném operačním systému Windows, můžete na něj nainstalovat agenta Azure Backup.

**Otázka č. 22: Mohu nainstalovat agenta Azure Backup na virtuální počítač Azure a použít ho k zálohování souborů a složek umístěných na dočasném úložišti poskytnutém virtuálním počítačem Azure?** <br/>
Odpověď č. 22: Agenta Azure Backup můžete nainstalovat na hostovaný operační systém Windows a zálohovat soubory a složky na dočasné úložiště. Nicméně počítejte s tím, že zálohy selžou, jakmile dojde k vymazání dat na dočasném úložišti.  Navíc pokud dojde k vymazání dat na dočasném úložišti, budete moci provést obnovení pouze na stálé úložiště.

**Otázka č. 23: Nainstaloval jsem agenta Azure Backup, aby chránil moje soubory a složky. Mohu nyní do Azure nainstalovat SCDPM, aby spolupracoval s Azure Backup při ochraně místních aplikací a úloh virtuálního počítače?** <br/>
Odpověď č. 23: Chcete-li používat Azure Backup s SCDPM, doporučujeme nejdříve nainstalovat SCDPM a až poté nainstalovat agenta Azure Backup. To zajistí bezproblémovou integraci agenta Azure Backup s SCDPM a umožní ochranu souborů a složek, úloh aplikací a virtuálních počítačů do Azure přímo z konzoly pro správu SCDPM. Instalovat SCDPM až po instalaci agenta Azure Backup pro výše uvedené účely není doporučeno, ani podporováno.

**Otázka č. 24: Jaká je maximální délka cesty k souboru, kterou lze zadat jako součást zásady Azure Backup pomocí agenta Azure Backup?** <br/>  
Odpověď č. 24: Agent Azure Backup se spoléhá na systém souborů NTFS. [Specifikace délky cesty k souboru je omezená rozhraním API systému Windows](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). V případě zálohování souborů, u nichž cesta k souboru přesahuje délku určenou rozhraním API systému Windows, můžete k zálohování zvolit nadřazenou složku nebo diskovou jednotku se soubory, které chcete zálohovat.  

**Otázka č. 25: Jaké znaky jsou povolené v cestě k souboru zásady Azure Backup pomocí agenta Azure Backup?** <br>  
Odpověď č. 25: Agent Azure Backup se spoléhá na systém souborů NTFS. Pro specifikaci souboru povoluje [znaky podporované systémem souborů NTFS](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions).  

**Otázka č. 26: Mohu použít server Azure Backup k vytvoření zálohy úplného obnovení (BMR) pro fyzický server?** <br/>
Odpověď č. 26: Ano.

**Otázka č. 27: Mohu nakonfigurovat službu Backup, aby v případě selhání úlohy zálohování zaslala email?** <br/>
Odpověď č. 27: Ano, služba Backup obsahuje několik výstrah založených na událostech, které lze použít ve skriptu prostředí PowerShell. Úplný popis naleznete v tématu [Oznámení o upozorněních](backup-azure-manage-vms.md#alert-notifications).



## Zálohování a uchovávání
**Otázka č. 1: Existuje nějaké omezení velikosti jednotlivých zdrojů zálohovaných dat?** <br/>
Odpověď č. 1: K srpnu 2015 je maximální velikost zdroje dat pro podporované operační systémy:

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

**Otázka č. 2: Existují omezení počtu naplánovaných úloh zálohování za den?**<br/>
Odpověď č. 2: Ano, úlohy zálohování na Windows Serveru nebo na klientu Windows můžete spustit až třikrát za den. Úlohy zálohování na System Center DPM můžete spustit až dvakrát za den. Úlohy zálohování pro virtuální počítače IaaS můžete spustit jednou za den.

**Otázka č. 3: Je nějaký rozdíl mezi zásadami plánování pro DPM a Windows Server (tj. pro Windows Server bez DPM)?** <br/>
Odpověď č. 3: Ano. Pomocí DPM můžete zadat denní, týdenní, měsíční nebo roční plány. Windows Server (bez DPM) umožňuje zadat pouze denní nebo týdenní plány.

**Otázka č. 4: Je nějaký rozdíl mezi zásadami uchovávání informací pro DPM a Windows Server nebo klienta (tj. pro Windows Server bez DPM)?**<br/>
Odpověď č. 4: Ne, DPM i Windows Server nebo klient mohou mít denní, týdenní, měsíční nebo roční zásady uchovávání.

**Otázka č. 5: Mohu konfigurovat zásady uchovávání informací selektivně – tj. nakonfigurovat týdenní a denní, ale ne roční a měsíční?**<br/>
Odpověď č. 5: Ano, struktura uchovávání Azure Backup vám poskytuje úplnou flexibilitu při definování zásad uchovávání informací podle vašich požadavků.

**Otázka č. 6: Mohu „naplánovat zálohování“ na 18:00 a pro „zásady uchovávání informací“ zadat jiný čas?**<br/>
Odpověď č. 6: Ne. Zásady uchovávání informací lze aplikovat pouze na body záloh. Následující obrázek ukazuje zadanou zásadu uchovávání informací pro zálohy pořízené v 00:00 a v 18:00. <br/>

![Plánování zálohování a uchovávání](./media/backup-azure-backup-faq/Schedule.png)
<br/>

**Otázka č. 7: Je naplánované přenášení přírůstkové kopie pro zásady uchovávání informací?** <br/>
Odpověď č. 7: Ne, přírůstková kopie se odesílá podle času uvedeného na stránce plánu zálohování. Body, které mohou být uchované, se určují podle zásady uchovávání informací.

**Otázka č. 8: Pokud se záloha uchovává po dlouhou dobu, trvá pak obnovení staršího datového bodu déle?** <br/>
Odpověď č. 8: Ne – obnovení nejstaršího i nejnovějšího bodu trvá stejně dlouho. Každý bod obnovení se chová jako úplný bod.

**Otázka č. 9: Jestliže se každý bod obnovení chová jako úplný bod, ovlivní to celkové fakturovatelné úložiště zálohování?**<br/>
Odpověď č. 9:  Typické produkty s dlouhodobými body uchování ukládají zálohovaná data jako úplné body.  Úplné body jsou *neefektivní* z hlediska úložiště, ale jejich obnovení je snadnější a rychlejší. Přírůstkové kopie jsou *efektivní* z hlediska úložiště, ale vyžadují obnovení řetězu dat, což ovlivňuje dobu obnovení. Architektura úložiště Azure Backup nabízí to nejlepší z obou světů – optimální ukládání dat pro rychlé obnovení a nízké poplatky za úložiště. Tento přístup k ukládání dat zajišťuje efektivní využití příchozí i odchozí šířky pásma. Velikost úložiště dat i čas potřebný k obnovení dat jsou omezeny na minimum.

**Otázka č. 10: Je nějak omezený počet bodů obnovení, které lze vytvořit?**<br/>
Odpověď č. 10: Ne. Omezení počtu bodů obnovení jsme odstranili. Můžete vytvořit tolik bodů obnovení, kolik si přejete.

**Otázka č. 11: Proč se liší množství přenesených dat v záloze od množství dat, která jsem zálohoval?**<br/>
Odpověď č. 11: Všechna zálohovaná data jsou před přenášením komprimovaná a šifrovaná. Po kompresi a šifrování jsou data v trezoru záloh o 30-40 % menší.

**Otázka č. 12: Existuje způsob, jak nastavit šířku pásma používaného službou Backup?**<br/>
Odpověď č. 12: Ano, k úpravě šířky pásma použijte možnost **Změnit vlastnosti** v agentu Backup. Upravte šířku pásma a dobu, kdy tuto šířku pásma používáte. Další informace naleznete v tématu [Omezování sítě](../backup-configure-vault.md#enable-network-throttling).

**Otázka č. 13: Moje šířka internetového pásma je omezená pro množství dat, která potřebuji zálohovat. Existuje způsob, jak přenést data do určitého umístění s velkým síťovým kanálem a předat je do Azure?** <br/>
Odpověď č. 13: Data do Azure můžete zálohovat přes standardní online proces zálohování, nebo můžete využít službu Import/export úložiště Azure pro přenos dat do úložiště objektů blob v Azure. Jiný způsob přenosu zálohovaných dat do úložiště Azure neexistuje. Informace o tom, jak používat službu Import/export úložiště Azure s Azure Backup naleznete v článku [Pracovní postup offline zálohování](backup-azure-backup-import-export.md).


## Obnovení
**Otázka č. 1: Kolikrát mohu provést obnovení dat zálohovaných v Azure?**<br/>
Odpověď č. 1: Počet obnovení z Azure Backup není omezený.

**Otázka č. 2: Musím platit za výchozí přenos z datového centra Azure během obnovování?**<br/>
Odpověď č. 2: Ne. Vaše obnovení jsou zdarma a výchozí přenos vám není účtován.

## Zabezpečení
**Otázka č. 1: Jsou data odesílaná do Azure šifrovaná?** <br/>
Odpověď č. 1: Ano. Data jsou zašifrována na místním serveru, klientu nebo počítači SCDPM pomocí AES256 a odesílaná jsou přes zabezpečené spojení HTTPS.

**Otázka č. 2: Jsou šifrovaná i zálohovaná data v Azure?**<br/>
Odpověď č. 2: Ano. Data odeslaná do Azure zůstávají šifrovaná (neaktivní uložená data). Microsoft nikdy nedešifruje zálohovaná data.

**Otázka č. 3: Jaká je minimální délka šifrovacího klíče, který se používá k šifrování zálohovaných dat?** <br/>
Odpověď č. 3: Šifrovací klíč by měl obsahovat alespoň 16 znaků.

**Otázka č. 4: Co se stane, když ztratím šifrovací klíč? Mohu obnovit data (nebo) může Microsoft obnovit data?** <br/>
Odpověď č. 4: Klíč, pomocí kterého se šifrují zálohovaná data, je přítomen pouze u zákazníka. Microsoft neudržuje jeho kopii v Azure a nemá ke klíči žádný přístup. Pokud zákazník klíč ztratí, Microsoft nemůže zálohovaná data obnovit.
 

## Mezipaměť zálohování

**Otázka č. 1: Jak změním umístění mezipaměti, zadané pro agenta Azure Backup?**<br/>
Odpověď č. 1: Chcete-li změnit umístění mezipaměti, postupujte podle následujícího seznamu.
- Zastavte modul zálohování spuštěním následujícího příkazu v příkazovém řádku se zvýšenými oprávněními:

  ```PS C:\> Net stop obengine```

- Soubory nepřesouvejte. Místo toho zkopírujte složku s místem v mezipaměti na jinou jednotku s dostatkem volného místa. Po potvrzení, že zálohování s novým místem v mezipaměti funguje správně, můžete původní místo v mezipaměti odebrat.

- U následujících položek registru aktualizujte cestu k nové složce s místem v mezipaměti.<br/>

|Cesta k registru | Klíč registru | Hodnota |
| ------ | ------- | ------|
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` | ScratchLocation | *Nové umístění složky mezipaměti* |
| `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` | ScratchLocation | *Nové umístění složky mezipaměti* |

- Znovu spusťte modul Backup spuštěním následujícího příkazu v příkazovém řádku se zvýšenými oprávněními:

  ```PS C:\> Net start obengine```

  Po úspěšném dokončení vytvoření zálohy v novém umístění mezipaměti můžete původní složku mezipaměti odebrat.

**Otázka č. 2: Kam mohu dát složku mezipaměti, aby agent Azure Backup fungoval podle očekávání?**<br/>
Odpověď č. 2: Pro složku mezipaměti nedoporučujeme používat následující umístění:

- Sdílenou síťovou složku nebo vyměnitelné médium: Složka mezipaměti musí být místní pro server, který potřebuje zálohování pomocí online zálohování. Síťová umístění a vyměnitelná média jako jednotky USB nejsou podporovaná.
- Offline svazky: Složka mezipaměti musí být online pro očekávané zálohování pomocí agenta Azure Backup.

**Otázka č. 3: Jsou nějaké atributy složky mezipaměti, které nejsou podporované?**<br/>
Odpověď č. 3: Složka mezipaměti nepodporuje následující atributy nebo jejich kombinace:

- Šifrované
- S odstraněním duplicit
- Komprimované
- Řídké
- Bod rozboru

Pro správné fungování agenta Azure Backup doporučujeme, aby složka mezipaměti ani metadata virtuálního pevného disku neobsahovaly výše uvedené atributy.



<!---HONumber=Aug16_HO4-->


