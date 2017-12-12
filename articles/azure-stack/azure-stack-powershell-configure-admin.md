---
title: "Konfigurace prostředí PowerShell Azure zásobníku operátor | Microsoft Docs"
description: "Informace o konfiguraci Azure zásobníku operátor prostředí PowerShell."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 37D9CAC9-538B-4504-B51B-7336158D8A6B
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/23/2017
ms.author: mabrigg
ms.openlocfilehash: 96ce59d0390affaa57d05d6d08657f5c1a3c466a
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="configure-the-azure-stack-operators-powershell-environment"></a>Konfigurace prostředí PowerShell Azure zásobníku operátor

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Jako operátor zásobník Azure můžete nakonfigurovat vaše Azure zásobníku Development Kit na prostředí PowerShell. Po dokončení konfigurace, můžete použít PowerShell ke správě prostředků Azure zásobníku jako je například vytváření nabídek, plánů, kvóty, Správa výstrah a podobně. Toto téma je vymezen na použití s operátor cloudu prostředí pouze, pokud chcete pro nastavení prostředí PowerShell pro uživatelského prostředí, podívejte se na [nakonfigurovat prostředí PowerShell Azure zásobník uživatele](user/azure-stack-powershell-configure-user.md) tématu. 

## <a name="prerequisites"></a>Požadavky

Spusťte následující předpoklady, některý z [development kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), nebo ze systému Windows externí klienta Pokud jste [připojení prostřednictvím VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn): 

* Nainstalujte [modulů prostředí Azure PowerShell kompatibilní s Azure zásobníku](azure-stack-powershell-install.md).  
* Stažení [nástroje potřebné pro práci s Azure zásobníku](azure-stack-powershell-download.md).  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>Konfigurace prostředí operátor a přihlaste se k Azure zásobníku

Na základě typu nasazení (Azure AD ani AD FS), spusťte následující skript pro konfiguraci prostředí operátor zásobník Azure pomocí prostředí PowerShell (ujistěte se, zda jste nahradili AAD tenantName, GraphAudience koncový bod a hodnoty ArmEndpoint podle vašeho prostředí Konfigurace):

### <a name="azure-active-directory-aad-based-deployments"></a>Nasazení na bázi Azure Active Directory (AAD)
       
  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = “<Keyvault DNS suffix for your environment>”


  # Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint
    
  Set-AzureRmEnvironment `
    -Name "AzureStackAdmin" `
    -GraphAudience $GraphAudience 

  # Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
    -EnvironmentName "AzureStackAdmin"

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackAdmin" `
    -TenantId $TenantID 
  ```

### <a name="active-directory-federation-services-ad-fs-based-deployments"></a>Nasazení na základě služby Active Directory Federation Services (AD FS)
         
  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# For Azure Stack development kit, this value is adminvault.local.azurestack.external 
$KeyvaultDnsSuffix = “<Keyvault DNS suffix for your environment>”


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

Teď, když My jsme všechna nastavení, umožňuje pomocí prostředí PowerShell vytvořit prostředky v rámci Azure zásobníku. Můžete například vytvořit skupinu prostředků pro aplikace a přidat virtuální počítač. Chcete-li vytvořit skupinu prostředků s názvem "MyResourceGroup" použijte následující příkaz:

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Další kroky
* [Vývoj šablon pro Azure zásobníku](user/azure-stack-develop-templates.md)
* [Nasazení šablon pomocí PowerShellu](user/azure-stack-deploy-template-powershell.md)
