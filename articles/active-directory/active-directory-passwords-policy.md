---
title: "Zásady: Azure AD SSPR | Microsoft Docs"
description: "Možnosti zásad resetování hesel samoobslužné služby Azure AD"
services: active-directory
keywords: "Správa hesel služby Active directory, správou hesel Azure AD samoobslužném resetování hesla služby"
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
ms.openlocfilehash: 5c33f08e54d522e0eea13a3e267f14f407fc59b6
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Zásady hesel a omezení v Azure Active Directory

Tento článek popisuje zásady pro hesla a požadavky na složitost spojené s uživatelskými účty, které jsou uložené v klientovi služby Azure AD.

## <a name="administrator-password-policy-differences"></a>Rozdíly zásady hesla správce

Microsoft vynucuje silné výchozí zásady resetování hesel se **dvěma branami** pro libovolnou roli správce Azure (příklad: globální správce, správce technické podpory, správce hesel atd.).

To zakáže správci pomocí bezpečnostních otázek a vynucuje následující.

Dvě zásady, které vyžadují dva kusy data ověřování brány (e-mailová adresa **a** telefonní číslo), v následujících případech se vztahuje

* Všechny role Správce služby Azure
  * Správce technické podpory
  * Správce služeb
  * Správce fakturace
  * Podpora Tier1 partnera
  * Podpora Tier2 partnera
  * Správce služby Exchange
  * Správce služby Lync
  * Správce účtu uživatele
  * Directory zapisovače
  * Globální správce správce nebo společnosti
  * Správce služby SharePoint
  * Správce dodržování předpisů
  * Správce aplikací
  * Správce zabezpečení.
  * Privilegované Role Správce
  * Správce služby Intune
  * Správce služby Proxy aplikace
  * Správce služeb CRM
  * Správce služby Power BI
  
* Po uplynutí 30 dnů ve zkušební verzi **nebo**
* Je k dispozici jednoduché domény (contoso.com) **nebo**
* Azure AD Connect je synchronizaci identit z vašeho místního adresáře

### <a name="exceptions"></a>Výjimky
Jedna brána zásadu, vyžadující jeden prvek ověření dat (e-mailová adresa **nebo** telefonní číslo), v následujících případech se vztahuje

* Prvních 30 dnů a zkušební **nebo**
* Jednoduché domény není k dispozici (*. onmicrosoft.com) **a** Azure AD Connect není synchronizaci identit


## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>UserPrincipalName zásady, které platí pro všechny uživatelské účty

Každý uživatelský účet, který se musí přihlásit do služby Azure AD musí mít hodnotu atributu jedinečného uživatelského hlavní název (UPN) přidružené k účtu. Následující tabulce jsou podrobněji popsány dále zásady, které platí pro místní uživatelské účty služby Active Directory synchronizovat do cloudu a jenom pro cloud uživatelské účty.

| Vlastnost | UserPrincipalName požadavky |
| --- | --- |
| Povolené znaky |<ul> <li>A – Z</li> <li>a – z</li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
| Znaky nejsou povoleny |<ul> <li>Všechny ' @' znak, který není oddělení uživatelské jméno z domény.</li> <li>Nesmí obsahovat znak tečky '.' bezprostředně před ' @' – symbol</li></ul> |
| Omezení délky |<ul> <li>Celková délka nesmí přesáhnout délku 113 znaků</li><li>než 64 znaků ' @' – symbol</li><li>48 znaků po ' @' – symbol</li></ul> |

## <a name="password-policies-that-apply-only-to-cloud-user-accounts"></a>Zásady pro hesla, které se vztahují pouze na uživatelské účty v cloudu

Následující tabulka popisuje nastavení zásad k dispozici heslo, které mohou být použity pro uživatelské účty, které jsou vytvořeny a spravované ve službě Azure AD.

| Vlastnost | Požadavky |
| --- | --- |
| Povolené znaky |<ul><li>A – Z</li><li>a – z</li><li>0 – 9</li> <li>@ # $ % ^ & * - _ ! + = [ ] { } &#124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
| Znaky nejsou povoleny |<ul><li>Znaky kódování Unicode</li><li>Mezery</li><li> **Silná hesla jenom**: nesmí obsahovat tečku znak '.' bezprostředně předcházející ' @' symbol</li></ul> |
| Omezení hesla |<ul><li>minimální 8 znaků a maximálně 16 znaků.</li><li>**Silná hesla jenom**: vyžaduje 3 mimo 4 z následujících akcí:<ul><li>Malá písmena</li><li>Velká písmena</li><li>Čísla (0-9)</li><li>Symboly (viz výše uvedené omezení hesla)</li></ul></li></ul> |
| Doba vypršení platnosti hesla |<ul><li>Výchozí hodnota: **90** dnů </li><li>Hodnota je možné konfigurovat pomocí rutiny Set-MsolPasswordPolicy z Azure Active Directory modul pro prostředí Windows PowerShell.</li></ul> |
| Oznámení o vypršení hesla |<ul><li>Výchozí hodnota: **14** (před vypršením platnosti hesla)</li><li>Hodnota je možné konfigurovat pomocí rutiny Set-MsolPasswordPolicy.</li></ul> |
| Vypršení platnosti hesla |<ul><li>Výchozí hodnota: **false** dní (to znamená, že vypršení platnosti hesla je povolené) </li><li>Hodnota jde konfigurovat pomocí rutiny Set-MsolUser jednotlivých uživatelských účtů. </li></ul> |
| Heslo **změnit** historie |Poslední heslo **nelze** znovu použít při **změna** heslo. |
| Heslo **resetovat** historie | Poslední heslo **může** znovu použít při **resetování** zapomenuté heslo. |
| Uzamčení účtu |Po 10 neúspěšných pokusech o přihlášení (chybné heslo) bude uživatel uzamčen po dobu jedné minuty. Další nesprávný pokusy o přihlášení uzamčení uživatele pro zvýšení doby trvání. |

## <a name="set-password-expiration-policies-in-azure-active-directory"></a>Nastavení zásad vypršení platnosti hesla ve službě Azure Active Directory

Globální správce pro cloudové služby Microsoftu vám pomůže nastavit uživatelská hesla nechcete vyprší Microsoft Azure Active Directory modul pro prostředí Windows PowerShell. Rutiny prostředí Windows PowerShell můžete použít také k odebrání nikdy-vyprší platnost konfigurace, nebo pokud chcete zjistit, které uživatel hesla jsou nastavte nikoli k vypršení platnosti. Tyto pokyny platí pro jiných poskytovatelů, jako například Microsoft Intune a Office 365, které také závisí na Microsoft Azure Active Directory pro identitu a adresářové služby.

> [!NOTE]
> Jenom hesla pro uživatelské účty, které nejsou synchronizovány pomocí synchronizace adresáře může být nakonfigurován pro nevyprší platnost. Další informace o synchronizaci adresářů v tématu[AD s Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).
>
>

## <a name="set-or-check-password-policies-using-powershell"></a>Nastavte nebo zkontrolujte zásady pro hesla pomocí prostředí PowerShell

Chcete-li začít pracovat, je potřeba [stáhněte a nainstalujte modul Azure AD PowerShell](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0). Až budete mít nainstalováno, můžete postupujte podle pokynů níže nakonfigurujte každé pole.

### <a name="how-to-check-expiration-policy-for-a-password"></a>Postup kontroly zásad vypršení platnosti hesla
1. Připojení k prostředí Windows PowerShell pomocí svých přihlašovacích údajů správce společnosti.
2. Spusťte jeden z následujících příkazů:

   * Pokud chcete zobrazit, zda je nastaven jednoho uživatelského hesla bez vypršení platnosti, spusťte následující rutinu pomocí hlavního názvu uživatele (UPN) (například aprilr@contoso.onmicrosoft.com) nebo ID uživatele uživatele, které chcete zkontrolovat:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`
   * Pokud chcete zobrazit nastavení "Heslo je platné stále" pro všechny uživatele, spusťte následující rutinu:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

### <a name="set-a-password-to-expire"></a>Nastavení hesla vyprší

1. Připojení k prostředí Windows PowerShell pomocí svých přihlašovacích údajů správce společnosti.
2. Spusťte jeden z následujících příkazů:

   * Nastavte heslo jednoho uživatele, aby vyprší platnost hesla, spusťte následující rutinu pomocí hlavního názvu uživatele (UPN) nebo ID uživatele, uživatel:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`
   * Nastavte hesla všichni uživatelé v organizaci, aby vypršení jejich platnosti, použijte následující rutinu:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

### <a name="set-a-password-to-never-expire"></a>Nastavení hesla bez vypršení platnosti

1. Připojení k prostředí Windows PowerShell pomocí svých přihlašovacích údajů správce společnosti.
2. Spusťte jeden z následujících příkazů:

   * Pokud chcete nastavit heslo jeden uživatel nikdy nevyprší, spusťte následující rutinu pomocí hlavního názvu uživatele (UPN) nebo ID uživatele, uživatel:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`
   * Pokud chcete nastavit hesel všech uživatelů v organizaci bez vypršení platnosti, spusťte následující rutinu:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Další kroky

Na následujících odkazech najdete další informace o resetování hesla pomocí Azure AD

* [Jak dokončení úspěšné zavedení SSPR?](active-directory-passwords-best-practices.md)
* [Resetovat nebo změnit heslo](active-directory-passwords-update-your-own-password.md).
* [Registrace pro resetování hesla pomocí samoobslužné služby](active-directory-passwords-reset-register.md).
* [Máte dotaz Licensing?](active-directory-passwords-licensing.md)
* [Jaká data používá SSPR a jaká data by měla můžete naplnit pro vaše uživatele?](active-directory-passwords-data.md)
* [Jaké metody ověřování jsou k dispozici pro uživatele?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Co je zpětný zápis hesla a proč je starat o něm?](active-directory-passwords-writeback.md)
* [Jak sestavy na aktivitu v SSPR](active-directory-passwords-reporting.md)
* [Co jsou všechny možnosti v SSPR a co znamená, že?](active-directory-passwords-how-it-works.md)
* [Myslím, že něco je poškozená. Jak odstranit SSPR?](active-directory-passwords-troubleshoot.md)
* [Je nutné zadat otázku, která nebyla někde zahrnutých, jinak](active-directory-passwords-faq.md)
