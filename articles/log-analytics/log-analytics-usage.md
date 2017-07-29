---
title: "Analýza využití dat v Log Analytics | Dokumentace Microsoftu"
description: "Pomocí řídicího panelu Využití v Log Analytics můžete zobrazit, kolik dat se odesílá do služby Log Analytics, a řešit potíže způsobující odesílání velkých objemů dat."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/21/2017
ms.author: magoedte
ms.translationtype: HT
ms.sourcegitcommit: 8021f8641ff3f009104082093143ec8eb087279e
ms.openlocfilehash: 5f57cbdb1678dd61eda449d2103125d8db83892e
ms.contentlocale: cs-cz
ms.lasthandoff: 07/21/2017

---
# <a name="analyze-data-usage-in-log-analytics"></a>Analýza využití dat v Log Analytics
Log Analytics obsahuje informace o objemu shromážděných dat, počítačích odesílajících data a různých typech odesílaných dat.  Pomocí řídicího panelu **Využití Log Analytics** můžete zobrazit objem dat odesílaných do služby Log Analytics. Řídicí panel ukazuje, kolik dat shromažďují jednotlivá řešení a kolik dat odesílají vaše počítače.

## <a name="understand-the-usage-dashboard"></a>Vysvětlení řídicího panelu Využití
Řídicí panel **Využití Log Analytics** obsahuje tyto informace:

- Objem dat
    - Objem dat v průběhu času (v závislosti na vašem aktuálním časovém rozsahu)
    - Objem dat podle řešení
    - Data, která nejsou spojena s počítačem
- Počítače
    - Počítače odesílající data
    - Počítače, které během posledních 24 hodin neodeslaly žádná data
- Nabídky
    - Uzly Insight and Analytics
    - Uzly Automation and Control
    - Uzly zabezpečení
- Výkon
    - Čas potřebný k shromáždění a indexaci dat
- Seznam dotazů

![řídicí panel využití](./media/log-analytics-usage/usage-dashboard01.png)

### <a name="to-work-with-usage-data"></a>Práce s daty o využití
1. Pokud jste to ještě neudělali, přihlaste se na webu [Azure Portal](https://portal.azure.com) pomocí svého předplatného Azure.
2. V nabídce **Centra** klikněte na **Další služby** a v seznamu prostředků zadejte **Log Analytics**. Seznam se průběžně filtruje podle zadávaného textu. Klikněte na **Log Analytics**.  
    ![Centrum Azure](./media/log-analytics-usage/hub.png)
3. Na řídicím panelu **Log Analytics** se zobrazí seznam vašich pracovních prostorů. Vyberte pracovní prostor.
4. Na řídicím panelu *pracovního prostoru* klikněte na **Využití Log Analytics**.
5. Na řídicím panelu **Využití Log Analytics** klikněte na **Čas: Posledních 24 hodin** a změňte časový interval.  
    ![časový interval](./media/log-analytics-usage/time.png)
6. Zobrazte okna kategorií využití s oblastmi, které vás zajímají. Vyberte okno a pak v něm klikněte na některou položku, abyste zobrazili další podrobnosti v [Prohledávání protokolů](log-analytics-log-searches.md).  
    ![příklad okna využití dat](./media/log-analytics-usage/blade.png)
7. Na řídicím panelu Prohledávání protokolů zkontrolujte vrácené výsledky hledání.  
    ![příklad prohledávání protokolů využití](./media/log-analytics-usage/usage-log-search.png)

## <a name="create-an-alert-when-data-collection-is-higher-than-expected"></a>Vytvoření upozornění při větším než očekávaném shromažďování dat
Tato část popisuje postup vytvoření upozornění v těchto případech:
- Objem dat překračuje zadanou velikost.
- Očekává se, že objem dat překročí zadanou velikost.

[Upozornění](log-analytics-alerts-creating.md) v Log Analytics používají vyhledávací dotazy. Následující dotaz vrátí výsledek, pokud se za posledních 24 hodin shromáždilo více než 100 GB dat:

`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(Quantity,1024)) as DataGB by Type | where DataGB > 100`

Následující dotaz pomocí jednoduchého vzorce předvídá, jestli dojde k odeslání více než 100 GB dat za den: 

`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(mul(Quantity,8),1024)) as EstimatedGB by Type | where EstimatedGB > 100`

Pokud chcete upozornit na jiný objem dat, změňte v dotazech hodnotu 100 na počet GB, na který chcete upozornit.

Pokud chcete být upozorňováni při větším než očekávaném shromažďování dat, postupujte podle kroků popsaných v tématu týkajícím se [vytvoření pravidla upozornění](log-analytics-alerts-creating.md#create-an-alert-rule).

Při vytváření upozornění pro první dotaz (více než 100 GB dat během 24 hodin) nastavte:
- **Název** na *Větší objem dat než 100 GB během 24 hodin*.
- **Závažnost** na *Upozornění*.
- **Vyhledávací dotaz** na `Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(Quantity,1024)) as DataGB by Type | where DataGB > 100`.
- **Časový interval** na *24 hodin*.
- **Četnosti upozornění** na 1 hodinu, protože se data o využití aktualizují pouze jednou za hodinu.
- **Generovat upozornění na základě** na *Počet výsledků*.
- **Počet výsledků** na *Větší než 0*.

Pomocí kroků popsaných v tématu o [přidání akcí k pravidlům upozornění](log-analytics-alerts-actions.md) nakonfigurujte pro pravidlo upozornění akci e-mailu, webhooku nebo runbooku.

Při vytváření upozornění pro druhý dotaz (předpověď, že během 24 hodin bude shromážděno více než 100 GB dat) nastavte:
- **Název** na *Očekávaný větší objem dat než 100 GB během 24 hodin*.
- **Závažnost** na *Upozornění*.
- **Vyhledávací dotaz** na `Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(mul(Quantity,8),1024)) as EstimatedGB by Type | where EstimatedGB > 100`.
- **Časový interval** na *3 hodiny*.
- **Četnosti upozornění** na 1 hodinu, protože se data o využití aktualizují pouze jednou za hodinu.
- **Generovat upozornění na základě** na *Počet výsledků*.
- **Počet výsledků** na *Větší než 0*.

Pokud obdržíte upozornění, pomocí kroků v následující části můžete řešit potíže způsobující větší využití, než se čekalo.

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Řešení potíží způsobujících větší využití, než se čekalo
Řídicí panel Využití vám pomůže určit, proč je využití (a tedy i náklady) větší, než očekáváte.

Větší využití je způsobeno jedním nebo obojím z těchto aspektů:
- Do Log Analytics se odesílá více dat, než se čekalo.
- Do Log Analytics odesílá data více uzlů, než se čekalo.

### <a name="check-if-there-is-more-data-than-expected"></a>Kontrola, jestli se shromažďuje více dat, než se čekalo 
Na stránce Využití jsou dvě klíčové části, které pomáhají určit, co způsobuje shromažďování většího objemu dat.

Graf *Objem dat v průběhu času* ukazuje celkový objem odeslaných dat a počítače odesílající nejvíce dat. V grafu v horní části můžete vidět, jestli vaše celkové využití dat roste, je konstantní nebo se snižuje. Seznam počítačů ukazuje prvních 10 počítačů odesílajících nejvíce dat.

Graf *Objem dat podle řešení* ukazuje objem dat, který odesílají jednotlivá řešení, a řešení odesílající nejvíce dat. Graf v horní části ukazuje celkový objem dat odesílaných jednotlivými řešeními v průběhu času. Pomocí těchto informací můžete určit, jestli řešení v průběhu času odesílá více dat, zhruba stejné množství dat nebo méně dat. Seznam řešení ukazuje prvních 10 řešení odesílajících nejvíce dat. 

Tyto dva grafy zobrazují veškerá data. Některá data jsou fakturovatelná, ostatní jsou zdarma. Pokud se chcete zaměřit pouze na data, která jsou fakturovatelná, změňte dotaz na vyhledávací stránce tak, aby zahrnoval `IsBillable=true`.  

![Grafy objemu dat](./media/log-analytics-usage/log-analytics-usage-data-volume.png)

Podívejte se na graf *Objem dat v průběhu času*. Pokud chcete zobrazit datové typy a řešení odesílající nejvíce dat z konkrétního počítače, klikněte na název počítače. Klikněte na název prvního počítače v seznamu.

Na následujícím snímku obrazovky odesílá z počítače nejvíce dat datový typ *Správa protokolů / Výkon*. 

![objem dat pro počítač](./media/log-analytics-usage/log-analytics-usage-data-volume-computer.png)

Potom se vraťte na řídicí panel *Využití* a podívejte se na graf *Objem dat podle řešení*. Pokud chcete zobrazit počítače odesílající nejvíce dat pro řešení, klikněte na název řešení v seznamu. Klikněte na název prvního řešení v seznamu. 

Na následujícím snímku obrazovky je potvrzení, že počítač *acmetomcat* odesílá nejvíce dat pro řešení Správa protokolů.

![objem dat pro řešení](./media/log-analytics-usage/log-analytics-usage-data-volume-solution.png)

V případě potřeby proveďte další analýzy k identifikaci velkých objemů v rámci řešení nebo datového typu. Ukázky dotazů:

+ Řešení **zabezpečení**
  - `Type=SecurityEvent | measure count() by EventID`
+ Řešení **pro správu protokolů**
  - `Type=Usage Solution=LogManagement IsBillable=true | measure count() by DataType`
+ Datový typ **Perf**
  - `Type=Perf | measure count() by CounterPath`
  - `Type=Perf | measure count() by CounterName`
+ Datový typ **Event**
  - `Type=Event | measure count() by EventID`
  - `Type=Event | measure count() by EventLog, EventLevelName`
+ Datový typ **Syslog**
  - `Type=Syslog | measure count() by Facility, SeverityLevel`
  - `Type=Syslog | measure count() by ProcessName`

Pomocí následujících kroků snižte objem shromažďovaných protokolů:

| Zdroj velkého objemu dat | Postup snížení objemu dat |
| -------------------------- | ------------------------- |
| Události zabezpečení            | Vyberte [běžné nebo minimální události zabezpečení](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/). <br> Změňte zásady auditu zabezpečení tak, aby se shromažďovaly jenom potřebné události. Zaměřte se hlavně na potřebu shromažďovat události pro <br> - [audit platformy Filtering Platform](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [audit registru](https://docs.microsoft.com/windows/device-security/auditing/audit-registry)<br> - [audit systému souborů](https://docs.microsoft.com/windows/device-security/auditing/audit-file-system)<br> - [audit objektu jádra](https://docs.microsoft.com/windows/device-security/auditing/audit-kernel-object)<br> - [audit manipulace s popisovačem](https://docs.microsoft.com/windows/device-security/auditing/audit-handle-manipulation)<br> - [audit vyměnitelného úložiště](https://docs.microsoft.com/windows/device-security/auditing/audit-removable-storage) |
| Čítače výkonu       | Změňte [konfiguraci čítačů výkonu](log-analytics-data-sources-performance-counters.md) tak, aby se: <br> – Snížila četnost shromažďování dat <br> – Snížil počet čítačů výkonu |
| Protokoly událostí                 | Změňte [konfiguraci protokolů událostí](log-analytics-data-sources-windows-events.md) tak, aby se: <br> – Snížil počet shromažďovaných protokolů událostí <br> – Shromažďovaly pouze požadované úrovně událostí Například zrušte shromažďování událostí úrovně *Informace*. |
| Syslog                     | Změňte [konfiguraci syslogu](log-analytics-data-sources-syslog.md) tak, aby se: <br> – Snížil počet zařízení, ze kterých se shromažďují data <br> – Shromažďovaly pouze požadované úrovně událostí Například zrušte shromažďování událostí úrovně *Informace* a *Ladění*. |
| Data řešení z počítačů, které řešení nepotřebují | Použijte [cílení na řešení](../operations-management-suite/operations-management-suite-solution-targeting.md) a shromažďujte data pouze z požadované skupiny počítačů. |

### <a name="check-if-there-are-more-nodes-than-expected"></a>Kontrola, jestli existuje více uzlů, než se čekalo
Pokud jste na cenové úrovni *za uzel (OMS)*, pak se vám bude účtovat v závislosti na počtu používaných uzlů a řešení. V části *nabídek* řídicího panelu využití můžete zobrazit, kolik uzlů každé nabídky se používá.

![řídicí panel využití](./media/log-analytics-usage/log-analytics-usage-offerings.png)

Kliknutím na **Zobrazit všechno...** zobrazte úplný seznam počítačů odesílajících data do vybrané nabídky.

Použijte [cílení na řešení](../operations-management-suite/operations-management-suite-solution-targeting.md) a shromažďujte data pouze z požadované skupiny počítačů.


## <a name="next-steps"></a>Další kroky
* V tématu [Prohledávání protokolů v Log Analytics](log-analytics-log-searches.md) zjistíte, jak používat jazyk vyhledávání. Pomocí vyhledávacích dotazů můžete na datech o využití provádět další analýzy.
* Pokud chcete být upozorňováni při splnění kritéria vyhledávání, postupujte podle kroků popsaných v tématu týkajícím se [vytvoření pravidla upozornění](log-analytics-alerts-creating.md#create-an-alert-rule).
* Použijte [cílení na řešení](../operations-management-suite/operations-management-suite-solution-targeting.md) a shromažďujte data jenom z požadované skupiny počítačů.
* Vyberte [běžné nebo minimální události zabezpečení](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/).
* Změňte [konfiguraci čítačů výkonu](log-analytics-data-sources-performance-counters.md).
* Změňte [konfiguraci protokolů událostí](log-analytics-data-sources-windows-events.md).
* Změňte [konfiguraci syslogu](log-analytics-data-sources-syslog.md).

