---
title: Importovat Azure Log Analytics data do Power BI | Microsoft Docs
description: "Power BI je služba cloudové obchodní analýzy od společnosti Microsoft, která poskytuje bohatých vizualizací a sestav pro analýzu dat různé sady.  Tento článek popisuje postup konfigurace do Power BI importovat data analýzy protokolů a nakonfigurovat, aby automaticky aktualizovat."
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
ms.date: 11/27/2017
ms.author: bwren
ms.openlocfilehash: 163ac33af43a8cb7a23742f6336efca5fe7c4b4e
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="import-azure-log-analytics-data-into-power-bi"></a>Importovat Azure Log Analytics data do Power BI


[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) je cloudové obchodní analýzy služba společnosti Microsoft, která poskytuje bohatých vizualizací a sestav pro analýzu dat různé sady.  Výsledky hledání protokolu analýzy protokolů můžete importovat do datové sady Power BI umožňuje využívat jeho funkce suchas kombinování dat z různých zdrojů a sdílení sestavy na web a mobilní zařízení.

Tento článek poskytuje podrobné informace o importování dat analýzy protokolů do Power BI a plánování při aktualizaci automaticky.  Různé procesy, které jsou zahrnuté pro [upgradovat](#upgraded-workspace) a [starší verze](#legacy-workspace) pracovního prostoru.

## <a name="upgraded-workspace"></a>Upgradovaná prostoru


Pro import dat z [upgradovat pracovní prostor analýzy protokolů](log-analytics-log-search-upgrade.md) do Power BI, vytvoříte datové sady ve službě Power BI založené na protokolu vyhledávací dotaz v analýzy protokolů.  Pokaždé, když se aktualizují datovou sadu spuštění dotazu.  Potom můžete vytvořit sestavy Power BI, které používají data z datové sady.  Vytvořit datová sada v Power BI, které exportujete dotazu z analýzy protokolů pro [Power Query (M) jazyk](https://msdn.microsoft.com/library/mt807488.aspx).  To poté použít k vytvoření dotazu v Power BI Desktop a potom jej publikujte do Power BI jako datové sady.  Podrobnosti tohoto procesu jsou popsané níže.

![Analýzy protokolů k Power BI.](media/log-analytics-powerbi/overview.png)

### <a name="export-query"></a>Export dotazu
Začněte vytvořením [hledání protokolů](log-analytics-log-search-new.md) , vrací data z analýzy protokolů chcete naplnění datové sady Power BI.  Poté exportujte tento dotaz, který [Power Query (M) jazyk](https://msdn.microsoft.com/library/mt807488.aspx) který může být použit Power BI Desktop.

1. Vytvořte hledání protokolů v analýzy protokolů pro extrahování dat pro datovou sadu.
2. Pokud používáte portál vyhledávání protokolu, klikněte na tlačítko **Power BI**.  Pokud používáte portál analýzy, vyberte **exportovat** > **Power BI dotazu (M)**.  Obě tyto možnosti dotazu exportovat do textového souboru s názvem **PowerBIQuery.txt**. 

    ![Export protokolu vyhledávání](media/log-analytics-powerbi/export-logsearch.png) ![Export protokolu vyhledávání](media/log-analytics-powerbi/export-analytics.png)

3. Otevřete textový soubor a zkopírujte její obsah.

### <a name="import-query-into-power-bi-desktop"></a>Importovat dotaz do Power BI Desktop
Power BI Desktop je plochy aplikace, která vám umožní vytvořit datové sady a sestavy, které může být publikována do služby Power BI.  Můžete ji použít i k vytvoření dotazu pomocí doplňku Power Query jazyka exportovaný z analýzy protokolů. 

1. Nainstalujte [Power BI Desktop](https://powerbi.microsoft.com/desktop/) Pokud nepoužíváte ji už máte a pak otevřete aplikaci.
2. Vyberte **načíst Data** > **prázdné dotazu** otevřete nový dotaz.  Potom vyberte **Upřesnit** a vložte obsah exportovaný soubor do dotazu. Klikněte na **Done** (Hotovo).

    ![Power BI Desktop dotazu](media/log-analytics-powerbi/desktop-new-query.png)

5. Spuštění dotazu a její výsledky se zobrazí.  Můžete být vyzváni k zadání pověření pro připojení k Azure.  
6. Zadejte popisný název pro dotaz.  Výchozí hodnota je **dotaz 1**. Klikněte na tlačítko **zavřete a použít** do sestavy přidat datovou sadu.

    ![Název Power BI Desktop](media/log-analytics-powerbi/desktop-results.png)



### <a name="publish-to-power-bi"></a>Publikování do služby Power BI
Když publikujete do Power BI, vytvoří se datové sady a sestavy.  Pokud vytvoříte sestavu v Power BI Desktop, pak to bude publikována s daty.  Pokud ne, pak se vytvoří prázdné sestavy.  Můžete upravit sestavu v Power BI nebo vytvořte novou založené na datovou sadu.

8. Vytvoření sestavy založené na vaše data.  Použití [Power BI Desktop dokumentaci](https://docs.microsoft.com/power-bi/desktop-report-view) Pokud nejste obeznámeni s ním.  Až budete připraveni k odeslání do Power BI, klikněte na tlačítko **publikovat**.  Po zobrazení výzvy vyberte cílové umístění ve vašem účtu Power BI.  Pokud máte na určité cílové místo v paměti, používat **pracovního prostoru**.

    ![Publikování Power BI Desktop](media/log-analytics-powerbi/desktop-publish.png)

3. Po dokončení publikování, klikněte na tlačítko **otevřít v Power BI** otevřít Power BI se vaše nová datová sada.


### <a name="configure-scheduled-refresh"></a>Nakonfigurujte plánované aktualizace
Datové sady vytvořené v Power BI bude mít stejná data, která jste předtím viděli v Power BI Desktop.  Budete muset aktualizovat datová sada pravidelně k spusťte dotaz znovu a jeho naplnění nejnovější data z analýzy protokolů.  

1. V pracovním prostoru, kde jste nahráli sestavy a vyberte příkaz **datové sady** nabídky. Vyberte příslušnou kontextovou nabídku vedle vaše nová datová sada a vyberte **nastavení**. V části **přihlašovací údaje ke zdroji dat** byste měli mít zprávu, že přihlašovací údaje jsou neplatné.  Je to proto, že nebyly zadali přihlašovací údaje ještě pro datovou sadu pro použití při ho aktualizuje jeho data.  Klikněte na tlačítko **upravit přihlašovací údaje** a zadejte přihlašovací údaje s přístupem k analýze protokolů.

    ![Plán Power BI](media/log-analytics-powerbi/powerbi-schedule.png)

5. V části **naplánovaná aktualizace** zapnout možnost **průběžně aktualizovat vaše data**.  Volitelně můžete změnit **obnovovací frekvence** a určitých časech spustit aktualizaci.

    ![Aktualizace Power BI](media/log-analytics-powerbi/powerbi-schedule-refresh.png)

## <a name="legacy-workspace"></a>Starší verze prostoru
Když konfigurujete Power BI s [starší verze pracovní prostor analýzy protokolů](log-analytics-powerbi.md), vytváření dotazů protokolu, které jejich výsledky exportovat do odpovídající datové sady ve službě Power BI.  Dotaz a export i nadále spouštět automaticky podle plánu, který definujete, aby byl aktuální datovou sadu s nejnovější data shromažďovaná společností analýzy protokolů.

![Analýzy protokolů k Power BI.](media/log-analytics-powerbi/overview-legacy.png)

### <a name="power-bi-schedules"></a>Plány Power BI
A *Power BI plán* zahrnuje hledání protokolů, který exportuje sady dat z úložiště OMS na odpovídající datovou sadu v Power BI a plán, který definuje, jak často se spustí tohoto hledání zachovat aktuální datovou sadu.

Pole v sadě dat bude odpovídat vlastnosti záznamů protokolu nalezené.  Pokud hledání vrátí záznamy různých typů, bude obsahovat datovou sadu všechny vlastnosti z jednotlivých typů součástí záznamu.  

### <a name="connecting-oms-workspace-to-power-bi"></a>Pracovní prostor OMS připojuje k Power BI
Před exportem z analýzy protokolů pro Power BI, je nutné připojit ke svému účtu Power BI pomocí následujícího postupu vaším pracovním prostorem OMS.  

1. V konzole OMS klikněte na **nastavení** dlaždici.
2. Vyberte **účty**.
3. V **informace o pracovním prostoru** klikněte na část **připojit k účtu Power BI**.
4. Zadejte přihlašovací údaje pro svůj účet Power BI.

### <a name="create-a-power-bi-schedule"></a>Vytvoření plánu Power BI
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

### <a name="viewing-and-removing-power-bi-schedules"></a>Zobrazení a odebrání plány Power BI
Zobrazení seznamu existující plán Power BI pomocí následujícího postupu.

1. V konzole OMS klikněte na **nastavení** dlaždici.
2. Vyberte **Power BI**.

Kromě podrobnosti plánu se zobrazí počet používající plán minulého týdne a stav poslední synchronizace.  Pokud synchronizace došlo k chybám, můžete kliknutím na odkaz ke spuštění vyhledávání protokolu pro záznamy s informace o chybě.

Plán můžete odebrat tak, že kliknete na **X** v **odebrat sloupec**.  Plán můžete zakázat tak, že vyberete **vypnout**.  Změna plánu je třeba odebrat ji a znovu ji vytvořte s novým nastavením.

![Plány Power BI](media/log-analytics-powerbi/schedules.png)

### <a name="sample-walkthrough"></a>Ukázka návod
V následující části vás provede příklad vytvoření plánu Power BI a vytvoříte jednoduchou sestavu pomocí její datové sady.  V tomto příkladu se exportují všechny údaje o výkonu pro skupinu počítačů do Power BI a pak se vytvoří spojnicový graf pro zobrazení využití procesoru.

#### <a name="create-log-search"></a>Vytvoření hledání protokolů
Začneme vytvořením hledání protokolů pro data, která nám chcete poslat do datové sady.  V tomto příkladu použijeme dotaz, který vrátí všechny údaje o výkonu pro počítače s názvem, které začíná *srv*.  

![Plány Power BI](media/log-analytics-powerbi/walkthrough-query.png)

#### <a name="create-power-bi-search"></a>Vytvoření hledání Power BI
Kliknete na **Power BI** tlačítko otevřete dialogové okno Power BI a zadejte požadované informace.  Chceme tohoto hledání spustit jednou za hodinu a vytvořit datovou sadu s názvem *Contoso výkonu*.  Vzhledem k tomu, že již máme otevřete vyhledávání, který vytváří dat chceme, jsme ponechte výchozí *použít aktuální vyhledávací dotaz* pro **uložené výsledky hledání**.

![Power BI vyhledávání](media/log-analytics-powerbi/walkthrough-schedule.png)

#### <a name="verify-power-bi-search"></a>Ověřte Power BI vyhledávání
Chcete zkontrolovat, že jsme vytvořili plán správně, nemůžeme zobrazit seznam hledání Power BI pod **nastavení** dlaždici na řídicím panelu OMS.  Jsme Počkejte několik minut a aktualizujte toto zobrazení, dokud ho hlásí, že synchronizace byl spuštěn.  Obvykle budete naplánovat datovou sadu, která aktualizuje automaticky.

![Power BI vyhledávání](media/log-analytics-powerbi/walkthrough-schedules.png)

#### <a name="verify-the-dataset-in-power-bi"></a>Ověření datové sady ve službě Power BI
Jsme Přihlaste se k naší účtu v [powerbi.microsoft.com](http://powerbi.microsoft.com/) a přejděte k položce **datové sady** v dolní části levého podokna.  Jsme uvidíte, že *Contoso výkonu* datové sady je uveden indikující, že naše exportu byla úspěšně spuštěna.

![Datová sada Power BI](media/log-analytics-powerbi/walkthrough-datasets.png)

#### <a name="create-report-based-on-dataset"></a>Vytvoření sestavy založené na datové sady
Jsme vyberte **Contoso výkonu** datovou sadu a potom klikněte na **výsledky** v **pole** podokně na pravé straně, chcete-li zobrazit pole, které jsou součástí této datové sady.  Pokud chcete vytvořit spojnicový graf zobrazující využití procesoru pro každý počítač, můžeme provést následující akce.

1. Vyberte graf vizualizaci řádku.
2. Přetáhněte **ObjectName** k **filtr úrovně sestav** a zkontrolujte **procesoru**.
3. Přetáhněte **název_čítače** k **filtr úrovně sestav** a zkontrolujte **% času procesoru**.
4. Přetáhněte **přepočtené** k **hodnoty**.
5. Přetáhněte **počítače** k **legendy**.
6. Přetáhněte **TimeGenerated** k **osy**.

Vidíme, že se zobrazí výsledná spojnicový graf s daty z naší datové sadě.

![Power BI spojnicový graf](media/log-analytics-powerbi/walkthrough-linegraph.png)

#### <a name="save-the-report"></a>Uložení sestavy
Jsme sestavu uložte kliknutím na tlačítko Uložit v horní části obrazovky a ověřit, že je teď uvedený v části sestavy v levém podokně.

![Sestavy Power BI](media/log-analytics-powerbi/walkthrough-report.png)



## <a name="next-steps"></a>Další kroky
* Další informace o [protokolu hledání](log-analytics-log-searches.md) vytvářet dotazy, které je možné exportovat do Power BI.
* Další informace o [Power BI](http://powerbi.microsoft.com) vytvářet vizualizace podle exportuje analýzy protokolů.
