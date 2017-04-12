---
title: 'Application Insights: jazyky, platformy a integrace | Dokumentace Microsoftu'
description: "Jazyky, platformy a integrace, které jsou k dispozici pro službu Application Insights"
services: application-insights
documentationcenter: 
author: OlegAnaniev-MSFT
manager: douge
ms.assetid: 974db106-54ff-4318-9f8b-f7b3a869e536
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 22adefb1d506376e72bf27e295e009b1cf56d630
ms.lasthandoff: 04/12/2017


---
# <a name="developer-analytics-languages-platforms-and-integrations"></a>Developer Analytics: jazyky, platformy a integrace
Tyto položky jsou implementace [Application Insights](app-insights-overview.md), o kterých jsme se dozvěděli. Zahrnují také některé, které dodávají třetí strany.

## <a name="languages"></a>Jazyky
* [C#|VB (.NET)](app-insights-asp-net.md)
* [Java](app-insights-java-get-started.md)
* [Webové stránky JavaScript](app-insights-web-track-usage.md)
* [Objective-C](https://github.com/Microsoft/ApplicationInsights-iOS)
* [PHP](https://github.com/Microsoft/ApplicationInsights-PHP)
* [Python](https://pypi.python.org/pypi/applicationinsights/0.1.0)
* [Ruby](https://rubygems.org/gems/application_insights)
* [Cokoliv jiného](#projects)

## <a name="platforms-and-frameworks"></a>Platformy a rozhraní
* [Angular](https://www.npmjs.com/package/angular-applicationinsights)
* [ASP.NET](app-insights-asp-net.md)
* [ASP.NET – pro aplikace, které jsou již nasazeny](app-insights-monitor-performance-live-website-now.md)
* [Jádro ASP.NET](app-insights-asp-net-core.md)
* [Android](https://github.com/Microsoft/ApplicationInsights-Android) (HockeyApp)
* [Azure Web Apps](app-insights-azure-web-apps.md)
* [Azure Cloud Services](app-insights-cloudservices.md)&#151; včetně webových a pracovních rolí
* [Docker](app-insights-docker.md)
* [Glimpse](https://azure.microsoft.com/blog/glimpse-application-insights/)
* [iOS](https://github.com/Microsoft/ApplicationInsights-iOS) (HockeyApp)
* [J2EE](app-insights-java-get-started.md)
* [J2EE – pro aplikace, které jsou již nasazeny](app-insights-java-live.md)
* [Aplikace systému Mac OS X](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-mac-os-x) (HockeyApp)
* [Node.js](https://www.npmjs.com/package/applicationinsights)
* [OSX](https://github.com/Microsoft/ApplicationInsights-OSX)
* [Spring](http://joe.blog.freemansoft.com/2015/12/enabling-microsoft-application-insight.html)
* [Univerzální aplikace pro Windows](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/how-to-create-an-app-for-uwp) (HockeyApp)
* [WCF](https://github.com/Microsoft/ApplicationInsights-SDK-Labs/blob/master/WCF/readme.md)
* [Aplikace pro Windows Phone 8 a 8.1](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-phone-silverlight-apps-80-and-81) (HockeyApp)
* [Aplikace pro grafický subsystém Windows Presentation Foundation](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-wpf-apps) (HockeyApp)
* [Aplikace pracovní plochy Windows, služby a pracovní role](app-insights-windows-desktop.md)
* [Cokoliv jiného](#projects)

## <a name="logging-frameworks"></a>Protokolování rozhraní
* [Log4Net, NLog nebo System.Diagnostics.Trace](app-insights-diagnostic-search.md)
* [Java, Log4J nebo Logback](app-insights-java-trace-logs.md)
* [Sémantické protokolování (SLAB)](https://github.com/fidmor89/SLAB_AppInsights) – integruje se [sémantickým blokem protokolování aplikace](https://msdn.microsoft.com/library/dn440729.aspx)
* [Cloudové zátěžové testování](http://blogs.msdn.com/b/visualstudioalm/archive/2015/07/30/getting-application-insights-counters-with-cloud-based-load-testing.aspx)
* [Modul plug-in pro LogStash](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [OMS Log Analytics](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)

## <a name="content-management-systems"></a>Systémy správy obsahu
* [Concrete](https://github.com/fidmor89/appInsights-Concrete)
* [Drupal](https://github.com/fidmor89/AppInsights-Drupal)
* [Joomla](https://github.com/fidmor89/AppInsights-Joomla)
* [Orchard](https://orchardazureappinsights.codeplex.com)
* [SharePoint](app-insights-sharepoint.md)
* [WordPress](https://wordpress.org/plugins/application-insights/)

## <a name="export-and-data-analysis"></a>Export a analýza dat
* [Alooma](https://www.alooma.com/blog/application-insights-amazon-redshift)
* [Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx)
* [Stream Analytics](app-insights-export-power-bi.md)

## <a name="projects"></a> Sestavte si vlastní sadu SDK
Pokud ještě není SDK k dispozici pro váš jazyk nebo platformu, možná si chcete jednu vytvořit? Prohlédněte si kód existující sady SDK, které jsou uvedeny v [Application Insights SDK projektu na Githubu](https://github.com/Microsoft/AppInsights-Home).

