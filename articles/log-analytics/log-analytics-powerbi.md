---
title: "Exportovat data analýzy protokolů do Power BI | Microsoft Docs"
description: "Power BI je služba cloudové obchodní analýzy od společnosti Microsoft, která poskytuje bohatých vizualizací a sestav pro analýzu dat různé sady.  Analýzy protokolů můžete nepřetržitě exportovat data z úložiště OMS do Power BI, můžete využít jeho vizualizace a nástrojů pro analýzu.  Tento článek popisuje, jak nakonfigurovat dotazy v analýzy protokolů, které automaticky exportovat do Power BI v pravidelných intervalech."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: bwren
ms.openlocfilehash: 271747e25f319c76195ec643025d24c6b7cdc9c5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="export-log-analytics-data-to-power-bi"></a>Exportovat data analýzy protokolů do Power BI

>[!NOTE]
> Pokud pracovní prostor byl upgradován na verzi [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak tento proces exportu analýzy protokolů dat do Power BI přestane fungovat.  Všechny existující plány, které jste vytvořili před upgradem bude zakázán. Zobrazí se také již možnost zapnout možnost exportu Power BI v nastavení v části funkce verze Preview, protože tato funkce je plně vydané v upgradované pracovních prostorů. 
>
> Po upgradu, analýzy protokolů Azure používá stejnou platformu jako Application Insights a použít stejný postup exportování dotazů analýzy protokolů pro Power BI jako [proces exportu dotazy Application Insights do Power BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).  Můžete buď export dotazu pomocí konzole analýzy, jak je popsáno v tomto článku, nebo můžete vybrat **Power BI** tlačítka v horní části obrazovky na portálu hledání protokolů.
>
> Uživatelé budou potřebovat přístup k prostředku prostoru v Azure a použít capabilitiy export Power BI v upgradované pracovních prostorů. Bez přístupu uživatelům se zobrazí chybová zpráva při importu dotaz pro Power BI desktop nemají přístup.



[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) je cloudové obchodní analýzy služba společnosti Microsoft, která poskytuje bohatých vizualizací a sestav pro analýzu dat různé sady.  Analýzy protokolů můžete automaticky exportovat data z úložiště OMS do Power BI, můžete využít jeho vizualizace a nástrojů pro analýzu.

Když nakonfigurujete Power BI s analýzy protokolů, můžete vytvořit dotazy protokolu, které jejich výsledky exportovat do odpovídající datové sady ve službě Power BI.  Dotaz a export i nadále spouštět automaticky podle plánu, který definujete, aby byl aktuální datovou sadu s nejnovější data shromažďovaná společností analýzy protokolů.

![Analýzy protokolů k Power BI.](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a>Plány Power BI
A *Power BI plán* zahrnuje hledání protokolů, který exportuje sady dat z úložiště OMS na odpovídající datovou sadu v Power BI a plán, který definuje, jak často se spustí tohoto hledání zachovat aktuální datovou sadu.

Pole v sadě dat bude odpovídat vlastnosti záznamů protokolu nalezené.  Pokud hledání vrátí záznamy různých typů, bude obsahovat datovou sadu všechny vlastnosti z jednotlivých typů součástí záznamu.  

> [!NOTE]
> Je vhodné použít dotaz vyhledávání protokolu, který vrátí data o hrubém a provádění všech konsolidace pomocí příkazů jako třeba [měr](log-analytics-search-reference.md#measure).  Všechny agregací a výpočtů v Power BI můžete provést z nezpracovaná data.
>
>

## <a name="connecting-oms-workspace-to-power-bi"></a>Pracovní prostor OMS připojuje k Power BI
Před exportem z analýzy protokolů pro Power BI, je nutné připojit ke svému účtu Power BI pomocí následujícího postupu vaším pracovním prostorem OMS.  

1. V konzole OMS klikněte na **nastavení** dlaždici.
2. Vyberte **účty**.
3. V **informace o pracovním prostoru** klikněte na část **připojit k účtu Power BI**.
4. Zadejte přihlašovací údaje pro svůj účet Power BI.

## <a name="create-a-power-bi-schedule"></a>Vytvoření plánu Power BI
Vytvoření plánu Power BI pro každé datové sady, pomocí následujícího postupu.

1. V konzole OMS klikněte na **hledání protokolů** dlaždici.
2. Zadejte nový dotaz nebo vyberte uloženého hledání, který vrací data, která chcete exportovat do **Power BI**.  
3. Klikněte **Power BI** tlačítka v horní části stránky otevřete **Power BI** dialogové okno.
4. Poskytování informací v následující tabulce a klikněte na tlačítko **Uložit**.

| Vlastnost | Popis |
|:--- |:--- |
| Name (Název) |Název pro identifikaci podle plánu, při zobrazení seznamu plány Power BI. |
| Uložené hledání |Vyhledávání protokolu ke spuštění.  Můžete vybrat aktuální dotaz nebo rozevíracího seznamu vyberte položku existujícího uloženého hledání. |
| Plán |Jak často ke spuštění uložené hledání a exportovat do datové sady Power BI.  Hodnota musí být mezi 15 minutami a 24 hodin. |
| Název datové sady |Název datové sady ve službě Power BI.  Bude vytvořen, pokud neexistuje a aktualizovat, pokud neexistuje. |

## <a name="viewing-and-removing-power-bi-schedules"></a>Zobrazení a odebrání plány Power BI
Zobrazení seznamu existující plán Power BI pomocí následujícího postupu.

1. V konzole OMS klikněte na **nastavení** dlaždici.
2. Vyberte **Power BI**.

Kromě podrobnosti plánu se zobrazí počet používající plán minulého týdne a stav poslední synchronizace.  Pokud synchronizace došlo k chybám, můžete kliknutím na odkaz ke spuštění vyhledávání protokolu pro záznamy s informace o chybě.

Plán můžete odebrat tak, že kliknete na **X** v **odebrat sloupec**.  Plán můžete zakázat tak, že vyberete **vypnout**.  Změna plánu je třeba odebrat ji a znovu ji vytvořte s novým nastavením.

![Plány Power BI](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a>Ukázka návod
V následující části vás provede příklad vytvoření plánu Power BI a vytvoříte jednoduchou sestavu pomocí její datové sady.  V tomto příkladu se exportují všechny údaje o výkonu pro skupinu počítačů do Power BI a pak se vytvoří spojnicový graf pro zobrazení využití procesoru.

### <a name="create-log-search"></a>Vytvoření hledání protokolů
Začneme vytvořením hledání protokolů pro data, která nám chcete poslat do datové sady.  V tomto příkladu použijeme dotaz, který vrátí všechny údaje o výkonu pro počítače s názvem, které začíná *srv*.  

![Plány Power BI](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a>Vytvoření hledání Power BI
Kliknete na **Power BI** tlačítko otevřete dialogové okno Power BI a zadejte požadované informace.  Chceme tohoto hledání spustit jednou za hodinu a vytvořit datovou sadu s názvem *Contoso výkonu*.  Vzhledem k tomu, že již máme otevřete vyhledávání, který vytváří dat chceme, jsme ponechte výchozí *použít aktuální vyhledávací dotaz* pro **uložené výsledky hledání**.

![Power BI vyhledávání](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a>Ověřte Power BI vyhledávání
Chcete zkontrolovat, že jsme vytvořili plán správně, nemůžeme zobrazit seznam hledání Power BI pod **nastavení** dlaždici na řídicím panelu OMS.  Jsme Počkejte několik minut a aktualizujte toto zobrazení, dokud ho hlásí, že synchronizace byl spuštěn.

![Power BI vyhledávání](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-the-dataset-in-power-bi"></a>Ověření datové sady ve službě Power BI
Jsme Přihlaste se k naší účtu v [powerbi.microsoft.com](http://powerbi.microsoft.com/) a přejděte k položce **datové sady** v dolní části levého podokna.  Jsme uvidíte, že *Contoso výkonu* datové sady je uveden indikující, že naše exportu byla úspěšně spuštěna.

![Datová sada Power BI](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a>Vytvoření sestavy založené na datové sady
Jsme vyberte **Contoso výkonu** datovou sadu a potom klikněte na **výsledky** v **pole** podokně na pravé straně, chcete-li zobrazit pole, které jsou součástí této datové sady.  Pokud chcete vytvořit spojnicový graf zobrazující využití procesoru pro každý počítač, můžeme provést následující akce.

1. Vyberte graf vizualizaci řádku.
2. Přetáhněte **ObjectName** k **filtr úrovně sestav** a zkontrolujte **procesoru**.
3. Přetáhněte **název_čítače** k **filtr úrovně sestav** a zkontrolujte **% času procesoru**.
4. Přetáhněte **přepočtené** k **hodnoty**.
5. Přetáhněte **počítače** k **legendy**.
6. Přetáhněte **TimeGenerated** k **osy**.

Vidíme, že se zobrazí výsledná spojnicový graf s daty z naší datové sadě.

![Power BI spojnicový graf](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-the-report"></a>Uložení sestavy
Jsme sestavu uložte kliknutím na tlačítko Uložit v horní části obrazovky a ověřit, že je teď uvedený v části sestavy v levém podokně.

![Sestavy Power BI](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a>Další kroky
* Další informace o [protokolu hledání](log-analytics-log-searches.md) vytvářet dotazy, které je možné exportovat do Power BI.
* Další informace o [Power BI](http://powerbi.microsoft.com) vytvářet vizualizace podle exportuje analýzy protokolů.
