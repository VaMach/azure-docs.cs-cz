---
title: "Optimalizace prostředí služby Active Directory s Azure Log Analytics | Microsoft Docs"
description: "Active Directory Assessment řešení můžete použít k vyhodnocení rizik a stavu serveru prostředí v pravidelných intervalech."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 81eb41b8-eb62-4eb2-9f7b-fde5c89c9b47
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 97368f0b9e89ffd0cd982b6e8670d5a1f62ad42c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="optimize-your-active-directory-environment-with-the-active-directory-assessment-solution-in-log-analytics"></a>Optimalizace prostředí služby Active Directory s řešením Active Directory Assessment v analýzy protokolů

![Symbol hodnocení AD](./media/log-analytics-ad-assessment/ad-assessment-symbol.png)

Active Directory Assessment řešení můžete použít k vyhodnocení rizik a stavu serveru prostředí v pravidelných intervalech. Tento článek vám pomůže nainstalovat a použít řešení tak, že lze provádět opravné akce pro potenciální problémy.

Toto řešení poskytuje seznam doporučení, které jsou specifické pro infrastrukturu nasazené serverů seřazený podle priority. Doporučení jsou rozdělené mezi čtyři konkrétní oblasti, které vám pomůžou rychle pochopit riziko a provést akci.

Doporučení jsou založené na znalosti a zkušenosti technici Microsoft z tisíce zákazníka návštěvách. Každé doporučení obsahuje informace, proč může pro vás důležitá problém a implementaci navrhované změny.

Můžete vybrat konkrétní oblasti, které jsou důležité pro vaši organizaci a sledovat průběh směrem k spuštění prostředí riziko volné a v pořádku.

Když jste přidali řešení a posouzení je dokončené, souhrnné informace pro konkrétní oblasti je zobrazena na **hodnocení AD** řídicí panel infrastruktury ve vašem prostředí. Následující části popisují, jak používat informace na **hodnocení AD** řídicí panel, kde můžete zobrazit a pak proveďte doporučené akce pro serverové infrastruktury služby Active Directory.

![Obrázek dlaždice vyhodnocení SQL](./media/log-analytics-ad-assessment/ad-tile.png)

![bitové kopie řídicího panelu, vyhodnocení SQL](./media/log-analytics-ad-assessment/ad-assessment.png)

## <a name="installing-and-configuring-the-solution"></a>Instalace a konfigurace řešení
Použijte následující informace k instalaci a konfiguraci řešení.

* Na řadičích domény, které jsou členy domény, který se má vyhodnotit, je nutné nainstalovat agenty.
* Active Directory Assessment řešení vyžaduje podporovanou verzi rozhraní .NET Framework 4 (4.5.2 nebo novější) nainstalován na každý počítač, který má OMS agent.
* Přidat řešení Active Directory Assessment do pracovního prostoru OMS z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ADAssessmentOMS?tab=Overview) nebo pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).  Není nutná žádná další konfigurace.

  > [!NOTE]
  > Po přidání řešení, soubor AdvisorAssessment.exe je přidán na servery s agenty. Konfigurační data je čtení a pak se odešle službě OMS v cloudu pro zpracování. Logika se použije pro přijatá data a cloudové služby zaznamenává data.
  >
  >

## <a name="active-directory-assessment-data-collection-details"></a>Informace o kolekci datových Active Directory hodnocení

Active Directory Assessment shromažďuje data z agentů, které jste povolili pomocí následujících zdrojů:

- Kolektory registru
- Kolektory LDAP
- Rozhraní .NET framework
- Kolektory protokolů událostí
- Služba Active Directory rozhraní (ADSI)
- Windows PowerShell
- Kolekce dat souboru
- Windows Management Instrumentation (WMI)
- Nástroj DCDIAG rozhraní API
- Rozhraní API služby (NTFRS) replikace souborů
- Kód vlastní C#


Následující tabulka uvádí metody shromažďování dat pro agenty, jestli se vyžaduje Operations Manager (SCOM) a jak často data jsou shromažďována agentem.

| Platforma | Přímé agenta | Agenta nástroje SCOM | Azure Storage | SCOM vyžaduje? | Data agenta SCOM odeslána prostřednictvím skupiny pro správu | Frekvence kolekce |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |&#8226; |&#8226; |  |  |&#8226; |7 dní |

## <a name="understanding-how-recommendations-are-prioritized"></a>Pochopení, jak budou doporučení mít vyšší prioritu
Každé doporučení je zadána hodnota vyvážení, která určuje relativní důležitost doporučení. Jsou zobrazeny pouze 10 nejdůležitějších doporučení.

### <a name="how-weights-are-calculated"></a>Jak jsou vypočítávány váhu
Váhy jsou agregovaných hodnot založena na tři klíčové faktory:

* *Pravděpodobnosti* , způsobuje problémy, problém identifikovat. Vyšší pravděpodobnost rovná větší celkové skóre pro doporučení.
* *Dopad* problému na vaší organizaci, pokud ji způsobovat problémy. Vyšší dopad rovná větší celkové skóre pro doporučení.
* *Úsilí* potřebnou k implementaci doporučení. Vyšší úsilí rovná menší celkové skóre pro doporučení.

Vyvážení pro každé doporučení je vyjádřený jako procentní podíl celkové skóre, které jsou k dispozici pro každou oblast fokus. Například pokud doporučení v oblasti zabezpečení a dodržování předpisů fokus je skóre % 5, implementace tímto doporučením zvyšuje celkové skóre podle 5 % zabezpečení a dodržování předpisů.

### <a name="focus-areas"></a>Konkrétní oblasti
**Zabezpečení a dodržování předpisů** – v tomto poli fokus zobrazí doporučení pro potenciální bezpečnostní hrozby a narušení, podnikové zásady a dodržování předpisů technických, právních i regulačních požadavků.

**Dostupnost a provozní kontinuita** – v tomto poli fokus zobrazí doporučení pro dostupnost služeb, odolnost vaší infrastruktury a obchodní ochrany.

**Výkon a škálovatelnost** – v tomto poli fokus zobrazí doporučení, která pomůžou vaší organizace IT infrastruktury růst, zajistěte, aby vaše IT prostředí splňuje aktuální požadavky na výkon a schopné reagovat na měnící se infrastruktury potřebuje.

**Upgrade, nasazení a migrace** – v tomto poli fokus zobrazí doporučení, která vám pomohou upgradovat, migrace a nasazení služby Active Directory vaší stávající infrastruktuře.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Mají usilovat o stanovení skóre 100 % v každé oblasti fokus?
Ne nutně. Doporučení jsou založené na prostředí, které nebyly získány prostřednictvím Microsoft technici napříč tisíce návštěvy zákazníka a znalostní báze. Ale žádné serverové infrastruktury jsou stejné, a konkrétní doporučení může být vyšší nebo nižší relevantní pro vás. Například může být několik doporučení zabezpečení méně důležité, pokud vaše virtuální počítače nejsou vystaveny v Internetu. Některá doporučení, jaké dostupnosti může být méně důležité pro služby, které poskytují kolekce s nízkou prioritou ad hoc dat a vytváření sestav. Problémy, které jsou důležité, abyste vyspělých firmy může být méně důležité Startup. Můžete určit, které konkrétní oblasti mají vašich priorit a podívejte se na tom, jak se vaše skóre časem změnit.

Každé doporučení obsahuje pokyny o tom, proč je důležité. Měli byste použít tyto pokyny k vyhodnocení, zda implementace doporučení je vhodné pro vás, daná povaze vašich IT služeb a obchodním potřebám vaší organizace.

## <a name="use-assessment-focus-area-recommendations"></a>Použít assessment fokus oblasti doporučení
Než v OMS můžete použít řešení pro vyhodnocení, musíte mít nainstalován řešení. Další informace o instalaci řešení, najdete v části [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md). Po instalaci, zobrazí se souhrn doporučení pomocí dlaždice hodnocení na stránce Přehled v OMS.

Zobrazte vyhodnocování souhrnné dodržování předpisů pro infrastrukturu a potom přejít k podrobnostem doporučení.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Proveďte opravné akce a zobrazit doporučení pro oblastí zájmu
1. Na **přehled** klikněte na tlačítko **Assessment** dlaždici pro váš server infrastruktury.
2. Na **Assessment** zkontrolujte souhrnné informace v jednom z okna oblasti fokus a pak klikněte na jednu zobrazíte doporučení pro tuto oblast fokus.
3. Na všech stránkách oblasti fokus můžete zobrazit seřazený podle priority doporučení, která se pro vaše prostředí. Klikněte na tlačítko doporučení v části **vliv na objekty** Chcete-li zobrazit podrobnosti, proč se provádí doporučení.  
    ![Obrázek doporučení pro interní hodnocení](./media/log-analytics-ad-assessment/ad-focus.png)
4. Můžete provést nápravné akce navržený v **doporučované akce**. Když položka byla řešit, novější vyhodnocování záznamy, které doporučené akce provedené a zvýší vaše skóre dodržování předpisů. Opravené položky se zobrazí jako **předán objekty**.

## <a name="ignore-recommendations"></a>Ignorovat doporučení
Pokud máte doporučení, které chcete ignorovat, můžete vytvořit textový soubor, který OMS použije k zabránění doporučení ze storu ve výsledky hodnocení.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>K identifikaci doporučení, které se budou ignorovat.
1. Přihlaste se do pracovního prostoru a otevřete vyhledávání protokolu. Pro počítače ve vašem prostředí použijte následující dotaz, který seznam doporučení, které selhaly.

   ```
   Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
>[!NOTE]
> Pokud pracovní prostor byl upgradován na verzi [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak výše uvedeném dotazu by změnit na následující.
>
> `ADAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

   Zde je snímek obrazovky zobrazující protokolu vyhledávací dotaz: ![doporučení se nezdařilo](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)
2. Zvolte doporučení, které chcete ignorovat. Hodnoty pro RecommendationId budete používat v dalším postupu.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Vytváření a používání textový soubor s IgnoreRecommendations.txt
1. Vytvořte soubor s názvem IgnoreRecommendations.txt.
2. Vložit nebo zadejte každou RecommendationId pro jednotlivá doporučení, které chcete analýzy protokolů ignorovat na samostatném řádku a potom uložte a zavřete soubor.
3. Uložte soubor v následující složce na každém počítači, kam chcete OMS ignorovat doporučení.
   * Na počítačích s Microsoft Monitoring Agent (připojené přímo nebo prostřednictvím nástroje Operations Manager) - *SystemDrive*: \Program Files\Microsoft Agent\Agent monitorování
   * Na serveru pro správu nástroje Operations Manager - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Chcete-li ověřit, že se ignorovat doporučení
Po na další naplánované vyhodnocení běží ve výchozím nastavení každých 7 dní, jsou označené zadanou doporučení *ignorovaná* a nezobrazí se na řídicím panelu hodnocení.

1. Následující dotazy hledání protokolů můžete použít k zobrazení seznamu všech ignorováno doporučení.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
>[!NOTE]
> Pokud pracovní prostor byl upgradován na verzi [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak výše uvedeném dotazu by změnit na následující.
>
> `ADAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

2. Pokud se později rozhodnete, zda chcete zobrazit ignorováno doporučení, odeberte všechny soubory IgnoreRecommendations.txt nebo RecommendationIDs můžete odebrat z nich.

## <a name="ad-assessment-solutions-faq"></a>Řešení hodnocení AD – nejčastější dotazy
*Jak často posouzení spustit?*

* Posouzení spouští každých 7 dní.

*Existuje způsob, jak nakonfigurovat, jak často se hodnocení spouštět?*

* V tuto chvíli to není možné.

*Pokud jiný server pro je zjištěno po byly přidány řešení pro vyhodnocení, bude ho vyhodnocena?*

* Ano, jakmile se zjistí, že se hodnotí z pak, každých 7 dní.

*Pokud dojde k deaktivaci serveru, když ho se odebere z hodnocení?*

* Pokud server není odesílání dat 3 týdny, bude odebrán.

*Jaký je název procesu, který nemá shromažďování dat?*

* AdvisorAssessment.exe

*Jak dlouho trvá na shromáždění dat?*

* Kolekce skutečná data na serveru trvá asi 1 hodina. Může trvat déle na serverech, které mají velký počet servery služby Active Directory.

*Existuje způsob, jak nakonfigurovat, když jsou shromažďována data?*

* V tuto chvíli to není možné.

*Proč zobrazit pouze prvních 10 doporučení?*

* Namísto udělení vyčerpávající čtenáře seznam úloh, doporučujeme můžete soustředit na první adresování seřazený podle priority doporučení. Po jejich řešení, bude k dispozici další doporučení. Pokud ji chcete zobrazit podrobný seznam, můžete zobrazit všechna doporučení pomocí hledání protokolů.

*Existuje způsob, jak ignorovat doporučení?*

* Ano, najdete v části [ignorovat doporučení](#ignore-recommendations) část výše.

## <a name="next-steps"></a>Další kroky
* Použití [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md) zobrazíte podrobné dat hodnocení AD a doporučení.
