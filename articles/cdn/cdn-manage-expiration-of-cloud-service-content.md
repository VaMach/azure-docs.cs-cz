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
ms.openlocfilehash: fe519c3ad5f99899277bf005929142c52a4c4724
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/23/2017
---
# <a name="manage-expiration-of-web-content-in-azure-content-delivery-network"></a>Spravovat vypršení platnosti webového obsahu v síti pro doručování obsahu Azure
> [!div class="op_single_selector"]
> * [Azure webového obsahu](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Storage](cdn-manage-expiration-of-blob-content.md)
> 

Soubory z jakékoli veřejně přístupné počátek webový server může do mezipaměti v Azure Content Delivery Network (CDN), dokud uplynutí jejich time to live (TTL). Hodnota TTL je dáno `Cache-Control` hlavičku HTTP odpovědi ze zdrojového serveru. Tento článek popisuje, jak nastavit `Cache-Control` hlavičky pro funkci webové aplikace Microsoft Azure App Service, Azure Cloud Services, aplikace ASP.NET a weby Internetové informační služby (IIS), které jsou nakonfigurované podobně. Můžete nastavit `Cache-Control` záhlaví buď pomocí konfiguračních souborů nebo prostřednictvím kódu programu.

> [!TIP]
> Můžete nastavit žádné TTL na soubor. V takovém případě Azure CDN automaticky použije výchozí hodnotu TTL sedm dní. Toto výchozí nastavení TTL se vztahuje pouze na obecné webové doručení optimalizace. Pro optimalizace na velkých souborů je výchozí hodnota TTL je jeden den a pro streamování optimalizace médií, je výchozí hodnota TTL je jeden rok.
> 
> Další informace o tom, jak funguje Azure CDN pro urychlení přístupu k souborům a dalším prostředkům najdete v tématu [přehled Azure Content Delivery Network](cdn-overview.md).
> 

## <a name="setting-cache-control-headers-by-using-configuration-files"></a>Nastavení hlavičky Cache-Control pomocí konfiguračních souborů
Statický obsah, jako jsou bitové kopie a stylů, můžete řídit četnosti aktualizace úpravou **applicationHost.config** nebo **Web.config** konfigurační soubory pro vaši webovou aplikaci. `<system.webServer>/<staticContent>/<clientCache>` Element buď soubor sad `Cache-Control` hlavičky pro obsah.

### <a name="using-applicationhostconfig-files"></a>Pomocí souboru ApplicationHost.config souborů
**ApplicationHost.config** soubor je v kořenovém souboru systému konfigurace služby IIS. Nastavení konfigurace v **ApplicationHost.config** souboru vliv na všechny aplikace na webu, ale jsou přepsat nastavení všech **Web.config** souborů, které existují pro webovou aplikaci.

### <a name="using-webconfig-files"></a>Pomocí souborů Web.config
S **Web.config** souboru, můžete upravit způsob celé webové aplikace nebo konkrétní adresáře na vaši webovou aplikaci se chová. Obvykle mají alespoň jednu **Web.config** soubor v kořenové složce webové aplikace. Pro každou **Web.config** souboru v konkrétní složce, nastavení konfigurace ovlivnit všechno, co v této složce a jejích podsložkách, pokud je na úrovni podsložky přepsána jiným **Web.config**souboru. Například můžete nastavit `<clientCache>` element v **Web.config** soubor v kořenové složce webové aplikace pro ukládání do mezipaměti všechny statický obsah na webové aplikace pro tři dny. Můžete také přidat **Web.config** soubor v podsložce s více proměnné obsah (například `\frequent`) a nastavit jeho `<clientCache>` element pro ukládání do mezipaměti obsah název podsložky pro šest hodin. Net výsledkem je tento obsah na celý webový server bude pro tři dny, s výjimkou žádný obsah do mezipaměti `\frequent` adresáři, který bude do mezipaměti jenom po dobu šesti hodin.  

Následující příklad XML ukazuje, jak nastavit `<clientCache>` element v konfiguračním souboru, chcete-li určit maximální dobu tří dnů:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Použít **cacheControlMaxAge** atribut, musíte nastavit hodnotu **cacheControlMode** atribut `UseMaxAge`. Toto nastavení způsobila hlavičky protokolu HTTP a direktiva, `Cache-Control: max-age=<nnn>`, který se má přidat do odpovědi. Formát hodnoty časový interval pro **cacheControlMaxAge** atribut je `<days>.<hours>:<min>:<sec>`. Její hodnota je převeden na sekund a slouží jako hodnotu `Cache-Control` `max-age` – direktiva. Další informace o `<clientCache>` elementu, najdete v části [mezipaměti klienta <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-programmatically"></a>Nastavení hlavičky Cache-Control prostřednictvím kódu programu
Pro aplikace ASP.NET, můžete řídit chování ukládání do mezipaměti prostřednictvím kódu programu nastavením CDN **HttpResponse.Cache** vlastnost .NET API. Informace o **HttpResponse.Cache** vlastnost, najdete v části [HttpResponse.Cache vlastnost](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) a [HttpCachePolicy třída](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Prostřednictvím kódu programu obsah do mezipaměti aplikace technologie ASP.NET postupujte takto:
   1. Ověřte, že obsah je označen jako Uložitelný nastavením `HttpCacheability` k `Public`. 
   2. Nastavit validátor mezipaměti voláním jednu z následujících `HttpCachePolicy` metody:
      - Volání `SetLastModified` nastavit hodnotu časového razítka `Last-Modified` záhlaví.
      - Volání `SetETag` nastavit hodnotu `ETag` záhlaví.
   3. Volitelně můžete nastavit dobu platnosti mezipaměti voláním `SetExpires` nastavit hodnotu `Expires` záhlaví. Jinak platí výchozí mezipaměti heuristické metody popsané v tomto dokumentu.

Například do mezipaměti obsah pro jednu hodinu, přidejte následující kód C#:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="testing-the-cache-control-header"></a>Testování hlavička Cache-Control
Snadno můžete ověřit nastavení TTL webovému obsahu. V prohlížeči [nástroje pro vývojáře](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), test, který zahrnuje webový obsah `Cache-Control` hlavičky odpovědi. Můžete také použít nástroj jako **wget**, [Postman](https://www.getpostman.com/), nebo [Fiddler](http://www.telerik.com/fiddler) a prověří hlavičky odpovědi.

## <a name="next-steps"></a>Další kroky
* [Přečtěte si podrobnosti o **clientCache** – element](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [Přečtěte si dokumentaci k **HttpResponse.Cache** vlastnost](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
* [Přečtěte si dokumentaci k **HttpCachePolicy třída**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

