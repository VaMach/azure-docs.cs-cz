---
title: "Zálohování Azure: Zálohování úloh DPM pomocí Powershellu | Microsoft Docs"
description: "Zjistěte, jak nasadit a spravovat Azure Backup pro Data Protection Manager (DPM) pomocí prostředí PowerShell"
services: backup
documentationcenter: 
author: Nkolli1
manager: shreeshd
editor: 
ms.assetid: bcbcef79-9d33-4e84-a558-9866614f2cae
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cwatson
ms.openlocfilehash: d296626c5bafcf7a059a617bcfef2d4524020713
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/15/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-data-protection-manager-dpm-servers-using-powershell"></a>Nasazení a správa zálohování do Azure pro servery DPM (Data Protection Manager) pomocí PowerShellu
> [!div class="op_single_selector"]
> * [ARM](backup-dpm-automation.md)
> * [Classic](backup-dpm-automation-classic.md)
>
>

Tento článek vysvětluje, jak pomocí prostředí PowerShell k zálohování a obnovení dat aplikace DPM z úložiště záloh. Společnost Microsoft doporučuje používat trezory služeb zotavení pro všechna nová nasazení. Pokud jste nového uživatele Azure Backup, použijte v článku [nasadit a spravovat Data Protection Manager dat do Azure pomocí prostředí PowerShell](backup-dpm-automation.md), takže data ukládáte do trezoru služeb zotavení.

> [!IMPORTANT]
> Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services. Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services. Po 30. listopadu 2017 nebudete moct pomocí prostředí PowerShell vytvořit trezory Backup. **2017 30 listopadu**:
>- Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.
>- Nebudete mít přístup k datům záloh na portálu Classic. Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.
>

## <a name="setting-up-the-powershell-environment"></a>Nastavení prostředí PowerShell
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

Před použitím prostředí PowerShell pro správu z aplikace Data Protection Manager zálohování do Azure, musíte mít správné prostředí v prostředí PowerShell. Na začátku relace prostředí PowerShell Ujistěte se, že spustíte následující příkaz a naimportovat moduly vpravo umožňují správně odkazovat rutiny aplikace DPM:

```
PS C:> & "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"

Welcome to the DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## <a name="setup-and-registration"></a>Instalace a registrace
Chcete-li začít:

1. [Stáhněte si nejnovější PowerShell](https://github.com/Azure/azure-powershell/releases) (minimální požadovaná verze je: 1.0.0)
2. Povolte rutiny pro zálohování Azure tak, že přepnutí na *AzureResourceManager* režimu pomocí **Switch-AzureMode** příkaz:

```
PS C:\> Switch-AzureMode AzureResourceManager
```

Následující instalaci a registraci úlohy je možné automatizovat pomocí prostředí PowerShell:

* Vytvoření trezoru záloh
* Instalace agenta Azure Backup
* Registrace u služby zálohování Azure
* Nastavení sítě
* Nastavení šifrování

### <a name="create-a-backup-vault"></a>Vytvoření trezoru záloh
> [!WARNING]
> Pro zákazníky pomocí služby Azure Backup poprvé budete muset registraci poskytovatele Azure Backup pro použití s vaším předplatným. To můžete provést spuštěním následujícího příkazu: Register-AzureProvider - ProviderNamespace "Microsoft.Backup"
>
>

Můžete vytvořit nové úložiště záloh pomocí **New-AzureRMBackupVault** příkaz. Trezor záloh je prostředek ARM, proto musíte umístit do skupiny prostředků. V prostředí Azure PowerShell konzolu se zvýšenými oprávněními spusťte následující příkazy:

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GRS
```

Můžete získat seznam všech záloh v daném předplatném pomocí **Get-AzureRMBackupVault** příkaz.

### <a name="installing-the-azure-backup-agent-on-a-dpm-server"></a>Instalace agenta Azure Backup na serveru aplikace DPM
Před instalací agenta Azure Backup, musíte mít instalační program stažené a existuje v systému Windows Server. Můžete získat nejnovější verzi instalačního programu z [Microsoft Download Center](http://aka.ms/azurebackup_agent) nebo ze stránky řídicího panelu trezoru záloh. Instalační program uložte na snadno dostupném místě jako * C:\Downloads\*.

Pokud chcete nainstalovat agenta, spusťte následující příkaz v konzolu prostředí PowerShell se zvýšenými oprávněními **na serveru DPM**:

```
PS C:\> MARSAgentInstaller.exe /q
```

Tím se nainstaluje agent s výchozími možnostmi. Instalace trvá několik minut na pozadí. Pokud nezadáte */nu* možnost **Windows Update** otevře se okno na konci instalace k vyhledání všech aktualizací.

Agenta se zobrazí v seznamu nainstalovaných programů. Chcete-li zobrazit seznam nainstalovaných programů, přejděte na **ovládací panely** > **programy** > **programy a funkce**.

![Instalaci agenta](./media/backup-dpm-automation/installed-agent-listing.png)

#### <a name="installation-options"></a>Možnosti instalace
Pokud chcete zobrazit všechny možnosti, které jsou k dispozici prostřednictvím příkazového řádku, použijte následující příkaz:

```
PS C:\> MARSAgentInstaller.exe /?
```

Mezi dostupné možnosti patří:

| Možnost | Podrobnosti | Výchozí |
| --- | --- | --- |
| /q |Tichá instalace |- |
| / p: "umístění" |Cesta ke složce instalace pro agenta Azure Backup. |C:\Program Files\Microsoft Azure Recovery Services agenta |
| / s: "umístění" |Cesta ke složce mezipaměti pro agenta Azure Backup. |C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch |
| /m |Výslovný souhlas ke službě Microsoft Update |- |
| /nu |Nekontrolovat aktualizace po dokončení instalace |- |
| /d |Odinstaluje Agenta Microsoft Azure Recovery Services. |- |
| /pH |Adresa proxy hostitele |- |
| /Po |Číslo portu proxy hostitele |- |
| /Pu |Uživatelské jméno proxy hostitele |- |
| /pW |Heslo pro proxy server |- |

### <a name="registering-with-the-azure-backup-service"></a>Registrace u služby zálohování Azure
Než můžete zaregistrovat pomocí služby Azure Backup, musíte zajistit, aby [požadavky](backup-azure-dpm-introduction.md) jsou splněny. Postupujte takto:

* Máte platné předplatné Azure
* Mít úložiště záloh

Chcete-li stáhnout přihlašovací údaje trezoru, spusťte **Get-AzureBackupVaultCredentials** příkaz do konzoly Azure PowerShell a úložiště do vhodného umístění, například * C:\Downloads\*.

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

Počítač zaregistrovat v úložišti se provádí pomocí [Start-DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) rutiny:

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-DPMCloudRegistration -DPMServerName "TestingServer" -VaultCredentialsFilePath $cred
```

To bude registrace serveru DPM s názvem "Testovacím" s trezorem Microsoft Azure pomocí přihlašovacích údajů, zadaný trezor.

> [!IMPORTANT]
> Nepoužívejte relativní cesty k určení souboru s přihlašovacími údaji. Jako vstup do rutiny, musí zadat absolutní cestu.
>
>

### <a name="initial-configuration-settings"></a>Počáteční konfigurace nastavení
Jakmile je DPM Server registrován s úložištěm záloh Azure, se spustí s výchozím nastavením odběru. Tato nastavení odběru zahrnují sítě, šifrování a pracovní oblasti. Chcete-li začít, změna nastavení přihlášení k odběru je nutné nejprve získat popisovač na existující nastavení (výchozí), pomocí [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) rutiny:

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

Všechny změny provedené tento objekt místní prostředí PowerShell ```$setting``` a pak se zaměřuje na aplikace DPM a Azure Backup, o jejich uložení úplné objekt pomocí [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny. Budete muset použít ```–Commit``` příznak zajistit, že jsou nastavené jako trvalé změny. Nastavení nebude použita a pokud potvrzené používá Azure Backup.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

### <a name="networking"></a>Sítě
Pokud připojení počítače aplikace DPM do služby Azure Backup na Internetu prostřednictvím proxy serveru, by měl nastavení serveru proxy zadaný pro zálohování úspěšné. To se provádí pomocí ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` a ```ProxyPassword``` parametry se [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny. V tomto příkladu není žádný proxy server, jsme jsou explicitně vymazání údaje související s proxy serverem.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

Využití šířky pásma se dá taky nastavit pomocí možnosti ```-WorkHourBandwidth``` a ```-NonWorkHourBandwidth``` pro danou sadu dny v týdnu. V tomto příkladu jsme nejsou nastavení žádné omezení.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

### <a name="configuring-the-staging-area"></a>Konfigurace pracovní oblasti
Agent Azure Backup spuštěný na serveru DPM potřebuje dočasné úložiště pro data obnovená z cloudu (místní pracovní oblasti). Nakonfigurovat pracovní oblasti pomocí [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny a ```-StagingAreaPath``` parametr.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

V příkladu nahoře, je možnost pracovní oblasti *C:\StagingArea* v prostředí PowerShell objektu ```$setting```. Ujistěte se, že v zadané složce již existuje, jinak se nezdaří poslední zápis nastavení přihlášení k odběru.

### <a name="encryption-settings"></a>Nastavení šifrování
Zálohování data přenášená do služby Azure Backup je zašifrovaná chránit jejich důvěrnost dat. Šifrovací přístupové heslo je 'heslo' k dešifrování dat v době obnovení. Je důležité udržovat tento informace o bezpečném po nastavení.

V následujícím příkladu převede první příkaz řetězec ```passphrase123456789``` zabezpečený řetězec a přiřadí zabezpečený řetězec proměnné s názvem ```$Passphrase```. v druhém příkazu nastaví zabezpečený řetězec v ```$Passphrase``` jako heslo pro šifrování záloh.

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> Zachování informací o přístupové heslo bezpečném, po nastavení. Nebudete schopni obnovit data ze služby Azure bez tohoto hesla.
>
>

V tomto okamžiku jste měli udělali všechny požadované změny pro ```$setting``` objektu. Mějte na paměti k provedení změn.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-to-azure-backup"></a>Ochrana dat do služby Azure Backup
V této části přidáte provozním serveru DPM a poté proveďte ochranu dat do místního úložiště DPM a pak do služby Azure Backup. V příkladech jsme se ukazují, jak zálohovat soubory a složky. Chcete-li zálohovat všechny zdroje dat aplikace DPM podporováno lze snadno rozšířit logiku. Všechny zálohy aplikace DPM se řídí pomocí ochranu skupiny (PG) s čtyři části:

1. **Členy skupiny** je seznam chránitelné objekty (také označované jako *zdrojů dat* v DPM), které chcete chránit ve stejné skupině ochrany. Chcete třeba chránit produkční virtuálních počítačů na jednu skupinu ochrany a databáze systému SQL Server v jiné skupiny ochrany, protože mají různé požadavky na zálohování. Předtím, než můžete zálohovat žádný zdroj dat na provozním serveru, které potřebujete, abyste měli jistotu, agenta aplikace DPM je nainstalován na serveru a je spravovaných aplikací DPM. Postupujte podle kroků pro [instalace agenta DPM](https://technet.microsoft.com/library/bb870935.aspx) a propojení na příslušný Server aplikace DPM.
2. **Způsob ochrany dat** Určuje umístění zálohování cíl - páska, disk a cloud. V našem příkladu jsme bude chránit data na místní disk a do cloudu.
3. A **plán zálohování** určující, kdy je třeba přijmout zálohy a jak často se mají synchronizovat data mezi serverem DPM a na provozním serveru.
4. A **plán uchovávání informací** který určuje, jak dlouho chcete zachovat body obnovení v Azure.

### <a name="creating-a-protection-group"></a>Vytvoření skupiny ochrany
Začněte vytvořením nové skupiny ochrany pomocí [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) rutiny.

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

Výše uvedené rutina vytvoří skupiny ochrany s názvem *ProtectGroup01*. Existující skupiny ochrany můžete také upravit později přidat zálohování do cloudu Azure. Ale žádné změny do skupiny ochrany – nové nebo existující - musíme získat popisovač *upravitelnými* pomocí [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) rutiny.

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-to-the-protection-group"></a>Přidání členů skupiny do skupiny ochrany
Každý Agent aplikace DPM zná seznam zdrojů dat na serveru, který je nainstalovaný. Přidat zdroje dat do skupiny ochrany, musí nejprve poslat seznam zdrojů dat zpět na DPM server agenta aplikace DPM. Jeden nebo více zdrojů dat se pak vybrali a přidají se do skupiny ochrany. Prostředí PowerShell kroky potřebné k získání dosáhnout tyto:

1. Získat seznam všech serverů spravovaných aplikací DPM pomocí agenta aplikace DPM.
2. Vyberte konkrétní server.
3. Získat seznam všech zdrojů dat na serveru.
4. Vyberte jeden nebo více zdrojů dat a přidat je do skupiny ochrany

Seznam serverů, ve kterých je nainstalován Agent aplikace DPM a je spravován serverem aplikace DPM je získán pomocí [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) rutiny. V tomto příkladu jsme bude filtrovat a konfigurovat jenom PS s názvem *productionserver01* pro zálohování.

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

Nyní se načíst seznam zdrojů dat v ```$server``` pomocí [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) rutiny. V tomto příkladu jsme pro svazek filtrování * D:\* který chcete konfigurovat pro zálohování. Tento zdroj dat se pak přidá do skupiny ochrany pomocí [přidat DPMChildDatasource](https://technet.microsoft.com/library/hh881732) rutiny. Nezapomeňte použít *modifable* objektu skupiny ochrany ```$MPG``` k budou přidány.

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

Opakujte tento krok tolikrát, kolikrát podle potřeby, dokud nepřidáte všechny zvolené zdroje dat do skupiny ochrany. Můžete začít s jenom jeden zdroj dat a dokončení pracovního postupu pro vytvoření skupiny ochrany a později přidat další datové zdroje do skupiny ochrany.

### <a name="selecting-the-data-protection-method"></a>Výběr způsobu ochrany dat
Jakmile zdroje dat byly přidány do skupiny ochrany, dalším krokem je zadat, ochranu pomocí metody [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) rutiny. V tomto příkladu skupiny ochrany bude instalační program pro místního disku a zálohování. Budete taky muset zadat zdroj dat, který chcete chránit, do cloudu pomocí [přidat DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) rutiny příznakem - Online.

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-the-retention-range"></a>Nastavení uchování
Nastavení uchovávání bodů zálohování pomocí [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) rutiny. Když může být lichý nastavení uchovávání předtím, než byla definována plán zálohování, pomocí ```Set-DPMPolicyObjective``` rutiny automaticky nastaví výchozí plán zálohování, který je pak možné upravit. Vždy je možné sady nejdřív naplánovat zálohování a po zásady uchovávání informací.

V následujícím příkladu rutiny nastavuje parametry, uchování pro zálohy disku. To bude uchování záloh pro 10 dnů a synchronizaci dat každých 6 hodin mezi provozním serveru a serverem DPM. ```SynchronizationFrequencyMinutes``` Nedefinuje četnost zálohování je bod vytvořen, ale jak často data se zkopírují na serveru aplikace DPM; záloh zabrání příliš velká.

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

Pro zálohování přejdete do Azure (aplikace DPM se týká jako zálohování Online) rozsahy uchovávání mohou být konfigurovány pro [dlouhodobé uchovávání používá schéma Dědečka. otec SYN (GFS)](backup-azure-backup-cloud-as-tape.md). To znamená můžete definovat zásady uchovávání informací kombinované zahrnující denní, týdenní, měsíční a roční zásady uchovávání informací. V tomto příkladu vytvoření představující schéma komplexní uchovávání, který má být pole a pak nakonfigurujte pomocí rozsah uchování [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) rutiny.

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-the-backup-schedule"></a>Nastavte plán zálohování
Aplikace DPM nastaví výchozí plán zálohování automaticky, pokud zadáte cíle ochrany pomocí ```Set-DPMPolicyObjective``` rutiny. Ke změně výchozích plánů, použijte [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) následovanou [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) rutiny.

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

V příkladu nahoře ```$onlineSch``` je pole se čtyřmi prvky, které obsahuje existující plán ochranu online pro skupinu ochrany ve schématu GFS:

1. ```$onlineSch[0]```bude obsahovat denní plán
2. ```$onlineSch[1]```bude obsahovat týdenní plán
3. ```$onlineSch[2]```bude obsahovat plánování měsíčně
4. ```$onlineSch[3]```bude obsahovat roční plán

Takže pokud budete muset upravit týdenní plán, budete muset naleznete ```$onlineSch[1]```.

### <a name="initial-backup"></a>Prvotní zálohování
Při první zálohování zdroje dat, aplikace DPM vyžaduje pro vytvoření počáteční repliky, který se vytvoří kopie zdroje dat, měly by být na svazku repliky DPM. Tato aktivita bude naplánována buď pro určitý čas nebo lze spustit ručně, pomocí [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) rutiny s parametrem ```-NOW```.

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-the-size-of-dpm-replica--recovery-point-volume"></a>Změna velikosti repliky aplikace DPM a svazek bodu obnovení
Můžete také změnit velikost svazku repliky DPM, jakož i stínové kopie svazku pomocí [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) rutiny jako v následujícím příkladu: Get-DatasourceDiskAllocation - Datasource $DS Set-DatasourceDiskAllocation -Datasource $DS - ProtectionGroup $MPG – ruční - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)

### <a name="committing-the-changes-to-the-protection-group"></a>Potvrzení změny do skupiny ochrany
Nakonec změny je nutné před provedením aplikace DPM může provést zálohování na novou konfiguraci skupiny ochrany. To se provádí pomocí [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) rutiny.

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-the-backup-points"></a>Zobrazení body zálohy
Můžete použít [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) rutiny se získat seznam všech bodů obnovení pro zdroj dat. V tomto příkladu provedeme následující:

* načtení všech PGs na serveru DPM, který se uloží v matici```$PG```
* získání zdroje dat odpovídající```$PG[0]```
* získání všech bodů obnovení pro zdroj dat.

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a>Obnovit data chráněná v Azure
Obnovení dat je kombinací ```RecoverableItem``` objektu a ```RecoveryOption``` objektu. V předchozí části My seznam body zálohy pro zdroj dat.

V následujícím příkladu jsme ukazují, jak k obnovení virtuálního počítače technologie Hyper-V z Azure Backup kombinací body zálohy s cíle pro obnovení. To zahrnuje následující:

* Vytváření možnost obnovení pomocí [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) rutiny.
* Načítání pole body zálohy pomocí ```Get-DPMRecoveryPoint``` rutiny.
* Výběr bod obnovení ze zálohy.

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

Příkazy můžete snadno rozšířit pro jakýkoli typ datasource.

## <a name="next-steps"></a>Další kroky
* Další informace o zálohování Azure pro aplikaci DPM najdete v části [Úvod k zálohování aplikace DPM](backup-azure-dpm-introduction.md)
