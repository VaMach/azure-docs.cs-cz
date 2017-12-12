---
title: "Webové aplikace Azure AD .NET Začínáme | Microsoft Docs"
description: "Vytvoření webové aplikace .NET MVC, která se integruje se službou Azure AD pro přihlášení."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mtillman
editor: 
ms.assetid: e15a41a4-dc5d-4c90-b3fe-5dc33b9a1e96
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 5025ebda6eb47c4155c098be4a5479d5c3814942
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="aspnet-web-app-sign-in-and-sign-out-with-azure-ad"></a>Webové aplikace ASP.NET přihlášení a odhlášení s Azure AD
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Tím, že poskytuje jeden přihlášení a odhlášení jenom pár řádků kódu, Azure Active Directory (Azure AD) umožňuje snadno můžete využít Správa identit pro webové aplikace k. Uživatelé a deaktivovat webové aplikace ASP.NET můžete přihlásit pomocí implementace Microsoft Open Web Interface pro .NET (OWIN) middleware. Rozhraní .NET Framework 4.5 je součástí komunitou vytvářený middleware OWIN. Tento článek ukazuje, jak používat OWIN na:

* Přihlášení uživatelů k webové aplikace pomocí Azure AD jako zprostředkovatele identity.
* Zobrazí některé informace o uživateli.
* Přihlášení uživatelů z aplikace.

## <a name="before-you-get-started"></a>Než začnete
* Stažení [kostru aplikace](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) nebo stáhnout [hotová ukázka](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).
* Musíte taky klient služby Azure AD, ve kterém pro registraci aplikace. Pokud ještě nemáte klient služby Azure AD [zjistěte, jak získat](active-directory-howto-tenant.md).

Až budete připravení, postupujte podle pokynů v další čtyři části.

## <a name="step-1-register-the-new-app-with-azure-ad"></a>Krok 1: Zaregistrujte novou aplikaci s Azure AD
Chcete-li nastavit aplikaci pro ověřování uživatelů, nejprve zaregistrovat ve vašem klientovi provedením následujících akcí:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na horním panelu klikněte na název účtu. V části **Directory** vyberte klienta služby Active Directory, ve které chcete zaregistrovat aplikaci.
3. Klikněte na tlačítko **více služeb** v levém podokně a potom vyberte **Azure Active Directory**.
4. Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.
5. Postupujte podle výzev a vytvořte novou **webové aplikace nebo WebAPI**.
  * **Název** popis aplikace pro uživatele.
  * **Adresa URL přihlašování** je základní adresu URL aplikace. Výchozí adresa URL kostru je https://localhost:44320 /.
6. Po dokončení registrace Azure AD přiřadí aplikace ID jedinečný aplikace. Zkopírujte hodnotu na stránce aplikace používat v dalších částech.
7. Z **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte identifikátor ID URI aplikace. **Identifikátor ID URI aplikace** je jedinečný identifikátor pro aplikaci. Zásady vytváření názvů je `https://<tenant-domain>/<app-name>` (například `https://contoso.onmicrosoft.com/my-first-aad-app`).

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a>Krok 2: Nastavení aplikace pro použití ověřovacího kanálu OWIN
V tomto kroku nakonfigurujete middleware OWIN pro použití ověřovacího protokolu OpenID Connect. OWIN slouží k vydávání požadavků na přihlášení a odhlášení, správě uživatelských relací, získání informací o uživateli a tak dále.

1. Pokud chcete začít, přidejte do projektu pomocí konzoly Správce balíčků balíčky NuGet middlewaru OWIN.

     ```
     PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
     PM> Install-Package Microsoft.Owin.Security.Cookies
     PM> Install-Package Microsoft.Owin.Host.SystemWeb
     ```

2. Přidat třídu OWIN při spuštění projektu názvem `Startup.cs`, klikněte pravým tlačítkem na projekt, vyberte **přidat**, vyberte **nová položka**a poté vyhledejte **OWIN**. Vyvolá middlewaru OWIN, který **Configuration(...)**  metoda při spuštění aplikace.
3. Změňte deklaraci třídy k `public partial class Startup`. Již implementovali jsme součástí této třídy pro vás v jiném souboru. V **Configuration(...)**  metoda, zkontrolujte zavolá **ConfgureAuth(...)**  nastavení ověřování pro aplikaci.  

     ```C#
     public partial class Startup
     {
         public void Configuration(IAppBuilder app)
         {
             ConfigureAuth(app);
         }
     }
     ```

4. Otevřete soubor App_Start\Startup.Auth.cs a potom implementovat **ConfigureAuth(...)**  metoda. Parametry, zadejte v *OpenIDConnectAuthenticationOptions* sloužit jako souřadnice aplikace komunikovat s Azure AD. Musíte taky nastavit ověřování souborů cookie, protože middleware OpenID Connect používá soubory cookie na pozadí.

     ```C#
     public void ConfigureAuth(IAppBuilder app)
     {
         app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

         app.UseCookieAuthentication(new CookieAuthenticationOptions());

         app.UseOpenIdConnectAuthentication(
             new OpenIdConnectAuthenticationOptions
             {
                 ClientId = clientId,
                 Authority = authority,
                 PostLogoutRedirectUri = postLogoutRedirectUri,
                 Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = context =>
                        {
                            context.HandleResponse();
                            context.Response.Redirect("/Error?message=" + context.Exception.Message);
                            return Task.FromResult(0);
                        }
                    }
             });
     }
     ```

5. Otevřete soubor web.config v kořenovém adresáři projektu a potom zadejte hodnoty konfigurace v `<appSettings>` oddílu.
  * `ida:ClientId`: Identifikátor GUID, které jste zkopírovali z portálu Azure v "krok 1: Zaregistrujte novou aplikaci s Azure AD."
  * `ida:Tenant`: Název klienta služby Azure AD (například contoso.onmicrosoft.com).
  * `ida:PostLogoutRedirectUri`: Indikátoru, která říká službě Azure AD, kde by měl být uživatel přesměrován po úspěšném dokončení žádost o odhlášení.

## <a name="step-3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>Krok 3: Použití OWIN pro zasílání požadavků na přihlášení a odhlášení do Azure AD
Aplikace je nyní správně nakonfigurován pro komunikaci se službou Azure AD pomocí ověřovacího protokolu OpenID Connect. OWIN má zpracovává všechny podrobnosti o věnujte zpráv ověřování, ověřování tokenů z Azure AD a údržbě uživatelských relací. Už jen zbývá umožnit uživatelům způsob, jak přihlášení a odhlášení.

1. Můžete použít autorizovat značky v řadičích budou muset uživatelé přihlásit před přístupem k určité stránky. Uděláte to tak, otevřete Controllers\HomeController.cs a pak přidejte `[Authorize]` značky o akci.

     ```C#
     [Authorize]
     public ActionResult About()
     {
       ...
     ```

2. Můžete taky OWIN přímo vydání žádosti o ověření z vašeho kódu. Chcete-li to provést, otevřete Controllers\AccountController.cs. Potom v SignIn() a SignOut() akce, vydávat OpenID Connect výzvy a odhlášení požadavky.

     ```C#
     public void SignIn()
     {
         // Send an OpenID Connect sign-in request.
         if (!Request.IsAuthenticated)
         {
             HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
         }
     }
     public void SignOut()
     {
         // Send an OpenID Connect sign-out request.
         HttpContext.GetOwinContext().Authentication.SignOut(
              OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
     }
     ```

3. Otevřete Views\Shared\_LoginPartial.cshtml se uživateli zobrazí aplikace přihlášení a odhlášení odkazy a vytiskněte uživatelské jméno v zobrazení.

    ```HTML
    @if (Request.IsAuthenticated)
    {
     <text>
         <ul class="nav navbar-nav navbar-right">
             <li class="navbar-text">
                 Hello, @User.Identity.Name!
             </li>
             <li>
                 @Html.ActionLink("Sign out", "SignOut", "Account")
             </li>
         </ul>
     </text>
    }
    else
    {
     <ul class="nav navbar-nav navbar-right">
         <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
     </ul>
    }
    ```

## <a name="step-4-display-user-information"></a>Krok 4: Zobrazit informace o uživateli
Během ověřování uživatelů s OpenID Connect, Azure AD vrátí požadavku id_token na aplikaci, která obsahuje "deklarace identity" a tvrzením o uživateli. Tyto deklarace můžete použít k přizpůsobení funkcí aplikace následujícím způsobem:

1. Otevřete soubor Controllers\HomeController.cs. Dostanete deklaracích identity uživatele v řadičích prostřednictvím `ClaimsPrincipal.Current` zaregistrovaný objekt zabezpečení.

 ```C#
 public ActionResult About()
 {
     ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
     ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
     ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
     ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
     ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

     return View();
 }
 ```

2. Sestavte a spusťte aplikaci. Pokud již jste dosud nevytvořili nového uživatele ve vašem klientovi s doméně onmicrosoft.com, nyní je čas Uděláte to tak. Zde je uveden postup:

  a. Přihlaste se pomocí tohoto uživatele a Všimněte si, jak identitu uživatele se projeví na horním panelu.

  b. Odhlásit se a přihlaste se zpět jako jiný uživatel ve vašem klientovi.

  c. Pokud se cítíte zvlášť ambiciózní, zaregistrujte a spustit jiná instance této aplikace (pomocí vlastní clientId) a podívejte se na jednom přihlášení v akci.

## <a name="next-steps"></a>Další kroky
Odkaz, najdete v části [je hotová ukázka](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (bez vašich hodnot nastavení).

Nyní se můžete přesunout k pokročilejším tématům. Zkuste například [zabezpečení webového rozhraní API s Azure AD](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
