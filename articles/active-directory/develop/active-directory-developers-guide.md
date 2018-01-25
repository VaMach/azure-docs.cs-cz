---
title: "Azure Active Directory pro vývojáře | Dokumentace Microsoftu"
description: "Tento článek obsahuje přehled přihlašování pracovních a školních účtů Microsoft pomocí Azure Active Directory."
services: active-directory
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 89a232af6387f6403e6e341cced16d06e9979dae
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
# <a name="azure-active-directory-for-developers"></a>Azure Active Directory pro vývojáře
Azure Active Directory (Azure AD) je cloudová služba identit, která vývojářům umožňuje bezpečně přihlásit uživatele s pracovním nebo školním účtem Microsoft. Tato dokumentace vám ukáže, jak do vaší aplikace přidat podporu Azure AD s použitím standardních oborových protokolů OAuth 2.0 a OpenID Connect.

| | |
| --- | --- |
|[Základy ověřování](active-directory-authentication-scenarios.md) | Úvod k ověřování pomocí Azure AD |
|[Typy aplikací](active-directory-authentication-scenarios.md#application-types-and-scenarios) | Přehled scénářů ověřování, které podporuje Azure AD |                                
                                                                              
## <a name="get-started"></a>Začínáme
Následující praktická nastavení vás provedou použitím knihoven ověřování Microsoftu k přihlášení uživatelů Azure AD.

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobilní a desktopové aplikace](./media/active-directory-developers-guide/NativeApp_Icon.png)<br />Mobilní a desktopové aplikace</center> | [Přehled](active-directory-authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](active-directory-devquickstarts-ios.md)<br /><br />[Android](active-directory-devquickstarts-android.md) | [.NET (WPF)](active-directory-devquickstarts-dotnet.md)<br /><br />[.NET (UPW)](active-directory-devquickstarts-windowsstore.md)<br /><br />[Xamarin](active-directory-devquickstarts-xamarin.md) | [Cordova](active-directory-devquickstarts-cordova.md) |
| <center>![Webové aplikace](./media/active-directory-developers-guide/Web_app.png)<br />Webové aplikace</center> | [Přehled](active-directory-authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](active-directory-devquickstarts-webapp-dotnet.md)<br /><br />[Java](active-directory-devquickstarts-webapp-java.md) | [Node.js](active-directory-devquickstarts-openidconnect-nodejs.md) |  |
| <center>![Jednostránkové aplikace](./media/active-directory-developers-guide/SPA.png)<br />Jednostránkové aplikace</center> | [Přehled](active-directory-authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](active-directory-devquickstarts-angular.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |  |  |
| <center>![Webová rozhraní API](./media/active-directory-developers-guide/Web_API.png)<br />Webová rozhraní API</center> | [Přehled](active-directory-authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](active-directory-devquickstarts-webapi-dotnet.md)<br /><br />[Node.js](active-directory-devquickstarts-webapi-nodejs.md) | &nbsp; |
| <center>![Služba-služba](./media/active-directory-developers-guide/Service_App.png)<br />Služba-služba</center> | [Přehled](active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](active-directory-code-samples.md#server-or-daemon-application-to-web-api)|  |

## <a name="how-to-guides"></a>Návody
Následující návody obsahují informace o provádění běžných úloh s Azure AD.

|                                                                           |  |
|---------------------------------------------------------------------------| --- |
|[Registrace aplikace](active-directory-integrating-applications.md)           | Postup registrace aplikace v Azure AD |
|[Víceklientské aplikace](active-directory-devhowto-multi-tenant-overview.md)    | Postup přihlášení libovolného pracovního účtu Microsoft |
|[Protokoly OAuth a OpenID Connect](active-directory-protocols-openid-connect-code.md)| Postup přihlášení uživatelů a volání webových rozhraní API pomocí ověřovacích protokolů Microsoftu |
|[Další návody](active-directory-developers-guide-index.md#guides)        |  Seznam návodů, které jsou k dispozici pro Azure AD   |

## <a name="reference-topics"></a>Referenční témata
Následující články poskytují podrobné informace o rozhraních API, zprávách protokolů a termínech používaných v Azure AD.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Knihovny ověřování (ADAL)](active-directory-authentication-libraries.md)   | Přehled knihoven a sad SDK, které poskytuje Azure AD |
| [Ukázky kódu](active-directory-code-samples.md)                                  | Seznam všech ukázek kódu Azure AD |
| [Glosář](active-directory-dev-glossary.md)                                      | Terminologie a definice slov, která se používají v této dokumentaci |
| [Další referenční témata](active-directory-developers-guide-index.md#reference)| Seznam referenčních témat, která jsou k dispozici pro Azure AD   |


> [!NOTE]
> Pokud potřebujete přihlašovat osobní účty Microsoft, měli byste zvážit použití [koncového bodu Azure AD v2.0](active-directory-appmodel-v2-overview.md). Koncový bod Azure AD v2.0 sjednocuje osobní účty Microsoft a pracovní účty Microsoft (z Azure AD) do jednoho ověřovacího systému.


[!INCLUDE  [Help and support](../../../includes/active-directory-develop-help-support-include.md)]