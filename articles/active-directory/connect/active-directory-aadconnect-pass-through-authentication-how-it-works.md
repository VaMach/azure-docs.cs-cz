---
title: "Azure AD Connect: Předávací ověřování – jak to funguje? | Dokumentace Microsoftu"
description: "Tento článek popisuje, jak funguje Azure Active Directory předávací ověřování."
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
ms.openlocfilehash: c7863e38671349b6424ee08330da8aaa49cb2a70
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Azure předávací ověřování služby Active Directory: Technické podrobné informace
V následujícím článku je přehled toho, jak funguje Azure AD předávací ověřování.  Přímý technické a informace o zabezpečení najdete v článku [ **deep Dive informace o zabezpečení** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) článku.

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Jak funguje Azure předávací ověřování služby Active Directory?

Když se uživatel pokusí přihlásit k aplikaci zabezpečené službou Azure Active Directory (Azure AD), a pokud je povolené předávací ověřování u klienta, jsou provedeny následující kroky:

1. Uživatel se pokusí o přístup k aplikaci (například aplikaci Outlook Web App - https://outlook.office365.com/owa/).
2. Pokud již není přihlášený uživatel, se uživatel přesměruje na přihlašovací stránku služby Azure AD.
3. Uživatel zadá své uživatelské jméno a heslo na přihlašovací stránku služby Azure AD a klikne na tlačítko "Přihlásit".
4. Azure AD, na přijme požadavek přihlášení umístí uživatelského jména a hesla (šifrované pomocí veřejného klíče) ve frontě.
5. Agent ověřování místní načte uživatelské jméno a heslo šifrované z fronty.
6. Agent dešifruje heslo pomocí jeho privátní klíč.
7. Agent pak ověří uživatelské jméno a heslo pro službu Active Directory pomocí standardních API systému Windows (podobně jako mechanismus pro co je používán Active Directory Federation Services). Uživatelské jméno může být buď místní výchozí uživatelské jméno (obvykle `userPrincipalName`) nebo jiný atribut, které jsou nakonfigurované v Azure AD Connect (označované jako `Alternate ID`).
8. V místní službě Active Directory řadiči domény (DC) pak vyhodnotí žádost a vrátí odpovídající odpověď (úspěch, chyba, platnost hesla nebo uzamčení uživatele) k agentovi.
9. Agent ověřování pak vrátí tuto odpověď zpět do Azure AD.
10. Azure AD vyhodnotí odpovědi a odpovídat na uživatele podle potřeby – například ji okamžitě přihlásí uživatel nebo požadavky pro multi-Factor Authentication (MFA).
11. Pokud přihlášení uživatele je úspěšné, uživatel je možné získat přístup k aplikaci.

Následující diagram znázorňuje všechny součásti a kroky.

![Předávací ověřování](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a>Další kroky
- [**Aktuální omezení** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) – zjistěte, jaké postupy se podporují, a ty, které nejsou.
- [**Rychlý Start** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – zprovoznění a systémem Azure AD předávací ověřování.
- [**Inteligentní uzamčení** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) -možnost konfigurace inteligentní uzamčení na vašeho klienta k ochraně uživatelské účty.
- [**Nejčastější dotazy** ](active-directory-aadconnect-pass-through-authentication-faq.md) -odpovědi na nejčastější dotazy.
- [**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – zjistěte, jak řešit obvyklé problémy s funkcí.
- [**Podrobné informace zabezpečení** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -další hluboké technické informace o funkci.
- [**Azure AD bezproblémové SSO** ](active-directory-aadconnect-sso.md) -Další informace o této funkci vzájemně doplňují.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.
