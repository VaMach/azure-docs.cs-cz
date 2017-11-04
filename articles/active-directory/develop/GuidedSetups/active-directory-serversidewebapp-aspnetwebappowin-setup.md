---
title: "Azure AD v2 rozhraní ASP.NET Web Server získávání spuštěno – nastavení | Microsoft Docs"
description: "Implementace přihlašování společnosti Microsoft na řešení technologie ASP.NET s tradiční webovou aplikací využívajících prohlížeč pomocí OpenID Connect standard"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: ebf54f5a203adb7f0e5b0c47dcc07595e269e218
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
## <a name="set-up-your-project"></a>Nastavení projektu

Tato část uvádí kroky pro instalaci a konfiguraci kanálu ověřování prostřednictvím middlewaru OWIN v projektu ASP.NET pomocí OpenID Connect. 

> Místo toho stáhněte projekt Visual Studio Tato ukázka dávají přednost? [Stažení projektu](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) a pokračujte [krok konfigurace](#create-an-application-express) před provedením konfigurace ukázka kódu.

<!--start-collapse-->
> ### <a name="create-your-aspnet-project"></a>Vytvoření projektu ASP.NET

> 1. V sadě Visual Studio:`File` > `New` > `Project`<br/>
> 2. V části *Visual C# \Web*, vyberte `ASP.NET Web Application (.NET Framework)`.
> 3. Název aplikace a klikněte na tlačítko *OK*
> 4. Vyberte `Empty` a zaškrtněte políčko Přidat `MVC` odkazy
<!--end-collapse-->

## <a name="add-authentication-components"></a>Přidat ověřování součásti

1. V sadě Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Přidat *balíčky NuGet middleware OWIN* pomocí následujícího příkazu v okně konzoly Správce balíčků:

```powershell
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
```

<!--start-collapse-->
> ### <a name="about-these-libraries"></a>O tyto knihovny

>Výše uvedené knihovny povolit jednotné přihlašování (SSO) prostřednictvím ověřování na základě souborů cookie pomocí OpenID Connect. Po dokončení ověření a token představující uživatele je odeslána do vaší aplikace, middlewaru OWIN, který vytvoří soubor cookie relace. Tento soubor cookie v prohlížeči pak používá na následné žádosti, takže uživatel nebude muset znovu zadejte své heslo a je potřeba žádné další ověření.
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a>Konfigurace ověřování kanálu
Následující postup slouží k vytváření middleware OWIN při spuštění třída ke konfiguraci ověřování OpenID Connect. Tato třída bude provedena automaticky při spuštění procesu služby IIS.

> Pokud nemá projektu `Startup.cs` souboru v kořenové složce:<br/>
> 1. Klikněte pravým tlačítkem na kořenové složky projektu: >`Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Název`Startup.cs`

> Zajistěte, aby byl vybranou třídu třídy pro spuštění OWIN a není standardní C# třídu. To můžete ověřit kontrolou, pokud se zobrazí `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` výše obor názvů.


1. Přidat *OWIN* a *Microsoft.IdentityModel* odkazuje na `Startup.cs`:

```csharp
using Microsoft.Owin;
using Owin;
using Microsoft.IdentityModel.Protocols;
using Microsoft.Owin.Security;
using Microsoft.Owin.Security.Cookies;
using Microsoft.Owin.Security.OpenIdConnect;
using Microsoft.Owin.Security.Notifications;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Nahraďte třída při spuštění následující kód:
</li>
</ol>

```csharp
public class Startup
{        
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    string clientId = System.Configuration.ConfigurationManager.AppSettings["ClientId"];

    // RedirectUri is the URL where the user will be redirected to after they sign in.
    string redirectUri = System.Configuration.ConfigurationManager.AppSettings["RedirectUri"];

    // Tenant is the tenant ID (e.g. contoso.onmicrosoft.com, or 'common' for multi-tenant)
    static string tenant = System.Configuration.ConfigurationManager.AppSettings["Tenant"];

    // Authority is the URL for authority, composed by Azure Active Directory v2 endpoint and the tenant name (e.g. https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0)
    string authority = String.Format(System.Globalization.CultureInfo.InvariantCulture, System.Configuration.ConfigurationManager.AppSettings["Authority"], tenant);

    /// <summary>
    /// Configure OWIN to use OpenIdConnect 
    /// </summary>
    /// <param name="app"></param>
    public void Configuration(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());
            app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Sets the ClientId, authority, RedirectUri as obtained from web.config
                ClientId = clientId,
                Authority = authority,
                RedirectUri = redirectUri,
                // PostLogoutRedirectUri is the page that users will be redirected to after sign-out. In this case, it is using the home page
                PostLogoutRedirectUri = redirectUri,
                Scope = OpenIdConnectScopes.OpenIdProfile,
                // ResponseType is set to request the id_token - which contains basic information about the signed-in user
                ResponseType = OpenIdConnectResponseTypes.IdToken,
                // ValidateIssuer set to false to allow personal and work accounts from any organization to sign in to your application
                // To only allow users from a single organizations, set ValidateIssuer to true and 'tenant' setting in web.config to the tenant name
                // To allow users from only a list of specific organizations, set ValidateIssuer to true and use ValidIssuers parameter 
                TokenValidationParameters = new System.IdentityModel.Tokens.TokenValidationParameters() { ValidateIssuer = false },
                // OpenIdConnectAuthenticationNotifications configures OWIN to send notification of failed authentications to OnAuthenticationFailed method
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    AuthenticationFailed = OnAuthenticationFailed
                }
            }
        );
    }

    /// <summary>
    /// Handle failed authentication requests by redirecting the user to the home page with an error in the query string
    /// </summary>
    /// <param name="context"></param>
    /// <returns></returns>
    private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> context)
    {
        context.HandleResponse();
        context.Response.Redirect("/?errormessage=" + context.Exception.Message);
        return Task.FromResult(0);
    }
}

```
<!--start-collapse-->
> ### <a name="more-information"></a>Další informace

> Parametry, zadejte v *OpenIDConnectAuthenticationOptions* sloužit jako souřadnice aplikace komunikovat s Azure AD. Protože middleware OpenID Connect používá soubory cookie na pozadí, musíte taky nastavit ověřování souborů cookie jako kód výš ukazuje. *Atribut ValidateIssuer* hodnota informuje OpenIdConnect není omezit přístup k jedné konkrétní organizaci.
<!--end-collapse-->

