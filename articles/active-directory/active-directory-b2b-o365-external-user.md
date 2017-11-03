---
title: "Externí sdílení Office 365 a Azure Active Directory s B2B spolupráci | Microsoft Docs"
description: "referenční dokumentace pro Azure Active Directory s B2B spolupráce mapování deklarace identity"
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
ms.openlocfilehash: cad0ce8f745f3d6ca14436fd714b08c60de0e459
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a>Externí sdílení Office 365 a spolupráce Azure Active Directory s B2B

Externí sdílení v Office 365 (OneDrive, SharePoint Online, Unified skupin atd.) a spolupráce B2B Azure Active Directory (Azure AD) jsou technicky samé. Všechny externí sdílení (kromě OneDrive nebo SharePoint Online), včetně hostů v skupiny Office 365, už používá pozvánku spolupráce Azure AD B2B rozhraní API pro sdílení.

OneDrive nebo SharePoint Online má správce samostatné pozvánku. Podpora pro externí sdílení v OneDrive nebo SharePoint Online spuštěna před jeho podporu vyvinul Azure AD. V čase OneDrive nebo SharePoint Online externí sdílení rozlišeny několik funkcí a mnoha milionů uživatelů, kteří používají produktu je v vytvořená sdílení vzor. Existují však určité jemně rozdíly mezi OneDrive nebo SharePoint Online externí sdílení jak funguje a jak funguje spolupráce Azure AD B2B:

- OneDrive nebo SharePoint Online přidá uživatele do adresáře po uživatelé mají uplatněn svoje pozvánky. Ano před uplatnění, nevidíte uživatele na portálu Azure AD. Pokud jiný web vyzývá uživatel do té doby, vygeneruje se nová pozvánka. Však při použití spolupráce Azure AD B2B, uživatelé jsou okamžitě přidány na pozvánku, aby zobrazovala až everywhere.

- Uplatnění dojde v OneDrive nebo SharePoint Online vypadá jinak než dojde v spolupráce Azure AD B2B. Jakmile uživatel uplatňuje pozvánku, vypadat podobně zkušenosti.

- Azure AD s B2B spolupráce pozvánku, uživatelé mohou být zachyceny z OneDrive nebo SharePoint Online sdílení dialogová okna. OneDrive nebo SharePoint Online pozvat uživatele také zobrazí v Azure AD poté, co se uplatnit své pozvánky.

- Chcete-li spravovat externí sdílení v OneDrive nebo SharePoint Online pomocí spolupráce Azure AD B2B, nastavte OneDrive nebo SharePoint Online externí sdílení nastavení **pouze povolit sdílení s externími uživateli již v adresáři**. Uživatelé přejít na externě sdílené weby a vyberte z externími spolupracovníky přidané správce. Správce můžete přidat externí spolupracovníky prostřednictvím pozvánku spolupráce B2B rozhraní API.

![OneDrive nebo SharePoint Online externí sdílení nastavení](media/active-directory-b2b-o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a>Další kroky

Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Vlastnosti uživatele spolupráce B2B](active-directory-b2b-user-properties.md)
* [Přidání uživatele spolupráce B2B k roli](active-directory-b2b-add-guest-to-role.md)
* [Delegovat pozvánek spolupráce B2B](active-directory-b2b-delegate-invitations.md)
* [Dynamické skupiny a spolupráci B2B](active-directory-b2b-dynamic-groups.md)
* [Kód spolupráce B2B a ukázky prostředí PowerShell](active-directory-b2b-code-samples.md)
* [Konfigurace aplikací SaaS pro spolupráci B2B](active-directory-b2b-configure-saas-apps.md)
* [Tokeny uživatele spolupráce B2B](active-directory-b2b-user-token.md)
* [Deklarace uživatele spolupráce B2B mapování](active-directory-b2b-claims-mapping.md)
* [Aktuální omezení spolupráce B2B](active-directory-b2b-current-limitations.md)
