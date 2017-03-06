---
title: Jak funguje Azure App Service
description: "Seznámení s fungováním služby App Service"
keywords: "app service, azure app service, škálování, škálovatelné, plán služby App Service, náklady služby App Service"
services: app-service
documentationcenter: 
author: yochay
manager: erikre
editor: 
ms.assetid: ae74fc32-969e-4580-8d61-02c922f1f184
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/23/2017
ms.author: yochayk
translationtype: Human Translation
ms.sourcegitcommit: edb3325414adf876548181243ddfa2d515aeb0b8
ms.openlocfilehash: 2d830963d3d2adba71a6ca99f79eac0fc8cbfb12
ms.lasthandoff: 02/24/2017


---
# <a name="how-app-service-works"></a>Jak funguje App Service
Azure App Service je cloudová služba navržená k řešení praktických problémů, kterým čelí dnešní vývojáři.
App Service se zaměřuje na zajištění vysoké produktivity při vývoji bez kompromisů při zajišťování aplikací v cloudovém rozsahu. 

App Service navíc poskytuje funkce a rozhraní, které jsou nezbytné pro vytváření podnikových obchodních aplikací. App Service umožňuje vyvíjet aplikace v nejoblíbenějších vývojových jazycích, včetně Javy, PHP, Node.js, Pythonu a jazyků Microsoft .NET. App Service umožňuje:

* Vytvářet vysoce škálovatelné webové aplikace
* Rychle vytvářet back-endy mobilních aplikací pomocí sady snadno použitelných mobilních funkcí, jako jsou datové back-endy, ověřování uživatelů nebo nabízená oznámení
* Implementovat, nasazovat a publikovat rozhraní API pomocí funkce API Apps
* Spojovat obchodní aplikace do kombinovaných workflow a transformovat data pomocí Logic Apps.

> [!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]
> 
> 

Všechny typy aplikací se opírají o škálovatelnou, flexibilní platformu webových aplikací, která vývojářům umožňuje vytvářet optimalizované kompletní životní cykly: od návrhu aplikace až po její údržbu. Funkce životních cyklů umožňují:

* **Rychlé vytváření aplikací**. Můžete začít od nuly nebo využít vhodný balíček OSS z webu Azure Marketplace.
* **Průběžné nasazování**. Automaticky nasadíte nový kód z oblíbených řešení pro řízení zdrojů, jako je například TFS, GitHub nebo BitBucket, a můžete synchronizovat obsah ze služeb online úložiště, jako je například OneDrive nebo DropBox.
* **Testování v produkčním prostředí**. Hladce vytvoříte předprodukční prostředí a zajistíte správu provozu, který v něm probíhá. Pokud je potřeba, odladíte nasazení v cloudu a jestliže se objeví chyby, můžete ho vrátit zpět.
* **Spuštění asynchronních a dávkových úloh**. Kód můžete spouštět v procesech na pozadí nebo aktivovat na základě událostí (například doručení zpráv do fronty Azure Storage) a plánovaných časů (CRON).
* **Škálování aplikací**. K dispozici je mnoho možností automatického škálování služby – horizontálního i vertikálního – podle provozu a využití prostředků. Můžete konfigurovat privátní prostředí vyhrazená pro vaše aplikace.   
* **Údržba aplikací**. V nabídce je mnoho funkcí ladění a diagnostiky umožňujících předcházet problémům a efektivně je řešit, ať už v reálném čase (pomocí funkcí, jako je samoopravení nebo živé ladění), nebo po jejich výskytu prostřednictvím analýzy protokolů a výpisů stavu paměti.

V kostce řečeno, funkce App Service umožňují vývojářům soustředit se na kód a rychle dosáhnout stabilního a vysoce škálovatelného provozního stavu. Pomocí funkcí API Apps a Logic Apps můžou vývojáři vytvářet odolné podnikové aplikace překračující bariéry mezi různými obchodními řešeními i překážky v integraci místních prostředků s cloudem. 

## <a name="videos"></a>Videa
* [Architektura služby Azure App Service](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/)

## <a name="next-steps"></a>Další kroky

Víc se o App Service dozvíte v jednom z následujících témat:

* [Co je Azure App Service?](app-service-value-prop-what-is.md)
  * [Webová aplikace](../app-service-web/app-service-web-overview.md)
  * [Mobilní aplikace](../app-service-mobile/app-service-mobile-value-prop.md)
  * [Aplikace API](../app-service-api/app-service-api-apps-why-best-platform.md)
* [Architektura služby Azure App Service (prezentace)](http://www.slideshare.net/maartenba/windows-azure-web-sites-things-they-dont-teach-kids-in-school-comunity-day-2013)
* [Porovnání služby Azure App Service, služby Cloud Services a služby Virtual Machines](../app-service-web/choose-web-site-cloud-service-vm.md)
* [Pochopení plánů služby App Service](azure-web-sites-web-hosting-plans-in-depth-overview.md)
* [Úvod do prostředí App Service](../app-service-web/app-service-app-service-environment-intro.md)
  * [Cvičení: Vytvoření prostředí služby App Service](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [Podpora vývojových balíků služby Azure App Service](https://azure.microsoft.com/blog/windows-azure-websites-development-stacks-support/)




