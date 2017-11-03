---
title: "Vytvořit aplikaci .NET pro Service Fabric | Microsoft Docs"
description: "Zjistěte, jak vytvořit aplikaci s front-endové ASP.NET Core a spolehlivá služba stavová back-end a nasazení aplikací do clusteru."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 5a095663b7e716fd63322c9f89f67a1f3187638b
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2017
---
# <a name="create-and-deploy-an-application-with-an-aspnet-core-web-api-front-end-service-and-a-stateful-back-end-service"></a>Vytvoření a nasazení aplikace pomocí služby front-endové webové rozhraní API ASP.NET Core a stavové služby back-end
V tomto kurzu je součástí, jednu z řady.  Se dozvíte, jak vytvořit aplikaci Azure Service Fabric pomocí webového rozhraní API ASP.NET Core front-end a stavové služby back-end ukládat data. Jakmile budete hotovi, máte hlasovací aplikaci s ASP.NET Core web, který je front-end, který uloží výsledků hlasování ve stavové služby back-end v clusteru. Pokud nechcete, aby ručně vytvořit hlasovací aplikaci, můžete [stáhnout zdrojový kód](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) pro hotová aplikace a přeskočit na [provede hlasujících ukázkovou aplikaci](#walkthrough_anchor).

![Diagram aplikace](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

V rámci jedna řada, zjistíte, jak:

> [!div class="checklist"]
> * Vytvoření služby webového rozhraní API ASP.NET Core jako stavové spolehlivé služby
> * Vytvoření služby webové aplikace ASP.NET Core jako bezstavové webové služby
> * Použít reverzní proxy server ke komunikaci s stavové služby

V této série kurzu zjistíte, jak:
> [!div class="checklist"]
> * Sestavení aplikace .NET Service Fabric
> * [Nasazení aplikace na vzdálený cluster](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Konfigurace CI/CD pomocí Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Nastavení monitorování a diagnostiky pro aplikaci](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu:
- Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Nainstalovat Visual Studio 2017](https://www.visualstudio.com/) a nainstalujte **Azure development** a **ASP.NET a webové vývoj** úlohy.
- [Instalace Service Fabric SDK](service-fabric-get-started.md)

## <a name="create-an-aspnet-web-api-service-as-a-reliable-service"></a>Vytvoření služby ASP.NET Web API jako spolehlivě.
Nejprve vytvořte webu front-end hlasovací aplikace pomocí ASP.NET Core. ASP.NET Core je odlehčený, napříč platformami webové rozhraní vývoj, které vám pomůže vytvořit moderní webové uživatelské rozhraní a webovým rozhraním API. Pokud chcete získat úplný znalosti o tom, jak ASP.NET Core integruje se službou Service Fabric, důrazně doporučujeme čtení [ASP.NET Core v Service Fabric spolehlivé služby](service-fabric-reliable-services-communication-aspnetcore.md) článku. Teď může v tomto kurzu vám rychle začít. Další informace o ASP.NET Core, najdete v článku [ASP.NET základní dokumentace](https://docs.microsoft.com/aspnet/core/).

> [!NOTE]
> V tomto kurzu vychází z [ASP.NET Core nástrojů pro Visual Studio 2017](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/start-mvc). .NET Core nástrojů pro Visual Studio 2015 se již nebude aktualizováno.

1. Spusťte sadu Visual Studio jako **správce**.

2. Vytvoření projektu s **soubor**->**nový**->**projektu**

3. V dialogovém okně **Nový projekt** zvolte **Cloud > Aplikace Service Fabric**.

4. Název aplikace **Voting** a stiskněte klávesu **OK**.

   ![Dialogové okno Nový projekt ve Visual Studiu](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog.png)

5. Na **nové služby Fabric** vyberte **bezstavové ASP.NET Core**a název vaší služby **VotingWeb**.
   
   ![Výběr v dialogovém okně Nový služby webovou službu ASP.NET.](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog-2.png) 

6. Další stránka obsahuje sadu ASP.NET Core šablony projektů. V tomto kurzu zvolte **webové aplikace**. 
   
   ![Zvolte typ projektu ASP.NET](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog.png)

   Visual Studio vytvoří aplikaci a projekt služby a zobrazí je v Průzkumníku řešení.

   ![Průzkumník řešení po vytvoření aplikace ASP.NET základní webového rozhraní API služby]( ./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-angularjs-to-the-votingweb-service"></a>Přidat ke službě VotingWeb AngularJS
Přidat [AngularJS](http://angularjs.org/) pro vaši službu pomocí integrovaných [Bower podporu](/aspnet/core/client-side/bower). Otevřete *bower.json* a přidejte položky pro úhlová a úhlová bootstrap a pak změny uložte.

```json
{
  "name": "asp.net",
  "private": true,
  "dependencies": {
    "bootstrap": "3.3.7",
    "jquery": "2.2.0",
    "jquery-validation": "1.14.0",
    "jquery-validation-unobtrusive": "3.2.6",
    "angular": "v1.6.5",
    "angular-bootstrap": "v1.1.0"
  }
}
```
Při ukládání *bower.json* soubor, úhlová je nainstalován ve vašem projektu *wwwroot/lib* složky. Kromě toho je uveden v rámci *závislosti/Bower* složky.

### <a name="update-the-sitejs-file"></a>Aktualizovat soubor site.js
Otevřete *wwwroot/js/site.js* souboru.  Nahraďte jeho obsah JavaScript používané domovské zobrazení:

```javascript
var app = angular.module('VotingApp', ['ui.bootstrap']);
app.run(function () { });

app.controller('VotingAppController', ['$rootScope', '$scope', '$http', '$timeout', function ($rootScope, $scope, $http, $timeout) {

    $scope.refresh = function () {
        $http.get('api/Votes?c=' + new Date().getTime())
            .then(function (data, status) {
                $scope.votes = data;
            }, function (data, status) {
                $scope.votes = undefined;
            });
    };

    $scope.remove = function (item) {
        $http.delete('api/Votes/' + item)
            .then(function (data, status) {
                $scope.refresh();
            })
    };

    $scope.add = function (item) {
        var fd = new FormData();
        fd.append('item', item);
        $http.put('api/Votes/' + item, fd, {
            transformRequest: angular.identity,
            headers: { 'Content-Type': undefined }
        })
            .then(function (data, status) {
                $scope.refresh();
                $scope.item = undefined;
            })
    };
}]);
```

### <a name="update-the-indexcshtml-file"></a>Aktualizovat soubor Index.cshtml
Otevřete *Views/Home/Index.cshtml* souboru konkrétní domovskou řadiče zobrazení.  Nahraďte jeho obsah následujícím, pak uložte provedené změny.

```html
@{
    ViewData["Title"] = "Service Fabric Voting Sample";
}

<div ng-controller="VotingAppController" ng-init="refresh()">
    <div class="container-fluid">
        <div class="row">
            <div class="col-xs-8 col-xs-offset-2 text-center">
                <h2>Service Fabric Voting Sample</h2>
            </div>
        </div>

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <form class="col-xs-12 center-block">
                    <div class="col-xs-6 form-group">
                        <input id="txtAdd" type="text" class="form-control" placeholder="Add voting option" ng-model="item" />
                    </div>
                    <button id="btnAdd" class="btn btn-default" ng-click="add(item)">
                        <span class="glyphicon glyphicon-plus" aria-hidden="true"></span>
                        Add
                    </button>
                </form>
            </div>
        </div>

        <hr />

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <div class="row">
                    <div class="col-xs-4">
                        Click to vote
                    </div>
                </div>
                <div class="row top-buffer" ng-repeat="vote in votes.data">
                    <div class="col-xs-8">
                        <button class="btn btn-success text-left btn-block" ng-click="add(vote.key)">
                            <span class="pull-left">
                                {{vote.key}}
                            </span>
                            <span class="badge pull-right">
                                {{vote.value}} Votes
                            </span>
                        </button>
                    </div>
                    <div class="col-xs-4">
                        <button class="btn btn-danger pull-right btn-block" ng-click="remove(vote.key)">
                            <span class="glyphicon glyphicon-remove" aria-hidden="true"></span>
                            Remove
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
```

### <a name="update-the-layoutcshtml-file"></a>Aktualizovat soubor _Layout.cshtml
Otevřete *Views/Shared/_Layout.cshtml* souboru, výchozí rozložení pro aplikace ASP.NET.  Nahraďte jeho obsah následujícím, pak uložte provedené změny.

```html
<!DOCTYPE html>
<html ng-app="VotingApp" xmlns:ng="http://angularjs.org">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>@ViewData["Title"]</title>

    <link href="~/lib/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet" />
    <link href="~/css/site.css" rel="stylesheet" />

</head>
<body>
    <div class="container body-content">
        @RenderBody()
    </div>

    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/lib/angular/angular.js"></script>
    <script src="~/lib/angular-bootstrap/ui-bootstrap-tpls.js"></script>
    <script src="~/js/site.js"></script>

    @RenderSection("Scripts", required: false)
</body>
</html>
```

### <a name="update-the-votingwebcs-file"></a>Aktualizovat soubor VotingWeb.cs
Otevřete *VotingWeb.cs* souboru, který vytvoří webového hostitele jádro ASP.NET uvnitř bezstavové služby pomocí WebListener webový server.  Přidat `using System.Net.Http;` direktivy do horní části souboru.  Nahraďte `CreateServiceInstanceListeners()` fungovat s následující a potom uložte změny.

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
            {
                ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting WebListener on {url}");

                return new WebHostBuilder().UseWebListener()
                            .ConfigureServices(
                                services => services
                                    .AddSingleton<StatelessServiceContext>(serviceContext)
                                    .AddSingleton<HttpClient>())
                            .UseContentRoot(Directory.GetCurrentDirectory())
                            .UseStartup<Startup>()
                            .UseApplicationInsights()
                            .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                            .UseUrls(url)
                            .Build();
            }))
    };
}
```

### <a name="add-the-votescontrollercs-file"></a>Přidejte soubor VotesController.cs
Přidáte řadič, který definuje hlasujících akce. Klikněte pravým tlačítkem na **řadiče** složku, pak vyberte **Přidat -> Nový položky -> třída**.  Název souboru "VotesController.cs" a klikněte na tlačítko **přidat**.  Nahraďte obsah souboru následující příkaz, potom uložte změny.  Dále v [aktualizovat soubor VotesController.cs](#updatevotecontroller_anchor), tento soubor se změní pro čtení a zápis hlasujících data z back endové službě.  Prozatím se vrátí řadičem statický řetězec data k zobrazení.

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using Newtonsoft.Json;
using System.Text;
using System.Net.Http;
using System.Net.Http.Headers;

namespace VotingWeb.Controllers
{
    [Produces("application/json")]
    [Route("api/Votes")]
    public class VotesController : Controller
    {
        private readonly HttpClient httpClient;

        public VotesController(HttpClient httpClient)
        {
            this.httpClient = httpClient;
        }

        // GET: api/Votes
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            List<KeyValuePair<string, int>> votes= new List<KeyValuePair<string, int>>();
            votes.Add(new KeyValuePair<string, int>("Pizza", 3));
            votes.Add(new KeyValuePair<string, int>("Ice cream", 4));

            return Json(votes);
        }
     }
}
```



### <a name="deploy-and-run-the-application-locally"></a>Nasazení a spuštění aplikace místně
Teď můžete pokračovat a spusťte aplikaci. Stisknutím klávesy `F5` v sadě Visual Studio aplikaci nasaďte pro účely ladění. `F5`selže, pokud nebyla dříve otevřete Visual Studio jako **správce**.

> [!NOTE]
> Při prvním místním spuštění a nasazení aplikace sada Visual Studio vytvoří místní cluster pro účely ladění.  Vytvoření clusteru může trvat delší dobu. Stav vytváření clusteru se zobrazí v okně výstupu sady Visual Studio.

Webové aplikace v tomto okamžiku by měl vypadat přibližně takto:

![ASP.NET Core front-endu](./media/service-fabric-tutorial-create-dotnet-app/debug-front-end.png)

Aby se ukončilo ladění aplikace, přejděte zpět do sady Visual Studio a stiskněte klávesu **Shift + F5**.

## <a name="add-a-stateful-back-end-service-to-your-application"></a>Přidání stavová služba back endu do aplikace
Teď, když máme rozhraní ASP.NET Web API služby spuštěné v naší aplikaci, přejdeme dopředu a přidejte stavové spolehlivé služby k uložení některá data v naší aplikaci.

Service Fabric můžete konzistentně a spolehlivě ukládat vaše právo data uvnitř vaší služby pomocí spolehlivé kolekce. Spolehlivé kolekce jsou sadu třídy vysoce dostupné a spolehlivého kolekce, které jsou pro každý, kdo má používá kolekce C#.

V tomto kurzu vytvoříte službu, která ukládá hodnotu čítače v kolekci spolehlivé.

1. V Průzkumníku řešení klikněte pravým tlačítkem na **služby** v aplikaci projektu a zvolte **Přidat > Nový Service Fabric Service**.
   
    ![Přidání nové služby do existující aplikace](./media/service-fabric-tutorial-create-dotnet-app/vs-add-new-service.png)

2. V **nové služby Fabric** dialogovém okně, vyberte **Stateful ASP.NET Core**a název služby **VotingData** a stiskněte klávesu **OK**.

    ![Dialogové okno Nová služba ve Visual Studiu](./media/service-fabric-tutorial-create-dotnet-app/add-stateful-service.png)

    Po vytvoření svůj projekt služby, máte dvě služby ve vaší aplikaci. Při dalším sestavit aplikaci, můžete přidat další služby stejným způsobem. Každý může být nezávisle verzí a upgradovaný.

3. Další stránka obsahuje sadu ASP.NET Core šablony projektů. V tomto kurzu zvolte **webového rozhraní API**.

    ![Zvolte typ projektu ASP.NET](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog2.png)

    Visual Studio vytvoří projekt služby a zobrazí je v Průzkumníku řešení.

    ![Průzkumník řešení](./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-webapi-service.png)

### <a name="add-the-votedatacontrollercs-file"></a>Přidejte soubor VoteDataController.cs

V **VotingData** projektu klikněte pravým tlačítkem na **řadiče** složku, pak vyberte **Přidat -> Nový položky -> – třída**. Název souboru "VoteDataController.cs" a klikněte na tlačítko **přidat**. Nahraďte obsah souboru následující příkaz, potom uložte změny.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.ServiceFabric.Data;
using System.Threading;
using Microsoft.ServiceFabric.Data.Collections;

namespace VotingData.Controllers
{
    [Route("api/[controller]")]
    public class VoteDataController : Controller
    {
        private readonly IReliableStateManager stateManager;

        public VoteDataController(IReliableStateManager stateManager)
        {
            this.stateManager = stateManager;
        }

        // GET api/VoteData
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            var ct = new CancellationToken();

            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                var list = await votesDictionary.CreateEnumerableAsync(tx);

                var enumerator = list.GetAsyncEnumerator();

                var result = new List<KeyValuePair<string, int>>();

                while (await enumerator.MoveNextAsync(ct))
                {
                    result.Add(enumerator.Current);
                }

                return Json(result);
            }
        }

        // PUT api/VoteData/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                await votesDictionary.AddOrUpdateAsync(tx, name, 1, (key, oldvalue) => oldvalue + 1);
                await tx.CommitAsync();
            }

            return new OkResult();
        }

        // DELETE api/VoteData/name
        [HttpDelete("{name}")]
        public async Task<IActionResult> Delete(string name)
        {
            var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                if (await votesDictionary.ContainsKeyAsync(tx, name))
                {
                    await votesDictionary.TryRemoveAsync(tx, name);
                    await tx.CommitAsync();
                    return new OkResult();
                }
                else
                {
                    return new NotFoundResult();
                }
            }
        }
    }
}
```


## <a name="connect-the-services"></a>Připojení služby
V tomto kroku další jsme připojení dvě služby a zkontrolujte front-end webové aplikace, získání a nastavení, hlasování informace z back endové službě.

Service Fabric nabízí flexibilitu v tom, jak komunikovat se službami reliable services. V rámci jedné aplikace můžete mít služby, které jsou přístupné přes TCP. Dalším službám, které může být přístupné přes rozhraní HTTP REST API a stále dalších služeb může být přístupné přes webové sokety. Pro informace o dostupných možnostech a kompromisy související se situací, viz [komunikaci se službou](service-fabric-connect-and-communicate-with-services.md).

V tomto kurzu používáme [webového rozhraní API ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md).

<a id="updatevotecontroller" name="updatevotecontroller_anchor"></a>

### <a name="update-the-votescontrollercs-file"></a>Aktualizovat soubor VotesController.cs
V **VotingWeb** projekt, otevřete *Controllers/VotesController.cs* souboru.  Nahraďte `VotesController` třídy definice obsah následujícím kódem a pak změny uložte.

```csharp
    public class VotesController : Controller
    {
        private readonly HttpClient httpClient;
        string serviceProxyUrl = "http://localhost:19081/Voting/VotingData/api/VoteData";
        string partitionKind = "Int64Range";
        string partitionKey = "0";

        public VotesController(HttpClient httpClient)
        {
            this.httpClient = httpClient;
        }

        // GET: api/Votes
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            IEnumerable<KeyValuePair<string, int>> votes;

            HttpResponseMessage response = await this.httpClient.GetAsync($"{serviceProxyUrl}?PartitionKind={partitionKind}&PartitionKey={partitionKey}");

            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int)response.StatusCode);
            }

            votes = JsonConvert.DeserializeObject<List<KeyValuePair<string, int>>>(await response.Content.ReadAsStringAsync());

            return Json(votes);
        }

        // PUT: api/Votes/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            string payload = $"{{ 'name' : '{name}' }}";
            StringContent putContent = new StringContent(payload, Encoding.UTF8, "application/json");
            putContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");

            string proxyUrl = $"{serviceProxyUrl}/{name}?PartitionKind={partitionKind}&PartitionKey={partitionKey}";

            HttpResponseMessage response = await this.httpClient.PutAsync(proxyUrl, putContent);

            return new ContentResult()
            {
                StatusCode = (int)response.StatusCode,
                Content = await response.Content.ReadAsStringAsync()
            };
        }

        // DELETE: api/Votes/name
        [HttpDelete("{name}")]
        public async Task<IActionResult> Delete(string name)
        {
            HttpResponseMessage response = await this.httpClient.DeleteAsync($"{serviceProxyUrl}/{name}?PartitionKind={partitionKind}&PartitionKey={partitionKey}");

            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int)response.StatusCode);
            }

            return new OkResult();

        }
    }
```
<a id="walkthrough" name="walkthrough_anchor"></a>

## <a name="walk-through-the-voting-sample-application"></a>Provede hlasujících ukázkové aplikace
Hlasovací aplikaci se skládá ze dvou služeb:
- Webová služba front-endu (VotingWeb) – ASP.NET Core webových front-endové služby, která obsluhuje webové stránky a zpřístupňuje rozhraní API pro komunikaci s back-end službu.
- Služba back endu (VotingData)-ASP.NET Core webová služba, která zpřístupňuje rozhraní API můžete ukládat výsledky hlas ve slovníku spolehlivé trvalé na disku.

![Diagram aplikace](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

Když hlasovat v aplikaci dojít k následujícím událostem:
1. JavaScript odešle žádost hlas webovému rozhraní API ve front-endové webové službě jako požadavek HTTP PUT.

2. Webovou službu front-endu používá proxy server pro vyhledání a předávat požadavek HTTP PUT ve službě back-end.

3. Back endové službě přijímá příchozí požadavky a ukládá aktualizované výsledek v spolehlivé slovník, který získá replikují do několika uzly v clusteru a trvalé na disku. Všechny aplikační data se ukládají v clusteru, takže se žádná databáze.

## <a name="debug-in-visual-studio"></a>Ladění v sadě Visual Studio
Při ladění aplikace v sadě Visual Studio, kterou používáte místní cluster Service Fabric vývoj. Máte možnost upravit prostředí ladění pro váš scénář. V této aplikaci uložíme data v naší službě back-end pomocí slovník spolehlivé. Visual Studio odebere aplikaci za výchozí při zastavení ladicího programu. Odebrání aplikace způsobí, že data ve službě back-end taky odeberou. Chcete-li zachovat data mezi relace ladění, můžete změnit **režim ladění aplikací** jako vlastnost na **Voting** projektu v sadě Visual Studio.

Podívat se na co se stane, že v kódu, proveďte následující kroky:
1. Otevřete **VotesController.cs** souboru a nastavit zarážky ve webové rozhraní API **Put** – metoda (řádku 47) – můžete vyhledat soubor v Průzkumníku řešení v sadě Visual Studio.

2. Otevřete **VoteDataController.cs** souboru a nastavit zarážky v tomto rozhraní web API **Put** – metoda (řádku 50).

3. Přejděte zpět do prohlížeče a klikněte na hlasování možnost nebo přidat novou možnost hlasování. Kliknutí na první zarážky do kontroleru webového přední konci na rozhraní api.
    
    1. Toto je, kde JavaScript v prohlížeči odešle požadavek na kontroler API web ve službě front-endu.
    
    ![Přidejte Front-End služby hlas](./media/service-fabric-tutorial-create-dotnet-app/addvote-frontend.png)

    2. Nejprve jsme vytvořit adresu URL ReverseProxy pro naši službu back-end **(1)**.
    3. Potom jsme poslat PUT požadavek HTTP ReverseProxy **(2)**.
    4. Nakonec vrátíme odpověď z back-end službu do klienta **(3)**.

4. Stiskněte klávesu **F5** pokračovat
    1. Nyní jste na zarážce ve službě back-end.
    
    ![Přidat hlas Back-End službu](./media/service-fabric-tutorial-create-dotnet-app/addvote-backend.png)

    2. Na prvním řádku v metodě **(1)** používáme `StateManager` nebo přidat spolehlivé slovník nazývaný `counts`.
    3. Všechny interakce s hodnotami ve slovníku spolehlivé vyžadují transakci, této konfigurace pomocí příkazu **(2)** vytvoří této transakce.
    4. V transakci, jsme potom aktualizujte hodnotu relevantní klíče pro možnost hlasování a provede operaci **(3)**. Po potvrzení metoda vrátí, data aktualizovat ve slovníku a replikovat do jiných uzlů v clusteru. Data jsou teď bezpečně uložená v clusteru, a může převzít back-end službu do dalších uzlů, i nadále s dostupná data.
5. Stiskněte klávesu **F5** pokračovat

Chcete-li ukončit relaci ladění, stiskněte **Shift + F5**.


## <a name="next-steps"></a>Další kroky
V této části kurzu jste se dozvěděli, jak:

> [!div class="checklist"]
> * Vytvoření služby webového rozhraní API ASP.NET Core jako stavové spolehlivé služby
> * Vytvoření služby webové aplikace ASP.NET Core jako bezstavové webové služby
> * Použít reverzní proxy server ke komunikaci s stavové služby

Přechodu na další kurz:
> [!div class="nextstepaction"]
> [Nasaďte aplikaci do Azure](service-fabric-tutorial-deploy-app-to-party-cluster.md)
