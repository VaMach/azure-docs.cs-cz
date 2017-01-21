---
title: "Použití Azure Redis Cache s Javou | Dokumentace Microsoftu"
description: "Začínáme s Azure Redis Cache pomocí Javy"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 29275a5e-2e39-4ef2-804f-7ecc5161eab9
ms.service: cache
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 01/06/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: c42aebb3aaf5c32ebdc4f79e2ace2f127e4fb20d
ms.openlocfilehash: fe875fba2651b770d910d257282f5e9f41f8a043


---
# <a name="how-to-use-azure-redis-cache-with-java"></a>Použití Azure Redis Cache s Javou
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Azure Redis Cache vám umožňuje přístup do vyhrazené mezipaměti Redis spravované Microsoftem. Vaše mezipaměť je přístupná ze všech aplikací v rámci Microsoft Azure.

Toto téma ukazuje, jak začít s Azure Redis Cache pomocí Javy.

## <a name="prerequisites"></a>Požadavky
[Jedis](https://github.com/xetorthio/jedis) – Java klient pro Redis

V tomto kurzu používáme Jedis, ale můžete použít jakéhokoli Java klienta uvedeného na stránce [http://redis.io/clients](http://redis.io/clients).

## <a name="create-a-redis-cache-on-azure"></a>Vytvoření mezipaměti Redis v Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Načtení názvu hostitele a přístupových klíčů
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Bezpečné připojení k mezipaměti pomocí protokolu SSL
Nejnovější sestavení [jedis](https://github.com/xetorthio/jedis) poskytuje podporu pro připojení k Azure Redis Cache pomocí protokolu SSL. Následující příklad ukazuje, jak se připojit k mezipaměti Redis Azure pomocí protokolu SSL koncového bodu 6380. Nahraďte `<name>` názvem mezipaměti a `<key>` primárním nebo sekundárním klíčem popsaným v předchozí části [Načtení názvu hostitele a přístupových klíčů](#retrieve-the-host-name-and-access-keys).

    boolean useSsl = true;
    /* In this line, replace <name> with your cache name: */
    JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Přidání dat do mezipaměti a jejich načtení
    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    public class App
    {
      public static void main( String[] args )
      {
        boolean useSsl = true;
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379, useSsl);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Další kroky
* [Povolte diagnostiku mezipaměti](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics), abyste mohli [monitorovat](https://msdn.microsoft.com/library/azure/dn763945.aspx) stav svojí mezipaměti.
* Přečtěte si oficiální [dokumentaci k Redis](http://redis.io/documentation).



<!--HONumber=Dec16_HO3-->


