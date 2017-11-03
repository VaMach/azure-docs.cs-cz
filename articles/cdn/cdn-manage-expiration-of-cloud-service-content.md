---
title: "Spravovat vypršení platnosti webového obsahu v Azure CDN | Microsoft Docs"
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
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: c207d780857a61d4b1fc0f39e6185cae67abc955
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Spravovat konec platnosti obsahu Azure webové aplikace nebo cloudové služby, ASP.NET nebo služby IIS v Azure CDN
> [!div class="op_single_selector"]
> * [Službě Azure Web Apps nebo cloudové služby, ASP.NET nebo služby IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Služba Azure blob Storage](cdn-manage-expiration-of-blob-content.md)
> 
> 

Soubory z jakékoli veřejně přístupné původu webového serveru můžete v Azure CDN do mezipaměti, dokud uplynutí jeho time to live (TTL).  Hodnota TTL je dáno [ *Cache-Control* záhlaví](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) v odpovědi HTTP ze zdrojového serveru.  Tento článek popisuje, jak nastavit `Cache-Control` hlavičky pro webové aplikace Azure, Azure Cloud Services, aplikace a Internetová informační služba lokalit, které jsou nakonfigurované podobně.

> [!TIP]
> Můžete nastavit žádné TTL na soubor.  V takovém případě Azure CDN automaticky použije výchozí hodnotu TTL sedm dní.
> 
> Další informace o tom, jak funguje Azure CDN pro urychlení přístupu k souborům a dalším prostředkům, najdete v článku [přehled CDN Azure](cdn-overview.md).
> 
> 

## <a name="setting-cache-control-headers-in-configuration"></a>Nastavení hlavičky Cache-Control v konfiguraci
Statický obsah, jako jsou bitové kopie a stylů, můžete řídit četnosti aktualizace úpravou **applicationHost.config** nebo **web.config** soubory pro vaši webovou aplikaci.  **System.webServer\staticContent\clientCache** nastaví element v konfiguračním souboru `Cache-Control` hlavičky pro obsah. Pro **web.config**, nastavení konfigurace ovlivní všechno, co ve složce a všechny podsložky, není-li přepsat na úrovni podsložky.  Můžete například nastavit výchozí čas to-live v kořenovém adresáři mít všechny statický obsah do mezipaměti pro 3 dny, ale mají do podsložky, která má více proměnné obsah s nastavení mezipaměti na 6 hodin.  Pro **applicationHost.config**, bude mít vliv na všechny aplikace na webu, ale mohou být přepsána nastaveními v **web.config** soubory v aplikacích.

Následující kód XML znázorňuje příklad nastavení **clientCache** zadat maximální stáří 3 dny:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Určení **UseMaxAge** přidá `Cache-Control: max-age=<nnn>` hlavičky odpovědi na základě hodnoty zadané v **CacheControlMaxAge** atribut. Není ve formátu časový interval pro **cacheControlMaxAge** atribut je <days>.<hours>:<min>:<sec>. Další informace o **clientCache** uzlu, najdete v části [mezipaměti klienta <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Nastavení hlavičky Cache-Control v kódu
Pro aplikace ASP.NET, můžete nastavit CDN chování ukládání do mezipaměti prostřednictvím kódu programu nastavením **HttpResponse.Cache** vlastnost. Další informace o **HttpResponse.Cache** vlastnost, najdete v části [HttpResponse.Cache vlastnost](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) a [HttpCachePolicy třída](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Pokud chcete prostřednictvím kódu programu obsah mezipaměti aplikace technologie ASP.NET, ujistěte se, že obsah je označen jako Uložitelný nastavením HttpCacheability na *veřejné*. Také ověřte, zda validátor mezipaměti. Validátor mezipaměti může být poslední změny nastavit časové razítko nastavit, že zavoláte SetLastModified nebo hodnotu etag voláním SetETag. V případě potřeby můžete také nastavit dobu platnosti mezipaměti voláním SetExpires, nebo můžete spoléhat na výchozí mezipaměti heuristické metody popsané výše v tomto dokumentu.  

Například do mezipaměti obsah pro jednu hodinu, přidejte následující příkaz:  

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

