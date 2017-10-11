---
title: "Delegovat pozvánky pro spolupráci Azure Active Directory s B2B | Microsoft Docs"
description: "Vlastnosti uživatelů služby Active Directory s B2B spolupráce se dají konfigurovat"
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
ms.openlocfilehash: 78613cc978b585a98d235245194c02371f7f3849
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a>Delegát pozvánky pro spolupráci Azure Active Directory s B2B

Se spoluprací business-to-business (B2B) Azure Active Directory (Azure AD) není nutné být globálním správcem odeslat pozvánky. Místo toho můžete použít zásady a delegovat pozvánek uživatelům, jejichž role umožňují odeslání pozvánky. Je důležité nové možné delegovat pozvánek uživatele guest prostřednictvím role pozvání hosta odeslal.

## <a name="guest-inviter-role"></a>Pozvání hosta odeslal role
Jsme můžete přiřadit uživatele k roli pozvání hosta odeslal odeslat pozvánky. Nemusíte být členem role globálního správce odeslat pozvánky. Ve výchozím nastavení můžete běžní uživatelé také vyvolat rozhraní API pozvání, pokud není globální správce zakázáno pozvánky k běžní uživatelé. Uživatele můžete také vyvolat rozhraní API pomocí portálu Azure nebo Powershellu.

Tady je příklad, který ukazuje, jak přidat uživatele k roli pozvání hosta odeslal pomocí prostředí PowerShell:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a>Ovládací prvek, který můžete pozvat

![Řídí pozvat](media/active-directory-b2b-delegate-invitations/control-who-to-invite.png)

S spolupráce Azure AD B2B správce klienta můžete nastavit tyto zásady pozvání:

- Vypnout pozvánek
- Pouze správci a uživatelé v roli hosta pozvánky můžete pozvat
- Můžete pozvat správci, pozvání hosta odeslal role a členů
- Všichni uživatelé, včetně hostů, můžete pozvat

Ve výchozím nastavení jsou klienti nastavené na #4. (Všechny uživatele, včetně hostů, můžete uživatele pozvat, B2B.)

## <a name="next-steps"></a>Další kroky

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Vlastnosti uživatele spolupráce B2B](active-directory-b2b-user-properties.md)
* [Přidání uživatele spolupráce B2B k roli](active-directory-b2b-add-guest-to-role.md)
* [Dynamické skupiny a spolupráci B2B](active-directory-b2b-dynamic-groups.md)
* [Kód spolupráce B2B a ukázky prostředí PowerShell](active-directory-b2b-code-samples.md)
* [Konfigurace aplikací SaaS pro spolupráci B2B](active-directory-b2b-configure-saas-apps.md)
* [Tokeny uživatele spolupráce B2B](active-directory-b2b-user-token.md)
* [Deklarace uživatele spolupráce B2B mapování](active-directory-b2b-claims-mapping.md)
* [Externí sdílení Office 365](active-directory-b2b-o365-external-user.md)
* [Aktuální omezení spolupráce B2B](active-directory-b2b-current-limitations.md)
