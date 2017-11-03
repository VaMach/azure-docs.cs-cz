---
title: "Omezení spolupráce Azure Active Directory s B2B | Microsoft Docs"
description: "Aktuální omezení pro spolupráci Azure Active Directory s B2B"
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
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 581e5d1fb5fb08d0dc89ed2c85edcb5f0005650b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a>Omezení spolupráce Azure AD B2B
Spolupráce Azure Active Directory (Azure AD) s B2B aktuálně podléhá omezení popsaná v tomto článku.

## <a name="possible-double-multi-factor-authentication"></a>Možné double aplikace Multi-Factor authentication
S B2B Azure AD můžete vynutit ověřování Multi-Factor authentication v organizaci prostředků (pozváním organizace). Důvody tohoto přístupu jsou podrobně popsané na [podmíněného přístupu pro uživatele spolupráce B2B](active-directory-b2b-mfa-instructions.md). Pokud partnera již obsahuje službu Multi-Factor authentication nastavit a vynutit, uživatelé pravděpodobně k provedení ověření jednou v jejich domovskou organizaci a potom znovu za vaše.

## <a name="instant-on"></a>Okamžitého
Toky spolupráce B2B nemůžeme přidat uživatele do adresáře a dynamicky aktualizovat během uplatnění pozvánku, aplikace přiřazení a tak dále. Aktualizace a zápisy normálně dojít v jednom adresáři instance a musí být replikována napříč všemi instancemi. Jakmile jsou aktualizovány všechny instance po dokončení replikace. V některých případech, pokud objekt se zapisují nebo aktualizuje v jedné instance a volání k načtení tohoto objektu je k jiné instanci, může dojít latenci replikace. Pokud k tomu dojde, aktualizovat nebo opakujte pomohou. Pokud píšete mobilní aplikace pomocí našem rozhraní API, opakování s některé back vypnout je dobré, Obranným postupem se tím tento problém.

## <a name="next-steps"></a>Další kroky

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Vlastnosti uživatele spolupráce B2B](active-directory-b2b-user-properties.md)
* [Přidání uživatele spolupráce B2B k roli](active-directory-b2b-add-guest-to-role.md)
* [Delegovat pozvánek B2bB spolupráce](active-directory-b2b-delegate-invitations.md)
* [Dynamické skupiny a spolupráci B2B](active-directory-b2b-dynamic-groups.md)
* [Kód spolupráce B2B a ukázky prostředí PowerShell](active-directory-b2b-code-samples.md)
* [Konfigurace aplikací SaaS pro spolupráci B2B](active-directory-b2b-configure-saas-apps.md)
* [Tokeny uživatele spolupráce B2B](active-directory-b2b-user-token.md)
* [Deklarace uživatele spolupráce B2B mapování](active-directory-b2b-claims-mapping.md)
* [Externí sdílení Office 365](active-directory-b2b-o365-external-user.md)
