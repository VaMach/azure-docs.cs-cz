
# <a name="call-the-microsoft-graph-api-from-an-ios-application"></a>Volání rozhraní Microsoft Graph API z aplikace pro iOS

Tato příručka ukazuje, jak aplikace nativní aplikace pro iOS (Swift) můžete volat rozhraní API, které vyžadují přístupové tokeny z koncového bodu v2.0 Microsoft Azure Active Directory (Azure AD). Průvodce vysvětluje, jak získat přístupové tokeny a použít je v volání rozhraní Graph API Microsoft a jiných rozhraní API.

Po dokončení praktická cvičení v této příručce, může volat chráněné rozhraní API v jakémkoli společnosti nebo organizace, která má Azure AD. Aplikace můžete nastavit chráněné volání rozhraní API pomocí osobní účty, jako je live.com, outlook.com a dalších, jakož i pracovní nebo školní účty.

## <a name="prerequisites"></a>Požadavky
- XCode verze 8.x je vyžadována pro vzorku, který se vytvoří v této příručce. XCode z si můžete stáhnout [iTunes webu](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode stáhnout URL").
- [Carthage](https://github.com/Carthage/Carthage) manager závislostí je nutná pro správu balíčku.

## <a name="how-this-guide-works"></a>Jak funguje tato příručka

![Jak funguje tato příručka](media/active-directory-develop-guidedsetup-ios-introduction/iosintro.png)

V této příručce umožňuje ukázkovou aplikaci aplikace pro iOS k dotazování Microsoft Graph API nebo webové rozhraní API, které přijímá tokeny z koncového bodu v2.0 Azure AD. V tomto scénáři token se přidá na požadavky HTTP pomocí **autorizace** záhlaví. Získání tokenu a obnovení, jsou zpracovávány pomocí knihovny Microsoft ověřování (MSAL).


### <a name="handle-token-acquisition-for-access-to-protected-web-apis"></a>Popisovač získání tokenu pro přístup k chráněné webové rozhraní API

Jakmile se uživatel ověřuje, ukázkové aplikace získá token zabezpečení. Token se používá k dotazování Microsoft Graph API nebo webové rozhraní API, která je zabezpečená službou koncového bodu v2.0 Azure AD.

Rozhraní API, jako je například Microsoft Graph vyžadují token přístupu povolit přístup ke konkrétním prostředkům. Tokeny jsou potřeba ke čtení profilu uživatele, přístup uživatele kalendář, e-mailovou zprávu a tak dále. Aplikace může požadovat token přístupu pomocí MSAL a zadání obory rozhraní API. Přístupový token se přidá do HTTP **autorizace** záhlaví pro každé volání, které se provádí před k chráněnému prostředku.

MSAL spravuje ukládání do mezipaměti a aktualizaci přístupových tokenů, aby vaše aplikace nemusí.


## <a name="libraries"></a>Knihovny

Tato příručka používá následující knihovny:

|Knihovna|Popis|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|Microsoft ověřování knihovny Preview pro iOS|

