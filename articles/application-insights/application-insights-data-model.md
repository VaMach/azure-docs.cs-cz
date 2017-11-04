---
title: "Azure Application Insights Telemetrie datový Model | Microsoft Docs"
description: "Přehled modelu data aplikací – přehled"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: mbullwin
ms.openlocfilehash: b14eea46e773a4b92ba20cd3121cd258f86307c9
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="application-insights-telemetry-data-model"></a>Application Insights telemetrie datový model

[Azure Application Insights](app-insights-overview.md) odesílá telemetrii z webovou aplikaci na portálu Azure, takže můžete analyzovat výkon a využití vaší aplikace. Model telemetrie je standardizované, aby bylo možné vytvořit monitorování nezávislé na jazyku a platformu. 

Data shromážděná pomocí Application Insights modelů tento vzor provádění Typická aplikace:

![Application Insights aplikačního modelu](./media/application-insights-data-model/application-insights-data-model.png)

Následující typy telemetrických dat se používají ke sledování provádění vaší aplikace. Následující tři typy jsou obvykle automaticky shromážděná sadou Application Insights SDK z Architektura webových aplikací:

* [**Žádost o** ](application-insights-data-model-request-telemetry.md) – vygenerované protokolu žádost přijatých vaší aplikace. Například web Application Insights SDK automaticky generuje položku telemetrická žádost pro každý požadavek HTTP, která přijímá vaší webové aplikace. 

    **Operace** je vláken provádění, který zpracuje žádost. Můžete také [napsat kód](app-insights-api-custom-events-metrics.md#trackrequest) monitorování jiné typy operací, například "probuzení" v webové úlohy nebo funkce, pravidelně zpracovává data.  Každá operace má identifikátor. Toto ID, které můžou být použité k [skupiny](application-insights-correlation.md) všechny telemetrii vygenerovanou při aplikace je zpracování požadavku. Každou operaci buď úspěšné nebo selže a doba trvání intervalu.
* [**Výjimka** ](application-insights-data-model-exception-telemetry.md) – obvykle představuje výjimku, která způsobuje selhání operace.
* [**Závislost** ](application-insights-data-model-dependency-telemetry.md) -představuje volání z vaší aplikace na externí služby nebo úložiště, jako je rozhraní API REST nebo SQL. V technologii ASP.NET, jsou definovány volání závislost SQL `System.Data`. Volání do koncových bodů protokolu HTTP jsou definovány `System.Net`. 

Application Insights obsahuje tři typy další data pro vlastní telemetrii:

* [Trasování](application-insights-data-model-trace-telemetry.md) – použít buď přímo nebo prostřednictvím adaptér implementovat protokolování diagnostiky pomocí představuje instrumentace rozhraní, které je pro vás, jako například `Log4Net` nebo `System.Diagnostics`.
* [Událost](application-insights-data-model-event-telemetry.md) – obvykle používané k zachycení interakce uživatele s vaší služby, k analýze vzorce používání.
* [Metrika](application-insights-data-model-metric-telemetry.md) – slouží jako pravidelná skalární měření sestavy.

Každá položka telemetrie můžete definovat [informace o kontextu](application-insights-data-model-context.md) jako id aplikace verze nebo uživatelské relace. Kontext není sadu silného typu pole, která odblokuje určité scénáře. Když verze aplikace správně inicializována, Application Insights zjistit nový vzory v korelaci s novým nasazením chování aplikace. Id relace lze použít k výpočtu se výpadek nebo problém dopad na uživatele. Pro některé výpočet jednoznačného počtu hodnot id relace došlo k chybě závislosti, Chyba trasování nebo kritické výjimky dává dobrou znalost jazyka vliv.

Application Insights telemetrie model definuje způsob, jak [korelovat](application-insights-correlation.md) telemetrie operaci, která je součástí. Například žádost o můžete provádět volání databáze SQL a zaznamenává informace o diagnostiky. Můžete nastavit kontext korelace telemetrie položek, které tie zpět na telemetrická žádost.

## <a name="schema-improvements"></a>Vylepšení schématu

Application Insights datový model je jednoduchý a základní ještě efektivní způsob, jak model telemetrii aplikace. Snažíme zachovat modelu jednoduché a tenký pro podporu základní scénáře a povolit rozšíření schématu pro pokročilé uživatele.

Chcete sestavu datový model nebo schématu problémy a návrhy použít Githubu [ApplicationInsights domovské](https://github.com/Microsoft/ApplicationInsights-Home/labels/schema) úložiště.

## <a name="next-steps"></a>Další kroky

- [Psát vlastní telemetrii](app-insights-api-custom-events-metrics.md)
- Zjistěte, jak [rozšířit a filtrovat telemetrie](app-insights-api-filtering-sampling.md).
- Použití [vzorkování](app-insights-sampling.md) minimalizovat objem telemetrii na základě dat modelu.
- Podívejte se na [platformy](app-insights-platforms.md) nepodporuje Application Insights.
