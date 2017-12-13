---
title: "Inteligentní detekce - potenciální nevrácená paměť systému detekovaných službou Azure Application Insights | Microsoft Docs"
description: "Monitorování aplikací pomocí služby Azure Application Insights pro potenciální nevracení paměti."
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
ms.openlocfilehash: 452d0a9d0231e54df2a7f1df76c3c2c0fcd94d87
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/13/2017
---
# <a name="memory-leak-detection-preview"></a>Zjišťování nevracení paměti (preview)

Application Insights automaticky analyzuje využití paměti jednotlivých procesů ve vaší aplikaci a může vás upozorní na potenciální nevracení paměti nebo využití vyšší paměti.

Tato funkce vyžaduje žádné speciální nastavení jiné než [Konfigurace čítačů výkonu](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-performance-counters) pro vaši aplikaci. Bude aktivní, pokud vaše aplikace generuje telemetrická čítače výkonu dostatek paměti (například soukromých bajtů).


## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Při obdržení tento typ upozornění Inteligentní detekce
Typické oznámení bude postupovat podle konzistentní zvýšení spotřeby paměti po dlouhou dobu času (několik hodin) do jednoho nebo více procesů nebo jeden nebo více počítačů, které jsou součástí vaší aplikace.
Algoritmy strojového učení se používají pro zjišťování spotřeby vyšší paměti, který odpovídá vzorku nevrácená paměť systému, na rozdíl od spotřeby vyšší paměti z důvodu přirozeně zvýšení využití aplikace.

## <a name="does-my-app-definitely-have-a-problem"></a>Aplikace my máme věci má problém?
Ne, oznámení neznamená, že vaše aplikace výborný došlo k problému. I když vzory nevracení paměti obvykle naznačují problém aplikace, tyto vzory může být typické pro váš konkrétní proces, nebo může mít přirozené obchodního oprávnění a můžete ignorovat.

## <a name="how-do-i-fix-it"></a>Jakým způsobem ji lze upravit?
Oznámení zahrnují diagnostické informace pro podporu v procesu diagnostiky analýzy:
1. **Třídění.** Oznámení zobrazuje zvýšit množství paměti (v GB) a časové rozmezí, ve kterém zvýšilo paměť. Můžete přiřadit prioritu problém.
2. **Rozsah.** Kolik počítačů vystavených vzoru nevracení paměti? Kolik výjimek byly aktivuje během potenciální nevrácená paměť systému? Tyto informace můžete získat z oznámení.
3. **Diagnostika.** Detekce obsahuje vzoru nevracení paměti zobrazující spotřebu paměti procesu v čase. Můžete také související položky a sestavy propojení na podpůrné informace vám pomohou další diagnostikovat problém.
