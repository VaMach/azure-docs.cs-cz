<properties
    pageTitle="Konfigurace účtu Spustit v Azure jako | Microsoft Azure"
    description="Kurz vás provede vytvořením, otestováním a ukázkovým použitím ověření objektu zabezpečení ve službě Azure Automation."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="service principal name, setspn, azure authentication"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/07/2016"
    ms.author="magoedte"/>

# Ověření runbooků pomocí účtu Spustit v Azure jako
V tomto tématu se seznámíte se způsobem konfigurace účtu Automation na portálu Azure pomocí nové funkce účtu Spustit jako (označované také jako objekt služby) za účelem získání přístupu k prostředkům správce Azure Resource Manager ve svém předplatném pomocí runbooků Automation.  Když vytvoříte nový účet Automation na portálu Azure, účet ve výchozím nastavení ve vašem předplatném automaticky vytvoří nový objekt služby a přispěvateli přiřazenou roli řízení přístupu na základě rolí (RBAC).  Tato funkce zjednodušuje proces a pomůže vám rychle začít vytvářet a nasazovat runbooky na podporu vašich automatizačních potřeb.      

Při používání objektu služby můžete následující:

* Zajištění standardizovaného způsobu ověření pomocí Azure při správě správce prostředků Azure Resource Manager pomocí runbooků
* Automatizace používání globálních runbooků nakonfigurovaných ve výstrahách Azure


>[AZURE.NOTE] [Funkce integrace výstrah](../azure-portal/insights-receive-alert-notifications.md) Azure pomocí globálních runbooků Automation vyžaduje účet Automation, který je nakonfigurovaný pomocí objektu služby. Můžete vybrat účet Automation, který už má definovaného uživatele objektu služby, nebo můžete vytvořit nový.



Ukážeme vám, jak vytvořit účet Automation na portálu Azure i prostřednictvím aktualizace účtu pomocí účtu Spustit jako (využijeme Azure PowerShell) a jak pomocí tohoto objektu služby ve vašich runboocích ověřovat.  

## Vytvoření nového účtu Automation na portálu Azure
V této části provedete následující kroky a vytvoříte nový účet a objekt služby Azure Automation na portálu Azure.

>[AZURE.NOTE] Uživatel, který tyto provádí, *musí* být členem role Správci předplatného.

1. Přihlaste se k portálu Azure jako správce služby pro předplatné Azure, které chcete spravovat.
2. Vyberte **Účty Automation**.
3. V okně Účty Automation klikněte na **Přidat**.<br>![Přidání účtu Automation](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties.png)
4. V okně **Přidat účet Automation** do pole **Název** zadejte název nového účtu Automation.
5. Pokud máte více než jedno předplatné, zadejte předplatné pro nový účet a také novou nebo existující **skupinu prostředků** a **umístění** datového centra Azure.
6. U možnosti **Vytvořit účet Spustit v Azure jako** zkontrolujte, jestli je vybraná položka **Ano**, a klikněte na tlačítko **Vytvořit**.  

    ![Přidání upozornění k účtu Automation](media/automation-sec-configure-azure-runas-account/add-account-decline-create-runas-msg.png)

    >[AZURE.NOTE] Pokud se rozhodnete nevytvořit účet Spustit v Azure jako tím, že nevyberete možnost **Ne**, zobrazí se v okně **Přidání účtu Automation** zpráva upozornění.  Při vytváření účtu a jeho přiřazování k roli **přispěvatele** v předplatném, nebude mít účet odpovídající identitu ověřování v rámci adresářové služby vašeho předplatného, a proto nebude předplatné obsahovat žádné přístupové prostředky.  Všechny runbooky odkazující na tento účet kvůli tomu nebudou moct ověřit a provádět úlohy s prostředky Azure Resource Manager.

    ![Přidání upozornění k účtu Automation](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties-error.png)

    >[AZURE.NOTE] Pokud se po kliknutí na tlačítko **Vytvořit** zobrazí chybová zpráva o odepření oprávnění, znamená to, že váš účet není členem role Správce předplatného.  

7. Zatímco Azure vytváří účet Automation, můžete průběh sledovat v nabídce v části **Oznámení**.

### Zahrnuté prostředky
Po vytvoření účtu Automation bude pro vaši potřebu automaticky vytvořeno několik prostředků. Ty jsou uvedené v následující tabulce.

Prostředek|Popis 
----|----
Runbook AzureAutomationTutorial|Ukázkový runbook, který předvádí ověření pomocí účtu Spustit jako a zobrazuje prvních 10 virtuálních počítačů Azure v rámci vašeho předplatného.
AzureRunAsCertificate|Asset certifikátu, který byl vytvořen, když jste si nechali vytvořit účet Spustit jako během vytváření účtu Automation, nebo když jste použili níže uvedený powershellový skript pro existující účet.  Tento certifikát má životnost jeden rok. 
AzureRunAsConnection|Asset připojení, který byl vytvořen, když jste si nechali vytvořit účet Spustit jako během vytváření účtu Automation, nebo když jste použili níže uvedený powershellový skript pro existující účet.
Moduly|15 modulů s rutinami pro Azure, PowerShell a Automation, se kterými můžete svoje runbooky začít okamžitě používat.  

## Aktualizace účtu Automation pomocí PowerShellu
Následující postup aktualizuje existující účet Automation a pomocí PowerShellu vytvoří objekt služby.  Tento postup je nutný, pokud jste vytvořili účet, ale odmítli vytvoření účtu Spustit jako.

Než budete pokračovat, zkontrolujte následující:

1. Stáhli a nainstalovali jste [Windows Management Framework (WMF) 4.0](https://www.microsoft.com/download/details.aspx?id=40855) – pokud používáte Windows 7.   
    Pokud používáte Windows Server 2012 R2, Windows Server 2012, Windows 2008 R2, Windows 8.1 nebo Windows 7 SP1, je k instalaci dostupný [Windows Management Framework 5.0](https://www.microsoft.com/download/details.aspx?id=50395).
2. Azure PowerShell 1.0. Informace o této verzi a její instalaci najdete v článku [Postup instalace a konfigurace Azure PowerShellu](../powershell-install-configure.md). 
3. Vytvořili jste účet Automation.  Na tento účet se bude v níže uvedeném skriptu odkazovat jako na hodnotu pro parametry - AutomationAccountName a -ApplicationDisplayName.


Powershellový skript nakonfiguruje následující:

* Aplikaci Azure AD, která bude ověřena pomocí certifikátu podepsaného svým držitelem, vytvoření účtu objektu služby pro tuto aplikaci ve službě Azure AD a přiřazení role Přispěvatele (toto můžete změnit na roli Vlastníka nebo jinou roli) pro tento účet v aktuálním předplatném.  Další informace najdete v článku [Řízení přístupu na základě rolí ve službě Azure Automation](../automation/automation-role-based-access-control.md).  
* Asset certifikátu Automation na určeném účtu Automation s názvem **AzureRunAsCertificate**, který obsahuje certifikát použitý v objektu služby.
* Asset připojení Automation v určeném účtu Automation s názvem **AzureRunAsConnection**, který obsahuje applicationId, tenantId, subscriptionId a kryptografický otisk certifikátu.  


### Spuštění powershellového skriptu

1. Uložte následující skript do počítače.  V tomto příkladu ho uložte s názvem **New- AzureServicePrincipal.ps1**.  

    ```
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
    While ($NewRole -eq $null -and $Retries -le 2)
    {
      # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
      Sleep 5
      New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
      Sleep 5
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
    $ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId.SubscriptionId}
    New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues
    ```
<br>
2. Na svém počítači na obrazovce **Start** spusťte **Windows PowerShell** se zvýšenými uživatelskými právy.
3. Z prostředí příkazového řádku PowerShellu se zvýšenými právy přejděte do složky, který obsahuje skript vytvořený v kroku 1, a spusťte tento skript změnou hodnoty parametrů *-ResourceGroup*, *-AutomationAccountName*, *-ApplicationDisplayName*, *-SubscriptionId* a *-CertPlainPassword*.<br>

    ```
    .\New-AzureServicePrincipal.ps1 -ResourceGroup <ResourceGroupName> 
     -AutomationAccountName <NameofAutomationAccount> `
     -ApplicationDisplayName <DisplayNameofAutomationAccount> `
     -SubscriptionId <SubscriptionId> `
     -CertPlainPassword "<StrongPassword>"
    ```   
<br>

    >[AZURE.NOTE] Po spuštění skriptu budete vyzváni k ověření pomocí Azure.  *Musíte* být přihlášeni pomocí účtu, který je v předplatném správcem služby.  
<br>
4. Po úspěšném dokončení skriptu pokračujte do další části, kde otestujete a ověříte konfiguraci nových přihlašovacích údajů.

### Kontrola ověřování
V dalším kroku provedeme malý test, abychom potvrdili, že můžete provádět ověření pomocí nového objektu služby. Pokud ověření neproběhne úspěšně, vraťte se ke kroku 1 a znovu potvrďte všechny předchozí kroky.    

1. Na portálu Azure otevřete účet Automation, který jste vytvořili dříve.  
2. Kliknutím na dlaždici **Runbooky** otevřete seznam runbooků.
3. Kliknutím na tlačítko **Přidat runbook** a potom výběrem možnosti **Vytvořit nový runbook** v okně **Přidání runbooku**vytvořte nový runbook.
4. Runbook pojmenujte *Test-SecPrin-Runbook* a jako **Typ runbooku** vyberte PowerShell.  Kliknutím na **Vytvořit** runbook vytvořte.
5. V okně **Úprava powershellového runbooku** vložte na plátno následující kód:<br>

    ```
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
     -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    ```  
<br>
6. Kliknutím na **Uložit** runbook uložte.
7. Kliknutím na **Testovací podokno** otevřete okno **Test**.
8. Kliknutím na **Spustit** spustíte test.
9. Vytvoří se [úloha runbooku](automation-runbook-execution.md) a její stav se zobrazí v podokně.  
10. Počáteční stav úlohy bude *Zařazeno ve frontě*. To označuje, že čekáte na zpřístupnění pracovního procesu runbooku v cloudu. Když pracovní proces úlohu přijme, změní se stav na *Spouštění*, a když se runbook skutečně spustí, změní se na *Spuštěno*.  
11. Po dokončení úlohy runbooku se zobrazí jeho výstup. V našem případě bychom měli vidět stav **Dokončeno**.<br> ![Test runbooku objektu zabezpečení](media/automation-sec-configure-azure-runas-account/runbook-test-results.png)<br>
12. Zavřením okna **Test** se vraťte se na plátno.
13. Zavřete okno **Úprava powershellového runbooku**.
14. Zavřete okno **Test-SecPrin-Runbook**.

## Ukázkový kód pro ověření pomocí prostředků Resource Manageru

Můžete použít níže uvedený aktualizovaný ukázkový kód z ukázkového runbooku AzureAutomationTutorial a provést ověření pomocí účtu Spustit jako, abyste mohli prostředky Resource Manageru spravovat pomocí svých runbooků. 

   ```
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
   ```

Skript obsahuje dva další řádky kódu, které podporují odkazování na kontext předplatného, abyste mohli lépe pracovat s několika předplatnými. Proměnný asset s názvem SubscriptionId obsahuje ID předplatného a po příkazu rutiny Add-AzureRmAccount bude [rutina Set-AzureRmContext](https://msdn.microsoft.com/library/mt619263.aspx) uvedená v sadě parametrů *-SubscriptionId*. Pokud je název proměnné příliš obecný, můžete ho upravit přidáním předpony nebo pomocí jiné zásady vytváření názvů, abyste proměnnou dokázali lépe identifikovat. Alternativně můžete místo -SubscriptionId použít sadu parametrů -SubscriptionName s odpovídajícím proměnným assetem.  

## Další kroky
- Další informace o objektech služby najdete v článku [Objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md).
- Další informace o řízení přístupu na základě rolí ve službě Azure Automation najdete v článku [Řízení přístupu na základě rolí ve službě Azure Automation](../automation/automation-role-based-access-control.md).



<!--HONumber=Jun16_HO2-->


