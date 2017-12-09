---
title: "Inteligentní detekce - snížení poměr závažnost trasování, ve službě Azure Application Insights | Microsoft Docs"
description: "Monitorování trasování aplikací pomocí služby Azure Application Insights pro neobvyklou vzorců trasování telemetrie."
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
ms.date: 11/27/2017
ms.author: mbullwin
ms.openlocfilehash: 29ed195dadb7230df425d6d981a0a482e09ee79f
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/09/2017
---
# <a name="degradation-in-trace-severity-ratio-preview"></a>Snížení trasování závažnost poměr (preview)

Trasování se často používá v aplikacích, jako mohou pomoci zjistit, co se stane, že na pozadí a jeho příběh. Když dojde k potížím, trasování poskytují velmi důležitý přehled posloupnost událostí, což nežádoucí stavu. Trasování jsou obecně nestrukturovaných, je jedna z věcí, které je možné zjistit namítají z nich – jejich úroveň závažnosti. V stabilního stavu aplikace, by Očekáváme, že poměr mezi "dobré" trasování (*informace* a *podrobné*) a "bad" trasování (*upozornění*, *chyba* a *kritický*) zůstane stabilní. Předpokladem je, že "bad" trasování se může stát v pravidelných intervalech do určité míry z důvodu z nejrůznějších důvodů (například přechodný síťový problémů). Ale po zahájení se rozšiřující skutečný problém obvykle manifesty jako zvýšení relativní podíl "bad" trasování vs "dobré" trasování. Inteligentní detekce aplikací Insights automaticky analyzuje trasování zaznamenaných vaší aplikace a může vás upozorní na neobvyklé vzorců závažnost telemetrie trasování.

Tato funkce vyžaduje žádné speciální nastavení, než konfigurace protokolování trasování pro vaši aplikaci (viz Postup konfigurace naslouchací proces protokolu trasování pro [.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-trace-logs) nebo [Java](https://docs.microsoft.com/azure/application-insights/app-insights-java-trace-logs)). Bude aktivní, pokud vaše aplikace generuje dost telemetrických dat výjimky.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Při obdržení tento typ upozornění Inteligentní detekce
Tento typ upozornění se mohou objevit, pokud poměr mezi "dobré" trasování (protokolována s úrovní trasování *informace* nebo *podrobné*) a "bad" trasování (protokolována s úrovní trasování *upozornění*, * Chyby, nebo *závažná chyba*) je docházelo k omezení v určitý den, ve srovnání s směrný plán, vypočítávaný u předchozích sedm dní.

## <a name="does-my-app-definitely-have-a-problem"></a>Aplikace my máme věci má problém?
Ne, oznámení neznamená, že vaše aplikace výborný došlo k problému. I když snížení v poměr mezi "dobré" a "bad" trasování může značit problém aplikace, může být neškodné tato změna v poměr. Zvýšení může být například kvůli nový tok v aplikaci generování další "bad" trasování než existující toky).

## <a name="how-do-i-fix-it"></a>Jakým způsobem ji lze upravit?
Oznámení zahrnují diagnostických informací, které podporují procesu diagnostiky:
1. **Třídění.** Oznámení se dozvíte, jak mnoho operací se vztahuje. Můžete přiřadit prioritu problém.
2. **Rozsah.** Problém ovlivňuje veškerý provoz, nebo jen některé operace? Tyto informace můžete získat z oznámení.
3. **Diagnostika.** Můžete použít související položky a sestavy propojení na podpůrné informace vám pomohou další problém diagnostikovat.


