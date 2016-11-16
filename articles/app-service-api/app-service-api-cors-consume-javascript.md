---
title: Podpora CORS v App Service | Dokumentace Microsoftu
description: "Zjistěte, jak používat podporu CORS v Azure App Service."
services: app-service\api
documentationcenter: .net
author: tdykstra
manager: wpickett
editor: 
ms.assetid: 4f980a97-b9f5-4d1d-87ab-82b60bb96e1c
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/27/2016
ms.author: rachelap
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 14de66f6035d8cd0579351d64b85bd7e1c8f2ab2


---
# <a name="consume-an-api-app-from-javascript-using-cors"></a>Využití aplikace API z JavaScriptu pomocí CORS
App Service má integrovanou podporu [sdílení prostředků mezi zdroji (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), která umožňuje klientům JavaScript volat rozhraní API hostovaná v aplikacích API mezi doménami. App Service umožňuje nakonfigurovat v CORS přístup k rozhraní API, aniž by bylo potřeba psát v rozhraní API nějaký kód.

Tento článek obsahuje dvě části:

* Část [Postup konfigurace CORS](#corsconfig) popisuje obecně konfiguraci CORS pro libovolnou aplikaci API, webovou aplikaci nebo mobilní aplikaci. Platí stejně pro všechna rozhraní podporovaná službou App Service, včetně .NET, Node.js a Java. 
* Počínaje částí [Pokračování úvodních kurzů k .NET](#tutorialstart) obsahuje tento článek ukázku podpory CORS, která rozvíjí základy z [úvodního kurzu k aplikacím API](app-service-api-dotnet-get-started.md). 

## <a name="a-idcorsconfiga-how-to-configure-cors-in-azure-app-service"></a><a id="corsconfig"></a> Postup konfigurace CORS v Azure App Service
CORS můžete nakonfigurovat na Portálu Azure nebo pomocí nástrojů, které nabízí [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

#### <a name="configure-cors-in-the-azure-portal"></a>Konfigurace CORS na Portálu Azure
1. Přejděte v prohlížeči na [Portál Azure](https://portal.azure.com/).
2. Klikněte na **App Services** a pak klikněte na název aplikace API.
   
    ![Výběr aplikace API na portálu](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. V okně **Nastavení**, které se otevře napravo od okna **Aplikace API**, najděte část **API** a potom klikněte na **CORS**.
   
   ![Výběr CORS v okně Nastavení](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. Do textového pole zadejte adresu nebo adresy URL, které mají mít povoleno předávat volání JavaScriptu.

    Pokud například nasazujete aplikaci JavaScript do webové aplikace s názvem todolistangular, zadejte https://todolistangular.azurewebsites.net. Případně můžete zadat hvězdičku (*), pokud chcete povolit všechny zdrojové domény.


1. Klikněte na **Uložit**.
   
   ![Kliknutí na Uložit](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   Po kliknutí na **Uložit** bude aplikace API přijímat volání JavaScriptu ze zadaných adres URL.

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a>Konfigurace CORS pomocí nástrojů, které nabízí Azure Resource Manager
CORS můžete pro aplikaci API nakonfigurovat také pomocí [šablon Azure Resource Manageru](../resource-group-authoring-templates.md) v nástrojích příkazového řádku, jako je [Azure PowerShell](../powershell-install-configure.md) nebo [Azure CLI](../xplat-cli-install.md). 

Příklad šablony Azure Resource Manageru, která nastaví vlastnost CORS, najdete v [souboru azuredeploy.json v úložišti pro ukázkovou aplikaci tohoto kurzu](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Najděte část šablony, která je podobná následujícímu příkladu:

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <a name="a-idtutorialstarta-continuing-the-net-gettingstarted-tutorial"></a><a id="tutorialstart"></a> Pokračování úvodních kurzů k .NET
Pokud jste si vybrali sérii úvodních kurzů k aplikacím API pro Node.js nebo Java, pak jste tuto sérii dokončili. Přeskočte část [Další kroky](#next-steps) a najděte část s návrhy dalších kurzů k API Apps.

Zbývající část tohoto článku je pokračování série úvodních kurzů k .NET, která předpokládá, že jste úspěšně dokončili [první kurz](app-service-api-dotnet-get-started.md).

## <a name="deploy-the-todolistangular-project-to-a-new-web-app"></a>Nasazení projektu ToDoListAngular do nové webové aplikace
V [prvním kurzu](app-service-api-dotnet-get-started.md) jste vytvořili aplikaci API střední vrstvy a aplikaci API datové vrstvy. V tomto kurzu vytvoříte jednostránkovou webovou aplikaci (SPA), která bude volat aplikaci API střední vrstvy. Aby tato jednostránková aplikace fungovala, je potřeba, abyste pro aplikaci API střední vrstvy povolili CORS. 

V [ukázkové aplikaci ToDoList](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) je projekt ToDoListAngular jednoduchý klient AngularJS, který volá projekt webového rozhraní API ToDoListAPI střední vrstvy. Kód jazyka JavaScript v souboru *app/scripts/todoListSvc.js* zavolá rozhraní API pomocí zprostředkovatele protokolu HTTP AngularJS. 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 

            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-the-todolistangular-project"></a>Vytvoření nové webové aplikace projektu ToDoListAngular
Postup vytvoření nové webové aplikace App Service a nasazení projektu do ní je podobný postupu [vytvoření a nasazení aplikace API, který už znáte z prvního kurzu této série](app-service-api-dotnet-get-started.md#createapiapp). Jediný rozdíl je v tom, že se nejedná o **aplikaci API**, ale o **webovou aplikaci**.  Snímky obrazovky dialogových oken viz: 

1. V **Průzkumníku řešení** klikněte pravým tlačítkem na projekt ToDoListAngular a potom klikněte na **Publikovat**.
2. Na kartě **Profil** průvodce **Publikovat web** klikněte na **Microsoft Azure App Service**.
3. V dialogovém okně **App Service** klikněte na **Nová**.
4. Na kartě **Hosting** v dialogovém okně **Vytvořit službu App Service** zadejte **název webové aplikace**, který bude v doméně *azurewebsites.net* jedinečný. 
5. Zvolte **předplatné** Azure, s kterým chcete pracovat.
6. V rozevíracím seznamu **Skupina prostředků** zvolte stejnou skupinu prostředků, kterou jste vytvořili dříve.
7. V rozevíracím seznamu **Plán služby App Service** vyberte stejný plán, který jste vytvořili dříve. 
8. Klikněte na možnost **Vytvořit**.
   
    Visual Studio vytvoří webovou aplikaci, vytvoří pro ni profil publikování a zobrazí krok **Připojení** průvodce **Publikovat web**.
   
    Na tlačítko **Publikovat** ještě neklikejte. V následující části nakonfigurujete novou webovou aplikaci, která bude volat aplikaci API střední vrstvy běžící ve službě App Service. 

### <a name="set-the-middle-tier-url-in-web-app-settings"></a>Nastavení adresy URL střední vrstvy v nastavení webové aplikace
1. Přejděte na [Portál Azure](https://portal.azure.com/) a otevřete okno **Webová aplikace** pro aplikaci, kterou jste vytvořili k hostování projektu TodoListAngular (front-endu).
2. Klikněte na **Nastavení > Nastavení aplikace**.
3. V části **Nastavení aplikace** přidejte následující klíč a hodnotu:
   
   | Klíč | Hodnota | Příklad |
   | --- | --- | --- |
   | toDoListAPIURL |https://{název vaší aplikace API střední vrstvy}.azurewebsites.net |https://todolistapi0121.azurewebsites.net |
4. Klikněte na **Uložit**.
   
    Při spuštění kódu v Azure tato hodnota přepíše adresu URL místního hostitele, která je v souboru *Web.config*. 
   
    Kód, který získá hodnotu nastavení, je uložen v souboru *index.cshtml*:
   
        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>
   
    Kód v souboru *todoListSvc.js* používá nastavení:
   
        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-the-todolistangular-web-project-to-the-new-web-app"></a>Nasazení webového projektu ToDoListAngular do nové webové aplikace
* V nástroji Visual Studio klikněte v kroku **Připojení** průvodce **Publikovat web** na **Publikovat**.
  
   Visual Studio nasadí projekt ToDoListAngular do nové webové aplikace a otevře v prohlížeči adresu URL této webové aplikace. 

### <a name="test-the-application-without-cors-enabled"></a>Testování aplikace bez povolení CORS
1. Ve vývojářských nástrojích prohlížeče otevřete okno konzoly.
2. V okně prohlížeče, které zobrazuje uživatelské rozhraní AngularJS, klikněte na odkaz **Seznam úkolů**.
   
    Kód jazyka JavaScript se pokusí zavolat aplikaci API střední vrstvy, ale volání selže, protože front-end běží v jiné doméně než back-end. Okno konzoly vývojářských nástrojů prohlížeče zobrazuje chybovou zprávu nepůvodního zdroje.
   
    ![Chybová zpráva nepůvodního zdroje](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-the-middle-tier-api-app"></a>Konfigurace CORS pro aplikaci API střední vrstvy
V této části nakonfigurujete nastavení CORS v Azure pro aplikaci API ToDoListAPI střední vrstvy. Toto nastavení umožní aplikaci API střední vrstvy přijímat volání JavaScriptu z webové aplikace, kterou jste vytvořili pro projekt ToDoListAngular.

1. Přejděte v prohlížeči na [Portál Azure](https://portal.azure.com/).
2. Klikněte na **App Services** a potom na aplikaci API ToDoListAPI (střední úroveň).
   
    ![Výběr aplikace API na portálu](./media/app-service-api-cors-consume-javascript/browseapiapps.png)
3. V okně **Nastavení**, které se otevře napravo od okna **Aplikace API**, najděte část **API** a potom klikněte na **CORS**.
   
   ![Výběr CORS na portálu](./media/app-service-api-cors-consume-javascript/clicksettings.png)
4. Do textového pole zadejte adresu URL webové aplikace ToDoListAngular (front-endu). Pokud jste například nasadili projekt ToDoListAngular do webové aplikace s názvem todolistangular0121, povolte volání z adresy URL `https://todolistangular0121.azurewebsites.net`.
   
   Případně můžete zadat hvězdičku (*), pokud chcete povolit všechny zdrojové domény.
5. Klikněte na **Uložit**.
   
   ![Kliknutí na Uložit](./media/app-service-api-cors-consume-javascript/corsinportal.png)
   
   Po kliknutí na **Uložit** bude aplikace API přijímat volání JavaScriptu ze zadané adresy URL. Na tomto snímku obrazovky bude aplikace API ToDoListAPI0223 přijímat volání klienta JavaScript z webové aplikace ToDoListAngular.

### <a name="test-the-application-with-cors-enabled"></a>Testování aplikace s povolením CORS
* Otevřete v prohlížeči adresu URL HTTPS webové aplikace. 
  
    V tomto případě aplikace umožňuje zobrazovat, přidávat, upravovat a odstraňovat položky seznamu úkolů. 
  
    ![Stránka seznamu úkolů ukázkové aplikace](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a>CORS služby App Service versus CORS webového rozhraní API
V projektu webového rozhraní API můžete nainstalovat balíček NuGet [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/), a určit tak pomocí kódu, ze kterých domén bude rozhraní API přijímat volání JavaScriptu.

Podpora CORS webového rozhraní API je flexibilnější než podpora CORS služby App Service. Můžete například určit různé povolené zdroje pro různé metody akcí, zatímco u CORS služby App Service zadáváte jednu sadu povolených zdrojů pro všechny metody aplikace API.

> [!NOTE]
> Nedoporučujeme používat v jedné aplikaci API jak CORS webového rozhraní API, tak CORS služby App Service. CORS služby App Service bude mít přednost a CORS webového rozhraní API nebude mít žádný vliv. Pokud například v App Service povolíte jednu zdrojovou doménu a v kódu webového rozhraní API povolíte všechny zdrojové domény, vaše aplikace API Azure bude přijímat jen volání z domény, kterou jste zadali v Azure.
> 
> 

### <a name="how-to-enable-cors-in-web-api-code"></a>Postup povolení CORS v kódu webového rozhraní API
Následující kroky shrnují proces povolení podpory CORS webového rozhraní API. Další informace najdete v tématu [Povolení žádostí napříč zdroji ve webovém rozhraní API 2 ASP.NET](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

1. V projektu webového rozhraní API nainstalujte balíček NuGet [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/).
2. Do metody **Register** třídy **WebApiConfig** doplňte řádek kódu `config.EnableCors()`, jak to ukazuje následující příklad. 
   
        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
   
                // The following line enables you to control CORS by using Web API code
                config.EnableCors();
   
                // Web API routes
                config.MapHttpAttributeRoutes();
   
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }
3. Do kontroleru webového rozhraní API přidejte příkaz `using` pro obor názvů `System.Web.Http.Cors` a do třídy kontroleru nebo do jednotlivých metod akcí přidejte atribut `EnableCors`. V následujícím příkladu platí podpora CORS pro celý kontroler.
   
        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController

## <a name="using-azure-api-management-with-api-apps"></a>Použití služby API Management Azure pro aplikace API
Pokud používáte API Management s aplikací API, nakonfigurujte CORS ve službě API Management, ne v aplikaci API. Další informace najdete v následujících materiálech:

* [Přehled služby Azure API Management (video: CORS začíná ve 12:10)](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [Mezidoménové zásady služby API Management](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)

## <a name="troubleshooting"></a>Řešení potíží
V případě, že při procházení tohoto kurzu narazíte na problém, můžete ho zkusit vyřešit pomocí následujících kroků.

* Zkontrolujte, že používáte nejnovější verzi [sady Azure SDK pro .NET pro Visual Studio 2015](http://go.microsoft.com/fwlink/?linkid=518003).
* Zkontrolujte, že jste do nastavení CORS zadali `https` a že protokol `https` používáte ke spouštění front-endové webové aplikace.
* Ověřte, že jste nastavení CORS zadali do aplikace API střední vrstvy, ne do front-endové webové aplikace.
* Pokud konfigurujete CORS v kódu aplikace i v Azure App Service, přepíše nastavení CORS v App Service vaše změny v kódu aplikace. 

Další informace o funkcích nástroje Visual Studio, které usnadňují řešení potíží, najdete v tématu [Řešení potíží s aplikacemi Azure App Service v nástroji Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Další kroky
V tomto článku jste se seznámili s postupem, jak povolit podporu CORS v App Service, aby mohl kód JavaScript klienta volat rozhraní API v jiné doméně. Pokud se chcete dozvědět další informace o aplikacích API, přečtěte si [úvod k ověřování v App Service](../app-service/app-service-authentication-overview.md) a pak přejděte na kurz [ověřování uživatele pro aplikace API](app-service-api-dotnet-user-principal-auth.md).




<!--HONumber=Nov16_HO2-->


