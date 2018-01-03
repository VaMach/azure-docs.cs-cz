---
title: "Vytvářet, zobrazovat a spravovat výstrahy pomocí Azure monitorování – výstrahy (Preview) | Microsoft Docs"
description: "Pomocí nové sjednocené Azure výstrahy prostředí vytvořit, zobrazit a spravovat metrika a protokolu pravidla výstrah na jednom místě."
author: msvijayn
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: vinagara
ms.openlocfilehash: e3902ec5fc27c829fa97042d15552c8c895c6408
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/19/2017
---
# <a name="create-view-and-manage-alerts-using-azure-monitor---alerts-preview"></a>Vytvoření, zobrazení a Správa výstrah pomocí Azure monitorování – výstrahy (Preview)

## <a name="overview"></a>Přehled
Tento článek ukazuje, jak nastavit výstrah pomocí rozhraní nové výstrahy (Preview) v portálu Azure. Definice pravidla výstrahy se tří částí:
- Cíl: Konkrétní prostředků Azure, který má být monitorován
- Kritéria: Určité podmínky nebo logiku, když zobrazená v signál, by měly aktivovat akce
- Akce: Odeslané konkrétní volání k příjemce oznámení – e-mailu, SMS, webhooku atd.

Výstrahy (Preview) používá termín **protokolu výstrahy** k popisu výstrahy, kde je signál vlastní dotaz na základě [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md). Metriky výstrahy schopností volat [téměř v reálném čase metrika výstrahy](monitoring-near-real-time-metric-alerts.md) v existující výstrahy zkušeností se označuje jako **metrika výstrahy** ve výstrahách (Preview). V *metrika výstrahy*, poskytovat některé typy prostředků [multidimenzionální metriky](monitoring-metric-charts.md) pro konkrétní prostředků Azure a proto výstrahy na takových prostředků můžete provedeny konkrétnější pomocí další filtry na dimenze; Tyto výstrahy jsou označovány jako **Multi-Dimensional metrika výstrahy**. Výstrahy Azure (Preview) také poskytuje jednotný pohled na všechna pravidla výstrah a schopnost spravovat na jednom místě; včetně zobrazení žádné nevyřešené výstrahy. Další informace o funkcích z [Alerts(Preview) Azure – přehled](monitoring-overview-unified-alerts.md).

> [!NOTE]
> Při výstrahách Azure (Preview) nabízí nové a vylepšené prostředí pro vytváření výstrah v Azure. Existující [Azure výstrahy](monitoring-overview-alerts.md) zůstávají použitelné prostředí
>

Podrobné následující je podrobný návod k používání výstrah Azure (Preview).

## <a name="create-an-alert-rule-with-the-azure-portal"></a>Pravidlo výstrahy vytvořit pomocí portálu Azure
1. V [portál](https://portal.azure.com/), vyberte **monitorování** a v části monitorování – zvolte **výstrahy (Preview)**.  
    ![Monitorování](./media/monitor-alerts-unified/AlertsPreviewMenu.png)

2. Vyberte **nové pravidlo výstrahy** tlačítko Vytvořit nové oznámení v Azure.
    ![Přidání oznámení](./media/monitor-alerts-unified/AlertsPreviewOption.png)

3. V části Vytvoření upozornění se zobrazí se tří částí, který se skládá z: *definovat výstrahy podmínku*, *definovat podrobnosti výstrahy*, a *definovat akce skupiny*.
    ![Vytvoření pravidla](./media/monitor-alerts-unified/AlertsPreviewAdd.png)

4.  Definovat podmínka upozornění první pomocí **vyberte prostředek** odkaz a určení cílového výběrem prostředku. Filtr správně výběrem potřeby *předplatné*, *typ prostředku*, a nakonec vyberte požadované *prostředků*. Jak výstrahy Azure (Preview) umožňuje vytvoření různých typů výstrah z jednotném rozhraní; na základě typu výstrahy potřeby vyberte jako další krok:
    - Pro **metrika výstrahy**: postupujte podle kroků 5 až 7; přejít přímo na krok 11
    - Pro **protokolu výstrahy**, přejít ke kroku 8 a vyšší

5. *Metriky výstrahy*: Zkontrolujte **typ prostředku** je vybraná služba platformy nebo monitorování (jiné než *analýzy protokolů*), pak jednou odpovídající **prostředků** je Klikněte na tlačítko Vybrat *provádí* tlačítko se vrátíte k vytvoření výstrahy. Potom použít **přidat kritéria** tlačítko můžete vybrat ze seznamu možnosti signál, jejich monitorování služby a typ seznamu - určitý signál, které jsou k dispozici pro prostředek vybrali dříve.
    ![Vyberte prostředek](./media/monitor-alerts-unified/AlertsPreviewResourceSelection.png)

    > [!NOTE]
    > Nové metriky funkce zavedená pro rychlé výstrahy, jsou součástí signál typy jako metriky ze služby platformy
  

6. *Metriky výstrahy*: po signál zaškrtnuto, lze stanovit, logiku pro výstrahy. Pro odkaz, se zobrazí historických dat signálem s možností pro upravit současně pomocí okna **zobrazit historii**, různých z posledních šest hodin k minulého týdne. S vizualizaci na místě **výstrahy logiku** je možné vybrat z zobrazené možnosti podmínku, agregace a nakonec prahovou hodnotu. Jako verze preview poskytuje logiku podmínku se zobrazí v vizualizaci společně s historie signál, o tom, kdy by mít výstraha. Nakonec zadejte pro jaké doby trvání, by měla vypadat výstrahy pro zadanou podmínku tak, že zvolíte **období** možnost společně s jak často se má spouštět výstraha výběrem **frekvence**.
    ![Konfigurovat logiku signál pro metriku](./media/monitor-alerts-unified/AlertsPreviewCriteria.png)

7. *Metriky výstrahy*: Pokud signál je metriky a pak výstrahy okna je možné filtrovat pomocí několika datových bodů nebo dimenze pro dané prostředků Azure. Podobně jako u metriky výstrahy, vizualizace historie signál lze zvolit uvádí trvání **zobrazit historii** rozevíracího seznamu. Kromě toho lze vybrat dimenze pro zvolené Metrika pro filtrování pro požadované časové řady; Výběr dimenze je volitelný a až pět dimenze mohou být použity. **Výstrahy logiku** je možné vybrat z zobrazené možnosti podmínku, agregace a nakonec prahovou hodnotu. Jako verze preview poskytuje logiku podmínku se zobrazí v vizualizaci společně s signál historie signalizují, že výstraha by byla spuštěna v minulosti. Nakonec zadejte pro jaké doby trvání, by měla vypadat výstrahy pro zadanou podmínku tak, že zvolíte **období** možnost společně s jak často se má spouštět výstraha výběrem **frekvence**.
    ![Konfigurovat logiku signál pro multidimenzionální metrika](./media/monitor-alerts-unified/AlertsPreviewCriteriaMultiDim.png)

8. *Protokolování výstrahy*: Ujistěte se, **typ prostředku** je zdrojem analytics jako *analýzy protokolů*, pak jednou odpovídající **prostředků** je zvolený klikněte na tlačítko *Provádí* tlačítko. Potom použít **přidat kritéria** tlačítko Zobrazit seznam možností signál k dispozici pro daný prostředek a ze seznamu signál **hledání protokolů vlastní** možnost.
   ![Vyberte prostředek – vlastní protokol hledání](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog.png)

9.  *Přihlaste se výstrahy*: Po výběru, v lze stanovit dotazu pro zobrazení výstrah **vyhledávací dotaz** pole; Pokud syntaxe dotazu je nesprávný poli se zobrazí chyba červeně. Pokud syntaxe dotazu je správný – historických dat stanovené dotazu pro referenci zobrazí jako graf s možností pro upravit časový interval z posledních šest hodin minulého týdne. 
   ![Konfigurace pravidla výstrah](./media/monitor-alerts-unified/AlertsPreviewAlertLog.png)

    > [!NOTE]
    > Vizualizaci historických dat lze zobrazit pouze pokud výsledky dotazu obsahovat podrobnosti času. Pokud dotaz výsledkem souhrnná data nebo konkrétní sloupec hodnoty - stejné zobrazen singulární vykreslení. 

10.  *Protokolování výstrahy*: S vizualizaci na místě, **výstrahy logiku** je možné vybrat z zobrazené možnosti podmínku, agregace a nakonec prahovou hodnotu. Nakonec zadejte v logiku, čas pro vyhodnocení zadanou podmínku pomocí **období** možnost. Společně s jak často se má spouštět výstraha výběrem **frekvence**.
Pro **protokolu výstrahy** výstrahy může být založené na:
   - *Počet záznamů*: výstraha se vytvoří, pokud je počet záznamů vrácených dotazem větší nebo menší než zadaná hodnota.
   - *Metriky měření*: vytváří výstrahu, pokud každý *agregovat hodnotu* ve výsledcích překračuje prahovou hodnotu zadaná a je *seskupené podle* vybrali hodnotu. Počet porušení u výstraha je počet, který je prahová hodnota ve zvoleném časovém období. Můžete určit celkový počet porušení u libovolné kombinace narušení přes sadu výsledků nebo po sobě jdoucích narušení tak, aby vyžadovala, musí dojít k porušení v po sobě jdoucích vzorků.

   Další informace o [protokolu výstrahy a jejich typy](monitor-alerts-unified-log.md)

    > [!TIP]
    > Aktuálně ve výstrahách (Preview) – oznámení hledání protokolu může trvat vlastní *období* a *frekvence* hodnota v minuty. Hodnoty se může lišit od 5 minut až 1 440 minut (tj.) 24 hodin. Takže pokud chcete výstrahu limit vyslovte tři hodiny, převeďte ho do minut – 180 minut, než použití

11. Druhým krokem, definujte název upozornění v **název pravidla výstrahy** pole spolu s **popis** s podrobnostmi o podrobností pro výstrahu a **závažnost** z hodnoty poskytuje možnosti. Tyto podrobnosti jsou opakovaně v všechny výstrahy e-mailů, oznámení nebo nabízené provádí monitorování Azure. Kromě toho uživatel okamžitě aktivovat pravidlo výstrahy na vytváření správně přepnutím **pravidlo povolit po vytvoření** možnost.

Pro **protokolu výstrahy** pouze některé další funkce je k dispozici v podrobnostech výstrahy:
- *Potlačit upozornění*: po zapnutí potlačení pro pravidlo výstrahy akce pravidla jsou zakázány definované dobu, po vytvoření nová výstraha. Pravidlo je stále spuštěná a vytváří výstrahy záznamy zadaný splnění kritérií. Povolení je čas k odstranění problému bez spuštění duplicitní akce.
    ![Potlačit oznámení pro výstrahy protokolu](./media/monitor-alerts-unified/AlertsPreviewSuppress.png)

12. Jako třetí a poslední krok, zadejte případných **akce skupiny** musí aktivuje pro pravidlo výstrahy, když je splněna podmínka výstrahy. Můžete vybrat všechny existující skupiny akce s výstraha nebo vytvořte novou skupinu akce. Podle vybrané akce skupiny, když je výstraha bude aktivace Azure: Odeslat email(s), odeslání SMS(s), volání Webhook(s), opravit pomocí Azure Runbooků, nabízené ITSM nástroj, atd. Další informace o [skupiny akcí](monitoring-action-groups.md).

Pro **protokolu výstrahy** některé další funkce, je možné přepsat výchozí akce:
- *Oznámení e-mailem*: přepsání předmět e-mailem, odeslána prostřednictvím akce skupiny. Tělo e-mailu se nedá změnit.
- *Zahrnout vlastní datovou část Json*: přepsání webhooku Json používané skupiny akcí a místo toho nahraďte vlastní datovou část datové části výchozí ![akce přepsání pro výstrahy protokolu](./media/monitor-alerts-unified/AlertsPreviewOverrideLog.png)

13. Pokud všechna pole jsou platné a s zelená značka **vytvořit pravidlo výstrahy** kliknutí na tlačítko a výstraha je vytvořen v Azure monitorování – výstrahy (Preview). Všechny výstrahy lze zobrazit na řídicím panelu výstrahy (Preview).
    ![Vytvoření pravidla](./media/monitor-alerts-unified/AlertsPreviewCreate.png)

Během několika minut výstraha je aktivní a jak se popisuje výš se aktivuje.

## <a name="view-your-alerts-in-azure-portal"></a>Zobrazit upozornění na portálu Azure
1. V [portál](https://portal.azure.com/), vyberte **monitorování** a v části monitorování – zvolte **výstrahy (Preview)**.  

2. **Výstrahy řídicí panel (Preview)** se zobrazí – ve kterém jsou všechny výstrahy Azure unified a zobrazí v jednotném čísle Tabule ![výstrahy řídicí panel](./media/monitoring-overview-unified/alerts-preview-overview.png)
1. Z horní části zleva doprava řídicího panelu ukazuje na první pohled, následující – které sloužící k najdete podrobné informace:
    - *Aktivováno výstrahy*: počet aktuálně výstrahy, které se mají splněna logiku a v aktivováno stavu
    - *Celkový počet pravidla výstrah*: číslo vytvořit pravidla výstrah a v subtext, číslo, které jsou nyní zapnuta
2. Se zobrazí seznam všechny aktivní výstrahy, které může uživatel kliknutím zobrazíte podrobnosti
3. Pomoc v konkrétní hledání výstrah; možnosti rozevírací seznam v horní části jeden můžete použít pro filtrování konkrétní *předplatné, skupinu prostředků nebo prostředek*. Další pro žádné nevyřešené výstrahy, jedno použití *upozornění filtru* možnost Najít pro zadaný – klíčové slovo - konkrétní odpovídající výstrahy s *název, výstraha kritéria, skupinu prostředků a cílový prostředek*

## <a name="managing-your-alerts-in-azure-portal"></a>Správa upozornění na portálu Azure
1. V [portál](https://portal.azure.com/), vyberte **monitorování** a v části monitorování – zvolte **výstrahy (Preview)**.  
2. Vyberte **spravovat pravidla** tlačítko na horním panelu přejděte do části Správa pravidlo – kde jsou uvedeny všechny výstrahy pravidla vytvořená; včetně výstrahy, které byly zakázány.
3. Pro konkrétní pravidla výstrah najdete jednu buď pomocí rozevíracího seznamu filtrů v horní části, které umožňují shortlist pravidla výstrah pro konkrétní *předplatné, skupiny prostředků nebo prostředek*. Můžete také na pomocí vyhledávání označena podokně nad seznamu pravidlo výstrahy *filtrování výstrah*, jeden může poskytnout – klíčové slovo, které je shoda na základě *název výstrahy, podmínku a cílový prostředek*; Chcete-li zobrazit pouze odpovídající pravidla. 
   ![Spravovat pravidla výstrah](./media/monitoring-overview-unified/alerts-preview-rules.png)
4. Chcete-li zobrazit nebo upravit konkrétní pravidlo výstrahy, klikněte na jeho název, který se bude zobrazovat jako prokliknutelný odkaz.
5. Se zobrazí výstraha definované – struktura tři fáze: 1) výstraha podmínka výstrahy 2) podrobností (3) akci skupiny. **Cíl kritéria** sloužící k úpravě logice výstrah nebo novou po použití na ikonu koše odstranit starší logiku lze přidat kritéria. Podobně, v části Podrobnosti výstrahy - **popis** a **závažnost** je možné upravit. A skupinu akce lze změnit nebo může být tak nový, aby propojení na výstrahy pomocí **nové skupiny akce** tlačítko ![upravit pravidlo výstrah](./media/monitor-alerts-unified/AlertsPreviewEdit.png)
6. Pomocí horním panelu, změny výstrahy může být odrážející včetně: *Uložit* potvrzení změny provést s výstrahou, *zahodit* a vraťte se bez potvrzení změny provedené v výstrahy *zakázat*  deaktivovat výstrahu - po jejímž uplynutí ho už běží nebo se aktivuje žádnou akci. A nakonec *odstranit* trvale odeberete celý pravidlo výstrahy z Azure.

## <a name="next-steps"></a>Další kroky

* Další informace o nové [téměř v reálném čase metriky výstrahy (preview)](monitoring-near-real-time-metric-alerts.md)
* Získat [přehled metriky kolekce](insights-how-to-customize-monitoring.md) zkontrolovat služby je k dispozici a dobře reagovaly.
* Další informace o [protokolu výstrahy ve výstrahách Azure (preview)](monitor-alerts-unified-log.md)
