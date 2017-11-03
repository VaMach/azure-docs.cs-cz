---
title: "Diagnostika problémů s výkonem pomocí služby Azure Application Insights | Microsoft Docs"
description: "Kurz najít a diagnostikovat problémy s výkonem do vaší aplikace pomocí služby Azure Application Insights."
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/18/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 1176e6ac33db5b9428a323c3a6271818807afc72
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="find-and-diagnose-performance-issues-with-azure-application-insights"></a>Najít a diagnostikovat problémy s výkonem pomocí služby Azure Application Insights

Azure Application Insights shromažďuje telemetrie z vaší aplikace a pomáhá analyzovat jeho operace a výkonu.  Tyto informace můžete použít k identifikaci problémů, které může být vyskytnou nebo k identifikaci vylepšení k aplikaci, která bude většina uživatelů dopad.  Tento kurz vás provede procesem Analýza výkonu součásti serveru vaší aplikace a perspektivy klienta.  Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Identifikovat výkon operací na straně serveru
> * Analýza operace serverů určit hlavní příčinu nízký výkon
> * Určuje nejpomalejší klientské operace
> * Analyzovat podrobnosti o pomocí dotazovacího jazyka pro zobrazení stránky


## <a name="prerequisites"></a>Požadavky

K provedení kroků v tomto kurzu je potřeba:

- Nainstalovat [Visual Studio 2017](https://www.visualstudio.com/downloads/) s následujícími sadami funkcí:
    - Vývoj pro ASP.NET a web
    - Vývoj pro Azure
- Nasazení aplikace .NET do Azure a [povolit Application Insights SDK](app-insights-asp-net.md).
- [Povolit profileru Application Insights](app-insights-profiler.md#enable-the-profiler) pro vaši aplikaci.

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.
Přihlaste se k portálu Azure v [https://portal.azure.com](https://portal.azure.com).

## <a name="identify-slow-server-operations"></a>Určuje operace, pomalé serveru
Application Insights shromažďuje detaily výkonu pro různé operace v aplikaci.  Určením těchto operací s nejdelší dobou trvání můžete diagnostice potenciálních problémů nebo nejlépe cíle vaší pokračujícího vývoje se zlepší celkový výkon aplikace.

1. Vyberte **Application Insights** a potom vyberte své předplatné.  
1. Otevřete **výkonu** panelu vyberte buď **výkonu** pod **prošetření** nabídky nebo klikněte na **doba odezvy serveru** grafu .

    ![Výkon](media/app-insights-tutorial-performance/performance.png)

2. **Výkonu** panel zobrazuje počet a průměrná doba trvání každou operaci pro aplikaci.  Tyto informace můžete použít k identifikaci tyto operace, že většina ovlivnily uživatele. V tomto příkladu **získat zákazníků nebo podrobnosti** a **domovské GET/Index** pravděpodobně kandidáty prozkoumat z důvodu jejich poměrně vysoké doba trvání a počet volání.  Jiné operace může být vyšší, ale měla zřídka volána, takže účinku jejich zlepšování byla minimální.  

    ![Panelu výkon](media/app-insights-tutorial-performance/performance-blade.png)

3. Graf zobrazuje aktuálně Průměrná doba všechny operace v čase.  Přidejte operace, které máte zájem tím, že je připnete do grafu.  Ukazuje to, že jsou některé vrcholů vhodné příčin.  Tato další izolujte snížením časový interval grafu.

    ![Operace PIN kódu](media/app-insights-tutorial-performance/pin-operations.png)

4.  Klikněte na tlačítko operace zobrazíte panelu její výkon na pravé straně. Ukazuje to distribuce doby trvání pro různé požadavky.  Uživatelé obvykle Všimněte si pomalý výkon v o půl sekundy, takže zmenší okno do požadavky přes 500 milisekund.  

    ![Doba trvání distribuce](media/app-insights-tutorial-performance/duration-distribution.png)

5.  V tomto příkladu vidíte, že velký počet požadavků trvá přes druhý ke zpracování. Zobrazí podrobnosti o této operaci kliknutím na **podrobnosti operace**.

    ![Podrobnosti o operaci](media/app-insights-tutorial-performance/operation-details.png)

6.  Informace, které jste shromáždili dosavadní pouze potvrdí, že je nízký výkon, ale dělá málo získat příčiny.  **Profileru** pomáhá s tímto zobrazením skutečný kód, který byl spuštěn pro operaci a čas potřebný pro každý krok. Některé operace, nemusí mít trasování, protože profileru spouštěn pravidelně.  V průběhu času by měl mít dalších operacích trasování.  Chcete-li spustit profileru pro operaci, klikněte na tlačítko **profileru trasování**.
5.  Trasování uvádí jednotlivé události pro každou operaci, můžete diagnostikou hlavní příčiny po dobu trvání celou operaci.  Klikněte na jednu z horní příklady, které mají nejdelší dobu trvání.
6.  Klikněte na tlačítko **zobrazit aktivní trase** chcete zvýraznit konkrétní cestu události, které většinu podílet se na celkovou dobu trvání operace.  V tomto příkladu vidíte, že nejpomalejší volání je z *FabrikamFiberAzureStorage.GetStorageTableData* metoda. V části, která přebírá většinu času *CloudTable.CreateIfNotExist* metoda. Pokud tento řádek kódu se spustí pokaždé, když funkce volala, budou využívat zbytečným síťovým volání a prostředků procesoru. Nejlepší způsob, jak opravit kódu je uvést tento řádek do některé spuštění metoda, která pouze jednou pro spouštění. 

    ![Podrobnosti o profileru](media/app-insights-tutorial-performance/profiler-details.png)

7.  **Výkonu Tip** v horní části obrazovky podporuje hodnocení, která nadměrné doba je z důvodu čekání.  Klikněte na tlačítko **čekání** odkaz pro dokumentaci o interpretaci různé typy událostí.

    ![Tip výkonu](media/app-insights-tutorial-performance/performance-tip.png)

8.  Pro další analýzu, můžete kliknout na **stáhnout trasování ETL** stahování trasování v sadě Visual Studio.

## <a name="use-analytics-data-for-server"></a>Pomocí analýzy dat pro server
Application Insights Analytics poskytuje plnohodnotný dotazovací jazyk, který umožňuje analyzovat všechna data shromážděná pomocí Application Insights.  Můžete použít k provedení hloubkovou analýzu na data požadavku a výkonu.

1. Vraťte na panelu podrobností operaci a klikněte na tlačítko Analytics.

    ![Tlačítko Analytics](media/app-insights-tutorial-performance/server-analytics-button.png)

2. Analýza Statistika aplikace se otevře pomocí dotazu pro každou ze zobrazení panelu.  Tyto dotazy můžete spustit, jako jsou nebo je upravit pro vaše požadavky.  První dotaz zobrazí dobu trvání pro tuto operaci v čase.

    ![Analýza](media/app-insights-tutorial-performance/server-analytics.png)


## <a name="identify-slow-client-operations"></a>Identifikovat pomalé klientské operace
Kromě určení procesů serveru za účelem optimalizace, Application Insights můžete analyzovat perspektivy prohlížeče klienta.  To může pomoct identifikovat potenciální vylepšení součásti klienta a i identifikují problémy se různé prohlížeče nebo jiné umístění.

1. Vyberte **prohlížeče** pod **prošetření** otevřete v prohlížeči souhrn.  To poskytuje přehled různých telemetries vaší aplikace z pohledu prohlížeče.

    ![Souhrn prohlížeče](media/app-insights-tutorial-performance/browser-summary.png)

2.  Přejděte dolů k položce **co jsou moje nejpomalejší stránky?**.  Ukazuje to seznam stránek v aplikaci, které jste pořídili nejdelší dobu klientům načíst.  Tyto informace můžete použít k určení priority tyto stránek, které mají nejvýraznější dopad na uživatele.
3.  Klikněte na jednu stránek, otevřete **stránky zobrazení** panelu.  V příkladu **/FabrikamProd** stránka se zobrazuje nadměrné Průměrná doba trvání.  **Stránky zobrazení** panel poskytuje podrobnosti o této stránce, včetně rozpis rozsahů jinou dobu trvání.

    ![Zobrazení stránky](media/app-insights-tutorial-performance/page-view.png)

4.  Klikněte na tlačítko pro dobu trvání nejvyšší zkontrolujte podrobnosti o tyto požadavky.  Klikněte na tlačítko jednotlivé žádosti k zobrazení podrobností žádosti stránku, včetně typu prohlížeče a jeho umístění klienta.  Tyto informace vám může pomoci při určování, zda jsou výkonu problémy související s konkrétní typy klientů.

    ![Podrobnosti požadavku](media/app-insights-tutorial-performance/request-details.png)

## <a name="use-analytics-data-for-client"></a>Pomocí analýzy dat pro klienta
Například pro výkon serveru shromažďovat data Application Insights zpřístupní všech klientských dat pro hloubkovou analýzu pomocí Analytics.

1. Vraťte se do prohlížeče, souhrn a klikněte na ikonu Analytics.

    ![Ikona analýzy](media/app-insights-tutorial-performance/client-analytics-icon.png)

2. Analýza Statistika aplikace se otevře pomocí dotazu pro každou ze zobrazení panelu. První dotaz zobrazí dobu trvání pro různá zobrazení stránky průběhu času.

    ![Analýza](media/app-insights-tutorial-performance/client-analytics.png)

3.  Inteligentní diagnostiky je funkce Analýza statistiky aplikace, který identifikuje jedinečný vzorů v datech.  Po kliknutí na tlačítko inteligentní diagnostiky tečky v spojnicový graf, stejný dotaz běží bez záznamů, které způsobily anomálií.  Podrobnosti o tyto záznamy jsou uvedené v sekci komentáře dotazu, abyste mohli identifikovat vlastnosti zobrazení tyto stránek, které způsobují nadměrné doba trvání.

    ![Inteligentní diagnostiky](media/app-insights-tutorial-performance/client-smart-diagnostics.png)


## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili zjištění výjimky v době běhu, přechodu na v dalším kurzu se dozvíte, jak vytvářet oznámení v odpovědi na chyby.

> [!div class="nextstepaction"]
> [Výstraha na stav aplikací](app-insights-tutorial-alert.md)
