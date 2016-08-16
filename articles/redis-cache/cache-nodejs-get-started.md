<properties
    pageTitle="Použití Azure Redis Cache s Node.js | Microsoft Azure"
    description="Začínáme s Azure Redis Cache pomocí Node.js a node_redis."
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="05/31/2016"
    ms.author="sdanie"/>

# Použití Azure Redis Cache s Node.js

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis Cache vám umožňuje přístup do zabezpečené, vyhrazené mezipaměti Redis spravované Microsoftem. Vaše mezipaměť je přístupná ze všech aplikací v rámci Microsoft Azure.

Toto téma ukazuje, jak začít s Azure Redis Cache pomocí Node.js. Další příklad použití Azure Redis Cache s Node.js najdete v tématu [Sestavení aplikace Chat v Node.js se Socket.IO na webu Azure](../app-service-web/web-sites-nodejs-chat-app-socketio.md).


## Požadavky

Nainstalujte [node_redis](https://github.com/mranney/node_redis):

    npm install redis

V tomto kurzu používáme [node_redis](https://github.com/mranney/node_redis), ale můžete použít jakéhokoli Node.js klienta uvedeného na stránce [http://redis.io/clients](http://redis.io/clients).

## Vytvoření mezipaměti Redis v Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## Načtení názvu hostitele a přístupových klíčů

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## Povolení koncového bodu bez SSL

Někteří klienti Redis nepodporují SSL a ve výchozím nastavení je [port bez SSL pro nové instance služby Azure Redis Cache zakázán](cache-configure.md#access-ports). V době psaní tohoto textu klient [node_redis](https://github.com/mranney/node_redis) nepodporuje SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## Přidání dat do mezipaměti a jejich načtení

      var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});
    
    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });
    
    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Výstup:

    OK
    value


## Další kroky

- [Povolte diagnostiku mezipaměti](cache-how-to-monitor.md#enable-cache-diagnostics), abyste mohli [monitorovat](cache-how-to-monitor.md) stav svojí mezipaměti.
- Přečtěte si oficiální [dokumentaci k Redis](http://redis.io/documentation).






<!--HONumber=Jun16_HO2-->


