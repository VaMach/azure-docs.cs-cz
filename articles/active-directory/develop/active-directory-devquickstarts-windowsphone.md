---
title: "Začínáme se službou Azure AD Windows Phone | Microsoft Docs"
description: "Jak vytvořit aplikaci pro Windows Phone, který se integruje s Azure AD pro přihlašování a volá Azure AD chráněné rozhraní API pomocí metody OAuth."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mtillman
editor: 
ms.assetid: 66f5ac20-5e1f-4b9d-bb99-9b3305e26416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 11/30/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: fc243f4ca6d323bf2b90cb9ab8fa9b77e1df9f8e
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="azure-ad-windows-phone-getting-started"></a>Azure AD na Windows Phone Začínáme
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> Projekty pro Windows Phone 8.1 a starší verze nejsou podporovány v sadě Visual Studio 2017.  Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

Pokud vyvíjíte aplikace Windows Phone 8.1, Azure AD je jednoduchá a přímočará pro vás k ověřování uživatelů s účty služby Active Directory.  Taky umožňuje vaší aplikaci bezpečně využívat žádné webové rozhraní API, které jsou chráněné službou Azure AD, jako je například rozhraní API Office 365 nebo rozhraní API služby Azure.

> [!NOTE]
> Tento příklad používá ADAL v2.0.  Pro nejnovější technologie, můžete místo toho zkuste naše [kurz pro univerzální aplikace Windows pomocí ADAL v3.0](active-directory-devquickstarts-windowsstore.md).  Pokud vytváříte skutečně aplikace pro Windows Phone 8.1, je to na správném místě.  ADAL v2.0 je stále plně podporovaná a je doporučený způsob vývoje agianst aplikace Windows Phone 8.1 pomocí služby Azure AD.
> 
> 

Azure AD pro rozhraní .NET nativní klienty, kteří potřebují přístup k chráněným prostředkům, poskytuje knihovna ověřování Active Directory nebo ADAL.  Jediným účelem je ADAL v životní je snadno pro aplikaci, kterou chcete získat přístupové tokeny.  K předvedení právě jak je snadné, zde jsme budete sestavení "Directory osobou hledající" aplikace Windows Phone 8.1 který:

* Získá přístup k tokeny pro volání pomocí Azure AD Graph API [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Vyhledá adresář pro uživatele s danou UPN.
* Uživatelé přihlásí se.

K vytvoření dokončení funkční aplikaci, musíte:

1. Registrace vaší aplikace s Azure AD.
2. Instalace a konfigurace ADAL.
3. Pomocí ADAL získat tokeny z Azure AD.

Abyste mohli začít, [stáhnout kostru projektu](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) nebo [stažení je hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Každá je řešením sady Visual Studio 2013.  Budete také potřebovat klient služby Azure AD, ve kterém můžete vytvořit uživatele a zaregistrovat aplikaci.  Pokud ještě nemáte klienta, [zjistěte, jak získat](active-directory-howto-tenant.md).

## <a name="1-register-the-directory-searcher-application"></a>1. Registrace aplikace osobou hledající adresáře
Chcete-li aplikaci, kterou chcete získat tokeny, budete nejprve muset zaregistrovat v klientovi služby Azure AD a udělit mu oprávnění k přístupu k Azure AD Graph API:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na horním panelu klikněte na tlačítko na vašem účtu a v části **Directory** vyberte klienta služby Active Directory, kam chcete registrace vaší aplikace.
3. Klikněte na **všechny služby** v navigaci vlevo a zvolte **Azure Active Directory**.
4. Klikněte na **registrace aplikace** a zvolte **přidat**.
5. Postupujte podle výzev a vytvořte novou **nativní klientská aplikace**.
  * **Název** aplikace popíše aplikaci pro koncové uživatele
  * **Identifikátor Uri pro přesměrování** je kombinace schéma a řetězec, který Azure AD použije k vrácení odpovědi tokenu.  Zadejte hodnotu zástupného symbolu pro nyní, například `http://DirectorySearcher`.  Tato hodnota není později budete nahradit.
6. Po dokončení registrace AAD přiřadí vaší aplikace, jedinečné ID aplikace  Tuto hodnotu budete potřebovat v další části, zkopírujte jej na kartě aplikace.
7. Z **nastavení** vyberte **požadovaných oprávnění** a zvolte **přidat**. Vyberte **Microsoft Graph** jako rozhraní API a přidejte **čtení dat adresáře** oprávnění v rámci **delegovaná oprávnění**.  Tato akce povolí aplikaci zpracovat dotaz rozhraní Graph API pro uživatele.

## <a name="2-install--configure-adal"></a>2. Instalace a konfigurace ADAL
Teď, když máte aplikaci ve službě Azure AD, můžete nainstalovat ADAL a zadejte kód, týkající se identity.  Aby ADAL být schopni komunikovat se službou Azure AD budete muset poskytnout některé informace o registraci vaší aplikace.

* Začněte tím, že přidávání ADAL k DirectorySearcher projektu pomocí konzoly Správce balíčků.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* Otevřete v projektu DirectorySearcher `MainPage.xaml.cs`.  Nahraďte hodnoty v `Config Values` oblast tak, aby odrážela hodnoty vstup na portálu Azure.  Váš kód bude odkazovat tyto hodnoty vždy, když ho využívá ADAL.
  * `tenant` Je doména klienta služby Azure AD, např. contoso.onmicrosoft.com
  * `clientId` Je clientId vaší aplikace, které jste zkopírovali z portálu.
* Nyní je třeba zjistit identifikátor uri zpětného volání pro aplikace pro Windows Phone.  Na tomto řádku v zarážku `MainPage` metoda:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
* Spusťte aplikaci a zkopírujte hodnotu z produkce `redirectUri` při průchodu zarážkou.  Měl by vypadat nějak podobně jako

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

* Zpět na **konfigurace** karta aplikace v portálu pro správu Azure, nahraďte hodnotu **RedirectUri** s touto hodnotou.  

## <a name="3-use-adal-to-get-tokens-from-aad"></a>3. Získat tokeny z AAD pomocí ADAL
Základní princip za ADAL je, že vždy, když aplikace potřebuje přístupový token, jednoduše volá `authContext.AcquireToken(…)`, a zbývající ADAL.  

* Prvním krokem je k chybě při inicializaci aplikace `AuthenticationContext` -ADAL je primární třídou.  Toto je, kde je předat ADAL souřadnice musí komunikovat s Azure AD a určit, jak pro ukládání do mezipaměti tokenů.

```csharp
public MainPage()
{
    ...

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

* Nyní najít `Search(...)` metodu, která bude vyvolán při cliks uživatele "Vyhledat" tlačítko v uživatelském rozhraní aplikace.  Tato metoda vytváří požadavek GET na Azure AD Graph API k dotazu pro uživatele, jehož UPN začíná zadaný hledaný termín.  Pro dotaz na rozhraní Graph API, musíte zahrnout access_token v, ale `Authorization` hlavičky požadavku – to přichází ADAL.

```csharp
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try to get a token without triggering any user prompt.
    // ADAL will check whether the requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained the QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
* Pokud je nutné interaktivního ověřování, ADAL použije webové ověřování zprostředkovatele (soubor WAB) Windows Phone a [pokračování modelu](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) zobrazení Azure AD přihlašovací stránce.  Když se uživatel přihlásí, vaše aplikace musí předat ADAL výsledky WAB interakce.  To je jednoduché, implementace `ContinueWebAuthentication` rozhraní:

```csharp
// This method is automatically invoked when the application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass the authentication interaction results to ADAL, which will
    // conclude the token acquisition operation and invoke the callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

* Nyní je čas používat `AuthenticationResult` ADAL vrácená do vaší aplikace.  V `QueryGraph(...)` zpětné volání, připojte access_token jste získali na požadavek GET v hlavičce autorizace:

```csharp
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
* Můžete také `AuthenticationResult` objekt, který chcete zobrazit informace o uživateli ve vaší aplikaci. V `QueryGraph(...)` metoda, použijte výsledek pro zobrazení ID uživatele na stránce:

```csharp
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
* Nakonec se přihlásit uživatele mimo aplikace také můžete ADAL.  Když uživatel klikne na tlačítko "Odhlásit", chceme, abyste ověřili, že další volání `AcquireTokenSilentAsync(...)` se nezdaří.  Pomocí knihovny ADAL to je stejně snadná jako vymazání mezipamětí tokenů:

```csharp
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Blahopřejeme! Teď máte funkční aplikaci pro Windows Phone, která má schopnost ověřovat uživatele, bezpečně volání webového rozhraní API pomocí OAuth 2.0 a získat základní informace o uživateli.  Pokud jste to ještě neudělali, nyní je čas k naplnění vašeho klienta s některými uživateli.  Spuštění aplikace DirectorySearcher a přihlaste se pomocí jeden z těchto uživatelů.  Hledání jiných uživatelů podle jejich UPN.  Zavřete aplikaci a znovu jej spusťte.  Všimněte si, jak uživatelské relace zůstává beze změn.  Odhlásit se a přihlaste se zpět jako jiný uživatel.

ADAL usnadňuje všechny tyto běžné funkce identity začlenit do vaší aplikace.  Postará všechny dirty činností, které pro vás - Správa mezipaměti, podpora protokolu OAuth, představuje uživatele s přihlašovacími údaji uživatelského rozhraní, aktualizace vypršení platnosti tokenů a další.  Všechny skutečně potřebujete vědět, je jednoho volání rozhraní API `authContext.AcquireToken*(…)`.

Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) zajišťuje [zde](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Nyní se můžete přesunout další identity scénářů.  Můžete se pokusit:

[Zabezpečení rozhraní .NET webového rozhraní API pomocí Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

