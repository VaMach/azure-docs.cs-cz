---
title: "Azure Active Directory Domain Services: Příručka pro správu | Microsoft Docs"
description: "Připojení virtuálního počítače s Windows k spravované doméně pomocí prostředí Azure PowerShell a modelu nasazení classic."
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 9143b843-7327-43c3-baab-6e24a18db25e
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 1bb1546fb616131a1e1868a0d0610c4cad5d73e2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a>Připojení virtuálního počítače Windows serveru ke spravované doméně pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [Portál Azure classic – Windows](active-directory-ds-admin-guide-join-windows-vm.md)
> * [PowerShell – Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se věnuje použití klasického modelu nasazení. Azure AD Domain Services modelu Resource Manager aktuálně nepodporuje.
>
>

Tyto kroky ukazují, jak přizpůsobit sadu příkazů prostředí Azure PowerShell, které vytvářejí a předem nakonfigurujte virtuální počítač systému Windows Azure pomocí stavebním blokem přístup. Tyto kroky můžete vytvořit virtuální počítač systému Windows Azure a připojte ho k spravované doméně služby Azure AD Domain Services.

Tyto kroky použijte přístup doplňovat-v-the-prázdné hodnoty pro vytvoření sady příkazů prostředí Azure PowerShell. Tento přístup může být užitečné, pokud začínáte používat prostředí PowerShell nebo pokud budete chtít vědět, jaké hodnoty Pokud chcete zadat pro úspěšné konfiguraci. Pokročilí uživatelé prostředí PowerShell můžete provést příkazy a nahraďte vlastní hodnoty pro proměnné (řádky začínající "$").

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

## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a>Krok 3: Podrobný návod - zřízení virtuálního počítače a připojte ho k spravované doméně
Zde je příslušného příkazu prostředí Azure PowerShell k vytvoření tohoto virtuálního počítače s prázdné řádky mezi každého bloku čitelnější.

Zadejte informace o virtuálního počítače s Windows, které se má zřídit.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

Hodnoty InstanceSize pro D – DS- a G-series virtuálních počítačů naleznete v části [virtuálního počítače a Cloud velikost služeb pro Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Zadejte informace o spravované domény.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Zadejte název cloudové služby.

    $svcname="Contoso100-test"

Zadejte název virtuální sítě, ke které by měl být připojený virtuální počítač. Ujistěte se, že spravované doméně AAD DS je k dispozici v této virtuální síti.

    $vnetname="MyPreviewVnet"

Vyberte obrázek virtuálního počítače, který má být použito k přidělení virtuálního počítače.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Konfigurace virtuálního počítače – název sady virtuálních počítačů, velikost instance & bitové kopie, který se má použít.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Získejte přihlašovací údaje místního správce pro virtuální počítač. Zvolte heslo, silné místního správce.

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

Získejte přihlašovací údaje pro účet uživatele patří do skupiny "Správci AAD řadič domény, pro připojení virtuálních počítačů k spravované doméně. Nezadávejte název domény – například v našem příkladu, určíme "bob" jako uživatelské jméno.

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

Konfigurace virtuálního počítače – zadejte požadované pověření systému & požadavek na připojení k doméně.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Nastavte podsíť pro virtuální počítač.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Volitelné: Přejděte na adresu IP domény. Pokud nastavíte IP adresy služby Azure AD Domain Services spravované domény, servery DNS pro virtuální síť, tento krok není povinný.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Nyní zřídit virtuální počítač připojený k doméně systému Windows.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a>Skript, který zřídit virtuální počítač s Windows a automaticky připojí k spravované doméně služby AAD Domain Services
Tuto sadu příkazů prostředí PowerShell vytvoří virtuální počítač pro server obchodní, který:

* Používá bitovou kopii systému Windows Server 2012 R2 Datacenter.
* Je velmi malé virtuální počítač.
* Obsahuje název Contoso100 testu.
* Je automaticky připojené do domény k spravované doméně contoso100.
* Se přidá do stejné virtuální síti jako spravované domény.

Zde je úplný ukázkový skript pro vytvoření virtuálního počítače s Windows a automaticky připojí k spravované doméně služby Azure AD Domain Services.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>Související obsah
* [Azure AD Domain Services – Příručka Začínáme](active-directory-ds-getting-started.md)
* [Správa spravované domény služby Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)
