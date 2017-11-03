---
title: 'Azure AD Connect: Funkce ve verzi preview | Microsoft Docs'
description: "Toto téma popisuje v další podrobnosti o funkcích, které jsou ve verzi preview v Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: c75cd8cf-3eff-4619-bbca-66276757cc07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: cbf8f729d0ebfb271bb0d8702ac043442b42c262
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="more-details-about-features-in-preview"></a>Další informace o funkcích ve verzi preview
Toto téma popisuje, jak používat funkce, které jsou aktuálně ve verzi preview.

## <a name="group-writeback"></a>Zpětný zápis skupin
Možnosti pro zpětný zápis skupin v volitelné funkce umožňuje zpětný zápis **skupiny Office 365** do doménové struktury s Exchange nainstalované. Toto je skupinu, která řídí se vždy hlavním v cloudu. Pokud máte místní Exchange, pak můžete napsat zpět tyto skupiny do místního uživatele s poštovní schránky systému Exchange místně odesílat a přijímat e-maily z těchto skupin.

Další informace o skupiny Office 365 a jejich použití naleznete [zde](http://aka.ms/O365g).

Skupiny služby Office 365 je reprezentován jako distribuční skupiny v místní službě AD DS. Vaše místní Exchange server musí být na serveru Exchange 2013 kumulativní aktualizaci 8 (vydané v března 2015) nebo Exchange 2016 rozpoznat tento nový typ skupiny.

**Poznámky k ve verzi Preview**

* Atribut kniha adresa není aktuálně naplněna ve verzi preview. Bez tohoto atributu skupiny není zobrazená v globálním seznamu adres. Nejjednodušší způsob, jak naplnit tento atribut je pomocí rutiny Exchange PowerShell `update-recipient`.
* Pouze doménové struktury se schématem systému Exchange jsou platné cíle pro skupiny. Pokud byla zjištěna žádná Exchange, není zpětný zápis skupin povolit.
* Aktuálně jsou podporovány pouze jednou doménovou strukturou nasazení organizaci Exchange. Pokud máte více než jednu organizaci místní Exchange, musíte do místní GALSync řešení pro tyto skupiny se objeví ve vašem jiných doménových strukturách.
* Funkce zpětného zápisu skupiny nezpracovává skupiny zabezpečení nebo distribuční skupiny.

> [!NOTE]
> Předplatné služby Azure AD Premium je vyžadována pro zpětný zápis skupin.
> 
>

## <a name="user-writeback"></a>Zpětný zápis uživatelů.
> [!IMPORTANT]
> Funkce preview zpětný zápis uživatelů byla odebrána v srpnu 2015 aktualizaci na Azure AD Connect. Pokud jste ho povolili, měli byste zakázat tuto funkci.
>
>

## <a name="next-steps"></a>Další kroky
Pokračovat vaše [vlastní instalace Azure AD Connect](active-directory-aadconnect-get-started-custom.md).

Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
