---
title: "Co je spolupráce Azure Active Directory B2B? | Dokumenty Microsoft"
description: "Spolupráce Azure Active Directory s B2B podporuje vaše vztahy povolením obchodním partnerům umožní selektivní přístup k podnikovým aplikacím."
services: active-directory
documentationcenter: 
author: twooley
manager: mtillman
editor: 
tags: 
ms.assetid: 1464387b-ee8b-4c7c-94b3-2c5567224c27
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 06/27/2017
ms.author: twooley
ms.custom: aaddev
ms.reviewer: sasubram
ms.openlocfilehash: e1fa0a3e32935a8d601eac83c19c6c2b91ded210
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="what-is-azure-ad-b2b-collaboration"></a>Co je spolupráce B2B ve službě Azure AD?

>[!VIDEO https://www.youtube.com/embed/AhwrweCBdsc]

Možnosti (B2B) spolupráce Azure AD business-to-business povolit všechny organizace pomocí služby Azure AD pro práci s uživatele z jakékoli jiné organizaci, malý nebo velký bezpečně a bezpečně. Tyto organizace může být s Azure AD nebo bez, nebo dokonce i s IT organizace nebo bez. 

Organizace pomocí služby Azure AD může poskytnout přístup k dokumentům, prostředky a aplikace na jejich partnery, při zachování plnou kontrolu nad své vlastní firemní data. Vývojáři mohou pomocí Azure AD business-to-business rozhraní API pro psaní aplikací, u kterých se dvě organizace v další bezpečně. Je také velmi snadné pro koncové uživatele pro navigaci.

97 % naše zákazníky jste se dozvěděli, že je velmi důležité, abyste je spolupráce Azure AD B2B.

![Výsečový graf](media/active-directory-b2b-what-is-azure-ad-b2b/97-percent-support.png)

Od verze časná dubna 2017 jsme měli o 3 miliony uživatelů již pomocí možnosti spolupráce Azure AD B2B. A maximálně 23 % organizace Azure AD, které mají víc než 10 uživateli již využily tyto možnosti.

## <a name="the-key-benefits-of-azure-ad-b2b-collaboration-to-your-organization"></a>Hlavní výhody spolupráce Azure AD B2B pro vaši organizaci

### <a name="work-with-any-user-from-any-partner"></a>Práce s všechny uživatele z partnerských

* Partneři používat svoje vlastní přihlašovací údaje

* Nevyžaduje se pro partnery používat Azure AD

* Žádné externí adresáře nebo komplexní nastavení vyžaduje

### <a name="simple-and-secure-collaboration"></a>Jednoduché a bezpečné spolupráce

* Poskytnout přístup k žádné podnikové aplikace nebo data, při použití sofistikované, Azure AD zapnuté autorizační zásady

* Snadné pro uživatele

* Podnikové úrovni zabezpečení pro aplikace a data

### <a name="no-management-overhead"></a>Bez režie na správu

* Externí správu účet nebo heslo

* Žádná synchronizace nebo ruční správa životního cyklu účtu

* Žádné externí režijní náklady na správu

## <a name="you-can-easily-add-b2b-collaboration-users-to-your-organization"></a>Můžete snadno přidat uživatele spolupráce B2B pro vaši organizaci

Správci mohou přidat uživatele spolupráce (Host) B2B [portál Azure](https://portal.azure.com).

![Přidat uživatele typu Host](media/active-directory-b2b-what-is-azure-ad-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-to-bring-their-own-identity"></a>Povolit vaší spolupracovníci aby své vlastní identity

B2B spolupracovníci se můžete přihlásit pomocí identity si sami vyberou. Pokud uživatel nemá účet Microsoft nebo účtu Azure AD – jedna se vytvoří pro ně bezproblémově v době pro uplatnění nabídka.

![Volba identity přihlášení](media/active-directory-b2b-what-is-azure-ad-b2b/sign-in-identity-choice.png)

### <a name="delegate-to-application-and-group-owners"></a>Delegovat na aplikace a vlastníků skupiny 
Aplikace a vlastníci skupiny můžete přidat B2B uživatele přímo do jakékoli aplikace, která se zajímáte o, zda je aplikace Microsoft, nebo ne. Správci mohou delegovat oprávnění k přidání uživatele B2B do bez oprávnění správce. Můžete použít bez práv správce [Azure AD Application přístupový Panel](https://myapps.microsoft.com) k přidání uživatele spolupráce B2B do skupiny nebo aplikace.

![panel přístupu](media/active-directory-b2b-what-is-azure-ad-b2b/access-panel.png)

![Přidat člena](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a>Zásady autorizace chránit vaše firemní obsah

Zásady podmíněného přístupu, jako je například služba Multi-Factor authentication, je možné vynutit:
- Na úrovni klienta
- Na úrovni aplikace
- Pro konkrétní uživatele chránit podnikové aplikace a data

![Přidat člena](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="use-our-apis-and-sample-code-to-easily-build-applications-to-onboard"></a>Pomocí rozhraní API a ukázkový kód snadno vytvářet aplikace zařadit do provozu
Přepněte externími partnery na palubě způsoby přizpůsobit potřebám vaší organizace.

Pomocí [pozvánku spolupráce B2B rozhraní API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation), můžete přizpůsobit své zkušenosti registrace, včetně vytváření samoobslužné portály registrace. Poskytujeme ukázkový kód pro samoobslužný portál [na Githubu](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

![registrační portál](media/active-directory-b2b-what-is-azure-ad-b2b/sign-up-portal.png)

S spolupráce Azure AD B2B můžete získat úplné power Azure AD k ochraně vaše vztahy s partnery tak, aby koncoví uživatelé najít snadný a intuitivní. Proto pokračujte, připojte na tisíce organizací, které už používají Azure AD B2B pro jejich externí spolupráce!

## <a name="next-steps"></a>Další postup

* Prostředí správce se nacházejí v [portál Azure](https://portal.azure.com).

* Informace o pracovním prostředí jsou k dispozici v [přístupový Panel](https://myapps.microsoft.com).

* A jako vždy připojit s produktový tým pro všechny zpětnou vazbu, diskusí a návrhy prostřednictvím našich [Microsoft technická komunita](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Jak Azure Active Directory správci přidat uživatele spolupráce B2B?](active-directory-b2b-admin-add-users.md)
* [Jak informační pracovníci přidat uživatele spolupráce B2B?](active-directory-b2b-iw-add-users.md)
* [Elementy e-mail pozvánku spolupráce B2B](active-directory-b2b-invitation-email.md)
* [Uplatnění pozvánku spolupráce B2B](active-directory-b2b-redemption-experience.md)
* [Licencování Azure AD s B2B spolupráce](active-directory-b2b-licensing.md)
* [Řešení potíží s spolupráce Azure Active Directory s B2B](active-directory-b2b-troubleshooting.md)
* [Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)](active-directory-b2b-faq.md)
* [Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení](active-directory-b2b-api.md)
* [Vícefaktorové ověřování pro uživatele pro spolupráci B2B](active-directory-b2b-mfa-instructions.md)
* [Přidání uživatelů spolupráce B2B bez Pozvánka](active-directory-b2b-add-user-without-invite.md)
* [Uživatel spolupráce B2B auditování a vytváření sestav](active-directory-b2b-auditing-and-reporting.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
