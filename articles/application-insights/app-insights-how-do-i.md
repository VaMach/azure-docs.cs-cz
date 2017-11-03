---
title: "Jak na to... ve službě Azure Application Insights | Microsoft Docs"
description: "Nejčastější dotazy týkající se ve službě Application Insights."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 48b2b644-92e4-44c3-bc14-068f1bbedd22
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: mbullwin
ms.openlocfilehash: a32127f14c93012b5ace11ff982824f9ecba7d94
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="how-do-i--in-application-insights"></a>Jak mám udělat ... pomocí Application Insights?
## <a name="get-an-email-when-"></a>Získat e-mailem při...
### <a name="email-if-my-site-goes-down"></a>Pokud web přestane fungovat e-mailu
Nastavit [dostupnosti webového testu](app-insights-monitor-web-app-availability.md).

### <a name="email-if-my-site-is-overloaded"></a>E-mailu, pokud je přetížena mé lokality
Nastavit [výstraha](app-insights-alerts.md) na **doba odezvy serveru**. Prahová hodnota mezi 1 a 2 sekundami by měly fungovat.

![](./media/app-insights-how-do-i/030-server.png)

Aplikace může také zobrazit známky kmen vrácením selhání kódy. Nastavit výstrahy na **neúspěšné požadavky**.

Pokud chcete nastavit upozornění na **výjimky serveru**, možná budete muset udělat [některé další nastavení](app-insights-asp-net-exceptions.md) Chcete-li zobrazit data.

### <a name="email-on-exceptions"></a>E-mailu na výjimky
1. [Nastavili monitorování výjimek](app-insights-asp-net-exceptions.md)
2. [Nastavit výstrahy](app-insights-alerts.md) na výjimku počet metrika

### <a name="email-on-an-event-in-my-app"></a>E-mailu na události v mé aplikace
Předpokládejme, že chcete získat e-mailu, když dojde k určité události. Application Insights neposkytuje tato zařízení přímo, ale může [odešle výstrahu, když metriky překračuje prahovou hodnotu](app-insights-alerts.md).

Výstrahy lze nastavit u [vlastní metriky](app-insights-api-custom-events-metrics.md#trackmetric), i když není vlastních událostí. Napište kód, který zvýšit metriky, když dojde k události:

    telemetry.TrackMetric("Alarm", 10);

nebo:

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

Vzhledem k tomu, že výstrahy dvou stavů, budete muset odeslat nízkou hodnotu, při zvažování výstrahu, kterou chcete být ukončeny:

    telemetry.TrackMetric("Alarm", 0.5);

Vytvoření grafu v [metriky explorer](app-insights-metrics-explorer.md) zobrazíte vaší výstrahy:

![](./media/app-insights-how-do-i/010-alarm.png)

Nyní nastavte výstrahu má provést při metriku překročí hodnotu mid na krátkou dobu:

![](./media/app-insights-how-do-i/020-threshold.png)

Průměrný interval nastavte na minimum.

E-mailů získáte, když překročí metriku i pod prahovou hodnotou.

Některé body vzít v úvahu:

* Výstraha má dva stavy ("upozornění" a "v pořádku"). Stav se vyhodnocují jenom v případě, že je obdržena metriky.
* E-mail je odeslán, pouze pokud se stav změní. Toto je proč budete muset odeslat obě vysoké a nízké hodnoty metriky.
* Abyste mohli vyhodnotit výstrahy, průměr prováděné přijaté hodnoty předchozí období. Aby byla odeslána e-mailů častěji, než nastavený interval proběhne pokaždé, když byl přijat metriky.
* Vzhledem k tomu, že se odesílají e-mailů, "upozornění" a "v pořádku", můžete znovu myslím jednorázové událost jako podmínku dvou stavů. Například místo události "úloha dokončena" máte podmínku "úloha v průběhu", kde získat e-mailů při spuštění a ukončení úlohy.

### <a name="set-up-alerts-automatically"></a>Nastavit výstrahy automaticky
[Pomocí prostředí PowerShell vytvořit nové výstrahy](app-insights-alerts.md#automation)

## <a name="use-powershell-to-manage-application-insights"></a>Pomocí prostředí PowerShell pro správu služby Application Insights
* [Vytvoření nové prostředky](app-insights-powershell-script-create-resource.md)
* [Vytvoření nové výstrahy](app-insights-alerts.md#automation)

## <a name="separate-telemetry-from-different-versions"></a>Samostatné telemetrická data z různých verzí

* Více rolí v aplikaci: pomocí jednoho prostředku Application Insights a filtrovat cloud_Rolename. [Další informace](app-insights-monitor-multi-role-apps.md)
* Oddělení vývoj, testování a verze: použití různých prostředků Application Insights. Vyberte si klíčů instrumentace ze souboru web.config. [Další informace](app-insights-separate-resources.md)
* Vytváření sestav sestavení verze: Přidání vlastnosti pomocí inicializátoru telemetrie. [Další informace](app-insights-separate-resources.md)

## <a name="monitor-backend-servers-and-desktop-apps"></a>Monitorování back-end serverů a aplikace klasické pracovní plochy
[Použít modul Windows Server SDK](app-insights-windows-desktop.md).

## <a name="visualize-data"></a>Vizualizaci dat
#### <a name="dashboard-with-metrics-from-multiple-apps"></a>Řídicí panel se metriky z více aplikací
* V [Explorer metrika](app-insights-metrics-explorer.md), graf přizpůsobit a uložit jako oblíbenou položku. Připnete na řídicí panel Azure.

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a>Řídicí panel se data z jiných zdrojů a Application Insights
* [Export telemetrie do Power BI](app-insights-export-power-bi.md).

Nebo

* Používání služby SharePoint jako řídicí panel, zobrazení dat ve webové části služby SharePoint. [Použít průběžné export a Stream Analytics exportovat do SQL](app-insights-code-sample-export-sql-stream-analytics.md).  Kontrola databáze pomocí PowerView a vytvoření webové části služby SharePoint pro PowerView.

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a>Filtrovat anonymní nebo ověřeného uživatele
Pokud vaši uživatelé přihlásí, můžete nastavit [ověřit id uživatele](app-insights-api-custom-events-metrics.md#authenticated-users). (Ho nedojde automaticky.)

Pak můžete:

* Hledání ID konkrétního uživatele

![](./media/app-insights-how-do-i/110-search.png)

* Filtr metriky pro anonymní nebo ověřeného uživatele

![](./media/app-insights-how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a>Upravit názvy vlastností nebo hodnot
Vytvoření [filtru](app-insights-api-filtering-sampling.md#filtering). Díky tomu můžete upravit nebo filtrování telemetrie před odesláním z vaší aplikace do služby Application Insights.

## <a name="list-specific-users-and-their-usage"></a>Seznam konkrétních uživatelů a jejich využití
Pokud chcete [vyhledávání pro konkrétní uživatele](#search-specific-users), můžete nastavit [ověřit id uživatele](app-insights-api-custom-events-metrics.md#authenticated-users).

Pokud chcete seznam uživatelů s daty, jako je například jaké stránky se podívejte se na nebo jak často se přihlásit, máte dvě možnosti:

* [Id sady ověřený uživatel](app-insights-api-custom-events-metrics.md#authenticated-users), [exportovat do databáze](app-insights-code-sample-export-sql-stream-analytics.md) a použití nástrojů vhodný k analýze dat uživatele existuje.
* Pokud máte pouze malý počet uživatelů, odesílat vlastní události nebo metriky, používají data týkající se jako název metriky hodnotu nebo událost a nastavení jako vlastnost id uživatele. Chcete-li analyzovat zobrazení stránky, nahraďte standardní trackPageView volání jazyka JavaScript. K analýze telemetrických dat na straně serveru, pomocí inicializátoru telemetrie přidejte id uživatele pro všechny telemetrii serveru. Pak můžete filtrovat a segment metriky a hledání na id uživatele.

## <a name="reduce-traffic-from-my-app-to-application-insights"></a>Omezit přenos z mé aplikace do služby Application Insights
* V [souboru ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), zakažte všechny moduly, nepotřebujete, takové do kolekce pro čítač výkonu.
* Použití [vzorkování a filtrování](app-insights-api-filtering-sampling.md) v sadě SDK.
* Na webových stránkách omezte počet volání Ajax hlášených pro každé stránky zobrazení. V tomto fragmentu kódu skriptu po `instrumentationKey:...` , vložte: `,maxAjaxCallsPerView:3` (nebo vhodný číslo).
* Pokud používáte [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), výpočetní agregace dávek hodnot metriky před odesláním výsledek. Přetížení TrackMetric(), která poskytuje pro tento není k dispozici.

Další informace o [ceny a kvóty](app-insights-pricing.md).

## <a name="disable-telemetry"></a>Zakázat telemetrii
K **dynamicky zastavit a spustit** shromažďování a předávání telemetrie ze serveru:

```

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```



K **zakázat vybrané standardní Kolektory** – například čítače výkonu, požadavky HTTP nebo závislosti – odstranit nebo komentář příslušné řádky v [souboru ApplicationInsights.config](app-insights-api-custom-events-metrics.md). Můžete tak učinit, například pokud chcete odeslat vlastní TrackRequest data.

## <a name="view-system-performance-counters"></a>Čítače výkonu systému zobrazení
Mezi metriky, které můžete zobrazit v Průzkumníku metrik jsou sady systému čítače výkonu. Je předdefinovaný okno s názvem **servery** který zobrazí několik z nich.

![Otevřete prostředek Application Insights a klikněte na servery](./media/app-insights-how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a>Pokud se zobrazí žádná data čítače výkonu
* **Server služby IIS** vlastní počítače nebo na virtuálním počítači. [Nainstalujte monitorování stavu](app-insights-monitor-performance-live-website-now.md).
* **Webu Azure** -ještě nepodporujeme čítače výkonu. Existuje několik metriky, které můžete získat jako standardní součást ovládacího panelu webu Azure.
* **UNIX server** - [nainstalujte collectd](app-insights-java-collectd.md)

### <a name="to-display-more-performance-counters"></a>Chcete-li zobrazit další čítače výkonu
* První, [přidejte nový graf](app-insights-metrics-explorer.md) a zjistit, jestli čítač základní sady, které nabízíme.
* Pokud ne, [přidat čítač do sady shromažďují modul čítače výkonu](app-insights-performance-counters.md).
