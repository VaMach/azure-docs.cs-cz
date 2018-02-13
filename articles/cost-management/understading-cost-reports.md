---
title: "Seznámení s náklady na sestav v Azure náklady na správu | Microsoft Docs"
description: "Tento článek vám pomůže pochopit základní strukturu Cloudyn sestavy a funkce."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/30/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: 38c1313f42a58403e158cad9c2930b6541da5adc
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="understanding-cost-reports"></a>Vysvětlení sestav nákladů

Tento článek vám pomůže pochopit základní strukturu Cloudyn sestavy a funkce. Většina Cloudyn sestavy jsou intuitivní a mít jednotný vzhled a chování. Po přečtení tohoto článku, jsou připravené k použití všechny sestavy. Mnoho funkcí standard je k dispozici v rámci různých sestavy, umožňující snadno vyhledat sestavy. Sestavy jsou přizpůsobitelné, a můžete vybrat z několika možností k výpočtu a zobrazení výsledků.

## <a name="report-fields-and-options"></a>Pole sestavy a možnosti

Zde je podívejte se na příklad sestavy náklady v čase. Většina sestavy Cloudyn mají podobné rozložení.

![Ukázková sestava](./media/understanding-cost-reports/sample-report.png)

Každou číslem oblast na předchozím obrázku je podrobně popsaná v následující informace:

1. **Rozsah kalendářních dat**

    Pomocí seznamu rozsah můžete definovat časový interval sestavy, pomocí přednastavených nebo vlastní.
2. **Uložený filtr**

    Pomocí seznamu uložit filtru můžete uložit aktuální skupiny a filtry, které se použijí pro sestavu. Uložené filtry jsou k dispozici napříč nákladů a výkonu sestav, včetně:

      - Analýza nákladů
      - Přidělení
      - Správa majetku
      - Optimalizace

  Zadejte název filtru a kliknutím na možnost **Uložit**.

3. **Značky**

    Pomocí značky oblasti do skupiny podle kategorií značky. Značky, které jsou uvedené v nabídce jsou Azure oddělení nebo náklady center značky nebo jsou náklady na Cloudyn entity a předplatné značky. Vyberte značek k filtrování výsledků. Můžete také zadat název značky (– klíčové slovo) pro filtrování výsledků.

    ![Vyberte možnosti](./media/understanding-cost-reports/select-options.png)

    Klikněte na tlačítko **přidat** přidat nový filtr.

    ![Přidání filtru](./media/understanding-cost-reports/add-filter.png)

    Značka seskupení nebo filtrování se nevztahuje na prostředky Azure nebo značky skupiny prostředků.

    Jsou k dispozici v seskupení značky přidělení nákladů a filtrování **skupiny** možnost nabídky.

4. **Skupin v sestavách**

    Použití skupin v analýza nákladů sestavy zobrazíte standard uvedeno kategorií z fakturace data v sestavě.  Na základě značky kategorií zobrazení však skupin v zobrazení sestavy přidělení nákladů. Na základě značky kategorie jsou definovány v modelu přidělení náklady a standardní rozepsané kategorií z fakturační údaje.

    ![skupiny značky](./media/understanding-cost-reports/groups-tags01.png)

    ![skupiny značky](./media/understanding-cost-reports/groups-tags02.png)

    V sestavách přidělení náklady mohou zahrnovat skupiny do kategorií na základě značky skupiny:
      - Značky
      - značky prostředku skupiny
      - Cloudyn náklady značek entit
      - Předplatné značky kategorií pro účely přidělení náklady

  Příklady mohou zahrnovat:
     - Nákladové středisko
     - Oddělení
     - Aplikace
     - Prostředí
     - Náklady kódu

5. **Filtry**

    Pomocí jednoho nebo více vyberte filtry nastavte rozsahy na vybraných hodnot. Pokud chcete nastavit filtr, klikněte na tlačítko **přidat** a potom vyberte filtr kategorií a hodnoty.

6. **Model nákladů**

    Slouží k výběru náklady na model, který jste dříve vytvořili s náklady na přidělení 360 náklady na modelu. Můžete mít více Cloudyn náklady modelů, v závislosti na požadavcích vaší náklady přidělení. Některé týmům organizace může mít náklady přidělení požadavky, které se liší od ostatních. Každý tým může mít vlastní náklady na vyhrazené modelu.

    Informace o vytváření definici náklady přidělení modelu najdete v tématu [pomocí vlastních značek přidělení náklady](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs).

7. **Amortizace**

    Použití amortizace v sestavách přidělení nákladů a zobrazte tak bez použití na základě služby poplatky nebo jednorázové závazky náklady a rozloženy rovnoměrně době jejich životnost jejich náklady. Příklady jednorázové poplatky mohou zahrnovat:
    - Roční poplatky podpory
    - Roční poplatky součásti zabezpečení
    - Vyhrazená instance zakoupit poplatků
    - Některé položky Azure Marketplace.

  V části amortizace, vyberte **amortizovaný náklady** nebo **skutečné náklady**.

8. **Řešení**

    Vyberte dobu řešení v rámci vybraného rozsahu dat pomocí řešení. Rozlišení vašeho doba určuje, jak se zobrazují v sestavě jednotky a může být:
    - Denně
    - Týdně
    - Měsíční
    - Čtvrtletně
    - Roční

9. **Přidělení pravidla**

    Použití přidělení pravidla použít nebo zakázat přidělení nákladů nákladů při každém přepočítání. Můžete povolit nebo zakázat přepočítání přidělení náklady pro fakturační údaje. Přepočítání se vztahuje na vybraných kategorií v sestavě. Umožňuje posoudit dopad přepočítání přidělení náklady proti nezpracovaná data fakturace.

10. **Nezařazeno do kategorie**

    Použijte Nezařazeno zahrnout nebo vyloučit Nezařazeno do kategorie náklady na v sestavě.

11. **Zobrazit/skrýt pole**

    Zobrazit nebo skrýt možnost nemá žádný vliv v sestavách.

12.   **Formáty zobrazení**

    Formáty zobrazení slouží k výběru různých zobrazení grafu nebo tabulky.

    ![formáty zobrazení](./media/understanding-cost-reports/display-formats.png)

13. **Multi-color**

    Použití více barvu, která nastavení barvy grafy v sestavě.

14. **Akce**

    Pomocí akce Uložit, exportujte nebo plánování sestavy.

## <a name="next-steps"></a>Další postup

- Pokud jste ještě nedokončili první kurz pro náklady na správu, přečtěte si ho na [zkontrolujte využití a náklady na](tutorial-review-usage.md).
