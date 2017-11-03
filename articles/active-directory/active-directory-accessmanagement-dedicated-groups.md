---
title: "Vyhrazené skupiny ve službě Azure Active Directory | Microsoft Docs"
description: "Přehled o tom, jak vyhrazené skupiny fungují v Azure Active Directory a jak se vytvářejí."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 86158909-083a-41fe-8090-955e96ad1865
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro;oldportal
ms.openlocfilehash: 992f4563064d7a292cf4fdd90a9a3c84cdec91c0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="dedicated-groups-in-azure-active-directory"></a>Vyhrazené skupiny ve službě Azure Active Directory
Funkci vyhrazené skupiny ve službě Azure Active Directory (Azure AD), automaticky vytvoří a naplní členství ve skupinách Azure AD předdefinované. Členové vyhrazené skupiny nelze přidat nebo odebrat pomocí portálu Azure classic, rutiny prostředí Windows PowerShell, nebo prostřednictvím kódu programu.

> [!NOTE]
> Vyhrazené skupiny vyžadovat, že je přiřazený licenci Azure AD Premium
>
> * správce, který spravuje pravidlo na skupinu
> * všichni uživatelé, kteří jsou pomocí pravidla Vybraní jako členové skupiny
>
>

**Chcete-li povolit vyhrazené skupiny**

1. V [portál Azure](https://portal.azure.com), vyberte **služby Active Directory**a pak otevřete adresáři vaší organizace.
2. Vyberte **skupiny** kartě a pak otevřete skupinu, kterou chcete upravit.
3. Vyberte **konfigurace** kartě a poté nastavte **povolte vyhrazené skupiny** k **Ano**.

Povolit vyhrazené skupiny přepínač nastavenou na hodnotu **Ano**, můžete povolit další adresář, který chcete automaticky vytvořit vyhrazenou skupinu všichni uživatelé nastavením **povolit "Všichni uživatelé" skupina** přepnout na **Ano**. Můžete pak taky upravit název této vyhrazené skupiny zadáním **zobrazovaný název "Všichni uživatelé" skupina** pole.

Všichni uživatelé skupiny lze přiřadit stejná oprávnění pro všechny uživatele ve vašem adresáři. Například můžete udělit všichni uživatelé v adresáři přístup k aplikaci SaaS přiřadit přístup pro vyhrazené skupině všichni uživatelé k této aplikaci.

Vyhrazené všichni uživatelé skupina obsahuje všechny uživatele v adresáři, včetně hostů a externí uživatele. Pokud potřebujete skupinu, vyloučí externí uživatele, a poté se dá dosáhnout vytvořením skupiny se na základě atributů dynamické pravidlo například následující:

                (user.userPrincipalName -notContains "#EXT#@")

Pro skupinu, která nezahrnuje všechny hosté použijte například následující pravidlo:

                (user.userType -ne "Guest")

Další informace o vytváření *rozšířených* pravidel (která můžou obsahovat několik porovnání) pro dynamické členství ve skupině najdete v článku o [používání atributů k vytvoření rozšířených pravidel](active-directory-accessmanagement-groups-with-advanced-rules.md).

### <a name="next-steps"></a>Další kroky
Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Představení služby Azure Active Directory](active-directory-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
