---
title: "Azure AD Connect: Řešení potíží s bezproblémové jednotné přihlašování | Microsoft Docs"
description: "Toto téma popisuje postupy řešení potíží s Azure Active Directory bezproblémové jednotné přihlašování (Azure AD bezproblémové jednotné přihlašování)."
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
ms.openlocfilehash: b383a21500c753d8d2fe6747756541a3ff94ef02
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2017
---
# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Řešení potíží s Azure Active Directory bezproblémové jednotné přihlašování

Tento článek vám pomůže najít informace o běžných problémech týkající se Azure AD bezproblémové jednotného přihlašování k řešení potíží.

## <a name="known-issues"></a>Známé problémy

- V určitých případech povolení jednotného přihlašování bezproblémové může trvat až 30 minut.
- Podpora prohlížeče Edge není k dispozici.
- Spouštění klientů sady Office, obzvláště v případech sdílený počítač, způsobit velmi přihlášení výzvy pro uživatele. Uživatelé budou muset často zadejte uživatelská jména, ale není hesla.
- Pokud bezproblémové jednotného přihlašování k úspěšné, uživatel není zadána možnost vybrat "Zůstat přihlášeni". Z důvodu toto chování služby SharePoint a OneDrive scénáře mapování nefungují.
- Bezproblémové SSO nefunguje v privátním režimu procházení na Firefox.
- Po zapnutí režimu rozšířené ochrany bezproblémové SSO nefunguje v aplikaci Internet Explorer.
- Bezproblémové SSO nefunguje v mobilních prohlížečích na iOS a Android.
- Pokud se synchronizace 30 nebo více doménových struktur služby AD, nelze povolit bezproblémové jednotného přihlašování pomocí služby Azure AD Connect. Jako alternativní řešení můžete [ručně povolit](#manual-reset-of-azure-ad-seamless-sso) funkci na klientovi.
- Přidání adresy URL služby Azure AD (https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net) do zóny "Důvěryhodné servery" místo "místní" intranet **blokuje uživatelům v přihlašování**.

## <a name="check-status-of-the-feature"></a>Zkontrolujte stav funkce

Zajistěte, aby funkce jednotného přihlašování bezproblémové stále **povoleno** na klientovi. Stav můžete zkontrolovat přechodem na **Azure AD Connect** okno na [centra pro správu Azure Active Directory](https://aad.portal.azure.com/).

![Azure Centrum pro správu služby Active Directory – okno Azure AD Connect](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="sign-in-failure-reasons-on-the-azure-active-directory-admin-center-needs-premium-license"></a>Chyba přihlášení důvodů v Centru pro správu Azure Active Directory (musí licencí Premium)

Pokud váš klient má licenci Azure AD Premium s ním spojená, můžete také prohlédnout [přihlašovací aktivita sestavy](../active-directory-reporting-activity-sign-ins.md) na [centra pro správu Azure Active Directory](https://aad.portal.azure.com/).

![Azure Active Directory Centrum pro správu – sestava přihlášení](./media/active-directory-aadconnect-sso/sso9.png)

Přejděte na **Azure Active Directory** -> **přihlášení** na [centra pro správu Azure Active Directory](https://aad.portal.azure.com/) a klikněte na aktivitu přihlášení příslušného uživatele. Vyhledejte **kód chyby SIGN-IN** pole. Mapování hodnotu tohoto pole na příčinu selhání a řešení v následující tabulce:

|Kód chyby přihlášení|Důvod selhání přihlášení|Řešení
| --- | --- | ---
| 81001 | Lístek Kerberos uživatele je příliš velký. | Snižte členství uživatele ve skupině a zkuste to znovu.
| 81002 | Nepodařilo se ověřit lístek Kerberos uživatele. | V tématu [řešení potíží s kontrolní seznam](#troubleshooting-checklist).
| 81003 | Nepodařilo se ověřit lístek Kerberos uživatele. | V tématu [řešení potíží s kontrolní seznam](#troubleshooting-checklist).
| 81004 | Pokus o ověření protokolu Kerberos selhal. | V tématu [řešení potíží s kontrolní seznam](#troubleshooting-checklist).
| 81008 | Nepodařilo se ověřit lístek Kerberos uživatele. | V tématu [řešení potíží s kontrolní seznam](#troubleshooting-checklist).
| 81009 | "Nelze ověřit uživatele lístek protokolu Kerberos. | V tématu [řešení potíží s kontrolní seznam](#troubleshooting-checklist).
| 81010 | Bezproblémové jednotné přihlašování selhalo, protože vypršela platnost lístku Kerberos uživatele nebo je neplatný. | Uživatel musí k přihlášení ze zařízení připojeného k doméně uvnitř firemní sítě.
| 81011 | Nepodařilo se najít objekt uživatele na základě informací v lístku Kerberos uživatele. | Pomocí Azure AD Connect synchronizovat informace o uživateli do služby Azure AD.
| 81012 | Uživatel, který se pokouší přihlásit k Azure AD, je jiný než uživatel přihlášený na zařízení. | Přihlásit z jiného zařízení.
| 81013 | Nepodařilo se najít objekt uživatele na základě informací v lístku Kerberos uživatele. |Pomocí Azure AD Connect synchronizovat informace o uživateli do služby Azure AD. 

## <a name="troubleshooting-checklist"></a>Řešení potíží s kontrolní seznam

Následující kontrolní seznam používejte k řešení potíží bezproblémové jednotné přihlašování:

- Zkontrolujte, zda je povolena funkce bezproblémové jednotného přihlašování k v Azure AD Connect. Nelze-li povolit funkci (například z důvodu blokované port), ujistěte se, že budete mít vše [předpoklady](active-directory-aadconnect-sso-quick-start.md#step-1-check-prerequisites) na místě.
- Zkontrolujte, zda jsou obě tyto Azure AD adresy URL (https://autologon.microsoftazuread-sso.com a https://aadg.windows.net.nsatc.net) součástí nastavení zóny intranetu uživatele.
- Zajistěte, aby že o firemní zařízení je připojené k doméně AD.
- Zkontrolujte, zda že je uživatel přihlášen do zařízení pomocí účtu domény AD.
- Ujistěte se, že účet uživatele z doménové struktury služby AD, kde byl bezproblémové jednotného přihlašování k nastavení.
- Zkontrolujte, zda že je zařízení připojené v podnikové síti.
- Zajistěte, aby zařízení čas je synchronizován se službou Active Directory a v řadičích domény čas a během pěti minut vzájemně.
- Seznam existujících lístků protokolu Kerberos na zařízení pomocí **klist** příkazu z příkazového řádku. Zkontrolujte, pokud se lístky vydané pro `AZUREADSSOACCT` účet počítače jsou k dispozici. Lístky protokolu Kerberos uživatelů jsou obvykle platné po dobu 12 hodin. Můžete mít různá nastavení ve službě Active Directory.
- Vymazání existujících lístků protokolu Kerberos z zařízení pomocí **klist mazání** příkaz a akci opakujte.
- Chcete-li zjistit, jestli existují problémy související s JavaScript, zkontrolujte protokoly konzoly prohlížeče (v části "Developer Tools").
- Zkontrolujte [řadič domény](#domain-controller-logs) také.

### <a name="domain-controller-logs"></a>Řadič domény

Pokud je auditování úspěch povoleno na vašem řadiči domény a potom pokaždé, když se uživatel přihlásí pomocí bezproblémové jednotného přihlašování k zabezpečení položky se zaznamená do protokolu událostí. Můžete najít tyto události zabezpečení pomocí následujícího dotazu (vyhledejte událost **4769** přidružené k účtu počítače **AzureADSSOAcc$**):

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-the-feature"></a>Ruční vynulování funkce

Pokud vám nepomohly řešení potíží, můžete ručně obnovit funkci na klientovi. Proveďte tyto kroky na místní server, kde je spuštěn nástroj Azure AD Connect:

### <a name="step-1-import-the-seamless-sso-powershell-module"></a>Krok 1: Naimportujte modul Powershellu bezproblémové jednotného přihlašování

1. Nejprve stáhnout a nainstalovat [Microsoft Online Services Sign-In Assistant](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Potom stáhněte a nainstalujte [64-bit modulu Azure Active Directory pro prostředí Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Přejděte na `%programfiles%\Microsoft Azure Active Directory Connect` složky.
4. Importujte modul PowerShell bezproblémové jednotného přihlašování k použití tohoto příkazu: `Import-Module .\AzureADSSO.psd1`.

### <a name="step-2-get-the-list-of-ad-forests-on-which-seamless-sso-has-been-enabled"></a>Krok 2: Získání seznamu doménových struktur služby AD, u kterých je povolena bezproblémové jednotného přihlašování

1. Spusťte prostředí PowerShell jako správce. V prostředí PowerShell, zavolejte `New-AzureADSSOAuthenticationContext`. Po zobrazení výzvy zadejte přihlašovací údaje globálního správce vašeho klienta.
2. Volání `Get-AzureADSSOStatus`. Tento příkaz poskytuje seznam doménových struktur AD (podívejte se na seznamu "Domény") na které tato funkce povolená.

### <a name="step-3-disable-seamless-sso-for-each-ad-forest-that-it-was-set-it-up-on"></a>Krok 3: Zakázat bezproblémové jednotného přihlašování pro každou doménovou strukturu AD, který ho se ho nastavit na

1. Volání `$creds = Get-Credential`. Po zobrazení výzvy zadejte přihlašovací údaje správce domény určený doménové struktuře AD.
2. Volání `Disable-AzureADSSOForest -OnPremCredentials $creds`. Tento příkaz odebere `AZUREADSSOACCT` účet počítače z místního řadiče domény pro tento konkrétní doménovou strukturu AD.
3. Opakujte předchozí kroky pro každou doménovou strukturu AD, který jste nastavili funkci na.

### <a name="step-4-enable-seamless-sso-for-each-ad-forest"></a>Krok 4: Povolení bezproblémové jednotného přihlašování pro každou doménovou strukturu AD

1. Volání `Enable-AzureADSSOForest`. Po zobrazení výzvy zadejte přihlašovací údaje správce domény určený doménové struktuře AD.
2. Opakujte předchozí kroky pro každou doménovou strukturu AD, kterou chcete nastavit tuto funkci v.

### <a name="step-5-enable-the-feature-on-your-tenant"></a>Krok 5. Povolit tuto funkci v klientovi

Volání `Enable-AzureADSSO` a zadejte hodnotu "true" na `Enable: ` výzva k zapnutí funkce ve vašem klientovi.
