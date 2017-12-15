
## <a name="use-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Použít MSAL k získání tokenu pro rozhraní Microsoft Graph API

V této části použijete MSAL k získání tokenu pro rozhraní Microsoft Graph API.

1.  V *MainWindow.xaml.cs* soubor, přidejte odkaz na pro MSAL do třídy:

    ```csharp
    using Microsoft.Identity.Client;
    ```
<!-- Workaround for Docs conversion bug -->

2. Nahraďte `MainWindow` kódu pomocí následující třídy:

    ```csharp
    public partial class MainWindow : Window
    {
        //Set the API Endpoint to Graph 'me' endpoint
        string _graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

        //Set the scope for API call to user.read
        string[] _scopes = new string[] { "user.read" };

        public MainWindow()
        {
            InitializeComponent();
        }

        /// <summary>
        /// Call AcquireTokenAsync - to acquire a token requiring user to sign-in
        /// </summary>
        private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
        {
            AuthenticationResult authResult = null;

            try
            {
                authResult = await App.PublicClientApp.AcquireTokenSilentAsync(_scopes, App.PublicClientApp.Users.FirstOrDefault());
            }
            catch (MsalUiRequiredException ex)
            {
                // A MsalUiRequiredException happened on AcquireTokenSilentAsync. This indicates you need to call AcquireTokenAsync to acquire a token
                System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

                try
                {
                    authResult = await App.PublicClientApp.AcquireTokenAsync(_scopes);
                }
                catch (MsalException msalex)
                {
                    ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
                }
            }
            catch (Exception ex)
            {
                ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
                return;
            }

            if (authResult != null)
            {
                ResultText.Text = await GetHttpContentWithToken(_graphAPIEndpoint, authResult.AccessToken);
                DisplayBasicTokenInfo(authResult);
                this.SignOutButton.Visibility = Visibility.Visible;
            }
        }
    }
    ```

<!--start-collapse-->
### <a name="more-information"></a>Další informace
#### <a name="get-a-user-token-interactively"></a>Získání tokenu uživatele interaktivně
Volání `AcquireTokenAsync` metoda výsledky v okně, které vyzve uživatele k přihlášení. Aplikace obvykle vyžadují uživatelům interaktivní přihlášení při prvním potřebují přístup k chráněnému prostředku. Potřebují může také k přihlášení během bezobslužné operace k získání tokenu selže (například pokud vypršela platnost hesla uživatele).

#### <a name="get-a-user-token-silently"></a>Získání tokenu uživatele bez upozornění
`AcquireTokenSilentAsync` Metoda zpracovává tokenu pořízení a obnovení bez nutnosti zásahu uživatele. Po `AcquireTokenAsync` se spustí poprvé, `AcquireTokenSilentAsync` je obvykle způsob, kterým se získat tokeny, které přístup k chráněným prostředkům pro následující volání, protože k vyžádání nebo obnovení tokeny volání bezobslužně.

Nakonec `AcquireTokenSilentAsync` metoda se nezdaří. Příčiny chyby může být, že uživatel má odhlášení nebo změnit své heslo na jiném zařízení. Když MSAL zjistí, že problém lze vyřešit tím, že vyžaduje interaktivní akce, aktivuje se ho `MsalUiRequiredException` výjimka. Aplikace může zpracovat výjimku dvěma způsoby:

* Provádět volání proti `AcquireTokenAsync` okamžitě. Toto volání se výsledkem výzvy pro uživatele k přihlášení. Tento vzor se obvykle používá v online aplikace tam, kde není žádná dostupné offline obsah pro uživatele. Ukázka generované touto s průvodcem instalací následuje tento vzor, který se zobrazí v čase akce první spuštění ukázky. 
    * Vzhledem k tomu, že se žádný uživatel používá aplikaci, `PublicClientApp.Users.FirstOrDefault()` obsahuje hodnotu null a `MsalUiRequiredException` je vyvolána výjimka. 
    * Kód v ukázce pak ošetří výjimku voláním `AcquireTokenAsync`, výsledkem výzvy pro uživatele k přihlášení.

* Vizuální označení se místo toho může být pro uživatele, kteří interaktivní přihlášení je povinné, tak, aby mohou vybrat správný čas k přihlášení. Nebo můžete zkusit aplikaci `AcquireTokenSilentAsync` později. Tento vzor se často používá, kdy mohou uživatelé používat další funkce aplikace bez přerušení – například, když offline obsah je dostupný v aplikaci. V takovém případě uživatelé mohou rozhodnout, když chtějí Přihlaste se k přístupu k chráněnému prostředku nebo aktualizovat zastaralé informace. Alternativně můžete určit aplikace, aby opakujte `AcquireTokenSilentAsync` obnovení po síti poté, co bylo dočasně nedostupná.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Pro token, který jste obdrželi volání rozhraní Graph API Microsoft

Přidejte následující metodu nové do vaší `MainWindow.xaml.cs`. Aby se používá metoda `GET` požadavek na rozhraní Graph API pomocí hlavičku autorizovat:

```csharp
/// <summary>
/// Perform an HTTP GET request to a URL using an HTTP Authorization header
/// </summary>
/// <param name="url">The URL</param>
/// <param name="token">The token</param>
/// <returns>String containing the results of the GET operation</returns>
public async Task<string> GetHttpContentWithToken(string url, string token)
{
    var httpClient = new System.Net.Http.HttpClient();
    System.Net.Http.HttpResponseMessage response;
    try
    {
        var request = new System.Net.Http.HttpRequestMessage(System.Net.Http.HttpMethod.Get, url);
        //Add the token in Authorization header
        request.Headers.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
        response = await httpClient.SendAsync(request);
        var content = await response.Content.ReadAsStringAsync();
        return content;
    }
    catch (Exception ex)
    {
        return ex.ToString();
    }
}
```
<!--start-collapse-->
### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Další informace o tom, jak volání REST chráněné rozhraní API

V této ukázkové aplikaci používáte `GetHttpContentWithToken` metoda aby HTTP `GET` požadavku pro chráněný prostředek, který vyžaduje token a pak se vraťte obsah volajícímu. Tato metoda přidá získal token v hlavičce HTTP autorizace. Tato ukázka je prostředek Microsoft Graph API *mi* koncový bod, který zobrazí informace o profilu uživatele.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-a-user"></a>Přidání metody se odhlásit uživatele

Odhlášení uživatele, přidejte následující metodu do vaší `MainWindow.xaml.cs` souboru:

```csharp
/// <summary>
/// Sign out the current user
/// </summary>
private void SignOutButton_Click(object sender, RoutedEventArgs e)
{
    if (App.PublicClientApp.Users.Any())
    {
        try
        {
            App.PublicClientApp.Remove(App.PublicClientApp.Users.FirstOrDefault());
            this.ResultText.Text = "User has signed-out";
            this.CallGraphButton.Visibility = Visibility.Visible;
            this.SignOutButton.Visibility = Visibility.Collapsed;
        }
        catch (MsalException ex)
        {
            ResultText.Text = $"Error signing-out user: {ex.Message}";
        }
    }
}
```
<!--start-collapse-->
### <a name="more-information-about-user-sign-out"></a>Další informace o odhlášení uživatele

`SignOutButton_Click` Metoda odstraní uživatele z mezipaměti MSAL uživatele, který efektivně informuje MSAL zapomenutí aktuálního uživatele, aby budoucí žádost o získání tokenu bude úspěšné pouze v případě, že je k interaktivní.

I když aplikace v této ukázce podporuje jednoho uživatele, MSAL podporuje scénáře, kde můžete více účtů přihlášení ve stejnou dobu. Příkladem je e-mailové aplikace, které má uživatel více účtů.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Zobrazit základní informace o tokenu

Chcete-li zobrazit základní informace o tokenu, přidejte následující metodu do vaší *MainWindow.xaml.cs* souboru:

```csharp
/// <summary>
/// Display basic information contained in the token
/// </summary>
private void DisplayBasicTokenInfo(AuthenticationResult authResult)
{
    TokenInfoText.Text = "";
    if (authResult != null)
    {
        TokenInfoText.Text += $"Name: {authResult.User.Name}" + Environment.NewLine;
        TokenInfoText.Text += $"Username: {authResult.User.DisplayableId}" + Environment.NewLine;
        TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
        TokenInfoText.Text += $"Access Token: {authResult.AccessToken}" + Environment.NewLine;
    }
}
```
<!--start-collapse-->
### <a name="more-information"></a>Další informace

Kromě přístupový token, který se používá k volání rozhraní Graph API Microsoft po uživatel přihlásí MSAL také získá ID token. Tento token obsahovat malou podmnožinu informace, které jsou relevantní pro uživatele. `DisplayBasicTokenInfo` Metoda zobrazí základní informace, které jsou obsaženy v tokenu. Například zobrazí uživatele zobrazovaný název a ID, a také datum vypršení platnosti tokenu a řetězec představující přístupový token, sám sebe. Můžete vybrat *volání Microsoft Graph API* tlačítko víckrát a zjistí, že byl znovu stejný token pro následné požadavky. Zobrazí se také datum vypršení platnosti rozšiřovanou při MSAL rozhodne ho je čas k obnovení tokenu.
<!--end-collapse-->

