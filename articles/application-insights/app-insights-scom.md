---
title: Integrace SCOM s Application Insights | Microsoft Docs
description: "Pokud si uživatelé SCOM, sledovat výkon a diagnostikovat problémy s nástrojem Application Insights. Komplexní řídicí panely, inteligentní výstrahy, výkonné diagnostické nástroje a analýzy dotazy."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 606e9d03-c0e6-4a77-80e8-61b75efacde0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/12/2016
ms.author: bwren
ms.openlocfilehash: 9c205465981fabdbb696cdc44f765532bbb992b5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="application-performance-monitoring-using-application-insights-for-scom"></a>Sledování výkonu aplikací pomocí Application Insights pro SCOM
Pokud používáte System Center Operations Manager (SCOM) ke správě vašich serverů, můžete sledovat výkon a diagnostikovat problémy s výkonem za pomoci [Azure Application Insights](app-insights-asp-net.md). Application Insights monitoruje webové aplikace příchozí požadavky a odchozí REST a volání SQL, výjimkami a protokolu trasování. Poskytuje řídicí panely pomocí metriky grafů a inteligentní výstrahy, a také výkonné diagnostické vyhledávání a analytické dotazy přes tuto telemetrii. 

Můžete přepnout na Application Insights monitorování pomocí sady management pack SCOM.

## <a name="before-you-start"></a>Než začnete
Předpokládáme:

* Seznamte se s SCOM a že používáte ke správě vašeho IIS SCOM 2012 R2 nebo 2016 webových serverů.
* Již jste nainstalovali na serverech webové aplikace, které chcete monitorovat pomocí nástroje Application Insights.
* Je aplikace framework verze rozhraní .NET 4.5 nebo novější.
* Máte přístup k odběru v [Microsoft Azure](https://azure.com) a můžete přihlásit k [portál Azure](https://portal.azure.com). Vaše organizace může mít odběr a do něj může přidat účet Microsoft.

(Vývojový tým může vytvořit [Application Insights SDK](app-insights-asp-net.md) do webové aplikace. Tento čas sestavení instrumentace je dává větší flexibilitu při psaní vlastní telemetrii. Ale nezáleží: provedením kroků popsaných v tomto poli s nebo bez SDK integrovanou.)

## <a name="one-time-install-application-insights-management-pack"></a>(Jednou) Instalovat sadu management pack Application Insights
Na počítači, kde spuštění nástroje Operations Manager:

1. Odinstalujte všechny předchozí verzi sady management pack:
   1. V nástroji Operations Manager otevřete správy, sad Management Pack. 
   2. Odstraňte staré verze.
2. Stáhněte a nainstalujte sadu management pack z katalogu.
3. Restartujte nástroj Operations Manager.

## <a name="create-a-management-pack"></a>Vytvořit sadu management pack
1. V nástroji Operations Manager, otevřete **vytváření**, **rozhraní .NET... with Application Insights**, **Průvodce přidáním monitorování**a znovu vyberte **rozhraní .NET... s aplikací Statistika**.
   
    ![](./media/app-insights-scom/020.png)
2. Název konfigurace po vaší aplikace. (Máte instrumentace jednu aplikaci najednou.)
   
    ![](./media/app-insights-scom/030.png)
3. Na stejné stránce průvodce vytvořte novou sadu management pack nebo vyberte balíček, který jste předtím vytvořili pro Application Insights.
   
     (Application Insights [sada management pack](https://technet.microsoft.com/library/cc974491.aspx) je šablona, ze kterého můžete vytvořit instanci. Můžete opakovaně použít stejnou instanci později.)

    ![Na kartě Obecné vlastnosti zadejte název aplikace. Klikněte na nový a zadejte název pro sadu management pack. Klikněte na tlačítko OK a potom klikněte na tlačítko Další.](./media/app-insights-scom/040.png)

1. Vyberte jednu aplikaci, kterou chcete monitorovat. Vyhledávací funkce hledá mezi aplikace nainstalované na serverech.
   
    ![K tomu, co k monitorování klikněte na tlačítko Přidat, zadejte část názvu aplikace, klikněte na tlačítko Hledat, vyberte aplikaci a pak přidat OK.](./media/app-insights-scom/050.png)
   
    Volitelné pole obor monitorování umožňuje určit podmnožinu vašim serverům, pokud nechcete, aby ke sledování aplikace na všech serverech.
2. Na další stránce průvodce musíte nejprve zadejte svoje přihlašovací údaje pro přihlášení do služby Microsoft Azure.
   
    Na této stránce zvolte prostředek Application Insights, kam chcete data telemetrie analyzovat a zobrazení. 
   
   * Pokud aplikace byla nakonfigurována pro službu Application Insights během vývoje, vyberte svůj existující prostředek.
   * Jinak vytvořte nový prostředek s názvem aplikace. Pokud existují další aplikace, které jsou součástí ve stejném systému, umístí je ve stejné skupině prostředků, aby přístup k telemetrii snazší správa.
     
     Toto nastavení můžete později změnit.
     
     ![Na kartě Nastavení Application Insights klikněte na tlačítko "přihlášení" a zadejte přihlašovací údaje účtu Microsoft Azure. Potom vyberte předplatné, skupinu prostředků a prostředků.](./media/app-insights-scom/060.png)
3. Dokončete průvodce.
   
    ![Kliknutí na Vytvořit](./media/app-insights-scom/070.png)

Tento postup opakujte pro každou aplikaci, kterou chcete monitorovat.

Pokud budete potřebovat později změnit nastavení, otevřete znovu vlastnosti monitorování z okna pro vytváření obsahu.

![V vytváření obsahu, vyberte monitorování výkonu aplikací .NET pomocí nástroje Application Insights, vyberte monitoru a klikněte na položku Vlastnosti.](./media/app-insights-scom/080.png)

## <a name="verify-monitoring"></a>Ověřte monitorování
Monitorování nainstalovanou vyhledávání pro vaši aplikaci na každém serveru. Pokud zjistí aplikace a konfiguruje monitorování stavu Application Insights pro sledování aplikace. V případě potřeby nainstaluje nejprve monitorování stavu na serveru.

Můžete ověřit, které instance aplikace má najít:

![V monitorování, otevřete službu Application Insights](./media/app-insights-scom/100.png)

## <a name="view-telemetry-in-application-insights"></a>Zobrazení telemetrie Application Insights
V [portál Azure](https://portal.azure.com), přejděte k prostředku pro vaši aplikaci. Můžete [zobrazili grafy znázorňující telemetrie](app-insights-dashboards.md) z vaší aplikace. (Pokud se ještě zobrazovat na hlavní stránce ještě, klikněte na živý datový proud metriky.)

## <a name="next-steps"></a>Další kroky
* [Nastavení řídicího panelu](app-insights-dashboards.md) sdružujícího nejdůležitější grafy monitorování tato a další aplikace.
* [Další informace o metriky](app-insights-metrics-explorer.md)
* [Nastavení výstrah](app-insights-alerts.md)
* [Diagnostika problémů s výkonem](app-insights-detect-triage-diagnose.md)
* [Efektivní analytické dotazy](app-insights-analytics.md)
* [Testy dostupnosti webu](app-insights-monitor-web-app-availability.md)

