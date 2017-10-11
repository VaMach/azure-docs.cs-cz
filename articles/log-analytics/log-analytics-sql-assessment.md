---
title: "Optimalizace prostředí systému SQL Server s nástrojem Azure Log Analytics | Microsoft Docs"
description: "S Azure Log Analytics můžete řešení pro vyhodnocení SQL pro vyhodnocení rizik a stavu prostředí serveru SQL v pravidelných intervalech."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e297eb57-1718-4cfe-a241-b9e84b2c42ac
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2aed3315fe60ace46dfb4176dc13aa417257b0c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="optimize-your-sql-server-environment-with-the-sql-assessment-solution-in-log-analytics"></a>Optimalizace prostředí SQL serveru do řešení pro vyhodnocení SQL v analýzy protokolů

![Symbol vyhodnocení SQL](./media/log-analytics-sql-assessment/sql-assessment-symbol.png)

Řešení pro vyhodnocení SQL můžete použít k vyhodnocení rizik a stavu serveru prostředí v pravidelných intervalech. Tento článek vám pomůže nainstalovat řešení tak, že lze provádět opravné akce pro potenciální problémy.

Toto řešení poskytuje seznam doporučení, které jsou specifické pro infrastrukturu nasazené serverů seřazený podle priority. Doporučení jsou rozdělené mezi šest konkrétní oblasti, které vám pomůžou rychle pochopit riziko a proveďte opravné akce.

Doporučení, která jsou založené na znalosti a zkušenosti technici Microsoft z tisíce zákazníka návštěvách. Každé doporučení obsahuje informace, proč může pro vás důležitá problém a implementaci navrhované změny.

Můžete vybrat konkrétní oblasti, které jsou důležité pro vaši organizaci a sledovat průběh směrem k spuštění prostředí riziko volné a v pořádku.

Poté, co jste přidali řešení a posouzení je dokončené, souhrnné informace pro konkrétní oblasti je zobrazený na **vyhodnocení SQL** řídicí panel infrastruktury ve vašem prostředí. Následující části popisují, jak používat informace na **vyhodnocení SQL** řídicí panel, kde můžete zobrazit a pak proveďte doporučené akce pro vaši infrastrukturu serveru SQL.

![Obrázek dlaždice vyhodnocení SQL](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![bitové kopie řídicího panelu, vyhodnocení SQL](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-the-solution"></a>Instalace a konfigurace řešení
Vyhodnocení SQL pracuje s všechny aktuálně podporované verze systému SQL Server v edicích Standard, Developer a Enterprise.

Použijte následující informace k instalaci a konfiguraci řešení.

* Na serverech, které mají nainstalovaný server SQL Server je nutné nainstalovat agenty.
* Řešení pro vyhodnocení SQL vyžaduje podporovanou verzi rozhraní .NET Framework 4 nainstalován na každý počítač, který má OMS agent.
* Abyste mohli nainstalovat řešení, musí být uživatel správce nebo Přispěvatel k předplatnému Azure při použití portálu Azure. Navíc musí být uživatel členem skupiny přispěvatelů pracovního prostoru OMS nebo mít roli administrátora na portálu OMS.
* Při použití agenta nástroje Operations Manager s vyhodnocení SQL, musíte použít účet Operations Manager Run-As. V tématu [účty nástroje Operations Manager spustit jako pro OMS](#operations-manager-run-as-accounts-for-oms) níže Další informace.

  > [!NOTE]
  > MMA agent nepodporuje účty Operations Manager Run-As.
  >
  >
* Přidat do pracovního prostoru OMS pomocí postupu popsaného v řešení pro vyhodnocení SQL [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md). Není nutná žádná další konfigurace.

> [!NOTE]
> Po přidání řešení, soubor AdvisorAssessment.exe je přidán na servery s agenty. Konfigurační data je čtení a pak se odešle službě OMS v cloudu pro zpracování. Logika se použije pro přijatá data a cloudové služby zaznamenává data.

## <a name="sql-assessment-data-collection-details"></a>Podrobnosti kolekce dat vyhodnocení SQL
Vyhodnocení SQL shromažďuje data rozhraní WMI, registru dat, údaje o výkonu a výsledky zobrazení dynamické správy SQL Server pomocí agentů, které jste povolili.

Následující tabulka uvádí metody shromažďování dat pro agenty, jestli se vyžaduje Operations Manager (SCOM) a jak často data jsou shromažďována agentem.

| Platforma | Přímé agenta | Agenta nástroje SCOM | Azure Storage | SCOM vyžaduje? | Data agenta SCOM odeslána prostřednictvím skupiny pro správu | Frekvence kolekce |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  | &#8226; |7 dní |

## <a name="operations-manager-run-as-accounts-for-oms"></a>Účty spustit jako nástroje Operations Manager pro OMS
Analýzy protokolů v OMS skupinu pro správu a agent nástroje Operations Manager používá ke shromažďování a odesílání dat do služby OMS. Sestavení OMS na sady management Pack pro úlohy zajistit přidanou hodnotou services. Každé zatížení vyžaduje určeného pro konkrétní úlohy oprávnění ke spouštění sad management Pack v odlišném kontextu zabezpečení, jako je například účet domény. Budete muset zadat přihlašovací údaje nakonfigurováním Operations Manager účet Spustit jako.

K nastavení účet Spustit jako nástroje Operations Manager pro vyhodnocení SQL použijte následující informace.

### <a name="set-the-run-as-account-for-sql-assessment"></a>Nastavit účet Spustit jako pro vyhodnocení SQL
 Pokud už používáte sadu management pack systému SQL Server, používejte tento účet Spustit jako.

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a>Jak nakonfigurovat účet Spustit jako SQL v konzoli Operations console
> [!NOTE]
> Pokud používáte přímé agenta OMS, nikoli agenta nástroje SCOM, sada management pack vždy běží v kontextu zabezpečení účtu místního systému. Přeskočit kroky 1 až 5 níže a spusťte buď T-SQL nebo ukázce prostředí Powershell zadáním NT AUTHORITY\SYSTEM jako uživatelské jméno.
>
>

1. V nástroji Operations Manager, otevřete konzoli Operations console a pak klikněte na tlačítko **správy**.
2. V části **konfigurace spustit jako**, klikněte na tlačítko **profily**a otevřete **OMS SQL Assessment profilu spustit jako**.
3. Na **účty spustit jako** klikněte na tlačítko **přidat**.
4. Vyberte účet Spustit jako systému Windows, který obsahuje přihlašovacích údajů nezbytných pro systém SQL Server, nebo klikněte na **nový** k jeho vytvoření.

   > [!NOTE]
   > Typ účtu spustit jako musí být Windows. Účet Spustit jako musí být také součástí místní skupiny Administrators na všech serverech Windows, který je hostitelem instance serveru SQL.
   >
   >
5. Klikněte na **Uložit**.
6. Upravit a potom spusťte následující ukázka T-SQL na každou instanci systému SQL Server k udělení minimální oprávnění potřebná k provedení vyhodnocení SQL pro účet Spustit jako. Ale nemusíte to dělat, když se účet Spustit jako už je součástí role serveru sysadmin na instance systému SQL Server.

```
---
    -- Replace <UserName> with the actual user name being used as Run As Account.
    USE master

    -- Create login for the user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions to user.
    GRANT VIEW SERVER STATE TO [<UserName>]
    GRANT VIEW ANY DEFINITION TO [<UserName>]
    GRANT VIEW ANY DATABASE TO [<UserName>]

    -- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
    -- NOTE: This command must be run anytime new databases are added to SQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="to-configure-the-sql-run-as-account-using-windows-powershell"></a>Jak nakonfigurovat účet Spustit jako SQL pomocí prostředí Windows PowerShell
Otevřete okno prostředí PowerShell a spusťte následující skript po aktualizaci s vaše informace:

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"

    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a>Pochopení, jak budou doporučení mít vyšší prioritu
Každé doporučení je zadána hodnota vyvážení, která určuje relativní důležitost doporučení. Zobrazí se jenom deset nejdůležitějších doporučení.

### <a name="how-weights-are-calculated"></a>Jak jsou vypočítávány váhu
Váhy jsou agregovaných hodnot založena na tři klíčové faktory:

* *Pravděpodobnosti* způsobí, že problém identifikovat problémy. Vyšší pravděpodobnost rovná větší celkové skóre pro doporučení.
* *Dopad* problému na vaší organizaci, pokud ji způsobovat problémy. Vyšší dopad rovná větší celkové skóre pro doporučení.
* *Úsilí* potřebnou k implementaci doporučení. Vyšší úsilí rovná menší celkové skóre pro doporučení.

Vyvážení pro každé doporučení je vyjádřený jako procentní podíl celkové skóre, které jsou k dispozici pro každou oblast fokus. Například pokud doporučení v oblasti zabezpečení a dodržování předpisů fokus je skóre % 5, implementace tímto doporučením zvýší celkové skóre podle 5 % zabezpečení a dodržování předpisů.

### <a name="focus-areas"></a>Konkrétní oblasti
**Zabezpečení a dodržování předpisů** – v tomto poli fokus zobrazí doporučení pro potenciální bezpečnostní hrozby a narušení, podnikové zásady a dodržování předpisů technických, právních i regulačních požadavků.

**Dostupnost a provozní kontinuita** – v tomto poli fokus zobrazí doporučení pro dostupnost služeb, odolnost vaší infrastruktury a obchodní ochrany.

**Výkon a škálovatelnost** – v tomto poli fokus zobrazí doporučení, která pomůžou vaší organizace IT infrastruktury růst, zajistěte, aby vaše IT prostředí splňuje aktuální požadavky na výkon a schopné reagovat na měnící se infrastruktury potřebuje.

**Upgrade, nasazení a migrace** – v tomto poli fokus zobrazí doporučení, která vám pomohou upgradovat, migrace a nasazení systému SQL Server k vaší stávající infrastruktury.

**Operace a monitorování** – v tomto poli fokus zobrazí doporučení, která pomůžou zjednodušení operací IT a implementaci preventivní údržby, maximalizovat výkon.

**Správa změn a konfigurací** – v tomto poli fokus zobrazí doporučení pomáhají chránit každodenní operace, zkontrolujte, zda nemáte změny negativně ovlivnit vaši infrastrukturu, vytvořit procedury řízení změn a ke sledování a monitorování Konfigurace systému.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Mají usilovat o stanovení skóre 100 % v každé oblasti fokus?
Ne nutně. Doporučení jsou založené na prostředí, které nebyly získány prostřednictvím Microsoft technici napříč tisíce návštěvy zákazníka a znalostní báze. Ale žádné serverové infrastruktury jsou stejné, a konkrétní doporučení může být vyšší nebo nižší relevantní pro vás. Například může být několik doporučení zabezpečení méně důležité, pokud vaše virtuální počítače nejsou vystaveny v Internetu. Některá doporučení, jaké dostupnosti může být méně důležité pro služby, které poskytují kolekce s nízkou prioritou ad hoc dat a vytváření sestav. Problémy, které jsou důležité, abyste vyspělých firmy může být méně důležité Startup. Můžete určit, které konkrétní oblasti mají vašich priorit a podívejte se na tom, jak se vaše skóre časem změnit.

Každé doporučení obsahuje pokyny o tom, proč je důležité. Měli byste použít tyto pokyny k vyhodnocení, zda implementace doporučení je vhodné pro vás, daná povaze vašich IT služeb a obchodním potřebám vaší organizace.

## <a name="use-assessment-focus-area-recommendations"></a>Použít assessment fokus oblasti doporučení
Než v OMS můžete použít řešení pro vyhodnocení, musíte mít nainstalován řešení. Další informace o instalaci řešení, najdete v části [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md). Po instalaci, zobrazí se souhrn doporučení pomocí dlaždice vyhodnocení SQL na stránce Přehled v OMS.

Zobrazte vyhodnocování souhrnné dodržování předpisů pro infrastrukturu a potom přejít k podrobnostem doporučení.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Proveďte opravné akce a zobrazit doporučení pro oblastí zájmu
1. Na **přehled** klikněte na tlačítko **vyhodnocení SQL** dlaždici.
2. Na **vyhodnocení SQL** zkontrolujte souhrnné informace v jednom z okna oblasti fokus a pak klikněte na jednu zobrazíte doporučení pro tuto oblast fokus.
3. Na všech stránkách oblasti fokus můžete zobrazit seřazený podle priority doporučení, která se pro vaše prostředí. Klikněte na tlačítko doporučení v části **vliv na objekty** Chcete-li zobrazit podrobnosti, proč se provádí doporučení.  
    ![Obrázek doporučení vyhodnocení SQL](./media/log-analytics-sql-assessment/sql-assess-focus.png)
4. Můžete provést nápravné akce navržený v **doporučované akce**. Když položka vyřeší, bude záznam novější vyhodnocování které doporučené akce provedené a zvýší se vaše skóre dodržování předpisů. Opravené položky se zobrazí jako **předán objekty**.

## <a name="ignore-recommendations"></a>Ignorovat doporučení
Pokud máte doporučení, které chcete ignorovat, můžete vytvořit textový soubor, který OMS použije k zabránění doporučení ze storu ve výsledky hodnocení.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="to-identify-recommendations-that-you-will-ignore"></a>K identifikaci doporučení, které se budou ignorovat.
1. Přihlaste se do pracovního prostoru a otevřete vyhledávání protokolu. Pro počítače ve vašem prostředí použijte následující dotaz, který seznam doporučení, které selhaly.

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```

   Zde je snímek obrazovky zobrazující protokolu vyhledávací dotaz: ![doporučení se nezdařilo](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)
2. Zvolte doporučení, které chcete ignorovat. Hodnoty pro RecommendationId budete používat v dalším postupu.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Vytváření a používání textový soubor s IgnoreRecommendations.txt
1. Vytvořte soubor s názvem IgnoreRecommendations.txt.
2. Vložit nebo zadejte každou RecommendationId pro jednotlivá doporučení, které chcete OMS ignorovat na samostatném řádku a potom uložte a zavřete soubor.
3. Uložte soubor v následující složce na každém počítači, kam chcete OMS ignorovat doporučení.
   * Na počítačích s Microsoft Monitoring Agent (připojené přímo nebo prostřednictvím nástroje Operations Manager) - *SystemDrive*: \Program Files\Microsoft Agent\Agent monitorování
   * Na serveru pro správu nástroje Operations Manager - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Chcete-li ověřit, že se ignorovat doporučení
1. Po na další naplánované vyhodnocení běží ve výchozím nastavení každých 7 dní, zadaný doporučení jsou označeny ignorovaná a nezobrazí se na řídicím panelu hodnocení.
2. Následující dotazy hledání protokolů můžete použít k zobrazení seznamu všech ignorováno doporučení.

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
3. Pokud se později rozhodnete, zda chcete zobrazit ignorováno doporučení, odeberte všechny soubory IgnoreRecommendations.txt nebo RecommendationIDs můžete odebrat z nich.

## <a name="sql-assessment-solution-faq"></a>Řešení pro vyhodnocení SQL – nejčastější dotazy
*Jak často posouzení spustit?*

* Posouzení spouští každých 7 dní.

*Existuje způsob, jak nakonfigurovat, jak často se hodnocení spouštět?*

* V tuto chvíli to není možné.

*Pokud je zjištěno jiný server, poté, co byly přidány řešení pro vyhodnocení SQL, bude ho vyhodnocena?*

* Ano, jakmile se zjistí, že se hodnotí z pak, každých 7 dní.

*Pokud dojde k deaktivaci serveru, když ho se odebere z hodnocení?*

* Pokud server není odesílání dat 3 týdny, bude odebrán.

*Jaký je název procesu, který nemá shromažďování dat?*

* AdvisorAssessment.exe

*Jak dlouho trvá na shromáždění dat?*

* Kolekce skutečná data na serveru trvá asi 1 hodina. Může trvat déle na serverech, které mají velký počet instancí SQL nebo databáze.

*Jaký typ dat shromažďovaných?*

* Se shromažďují následující typy dat:
  * ROZHRANÍ WMI
  * Registru
  * Čítače výkonu
  * Zobrazení dynamické správy SQL (DMV).

*Existuje způsob, jak nakonfigurovat, když jsou shromažďována data?*

* V tuto chvíli to není možné.

*Proč je nutné nakonfigurovat účet Spustit jako?*

* Pro systém SQL Server se spouštějí na malý počet dotazů SQL. Aby, aby se spouštěly musíte použít účet Spustit jako s oprávnění VIEW SERVER STATE do systému SQL.  Kromě toho aby bylo možné odeslat dotaz rozhraní WMI, jsou požadované přihlašovací údaje místního správce.

*Proč zobrazit pouze prvních 10 doporučení?*

* Namísto udělení vyčerpávající čtenáře seznam úloh, doporučujeme můžete soustředit na první adresování seřazený podle priority doporučení. Po jejich řešení, bude k dispozici další doporučení. Pokud ji chcete zobrazit podrobný seznam, můžete zobrazit všechna doporučení pomocí vyhledávání protokolu OMS.

*Existuje způsob, jak ignorovat doporučení?*

* Ano, najdete v části [ignorovat doporučení](#ignore-recommendations) část výše.

## <a name="next-steps"></a>Další kroky
* [V protokolech Hledat](log-analytics-log-searches.md) zobrazíte doporučení a podrobné údaje o vyhodnocení SQL.
