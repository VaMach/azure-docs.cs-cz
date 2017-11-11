---
title: "Spravovat zálohy systému Windows Server v Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Nasazení a Správa zálohování Windows serveru pomocí prostředí PowerShell."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: e7e269e2-1f11-41a9-957b-a2155de6a1e0
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: saurse;markgal;nkolli;trinadhk
ms.openlocfilehash: 21e84d6dd7d5064908555a2f7c64f39816c2956f
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/11/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-windows-serverwindows-client-using-powershell"></a>Nasazení a správa zálohování do Azure pro servery Windows / klienty Windows pomocí PowerShellu
> [!div class="op_single_selector"]
> * [ARM](backup-client-automation.md)
> * [Classic](backup-client-automation-classic.md)
>
>

Tento článek vysvětluje, jak pomocí prostředí PowerShell pro zálohování systému Windows Server nebo Windows pracovní stanice data do úložiště záloh. Společnost Microsoft doporučuje používat trezory služeb zotavení pro všechna nová nasazení. Pokud jsou nového uživatele Azure Backup a nevytvořili trezor záloh v rámci vašeho předplatného, použijte v článku [nasadit a spravovat Data Protection Manager dat do Azure pomocí prostředí PowerShell](backup-client-automation.md) tak data ukládáte do trezoru služeb zotavení. 

> [!IMPORTANT]
> Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services. Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.<br/> Po 30. listopadu 2017 už budete moci použít PowerShell k vytvoření trezory Backup. **2017 30 listopadu**:
>- Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.
>- Nebudete mít přístup k datům záloh na portálu Classic. Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.
>

## <a name="install-azure-powershell"></a>Instalace prostředí Azure PowerShell
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

Října 2015 byla vydána Azure PowerShell 1.0. Tato verze 0.9.8 bylo úspěšně dokončeno. Uvolněte a uvést do režimu o významné změny, zejména v vzoru pro pojmenovávání rutin. Rutiny verze 1.0 dodržují formát pojmenování {sloveso}-AzureRm {podstatné jméno}, kdežto názvy ve verzi 0.9.8 neobsahují označení **Rm** (třeba New-AzureRmResourceGroup místo New-AzureResourceGroup). Pokud používáte prostředí Azure PowerShell 0.9.8, musíte nejdřív spuštěním příkazu **Switch-AzureMode AzureResourceManager** spustit režim Resource Manager. Tento příkaz není nutné v 1.0 nebo novější.

Pokud chcete použít skripty napsané pro verzi 0.9.8 prostředí, v prostředí 1.0 nebo novější, měli byste pečlivě otestovat skripty v předprodukčním prostředí před jejich používání v produkčním prostředí předejdete neočekávané dopad.

[Stáhněte si nejnovější verzi prostředí PowerShell](https://github.com/Azure/azure-powershell/releases) (minimální požadovaná verze je: 1.0.0)

[!INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-backup-vault"></a>Vytvoření trezoru záloh
> [!WARNING]
> Pro zákazníky pomocí služby Azure Backup poprvé budete muset registraci poskytovatele Azure Backup pro použití s vaším předplatným. To můžete provést spuštěním následujícího příkazu: Register-AzureProvider - ProviderNamespace "Microsoft.Backup"
>
>

Můžete vytvořit nové úložiště záloh pomocí **New-AzureRMBackupVault** rutiny. Trezor záloh je prostředek ARM, proto musíte umístit do skupiny prostředků. V prostředí Azure PowerShell konzolu se zvýšenými oprávněními spusťte následující příkazy:

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

Použití **Get-AzureRMBackupVault** rutiny seznamu trezorů záloh v předplatném.

## <a name="installing-the-azure-backup-agent"></a>Instalace agenta Azure Backup
Před instalací agenta Azure Backup, musíte mít instalační program stažené a existuje v systému Windows Server. Můžete získat nejnovější verzi instalačního programu z [Microsoft Download Center](http://aka.ms/azurebackup_agent) nebo ze stránky řídicího panelu trezoru záloh. Instalační program uložte na snadno dostupném místě jako * C:\Downloads\*.

Pokud chcete nainstalovat agenta, spusťte v konzolu prostředí PowerShell se zvýšenými oprávněními následující příkaz:

```
PS C:\> MARSAgentInstaller.exe /q
```

Tím se nainstaluje agent s výchozími možnostmi. Instalace trvá několik minut na pozadí. Pokud nezadáte */nu* možnost potom **Windows Update** otevře se okno na konci instalace k vyhledání všech aktualizací. Po instalaci agenta se zobrazí v seznamu nainstalovaných programů.

Chcete-li zobrazit seznam nainstalovaných programů, přejděte na **ovládací panely** > **programy** > **programy a funkce**.

![Instalaci agenta](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a>Možnosti instalace
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

## <a name="registering-with-the-azure-backup-service"></a>Registrace u služby zálohování Azure
Než můžete zaregistrovat pomocí služby Azure Backup, musíte zajistit, aby [požadavky](backup-configure-vault.md) jsou splněny. Postupujte takto:

* Máte platné předplatné Azure
* Mít úložiště záloh

Chcete-li stáhnout přihlašovací údaje trezoru, spusťte **Get-AzureRMBackupVaultCredentials** rutiny do konzoly Azure PowerShell a úložiště do vhodného umístění, například * C:\Downloads\*.

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

Počítač zaregistrovat v úložišti se provádí pomocí [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) rutiny:

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration -VaultCredentials $cred -Confirm:$false

CertThumbprint      : 7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName : test-vault
Region              : West US

Machine registration succeeded.
```

> [!IMPORTANT]
> Nepoužívejte relativní cesty k určení souboru s přihlašovacími údaji. Jako vstup do rutiny, musí zadat absolutní cestu.
>
>

## <a name="networking-settings"></a>Nastavení sítě
Pokud připojení Windows počítače k Internetu prostřednictvím proxy serveru, nastavení proxy serveru se dá zadat i k agentovi. V tomto příkladu neexistuje žádný proxy server, takže jsme jsou explicitně vymazání údaje související s proxy serverem.

Využití šířky pásma se dá taky nastavit pomocí možnosti ```work hour bandwidth``` a ```non-work hour bandwidth``` pro danou sadu dny v týdnu.

Nastavení serveru proxy a šířky pásma podrobnosti se provádí pomocí [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) rutiny:

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a>Nastavení šifrování
Zálohování data přenášená do služby Azure Backup je zašifrovaná chránit jejich důvěrnost dat. Šifrovací přístupové heslo je 'heslo' k dešifrování dat v době obnovení.

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
Server properties updated successfully
```

> [!IMPORTANT]
> Zachování informací o přístupové heslo bezpečném, po nastavení. Nebudete schopni obnovit data ze služby Azure bez tohoto hesla.
>
>

## <a name="back-up-files-and-folders"></a>Zálohování souborů a složek
Zásady se řídí všechny zálohy ze Windows serverů a klientů do služby Azure Backup. Zásady se skládá ze tří částí:

1. A **plán zálohování** určující při zálohování muset provést a synchronizovat se službou.
2. A **plán uchovávání informací** který určuje, jak dlouho chcete zachovat body obnovení v Azure.
3. A **zadání zahrnutí nebo vyloučení souboru** který určuje, co by měl být zálohovány.

V tomto dokumentu protože jsme se automatizaci zálohování, budete předpokládáme, že nic nebyl nakonfigurován. Začneme vytvořením nové zásady zálohování pomocí [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) rutiny a jeho použití.

```
PS C:\> $newpolicy = New-OBPolicy
```

V tuto chvíli je prázdné zásady a ostatní rutiny jsou potřebné k definování položky, které je budou zahrnuté ani vyloučené, při zálohování se spustí, a tam, kde se uloží v zálohování.

### <a name="configuring-the-backup-schedule"></a>Konfigurace plánu zálohování
První den 3 části zásada je plán zálohování, která je vytvořena pomocí [New-OBSchedule](https://technet.microsoft.com/library/hh770401) rutiny. Plán zálohování definuje, kdy je třeba přijmout zálohy. Při vytváření plánu je třeba zadat 2 vstupní parametry:

* **Dny v týdnu** , má-li spustit zálohování. Můžete spustit úlohu zálohování na právě jeden den nebo každý den v týdnu nebo libovolnou kombinaci mezi nimi.
* **Časy dne** spuštění zálohování. Můžete definovat až 3 různou dobu, kdy se spustí zálohování.

Například může nakonfigurovat zásady zálohování, který se spustí na 16: 00 každou sobotu a neděli.

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

Plán zálohování musí být přidružený k zásadě, a to jde dosáhnout pomocí [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) rutiny.

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a>Konfigurace zásad uchovávání
Zásady uchovávání informací definuje, jak dlouho se uchovají body obnovení vytvořené z úlohy zálohování. Při vytváření nové zásady uchovávání informací pomocí [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) rutiny, můžete zadat počet dnů, které je třeba body obnovení záloh pro zachování s Azure Backup. Následující příklad nastaví zásady uchovávání informací 7 dní.

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

Zásady uchovávání informací musí být přidružené hlavní zásadám, pomocí rutiny [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):

```
PS C:\> Set-OBRetentionPolicy -Policy $newpolicy -RetentionPolicy $retentionpolicy

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          :
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```
### <a name="including-and-excluding-files-to-be-backed-up"></a>Zahrnutí a vyloučení souborů k zálohování
```OBFileSpec``` Objekt definuje soubory zahrnout a vyloučit zálohu. To je sada pravidel, která obor chráněné soubory a složky na počítači. Můžete mít mnoho souboru pravidla zahrnutí nebo vyloučení podle potřeby a přidružovat je k zásadě. Při vytváření nového objektu OBFileSpec, můžete:

* Zadejte soubory a složky, které mají být zahrnuty
* Zadejte soubory a složky, které se mají vyloučit
* Zadejte rekurzivní zálohování dat v složku (nebo) zda pouze nejvyšší úrovně soubory v zadané složce by měl být zálohuje nahoru.

K tomu se dosáhne použitím příznaku - nerekurzivní v příkazu New-OBFileSpec.

V následujícím příkladu jsme budete zálohování svazku C: a D: a vyloučit binární soubory operačního systému ve složce Windows a všechny dočasné složky. K tomu vytvoříme dvě specifikace pomocí souboru [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) rutiny – jeden pro zahrnutí a jeden pro vyloučení. Po vytvoření souboru specifikace jsou spojené s použitím zásad [přidat OBFileSpec](https://technet.microsoft.com/library/hh770424) rutiny.

```
PS C:\> $inclusions = New-OBFileSpec -FileSpec @("C:\", "D:\")

PS C:\> $exclusions = New-OBFileSpec -FileSpec @("C:\windows", "C:\temp") -Exclude

PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $inclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid


PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $exclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\windows
                  IsExclude:True
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\temp
                  IsExclude:True
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```

### <a name="applying-the-policy"></a>Použití zásad
Objekt zásad teď dokončení a má přidružené plánu zálohování, zásady uchovávání informací a seznam souborů zahrnutí nebo vyloučení. Tato zásada může být nyní potvrdit pro zálohování Azure používat. Před použitím nově vytvořenou zásadu zajistěte, aby žádné existující zásady zálohování, které jsou přidružené k serveru pomocí [odebrat OBPolicy](https://technet.microsoft.com/library/hh770415) rutiny. Odebrání zásady zobrazí výzvu k potvrzení. Přeskočit použití potvrzení ```-Confirm:$false``` příznak pomocí rutiny.

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want to remove this backup policy? This will delete all the backed up data. [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
```

Potvrzení objektu zásad se provádí pomocí [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) rutiny. Také to požádá o potvrzení. Přeskočit použití potvrzení ```-Confirm:$false``` příznak pomocí rutiny.

```
PS C:> Set-OBPolicy -Policy $newpolicy
Microsoft Azure Backup Do you want to save this backup policy ? [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s)
DsList : {DataSource
         DatasourceId:4508156004108672185
         Name:C:\
         FileSpec:FileSpec
         FileSpec:C:\
         IsExclude:False
         IsRecursive:True,

         FileSpec
         FileSpec:C:\windows
         IsExclude:True
         IsRecursive:True,

         FileSpec
         FileSpec:C:\temp
         IsExclude:True
         IsRecursive:True,

         DataSource
         DatasourceId:4508156005178868542
         Name:D:\
         FileSpec:FileSpec
         FileSpec:D:\
         IsExclude:False
         IsRecursive:True
    }
PolicyName : c2eb6568-8a06-49f4-a20e-3019ae411bac
RetentionPolicy : Retention Days : 7
              WeeklyLTRSchedule :
              Weekly schedule is not set

              MonthlyLTRSchedule :
              Monthly schedule is not set

              YearlyLTRSchedule :
              Yearly schedule is not set
State : Existing PolicyState : Valid
```

Můžete zobrazit podrobnosti o existující zásady zálohování pomocí [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) rutiny. Vám může procházení další pomocí [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) rutina pro plán zálohování a [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) rutina pro zásady uchovávání informací

```
PS C:> Get-OBPolicy | Get-OBSchedule
SchedulePolicyName : 71944081-9950-4f7e-841d-32f0a0a1359a
ScheduleRunDays : {Saturday, Sunday}
ScheduleRunTimes : {16:00:00}
State : Existing

PS C:> Get-OBPolicy | Get-OBRetentionPolicy
RetentionDays : 7
RetentionPolicyName : ca3574ec-8331-46fd-a605-c01743a5265e
State : Existing

PS C:> Get-OBPolicy | Get-OBFileSpec
FileName : *
FilePath : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
FileSpec : D:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\
FileSpec : C:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\windows
FileSpec : C:\windows
IsExclude : True
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\temp
FileSpec : C:\temp
IsExclude : True
IsRecursive : True
```

### <a name="performing-an-ad-hoc-backup"></a>Provádění zálohu ad-hoc
Jakmile byla nastavena zásada zálohování dojde k zálohování podle plánu. Aktivuje zálohu ad-hoc je také možné pomocí [Start-OBBackup](https://technet.microsoft.com/library/hh770426) rutiny:

```
PS C:> Get-OBPolicy | Start-OBBackup
Taking snapshot of volumes...
Preparing storage...
Estimating size of backup items...
Estimating size of backup items...
Transferring data...
Verifying backup...
Job completed.
The backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a>Obnovení dat z Azure Backup
Tato část vás provede kroky pro automatizaci obnovení dat z Azure Backup. Díky tomu zahrnuje následující kroky:

1. Vyberte zdrojový svazek
2. Vyberte bod obnovení zálohy
3. Vyberte položku, kterou chcete obnovit
4. Aktivační událost v procesu obnovení

### <a name="picking-the-source-volume"></a>Výběr zdrojovém svazku
Chcete-li obnovit položky z Azure Backup, musíte nejprve k identifikaci zdroje položky. Vzhledem k tomu, že jsme se provádění příkazů v kontextu systému Windows Server nebo klienta Windows, počítač je už definovaný. Dalším krokem při identifikaci zdroj je k identifikaci svazku, který ji obsahuje. Seznam svazků nebo zdrojů zálohovaných z tohoto počítače se dá načíst spuštěním [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) rutiny. Tento příkaz vrátí pole všech zdrojů zálohovat z tohoto serveru nebo klienta.

```
PS C:> $source = Get-OBRecoverableSource
PS C:> $source
FriendlyName : C:\
RecoverySourceName : C:\
ServerName : myserver.microsoft.com

FriendlyName : D:\
RecoverySourceName : D:\
ServerName : myserver.microsoft.com
```

### <a name="choosing-a-backup-point-to-restore"></a>Výběr bodu zálohy obnovit
Seznam body zálohy můžete načíst tak, že spustíte [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) rutiny s příslušnými parametry. V našem příkladu jsme vybrali nejnovější bod zálohy pro zdrojový svazek *D:* a použít ho k obnovení konkrétní soubor.

```
PS C:> $rps = Get-OBRecoverableItem -Source $source[1]
IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :

IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 17-Jun-15 6:31:31 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :
```
Objekt ```$rps``` je pole body zálohy. První prvek je poslední bod a n-tou elementu bude nejstarší bod. Zvolte nejnovější bod, použijeme ```$rps[0]```.

### <a name="choosing-an-item-to-restore"></a>Vybrat položku, kterou chcete obnovit
K identifikaci přesný soubor nebo složku pro obnovení, použijte rekurzivně [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) rutiny. Tímto způsobem lze procházet hierarchii složek výhradně pomocí ```Get-OBRecoverableItem```.

V tomto příkladu, pokud chcete obnovit soubor *finances.xls* jsme může odkazovat, který používá objekt ```$filesFolders[1]```.

```
PS C:> $filesFolders = Get-OBRecoverableItem $rps[0]
PS C:> $filesFolders
IsDir : True
ItemNameFriendly : D:\MyData\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\
LocalMountPoint : D:\
MountPointName : D:\
Name : MyData
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime : 15-Jun-15 8:49:29 AM

PS C:> $filesFolders = Get-OBRecoverableItem $filesFolders[0]
PS C:> $filesFolders
IsDir : False
ItemNameFriendly : D:\MyData\screenshot.oxps
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\screenshot.oxps
LocalMountPoint : D:\
MountPointName : D:\
Name : screenshot.oxps
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 228313
ItemLastModifiedTime : 21-Jun-14 6:45:09 AM

IsDir : False
ItemNameFriendly : D:\MyData\finances.xls
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\finances.xls
LocalMountPoint : D:\
MountPointName : D:\
Name : finances.xls
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 96256
ItemLastModifiedTime : 21-Jun-14 6:43:02 AM
```

Můžete také vyhledat položky k obnovení pomocí ```Get-OBRecoverableItem``` rutiny. V našem příkladu, k vyhledání *finances.xls* nám může získat popisovač v souboru tak, že spustíte tento příkaz:

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-the-restore-process"></a>Spuštění procesu obnovení
K aktivaci v procesu obnovení, musíme nejprve nastavte možnosti obnovení. To můžete provést pomocí [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) rutiny. Pro tento příklad předpokládejme, že chceme obnovit soubory *C:\temp*. Také Předpokládejme, že chceme přeskočit soubory, které již existují na cílovou složku *C:\temp*. Pokud chcete vytvořit možnost obnovení, použijte následující příkaz:

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

Teď spustit obnovení pomocí [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) příkaz na vybraném ```$item``` z výstupu ```Get-OBRecoverableItem``` rutiny:

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
The recovery operation completed successfully.
```


## <a name="uninstalling-the-azure-backup-agent"></a>Odinstalace agenta Azure Backup
Odinstalace agenta Azure Backup lze provést pomocí následujícího příkazu:

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

Odinstalace binárních souborů agenta z počítače má některé důsledky vzít v úvahu:

* Odebere filtr souborů z počítače a sledování změn je zastavena.
* Všechny informace o zásadách se odebere z počítače, ale informace o zásadách dále uložena ve službě.
* Se odeberou všechny plány zálohování a jsou provedeny žádné další zálohy.

Však data uložená v Azure zůstane a se uchovávají podle nastavení zásad uchovávání informací, které jste. Starší body jsou automaticky stará.

## <a name="remote-management"></a>Vzdálená správa
Veškerá správa, kolem agenta Azure Backup, zásady a zdroje dat lze provést vzdáleně pomocí prostředí PowerShell. Na počítač, který bude spravovat vzdáleně musí být správně připravena.

Ve výchozím nastavení je Služba WinRM nakonfigurován pro ruční spuštění. Typ spuštění musí být nastavený na *automatické* a musí být spuštěna. Pokud chcete ověřit, že Služba WinRM je spuštěná, by měla být hodnota vlastnosti stav *systémem*.

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

Prostředí PowerShell by měl být nakonfigurovaný pro vzdálenou komunikaci.

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up to receive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

Tento počítač teď můžete spravovat vzdáleně – od instalace agenta. Například následující skript zkopíruje agenta ke vzdálenému počítači a ho nainstaluje.

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a>Další kroky
Další informace o Azure Backup pro Windows Server nebo klienta najdete v tématu

* [Seznámení s Azure Backup](backup-introduction-to-azure-backup.md)
* [Zálohování serverů Windows](backup-configure-vault.md)
