---
title: "Datový Model aplikace Azure Statistika Telemetrie - Telemetrických závislostí | Microsoft Docs"
description: "Application Insights datový model pro telemetrických závislostí"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/17/2017
ms.author: mbullwin
ms.openlocfilehash: aa305c30dc358997420be6802d43fa69e45f4a5f
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="dependency-telemetry-application-insights-data-model"></a>Telemetrických závislostí: Application Insights datový model

Telemetrických závislostí (v [Application Insights](app-insights-overview.md)) představuje interakci monitorované součásti pomocí vzdáleného součásti, jako je například SQL nebo koncový bod HTTP.

## <a name="name"></a>Name (Název)

Název příkazu iniciovala s toto volání závislostí. Mohutnost nízkou hodnotu. Příklady jsou název uložené procedury a šablonu cesty URL.

## <a name="id"></a>ID

Identifikátor závislosti volání instance. Slouží pro korelaci s položkou telemetrie požadavku odpovídající toto volání závislostí. Další informace najdete v tématu [korelace](application-insights-correlation.md) stránky.

## <a name="data"></a>Data

Příkaz iniciovaná toto volání závislostí. Příklady jsou příkaz jazyka SQL a adresy URL protokolu HTTP se všemi parametry dotazu.

## <a name="type"></a>Typ

Název typu závislosti. Hodnota nízkou mohutnost pro logické seskupení závislosti a výklad další pole jako commandName a resultCode. Příklady jsou SQL, Azure table a HTTP.

## <a name="target"></a>cíl

Cílovou lokalitu volání závislostí. Příklady jsou název serveru, adresa hostitele. Další informace najdete v tématu [korelace](application-insights-correlation.md) stránky.

## <a name="duration"></a>Doba trvání

Žádosti o trvání ve formátu: `DD.HH:MM:SS.MMMMMM`. Musí být menší než `1000` dnů.

## <a name="result-code"></a>Kód výsledku

Kód výsledku volání závislostí. Příklady kódu chyby SQL a stavový kód HTTP.

## <a name="success"></a>Úspěch

Údaj o volání úspěšná nebo neúspěšná.

## <a name="custom-properties"></a>Vlastní vlastnosti

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Vlastní měření

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a>Další kroky

- Nastavení sledování závislostí [.NET](app-insights-asp-net-dependencies.md).
- Nastavení sledování závislostí [Java](app-insights-java-agent.md).
- [Psát vlastní závislosti telemetrii](app-insights-api-custom-events-metrics.md#trackdependency)
- V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.
- Podívejte se na [platformy](app-insights-platforms.md) nepodporuje Application Insights.
