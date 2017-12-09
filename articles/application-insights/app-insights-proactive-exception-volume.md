---
title: "Inteligentní detekce - neobvyklé zvýšení svazku výjimka ve službě Azure Application Insights | Microsoft Docs"
description: "Sledování výjimek aplikace pomocí služby Azure Application Insights pro neobvyklou vzory ve svazku výjimka."
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
ms.date: 12/08/2017
ms.author: mbullwin
ms.openlocfilehash: 8030f3331a03170bb265c417a57725544bdc7d3f
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/09/2017
---
# <a name="abnormal-rise-in-exception-volume-preview"></a>Nestandardní zvýšení výjimka svazek (preview)

Application Insights automaticky analyzuje výjimek vyvolaných ve vaší aplikaci a může vás upozorní na neobvyklé vzorců telemetrie výjimek.

Tato funkce vyžaduje žádné speciální nastavení jiné než [konfigurace generování sestav výjimky](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-exceptions#set-up-exception-reporting) pro vaši aplikaci. Bude aktivní, pokud vaše aplikace generuje dost telemetrických dat výjimky.

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Při obdržení tento typ upozornění Inteligentní detekce
Tento typ upozornění může získat, pokud vaše aplikace dochází k neobvyklé nárůstem počtu výjimek určitého typu během dne, ve srovnání s směrný plán, vypočítávaný u předchozích sedm dní.
Algoritmy strojového učení se používá pro zjišťování nárůst počtu výjimka při přihlédnutím k přirozené růstu využití vaší aplikace.

## <a name="does-my-app-definitely-have-a-problem"></a>Aplikace my máme věci má problém?
Ne, oznámení neznamená, že vaše aplikace výborný došlo k problému. I když nadměrný počet výjimek obvykle označuje potíže, aplikace, tyto výjimky může být neškodné a správně zpracováván vaší aplikace.

## <a name="how-do-i-fix-it"></a>Jakým způsobem ji lze upravit?
Oznámení zahrnují diagnostických informací, které podporují procesu diagnostiky:
1. **Třídění.** Oznámení se zobrazuje počet uživatelů, kteří nebo se vztahuje jak mnoho požadavků. Můžete přiřadit prioritu problém.
2. **Rozsah.** Problém ovlivňuje veškerý provoz, nebo jen některé operace? Tyto informace můžete získat z oznámení.
3. **Diagnostika.** Detekce obsahuje informace o metodě, z níž byla výjimka vydána, a také typ výjimky. Můžete také související položky a sestavy propojení na podpůrné informace vám pomohou další diagnostikovat problém.