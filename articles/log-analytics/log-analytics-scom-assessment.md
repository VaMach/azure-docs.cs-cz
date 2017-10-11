---
title: "Optimalizace prostředí System Center Operations Manager s nástrojem Azure Log Analytics | Microsoft Docs"
description: "Řešení System Center Operations Manager posouzení můžete použít k vyhodnocení rizik a stavu serveru prostředí v pravidelných intervalech."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: tysonn
ms.assetid: 49aad8b1-3e05-4588-956c-6fdd7715cda1
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4992d98397da409f7c1cfbdeb40fdb0cdd0d2f19
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="optimize-your-environment-with-the-system-center-operations-manager-assessment-preview-solution"></a>Optimalizace prostředí s nástrojem System Center Operations Manager Assessment (Preview) řešení

![System Center Operations Manager Assessment symbol](./media/log-analytics-scom-assessment/scom-assessment-symbol.png)

Řešení System Center Operations Manager posouzení můžete použít k vyhodnocení rizik a stavu prostředí serveru System Center Operations Manager v pravidelných intervalech. Tento článek vám umožňuje instalovat, konfigurovat a používat řešení, takže můžete provést nápravné akce pro potenciální problémy.

Toto řešení poskytuje seznam doporučení, které jsou specifické pro infrastrukturu nasazené serverů seřazený podle priority. Doporučení jsou rozdělené mezi čtyři konkrétní oblasti, které vám pomůžou rychle pochopit riziko a proveďte opravné akce.

Doporučení, která jsou založené na znalosti a zkušenosti technici Microsoft z tisíce zákazníka návštěvách. Každé doporučení obsahuje informace, proč může pro vás důležitá problém a implementaci navrhované změny.

Můžete vybrat konkrétní oblasti, které jsou důležité pro vaši organizaci a sledovat průběh směrem k spuštění prostředí riziko volné a v pořádku.

Když jste přidali řešení a posouzení je dokončené, souhrnné informace pro konkrétní oblasti je zobrazena na **System Center Operations Manager Assessment** řídicí panel pro vaši infrastrukturu. Následující části popisují, jak používat informace na **System Center Operations Manager Assessment** řídicí panel, kde můžete zobrazit a pak proveďte doporučené akce pro vaši infrastrukturu SCOM.

![Dlaždice řešení System Center Operations Manager](./media/log-analytics-scom-assessment/scom-tile.png)

![System Center Operations Manager Assessment řídicí panel](./media/log-analytics-scom-assessment/scom-dashboard01.png)

## <a name="installing-and-configuring-the-solution"></a>Instalace a konfigurace řešení

Toto řešení funguje s Microsoft systému Operations Manager 2012 R2 a 2012 s aktualizací SP1.

Použijte následující informace k instalaci a konfiguraci řešení.

 - Než v OMS můžete použít řešení pro vyhodnocení, musíte mít nainstalován řešení. Instalaci řešení z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SCOMAssessmentOMS?tab=Overview) nebo podle pokynů v [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).

 - Po přidání řešení do pracovního prostoru, System Center Operations Manager Assessment dlaždice na řídicím panelu zobrazí zpráva vyžaduje další konfiguraci. Klikněte na dlaždici a postupujte podle kroků konfigurace uvedených na stránce

 ![Dlaždice řídicího panelu System Center Operations Manager](./media/log-analytics-scom-assessment/scom-configrequired-tile.png)

 Konfigurace nástroje System Center Operations Manager lze provést prostřednictvím skript podle kroků uvedených na stránce konfigurace řešení v OMS.

 Místo toho nakonfigurovat assessment prostřednictvím konzoly SCOM, použijte následujících kroků ve stejném pořadí
1. [Nastavit účet Spustit jako pro System Center Operations Manager hodnocení](#operations-manager-run-as-accounts-for-oms)  
2. [Konfigurace pravidla System Center Operations Manager hodnocení](#configure-the-assessment-rule)

## <a name="system-center-operations-manager-assessment-data-collection-details"></a>Informace o System Center Operations Manager assessment datových kolekce

Hodnocení nástroje System Center Operations Manager shromažďuje data rozhraní WMI, registru dat, data protokolu událostí, data nástroje Operations Manager prostřednictvím prostředí Windows PowerShell a dotazy SQL, souborů informace kolekci pomocí serveru, který jste povolili.

Následující tabulka uvádí metody shromažďování dat pro System Center Operations Manager hodnocení a jak často se data shromážděná pomocí agenta.

| Platforma | Přímé agenta | Agenta nástroje SCOM | Azure Storage | SCOM vyžaduje? | Data agenta SCOM odeslána prostřednictvím skupiny pro správu | Frekvence kolekce |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | | | | &#8226; | | sedm dní |

## <a name="operations-manager-run-as-accounts-for-oms"></a>Účty spustit jako nástroje Operations Manager pro OMS

OMS staví na sady management Pack pro úlohy zajistit přidanou hodnotou services. Každé zatížení vyžaduje určeného pro konkrétní úlohy oprávnění ke spouštění sad management Pack v odlišném kontextu zabezpečení, jako je například účet domény. Konfigurace Operations Manager účet Spustit jako a zadejte přihlašovací údaje.

K nastavení účet Spustit jako nástroje Operations Manager pro System Center Operations Manager Assessment použijte následující informace.

### <a name="set-the-run-as-account"></a>Nastavit účet Spustit jako

1. V konzole nástroje Operations Manager, přejděte do **správy** kartě.
2. V části **konfigurace spustit jako**, klikněte na tlačítko **účty**.
3. Vytvořte účet Spustit jako, následující pomocí Průvodce vytvoření účtu systému Windows. Účet, který chcete použít, je ten, identifikovat a má následující požadavky:

    >[!NOTE]
    Účet Spustit jako, musí splňovat následující požadavky:
    - Domény účtu, členem místní skupiny Administrators na všech serverech v prostředí (všechny Operations Manager role - Server pro správu, databáze OpsMgr, datového skladu, vytváření sestav, webové konzole, brány)
    - Role správce Manager operace pro skupinu pro správu probíhá vyhodnocení
    - Spuštění [skriptu](#sql-script-to-grant-granular-permissions-to-the-run-as-account) udělit oprávnění na podrobné úrovni k účtu v instanci SQL používaných nástrojem Operations Manager.
      Poznámka: Pokud tento účet již má oprávnění správce systému, potom přeskočte provádění skriptu.

4. V části **zabezpečení distribuce**, vyberte **bezpečnější**.
5. Určení serveru správy, kde je účet distribuován.
3. Vraťte se zpátky a konfigurace spustit jako a klikněte na tlačítko **profily**.
4. Vyhledejte *SCOM Assessment profil*.
5. Název profilu musí být: *Microsoft System Center Advisor SCOM Assessment profilu spustit jako*.
6. Klikněte pravým tlačítkem myši a aktualizujte jeho vlastnosti a přidejte naposledy vytvořený účet Spustit jako jste vytvořili v kroku 3.

### <a name="sql-script-to-grant-granular-permissions-to-the-run-as-account"></a>Skript SQL k udělení oprávnění na podrobné úrovni k účtu spustit jako

Spusťte následující skript SQL udělit potřebná oprávnění k účtu spustit jako v instanci SQL používaných nástrojem Operations Manager.

```
-- Replace <UserName> with the actual user name being used as Run As Account.
USE master

-- Create login for the user, comment this line if login is already created.
CREATE LOGIN [UserName] FROM WINDOWS


--GRANT permissions to user.
GRANT VIEW SERVER STATE TO [UserName]
GRANT VIEW ANY DEFINITION TO [UserName]
GRANT VIEW ANY DATABASE TO [UserName]

-- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
-- NOTE: This command must be run anytime new databases are added to SQL Server instances.
EXEC sp_msforeachdb N'USE [?]; CREATE USER [UserName] FOR LOGIN [UserName];'

Use msdb
GRANT SELECT To [UserName]
Go

--Give SELECT permission on all Operations Manager related Databases

--Replace the Operations Manager database name with the one in your environment
Use [OperationsManager];
GRANT SELECT To [UserName]
GO

--Replace the Operations Manager DatawareHouse database name with the one in your environment
Use [OperationsManagerDW];
GRANT SELECT To [UserName]
GO

--Replace the Operations Manager Audit Collection database name with the one in your environment
Use [OperationsManagerAC];
GRANT SELECT To [UserName]
GO

--Give db_owner on [OperationsManager] DB
--Replace the Operations Manager database name with the one in your environment
USE [OperationsManager]
GO
ALTER ROLE [db_owner] ADD MEMBER [UserName]

```


### <a name="configure-the-assessment-rule"></a>Konfigurace pravidla hodnocení

Sada management pack řešení System Center Operations Manager Assessment zahrnuje na pravidlo s názvem *Microsoft System Center Advisor SCOM Assessment spustit Assessment pravidlo*. Toto pravidlo je zodpovědná za spuštění hodnocení. Následující postupy použijte, pokud chcete povolit pravidlo a nastavte četnost.

Microsoft System Center Advisor SCOM Assessment spustit Assessment pravidlo je ve výchozím nastavení zakázaná. Chcete-li spustit vyhodnocení, je nutné povolit pravidlo na serveru pro správu. Pomocí následujících kroků.

#### <a name="enable-the-rule-for-a-specific-management-server"></a>Povolení pravidla pro konkrétní management server

1. V **vytváření** prostoru konzoly nástroje Operations Manager, vyhledejte pravidlo *Microsoft System Center Advisor SCOM Assessment spustit Assessment pravidlo* v **pravidla** podokno.
2. Ve výsledcích hledání vyberte ten, který obsahuje text *typu: Server pro správu*.
3. Klikněte pravým tlačítkem na pravidlo a pak klikněte na **přepsání** > **pro konkrétní objekt třídy: Server pro správu**.
4.  V seznamu serverů pro správu k dispozici vyberte server pro správu kde se má pravidlo spustit.
5.  Ujistěte se, že změníte hodnotu přepsání **True** pro **povoleno** hodnota parametru.  
    ![Přepište parametr](./media/log-analytics-scom-assessment/rule.png)

Při stále v tomto okně nakonfigurujte četnost spustit pomocí dalšího postupu.

#### <a name="configure-the-run-frequency"></a>Nastavte četnost, spuštění

Hodnocení je nakonfigurovaná pro spuštění každých 10 080 minut (nebo sedm dní), je výchozí interval. Hodnota, která má minimální hodnotu 1 440 minut (nebo jeden den), můžete přepsat. Hodnota představuje minimální časové prodlevy požadované mezi následných vyhodnocení běží. K přepsání intervalu, použijte následující postup.

1. V **vytváření** prostoru konzoly nástroje Operations Manager, vyhledejte pravidlo *Microsoft System Center Advisor SCOM Assessment spustit Assessment pravidlo* v **pravidla** podokno.
2. Ve výsledcích hledání vyberte ten, který obsahuje text *typu: Server pro správu*.
3. Klikněte pravým tlačítkem na pravidlo a pak klikněte na **přepsat pravidlo** > **pro všechny objekty třídy: Server pro správu**.
4. Změna **Interval** hodnota parametru hodnotu požadovaného intervalu pro. V následujícím příkladu je hodnota nastavena na 1 440 minut (jeden den).  
    ![Parametr interval](./media/log-analytics-scom-assessment/interval.png)  
    Pokud je hodnota nastavena na méně než 1 440 minut, se pravidlo spustí v intervalu jeden den. V tomto příkladu pravidlo ignoruje hodnota intervalu a spustí frekvencí jeden den.


## <a name="understanding-how-recommendations-are-prioritized"></a>Pochopení, jak budou doporučení mít vyšší prioritu

Každé doporučení je zadána hodnota vyvážení, která určuje relativní důležitost doporučení. Jsou zobrazeny pouze 10 nejdůležitějších doporučení.

### <a name="how-weights-are-calculated"></a>Jak jsou vypočítávány váhu

Váhy jsou agregovaných hodnot založena na tři klíčové faktory:

- *Pravděpodobnosti* způsobí, že problém identifikovat problémy. Vyšší pravděpodobnost rovná větší celkové skóre pro doporučení.
- *Dopad* problému na vaší organizaci, pokud ji způsobovat problémy. Vyšší dopad rovná větší celkové skóre pro doporučení.
- *Úsilí* potřebnou k implementaci doporučení. Vyšší úsilí rovná menší celkové skóre pro doporučení.

Vyvážení pro každé doporučení je vyjádřený jako procentní podíl celkové skóre, které jsou k dispozici pro každou oblast fokus. Například pokud doporučení v oblasti fokus dostupnost a provozní kontinuita je skóre % 5, implementace tímto doporučením zvyšuje celkové skóre podle 5 % dostupnost a provozní kontinuita.

### <a name="focus-areas"></a>Konkrétní oblasti

**Dostupnost a provozní kontinuita** – v tomto poli fokus zobrazí doporučení pro dostupnost služeb, odolnost vaší infrastruktury a obchodní ochrany.

**Výkon a škálovatelnost** – v tomto poli fokus zobrazí doporučení, která pomůžou vaší organizace IT infrastruktury růst, zajistěte, aby vaše IT prostředí splňuje aktuální požadavky na výkon a schopné reagovat na měnící se infrastruktury potřebuje.

**Nasazení, upgrade a migrace** – v tomto poli fokus zobrazí doporučení, která vám pomohou upgradovat, migrace a nasazení systému SQL Server k vaší stávající infrastruktury.

**Operace a monitorování** – v tomto poli fokus zobrazí doporučení, která pomůžou zjednodušení operací IT a implementaci preventivní údržby, maximalizovat výkon.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Mají usilovat o stanovení skóre 100 % v každé oblasti fokus?

Ne nutně. Doporučení jsou založené na prostředí, které nebyly získány prostřednictvím Microsoft technici napříč tisíce návštěvy zákazníka a znalostní báze. Ale žádné serverové infrastruktury jsou stejné, a konkrétní doporučení může být vyšší nebo nižší relevantní pro vás. Například může být několik doporučení zabezpečení méně důležité, pokud vaše virtuální počítače nejsou vystaveny v Internetu. Některá doporučení, jaké dostupnosti může být méně důležité pro služby, které poskytují kolekce s nízkou prioritou ad hoc dat a vytváření sestav. Problémy, které jsou důležité, abyste vyspělých firmy může být méně důležité Startup. Můžete určit, které konkrétní oblasti mají vašich priorit a podívejte se na tom, jak se vaše skóre časem změnit.

Každé doporučení obsahuje pokyny o tom, proč je důležité. Tyto pokyny slouží k vyhodnocení, zda implementace doporučení je vhodné pro vás, daná povaze vašich IT služeb a obchodním potřebám vaší organizace.

## <a name="use-assessment-focus-area-recommendations"></a>Použít assessment fokus oblasti doporučení

Než v OMS můžete použít řešení pro vyhodnocení, musíte mít nainstalován řešení. Další informace o instalaci řešení, najdete v části [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md). Po instalaci, zobrazí se souhrn doporučení pomocí System Center Operations Manager Assessment dlaždice na stránce Přehled v OMS.

Zobrazte vyhodnocování souhrnné dodržování předpisů pro infrastrukturu a potom přejít k podrobnostem doporučení.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Proveďte opravné akce a zobrazit doporučení pro oblastí zájmu

1. Na **přehled** klikněte na tlačítko **System Center Operations Manager Assessment** dlaždici.
2. Na **System Center Operations Manager Assessment** zkontrolujte souhrnné informace v jednom z okna oblasti fokus a pak klikněte na jednu zobrazíte doporučení pro tuto oblast fokus.
3. Na všech stránkách oblasti fokus můžete zobrazit seřazený podle priority doporučení, která se pro vaše prostředí. Klikněte na tlačítko doporučení v části **vliv na objekty** Chcete-li zobrazit podrobnosti, proč se provádí doporučení.  
    ![oblastí zájmu](./media/log-analytics-scom-assessment/focus-area.png)
4. Můžete provést nápravné akce navržený v **doporučované akce**. Když položka vyřeší, bude záznam novější vyhodnocování které doporučené akce provedené a zvýší se vaše skóre dodržování předpisů. Opravené položky se zobrazí jako **předán objekty**.

## <a name="ignore-recommendations"></a>Ignorovat doporučení

Pokud máte doporučení, které chcete ignorovat, můžete vytvořit textový soubor, který OMS používá k ochraně před doporučení ze storu ve výsledky hodnocení.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="to-identify-recommendations-that-you-want-to-ignore"></a>K identifikaci doporučení, které chcete ignorovat

1. Přihlaste se do pracovního prostoru a otevřete vyhledávání protokolu. Pro počítače ve vašem prostředí použijte následující dotaz, který seznam doporučení, které selhaly.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Zde je snímek obrazovky zobrazující protokolu vyhledávací dotaz:  
    ![prohledávání protokolů](./media/log-analytics-scom-assessment/scom-log-search.png)

2. Zvolte doporučení, které chcete ignorovat. Hodnoty pro RecommendationId budete používat v dalším postupu.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Vytváření a používání textový soubor s IgnoreRecommendations.txt

1. Vytvořte soubor s názvem IgnoreRecommendations.txt.
2. Vložit nebo zadejte každou RecommendationId pro jednotlivá doporučení, které chcete OMS ignorovat na samostatném řádku a potom uložte a zavřete soubor.
3. Uložte soubor v následující složce na každém počítači, kam chcete OMS ignorovat doporučení.
4. Na serveru pro správu nástroje Operations Manager - *SystemDrive*: \Program Files\Microsoft System Center 2012 R2\Operations Manager\Server.

### <a name="to-verify-that-recommendations-are-ignored"></a>Chcete-li ověřit, že se ignorovat doporučení

1. Po na další naplánované vyhodnocení běží ve výchozím nastavení každých sedm dní, zadaný doporučení jsou označeny ignorovaná a nezobrazí se na řídicím panelu hodnocení.
2. Následující dotazy hledání protokolů můžete použít k zobrazení seznamu všech ignorováno doporučení.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

3. Pokud se později rozhodnete, zda chcete zobrazit ignorováno doporučení, odeberte všechny soubory IgnoreRecommendations.txt nebo RecommendationIDs můžete odebrat z nich.

## <a name="system-center-operations-manager-assessment-solution-faq"></a>Řešení System Center Operations Manager Assessment – nejčastější dotazy

*Po přidání řešení pro vyhodnocení do části pracovní prostor OMS. Ale doporučení se nezobrazí. Proč ne?* Po přidání řešení, použijte následující postup zobrazení doporučení na řídicím panelu OMS.  

- [Nastavit účet Spustit jako pro System Center Operations Manager hodnocení](#operations-manager-run-as-accounts-for-oms)  
- [Konfigurace pravidla System Center Operations Manager hodnocení](#configure-the-assessment-rule)


*Existuje způsob, jak nakonfigurovat, jak často se hodnocení spouštět?* Ano. V tématu [nakonfigurovat spuštění četnost](#configure-the-run-frequency).

*Pokud je zjištěno jiný server, poté, co byly přidány řešení System Center Operations Manager hodnocení, bude ho vyhodnocena?* Ano, po zjišťování, se bude posouzeno chvíle – ve výchozím nastavení, každých sedm dní.

*Jaký je název procesu, který nemá shromažďování dat?* AdvisorAssessment.exe

*Kde proces AdvisorAssessment.exe spustit?* AdvisorAssessment.exe kompatibilní serveru pro správu služby Health Service se podporou pravidlo hodnocení. Pomocí tohoto procesu, zjišťování celé prostředí je dosaženo pomocí vzdálených dat kolekce.

*Jak dlouho trvá pro shromažďování dat?* Shromažďování dat na serveru trvá asi jednu hodinu. Může trvat déle v prostředích, která mají mnoho instancí nástroje Operations Manager nebo databáze.

*Co když lze nastavit interval hodnocení na méně než 1 440 minut?* Hodnocení je předem nakonfigurovaný pro spouštění na maximálně jednou za den. Pokud přepíšete hodnota intervalu na hodnotu menší než 1 440 minut, pak hodnocení používá jako hodnota intervalu 1 440 minut.

*Jak vědět, pokud se předběžné selhání?* Pokud spustili hodnocení a se nezobrazí výsledky, je pravděpodobné, že některé předpoklady pro hodnocení se nezdařilo. Můžete provést dotazy: `Type=Operation Solution=SCOMAssessment` a `Type=SCOMAssessmentRecommendation FocusArea=Prerequisites` v protokolu hledat neúspěšné požadavky.

*Došlo `Failed to connect to the SQL Instance (….).` zpráva předběžné selhání. Co je problém?* AdvisorAssessment.exe, proces, který shromažďuje data, se spustí v serveru pro správu služby Health Service. V rámci hodnocení se proces pokusí připojit k systému SQL Server, kde databáze nástroje Operations Manager nachází. Této chybě může dojít, když pravidla brány firewall zablokovat připojení k instanci systému SQL Server.

*Jaký typ dat shromažďovaných?* Se shromažďují následující typy dat: - dat služby WMI – registru data – data protokolu událostí – Operations Manager data prostřednictvím prostředí Windows PowerShell, dotazy SQL a soubor informace kolekce.

*Proč je nutné nakonfigurovat účet Spustit jako?* Pro server Operations Manager se spouštějí různé dotazy SQL. Aby mohly spustit musíte použít účet Spustit jako s potřebnými oprávněními. Kromě toho je potřeba dotaz WMI přihlašovací údaje místního správce.

*Proč zobrazit pouze prvních 10 doporučení?* Namísto udělení vyčerpávající, čtenáře seznam úloh, doporučujeme, byste se zaměřit na první adresování seřazený podle priority doporučení. Po jejich řešení, bude k dispozici další doporučení. Pokud ji chcete zobrazit podrobný seznam, můžete zobrazit všechna doporučení pomocí hledání protokolů.

*Existuje způsob, jak ignorovat doporučení?* Ano, najdete v článku [ignorovat doporučení](#Ignore-recommendations).


## <a name="next-steps"></a>Další kroky

- [V protokolech Hledat](log-analytics-log-searches.md) zobrazíte doporučení a podrobné údaje o System Center Operations Manager hodnocení.
