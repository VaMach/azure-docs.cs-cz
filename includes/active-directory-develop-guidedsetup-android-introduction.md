
# <a name="call-the-microsoft-graph-api-from-an-android-app"></a>Volání Microsoft Graph API z aplikace pro Android

Tato příručka ukazuje, jak získat přístupový token a volání Microsoft Graph API nebo jiná rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 nativní aplikace pro Android.

Na konci tohoto průvodce bude moci volat chráněné API používání osobních účtů (včetně live.com, outlook.com a dalších) a také pracovní a školní účty z jakékoli společnosti nebo organizace, která má Azure Active Directory vaší aplikace.  

### <a name="how-this-sample-works"></a>Fungování této ukázky
![Fungování této ukázky](media/active-directory-develop-guidedsetup-android-intro/android-intro.png)

Ukázka vytvořit v této příručce je založena na scénář, kde aplikace pro Android se používá k dotazování webové rozhraní API, které přijímá tokeny z Azure Active Directory v2 koncového bodu – v tomto případě Microsoft Graph API. V tomto scénáři se token přidá na požadavky HTTP přes autorizační hlavičky. Získání tokenu a obnova zpracovává pomocí knihovny Microsoft ověřování (MSAL).

### <a name="pre-requisites"></a>Požadavky
* Tuto s průvodcem instalací se zaměřuje na Android Studio, ale žádné jiné vývojové prostředí Android aplikace je také přijatelné. 
* Je vyžadován Android SDK 21 nebo novější (SDK 25 je doporučeno).
* Google Chrome nebo pomocí webového prohlížeče pomocí vlastní karty je požadovaný pro tuto verzi systému Microsoft ověřování knihovny (MSAL) pro Android.

> Poznámka: Google Chrome není součástí Visual Studio Emulator for Android. Doporučujeme k otestování tohoto kódu na emulátoru s 25 rozhraní API nebo bitovou kopii s 21 rozhraní API nebo novější, Google Chrome nainstaloval.


### <a name="how-to-handle-token-acquisition-to-access-a-protected-web-api"></a>Postupy: zpracování získání tokenu pro přístup k chráněné webové rozhraní API

Jakmile se uživatel ověřuje, ukázkové aplikace obdrží token, který slouží k dotazu Microsoft Graph API nebo webového rozhraní API zabezpečeny v2 Microsoft Azure Active Directory.

Rozhraní API, jako je například Microsoft Graph vyžadují token přístupu umožňuje přístup ke konkrétním prostředkům – například čtení profilu uživatele, kalendáře přístup uživatele nebo poslat e-mailu. Aplikace může požadovat token přístupu pomocí MSAL k těmto prostředkům přistupovat tak, že zadáte obory rozhraní API. Tento přístupový token se pak přidá do hlavičky HTTP autorizace pro každé volání proti k chráněnému prostředku. 

MSAL spravuje ukládání do mezipaměti a aktualizaci přístupových tokenů, aby vaše aplikace nemusí.

### <a name="libraries"></a>Knihovny

Tato příručka používá následující knihovny:

|Knihovna|Popis|
|---|---|
|[com.microsoft.identity.Client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Knihovna Microsoft ověřování (MSAL)|
