<properties
    pageTitle="Azure AD B2C ve verzi Preview | Microsoft Azure"
    description="Postup pro sestavení webového rozhraní API .NET pomocí Azure Active Directory B2C zabezpečeného za použití přístupových tokenů OAuth 2.0 pro ověřování."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="msmbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/16/2016"
    ms.author="dastrock"/>

# Azure Active Directory B2C ve verzi Preview: Sestavení webového rozhraní API .NET

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

S Azure Active Directory (Azure AD) B2C můžete zabezpečit webové rozhraní API pomocí přístupových tokenů OAuth 2.0. Tyto tokeny umožňují ověření přístupu klientských aplikací, které používají Azure AD B2C, do rozhraní API. Tento článek ukazuje, jak vytvořit aplikaci "seznam úkolů" pomocí modelu .NET Model-View-Controller (MVC), která zahrnuje uživatelskou registraci, přihlašování a správu profilu. Seznam úkolů každého uživatele bude uložen službou úkolů. Toto webové rozhraní API umožňuje ověřeným uživatelům vytváření a čtení úkolů ve svém seznamu úkolů.

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Vytvoření adresáře Azure AD B2C

Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta. Adresář je kontejner pro všechny vaše uživatele, aplikace, skupiny a další. Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.

## Vytvoření aplikace

Dále musíte vytvořit aplikaci v adresáři B2C. Azure AD díky tomu získá informace potřebné k bezpečné komunikaci s vaší aplikací. Chcete-li vytvořit aplikaci, postupujte podle [těchto pokynů](active-directory-b2c-app-registration.md). Ujistěte se, že:

- Jste do aplikace zahrnuli **webovou aplikaci** nebo **webové rozhraní API**.
- Pro webovou aplikaci používáte **identifikátor URI přesměrování** `https://localhost:44316/`. Toto je výchozí umístění klienta webové aplikace pro tuto ukázku kódu.
- Poznamenejte si **ID aplikace** přiřazené vaší aplikaci. Budete ho potřebovat později.

 [AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Vytvořte svoje zásady

V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md). Klient v této ukázce kódu obsahuje tři činnosti identity: registrace, přihlášení a úprava profilu. Pro každý typ činnosti musíte vytvořit zásadu, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Když vytváříte tyto tři zásady, nezapomeňte:

- Zvolit v okně zprostředkovatelé identity buď **Registrace pomocí ID uživatele** nebo **Registrace pomocí e-mailu**.
- Zvolit **Zobrazovaný název** a další atributy registrace ve svojí registrační zásadě.
- Zvolit **Zobrazovaný název** a deklarace identity **ID objektu** jako deklarace identity aplikace v každé zásadě. Můžete zvolit i další deklarace identity.
- Po vytvoření každé zásady si poznamenejte její **Název**. Tyto názvy zásad budete potřebovat později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po úspěšném vytvoření těchto tří zásad jste připraveni k sestavení aplikace.

## Stáhněte si kód

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-devquickstarts-bug-fix.md)]

Kód k tomuto kurzu [je udržovaný na GitHubu](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet). Chcete-li během čtení tohoto návodu rovnou sestavit ukázku, můžete si [stáhnout kostru projektu v souboru ZIP](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip). Kostru můžete také klonovat:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

Dokončená aplikace je také [k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) nebo ve větvi `complete` stejného úložiště.

Po stažení ukázkového kódu otevřete soubor Visual Studio .sln, abyste mohli začít. Soubor řešení obsahuje dva projekty: `TaskWebApp` a `TaskService`. `TaskWebApp` je webová aplikace MVC, se kterou uživatel komunikuje. `TaskService` je back-endové webové rozhraní API aplikace, které ukládá seznam úkolů každého uživatele.

## Konfigurace webové aplikace úkolů

Když uživatel komunikuje s `TaskWebApp`, klient odešle požadavky do Azure AD a získá zpět tokeny, které lze použít k volání webového rozhraní API `TaskService`. Chcete-li přihlásit uživatele a získat tokeny, musíte poskytnout `TaskWebApp` určité informace o vaší aplikaci. V projektu `TaskWebApp` otevřete v kořenovém adresáři projektu soubor `web.config` a nahraďte hodnoty v oddílu `<appSettings>`:

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="{Enter the name of your B2C directory, e.g. contoso.onmicrosoft.com}" />
    <add key="ida:ClientId" value="{Enter the Application ID assigned to your app by the Azure Portal, e.g.580e250c-8f26-49d0-bee8-1c078add1609}" />
    <add key="ida:ClientSecret" value="{Enter the Application Secret you created in the Azure Portal, e.g. yGNYWwypRS4Sj1oYXd0443n}" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}{1}{2}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="[Enter your sign up policy name, e.g. b2c_1_sign_up]" />
    <add key="ida:SignInPolicyId" value="[Enter your sign in policy name, e.g. b2c_1_sign_in]" />
    <add key="ida:UserProfilePolicyId" value="[Enter your edit profile policy name, e.g. b2c_1_profile_edit" />
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
</appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Jsou zde také dva dekoratéry `[PolicyAuthorize]`, kterým musíte poskytnout název vaší registrační zásady. Atribut `[PolicyAuthorize]` slouží k vyvolání konkrétní zásady, když se uživatel pokusí přistoupit ke stránce aplikace, která vyžaduje ověření.

```C#
// Controllers\HomeController.cs

[PolicyAuthorize(Policy = "{Enter the name of your sign in policy, e.g. b2c_1_my_sign_in}")]
public ActionResult Claims()
{
```

```C#
// Controllers\TasksController.cs

[PolicyAuthorize(Policy = "{Enter the name of your sign in policy, e.g. b2c_1_my_sign_in}")]
public class TasksController : Controller
{
```

Chcete-li zjistit, jak webová aplikace jako je `TaskWebApp` používá Azure AD B2C, navštivte téma [Sestavení webové aplikace .NET](active-directory-b2c-devquickstarts-web-dotnet.md).

## Zabezpečení rozhraní API

Pokud máte klienta, který volá rozhraní API jménem uživatele, můžete `TaskService` zabezpečit pomocí nosných tokenů OAuth 2.0. Vaše rozhraní API může přijímat a ověřovat tokeny pomocí knihovny Microsoft's Open Web Interface pro .NET (OWIN).

### Instalace OWIN
Začněte instalací ověřovacího kanálu OWIN OAuth:

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### Zadejte podrobnosti o svém B2C
Otevřete soubor `web.config` v kořenovém adresáři projektu `TaskService` a nahraďte hodnoty v oddílu `<appSettings>`. Tyto hodnoty budou použity v celém rozhraní API a v knihovně OWIN.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/{1}/{2}?p={3}" />
    <add key="ida:Tenant" value="{Enter the name of your B2C tenant - it usually looks like constoso.onmicrosoft.com}" />
    <add key="ida:ClientId" value="{Enter the Application ID assigned to your app by the Azure Portal}" />
    <add key="ida:PolicyId" value="{Enter the name of one of the policies you created, like `b2c_1_my_sign_in_policy`}" />
</appSettings>
```

### Přidání třídy pro spuštění OWIN
Přidejte třídu pro spuštění OWIN s názvem `Startup.cs` do projektu `TaskService`.  Klikněte pravým tlačítkem myši na projekt, vyberte **Přidat** a **Nová položka**, a poté vyhledejte OWIN.


```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

### Konfigurace ověřování OAuth 2.0
Otevřete soubor `App_Start\Startup.Auth.cs` a implementujte metodu `ConfigureAuth(...)`:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string commonPolicy = ConfigurationManager.AppSettings["ida:PolicyId"];
    private const string discoverySuffix = ".well-known/openid-configuration";

    public void ConfigureAuth(IAppBuilder app)
    {   
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API accepts tokens only from its own clients
            ValidAudience = clientId,
        };

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {   
            // This SecurityTokenProvider fetches the Azure AD B2C metadata and signing keys from the OpenID Connect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, "v2.0", discoverySuffix, commonPolicy)))
        });
    }
}
```

### Zabezpečení ovladače úkolů
Jakmile je aplikace nakonfigurovaná pro použití ověřování OAuth 2.0, můžete svoje webové rozhraní API zabezpečit přidáním značky `[Authorize]` do ovladače úkolů. To je ovladač, kde se odehrává veškerá manipulace se seznamem úkolů, takže byste měli zabezpečit celý ovladač na úrovni tříd. Pro větší kontrolu můžete značku `[Authorize]` přidat také k jednotlivým akcím.

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### Získání informací o uživateli z tokenu
`TasksController` ukládá úkoly do databáze, kde má každý úkol přidruženého uživatele, který úkol „vlastní“. Vlastník je identifikován pomocí **ID objektu** uživatele. (To je důvod, proč bylo nutné přidat ID objektu jako deklaraci identity aplikace ve všech vašich zásadách.)

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## Spuštění ukázkové aplikace

Nakonec sestavte a spusťte `TaskWebApp` a `TaskService`. Zaregistrujte se do aplikace pomocí e-mailové adresy nebo uživatelského jména. Vytvořte nějaké úkoly v seznamu úkolů uživatele a všimněte si, že jsou zachované v rozhraní API i po vypnutí a restartování klienta.

## Úprava zásad

Po zabezpečení rozhraní API pomocí Azure AD B2C můžete experimentovat se zásadami svojí aplikace a zjistit, jaký vliv mají (nebo nemají) na rozhraní API. Můžete také <!--add **identity providers** to the policies, allowing you users to sign into the Task Client using social accounts.  You can also -->manipulovat s deklaracemi identity aplikace v zásadách a změnit, jaké informace o uživateli mají být přístupné v rozhraní API. Jakékoli přidané deklarace identity budou dostupné vašemu webovému rozhraní API .NET MVC v objektu `ClaimsPrincipal`, jak je popsáno výše.

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->



<!--HONumber=Jun16_HO2-->


