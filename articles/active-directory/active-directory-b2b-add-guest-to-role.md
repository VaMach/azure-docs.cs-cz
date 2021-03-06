---
title: "Přidání uživatele spolupráce Azure Active Directory s B2B roli | Microsoft Docs"
description: "Přidat uživatele guest do role v Azure Active Directory"
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
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b788b003299ff11bcbdcb11a2c7a21ac5081d634
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="grant-permissions-to-users-from-partner-organizations-in-your-azure-active-directory-tenant"></a>Udělení oprávnění uživatelům z partnerských organizací ve vašem klientu Azure Active Directory

Uživatelé spolupráce Azure Active Directory (Azure AD) s B2B jsou přidány jako uživatele typu Host k adresáři a jsou ve výchozím nastavení omezuje oprávnění hostovaného v adresáři. Vaší firmy může být nutné některé uživatele typu Host k vyplnění vyšší oprávnění role ve vaší organizaci. Pro podporu, definování rolí vyšší oprávnění, můžete přidat uživatele typu Host k žádné roli požadavky, na základě potřeb vaší organizace.

## <a name="default-role"></a>Výchozí role

![Výchozí role](./media/active-directory-b2b-add-guest-to-role/default-role.png)

## <a name="global-administrator-role"></a>Role Globální správce

![role Globální správce](./media/active-directory-b2b-add-guest-to-role/global-admin-role.png)

## <a name="limited-administrator-role"></a>Role omezené správce

![role omezené správce](./media/active-directory-b2b-add-guest-to-role/limited-admin-role.png)

## <a name="next-steps"></a>Další postup

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Vlastnosti uživatele spolupráce B2B](active-directory-b2b-user-properties.md)
* [Delegovat pozvánek B2bB spolupráce](active-directory-b2b-delegate-invitations.md)
* [Dynamické skupiny a spolupráci B2B](active-directory-b2b-dynamic-groups.md)
* [Kód spolupráce B2B a ukázky prostředí PowerShell](active-directory-b2b-code-samples.md)
* [Konfigurace aplikací SaaS pro spolupráci B2B](active-directory-b2b-configure-saas-apps.md)
* [Tokeny uživatele spolupráce B2B](active-directory-b2b-user-token.md)
* [Deklarace uživatele spolupráce B2B mapování](active-directory-b2b-claims-mapping.md)
* [Externí sdílení Office 365](active-directory-b2b-o365-external-user.md)
* [Aktuální omezení spolupráce B2B](active-directory-b2b-current-limitations.md)
