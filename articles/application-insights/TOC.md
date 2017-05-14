# Přehled
## [Co je Application Insights?](app-insights-overview.md)
## [Sledování výkonu v cyklu vývoje a provozu](app-insights-detect-triage-diagnose.md)

# Začínáme
## Monitorování Azure
### [Azure Web Apps](app-insights-azure-web-apps.md)
### [Azure Cloud Services](app-insights-cloudservices.md)

## Monitorování aplikací v ASP.NET
### [Web Apps](app-insights-asp-net.md)
### [Již spuštěné webové aplikace](app-insights-monitor-performance-live-website-now.md)
### [Služby Windows](app-insights-windows-services.md)
### [Plocha Windows](app-insights-windows-desktop.md)

## Monitorování aplikací v Javě
### [Web Apps](app-insights-java-get-started.md)
### [Web Apps – běhové prostředí](app-insights-java-live.md)
### [Aplikace v Dockeru](app-insights-docker.md)

## Monitorování aplikací Node.js
### [Node.js](app-insights-nodejs.md)

## Monitorování webových stránek
### [JavaScript](app-insights-javascript.md)
 
## Monitorování jiných platforem
### [Weby SharePoint](app-insights-sharepoint.md)
### [Další platformy](app-insights-platforms.md)


# Postup
## Plánování a návrh
### [Hloubková diagnostika webových aplikací a služeb](app-insights-devops.md)
### [Sledování výkonu webových aplikací](app-insights-web-monitor-performance.md)
### [Oddělení prostředků Application Insights](app-insights-separate-resources.md)
### [Jak mám... pomocí Application Insights?](app-insights-how-do-i.md)


## Konfigurace
### Azure
#### [Profiler](app-insights-profiler.md)
#### [Diagnostika](app-insights-azure-diagnostics.md)

### ASP.NET
#### [Získání další telemetrie](app-insights-asp-net-more.md)
#### [Výjimky](app-insights-asp-net-exceptions.md)
#### [Trasování protokolů](app-insights-asp-net-trace-logs.md)
#### [Čítače výkonu](app-insights-performance-counters.md)
#### [Závislosti](app-insights-asp-net-dependencies.md)
#### [Poznámky k verzi](app-insights-annotations.md)
#### [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md)
#### [Ladicí program snímků](app-insights-snapshot-debugger.md)

### J2EE
#### [Trasování protokolů](app-insights-java-trace-logs.md)
#### [Unixové metriky](app-insights-java-collectd.md)
#### [Závislosti](app-insights-java-agent.md)
#### [Filtrování telemetrických dat](app-insights-java-filter-telemetry.md)

### Výstrahy

#### [Dostupnost](app-insights-monitor-web-app-availability.md)
#### [Metriky a výstrahy](app-insights-alerts.md)

### [Inteligentní detekce](app-insights-proactive-diagnostics.md)
#### [Anomálie selhání](app-insights-proactive-failure-diagnostics.md)
#### [Anomálie výkonu](app-insights-proactive-performance-diagnostics.md)

### [Vytvoření prostředku](app-insights-create-new-resource.md)

## Analýza

### Portál Application Insights

#### [Řídicí panely](app-insights-dashboards.md)
#### [Search](app-insights-diagnostic-search.md)
#### [Metriky](app-insights-metrics-explorer.md)
#### [Živý datový proud](app-insights-live-stream.md)

#### [Mapa aplikace](app-insights-app-map.md)
#### [Aplikace HockeyApp](app-insights-hockeyapp-bridge-app.md)

### Visual Studio

#### [F5 insights](app-insights-visual-studio.md)
#### [Trendy](app-insights-visual-studio-trends.md)
#### [CodeLens](app-insights-visual-studio-codelens.md)

### Využití
#### [Přehled](app-insights-usage-overview.md)
#### [Uchování](app-insights-usage-retention.md)
#### [Uživatelé, relace, události](app-insights-usage-segmentation.md)

### Analýza

#### [Přehled](app-insights-analytics.md)
#### [Prohlídka](app-insights-analytics-tour.md)
#### [Použití](app-insights-analytics-using.md)
#### [Import](app-insights-analytics-import.md)
#### [Diagnostika](app-insights-analytics-diagnostics.md)

## Automatizace

### [Konfigurace PowerShell](app-insights-powershell.md)
### [Vytvoření prostředků](app-insights-powershell-script-create-resource.md)
### [Nastavení výstrah](app-insights-powershell-alerts.md)
### [Získání diagnostiky Azure](app-insights-powershell-azure-diagnostics.md)

## Vývoj

### [Rozhraní API pro vlastní události a metriky](app-insights-api-custom-events-metrics.md)
### [Filtrování a předběžné zpracování telemetrie](app-insights-api-filtering-sampling.md)
### [Vzorkování](app-insights-sampling.md)
### [Jádro ASP.NET](app-insights-asp-net-core.md)

## Správa
### [Správa cen a kvót](app-insights-pricing.md)
### [Sledování výkonu aplikací pomocí Application Insights pro SCOM](app-insights-scom.md)

## Export
### [Průběžný export](app-insights-export-telemetry.md)
### [Export datového modelu](app-insights-export-data-model.md)
### [Export do Power BI](app-insights-export-power-bi.md)

## Zabezpečení
### [Shromažďování, uchovávání a ukládání dat](app-insights-data-retention-privacy.md)
### [Prostředky, role a řízení přístupu](app-insights-resources-roles-access-control.md)
### [IP adresy](app-insights-ip-addresses.md)

## Řešení potíží
### [Žádná data v .NET](app-insights-asp-net-troubleshoot-no-data.md)
### [Analýzy](app-insights-analytics-troubleshooting.md)
### [Java](app-insights-java-troubleshoot.md)

# Referenční informace
## [Analýzy](app-insights-analytics-reference.md)
## [.NET](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights)
## [Java](/java/api/com.microsoft.applicationinsights)
## [JavaScript](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)
## [REST](https://dev.applicationinsights.io/)
## [Datový model](application-insights-data-model.md)
### [Požadavek](application-insights-data-model-request-telemetry.md)
### [Závislost](application-insights-data-model-dependency-telemetry.md)
### [Výjimka](application-insights-data-model-exception-telemetry.md)
### [Trasování](application-insights-data-model-trace-telemetry.md)
### [Události](application-insights-data-model-event-telemetry.md)
### [Metrika](application-insights-data-model-metric-telemetry.md)
## [Korelace telemetrie](application-insights-correlation.md)

# Zdroje

## [Jazyky a platformy](app-insights-platforms.md)

## [Ceny](https://azure.microsoft.com/pricing/details/application-insights/)  

## Novinky

### [Blog](https://azure.microsoft.com/blog/tag/application-insights/)
### [Aktualizace služeb](https://azure.microsoft.com/updates/?product=application-insights) 
### [Poznámky k verzi sady SDK](app-insights-release-notes.md)
### [Poznámky k verzi pro Developer Analytics Tools](app-insights-release-notes-vsix.md)

## [Nejčastější dotazy](app-insights-troubleshoot-faq.md)

## Nápověda
### [Fórum MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=ApplicationInsights)  
### [Stack Overflow](http://stackoverflow.com/questions/tagged/az-application-insights)
### [User Voice](https://visualstudio.uservoice.com/forums/357324-application-insights)
### [Podpora](https://azure.microsoft.com/support/)

## [Videa](https://azure.microsoft.com/documentation/videos/index/?services=application-insights)


