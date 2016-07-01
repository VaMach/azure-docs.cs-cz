<properties 
    pageTitle="Stav relace s mezipamětí Azure Redis ve službě Azure App Service" 
    description="Naučte se používat službu mezipaměti Azure k podpoře ukládání stavu relace ASP.NET do mezipaměti." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="Rick-Anderson" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="get-started-article" 
    ms.date="02/27/2016" 
    ms.author="riande"/>


# Stav relace s mezipamětí Azure Redis ve službě Azure App Service


Toto téma vysvětluje, jak používat službu Azure Redis Cache pro stav relace.

Pokud webová aplikace ASP.NET používá stav relace, bude nutné nakonfigurovat externího poskytovatele stavu relace (buď službu Redis Cache, nebo poskytovatele stavu relace serveru SQL Server). Pokud používáte stav relace a nepoužíváte externího poskytovatele, budete omezeni na jednu instanci webové aplikace. Službu Redis Cache lze povolit nejrychleji a nejsnáze.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

##<a id="createcache"></a>Vytvoření mezipaměti
Chcete-li vytvořit mezipaměť, postupujte podle [těchto pokynů](../cache-dotnet-how-to-use-azure-redis-cache.md#create-cache).

##<a id="configureproject"></a>Přidání balíčku NuGet RedisSessionStateProvider do webové aplikace
Nainstalujte balíček NuGet `RedisSessionStateProvider`.  Použijte následující příkaz k instalaci z konzoly správce balíčků (**Nástroje** > **Správce balíčků NuGet** > **Konzola správce balíčků**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`
  
Chcete-li instalovat z položky **Nástroje** > **Správce balíčků NuGet** > **Správa balíčků NuGet pro řešení**, vyhledejte položku `RedisSessionStateProvider`.

Další informace naleznete v tématu [Stránka NuGet RedisSessionStateProvider](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/ ) a [Konfigurace klienta mezipaměti](../cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).

##<a id="configurewebconfig"></a>Úprava souboru Web.Config
Kromě vytváření odkazů na sestavení pro mezipaměť přidává balíček NuGet položky zástupných procedur do souboru *web.config*. 

1. Otevřete soubor *web.config* a najděte element **sessionState**.

1. Zadejte hodnoty položek `host`, `accessKey`, `port` (port SSL by měl být 6380) a nastavte položku `SSL` na hodnotu `true`. Tyto hodnoty lze získat v okně instance mezipaměti v [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715). Další informace naleznete v tématu [Připojení k mezipaměti](../cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache). Upozorňujeme, že port bez SSL je u nových mezipamětí ve výchozím nastavení zakázán. Další informace o povolení portu bez SSL naleznete v části [Přístupové porty](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) v tématu [Konfigurace mezipaměti ve službě Azure Redis Cache](https://msdn.microsoft.com/library/azure/dn793612.aspx). Následující kód ukazuje změny souboru *web.config*, konkrétně změny položek *port*, *host*, accessKey* a *ssl*.

          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;


##<a id="usesessionobject"></a> Použití objektu Session v kódu
Posledním krokem je začít používat objekt Session v kódu ASP.NET. Objekty lze přidávat do stavu relace pomocí metody **Session.Add**. Tato metoda ukládá položky do mezipaměti stavu relace pomocí párů hodnot klíčů.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

Následující kód tuto hodnotu načítá ze stavu relace.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue; 

Můžete také použít službu Redis Cash k načtení objektů ve webové aplikaci do mezipaměti. Další informace naleznete v tématu [Filmová aplikace MVC se službou Azure Redis Cache během 15 minut](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).
Podrobnější informace týkající se používání stavu relace ASP.NET naleznete v tématu [Přehled stavu relace ASP.NET][].

>[AZURE.NOTE] Pokud chcete začít používat službu Azure App Service před registrací k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service](http://go.microsoft.com/fwlink/?LinkId=523751), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci. Není vyžadována platební karta a nevzniká žádný závazek.

## Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

  *Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)*
  
  [nainstalován nejnovější]: http://www.windowsazure.com/downloads/?sdk=net  
  [Přehled stavu relace ASP.NET]: http://msdn.microsoft.com/library/ms178581.aspx

  [NováIkona]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
  [DialogNováMezipaměť]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
  [IkonaMezipaměť]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
  [DialogNuGet]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
  [KonfigVýstup]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
  [KonfigMezipaměť]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
  [URLKoncovýBod]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
  [SprávaKlíčů]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png
 



<!--HONumber=Jun16_HO2-->


