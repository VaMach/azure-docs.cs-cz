---
title: "Služba Azure AD v2.0 .NET webové aplikace volání rozhraní API, Začínáme | Microsoft Docs"
description: "Postup vytvoření webové aplikace .NET MVC tohoto volání webové služby, používání osobních účtů Microsoft a pracovní nebo školní účty pro přihlášení."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 56be906e-71de-469d-9a5c-9fc08aae4223
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: dc3162ae8e6ce622139125c2e78fa45d2e90d534
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="calling-a-web-api-from-a-net-web-app"></a>Volání webového rozhraní API z webové aplikace .NET
S koncovým bodem v2.0 můžete rychle přidání ověřování do vaší webové aplikace a webové rozhraní API s podporou pro oba osobní účty Microsoft a pracovní nebo školní účty.  Zde jsme budete vytvářet webové aplikace MVC, která podepisuje uživatele pomocí OpenID Connect, s pomoc od společnosti Microsoft OWIN middleware.  Webové aplikace se získání přístupových tokenů OAuth 2.0 pro webové rozhraní api, které jsou zabezpečeny OAuth 2.0, který umožňuje vytvořit, číst a odstranit na daného uživatele "seznam úkolů".

V tomto kurzu se soustředí hlavně na pomocí MSAL na získání a používání ve webové aplikaci, popsané v úplné přístupové tokeny [zde](active-directory-v2-flows.md#web-apps).  Jako požadavky, můžete chtít nejdřív zjistěte, jak [přidat základní přihlášení do webové aplikace](active-directory-v2-devquickstarts-dotnet-web.md) nebo jak [správně zabezpečení webového rozhraní API](active-directory-v2-devquickstarts-dotnet-api.md).

> [!NOTE]
> Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0.  Pokud chcete zjistit, pokud byste měli používat koncový bod v2.0, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
> 
> 

## <a name="download-sample-code"></a>Stáhněte si ukázkový kód
Kód k tomuto kurzu je udržovaný [na GitHubu](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).  Chcete-li sledovat, můžete [stáhnout kostru aplikace jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) nebo tuto kostru klonovat:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

Alternativně můžete [stáhnout dokončené aplikace jako ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) nebo klonovat dokončené aplikace:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Registrace aplikace
Vytvoření nové aplikace v [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle těchto [podrobné kroky](active-directory-v2-app-registration.md).  Zkontrolujte, že:

* Zkopírování **Id aplikace** přiřazené vaší aplikaci, budete ho potřebovat brzy k dispozici.
* Vytvoření **tajný klíč aplikace** z **heslo** typu a poznamenejte jeho hodnoty pro později
* Přidat **webové** platformu pro vaši aplikaci.
* Zadejte správný **identifikátor URI pro přesměrování**. Určuje identifikátor uri přesměrování do služby Azure AD, kde se mají směrovat ověřování odpovědí – výchozí hodnota pro tento kurz je `https://localhost:44326/`.

## <a name="install-owin"></a>Instalace OWIN
Přidání balíčků NuGet middleware OWIN pro `TodoList-WebApp` projektu pomocí konzoly Správce balíčků.  Middlewaru OWIN, který se použije k vydávání požadavků na přihlášení a odhlášení, spravovat relace uživatele a získat informace o uživateli, mimo jiné.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a>Přihlášení uživatele
Teď nakonfigurovat middlewaru OWIN, který má být použit [ověřovacího protokolu OpenID Connect](active-directory-v2-protocols.md).  

* Otevřete `web.config` soubor v kořenovém `TodoList-WebApp` projektu a zadejte hodnoty konfigurace vaší aplikace v `<appSettings>` oddílu.
  * `ida:ClientId` Je **Id aplikace** přiřazené vaší aplikaci v portálu pro registraci.
  * `ida:ClientSecret` Je **tajný klíč aplikace** jste vytvořili v portálu pro registraci.
  * `ida:RedirectUri` Je **identifikátor Uri pro přesměrování** jste zadali v portálu.
* Otevřete `web.config` soubor v kořenovém `TodoList-Service` projektu a nahraďte `ida:Audience` se stejným **Id aplikace** jako výše.
* Otevřete soubor `App_Start\Startup.Auth.cs` a přidejte `using` příkazy pro knihovny z výše.
* Ve stejném souboru implementovat `ConfigureAuth(...)` metoda.  Parametry, zadejte v `OpenIDConnectAuthenticationOptions` bude sloužit jako souřadnice pro vaši aplikaci komunikovat s Azure AD.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {

                    // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
                    // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                    // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                    ClientId = clientId,
                    Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a>Použití MSAL získat přístupové tokeny
V `AuthorizationCodeReceived` oznámení, chceme používat [OAuth 2.0 v kombinaci s OpenID Connect](active-directory-v2-protocols.md) chcete uplatnit authorization_code pro token přístupu ke službě seznamu úkolů.  MSAL můžete tento proces snadno pro vás provedl:

* Nejdřív nainstalujte verzi preview MSAL:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```

* A přidat další `using` příkaz, který má `App_Start\Startup.Auth.cs` MSAL v souboru.
* Nyní přidejte nové metody, `OnAuthorizationCodeReceived` obslužné rutiny události.  Tato obslužná rutina použije MSAL získat token přístupu k rozhraní API seznamu úkolů a uloží token v mezipaměti na MSAL tokenu pro pozdější:

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

* Ve službě web apps má MSAL extensible mezipamětí tokenů, který slouží k uložení tokeny.  Tato ukázka implementuje `NaiveSessionCache` úložiště relace http pro tokeny mezipaměti, který používá.

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a>Volání webového rozhraní API
Nyní je čas skutečně používáte access_token, které jste získali v kroku 3.  Otevřete web app `Controllers\TodoListController.cs` souboru, který provede všechny požadavky CRUD rozhraní API seznamu úkolů.

* Můžete MSAL znovu zde načíst access_tokens z mezipaměti MSAL.  Nejprve přidejte `using` příkaz pro MSAL do tohoto souboru.
  
    `using Microsoft.Identity.Client;`
* V `Index` akce, použijte MSAL `AcquireTokenSilentAsync` metoda získat access_token, které je možné načíst data ze služby na seznam úkolů:

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

* Ukázka pak přidá výsledný token na požadavek HTTP GET `Authorization` hlavičky, která služba seznam úkolů používá k ověření žádosti.
* Pokud vrátí seznam úkolů služby `401 Unauthorized` odpovědi, access_tokens v MSAL mít zneplatní z nějakého důvodu.  V takovém případě by měl vyřaďte všechny access_tokens z mezipaměti MSAL a zobrazí uživateli zprávu, která budou muset přihlásit znovu, který bude restartován tok tokenu pořízení.

```C#
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

* Podobně pokud MSAL nelze vrátit access_token z jakéhokoli důvodu, byste měli vyzvat uživatele se znovu přihlásit.  To je jednoduché, zachytávání žádné `MSALException`:

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

* Stejné přesnou `AcquireTokenSilentAsync` volání je implementd v `Create` a `Delete` akce.  Ve službě web apps můžete použít tuto metodu MSAL získat access_tokens kdykoli budete potřebovat ve vaší aplikaci.  MSAL se postará o získávání, ukládání do mezipaměti a aktualizaci tokeny pro vás.

Nakonec sestavte a spusťte aplikaci!  Přihlaste se pomocí Account Microsoft nebo účtu Azure AD a Všimněte si, jak se odrazí identitu uživatelů v horním navigačním panelu.  Přidat a odstranit některé položky ze seznamu úkolů uživatele chcete zjistit že OAuth 2.0 zabezpečená volání rozhraní API v akci.  Nyní máte webovou aplikaci & webového rozhraní API, jak zabezpečit pomocí standardních protokolů, které může ověřit uživatele s svoje osobní, tak i pracovní nebo školní účty.

Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) [je tady uvedené](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).  

## <a name="next-steps"></a>Další kroky
Další zdroje projděte si:

* [Příručka vývojáře v2.0 >>](active-directory-appmodel-v2-overview.md)
* [Značka StackOverflow "azure-active-directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Získejte bezpečnostní aktualizace našich produktů
Doporučujeme vám získávat oznámení o bezpečnostních incidentech tak, že navštívíte [tuto stránku](https://technet.microsoft.com/security/dd252948) a přihlásíte se k odběru služby Security Advisory Alerts.

