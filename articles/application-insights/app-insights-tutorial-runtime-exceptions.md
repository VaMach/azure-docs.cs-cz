---
title: "Diagnostikovat výjimky v době běhu pomocí služby Azure Application Insights | Microsoft Docs"
description: "Kurz najít a diagnostikovat výjimky v době běhu do vaší aplikace pomocí služby Azure Application Insights."
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/19/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: f6844dd6747854a60ff8eb8be0d913b73ca2bdb2
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="find-and-diagnose-run-time-exceptions-with-azure-application-insights"></a>Najít a diagnostikovat výjimky v době běhu pomocí služby Azure Application Insights

Azure Application Insights shromažďuje telemetrie z vaší aplikace, které pomáhají identifikovat a diagnostikovat výjimky v době běhu.  Tento kurz vás provede tento proces s vaší aplikací.  Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Upravit projektu povolit sledování výjimek
> * Identifikovat výjimky pro různé součásti aplikace
> * Zobrazit podrobnosti výjimky
> * Stáhněte si snímek výjimky pro Visual Studio pro ladění
> * Analyzovat podrobnosti o neúspěšných požadavků pomocí dotazovacího jazyka pro
> * Vytvoření nové pracovní položky a opravte chybný kód


## <a name="prerequisites"></a>Požadavky

K provedení kroků v tomto kurzu je potřeba:

- Nainstalovat [Visual Studio 2017](https://www.visualstudio.com/downloads/) s následujícími sadami funkcí:
    - Vývoj pro ASP.NET a web
    - Vývoj pro Azure
- Stáhněte a nainstalujte [Visual Studio Debugger snímku](http://aka.ms/snapshotdebugger).
- Povolit [snímku ladicího programu sady Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger)
- Nasazení aplikace .NET do Azure a [povolit Application Insights SDK](app-insights-asp-net.md). 
- Kurz sleduje identifikace výjimku ve vaší aplikaci, takže upravit kód ve vašem prostředí vývoj nebo testování generovat výjimku. 

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.
Přihlaste se k webu Azure Portal na adrese [https://portal.azure.com](https://portal.azure.com).


## <a name="analyze-failures"></a>Analýza selhání
Application Insights shromažďuje případných selhání v aplikaci a umožňuje zobrazit jejich četnost mezi různé operace můžete zaměřit na ty s nejvyšší dopad.  Potom podrobnostem na podrobnosti k identifikaci hlavní příčinu selhání.   

1. Vyberte **Application Insights** a pak vašeho předplatného.  
1. Otevřete **selhání** panelu vyberte buď **selhání** pod **prošetření** nabídky nebo klikněte na **neúspěšné požadavky** grafu.

    ![Neúspěšné požadavky](media/app-insights-tutorial-runtime-exceptions/failed-requests.png)

2. **Neúspěšné požadavky** panelu ukazuje počet neúspěšných požadavků a počet uživatelů ovlivněných pro každou operaci pro aplikaci.  Tyto informace uživatelem seřazením podle můžete identifikovat selhání, většinu ovlivnily uživatele.  V tomto příkladu **získat zaměstnanci nebo vytvořit** a **získat zákazníků nebo podrobnosti** pravděpodobně kandidáty prozkoumat kvůli jejich velkému počtu selhání a dotyčným uživatelům.  Výběr operace další zobrazí informace o této operaci v pravém panelu.

    ![Požadavky panelu se nezdařilo](media/app-insights-tutorial-runtime-exceptions/failed-requests-blade.png)

3. Snižte časový interval pro přiblížení období ukazuje, kde je míra selhání Špička.

    ![Okno neúspěšných požadavků](media/app-insights-tutorial-runtime-exceptions/failed-requests-window.png)

4. Klikněte na tlačítko **zobrazit podrobnosti o** a zobrazit podrobnosti pro operaci.  To zahrnuje Ganttova, který ukazuje dva selhání závislosti, které souhrnně trvalo téměř polovinu druhý k dokončení.  Můžete najít další informace o analýze problémy s výkonem přejít ke kurzu [najít a diagnostikovat problémy s výkonem pomocí služby Azure Application Insights](app-insights-tutorial-performance.md).

    ![Podrobnosti žádosti se nezdařilo](media/app-insights-tutorial-runtime-exceptions/failed-requests-details.png)

5. Podrobnosti operace také ukazuje FormatException který pravděpodobně způsobila selhání.  Klikněte na výjimku nebo na **typy výjimek horní 3** počet zobrazíte její podrobnosti.  Uvidíte, že je z důvodu neplatný kód zip.

    ![Podrobnosti o výjimce](media/app-insights-tutorial-runtime-exceptions/failed-requests-exception.png)



## <a name="identify-failing-code"></a>Identifikovat selhání kódu
Ladicí program snímku shromažďuje snímky nejčastěji se vyskytující výjimek v aplikaci vám pomůže při diagnostikování jeho hlavní příčinu v produkčním prostředí.  Ladění snímky můžete zobrazit na portálu najdete v části volání zásobníku a zkontrolovat proměnné v každé rámce zásobníku volání. Zdrojový kód pak můžete ladit stažením snímku a ho otevřít v aplikaci Visual Studio 2017.

1. Ve vlastnostech výjimky, klikněte na tlačítko **otevřete ladění snímku**.
2. **Ladění snímku** panelu otevře s zásobníku volání pro daný požadavek.  Klikněte na libovolné metody si prohlédněte hodnoty všech místních proměnných v době požadavku.  Od hlavní metoda v tomto příkladu, vidíme, místní proměnné, které nemají žádnou hodnotu.

    ![Ladění snímku](media/app-insights-tutorial-runtime-exceptions/debug-snapshot-01.png)

4. První volání, která má platné hodnoty je **ValidZipCode**, a My uvidíme, že zip kód byl zadán v rámci písmena, která není možné převést na celé číslo.  To zdá být chyba v kódu, který je třeba opravit.

    ![Ladění snímku](media/app-insights-tutorial-runtime-exceptions/debug-snapshot-02.png)

5. Chcete-li stáhnout tento snímek do sady Visual Studio, které jsme najít skutečný kód, který je třeba opravit, klikněte na tlačítko **stáhnout snímku**.
6. Snímek je načten do sady Visual Studio.
7. Teď můžete spustit na relaci ladění v sadě Visual Studio, který identifikuje rychle řádek kódu, který způsobil výjimku.

    ![Výjimka v kódu](media/app-insights-tutorial-runtime-exceptions/exception-code.png)


## <a name="use-analytics-data"></a>Použití analytická data
Všechna data shromážděná pomocí Application Insights je uložená v Azure Log Analytics, která poskytuje plnohodnotný dotazovací jazyk, který umožňuje analyzovat data v mnoha různými způsoby.  Tyto údaje můžete použít k analýze požadavků, které generuje výjimky, které jsme se analýza. 

8. Klikněte na tlačítko informace Codelensu výše kód k zobrazení telemetrie poskytované Application Insights.

    ![Kód](media/app-insights-tutorial-runtime-exceptions/codelens.png)

9. Klikněte na tlačítko **analyzovat dopad** otevřete Application Insights Analytics.  Je naplněna s několika dotazy, které poskytují podrobné informace o chybných žádostech. například dotyčným uživatelům, prohlížečů a oblastech.<br><br>![Analýzy](media/app-insights-tutorial-runtime-exceptions/analytics.png)<br>

## <a name="add-work-item"></a>Přidat pracovní položky
Pokud připojíte Application Insights na sledování systém jako Visual Studio Team Services nebo Githubu, můžete vytvořit pracovní položky přímo z Application Insights.

1. Vraťte se na **vlastnosti výjimky** panelu ve službě Application Insights.
2. Klikněte na tlačítko **novou pracovní položku**.
3. **Novou pracovní položku** panelu otevře s podrobnostmi o výjimce už vyplněné.  Můžete přidat žádné další údaje před uložením.

    ![Novou pracovní položku](media/app-insights-tutorial-runtime-exceptions/new-work-item.png)

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili zjištění výjimky v době běhu, přechodu na v dalším kurzu se dozvíte, jak identifikovat a diagnostikovat problémy s výkonem.

> [!div class="nextstepaction"]
> [Identifikovat problémy s výkonem](app-insights-tutorial-performance.md)