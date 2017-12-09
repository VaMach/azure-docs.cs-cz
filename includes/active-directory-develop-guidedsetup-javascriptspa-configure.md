
## <a name="register-your-application"></a>Registrace vaší aplikace

Chcete-li vytvořit aplikaci, vyberte jeden z nich několika způsoby:

### <a name="option-1-register-your-application-express-mode"></a>Možnost 1: Registrace vaší aplikace (expresní režim)
Nyní je nutné zaregistrovat aplikaci v *portálu pro registraci aplikace Microsoft*:

1.  Registrace vaší aplikace pomocí [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)
2.  Zadejte název vaší aplikace a e-mailu
3.  Ujistěte se, možnost *instalace na základě* je zaškrtnuta možnost
4.  Postupujte podle pokynů a získat ID aplikace a vložte jej do vašeho kódu

### <a name="option-2-register-your-application-advanced-mode"></a>Možnost 2: Registrace vaší aplikace (pokročilé režim)

1. Přejděte na [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app) zaregistrovat aplikaci
2. Zadejte název vaší aplikace a e-mailu 
3. Ujistěte se, možnost *instalace na základě* není zaškrtnuta
4.  Klikněte na tlačítko `Add Platform`, zvolte položku`Web`
5. Přidat `Redirect URL` , odpovídají adrese URL aplikace založené na vašem webovém serveru. Najdete v částech níže pokyny o tom, jak nastavit / získat adresu URL přesměrování v sadě Visual Studio a Python.
6. Klikněte na tlačítko *uložit*

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio pokyny pro získání adresy URL přesměrování
> Postupujte podle pokynů a získat adresu URL přesměrování:
> 1.    V *Průzkumníku řešení*, vyberte projekt a podívejte se na `Properties` okno (Pokud se nezobrazí okno vlastností, stiskněte klávesu `F4`)
> 2.    Zkopírujte hodnotu z `URL` do schránky:<br/> ![Vlastnosti projektu](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Přepnout zpět *portálu pro registraci aplikace* a vložte hodnotu jako `Redirect URL` a klikněte na tlačítko 'uložit.

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Nastavení přesměrování URL pro jazyk Python
> Pro jazyk Python může web nastavit port serveru prostřednictvím příkazového řádku. Tato instalace s průvodcem používá port 8080 odkaz ale chování můžete používat všechny ostatní porty, které jsou k dispozici. V každém případě postupujte podle pokynů níže nastavit adresu URL přesměrování v informace o registraci aplikace:<br/>
> - Přepnout zpět *portálu pro registraci aplikace* a nastavte `http://localhost:8080/` jako `Redirect URL`, nebo použijte `http://localhost:[port]/` Pokud používáte vlastní port TCP (kde *[port]* vlastní port TCP je číslo) a klikněte na tlačítko 'uložit.


#### <a name="configure-your-javascript-spa"></a>Konfigurace vaší JavaScript SPA

1.  Vytvořte soubor s názvem `msalconfig.js` obsahující informace o registraci aplikace. Pokud používáte Visual Studio, vyberte projektu (projektu do kořenové složky), klikněte pravým tlačítkem a vyberte: `Add`  >  `New Item`  >  `JavaScript File`. Název`msalconfig.js`
2.  Přidejte následující kód do vaší `msalconfig.js` souboru:

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
Nahraďte <code>Enter_the_Application_Id_here</code> s Id aplikace, který jste právě zaregistrovali
</li>
</ol>
