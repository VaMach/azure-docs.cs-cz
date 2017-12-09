
## <a name="create-an-application-express"></a>Vytvoření aplikace (Express)
Nyní je nutné zaregistrovat aplikaci v *portálu pro registraci aplikace Microsoft*:
1. Registrace vaší aplikace pomocí [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure)
2.  Zadejte název vaší aplikace a e-mailu
3.  Ujistěte se, že je zaškrtnuté políčko pro instalaci na základě
4.  Postupujte podle pokynů a získat ID aplikace a vložte jej do vašeho kódu

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Přidat informace o registraci aplikace k řešení (Upřesnit)
Nyní je nutné zaregistrovat aplikaci v *portálu pro registraci aplikace Microsoft*:
1. Přejděte na [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app) zaregistrovat aplikaci
2. Zadejte název vaší aplikace a e-mailu 
3. Ujistěte se, že není zaškrtnuto políčko pro instalaci na základě
4. Klikněte na tlačítko `Add Platform`, zvolte položku `Native Application` a klikněte na tlačítko Uložit
5.  Otevřete `MainActivity` (v části `app`  >  `java`  >   *`{host}.{namespace}`* )
6.  Nahraďte *[Zadejte sem aplikace Id]* v řádku počínaje `final static String CLIENT_ID` s ID aplikace, který jste právě zaregistrovali:

```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Otevřete `AndroidManifest.xml` (v části `app`  >  `manifests`) přidejte následující aktivitu pro `manifest\application` uzlu. Takovém postupu zaregistruje `BrowserTabActivity` umožňující operačního systému a pokračovat v aplikaci po dokončení ověřování:
</li>
</ol>

```xml
<!--Intent filter to capture System Browser calling back to our app after Sign In-->
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
<ol start="8">
<li>
V `BrowserTabActivity`, nahraďte `[Enter the application Id here]` s ID aplikace.
</li>
</ol>
