---
title: "Azure AD Connect: Předávací ověřování – aktuální omezení | Microsoft Docs"
description: "Tento článek popisuje aktuální omezení předávací ověřování Azure Active Directory (Azure AD)."
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
ms.openlocfilehash: 4a33df43ca218545d6c684103a64f2cd1460913b
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure předávací ověřování služby Active Directory: Aktuální omezení

>[!IMPORTANT]
>Azure AD předávací ověřování je bezplatné funkce a nepotřebujete žádné placené edice Azure AD pro použití. Předávací ověřování je k dispozici v celém světě instanci Azure AD a ne v pouze [Microsoft Cloud Německo](http://www.microsoft.de/cloud-deutschland) a [cloudu Microsoft Azure Government](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Podporované scénáře

Plně podporuje následující scénáře:

- Přihlášení uživatele do všech webových aplikací založené na prohlížeči.
- Uživatelská přihlášení do služeb Office 365 klientské aplikace, které podporují [moderní ověřování](https://aka.ms/modernauthga) -Office 2016 a Office 2013 _s_ moderní ověřování.
- Azure AD Join pro zařízení s Windows 10.
- Podpora protokolu Exchange ActiveSync.

## <a name="unsupported-scenarios"></a>Nepodporované scénáře

Následující scénáře jsou _není_ podporovány:

- Přihlášení uživatele do starší verze aplikace klienta Office - Office 2010 a Office 2013 _bez_ moderní ověřování). Organizace doporučujeme přepnout na moderní ověřování, pokud je to možné. Moderní ověřování umožňuje podporu předávací ověřování, ale také pomáhá se zabezpečením vašich uživatelských účtů pomocí [podmíněného přístupu](../active-directory-conditional-access-azure-portal.md) funkce jako je například služba Multi-Factor Authentication (MFA).
- Uživatelská přihlášení do Skype pro firmy klientské aplikace, včetně Skype pro firmy 2016.
- Přihlášení uživatele do prostředí PowerShell 1.0. Doporučuje se místo toho použít PowerShell v2.0.
- Služba Azure AD Domain Services.
- Hesla aplikací pro MFA.
- Zjišťování uživatelů s [úniku přihlašovacích údajů](../active-directory-reporting-risk-events.md#leaked-credentials).

>[!IMPORTANT]
>Jako alternativní řešení pro nepodporované scénáře _pouze_, povolte synchronizaci hodnoty Hash hesla na [volitelné funkce](active-directory-aadconnect-get-started-custom.md#optional-features) stránku průvodce Azure AD Connect. Povolení synchronizaci hodnoty Hash hesla také vám dává možnost převzetí služeb při selhání ověřování Pokud na místní infrastrukturu úplně dojde k narušení. Toto převzetí služeb při selhání z předávací ověřování synchronizaci hodnoty Hash hesla není automatická, ale k dokončení za pomoci Microsoft Support.

## <a name="next-steps"></a>Další kroky
- [**Rychlý Start** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – zprovoznění a systémem Azure AD předávací ověřování.
- [**Inteligentní uzamčení** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) -možnost konfigurace inteligentní uzamčení na vašeho klienta k ochraně uživatelské účty.
- [**Podrobné technické informace** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -pochopit, jak tato funkce funguje.
- [**Nejčastější dotazy** ](active-directory-aadconnect-pass-through-authentication-faq.md) -odpovědi na nejčastější dotazy.
- [**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – zjistěte, jak řešit obvyklé problémy s funkcí.
- [**Podrobné informace zabezpečení** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -další hluboké technické informace o funkci.
- [**Azure AD bezproblémové SSO** ](active-directory-aadconnect-sso.md) -Další informace o této funkci vzájemně doplňují.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.
