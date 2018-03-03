---
title: "Registrační portál samoobslužné služby pro spolupráci Azure Active Directory s B2B | Microsoft Docs"
description: "Spolupráce B2B ve službě Azure Active Directory podporuje vaše vztahy s ostatními společnostmi tím, že vašim obchodním partnerům umožní selektivní přístup ke podnikovým aplikacím"
services: active-directory
documentationcenter: 
author: twooley
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/24/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: bb63a3b23bdcaac5c94d43bb8e7294a82b0c3fa0
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Samoobslužný portál pro registraci spolupráce Azure AD B2B

Zákazníci můžete dělat mnohem s integrované funkce, které jsou k dispozici prostřednictvím správce IT [portál Azure](https://portal.azure.com) a [Panel přístupu aplikace](https://myapps.microsoft.com) pro koncové uživatele. Ale máme také informace, že podnikům potřeba přizpůsobení pracovního postupu registrace pro uživatele B2B podle potřeb jejich organizace. Dělají to pomocí [pozvánku rozhraní API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

Diskuze s zákazníků je jedna běžné potřeby roste až nad všemi ostatními. Pozváním organizace nemusí vědět předem, kdo jsou jednotlivé externími spolupracovníky kteří potřebují přístup ke svým prostředkům. Způsob, jak se chtěli pro uživatele z partnerských společností sami zaregistrovat pomocí sady zásad, které řídí pozváním organizace. Tento scénář je možné provádět prostřednictvím rozhraní API, takže jsme publikovaná projektu na Githubu, který se právě,: [ukázkový projekt Githubu](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

Tento projekt Githubu ukazuje, jak organizace pomocí rozhraní API a poskytovat na základě zásad, samoobslužné registrace funkce pro jejich důvěryhodným partnerům, pravidla, která určit aplikace, které mají přístup. Uživatelé partnera můžete získat přístup k prostředkům, kdy je potřebují, bezpečně, bez nutnosti jejich pozváním organizace ručně zařadit do provozu. Můžete snadno nasaďte projekt do předplatného služby Azure podle svého výběru.

## <a name="as-is-code"></a>Jako-je kód

Mějte na paměti, že tento kód je k dispozici jako ukázka k předvedení využití pozvánku Azure Active Directory s B2B rozhraní API. Je nutné přizpůsobit váš vývojový tým nebo partnera a by měl být zkontrolovány před nasazením v produkční scénář.

## <a name="next-steps"></a>Další postup

Procházet další články v spolupráce Azure AD B2B:
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