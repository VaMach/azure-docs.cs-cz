---
title: "Vytvoření zobrazení k analýze dat v OMS Log Analytics | Microsoft Docs"
description: "Návrhář zobrazení v analýzy protokolů umožňuje vytvářet vlastní zobrazení, které se zobrazují na portálu OMS a Azure a obsahovat různé vizualizace dat v úložišti OMS. Tento článek obsahuje přehled Návrhář zobrazení a postupy pro vytváření a úpravy vlastních zobrazení."
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
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: e3c463d749dc4179df58286b9bb75584880a6bc6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-view-designer-to-create-custom-views-in-log-analytics"></a>Pomocí zobrazení návrhu můžete vytvořit vlastní zobrazení v analýzy protokolů
Návrhář zobrazení v [analýzy protokolů](log-analytics-overview.md) vám umožní vytvořit vlastní zobrazení v konzole OMS, které obsahují různé vizualizace dat v úložišti OMS. Tento článek obsahuje přehled Návrhář zobrazení a postupy pro vytváření a úpravy vlastních zobrazení.

Další články, které jsou k dispozici pro Návrhář zobrazení jsou:

* [Odkaz na dlaždici](log-analytics-view-designer-tiles.md) – odkaz na nastavení pro každou z dlaždice, které jsou k dispozici pro použití ve vaší vlastní zobrazení.
* [Odkaz na část vizualizace](log-analytics-view-designer-parts.md) – odkaz na nastavení pro každou z dlaždice, které jsou k dispozici pro použití ve vaší vlastní zobrazení.

>[!NOTE]
> Pokud pracovní prostor byl upgradován na verzi [nové protokolu Analytics query language](log-analytics-log-search-upgrade.md), pak dotazů ve všech zobrazeních, musí být napsaná [nové dotazovací jazyk](https://go.microsoft.com/fwlink/?linkid=856078).  Všechna zobrazení, které byly vytvořeny před byl upgradován pracovním prostoru bude automtically převést.

## <a name="concepts"></a>Koncepty
Zobrazení vytvořená pomocí návrháře zobrazení obsahovat elementy v následující tabulce.

| Část | Popis |
|:--- |:--- |
| Dlaždice |Zobrazí na hlavním řídicím přehled protokolu Analytics.  Zahrnuje visual sumarizační informace obsažené v vlastní zobrazení.  Různé typy dlaždici poskytovat různé vizualizace záznamů v úložišti OMS.  Klikněte na dlaždici otevřít vlastní zobrazení. |
| Vlastní zobrazení |Zobrazí, když uživatel klikne na dlaždici.  Obsahuje jednu nebo více částí vizualizace. |
| Vizualizace částí |Vizualizace dat v úložišti OMS založené na jeden nebo více [protokolu hledání](log-analytics-log-searches.md).  Většina částí bude obsahovat hlavičku, která poskytuje na nejvyšší úrovni vizualizaci a seznam nejvyšší výsledků.  Typy jinou částí poskytují různé vizualizace záznamů v úložišti OMS.  Klikněte na elementy v části provést vyhledávání protokolu poskytuje podrobné záznamy. |

![Zobrazit přehled návrháře](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-to-your-workspace"></a>Návrhář zobrazení přidat do pracovního prostoru
Při zobrazení návrhu je ve verzi preview, je třeba přidat ji do pracovního prostoru výběrem **funkce verze Preview** v **nastavení** sekci portálu OMS.

![Povolit náhled](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Vytvoření a úprava zobrazení
### <a name="create-a-new-view"></a>Vytvoření nového zobrazení
Otevřít nové zobrazení v **Návrhář zobrazení** kliknutím na dlaždici Návrhář zobrazení v řídicím panelu hlavní OMS.

![Dlaždice Návrhář zobrazení](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Upravit stávající zobrazení
Chcete-li upravit stávající zobrazení v Návrháři zobrazení, otevřete zobrazení kliknutím na jeho dlaždici na řídicím panelu hlavní OMS.  Klikněte **upravit** tlačítko k otevření zobrazení v Návrháři zobrazení.

![Upravit zobrazení](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Klonování existující zobrazení
Pokud jste klonovat zobrazení, vytvoří nové zobrazení a otevře ji v Návrháři zobrazení.  Nové zobrazení bude mít stejný název jako původní s "zkopírovat" připojením na konec.  Když kliknete na jeho dlaždici na řídicím panelu hlavní OMS klonovat zobrazení, otevřete existující zobrazení.  Klikněte **klon** tlačítko k otevření zobrazení v Návrháři zobrazení.

![Clone – zobrazení](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Odstraňte existující zobrazení
Chcete-li odstranit stávající zobrazení, otevřete zobrazení kliknutím na jeho dlaždici na řídicím panelu hlavní OMS.  Klikněte **upravit** tlačítko k otevření zobrazení v Návrháři zobrazení a klikněte na tlačítko **odstranit zobrazení**.

![Odstranit zobrazení](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Export existující zobrazení
Zobrazení můžete exportovat do souboru JSON, který lze importovat do jiného prostoru nebo lze použít v [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).  Pokud chcete exportovat stávající zobrazení, otevření zobrazení kliknutím na jeho dlaždici na řídicím panelu hlavní OMS.  Klikněte **exportovat** tlačítko pro vytvoření souboru ve složce pro stahování v prohlížeči.  Název souboru, bude název zobrazení s příponou *omsview*.

![Export zobrazení](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Importovat stávající zobrazení
Můžete importovat *omsview* soubor, který jste exportovali z jiné skupiny pro správu.  Chcete-li importovat existující zobrazení, nejdřív vytvořte nové zobrazení.  Klikněte **Import** tlačítko a vyberte *omsview* souboru.  Konfigurace v souboru se zkopírují do existující zobrazení.

![Export zobrazení](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Práce s Návrhář zobrazení
Návrhář zobrazení má tři podokna.  **Návrhu** podokně představuje vlastní zobrazení.  Když přidáte dlaždice a částí z **řízení** podokně **návrhu** podokně budou přidány do zobrazení.  **Vlastnosti** podokně se zobrazí vlastnosti pro dlaždice nebo vybraných součástí.

![Návrhář zobrazení](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Konfigurace zobrazení dlaždice
Vlastní zobrazení může mít pouze jedna dlaždice.  Vyberte **dlaždici** ve **řízení** podokně zobrazit aktuální dlaždice nebo vyberte alternativní šablonu.  **Vlastnosti** podokně se zobrazí vlastnosti pro aktuální dlaždice.  Konfigurovat vlastnosti dlaždice podle podrobné informace v [odkaz na dlaždici](log-analytics-view-designer-tiles.md) a klikněte na tlačítko **použít** uložte změny.

### <a name="configure-visualization-parts"></a>Konfigurace vizualizace částí
Zobrazení může obsahovat libovolný počet částí vizualizace.  Vyberte **zobrazení** kartu a pak součástí vizualizace pro přidání do zobrazení.  **Vlastnosti** podokně se zobrazí vlastnosti vybrané součásti.  Zobrazit vlastnosti podle podrobné informace v konfiguraci [vizualizace část odkaz](log-analytics-view-designer-parts.md) a klikněte na tlačítko **použít** se uložit změny.

### <a name="delete-a-visualization-part"></a>Odstraňte část vizualizace
Součástí vizualizace můžete odebrat ze zobrazení klepnutím **X** tlačítko v pravém horním rohu na druhé.

### <a name="rearrange-visualization-parts"></a>Změna uspořádání částí vizualizace
Zobrazení mít pouze jeden řádek částí vizualizace.  Změna uspořádání existující částí v zobrazení kliknutím a přetažením je do nového umístění.

## <a name="next-steps"></a>Další kroky
* Přidat [dlaždice](log-analytics-view-designer-tiles.md) do vlastních zobrazení.
* Přidat [vizualizace částí](log-analytics-view-designer-parts.md) do vlastních zobrazení.
