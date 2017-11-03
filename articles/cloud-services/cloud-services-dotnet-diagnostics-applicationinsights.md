---
title: "Řešení potíží s Cloud Services pomocí Application Insights | Microsoft Docs"
description: "Informace o řešení problémů služby cloudu pomocí Application Insights pro zpracování dat z Azure Diagnostics."
services: cloud-services
documentationcenter: .net
author: sbtron
manager: timlt
editor: tysonn
ms.assetid: e93f387b-ef29-4731-ae41-0676722accb6
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: saurabh
ms.openlocfilehash: 4001ca908ff00b1a40829d687589080e9b07b18a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-cloud-services-using-application-insights"></a>Řešení potíží s Cloud Services pomocí Application Insights
S [Azure SDK 2.8](https://azure.microsoft.com/downloads/) a rozšíření Azure diagnostiky 1.5, bude možné odesílat data diagnostiky Azure pro cloudové služby přímo do služby Application Insights. Shromažďují v protokolech Azure Diagnostics&mdash;včetně protokoly aplikací, protokoly událostí systému Windows, protokoly trasování událostí pro Windows a čítače výkonu&mdash;lze odeslat buď do Application Insights. Potom můžete vizualizovat tyto informace na portálu služby Application Insights uživatelského rozhraní. Application Insights SDK pak můžete získat přehled o metriky a protokoly, které pocházejí z vaší aplikace, a také systému a data na úrovni infrastruktury, který přichází z Azure Diagnostics.

## <a name="configure-azure-diagnostics-to-send-data-to-application-insights"></a>Konfigurace Azure Diagnostics k odesílání dat do služby Application Insights
Postupujte podle těchto kroků nastavte projekt cloudové služby k odesílání dat Azure Diagnostics do Application Insights.

1. V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na roli a vyberte **vlastnosti** otevřete návrháře Role.

    ![Vlastnosti rolí Průzkumník řešení][1]

2. V **diagnostiky** části návrháře Role, vyberte **poslat diagnostická data do služby Application Insights** možnost.

    ![Návrhář role odesílat data diagnostiky application insights][2]

3. V dialogovém okně, které se zobrazí vyberte prostředek Application Insights, který chcete odesílat data Azure diagnostics. Dialogové okno můžete vybrat existující prostředek Application Insights ze svého předplatného, a také ručně zadat kód instrumentace pro prostředek Application Insights. Další informace o vytváření prostředek Application Insights najdete v tématu [vytvořte nový prostředek Application Insights](../application-insights/app-insights-create-new-resource.md).

    ![Vyberte prostředek application insights][3]

    Po přidání prostředku Application Insights, klíč instrumentace pro tento prostředek je uloženo jako nastavení konfigurace služby s názvem **APPINSIGHTS_INSTRUMENTATIONKEY**. Můžete změnit tato nastavení konfigurace pro každou konfiguraci služby nebo prostředí. To pokud chcete udělat, vyberte jinou konfiguraci z **konfigurace služby** seznamu a zadejte nový klíč instrumentace pro danou konfiguraci.

    ![Vyberte konfiguraci služby][4]

    **APPINSIGHTS_INSTRUMENTATIONKEY** nastavení konfigurace Visual Studio slouží ke konfiguraci rozšíření diagnostiky pomocí příslušné informace o prostředcích Application Insights během publikování. Nastavení konfigurace je pohodlný způsob definování různých instrumentace klíčů pro různé služby konfigurace. Visual Studio se převede tohoto nastavení a vložit do veřejné konfigurace rozšíření diagnostiky během procesu publikování. Ke zjednodušení procesu konfigurace rozšíření diagnostiky pomocí prostředí PowerShell, balíček výstup ze sady Visual Studio také obsahuje veřejné konfigurace XML se příslušný klíč instrumentace Application Insights. Veřejné konfigurační soubory jsou vytvořeny ve složce rozšíření a postupujte podle vzoru *PaaSDiagnostics.&lt; RoleName&gt;. PubConfig.xml*. Všechna nasazení pomocí prostředí PowerShell můžete použít tento vzor pro mapování každé konfiguraci k roli.

4) Chcete-li nakonfigurovat Azure diagnostics odeslat všechny protokoly úroveň chyb shromážděné agentem Azure diagnostics Application Insights a čítače výkonu, povolte **poslat diagnostická data do služby Application Insights** možnost. 

    Pokud chcete provést další konfiguraci jaká data se budou odesílat do služby Application Insights, je nutné ručně upravit *diagnostics.wadcfgx* soubor pro každou roli. V tématu [konfigurace Azure Diagnostics k odesílání dat do služby Application Insights](#configure-azure-diagnostics-to-send-data-to-application-insights) Další informace o ruční aktualizaci konfigurace.

Pokud cloudové služby je nakonfigurovaná pro odesílání dat Azure diagnostics application insights, můžete nasadit ho do Azure normálně, ujistěte se, že je povolené rozšíření Azure diagnostics. Další informace najdete v tématu [publikování cloudové služby pomocí sady Visual Studio](../vs-azure-tools-publishing-a-cloud-service.md).  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>Zobrazení dat Azure diagnostics ve službě Application Insights
Azure diagnostiky telemetrii se zobrazí v prostředku Application Insights nakonfigurovaný pro cloudové služby.

Azure diagnostics protokolu typy mapy na Application Insights koncepty těmito způsoby:

* Čítače výkonu zobrazují jako vlastní metriky ve službě Application Insights.
* Protokoly událostí systému Windows se zobrazí jako trasování a vlastní události ve službě Application Insights.
* Aplikace protokoly, protokoly trasování událostí pro Windows a žádné infrastruktury diagnostické protokoly jsou zobrazeny jako trasování ve službě Application Insights.

Chcete-li zobrazit Azure diagnostická data ve službě Application Insights, proveďte jednu z následujících:

* Použití [Průzkumníku metrik](../application-insights/app-insights-metrics-explorer.md) k vizualizaci všechny vlastní čítače výkonu nebo počet různých typů událostí protokolu událostí systému Windows.

    ![Vlastní metriky v Průzkumníku metrik][5]

* Použití [vyhledávání](../application-insights/app-insights-diagnostic-search.md) hledání mezi protokoly trasování poslal Azure Diagnostics. Například pokud nezpracovanou výjimku způsobila roli a havárií recyklovat, informace o výjimce objeví *aplikace* kanál *protokolu událostí systému Windows*. Hledání můžete vyhledat chyby v protokolu událostí systému Windows a získat trasování úplné zásobníku pro výjimku, které vám pomohou najít příčinu problému.

    ![Hledání trasování][6]

## <a name="next-steps"></a>Další kroky
* [Přidejte Application Insights SDK do cloudové služby](../application-insights/app-insights-cloudservices.md) k odesílání dat o požadavky, výjimky, závislosti a jakékoli vlastní telemetrii z vaší aplikace. V kombinaci s daty Azure Diagnostics, tyto informace můžete získat úplné zobrazení vaší aplikace a systému, ve stejné aplikace přehled prostředků.  

<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
