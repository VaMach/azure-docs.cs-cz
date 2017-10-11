---
title: "Replikace virtuálních počítačů technologie Hyper-V přes PowerShell a Azure Resource Manager | Microsoft Docs"
description: "Automatizovat replikaci virtuálních počítačů Hyper-V do Azure s Azure Site Recovery pomocí Powershellu a Azure Resource Manager."
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: 
ms.assetid: 05e0d76e-c3f5-4845-8052-094019b6d102
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: dbd562b73b0caecd0feb993bd6fb796dddb0438c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a>Replikaci mezi místními technologie Hyper-V virtuální počítače a Azure pomocí prostředí PowerShell a Azure Resource Manager
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
> * [Portál Classic](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

## <a name="overview"></a>Přehled
Azure Site Recovery přispívá ke strategii obchodní kontinuitu a po havárii obnovení tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů v různých scénářích nasazení. Úplný seznam scénářů nasazení, najdete v článku [přehled Azure Site Recovery](site-recovery-overview.md).

Prostředí Azure PowerShell je modul, který poskytuje rutiny pro správu Azure pomocí prostředí Windows PowerShell. Můžete pracovat se dva typy modulů: modul profilu Azure nebo modulu Azure Resource Manager.

Rutiny prostředí PowerShell obnovení lokality pomocí prostředí Azure PowerShell pro Azure Resource Manager, můžete chránit a obnovit vaše servery v Azure.

Tento článek popisuje, jak pomocí prostředí Windows PowerShell, společně s Azure Resource Manager, nasazení Site Recovery pro konfiguraci a orchestraci ochrany serveru do Azure. Příklad používané v tomto článku se dozvíte, jak chránit, převzetí služeb při selhání a obnovení virtuálních počítačů na hostiteli technologie Hyper-V do Azure pomocí Azure PowerShell s Azure Resource Manager.

> [!NOTE]
> Rutiny prostředí PowerShell obnovení lokality aktuálně umožňují konfigurovat následující: jednu lokalitu nástroje Virtual Machine Manager do jiného, lokalitu nástroje Virtual Machine Manager do Azure a Web Hyper-V do Azure.
>
>

Nemusíte být odborné prostředí PowerShell použít v tomto článku, ale potřebujete seznámit se základními koncepcemi, jako jsou moduly, rutiny a relace. Další informace o prostředí Windows PowerShell najdete v tématu [Začínáme s prostředím Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).

Také další informace o [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md).

> [!NOTE]
> Partneři Microsoftu, které jsou součástí programu Cloud Solution Provider (CSP) můžete konfigurovat a spravovat ochranu systému serverů se svým zákazníkům svým zákazníkům příslušného poskytovatele CSP předplatným (odběry klienta).
>
>

## <a name="before-you-start"></a>Než začnete
Ujistěte se, že máte zavedenou tyto požadavky:

* A [Microsoft Azure](https://azure.microsoft.com/) účtu. Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/). Kromě toho si můžete přečíst o [cenách Azure Site Recovery Manageru](https://azure.microsoft.com/pricing/details/site-recovery/).
* Azure PowerShell 1.0. Informace o této verzi a k jeho instalaci najdete v tématu [Azure PowerShell 1.0.](https://azure.microsoft.com/)
* [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) a [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) moduly. Můžete získat nejnovější verze tyto moduly z [Galerie prostředí PowerShell](https://www.powershellgallery.com/)

Tento článek ukazuje, jak pomocí prostředí Azure Powershell s Azure Resource Manager ke konfiguraci a správě ochrany vašich serverů. Příklad používané v tomto článku se dozvíte, jak k ochraně virtuálního počítače, spuštěná v hostiteli technologie Hyper-V do Azure. Následující požadované součásti jsou specifické pro tento příklad. Komplexnější sadu požadavků pro různé scénáře obnovení lokality naleznete v dokumentaci týkající se tento scénář.

* Hostitele Hyper-V se systémem Windows Server 2012 R2 nebo Microsoft Hyper-V Server 2012 R2, který obsahuje jeden nebo více virtuálních počítačů.
* Servery Hyper-V připojený k Internetu, buď přímo nebo prostřednictvím proxy serveru.
* Virtuální počítače, které chcete chránit musí být v souladu s [požadavky virtuálního počítače](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

## <a name="step-1-sign-in-to-your-azure-account"></a>Krok 1: Přihlaste se k účtu Azure
1. Otevřete konzolu prostředí PowerShell a spusťte tento příkaz k přihlášení k účtu Azure. Rutina zobrazí webové stránky, která vás vyzve k zadání pověření účtu.

        Login-AzureRmAccount

    Alternativně může také obsahovat přihlašovací údaje účtu jako parametr, který se `Login-AzureRmAccount` rutiny, pomocí `-Credential` parametr.

    Pokud jste poskytovatel CSP partnera práce jménem klienta, zadejte zákazníka jako klient, pomocí jejich název primární domény tenantID nebo klienta.

        Login-AzureRmAccount -Tenant "fabrikam.com"
2. Účet může mít několik odběrů, takže přidružíte odběr, který chcete používat s účtem.

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName
3. Ověřte, že vaše předplatné je registrovaný k použití Azure zprostředkovatele služeb zotavení a obnovení lokality pomocí následujících příkazů:

   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

   Ve výstupu z těchto příkazů Pokud **RegistrationState** je nastaven na **registrovaná**, můžete přejít ke kroku 2. V opačném případě byste měli zaregistrovat chybějící zprostředkovatele v rámci vašeho předplatného.

   Chcete-li zaregistrovat zprostředkovatele Azure Site Recovery a služeb zotavení, spusťte následující příkazy:

       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

   Ověřte, že zprostředkovatele úspěšně zaregistrován pomocí následujících příkazů: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` a `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.

## <a name="step-2-set-up-the-recovery-services-vault"></a>Krok 2: Nastavení trezoru služeb zotavení
1. Vytvořte skupinu prostředků Azure Resource Manager, ve kterém budete vytvořit trezor nebo použít existující skupinu prostředků. Pomocí následujícího příkazu můžete vytvořit novou skupinu prostředků:

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    kde proměnnou $ResourceGroupName obsahuje název skupiny prostředků, kterou chcete vytvořit, a proměnná $Geo obsahuje oblast Azure, ve kterém chcete vytvořit skupinu prostředků (například "Brazílie – jih").

    Seznam skupin prostředků v rámci vašeho předplatného můžete získat pomocí `Get-AzureRmResourceGroup` rutiny.
2. Vytvořte nový trezor služeb zotavení Azure následujícím způsobem:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Seznam trezorů existující můžete načíst pomocí `Get-AzureRmRecoveryServicesVault` rutiny.

> [!NOTE]
> Pokud chcete k provádění operací na trezorů Site Recovery vytvořené pomocí portálu classic nebo modulu Azure Service Management PowerShell, můžete načíst seznam takové trezorů pomocí `Get-AzureRmSiteRecoveryVault` rutiny. Měli byste vytvořit nový trezor služeb zotavení pro všechny nové operace. Trezorů Site Recovery, který jste dříve vytvořili podporují, ale nemají nejnovější funkce.
>
>

## <a name="step-3-set-the-recovery-services-vault-context"></a>Krok 3: Nastavte kontext trezoru služeb zotavení
1. Nastavte kontext trezoru spuštěním následujícího příkazu:

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-the-site"></a>Krok 4: Vytvoření serveru technologie Hyper-V a vygenerovat nový registrační klíč trezoru pro lokalitu.
1. Vytvoří nový web s technologií Hyper-V následujícím způsobem:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    Tato rutina se spustí úloha Site Recovery pro vytvoření webu a vrátí objekt úlohy Site Recovery. Počkejte na dokončení a ověřte, zda úlohy úloha byla úspěšně dokončena.

    Můžete načíst objekt úlohy a tím zkontrolujte aktuální stav úlohy, pomocí rutiny Get-AzureRmSiteRecoveryJob.
2. Vygenerování a stažení registrační klíč pro lokalitu, následujícím způsobem:

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    Zkopírujte stažený klíč na hostitele Hyper-V. Je nutné klíč k registraci hostitele technologie Hyper-V do lokality.

## <a name="step-5-install-the-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a>Krok 5: Instalace zprostředkovatele Azure Site Recovery a agenta služeb zotavení Azure na vašem hostiteli Hyper-V
1. Stažení instalačního programu pro nejnovější verzi zprostředkovatele ze [Microsoft](https://aka.ms/downloaddra).
2. Spusťte instalační program na vašem hostiteli Hyper-V a na konci instalace pokračujte krokem registrace.
3. Po zobrazení výzvy zadejte registrační klíč stažený lokality a dokončení registrace hostitele technologie Hyper-V do lokality.
4. Ověřte, že hostitel Hyper-V je zaregistrován k lokalitě pomocí následujícího příkazu:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-the-protection-container"></a>Krok 6: Vytvoření zásady replikace a přidružte ji k kontejner ochrany
1. Vytvořte zásadu replikace následujícím způsobem:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    Zkontrolujte vrácený úlohu, která zajistí, že úspěšné vytvoření zásad replikace.

   > [!IMPORTANT]
   > Zadaný účet úložiště by měl být ve stejné oblasti jako trezor služeb zotavení Azure a musí mít povolenou geografickou replikací.
   >
   > * Pokud je zadaný účet úložiště obnovení typ úložiště Azure (klasický), převzetí služeb při selhání chráněných počítačů obnovit počítače Azure IaaS (klasické).
   > * Pokud je zadaný účet úložiště obnovení typ úložiště Azure (Azure Resource Manager), převzetí služeb při selhání chráněných počítačů obnovit počítače Azure IaaS (Azure Resource Manager).
   >
   >
2. Získání kontejneru ochrany odpovídající k lokalitě, následujícím způsobem:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. Spusťte přidružení kontejner ochrany zásadám replikace, následujícím způsobem:

     $Policy = get-AzureRmSiteRecoveryPolicy - FriendlyName $PolicyName $associationJob = Start AzureRmSiteRecoveryPolicyAssociationJob-zásady $Policy - PrimaryProtectionContainer $protectionContainer

   Počkejte na dokončení úlohy přidružení a ujistěte se, že byla úspěšně dokončena.

## <a name="step-7-enable-protection-for-virtual-machines"></a>Krok 7: Povolení ochrany pro virtuální počítače
1. Získáte entita ochrany odpovídající virtuálních počítačů, které chcete chránit, následujícím způsobem:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. Začněte s ochranou virtuální počítač následujícím způsobem:

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

   > [!IMPORTANT]
   > Zadaný účet úložiště by měl být ve stejné oblasti jako trezor služeb zotavení Azure a musí mít povolenou geografickou replikací.
   >
   > * Pokud je zadaný účet úložiště obnovení typ úložiště Azure (klasický), převzetí služeb při selhání chráněných počítačů obnovit počítače Azure IaaS (klasické).
   > * Pokud je zadaný účet úložiště obnovení typ úložiště Azure (Azure Resource Manager), převzetí služeb při selhání chráněných počítačů obnovit počítače Azure IaaS (Azure Resource Manager).
   >
   > Pokud chráníte virtuální počítač má více než jeden disk připojený k němu, zadejte disku operačního systému pomocí *OSDiskName* parametr.
   >
   >
3. Počkejte, než pro virtuální počítače dosáhne chráněném stavu po počáteční replikaci. To může nějakou dobu trvat, v závislosti na faktorech, jako je množství dat, které se budou replikovat a dostupnou šířku pásma odesílání dat do Azure. Stav úlohy a StateDescription jsou aktualizované následujícím způsobem, při dosažení chráněném stavu virtuálního počítače.

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. Aktualizujte vlastnosti, obnovení, například velikost role virtuálního počítače a síť Azure připojit virtuální počítač karty síťového rozhraní na po převzetí služeb při selhání.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob

        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Krok 8: Spustit testovací převzetí služeb
1. Testovací převzetí služeb při selhání úlohy, spusťte následujícím způsobem:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. Ověřte, že testovací virtuální počítač je vytvořen v Azure. (Testovací převzetí služeb při selhání úlohy je pozastaven, po vytvoření testovacího virtuálního počítače v Azure. Dokončení úlohy tak, že vyčistí vytvořený rušivé vlivy při obnovení úlohy, jak je ukázáno v dalším kroku.)
3. Dokončete testovací převzetí služeb při selhání, následujícím způsobem:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a>Další kroky
[Další informace](https://msdn.microsoft.com/library/azure/mt637930.aspx) o Azure Site Recovery pomocí rutin prostředí PowerShell Azure Resource Manager.
