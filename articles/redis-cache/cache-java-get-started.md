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
ms.date: 08/24/2016
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 907f75dc02bff7e25712a564410c1974e22f0d99


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

## <a name="enable-the-nonssl-endpoint"></a>Povolení koncového bodu bez SSL
Někteří klienti Redis nepodporují SSL a ve výchozím nastavení je [port bez SSL pro nové instance služby Azure Redis Cache zakázán](cache-configure.md#access-ports). V době psaní tohoto textu klient [Jedis](https://github.com/xetorthio/jedis) nepodporuje SSL. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]

## <a name="add-something-to-the-cache-and-retrieve-it"></a>Přidání dat do mezipaměti a jejich načtení
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


## <a name="next-steps"></a>Další kroky
* [Povolte diagnostiku mezipaměti](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics), abyste mohli [monitorovat](https://msdn.microsoft.com/library/azure/dn763945.aspx) stav svojí mezipaměti.
* Přečtěte si oficiální [dokumentaci k Redis](http://redis.io/documentation).




<!--HONumber=Nov16_HO2-->


