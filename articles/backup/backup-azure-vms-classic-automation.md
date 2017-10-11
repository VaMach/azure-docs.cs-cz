---
title: "Nasadit a spravovat zálohy pro virtuální počítače Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak nasadit a spravovat Azure Backup pomocí prostředí PowerShell."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 2e24b1d9-4375-4049-a28d-e3bc01152f32
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/2/2017
ms.author: markgal;trinadhk;jimpark
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5f0f06adb8177ce2d17aa0b40666470279c04e22
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-azurermbackup-cmdlets-to-back-up-virtual-machines"></a>Pomocí rutin AzureRM.Backup zálohování virtuálních počítačů
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-vms-automation.md)
> * [Classic](backup-azure-vms-classic-automation.md)
>
>

Tento článek ukazuje, jak pomocí prostředí Azure PowerShell pro zálohování a obnovení virtuálních počítačů Azure. Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: Resource Manager a Classic. Tento článek se zabývá pomocí modelu nasazení Classic k zálohování dat do úložiště záloh. Pokud jste dosud nevytvořili úložiště záloh ve vašem předplatném, najdete v části správce prostředků verzi tohoto článku, [AzureRM.RecoveryServices.Backup použití rutiny pro zálohování virtuálních počítačů](backup-azure-vms-automation.md). Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.

> [!IMPORTANT]
> Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services. Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.<br/> Od 15. října 2017 nebude možné pomocí PowerShellu vytvářet trezory služby Backup. **Do 1. listopadu 2017:**
>- Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.
>- Nebudete mít přístup k datům záloh na portálu Classic. Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.
>

## <a name="concepts"></a>Koncepty
Tento článek obsahuje informace specifické pro rutiny prostředí PowerShell použít k zálohování virtuálních počítačů. Úvodní informace o ochraně virtuálních počítačích Azure, najdete v tématu [plánování vaší infrastruktury zálohování virtuálních počítačů v Azure](backup-azure-vms-introduction.md).

> [!NOTE]
> Než začnete, přečtěte si [požadavky](backup-azure-vms-prepare.md) potřebné pro práci s Azure Backup a [omezení](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm) aktuální virtuálního počítače řešení pro zálohování.
>
>

Pomocí prostředí PowerShell efektivně, pozorně pochopit hierarchii objektů a odkud spustit.

![Hierarchie objektů](./media/backup-azure-vms-classic-automation/object-hierarchy.png)

Dva nejdůležitější toky jsou povolení ochrany pro virtuální počítač a obnovení dat z bodu obnovení. Cílem tohoto článku je se adept na práci s rutinami prostředí PowerShell, aby tyto dva scénáře lépe.

## <a name="setup-and-registration"></a>Instalace a registrace
Chcete-li začít:

1. [Stáhněte si nejnovější PowerShell](https://github.com/Azure/azure-powershell/releases) (minimální požadovaná verze je: 1.0.0)
2. Rutiny prostředí PowerShell zálohování Azure k dispozici najděte tak, že zadáte následující příkaz:

```
PS C:\> Get-Command *azurermbackup*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Backup-AzureRmBackupItem                           1.0.1      AzureRM.Backup
Cmdlet          Disable-AzureRmBackupProtection                    1.0.1      AzureRM.Backup
Cmdlet          Enable-AzureRmBackupContainerReregistration        1.0.1      AzureRM.Backup
Cmdlet          Enable-AzureRmBackupProtection                     1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupContainer                         1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupItem                              1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupJob                               1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupJobDetails                        1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupRecoveryPoint                     1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupVaultCredentials                  1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupRetentionPolicyObject             1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Register-AzureRmBackupContainer                    1.0.1      AzureRM.Backup
Cmdlet          Remove-AzureRmBackupProtectionPolicy               1.0.1      AzureRM.Backup
Cmdlet          Remove-AzureRmBackupVault                          1.0.1      AzureRM.Backup
Cmdlet          Restore-AzureRmBackupItem                          1.0.1      AzureRM.Backup
Cmdlet          Set-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          Set-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Stop-AzureRmBackupJob                              1.0.1      AzureRM.Backup
Cmdlet          Unregister-AzureRmBackupContainer                  1.0.1      AzureRM.Backup
Cmdlet          Wait-AzureRmBackupJob                              1.0.1      AzureRM.Backup
```

Následující instalaci a registraci úlohy je možné automatizovat pomocí prostředí PowerShell:

* Vytvoření trezoru záloh
* Registrace virtuálních počítačů pomocí služby Azure Backup

### <a name="create-a-backup-vault"></a>Vytvoření trezoru záloh
> [!WARNING]
> Pro zákazníky pomocí služby Azure Backup poprvé budete muset registraci poskytovatele Azure Backup pro použití s vaším předplatným. To můžete provést spuštěním následujícího příkazu: Register-AzureRmResourceProvider - ProviderNamespace "Microsoft.Backup"
>
>

Můžete vytvořit nové úložiště záloh pomocí **New-AzureRmBackupVault** rutiny. Trezor záloh je prostředek ARM, proto musíte umístit do skupiny prostředků. V prostředí Azure PowerShell konzolu se zvýšenými oprávněními spusťte následující příkazy:

```
PS C:\> New-AzureRmResourceGroup –Name “test-rg” –Location “West US”
PS C:\> $backupvault = New-AzureRmBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

Můžete získat seznam všech záloh v daném předplatném pomocí **Get-AzureRmBackupVault** rutiny.

> [!NOTE]
> Je vhodné pro uložení objektu úložiště záloh do proměnné. Objekt trezoru je potřeba jako vstup pro mnoho rutin Azure Backup.
>
>

### <a name="registering-the-vms"></a>Registrace virtuálních počítačů
Prvním krokem ke konfiguraci zálohování Azure Backup je při registraci počítače nebo virtuální počítač s trezoru služby Azure Backup. **Register-AzureRmBackupContainer** rutina zpracovává vstupní informace virtuálního počítače Azure IaaS a zaregistruje ho se zadanou trezoru. Operaci registrace přidruží virtuální počítač Azure úložiště záloh a sleduje virtuálního počítače pomocí zálohování životního cyklu.

Registrace virtuálního počítače pomocí služby Azure Backup vytvoří objekt kontejner nejvyšší úrovně. Kontejner obvykle obsahuje více položek, které lze zálohovat, ale v případě virtuálních počítačů bude jen jednu položku Zálohování kontejneru.

```
PS C:\> $registerjob = Register-AzureRmBackupContainer -Vault $backupvault -Name "testvm" -ServiceName "testvm"
```

## <a name="backup-azure-vms"></a>Zálohování virtuálních počítačů Azure
### <a name="create-a-protection-policy"></a>Vytvoření zásady ochrany
Není to povinné pro vytvoření nové zásady ochrany spustit zálohování virtuálních počítačů. Úložiště se dodává s 'výchozí zásadu", můžete použít k rychlému povolení ochrany a potom je upravit později pomocí podrobností vpravo. Seznam zásad, které jsou k dispozici v trezoru můžete získat pomocí **Get-AzureRmBackupProtectionPolicy** rutiny:

```
PS C:\> Get-AzureRmBackupProtectionPolicy -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DefaultPolicy             AzureVM            Daily              26-Aug-15 12:30:00 AM
```

> [!NOTE]
> Časové pásmo BackupTime pole v prostředí PowerShell je UTC. Ale pokud čas zálohování se zobrazí na portálu Azure, časové pásmo je zarovnán do místního systému, společně s posunem od standardu UTC.
>
>

Zásady zálohování je přidružen alespoň jeden zásady uchovávání informací. Zásady uchovávání informací definuje, jak dlouho se uchovává bodu obnovení se Azure Backup. **New-AzureRmBackupRetentionPolicy** rutina vytvoří prostředí PowerShell objekty, které obsahují informace o zásadách uchovávání informací. Tyto objekty zásad uchovávání dat se používají jako vstupy pro *New-AzureRmBackupProtectionPolicy* rutiny, nebo přímo pomocí *povolit AzureRmBackupProtection* rutiny.

Zásady zálohování definují, kdy a jak často se provádí zálohování položky. **New-AzureRmBackupProtectionPolicy** rutina vytvoří objekt prostředí PowerShell, který obsahuje informace zásady zálohování. Zásady zálohování se používá jako vstup *povolit AzureRmBackupProtection* rutiny.

```
PS C:\> $Daily = New-AzureRmBackupRetentionPolicyObject -DailyRetention -Retention 30
PS C:\> $newpolicy = New-AzureRmBackupProtectionPolicy -Name DailyBackup01 -Type AzureVM -Daily -BackupTime ([datetime]"3:30 PM") -RetentionPolicy $Daily -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DailyBackup01             AzureVM            Daily              01-Sep-15 3:30:00 PM
```

### <a name="enable-protection"></a>Povolení ochrany
Povolení ochrany zahrnuje dva objekty - položky a zásady, a obě musí patřit ke stejnému trezoru. Jakmile zásady je už přidružená k položce, bude zálohování pracovního postupu nové podle definovaného plánu.

```
PS C:\> Get-AzureRmBackupContainer -Type AzureVM -Status Registered -Vault $backupvault | Get-AzureRmBackupItem | Enable-AzureRmBackupProtection -Policy $newpolicy
```

### <a name="initial-backup"></a>Prvotní zálohování
Plán zálohování se postará o provádění a úplné počáteční kopie pro položku přírůstkové kopie pro následné zálohy. Ale pokud chcete vynutit prvotní zálohování k dojít v určitém čase nebo i okamžitě použijte **zálohování AzureRmBackupItem** rutiny:

```
PS C:\> $container = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -Name "testvm"
PS C:\> $backupjob = Get-AzureRmBackupItem -Container $container | Backup-AzureRmBackupItem
PS C:\> $backupjob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

> [!NOTE]
> Časové pásmo čas spuštění a čas ukončení pole zobrazená v prostředí PowerShell je čas UTC. Ale když podobné informace se zobrazí na portálu Azure, časové pásmo je zarovnán do místních systémových hodin.
>
>

### <a name="monitoring-a-backup-job"></a>Monitorování úlohy zálohování
Většina dlouho běžící operace ve službě Azure Backup se modelována jako úlohu. To umožňuje snadno sledovat průběh, aniž by bylo nutné zachovat portálu Azure otevřete za všech okolností.

Chcete-li získat nejnovější stav probíhající úlohy, použijte **Get-AzureRmBackupJob** rutiny.

```
PS C:\> $joblist = Get-AzureRmBackupJob -Vault $backupvault -Status InProgress
PS C:\> $joblist[0]

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

Místo dotazování tyto úlohy pro dokončení – což je zbytečné, další kód – je jednodušší použít **čekání AzureRmBackupJob** rutiny. Při použití ve skriptu, bude rutina pozastavit provádění až do dokončení úlohy nebo zadaný časový limit.

```
PS C:\> Wait-AzureRmBackupJob -Job $joblist[0] -Timeout 43200
```


## <a name="restore-an-azure-vm"></a>Obnovení virtuálního počítače Azure
Chcete-li obnovit zálohovaná data, musíte určit položku zálohovanou a bod obnovení, který obsahuje data v daném okamžiku. Tyto informace jsou poskytovány pro rutinu obnovení AzureRmBackupItem zahájíte obnovení dat z trezoru s účtem zákazníka.

### <a name="select-the-vm"></a>Vyberte virtuální počítač
GET pro objekt prostředí PowerShell, který označuje správné záložní položku, musíte spustit z kontejneru v trezoru a směrem dolů hierarchie objektů. Vyberte kontejner, který představuje virtuální počítač, použijte **Get-AzureRmBackupContainer** rutiny a aby zřetězit **Get-AzureRmBackupItem** rutiny.

```
PS C:\> $backupitem = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -name "testvm" | Get-AzureRmBackupItem
```

### <a name="choose-a-recovery-point"></a>Vyberte bod obnovení
Nyní můžete vytvořit seznam všech bodů obnovení pro zálohování položek pomocí **Get-AzureRmBackupRecoveryPoint** rutiny a vyberte bod obnovení pro obnovení. Obvykle vyberte nejnovější uživatelé *AppConsistent* bodu v seznamu.

```
PS C:\> $rp =  Get-AzureRmBackupRecoveryPoint -Item $backupitem
PS C:\> $rp

RecoveryPointId    RecoveryPointType  RecoveryPointTime      ContainerName
---------------    -----------------  -----------------      -------------
15273496567119     AppConsistent      01-Sep-15 12:27:38 PM  iaasvmcontainer;testvm;testv...
```

Proměnná ```$rp``` je pole bodů obnovení pro vybranou zálohu položky, seřazeny v opačném pořadí času – nejnovější bod obnovení je na pozici 0. Použijte standardní prostředí PowerShell pole indexování a vyberte bod obnovení. Příklad: ```$rp[0]``` vybere nejnovější bod obnovení.

### <a name="restoring-disks"></a>Obnovování disků
Je klíčové rozdíl mezi operace obnovení provádí prostřednictvím portálu Azure a pomocí prostředí Azure PowerShell. Pomocí prostředí PowerShell operace obnovení zastaví obnovování disky a konfigurační informace z bodu obnovení. Virtuální počítač se nevytvoří.

> [!WARNING]
> Obnovení-AzureRmBackupItem nedojde k vytvoření virtuálního počítače. Obnoví pouze disky pro zadaný účet úložiště. Nejedná se o stejné chování, které si všimnete na portálu Azure.
>
>

```
PS C:\> $restorejob = Restore-AzureRmBackupItem -StorageAccountName "DestAccount" -RecoveryPoint $rp[0]
PS C:\> $restorejob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Restore         InProgress      01-Sep-15 1:14:01 PM   01-Jan-01 12:00:00 AM
```

Můžete získat podrobnosti o použití operaci obnovení **Get-AzureRmBackupJobDetails** rutiny po dokončení úlohy obnovení. *Detaily chyby* vlastnost bude mít informace potřebné pro virtuální počítač znovu sestavit.

```
PS C:\> $restorejob = Get-AzureRmBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmBackupJobDetails -Job $restorejob
```

### <a name="build-the-vm"></a>Vytvoření virtuálního počítače
Vytváření virtuálních počítačů mimo obnovené disky lze provést pomocí starší rutin Azure Service Management PowerShell nové šablony Azure Resource Manager, nebo i pomocí portálu Azure. V rychlé příkladu ukážeme, jak získat pomocí rutiny Azure Service Management došlo.

```
$properties  = $details.Properties

$storageAccountName = $properties["Target Storage Account Name"]
$containerName = $properties["Config Blob Container Name"]
$blobName = $properties["Config Blob Name"]

$keys = Get-AzureStorageKey -StorageAccountName $storageAccountName
$storageAccountKey = $keys.Primary
$storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey


$destination_path = "C:\Users\admin\Desktop\vmconfig.xml"
Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path -Context $storageContext


$obj = [xml](((Get-Content -Path $destination_path -Encoding UniCode)).TrimEnd([char]0x00))
$pvr = $obj.PersistentVMRole
$os = $pvr.OSVirtualHardDisk
$dds = $pvr.DataVirtualHardDisks
$osDisk = Add-AzureDisk -MediaLocation $os.MediaLink -OS $os.OS -DiskName "panbhaosdisk"
$vm = New-AzureVMConfig -Name $pvr.RoleName -InstanceSize $pvr.RoleSize -DiskName $osDisk.DiskName

if (!($dds -eq $null))
{
    foreach($d in $dds.DataVirtualHardDisk)
    {
        $lun = 0
        if(!($d.Lun -eq $null))
        {
            $lun = $d.Lun
        }
        $name = "panbhadataDisk" + $lun
        Add-AzureDisk -DiskName $name -MediaLocation $d.MediaLink
        $vm | Add-AzureDataDisk -Import -DiskName $name -LUN $lun
    }
}

New-AzureVM -ServiceName "panbhasample" -Location "SouthEast Asia" -VM $vm
```

Další informace o tom, jak vytvořit virtuální počítač z obnovené disků přečtěte si o následující rutiny:

* [Přidat AzureDisk](https://msdn.microsoft.com/library/azure/dn495252.aspx)
* [Nové AzureVMConfig](https://msdn.microsoft.com/library/azure/dn495159.aspx)
* [Nový AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)

## <a name="code-samples"></a>Ukázky kódů
### <a name="1-get-the-completion-status-of-job-sub-tasks"></a>1. Získat stav dokončení dílčí úlohy
Chcete-li sledovat stav dokončení jednotlivé dílčí úkoly, můžete použít **Get-AzureRmBackupJobDetails** rutiny:

```
PS C:\> $details = Get-AzureRmBackupJobDetails -JobId $backupjob.InstanceId -Vault $backupvault
PS C:\> $details.SubTasks

Name                                                        Status
----                                                        ------
Take Snapshot                                               Completed
Transfer data to Backup vault                               InProgress
```

### <a name="2-create-a-dailyweekly-report-of-backup-jobs"></a>2. Vytvoření sestavy denně nebo týdně úloh zálohování
Správci obvykle chtít vědět, úlohy zálohování, která byla spuštěna v posledních 24 hodin, stav tyto úlohy zálohování. Kromě toho množství dat přenesených dává správcům možnost odhadnout měsíční využití dat. Níže uvedeném skriptu získává nezpracovaná data ze služby Azure Backup a zobrazí informace v konzole PowerShell.

```
param(  [Parameter(Mandatory=$True,Position=1)]
        [string]$backupvaultname,

        [Parameter(Mandatory=$False,Position=2)]
        [int]$numberofdays = 7)


#Initialize variables
$DAILYBACKUPSTATS = @()
$backupvault = Get-AzureRmBackupVault -Name $backupvaultname
$enddate = ([datetime]::Today).AddDays(1)
$startdate = ([datetime]::Today)

for( $i = 1; $i -le $numberofdays; $i++ )
{
    # We query one day at a time because pulling 7 days of data might be too much
    $dailyjoblist = Get-AzureRmBackupJob -Vault $backupvault -From $startdate -To $enddate -Type AzureVM -Operation Backup
    Write-Progress -Activity "Getting job information for the last $numberofdays days" -Status "Day -$i" -PercentComplete ([int]([decimal]$i*100/$numberofdays))

    foreach( $job in $dailyjoblist )
    {
        #Extract the information for the reports
        $newstatsobj = New-Object System.Object
        $newstatsobj | Add-Member -Type NoteProperty -Name Date -Value $startdate
        $newstatsobj | Add-Member -Type NoteProperty -Name VMName -Value $job.WorkloadName
        $newstatsobj | Add-Member -Type NoteProperty -Name Duration -Value $job.Duration
        $newstatsobj | Add-Member -Type NoteProperty -Name Status -Value $job.Status

        $details = Get-AzureRmBackupJobDetails -Job $job
        $newstatsobj | Add-Member -Type NoteProperty -Name BackupSize -Value $details.Properties["Backup Size"]
        $DAILYBACKUPSTATS += $newstatsobj
    }

    $enddate = $enddate.AddDays(-1)
    $startdate = $startdate.AddDays(-1)
}

$DAILYBACKUPSTATS | Out-GridView
```

Pokud chcete přidat do této sestavy výstup možností vytváření grafů, přečtěte si z v příspěvku blogu TechNet [grafů, pomocí prostředí PowerShell](http://blogs.technet.com/b/richard_macdonald/archive/2009/04/28/3231887.aspx)

## <a name="next-steps"></a>Další kroky
Pokud dáváte přednost, abyste upoutali vašich prostředků Azure pomocí prostředí PowerShell, podívejte se na článek prostředí PowerShell pro ochranu v systému Windows Server [nasadit a spravovat zálohy pro Windows Server](backup-client-automation-classic.md). Je také článku prostředí PowerShell pro správu zálohování DPM [nasadit a spravovat zálohy pro DPM](backup-dpm-automation-classic.md). Mají obě z těchto článků verze pro nasazení Resource Manager a také nasazení Classic.
