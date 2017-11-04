---
title: "Azure AD Connect: Bezproblémové jednotné přihlašování – rychlý Start | Microsoft Docs"
description: "Tento článek popisuje, jak začít pracovat s Azure Active Directory bezproblémové jednotné přihlašování."
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
ms.openlocfilehash: 8975a82c5573cc0c284e1fc76cd0ef2c19fbbd72
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Azure Active Directory bezproblémové jednotné přihlašování: rychlý start

## <a name="how-to-deploy-seamless-sso"></a>Postup nasazení bezproblémové jednotného přihlašování

Azure Active Directory bezproblémové jednotné přihlašování (Azure AD bezproblémové jednotné přihlašování) automaticky přihlásí uživatele v když jsou v jejich podnikových počítačů připojených k podnikové síti. Poskytuje uživatelům snadný přístup k vaší cloudové aplikace bez nutnosti jakékoli další místní komponenty.

Chcete-li nasadit bezproblémové jednotné přihlašování, postupujte takto:

## <a name="step-1-check-prerequisites"></a>Krok 1: Kontrola předpokladů

Ujistěte se, že jsou splněné následující požadavky:

1. Nastavit server Azure AD Connect: Pokud používáte [předávací ověřování](active-directory-aadconnect-pass-through-authentication.md) jako způsob přihlášení je vyžadována žádné další předběžné kontrola. Pokud používáte [synchronizaci hodnoty Hash hesla](active-directory-aadconnectsync-implement-password-synchronization.md) jako způsob přihlášení a pokud je brána firewall mezi Azure AD Connect a službou Azure AD, ověřte, že:
- Používáte-li verze 1.1.644.0 nebo novější služby Azure AD Connect. 
- Pokud je brána firewall nebo proxy server umožňuje povolených DNS, povolených připojení k  **\*. msappproxy.net** adresy URL přes port 443. Pokud ne, povolit přístup k [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/details.aspx?id=41653), se aktualizují každý týden. Tento požadavek se vztahuje pouze v případě, že povolíte funkci, ne pro skutečné uživatelská přihlášení.

    >[!NOTE]
    >Azure AD Connect verze 1.1.557.0, 1.1.558.0, 1.1.561.0 a 1.1.614.0 mít problém související s synchronizaci hodnoty Hash hesla. Pokud jste _nemáte_ v úmyslu použít synchronizaci hodnoty Hash hesla ve spojení se předávací ověřování, přečtěte si [poznámky k verzi Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470) Další informace.

2. Potřebujete přihlašovací údaje správce domény pro každou doménovou strukturu AD, která synchronizovat do Azure AD (přes Azure AD Connect) a pro uživatele, jehož chcete povolit bezproblémové jednotné přihlašování.

## <a name="step-2-enable-the-feature"></a>Krok 2: Povolení funkce

Bezproblémové jednotné přihlašování se dá nastavit pomocí [Azure AD Connect](active-directory-aadconnect.md).

Pokud se jedná o novou instalaci Azure AD Connect, vyberte [vlastní cesta](active-directory-aadconnect-get-started-custom.md). Na stránce "Přihlášení uživatele" a zaškrtněte možnost "Povolit jednotné přihlašování na".

![Azure AD Connect – přihlášení uživatele](./media/active-directory-aadconnect-sso/sso8.png)

Pokud již máte instalace služby Azure AD Connect, zvolte "Změna uživatele přihlašovací stránka" na Azure AD Connect a klikněte na tlačítko Další. Zkontrolujte možnost "Povolit jednotné přihlašování na".

![Azure AD Connect – přihlášení uživatele změn](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Postupujte podle pokynů Průvodce až na stránce "Povolit jednotné přihlašování na". Zadejte pověření správce domény pro každou doménovou strukturu AD, která synchronizovat do Azure AD (přes Azure AD Connect) a pro uživatele, jehož chcete povolit bezproblémové jednotné přihlašování. 

Po dokončení průvodce je povoleno bezproblémové jednotného přihlašování na klientovi.

>[!NOTE]
> Přihlašovací údaje správce domény nejsou uložené ve službě Azure AD Connect nebo ve službě Azure AD, ale používají jenom k povolení této funkce.

Postupujte podle těchto pokynů k ověření, že je povoleno jednotné přihlašování bezproblémové správně:

1. Přihlaste se k [centra pro správu Azure Active Directory](https://aad.portal.azure.com) pomocí přihlašovacích údajů globálního správce pro vašeho klienta.
2. Vyberte **Azure Active Directory** na levé straně.
3. Vyberte **Azure AD Connect**.
4. Ověřte, zda **bezproblémové jednotné přihlašování** zobrazí jako **povoleno**.

![Portál Azure – okno Azure AD Connect](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-the-feature"></a>Krok 3: Přejít na funkci

K zavedení funkci pro vaše uživatele, je nutné přidat následující adresy URL služby Azure AD k nastavení zóny intranetu uživatelů pomocí zásad skupiny ve službě Active Directory:

- https://AutoLogon.microsoftazuread-sso.com
- https://aadg.Windows.NET.nsatc.NET

Kromě toho budete muset povolit zóny intranetu nastavení zásady (pomocí zásad skupiny) názvem "Povolit aktualizace stavového řádku prostřednictvím skript".

>[!NOTE]
> Podle následujících pokynů fungovat pouze pro Internet Explorer a Google Chrome v systému Windows (pokud ji sdílí sadu adres URL důvěryhodných serverů s aplikací Internet Explorer). Přečtěte si další části Pokyny k nastavení Mozilla Firefox a Google Chrome na macu.

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a>Proč je třeba změnit nastavení zóny intranetu uživatelů?

Ve výchozím prohlížeči automaticky vypočítá správné zóny (Internetu nebo intranetu) z adresy URL. Například http://contoso/ je namapována na zóny intranetu, zatímco http://intranet.contoso.com/ je namapovaný na zóně Internet, (protože adresa URL obsahuje tečku). Prohlížeče Neodesílat lístky protokolu Kerberos pro koncový bod cloudu – jako dvou Azure AD adres URL - pokud jeho adresa URL je přidané do zóny intranetu prohlížeče.

### <a name="detailed-steps"></a>Podrobné kroky

1. Spusťte nástroj pro správu zásad skupiny.
2. Upravit zásady skupiny, které se použije k některým nebo všem uživatelům. V tomto příkladu používáme **výchozí zásady domény**.
3. Přejděte na **uživatele Konfigurace počítače\Šablony pro správu\Součásti systému Windows\Internet Internetu řízení Panel\Security stránky** a vyberte **webu do zóny přiřazení seznamu**.
![Jednotné přihlašování](./media/active-directory-aadconnect-sso/sso6.png)
4. Povolení zásad a zadejte následující hodnoty (které jsou předávány lístky protokolu Kerberos URL Azure AD) a data (*1* označuje zóny intranetu) v dialogovém okně.

        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.windows.net.nsatc.net
        Data: 1
>[!NOTE]
> Pokud chcete zakázat některé uživatele pomocí bezproblémové jednotné přihlašování – například pokud tito uživatelé se přihlašujete na sdílené veřejné terminály – nastavte předchozí hodnoty na *4*. Tato akce přidá do zóny s omezeným přístupem adresy URL Azure AD a selže vždy bezproblémové jednotné přihlašování.

5. Klikněte na tlačítko **OK** a **OK** znovu.
![Jednotné přihlašování](./media/active-directory-aadconnect-sso/sso7.png)
6. Přejděte na **uživatele Konfigurace počítače\Šablony pro správu\Součásti systému Windows\Internet Internetu řízení Panel\Security Page\Intranet zóny** a vyberte **povolit aktualizace stavového řádku pomocí skriptu**.
![Jednotné přihlašování](./media/active-directory-aadconnect-sso/sso11.png)
7. Povolit nastavení zásady a klikněte na tlačítko **OK**.
![Jednotné přihlašování](./media/active-directory-aadconnect-sso/sso12.png)

### <a name="browser-considerations"></a>Důležité informace o prohlížeči

#### <a name="mozilla-firefox"></a>Mozilla Firefox

Mozilla Firefox automaticky neprovádí ověřování protokolu Kerberos. Každý uživatel má Azure AD adresy URL. ručně přidat do jejich nastavení Firefox pomocí následujících kroků:
1. Spustit Firefox a zadejte `about:config` na panelu Adresa. Zavřete všechny oznámení, které vidíte.
2. Vyhledejte **identifikátory URI network.negotiate auth.trusted** předvoleb. Tato předvolba obsahuje seznam důvěryhodných serverů na Firefox ověřování protokolem Kerberos.
3. Klikněte pravým tlačítkem a vyberte "Upravit".
4. Zadejte "https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net" v poli.
5. Klikněte na tlačítko "OK" a znovu otevřete v prohlížeči.

#### <a name="safari-on-mac-os"></a>Safari v systému Mac OS

Ujistěte se, že počítač se systémem Mac OS je připojený k AD. Zobrazí pokyny o tom, jak to udělat [zde](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).

#### <a name="google-chrome-on-mac-os"></a>Google Chrome systému Mac OS

Google Chrome na ostatní platformy jiný systém než Windows a Mac OS, najdete v části [v tomto článku](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) informace o tom, jak vytvořit bílou Azure AD adresy URL pro integrované ověřování.

Pomocí rozšíření zásad skupiny služby Active Directory třetích stran k zavedení adresy URL Azure AD a Firefox, Google Chrome na uživatele počítačů Mac je mimo rozsah tohoto článku.

#### <a name="known-browser-limitations"></a>Omezení známé prohlížeče

Bezproblémové SSO nefunguje v privátním režimu procházení na prohlížeče Firefox a okraj. Také nefunguje v Internet Exploreru Pokud prohlížeče běží v režimu rozšířené ochrany.

>[!IMPORTANT]
>Jsme nedávno vrácena podporu pro hraniční síti, aby prozkoumat problémy hlášené zákazníka.

## <a name="step-4-test-the-feature"></a>Krok 4: Testování funkce

Chcete-li otestovat funkci pro konkrétního uživatele, ověřte, zda _všechny_ jsou splněné následující podmínky:
  - Uživatel je přihlášení na firemní zařízení.
  - Zařízení byl dříve připojen k doméně služby Active Directory (AD).
  - Zařízení má přímé připojení k vaší řadiči domény (DC), buď v podnikové síti pevné nebo bezdrátové sítě nebo prostřednictvím připojení vzdáleného přístupu, jako je například připojení k síti VPN.
  - Máte [nasazuje funkci](##step-3-roll-out-the-feature) na tohoto uživatele pomocí zásad skupiny.

K otestování tohoto scénáře, kde uživatel zadá pouze uživatelské jméno, ale ne heslo:
- Přihlaste se k *https://myapps.microsoft.com/* v nové relaci privátní prohlížeče.

K otestování tohoto scénáře, kde uživatel nemusí zadávat uživatelské jméno nebo heslo: 
- Přihlaste se k *https://myapps.microsoft.com/contoso.onmicrosoft.com* v nové relaci privátní prohlížeče. Nahraďte "*contoso*" s název vašeho klienta.
- Nebo se přihlaste *https://myapps.microsoft.com/contoso.com* v nové relaci privátní prohlížeče. Nahraďte "*contoso.com*" s ověřenou doménu (nikoli federované domény) ve vašem klientovi.

## <a name="step-5-roll-over-keys"></a>Krok 5: Vrácení nad klíči

V kroku 2 vytvoří Azure AD Connect účty počítačů (představující Azure AD) ve všech AD doménových strukturách, na kterých jste povolili bezproblémové jednotné přihlašování. Další informace v podrobněji [zde](active-directory-aadconnect-sso-how-it-works.md). Pro lepší zabezpečení se doporučuje, aby pravidelně nezaregistrujete prostřednictvím protokolu Kerberos dešifrovací klíče tyto účty počítačů. Tyto pokyny o tom, jak vrátit jsou [zde](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account).

>[!IMPORTANT]
>Nemusíte proveďte tento krok _okamžitě_ poté, co povolíte funkci. Mění dešifrovací klíče protokolu Kerberos minimálně každých 30 dnů.

## <a name="next-steps"></a>Další kroky

- [Podrobné technické informace](active-directory-aadconnect-sso-how-it-works.md) -pochopit, jak tato funkce funguje.
- [Nejčastější dotazy](active-directory-aadconnect-sso-faq.md) -odpovědi na nejčastější dotazy.
- [Řešení potíží s](active-directory-aadconnect-troubleshoot-sso.md) – zjistěte, jak řešit obvyklé problémy s funkcí.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.
