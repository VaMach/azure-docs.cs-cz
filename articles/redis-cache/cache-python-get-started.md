---
title: Použití Azure Redis Cache s Pythonem | Microsoft Docs
description: Začínáme s Azure Redis Cache pomocí Pythonu
services: redis-cache
documentationcenter: ''
author: steved0x
manager: douge
editor: v-lincan

ms.service: cache
ms.devlang: python
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 08/16/2016
ms.author: sdanie

---
# Použití Azure Redis Cache s Pythonem
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Toto téma ukazuje, jak začít s Azure Redis Cache pomocí Pythonu.

## Požadavky
Nainstalujte [redis-py](https://github.com/andymccurdy/redis-py).

## Vytvoření mezipaměti Redis v Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## Načtení názvu hostitele a přístupových klíčů
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## Povolení koncového bodu bez SSL
Někteří klienti Redis nepodporují SSL a ve výchozím nastavení je [port bez SSL pro nové instance služby Azure Redis Cache zakázán](cache-configure.md#access-ports). V době psaní tohoto textu klient [redis-py](https://github.com/andymccurdy/redis-py) nepodporuje SSL. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]

## Přidání dat do mezipaměti a jejich načtení
    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Nahraďte `<name>` názvem vaší mezipaměti a `key` vaším přístupovým klíčem.

<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png



<!---HONumber=Aug16_HO4-->


