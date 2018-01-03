---
title: "Vytvoření virtuálního počítače Windows pomocí prostředí PowerShell | Microsoft Docs"
description: "Vytvořte virtuální počítače s Windows pomocí Azure PowerShell a modelu nasazení classic."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 42c0d4be-573c-45d1-b9b0-9327537702f7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: af672a873b33cbd0b6151fd564e84c96f0b6e1e3
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-windows-virtual-machine-with-powershell-and-the-classic-deployment-model"></a>Vytvoření virtuálního počítače s Windows pomocí prostředí PowerShell a modelu nasazení classic
> [!div class="op_single_selector"]
> * [Portál Azure – Windows](tutorial.md)
> * [PowerShell – Windows](create-powershell.md)
> 
> 

<br>

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic. Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager. Zjistěte, jak [provést tento postup pomocí modelu Resource Manageru](../../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Tyto kroky ukazují, jak přizpůsobit sadu příkazů prostředí Azure PowerShell, které vytvářejí a předem nakonfigurujte virtuální počítač systému Windows Azure pomocí stavebním blokem přístup. Tento proces slouží k rychlému vytváření sadu příkazů pro nový virtuální počítač systému Windows a rozbalte stávajícího nasazení nebo vytvořit více sady příkazů, které rychle vytvářet vlastní vývoj/testování nebo pro prostředí IT.

Tyto kroky použijte přístup doplňovat-v-the-prázdné hodnoty pro vytvoření sady příkazů prostředí Azure PowerShell. Tento přístup může být užitečné, pokud začínáte používat prostředí PowerShell nebo jenom chcete vědět, jaké hodnoty chcete zadat pro úspěšné konfiguraci. Pokročilí uživatelé prostředí PowerShell můžete provést příkazy a nahraďte vlastní hodnoty pro proměnné (řádky začínající "$").

Pokud jste tak ještě neučinili, postupujte podle pokynů v [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) nainstalujte prostředí Azure PowerShell v místním počítači. Potom otevřete příkazový řádek prostředí Windows PowerShell.

## <a name="step-1-add-your-account"></a>Krok 1: Přidání účtu
1. Zadejte v příkazovém prostředí PowerShell, **Add-AzureAccount** a klikněte na tlačítko **Enter**. 
2. Zadejte e-mailová adresa spojená s předplatným Azure a klikněte na tlačítko **pokračovat**. 
3. Zadejte heslo k vašemu účtu. 
4. Klikněte na tlačítko **přihlášení**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Krok 2: Nastavení vaše předplatné a účet úložiště
Spuštěním těchto příkazů na příkazovém řádku prostředí Windows PowerShell nastavte předplatné a účet úložiště. Nahraďte všechna data v uvozovkách, včetně < a > znaky s správné názvy.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Název správné předplatné můžete získat z vlastnosti Název_předplatného výstup **Get-AzureSubscription** příkaz. Název účtu úložiště správné můžete získat z vlastnosti Label výstup **Get-AzureStorageAccount** příkaz po spuštění **Select-AzureSubscription** příkaz.

## <a name="step-3-determine-the-imagefamily"></a>Krok 3: Určení ImageFamily
Dále musíte určit ImageFamily nebo popisek hodnotu pro tento konkrétní obrázek odpovídající virtuální počítač Azure, kterou chcete vytvořit. Můžete získat seznam dostupných hodnot ImageFamily pomocí tohoto příkazu.

    Get-AzureVMImage | select ImageFamily -Unique

Tady jsou některé příklady ImageFamily hodnot pro počítače se systémem Windows:

* Windows Server 2012 R2 Datacenter
* Windows Server 2008 R2 SP1
* Windows Server 2016 Technical Preview 4
* SQL Server 2012 SP1 Enterprise na systém Windows Server 2012

Pokud zjistíte, bitovou kopii, kterou hledáte, otevřete novou instanci textového editoru zvoleného nebo prostředí PowerShell integrovaném skriptovacím prostředí (ISE). Zkopírujte následující do nového textového souboru nebo prostředí PowerShell ISE, nahraďte hodnotu ImageFamily.

    $family="<ImageFamily value>"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

V některých případech je název bitové kopie ve vlastnosti popisek namísto hodnoty ImageFamily. Pokud nebyl nalezen bitovou kopii, kterou hledáte pomocí vlastnosti ImageFamily, seznam vlastností jejich popisek pomocí tohoto příkazu bitové kopie.

    Get-AzureVMImage | select Label -Unique

Pokud zjistíte správné bitové kopie s Tento příkaz, otevřete novou instanci textového editoru zvoleného nebo prostředí PowerShell ISE. Zkopírujte následující do nového textového souboru nebo prostředí PowerShell ISE, nahraďte hodnotu popisku.

    $label="<Label value>"
    $image = Get-AzureVMImage | where { $_.Label -eq $label } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

## <a name="step-4-build-your-command-set"></a>Krok 4: Vytvoření vaší sadu příkazů
Sestavení zbytek příkazu nastavte kopírování příslušné sady bloky níže do nového textového souboru nebo ISE a pak vyplněním hodnoty proměnné a odebrání < a > znaků. V tématu dva [příklady](#examples) na konci tohoto článku představu o konečný výsledek.

Spuštění příkazu nastavte volbou jedné z těchto dvou příkaz bloky (povinné).

Možnost 1: Zadejte název virtuálního počítače a velikost.

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Možnost 2: Zadejte název, velikost a název sady dostupnosti.

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $availset="<set name>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image -AvailabilitySetName $availset

Hodnoty InstanceSize pro D – DS- a G-series virtuálních počítačů naleznete v části [virtuálního počítače a Cloud velikost služeb pro Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

> [!NOTE]
> Pokud máte smlouvu Enterprise Agreement pomocí programu Software Assurance a chcete využít výhod Windows Server [výhody použití hybridní](https://azure.microsoft.com/pricing/hybrid-use-benefit/), přidejte **- LicenseType** parametru  **Nové AzureVMConfig** rutiny, předejte hodnotu **Windows_Server** pro typické případy použití.  Ujistěte se, že používáte bitovou kopii, kterou jste nahráli; standardní bitové kopie z Galerie nelze použít s výhodou použití hybridní.
> 
> 

Volitelně můžete pro samostatné počítače se systémem Windows zadejte účet místního správce a heslo.

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

Zvolte silné heslo. Pokud chcete zkontrolovat její síly, najdete v části [kontrolu hesla: pomocí silného hesla](https://www.microsoft.com/security/pc-security/password-checker.aspx).

Volitelně můžete přidat počítače se systémem Windows do existující domény služby Active Directory, zadejte účet místního správce a heslo, domény a název a heslo účtu domény.

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account."
    $cred2=Get-Credential –Message "Now type the name (not including the domain) and password of an account that has permission to add the machine to the domain."
    $domaindns="<FQDN of the domain that the machine is joining>"
    $domacctdomain="<domain of the account that has permission to add the machine to the domain>"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

Další možnosti předběžné konfigurace pro virtuální počítače se systémem Windows, najdete v části Syntaxe **Windows** a **třídy WindowsDomain** parametr nastaví v [přidat AzureProvisioningConfig ](/powershell/module/azure/add-azureprovisioningconfig).

Virtuální počítač volitelně přiřadíte konkrétní IP adresu, označuje jako statické vyhrazené IP adresy.

    $vm1 | Set-AzureStaticVNetIP -IPAddress <IP address>

Můžete ověřit, že je k dispozici s konkrétní IP adresu:

    Test-AzureStaticVNetIP –VNetName <VNet name> –IPAddress <IP address>

Volitelně můžete přiřadíte virtuální počítač do konkrétní podsítě v virtuální sítě Azure.

    $vm1 | Set-AzureSubnet -SubnetNames "<name of the subnet>"

Volitelně můžete přidáte jeden datový disk k virtuálnímu počítači.

    $disksize=<size of the disk in GB>
    $disklabel="<the label on the disk>"
    $lun=<Logical Unit Number (LUN) of the disk>
    $hcaching="<Specify one: ReadOnly, ReadWrite, None>"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

Pro řadič domény služby Active Directory nastavte $hcaching na "Žádný".

Volitelně můžete přidáte virtuální počítač do existující skupinu s vyrovnáváním zatížení pro externí přenosy.

    $protocol="<Specify one: tcp, udp>"
    $localport=<port number of the internal port>
    $pubport=<port number of the external port>
    $endpointname="<name of the endpoint>"
    $lbsetname="<name of the existing load-balanced set>"
    $probeprotocol="<Specify one: tcp, http>"
    $probeport=<TCP or HTTP port number of probe traffic>
    $probepath="<URL path for probe traffic>"
    $vm1 | Add-AzureEndpoint -Name $endpointname -Protocol $protocol -LocalPort $localport -PublicPort $pubport -LBSetName $lbsetname -ProbeProtocol $probeprotocol -ProbePort $probeport -ProbePath $probepath

Nakonec vyberte jednu z těchto bloků požadovaný příkaz pro vytvoření virtuálního počítače.

Možnost 1: Vytvoření virtuálního počítače ve stávající cloudovou službu.

    New-AzureVM –ServiceName "<short name of the cloud service>" -VMs $vm1

Krátký název cloudové služby je název, který se zobrazí v seznamu cloudové služby na portálu Azure nebo v seznamu skupin prostředků na portálu Azure.

Možnost 2: Vytvoření virtuálního počítače do existující cloudové služby a virtuální sítě.

    $svcname="<short name of the cloud service>"
    $vnetname="<name of the virtual network>"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

## <a name="step-5-run-your-command-set"></a>Krok 5: Spuštění vaší sadu příkazů
Prohlédněte si sadu Azure PowerShell příkaz, který jste vytvořili v textovém editoru nebo prostředí PowerShell ISE, který se skládá z více bloků příkazy z kroku 4. Ujistěte se, zda jste zadali všechny potřebné proměnné a zda mají správné hodnoty. Také se ujistěte, že jste odstranili všechny < a > znaků.

Pokud používáte textový editor, zkopírujte do schránky sadu příkazů nástroje a potom klikněte pravým tlačítkem spusťte příkazový řádek prostředí Windows PowerShell. To bude vydávat sadu příkazů nástroje jako řadu příkazů prostředí PowerShell a vytvořte virtuální počítač Azure. Alternativně spusťte příkaz nastavit v integrovaném Skriptovacím prostředí PowerShell.

Pokud vytvoříte tento virtuální počítač znovu nebo podobné jeden, můžete:

* Uložte tento příkaz nastavit jako soubor skriptu prostředí PowerShell (*.ps1).
* Uložit tento příkaz nastavit jako runbook služby Azure Automation v **účty Automation** části portálu Azure.

## <a id="examples"></a>Příklady
Tady jsou dva příklady použití výše uvedených kroků k sestavení sady příkazů prostředí Azure PowerShell, vytvořené na základě systému Windows Azure virtuální počítače.

### <a name="example-1"></a>Příklad 1
Potřebuji prostředí PowerShell sady příkazů vytvoření počáteční virtuálního počítače pro řadič domény služby Active Directory, který:

* Používá bitovou kopii systému Windows Server 2012 R2 Datacenter.
* Obsahuje název AZDC1.
* Je samostatný počítač.
* Má k dispozici další data disk 20 GB.
* Má statickou IP adresu 192.168.244.4.
* Je v podsíti back-end AZDatacenter virtuální sítě.
* Je v cloudové službě Azure-Válcovny.

Zde je příslušného příkazu prostředí Azure PowerShell k vytvoření tohoto virtuálního počítače s prázdné řádky mezi každého bloku čitelnější.

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="AZDC1"
    $vmsize="Medium"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

    $vm1 | Set-AzureSubnet -SubnetNames "BackEnd"

    $vm1 | Set-AzureStaticVNetIP -IPAddress 192.168.244.4

    $disksize=20
    $disklabel="DCData"
    $lun=0
    $hcaching="None"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

### <a name="example-2"></a>Příklad 2
Potřebuji prostředí PowerShell sady příkazů vytvoření virtuálního počítače pro server obchodní, který:

* Používá bitovou kopii systému Windows Server 2012 R2 Datacenter.
* Obsahuje název LOB1.
* Je členem domény corp.contoso.com.
* Má k dispozici další data disk 200 GB.
* Je v podsíti front-endu AZDatacenter virtuální sítě.
* Je v cloudové službě Azure-Válcovny.

Zde je příslušného příkazu prostředí Azure PowerShell k vytvoření tohoto virtuálního počítače.

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="LOB1"
    $vmsize="Large"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account."
    $cred2=Get-Credential –Message "Now type the name (not including the domain) and password of an account that has permission to add the machine to the domain."
    $domaindns="corp.contoso.com"
    $domacctdomain="CORP"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "FrontEnd"

    $disksize=200
    $disklabel="LOBData"
    $lun=0
    $hcaching="ReadWrite"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname


## <a name="next-steps"></a>Další kroky
Pokud budete potřebovat disk s operačním systémem, která je větší než 127 GB, můžete [rozbalte jednotku operačního systému](../../virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

