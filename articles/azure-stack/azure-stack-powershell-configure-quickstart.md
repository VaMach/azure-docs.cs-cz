---
title: "Instalace a konfigurace prostředí PowerShell pro rychlý start Azure zásobníku | Microsoft Docs"
description: "Další informace o instalaci a konfiguraci prostředí PowerShell pro Azure zásobníku."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 6996DFC1-5E05-423A-968F-A9427C24317C
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: mabrigg
ms.openlocfilehash: ca61562607da274f0e0c7f504d1a24723210f2f7
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="get-up-and-running-with-powershell-in-azure-stack"></a>A její spuštění pomocí prostředí PowerShell v Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Tento rychlý Start umožňuje instalace a konfigurace prostředí Azure zásobníku pomocí prostředí PowerShell. Skript, který nabízíme v tomto článku je vymezen na **zásobník Azure operátor** pouze.

Tento článek je zhuštěný verzi kroky, které jsou popsané v [instalaci prostředí PowerShell]( azure-stack-powershell-install.md), [stáhnout nástroje]( azure-stack-powershell-download.md), a [nakonfigurovat prostředí PowerShell Azure zásobníku operátor]( azure-stack-powershell-configure-admin.md) články. Pomocí skriptů v tomto tématu můžete nastavit prostředí PowerShell pro Azure zásobníku prostředí, které se nasadí s Azure Active Directory nebo Active Directory Federation Services (AD FS).  


## <a name="set-up-powershell-for-azure-active-directory-based-deployments"></a>Nastavení prostředí PowerShell pro nasazení založené na Azure Active Directory

Pokud jste připojení prostřednictvím sítě VPN, přihlaste se k vaší Azure zásobníku Development Kit nebo externí klienta se systémem Windows. Otevřete relaci prostředí PowerShell ISE zvýšenými oprávněními a spusťte následující skript. Nezapomeňte aktualizovat **TenantName**, **ArmEndpoint**, a **GraphAudience** proměnné v případě potřeby pro konfiguraci prostředí:

> [!IMPORTANT]
> Verze modulu PowerShell AzureRM 1.2.11 obsahuje seznam nejnovější změny. K upgradu z 1.2.10 verze, najdete v článku [příručka k migraci](https://aka.ms/azspowershellmigration).

```powershell
# Specify Azure Active Directory tenant name.
$TenantName = "<mydirectory>.onmicrosoft.com"

# Set the module repository and the execution policy.
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned `
  -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions, and then run the following command:
Get-Module -ListAvailable -Name Azure* | `
  Uninstall-Module

# Install PowerShell for Azure Stack.
Install-Module `
  -Name AzureRm.BootStrapper `
  -Force

Use-AzureRmProfile `
  -Profile 2017-03-09-profile `
  -Force

Install-Module `
  -Name AzureStack `
  -RequiredVersion 1.2.11 `
  -Force 

# Download Azure Stack tools from GitHub and import the connect module.
cd \

invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

expand-archive master.zip `
  -DestinationPath . `
  -Force

cd AzureStack-Tools-master

Import-Module .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = "<Keyvault DNS suffix for your environment>"


# Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint

# Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -AADTenantName $TenantName `
    -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID 
```

## <a name="set-up-powershell-for-ad-fs-based-deployments"></a>Nastavení prostředí PowerShell pro nasazení služby AD FS na základě

Pokud jsou v provozu Azure zásobníku při připojení k Internetu, můžete použít následující skript. Ale pokud pracujete zásobník Azure bez připojení k Internetu, použijte [odpojení způsob instalace prostředí PowerShell](azure-stack-powershell-install.md#install-powershell-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity) a zůstane stejný, jak ukazuje tento skript rutiny ke konfiguraci prostředí PowerShell. Pokud jste připojení prostřednictvím sítě VPN, přihlaste se k vaší Azure zásobníku Development Kit nebo externí klienta se systémem Windows. Otevřete relaci prostředí PowerShell ISE zvýšenými oprávněními a spusťte následující skript. Nezapomeňte aktualizovat **ArmEndpoint** a **GraphAudience** proměnné v případě potřeby pro konfiguraci prostředí:

```powershell

# Set the module repository and the execution policy.
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned `
  -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions and run the following command:
Get-Module -ListAvailable -Name Azure* | `
  Uninstall-Module

# Install PowerShell for Azure Stack.
Install-Module `
  -Name AzureRm.BootStrapper `
  -Force

Use-AzureRmProfile `
  -Profile 2017-03-09-profile `
  -Force

Install-Module `
  -Name AzureStack `
  -RequiredVersion 1.2.11 `
  -Force 

# Download Azure Stack tools from GitHub and import the connect module.
cd \
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

expand-archive master.zip `
  -DestinationPath . `
  -Force

cd AzureStack-Tools-master

Import-Module .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
$ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = "<Keyvault DNS suffix for your environment>"

# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint

# Get the Active Directory tenantId that is used to deploy Azure Stack     
$TenantID = Get-AzsDirectoryTenantId `
    -ADFS `
    -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID
```

## <a name="test-the-connectivity"></a>Testovací připojení

Nyní, když jste nakonfigurovali prostředí PowerShell, můžete otestovat konfiguraci tak, že vytvoříte skupinu prostředků:

```powershell
New-AzureRMResourceGroup -Name "ContosoVMRG" -Location Local
```

Po vytvoření skupiny prostředků **Stav zřizování** je nastavena na **úspěšné**.

## <a name="next-steps"></a>Další postup

* [Instalace a konfigurace rozhraní příkazového řádku](azure-stack-connect-cli.md)

* [Vývoj šablon](user/azure-stack-develop-templates.md)







