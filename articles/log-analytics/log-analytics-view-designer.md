---
title: "Vytvoření zobrazení k analýze dat v Azure Log Analytics | Microsoft Docs"
description: "Návrhář zobrazení v analýzy protokolů umožňuje vytvářet vlastní zobrazení, které se zobrazí na portálu Azure a obsahovat různé vizualizace dat v pracovním prostoru analýzy protokolů. Tento článek obsahuje přehled Návrhář zobrazení a postupy pro vytváření a úpravy vlastních zobrazení."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: bwren
ms.openlocfilehash: a84f40503c1b9778c496461ebbf6864f99bd1c4b
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="use-view-designer-to-create-custom-views-in-log-analytics"></a>Pomocí zobrazení návrhu můžete vytvořit vlastní zobrazení v analýzy protokolů
Návrhář zobrazení v [analýzy protokolů](log-analytics-overview.md) vám umožní vytvořit vlastní zobrazení na portálu Azure, které obsahují různé vizualizace dat v pracovním prostoru analýzy protokolů. Tento článek obsahuje přehled Návrhář zobrazení a postupy pro vytváření a úpravy vlastních zobrazení.

Další články, které jsou k dispozici pro Návrhář zobrazení jsou:

* [Odkaz na dlaždici](log-analytics-view-designer-tiles.md) – odkaz na nastavení pro každou z dlaždice, které jsou k dispozici pro použití ve vaší vlastní zobrazení.
* [Odkaz na část vizualizace](log-analytics-view-designer-parts.md) – odkaz na nastavení pro každou z dlaždice, které jsou k dispozici pro použití ve vaší vlastní zobrazení.

>[!NOTE]
> Pokud pracovní prostor byl upgradován na verzi [nové protokolu Analytics query language](log-analytics-log-search-upgrade.md), pak dotazů ve všech zobrazeních, musí být napsaná [nové dotazovací jazyk](https://go.microsoft.com/fwlink/?linkid=856078).  Všechna zobrazení, které byly vytvořeny před byl upgradován pracovním prostoru bude automtically převést.

## <a name="concepts"></a>Koncepty
Zobrazení se zobrazují na **přehled** stránka pracovní prostor analýzy protokolů na portálu Azure.  Dlaždice pro každý vlastní zobrazení se zobrazí abecedně dlaždice pro řešení nainstalován ve stejném pracovním prostoru.

![Stránka s přehledem](media/log-analytics-view-designer/overview-page.png)

Zobrazení vytvořená pomocí návrháře zobrazení obsahovat elementy v následující tabulce.

| Část | Popis |
|:--- |:--- |
| Dlaždice |Zobrazí na stránce Přehled pro pracovní prostor analýzy protokolů.  Zahrnuje visual sumarizační informace obsažené v vlastní zobrazení.  Různé typy dlaždici poskytovat různé vizualizace záznamů.  Klikněte na dlaždici otevřít vlastní zobrazení. |
| Vlastní zobrazení |Zobrazí, když uživatel klikne na dlaždici.  Obsahuje jednu nebo více částí vizualizace. |
| Vizualizace částí |Vizualizace dat v pracovním prostoru analýzy protokolů založené na jednom nebo několika [protokolu hledání](log-analytics-log-searches.md).  Většina částí bude obsahovat hlavičku, která poskytuje na nejvyšší úrovni vizualizaci a seznam nejvyšší výsledků.  Typy jinou částí poskytují různé vizualizace záznamy v pracovním prostoru analýzy protokolů.  Klikněte na elementy v části provést vyhledávání protokolu poskytuje podrobné záznamy. |


## <a name="work-with-an-existing-view"></a>Práce s existující zobrazení
Když otevřete zobrazení, které bylo vytvořeno pomocí návrháře zobrazení, bude mít nabídku s možností v následující tabulce.

![Přehled nabídky](media/log-analytics-view-designer/overview-menu.png)


| Možnost | Popis |
|:--|:--|
| Obnovení   | Aktualizujte zobrazení s nejnovější data. | 
| Analýzy | Otevřete [Advanced Analytics portál](log-analytics-log-search-portals.md#advanced-analytics-portal) k analýze dat pomocí protokolu hledání. () log-Analytics-log-Search-portals.MD#Advanced-Analytics-Portal). |
| Filtr    | Nastavte filtr času pro dat obsažených v zobrazení. |
| Upravit      | Otevření zobrazení v Návrháři zobrazení, chcete-li upravit jeho obsah a konfiguraci.   |
| Klon     | Vytvoření nového zobrazení a otevřete jej v Návrháři zobrazení.  Nové zobrazení má stejný název jako původní s "zkopírovat" připojením na konec. |


## <a name="create-a-new-view"></a>Vytvoření nového zobrazení
Vytvořit nové zobrazení v **Návrhář zobrazení** kliknutím na dlaždici Návrhář zobrazení na stránce Přehled pracovního prostoru analýzy protokolů na portálu Azure.

![Dlaždice Návrhář zobrazení](media/log-analytics-view-designer/view-designer-tile.png)


## <a name="working-with-view-designer"></a>Práce s Návrhář zobrazení
Návrhář zobrazení budete pracovat, zda vytváříte nové zobrazení nebo úpravou existující.  

Návrhář zobrazení má tři podokna.  **Návrhu** podokně obsahuje vlastní zobrazení, který vytváříte nebo upravujete.  Přidat dlaždice a částí z **řízení** podokně **návrhu** podokně.  **Vlastnosti** podokně se zobrazí vlastnosti pro dlaždice nebo vybraných součástí.

![Návrhář zobrazení](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Konfigurace zobrazení dlaždice
Vlastní zobrazení může mít pouze jedna dlaždice.  Vyberte **dlaždici** ve **řízení** podokně zobrazit aktuální dlaždice nebo vyberte alternativní šablonu.  **Vlastnosti** podokně se zobrazí vlastnosti pro aktuální dlaždice.  Konfigurovat vlastnosti dlaždice podle podrobné informace v [odkaz na dlaždici](log-analytics-view-designer-tiles.md) a klikněte na tlačítko **použít** uložte změny.

### <a name="configure-visualization-parts"></a>Konfigurace vizualizace částí
Zobrazení může obsahovat libovolný počet částí vizualizace.  Vyberte **zobrazení** kartu a pak součástí vizualizace pro přidání do zobrazení.  **Vlastnosti** podokně se zobrazí vlastnosti vybrané součásti.  Zobrazit vlastnosti podle podrobné informace v konfiguraci [vizualizace část odkaz](log-analytics-view-designer-parts.md) a klikněte na tlačítko **použít** se uložit změny.

Zobrazení mít pouze jeden řádek částí vizualizace.  Změna uspořádání existující částí v zobrazení kliknutím a přetažením je do nového umístění.

Součástí vizualizace můžete odebrat ze zobrazení klepnutím **X** tlačítko v pravém horním rohu na druhé.


### <a name="menu-options"></a>Možnosti nabídky
Při práci se zobrazením v režimu úprav, máte možností v nabídce v následující tabulce.

![Upravit nabídky](media/log-analytics-view-designer/edit-menu.png)

| Možnost | Popis |
|:--|:--|
| Uložit        | Změny uložte a zavřete zobrazení. |
| Zrušit      | Zahodit změny a zavřete zobrazení. |
| Odstranit zobrazení | Odstraňte zobrazení. |
| Export      | Export zobrazení [šablony Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md) který můžete importovat do jiného pracovního prostoru.  Název souboru, bude název zobrazení s příponou *omsview*. |
| Import      | Import *omsview* soubor, který jste exportovali z jiného pracovního prostoru.  Tato akce přepíše konfiguraci existující zobrazení. |
| Klon       | Vytvoření nového zobrazení a otevřete jej v Návrháři zobrazení.  Nové zobrazení má stejný název jako původní s "zkopírovat" připojením na konec. |
| Publikování     | Exportovat do souboru JSON, který lze vložit do zobrazení [Mangagement řešení](../operations-management-suite/operations-management-suite-solutions-resources-views.md).  Název souboru, bude název zobrazení s příponou *json*. Je vytvořen druhý soubor s příponou *resjson* obsahující hodnoty pro prostředky, které jsou definované v souboru JSON.

## <a name="next-steps"></a>Další postup
* Přidat [dlaždice](log-analytics-view-designer-tiles.md) do vlastních zobrazení.
* Přidat [vizualizace částí](log-analytics-view-designer-parts.md) do vlastních zobrazení.
