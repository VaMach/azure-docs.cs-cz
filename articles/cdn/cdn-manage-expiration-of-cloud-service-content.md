---
title: "Spravovat vypršení platnosti webového obsahu v síti pro doručování obsahu Azure | Microsoft Docs"
description: "Zjistěte, jak spravovat platnost obsahu Azure webové aplikace nebo cloudové služby, ASP.NET nebo služby IIS v Azure CDN."
services: cdn
documentationcenter: .NET
author: zhangmanling
manager: erikre
editor: 
ms.assetid: bef53fcc-bb13-4002-9324-9edee9da8288
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/10/2017
ms.author: mazha
ms.openlocfilehash: d58a245923242b3963b188ca869e8290d999c0a2
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/13/2017
---
# <a name="manage-expiration-of-web-content-in-azure-content-delivery-network"></a>Spravovat vypršení platnosti webového obsahu v síti pro doručování obsahu Azure
 v Azure CDN
> [!div class="op_single_selector"]
> * [Službě Azure Web Apps nebo cloudové služby, ASP.NET nebo služby IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Storage](cdn-manage-expiration-of-blob-content.md)
> 

Soubory z jakékoli veřejně přístupné počátek webový server může do mezipaměti v Azure Content Delivery Network (CDN), dokud uplynutí jejich time to live (TTL). Hodnota TTL je dáno [ `Cache-Control` záhlaví](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) v odpovědi HTTP ze zdrojového serveru. Tento článek popisuje, jak nastavit `Cache-Control` hlavičky pro funkci webové aplikace Microsoft Azure App Service, Azure Cloud Services, aplikace ASP.NET a Internetová informační služba lokalit, které jsou nakonfigurované podobně.

> [!TIP]
> Můžete nastavit žádné TTL na soubor. V takovém případě Azure CDN automaticky použije výchozí hodnotu TTL sedm dní.
> 
> Další informace o tom, jak funguje Azure CDN pro urychlení přístupu k souborům a dalším prostředkům najdete v tématu [přehled Azure Content Delivery Network](cdn-overview.md).
> 

## <a name="setting-cache-control-headers-in-configuration-files"></a>Hlavičky Cache-Control nastavení v konfiguračních souborech
Statický obsah, jako jsou bitové kopie a stylů, můžete řídit četnosti aktualizace úpravou **applicationHost.config** nebo **web.config** soubory pro vaši webovou aplikaci. **System.webServer\staticContent\clientCache** element v souboru sady configuration `Cache-Control` hlavičky pro obsah. Pro **web.config** soubory, nastavení konfigurace ovlivnit všechno, co ve složce a jejích podsložkách přepsána na úrovni podsložky. Můžete například nastavit výchozí hodnotu TTL nastavení v kořenové složce pro ukládání do mezipaměti všechny statický obsah tři dny a nastavte podsložku s více proměnné obsah do mezipaměti jeho obsah jenom po dobu šesti hodin. I když **applicationHost.config** soubor nastavení ovlivní všechny aplikace v lokalitě, jsou nastavení jakékoli existující přepsána **web.config** soubory v aplikacích.

Následující příklad XML ukazuje, jak nastavit **clientCache** zadat maximální stáří tří dnů:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Určení **UseMaxAge** způsobí, že `Cache-Control: max-age=<nnn>` záhlaví, který se má přidat do odpovědi na základě hodnoty zadané v **CacheControlMaxAge** atribut. Formát časový interval pro **cacheControlMaxAge** atribut je `<days>.<hours>:<min>:<sec>`. Další informace o **clientCache** uzlu, najdete v části [mezipaměti klienta <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Nastavení hlavičky Cache-Control v kódu
Pro aplikace ASP.NET, můžete řídit chování ukládání do mezipaměti prostřednictvím kódu programu nastavením CDN **HttpResponse.Cache** vlastnost. Další informace o **HttpResponse.Cache** vlastnost, najdete v části [HttpResponse.Cache vlastnost](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) a [HttpCachePolicy třída](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Prostřednictvím kódu programu obsah do mezipaměti aplikace technologie ASP.NET postupujte takto:
   1. Ověřte, že obsah je označen jako Uložitelný nastavením `HttpCacheability` k *veřejné*. 
   2. Nastavte validátor mezipaměti voláním jednu z následujících metod:
      - Volání `SetLastModified` nastavit časové razítko změněno.
      - Volání `SetETag` nastavit `ETag` hodnotu.
   3. Volitelně můžete nastavit dobu platnosti mezipaměti voláním `SetExpires`. Jinak platí výchozí mezipaměti heuristické metody popsané v tomto dokumentu.

Například do mezipaměti obsah pro jednu hodinu, přidejte následující kód C#:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Další kroky
* [Přečtěte si podrobnosti o **clientCache** – element](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [Přečtěte si dokumentaci k **HttpResponse.Cache** vlastnost](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* [Přečtěte si dokumentaci k **HttpCachePolicy třída**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

