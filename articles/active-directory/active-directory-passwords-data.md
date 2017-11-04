---
title: "Požadavky na Azure AD SSPR dat | Microsoft Docs"
description: "Požadavky na data pro hesla pomocí samoobslužné služby Azure AD pro resetování a postup jejich splnění"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 7936f47007285e3f7fa1d3220efa022a6e3881ca
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a>Nasazení bez nutnosti registrace koncového uživatele pro vytvoření nového hesla

Nasazení samoobslužného resetování hesla (SSPR) vyžaduje ověřování dat být k dispozici. Některé organizace mají své uživatele, zadejte svoje data ověřování sami, ale v mnoha organizacích je synchronizovat s existujícími daty ve službě Active Directory. Pokud jste správně formátovaných dat ve vašem místním adresáři a nakonfigurovat [Azure AD Connect pomocí expresního nastavení](./connect/active-directory-aadconnect-get-started-express.md), že dat je k dispozici pro Azure AD a SSPR se nevyžaduje zásah uživatele.

Žádné telefonní číslo musí být ve formátu + CountryCode PhoneNumber příklad: + 1 4255551234 správně fungovat.

> [!NOTE]
> Resetování hesla nepodporuje telefonní klapky. I ve formátu 12345 4255551234 + 1 X jsou rozšíření odebrat před uvedením volání.

## <a name="fields-populated"></a>Pole se vyplní

Pokud použijete výchozí nastavení v Azure AD Connect jsou provedeny následující mapování.

| Místní AD | Azure AD | Azure AD Authentication kontaktní údaje |
| --- | --- | --- |
| telephoneNumber | Telefon do kanceláře | Alternativní telefonní |
| mobilní | Mobilní telefon | Telefon |


## <a name="security-questions-and-answers"></a>Bezpečnostní otázky a odpovědi

Bezpečnostní otázky a odpovědi jsou bezpečně uloženy v klientovi služby Azure AD a jsou přístupné prostřednictvím jenom [portálu pro registraci SSPR](https://aka.ms/ssprsetup). Správci nelze zobrazit ani upravovat obsah jiného uživatele otázky a odpovědi.

### <a name="what-happens-when-a-user-registers"></a>Co se stane, když se uživatel zaregistruje

Když se uživatel zaregistruje, nastavuje registrační stránce následující pole:

* Telefon pro ověření
* Ověření e-mailu
* Bezpečnostní otázky a odpovědi

Pokud jste zadali hodnotu **mobilní telefon** nebo **alternativní e-mailovou**, uživatelé mohou okamžitě pomocí tyto hodnoty resetovat vlastní hesla i v případě, že nebyly registrovány pro službu. Kromě toho uživatelé zobrazit tyto hodnoty při první registraci a je upravit, pokud si přejí. Po jejich úspěšně zaregistrovat, budou tyto hodnoty v jako trvalý **telefon pro ověření** a **ověřování e-mailu** pole, v uvedeném pořadí.

## <a name="set-and-read-authentication-data-using-powershell"></a>Nastavit a číst data ověřování pomocí prostředí PowerShell

Následující pole se dá nastavit pomocí prostředí PowerShell

* Alternativní e-mailu
* Mobilní telefon
* Telefon do kanceláře - lze nastavit pouze v případě není synchronizaci s místním adresářem

### <a name="using-powershell-v1"></a>Pomocí prostředí PowerShell V1

Chcete-li začít pracovat, je potřeba [stáhněte a nainstalujte modul Azure AD PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule). Až budete mít nainstalováno, můžete provést kroky, které provést při konfiguraci každé pole.

#### <a name="set-authentication-data-with-powershell-v1"></a>Nastavení ověření dat pomocí prostředí PowerShell V1

```
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-authentication-data-with-powershellpowershell-v1"></a>Čtení dat ověřování pomocí PowerShellPowerShell V1

```
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="authentication-phone-and-authentication-email-can-only-be-read-using-powershell-v1-using-the-commands-that-follow"></a>Telefon pro ověření a e-mailu ověřování lze číst pouze pomocí prostředí Powershell V1 pomocí příkazů, které následují

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="using-powershell-v2"></a>Pomocí prostředí PowerShell V2

Chcete-li začít pracovat, je potřeba [stáhněte a nainstalujte modul prostředí PowerShell Azure AD V2](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md). Až budete mít nainstalováno, můžete provést kroky, které provést při konfiguraci každé pole.

K instalaci rychle z nejnovější verze prostředí PowerShell, které podporují instalaci modulu, spuštěním těchto příkazů (první řádek jednoduše kontroluje, zda je již nainstalována):

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-authentication-data-with-powershell-v2"></a>Nastavení ověření dat pomocí prostředí PowerShell V2

```
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

### <a name="read-authentication-data-with-powershell-v2"></a>Čtení dat ověřování pomocí prostředí PowerShell V2

```
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a>Další kroky

* [Jak dokončení úspěšné zavedení SSPR?](active-directory-passwords-best-practices.md)
* [Resetovat nebo změnit heslo](active-directory-passwords-update-your-own-password.md).
* [Registrace pro resetování hesla pomocí samoobslužné služby](active-directory-passwords-reset-register.md).
* [Máte dotaz Licensing?](active-directory-passwords-licensing.md)
* [Jaké metody ověřování jsou k dispozici pro uživatele?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Jaké jsou možnosti zásad s SSPR?](active-directory-passwords-policy.md)
* [Co je zpětný zápis hesla a proč je starat o něm?](active-directory-passwords-writeback.md)
* [Jak sestavy na aktivitu v SSPR](active-directory-passwords-reporting.md)
* [Co jsou všechny možnosti v SSPR a co znamená, že?](active-directory-passwords-how-it-works.md)
* [Myslím, že něco je poškozená. Jak odstranit SSPR?](active-directory-passwords-troubleshoot.md)
* [Je nutné zadat otázku, která nebyla někde zahrnutých, jinak](active-directory-passwords-faq.md)
