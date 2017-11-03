---
title: "Azure AD Connect: Bezproblémové Single Sign-On - jak to funguje | Microsoft Docs"
description: "Tento článek popisuje, jak funkce Azure Active Directory bezproblémové jednotné přihlašování funguje."
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
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: 38b107513e72635fd034bb86d0d866bcb0fcb8e4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-technical-deep-dive"></a>Azure Active Directory bezproblémové jednotné přihlašování: podrobné technické informace

Tento článek poskytuje podrobné technické informace do Princip funkce Azure Active Directory bezproblémové jednotné přihlašování (SSO bezproblémové).

## <a name="how-does-seamless-sso-work"></a>Jak funguje bezproblémové jednotné přihlašování?

Tato část obsahuje dvě části:
1. Instalace funkce bezproblémové jednotné přihlašování.
2. Jak funguje jednoho uživatele přihlásit transakce pomocí bezproblémové jednotného přihlašování.

### <a name="how-does-set-up-work"></a>Jak nastavit pracovní?

Bezproblémové jednotného přihlašování je povoleno pomocí Azure AD Connect, jak je znázorněno [zde](active-directory-aadconnect-sso-quick-start.md). Při povolení funkce, jsou provedeny následující kroky:
- Účet počítače s názvem `AZUREADSSOACC` (která představuje Azure AD) je vytvořen v místní službě Active Directory (AD).
- Účet počítače pomocí protokolu Kerberos dešifrovací klíč je bezpečně sdílet s Azure AD.
- Kromě toho jsou dvě Kerberos hlavní názvy služby (SPN) vytvoří představují dvou adres URL, které se používají při přihlášení k Azure AD.

>[!NOTE]
> V každé doménové struktuře AD synchronizovat do Azure AD (přes Azure AD Connect) a pro uživatele, jehož chcete bezproblémové jednotné přihlašování se vytvoří účet počítače a SPN protokolu Kerberos. Přesunout `AZUREADSSOACC` účet počítače pro organizaci jednotce (OU) ukládat další účty počítačů zajistit, že je spravovat stejným způsobem a není odstraněn.

>[!IMPORTANT]
>Důrazně doporučujeme, aby vám [mění dešifrovací klíč protokolu Kerberos](active-directory-aadconnect-sso-faq.md#how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account) z `AZUREADSSOACC` účet počítače minimálně každých 30 dnů.

### <a name="how-does-sign-in-with-seamless-sso-work"></a>Jak Přihlaste se pomocí pracovních bezproblémové jednotné přihlašování?

Po dokončení instalace bezproblémové jednotné přihlašování funguje stejně jako všechny ostatní přihlášení používající integrované ověřování systému Windows (IWA). Postup je následující:

1. Uživatel se pokusí o přístup k aplikaci (například aplikaci Outlook Web App - https://outlook.office365.com/owa/) z připojených k doméně podnikové zařízení uvnitř firemní sítě.
2. Pokud již není přihlášený uživatel, se uživatel přesměruje na přihlašovací stránku služby Azure AD.

  >[!NOTE]
  >Pokud Azure AD přihlášení požadavek obsahuje `domain_hint` (například identifikaci klienta -, contoso.onmicrosoft.com) nebo `login_hint` (Identifikace uživatel – třeba user@contoso.onmicrosoft.com nebo user@contoso.com) parametr a potom krok 2 bude přeskočena.

3. Typy uživatelů ve své uživatelské jméno na přihlašovací stránku služby Azure AD.
4. Azure AD pomocí jazyka JavaScript na pozadí, vyzve prohlížeče, prostřednictvím 401 neoprávněný odpověď, k poskytování lístek protokolu Kerberos.
5. Prohlížeč, pak vyžádá lístek ze služby Active Directory pro `AZUREADSSOACC` účet počítače (což představuje Azure AD).
6. Služby Active Directory vyhledá účet počítače a vrátí lístek protokolu Kerberos v prohlížeči šifrován tajný klíč pro účet počítače.
7. V prohlížeči předává lístek protokolu Kerberos je získat ze služby Active Directory do Azure AD (v jednom z [Azure AD adresy URL, dříve přidány do nastavení zóny intranetu prohlížeče](active-directory-aadconnect-sso-quick-start.md#step-3-roll-out-the-feature)).
8. Azure AD dešifruje lístek protokolu Kerberos, která zahrnuje identitu uživatele přihlášeni firemních zařízení pomocí dříve sdílený klíč.
9. Po vyhodnocení, Azure AD buď vrátí token zpět do aplikace nebo vyzývá uživatel k provedení dodatečné důkazy, jako je Vícefaktorové ověřování.
10. Pokud přihlášení uživatele je úspěšné, uživatel je možné získat přístup k aplikaci.

Následující diagram znázorňuje všechny součásti a kroky.

![Bezproblémové jednotného přihlašování](./media/active-directory-aadconnect-sso/sso2.png)

Bezproblémové jednotného přihlašování je oportunistické, což znamená, že pokud se nezdaří, přihlašování uživatelů spadne zpět na regulární chování – tj, uživatel musí zadat svoje heslo k přihlášení.

## <a name="next-steps"></a>Další kroky

- [**Rychlý Start** ](active-directory-aadconnect-sso-quick-start.md) – zprovoznění a systémem Azure bezproblémové jednotného přihlašování k AD.
- [**Nejčastější dotazy** ](active-directory-aadconnect-sso-faq.md) -odpovědi na nejčastější dotazy.
- [**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-sso.md) – zjistěte, jak řešit obvyklé problémy s funkcí.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.
