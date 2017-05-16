---
title: "Azure Active Directory pro vývojáře | Dokumentace Microsoftu"
description: "Tento článek obsahuje přehled přihlašování pracovních a školních účtů Microsoft pomocí Azure Active Directory."
services: active-directory
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/07/2017
ms.author: bryanla
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: c7f758900ca89ed8bf08090cda5964eccc876e1d
ms.contentlocale: cs-cz
ms.lasthandoff: 05/08/2017

---
# <a name="azure-active-directory-for-developers"></a>Azure Active Directory pro vývojáře
Azure Active Directory je cloudová služba identit, která vývojářům umožňuje bezpečně přihlásit jakéhokoli uživatele s pracovním nebo školním účtem, který zajišťuje Microsoft.  Dokumentace, kterou tady najdete, vám ukáže, jak do vaší aplikace přidat podporu Azure AD s použitím standardních oborových ověřovacích protokolů OAuth a OpenID Connect.

| | |
| --- | --- |
|[Základy ověřování](active-directory-authentication-scenarios.md) | Úvod k ověřování pomocí Azure AD |
|[Typy aplikací](active-directory-authentication-scenarios.md#application-types-and-scenarios) | Přehled scénářů ověřování, které podporuje Azure AD |                                
                                                                              
## <a name="get-started"></a>Začínáme
Tato praktická nastavení vás provedou použitím našich knihoven ověřování k přihlášení uživatelů Azure Active Directory.

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobilní aplikace a aplikace počítače](./media/active-directory-developers-guide/NativeApp_Icon.png)<br />Mobilní aplikace a aplikace počítače</center> | [Přehled](active-directory-authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](active-directory-devquickstarts-ios.md)<br /><br />[Android](active-directory-devquickstarts-android.md) | [.NET](active-directory-devquickstarts-dotnet.md)<br /><br />[Windows](active-directory-devquickstarts-windowsstore.md)<br /><br />[Xamarin](active-directory-devquickstarts-xamarin.md) | [Cordova](active-directory-devquickstarts-cordova.md)<br /><br />[OAuth 2.0](active-directory-protocols-oauth-code.md) |
| <center>![Web Apps](./media/active-directory-developers-guide/Web_app.png)<br />Web Apps</center> | [Přehled](active-directory-authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](active-directory-devquickstarts-webapp-dotnet.md)<br /><br />[Java](active-directory-devquickstarts-webapp-java.md) | [NodeJS](active-directory-devquickstarts-openidconnect-nodejs.md)<br /><br />[OpenID Connect 1.0](active-directory-protocols-openid-connect-code.md) |  |
| <center>![Jednostránkové aplikace](./media/active-directory-developers-guide/SPA.png)<br />Jednostránkové aplikace</center> | [Přehled](active-directory-authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](active-directory-devquickstarts-angular.md)<br /><br />[JavaScript]((https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)) |  |  |
| <center>![Webová rozhraní API](./media/active-directory-developers-guide/Web_API.png)<br />Webová rozhraní API</center> | [Přehled](active-directory-authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](active-directory-devquickstarts-webapi-dotnet.md)<br /><br />[NodeJS](active-directory-devquickstarts-webapi-nodejs.md) | &nbsp; |
| <center>![Služba-služba](./media/active-directory-developers-guide/Service_App.png)<br />Služba-služba</center> | [Přehled](active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](active-directory-code-samples.md#server-or-daemon-application-to-web-api)<br /><br />[Přihlašovací údaje pro klienta OAuth 2.0](active-directory-protocols-oauth-service-to-service.md) |  |

## <a name="guides"></a>Průvodci
Tyto články obsahují informace o provádění běžných úloh s Azure Active Directory.

|                                                                           |  |
|---------------------------------------------------------------------------| --- |
|[Registrace aplikace](active-directory-integrating-applications.md)           | Postup registrace aplikace v Azure AD |
|[Aplikace s více tenanty](active-directory-devhowto-multi-tenant-overview.md)    | Postup přihlášení libovolného pracovního účtu Microsoft |
|[OAuth a OpenID Connect](active-directory-protocols-openid-connect-code.md)| Postup přihlášení uživatelů a volání webových rozhraní API pomocí našich moderních ověřovacích protokolů |
|[Další průvodci...](active-directory-developers-guide-index.md#guides)        |     |

## <a name="reference"></a>Referenční informace
Tyto články poskytují podrobné informace o rozhraních API, zprávách protokolů a termínech používaných v Azure Active Directory.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Knihovny ověřování (ADAL)](active-directory-authentication-libraries.md)   | Přehled knihoven a sad SDK, které poskytuje Azure AD |
| [Ukázky kódu](active-directory-code-samples.md)                                  | Seznam všech ukázek kódu Azure AD |
| [Glosář](active-directory-dev-glossary.md)                                      | Terminologie a definice slov, která se používají v této dokumentaci |
| [Další referenční materiály...](active-directory-developers-guide-index.md#reference)|     |

## <a name="help--support"></a>Nápověda a podpora
Na těchto místech získáte nejlepší pomoc s vývojem v Azure Active Directory.

|  |  
|---|
|[Značky `azure-active-directory` a `adal` na Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)      |
|[Zpětná vazba k Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)|

<br />

> [!NOTE]
> Pokud potřebujete přihlašovat osobní účty Microsoft, měli byste zvážit použití [koncového bodu Azure AD v2.0](active-directory-appmodel-v2-overview.md).  Koncový bod Azure AD v2.0 sjednocuje osobní účty Microsoft a pracovní účty Microsoft (z Azure AD) do jednoho ověřovacího systému.

