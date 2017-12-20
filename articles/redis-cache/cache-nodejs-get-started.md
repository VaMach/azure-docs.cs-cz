---
title: "Použití Azure Redis Cache s Node.js | Dokumentace Microsoftu"
description: "Začínáme s Azure Redis Cache pomocí Node.js a node_redis."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: v-lincan
ms.assetid: 06fddc95-8029-4a8d-83f5-ebd5016891d9
ms.service: cache
ms.devlang: nodejs
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/10/2017
ms.author: sdanie
ms.openlocfilehash: f2c448af24e180db58f3ef3d39e90036dda3f7eb
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Použití Azure Redis Cache s Node.js
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Azure Redis Cache vám umožňuje přístup do zabezpečené, vyhrazené mezipaměti Redis spravované Microsoftem. Vaše mezipaměť je přístupná ze všech aplikací v rámci Microsoft Azure.

Toto téma ukazuje, jak začít s Azure Redis Cache pomocí Node.js. 

## <a name="prerequisites"></a>Požadavky
Nainstalujte [node_redis](https://github.com/mranney/node_redis):

    npm install redis

Tento kurz používá [node_redis](https://github.com/mranney/node_redis). Příklady použití dalších klientů Node.js najdete v individuální dokumentaci pro klienty Node.js uvedené v [klientech Node.js Redis](http://redis.io/clients#nodejs).

## <a name="create-a-redis-cache-on-azure"></a>Vytvoření mezipaměti Redis v Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Načtení názvu hostitele a přístupových klíčů
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Bezpečné připojení k mezipaměti pomocí protokolu SSL
Nejnovější sestavení [node_redis](https://github.com/mranney/node_redis) poskytuje podporu pro připojení k mezipaměti Azure Redis pomocí protokolu SSL. Následující příklad ukazuje, jak se připojit k mezipaměti Redis Azure pomocí protokolu SSL koncového bodu 6380. Nahraďte `<name>` názvem mezipaměti a `<key>` primárním nebo sekundárním klíčem popsaným v předchozí části [Načtení názvu hostitele a přístupových klíčů](#retrieve-the-host-name-and-access-keys).

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

> [!NOTE]
> Port bez SSL je pro nové instance Azure Redis Cache zakázaný. Pokud používáte jiného klienta, který nepodporuje SSL, přečtěte si téma věnované tomu, jak [povolit port bez SSL](cache-configure.md#access-ports).
> 
> 

## <a name="add-something-to-the-cache-and-retrieve-it"></a>Přidání dat do mezipaměti a jejich načtení
Následující příklad ukazuje, jak se připojit k instanci mezipaměti Redis Azure a uložit a načíst položku z mezipaměti. Pro další příklady použití Redis pomocí klienta [node_redis](https://github.com/mranney/node_redis) si zobrazte [http://redis.js.org/](http://redis.js.org/).

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


## <a name="next-steps"></a>Další kroky
* [Povolte diagnostiku mezipaměti](cache-how-to-monitor.md#enable-cache-diagnostics), abyste mohli [monitorovat](cache-how-to-monitor.md) stav svojí mezipaměti.
* Přečtěte si oficiální [dokumentaci k Redis](http://redis.io/documentation).

