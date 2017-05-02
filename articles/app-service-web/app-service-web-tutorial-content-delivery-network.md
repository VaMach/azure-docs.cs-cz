---
title: "Přidání sítě Content Delivery Network do služby Azure App Service | Dokumentace Microsoftu"
description: "Přidejte do služby Azure App Service síť Content Delivery Network pro doručování statických souborů z hraničních uzlů."
services: app-service
author: syntaxc4
ms.author: cfowler
ms.date: 04/03/2017
ms.topic: hero-article
ms.service: app-service-web
manager: erikre
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: 7ba3737566401152a3171e8926beca188045230c
ms.lasthandoff: 04/22/2017

---
# <a name="add-a-content-deliver-network-on-an-azure-app-service"></a>Přidání sítě Content Delivery Network do služby Azure App Service

V tomto kurzu přidáte síť Content Delivery Network (CDN) do služby Azure App Service, abyste zpřístupnili statický obsah na serveru Edge. Vytvoříte profil CDN, což je kolekce až 10 koncových bodů CDN.

Síť Content Delivery Network ukládá statický webový obsah do mezipaměti na strategicky umístěných místech, a tak poskytuje maximální propustnost pro doručování obsahu uživatelům. Mezi výhody používání CDN k ukládání webových prostředků do mezipaměti patří:

* Lepší výkon a uživatelské prostředí pro koncové uživatele, zvláště při používání aplikací, které k načtení obsahu vyžadují vícenásobný přenos.
* Velké škálování pro lepší zvládání náhlého vysokého zatížení, například v případě uvedení produktu na trh.
* Díky distribuci uživatelských požadavků a poskytování obsahu ze serverů Edge dochází k nižšímu přenosu ke zdroji.

> [!TIP]
> Zkontrolujte aktuální seznam [lokalit POP v Azure CDN](https://docs.microsoft.com/en-us/azure/cdn/cdn-pop-locations).
>

## <a name="deploy-the-sample"></a>Nasazení ukázky

K dokončení tohoto kurzu budete potřebovat aplikaci nasazenou ve webové aplikaci. Postupujte podle [rychlého úvodu ke statickému HTML](app-service-web-get-started-html.md), který poskytuje základy pro tento kurz.

## <a name="step-1---login-to-azure-portal"></a>Krok 1 – přihlášení k webu Azure Portal

Nejprve otevřete svůj oblíbený prohlížeč a přejděte na web [Azure Portal](https://portal.azure.com).

## <a name="step-2---create-a-cdn-profile"></a>Krok 2 – Vytvoření profilu CDN

Klikněte na tlačítko `+ New` (+ Nový) v levém navigačním panelu a pak klikněte na **Web a mobilní zařízení**. V kategorii Web a mobilní zařízení vyberte **CDN**.

Do následujících polí zadejte:

| Pole | Ukázková hodnota | Popis |
|---|---|---|
| Name (Název) | myCDNProfile | Název profilu CDN. |
| Umístění | Západní Evropa | Toto je umístění v Azure, kam se uloží informace o vašem profilu CDN. Nemá žádný vliv na umístění koncových bodů CDN. |
| Skupina prostředků | myResourceGroup | Další informace o skupinách prostředků najdete v tématu [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md#resource-groups). |
| Cenová úroveň | Akamai Standard | V článku [Přehled sítě CDN](../cdn/cdn-overview.md#azure-cdn-features) najdete porovnání cenových úrovní. |

Klikněte na možnost **Vytvořit**.

Otevřete centrum skupin prostředků z levého navigačního panelu a vyberte **myResourceGroup**. Ze seznamu prostředků vyberte **myCDNProfile**.

![azure-cdn-profile-created](media/app-service-web-tutorial-content-delivery-network/azure-cdn-profile-created.png)

## <a name="step-3---create-a-cdn-endpoint"></a>Krok 3 – Vytvoření koncového bodu CDN

Klikněte na **+ Koncový bod** z příkazů vedle pole hledání, a tím otevřete okno pro vytvoření koncového bodu.

Do následujících polí zadejte:

| Pole | Ukázková hodnota | Popis |
|---|---|
| Name (Název) |  | Tento název se použije pro přístup k prostředkům v mezipaměti v doméně `<endpointname>.azureedge.net`. |
| Typ zdroje | Webová aplikace | Po výběru typu zdroje se zobrazí místní nabídky pro zbývající pole. Pokud vyberete vlastní zdroj, zobrazí se textové pole pro název počátečního hostitele. |
| Název počátečního hostitele | |  Tento rozevírací seznam bude obsahovat všechny dostupné zdroje zadaného typu. Pokud jste jako Typ zdroje vybrali možnost Vlastní zdroj, zadáte doménu vlastního zdroje.  |

Klikněte na tlačítko **Add** (Přidat).

Vytvoří se koncový bod, a jakmile se vytvoří koncový bod Content Delivery Network, stav se zaktualizuje na **spuštění**.

![azure-cdn-endpoint-created](media/app-service-web-tutorial-content-delivery-network/azure-cdn-endpoint-created.png)

## <a name="step-4---serve-from-azure-cdn"></a>Krok 4 – Poskytování obsahu z Azure CDN

Teď, když je koncový bod CDN **spuštěný**, byste měli mít přístup k obsahu z koncového bodu CDN.

Vzhledem k tomu, že jsme jako základ pro tento kurz použili [rychlý úvod ke statickému HTML](app-service-web-get-started-html.md), měli bychom mít v naší síti CDN dostupné následující složky: `css`, `img` a `js`.

Cesty k obsahu v adrese URL webové aplikace `http://<app_name>.azurewebsites.net/img/` a adrese URL koncového bodu CDN `http://<endpointname>.azureedge.net/img/` jsou stejné. To znamená, že můžete jednoduše dosadit doménu koncového bodu CDN pro jakýkoli obsah, který se pak bude poskytovat ze sítě CDN.

Načtěte z koncového bodu CDN první obrázek tak, že přejdete ve svém oblíbeném webovém prohlížeči na následující adresu URL:

```bash
http://<endpointname>.azureedge.net/img/03-enterprise.png
```

Když je teď statický obsah dostupný v síti CDN, můžete svou aplikaci aktualizovat, aby používala k doručování obsahu koncovému uživateli koncový bod CDN.

V závislosti na jazyku, na kterém je váš web postavený, může být k dispozici mnoho rozhraní, která vám pomohou s náhradním řešením v případě selhání CDN. Například ASP.NET poskytuje podporu [sdružování a minimalizace](https://docs.microsoft.com/en-us/aspnet/mvc/overview/performance/bundling-and-minification#using-a-cdn) umožňující také náhradní řešení v případě selhání CDN.

Pokud ve vašem jazyce není podpora náhradního řešení v případě selhání CDN integrovaná nebo pro ni není k dispozici knihovna, můžete použít rozhraní JavaScriptu, jako je [FallbackJS](http://fallback.io/), které podporuje načítání [skriptů](https://github.com/dolox/fallback/tree/master/examples/loading-scripts), [šablon stylů](https://github.com/dolox/fallback/tree/master/examples/loading-stylesheets) a [obrázků](https://github.com/dolox/fallback/tree/master/examples/loading-images).

## <a name="step-5---purge-the-cdn"></a>Krok 5 – Vyprázdnění CDN

Někdy může být nutné vynutit vyprázdnění CDN, například pokud chcete ukončit platnost obsahu před vypršení hodnoty TTL (Time to Live).

Azure CDN je možné vyprázdnit ručně z okna Profil CDN nebo Koncový bod CDN. Pokud vyberete vyprázdnění na stránce Profil, bude nutné vybrat, který koncový bod chcete vyprázdnit.

Chcete-li vyprázdnit obsah, zadejte cesty k obsahu, který chcete vyprázdnit. Můžete předat úplnou cestu k souboru pro vyprázdnění jednotlivých souborů, nebo část cesty pro vyprázdnění a obnovení obsahu z konkrétní složky.

Jakmile zadáte všechny cesty k obsahu, který chcete vyprázdnit, klikněte na **Vyprázdnit**.

![app-service-web-purge-cdn](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

## <a name="step-6---map-a-custom-domain"></a>Krok 6 – Mapování vlastní domény

Mapování vlastní domény na koncový bod CDN poskytuje jednotnou doménu pro vaši webovou aplikaci.

Pokud chcete mapovat vlastní doménu na koncový bod CDN, vytvořte u svého registrátora domény záznam CNAME.

> [!NOTE]
> Záznam CNAME je funkce DNS, která mapuje zdrojovou doménu, například `www.contosocdn.com` nebo `static.contosocdn.com`, na cílovou doménu.

V tomto případě přidáme zdrojovou doménu `static.contosocdn.com`, která bude odkazovat na cílovou doménu, což je koncový bod CDN.

| zdrojová doména | cílová doména |
|---|---|
| static.contosocdn.com | &lt;název_koncového_bodu&gt;.azureedge.net |

V okně přehledu koncového bodu CDN klikněte na tlačítko `+ Custom domain` (+ Vlastní doména).

V okně Přidat vlastní doménu zadejte do dialogového okna vlastní doménu včetně poddomény. Zadejte například název domény ve formátu `static.contosocdn.com`.

Klikněte na tlačítko **Add** (Přidat).

## <a name="step-7---version-content"></a>Krok 7 – Správa verzí obsahu

V levém navigačním panelu koncového bodu CDN vyberte **Mezipaměť** v části Nastavení.

Okno **Mezipaměť** umožňuje konfigurovat, jak síť CDN zpracovává řetězce dotazu v žádosti.

> [!NOTE]
> Popis možností chování při ukládání řetězců dotazu do mezipaměti najdete v tématu [Řízení chování při ukládání do mezipaměti Azure CDN pomocí řetězců dotazu](../cdn/cdn-query-string.md).

Z rozevíracího seznamu Chování při ukládání řetězců dotazu do mezipaměti vyberte **Ukládat do mezipaměti každou jedinečnou adresu URL**.

Klikněte na **Uložit**.

## <a name="next-steps"></a>Další kroky

* [Co je Azure CDN?](../best-practices-cdn.md?toc=%2fazure%2fcdn%2ftoc.json)
* [Povolení HTTPS pro vlastní doménu Azure CDN](../cdn/cdn-custom-ssl.md)
* [Vylepšení výkonu prostřednictvím komprimace souborů v Azure CDN](../cdn/cdn-improve-performance.md)
* [Předběžné načtení prostředků v koncovém bodu Azure CDN](../cdn/cdn-preload-endpoint.md)

