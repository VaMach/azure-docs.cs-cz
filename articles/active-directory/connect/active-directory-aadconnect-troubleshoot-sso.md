---
title: "Azure Active Directory Connect: Řešení potíží s bezproblémové jednotné přihlašování | Microsoft Docs"
description: "Toto téma popisuje postupy řešení potíží s Azure Active Directory bezproblémové jednotné přihlašování"
services: active-directory
keywords: "Co je Azure AD Connect, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: b4efb457a58d8b54c9ebb126a8d84fdef01b3847
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Řešení potíží s Azure Active Directory bezproblémové jednotné přihlašování

Tento článek vám pomůže najít informace o běžné problémy týkající se služby Azure Active Directory (Azure AD) bezproblémové jednotné přihlašování (SSO bezproblémové) k řešení potíží.

## <a name="known-problems"></a>Známé problémy

- V určitých případech povolení jednotného přihlašování bezproblémové může trvat až 30 minut.
- Podpora prohlížeče Edge není k dispozici.
- Od klientů Office, obzvláště v případech sdílený počítač, způsobí, že výzvy velmi přihlášení pro uživatele. Uživatelé musí zadat jejich uživatelských jmen často, ale není jejich hesla.
- Pokud bezproblémové jednotného přihlašování k úspěšné, uživatel nemá možnost vybrat **zůstat přihlášeni**. Z důvodu toto chování služby SharePoint a OneDrive scénáře mapování nefungují.
- Bezproblémové SSO nefunguje v privátním režimu procházení na Firefox.
- Po zapnutí rozšířeného chráněný režim bezproblémové jednotného přihlašování k nefunguje v aplikaci Internet Explorer.
- Bezproblémové SSO nefunguje v mobilních prohlížečích na iOS a Android.
- Pokud se synchronizace 30 nebo více doménových struktur služby Active Directory, nelze povolit bezproblémové jednotné přihlašování přes Azure AD Connect. Jako alternativní řešení můžete [ručně povolit](#manual-reset-of-azure-ad-seamless-sso) funkci na klientovi.
- Přidání adresy URL služby Azure AD (https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net) do zóny důvěryhodných serverů místo zóny místního intranetu *blokuje uživatelům v přihlašování*.

## <a name="check-the-status-of-the-feature"></a>Zkontrolujte stav funkce

Zajistěte, aby funkce jednotného přihlašování bezproblémové stále **povoleno** na klientovi. Stav můžete zkontrolovat přechodem na **Azure AD Connect** v podokně [centra pro správu Azure Active Directory](https://aad.portal.azure.com/).

![Centrum pro správu Azure Active Directory: podokně Azure AD Connect](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="sign-in-failure-reasons-in-the-azure-active-directory-admin-center-needs-a-premium-license"></a>Chyba přihlášení z důvodů v Centru správy služby Azure Active Directory (potřebuje licenci Premium)

Pokud váš klient má licenci Azure AD Premium s ním spojená, můžete také prohlédnout [přihlašovací aktivita sestavy](../active-directory-reporting-activity-sign-ins.md) v [centra pro správu Azure Active Directory](https://aad.portal.azure.com/).

![Centrum pro správu Azure Active Directory: Sestava přihlášení](./media/active-directory-aadconnect-sso/sso9.png)

Přejděte do **Azure Active Directory** > **přihlášení** v [centra pro správu Azure Active Directory](https://aad.portal.azure.com/)a potom vyberte přihlašovací aktivity konkrétního uživatele. Vyhledejte **kód chyby SIGN-IN** pole. Mapování hodnotu tohoto pole na příčinu selhání a řešení v následující tabulce:

|Kód chyby přihlášení|Důvod selhání přihlášení|Řešení
| --- | --- | ---
| 81001 | Lístek Kerberos uživatele je příliš velký. | Snižte členství uživatele ve skupinách a zkuste to znovu.
| 81002 | Nelze ověřit uživatele lístek protokolu Kerberos. | Najdete v článku [řešení potíží s kontrolní seznam](#troubleshooting-checklist).
| 81003 | Nelze ověřit uživatele lístek protokolu Kerberos. | Najdete v článku [řešení potíží s kontrolní seznam](#troubleshooting-checklist).
| 81004 | Pokus o ověření protokolu Kerberos selhal. | Najdete v článku [řešení potíží s kontrolní seznam](#troubleshooting-checklist).
| 81008 | Nelze ověřit uživatele lístek protokolu Kerberos. | Najdete v článku [řešení potíží s kontrolní seznam](#troubleshooting-checklist).
| 81009 | Nelze ověřit uživatele lístek protokolu Kerberos. | Najdete v článku [řešení potíží s kontrolní seznam](#troubleshooting-checklist).
| 81010 | Bezproblémové jednotné přihlašování selhalo, protože vypršela platnost lístku Kerberos uživatele nebo je neplatný. | Uživatel potřebuje k přihlášení ze zařízení připojeného k doméně uvnitř firemní sítě.
| 81011 | Nepodařilo se najít objekt uživatele na základě informací v lístku protokolu Kerberos uživatele. | Použijte Azure AD Connect k synchronizaci informací o uživateli do služby Azure AD.
| 81012 | Uživatel pokusu o přihlášení k Azure AD se liší od uživatele, který je přihlášený do zařízení. | Uživatel musí přihlásit z jiného zařízení.
| 81013 | Nepodařilo se najít objekt uživatele na základě informací v lístku protokolu Kerberos uživatele. |Použijte Azure AD Connect k synchronizaci informací o uživateli do služby Azure AD. 

## <a name="troubleshooting-checklist"></a>Řešení potíží s kontrolní seznam

Následující kontrolní seznam využít k řešení problémů bezproblémové jednotné přihlašování:

- Ujistěte se, povolení funkce bezproblémové jednotné přihlašování v Azure AD Connect. Nelze-li povolit funkci (například z důvodu blokované port), ujistěte se, že budete mít vše [požadavky](active-directory-aadconnect-sso-quick-start.md#step-1-check-the-prerequisites) na místě.
- Ujistěte se, že obě tyto Azure AD adresy URL (https://autologon.microsoftazuread-sso.com a https://aadg.windows.net.nsatc.net) jsou součástí nastavení zóny intranetu uživatele.
- Ujistěte se, že o firemní zařízení je připojený k doméně služby Active Directory.
- Ujistěte se, že je uživatel přihlášen do zařízení přes účet domény služby Active Directory.
- Ujistěte se, že účet uživatele z doménové struktury služby Active Directory, kde byl bezproblémové jednotného přihlašování k nastavení.
- Ujistěte se, že zařízení je připojené k podnikové síti.
- Ujistěte se, že je zařízení čas synchronizovaný s časem ve službě Active Directory a řadiče domény a že jsou během pěti minut.
- Seznam existujících lístků protokolu Kerberos na zařízení pomocí `klist` příkazu z příkazového řádku. Ujistěte se, že lístky vydané pro `AZUREADSSOACCT` účet počítače jsou k dispozici. Lístky protokolu Kerberos uživatelů jsou obvykle platné po dobu 12 hodin. Můžete mít různá nastavení ve službě Active Directory.
- Vyprázdnění existujících lístků protokolu Kerberos ze zařízení pomocí `klist purge` příkaz a akci opakujte.
- Pokud chcete zjistit, zda existují problémy související s JavaScript, projděte si protokoly konzoly prohlížeče (v části **Developer Tools**).
- Zkontrolujte [protokoly řadiče domény](#domain-controller-logs).

### <a name="domain-controller-logs"></a>Protokoly řadiče domény

Pokud povolíte auditování úspěšných na vašem řadiči domény a potom pokaždé, když se uživatel přihlásí pomocí bezproblémové jednotné přihlašování, položku zabezpečení se zaznamená do protokolu událostí. Tyto události zabezpečení můžete najít pomocí následujícího dotazu. (Vyhledejte událost **4769** přidružené k účtu počítače **AzureADSSOAcc$**.)

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-the-feature"></a>Ruční vynulování funkce

Pokud vám nepomohly řešení potíží, můžete ručně obnovit funkci na klientovi. Proveďte tyto kroky na místní server, kde spouštíte Azure AD Connect.

### <a name="step-1-import-the-seamless-sso-powershell-module"></a>Krok 1: Naimportujte modul Powershellu bezproblémové jednotného přihlašování

1. Stáhněte a nainstalujte [Microsoft Online Services Sign-In Assistant](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Stáhněte a nainstalujte [64-bit modulu Azure Active Directory pro prostředí Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Vyhledejte `%programfiles%\Microsoft Azure Active Directory Connect` složky.
4. Naimportujte modul Powershellu bezproblémové jednotného přihlašování pomocí tohoto příkazu: `Import-Module .\AzureADSSO.psd1`.

### <a name="step-2-get-the-list-of-active-directory-forests-on-which-seamless-sso-has-been-enabled"></a>Krok 2: Získání seznamu doménových struktur služby Active Directory, u kterých je povolena bezproblémové jednotného přihlašování

1. Spusťte prostředí PowerShell jako správce. V prostředí PowerShell, zavolejte `New-AzureADSSOAuthenticationContext`. Po zobrazení výzvy zadejte přihlašovací údaje globálního správce vašeho klienta.
2. Volání `Get-AzureADSSOStatus`. Tento příkaz vám poskytne seznam doménové struktury služby Active Directory (podívejte se na seznamu "Domény") na které tato funkce povolená.

### <a name="step-3-disable-seamless-sso-for-each-active-directory-forest-where-youve-set-up-the-feature"></a>Krok 3: Zakázat bezproblémové jednotného přihlašování pro každou doménovou strukturu služby Active Directory, kde jste nastavili funkci

1. Volání `$creds = Get-Credential`. Po zobrazení výzvy zadejte přihlašovací údaje správce domény určený doménové struktury služby Active Directory.
2. Volání `Disable-AzureADSSOForest -OnPremCredentials $creds`. Tento příkaz odebere `AZUREADSSOACCT` účet počítače z místního řadiče domény pro tento konkrétní doménovou strukturu služby Active Directory.
3. Opakujte předchozí kroky pro každou doménovou strukturu služby Active Directory, kde jste nastavili funkci.

### <a name="step-4-enable-seamless-sso-for-each-active-directory-forest"></a>Krok 4: Povolení bezproblémové jednotného přihlašování pro každou doménovou strukturu služby Active Directory

1. Volání `Enable-AzureADSSOForest`. Po zobrazení výzvy zadejte přihlašovací údaje správce domény určený doménové struktury služby Active Directory.
2. Opakujte předchozí krok pro každou doménovou strukturu služby Active Directory, ve které chcete nastavit tuto funkci.

### <a name="step-5-enable-the-feature-on-your-tenant"></a>Krok 5. Povolit tuto funkci v klientovi

Chcete-li zapnout funkci na klientovi, volejte `Enable-AzureADSSO` a zadejte **true** na `Enable:` řádku.
