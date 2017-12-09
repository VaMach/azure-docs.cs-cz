
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Volání Microsoft Graph API z aplikace na ploše systému Windows

Tato příručka ukazuje, jak získat přístupový token a volání Microsoft Graph API nebo jiná rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 nativní aplikace Windows Desktop .NET (XAML).

Na konci tohoto průvodce bude moci volat chráněné API používání osobních účtů (včetně live.com, outlook.com a dalších) a také pracovní a školní účty z jakékoli společnosti nebo organizace, která má Azure Active Directory vaší aplikace.  

> Tato příručka vyžaduje Visual Studio 2015 Update 3 nebo Visual Studio 2017.  Nemáte ho? [Stáhněte si Visual Studio 2017 zdarma](https://www.visualstudio.com/downloads/)

### <a name="how-this-guide-works"></a>Jak funguje tato příručka

![Jak funguje tato příručka](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.png)

Ukázkové aplikace vytvořené v této příručce umožňuje aplikace Windows Desktop dotazovat Microsoft Graph API nebo webového rozhraní API, které přijímá tokeny z koncového bodu v2 Azure Active Directory. V tomto scénáři se token přidá na požadavky HTTP přes autorizační hlavičky. Získání tokenu a obnova zpracovává pomocí knihovny Microsoft ověřování (MSAL).


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Zpracování získání tokenu pro přístup k chráněné webové rozhraní API

Jakmile se uživatel ověřuje, ukázkové aplikace obdrží token, který slouží k dotazu Microsoft Graph API nebo webového rozhraní API zabezpečeny v2 Microsoft Azure Active Directory.

Rozhraní API, jako je například Microsoft Graph vyžadují token přístupu umožňuje přístup ke konkrétním prostředkům – například čtení profilu uživatele, kalendáře přístup uživatele nebo poslat e-mailu. Aplikace může požadovat token přístupu pomocí MSAL k těmto prostředkům přistupovat tak, že zadáte obory rozhraní API. Tento přístupový token se pak přidá do hlavičky HTTP autorizace pro každé volání proti k chráněnému prostředku. 

MSAL spravuje ukládání do mezipaměti a aktualizaci přístupových tokenů, aby vaše aplikace nemusí.


### <a name="nuget-packages"></a>Balíčky NuGet

Tato příručka používá následující balíčky NuGet:

|Knihovna|Popis|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Knihovna Microsoft ověřování (MSAL)|

