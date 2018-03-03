---
title: "Mapování v Azure Active Directory deklarace identity uživatele spolupráce B2B | Microsoft Docs"
description: "referenční dokumentace pro Azure Active Directory s B2B spolupráce mapování deklarace identity"
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
ms.openlocfilehash: 3fe75e96c153a7cbcad638d606d0fbc248bd983a
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>Mapování v Azure Active Directory deklarace identity uživatele spolupráce B2B

Přizpůsobení deklarace identity vystavené v tokenu SAML pro uživatele spolupráce B2B Azure podporuje služby Active Directory (Azure AD). Když se uživatel přihlásí k aplikaci, Azure AD vydá SAML token na aplikaci, která obsahuje informace (nebo deklarace identity) o uživatele, který jednoznačně identifikuje je. Ve výchozím nastavení jedná se o uživatelské jméno, e-mailová adresa, jméno a příjmení uživatele. Můžete zobrazit nebo upravit deklarace identity odeslat v tokenu SAML, aby aplikace na kartě atributy.

Existují dvě možné důvody, proč je potřeba upravit deklarace identity vystavené v tokenu SAML.

1. Aplikace byla zapsána na vyžadují jinou sadu deklarací identity identifikátory URI nebo hodnot deklarací identity

2. Vaše aplikace vyžaduje NameIdentifier deklarace identity na něco jiného než hlavní název uživatele uložené v Azure Active Directory.

  ![Zobrazení deklarace identity v tokenu SAML](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

Informace o tom, jak přidávat a upravovat deklarace identity, podívejte se na tento článek na přizpůsobení deklarace identity, [přizpůsobení deklarace identity vystavené v tokenu SAML pro předběžně integrované aplikace v Azure Active Directory](develop/active-directory-saml-claims-customization.md). Pro spolupráci B2B uživatelů, mapování NameID a UPN mezi klienta nelze z bezpečnostních důvodů.


## <a name="next-steps"></a>Další postup

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Vlastnosti uživatele spolupráce B2B](active-directory-b2b-user-properties.md)
* [Přidání uživatele spolupráce B2B k roli](active-directory-b2b-add-guest-to-role.md)
* [Delegovat pozvánek B2bB spolupráce](active-directory-b2b-delegate-invitations.md)
* [Dynamické skupiny a spolupráci B2B](active-directory-b2b-dynamic-groups.md)
* [Kód spolupráce B2B a ukázky prostředí PowerShell](active-directory-b2b-code-samples.md)
* [Konfigurace aplikací SaaS pro spolupráci B2B](active-directory-b2b-configure-saas-apps.md)
* [Externí sdílení Office 365](active-directory-b2b-o365-external-user.md)
* [Tokeny uživatele spolupráce B2B](active-directory-b2b-user-token.md)
* [Aktuální omezení spolupráce B2B](active-directory-b2b-current-limitations.md)
