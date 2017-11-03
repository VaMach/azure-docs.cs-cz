---
title: "Azure AD Connect: Předávací ověřování – nejčastější dotazy | Microsoft Docs"
description: "Odpovědi na nejčastější dotazy k Azure Active Directory předávací ověřování."
services: active-directory
keywords: "Azure AD Connect předávací ověřování, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
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
ms.openlocfilehash: e1bd58797124210f7c31e90fb20d728289a04ba2
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-pass-through-authentication-frequently-asked-questions"></a>Azure Active Directory předávací ověřování: Nejčastější dotazy

V tomto článku jsme adres nejčastější dotazy k Azure Active Directory (Azure AD) předávací ověřování. Kontrolovat zpět pro nový obsah.

## <a name="which-of-the-azure-ad-sign-in-methods---pass-through-authentication-password-hash-synchronization-and-active-directory-federation-services-ad-fs---should-i-choose"></a>Která z Azure AD přihlášení metody - předávací ověřování, synchronizaci hodnoty Hash hesla a služby Active Directory Federation Services (AD FS) - by měl vybrat?

To závisí na místním prostředí a požadavků organizace. Přečtěte si tento článek [porovnání různé Azure AD přihlášení metody](active-directory-aadconnect-user-signin.md).

## <a name="is-pass-through-authentication-a-free-feature"></a>Předávací ověřování je bezplatná funkce?

Předávací ověřování je bezplatné funkce a nepotřebujete žádné placené edice Azure AD pro použití.

## <a name="is-pass-through-authentication-available-in-microsoft-cloud-germanyhttpwwwmicrosoftdecloud-deutschland-and-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Je k dispozici v předávací ověřování [Microsoft Cloud Německo](http://www.microsoft.de/cloud-deutschland) a [cloudu Microsoft Azure Government](https://azure.microsoft.com/features/gov/)?

Ne, předávací ověřování je k dispozici pouze v celém světě instanci Azure AD.

## <a name="does-conditional-accessactive-directory-conditional-accessmd-work-with-pass-through-authentication"></a>Nemá [podmíněného přístupu](../active-directory-conditional-access.md) pracovní pomocí předávacího ověřování?

Ano, všechny funkce podmíněného přístupu, včetně ověřování Azure Multi-Factor Authentication, pracovat s předávací ověřování.

## <a name="does-pass-through-authentication-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Podporuje předávací ověřování "Alternativní ID" jako uživatelské jméno, místo "userPrincipalName"?

Ano. Předávací ověřování podporuje `Alternate ID` jako uživatelské jméno při konfiguraci ve službě Azure AD Connect, jak je znázorněno [zde](active-directory-aadconnect-get-started-custom.md). Ne všechny aplikace Office 365 podporují `Alternate ID`. Vyhledejte konkrétní aplikaci dokumentaci pro příkaz podpory.

## <a name="does-password-hash-synchronization-act-as-a-fallback-to-pass-through-authentication"></a>Synchronizaci hodnoty Hash hesla fungovat jako nouzové řešení předávací ověřování?

Ne, předávací ověřování _nemá_ automaticky převzetí služeb při selhání pro synchronizaci hodnoty Hash hesla. Pouze slouží jako záložní pro [scénáře, které předávací ověřování nepodporuje Dnes](active-directory-aadconnect-pass-through-authentication-current-limitations.md#unsupported-scenarios). Aby se zabránilo neúspěšných přihlášení uživatele, měli byste nakonfigurovat předávací ověřování pro [vysokou dostupnost](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="can-i-install-an-azure-ad-application-proxyactive-directory-application-proxy-get-startedmd-connector-on-the-same-server-as-a-pass-through-authentication-agent"></a>Je možné nainstalovat [Azure AD Application Proxy](../active-directory-application-proxy-get-started.md) konektor na stejném serveru jako Agent předávací ověřování?

Ano, tato konfigurace je podporovaná v přejmenované verzích Agent předávací ověřování (verze 1.5.193.0 nebo novější).

## <a name="what-versions-of-azure-ad-connect-and-pass-through-authentication-agent-do-you-need"></a>Jaké verze Azure AD Connect a předávací ověřování agenta potřebujete?

Je třeba verze 1.1.486.0 nebo novější pro Azure AD Connect a 1.5.58.0 nebo novější pro předávací ověřování agenta pro tuto funkci fungovaly. Veškerý software by měly být nainstalovány na serverech se systémem Windows Server 2012 R2 nebo novější.

## <a name="what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-using-pass-through-authentication"></a>Co se stane, pokud vypršela platnost hesla pro daného uživatele a zkuste se přihlásit pomocí předávacího ověřování?

Pokud jste nakonfigurovali [zpětný zápis hesla](../active-directory-passwords-update-your-own-password.md) pro konkrétního uživatele, a pokud se uživatel přihlásí pomocí předávacího ověřování, můžou změnit a resetovat jejich hesla. Hesla jsou zapsány zpět na místní služby Active Directory podle očekávání.

Ale pokud zpětný zápis hesla není nakonfigurován pro konkrétního uživatele nebo pokud uživatel nemá platný Azure AD licence přiřazen, uživatel nemůže aktualizovat heslo, v cloudu. Jejich nelze aktualizovat heslo, i v případě, že vypršela platnost jejich hesla. Uživateli se místo toho zobrazí tato zpráva: "vaše organizace vám neumožňuje aktualizujte své heslo na tomto webu. Proveďte aktualizaci způsobem doporučovaným vaší organizací, nebo požádejte správce, pokud potřebujete pomoc." Uživatel nebo správce má resetovat heslo v místní Active Directory.

## <a name="how-does-pass-through-authentication-protect-you-against-brute-force-password-attacks"></a>Jak předávací ověřování ochraně před útoky hrubou silou heslo?

Čtení [v tomto článku](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) Další informace.

## <a name="what-do-pass-through-authentication-agents-communicate-over-ports-80-and-443"></a>Co předávací ověřování agenti komunikovat přes porty 80 a 443?

- Agenti ověřování provádět požadavky HTTPS přes port 443 pro všechny operace funkce.
- Agenti ověřování provádět požadavky HTTP přes port 80 stáhnout seznamy odvolaných certifikátů protokolu SSL.

     >[!NOTE]
     >Poslední aktualizace jsme snižuje počet portů požadovaných funkcí. Pokud máte starší verze Azure AD Connect nebo agenta ověřování, nechte těchto portů také otevřené: 5671, 8080, 9090, 9091, 9350, 9352 a 10100 10120.

## <a name="can-the-pass-through-authentication-agents-communicate-over-an-outbound-web-proxy-server"></a>Předávací ověřování agenti komunikovat přes odchozí webového proxy serveru?

Ano. Pokud WPAD (Web Proxy Auto-Discovery) je povoleno v místním prostředí, ověřování agentů automaticky pokusí najít a používají webový proxy server v síti.

## <a name="can-i-install-two-or-more-pass-through-authentication-agents-on-the-same-server"></a>Můžete nainstalovat dvě nebo více agentů předávací ověřování na stejném serveru?

Ne, můžete nainstalovat pouze jeden Agent předávací ověřování na jednom serveru. Pokud chcete nakonfigurovat předávací ověřování pro zajištění vysoké dostupnosti, postupujte podle pokynů v tomto [článku](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability) místo.

## <a name="i-already-use-active-directory-federation-services-ad-fs-for-azure-ad-sign-in-how-do-i-switch-it-to-pass-through-authentication"></a>Používám již Active Directory Federation Services (AD FS) pro přihlášení k Azure AD. Jak lze přepnout ho předávací ověřování?

Pokud jste nakonfigurovali službu AD FS jako způsob přihlášení pomocí Průvodce službou Azure AD Connect, změňte metodu přihlášení uživatele na předávací ověřování. Tato změna umožňuje předávací ověřování u klienta a převede _všechny_ federovaný domény na spravované domény. Všechny následné přihlášení požadavky ve vašem klientovi jsou zpracovávány předávací ověřování. V současné době není nijak podporované v rámci Azure AD Connect používat kombinaci služby AD FS a předávací ověřování v různých doménách.

Pokud byl služby AD FS nakonfigurovaný jako metoda přihlašování _mimo_ Průvodce Azure AD Connect změnit předávací ověřování metoda přihlašování uživatele (z možnost "Nekonfigurujte"). Tato změna umožňuje předávací ověřování u klienta. Ale všechny domény federovaný i nadále používat službu AD FS pro přihlášení. Pomocí prostředí PowerShell ručně převést některé nebo všechny tyto federovaný domény spravované domény. Potom všechny požadavky přihlášení na spravované domény (_pouze_) jsou zpracovávány předávací ověřování.

>[!IMPORTANT]
>Předávací ověřování nemůže pracovat s přihlášení ke cloudové službě Azure AD users.

## <a name="can-i-use-pass-through-authentication-in-a-multi-forest-ad-environment"></a>Můžete použít předávací ověřování v prostředí s více doménovými strukturami AD?

Ano. Prostředí s více doménovými strukturami jsou podporovány, pokud existují vztahy důvěryhodnosti doménové struktury mezi doménovými strukturami vaší AD a v případě směrování přípon názvů je správně nakonfigurován.

## <a name="how-many-pass-through-authentication-agents-do-i-need-to-install"></a>Kolik předávací ověřování agentů je nutné nainstalovat?

Instalace více agentů předávací ověřování zajišťuje [vysokou dostupnost](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability). Neposkytuje ale deterministický vyrovnávání zátěže mezi agenty ověřování.

Vezměte v úvahu maximální a průměrné zatížení přihlášení požadavků, které byste měli vidět na klientovi. Jako srovnávací test může zpracovávat jednoho agenta ověřování než 300 000 k 400,000 ověření za sekundu na standardní 4 jádra procesoru, server se 16 GB paměti RAM. Pro většinu zákazníků dvě nebo tři agenty ověřování celkem postačí pro vysokou dostupnost a kapacity.

Doporučujeme nainstalovat agenty ověřování blízko řadičích domény ke zlepšení latence přihlášení.

## <a name="can-i-install-the-first-pass-through-authentication-agent-on-a-server-other-than-the-one-that-runs-azure-ad-connect"></a>Můžete nainstalovat první Agent předávací ověřování na jiném serveru než ten, který spouští Azure AD Connect?

Ne, tento scénář je _není_ podporována.

## <a name="how-many-pass-through-authentication-agents-should-i-install"></a>Kolik předávací ověřování agenty nainstalovat?

Doporučujeme, aby:

- Je celkem nainstalovat dvě nebo tři agenty ověřování. Tato konfigurace je dostatečné pro většinu zákazníků.
- Nainstalovat agenty ověřování na řadičích domény (nebo tak, jak blízko je nejdříve) ke zlepšení latence přihlášení.
- Zajistíte, že servery (kde ověřování jsou agenti nainstalovaní) jsou přidány do stejné doménové struktuře AD jako uživatele, jejichž hesla se musí ověřit.

>[!NOTE]
>Existuje omezení systému 12 ověřování agentů podle jednotlivých klientů.

## <a name="how-can-i-disable-pass-through-authentication"></a>Jak můžete zakázat předávací ověřování?

Spusťte znovu Průvodce službou Azure AD Connect a změňte metoda přihlašování uživatele z předávací ověřování na jinou metodu. Tato změna zakáže předávací ověřování u klienta a ze serveru odinstaluje agenta pro ověřování. Budete muset ručně odinstalovat agenty ověřování z jiných serverů.

## <a name="what-happens-when-i-uninstall-a-pass-through-authentication-agent"></a>Co se stane, když odinstalovat agenta předávací ověřování?

Odinstalování agenta předávací ověřování ze serveru způsobí zastavit přijímání žádostí o přihlášení. Zajistěte, abyste měli jiné ověřování agenta spuštěného před provedením této operace, aby se zabránilo rozdělení přihlášení uživatele z vašeho klienta.

## <a name="next-steps"></a>Další kroky
- [**Aktuální omezení** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) – zjistěte, jaké postupy se podporují, a ty, které nejsou.
- [**Rychlý Start** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – zprovoznění a systémem Azure AD předávací ověřování.
- [**Inteligentní uzamčení** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) -možnost konfigurace inteligentní uzamčení na vašeho klienta k ochraně uživatelské účty.
- [**Podrobné technické informace** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -pochopit, jak tato funkce funguje.
- [**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – zjistěte, jak řešit obvyklé problémy s funkcí.
- [**Podrobné informace zabezpečení** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -další hluboké technické informace o funkci.
- [**Azure AD bezproblémové SSO** ](active-directory-aadconnect-sso.md) -Další informace o této funkci vzájemně doplňují.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.
