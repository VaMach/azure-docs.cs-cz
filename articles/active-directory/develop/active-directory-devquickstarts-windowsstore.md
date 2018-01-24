---
title: "Azure AD univerzální platformu Windows (UWP/XAML) Začínáme | Microsoft Docs"
description: "Aplikace pro Windows Store sestavení, které integraci se službou Azure AD pro přihlášení a volání služby Azure AD chráněné rozhraní API pomocí metody OAuth."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mtillman
editor: 
ms.assetid: 3b96a6d1-270b-4ac1-b9b5-58070c896a68
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 11/30/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 8bc8c3a897363da2a8ebe7ac6bd8798c8e22ba04
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="azure-ad-windows-universal-platform-uwpxaml-getting-started"></a>Azure AD univerzální platformu Windows (UWP/XAML) Začínáme
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> Windows Store 8.1 a předchozí verze projekty nejsou podporované v Visual Studio 2017.  Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

Pokud vyvíjíte aplikace pro Windows Store, Azure Active Directory (Azure AD) je jednoduchá a přímočará k ověřování uživatelů s účty služby Active Directory. Díky integraci s Azure AD, můžete aplikace bezpečně využívat žádné webové rozhraní API, který je chráněný službou Azure AD, jako je například rozhraní API Office 365 nebo rozhraní API služby Azure.

Pro desktopové aplikace Windows Store, které potřebují přístup k chráněným prostředkům Azure AD poskytuje službě Active Directory Authentication Library (ADAL). Jediný účel ADAL je snadno pro aplikaci načíst tokeny přístupu. K předvedení, jak je snadné, tento článek ukazuje, jak vytvářet aplikace pro DirectorySearcher Windows Store který:

* Získá přístup k tokeny pro volání rozhraní API služby Azure AD Graph pomocí [protokol ověřování OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Vyhledá adresář pro uživatele s hlavní název daného uživatele (UPN).
* Uživatelé přihlásí se.

## <a name="before-you-get-started"></a>Než začnete
* Stažení [kostru projektu](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), nebo stáhnout [hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip). Každého stažení je řešením sady Visual Studio 2015.
* Musíte taky klient služby Azure AD, ve kterém se má vytvořit uživatele a registraci aplikace. Pokud ještě nemáte klienta, [zjistěte, jak získat](active-directory-howto-tenant.md).

Až budete připravení, postupujte podle pokynů v následujících třech částech.

## <a name="step-1-register-the-directorysearcher-app"></a>Krok 1: Zaregistrujte DirectorySearcher aplikace
Pokud chcete povolit aplikaci získat tokeny, musíte nejprve zaregistrovat v klientovi služby Azure AD a udělit mu oprávnění k přístupu k Azure AD Graph API. Zde je uveden postup:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na horním panelu klikněte na váš účet. Potom v části **Directory** vyberte klienta služby Active Directory, ve které chcete zaregistrovat aplikaci.
3. Klikněte na tlačítko **více služeb** v levém podokně a potom vyberte **Azure Active Directory**.
4. Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.
5. Postupujte podle výzev a vytvořte **nativní klientská aplikace**.
  * **Název** popis aplikace pro uživatele.
  * **Identifikátor URI pro přesměrování** je kombinace schématu a řetězec, Azure AD se používá k vrácení odpovědi tokenu. Zadejte hodnotu zástupného symbolu pro nyní (například **http://DirectorySearcher**). Později budete nahradí hodnotu.
6. Po dokončení registrace Azure AD přiřadí aplikace ID jedinečný aplikace. Zkopírujte hodnotu na **aplikace** kartě, protože ho budete potřebovat později.
7. Na **nastavení** vyberte **požadovaných oprávnění**a potom vyberte **přidat**.
8. Pro **Azure Active Directory** aplikace, vyberte **Microsoft Graph** jako rozhraní API.
9. V části **delegovaná oprávnění**, přidejte **přístup k adresáři jako přihlášeného uživatele** oprávnění. Díky tomu aplikaci dotaz rozhraní Graph API pro uživatele.

## <a name="step-2-install-and-configure-adal"></a>Krok 2: Instalace a konfigurace ADAL
Nyní když máte aplikaci ve službě Azure AD, můžete nainstalovat ADAL a zadejte kód, týkající se identity. Pokud chcete povolit ADAL ke komunikaci s Azure AD, jí některé informace o registraci aplikace.

1. Přidání ADAL do projektu DirectorySearcher pomocí konzoly Správce balíčků.

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

2. V projektu DirectorySearcher otevřete MainPage.xaml.cs.
3. Nahraďte hodnoty v **konfiguračních hodnot** oblasti s hodnotami, které jste zadali v portálu Azure. Váš kód odkazoval na tyto hodnoty vždy, když ho využívá ADAL.
  * *Klienta* je doména klienta služby Azure AD (například contoso.onmicrosoft.com).
  * *ClientId* je ID klienta aplikace, které jste zkopírovali z portálu.
4. Nyní je třeba zjistit identifikátor URI zpětného volání pro aplikace pro Windows Store. Na tomto řádku v zarážku `MainPage` metoda:
    ```
    redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
    ```
5. Sestavte řešení, a ujistěte se, že se obnoví všechny odkazy na balíček. Pokud chybí všechny balíčky, otevřete Správce balíčků NuGet a jejich obnovení.
6. Spusťte aplikaci a zkopírujte hodnotu `redirectUri` při průchodu zarážkou. Hodnota by měla vypadat přibližně takto:

    ```
    ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
    ```

7. Zpět na **nastavení** karta aplikace na webu Azure portal a přidat **RedirectUri** s předchozí hodnotou.  

## <a name="step-3-use-adal-to-get-tokens-from-azure-ad"></a>Krok 3: Použití ADAL získat tokeny z Azure AD
Základní princip za ADAL je, že vždy, když aplikace potřebuje přístupový token, jednoduše volá `authContext.AcquireToken(…)`, a zbývající ADAL.  

1. Inicializace aplikace `AuthenticationContext`, což je primární třídou adal. Tato akce předá ADAL souřadnice musí komunikovat s Azure AD a určit, jak pro ukládání do mezipaměti tokenů.

    ```csharp
    public MainPage()
    {
        ...

        authContext = new AuthenticationContext(authority);
    }
    ```

2. Vyhledejte `Search(...)` metodu, která je volána, když uživatelé kliknou na **vyhledávání** tlačítko v uživatelském rozhraní aplikace. Tato metoda vytváří požadavek get na Azure AD Graph API k dotazu pro uživatele, jehož UPN začíná zadaný hledaný termín. Dotaz na rozhraní Graph API, zahrňte přístupový token v žádosti **autorizace** záhlaví. Toto je, kde odeslán ADAL.

    ```csharp
    private async void Search(object sender, RoutedEventArgs e)
    {
        ...
        AuthenticationResult result = null;
        try
        {
            result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
        }
        catch (AdalException ex)
        {
            if (ex.ErrorCode != "authentication_canceled")
            {
                ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
            }
            return;
        }
        ...
    }
    ```
    Když aplikace požaduje token voláním `AcquireTokenAsync(...)`, ADAL pokusí vrátit token bez požadavku uživatele na přihlašovací údaje. Pokud ADAL zjistí, že uživatel musí pro přihlášení k získání tokenu, zobrazí přihlašovací dialogové okno, shromažďuje přihlašovací údaje uživatele a vrátí token po úspěšném provedení ověřování. Pokud se nepodařilo vrátit token z jakéhokoli důvodu ADAL *AuthenticationResult* stav je k chybě.
3. Nyní je čas použití tokenu přístupu, kterou jste právě získali. Také v `Search(...)` metoda, připojit k rozhraní Graph API požadavek get v tokenu **autorizace** hlavičky:

    ```csharp
    // Add the access token to the Authorization header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

    ```
4. Můžete použít `AuthenticationResult` objekt, který chcete zobrazit informace o uživateli v aplikaci, jako je například ID uživatele:

    ```csharp
    // Update the page UI to represent the signed-in user
    ActiveUser.Text = result.UserInfo.DisplayableId;
    ```
5. Můžete taky ADAL pro přihlášení uživatelé mimo aplikaci. Když uživatel klikne **Odhlásit** tlačítko, ujistěte se, že další volání `AcquireTokenAsync(...)` zobrazení přihlášení. Pomocí knihovny ADAL tato akce je stejně snadná jako vymazání mezipamětí tokenů:

    ```csharp
    private void SignOut()
    {
        // Clear session state from the token cache.
        authContext.TokenCache.Clear();

        ...
    }
    ```

## <a name="whats-next"></a>Kam dál
Teď máte funkční aplikace pro Windows Store, můžete ověřovat uživatele, bezpečně volání webových rozhraní API pomocí OAuth 2.0 a získat základní informace o uživateli.

Pokud již nejsou naplněny vašeho klienta s uživateli, nyní je čas Uděláte to tak.
1. Spuštění aplikace DirectorySearcher a pak se přihlaste pomocí jednoho uživatele.
2. Hledání jiných uživatelů podle jejich UPN.
3. Zavřete aplikaci a znovu ho spusťte. Všimněte si, jak uživatelské relace zůstává beze změn.
4. Odhlásit se kliknutím pravým tlačítkem myši zobrazíte dolním panelu a pak se přihlaste zpět v jako jiný uživatel.

ADAL usnadňuje začlenit všechny tyto běžné funkce identity do aplikace. Se postará všechnu práci dirty, jako je například Správa mezipaměti podpora protokolu OAuth, představuje uživatele s přihlašovacími údaji uživatelského rozhraní, a aktualizovat platnost tokenů. Je nutné znát jenom jednoho volání rozhraní API, `authContext.AcquireToken*(…)`.

Odkaz, stáhněte si [hotová ukázka](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (bez vašich hodnot nastavení).

Nyní se můžete přesunout další identity scénářů. Zkuste například [zabezpečení webového rozhraní API .NET s Azure AD](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
