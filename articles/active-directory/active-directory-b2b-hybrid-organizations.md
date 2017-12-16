---
title: "Spolupráce Azure Active Directory s B2B pro hybridní organizace | Microsoft Docs"
description: "Poskytnout partnerům přístup k i místních a cloudových prostředků s spolupráce Azure AD B2B"
services: active-directory
documentationcenter: 
author: twooley
manager: mtillman
editor: 
tags: 
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.date: 12/15/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 2e690eeea6a9f7e1cc10830a913774daa3c66689
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/16/2017
---
# <a name="azure-active-directory-b2b-collaboration-for-hybrid-organizations"></a>Spolupráce Azure Active Directory s B2B pro hybridní organizace

V hybridním organizaci, kde mají obě místních a cloudových prostředků, můžete chtít udělení externími partnery přístup k i místních a cloudových prostředků. Pro přístup k místním prostředkům můžete spravovat účty partnera místně v prostředí místní služby Active Directory. Partneři Přihlaste se pomocí svých přihlašovacích údajů účtu partnera pro přístup k prostředkům vaší organizace. Partneři udělit přístup k prostředkům cloudu pomocí stejné přihlašovací údaje, můžete nyní použijte připojení služby Azure Active Directory (Azure AD) k synchronizaci partnera účty v cloudu jako uživatele Azure AD s B2B (to znamená, uživatelé s UserType = hosta).

## <a name="identify-unique-attributes-for-usertype"></a>Určit jedinečné atributy pro UserType

Než povolíte synchronizaci atribut UserType nastavený na, musíte nejprve rozhodnout, jak do atribut UserType nastavený na odvozena z místní služby Active Directory. Jinými slovy jaké parametry v místním prostředí jsou jedinečné pro externími spolupracovníky? Určete parametr, který se odlišuje od členy vaší vlastní organizaci tyto externími spolupracovníky.

Dva přístupy společné pro tento je:

- Určete nepoužívané místní atributů služby Active Directory (například extensionAttribute1) chcete použít jako zdrojový atribut. 
- Hodnotu pro atribut UserType Alternativně odvozen od dalších vlastností. Například chcete synchronizovat všechny uživatele jako hosta, pokud jejich atribut Active Directory UserPrincipalName v místě skončí s doménou  *@partners.fabrikam123.org* .
 
Atribut podrobné požadavky najdete v tématu [povolení synchronizace UserType](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype). 

## <a name="configure-azure-ad-connect-to-sync-users-to-the-cloud"></a>Konfigurovat Azure AD Connect uživatelům synchronizace do cloudu

Po identifikaci jedinečný atribut, můžete nakonfigurovat synchronizaci těchto uživatelů do cloudu jako uživatele Azure AD s B2B Azure AD Connect (to znamená, uživatelé s UserType = hosta). Tito uživatelé ze autorizace hlediska, se neodlišuje od B2B uživatelé vytvoření pozvánky procesem spolupráce Azure AD B2B.

Implementace pokyny najdete v tématu [povolení synchronizace UserType](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype).

## <a name="next-steps"></a>Další kroky

- Přehled spolupráce Azure AD B2B najdete v tématu [co je spolupráce Azure AD B2B?](active-directory-b2b-what-is-azure-ad-b2b.md)
- Přehled služby Azure AD Connect, najdete v tématu [integraci místních adresářů se službou Azure Active Directory](connect/active-directory-aadconnect.md).

