---
title: "Připojení webové aplikace služby App Service k Redis prostřednictvím protokolu Memcache – Azure | Dokumentace Microsoftu"
description: "Připojení webové aplikace ve službě Azure App Service k Redis Cache pomocí protokolu Memcache"
services: app-service\web
documentationcenter: php
author: SyntaxC4
manager: wpickett
editor: riande
ms.assetid: 0fcdf9fa-2995-4839-ba4d-cfa389c4ba06
ms.service: app-service-web
ms.devlang: php
ms.topic: get-started-article
ms.tgt_pltfrm: windows
ms.workload: na
ms.date: 02/29/2016
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: 550b4154f0fe510cb37ec4f8bdef251b06f0b495
ms.openlocfilehash: 4d867f80cfbe5360962550fbda0091f88784b4af


---
# <a name="connect-a-web-app-in-azure-app-service-to-redis-cache-via-the-memcache-protocol"></a>Připojení webové aplikace ve službě Azure App Service k Redis Cache prostřednictvím protokolu Memcache
V tomto článku se dozvíte, jak připojit webovou aplikaci WordPress ve službě [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) ke službě [Azure Redis Cache][12] pomocí protokolu [Memcache][13]. Máte-li stávající webovou aplikaci, která k ukládání do mezipaměti v paměti používá server s protokolem Memcache, můžete migrovat do služby Azure App Service a použít řešení ukládání do mezipaměti první strany v Microsoft Azure s malými či žádnými změnami kódu své aplikace. Můžete také použít stávající odborné znalosti protokolu Memcache k vytvoření vysoce škálovatelných distribuovaných aplikací ve službě Azure App Service se službou Azure Redis Cache k ukládání do mezipaměti v paměti, a to pomocí oblíbených rozhraní aplikací, jako jsou .NET, PHP, Node.js, Java a Python.  

Služba App Service Web Apps umožňuje tento aplikační scénář pomocí shimu Web Apps Memcache, což je místní server s protokolem Memcache, který působí jako proxy server Memcache k ukládání volání služby Azure Redis Cache do mezipaměti. To umožňuje libovolné aplikaci, která komunikuje pomocí protokolu Memcache, ukládat data do mezipaměti ve službě Redis Cache. Tento shim Memcache pracuje na úrovni protokolu, takže jej může použít libovolná aplikace či rozhraní aplikace, pokud komunikuje pomocí protokolu Memcache.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="prerequisites"></a>Požadavky
Shim Web Apps Memcache lze použít s libovolnou aplikací, která komunikuje pomocí protokolu Memcache. V tomto konkrétním příkladu je referenční aplikací škálovatelný web WordPress, který může být zřízen z Azure Marketplace.

Postupujte podle kroků uvedených v těchto článcích:

* [Zřízení instance služby Azure Redis Cache][0]
* [Nasazení škálovatelného webu WordPress v Azure][1]

Jakmile nasadíte škálovatelný web WordPress a zřídíte instanci služby Redis Cache, bude připraveni pokračovat a povolit Memcache Shim ve službě Azure App Service Web Apps.

## <a name="enable-the-web-apps-memcache-shim"></a>Povolení shimu Web Apps Memcache
Chcete-li konfigurovat Memcache shim, je nutné vytvořit tři nastavení aplikace. To lze provést různými metodami, včetně [webu Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), [portálu Classic][3], [rutin Azure PowerShellu][5] nebo [rozhraní příkazového řádku Azure CLI][5]. Pro účely tohoto příspěvku provedeme nastavení aplikace pomocí [webu Azure Portal][4]. Následující hodnoty lze získat v okně **Nastavení** vaší instance služby Redis Cache.

![Okno nastavení služby Azure Redis Cache](./media/web-sites-connect-to-redis-using-memcache-protocol/1-azure-redis-cache-settings.png)

### <a name="add-redishost-app-setting"></a>Přidání nastavení aplikace REDIS_HOST
Prvním nastavením aplikace, které je nutné vytvořit, je nastavení aplikace **REDIS\_HOST**. Toto nastavení určuje cíl, do něhož shim předává informace mezipaměti. Hodnotu nezbytnou k nastavení aplikace REDIS_HOST lze získat v okně **Vlastnosti** instance služby Redis Cache.

![Název hostitele služby Azure Redis Cache](./media/web-sites-connect-to-redis-using-memcache-protocol/2-azure-redis-cache-hostname.png)

Klíč nastavení aplikace nastavte na **REDIS\_HOST** a hodnotu nastavení aplikace nastavte na **název hostitele** instance služby Redis Cache.

![Nastavení aplikace REDIS_HOST webové aplikace](./media/web-sites-connect-to-redis-using-memcache-protocol/3-azure-website-appsettings-redis-host.png)

### <a name="add-rediskey-app-setting"></a>Přidání nastavení aplikace REDIS_KEY
Druhým nastavením aplikace, které je nutné vytvořit, je nastavení aplikace **REDIS\_KEY**. Toto nastavení poskytuje ověřovací token nezbytný k bezpečnému přístupu k instanci služby Redis Cache. Hodnotu nezbytnou k nastavení aplikace REDIS_KEY lze získat v okně **Přístupové klíče** instance služby Redis Cache.

![Primární klíč služby Azure Redis Cache](./media/web-sites-connect-to-redis-using-memcache-protocol/4-azure-redis-cache-primarykey.png)

Klíč nastavení aplikace nastavte na **REDIS\_KEY** a hodnotu nastavení aplikace nastavte na **Primární klíč** instance služby Redis Cache.

![Nastavení aplikace REDIS_KEY webu Azure](./media/web-sites-connect-to-redis-using-memcache-protocol/5-azure-website-appsettings-redis-primarykey.png)

### <a name="add-memcacheshimredisenable-app-setting"></a>Přidání nastavení aplikace MEMCACHESHIM_REDIS_ENABLE
Poslední nastavení aplikace slouží k povolení Memcache Shim ve službě Web Apps, který se pomocí nastavení REDIS_HOST a REDIS_KEY připojuje k službě Azure Redis Cache a předává volání mezipaměti. Klíč nastavení aplikace nastavte na **MEMCACHESHIM\_REDIS\_ENABLE** a hodnotu nastavte na **true**.

![Nastavení aplikace MEMCACHESHIM_REDIS_ENABLE webové aplikace](./media/web-sites-connect-to-redis-using-memcache-protocol/6-azure-website-appsettings-enable-shim.png)

Jakmile dokončíte přidání uvedených tří (3) nastavení aplikace, klikněte na možnost **Uložit**.

## <a name="enable-memcache-extension-for-php"></a>Povolení rozšíření Memcache pro PHP
Chcete-li aplikaci umožnit komunikovat pomocí protokolu Memcache, je nutné nainstalovat rozšíření Memcache k PHP – jazykové rozhraní pro web WordPress.

### <a name="download-the-phpmemcache-extension"></a>Stažení rozšíření php_memcache
Přejděte do [PECL][6]. V kategorii ukládání do mezipaměti klikněte na [memcache][7]. Ve sloupci položek ke stažení klikněte na odkaz DLL.

![Web PHP PECL](./media/web-sites-connect-to-redis-using-memcache-protocol/7-php-pecl-website.png)

Stáhněte si odkaz Non-Thread Safe (NTS) x86 pro verzi PHP, která je povolena ve službě Web Apps. (Výchozí hodnota je PHP 5.4)

![Balíček Memcache webu PHP PECL](./media/web-sites-connect-to-redis-using-memcache-protocol/8-php-pecl-memcache-package.png)

### <a name="enable-the-phpmemcache-extension"></a>Povolení rozšíření php_memcache
Po stažení extrahujte soubor **php\_memcache.dll** a nahrajte jej do adresáře **d:\\domácí\\lokality\\wwwroot\\bin\\ext\\**. Po nahrání souboru php_memcache.dll do webové aplikace je nutné povolit rozšíření modulu PHP Runtime. Chcete-li povolit rozšíření Memcache na webu Azure Portal, otevřete okno **Nastavení aplikace** webové aplikace a přidejte nové nastavení aplikace, jehož klíč je **PHP\_EXTENSIONS** a jehož hodnota je **bin\\ext\\php_memcache.dll**.

> [!NOTE]
> Je-li nutné, aby webová aplikace načetla více rozšíření PHP, hodnota PHP_EXTENSIONS by měla obsahovat čárkami oddělený seznam relativních cest k souborům DLL.
> 
> 

![Nastavení aplikace PHP_EXTENSIONS webové aplikace](./media/web-sites-connect-to-redis-using-memcache-protocol/9-azure-website-appsettings-php-extensions.png)

Jakmile budete hotovi, klikněte na možnost **Uložit**.

## <a name="install-memcache-wordpress-plugin"></a>Instalace modulu plug-in Memcache WordPress
> [!NOTE]
> Můžete také stáhnout modul plug-in [Memcached Object Cache](https://wordpress.org/plugins/memcached/) z webu WordPress.org.
> 
> 

Na stránce modulů plug-in webu WordPress klikněte na možnost **Přidat nový**.

![Stránka modulů plug-in webu WordPress](./media/web-sites-connect-to-redis-using-memcache-protocol/10-wordpress-plugin.png)

Do vyhledávacího pole zadejte **memcached** a stiskněte klávesu **Enter**.

![Přidání nového modulu plug-in webu WordPress](./media/web-sites-connect-to-redis-using-memcache-protocol/11-wordpress-add-new-plugin.png)

Najděte v seznamu položku **Memcached Object Cache** a klikněte na možnost **Nainstalovat nyní**.

![Instalace modulu plug-in Memcache webu WordPress](./media/web-sites-connect-to-redis-using-memcache-protocol/12-wordpress-install-memcache-plugin.png)

### <a name="enable-the-memcache-wordpress-plugin"></a>Povolení modulu plug-in Memcache WordPress
> [!NOTE]
> Chcete-li nainstalovat Visual Studio Team Services, postupujte podle pokynů v tomto blogu týkajících se [Postupu povolení rozšíření webu ve službě Web Apps][8].
> 
> 

V souboru `wp-config.php` přidejte následující kód, a to nad komentář ukončení úprav u konce souboru.

```php
$memcached_servers = array(
    'default' => array('localhost:' . getenv("MEMCACHESHIM_PORT"))
);
```

Po vložení tohoto kódu monaco automaticky uloží dokument.

Dalším krokem je povolit modul plug-in mezipaměti objektů object-cache. To se provádí tak, že přetažením souboru **objekt cache.php** ze složky **wp-content/plugins/memcached** do složky **wp-content** povolíte funkci mezipaměti objektů Memcache Object Cache.

![Vyhledání modulu plug-in object-cache.php memcache](./media/web-sites-connect-to-redis-using-memcache-protocol/13-locate-memcache-object-cache-plugin.png)

Soubor **object-cache.php** je nyní ve složce **wp-content** a funkce mezipaměti objektů Memcache Object Cache je nyní povolena.

![Povolení modulu plug-in object-cache.php memcache](./media/web-sites-connect-to-redis-using-memcache-protocol/14-enable-memcache-object-cache-plugin.png)

## <a name="verify-the-memcache-object-cache-plugin-is-functioning"></a>Ověření funkce modulu plug-in Memcache Object Cache
Všechny kroky k povolení shimu Memcache služby Web Apps jsou nyní dokončeny. Zbývá jen ověřit, zda se instance služby Redis Cache zaplňuje daty.

### <a name="enable-the-non-ssl-port-support-in-azure-redis-cache"></a>Povolení podpory portu bez SSL ve službě Azure Redis Cache
> [!NOTE]
> V době napsání tohoto článku rozhraní příkazového řádku Redis nepodporuje připojení SSL, proto jsou nezbytné následující kroky.
> 
> 

Na webu Azure Portal přejděte k instanci služby Redis Cache, kterou jste pro tuto webovou aplikaci vytvořili. Jakmile se otevře okno požadované mezipaměti, klikněte na ikonu **Nastavení**.

![Tlačítko Nastavení služby Azure Redis Cache](./media/web-sites-connect-to-redis-using-memcache-protocol/15-azure-redis-cache-settings-button.png)

Vyberte v seznamu položku **Přístupové porty**.

![Přístupový port služby Azure Redis Cache](./media/web-sites-connect-to-redis-using-memcache-protocol/16-azure-redis-cache-access-port.png)

Klikněte na možnost **Ne** v položce **Povolit přístup jen přes SSL**.

![Přístupový port služby Azure Redis Cache pouze SSL](./media/web-sites-connect-to-redis-using-memcache-protocol/17-azure-redis-cache-access-port-ssl-only.png)

Uvidíte, že je nyní nastaven port bez SSL. Klikněte na **Uložit**.

![Přístupový portál Redis bez SSL služby Azure Redis Cache](./media/web-sites-connect-to-redis-using-memcache-protocol/18-azure-redis-cache-access-port-non-ssl.png)

### <a name="connect-to-azure-redis-cache-from-redis-cli"></a>Připojení k službě Azure Redis Cache z rozhraní příkazového řádku redis
> [!NOTE]
> Tento krok předpokládá, že je ve vývojovém počítači místně nainstalován redis. [Nainstalujte místně Redis podle těchto pokynů][9].
> 
> 

Otevřete zvolenou konzolu příkazového řádku a zadejte následující příkaz:

```shell
redis-cli –h <hostname-for-redis-cache> –a <primary-key-for-redis-cache> –p 6379
```

Položku **&lt;hostname-for-redis-cache&gt;** nahraďte skutečným názvem hostitele xxxxx.redis.cache.windows.net a položku **&lt;primary-key-for-redis-cache&gt;** nahraďte přístupovým klíčem mezipaměti. Poté stiskněte klávesu **Enter**. Jakmile se rozhraní příkazového řádku připojí k instanci služby Redis Cache, můžete zadat libovolný příkaz redis. Na snímku obrazovky níže jsem se rozhodl vypsat seznamu klíčů.

![Připojení k službě Azure Redis Cache z rozhraní příkazového řádku Redis v terminálu](./media/web-sites-connect-to-redis-using-memcache-protocol/19-redis-cli-terminal.png)

Volání k vypsání klíčů by mělo vrátit hodnotu. Pokud ne, zkuste přejít k příslušné webové aplikaci a zkuste to znovu.

## <a name="conclusion"></a>Závěr
Blahopřejeme! Aplikace WordPress má nyní centralizovanou mezipaměť v paměti, která pomáhá zvýšit propustnost. Nezapomeňte, že Web Apps Memcache Shim lze použít s libovolným klientem Memcache bez ohledu na programovací jazyk či rozhraní aplikace. Chcete-li poskytnout zpětnou vazbu nebo položit dotazy týkající se shimu Web Apps Memcache, zveřejněte příspěvek na [fórech na webu MSDN][10] nebo [StackOverflow][11].

> [!NOTE]
> Pokud chcete začít používat službu Azure App Service před registrací k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na služby App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[0]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache
[1]: http://bit.ly/1t0KxBQ
[2]: http://manage.windowsazure.com
[3]: http://portal.azure.com
[4]: /powershell/azureps-cmdlets-docs
[5]: /downloads
[6]: http://pecl.php.net
[7]: http://pecl.php.net/package/memcache
[8]: http://blog.syntaxc4.net/post/2015/02/05/how-to-enable-a-site-extension-in-azure-websites.aspx
[9]: http://redis.io/download#installation
[10]: https://social.msdn.microsoft.com/Forums/home?forum=windowsazurewebsitespreview
[11]: http://stackoverflow.com/questions/tagged/azure-web-sites
[12]: /services/cache/
[13]: http://memcached.org



<!--HONumber=Jan17_HO4-->


