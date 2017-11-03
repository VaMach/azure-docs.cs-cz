---
title: "Spouštění analytických dotazů ad hoc pro více databází SQL Azure | Dokumentace Microsoftu"
description: "Spuštění analytics dotazů ad-hoc napříč více databází SQL v příkladu víceklientské aplikace."
keywords: kurz k sql database
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: billgib; sstein
ms.openlocfilehash: 849f0570fb1550f6c3676fc070d0f862450ade9a
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="run-ad-hoc-analytics-queries-across-multiple-azure-sql-databases"></a>Spuštění analytics dotazů ad-hoc napříč více databází Azure SQL

V tomto kurzu spouštět distribuované dotazy napříč celou sadu databází víceklientské povolit ad-hoc analytics. Elastické dotazu slouží k povolení distribuované dotazy, které vyžaduje, že že další analýze databáze nasazení (do katalogu serveru). Tyto dotazy můžete extrahovat Statistika schovaný v každodenní provozních dat Wingtip SaaS aplikace.


V tomto kurzu se dozvíte:

> [!div class="checklist"]

> * O globální zobrazení každou databázi tato zobrazení povolit efektivní dotazování mezi klienty
> * Postup nasazení databázi analýzy ad-hoc
> * Postup spuštění distribuovaných dotazů mezi všechny databáze klienta



Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

* Adresář Wingtip SaaS aplikace je nasazená. Nasazení za méně než pět minut najdete v tématu [nasazení a seznamte se s Wingtip SaaS aplikace](sql-database-saas-tutorial.md)
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* SQL Server Management Studio (SSMS) je nainstalována. Chcete-li stáhnout a nainstalovat aplikace SSMS, přečtěte si téma [stáhnout SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


## <a name="ad-hoc-analytics-pattern"></a>Vzor analytics ad-hoc

Jeden z velké příležitosti s aplikacemi SaaS, je použití velká množství dat klienta centrálně uložených v cloudu. Tato data použijte a získáte přehled o operaci a využití vaší aplikace. Funkce vývoj, vylepšení použitelnost a další investice do vaší aplikace a služby, můžete tyto přehledy průvodce.

V jedné databázi s více tenanty je přístup k těmto datům jednoduchý, ale třeba v případě distribuce mezi tisícovky databází se situace komplikuje. Jeden z přístupů je použít [elastické dotazu](sql-database-elastic-query-overview.md), což umožňuje dotazování na distribuovanou sadu databází s společné schéma. Elastické dotaz používá jeden *head* databáze, ve kterém jsou definovány externí tabulky, zrcadlení tabulky a zobrazení v databázích distribuované (klientů). Dotazy odeslané do této hlavní databáze se kompilují a vzniká plán distribuovaného dotazu, který zajistí předávání částí dotazu potřebným tenantským databázím. Nástroj Elastic Query na základě mapy horizontálních oddílů v databázi katalogu poskytuje umístění databází tenantů. Instalační program a dotaz jsou jednoduché, pomocí standardní [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference)a podporují ad hoc dotazy z nástroje, například Power BI a Excel.

Elastické dotazu distribucí dotazy mezi databázemi klienta, poskytuje okamžitý přehled o provozu provozními daty. Však jako elastické dotaz získává data ze potenciálně velký počet databází, latence dotazu může někdy být vyšší než pro odeslání do jedné databáze víceklientské ekvivalentní dotazy. Nezapomeňte návrhu dotazů minimalizovat objem dat, která je vrácena. Elastické dotazu je často nejvhodnější pro dotazy malých objemů dat v reálném čase a sestavování často používané nebo komplexní analytické dotazy nebo sestavy. Pokud dotazy neprovádí dobře, podívejte se na [plán spuštění](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) jaká část dotazu bylo posunuto do vzdálené databáze a kolik dat se vrací. Dotazy, které vyžadují komplexní analytického zpracování může být lepší obsluhuje v některých případech extrahování dat klienta do vyhrazené databáze nebo datového skladu optimalizované pro analytické dotazy. Tento vzor je podrobně popsaný [kurzu analýza klienta](sql-database-saas-tutorial-tenant-analytics.md). 

## <a name="get-the-wingtip-application-scripts"></a>Získání skriptů aplikace Wingtip

Adresář Wingtip SaaS skripty a zdrojový kód aplikace, které jsou k dispozici v [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github. [Postup stažení skripty Wingtip SaaS](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="create-ticket-sales-data"></a>Vytvořit data prodeje lístků

Ke spouštění dotazů na zajímavějšího datové sady, vytvořte spuštěním lístku generátor prodejní data lístku.

1. V *prostředí PowerShell ISE*, otevřete... \\Learning moduly\\provozní Analytics\\ad hoc Analytics\\*ukázku AdhocAnalytics.ps1* skript a nastavte následující hodnoty:
   * **$DemoScenario** = 1, **zakoupit lístky pro události na všechna místa**.
2. Stiskněte klávesu **F5** generovat prodeje lístků a spusťte skript. Je spuštěn skript, pokračujte kroky v tomto kurzu. Data lístku je dotazován v *spouštění dotazů ad-hoc distribuované* část, proto počkejte generátoru lístek pro dokončení.

## <a name="explore-the-global-views"></a>Prozkoumejte globální zobrazení

Aplikace Wingtip SaaS vytvořená s využitím modelu klienta pro databáze, tak z hlediska jednoho klienta je definované schéma databáze klienta. Informace specifické pro klienta existuje v jedné tabulce *místo*, vždy má jeden řádek a je implementovaný jako haldy, bez primárního klíče. Ostatní tabulky v schéma nemusíte mít relaci s *místo* tabulky, protože při běžném použití, se nikdy všech nejistých, které klient patří data.

Ale při dotazování mezi všechny databáze, je důležité, aby elastické dotazu lze považovat data, pokud je součástí jedné logické databáze horizontálně dělené klientem. K simulaci tento vzor, sadu 'globální' zobrazení se přidají do databáze klienta daného projektu id klienta do každé z tabulek, které jsou předmětem dotazování globálně. Například *VenueEvents* zobrazení přidá počítaný *VenueId* na sloupce ze k projekci *události* tabulky. Definováním externí tabulky v databázi head přes *VenueEvents* (místo základní *události* tabulky), elastické dotazu je schopen nabízená dolů na základě spojení *VenueId*, mohou být provedeny souběžně na každém vzdálené databáze (ne na databázi head). Tím se výrazně snižuje množství dat, která je vrácena, což vede k podstatné zvýšení výkonu pro mnoho dotazů. Tato globální zobrazení byly předem vytvořené v všechny databáze klienta (a v *basetenantdb*).

1. Otevřete aplikaci SSMS a [připojit k tenants1 -&lt;uživatele&gt; server](sql-database-wtp-overview.md#explore-database-schema-and-execute-sql-queries-using-ssms).
2. Rozbalte položku **databáze**, klikněte pravým tlačítkem na **contosoconcerthall**a vyberte **nový dotaz**.
3. Spusťte následující dotazy a prozkoumejte rozdíl mezi tabulkami jednoho klienta a globální zobrazení:

   ```T-SQL
   -- The base Venue table, that has no VenueId associated.
   SELECT * FROM Venue

   -- Notice the plural name 'Venues'. This view projects a VenueId column.
   SELECT * FROM Venues

   -- The base Events table, which has no VenueId column.
   SELECT * FROM Events

   -- This view projects the VenueId retrieved from the Venues table.
   SELECT * FROM VenueEvents
   ```

V těchto zobrazeních *VenueId* se vypočítá jako hodnota hash místo názvu, ale žádné přístup může znamenat jedinečnou hodnotu. Tento přístup je podobným způsobem, který je počítaný klíč klienta pro použití v katalogu.

K prozkoumání definice *místa* zobrazení:

1. V **Průzkumník objektů**, rozbalte položku **contosoconcethall** > **zobrazení**:

   ![zobrazení](media/sql-database-saas-tutorial-adhoc-analytics/views.png)

2. Klikněte pravým tlačítkem na **dbo. Místa**.
3. Vyberte **skript zobrazení jako** > **vytvořit** > **nové okno editoru dotazů**

Skript žádnému jinému *místo* zobrazení pro zobrazení, jak přidat *VenueId*.

## <a name="deploy-the-database-used-for-ad-hoc-distributed-queries"></a>Nasazení databáze používá pro ad-hoc distribuované dotazy

Toto cvičení nasadí *adhocanalytics* databáze. Toto je head databáze, která obsahuje schéma použitého k dotazování mezi všechny databáze klienta. Databáze je nasazený na existující server katalogu, které je server používá pro všechny databáze v ukázkové aplikace týkajících se správy.

1. Otevřete skript ...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalytics.ps1* ve složce *PowerShell ISE* a nastavte následující hodnoty:
   * **$DemoScenario** = 2, **Deploy Ad-hoc analytics database** (Nasadit analytickou databázi ad hoc).

2. Stiskněte klávesu **F5** spusťte skript a vytvořit *adhocanalytics* databáze.

V další části přidáte schématu do databáze, může sloužit ke spuštění distribuovaných dotazů.

## <a name="configure-the-database-for-running-distributed-queries"></a>Konfigurace databáze pro spuštění distribuovaných dotazů

Tento postup přidá do databáze analýzy ad-hoc, která umožňuje dotazování mezi všechny databáze klienta schématu (externí zdroj dat a definice externí tabulky).

1. Otevřete SQL Server Management Studio a připojte k databázi ad hoc analýzy, kterou jste vytvořili v předchozím kroku. Název databáze je *adhocanalytics*.
2. Otevřete ...\Learning Modules\Operational Analytics\Adhoc Analytics\ *inicializovat AdhocAnalyticsDB.sql* v aplikaci SSMS.
3. Zkontrolujte skript SQL a vezměte na vědomí následující:

   Elastické dotazu pomocí přihlašovacích údajů platných pro databázi přístup ke každé databáze klienta. Tato pověření musí být k dispozici ve všech databází a by měl obvykle být uděleno minimální práva potřebná k povolení těchto dotazů ad-hoc.

    ![Vytvoření pověření](media/sql-database-saas-tutorial-adhoc-analytics/create-credential.png)

   Zdroj externích dat, která je definována pomocí mapování horizontálních klienta v databázi katalogu. Prostřednictvím tohoto jako zdroj externích dat jsou dotazy distribuovány do všech databází, které jsou zaregistrovány v katalogu při spuštění dotazu. Vzhledem k tomu, že názvy serverů se liší pro každé nasazení, získá tento skript inicializace umístění databáze katalogu načtením aktuální server (@@servername) němž se skript spustí.

    ![Vytvoření externího zdroje dat](media/sql-database-saas-tutorial-adhoc-analytics/create-external-data-source.png)

   Externí tabulky, které odkazují na globální zobrazení popsané v předchozí části a definovaná pomocí **distribuční = SHARDED(VenueId)**. Protože každý *VenueId* mapuje jedné databáze, to zvyšuje výkon pro mnoho scénářů, jak je znázorněno v následujícím oddílu.

    ![Vytvoření externí tabulky](media/sql-database-saas-tutorial-adhoc-analytics/external-tables.png)

   Místní tabulky *VenueTypes* , je vytvořeny a naplněny. Tuto referenční tabulku dat je běžné v všechny klienta databáze, tak může být reprezentován jako na místní tabulku a vyplněná běžné daty. Pro některé dotazy to může snížit množství dat přesunout mezi databázemi klienta a *adhocanalytics* databáze.

    ![Vytvoření tabulky](media/sql-database-saas-tutorial-adhoc-analytics/create-table.png)

   Pokud zahrnete referenční tabulky tímto způsobem, nezapomeňte aktualizovat schéma tabulky a data při každé aktualizaci databáze klienta.

4. Stiskněte klávesu **F5** inicializaci a spuštění skriptu *adhocanalytics* databáze. 

Nyní můžete spustit distribuované dotazy a shromažďovat statistiky napříč všichni klienti!

## <a name="run-ad-hoc-distributed-queries"></a>Spuštění dotazů ad-hoc distribuované

Teď, když *adhocanalytics* databáze je nastavit, aby ihned začít a spustit některé distribuované dotazy. Zahrnout plán spuštění lépe porozumět tomu, kde se zpracování dotazu děje. 

Při kontrole plán spuštění, pozastavte ukazatel myši nad ikony plán podrobnosti. 

Je důležité si uvědomit, že nastavení **distribuční = SHARDED(VenueId)** když jsme definovali externí zdroj dat, zlepšuje výkon pro mnoho scénářů. Protože každý *VenueId* mapuje jedné databáze, filtrování je snadno provést vzdáleně, vrací pouze data, potřebujeme.

1. Otevřete skript …\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalyticsQueries.sql* v SSMS.
2. Zkontrolujte připojení k **adhocanalytics** databáze.
3. Vyberte **dotazu** nabídky a klikněte na tlačítko **zahrnují skutečné naplánovat spuštění**
4. Zvýrazněte *místa, které jsou aktuálně registrované?* dotazu a stiskněte klávesu **F5**.

   Dotaz vrátí seznamu celý místo ilustrující způsob rychlé a snadné je k dotazování mezi všechny klienty a vrátit data z každého klienta.

   Zkontrolujte plán a zjistíte, že náklady je vzdálený dotaz, protože jsme jednoduše přejdete na každou databázi klienta a výběrem informace místo.

   ![Vybrat * z dbo. Místa](media/sql-database-saas-tutorial-adhoc-analytics/query1-plan.png)

5. Vyberte další dotaz a stiskněte klávesu **F5**.

   Tento dotaz připojí data z databáze klienta a místní *VenueTypes* tabulky (místní počítač, protože je tabulka *adhocanalytics* databáze).

   Zkontrolujte plán a zjistíte, že většina náklady je vzdálený dotaz, protože jsme dotaz na informace místo každého klienta (dbo. Místa), a poté proveďte rychlé místní spojení s místní *VenueTypes* tabulka, která se zobrazí popisný název.

   ![Připojení k vzdálené a místní data](media/sql-database-saas-tutorial-adhoc-analytics/query2-plan.png)

6. Nyní vybrat *den, kdy byly nejvíce lístků prodaných?* dotazu a stiskněte klávesu **F5**.

   Tento dotaz nemá trochu složitější spojování a agregaci. Co je důležité si uvědomit, je, že se vzdáleně provádí většinu zpracování a ještě jednou budeme navrácení pouze řádky, které potřebujeme, vrácení pouze jeden řádek pro každý místo agregační lístku prodej počet za den.

   ![query](media/sql-database-saas-tutorial-adhoc-analytics/query3-plan.png)


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]

> * Spouštění distribuovaných dotazů ve všech tenantských databázích
> * Nasazení databáze služby ad-hoc analýzy a přidejte do jeho spuštění distribuované dotazy schématu.


Nyní zkuste [klienta Analytics kurzu](sql-database-saas-tutorial-tenant-analytics.md) prozkoumat extrakce dat k databázi samostatné analytics pro zpracování složitějších analýzy...

## <a name="additional-resources"></a>Další zdroje

* Další [návodů, které stavět na adresář Wingtip SaaS aplikace](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastic Query](sql-database-elastic-query-overview.md)
