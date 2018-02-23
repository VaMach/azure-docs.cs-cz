---
title: "Replikace virtuálních počítačů technologie Hyper-V v cloudech VMM do sekundární lokality pomocí prostředí PowerShell (Azure Resource Manager) | Microsoft Docs"
description: "Popisuje, jak replikovat virtuální počítače Hyper-V v cloudech VMM do sekundární lokalita VMM pomocí prostředí PowerShell (Resource Manager)"
services: site-recovery
author: sujaytalasila
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: sutalasi
ms.openlocfilehash: a5e36546494223b20844303f3f76782746658411
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="replicate-hyper-v-vms-to-a-secondary-site-using-powershell-resource-manager"></a>Replikace virtuálních počítačů technologie Hyper-V do sekundární lokality pomocí prostředí PowerShell (Resource Manager)

Tento článek ukazuje Automatizace kroků pro replikaci virtuálních počítačů technologie Hyper-V v cloudech System Center Virtual Machine Manager (VMM) do cloudu VMM na sekundární místní lokalitu, pomocí [Azure Site Recovery](site-recovery-overview.md).

## <a name="prerequisites"></a>Požadavky

- Zkontrolujte [scénář architektura a komponenty](hyper-v-vmm-architecture.md).
- Zkontrolujte [požadavky na podporu](site-recovery-support-matrix-to-sec-site.md) pro všechny komponenty.
- Ujistěte se, že servery VMM a hostitelé Hyper-V souladu s [podporu požadavků](site-recovery-support-matrix-to-sec-site.md).
- Zkontrolujte, zda chcete replikovat virtuální počítače v souladu s [replikovat podpora počítačů](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions)


## <a name="prepare-for-network-mapping"></a>Příprava mapování sítě

[Mapování sítě](hyper-v-vmm-network-mapping.md) mapy mezi místní sítí virtuálních počítačů nástroje VMM v cloudech zdroje a cíle. Mapování provede následující akce:

- Virtuální počítače se připojí k příslušné cílové sítě virtuálních počítačů po převzetí služeb při selhání. 
- Optimálně umístí virtuální počítače repliky na cílové servery hostitele technologie Hyper-V. 
- Pokud nenakonfigurujete mapování sítě, virtuální počítače replik nebude připojen k síti virtuálních počítačů po převzetí služeb při selhání.

Příprava VMM následujícím způsobem:

1. Zajistěte, aby byla [VMM logické sítě](https://docs.microsoft.com/system-center/vmm/network-logical) na zdrojové a cílové servery VMM.
    - Logické sítě na zdrojovém serveru by měly být přidružené zdrojový cloud, ve které se nacházejí hostitelů Hyper-V.
    - Logické sítě na cílovém serveru by měly být přidružené cílový cloud.
1. Zajistěte, aby byla [sítě virtuálních počítačů](https://docs.microsoft.com/system-center/vmm/network-virtual) na zdrojové a cílové servery VMM. Sítě virtuálních počítačů musí být propojena na logickou síť v každém umístění.
2. Připojte virtuální počítače na hostitelích technologie Hyper-V zdroj ke zdrojové síti virtuálních počítačů. 

## <a name="prepare-for-powershell"></a>Příprava pro prostředí PowerShell

Ujistěte se, že máte Azure PowerShell připravené na vynucování.

- Pokud už používáte prostředí PowerShell, budete muset upgradovat na verzi 0.8.10 nebo novější.  [Další informace](/powershell/azureps-cmdlets-docs) o nastavení prostředí PowerShell.
- Po nastavení a nakonfigurovat prostředí PowerShell, zkontrolujte [služby rutiny](/powershell/azure/overview).
- Další informace o použití hodnoty parametrů, vstupů a výstupů v prostředí Azure PowerShell, najdete [Začínáme](/powershell/azure/get-started-azureps) průvodce.

## <a name="set-up-a-subscription"></a>Nastavení odběru
1. Z prostředí Azure PowerShell Přihlaste se k účtu Azure:

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. Načtení seznamu předplatné s ID předplatného. Poznamenejte si ID předplatného, ve kterém chcete vytvořit trezor služeb zotavení.   

        Get-AzureRmSubscription
3. Nastavte předplatné trezoru:

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení
1. Pokud nemáte, vytvořte skupinu prostředků Azure Resource Manager.

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Vytvořit nový trezor služeb zotavení a uložte tento trezor objekt v proměnné, která se použije později: 

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location
   
    Po vytvoření, pomocí rutiny Get-AzureRMRecoveryServicesVault můžete načíst objekt úložiště.

## <a name="set-the-vault-context"></a>Nastavit kontext trezoru
1. Načtěte existující trezor:

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Nastavte kontext úložiště:

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="install-the-azure-site-recovery-provider"></a>Nainstalujte zprostředkovatele Azure Site Recovery
1. Na počítači VMM vytvořte adresář tak, že spustíte následující příkaz:

       New-Item c:\ASR -type directory
2. Extrahujte soubory pomocí stažený instalační soubor zprostředkovatele: pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Nainstalujte poskytovatele a počkejte na dokončení instalace:

       .\SetupDr.exe /i
       $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
       do
       {
         $isNotInstalled = $true;
         if(Test-Path $installationRegPath)
         {
           $isNotInstalled = $false;
         }
       }While($isNotInstalled)

4. Zaregistrujte server v trezoru:

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="create-and-associate-a-replication-policy"></a>Vytvořit a přidružit zásady replikace
1. Vytvořte zásadu replikace (v tomto případě 2012 R2 Hyper-V), následujícím způsobem:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod

    > [!NOTE]
    > Cloudu VMM může obsahovat hostitelů Hyper-V s různými verzemi systému Windows Server, ale je zásada replikace pro konkrétní verzi operačního systému. Pokud máte různých hostitelích, které jsou spuštěné v různých operačních systémech, vytvořte zásady samostatné replikace u každého systému. Například pokud máte pět hostitelů, které se spuštěným operačním systémem Windows 2012 serverů a tři na Windows Server 2012 R2, vytvořte dvě zásady replikace – jednu pro každý typ operačního systému.

2. Načtení primární ochranu kontejneru (primární VMM c) a obnovení kontejneru ochrany (obnovení cloudu VMM):

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. Načtěte zásady replikace, kterou jste vytvořili pomocí popisného názvu:

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. Přidružení kontejneru ochrany (VMM Cloud) začněte zásady replikace:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. Počkejte na dokončení úlohy přidružení zásad. Můžete zkontrolovat, pokud byla dokončena úloha pomocí následující fragment kódu prostředí PowerShell:

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

   Po dokončení zpracování úlohy spusťte následující příkaz:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

Pokud chcete zkontrolovat na dokončení operace, postupujte podle kroků v [sledovat činnost](#monitor-activity).

##  <a name="configure-network-mapping"></a>Konfigurace mapování sítě
1. Použijte tento příkaz k načtení servery pro aktuální trezoru. Příkaz ukládá v proměnné pole $Servers servery Azure Site Recovery.

        $Servers = Get-AzureRmSiteRecoveryServer
2. Spusťte tento příkaz retrievet sítí pro zdrojový server VMM a cílovém serveru VMM.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > Zdrojový server VMM, může se jednat o první nebo druhé v poli servery. Zkontrolujte název serveru VMM a správně načíst sítí.


3. Tato rutina vytvoří mapování mezi primární síť a síti pro obnovení. Určuje primární síť jako první prvek $PrimaryNetworks a síti pro obnovení jako první prvek $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]


## <a name="enable-protection-for-vms"></a>Povolit ochranu pro virtuální počítače
Po serverů, cloudů a sítí jsou správně nakonfigurovaný, můžete povolit ochranu virtuálních počítačů v cloudu.

1. Pokud chcete povolit ochranu, spusťte následující příkaz pro načtení kontejner ochrany:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Získáte entita ochrany (VM) následujícím způsobem:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Povolení replikace pro virtuální počítač:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání

Nasazení můžete spustit testovací převzetí služeb při selhání pro jeden virtuální počítač nebo vytvoření plánu obnovení, který obsahuje víc virtuálních počítačů, otestovat a a spusťte testovací převzetí služeb při selhání pro plán. Testovací převzetí služeb při selhání simuluje váš mechanismus převzetí služeb při selhání a zotavení v izolované síti.

1. Získat virtuální počítač, do kterého bude převzít virtuální počítače:

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. Testovací převzetí služeb proveďte takto:
    - Pro jeden virtuální počítač

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult = start AzureRmSiteRecoveryTestFailoverJob-směr PrimaryToRecovery - ProtectionEntity $protectionEntity - VMNetwork $RecoveryNetworks [1]
    
    - Pro plán obnovení:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult = start AzureRmSiteRecoveryTestFailoverJob-směr PrimaryToRecovery - Recoveryplan $recoveryplan - VMNetwork $RecoveryNetworks [1]

Pokud chcete zkontrolovat na dokončení operace, postupujte podle kroků v [sledovat činnost](#monitor-activity).

## <a name="run-planned-and-unplanned-failovers"></a>Spusťte plánované a neplánované převzetí služeb při selhání

1. Proveďte plánované převzetí služeb při selhání následujícím způsobem:
    -  Pro jeden virtuální počítač:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult = start AzureRmSiteRecoveryPlannedFailoverJob-směr PrimaryToRecovery - ProtectionEntity $protectionEntity
    - Pro plán obnovení

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult = start AzureRmSiteRecoveryPlannedFailoverJob-směr PrimaryToRecovery - Recoveryplan $recoveryplan
2. Neplánované převzetí služeb při selhání proveďte takto:
    - Pro jeden virtuální počítač
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult = start AzureRmSiteRecoveryUnPlannedFailoverJob-směr PrimaryToRecovery - ProtectionEntity $protectionEntity

    - Pro plán obnovení:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult = start AzureRmSiteRecoveryUnPlannedFailoverJob-směr PrimaryToRecovery - ProtectionEntity $protectionEntity

## <a name="monitor-activity"></a>Sledování aktivity
Monitorování failver použijte následující příkazy. Všimněte si, že máte čekat mezi úlohy pro zpracování ukončíte.

    Do
    {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

    if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Další postup

[Další informace](/powershell/module/azurerm.recoveryservices.backup/#recovery) o Site Recovery pomocí rutin prostředí PowerShell Azure Resource Manager.
