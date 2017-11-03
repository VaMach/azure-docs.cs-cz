---
title: "Export do služby Power BI ze služby Application Insights | Microsoft Docs"
description: "Analytické dotazy lze zobrazit v Power BI."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 7f13ea66-09dc-450f-b8f9-f40fdad239f2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: mbullwin
ms.openlocfilehash: fe708b14fac971d18d95fd1619907023ec35af89
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="feed-power-bi-from-application-insights"></a>Kanálu Power BI z Application Insights
[Power BI](http://www.powerbi.com/) je sada nástrojů obchodní analýzy, které vám pomohou analyzovat data a sdílet uplatnitelné. Bohaté řídicí panely jsou k dispozici na každé zařízení. Můžete kombinovat data z mnoha zdrojů, včetně analytické dotazy z [Azure Application Insights](app-insights-overview.md).

Existují tři doporučené metody export dat Application Insights do Power BI. Můžete je používat samostatně nebo společně.

* [**Power BI adaptér** ](#power-pi-adapter) -nastavit kompletní řídicí panel telemetrie z vaší aplikace. Předdefinovaná sadu grafy, ale můžete přidat vlastní dotazy z jiných zdrojů.
* [**Export analytické dotazy** ](#export-analytics-queries) -zápisu žádný dotaz chcete pomocí Analytics a exportovat je do Power BI. Tento dotaz můžete umístit na řídicím panelu společně s dalšími daty.
* [**Průběžné exportu a Stream Analytics** ](app-insights-export-stream-analytics.md) – to zahrnuje další práci nastavit. Je užitečné, pokud chcete zachovat data dlouhou dobu. Doporučuje se, jinak hodnota jiné metody.

## <a name="power-bi-adapter"></a>Adaptér Power BI
Tato metoda vytvoří kompletní řídicí panel telemetrie. Předdefinovaná počáteční datovou sadu, ale do něj může přidat další data.

### <a name="get-the-adapter"></a>Získat adaptéru
1. Přihlaste se k [Power BI](https://app.powerbi.com/).
2. Otevřete **získat Data**, **služby**, **Application Insights**
   
    ![Získat ze zdroje dat Application Insights](./media/app-insights-export-power-bi/power-bi-adapter.png)
3. Zadejte podrobnosti o prostředku Application Insights.
   
    ![Získat ze zdroje dat Application Insights](./media/app-insights-export-power-bi/azure-subscription-resource-group-name.png)
4. Počkejte, než minutu nebo dvě pro data, která bude importována.
   
    ![Adaptér Power BI](./media/app-insights-export-power-bi/010.png)

Můžete upravit řídicí panel, kombinace Application Insights grafy s u jiných zdrojů a se analytické dotazy. Je Galerie vizualizace můžete získat více grafů, kde každý graf obsahuje parametry, které můžete zadat.

Po počáteční importu nadále denně aktualizovat řídicí panel a sestavy. Můžete řídit plán aktualizace na datovou sadu.

## <a name="export-analytics-queries"></a>Export analytické dotazy
Tato trasa umožňuje zapsat všechny Analytics dotaz, který chcete a export, na řídicí panel Power BI. (Můžete přidat na řídicí panel vytvořený adaptér.)

### <a name="one-time-install-power-bi-desktop"></a>Jednou: Nainstalujte Power BI Desktop
Chcete-li importovat dotaz Application Insights, použijete verze aplikace Power BI. Ale pak je můžete publikovat na webu nebo do pracovního prostoru cloudu Power BI. 

Nainstalujte [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).

### <a name="export-an-analytics-query"></a>Export dotaz Analytics
1. [Otevřete analýzy a napsat dotaz](app-insights-analytics-tour.md).
2. Testování a upřesnění dotazu, až budete s výsledky spokojeni.

   **Ujistěte se, že dotaz běží správně v Analytics předtím, než ho exportovat.**
3. Na **exportovat** nabídce zvolte **Power BI (M)**. Uložte tento textový soubor.
   
    ![Export dotazu Power BI](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. V Power BI Desktop vyberte **načíst Data, prázdné dotazu** a poté v editoru dotazů v rámci **zobrazení** vyberte **Advanced Editor dotazů**.

    Vložte do editoru dotazů Advanced exportovaný skript jazyka M.

    ![Rozšířený dotaz editoru](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

1. Možná budete muset zadat přihlašovací údaje, aby se mohl Power BI přístup k Azure. Použít 'účet organizace, a přihlaste se pomocí účtu Microsoft.
   
    ![Zadejte přihlašovací údaje Azure umožňující Power BI ke spuštění dotazu Application Insights](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

    (Pokud potřebujete k ověření přihlašovacích údajů, použijte příkaz nabídky nastavení zdroje dat v editoru dotazů. Vezměte v potaz a zadejte přihlašovací údaje, které používáte pro Azure, což může být liší od přihlašovacích údajů pro Power BI.)
2. Zvolte vizualizaci dotazu a vyberte pole pro osy x, y a segmentace dimenze.
   
    ![Vyberte vizualizace](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)
3. Publikujte sestavy do pracovního prostoru cloudu Power BI. Odtud můžete synchronizované verze vložit do jiných webových stránek.
   
    ![Vyberte vizualizace](./media/app-insights-export-power-bi/publish-power-bi.png)
4. Sestavu v intervalech aktualizovat ručně, nebo nastavit plánovaná aktualizace na stránce Možnosti.

## <a name="troubleshooting"></a>Řešení potíží

### <a name="401-or-403-unauthorized"></a>401 nebo 403 Neautorizováno 
To může dojít, pokud zatím není aktualizovaná refesh tokenu. Opakujte tyto kroky, které zajišťují, že máte přístup. Pokud máte přístup a refershing přihlašovací údaje se nedají použít, otevřete prosím lístek podpory.

1. Přihlaste se k portálu Azure a ujistěte se, že má přístup k prostředku
2. Zkuste aktualizovat přihlašovací údaje pro řídicí panel

### <a name="502-bad-gateway"></a>502 Chybná brána
To je obvykle způsobeno dotazu analýzy, který vrátí příliš mnoho dat. Měli zkuste použít menší časový rozsah nebo pomocí [před](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) nebo [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek) funguje pouze [projektu](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator) pole, je nutné.

Pokud snižuje datovou sadu pocházejících z dotazu Analytics není splňují vaše požadavky měli byste zvážit použití [rozhraní API](https://dev.applicationinsights.io/documentation/overview) vyžádat větší datové sady. Tady jsou pokyny, jak převést export M dotazu použít rozhraní API.

1. Vytvoření [klíč rozhraní API](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)
2. Aktualizovat Power BI M skript, který jste exportovali z Analytics nahrazením adresu URL ARM AI rozhraní API (viz následující příklad)
   * Nahraďte **https://management.azure.com/subscriptions/...**
   * s **https://api.applicationinsights.io/beta/apps/...**
3. Nakonec aktualizujte přihlašovací údaje k základní a používání vašeho klíče rozhraní API
  

**Existující skriptu**
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
**Aktualizované skriptu**
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a>O vzorkování
Pokud vaše aplikace odešle velké množství dat, může funkce adaptivního vzorkování pracovat a odesílat pouze procento vaší telemetrie. Totéž platí, pokud jste ručně nastavili vzorkování buď v sadě SDK, nebo na přijímání. [Přečtěte si další informace o vzorkování.](app-insights-sampling.md)


## <a name="next-steps"></a>Další kroky
* [Power BI - Další informace](http://www.powerbi.com/learning/)
* [Analýza kurzu](app-insights-analytics-tour.md)

