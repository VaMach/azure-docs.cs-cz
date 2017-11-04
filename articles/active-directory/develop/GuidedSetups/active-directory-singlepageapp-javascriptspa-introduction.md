---
title: "Azure AD v2 JS SPA na základě nastavení – Úvod | Microsoft Docs"
description: "Jak může aplikace JavaScript SPA volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: 3d195d0d67f8f82c9450ffd93767917698addee3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Volání Microsoft Graph API z jednoho jazyka JavaScript stránky aplikace (SPA)

Tato příručka ukazuje, jak můžete JavaScript aplikace pro jednu stránku (SPA) přihlásit osobní pracovní a školní účty, získání přístupového tokenu a volání rozhraní Graph API Microsoft nebo jiná rozhraní API, které vyžadují přístupové tokeny z koncového bodu v2 Azure Active Directory.

### <a name="how-this-guide-works"></a>Jak funguje tato příručka

![Jak funguje ukázková aplikace generované tímto průvodcem](media/active-directory-singlepageapp-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a>Další informace

Ukázkové aplikace vytvořené v této příručce umožňuje SPA JavaScript do dotazu Microsoft Graph API nebo webového rozhraní API, které přijímá tokeny z koncového bodu v2 Azure Active Directory. Pro tento scénář, poté, co uživatel přihlásí je token přístupu požadovaná a přidat na požadavky HTTP přes autorizační hlavičky. Získání tokenu a obnovení, jsou zpracovávány pomocí knihovny Microsoft ověřování (MSAL).

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Knihovny

Tato příručka používá následující knihovny:

|Knihovna|Popis|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Knihovna Microsoft ověřování pro JavaScript Preview|

> [!NOTE]
> *msal.js* cíle *koncový bod služby Azure Active Directory v2* – což umožňuje osobní, školní a pracovní účty pro přihlášení a získávat tokeny. *Koncový bod služby Azure Active Directory v2* má [určitá omezení](..\active-directory-v2-limitations.md). Pokud vás zajímá jenom pracovní a školní účty, použijte *adal.js* a *koncový bod V1*. Chcete-li pochopit rozdíly mezi v1 a v2 koncové body, přečtěte si [v1-v2 porovnání](..\active-directory-v2-compare.md).

<!--end-collapse-->
