---
title: "Rozšířené monitorování v řešení vzdáleného monitorování - Azure | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak monitorování zařízení s řídicím panelu řešení vzdáleného monitorování."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 09/28/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: d523902505e38ef3fccf60e7648ea5333daae4f7
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="perform-advanced-monitoring-using-the-remote-monitoring-solution"></a>Proveďte pokročilé monitorování pomocí řešení vzdáleného monitorování

Tento kurz ukazuje možnosti řídicím panelu vzdáleného monitorování. Zavádět tyto možnosti, tento kurz používá scénáři v aplikaci Contoso IoT.

V tomto kurzu použijete dvě Simulovaná zařízení vůz Contoso se dozvíte, jak monitorovat zařízení na řídicím panelu předkonfigurované řešení. Jako operátor Contoso budete muset sledování umístění a chování vašeho dodávky v poli.

V tomto kurzu se naučíte:

>[!div class="checklist"]
> * Filtrování zařízení v řídicím panelu
> * Zobrazení telemetrie v reálném čase
> * Zobrazení podrobností o zařízeních
> * Zobrazit výstrahy z vašich zařízení
> * Zobrazení systému klíčové ukazatele výkonu.

## <a name="prerequisites"></a>Požadavky

Chcete-li v tomto kurzu, je třeba nasazené instanci řešení vzdáleného monitorování ve vašem předplatném Azure.

Pokud jste nenasadili řešení vzdáleného monitorování ještě by se měla Dokončit [nasadit předkonfigurované řešení vzdáleného monitorování](iot-suite-remote-monitoring-deploy.md) kurzu.

## <a name="choose-the-devices-to-display"></a>Vyberte zařízení k zobrazení

Vyberte zařízení, která se zobrazí na **řídicí panel** stránky, použijte filtry. K zobrazení pouze **vůz** zařízení, zvolte integrované **dodávky** filtru v rozevíracím seznamu filtru:

![Filtr pro dodávky na řídicím panelu](media/iot-suite-remote-monitoring-monitor/dashboardtruckfilter.png)

Když použijete filtr, jenom zařízení, které splňují zadaná kritéria filtru zobrazuje v mapě na **řídicí panel** stránky:

![Zobrazení dodávky na mapě](media/iot-suite-remote-monitoring-monitor/dashboardtruckmap.png)

Filtr také určuje zařízení, která se zobrazí v **Telemetrie** grafu:

![Telemetrie vůz zobrazí na řídicím panelu](media/iot-suite-remote-monitoring-monitor/dashboardtelemetry.png)

Pokud chcete vytvořit, upravit a odstranit filtry, zvolte **Správa filtrů**.

## <a name="view-real-time-telemetry"></a>Zobrazení telemetrie v reálném čase

Předkonfigurované řešení používá podrobné v reálném čase telemetrická data v grafu ve **řídicí panel** stránky. Telemetrie graf zobrazuje telemetrické informace pro aktuální filtr vybraná zařízení:

![Výkresu nákladní vůz telemetrie](media/iot-suite-remote-monitoring-monitor/dashboardtelemetryview.png)

Hodnoty telemetrická data k zobrazení, vyberte typ telemetrie v horní části grafu:

![Výkresu nákladní vůz telemetrie](media/iot-suite-remote-monitoring-monitor/dashboardselecttelemetry.png)

Pozastavit za provozu telemetrická zobrazení, zvolte **Flowing**. Chcete-li znovu povolit za provozu zobrazení, zvolte **pozastavení**:

![Pozastavení a restartujte zobrazení telemetrie](media/iot-suite-remote-monitoring-monitor/dashboardtelemetrypause.png)

## <a name="use-the-map"></a>Použití mapy

Mapa zobrazuje informace o simulovaných dodávky vybrána aktuální filtr. Můžete přiblížení a posouvání mapě zobrazíte umístění podrobně vyšší nebo nižší. Ikony zařízení na mapě označují žádné **výstrahy** nebo **upozornění** , které jsou aktivní pro zařízení. Souhrn počtu **výstrahy** a **upozornění** zobrazí nalevo od mapy.

Chcete-li zobrazit podrobnosti o zařízení, posouvání a přiblížení mapy najít zařízení a pak klikněte na zařízení na mapě. Podrobnosti patří:

* Poslední hodnoty telemetrie
* Metody zařízení podporuje.
* Vlastnosti zařízení

![Zobrazit podrobnosti o zařízení na řídicím panelu](media/iot-suite-remote-monitoring-monitor/dashboarddevicedetail.png)

## <a name="view-alarms-from-your-devices"></a>Zobrazit výstrahy z vašich zařízení

Mapy označuje zařízení v aktuální filtr s **výstrahy** a **upozornění**. **Systému výstrahy** panelu zobrazí podrobné informace o poslední výstrahy ze všech zařízení:

![Zobrazení systému výstrahy na řídicím panelu](media/iot-suite-remote-monitoring-monitor/dashboardsystemalarms.png)

Můžete použít **systému výstrahy** filtru upravit časové rozpětí pro poslední výstrahy. Ve výchozím nastavení panelu zobrazí výstrahy z za poslední hodinu:

![Filtrování podle času signály](media/iot-suite-remote-monitoring-monitor/dashboardalarmsfilter.png)

> [!NOTE]
> Posuňte doprava zobrazíte rozpětí filtr času.

## <a name="view-the-system-kpis"></a>Zobrazení systému klíčové ukazatele výkonu.

**Řídicí panel** stránka zobrazuje systému klíčové ukazatele výkonu:

![Filtrování podle času signály](media/iot-suite-remote-monitoring-monitor/dashboardkpis.png)

Můžete použít **klíčového ukazatele výkonu systému** filtru upravit časové rozpětí pro agregaci klíčového ukazatele výkonu. Ve výchozím nastavení zobrazí na panelu klíčových ukazatelů výkonu agregovat přes za poslední hodinu.

## <a name="next-steps"></a>Další kroky

Tento kurz vám ukázal, jak používat **řídicí panel** stránky k filtrování a monitorovat simulované dodávky zřízené v řešení vzdáleného monitorování:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Filtrování zařízení v řídicím panelu
> * Zobrazení telemetrie v reálném čase
> * Zobrazení podrobností o zařízeních
> * Zobrazit výstrahy z vašich zařízení
> * Zobrazení systému klíčové ukazatele výkonu.

Teď, když jste se naučili monitorování zařízení, navrhované další kroky jsou další postup:

* [Zjistit problémy při použití pravidla na základě prahové hodnoty](./iot-suite-remote-monitoring-automate.md).
* [Správa a konfigurace zařízení](./iot-suite-remote-monitoring-manage.md).
* [Řešení potíží a opravám problémů zařízení](./iot-suite-remote-monitoring-maintain.md).
* [Testování řešení s Simulovaná zařízení](iot-suite-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->