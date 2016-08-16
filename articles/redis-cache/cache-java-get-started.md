<properties
   pageTitle="Použití Azure Redis Cache s Javou | Microsoft Azure"
    description="Začínáme s Azure Redis Cache pomocí Javy"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor=""/>

<tags
    ms.service="cache"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="05/31/2016"
    ms.author="sdanie"/>

# Použití Azure Redis Cache s Javou

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis Cache vám umožňuje přístup do vyhrazené mezipaměti Redis spravované Microsoftem. Vaše mezipaměť je přístupná ze všech aplikací v rámci Microsoft Azure.

Toto téma ukazuje, jak začít s Azure Redis Cache pomocí Javy.

## Požadavky

[Jedis](https://github.com/xetorthio/jedis) – Java klient pro Redis

V tomto kurzu používáme Jedis, ale můžete použít jakéhokoli Java klienta uvedeného na stránce [http://redis.io/clients](http://redis.io/clients).

## Vytvoření mezipaměti Redis v Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## Načtení názvu hostitele a přístupových klíčů

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## Povolení koncového bodu bez SSL

Někteří klienti Redis nepodporují SSL a ve výchozím nastavení je [port bez SSL pro nové instance služby Azure Redis Cache zakázán](cache-configure.md#access-ports). V době psaní tohoto textu klient [Jedis](https://github.com/xetorthio/jedis) nepodporuje SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]




## Přidání dat do mezipaměti a jejich načtení

    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    /* Make sure you turn on non-SSL port in Azure Redis using the Configuration section in the Azure Portal */
    public class App
    {
      public static void main( String[] args )
      {
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## Další kroky

- [Povolte diagnostiku mezipaměti](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics), abyste mohli [monitorovat](https://msdn.microsoft.com/library/azure/dn763945.aspx) stav svojí mezipaměti.
- Přečtěte si oficiální [dokumentaci k Redis](http://redis.io/documentation).




<!--HONumber=Jun16_HO2-->


