---
title: "Datový Model aplikace Azure Statistika Telemetrie - Telemetrie výjimek | Microsoft Docs"
description: "Application Insights datový model pro telemetrie výjimek"
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
ms.author: bwren
ms.openlocfilehash: 6b220b0cb6719bac606f599d657d08ab847c7590
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="exception-telemetry-application-insights-data-model"></a>Telemetrie výjimek: Application Insights datový model

V [Application Insights](app-insights-overview.md), představuje instanci výjimky zpracované nebo neošetřené výjimky, které došlo během provádění v monitorované aplikaci.

## <a name="problem-id"></a>Id problému

Identifikátor, kde byla výjimka vydána v kódu. Použít pro seskupení výjimky. Obvykle kombinace typ výjimky a funkce v zásobníku volání.

Maximální délka: 1024 znaků

## <a name="severity-level"></a>Úroveň závažnosti

Úroveň závažnosti trasování. Hodnota může být `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="exception-details"></a>Podrobnosti o výjimce

(Chcete-li být rozšířené)

## <a name="custom-properties"></a>Vlastní vlastnosti

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Vlastní měření

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Další kroky

- V tématu [datový model](application-insights-data-model.md) Application Insights typy a data modelu.
- Zjistěte, jak [diagnostikovat výjimky ve webových aplikacích pomocí služby Application Insights](app-insights-asp-net-exceptions.md).
- Podívejte se na [platformy](app-insights-platforms.md) nepodporuje Application Insights.
