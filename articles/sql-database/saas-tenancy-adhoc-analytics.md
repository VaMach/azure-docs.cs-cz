---
title: "Spuštění dotazů ad hoc vytváření sestav napříč více databází Azure SQL | Microsoft Docs"
description: "Spuštění dotazů ad hoc vytváření sestav napříč více databází SQL v příkladu víceklientské aplikace."
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
ms.topic: articles
ms.date: 11/13/2017
ms.author: billgib; sstein; AyoOlubeko
ms.openlocfilehash: ddad47ccac57ddbb9387709ababbc5be6bad3462
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="run-ad-hoc-analytics-queries-across-multiple-azure-sql-databases"></a>Spuštění dotazů ad hoc analytics napříč více databází Azure SQL

V tomto kurzu spustíte distribuované dotazy napříč celou sadu klienta databáze a umožnit tak vykazování ad hoc interaktivní. Tyto dotazy můžete extrahovat Statistika schovaný v každodenní provozních dat aplikace SaaS Wingtip lístků. K tomuto účelu nasazení další analýze databáze na server katalogu a můžete povolit distribuované dotazy pomocí elastické dotazu.


V tomto kurzu se dozvíte:

> [!div class="checklist"]

> * O globální zobrazení každou databázi tato zobrazení povolit efektivní dotazování mezi klienty
> * Postup nasazení databáze služby ad hoc generování sestav
> * Postup spuštění distribuovaných dotazů mezi všechny databáze klienta



Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:


* Nasazení aplikace Wingtip lístky SaaS databáze za klienta. Nasazení za méně než pět minut najdete v tématu [nasazení a seznamte se s aplikací Wingtip lístky SaaS databáze za klienta](saas-dbpertenant-get-started-deploy.md)
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* SQL Server Management Studio (SSMS) je nainstalována. Chcete-li stáhnout a nainstalovat aplikace SSMS, přečtěte si téma [stáhnout SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


## <a name="ad-hoc-reporting-pattern"></a>Vzor vykazování ad hoc

![generování sestav vzor ad hoc](media/saas-tenancy-adhoc-analytics/adhocreportingpattern_dbpertenant.png)

Jedním z velké příležitosti s aplikacemi SaaS je schopnost používat velká množství dat klienta centrálně uložených v cloudu a získáte přehled o operaci a využití vaší aplikace. Funkce vývoj, vylepšení použitelnost a další investice do vaší aplikace a služby, můžete tyto přehledy průvodce.

V jedné databázi s více tenanty je přístup k těmto datům jednoduchý, ale třeba v případě distribuce mezi tisícovky databází se situace komplikuje. Jeden z přístupů je použít [elastické dotazu](sql-database-elastic-query-overview.md), což umožňuje dotazování na distribuovanou sadu databází s společné schéma. Tyto databáze mohou být distribuovány na různé skupiny prostředků a předplatná, ale potřeba sdílet běžné přihlášení. Elastické dotaz používá jeden *head* databáze, ve kterém jsou definovány externí tabulky, zrcadlení tabulky a zobrazení v databázích distribuované (klientů). Dotazy odeslané do této hlavní databáze se kompilují a vzniká plán distribuovaného dotazu, který zajistí předávání částí dotazu potřebným tenantským databázím. Elastické dotaz používá k určení umístění všech databází klienta mapy horizontálního oddílu v databázi katalogu. Instalační program a dotaz jsou jednoduché, pomocí standardní [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference)a podporují ad hoc dotazy z nástroje, například Power BI a Excel.

Elastické dotazu distribucí dotazy mezi databázemi klienta, poskytuje okamžitý přehled o provozu provozními daty. Však jako elastické dotaz získává data ze potenciálně velký počet databází, latence dotazu může někdy být vyšší než pro odeslání do jedné databáze víceklientské ekvivalentní dotazy. Nezapomeňte návrhu dotazů minimalizovat objem dat, která je vrácena. Elastické dotazu je často nejvhodnější pro dotazy malých objemů dat v reálném čase a sestavování často používané nebo komplexní analytické dotazy nebo sestavy. Pokud dotazy neprovádí dobře, podívejte se na [plán spuštění](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) jaká část dotazu bylo posunuto do vzdálené databáze a kolik dat se vrací. Dotazy, které vyžadují komplexní analytického zpracování může být lepší obsluhuje v některých případech extrahování dat klienta do vyhrazené databáze nebo datového skladu optimalizované pro analytické dotazy. Tento vzor je podrobně popsaný [kurzu analýza klienta](saas-tenancy-tenant-analytics.md). 

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Získat aplikační skripty Wingtip lístky SaaS databáze za klienta

Adresář Wingtip lístky SaaS víceklientské databázové skripty a zdrojový kód aplikace, které jsou k dispozici v [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) úložiště GitHub. Podívejte se [obecné pokyny](saas-tenancy-wingtip-app-guidance-tips.md) kroky, jak stáhnout a odblokování skripty Wingtip lístky SaaS.

## <a name="create-ticket-sales-data"></a>Vytvořit data prodeje lístků

Ke spouštění dotazů na zajímavějšího datové sady, vytvořte spuštěním lístku generátor prodejní data lístku.

1. V *prostředí PowerShell ISE*, otevřete... \\Learning moduly\\provozní Analytics\\vykazování ad hoc\\*ukázku AdhocReporting.ps1* skript a nastavte následující hodnoty:
   * **$DemoScenario** = 1, **zakoupit lístky pro události na všechna místa**.
2. Stiskněte klávesu **F5** generovat prodeje lístků a spusťte skript. Je spuštěn skript, pokračujte kroky v tomto kurzu. Data lístku je dotazován v *spouštění dotazů ad-hoc distribuované* část, proto počkejte generátoru lístek pro dokončení.

## <a name="explore-the-global-views"></a>Prozkoumejte globální zobrazení

V aplikaci Wingtip lístky SaaS databáze za klienta každý klient je zadána databáze. Proto data obsažená v tabulkách databáze je vymezen na perspektivu jednoho klienta. Ale při dotazování mezi všechny databáze, je důležité, aby elastické dotazu lze považovat data, pokud je součástí jedné logické databáze horizontálně dělené klientem. 

K simulaci tento vzor, jsou sadu 'globální' zobrazení přidat do databáze klienta daného projektu ID klienta do každé z tabulek, které jsou předmětem dotazování globálně. Například *VenueEvents* zobrazení přidá počítaný *VenueId* na sloupce ze k projekci *události* tabulky. Podobně *VenueTicketPurchases* a *VenueTickets* zobrazení přidat počítaný *VenueId* sloupec projektovat z jejich odpovídajících tabulek. Tato zobrazení jsou používány elastické dotazu učinit paralelní dotazy a nabízená dolů odpovídající vzdáleného klienta je při zpracování databáze *VenueId* sloupec je k dispozici. Tím se výrazně snižuje množství dat, která je vrácena a výsledkem podstatné zvýšení výkonu pro mnoho dotazů. Tato globální zobrazení byly předem vytvořené v všechny databáze klienta.

1. Otevřete aplikaci SSMS a [připojit k tenants1 -&lt;uživatele&gt; server](saas-tenancy-wingtip-app-guidance-tips.md#explore-database-schema-and-execute-sql-queries-using-ssms).
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

1. V **Průzkumník objektů**, rozbalte položku **contosoconcerthall** > **zobrazení**:

   ![zobrazení](media/saas-tenancy-adhoc-analytics/views.png)

2. Klikněte pravým tlačítkem na **dbo. Místa**.
3. Vyberte **skript zobrazení jako** > **vytvořit** > **nové okno editoru dotazů**

Skript žádnému jinému *místo* zobrazení pro zobrazení, jak přidat *VenueId*.

## <a name="deploy-the-database-used-for-ad-hoc-distributed-queries"></a>Nasazení databáze používá pro distribuované dotazy ad hoc

Toto cvičení nasadí *adhocreporting* databáze. Toto je head databáze, která obsahuje schéma použitého k dotazování mezi všechny databáze klienta. Databáze je nasazený na existující server katalogu, které je server používá pro všechny databáze v ukázkové aplikace týkajících se správy.

1. Otevřete... \\Learning moduly\\provozní Analytics\\vykazování ad hoc\\*ukázku AdhocReporting.ps1* v *prostředí PowerShell ISE* a nastavte následující hodnoty:
   * **$DemoScenario** = 2, **Deploy Ad-hoc analytics database** (Nasadit analytickou databázi ad hoc).

2. Stiskněte klávesu **F5** spusťte skript a vytvořit *adhocreporting* databáze.

V další části přidáte schématu do databáze, může sloužit ke spuštění distribuovaných dotazů.

## <a name="configure-the-head-database-for-running-distributed-queries"></a>Konfiguraci databáze, head, pro spuštění distribuovaných dotazů

Tento postup přidá schématu (externí zdroj dat a externí tabulky definice) k databázi ad hoc analýzy, který umožňuje dotazování mezi všechny databáze klienta.

1. Otevřete SQL Server Management Studio a připojte k databázi vykazování ad hoc, kterou jste vytvořili v předchozím kroku. Název databáze je *adhocreporting*.
2. Otevřete ...\Learning Modules\Operational Analytics\Adhoc Reporting\ *inicializovat AdhocReportingDB.sql* v aplikaci SSMS.
3. Zkontrolujte skript SQL a Poznámka:

   Elastické dotazu pomocí přihlašovacích údajů platných pro databázi přístup ke každé databáze klienta. Tato pověření musí být k dispozici ve všech databází a by měl obvykle být uděleno minimální práva potřebná k povolení těchto ad hoc dotazy.

    ![Vytvoření pověření](media/saas-tenancy-adhoc-analytics/create-credential.png)

   Pomocí databáze katalogu jako zdroj externích dat jsou dotazy distribuován do všech databází, které jsou zaregistrovány v katalogu při spuštění dotazu. Vzhledem k tomu, že názvy serverů se liší pro každé nasazení, získá tento skript inicializace umístění databáze katalogu načtením aktuální server (@@servername) němž se skript spustí.

    ![Vytvoření externího zdroje dat](media/saas-tenancy-adhoc-analytics/create-external-data-source.png)

   Externí tabulky, které odkazují na globální zobrazení popsané v předchozí části a definovaná pomocí **distribuční = SHARDED(VenueId)**. Protože každý *VenueId* mapuje jedné databáze, to zvyšuje výkon pro mnoho scénářů, jak je znázorněno v následujícím oddílu.

    ![Vytvoření externí tabulky](media/saas-tenancy-adhoc-analytics/external-tables.png)

   Místní tabulky *VenueTypes* , je vytvořeny a naplněny. Tuto referenční tabulku dat je běžné v všechny klienta databáze, tak může být reprezentován jako na místní tabulku a vyplněná běžné daty. Pro některé dotazy, to může snížit množství dat přesunout mezi databázemi klienta a *adhocreporting* databáze.

    ![Vytvoření tabulky](media/saas-tenancy-adhoc-analytics/create-table.png)

   Pokud zahrnete referenční tabulky tímto způsobem, nezapomeňte aktualizovat schéma tabulky a data při každé aktualizaci databáze klienta.

4. Stiskněte klávesu **F5** inicializaci a spuštění skriptu *adhocreporting* databáze. 

Nyní můžete spustit distribuované dotazy a shromažďovat statistiky napříč všichni klienti!

## <a name="run-ad-hoc-distributed-queries"></a>Spuštění distribuovaných dotazů ad hoc

Teď, když *adhocreporting* databáze je nastavit, aby ihned začít a spustit některé distribuované dotazy. Zahrnout plán spuštění lépe porozumět tomu, kde se zpracování dotazu děje. 

Při kontrole plán spuštění, pozastavte ukazatel myši nad ikony plán podrobnosti. 

Je důležité si uvědomit, že nastavení **distribuční = SHARDED(VenueId)** zvyšuje výkon pro mnoho scénářů, kdy externí zdroj dat je definován. Protože každému *VenueId* mapuje jedné databáze, filtrování je snadno provést vzdáleně, vrací pouze data potřebná.

1. Otevřete... \\Učení moduly\\provozní Analytics\\vykazování ad hoc\\*ukázku AdhocReportingQueries.sql* v aplikaci SSMS.
2. Zkontrolujte připojení k **adhocreporting** databáze.
3. Vyberte **dotazu** nabídky a klikněte na tlačítko **zahrnují skutečné naplánovat spuštění**
4. Zvýrazněte *místa, které jsou aktuálně registrované?* dotazu a stiskněte klávesu **F5**.

   Dotaz vrátí seznamu celý místo ilustrující způsob rychlé a snadné je k dotazování mezi všechny klienty a vrátit data z každého klienta.

   Zkontrolujte plán a zjistíte, že náklady je vzdálený dotaz, protože každá databáze klienta zpracovává vlastní dotaz a vrátí informace o jeho místo.

   ![Vybrat * z dbo. Místa](media/saas-tenancy-adhoc-analytics/query1-plan.png)

5. Vyberte další dotaz a stiskněte klávesu **F5**.

   Tento dotaz připojí data z databáze klienta a místní *VenueTypes* tabulky (místní počítač, protože je tabulka *adhocreporting* databáze).

   Zkontrolujte plán a zjistíte, že většina náklady na vzdálený dotaz. Vrátí informace o jeho místo každou databázi klienta a provede místní spojení s místní *VenueTypes* tabulka, která se zobrazí popisný název.

   ![Připojení k vzdálené a místní data](media/saas-tenancy-adhoc-analytics/query2-plan.png)

6. Nyní vybrat *den, kdy byly nejvíce lístků prodaných?* dotazu a stiskněte klávesu **F5**.

   Tento dotaz nemá trochu složitější spojování a agregaci. Co je důležité si uvědomit, je, že se vzdáleně provádí většinu zpracování a ještě jednou, vrátí jenom na řádky potřeby jeden řádek pro každý místo agregační lístku prodej počet za den.

   ![query](media/saas-tenancy-adhoc-analytics/query3-plan.png)


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]

> * Spouštění distribuovaných dotazů ve všech tenantských databázích
> * Nasazení databáze služby ad hoc generování sestav a přidejte do jeho spuštění distribuované dotazy schématu.


Nyní zkuste [klienta Analytics kurzu](saas-tenancy-tenant-analytics.md) prozkoumat extrakce dat k databázi samostatné analytics pro zpracování složitějších analýzy.

## <a name="additional-resources"></a>Další zdroje

* Další [návodů, které aplikace pro adresář Wingtip lístky SaaS databáze za klienta](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastic Query](sql-database-elastic-query-overview.md)
