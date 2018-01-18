
# <a name="call-the-microsoft-graph-api-from-an-android-app"></a>Volání Microsoft Graph API z aplikace pro Android

Tato příručka ukazuje, jak získat přístupový token a volání rozhraní Graph API Microsoft nebo jiná rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 nativní aplikace pro Android.

Po dokončení průvodce, vaše aplikace bude moci přijímat přihlášení osobní účty (včetně outlook.com, live.com a dalších) a pracovním a školním účtům v jakémkoli společnosti nebo organizace, která využívá Azure Active Directory. Aplikace pak zavolá rozhraní API, který je chráněný službou Azure Active Directory koncový bod v2.  

## <a name="how-this-sample-works"></a>Fungování této ukázky
![Fungování této ukázky](media/active-directory-develop-guidedsetup-android-intro/android-intro.png)

Ukázkové aplikace, které vytvoříte v této příručce je založena na scénář, kde aplikace pro Android se používá k dotazování webové rozhraní API, které přijímá tokeny z koncového bodu Azure Active Directory v2 (Microsoft Graph API, v tomto případě). V tomto scénáři aplikace přidá získal token na požadavky HTTP přes autorizační hlavičky. Knihovna ověřování společnosti Microsoft (MSAL) zpracovává získání tokenu a obnovení pro vás.

## <a name="prerequisites"></a>Požadavky
* Tato instalace na základě se zaměřuje na Android Studio, ale žádné jiné vývojové prostředí Android aplikace je také přijatelné. 
* Je vyžadován Android SDK 21 nebo novější (SDK 25 je doporučeno).
* Google Chrome nebo webový prohlížeč, který používá vlastní karty je požadovaný pro tuto verzi MSAL pro Android.

> [!NOTE]
> Google Chrome není součástí Visual Studio Emulator for Android. Doporučujeme, abyste otestovali tento kód na emulátoru s 25 rozhraní API nebo bitovou kopii s 21 rozhraní API nebo novější s Google Chrome nainstalována.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Zpracování získání tokenu pro přístup k chráněné webové rozhraní API

Po ověření uživatele je ukázková aplikace obdrží přístupový token, který slouží k dotazu Microsoft Graph API nebo webového rozhraní API, která je zabezpečená službou Azure Active Directory v2.

Rozhraní API, jako je například Microsoft Graph vyžadují token přístupu povolit přístup ke konkrétním prostředkům. Přístupový token je například potřeba čtení profilu uživatele, přístup k kalendáře uživatele nebo odeslání e-mailu. Aplikace může požadovat token přístupu pomocí MSAL k těmto prostředkům přistupovat tak, že zadáte obory rozhraní API. Tento přístupový token se pak přidá do hlavičky HTTP autorizace pro každé volání, které se provádí před k chráněnému prostředku. 

MSAL spravuje ukládání do mezipaměti a obnovení přístupových tokenů pro vás, tak, aby vaše aplikace nemusí.

## <a name="libraries"></a>Knihovny

Tato příručka používá následující knihovny:

|Knihovna|Popis|
|---|---|
|[com.microsoft.identity.client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Knihovna Microsoft ověřování (MSAL)|
