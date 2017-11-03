---
title: "Registrační portál samoobslužné služby pro spolupráci Azure Active Directory s B2B | Microsoft Docs"
description: "Spolupráce B2B ve službě Azure Active Directory podporuje vaše vztahy s ostatními společnostmi tím, že vašim obchodním partnerům umožní selektivní přístup ke podnikovým aplikacím"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: 307373c75bbb87cec683f7a3097f8f159c9d5e61
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Samoobslužný portál pro registraci spolupráce Azure AD B2B

Zákazníci můžete dělat mnohem s integrované funkce, které jsou k dispozici prostřednictvím našich správce IT [portál Azure](https://portal.azure.com) a na našem [Panel přístupu aplikace](https://myapps.microsoft.com) pro koncové uživatele. Ale máme také informace, že podnikům potřeba přizpůsobení pracovního postupu registrace pro uživatele B2B podle potřeb jejich organizace. Dělají to pomocí [našem rozhraní API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

Diskuze s našich zákazníků vidíte, že jeden běžné potřebovat zvýšení až nad všemi ostatními. Pozváním organizace nemusí vědět předem, kdo jsou jednotlivé externími spolupracovníky kteří potřebují přístup ke svým prostředkům. Způsob, jak se chtěli pro uživatele z partnerských společností sami zaregistrovat pomocí sady zásad, které řídí pozváním organizace. Tento scénář je možné provádět prostřednictvím rozhraní API, takže jsme publikovaná projektu na Githubu, který se právě který: [ukázkový projekt Githubu](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

Naše Githubu projektu ukazuje, jak můžete použít rozhraní API organizace a poskytovat na základě zásad, samoobslužné funkce registrace pro jejich důvěryhodným partnerům, pravidla, která určit aplikace, které získají přístup k. Uživatelé partnera můžete získat přístup k prostředkům, kdy je potřebují, bezpečně, bez nutnosti jejich pozváním organizace ručně zařadit do provozu. Můžete snadno nasaďte projekt do předplatného služby Azure podle svého výběru.

## <a name="as-is-code"></a>Jako-je kód

Mějte na paměti, že tento kód je k dispozici jako ukázka k předvedení využití pozvánku Azure Active Directory s B2B rozhraní API. Je nutné přizpůsobit svým týmem vývojářů nebo partnera a by měl být zkontrolovány před nasazením v produkční scénář.

## <a name="next-steps"></a>Další kroky

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:
* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Jak Azure Active Directory správci přidat uživatele spolupráce B2B?](active-directory-b2b-admin-add-users.md)
* [Jak informační pracovníci přidat uživatele spolupráce B2B?](active-directory-b2b-iw-add-users.md)
* [Elementy e-mail pozvánku spolupráce B2B](active-directory-b2b-invitation-email.md)
* [Uplatnění pozvánku spolupráce B2B](active-directory-b2b-redemption-experience.md)
* [Licencování Azure AD s B2B spolupráce](active-directory-b2b-licensing.md)
* [Řešení potíží s spolupráce Azure Active Directory s B2B](active-directory-b2b-troubleshooting.md)
* [Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)](active-directory-b2b-faq.md)
* [Vícefaktorové ověřování pro uživatele pro spolupráci B2B](active-directory-b2b-mfa-instructions.md)
* [Přidání uživatelů spolupráce B2B bez Pozvánka](active-directory-b2b-add-user-without-invite.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)