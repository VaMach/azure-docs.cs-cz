---
title: "Použití Azure Redis Cache | Dokumentace Microsoftu"
description: "Zjistěte, jak zlepšit výkon aplikací Azure pomocí Azure Redis Cache."
services: redis-cache,app-service
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: c502f74c-44de-4087-8303-1b1f43da12d5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/27/2017
ms.author: sdanie
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 3dfc026490093523446650c510dbebdd660e8b6b
ms.contentlocale: cs-cz
ms.lasthandoff: 07/28/2017

---
# <a name="how-to-use-azure-redis-cache"></a>Použití Azure Redis Cache
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Tento průvodce vám ukáže, jak začít používat **Azure Redis Cache**. Microsoft Azure Redis Cache je založená na populární Open Source mezipaměti Redis Cache. Umožňuje vám přístup do zabezpečené, vyhrazené mezipaměti Redis spravované Microsoftem. Mezipaměť vytvořená pomocí Azure Redis Cache je přístupná ze všech aplikací v rámci Microsoft Azure.

Microsoft Azure Redis Cache je dostupná na následujících úrovních:

* **Basic** – jeden uzel. Více velikostí až do 53 GB.
* **Standard** – dva uzly Primární/Replika. Více velikostí až do 53 GB. 99,9% SLA.
* **Premium** – dva uzly Primární/Replika s až 10 horizontálními oddíly. Více velikostí od 6 GB do 530 GB. Všechny funkce úrovně Standard a navíc podpora [clusteru Redis](cache-how-to-premium-clustering.md), [trvalosti Redis](cache-how-to-premium-persistence.md) a [služby Azure Virtual Network](cache-how-to-premium-vnet.md). 99,9% SLA.

Každá úroveň se liší z hlediska funkcí a cen. Informace o cenách najdete na stránce [Podrobnosti o cenách Azure Redis Cache][Cache Pricing Details].

Tento průvodce vám ukáže, jak použít klienta [StackExchange.Redis][StackExchange.Redis] pomocí kódu v C\#. Pokryté scénáře zahrnují **vytvoření a konfiguraci mezipaměti**, **konfiguraci klientů mezipaměti** a **přidávání a odebírání objektů z mezipaměti**. Další informace o používání Azure Redis Cache najdete v části [Další kroky][Next Steps]. Podrobný kurz vytvoření webové aplikace s Redis Cache pomocí ASP.NET MVC najdete v tématu [Vytvoření webové aplikace s Redis Cache](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>

## <a name="get-started-with-azure-redis-cache"></a>Začínáme s Azure Redis Cache
Začít s Azure Redis Cache je jednoduché. Abyste mohli začít, zřídíte a nakonfigurujete mezipaměť. Dále nakonfigurujete klienty mezipaměti pro přístup do mezipaměti. Po nakonfigurování klientů mezipaměti s nimi můžete začít pracovat.

* [Vytvoření mezipaměti][Create the cache]
* [Konfigurace klientů mezipaměti][Configure the cache clients]

<a name="create-cache"></a>

## <a name="create-a-cache"></a>Vytvoření mezipaměti
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="to-access-your-cache-after-its-created"></a>Přístup do vaší mezipaměti po jejím vytvoření
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Další informace o konfiguraci mezipaměti najdete v tématu [Konfigurace Azure Redis Cache](cache-configure.md).

<a name="NuGet"></a>

## <a name="configure-the-cache-clients"></a>Konfigurace klientů mezipaměti
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

Po konfiguraci klientského projektu pro používání mezipaměti můžete pro práci s mezipamětí použít techniky popsané v následujících sekcích.

<a name="working-with-caches"></a>

## <a name="working-with-caches"></a>Práce s mezipamětí
Kroky v tomto oddílu popisují, jak provádět běžné úkoly s mezipamětí.

* [Připojení k mezipaměti][Connect to the cache]
* [Přidání objektů do mezipaměti a jejich načtení][Add and retrieve objects from the cache]
* [Práce s objekty .NET v mezipaměti](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>

## <a name="connect-to-the-cache"></a>Připojení k mezipaměti
Chcete-li pracovat s mezipamětí prostřednictvím kódu programu, potřebujete odkaz na mezipaměť. Přidejte následující řádek na začátek souboru, ve kterém chcete použít klienta StackExchange.Redis pro přístup do Azure Redis Cache.

    using StackExchange.Redis;

> [!NOTE]
> Klient StackExchange.Redis vyžaduje rozhraní .NET Framework 4 nebo vyšší.
> 
> 

Připojení k Azure Redis Cache spravuje třída `ConnectionMultiplexer`. Tato třída by se měla sdílet a opětovně používat v rámci klientské aplikace, a není nutné vytvářet ji pro každou operaci zvlášť. 

Chcete-li se připojit k Azure Redis Cache a vrátit instanci připojeného `ConnectionMultiplexer`, zavolejte statickou metodu `Connect` a předejte jí koncový bod mezipaměti a klíč. Jako parametr hesla použijte klíč vygenerovaný na webu Azure Portal.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

> [!IMPORTANT]
> Upozornění: Neuchovávejte přihlašovací údaje ve zdrojovém kódu. Pro zjednodušení této ukázku je ve zdrojovém kódu uvádíme. Informace o tom, jak ukládat přihlašovací údaje, najdete v tématu [Fungování řetězců aplikace a připojovacích řetězců][How Application Strings and Connection Strings Work].
> 
> 

Nechcete-li používat protokol SSL, nastavte hodnotu `ssl=false` nebo vynechejte parametr `ssl`.

> [!NOTE]
> Port bez SSL je ve výchozím nastavení pro nové mezipaměti zakázán. Pokyny pro povolení portu bez SSL najdete v tématu [Přístupové porty](cache-configure.md#access-ports).
> 
> 

Jeden ze způsobů sdílení instance `ConnectionMultiplexer` v aplikaci je pomocí statické vlastnosti, která vrací připojenou instanci, podobně jako v následujícím příkladu.  Tento přístup poskytuje způsob inicializace jedné připojené instance `ConnectionMultiplexer`, který je bezpečný pro přístup z více vláken. V těchto příkladech je hodnota `abortConnect` nastavená na false, to znamená, že volání je úspěšné i v případě, že nedojde k vytvoření připojení k Azure Redis Cache. Klíčovou vlastností `ConnectionMultiplexer` je automatické obnovení připojení k mezipaměti po vyřešení problémů se sítí nebo jiných příčin.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

Další informace o rozšířených možnostech konfigurace připojení najdete v tématu [Konfigurační model StackExchange.Redis][StackExchange.Redis configuration model].

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Po vytvoření připojení vrátíte odkaz na databázi mezipaměti Redis zavoláním metody `ConnectionMultiplexer.GetDatabase`. Objekt vrácený metodou `GetDatabase` je prostý průchozí objekt a není nutné jej ukládat.

    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Mezipaměti Azure Redis Cache mají konfigurovatelný počet databází (výchozí je 16), který lze použít k logickému oddělení dat v rámci mezipaměti Redis. Další informace najdete v tématu [Co jsou databáze Redis?](cache-faq.md#what-are-redis-databases) a [Výchozí konfigurace serveru Redis](cache-configure.md#default-redis-server-configuration).

Nyní, když už víte, jak se připojit k instanci služby Azure Redis Cache a vrátit odkaz na databázi mezipaměti, se podíváme na práci s mezipamětí.

<a name="add-object"></a>

## <a name="add-and-retrieve-objects-from-the-cache"></a>Přidání objektů do mezipaměti a jejich načtení
Položky lze ukládat a načítat z mezipaměti pomocí metod `StringSet``StringGet`

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Redis ukládá většinu dat jako řetězce Redis, ale tyto řetězce mohou obsahovat mnoho typů dat, včetně serializovaných binárních dat, která lze použít při ukládání objektů .NET v mezipaměti.

Při volání metody `StringGet` se vrátí objekt, pokud existuje, a hodnota `null` Pokud je vrácena hodnota `null`, můžete načíst hodnotu z požadovaného zdroje dat a uložit ji do mezipaměti pro pozdější použití. Tento způsob použití se označuje jako princip s doplňováním mezipaměti.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

Můžete použít také typ `RedisValue`, jak ukazuje následující příklad. Typ `RedisValue` má implicitní operátory pro práci s celočíselnými datovými typy a může být užitečný v případě, že očekávanou hodnotou pro položku v mezipaměti je `null`.


    RedisValue value = cache.StringGet("key1");
    if (!value.HasValue)
    {
        value = GetValueFromDataSource();
        cache.StringSet("key1", value);
    }


Chcete-li zadat vypršení platnosti položky v mezipaměti, použijte parametr `TimeSpan` metody `StringSet`.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-the-cache"></a>Práce s objekty .NET v mezipaměti
Azure Redis Cache může do mezipaměti ukládat objekty .NET i primitivní datové typy. Objekty .NET je však nutné před uložením do mezipaměti serializovat. Serializaci objektů .NET má na starosti vývojář aplikace, kterému je tak poskytnuta flexibilita při výběru serializátoru.

Jeden způsob, jak serializovat objekty, je použít metody serializace `JsonConvert` v balíčku [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) a serializovat a deserializovat tak objekty do a z formátu JSON. Následující příklad ukazuje získání a nastavení pomocí instance objektu `Employee`

    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }

        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>

## <a name="next-steps"></a>Další kroky
Nyní, když jste se naučili základy, pokračujte následujícími odkazy a zjistěte více o Azure Redis Cache.

* Prohlédněte si poskytovatele ASP.NET pro Azure Redis Cache.
  * [Zprostředkovatel stavu relace Azure Redis](cache-aspnet-session-state-provider.md)
  * [Poskytovatel výstupní mezipaměti ASP.NET služby Azure Redis Cache](cache-aspnet-output-cache-provider.md)
* [Povolte diagnostiku mezipaměti](cache-how-to-monitor.md#enable-cache-diagnostics), abyste mohli [monitorovat](cache-how-to-monitor.md) stav svojí mezipaměti. Metriky lze zobrazit na webu Azure Portal a můžete je také [stáhnout a revidovat](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) pomocí nástrojů dle vašeho výběru.
* Prohlédněte si [dokumentaci ke klientu mezipaměti StackExchange.Redis][StackExchange.Redis cache client documentation].
  * K Azure Redis Cache lze přistupovat z mnoha klientů Redis a programovacích jazyků. Další informace najdete na stránce [http://redis.io/clients][http://redis.io/clients].
* Azure Redis Cache lze rovněž použít se službami a nástroji třetích stran, jako jsou Redsmin a Redis Desktop Manager.
  * Další informace o nástroji Redsmin najdete v tématu [Načtení připojovacího řetězce Azure Redis a jeho použití s Redsmin][How to retrieve an Azure Redis connection string and use it with Redsmin].
  * [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager) umožňuje získat přístup k datům v Azure Redis Cache a zkoumat je pomocí grafického uživatelského rozhraní.
* Prohlédněte si dokumentaci k [Redis][redis] a přečtěte si o [datových typech Redis][redis data types] a [15minutový úvod do datových typů Redis][a fifteen minute introduction to Redis data types].

<!-- INTRA-TOPIC LINKS -->
[Next Steps]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Create the cache]: #create-cache
[Configure the cache]: #enable-caching
[Configure the cache clients]: #NuGet
[Working with Caches]: #working-with-caches
[Connect to the cache]: #connect-to-cache
[Add and retrieve objects from the cache]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session


<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png







<!-- LINKS -->
[http://redis.io/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[How to retrieve an Azure Redis connection string and use it with Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[StackExchange.Redis configuration model]: https://stackexchange.github.io/StackExchange.Redis/Configuration

[Work with .NET objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Cache Pricing Details]: http://www.windowsazure.com/pricing/details/cache/
[Azure portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis cache client documentation]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis data types]: http://redis.io/topics/data-types
[a fifteen minute introduction to Redis data types]: http://redis.io/topics/data-types-intro

[How Application Strings and Connection Strings Work]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/



