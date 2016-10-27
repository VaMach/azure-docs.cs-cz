<properties
    pageTitle="Vytvoření škálovací sady virtuálních počítačů pomocí PowerShellu | Microsoft Azure"
    description="Vytvoření škálovací sady virtuálních počítačů pomocí PowerShellu"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="davidmu"/>


# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Vytvoření 	škálovací sady virtuálních počítačů s Windows pomocí Azure PowerShellu

Tento postup navazuje na metodu vyplňování prázdných polí při vytváření škálovací sady virtuálních počítačů Azure. Další informace o škálovacích sadách najdete v tématu [Přehled škálovacích sad virtuálních počítačů](virtual-machine-scale-sets-overview.md).

Provedení kroků v tomto článku by mělo trvat asi 30 minut.

## <a name="step-1:-install-azure-powershell"></a>Krok1: Nainstalování prostředí Azure PowerShell

Projděte si článek [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md), kde najdete informace o instalaci nejnovější verze prostředí Azure PowerShell, výběru předplatného a přihlášení k účtu.

## <a name="step-2:-create-resources"></a>Krok 2: Vytvoření prostředků

Vytvořte prostředky, které jsou potřebné pro vaši novou škálovací sadu.

### <a name="resource-group"></a>Skupina prostředků

Škálovací sada virtuálních počítačů musí být součástí skupiny prostředků.

1. Získejte seznam dostupných umístění a podporovaných služeb:

        Get-AzureLocation | Sort Name | Select Name, AvailableServices

    Zobrazení by mělo být podobné následujícímu příkladu:

        Name                AvailableServices
        ----                -----------------
        Australia East      {Compute, Storage, PersistentVMRole, HighMemory}
        Australia Southeast {Compute, Storage, PersistentVMRole, HighMemory}
        Brazil South        {Compute, Storage, PersistentVMRole, HighMemory}
        Central India       {Compute, Storage, PersistentVMRole, HighMemory}
        Central US          {Compute, Storage, PersistentVMRole, HighMemory}
        East Asia           {Compute, Storage, PersistentVMRole, HighMemory}
        East US             {Compute, Storage, PersistentVMRole, HighMemory}
        East US 2           {Compute, Storage, PersistentVMRole, HighMemory}
        Japan East          {Compute, Storage, PersistentVMRole, HighMemory}
        Japan West          {Compute, Storage, PersistentVMRole, HighMemory}
        North Central US    {Compute, Storage, PersistentVMRole, HighMemory}
        North Europe        {Compute, Storage, PersistentVMRole, HighMemory}
        South Central US    {Compute, Storage, PersistentVMRole, HighMemory}
        South India         {Compute, Storage, PersistentVMRole, HighMemory}
        Southeast Asia      {Compute, Storage, PersistentVMRole, HighMemory}
        West Europe         {Compute, Storage, PersistentVMRole, HighMemory}
        West India          {Compute, Storage, PersistentVMRole, HighMemory}
        West US             {Compute, Storage, PersistentVMRole, HighMemory}

2. Vyberte umístění, která vám nejlépe vyhovuje, nahraďte jeho názvem hodnotu **$locName** a pak vytvořte proměnnou:

        $locName = "location name from the list, such as Central US"

3. Nahraďte hodnotu **$rgName** názvem, který chcete použít pro novou skupinu prostředků, a pak vytvořte proměnnou: 

        $rgName = "resource group name"
        
4. Vytvořte skupinu prostředků:
    
        New-AzureRmResourceGroup -Name $rgName -Location $locName

    Zobrazení by mělo být podobné následujícímu příkladu:

        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="storage-account"></a>Účet úložiště

Virtuální počítač používá účet úložiště k ukládání disku operačního systému a diagnostických dat používaných pro škálování. Pokud je to možné, doporučuje se, aby každý virtuální počítač vytvořený ve škálovací sadě měl vlastní účet úložiště. Pokud to není možné, nekalkulujte s více než 20 virtuálními počítači na jeden účet úložiště. V příkladu v tomto článku se vytvářejí 3 účty úložiště pro 3 virtuální počítače.

1. Nahraďte hodnotu **$saName** názvem pro účet úložiště. Otestujte, že je název jedinečný. 

        $saName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $saName

    Pokud je odpověď **True**, navrhovaný název je jedinečný.

3. Hodnotu **$saType** nahraďte typem účtu úložiště a pak vytvořte proměnnou:  

        $saType = "storage account type"
        
    Možné hodnoty: Standard_LRS, Standard_GRS, Standard_RAGRS nebo Premium_LRS.
        
4. Vytvořte účet:
    
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName

    Zobrazení by mělo být podobné následujícímu příkladu:

        ResourceGroupName   : myrg1
        StorageAccountName  : myst1
        Id                  : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft
                              .Storage/storageAccounts/myst1
        Location            : centralus
        AccountType         : StandardLRS
        CreationTime        : 3/15/2016 4:51:52 PM
        CustomDomain        :
        LastGeoFailoverTime :
        PrimaryEndpoints    : Microsoft.Azure.Management.Storage.Models.Endpoints
        PrimaryLocation     : centralus
        ProvisioningState   : Succeeded
        SecondaryEndpoints  :
        SecondaryLocation   :
        StatusOfPrimary     : Available
        StatusOfSecondary   :
        Tags                : {}
        Context             : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext

5. Opakováním kroků 1 až 4 vytvořte 3 účty úložiště, například myst1, myst2 a myst3.

### <a name="virtual-network"></a>Virtuální síť

Virtuální síť je pro virtuální počítače ve škálovací sadě nutná.

1. Nahraďte hodnotu **$subnetName** názvem, který chcete používat pro podsíť ve virtuální síti, a pak vytvořte proměnnou: 

        $subnetName = "subnet name"
        
2. Vytvořte konfiguraci podsítě:
    
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
        
    Předpona adresy může být ve vaší virtuální síti jiná.

3. Nahraďte hodnotu **$netName** názvem, který chcete používat pro virtuální síť, a pak vytvořte proměnnou: 

        $netName = "virtual network name"
        
4. Vytvořte virtuální síť:
    
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="public-ip-address"></a>Veřejná IP adresa

Před vytvořením síťového rozhraní je potřeba vytvořit veřejnou IP adresu.

1. Nahraďte hodnotu **$domName** popiskem názvu domény, který chcete používat s veřejnou IP adresou, a pak vytvořte proměnnou:  

        $domName = "domain name label"
        
    Popisek může obsahovat jen písmena, číslice a pomlčky a poslední znak musí být písmeno nebo číslice.
    
2. Otestujte, jestli je název jedinečný:
    
        Test-AzureRmDnsAvailability -DomainQualifiedName $domName -Location $locName

    Pokud je odpověď **True**, navrhovaný název je jedinečný.

3. Nahraďte hodnotu **$pipName** názvem, který chcete používat pro veřejnou IP adresu, a pak vytvořte proměnnou: 

        $pipName = "public ip address name"
        
4. Vytvořte veřejnou IP adresu:
    
        $pip = New-AzureRmPublicIpAddress -Name $pipName -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic -DomainNameLabel $domName

### <a name="network-interface"></a>Síťové rozhraní

Teď když máte veřejnou IP adresu, můžete vytvořit síťové rozhraní.

1. Nahraďte hodnotu **$nicName** názvem, který chcete používat pro síťové rozhraní, a pak vytvořte proměnnou: 

        $nicName = "network interface name"
        
2. Vytvořte síťové rozhraní:
    
        $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id

### <a name="configuration-of-the-scale-set"></a>Konfigurace škálovací sady

Máte teď všechny prostředky, které potřebujete pro konfiguraci škálovací sady, tak ji vytvořme.  

1. Nahraďte hodnotu **$ipName** názvem, který chcete používat pro konfiguraci IP, a pak vytvořte proměnnou: 

        $ipName = "IP configuration name"
        
2. Vytvořte konfiguraci IP:

        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id

2. Nahraďte hodnotu **$vmssConfig** názvem, který chcete používat pro škálovací sadu, a pak vytvořte proměnnou:   

        $vmssConfig = "Scale set configuration name"
        
3. Vytvořte konfiguraci pro škálovací sadu:

        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
        
    Tento příklad ukazuje škálovací sadu vytvářenou ze 3 virtuálních počítačů. Další informace o kapacitě škálovacích sad najdete v tématu [Přehled škálovacích sad virtuálních počítačů](virtual-machine-scale-sets-overview.md). Tento krok zahrnuje také nastavení velikosti (označované jako SkuName) virtuálních počítačů v sadě. Pokud chcete najít velikost, která vyhovuje vašim potřebám, nahlédněte do tématu [Velikosti virtuálních počítačů](../virtual-machines/virtual-machines-windows-sizes.md).
    
4. Do konfigurace škálovací sady přidejte konfiguraci síťového rozhraní:
        
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
        
    Zobrazení by mělo být podobné následujícímu příkladu:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>Profil operačního systému

1. Nahraďte hodnotu **$computerName** předponou názvu počítače, kterou chcete použít, a pak vytvořte proměnnou: 

        $computerName = "computer name prefix"
        
2. Nahraďte hodnotu **$adminName** názvem účtu správce na virtuálních počítačích a pak vytvořte proměnnou:

        $adminName = "administrator account name"
        
3. Nahraďte hodnotu **$adminPassword** heslem účtu a pak vytvořte proměnnou:

        $adminPassword = "password for administrator accounts"
        
4. Vytvořte profil operačního systému:

        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Profil úložiště

1. Nahraďte hodnotu **$storageProfile** názvem, který chcete používat pro profil úložiště, a pak vytvořte proměnnou:  

        $storageProfile = "storage profile name"
        
2. Vytvořte proměnné definující image, která se má použít:  
      
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
        
    Pokud chcete najít informace o jiných imagích, které je možné použít, nahlédněte do tématu [Procházení a výběr imagí virtuálních počítačů Azure pomocí Windows PowerShellu a Azure CLI](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md).
        
3. Nahraďte hodnotu **$vhdContainers** seznamem cest, ve kterých jsou uloženy virtuální pevné disky, například https://mystorage.blob.core.windows.net/vhds, a pak vytvořte proměnnou:
       
        $vhdContainers = @("https://myst1.blob.core.windows.net/vhds","https://myst2.blob.core.windows.net/vhds","https://myst3.blob.core.windows.net/vhds")
        
4. Vytvořte profil úložiště:

        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storageProfile -VhdContainer $vhdContainers -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>Škálovací sada virtuálních počítačů

Nakonec můžete vytvořit škálovací sadu.

1. Nahraďte hodnotu **$vmssName** názvem škálovací sady virtuálních počítačů a pak vytvořte proměnnou:

        $vmssName = "scale set name"
        
2. Vytvořte škálovací sadu:

        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss

    Mělo by se zobrazit něco jako v tomto příkladu, který zobrazuje úspěšné nasazení:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3:-explore-resources"></a>Krok 3: Prozkoumání prostředků

Škálovací sadu virtuálních počítačů, kterou jste vytvořili, můžete prozkoumat v následujících zdrojích:

- Azure Portal: Omezené množství informací je k dispozici prostřednictvím tohoto portálu.
- [Průzkumník prostředků Azure](https://resources.azure.com/): Tento nástroj je ideální pro ověření aktuálního stavu škálovací sady.
- Azure PowerShell: Informace získáte pomocí tohoto příkazu:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        
        Or 
        
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        

## <a name="next-steps"></a>Další kroky

- Právě vytvořenou škálovací sadu můžete spravovat na základě informací v tématu [Správa virtuálních počítačů ve škálovací sadě virtuálních počítačů](virtual-machine-scale-sets-windows-manage.md).
- Zvažte nastavení automatického škálování na základě pokynů v tématu [Automatické škálování a škálovací sady virtuálních počítačů](virtual-machine-scale-sets-autoscale-overview.md).
- Další informace o vertikálním škálování najdete v tématu [Vertikální automatické škálování se škálovacími sadami virtuálních počítačů](virtual-machine-scale-sets-vertical-scale-reprovision.md).



<!--HONumber=Oct16_HO3-->


