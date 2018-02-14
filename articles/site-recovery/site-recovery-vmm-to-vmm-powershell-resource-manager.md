---
title: "Replikace virtuálních počítačů technologie Hyper-V v nástroji VMM do sekundární lokality pomocí prostředí PowerShell (Azure Resource Manager) | Microsoft Docs"
description: "Popisuje, jak nasadit Azure Site Recovery k orchestraci replikace, převzetí služeb při selhání a obnovení virtuálních počítačů Hyper-V v cloudech VMM do sekundární lokalita VMM pomocí prostředí PowerShell (Resource Manager)"
services: site-recovery
documentationcenter: 
author: sujaytalasila
manager: rochakm
editor: raynew
ms.assetid: 9d38e9c3-217c-4e44-830c-575e9a4141f2
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: sutalasi
ms.openlocfilehash: c978c2e31e775f56824d765491f6d7b73648b8ae
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a>Replikace virtuálních počítačů technologie Hyper-V v cloudech VMM do sekundární lokalita VMM pomocí prostředí PowerShell (Resource Manager)

Tento článek ukazuje, jak pomocí prostředí PowerShell pro automatizaci běžných úkolů při nastavení Azure Site Recovery replikovat virtuální počítače Hyper-V v cloudech System Center VMM cloudech System Center VMM v sekundární lokalitě.



## <a name="on-premises-prerequisites"></a>Místní požadavky
Tady je co budete muset v lokalitách primární a sekundární místní nasazení tohoto scénáře:

| **Požadavky** | **Podrobnosti** |
| --- | --- |
| **VMM** |Doporučujeme že nasadit server VMM v primární lokalitě a server VMM v sekundární lokalitě.<br/><br/> Také můžete replikovat mezi cloudy na jednom serveru VMM. K tomu budete potřebovat aspoň dva cloudy, které jsou nakonfigurované na serveru VMM.<br/><br/> Servery VMM by měl používat minimálně System Center 2012 SP1 s nejnovějšími aktualizacemi.<br/><br/> Každý server VMM musí mít na jeden nebo více cloudy nakonfigurované a všechny cloudy musí mít sadu profilů kapacity technologie Hyper-V. <br/><br/>Cloudy musí obsahovat jeden nebo více skupin hostitelů VMM. Servery VMM by měl mít přístup k Internetu. |
| **Hyper-V** |Servery Hyper-V musí běžet minimálně Windows Server 2012 s rolí Hyper-V a mají nainstalované nejnovější aktualizace.<br/><br/> Server Hyper-V by měl obsahovat jeden nebo více virtuálních počítačů.<br/><br/>  Hostitelské servery Hyper-V by měl být umístěn v skupiny hostitelů v primárních a sekundárních cloudech VMM.<br/><br/> Pokud používáte technologii Hyper-V v clusteru na Windows Server 2012 R2 nainstalujte [aktualizovat 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Pokud používáte technologii Hyper-V v clusteru na Windows Server 2012 Poznámka že zprostředkovatel clusteru se nevytvoří automaticky, pokud máte statické IP adresy na základě clusteru. Budete ho muset nakonfigurovat ručně. [Přečtěte si další informace](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx). |
| **Poskytovatel** |Během nasazování Site Recovery nainstalujete zprostředkovatele Azure Site Recovery na serverech VMM. Zprostředkovatel komunikováním se službou Site Recovery přes protokol HTTPS 443 pro orchestraci replikace. Dojde k replikaci dat mezi primární a sekundární servery technologie Hyper-V přes síť LAN nebo připojení k síti VPN.<br/><br/> Zprostředkovatel, který běží na serveru VMM potřebuje přístup k tyto adresy URL: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.windows.net; *. backup.windowsazure.com; *. blob.core.windows.net; *. store.core.windows.net.<br/><br/> Kromě toho povolit bránu firewall komunikaci ze serverů VMM pro [rozsahy IP adres Azure datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) a povolit protokol HTTPS (443). |

### <a name="network-mapping-prerequisites"></a>Požadavky na mapování sítě
Mapování mapuje sítě mezi sítěmi virtuálních počítačů nástroje VMM na primární a sekundární servery VMM proveďte následující:

* Optimálně umístíte virtuální počítače repliky na sekundární hostitelů Hyper-V po převzetí služeb při selhání.
* Virtuální počítače repliky se připojte k příslušné sítě virtuálních počítačů.
* Pokud nenakonfigurujete sítě mapování repliky virtuálních počítačů se nepřipojí k žádné síti po převzetí služeb při selhání.
* Pokud chcete nastavit síť mapování během obnovení lokality v tomto poli je nasazení co budete potřebovat:

  * Zajistěte, aby virtuální počítače na zdrojovém hostitelském serveru Hyper-V byly připojené k síti virtuálních počítačů ve VMM. Tato síť musí být propojená na logickou síť, která je přidružená ke cloudu.
  * Ověřte, že sekundární cloudu, které budete používat pro obnovení má odpovídající síť virtuálních počítačů konfigurovanou. Tato síť virtuálních počítačů musí být propojena na logickou síť, která je přidružená sekundární cloudu.

Další informace o konfiguraci sítě VMM v následující články

* [Postup konfigurace logické sítě v nástroji VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [Postup konfigurace sítě virtuálních počítačů a bran v nástroji VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)


### <a name="powershell-prerequisites"></a>Požadavky prostředí PowerShell
Ujistěte se, že máte Azure PowerShell připravené na vynucování. Pokud už používáte prostředí PowerShell, budete muset upgradovat na verzi 0.8.10 nebo novější. Informace o nastavení prostředí PowerShell najdete v tématu [Průvodce instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs). Po nastavení a nakonfigurovat prostředí PowerShell, můžete zobrazit všechny dostupné rutiny služby [zde](/powershell/azure/overview).

Další informace o tipy, které mohou pomoci pomocí rutin, jako je například jak hodnoty parametrů, vstupy a výstupy jsou obvykle zpracovávány v prostředí Azure PowerShell najdete v článku [Průvodce Začínáme s Azure rutiny](/powershell/azure/get-started-azureps).

## <a name="step-1-set-the-subscription"></a>Krok 1: Nastavte předplatné
1. Z prostředí Azure powershell, přihlášení k účtu Azure: pomocí následujících rutin

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. Získejte seznam předplatných. SubscriptionIDs to taky zobrazí seznam všech odběrů. Poznamenejte si ID předplatného předplatné, ve kterém chcete vytvořit trezor služeb zotavení    

        Get-AzureRmSubscription
3. Nastavte předplatné, ve kterém má být vytvořen třeba uvést ID předplatného trezoru služeb zotavení

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="step-2-create-a-recovery-services-vault"></a>Krok 2 – Vytvoření trezoru Služeb zotavení.
1. Vytvořte skupinu prostředků Azure Resource Manager, pokud již nemáte

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Vytvořit nový trezor služeb zotavení a uložte objekt trezoru služeb zotavení v proměnné (použijí později). Můžete také načíst vytvoření trezoru objekt post pomocí rutiny Get-AzureRMRecoveryServicesVault:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a>Krok 3: Nastavte kontext trezoru služeb zotavení
1. Pokud máte již vytvořili trezor, spusťte níže získat trezoru.

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Nastavit kontext trezoru spuštěním následující příkaz.

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Krok 4: Instalace nástroje Azure Site Recovery Provider
1. Na počítači VMM vytvořte adresář tak, že spustíte následující příkaz:

       New-Item c:\ASR -type directory
2. Extrahujte soubory pomocí stažené poskytovatele tak, že spustíte následující příkaz

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Nainstalujte poskytovatele pomocí následujících příkazů:

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

   Počkejte na dokončení instalace.
4. Zaregistrujte server v trezoru, pomocí následujícího příkazu:

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="step-5-create-and-associate-a-replication-policy"></a>Krok 5: Vytvořte a přidružte zásadu replikace
1. Vytvořte zásadu replikace 2012 R2 Hyper-V tak, že spustíte následující příkaz:

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
    > Cloudu VMM může obsahovat hostitelů Hyper-V s různými verzemi systému Windows Server (jak je uvedeno v požadavky technologie Hyper-V), ale zásady replikace je konkrétní verze operačního systému. Pokud máte různých hostitelích, které jsou spuštěné v různých verzí operačních systémů, vytvořte samostatné replikace zásady pro každý typ verze operačního systému. Například pokud máte pět hostitelů, které se spuštěným operačním systémem Windows 2012 serverů a tři na Windows Server 2012 R2, vytvořte dvě zásady replikace – jednu pro každý typ verze operačního systému.

1. Získáte primární ochranu kontejneru (primární Cloud VMM) a obnovení kontejneru ochrany (obnovení cloudu VMM) tak, že spustíte následující příkazy:

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. Načtení na zásadu, kterou jste vytvořili v kroku 1 pomocí popisný název zásady

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. Přidružení kontejneru ochrany (VMM Cloud) začněte zásady replikace:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. Počkejte na dokončení úlohy přidružení zásad. Můžete se podívat, pokud byla dokončena úloha pomocí následující fragment kódu prostředí PowerShell.

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

Pokud chcete zkontrolovat na dokončení operace, postupujte podle kroků v [monitorování aktivity](#monitor).

## <a name="step-6-configure-network-mapping"></a>Krok 6: Konfigurace mapování sítě
1. První příkaz získá servery pro aktuální trezoru Azure Site Recovery. Příkaz ukládá v proměnné pole $Servers servery Microsoft Azure Site Recovery.

        $Servers = Get-AzureRmSiteRecoveryServer
2. Následující příkaz získat sítě Site Recovery pro server VMM zdrojovém a cílovém serveru VMM.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > Zdrojový server VMM může být první nebo druhý v poli servery. Zkontrolujte názvy serverů VMM a získejte sítě správně


1. Poslední rutina vytvoří mapování mezi primární síť a síti pro obnovení. Rutina určuje primární síť jako první prvek $PrimaryNetworks a obnovení sítě jako první prvek $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-7-configure-storage-mapping"></a>Krok 7: Konfigurace mapování úložiště
1. Níže příkaz získá seznam klasifikací úložiště do $storageclassifications proměnné.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification
2. Klasifikace zdroje do proměnné $SourceClassificaion a cíl klasifikace do proměnné $TargetClassification získat následující příkazy.

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    > [!NOTE]
    > Klasifikace zdrojem a cílem může být libovolný prvek v poli. Odkazovat na výstup níže příkazu obrázek indexu zdrojové a cílové klasifikace v poli $storageclassifications.

    > Get-AzureRmSiteRecoveryStorageClassification | Select-Object - vlastnost FriendlyName, Id | Format-Table


1. Následující rutina vytvoří mapování mezi klasifikace zdroj a cíl klasifikace.

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-8-enable-protection-for-virtual-machines"></a>Krok 8: Povolení ochrany pro virtuální počítače
Po serverů, cloudů a sítí jsou správně nakonfigurovaný, můžete povolit ochranu pro virtuální počítače v cloudu.

1. Pokud chcete povolit ochranu, spusťte následující příkaz k získání kontejneru ochrany:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Získáte entita ochrany (VM) tak, že spustíte následující příkaz:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Povolení replikace pro virtuální počítač tak, že spustíte následující příkaz:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="test-your-deployment"></a>Otestujte nasazení
Chcete nasazení otestovat můžete spustit testovací převzetí služeb při selhání pro jeden virtuální počítač, nebo vytvořit plán obnovení, který se skládá z více virtuálních počítačů a spustit testovací převzetí služeb pro plán. Testovací převzetí služeb při selhání simuluje váš mechanismus převzetí služeb při selhání a zotavení v izolované síti.

> [!NOTE]
> Můžete vytvořit plán obnovení pro aplikaci na portálu Azure.
>
>

Pokud chcete zkontrolovat na dokončení operace, postupujte podle kroků v [monitorování aktivity](#monitor).

### <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání
1. Spustit níže rutiny získat virtuální počítače k síti virtuálních počítačů, do které chcete testovat převzetí služeb při selhání.

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. Proveďte testovací převzetí služeb virtuálního počítače provedením následujících akcí:

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
3. Pomocí následujícího postupu proveďte testovací převzetí služeb při selhání plánu obnovení:

       $recoveryplanname = "test-recovery-plan"

       $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

### <a name="run-a-planned-failover"></a>Spusťte plánované převzetí služeb při selhání
1. Proveďte plánované převzetí služeb při selhání virtuálního počítače provedením následujících akcí:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
2. Proveďte plánované převzetí služeb při selhání plánu obnovení následujícím způsobem:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>Spustit neplánované převzetí služeb při selhání
1. Pomocí následujícího postupu proveďte neplánované převzetí služeb při selhání virtuálního počítače:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2. proveďte pomocí následujícího postupu neplánované převzetí služeb při selhání plánu obnovení:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <a name=monitor></a>Sledování aktivity
Použijte následující příkazy k monitorování aktivit. Všimněte si, že máte čekat mezi úlohy pro zpracování ukončíte.

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



## <a name="next-steps"></a>Další kroky
[Další informace](/powershell/module/azurerm.recoveryservices.backup/#recovery) o Azure Site Recovery pomocí rutin prostředí PowerShell Azure Resource Manager.
