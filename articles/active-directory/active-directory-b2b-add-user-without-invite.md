---
title: "Přidání uživatelů spolupráce B2B do Azure Active Directory bez pozvánku | Microsoft Docs"
description: "Můžete je nechat uživatele guest, přidat další uživatele typu Host do služby Azure AD bez uplatňuje Pozvánka v Azure Active Directory s B2B spolupráce."
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
ms.date: 03/15/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 68d828b8fb23f7074be5fe691a4250d0fa653ab7
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a>Přidat uživatele typu Host spolupráce B2B bez Pozvánka

Můžete povolit uživateli, jako je například zástupce partnera, přidat uživatele z partnera pro vaši organizaci bez nutnosti pozvánek k uplatnit. Jediné, co musíte udělat je uživateli udělit oprávnění výčet v adresáři, kterou používáte pro org. partnera 

Udělení těchto oprávnění, když:

1. Uživatel v organizaci hostitele (například WoodGrove) vyzývá jeden uživatel od partnerské organizace (například Sam@litware.com) jako hosta.
2. Správce v organizaci hostitele nastavuje zásady, které umožňují Sam identifikovat a přidat další uživatele od organizace partnera (Litware).
3. Nyní Sam můžete přidat další uživatele z Litware do adresáře WoodGrove, skupiny nebo aplikace bez nutnosti pozvánek k uplatnit. Pokud Sam má odpovídající výčtu oprávnění v Litware, probíhá automaticky.

### <a name="next-steps"></a>Další postup

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Jak Azure Active Directory správci přidat uživatele spolupráce B2B?](active-directory-b2b-admin-add-users.md)
* [Jak informační pracovníci přidat uživatele spolupráce B2B?](active-directory-b2b-iw-add-users.md)
* [Elementy e-mail pozvánku spolupráce B2B](active-directory-b2b-invitation-email.md)
* [Uplatnění pozvánku spolupráce B2B](active-directory-b2b-redemption-experience.md)
* [Licencování Azure AD s B2B spolupráce](active-directory-b2b-licensing.md)
* [Řešení potíží s spolupráce Azure Active Directory s B2B](active-directory-b2b-troubleshooting.md)
* [Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)](active-directory-b2b-faq.md)
* [Spolupráce Azure Active Directory s B2B rozhraní API a přizpůsobení](active-directory-b2b-api.md)
* [Vícefaktorové ověřování pro uživatele pro spolupráci B2B](active-directory-b2b-mfa-instructions.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)