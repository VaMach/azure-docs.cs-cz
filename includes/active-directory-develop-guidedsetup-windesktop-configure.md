
## <a name="create-an-application-express"></a>Vytvoření aplikace (Express)
Nyní je nutné zaregistrovat aplikaci v *portálu pro registraci aplikace Microsoft*:
1. Registrace vaší aplikace pomocí [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)
2.  Zadejte název vaší aplikace a e-mailu
3.  Ujistěte se, že je zaškrtnuté políčko pro instalaci na základě
4.  Postupujte podle pokynů a získat ID aplikace a vložte jej do vašeho kódu

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Přidat informace o registraci aplikace k řešení (Upřesnit)
Nyní je nutné zaregistrovat aplikaci v *portálu pro registraci aplikace Microsoft*:
1. Přejděte na [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app) zaregistrovat aplikaci
2. Zadejte název vaší aplikace a e-mailu 
3. Ujistěte se, že není zaškrtnuto políčko pro instalaci na základě
4. Klikněte na tlačítko `Add Platform`, zvolte položku `Native Application` a klikněte na tlačítko Uložit
5. Zkopírovat identifikátor GUID v ID aplikace, přejděte zpět do Visual Studio, otevřete `App.xaml.cs` a nahraďte `your_client_id_here` s ID aplikace, který jste právě zaregistrovali:

```csharp
private static string ClientId = "your_application_id_here";
```
