---
title: "Vytvoření účtu Azure Automation Spustit jako pomocí PowerShellu | Dokumentace Microsoftu"
description: "Tento článek popisuje, jak upgradovat váš účet Automation pomocí PowerShellu, abyste vytvořili účty Spustit jako, pokud jste tento krok neprovedli během počátečního vytváření z portálu."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/14/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: d34bb4a285f77f5c21a4af2d544bda3221d59e8d
ms.lasthandoff: 04/27/2017

---

# <a name="update-automation-run-as-account-using-powershell"></a>Aktualizace účtu Automation Spustit jako pomocí PowerShellu
Účet Automation můžete aktualizovat pomocí PowerShellu, pokud jste postupovali takto:

* Vytvořili jste účet Automation, ale odmítli jste vytvoření účtu Spustit jako.
* Už máte účet Automation pro správu prostředků Resource Manageru a chcete ho aktualizovat, aby zahrnoval účet Spustit jako pro ověřování runbooků.
* Už máte účet Automation pro správu klasických prostředků a chcete ho aktualizovat, abyste mohli použít účet Spustit jako pro Classic a nemuseli vytvářet nový účet a migrovat na něj runbooky a prostředky.   
* Rozhodli jste se vytvořit účet Spustit jako a účet Spustit jako pro Classic pomocí certifikátu, který vydala vaše podniková certifikační autorita.

## <a name="prerequisites"></a>Požadavky

* Tento skript je možné spustit jenom v systémech Windows 10 a Windows Server 2016 s nainstalovanými moduly Azure Resource Manageru verze 2.01 nebo novější. V předchozích verzích Windows není podporován.
* Azure PowerShell 1.0 nebo novější. Informace o vydání PowerShellu 1.0 najdete v článku [Postup instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).
* Účet Automation, na který se odkazuje jako na hodnotu parametrů *-AutomationAccountName* a *-ApplicationDisplayName* v následujícím skriptu PowerShellu.

Abyste získali hodnoty pro parametry *SubscriptionID*, *ResourceGroup* a *AutomationAccountName*, které jsou pro skripty povinné, postupujte takto:
1. Na webu Azure Portal v okně **Účet Automation** vyberte příslušný účet Automation a potom vyberte **Všechna nastavení**. 
2. V okně **Všechna nastavení** v části **Nastavení účtu** vyberte **Vlastnosti**. 
3. Hodnoty v okně **Vlastnosti** si poznamenejte.

![Podokno vlastností účtu Automation](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

## <a name="create-run-as-account-powershell-script"></a>Vytvoření powershellového skriptu pro účet Spustit jako
Tento skript PowerShellu zahrnuje podporu následujících konfigurací:

* Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem
* Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem
* Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu
* Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem v cloudu Azure Government

V závislosti na možnosti konfigurace, kterou vyberete, skript vytvoří následující položky.

**Pro účty Spustit jako:**

* Vytvoří aplikaci Azure AD, která se exportuje s veřejným klíčem certifikátu podepsaného svým držitelem nebo podnikového certifikátu, vytvoří účet instančního objektu pro tuto aplikaci v Azure AD a přiřadí roli přispěvatele pro tento účet v aktuálním předplatném. Toto nastavení můžete změnit na roli Vlastník nebo libovolnou jinou roli. Další informace najdete v tématu [Řízení přístupu na základě role ve službě Azure Automation](automation-role-based-access-control.md).
* V příslušném účtu Automation vytvoří prostředek certifikátu Automation s názvem *AzureRunAsCertificate*. Prostředek certifikátu obsahuje privátní klíč certifikátu, který používá aplikace Azure AD.
* V příslušném účtu Automation vytvoří prostředek připojení Automation s názvem *AzureRunAsConnection*. Prostředek připojení obsahuje parametry applicationId, tenantId, subscriptionId a certificate thumbprint.

**Pro účet Spustit jako pro Classic:**

* V příslušném účtu Automation vytvoří prostředek certifikátu Automation s názvem *AzureClassicRunAsCertificate*. Prostředek certifikátu obsahuje privátní klíč certifikátu, který používá certifikát pro správu.
* V příslušném účtu Automation vytvoří prostředek připojení Automation s názvem *AzureClassicRunAsConnection*. Prostředek propojení obsahuje název a ID předplatného a název prostředku certifikátu.

>[!NOTE]
> Pokud při vytváření účtu Spustit jako pro Classic vyberete libovolnou z těchto možností, po spuštění skriptu nahrajte veřejný certifikát (soubor s příponou .cer) do úložiště správy předplatného, ve kterém byl účet Automation vytvořený.
> 

1. Uložte následující skript do počítače. V tomto příkladu ho uložte s názvem *New-RunAsAccount.ps1*.

        #Requires -RunAsAdministrator
         Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory=$true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory=$true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$EnvironmentName="AzureCloud",

        [Parameter(Mandatory=$false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
        )

        function CreateSelfSignedCertificate([string] $keyVaultName, [string] $certificateName, [string] $selfSignedCertPlainPassword,
                                      [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
           -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
           -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired)

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
        }

        function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $CurrentDate = Get-Date
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $KeyId = (New-Guid).Guid

        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= [DateTime]$PfxCert.GetExpirationDateString()
        $KeyCredential.EndDate = $KeyCredential.EndDate.AddDays(-1)
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.CertValue  = $keyValue

        # Use key credentials and create an Azure AD application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
        Sleep -s 15
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           Sleep -s 10
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
           $Retries++;
        }
           return $Application.ApplicationId.ToString();
        }

        function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName,[string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force   
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
        }

        function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
        }

        Import-Module AzureRM.Profile
        Import-Module AzureRM.Resources

        $AzureRMProfileVersion= (Get-Module AzureRM.Profile).Version
        if (!(($AzureRMProfileVersion.Major -ge 2 -and $AzureRMProfileVersion.Minor -ge 1) -or ($AzureRMProfileVersion.Major -gt 2)))
        {
           Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
           return
        }

        Login-AzureRmAccount -EnvironmentName $EnvironmentName
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

        # Create a Run As account by using a service principal
        $CertifcateAssetName = "AzureRunAsCertificate"
        $ConnectionAssetName = "AzureRunAsConnection"
        $ConnectionTypeName = "AzureServicePrincipal"

        if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
        } else {
          $CertificateName = $AutomationAccountName+$CertifcateAssetName
          $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
          $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
          $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
          CreateSelfSignedCertificate $KeyVaultName $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create a service principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate the ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
            # Create a Run As account by using a service principal
            $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
            $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
            $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
            $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
                    "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                    "Then click Upload and upload the .cer format of #CERT#"

             if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
             $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
             $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
             $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        } else {
             $ClassicRunAsAccountCertificateName = $AutomationAccountName+$ClassicRunAsAccountCertifcateAssetName
             $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
             $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
             $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
             $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
             CreateSelfSignedCertificate $KeyVaultName $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. Na svém počítači na obrazovce **Start** spusťte **Windows PowerShell** se zvýšenými uživatelskými právy.
3. V prostředí příkazového řádku se zvýšenými oprávněními přejděte do složky, která obsahuje skript vytvořený v kroku 1.  
4. Spusťte tento skript s využitím hodnot parametrů pro požadovanou konfiguraci.

    **Vytvoření účtu Spustit v Azure jako pomocí certifikátu podepsaného svým držitelem**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    **Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    **Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí podnikového certifikátu**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    **Vytvoření účtu Spustit jako a účtu Spustit jako pro Classic pomocí certifikátu podepsaného svým držitelem v cloudu Azure Government**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > Po spuštění skriptu se zobrazí výzva k ověření pomocí Azure. Přihlaste se pomocí účtu, který je členem role správců předplatného a spolusprávcem předplatného.
    >
    >

Po úspěšném spuštění skriptu je třeba počítat s následujícím:
* Pokud jste vytvořili účet Spustit jako pro Classic s využitím veřejného certifikátu podepsaného svým držitelem (soubor .cer), skript ho vytvoří a uloží ve složce dočasných souborů ve vašem počítači pod profilem uživatele *%USERPROFILE%\AppData\Local\Temp*, který používáte ke spuštění relace PowerShellu.
* Pokud jste vytvořili účet Spustit jako pro Classic s využitím podnikového veřejného certifikátu (soubor .cer), použijte tento certifikát. Postupujte podle kroků pro [odeslání certifikátu rozhraní API pro správu na portál Azure Classic](../azure-api-management-certs.md) a potom použijte [ukázkový kód pro ověření pomocí prostředků nasazení Azure Classic](automation-verify-runas-authentication.md#classic-run-as-authentication) k ověření konfigurace přihlašovacích údajů pomocí prostředků nasazení Classic. 
* Pokud jste *nevytvořili* účet Spustit jako pro Classic, použijte k ověření pomocí prostředků Resource Manageru a ke kontrole konfigurace přihlašovacích údajů [ukázkový kód pro ověření s využitím prostředků správy služeb](automation-verify-runas-authentication.md#automation-run-as-authentication).

## <a name="next-steps"></a>Další kroky
* Další informace o objektech služby najdete v článku [Objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md).
* Další informace o certifikátech a službách Azure najdete v článku [Přehled certifikátů pro Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).

