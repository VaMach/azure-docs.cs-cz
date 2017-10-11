---
title: "Replikace virtuálních počítačů Hyper-V do Azure na portálu classic pomocí prostředí PowerShell | Microsoft Docs"
description: "Automatizovat replikaci virtuálních počítačů technologie Hyper-V v cloudech VMM pomocí Site Recovery a prostředí PowerShell na portálu classic"
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: tysonn
ms.assetid: 9011f567-e0b4-4306-951a-b30da19f5db6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: 581daaaa5cc0cf8be782f834c6bdb3f27ee413fb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="replicate-hyper-v-vms-to-azure-with-powershell-in-the-classic-portal"></a>Replikace virtuálních počítačů Hyper-V do Azure pomocí prostředí PowerShell na portálu classic
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [Portál Classic](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell – Classic](site-recovery-deploy-with-powershell.md)
>
>

## <a name="overview"></a>Přehled
Azure Site Recovery přispívá ke strategii obchodní kontinuitu a po havárii obnovení (BCDR) tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů v různých scénářích nasazení. Úplný seznam nasazení scénářů najdete v článku [přehled Azure Site Recovery](site-recovery-overview.md).

Tento článek ukazuje, jak pomocí prostředí PowerShell pro automatizaci běžných úkolů, které je třeba provést při nastavení Azure Site Recovery replikovat virtuální počítače Hyper-V v cloudech System Center VMM do úložiště Azure.

Článek obsahuje předpoklady pro scénář a ukazuje, jak nastavit trezor Site Recovery, nainstalujte zprostředkovatele Azure Site Recovery na zdrojovém serveru VMM, zaregistrujte server v trezoru, přidat účet úložiště Azure, nainstalujte Azure Recovery Agent služeb na hostitelských serverech technologie Hyper-V, nakonfigurovat nastavení ochrany pro cloudy VMM, které se použijí na všechny chráněné virtuální počítače a pak povolte ochranu pro tyto virtuální počítače. Nakonec otestujete převzetí služeb při selhání, abyste měli jistotu, že všechno funguje podle očekávání.

Pokud narazíte na problémy nastavení tento scénář, zveřejněte svoje otázky na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic.
>
>

## <a name="before-you-start"></a>Než začnete
Ujistěte se, že máte zavedenou tyto požadavky:

### <a name="azure-prerequisites"></a>Požadavky Azure
* Budete potřebovat účet [Microsoft Azure](https://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).
* K ukládání replikovaných dat budete potřebovat účet úložiště Azure. Účet musí povolenou geografickou replikací. Měl by být ve stejné oblasti jako trezor Azure Site Recovery a být přidružený ke stejnému předplatnému. [Další informace o Azure storage](../storage/common/storage-introduction.md).
* Budete potřebovat, abyste měli jistotu, že virtuální počítače, které chcete chránit vyhovují [požadavky virtuálního počítače Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

### <a name="vmm-prerequisites"></a>Požadavky VMM
* Budete potřebovat VMM server běžící na System Center 2012 R2.
* Budete potřebovat alespoň jeden cloud na serveru VMM, který chcete chránit. Cloud by měl obsahovat:
  * Minimálně jednu skupinu hostitelů VMM.
  * Jeden nebo více hostitelských serverů Hyper-V nebo clusterů v každé skupině hostitelů.
  * Jeden nebo více virtuálních počítačů na zdrojovém technologie Hyper-V serveru.

### <a name="hyper-v-prerequisites"></a>Požadavky technologie Hyper-V
* Servery hostitele technologie Hyper-V musí používat alespoň **systému Windows Server 2012** s rolí Hyper-V nebo **Microsoft Hyper-V Server 2012** a mít nainstalované nejnovější aktualizace.
* Pokud používáte technologii Hyper-V v clusteru, je důležité vědět, že zprostředkovatel clusteru se nevytvoří automaticky, pokud máte clustery založené na statických IP adresách. Budete ho muset nakonfigurovat ručně. Chcete-li to provést ve Správci serveru > Správce clusteru převzetí služeb při selhání, připojte se ke clusteru, klikněte na tlačítko **konfigurovat roli** a vyberte **zprostředkovatele replik technologie Hyper-V** v **vybrat roli** Obrazovka Průvodce vysokou dostupností.
* Libovolný server hostitele technologie Hyper-V nebo clusteru, pro který chcete spravovat ochranu musí být součástí cloudu VMM.

### <a name="network-mapping-prerequisites"></a>Požadavky na mapování sítě
Při ochraně virtuálních počítačů v síti Azure mapování mapuje sítě virtuálních počítačů na zdrojovém serveru VMM a cílové sítě Azure, aby bylo možné následující:

* Všechny počítače, které převzetí služeb při selhání ve stejné síti může připojit k sobě navzájem, bez ohledu na plánu obnovení jsou v.
* Pokud je v cílové síti Azure nastavená brána sítě, virtuální počítače se budou moct připojit k jiným místním virtuálním počítačům.
* Pokud nenakonfigurujete mapování sítě, budou se moct po předání služeb při selhání do Azure připojit k sobě navzájem pouze virtuální počítače, u kterých dojde k převzetí služeb při selhání v rámci stejného plánu obnovení.

Pokud chcete nasadit mapování sítě, budete potřebovat následující:

* Virtuální počítače, který chcete chránit na zdrojovém serveru VMM, musí být připojené k síti virtuálních počítačů. Tato síť musí být propojená na logickou síť, která je přidružená ke cloudu.
* Síť Azure, ke které se budou moct po převzetí služeb při selhání připojit replikované virtuální počítače. Tuto síť vyberete v okamžiku převzetí služeb při selhání. Síť musí být ve stejné oblasti jako vaše předplatné Azure Site Recovery.

### <a name="powershell-prerequisites"></a>Požadavky prostředí PowerShell
Ujistěte se, že máte Azure PowerShell připravené na vynucování. Pokud už používáte prostředí PowerShell, budete muset upgradovat na verzi 0.8.10 nebo novější. Informace o nastavení prostředí PowerShell najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs). Po nastavení a nakonfigurovat prostředí PowerShell, můžete zobrazit všechny dostupné rutiny služby [zde](/powershell/azure/overview).

Další informace o tipy, které mohou pomoci pomocí rutin, jako je například jak hodnoty parametrů, vstupy a výstupy jsou obvykle zpracovávány v prostředí Azure PowerShell najdete v části [Začínáme s Azure rutiny](/powershell/azure/get-started-azureps).

## <a name="step-1-set-the-subscription"></a>Krok 1: Nastavte předplatné
V prostředí PowerShell spusťte tyto rutiny:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

Prvky v rámci "< >" nahraďte konkrétní informace.

## <a name="step-2-create-a-site-recovery-vault"></a>Krok 2: Vytvoření trezoru Site Recovery
V prostředí PowerShell nahraďte elementy v rámci "< >" konkrétní informace a spusťte tyto příkazy:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>Krok 3: Vygenerování registračního klíče trezoru
Vygenerujte registrační klíč v trezoru. Po tom, co si stáhnete zprostředkovatele Azure Site Recovery a nainstalujete ho na server VMM, použijete tento klíč k registraci serveru VMM v trezoru.

1. Získat v souboru nastavení trezoru a nastavit kontext:

   ```

   $VaultName = "<testvault123>"
   $VaultGeo  = "<Southeast Asia>"
   $OutputPathForSettingsFile = "<c:\>"

   $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

   ```
2. Nastavte kontext trezoru spuštěním následujících příkazů:

   ```

   $VaultSettingFilePath = $vaultSetingsFile.FilePath
   $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

   ```

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Krok 4: Instalace nástroje Azure Site Recovery Provider
1. Na počítači VMM vytvořte adresář tak, že spustíte následující příkaz:

   ```

   pushd C:\ASR\

   ```
2. Extrahujte soubory pomocí stažené poskytovatele tak, že spustíte následující příkaz

   ```

   AzureSiteRecoveryProvider.exe /x:. /q

   ```
3. Nainstalujte poskytovatele pomocí následujících příkazů:

   ```

   .\SetupDr.exe /i

   ```

   ```

   $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
   do
   {
     $isNotInstalled = $true;
     if(Test-Path $installationRegPath)
     {
         $isNotInstalled = $false;
     }
   }While($isNotInstalled)

   ```

   Počkejte na dokončení instalace.
4. Zaregistrujte server v trezoru, pomocí následujícího příkazu:

   ```

   $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
   pushd $BinPath
   $encryptionFilePath = "C:\temp\"
   .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

   ```

## <a name="step-5-create-an-azure-storage-account"></a>Krok 5: Vytvoření účtu úložiště Azure
Pokud nemáte účet úložiště Azure, vytvořte účet povolenou geografickou replikací spuštěním následujícího příkazu:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Všimněte si, že účet úložiště musí být ve stejné oblasti jako služba Azure Site Recovery a být přidružený ke stejnému předplatnému.

## <a name="step-6-install-the-azure-recovery-services-agent"></a>Krok 6: Instalace agenta služeb zotavení Azure
Z portálu Azure nainstalujte agenta služeb zotavení Azure na každém serveru hostitele technologie Hyper-V v cloudech VMM, který chcete chránit.

Na všech hostitelích VMM, spusťte následující příkaz:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>Krok 7: Konfigurace cloudu nastavení ochrany
1. Vytvoření profilu ochrany cloudu do Azure tak, že spustíte následující příkaz:

   ```

   $ReplicationFrequencyInSeconds = "300";
   $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds     $ReplicationFrequencyInSeconds;

   ```
2. Kontejner ochrany získáte spuštěním následujících příkazů:

   ```

   $PrimaryCloud = "testcloud"
   $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

   ```
3. Přidružení kontejner ochrany začněte cloudu:

   ```

   $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

   ```
4. Po dokončení úlohy spusťte následující příkaz:

        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


1. Po dokončení zpracování úlohy spusťte následující příkaz:

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



Pokud chcete zkontrolovat na dokončení operace, postupujte podle kroků v [monitorování aktivity](#monitor).

## <a name="step-8-configure-network-mapping"></a>Krok 8: Nakonfigurování mapování sítě
Před zahájením mapování sítě ověřte, jestli jsou virtuální počítače na zdrojovém serveru VMM připojené k síti virtuálních počítačů. Kromě toho můžete vytvořte jeden nebo více virtuálních sítí Azure. Na jednu síť Azure je možné namapovat více sítí virtuálních počítačů.

Pamatujte, že pokud má cílová síť více podsítí a jedna z těchto podsítí má stejný název jako podsíť, ve které se nachází zdrojový virtuální počítač, bude virtuální počítač repliky po převzetí služeb při selhání připojen k této cílové podsíti. Pokud není cílová podsíť s odpovídajícím názvem, virtuální počítač se připojí k první podsíti v síti.

První příkaz získá servery pro aktuální trezoru Azure Site Recovery. Příkaz ukládá v proměnné pole $Servers servery Microsoft Azure Site Recovery.

    $Servers = Get-AzureSiteRecoveryServer


V druhém příkazu získá síť site recovery pro první server v poli $Servers. Příkaz ukládá v proměnné $Networks sítí.

    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

V třetím příkazu získá předplatné Azure pomocí rutiny Get-AzureSubscription a uloží tuto hodnotu v proměnné $Subscriptions.

    $Subscriptions = Get-AzureSubscription



Příkaz čtvrtý získá pomocí rutiny Get-AzureVNetSite a potom tuto hodnotu v proměnné $AzureVmNetworks virtuálních sítí Azure.

    $AzureVmNetworks = Get-AzureVNetSite



Poslední rutina vytvoří mapování mezi primární síť a síť virtuálního počítače Azure. Rutina určuje primární síť jako první prvek $Networks. Rutina určuje síť virtuálních počítačů jako první prvek $AzureVmNetworks pomocí jeho ID. Příkaz obsahuje ID vašeho předplatného Azure.

    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>Krok 9: Povolení ochrany pro virtuální počítače
Po správném nakonfigurování serverů, cloudů a sítí můžete povolit ochranu pro virtuální počítače v cloudu. Je třeba počítat s následujícím:

Virtuální počítače, musí splňovat [požadavky virtuálního počítače Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

Aby bylo možné povolit ochranu, musí být pro virtuální počítač nastavené vlastnosti operačního systému a disku operačního systému. Vlastnost můžete nastavit při vytváření virtuálního počítače v nástroji VMM pomocí šablony virtuálního počítače. Tyto vlastnosti také můžete nastavit pro existující virtuální počítače, a to na kartách **Obecné** a **Konfigurace hardwaru** ve vlastnostech virtuálního počítače. Pokud tyto vlastnosti nenastavíte v nástroji VMM, budete je moct nakonfigurovat na portálu Azure Site Recovery.

1. Pokud chcete povolit ochranu, spusťte následující příkaz k získání kontejneru ochrany:

     $ProtectionContainer = get-AzureSiteRecoveryProtectionContainer-název $CloudName
2. Získáte entita ochrany (VM) tak, že spustíte následující příkaz:

        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



1. Povolte DR pro virtuální počítač tak, že spustíte následující příkaz:

        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity     -Protection Enable -Force



## <a name="test-your-deployment"></a>Otestujte nasazení
Chcete nasazení otestovat můžete spustit testovací převzetí služeb při selhání pro jeden virtuální počítač, nebo vytvořit plán obnovení, který se skládá z více virtuálních počítačů a spustit testovací převzetí služeb pro plán. Testovací převzetí služeb při selhání simuluje váš mechanismus převzetí služeb při selhání a zotavení v izolované síti. Poznámky:

* Pokud se chcete připojit k virtuálnímu počítači v Azure pomocí Vzdálené plochy po převzetí služeb po selhání, pak před spuštěním testu převzetí služeb povolte na virtuálním počítači připojení ke Vzdálené ploše.
* Po převzetí služeb při selhání budete používat veřejnou IP adresu pro připojení k virtuálnímu počítači v Azure pomocí vzdálené plochy. Pokud to chcete provést, zkontrolujte, že nemáte žádné zásady domény, které by vám bránily v připojení k virtuálnímu počítači pomocí veřejné adresy.

Pokud chcete zkontrolovat na dokončení operace, postupujte podle kroků v [monitorování aktivity](#monitor).

### <a name="create-a-recovery-plan"></a>Vytvoření plánu obnovení
1. Vytvoření souboru XML jako šablona pro váš plán obnovení pomocí níže uvedené údaje a pak ho uložte jako "C:\RPTemplatePath.xml".
2. Změňte RecoveryPlan uzlu Id, název, PrimaryServerId a SecondaryServerId.
3. Změňte uzlu ProtectionEntity PrimaryProtectionEntityId (vmid z nástroje VMM).
4. Přidáním více uzlů ProtectionEntity můžete přidat další virtuální počítače.

        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"     PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"     SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03-    cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"     Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"     After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



1. Vyplňte dat v šabloně:

        $TemplatePath = "C:\RPTemplatePath.xml";



1. Vytvořte RecoveryPlan:

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;

### <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání
1. Získání objektu RecoveryPlan spuštěním následujícího příkazu:

     $RPObject = get-AzureSiteRecoveryRecoveryPlan-název $RPName;
2. Spusťte testovací převzetí služeb tak, že spustíte následující příkaz:

        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


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
[Další informace](/powershell/azure/overview) o rutin Powershellu pro Azure Site Recovery. </a>.
