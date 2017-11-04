---
title: "Zobrazit a analyzovat shromažďovaných dat Azure Log Analytics | Microsoft Docs"
description: "Tento článek obsahuje kurz, který popisuje, jak vytvořit protokolu hledání a analyzovat data uložená v prostředku Log Analytics pomocí portálu hledání protokolů.  Tento kurz zahrnuje spustíte pár jednoduchých dotazů vrátit různé typy dat a analýza výsledků."
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/26/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: dfcbb925a16ca1e53d10b7bf70d03e62bc9dae69
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2017
---
# <a name="view-or-analyze-data-collected-with-log-analytics-log-search"></a>Zobrazit a analyzovat data shromážděná pomocí hledání protokolů analýzy protokolů

V analýzy protokolů můžete využít vyhledávání protokolu vytvořením dotazy k analýze shromážděná data, používat existující řídicí panely, které můžete přizpůsobit s grafické zobrazení nejcennější hledání.  Teď, když jste definovali shromažďování provozních dat od protokoly aktivity a virtuální počítače Azure, v tomto kurzu zjistíte, jak:

> [!div class="checklist"]
> * Upgrade na novou dotazovací jazyk prostředku Azure Log Analytics 
> * Jednoduché hledání dat událostí a pomocí funkce upravit a filtrování výsledků 
> * Naučte se pracovat s daty výkonu

Pokud chcete provést v příkladu v tomto kurzu, musí mít existující virtuální počítač [připojené k pracovní prostor analýzy protokolů](log-analytics-quick-collect-azurevm.md).  

Vytváření a úpravy dotazů, kromě interaktivně práce vrácená data, může být provedené jeden ze dvou způsobů.  Pro základní dotazy pomocí protokolu hledání stránky na portálu Azure nebo pro pokročilé dotazování, můžete použít portál pokročilou analýzu. Další informace o rozdílu ve funkcích mezi dva portály najdete v tématu [portály pro vytváření a úpravy dotazů protokolu v Azure Log Analytics](log-analytics-log-search-portals.md)

V tomto kurzu jsme pracovat s protokolu Search na portálu Azure. 

## <a name="log-in-to-azure-portal"></a>Přihlaste se k portálu Azure
Přihlaste se k portálu Azure v [https://portal.azure.com](https://portal.azure.com). 

## <a name="open-the-log-search-portal"></a>Otevřete portál hledání protokolů 
Začněte otevřením portálu hledání protokolů.   

1. Na portálu Azure klikněte na tlačítko **další služby** v levém dolním nalezen. V seznamu prostředků zadejte **Log Analytics**. Seznam se průběžně filtruje podle zadávaného textu. Vyberte **protokolu analýzy**.
2. V podokně analýzy protokolů předplatných, vyberte pracovní prostor a pak vyberte **hledání protokolů** dlaždici.<br> ![Tlačítko vyhledat protokolu](media/log-analytics-tutorial-viewdata/azure-portal-01.png)

Jste si všimli hlavičky v horní části stránky analýzy protokolů prostředků na portálu pozvání můžete upgradovat.<br> ![Upozornění na portálu Azure na upgrade analýzy protokolů](media/log-analytics-tutorial-viewdata/log-analytics-portal-upgradebanner.png)

Analýzy protokolů pro usnadňují vytvořit dotazy díky nedávno zavést nové dotazovací jazyk, correlate data z různých zdrojů a analyzovat rychle identifikovat trendy a problémy.

Upgrade je jednoduché.  Spustit proces kliknutím na informační zprávě, která uvádí, že **Další informace a upgradujte**.  Si přečíst další informace o upgradu na stránce informace o upgradu a potom klikněte na **upgradovat nyní**.

Proces bude trvat několik minut a během této doby můžete sledovat průběh v části **oznámení** z nabídky. Další informace o [výhod nového dotazovací jazyk](log-analytics-log-search-upgrade.md#why-the-new-language).

## <a name="create-a-simple-search"></a>Vytvoření jednoduché hledání
Nejrychlejší způsob, jak načíst některá data pro práci s je jednoduchý dotaz, který vrátí všechny záznamy v tabulce.  Pokud máte jakékoli klienti systému Windows nebo Linux připojení do pracovního prostoru, pak budete mít data v událostí (Windows) nebo tabulka Syslog (Linux).

Zadejte jednu následující dotazy do vyhledávacího pole a klikněte na tlačítko Hledat.  

```
Event
```
```
Syslog
```

Data jsou vrácena ve výchozím zobrazení seznamu a můžete zobrazit celkový počet záznamů vrácených.

![Jednoduchý dotaz](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-01.png)

Zobrazí se pouze první několik vlastnosti každý záznam.  Klikněte na tlačítko **zobrazit další** zobrazíte všechny vlastnosti pro konkrétní záznam.

## <a name="filter-results-of-the-query"></a>Filtrování výsledků dotazu
Na levé straně obrazovky je podokno filtru, která umožňuje filtrování chcete do dotazu přidat bez změny přímo.  Zobrazí se několik záznamů vlastnosti pro tento typ záznamu a můžete vybrat jeden nebo více hodnot vlastností, které chcete-li zúžit výsledky vyhledávání.

Pokud pracujete s **událostí**, zaškrtněte políčko vedle **chyba** pod **EVENTLEVELNAME**.   Pokud pracujete s **Syslog**, zaškrtněte políčko vedle **chyba** pod **úroveň ZÁVAŽNOSTI**.  Tato operace změní dotaz na jednu z těchto omezit výsledky do chybové události.

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filtr](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-02.png)

Přidání vlastnosti do podokna filtru tak, že vyberete **přidat do filtry** na jeden ze záznamů v nabídce vlastnost.

![Přidání do nabídky filtru](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-03.png)

Stejný filtr můžete nastavit tak, že vyberete **filtru** z nabídky vlastnost pro záznam s hodnotou, který chcete filtrovat.  

Můžete mít pouze **filtru** možnost Vlastnosti s jejich název modře po přesunutí ukazatele myši je.  Jedná se o *prohledávatelné* pole, které jsou indexované pro podmínky vyhledávání.  Pole šedě jsou *volné text prohledávatelné* pole, které mají jenom **zobrazit odkazy** možnost.  Tato možnost vrátí záznamy, které mají tuto hodnotu v libovolné vlastnosti.

Výsledky na jedinou vlastností můžete seskupovat podle výběru **Seskupit podle** možnost v nabídce záznam.  Bude přidáno [shrnout](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) operátor do dotazu, který zobrazí výsledky v grafu.  Můžete seskupit na více než jednu vlastnost, ale budete muset upravit dotaz přímo.  Vyberte nabídku záznam Další **počítače** vlastnost a vyberte **Seskupit podle "Počítač"**.  

![Seskupit podle počítače](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-04.png)

## <a name="work-with-results"></a>Práce s výsledky
Portál vyhledávání protokolu obsahuje řadu funkcí pro práci s výsledky dotazu.  Můžete řadit, filtr a výsledky skupiny k analýze dat bez úpravy skutečné dotazu.  Ve výchozím nastavení nejsou seřazeny výsledků dotazu.

Chcete-li zobrazit data v tabulce formulář, který nabízí další možnosti pro filtrování a řazení, klikněte na tlačítko **tabulky**.  

![Zobrazení tabulky](media/log-analytics-tutorial-viewdata/log-search-portal-table-01.png)

Klikněte na šipku podle záznam zobrazíte podrobnosti pro tento záznam.

![Řazení výsledků](media/log-analytics-tutorial-viewdata/log-search-portal-table-02.png)

Řazení v některém poli kliknutím na záhlaví sloupce.

![Řazení výsledků](media/log-analytics-tutorial-viewdata/log-search-portal-table-03.png)

Filtrování výsledků na konkrétní hodnotu ve sloupci klepnutím na tlačítko filtru a poskytnutím podmínku filtrování.

![Filtrování výsledků](media/log-analytics-tutorial-viewdata/log-search-portal-table-04.png)

Skupiny na sloupci, přetáhněte záhlaví sloupce na začátek výsledky.  Přetažením více sloupců do horní části můžete seskupit více polí.

![Výsledky skupiny](media/log-analytics-tutorial-viewdata/log-search-portal-table-05.png)


## <a name="work-with-performance-data"></a>Práce s data výkonu
Údaje o výkonu pro systém Windows a Linux agentů je uložen v prostoru analýzy protokolů **výkonu** tabulky.  Zaznamenává výkonu vypadají stejně jako jakýkoli jiný záznam, a přidáme pomocí jednoduchého dotazu, která vrátí že všechny záznamy výkonu stejně jako s událostmi.

```
Perf
```

![Údaje o výkonu](media/log-analytics-tutorial-viewdata/log-analytics-portal-perfsearch-01.png)

Vrácení miliony záznamů pro všechny objekty výkonu a čítače, když není velmi užitečné.  Můžete použít stejné metody, které jste použili výše a filtrujte data nebo právě zadejte následující dotaz přímo do vyhledávacího pole protokolu.  Tento příkaz vrátí jenom procesoru záznamů využití pro počítače se systémy Windows a Linux.

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![Využití procesoru](media/log-analytics-tutorial-viewdata/log-analytics-portal-perfsearch-02.png)

Toto nastavení omezuje data na konkrétní čítače, ale je stále není pro něj formulář, který je obzvláště užitečné.  Můžete zobrazit data ve spojnicovém grafu, ale nejprve skupiny tak, že počítač a TimeGenerated.  K seskupení více polí, je nutné upravit dotaz přímo, takže upravit dotaz pro následující.  Tato služba využívá [průměr](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) na fungovat **přepočtené** vlastnost pro výpočet průměrné hodnoty přes každou hodinu.

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Data grafu výkonu](media/log-analytics-tutorial-viewdata/log-analytics-portal-perfsearch-03.png)

Teď, když vhodně seskupuje data, můžete zobrazit jeho v visual graf tak, že přidáte [vykreslení](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operátor.  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Spojnicový graf](media/log-analytics-tutorial-viewdata/log-analytics-portal-linechart-01.png)

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste zjistili, jak vytvořit základní protokolu hledání k analýze dat událostí a výkonu.  Přechodu na v dalším kurzu se dozvíte, jak k vizualizaci dat vytvořením řídicího panelu.

> [!div class="nextstepaction"]
> [Vytvářet a sdílet řídicí panely analýzy protokolů](log-analytics-tutorial-dashboards.md)