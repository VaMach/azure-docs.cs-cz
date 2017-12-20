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
ms.date: 08/31/2017
ms.author: sdanie
ms.openlocfilehash: b433eecb0424db85b616c40c5f0cdfc88692cef1
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
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
    JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */

> [!NOTE]
> Port bez SSL je pro nové instance Azure Redis Cache zakázaný. Pokud používáte jiného klienta, který nepodporuje SSL, přečtěte si téma věnované tomu, jak [povolit port bez SSL](cache-configure.md#access-ports).
> 
> 

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
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Další kroky
* [Povolte diagnostiku mezipaměti](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics), abyste mohli [monitorovat](https://msdn.microsoft.com/library/azure/dn763945.aspx) stav svojí mezipaměti.
* Přečtěte si oficiální [dokumentaci k Redis](http://redis.io/documentation).
* Zjistěte, [jak nakonfigurovat aplikaci Spring Initializr pro používání služby Redis Cache](cache-java-spring-boot-initializer-with-redis-cache.md).