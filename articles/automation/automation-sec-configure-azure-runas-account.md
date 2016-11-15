---
title: "Konfigurace účtu Spustit v Azure jako | Dokumentace Microsoftu"
description: "Kurz vás provede vytvořením, otestováním a ukázkovým použitím ověření objektu zabezpečení ve službě Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "název objektu služby, název instančního objektu, setspn, ověřování azure"
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 1e7529de2968b2745b42001cc16b54d70b2a5b86


---
# <a name="authenticate-runbooks-with-azure-run-as-account"></a>Ověření runbooků pomocí účtu Spustit v Azure jako
V tomto tématu se seznámíte se způsobem konfigurace účtu Automation na portálu Azure pomocí funkce účtu Spustit jako za účelem ověření prostředků, které spravují runbooky, buď v Azure Resource Manageru nebo ve službě Azure Service Management.

Když na portálu Azure vytvoříte nový účet Automation, účet automaticky vytvoří následující:

* Účet Spustit jako, který vytvoří nový objekt služby v Azure Active Directory (certifikát) a přiřadí přispěvateli řízení přístupu na základě rolí (RBAC), které se bude používat ke správě prostředků Resource Managera pomocí runbooků.   
* Účet Spustit jako pro Azure Classic tím, že odešle certifikát správy, který bude použit ke správě služby Azure Service Management nebo klasických prostředků pomocí runbooků.  

Tato funkce zjednodušuje proces a pomůže vám rychle začít vytvářet a nasazovat runbooky na podporu vašich automatizačních potřeb.      

Při používání účtu Spustit jako a účtu Spustit jako pro Azure Classic můžete následující:

* Zajistit standardizovaný způsob ověření pomocí Azure při správě prostředků Azure Resource Manageru nebo prostředků služby Azure Service Management pomocí runbooků na portálu Azure.  
* Automatizovat používání globálních runbooků nakonfigurovaných ve výstrahách Azure.

> [!NOTE]
> [Funkce integrace výstrah](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) Azure pomocí globálních runbooků Automation vyžaduje účet Automation, který je nakonfigurovaný pomocí účtů Spustit jako a Spustit jako pro Azure Classic. Můžete vybrat účet Automation, který už má definovaného uživatele účtů Spustit jako a Spustit jako pro Azure Classic, nebo můžete vytvořit nový.
> 
> 

Ukážeme vám, jak na portálu Azure vytvořit účet Automation, jak aktualizovat účet Automation pomocí PowerShellu a předvedeme vám ověření v runboocích.

Než se do toho pustíme, je tu několik věcí, které potřebujete pochopit a vzít do úvahy.

1. Toto neovlivní existující účty Automation, které už byly vytvořeny v klasickém modelu nasazení nebo v modelu nasazení Resource manageru.  
2. Bude to fungovat jenom na účty Automation, které vytvoříte na portálu Azure.  Pokus o vytvoření účtu na portálu Classic nebude úspěšný při replikaci konfigurace účtu Spustit jako.
3. Pokud v současnosti máte dříve vytvořené runbooky a assety (tj. plány, proměnné atd.) pro správu klasických prostředků a chcete, aby tyto runbooky ověřily nový účet Spustit jako pro Azure Classic, musíte je migrovat na nový účet Automation nebo stávající účet aktualizovat pomocí níže uvedeného skriptu PowerShellu.  
4. Abyste mohli ověřit nový účet Spustit jako a účet Spustit jako pro Azure Classic na účtu Automation, upravte svoje existující runbooky pomocí níže uvedeného ukázkového kódu.  **Všimněte si**, že účet Spustit jako slouží k ověřování v prostředcích Resource manageru pomocí certifikovaného objektu služby a účet Spustit jako pro Azure Classic slouží k ověřování v prostředcích služby správy pomocí certifikátu pro správu.     

## <a name="create-a-new-automation-account-from-the-azure-portal"></a>Vytvoření nového účtu Automation na portálu Azure
V této části provedete následující kroky a vytvoříte nový účet Azure Automation na portálu Azure.  Tím vytvoříte účet Spustit jako i účet Spustit jako pro Azure Classic.  

> [!NOTE]
> Uživatel provádějící tyto kroky *musí* být členem role správců předplatného a spolusprávcem předplatného, který uživatelům uděluje přístup k předplatnému.  Také je nutné přidat uživatele do výchozí služby Active Directory takového předplatného jako Uživatele. Účtu nemusíte přiřazovat privilegovanou roli.
> 
> 

1. Přihlaste se k portálu Azure pomocí účtu, který je členem role správců předplatného a spolusprávcem předplatného.
2. Vyberte **Účty Automation**.
3. V okně Účty Automation klikněte na **Přidat**.<br>![Přidání účtu Automation](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)
   
   > [!NOTE]
   > Pokud se v okně **Přidat účet Automation** zobrazí následující upozornění, důvodem je to, že váš účet není členem role správců předplatného a není spolusprávcem předplatného.<br>![Přidání upozornění do účtu Automation](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   > 
   > 
4. V okně **Přidat účet Automation** do pole **Název** zadejte název nového účtu Automation.
5. Pokud máte více než jedno předplatné, zadejte předplatné pro nový účet a také novou nebo existující **skupinu prostředků** a **umístění** datového centra Azure.
6. U možnosti **Vytvořit účet Spustit v Azure jako** zkontrolujte, jestli je vybraná položka **Ano**, a klikněte na tlačítko **Vytvořit**.  
   
   > [!NOTE]
   > Pokud se rozhodnete nevytvořit účet Spustit v Azure jako tím, že nevyberete možnost **Ne**, zobrazí se v okně **Přidání účtu Automation** zpráva upozornění.  Při vytváření účtu na portálu Azure nebude mít účet odpovídající identitu ověřování v rámci adresářové služby klasického předplatného nebo předplatného Resource Manageru a předplatné proto nebude mít přístup k prostředkům.  Všechny runbooky odkazující na tento účet kvůli tomu nebudou moct ověřit a provádět úlohy s prostředky v těchto modelech nasazení.
   > 
   > ![Přidání upozornění do účtu Automation](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)<br>
   > Pokud nevytvoříte objekt služby, nebude přiřazená role přispěvatele.
   > 
   > 
7. Zatímco Azure vytváří účet Automation, můžete průběh sledovat v nabídce v části **Oznámení**.

### <a name="resources-included"></a>Zahrnuté prostředky
Po úspěšném vytvoření účtu Automation se pro vaší potřebu automaticky vytvoří několik prostředků.  Následující tabulka shrnuje prostředky pro účet Spustit jako.<br>

| Prostředek | Popis |
| --- | --- |
| Runbook AzureAutomationTutorial |Ukázkový runbook PowerShellu, který předvádí ověření pomocí účtu Spustit jako a získává všechny prostředku Resource Manageru. |
| Runbook AzureAutomationTutorialScript |Ukázkový runbook PowerShellu, který předvádí ověření pomocí účtu Spustit jako a získává všechny prostředku Resource Manageru. |
| AzureRunAsCertificate |Asset certifikátu vytvořený automaticky během vytváření účtu Automation, nebo když jste použili níže uvedený powershellový skript pro existující účet.  Umožňuje ověření pomocí Azure, abyste mohli spravovat prostředky Azure Resource Manageru pomocí runbooků.  Tento certifikát má životnost jeden rok. |
| AzureRunAsConnection |Asset připojení vytvořený automaticky během vytváření účtu Automation, nebo když jste použili níže uvedený powershellový skript pro existující účet. |

Následující tabulka shrnuje prostředky pro účet Spustit jako pro Azure Classic.<br>

| Prostředek | Popis |
| --- | --- |
| Runbook AzureClassicAutomationTutorial |Ukázkový runbook, který získá všechny klasické virtuální počítače v rámci předplatného pomocí účtu Spustit jako pro Azure Classic (certifikát) a potom vypíše název a stav virtuálního počítače. |
| Runbook se skriptem AzureClassicAutomationTutorial |Ukázkový runbook, který získá všechny klasické virtuální počítače v rámci předplatného pomocí účtu Spustit jako pro Azure Classic (certifikát) a potom vypíše název a stav virtuálního počítače. |
| AzureClassicRunAsCertificate |Automaticky vytvořený asset certifikátu, který se používá k ověřování pomocí Azure, abyste mohli spravovat klasické prostředky Azure pomocí runbooků.  Tento certifikát má životnost jeden rok. |
| AzureClassicRunAsConnection |Automaticky vytvořený asset připojení, který se používá k ověřování pomocí Azure, abyste mohli spravovat klasické prostředky Azure pomocí runbooků. |

## <a name="verify-run-as-authentication"></a>Kontrola ověřování pomocí účtu Spustit jako
V dalším kroku provedeme malý test, abychom potvrdili, že můžete provádět ověření pomocí nového účtu Spustit jako.     

1. Na portálu Azure otevřete účet Automation, který jste vytvořili dříve.  
2. Kliknutím na dlaždici **Runbooky** otevřete seznam runbooků.
3. Vyberte runbook **AzureAutomationTutorialScript** a kliknutím na tlačítko **Spustit** runbook spusťte.  Zobrazí se výzva s dotazem, jestli chcete runbook spustit.
4. Vytvoří se [úloha runbooku](automation-runbook-execution.md), zobrazí se okno Úloha a na dlaždici **Souhrn úlohy** se zobrazí stav úlohy.  
5. Počáteční stav úlohy bude *Zařazeno ve frontě*. To označuje, že čekáte na zpřístupnění pracovního procesu runbooku v cloudu. Když pracovní proces úlohu přijme, změní se stav na *Spouštění*, a když se runbook skutečně spustí, změní se na *Spuštěno*.  
6. Po dokončení úlohy runbooku by se měla zobrazit zpráva **Dokončeno**.<br> ![Test runbooku objektu zabezpečení](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)<br>
7. Pokud chcete zobrazit podrobné výsledky runbooku, klikněte na dlaždici **Výstup**.
8. V okně **Výstup** byste měli vidět, že se úspěšně ověřil a vrátil seznam všech prostředků, které jsou ve skupině prostředků dostupné.
9. Zavřením okna **Výstup** se vrátíte do okna **Souhrn úlohy**.
10. Zavřete **Souhrn úlohy** a odpovídající okno runbooku **AzureAutomationTutorialScript**.

## <a name="verify-classic-run-as-authentication"></a>Kontrola ověřování pomocí účtu Spustit jako pro Azure Classic
V dalším kroku provedeme malý test, abychom potvrdili, že můžete provádět ověření pomocí nového účtu Spustit jako pro Azure Classic.     

1. Na portálu Azure otevřete účet Automation, který jste vytvořili dříve.  
2. Kliknutím na dlaždici **Runbooky** otevřete seznam runbooků.
3. Vyberte runbook **AzureClassicAutomationTutorialScript** a kliknutím na tlačítko **Spustit** runbook spusťte.  Zobrazí se výzva s dotazem, jestli chcete runbook spustit.
4. Vytvoří se [úloha runbooku](automation-runbook-execution.md), zobrazí se okno Úloha a na dlaždici **Souhrn úlohy** se zobrazí stav úlohy.  
5. Počáteční stav úlohy bude *Zařazeno ve frontě*. To označuje, že čekáte na zpřístupnění pracovního procesu runbooku v cloudu. Když pracovní proces úlohu přijme, změní se stav na *Spouštění*, a když se runbook skutečně spustí, změní se na *Spuštěno*.  
6. Po dokončení úlohy runbooku by se měla zobrazit zpráva **Dokončeno**.<br> ![Test runbooku objektu zabezpečení](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
7. Pokud chcete zobrazit podrobné výsledky runbooku, klikněte na dlaždici **Výstup**.
8. V okně **Výstup** byste měli vidět, že se úspěšně ověřil a vrátil seznam všech klasických virtuálních počítačů v rámci předplatného.
9. Zavřením okna **Výstup** se vrátíte do okna **Souhrn úlohy**.
10. Zavřete **Souhrn úlohy** a odpovídající okno runbooku **AzureClassicAutomationTutorialScript**.

## <a name="update-an-automation-account-using-powershell"></a>Aktualizace účtu Automation pomocí PowerShellu
Tady vám nabízíme možnost aktualizace účtu Automation pomocí PowerShellu v případě, že jste provedli následující:

1. Vytvořili jste účet Automation, ale odmítli jste vytvoření účtu Spustit jako.
2. Už máte účet Automation pro správu prostředků Resource manageru a chcete ho aktualizovat, aby zahrnoval účet Spustit jako pro ověřování runbooků.
3. Už máte účet Automation pro správu klasických prostředků a chcete ho aktualizovat, abyste mohli používat účet Spustit jako pro Azure Classic místo vytváření nového účtu a migrace runbooků a assetů na nový účet.   

Než budete pokračovat, zkontrolujte následující:

1. Stáhli a nainstalovali jste [Windows Management Framework (WMF) 4.0](https://www.microsoft.com/download/details.aspx?id=40855) – pokud používáte Windows 7.   
    Pokud používáte Windows Server 2012 R2, Windows Server 2012, Windows 2008 R2, Windows 8.1 nebo Windows 7 SP1, je k instalaci dostupný [Windows Management Framework 5.0](https://www.microsoft.com/download/details.aspx?id=50395).
2. Azure PowerShell 1.0. Informace o této verzi a její instalaci najdete v článku [Postup instalace a konfigurace Azure PowerShellu](../powershell-install-configure.md).
3. Vytvořili jste účet Automation.  Na tento účet se bude v obou níže uvedených skriptech odkazovat jako na hodnotu pro parametry -AutomationAccountName a -ApplicationDisplayName.

Pokud chcete získat hodnoty pro *SubscriptionID*, *ResourceGroup* a *AutomationAccountName*, které jsou požadovanými parametry skriptů, vyberte na portálu Azure v okně **Účet automatizace** svůj účet Automation a vyberte **Všechna nastavení**.  V okně **Všechna nastavení** v části **Nastavení účtu** vyberte **Vlastnosti**.  V okně **Vlastnosti** si můžete tyto hodnoty opsat.<br> ![Vlastnosti účtu Automation](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-run-as-account-powershell-script"></a>Vytvoření powershellového skriptu pro účet Spustit jako
Níže uvedený powershellový skript nakonfiguruje následující:

* Aplikaci Azure AD, která bude ověřena pomocí certifikátu podepsaného svým držitelem, vytvoření účtu objektu služby pro tuto aplikaci ve službě Azure AD a přiřazení role Přispěvatele (toto můžete změnit na roli Vlastníka nebo jinou roli) pro tento účet v aktuálním předplatném.  Další informace najdete v článku [Řízení přístupu na základě rolí ve službě Azure Automation](automation-role-based-access-control.md).
* Asset certifikátu Automation na určeném účtu Automation s názvem **AzureRunAsCertificate**, který obsahuje certifikát použitý objektem služby.
* Asset připojení Automation v určeném účtu Automation s názvem **AzureRunAsConnection**, který obsahuje applicationId, tenantId, subscriptionId a kryptografický otisk certifikátu.    

Následující postup vás provede procesem spuštění skriptu.

1. Uložte následující skript do počítače.  V tomto příkladu ho uložte s názvem **New- AzureServicePrincipal.ps1**.  
   
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
        [String] $CertPlainPassword,
   
        [Parameter(Mandatory=$false)]
        [int] $NoOfMonthsUntilExpired = 12
        )
   
        Login-AzureRmAccount
        Import-Module AzureRM.Resources
        Select-AzureRmSubscription -SubscriptionId $SubscriptionId
   
        $CurrentDate = Get-Date
        $EndDate = $CurrentDate.AddMonths($NoOfMonthsUntilExpired)
        $KeyId = (New-Guid).Guid
        $CertPath = Join-Path $env:TEMP ($ApplicationDisplayName + ".pfx")
   
        $Cert = New-SelfSignedCertificate -DnsName $ApplicationDisplayName -CertStoreLocation cert:\LocalMachine\My -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider"
   
        $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPath -Password $CertPassword -Force | Write-Verbose
   
        $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate -ArgumentList @($CertPath, $CertPlainPassword)
        $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())
   
        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= $EndDate
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.Type = "AsymmetricX509Cert"
        $KeyCredential.Usage = "Verify"
        $KeyCredential.Value = $KeyValue
   
        # Use Key credentials
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $ApplicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $keyCredential
   
        New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId | Write-Verbose
        Get-AzureRmADServicePrincipal | Where {$_.ApplicationId -eq $Application.ApplicationId} | Write-Verbose
   
        $NewRole = $null
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
           Sleep 5
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           Sleep 10
           $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
           $Retries++;
        }
   
        # Get the tenant id for this subscription
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
   
        # Create the automation resources
        New-AzureRmAutomationCertificate -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Path $CertPath -Name AzureRunAsCertificate -Password $CertPassword -Exportable | write-verbose
   
        # Create a Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        $ConnectionAssetName = "AzureRunAsConnection"
        Remove-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -Force -ErrorAction SilentlyContinue
        $ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId}
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues
2. Na svém počítači na obrazovce **Start** spusťte **Windows PowerShell** se zvýšenými uživatelskými právy.
3. Z prostředí příkazového řádku PowerShellu se zvýšenými právy přejděte do složky, který obsahuje skript vytvořený v kroku 1, a spusťte tento skript změnou hodnoty parametrů *-ResourceGroup*, *-AutomationAccountName*, *-ApplicationDisplayName*, *-SubscriptionId* a *-CertPlainPassword*.<br>
   
   > [!NOTE]
   > Po spuštění skriptu budete vyzváni k ověření pomocí Azure. Musíte se přihlásit k portálu Azure pomocí účtu, který je členem role správců předplatného a spolusprávcem předplatného.
   > 
   > 
   
        .\New-AzureServicePrincipal.ps1 -ResourceGroup <ResourceGroupName>
        -AutomationAccountName <NameofAutomationAccount> `
        -ApplicationDisplayName <DisplayNameofAutomationAccount> `
        -SubscriptionId <SubscriptionId> `
        -CertPlainPassword "<StrongPassword>"  
   <br>

Po úspěšném dokončení skriptu použijte níže uvedený [ukázkový kód](#sample-code-to-authenticate-with-resource-manager-resources) k ověření pomocí prostředků Resource manageru a ke kontrole konfigurace přihlašovacích údajů.

### <a name="create-classic-run-as-account-powershell-script"></a>Vytvoření powershellového skriptu pro účet Spustit jako pro Azure Classic
Níže uvedený powershellový skript nakonfiguruje následující:

* Asset certifikátu Automation na určeném účtu Automation s názvem **AzureClassicRunAsCertificate**, který obsahuje certifikát použitý k ověření vašich runbooků.
* Asset připojení Automation v určeném účtu Automation s názvem **AzureClassicRunAsConnection**, který obsahuje název předplatného, subscriptionId a název assetu certifikátu.

Skript vytvoří certifikát pro správu podepsaný svým držitelem a uloží ho do složky dočasných souborů v počítači v rámci profilu uživatele, který používáte ke spouštění relace prostředí PowerShellu – *%USERPROFILE%\AppData\Local\Temp*.  Po spuštění skriptu budete muset odeslat certifikát pro správu Azure do úložiště správy předplatného, ve kterém byl účet Automation vytvořený.  Následující postup vás provede procesem spuštění skriptu a odesláním certifikátu.  

1. Uložte následující skript do počítače.  V tomto příkladu ho uložte s názvem **New-AzureClassicRunAsAccount.ps1**.
   
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
        [String] $CertPlainPassword,
   
        [Parameter(Mandatory=$false)]
        [int] $NoOfMonthsUntilExpired = 12
        )
   
        Login-AzureRmAccount
        Import-Module AzureRM.Resources
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId
        $SubscriptionName = $subscription.Subscription.SubscriptionName
   
        $CurrentDate = Get-Date
        $EndDate = $CurrentDate.AddMonths($NoOfMonthsUntilExpired)
        $KeyId = (New-Guid).Guid
        $CertPath = Join-Path $env:TEMP ($ApplicationDisplayName + ".pfx")
        $CertPathCer = Join-Path $env:TEMP ($ApplicationDisplayName + ".cer")
   
        $Cert = New-SelfSignedCertificate -DnsName $ApplicationDisplayName -CertStoreLocation cert:\LocalMachine\My -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider"
   
        $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPathCer -Type CERT | Write-Verbose
   
        # Create the automation resources
        $ClassicCertificateAssetName = "AzureClassicRunAsCertificate"
        New-AzureRmAutomationCertificate -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Path $CertPath -Name $ClassicCertificateAssetName  -Password $CertPassword -Exportable | write-verbose
   
        # Create a Automation connection asset named AzureClassicRunAsConnection in the Automation account. This connection uses the ClassicCertificateAssetName.
        $ConnectionAssetName = "AzureClassicRunAsConnection"
        Remove-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -Force -ErrorAction SilentlyContinue
        $ConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicCertificateAssetName}
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureClassicCertificate -ConnectionFieldValues $ConnectionFieldValues
   
        Write-Host -ForegroundColor red "Please upload the cert $CertPathCer to the Management store by following the steps below."
        Write-Host -ForegroundColor red "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates."
        Write-Host -ForegroundColor red "Then click Upload and upload the certificate $CertPathCer"
2. Na svém počítači na obrazovce **Start** spusťte **Windows PowerShell** se zvýšenými uživatelskými právy.  
3. Z prostředí příkazového řádku PowerShellu se zvýšenými právy přejděte do složky, který obsahuje skript vytvořený v kroku 1, a spusťte tento skript změnou hodnoty parametrů *-ResourceGroup*, *-AutomationAccountName*, *-ApplicationDisplayName*, *-SubscriptionId* a *-CertPlainPassword*.<br>
   
   > [!NOTE]
   > Po spuštění skriptu budete vyzváni k ověření pomocí Azure. Musíte se přihlásit k portálu Azure pomocí účtu, který je členem role správců předplatného a spolusprávcem předplatného.
   > 
   > 
   
        .\New-AzureClassicRunAsAccount.ps1 -ResourceGroup <ResourceGroupName>
        -AutomationAccountName <NameofAutomationAccount> `
        -ApplicationDisplayName <DisplayNameofAutomationAccount> `
        -SubscriptionId <SubscriptionId> `
        -CertPlainPassword "<StrongPassword>"

Po úspěšném dokončení skriptu zkopírujte vytvořený certifikát do složky **Temp** v profilu uživatele.  Postupujte podle kroků pro [odeslání certifikátu rozhraní API pro správu](../azure-api-management-certs.md) na portál Azure Classic a potom použijte [ukázkový kód](#sample-code-to-authenticate-with-service-management-resources) k ověření konfigurace přihlašovacích údajů pomocí prostředků správy služeb.

## <a name="sample-code-to-authenticate-with-resource-manager-resources"></a>Ukázkový kód pro ověření pomocí prostředků Resource Manageru
Můžete použít níže uvedený aktualizovaný ukázkový kód z ukázkového runbooku **AzureAutomationTutorialScript** a provést ověření pomocí účtu Spustit jako, abyste mohli prostředky Resource Manageru spravovat pomocí svých runbooků.   

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get the connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

       "Logging in to Azure..."
       Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
       "Setting context to a specific subscription"     
       Set-AzureRmContext -SubscriptionId $SubId              
    }
    catch {
        if (!$servicePrincipalConnection)
        {
           $ErrorMessage = "Connection $connectionName not found."
           throw $ErrorMessage
         } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
         }
    }


Skript obsahuje dva další řádky kódu, které podporují odkazování na kontext předplatného, abyste mohli lépe pracovat s několika předplatnými. Proměnný asset s názvem SubscriptionId obsahuje ID předplatného a po příkazu rutiny Add-AzureRmAccount bude [rutina Set-AzureRmContext](https://msdn.microsoft.com/library/mt619263.aspx) uvedená v sadě parametrů *-SubscriptionId*. Pokud je název proměnné příliš obecný, můžete ho upravit přidáním předpony nebo pomocí jiné zásady vytváření názvů, abyste proměnnou dokázali lépe identifikovat. Alternativně můžete místo -SubscriptionId použít sadu parametrů -SubscriptionName s odpovídajícím proměnným assetem.  

Všimněte si, že rutina používaná pro ověřování v runbooku – **Add-AzureRmAccount**, používá sadu parametrů *ServicePrincipalCertificate*.  Ověřování provádí pomocí certifikátu objektu služby a ne pomocí přihlašovacích údajů.  

## <a name="sample-code-to-authenticate-with-service-management-resources"></a>Ukázkový kód pro ověření pomocí prostředků správy služby
Můžete použít níže uvedený aktualizovaný ukázkový kód z ukázkového runbooku **AzureClassicAutomationTutorialScript** a provést ověření pomocí účtu Spustit jako pro Azure Classic, abyste mohli klasické prostředky spravovat pomocí svých runbooků.

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get the connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate to Azure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in the Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting the certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in the Automation account."
    }

    Write-Verbose "Authenticating to Azure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID


## <a name="next-steps"></a>Další kroky
* Další informace o objektech služby najdete v článku [Objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md).
* Další informace o řízení přístupu na základě rolí ve službě Azure Automation najdete v článku [Řízení přístupu na základě rolí ve službě Azure Automation](automation-role-based-access-control.md).
* Další informace o certifikátech a službách Azure najdete v článku [Přehled certifikátů pro Azure Cloud Services](../cloud-services/cloud-services-certs-create.md)




<!--HONumber=Nov16_HO2-->


