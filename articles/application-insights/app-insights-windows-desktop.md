---
title: "Monitorování využití a výkonu desktopových aplikací pro Windows"
description: "Analyzujte využití a výkon vaší desktopové aplikace Windows pomocí HockeyApp a Application Insights."
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 19040746-3315-47e7-8c60-4b3000d2ddc4
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/26/2016
ms.author: mbullwin
ms.openlocfilehash: 1d80796073c0e85bb3475f9182c79ad2129ace8b
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>Monitorování využití a výkonu desktopových aplikací pro Windows


[Azure Application Insights](app-insights-overview.md) a [HockeyApp](https://hockeyapp.net) umožňují monitorovat využití a výkon nasazené aplikace.

> [!IMPORTANT]
> Doporučujeme, abyste k distribuci a monitorování desktopových aplikací a aplikací zařízení používali [HockeyApp](https://hockeyapp.net). S HockeyApp může spravovat distribuci, testování za provozu a zpětnou vazbu od uživatelů, a zároveň monitorovat sestavy využití a chyb. Můžete také [exportovat telemetrii a dávat na ni dotazy pomocí Analytics](app-insights-hockeyapp-bridge-app.md).
> 
> I když telemetrii lze odeslat do Application Insights z desktopové aplikace, hodí se hlavně pro ladění a experimentální účely.
> 
> 

## <a name="to-send-telemetry-to-application-insights-from-a-windows-application"></a>Odeslání telemetrie do Application Insights z aplikace Windows
1. Na webu [Azure Portal](https://portal.azure.com) [vytvořte prostředek Application Insights](app-insights-create-new-resource.md). Jako typ aplikace vyberte aplikaci ASP.NET.
2. Zkopírujte klíč instrumentace. Klíč najdete v rozevírací nabídce Základy nového prostředku, který jste právě vytvořili. 
3. V sadě Visual Studio upravte balíčky NuGet projektu aplikace a přidejte Microsoft.ApplicationInsights.WindowsServer. (Nebo zvolte Microsoft.ApplicationInsights, pokud chcete prosté rozhraní API bez standardních modulů kolekce telemetrie.)
4. Nastavte klíč instrumentace, buď ve vašem kódu:
   
    `TelemetryConfiguration.Active.InstrumentationKey = "`*váš klíč*`";` 
   
    nebo v souboru ApplicationInsights.config (pokud jste nainstalovali jeden ze standardních balíčků telemetrie):
   
    `<InstrumentationKey>`*váš klíč*`</InstrumentationKey>` 
   
    Pokud používáte soubor ApplicationInsights.config, ujistěte se, že jsou jeho vlastnosti v Průzkumníku řešení nastavené na: **Build Action = Content, Copy to Output Directory = Copy**.
5. [Použijte rozhraní API](app-insights-api-custom-events-metrics.md) k odesílání telemetrie.
6. Spusťte aplikaci a zobrazte telemetrii v prostředku, který jste vytvořili na webu Azure Portal.

## <a name="telemetry"></a>Příklad kódu
```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Další kroky
* [Vytvoření řídicího panelu](app-insights-dashboards.md)
* [Diagnostické vyhledávání](app-insights-diagnostic-search.md)
* [Zkoumání metrik](app-insights-metrics-explorer.md)
* [Psaní analytických dotazů](app-insights-analytics.md)

