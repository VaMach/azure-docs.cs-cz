---
title: "Mapa aplikace ve službě Azure Application Insights | Microsoft Docs"
description: "Vizuální prezentaci závislostí mezi součástmi aplikace označeny klíčových ukazatelů výkonu a výstrahy."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: e1eb2177d6032142781e6e31af6c7f6313d38f4d
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="application-map-in-application-insights"></a>Mapa aplikace ve službě Application Insights
V [Azure Application Insights](app-insights-overview.md), mapa aplikace je visual rozložení vztahů závislosti komponent vaší aplikace. Jednotlivé komponenty ukazuje klíčových ukazatelů výkonu, například zatížení, výkonu, chyb a výstrahy, které vám pomohou zjistit všechny součásti, která způsobila problém výkonu nebo selhání. Můžete kliknutím z libovolné součásti podrobnější diagnostiku, jako je například události Application Insights. Pokud vaše aplikace používá služby Azure, můžete také kliknutím na Azure diagnostics, jako je například doporučení Poradce pro databáze systému SQL.

Jako další grafy budete moct připnout mapování aplikace na Azure řídicí panel, kde je plně funkční. 

## <a name="open-the-application-map"></a>Otevřete aplikaci mapy
Mapy otevřete v okně Přehled pro aplikaci:

![Otevřete aplikaci mapy](./media/app-insights-app-map/01.png)

![Mapa aplikace](./media/app-insights-app-map/02.png)

Mapa zobrazuje:

* Testy dostupnosti
* Součásti klientské strany (monitorovat pomocí JavaScript SDK)
* Klientská součást produktu
* Závislosti součásti klienta a serveru

Můžete rozbalit nebo sbalit skupiny odkaz závislost:

![Sbalit](./media/app-insights-app-map/03.png)

Pokud máte velký počet závislostí jednoho typu (SQL, HTTP atd.), se mohou objevit seskupené. 

![seskupené závislosti](./media/app-insights-app-map/03-2.png)

## <a name="spot-problems"></a>Identifikaci problémů
Každý uzel má ukazatele relevantní výkonu, jako je například zatížení, výkonu a selhání sazby za tuto součást. 

Ikony upozornění upozorňují na možné problémy. Oranžové upozornění znamená, že tam jsou chyby v žádostech, zobrazení stránek nebo volání závislostí. Red se rozumí míra selhání výše 5 %. Pokud chcete upravit tyto prahové hodnoty, otevřete panel Možnosti.

![selhání ikony](./media/app-insights-app-map/04.png)

Aktivní výstrahy také zobrazit nahoru: 

![aktivní výstrahy](./media/app-insights-app-map/05.png)

Pokud používáte SQL Azure, je ikonu, která ukazuje, kdy jsou doporučení na tom, jak může zlepšit výkon. 

![Azure doporučení](./media/app-insights-app-map/06.png)

Kliknutím na libovolnou ikonu zobrazíte další podrobnosti:

![Azure doporučení](./media/app-insights-app-map/07.png)

## <a name="diagnostic-click-through"></a>Diagnostické klikněte na tlačítko prostřednictvím
Každý z uzlů na mapě nabízí cílové kliknutím prostřednictvím pro diagnostiku. Možnosti se liší v závislosti na typu uzlu.

![Možnosti serveru](./media/app-insights-app-map/09.png)

Mezi možnosti pro součásti, které jsou hostované v Azure, patří přímé odkazy na ně.

## <a name="filters-and-time-range"></a>Filtry a časový rozsah
Ve výchozím nastavení mapy souhrn všech dat k dispozici pro vybrané časové rozmezí. Ale můžete filtrovat tak, aby obsahovala pouze konkrétní operaci názvy nebo závislosti.

* Název operace: Jedná se o zobrazení stránky a typy požadavků na straně serveru. Pomocí této možnosti mapa znázorňuje klíčového ukazatele výkonu na uzlu serveru nebo klientské pro pouze vybrané operace. Zobrazuje závislosti volat v rámci těchto konkrétních operací.
* Název základní závislosti: Jedná se o AJAX prohlížeče závislosti a závislosti na straně serveru. Pokud sestavu vlastní závislosti telemetrie s rozhraním API TrackDependency zároveň jsou zde. Můžete vybrat závislosti zobrazit na mapě. Tento výběr aktuálně nefiltruje žádosti na straně serveru nebo zobrazení stránky na straně klienta.

![Nastavení filtrů](./media/app-insights-app-map/11.png)

## <a name="save-filters"></a>Uložit filtry
Pokud chcete uložit filtry, které jste použili, filtrované zobrazení na připnout [řídicí panel](app-insights-dashboards.md).

![Připnutí na řídicí panel](./media/app-insights-app-map/12.png)

## <a name="error-pane"></a>Podokno chyby
Při kliknutí na uzel v mapě, zobrazí se podokno k chybě na pravé straně shrnutí selhání pro tento uzel. Chyby jsou nejprve seskupené podle ID operace a potom seskupené podle ID problému.

![Podokno chyby](./media/app-insights-app-map/error-pane.png)

Kliknutím na selhání přejdete k nejnovější instanci tohoto selhání.

## <a name="resource-health"></a>Stav prostředků
Pro některé typy prostředků v horní části podokna chyba se zobrazí stav prostředku. Například kliknutím na uzel SQL se zobrazí stav databáze a všechny výstrahy, které mají aktivováno.

![Stav prostředků](./media/app-insights-app-map/resource-health.png)

Můžete kliknout na název prostředku zobrazíte standardní přehled metriky pro tento prostředek.

## <a name="end-to-end-system-app-maps"></a>Systém začátku do konce aplikace mapy

*Vyžaduje SDK verze 2.3 nebo vyšší*

Pokud aplikace obsahuje několik komponenty, například služby back-end kromě do webové aplikace - pak je můžete zobrazit všechny na mapě jednu integrované aplikaci.

![Nastavení filtrů](./media/app-insights-app-map/multi-component-app-map.png)

Mapa aplikace vyhledá uzly serveru pomocí následujících závislostí HTTP volání mezi servery s Application Insights SDK nainstalovat. Každý prostředek Application Insights se předpokládá, že obsahují jeden server.

### <a name="multi-role-app-map-preview"></a>Aplikace služby role mapy (preview)

Ukázkové aplikace s více role mapy funkce vám umožňuje používat mapy aplikace s více servery odesílání dat do stejného zdroje Application Insights nebo klíč instrumentace. Servery v mapě jsou oddělených vlastnost cloud_RoleName na telemetrie položky. Nastavit *mapování aplikace s více rolí* k *na* z okna verze Preview povolit tuto konfiguraci.

Tento přístup může požadovaných, v aplikaci micro-services, nebo v jiných scénářích, kde chcete korelovat události na více serverech v rámci jednoho prostředku Application Insights.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a>Váš názor
Zadejte prosím zpětnou vazbu prostřednictvím možnosti portálu zpětné vazby.

![Obrázek MapLink-1](./media/app-insights-app-map/13.png)


## <a name="next-steps"></a>Další kroky

* [Azure Portal](https://portal.azure.com)
