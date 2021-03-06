---
title: "Diagnostika problémů s výkonem pomocí Azure Application Insights | Microsoft Docs"
description: "Kurz popisující, jak v aplikaci vyhledat a diagnostikovat problémy s výkonem pomocí Azure Application Insights."
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/18/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 26f5acf369dd80d7877ab760806e0e08a49cfe6d
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/13/2018
---
# <a name="find-and-diagnose-performance-issues-with-azure-application-insights"></a>Vyhledání a diagnostika problémů s výkonem pomocí Azure Application Insights

Azure Application Insights shromažďuje telemetrii z vaší aplikace a pomáhá tak analyzovat její provoz a výkon.  Tyto informace můžete využít k identifikaci problémů, ke kterým může docházet, nebo k identifikaci zlepšení aplikace, která by měla největší dopad na uživatele.  Tento kurz vás provede procesem analýzy výkonu serverových komponent vaší aplikace i z pohledu uživatele.  Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Identifikace výkonu operací na straně serveru
> * Analýza operací serveru a určení původní příčiny pomalého výkonu
> * Identifikace nejpomalejších operací na straně klienta
> * Analýza podrobností o zobrazení stránek pomocí dotazovacího jazyka


## <a name="prerequisites"></a>Požadavky

K provedení kroků v tomto kurzu je potřeba:

- Nainstalovat [Visual Studio 2017](https://www.visualstudio.com/downloads/) s následujícími sadami funkcí:
    - Vývoj pro ASP.NET a web
    - Vývoj pro Azure
- Nasadit aplikaci .NET do Azure a [povolit sadu Application Insights SDK](app-insights-asp-net.md).
- [Povolit Application Insights Profiler](app-insights-profiler.md#installation) pro vaši aplikaci.

## <a name="log-in-to-azure"></a>Přihlášení k Azure
Přihlaste se k portálu Azure Portal na adrese [https://portal.azure.com](https://portal.azure.com).

## <a name="identify-slow-server-operations"></a>Identifikace pomalých operací serveru
Application Insights shromažďuje podrobnosti o výkonu různých operací ve vaší aplikaci.  Díky identifikaci operací, které trvají nejdéle, můžete diagnostikovat potenciální problémy nebo lépe cílit probíhající vývoj a vylepšit tak celkový výkon aplikace.

1. Vyberte **Application Insights** a pak vyberte své předplatné.  
1. Panel **Výkon** otevřete výběrem možnosti **Výkon** v nabídce **Prozkoumat** nebo kliknutím na graf **Doba odezvy serveru**.

    ![Výkon](media/app-insights-tutorial-performance/performance.png)

2. Na panelu **Výkon** se zobrazí počet a průměrná doba trvání jednotlivých operací aplikace.  Tyto informace můžete využít k identifikaci operací, které mají největší dopad na uživatele. V tomto příkladu jsou vzhledem k dlouhé době trvání a počtu volání vhodnými kandidáty na prošetření operace **GET Customers/Details** a **GET Home/Index**.  Jiné operace můžou mít delší dobu trvání, ale volají se jen zřídka, takže by jejich vylepšení mělo minimální vliv.  

    ![Panel Výkon](media/app-insights-tutorial-performance/performance-blade.png)

3. Graf aktuálně ukazuje průměrnou dobu trvání všech operací v průběhu času.  Připnutím na graf přidejte operace, které vás zajímají.  Ukazuje se, že by bylo vhodné prošetřit několik vrcholů.  Proveďte další izolaci zúžením časového okna grafu.

    ![Připnutí operací](media/app-insights-tutorial-performance/pin-operations.png)

4.  Kliknutím na operaci zobrazíte na pravé straně její panel Výkon. Ten zobrazuje distribuci doby trvání různých požadavků.  Uživatelé si obvykle všimnou pomalého výkonu po přibližně půl sekundě, takže zužte okno na požadavky trvající déle než 500 milisekund.  

    ![Distribuce doby trvání](media/app-insights-tutorial-performance/duration-distribution.png)

5.  V tomto příkladu vidíte, že zpracování velkého počtu požadavků trvá déle než sekundu. Podrobnosti o této operaci můžete zobrazit kliknutím na **Podrobnosti o operaci**.

    ![Podrobnosti o operaci](media/app-insights-tutorial-performance/operation-details.png)

    > [!NOTE]
    Povolte [prostředí ve verzi Preview](app-insights-previews.md) Unified details: E2E Transaction Diagnostics (Sjednocené podrobnosti: Diagnostika transakcí E2E) a zobrazte veškerou související telemetrii na straně serveru, jako jsou požadavky, závislosti, výjimky, trasování, události atd., v jednom zobrazení na celou obrazovku. 

    S povolenou verzí Preview můžete zobrazit čas strávený ve volání závislostí i případné chyby nebo výjimky v jednotném prostředí. V případě transakcí mezi komponentami vám graf Gantt společně s podoknem podrobností může pomoct rychle diagnostikovat komponentu, závislost nebo výjimku, která je původní příčinou. Můžete rozbalit dolní část a zobrazit časový průběh jakéhokoli trasování nebo událostí shromážděných pro vybranou operaci komponenty. [Další informace o novém prostředí](app-insights-transaction-diagnostics.md)  

    ![Diagnostika transakcí](media/app-insights-tutorial-performance/e2e-transaction-preview.png)


6.  Informace, které jste zatím shromáždili, pouze potvrzují pomalý výkon, ale téměř nijak nepomáhají dostat se k původní příčině.  **Profiler** vám s tím pomůže díky zobrazení skutečného kódu spuštěného pro příslušnou operaci a času, který zabraly jednotlivé kroky. Vzhledem k tomu, že se profiler spouští pravidelně, některé operace nemusejí mít trasování.  V průběhu času by trasování mělo mít více operací.  Pokud chcete pro operaci spustit profiler, klikněte na **Trasování Profileru**.
5.  Trasování zobrazí pro každou operaci jednotlivé události, takže můžete diagnostikovat původní příčinu v průběhu celé operace.  Klikněte na jeden z horních příkladů s nejdelší dobou trvání.
6.  Kliknutím na **Zobrazit kritickou cestu** zvýrazněte konkrétní cestu událostí, které se nejvíce podílejí na celkové době trvání operace.  V tomto příkladu vidíte, že nejpomalejší volání pochází z metody *FabrikamFiberAzureStorage.GetStorageTableData*. Částí, která trvá nejdéle, je metoda *CloudTable.CreateIfNotExist*. Pokud se tento řádek kódu provádí při každém volání funkce, budou se spotřebovávat zbytečná síťová volání a prostředky procesoru. Nejlepším způsobem, jak kód opravit, je vložit tento řádek do některé metody po spuštění, která se provede pouze jednou. 

    ![Podrobnosti v profileru](media/app-insights-tutorial-performance/profiler-details.png)

7.  **Tip k výkonu** v horní části obrazovky podporuje vyhodnocení, že důvodem nadměrné doby trvání je čekání.  Kliknutím na odkaz **čekáním** otevřete dokumentaci k interpretaci různých typů událostí.

    ![Tip k výkonu](media/app-insights-tutorial-performance/performance-tip.png)

8.  Pokud potřebujete další analýzu, můžete kliknout na **Stáhnout trasování .etl** a stáhnout trasování do sady Visual Studio.

## <a name="use-analytics-data-for-server"></a>Použití analytických dat pro server
Application Insights Analytics poskytuje bohatý dotazovací jazyk umožňující analýzu všech dat shromážděných službou Application Insights.  Můžete ho použít k provádění hloubkové analýzy dat o požadavcích a výkonu.

1. Vraťte se na panel s podrobnostmi o operaci a klikněte na tlačítko Analytics.

    ![Tlačítko Analytics](media/app-insights-tutorial-performance/server-analytics-button.png)

2. Na panelu se otevře Application Insights Analytics s dotazem pro každé zobrazení.  Tyto dotazy můžete spustit tak, jak jsou, nebo je upravit podle vlastních potřeb.  První dotaz zobrazí dobu trvání této operace v průběhu času.

    ![Analýzy](media/app-insights-tutorial-performance/server-analytics.png)


## <a name="identify-slow-client-operations"></a>Identifikace pomalých operací klienta
Kromě identifikace procesů serveru, které je potřeba optimalizovat, dokáže Application Insights provádět analýzu i z pohledu klientských prohlížečů.  To vám může pomoct identifikovat potenciální vylepšení komponent klienta a dokonce i identifikovat problémy s různými prohlížeči nebo umístěními.

1. V části **Prozkoumat** vyberte **Prohlížeč** a otevřete souhrn prohlížeče.  Ten poskytuje vizuální souhrn různých telemetrií vaší aplikace z pohledu prohlížeče.

    ![Souhrn prohlížeče](media/app-insights-tutorial-performance/browser-summary.png)

2.  Posuňte se dolů k části **Které moje stránky jsou nejpomalejší?**.  V této části se zobrazí seznam stránek ve vaší aplikaci, jejichž načtení trvalo klientům nejdéle.  Tyto informace můžete použít k určení priority stránek, které mají nejvýraznější dopad na uživatele.
3.  Kliknutím na jednu ze stránek otevřete panel **Zobrazení stránky**.  V tomto příkladu ukazuje stránka **/FabrikamProd** nadměrnou průměrnou dobu trvání.  Panel **Zobrazení stránky** obsahuje podrobnosti o této stránce, včetně rozpisu různých rozsahů doby trvání.

    ![Zobrazení stránky](media/app-insights-tutorial-performance/page-view.png)

4.  Klikněte na nejdelší dobu trvání a prozkoumejte podrobnosti o příslušných požadavcích.  Pak klikněte na jednotlivý požadavek a zobrazte podrobnosti o klientovi požadujícím příslušnou stránku, včetně typu prohlížeče a jeho umístění.  Tyto informace vám můžou pomoct s určením, jestli neexistují problémy s výkonem souvisejíc s konkrétními typy klientů.

    ![Podrobnosti o požadavku](media/app-insights-tutorial-performance/request-details.png)

## <a name="use-analytics-data-for-client"></a>Použití analytických dat pro klienta
Propojením shromážděných dat o výkonu serveru zpřístupňuje Application Insights veškerá data klientů pro hloubkovou analýzu pomocí Analytics.

1. Vraťte se do souhrnu prohlížeče a klikněte na ikonu Analytics.

    ![Ikona Analytics](media/app-insights-tutorial-performance/client-analytics-icon.png)

2. Na panelu se otevře Application Insights Analytics s dotazem pro každé zobrazení. První dotaz zobrazí dobu trvání různých zobrazení stránek v průběhu času.

    ![Analýzy](media/app-insights-tutorial-performance/client-analytics.png)

3.  Inteligentní diagnostika je funkce Application Insights Analytics, která v datech identifikuje jedinečné vzory.  Když kliknete na tečku inteligentní diagnostiky ve spojnicovém grafu, spustí se stejný dotaz bez záznamů, které anomálii způsobily.  Podrobnosti o těchto záznamech se zobrazí v části dotazu s komentáři, takže můžete identifikovat vlastnosti zobrazení stránek, které způsobují nadměrnou dobu trvání.

    ![Inteligentní diagnostika](media/app-insights-tutorial-performance/client-smart-diagnostics.png)


## <a name="next-steps"></a>Další kroky
Nyní, když jste se naučili, jak identifikovat výjimky za běhu, přejděte k dalšímu kurzu, ve kterém zjistíte, jak jako reakci na chyby vytvářet upozornění.

> [!div class="nextstepaction"]
> [Upozornění na stav aplikace](app-insights-tutorial-alert.md)
