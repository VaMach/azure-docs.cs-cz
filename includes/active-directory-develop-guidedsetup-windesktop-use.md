
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Použití knihovny ověřování společnosti Microsoft (MSAL) k získání tokenu pro rozhraní Microsoft Graph API

Tato část ukazuje způsob použití MSAL získat token Microsoft Graph API.

1.  V `MainWindow.xaml.cs`, přidejte odkaz na MSAL knihovny do třídy:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Nahraďte <code>MainWindow</code> kódu s použitím třídy:
</li>
</ol>

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
#### <a name="getting-a-user-token-interactive"></a>Získávání interaktivní token uživatele
Volání `AcquireTokenAsync` metoda výsledky v okně výzvy pro uživatele k přihlášení. Aplikace obvykle vyžadují uživatel interaktivní přihlášení při prvním potřebují přístup k chráněnému prostředku nebo když tichou operaci získat token selže (například heslo uživatele jeho platnost).

#### <a name="getting-a-user-token-silently"></a>Získání tokenu uživatele bez upozornění
`AcquireTokenSilentAsync`zpracovává tokenu pořízení a obnovení bez nutnosti zásahu uživatele. Po `AcquireTokenAsync` se spustí poprvé, `AcquireTokenSilentAsync` je obvykle metodu použitou k získat tokeny, použít pro přístup k chráněným prostředkům pro následující volání - volání na vyžádání nebo obnovení tokeny jsou vytvářeny bezobslužně.
Nakonec `AcquireTokenSilentAsync` selže – např. uživatel má odhlášení, nebo došlo ke změně hesla na jiném zařízení. Když MSAL zjistí, že problém lze vyřešit tím, že vyžaduje interaktivní akce, aktivuje se ho `MsalUiRequiredException`. Aplikace může zpracovat výjimku dvěma způsoby:

1.  Volání proti `AcquireTokenAsync` okamžitě, výsledkem výzvy k přihlášení. Tento vzor se obvykle používá v online aplikace tam, kde není žádná offline v aplikaci k dispozici obsah pro uživatele. Ukázka generované touto s průvodcem instalací používá tento vzor: Zobrazí se v čase akce první spustit ukázku: vzhledem k tomu, že žádný uživatel nikdy použili aplikaci, `PublicClientApp.Users.FirstOrDefault()` bude obsahovat hodnotu null a `MsalUiRequiredException` dojde k výjimce. Kód v ukázce pak ošetří výjimku voláním `AcquireTokenAsync` výsledkem výzvy k přihlášení.

2.  Aplikace můžete udělat taky vizuální označení uživateli, které interaktivní přihlášení je povinné, tak, aby si uživatel může vybrat správný čas pro přihlášení, nebo můžete zkusit aplikaci `AcquireTokenSilentAsync` později. To se obvykle používá při uživatel může použít další funkce aplikace bez narušení – například je offline obsah k dispozici v aplikaci. V takovém případě se můžete rozhodnout uživatele, když chtějí přihlášení pro přístup k chráněnému prostředku nebo aktualizovat zastaralé informace, nebo aplikaci můžete rozhodnout opakujte `AcquireTokenSilentAsync` obnovení po síti poté, co byla dočasně nedostupná.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Volání rozhraní Graph API Microsoft pomocí tokenu, který jste obdrželi

1. Přidejte novou metodu níže do vaší `MainWindow.xaml.cs`. Aby se používá metoda `GET` požadavek na rozhraní Graph API pomocí hlavičku autorizovat:

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
### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Další informace o volání REST chráněné rozhraní API

V této ukázkové aplikaci `GetHttpContentWithToken` metoda se používá k zajištění HTTP `GET` požadavku pro chráněný prostředek, který vyžaduje token a pak se vraťte obsah volajícímu. Tato metoda přidá získal token v *HTTP autorizační hlavičky*. Tato ukázka je prostředek Microsoft Graph API *mi* koncového bodu – zobrazí informace o profilu uživatele.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Přidání metody se odhlásit uživatele

1. Přidejte následující metodu do vaší `MainWindow.xaml.cs` se odhlásit uživatele:

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
### <a name="more-info-on-sign-out"></a>Další informace o odhlášení

`SignOutButton_Click`Odebere uživatele z mezipaměti uživatele MSAL – efektivně se tak dozví MSAL zapomenutí aktuálního uživatele, takže budoucí žádost o získání tokenu bude úspěšné pouze v případě, že je k interaktivní.
I když aplikace v této ukázce podporuje jenom jednoho konkrétního uživatele, MSAL podporuje scénáře, kde více účtů může být přihlášeného ve stejnou dobu – příklad je e-mailové aplikace, které má uživatel více účtů.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Zobrazit základní informace o tokenu

1. Přidejte následující metodu do k vaší `MainWindow.xaml.cs` zobrazit základní informace o token:

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

Tokeny získaných prostřednictvím *OpenID Connect* také obsahovat malou podmnožinu informace, které jsou relevantní pro uživatele. `DisplayBasicTokenInfo`Zobrazí základních informací obsažených v tokenu: například uživatele zobrazovaný název a ID, a také datum vypršení platnosti tokenu a řetězec představující přístup token sám sebe. Zobrazí se tato informace je zobrazen. Můžete dosáhl *volání Microsoft Graph API* tlačítko víckrát a zjistí, že byl znovu stejný token pro následné požadavky. Zobrazí se také datum vypršení platnosti rozšiřovanou při MSAL rozhodne ho je čas k obnovení tokenu.
<!--end-collapse-->

