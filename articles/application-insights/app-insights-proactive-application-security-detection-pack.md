---
title: "Inteligentní detekce - Pack detekce zabezpečení pomocí služby Azure Application Insights | Microsoft Docs"
description: "Monitorování aplikace pomocí služby Azure Application Insights pro potenciální potíže se zabezpečením."
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
ms.openlocfilehash: 837525d70f28c0710b8a8b86d411a7854402207f
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="application-security-detection-pack-preview"></a>Sada zjišťování zabezpečení aplikace (preview)

Application Insights automaticky analyzuje příchozí telemetrická data generované vaší aplikace a zjistí potenciální potíže se zabezpečením. Tato funkce umožňuje identifikovat případné problémy zabezpečení a mohli je zpracovat opravou aplikace nebo provedením nezbytná bezpečnostní opatření.

Tato funkce vyžaduje žádné speciální nastavení jiné než [konfigurace aplikace k odesílání telemetrie](https://docs.microsoft.com/azure/application-insights/app-insights-usage-overview).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Při obdržení tento typ upozornění Inteligentní detekce
Existují tři typy zabezpečení problémy, které jsou zjištěny:
1. Nezabezpečené přístup k adrese URL: adresu URL v aplikaci je přistupuje prostřednictvím protokolu HTTP a HTTPS. Adresu URL, která přijímá požadavky HTTPS, obvykle nesmí přijímal požadavky HTTP. To může znamenat chybu chyb nebo zabezpečení ve vaší aplikaci.
2. Nezabezpečené formuláře: formuláře (nebo jiným požadavkem "POST") v aplikaci používá protokol HTTP místo protokolu HTTPS. Pomocí protokolu HTTP může ohrozit uživatelská data, která odesílají formuláře.
3. Aktivita podezřelé uživatele: aplikace se přistupuje z několika zemích stejným uživatelem přibližně ve stejnou dobu. Například stejného uživatele dostupný aplikace a Spojených státech v tutéž hodinu. Toto zjišťování označuje pokus potenciálně škodlivým přístupem k vaší aplikaci.

## <a name="does-my-app-definitely-have-a-security-issue"></a>Aplikace my máme věci má problém zabezpečení?
Ne, oznámení neznamená, že má vaše aplikace výborný porušení zabezpečení. Detekce žádnou z výše uvedených scénářích určíte v řadě případů porušení zabezpečení. Detekce však mohou mít přirozené obchodního oprávnění a můžete ignorovat.

## <a name="how-do-i-fix-the-insecure-url-access-detection"></a>Jak lze upravit detekce "Nezabezpečené URL přístup"?
1. **Třídění.** Oznámení poskytuje počet uživatelů, kteří přístup nezabezpečené adresy URL a adresy URL, která byla většina vliv nezabezpečené přístup. Můžete přiřadit prioritu problém.
2. **Rozsah.** Jaké procento uživatelé přistupovat k nezabezpečené adresy URL? Kolik adresy URL situace měla vliv na? Tyto informace můžete získat z oznámení.
3. **Diagnostika.** Detekce poskytuje seznam nezabezpečené požadavky a seznamy adres URL a uživatele, kteří situace měla vliv na, při další diagnostice problému.

## <a name="how-do-i-fix-the-insecure-form-detection"></a>Jak lze upravit detekce "Nezabezpečené form"?
1. **Třídění.** Oznámení poskytuje počet nezabezpečené formulářů a počet uživatelů, jejichž data se potenciálně ohrožená. Můžete přiřadit prioritu problém.
2. **Rozsah.** Formulář, který byl součástí největšího počtu nezabezpečené přenosů a co je distribuce přenosů nezabezpečené časem? Tyto informace můžete získat z oznámení.
3. **Diagnostika.** Detekce poskytuje seznam nezabezpečené formulářů a rozpis číslo nezabezpečené přenosů pro každý formulář při další diagnostice problému.

## <a name="how-do-i-fix-the-suspicious-user-activity-detection"></a>Jak lze upravit detekce "podezřelé uživatele aktivita"?
1. **Třídění.** Oznámení poskytuje počet různých uživatelů, které vykazují podezřelé chování. Můžete přiřadit prioritu problém.
2. **Rozsah.** Z kterých zemích podezřelé požadavky pocházejí? Kteří uživatelé se nejvíce podezřelé? Tyto informace můžete získat z oznámení.
3. **Diagnostika.** Detekce poskytuje seznam podezřelé uživatele a seznam zemí, pro každého uživatele, při další diagnostice problému.
