
## <a name="register-your-application"></a>Registrace vaší aplikace
Aplikace můžete zaregistrovat v některém ze dvou způsobů.

### <a name="option-1-express-mode"></a>Možnost 1: Expresní režim
Pomocí následujícího postupu můžete rychle registrace vaší aplikace:
1. Přejděte na [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure).

2. Vyberte **přidat aplikaci**.

3. V **název aplikace** pole, zadejte název pro vaši aplikaci.

4. Ujistěte se, že **instalace na základě** zaškrtávací políčko je vybrané a pak vyberte **vytvořit**.

5. Postupujte podle pokynů pro získání ID aplikace a vložte jej do vašeho kódu.

### <a name="option-2-advanced-mode"></a>Možnost 2: Pokročilého režimu
Registrace vaší aplikace a přidat informace o registraci aplikace do vašeho řešení, postupujte takto:
1. Pokud jste ještě nezaregistrovali vaší aplikace, přejděte k [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app).

2. Vyberte **přidat aplikaci**.

3. V **název aplikace** pole, zadejte název pro vaši aplikaci. 

4. Ujistěte se, že **instalace na základě** je zaškrtávací políčko nezaškrtnuté a pak vyberte **vytvořit**.

5. Vyberte **přidejte platformu**, vyberte **nativní aplikace**a potom vyberte **Uložit**.

6. V **ID aplikace** pole, zkopírovat identifikátor GUID.

7. Přejděte na Visual Studio, otevřete *App.xaml.cs* souboru a potom můžete nahradit `your_client_id_here` s ID aplikace, který jste právě zaregistrované a zkopírovali.

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```
