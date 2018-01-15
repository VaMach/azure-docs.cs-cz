---
title: "Inteligentní detekce - nízkou míru využívání prostředků cloudu detekovaných službou Azure Application Insights | Microsoft Docs"
description: "Monitorování aplikací pomocí služby Azure Application Insights pro nízkou míru využívání prostředků cloudu."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: mbullwin
ms.openlocfilehash: 8382f6047ae222a01cc0e8d6ca9dcf5593d0dff6
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="low-utilization-of-cloud-resources-preview"></a>Nízkou míru využívání prostředků cloudu (preview)

Application Insights automaticky analyzuje spotřeby procesoru každá instance role v aplikaci a zjišťuje instance s nízkým využitím výkonu procesoru. Toto zjišťování umožňuje snížit vašich prostředků Azure a snižovat náklady, a tím snižují počet instancí role, které využívá pro každou roli nebo, a tím snižují počet rolí.

Tato funkce vyžaduje žádné speciální nastavení jiné než [Konfigurace čítačů výkonu](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) pro vaši aplikaci. Bude aktivní, pokud vaše aplikace generuje telemetrická čítače výkonu dostatek procesoru (% času procesoru).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Při obdržení tento typ upozornění Inteligentní detekce
Typické oznámení nastane, když řadu instance Role Web nebo Worker vykazovat nízkým využitím výkonu procesoru.

## <a name="does-my-app-definitely-consume-too-many-resources"></a>Aplikace my máme věci využívat příliš mnoho prostředků?
Ne, oznámení neznamená, že vaše aplikace výborný využívá příliš mnoho prostředků. I když tyto vzory nízkým využitím výkonu procesoru obvykle naznačují, že by se mohly snížit spotřebu prostředků, toto chování může být typické pro vaši konkrétní roli, nebo může mít přirozené obchodního oprávnění a můžete ignorovat. Například může být, že více instancí jsou potřebné pro jiné prostředky, třeba paměť nebo sítě a není procesoru.

## <a name="how-do-i-fix-it"></a>Jakým způsobem ji lze upravit?
Oznámení zahrnují diagnostických informací, které podporují procesu diagnostiky:
1. **Třídění.** Oznámení se zobrazí role ve vaší aplikaci, které vykazují nízkým využitím výkonu procesoru. Můžete přiřadit prioritu problém.
2. **Rozsah.** Kolik rolí vystavených nízkým využitím výkonu procesoru a jak velký počet instancí každé role využívat nízkou procesoru? Tyto informace můžete získat z oznámení.
3. **Diagnostika.** Detekce obsahuje procento využití procesoru, zobrazuje využití procesoru každé instance v čase. Související položky a sestavy propojení podpory informace, například percentily využití procesoru, můžete také použít při další diagnostice problému.
