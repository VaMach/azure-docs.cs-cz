---
title: "Pomocí portálu hledání protokolů v Azure Log Analytics | Microsoft Docs"
description: "Tento článek obsahuje kurz, který popisuje, jak vytvořit protokolu hledání a analyzovat data uložená v pracovním prostoru analýzy protokolů pomocí portálu hledání protokolů.  Tento kurz zahrnuje spustíte pár jednoduchých dotazů vrátit různé typy dat a analýza výsledků."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: bwren
ms.openlocfilehash: 3a2e8803d51d81ab0eda3dc814d01822e17bc14e
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
# <a name="create-log-searches-in-azure-log-analytics-using-the-log-search-portal"></a>Vytvoření protokolu hledání v Azure Log Analytics pomocí portálu hledání protokolů

> [!NOTE]
> Tento článek popisuje portálu hledání protokolů v Azure Log Analytics jazykem nový dotaz.  Můžete získat další informace o nový jazyk a získat postup upgradu pracovního prostoru v [upgradu pracovní prostor analýzy protokolů Azure na nové hledání protokolu](log-analytics-log-search-upgrade.md).  
>
> Pokud pracovní prostor nebyla upgradována, aby nové dotazovací jazyk, se seznamte s [najít data pomocí protokolu hledání v analýzy protokolů](log-analytics-log-searches.md) informace o aktuální verzi portálu hledání protokolů.

Tento článek obsahuje kurz, který popisuje, jak vytvořit protokolu hledání a analyzovat data uložená v pracovním prostoru analýzy protokolů pomocí portálu hledání protokolů.  Tento kurz zahrnuje spustíte pár jednoduchých dotazů vrátit různé typy dat a analýza výsledků.  Zaměřuje se na funkce na portálu hledání protokolů pro úpravy dotazu a nikoli změny přímo.  Podrobnosti o přímou úpravou dotazu, najdete v článku [referenční příručka jazyka dotazů](https://go.microsoft.com/fwlink/?linkid=856079).

Vytvoření vyhledávání v portálu pokročilé analýzy místo portálu vyhledávání protokolu naleznete v tématu [Začínáme s portálu analýza](https://go.microsoft.com/fwlink/?linkid=856587).  Oba Portály pomocí stejné dotazovací jazyk přístup ke stejným datům v pracovním prostoru analýzy protokolů.

## <a name="prerequisites"></a>Požadavky
Tento kurz předpokládá, že už máte pracovní prostor analýzy protokolů s alespoň jeden připojený zdroj, který generuje data pro dotazy k analýze.  

- Pokud nemáte pracovní prostor, můžete vytvořit volné jeden pomocí postupu v [začít pracovat s pracovní prostor analýzy protokolů](log-analytics-get-started.md).
- Připojit aspoň jeden [agenta Windows](log-analytics-windows-agent.md) nebo jednu [agenta systému Linux](log-analytics-linux-agents.md) do pracovního prostoru.  

## <a name="open-the-log-search-portal"></a>Otevřete portál hledání protokolů
Začněte otevřením portálu hledání protokolů. 

1. Otevřete web Azure Portal.
2. Přejděte k analýze protokolů a vyberte pracovní prostor.
3. Vyberte **protokolu vyhledávání**.

![Tlačítko vyhledat protokolu](media/log-analytics-log-search-log-search-portal/log-search-button.png)

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

![Jednoduchý dotaz](media/log-analytics-log-search-log-search-portal/log-search-portal-01.png)

Zobrazí se pouze první několik vlastnosti každý záznam.  Klikněte na tlačítko **zobrazit další** zobrazíte všechny vlastnosti pro konkrétní záznam.

![Podrobnosti záznamu](media/log-analytics-log-search-log-search-portal/log-search-portal-02.png)

## <a name="set-the-time-scope"></a>Nastavit obor čas
Každý záznam shromažďují analýzy protokolů má **TimeGenerated** vlastnost, která obsahuje datum a čas, který byl vytvořen záznam.  Dotaz na portálu hledání protokolů pouze vrátí záznamy s **TimeGenerated** v rámci oboru čas, který se zobrazí na levé straně obrazovky.  

Filtr času můžete změnit tak, že vyberete rozevíracího seznamu nebo změnou posuvníku.  Posuvník Zobrazí pruhový graf, který se zobrazuje číslo relativní záznamů pro každý segment čas v rozsahu.  Tento segment se bude lišit v závislosti na rozsahu.

Rozsah výchozí doba je **1 den**.  Změna této hodnoty na **7 dní**, a celkový počet záznamů měli zvýšit.

![Datum čas oboru](media/log-analytics-log-search-log-search-portal/log-search-portal-03.png)

## <a name="filter-results-of-the-query"></a>Filtrování výsledků dotazu
Na levé straně obrazovky je podokno filtru, která umožňuje filtrování chcete do dotazu přidat bez změny přímo.  Několik vlastností vrácené záznamy jsou zobrazit s jejich hodnoty prvních deset s jejich počet záznamů.

Pokud pracujete s **událostí**, zaškrtněte políčko vedle **chyba** pod **EVENTLEVELNAME**.   Pokud pracujete s **Syslog**, zaškrtněte políčko vedle **chyba** pod **úroveň ZÁVAŽNOSTI**.  Tato operace změní dotaz na jednu z těchto omezit výsledky do chybové události.

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filtr](media/log-analytics-log-search-log-search-portal/log-search-portal-04.png)

Přidání vlastnosti do podokna filtru tak, že vyberete **přidat do filtry** na jeden ze záznamů v nabídce vlastnost.

![Přidání do nabídky filtru](media/log-analytics-log-search-log-search-portal/log-search-portal-02a.png)

Stejný filtr můžete nastavit tak, že vyberete **filtru** z nabídky vlastnost pro záznam s hodnotou, který chcete filtrovat.  

Můžete mít pouze **filtru** možnost Vlastnosti s jejich název modře.  Jedná se o *prohledávatelné* pole, které jsou indexované pro podmínky vyhledávání.  Pole šedě jsou *volné text prohledávatelné* pole, které mají jenom **zobrazit odkazy** možnost.  Tato možnost vrátí záznamy, které mají tuto hodnotu v libovolné vlastnosti.

![Filtr nabídky](media/log-analytics-log-search-log-search-portal/log-search-portal-01a.png)

Výsledky na jedinou vlastností můžete seskupovat podle výběru **Seskupit podle** možnost v nabídce záznam.  Bude přidáno [shrnout](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) operátor do dotazu, který zobrazí výsledky v grafu.  Můžete seskupit na více než jednu vlastnost, ale budete muset upravit dotaz přímo.  Vyberte nabídku záznam Další **počítače** vlastnost a vyberte **Seskupit podle "Počítač"**.  

![Seskupit podle počítače](media/log-analytics-log-search-log-search-portal/log-search-portal-10.png)

## <a name="work-with-results"></a>Práce s výsledky
Portál vyhledávání protokolu obsahuje řadu funkcí pro práci s výsledky dotazu.  Můžete řadit, filtr a výsledky skupiny k analýze dat bez úpravy skutečné dotazu.  Ve výchozím nastavení nejsou seřazeny výsledků dotazu.

Chcete-li zobrazit data v tabulce formulář, který nabízí další možnosti pro filtrování a řazení, klikněte na tlačítko **tabulky**.  

![Zobrazení tabulky](media/log-analytics-log-search-log-search-portal/log-search-portal-05.png)

Klikněte na šipku podle záznam zobrazíte podrobnosti pro tento záznam.

![Řazení výsledků](media/log-analytics-log-search-log-search-portal/log-search-portal-06.png)

Řazení v některém poli kliknutím na záhlaví sloupce.

![Řazení výsledků](media/log-analytics-log-search-log-search-portal/log-search-portal-07.png)

Filtrování výsledků na konkrétní hodnotu ve sloupci klepnutím na tlačítko filtru a poskytnutím podmínku filtrování.

![Filtrování výsledků](media/log-analytics-log-search-log-search-portal/log-search-portal-08.png)

Skupiny na sloupci, přetáhněte záhlaví sloupce na začátek výsledky.  Přetažením více sloupců do horní části můžete seskupit více polí.

![Výsledky skupiny](media/log-analytics-log-search-log-search-portal/log-search-portal-09.png)



## <a name="work-with-performance-data"></a>Práce s data výkonu
Údaje o výkonu pro systém Windows a Linux agentů je uložen v prostoru analýzy protokolů **výkonu** tabulky.  Zaznamenává výkonu vypadají stejně jako jakýkoli jiný záznam a jsme může zapisovat jednoduchý dotaz, který vrátí že všechny záznamy výkonu stejně jako s událostmi.

```
Perf
```

![Údaje o výkonu](media/log-analytics-log-search-log-search-portal/log-search-portal-11.png)

Vrácení miliony záznamů pro všechny objekty výkonu a čítače, když není velmi užitečné.  Můžete použít stejné metody, které jste použili výše a filtrujte data nebo právě zadejte následující dotaz přímo do vyhledávacího pole protokolu.  Tento příkaz vrátí jenom procesoru záznamů využití pro počítače se systémy Windows a Linux.

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![Využití procesoru](media/log-analytics-log-search-log-search-portal/log-search-portal-12.png)

Toto nastavení omezuje data na konkrétní čítače, ale je stále není pro něj formulář, který je obzvláště užitečné.  Můžete zobrazit data ve spojnicovém grafu, ale nejprve skupiny tak, že počítač a TimeGenerated.  K seskupení více polí, je nutné upravit dotaz přímo, takže upravit dotaz pro následující.  Tato služba využívá [průměr](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) na fungovat **přepočtené** vlastnost pro výpočet průměrné hodnoty přes každou hodinu.

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Data grafu výkonu](media/log-analytics-log-search-log-search-portal/log-search-portal-13.png)

Teď, když vhodně seskupuje data, můžete zobrazit jeho v visual graf tak, že přidáte [vykreslení](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operátor.  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Spojnicový graf](media/log-analytics-log-search-log-search-portal/log-search-portal-14.png)

## <a name="next-steps"></a>Další postup

- Další informace o analýzy protokolů dotazovací jazyk v [Začínáme s portálu analýza](https://go.microsoft.com/fwlink/?linkid=856079).
- Provede kurz pomocí [Advanced Analytics portál](https://go.microsoft.com/fwlink/?linkid=856587) která umožňuje spouštět stejné dotazy a přístup ke stejným datům jako portál hledání protokolů.
