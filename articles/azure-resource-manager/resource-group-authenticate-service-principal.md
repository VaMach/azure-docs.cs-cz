---
title: "Vytvoření identity pro aplikaci Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Popisuje, jak používat prostředí Azure PowerShell k vytvoření aplikace Azure Active Directory a objektu zabezpečení a jí udělit přístup k prostředkům prostřednictvím řízení přístupu na základě rolí. Ukazuje, jak k ověření aplikace pomocí hesla nebo certifikátu."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/28/2017
ms.author: tomfitz
ms.openlocfilehash: df5d705a4451950110c11b7d468bf7d59e5474d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a>Vytvoření instančního objektu pro přístup k prostředkům pomocí prostředí Azure PowerShell

Pokud máte aplikace nebo skriptu, která potřebuje přístup k prostředkům, můžete nastavit identity aplikace a ověřit aplikaci s svoje vlastní přihlašovací údaje. Tato identita se označuje jako hlavní název služby. Tento přístup umožňuje:

* Přiřadíte oprávnění k identitě aplikace, která se liší od vlastní oprávnění. Tato oprávnění jsou obvykle omezené na přesně co aplikaci je třeba provést.
* Při provádění bezobslužného skriptu, použijte certifikát pro ověřování.

Toto téma ukazuje, jak používat [prostředí Azure PowerShell](/powershell/azure/overview) nastavit všechno, co potřebujete pro spuštění pod svou vlastní pověření a identity aplikace.

## <a name="required-permissions"></a>Požadovaná oprávnění
K dokončení tohoto tématu, musíte mít dostatečná oprávnění v Azure Active Directory a vašeho předplatného Azure. Konkrétně musí být schopná vytvořit aplikaci ve službě Azure Active Directory a přiřazení objektu služby roli. 

Nejjednodušším způsobem, jak zkontrolovat, jestli má váš účet dostatečná oprávnění, je použít k tomu portál. V tématu [zkontrolujte požadované oprávnění](resource-group-create-service-principal-portal.md#required-permissions).

Nyní přejděte k části k ověřování:

* [heslo](#create-service-principal-with-password)
* [certifikát podepsaný svým držitelem](#create-service-principal-with-self-signed-certificate)
* [certifikát od certifikační autority](#create-service-principal-with-certificate-from-certificate-authority)

## <a name="powershell-commands"></a>Příkazy prostředí PowerShell

Chcete-li nastavit hlavní název služby, použijte:

| Příkaz | Popis |
| ------- | ----------- | 
| [Nové AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | Vytvoří objekt služby Azure Active Directory |
| [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) | Zadaná role RBAC přiřadí objekt zadaný v zadaném oboru. |


## <a name="create-service-principal-with-password"></a>Vytvoření instančního objektu s heslem

Chcete-li vytvořit objekt služby k roli Přispěvatel pro vaše předplatné, použijte: 

```powershell
Login-AzureRmAccount
$sp = New-AzureRmADServicePrincipal -DisplayName exampleapp -Password "{provide-password}"
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

V příkladu v režimu spánku 20 sekund umožňující chvíli pro novou službu hlavní rozšíří v rámci Azure Active Directory. Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: Hlavní {ID} neexistuje v adresáři."

Následující skript umožňuje určit obor než výchozí předplatné a opakuje přiřazení role, pokud dojde k chybě:

```powershell
Param (

 # Use to set scope to resource group. If no value is provided, scope is set to subscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use to set subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $Password
)

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 
 # Create Service Principal for the AD app
 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
```

Několik položek si uvědomit o skriptu:

* K udělení přístupu identity k výchozí předplatné, není potřeba zadat parametry ResourceGroup nebo ID předplatného.
* Zadejte parametr ResourceGroup pouze v případě, že chcete omezit rozsah přiřazení role do skupiny prostředků.
*  V tomto příkladu přidáte objektu služby roli Přispěvatel. Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md).
* Skript v režimu spánku 15 sekund umožňující chvíli pro novou službu hlavní rozšíří v rámci Azure Active Directory. Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: Hlavní {ID} neexistuje v adresáři."
* Pokud potřebujete udělit přístup k hlavní službě pro další skupiny prostředků nebo předplatných, spusťte `New-AzureRMRoleAssignment` rutinu znovu s různými obory.


### <a name="provide-credentials-through-powershell"></a>Zadejte přihlašovací údaje pomocí prostředí PowerShell
Nyní budete muset přihlásit jako aplikace k provádění operací. Uživatelské jméno, použijte `ApplicationId` kterou jste vytvořili pro aplikaci. Heslo použijte ten, který jste zadali při vytváření účtu. 

```powershell   
$creds = Get-Credential
Login-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId {tenant-ID}
```

ID klienta nerozlišuje malá písmena, takže jej můžete vložit přímo ve vašem skriptu. Pokud potřebujete zjistit ID klienta, použijte:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

## <a name="create-service-principal-with-self-signed-certificate"></a>Vytvoření instančního objektu se certifikát podepsaný svým držitelem

Chcete-li vytvořit objekt služby se certifikát podepsaný svým držitelem a role Přispěvatel pro vaše předplatné, použijte: 

```powershell
Login-AzureRmAccount
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

V příkladu v režimu spánku 20 sekund umožňující chvíli pro novou službu hlavní rozšíří v rámci Azure Active Directory. Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: Hlavní {ID} neexistuje v adresáři."

Následující skript umožňuje určit obor než výchozí předplatné a opakuje přiřazení role, pokud dojde k chybě. Pokud nemáte Azure PowerShell 2.0 na Windows 10 nebo Windows Server 2016.

```powershell
Param (

 # Use to set scope to resource group. If no value is provided, scope is set to subscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use to set subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName
 )

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
 $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
```

Několik položek si uvědomit o skriptu:

* K udělení přístupu identity k výchozí předplatné, není potřeba zadat parametry ResourceGroup nebo ID předplatného.
* Zadejte parametr ResourceGroup pouze v případě, že chcete omezit rozsah přiřazení role do skupiny prostředků.
* V tomto příkladu přidáte objektu služby roli Přispěvatel. Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md).
* Skript v režimu spánku 15 sekund umožňující chvíli pro novou službu hlavní rozšíří v rámci Azure Active Directory. Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: Hlavní {ID} neexistuje v adresáři."
* Pokud potřebujete udělit přístup k hlavní službě pro další skupiny prostředků nebo předplatných, spusťte `New-AzureRMRoleAssignment` rutinu znovu s různými obory.

Pokud jste **nemají Windows 10 nebo Windows Server 2016 Technical Preview**, budete muset stáhnout [certifikát podepsaný svým držitelem generátor](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) z webu Microsoft Script Center. Rozbalte obsah a importovat rutiny, které potřebujete.

```powershell  
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```
  
Ve skriptu nahraďte následující dva řádky, které se vygeneruje certifikát.
  
```powershell
New-SelfSignedCertificateEx  -StoreLocation CurrentUser -StoreName My -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Zadejte certifikát pomocí automatizované skript prostředí PowerShell
Vždy, když se přihlásíte jako hlavní název služby, je třeba zadat ID klienta adresáře pro vaši aplikaci AD. Klient je instance služby Azure Active Directory. Pokud máte pouze jedno předplatné, můžete použít:

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertSubject,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -match $CertSubject }).Thumbprint
 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

ID aplikace a ID klienta nejsou písmena, takže je můžete vložit přímo ve vašem skriptu. Pokud potřebujete zjistit ID klienta, použijte:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Pokud potřebujete zjistit ID aplikace, použijte:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>Vytvoření instančního objektu pomocí certifikátu od certifikační autority
Pokud chcete použít certifikát vydaný certifikační autority vytvoření instančního objektu, použijte následující skript:

```powershell
Param (
 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword
 )

 Login-AzureRmAccount
 Import-Module AzureRM.Resources
 Set-AzureRmContext -SubscriptionId $SubscriptionId
 
 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force

 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName
 New-AzureRmADSpCredential -ObjectId $ServicePrincipal.Id -CertValue $KeyValue -StartDate $PFXCert.NotBefore -EndDate $PFXCert.NotAfter
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

Několik položek si uvědomit o skriptu:

* Přístup je vymezen k předplatnému.
* V tomto příkladu přidáte objektu služby roli Přispěvatel. Jiné role, naleznete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md).
* Skript v režimu spánku 15 sekund umožňující chvíli pro novou službu hlavní rozšíří v rámci Azure Active Directory. Pokud váš skript nečeká dost dlouho, zobrazí chybová zpráva: "PrincipalNotFound: Hlavní {ID} neexistuje v adresáři."
* Pokud potřebujete udělit přístup k hlavní službě pro další skupiny prostředků nebo předplatných, spusťte `New-AzureRMRoleAssignment` rutinu znovu s různými obory.

### <a name="provide-certificate-through-automated-powershell-script"></a>Zadejte certifikát pomocí automatizované skript prostředí PowerShell
Vždy, když se přihlásíte jako hlavní název služby, je třeba zadat ID klienta adresáře pro vaši aplikaci AD. Klient je instance služby Azure Active Directory.

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

ID aplikace a ID klienta nejsou písmena, takže je můžete vložit přímo ve vašem skriptu. Pokud potřebujete zjistit ID klienta, použijte:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Pokud potřebujete zjistit ID aplikace, použijte:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a>Změnit pověření

Můžete změnit pověření pro aplikaci AD, buď kvůli ohrožení zabezpečení nebo vypršení platnosti pověření [odebrat AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) a [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) rutiny.

Chcete-li odebrat všechny přihlašovací údaje pro aplikaci, použijte:

```powershell
Remove-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -All
```

Chcete-li přidat heslo, použijte:

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -Password p@ssword!
```

Přidání certifikátu hodnoty, vytvořte certifikát podepsaný svým držitelem, jak je znázorněno v tomto tématu. Pak použijte:

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
```

## <a name="save-access-token-to-simplify-log-in"></a>Uložit přístupový token pro zjednodušení přihlášení
Abyste se vyhnuli poskytování pověření hlavní služby pokaždé, když se musí se přihlásit, můžete uložit přístupový token.

Pokud chcete používat aktuální přístupový token v jiné relaci, uložení profilu.
   
```powershell
Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```
   
Otevřete profil a zkontrolujte jeho obsah. Všimněte si, že obsahuje přístupový token. Místo ručně přihlášení znovu načíst profil.
   
```powershell
Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```

> [!NOTE]
> Vyprší platnost přístupového tokenu, takže pomocí uloženého profilu funguje výhradně u tak dlouho, dokud token je platný.
>  

Alternativně můžete vyvolat operace REST z prostředí PowerShell k přihlášení. Z odpovědi ověřování může získat přístupový token pro použití s dalšími operacemi. Příklad získání tokenu přístupu vyvoláním operace REST, naleznete v části [generování tokenu přístupu](resource-manager-rest-api.md#generating-an-access-token).

## <a name="debug"></a>Ladění

Při vytváření objektu služby se můžete setkat s následujícími chybami:

* **"Authentication_Unauthorized"** nebo **"žádné předplatné nalezena v kontextu."** -Se zobrazí tato chyba, pokud váš účet nemá [požadovaná oprávnění](#required-permissions) v Azure Active Directory pro registraci aplikace. Obvykle se zobrazí tato chyba při jenom správci ve vašem Azure Active Directory můžete zaregistrovat aplikace a váš účet není správce. Požádejte správce buď můžete přiřadit role správce nebo umožňuje uživatelům registrovat aplikace.

* Váš účet **"nemá oprávnění k provedení akce 'Microsoft.Authorization/roleAssignments/write' u rozsahu: /subscriptions/ {guid}'."**  -Zobrazí tato chyba, pokud se váš účet nemá dostatečná oprávnění k přiřazení role identity. Požádejte správce předplatného můžete přidat do role správce přístupu uživatelů.

## <a name="sample-applications"></a>Ukázkové aplikace
Informace o protokolování jako aplikace prostřednictvím různých platforem najdete v tématu:

* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Další kroky
* Podrobné pokyny k integraci aplikace do Azure pro správu prostředků najdete v tématu [Příručka pro vývojáře k autorizaci s rozhraním API pro Azure Resource Manager](resource-manager-api-authentication.md).
* Podrobnější vysvětlení aplikací a objekty služby najdete v tématu [objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md). 
* Další informace o ověřování Azure Active Directory najdete v tématu [scénáře ověřování pro Azure AD](../active-directory/active-directory-authentication-scenarios.md).
* Seznam dostupných akcí, které může být povolen nebo odepřen uživatelům najdete v tématu [poskytovatel prostředků Azure Resource Manager operations](../active-directory/role-based-access-control-resource-provider-operations.md).

