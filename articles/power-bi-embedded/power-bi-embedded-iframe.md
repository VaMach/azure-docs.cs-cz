<properties
   pageTitle="Microsoft Power BI Embedded Preview – vložení sestavy Power BI pomocí elementu IFrame"
   description="Microsoft Power BI Embedded Preview – nepostradatelný kód pro integraci sestavy do aplikace; jak zajistit ověření pomocí tokenu aplikace Power BI Embedded; jak získat sestavy"
   services="power-bi-embedded"
   documentationCenter=""
   authors="dvana"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="05/16/2016"
   ms.author="derrickv"/>

# Vložení sestavy Power BI pomocí elementu IFrame
Tento článek vás seznámí s kódem nepostradatelným pro integraci, tedy vložení sestavy do vaší aplikace s použitím rozhraní REST API služby **Power BI Embedded**, tokenů aplikace, elementu IFrame a určitého kódu JavaScript.

V tématu [Začínáme s Microsoft Power BI Embedded Preview](power-bi-embedded-get-started.md) se naučíte konfigurovat **kolekci** **pracovních prostorů** (pracovní prostor může být jeden nebo jich může být více) pro obsah sestav. Potom v tématu [Začínáme s ukázkou Microsoft Power BI Embedded](power-bi-embedded-get-started-sample.md) naimportujete do **pracovního prostoru** sestavu.

Tento článek popisuje kroky pro vložení sestavy do vaší aplikace. Abyste podle nich mohli postupovat, měli byste si stáhnout ukázku [integrace sestavy pomocí elementu IFrame](https://github.com/Azure-Samples/power-bi-embedded-iframe) z webu GitHub. Tato ukázka představuje jednoduchou aplikaci webového formuláře ASP.NET a jejím cílem je ilustrovat nepostradatelný kód C# a JavaScript potřebný k integraci sestavy. Pokročilejší ukázku, kdy se k integraci sestavy využívá návrhové schéma Model-View-Controller (MVC), naleznete jako [ukázkovou webovou aplikaci pro řídicí panel](http://go.microsoft.com/fwlink/?LinkId=761493) na webu GitHub.

Začněme popisem toho, jak integrovat sestavu **Power BI Embedded** do vaší aplikace.

Zde jsou kroky pro integraci sestavy.

- Krok 1: [Získání sestavy v pracovním prostoru](#GetReport). V tomto kroku použijete tok tokenu aplikace pro získání přístupového tokenu k vyvolání operace REST [Get Reports](https://msdn.microsoft.com/library/mt711510.aspx). Po získání sestavy ze seznamu **Get Reports** můžete tuto sestavu vložit do aplikace pomocí elementu **IFrame**.
- Krok 2: [Vložení sestavy do aplikace](#EmbedReport). V tomto kroku integrujete, tedy vložíte sestavu do webové aplikace pomocí příslušného vkládacího tokenu, určitého kódu JavaScript a elementu IFrame.

Pokud se chcete podívat, jak integrace sestavy probíhá, stáhněte si ukázku [integrace sestavy pomocí elementu IFrame](https://github.com/Azure-Samples/power-bi-embedded-iframe) z webu GitHub a nakonfigurujte 3 položky nastavení v souboru Web.Config:

- **AccessKey**: **Přístupový klíč** slouží ke generování webového tokenu JSON (JWT), který se používá k získání sestav a vložení sestavy. Další informace o získání **přístupového klíče** naleznete v tématu [Začínáme s Microsoft Power BI Embedded Preview](power-bi-embedded-get-started.md).
- **WorkspaceName**: Další informace o získání **názvu pracovního prostoru** naleznete v tématu [Začínáme s Microsoft Power BI Embedded Preview](power-bi-embedded-get-started.md).
- **WorkspaceId**: Další informace o získání **ID pracovního prostoru** naleznete v tématu [Začínáme s Microsoft Power BI Embedded Preview](power-bi-embedded-get-started.md).

V dalších částech naleznete kód nutný pro integraci sestavy.

<a name="GetReport"/>
## Získání sestavy v pracovním prostoru

Při integraci sestavy do aplikace potřebujete její **ID** a **embedUrl**. Pokud chcete získat **ID** a **embedUrl** sestavy, vyvoláte operaci REST [Get Reports](https://msdn.microsoft.com/library/mt711510.aspx) a zvolíte sestavu ze seznamu ve formátu JSON. V části [Vložení sestavy do aplikace](#EmbedReport) použijete údaje **ID** a **embedUrl** k uvedené operaci.

### Odpověď na operaci Get Reports ve formátu JSON
```
{
  "@odata.context":"https://api.powerbi.com/beta/collections/{WorkspaceName}/workspaces/{WorkspaceId}/$metadata#reports","value":[
    {
      "id":"804d3664-…-e71882055dba","name":"Import report sample","webUrl":"https://embedded.powerbi.com/reports/804d3664-...-e71882055dba","embedUrl":"https://embedded.powerbi.com/appTokenReportEmbed?reportId=804d3664-...-e71882055dba"
    },{
      "id":"1d7cff58-…-380610e263a9","name":"Sample Report 2","webUrl":"https://embedded.powerbi.com/reports/1d7cff58-...-380610e263a9","embedUrl":"https://embedded.powerbi.com/appTokenReportEmbed?reportId=1d7cff58-...-380610e263a9"
    }
  ]
}

```

Při volání operace REST [Get Reports](https://msdn.microsoft.com/library/mt711510.aspx) použijete token aplikace. Další informace o toku tokenu aplikace v Power BI naleznete [v tomto tématu](power-bi-embedded-app-token-flow.md). Následující kód popisuje, jak získat seznam sestav ve formátu JSON. Postup vložení sestavy naleznete v části [Vložení sestavy do aplikace](#EmbedReport).

```
protected void getReportsButton_Click(object sender, EventArgs e)
{
    //Get an app token to generate a JSON Web Token (JWT). An app token flow is a claims-based design pattern.
    //To learn how you can code an app token flow to generate a JWT, see the PowerBIToken class.
    var appToken = PowerBIToken.CreateDevToken(workspaceName, workspaceId);

    //After you get a PowerBIToken which has Claims including your WorkspaceName and WorkspaceID,
    //you generate JSON Web Token (JWT) . The Generate() method uses classes from System.IdentityModel.Tokens: SigningCredentials,
    //JwtSecurityToken, and JwtSecurityTokenHandler.
    string jwt = appToken.Generate(accessKey);

    //Set app token textbox to JWT string to show that the JWT was generated
    appTokenTextbox.Text = jwt;

    //Construct reports uri resource string
    var uri = String.Format("https://api.powerbi.com/beta/collections/{0}/workspaces/{1}/reports", workspaceName, workspaceId);

    //Configure reports request
    System.Net.WebRequest request = System.Net.WebRequest.Create(uri) as System.Net.HttpWebRequest;
    request.Method = "GET";
    request.ContentLength = 0;

    //Set the WebRequest header to AppToken, and jwt
    //Note the use of AppToken instead of Bearer
    request.Headers.Add("Authorization", String.Format("AppToken {0}", jwt));

    //Get reports response from request.GetResponse()
    using (var response = request.GetResponse() as System.Net.HttpWebResponse)
    {
        //Get reader from response stream
        using (var reader = new System.IO.StreamReader(response.GetResponseStream()))
        {
            //Deserialize JSON string into PBIReports
            PBIReports PBIReports = JsonConvert.DeserializeObject<PBIReports>(reader.ReadToEnd());

            //Clear Textbox
            tb_reportsResult.Text = string.Empty;

            //Get each report
            foreach (PBIReport rpt in PBIReports.value)
            {
                tb_reportsResult.Text += String.Format("{0}\t{1}\t{2}\n", rpt.id, rpt.name, rpt.embedUrl);
            }
        }
    }
}
```

<a name="EmbedReport"/>
## Vložení sestavy do aplikace

Před vložením sestavy do vaší aplikace potřebujete příslušný vkládací token. Ten se podobá tokenu aplikace, který se používá k volání operací REST **Power BI Embedded**, ale generuje se pro prostředek představující sestavu, nikoli pro prostředek REST. Následující kód slouží k získání tokenu aplikace pro sestavu. Postup použití tokenu aplikace naleznete v části [Vložení sestavy do vaší aplikace](#EmbedReportJS).

<a name="EmbedReportToken"/>
### Získání tokenu aplikace pro sestavu

```
protected void getReportAppTokenButton_Click(object sender, EventArgs e)
{
    //Get an embed token for a report.
    var token = PowerBIToken.CreateReportEmbedToken(workspaceName, workspaceId, getReportAppTokenTextBox.Text);

    //After you get a PowerBIToken which has Claims including your WorkspaceName and WorkspaceID,
    //you generate JSON Web Token (JWT) . The Generate() method uses classes from System.IdentityModel.Tokens: SigningCredentials,
    //JwtSecurityToken, and JwtSecurityTokenHandler.
    string jwt = token.Generate(accessKey);

    //Set textbox to JWT string. The JavaScript embed code uses the embed jwt for a report.
    reportAppTokenTextbox.Text = jwt;
}
```

<a name="EmbedReportJS"/>
### Vložení sestavy do vaší aplikace

K vložení sestavy **Power BI** do aplikace použijete element IFrame a určitý kód JavaScript. Zde je příklad elementu IFrame a kódu JavaScript pro vložení sestavy. Pokud chcete vidět veškerý ukázkový kód pro vložení sestavy, podívejte se na ukázku [integrace sestavy pomocí elementu IFrame](https://github.com/Azure-Samples/power-bi-embedded-iframe) na webu GitHub.

![IFrame](media\power-bi-embedded-integrate-report\Iframe.png)


```
window.onload = function () {
    // Client side click to embed a selected report.
    var el = document.getElementById("bEmbedReportAction");
    if (el.addEventListener) {
        el.addEventListener("click", updateEmbedReport, false);
    } else {
        el.attachEvent('onclick', updateEmbedReport);
    }

};

// Update embed report
function updateEmbedReport() {
    // Check if the embed url was selected
    var embedUrl = document.getElementById('tb_EmbedURL').value;

    // To load a report do the following:
    // 1: Set the url
    // 2: Add a onload handler to submit the auth token
    var iframe = document.getElementById('iFrameEmbedReport');
    iframe.src = embedUrl;
    iframe.onload = postActionLoadReport;
}

// Post the auth token to the iFrame.
function postActionLoadReport() {

    // Get the app token.
    accessToken = document.getElementById('MainContent_reportAppTokenTextbox').value;

    // Construct the push message structure
    var m = { action: "loadReport", accessToken: accessToken };
    message = JSON.stringify(m);

    // Push the message.
    iframe = document.getElementById('iFrameEmbedReport');
    iframe.contentWindow.postMessage(message, "*");
}
```
Po vložení do aplikace můžete sestavu filtrovat. V další části se dozvíte, jak toho dosáhnout pomocí syntaxe adresy URL.

## Filtrování sestavy

Vloženou sestavu můžete filtrovat pomocí syntaxe adresy URL. Uděláte to tak, že parametr řetězce dotazu přidáte do zdrojové adresy URL elementu IFrame se specifikovaným filtrem. Můžete **filtrovat podle hodnoty** a **skrýt podokno filtru**.


**Filtrování podle hodnoty**

Pokud chcete filtrovat podle hodnoty, použijte syntaxi dotazu **$filter** s operátorem **eq**, a to takto:

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-0694-...-ee9655d54a4a&
$filter={tableName/fieldName}%20eq%20'{fieldValue}'
```

Například byste mohli vyfiltrovat položky s řetězcem úložiště Lindseys. Část adresy URL představující filtr by vypadala takto:

```
$filter=Store/Chain%20eq%20'Lindseys'
```

> [AZURE.NOTE] V řetězci {tableName/fieldName} nesmí být mezery ani speciální znaky. Na místo položky {FieldValue} je možné vložit jednu hodnotu představující kategorii.

**Skrytí podokna filtru**

Pokud chcete skrýt **podokno filtru**, přidáte do řetězce dotazu sestavy položku **filterPaneEnabled**, a to takto:

```
&filterPaneEnabled=false
```

## Závěr

V tomto článku jsme vás seznámili s kódem pro integraci sestavy **Power BI** do aplikace. Pokud chcete s integrací sestavy do aplikace rychle začít, stáhněte si tyto ukázky z webu GitHub:

- [Ukázka integrace sestavy pomocí elementu IFrame](https://github.com/Azure-Samples/power-bi-embedded-iframe)
- [Ukázková webová aplikace pro řídicí panel](http://go.microsoft.com/fwlink/?LinkId=761493)

## Viz také
- [Začínáme s Microsoft Power BI Embedded Preview](power-bi-embedded-get-started.md)
- [Začínáme s ukázkou](power-bi-embedded-get-started-sample.md)
- [System.IdentityModel.Tokens.SigningCredentials](https://msdn.microsoft.com/library/system.identitymodel.tokens.signingcredentials.aspx)
- [System.IdentityModel.Tokens.JwtSecurityToken](https://msdn.microsoft.com/library/system.identitymodel.tokens.jwtsecuritytoken.aspx)
- [System.IdentityModel.Tokens.JwtSecurityTokenHandler](https://msdn.microsoft.com/library/system.identitymodel.tokens.signingcredentials.aspx)
- [Operace Get Reports](https://msdn.microsoft.com/library/mt711510.aspx)



<!--HONumber=Jun16_HO2-->


