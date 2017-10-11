---
title: "Zkoumání metriky ve službě Azure Application Insights | Microsoft Docs"
description: "Interpretace grafy na metriky Průzkumníka a postup přizpůsobení okna Průzkumníka metriky."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: bwren
ms.openlocfilehash: a13500284caab79bbe221060ccf3d925ffb1fab5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="exploring-metrics-in-application-insights"></a>Zkoumání metriky ve službě Application Insights
Metriky v [Application Insights] [ start] jsou měřené hodnoty a počet událostí, které se odesílají v telemetrie z vaší aplikace. Pomáhají zjistit problémy s výkonem a sledujte trendy ve využití vaší aplikace. Je širokou škálu standardní metriky a můžete také vytvořit vlastní vlastní metriky a události.

Počet metriky a události se zobrazí v grafech agregované hodnoty, jako je například součtů a průměry, počty.

Zde je ukázka sadu grafy:

![](./media/app-insights-metrics-explorer/01-overview.png)

Metriky grafy všude, kde zjistíte v portálu služby Application Insights. Ve většině případů lze přizpůsobit a další grafy můžete přidat do okna. V okně Přehled kliknutím prostřednictvím podrobnější grafy (která mají názvy, například "Servery"), nebo klikněte na tlačítko **Průzkumníku metrik** otevřete nové okno, kde můžete vytvořit vlastní grafy.

## <a name="time-range"></a>Časové rozmezí
Můžete změnit časový rozsah, který je předmětem grafy nebo mřížky na libovolné okno.

![Otevře se okno Přehled vaší aplikace na portálu Azure](./media/app-insights-metrics-explorer/03-range.png)

Pokud očekáváte data, která ještě nebyla zobrazovaly, klikněte na tlačítko Aktualizovat. Grafy sami aktualizace v intervalech, ale jsou delší dobu, větší časových rozsahů intervalů. Může trvat nějakou dobu dat do režimu prostřednictvím kanálu analýzy do grafu.

Pro zvětšení část grafu, přetáhněte nad ním:

![Přetažením přes část grafu.](./media/app-insights-metrics-explorer/12-drag.png)

Klikněte na tlačítko zpět zvětšení pro jeho obnovení.

## <a name="granularity-and-point-values"></a>Hodnoty členitosti a bodu
Najeďte myší na graf k zobrazení hodnot metrik v tomto bodě.

![Najeďte myší na graf](./media/app-insights-metrics-explorer/02-focus.png)

Hodnota metriky na určitém místě je agregován v předchozím intervalu vzorkování.

Interval vzorkování nebo "členitosti" se zobrazí v horní části okna.

![Hlavička okno.](./media/app-insights-metrics-explorer/11-grain.png)

Můžete upravit členitost v okně rozsah čas:

![Hlavička okno.](./media/app-insights-metrics-explorer/grain.png)

K dispozici členitostí závisí na časový rozsah, který jste vybrali. Explicitní členitostí jsou alternativy "automatické" členitost časovém rozmezí.


## <a name="editing-charts-and-grids"></a>Úpravy grafy a mřížky
Chcete-li přidat nový graf do okna:

![V Průzkumníku metrik zvolte přidat graf](./media/app-insights-metrics-explorer/04-add.png)

Vyberte **upravit** u stávajícího nebo nového grafu upravit, se zobrazí:

![Vyberte jeden nebo více metriky](./media/app-insights-metrics-explorer/08-select.png)

Více než jeden metrika můžete zobrazit v grafu, i když existují omezení o kombinacích, které lze zobrazit najednou. Jakmile vyberete jeden metrika, některé jiné jsou zakázány.

Pokud jste programového [vlastní metriky] [ track] do vaší aplikace (volání TrackMetric a TrackEvent) budou uvedené v tomto poli.

## <a name="segment-your-data"></a>Segmentovat vaše data
Rozdělte metriky vlastnost – například k porovnání zobrazení stránky na klientských počítačích s jinými operačními systémy.

Vyberte graf nebo tabulku, přepínač na seskupení a vyberte vlastnosti, které chcete seskupit podle:

![Vyberte seskupení na a pak sadu vyberte vlastnost v Seskupit podle](./media/app-insights-metrics-explorer/15-segment.png)

> [!NOTE]
> Pokud používáte seskupení, zadejte typy oblasti a pruhový graf skládaný zobrazení. Toto je vhodný, kde je metoda agregace Sum. Ale kde typ agregace je průměr, vyberte řádek nebo Mřížka typů zobrazení.
>
>

Pokud jste programového [vlastní metriky] [ track] do vaší aplikace a obsahují hodnoty vlastností, budete moct v seznamu vyberte vlastnost.

Je příliš malá pro segmentovaným data grafu? Nastavte jeho výšku:

![Nastavte posuvník](./media/app-insights-metrics-explorer/18-height.png)

## <a name="aggregation-types"></a>Typy agregace
Legendu na stranu ve výchozím nastavení obvykle zobrazuje agregovaná hodnota období grafu. Pokud je ukazatel myši nad grafu, zobrazuje hodnotu v tomto bodě.

Každý datový bod na graf je agregace hodnot dat přijatých v předchozí interval vzorkování nebo "členitosti". Členitost se zobrazí v horní části okna a se liší podle celkové časová osa grafu.

Metriky lze agregovat různými způsoby:

* **Počet** je počet událostí přijatých v intervalu vzorkování. Používá se pro události, jako jsou žádosti o. Rozdíly v výška grafu označuje rozdíly v rychlost, kdy dochází k události. Ale Všimněte si, že číselná hodnota se změní při změně intervalu vzorkování.
* **Součet** přidá hodnot všech datových bodů přijatých prostřednictvím intervalu vzorkování nebo po dobu grafu.
* **Průměrná** rozdělí součet podle počtu přijatých prostřednictvím interval datových bodů.
* **Jedinečné** počty se používají pro počty uživatelů a účty. Během intervalu vzorkování, nebo za období grafu obrázku počet různých uživatelé vidět v té době.
* **%**-Procento verze každé agregace se používají pouze s segmentovaným grafy. Celkový počet vždy přidá až o 100 % a graf zobrazuje relativní příspěvek různé komponenty celkem.

    ![Procento agregace](./media/app-insights-metrics-explorer/percentage-aggregation.png)

### <a name="change-the-aggregation-type"></a>Změnit typ agregace

![Upravit graf a pak vyberte agregace](./media/app-insights-metrics-explorer/05-aggregation.png)

Když vytvoříte nový graf nebo když jsou všechny metriky není vybraná, je zobrazena výchozí metodou pro jednotlivé metriky:

![Zrušte výběr všechny metriky, které chcete zobrazit výchozí hodnoty](./media/app-insights-metrics-explorer/06-total.png)

## <a name="pin-y-axis"></a>Osy y PIN kódu 
Ve výchozím nastavení graf znázorňuje hodnoty osy Y od nuly do maximální hodnoty v oblasti dat, umožnit vizuální reprezentace quantum hodnot. Ale v některých případech víc než quantum mohou být zajímavé vizuální kontrola malých změn v hodnoty. Přizpůsobení jako tuto použijte rozsahu osy y úpravy funkce připnete osy y minimální nebo maximální hodnotu na požadovaný místě.
Klikněte na zaškrtávací políčko "Upřesnit nastavení" se zprovoznit osy y rozsah nastavení

![Klikněte na tlačítko Upřesnit nastavení, vyberte vlastní rozsah a zadejte maximální hodnoty min.](./media/app-insights-metrics-explorer/y-axis-range.png)

## <a name="filter-your-data"></a>Filtrování dat
Pokud chcete zobrazit jenom metriky pro vybranou sadu hodnot vlastností:

![Klikněte na tlačítko filtru, rozbalte vlastnost a zkontrolujte některé hodnoty](./media/app-insights-metrics-explorer/19-filter.png)

Pokud nevyberete žádné hodnoty pro požadovanou vlastnost, je stejná jako jejich všechny výběrem: pro tuto vlastnost neexistuje žádný filtr.

Všimněte si, počet událostí společně se všechny hodnoty vlastností. Když vyberete hodnoty jednu vlastnost, upraví se počty vedle dalších hodnot vlastností.

Filtry platí pro všechny grafy v okně. Pokud chcete použít na různých grafů různých filtrů, vytvořte a uložte okna jiné metriky. Pokud chcete, můžete připnout grafy z různých okna na řídicí panel, abyste je viděli vedle sebe navzájem.

### <a name="remove-bot-and-web-test-traffic"></a>Odebrat robota a webové přenosy testu
Pomocí filtru **skutečné nebo syntetické přenosů** a zkontrolujte **skutečné**.

Můžete také filtrovat podle **zdroj syntetické provoz**.

### <a name="to-add-properties-to-the-filter-list"></a>Přidání vlastnosti do seznamu filtru
Chcete filtrovat telemetrii na kategorii podle vlastní volby? Například možná zdola nahoru vaši uživatelé do různých kategorií a chcete segmentovat vaše data podle těchto kategorií.

[Vytvořit vlastní vlastnost](app-insights-api-custom-events-metrics.md#properties). Nastavit [Telemetrie inicializátoru](app-insights-api-custom-events-metrics.md#defaults) jej zobrazit v všechny telemetrie – včetně standardní telemetrie poslal jiné moduly SDK.

## <a name="edit-the-chart-type"></a>Upravit typ grafu.
Všimněte si, že můžete přepínat mezi mřížky a grafy:

![Vyberte graf nebo mřížky, a potom vyberte typ grafu](./media/app-insights-metrics-explorer/16-chart-grid.png)

## <a name="save-your-metrics-blade"></a>Uložit okně vaší metriky
Pokud jste vytvořili některé grafy, je uložte jako oblíbenou položku. Můžete zvolit, zda sdílet s ostatními členy týmu, pokud používáte účet organizace.

![Vyberte Oblíbené položky.](./media/app-insights-metrics-explorer/21-favorite-save.png)

Zobrazit okno znovu **přejděte do okna Přehled** a otevřete oblíbených položek:

![V okně Přehled vyberte Oblíbené položky](./media/app-insights-metrics-explorer/22-favorite-get.png)

Pokud jste zvolili relativní časové rozmezí, když jste uložili, v okně bude aktualizována nejnovější metriky. Pokud jste zvolili absolutní časové rozmezí, se zobrazí stejná data pokaždé, když.

## <a name="reset-the-blade"></a>Resetování okna
Pokud upravíte okno, ale pak chcete získat zpět do původního uložit sadu, stačí kliknout na resetovat.

![V tlačítka v horní části metrika Explorer](./media/app-insights-metrics-explorer/17-reset.png)

## <a name="live-metrics-stream"></a>Datový proud za provozu metriky

Zobrazení telemetrie a víc okamžitou otevřete [živý datový proud](app-insights-live-stream.md). Většina metriky trvat několik minut, než se objeví z důvodu proces agregace. Naopak za provozu metriky jsou optimalizované pro s nízkou latencí. 

## <a name="set-alerts"></a>Nastavení upozornění
Mají být informování tímto e-mailu neobvyklou hodnot všechny metriky, přidáte oznámení. Můžete buď k odeslání e-mailu pro účet správce, nebo na konkrétní e-mailové adresy.

![V Průzkumníku metrik zvolte pravidla výstrah, přidejte výstrah](./media/app-insights-metrics-explorer/appinsights-413setMetricAlert.png)

[Další informace o výstrahách][alerts].


## <a name="continuous-export"></a>Souvislý export
Pokud chcete data nepřetržitě exportovali, aby ho mohl zpracovávat externě, zvažte použití [průběžné export](app-insights-export-telemetry.md).

### <a name="power-bi"></a>Power BI
Pokud chcete, aby i bohatší zobrazení dat, můžete [exportovat do Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).

## <a name="analytics"></a>Analýza
[Analýza](app-insights-analytics.md) je rozmanitější způsob, jak analyzovat telemetrie pomocí účinný dotazovací jazyk. Použijte jej, pokud chcete kombinovat výpočetní výsledky z metriky nebo provést podrobné zkoumání poslední výkon vaší aplikace. 

Z metriky grafu můžete kliknutím na ikonu Analytics získat přímo na ekvivalentní Analytics dotazu.

## <a name="troubleshooting"></a>Řešení potíží
*V grafu se nezobrazí žádná data.*

* Filtry platí pro všechny grafy v okně. Ujistěte se, že když se zaměřením na jeden graf, nebyla nastavena filtr, který vylučuje všechna data na jiném.

    Pokud chcete nastavit různých filtrů na různých grafů, vytvořte je v různých oknech, je uložte jako samostatné Oblíbené položky. Pokud chcete, můžete je připnout na řídicí panel, abyste je viděli vedle sebe navzájem.
* Graf při seskupení podle vlastnosti, která není definován v metriku, pak nebude nic na graf. Vymažte "Seskupit podle", nebo zvolte jinou seskupení vlastnost.
* Údaje o výkonu (procesoru, vstupně-výstupní operace rychlost a tak dále) je k dispozici pro webové služby, Java, desktopové aplikace systému Windows, [IIS webové aplikace a služby, pokud nainstalujete monitorování stavu](app-insights-monitor-performance-live-website-now.md), a [Azure Cloud Services](app-insights-azure.md). Není k dispozici pro weby Azure.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Další kroky
* [Sledování použití s nástrojem Application Insights](app-insights-web-track-usage.md)
* [Pomocí vyhledávání diagnostiky](app-insights-diagnostic-search.md)

<!--Link references-->

[alerts]: app-insights-alerts.md
[start]: app-insights-overview.md
[track]: app-insights-api-custom-events-metrics.md
