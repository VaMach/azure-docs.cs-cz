---
title: "Azure AD v2 iOS Začínáme – Úvod | Microsoft Docs"
description: "Jak aplikace pro iOS (Swift) můžete volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
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
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 5fcd78eaa0c5b09b70aa973466a34556fff56071
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="call-the-microsoft-graph-api-from-an-ios-app"></a>Volání rozhraní Microsoft Graph API z aplikace pro iOS

Tato příručka ukazuje, jak získat přístupový token a volání rozhraní Graph API Microsoft nebo jiná rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 aplikace nativní aplikace pro iOS (Swift).

Na konci tohoto průvodce bude moci volat chráněné API používání osobních účtů (včetně live.com, outlook.com a dalších) a také pracovní a školní účty z jakékoli společnosti nebo organizace, která má Azure Active Directory vaší aplikace.

> ### <a name="pre-requisites"></a>Požadavky
> - XCode 8.x je vyžadována pro tento průvodce. Si můžete stáhnout XCode [odsud](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode stáhnout URL").
> - [Carthage](https://github.com/Carthage/Carthage) pro správy balíčků

### <a name="how-this-guide-works"></a>Jak funguje tato příručka

![Jak funguje tato příručka](media/active-directory-mobileanddesktopapp-ios-introduction/iosintro.png)

Ukázkové aplikace vytvořené v této příručce umožňuje aplikace pro iOS k dotazování Microsoft Graph API nebo webového rozhraní API, které přijímá tokeny z koncového bodu v2 Azure Active Directory. V tomto scénáři se token přidá na požadavky HTTP přes autorizační hlavičky. Získání tokenu a obnova zpracovává pomocí knihovny Microsoft ověřování (MSAL).


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Zpracování získání tokenu pro přístup k chráněné webové rozhraní API

Jakmile se uživatel ověřuje, ukázkové aplikace obdrží token, který slouží k dotazu Microsoft Graph API nebo webového rozhraní API zabezpečeny v2 Microsoft Azure Active Directory.

Rozhraní API, jako je například Microsoft Graph vyžadují token přístupu umožňuje přístup ke konkrétním prostředkům – například čtení profilu uživatele, kalendáře přístup uživatele nebo poslat e-mailu. Aplikace může požadovat token přístupu pomocí MSAL zadáním obory rozhraní API. Tento přístupový token se pak přidá do hlavičky HTTP autorizace pro každé volání proti k chráněnému prostředku.

MSAL spravuje ukládání do mezipaměti a aktualizaci přístupových tokenů, aby vaše aplikace nemusí.


### <a name="libraries"></a>Knihovny

Tato příručka používá následující knihovny:

|Knihovna|Popis|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|Microsoft ověřování knihovny Preview pro iOS|

