---
title: "Monitorování aplikací v Azure App Service | Microsoft Docs"
description: "Informace o monitorování aplikací v Azure App Service pomocí portálu Azure."
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: d273da4e-07de-48e0-b99d-4020d84a425e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: byvinyal
ms.openlocfilehash: 283428c603cc73d23f0afa94670a23dbb45068d5
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="how-to-monitor-apps-in-azure-app-service"></a>Postupy: monitorování aplikací v Azure App Service
[Služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714) poskytuje integrované monitorování funkce v [portál Azure](https://portal.azure.com).
To zahrnuje možnost zkontrolovat **kvóty** a **metriky** pro aplikace, jakož i plán služby App Service, nastavení **výstrahy** a i **škálování**automaticky v závislosti na tyto metriky.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Principy kvóty a metriky
### <a name="quotas"></a>Kvóty
Aplikace hostované ve službě App Service se vztahují určité *omezení* s prostředky, které mohou používat. Omezení jsou definovány **plán služby App Service** přidružené aplikaci.

Pokud je aplikace hostovaná v **volné** nebo **sdílené** plánu, a omezení pro aplikace může používat prostředky jsou definovány **kvóty**.

Pokud je aplikace hostovaná v **základní**, **standardní** nebo **Premium** plánu, omezení pro mohou používat prostředky jsou nastavena **velikost**(Malé, střední, velký) a **instance počet** (1, 2, 3,...) z **plán služby App Service**.

**Kvóty** pro **volné** nebo **sdílené** aplikace jsou:

* **CPU(Short)**
  * Nároky na výkon procesoru, které jsou povoleny pro tuto aplikaci v intervalu 5 minut. Tato kvóta resetuje každých 5 minut.
* **CPU(Day)**
  * Celkový počet nároky na výkon procesoru, které jsou povoleny pro tuto aplikaci za den. Tato kvóta resetuje každých 24 hodin půlnoci času UTC.
* **Paměť**
  * Celková velikost paměti pro tuto aplikaci povolena.
* **Šířka pásma**
  * Celková velikost odchozí šířky pásma povolená pro tuto aplikaci za den.
    Tato kvóta resetuje každých 24 hodin půlnoci času UTC.
* **Systém souborů**
  * Celková velikost úložiště, které jsou povoleny.

Pouze kvótu pro aplikace hostované na **základní**, **standardní** a **Premium** plány je **Filesystem**.

Další informace o konkrétní kvót, omezení a funkce, které jsou k dispozici pro jiné služby SKU aplikace naleznete zde: [omezení předplatné služby Azure](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Vynucení kvót
Pokud aplikace v jeho využití překročí **procesoru (krátký)**, **procesoru (den)**, nebo **šířky pásma** kvóty pak aplikace je zastavena, dokud se kvóta resetuje. Během této doby výsledkem všechny příchozí požadavky **HTTP 403**.
![][http403]

Pokud aplikace **paměti** dojde k překročení kvóty a potom se aplikace restartuje.

Pokud **Filesystem** dojde k překročení kvóty, pak všechny zápisu operace selže, který zahrnuje všechny zápisy do protokolů.

Kvóty můžete zvýšit nebo odebrat z vaší aplikace, že si upgradujete plán služby App Service.

### <a name="metrics"></a>Metriky
**Metriky** poskytují informace o aplikaci nebo chování plán služby App Service.

Pro **aplikace**, jsou dostupné metriky:

* **Průměrná doba odezvy**
  * Průměrná doba pro aplikaci obsluhovat požadavky v ms.
* **Průměrná paměti pracovní sady**
  * Průměrná velikost paměti v MIB používané aplikace.
* **Čas procesoru**
  * Množství procesoru v sekundách spotřebovávají aplikace. Další informace o tato metrika najdete v tématu: [procento vs procesoru čas procesoru](#cpu-time-vs-cpu-percentage)
* **Data v**
  * Množství příchozích šířky pásma spotřebovávají aplikace v MIB.
* **Data**
  * Množství odchozí šířky pásma spotřebovávají aplikace v MIB.
* **Http 2xx**
  * Počet požadavků, které jsou výsledkem stavový kód protokolu HTTP > = 200 ale < 300.
* **Http 3xx**
  * Počet požadavků, které jsou výsledkem stavový kód protokolu HTTP > = 300 ale < 400.
* **HTTP 401**
  * Počet požadavků, které jsou výsledkem stavový kód HTTP 401.
* **HTTP 403**
  * Počet požadavků, které jsou výsledkem stavový kód HTTP 403.
* **HTTP 404**
  * Počet požadavků, které jsou výsledkem stavový kód HTTP 404.
* **HTTP 406**
  * Počet požadavků, které jsou výsledkem stavový kód HTTP 406.
* **Http 4xx**
  * Počet požadavků, které jsou výsledkem stavový kód protokolu HTTP > = 400 ale < 500.
* **Chyby protokolu HTTP serveru**
  * Počet požadavků, které jsou výsledkem stavový kód protokolu HTTP > = 500, ale < 600.
* **Paměť pracovní sady**
  * Aktuální velikost paměti, které aplikace v MIB.
* **Požadavky**
  * Celkový počet požadavků bez ohledu na jejich výsledné stavový kód HTTP.

Pro **plán služby App Service**, jsou dostupné metriky:

> [!NOTE]
> Metriky plánu služby App Service jsou k dispozici pro plány v pouze **základní**, **standardní**, a **Premium** vrstev.
> 
> 

* **Procento využití procesoru**
  * Průměrná procesoru použít napříč všemi instancemi plánu.
* **Procento paměti**
  * Průměrná paměť použitá napříč všemi instancemi plánu.
* **Data v**
  * Průměrná příchozí šířku pásma používanou ve všech instancích plánu.
* **Data**
  * Průměr odchozí šířky pásma použít napříč všemi instancemi plánu.
* **Délka fronty disku**
  * Průměrný počet čtení i zápis požadavků, které byly zařazeny do fronty na úložiště. Délka fronty vysoké disku je údajem o aplikaci, která může být zpomalením z důvodu nadměrného diskových operací.
* **Délka fronty http**
  * Průměrný počet požadavků HTTP, které musely nacházejí na fronty před splnění. Vysoká nebo roste délka fronty HTTP je příznakem plánu v případě velkého zatížení.

### <a name="cpu-time-vs-cpu-percentage"></a>Procentuální hodnota vs procesoru času procesoru
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Existují dvě metriky, které odráží využití procesoru. **Čas procesoru** a **procento využití procesoru**

**Čas procesoru** je užitečné pro aplikace hostované v **volné** nebo **sdílené** plány, protože jeden z jejich kvóty je definována v minutách procesoru používané aplikace.

**Procento využití procesoru** je užitečné pro aplikace hostované v **základní**, **standardní** a **premium** plány, protože může být škálovat na více systémů a tato metrika je dobrá indikace toho celkové využití serveru ve všech instancích.

## <a name="metrics-granularity-and-retention-policy"></a>Metriky Členitosti a zásady uchovávání informací
Metriky pro aplikace a plán služby app service se protokolují a agregovat služba s následující členitostí a zásady uchovávání informací:

* **Minutu** členitosti metriky se zachovají pro **48 hodin**
* **Hodina** členitosti metriky se zachovají pro **30 dnů**
* **Den** členitosti metriky se zachovají pro **90 dnů**

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Monitorování kvóty a metriky na portálu Azure.
Můžete zkontrolovat stav různých **kvóty** a **metriky** ovlivnění aplikace [portál Azure](https://portal.azure.com).

![][quotas]
**Kvóty** naleznete v části Nastavení >**kvóty**. Uživatelského umožňuje zkontrolovat: (1) název kvóty, (2) jeho interval resetování, (3) jeho aktuální limit a (4) aktuální hodnotu.

![][metrics]
**Metriky** může být přístup přímo ze stránky prostředky. Můžete také upravit graf podle: (1) **klikněte na tlačítko** na a vyberte (2) **upravit graf**.
Odsud můžete změnit (3) **čas rozsah**, (4) **typ grafu**a (5) **metriky** k zobrazení.  

Další informace o metrikách zde: [monitorovat metriky služby](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Výstrahy a škálování
Metriky pro plán aplikace nebo služby App Service může být připojených k výstrahy. Další informace o tom najdete v tématu [dostávat oznámení o výstrahách](../monitoring-and-diagnostics/insights-alerts-portal.md).

Aplikace služby App Service hostované v basic, standard nebo premium podporu plány služby App Service **škálování**. To umožňuje můžete nakonfigurovat pravidla monitorování metriky plánu služby App Service a můžete zvýšit nebo snížit počet instancí, že poskytuje další zdroje informací, podle potřeby nebo ukládání peníze, když je aplikace přečerpání zřizování. Další informace o automatické škálování zde: [postup škálování](../monitoring-and-diagnostics/insights-how-to-scale.md) a zde [osvědčené postupy pro automatické škálování Azure monitorování](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
