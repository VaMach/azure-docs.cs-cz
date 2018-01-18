
## <a name="register-your-application"></a>Registrace vaší aplikace
Aplikace můžete zaregistrovat v některém ze dvou způsobů, jak je popsáno v následujících dvou částech.

### <a name="option-1-express-mode"></a>Možnost 1: Expresní režim
Pomocí následujícího postupu můžete rychle registrace vaší aplikace:
1. Přejděte na [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure).
2.  V **název aplikace** pole, zadejte název pro vaši aplikaci.

3. Ujistěte se, že **instalace na základě** zaškrtávací políčko je vybrané a pak vyberte **vytvořit**.

4. Postupujte podle pokynů pro získání ID aplikace a vložte jej do vašeho kódu.

### <a name="option-2-advanced-mode"></a>Možnost 2: Pokročilého režimu
Registrace vaší aplikace a přidat informace o registraci aplikace do vašeho řešení, postupujte takto:
1. Pokud jste ještě nezaregistrovali vaší aplikace, přejděte k [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app).
2. V **název aplikace** pole, zadejte název pro vaši aplikaci. 

3. Ujistěte se, že **instalace na základě** je zaškrtávací políčko nezaškrtnuté a pak vyberte **vytvořit**.

4. Vyberte **přidejte platformu**, vyberte **nativní aplikace**a potom vyberte **Uložit**.

5. V části **aplikace** > **java** > **{hostitele}. { obor názvů}**, otevřete `MainActivity`. 

6.  Nahraďte *[Zadejte sem aplikace Id]* s ID aplikace, které jste právě zaregistrovali na následujícím řádku:

    ```java
    final static String CLIENT_ID = "[Enter the application Id here]";
    ```
<!-- Workaround for Docs conversion bug -->
7. V části **aplikace** > **manifesty**, otevřete *AndroidManifest.xml* souboru.

8. V `manifest\application` uzlu, přidejte následující aktivitu. Díky tomu tak Registry `BrowserTabActivity` aktivity, která umožňuje operačního systému a pokračovat aplikaci po dokončení ověřování:

    ```xml
    <!--Intent filter to capture System Browser calling back to our app after sign-in-->
    <activity
        android:name="com.microsoft.identity.client.BrowserTabActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            
            <!--Add in your scheme/host from registered redirect URI-->
            <!--By default, the scheme should be similar to 'msal[appId]' -->
            <data android:scheme="msal[Enter the application Id here]"
                android:host="auth" />
        </intent-filter>
    </activity>
    ```
<!-- Workaround for Docs conversion bug -->
9. V `BrowserTabActivity` uzlu, nahraďte `[Enter the application Id here]` s ID aplikace.
