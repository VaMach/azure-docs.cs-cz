---
title: "Obnovení databáze Azure SQL v aplikaci SaaS víceklientské | Microsoft Docs"
description: "Postup obnovení databáze SQL jeden klienty po omylem odstraňování dat"
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
ms.date: 05/10/2017
ms.author: billgib;sstein
ms.openlocfilehash: a6ad292b9e63c6aaabed09e7569a86a41192ae97
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="restore-a-single-tenants-azure-sql-database-in-a-multi-tenant-saas-app"></a>Obnovení databáze Azure SQL jeden klienty v aplikaci SaaS více klientů

Aplikace Wingtip SaaS vytvořená s využitím modelu databáze za klienta, kde každý klient má své vlastní databázi. Jednou z výhod tohoto modelu je, že je snadno obnovit data jednoho klienta v izolaci bez dopadu na ostatních klientů.

V tomto kurzu zjistíte, dva vzory obnovení dat:

> [!div class="checklist"]

> * Obnovení databáze do paralelní databáze (-souběžného)
> * Obnovení databáze na místě


|||
|:--|:--|
| **Obnovení klienta do předchozího bodu v čase, do paralelní databáze** | Tento vzor lze klienta ke kontrole, auditování, dodržování předpisů a podobně. Původní databáze zůstane online a beze změny. |
| **Obnovení klienta na místě** | Tento vzor se obvykle používá k obnovení klienta do předchozího bodu v čase, poté, co klient omylem odstraní data. Původní databáze je uveden do režimu offline a nahradit obnovenou databázi. |
|||

Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

* Adresář Wingtip SaaS aplikace je nasazená. Nasazení za méně než pět minut najdete v tématu [nasazení a seznamte se s Wingtip SaaS aplikace](sql-database-saas-tutorial.md)
* Je nainstalované prostředí Azure PowerShell. Podrobnosti najdete v článku [Začínáme s prostředím Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-tenant-restore-pattern"></a>Úvod do vzoru SaaS klienta obnovení

Pro obnovení vzor klienta jsou dva jednoduché vzory pro obnovení dat jednoho klienta. Protože jsou od sebe navzájem oddělené klienta databáze, obnovení jednoho klienta nemá žádný vliv na data žádným jiným klientem na.

V prvním vzoru data obnovena do nové databáze. Klient pak je poskytnut přístup k této databáze spolu s jejich provozními daty. Tento vzor umožňuje Správce tenanta ke kontrole obnovená data a potenciálně ji použít k selektivně přepsat aktuální hodnoty data. Je to na návrháře SaaS aplikace k určení, jak sofistikované možnosti obnovení dat by měla být. Všechno, co je potřeba v některých scénářích možná jednoduše schopnost kontrolovat data ve stavu, ve kterém se nacházel k danému bodu v čase. Pokud se databáze používá [geografická replikace](sql-database-geo-replication-overview.md), doporučujeme, abyste kopírování požadovaná data z obnovené kopie do původní databáze. Pokud je původní databáze nahradit obnovenou databázi, budete muset překonfigurovat a poté opakujte synchronizaci geografická replikace.

V druhém vzorku, který se předpokládá, že klient má vyskytla ztráty nebo poškození dat, klienta produkční databázi obnovit do předchozího bodu v čase. V obnovení v místě vzoru klienta do režimu offline po krátkou dobu při databáze je obnovit a znovu online. Původní databáze je odstranit, ale můžete pořád obnovit z Pokud potřebujete přejít zpět do i dřívějšího bodu v čase. Varianta tohoto vzoru přejmenovat databázi místo odstranění, i když přejmenování databáze nabízí žádné další výhody z hlediska zabezpečení dat.

## <a name="get-the-wingtip-application-scripts"></a>Získání skriptů aplikace Wingtip

Adresář Wingtip SaaS skripty a zdrojový kód aplikace, které jsou k dispozici v [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github. [Postup stažení skripty Wingtip SaaS](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Simulovat klienta omylem odstraňování dat

K předvedení tyto scénáře obnovení, musíme *omylem* odstranit některá data v jedné z databází klienta. Když odstraníte všechny záznam, dalším krokem nastaví ukázku tak, aby metoda GET není blokován bránou porušení referenční integrity! Přidává také některé data nákupu lístku můžete později v *Wingtip SaaS Analytics kurzy*.

Spusťte skript generátor lístku a vytvořte další data. Generátor lístku záměrně není zakoupit lístky pro všechny klienty poslední události.

1. Otevřete... \\Učení moduly\\nástroje\\*ukázku TicketGenerator.ps1* v *prostředí PowerShell ISE*
1. Stiskněte klávesu **F5** spusťte skript a generovat zákazníků a prodejní data lístku.


### <a name="open-the-events-app-to-review-the-current-events"></a>Otevřete aplikaci události ke kontrole aktuální události

1. Otevřete *události rozbočovače* (http://events.wtp.&lt; Uživatel&gt;. trafficmanager.net) a klikněte na tlačítko **Contoso vzájemné součinnosti Hall**:

   ![centrum akcí](media/sql-database-saas-tutorial-restore-single-tenant/events-hub.png)

1. Posuňte se seznam událostí a poznamenejte si poslední událost v seznamu:

   ![poslední událost](media/sql-database-saas-tutorial-restore-single-tenant/last-event.png)


### <a name="run-the-demo-scenario-to-accidentally-delete-the-last-event"></a>Spustit ukázkový scénář se omylem odstranit poslední události

1. Otevřete... \\Učení moduly\\kontinuity podnikových procesů a zotavení po havárii\\RestoreTenant\\*ukázku RestoreTenant.ps1* v *prostředí PowerShell ISE*a nastavte následující hodnotu:
   * **$DemoScenario** = **1**, nastavte na **1** – odstranit události s žádné prodeje lístků.
1. Stiskněte klávesu **F5** a spusťte skript odstranit poslední událost. Zobrazí potvrzovací zpráva podobná této:

   ```Console
   Deleting unsold events from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

1. Otevře se stránka události Contoso. Posuňte se dolů a ověřte, zda že je událost pryč. Pokud tato událost je stále v seznamu, klikněte na tlačítko Aktualizovat a ověřte, zda že je pryč.

   ![poslední událost](media/sql-database-saas-tutorial-restore-single-tenant/last-event-deleted.png)


## <a name="restore-a-tenant-database-in-parallel-with-the-production-database"></a>Obnovení databáze klienta paralelně s provozní databáze

Toto cvičení obnoví databázi Hall vzájemné součinnosti Contoso do bodu v čase před událost byla odstraněna. Po odstranění události v předchozích krocích chcete obnovit a najdete v části odstraněná data. Není nutné obnovit produkční databázi s odstraněného záznamu, ale budete muset obnovit původní databázi pro přístup k původní data z jiných důvodů firmy.

 *Obnovení TenantInParallel.ps1* skript vytvoří paralelní klienta databáze a paralelní položky katalogu i s názvem *ContosoConcertHall\_staré*. Tento vzor obnovení je nejvhodnější pro obnovení před ztrátou dat menší nebo pro dodržování předpisů a auditování scénáře obnovení. Je také o doporučený postup při použití [geografická replikace](sql-database-geo-replication-overview.md).

1. Dokončení [simulovat uživatele nechtěně odstraní data](#simulate-a-tenant-accidentally-deleting-data) části.
1. Otevřete... \\Učení moduly\\kontinuity podnikových procesů a zotavení po havárii\\RestoreTenant\\_ukázku RestoreTenant.ps1_ v *prostředí PowerShell ISE*.
1. Nastavit **$DemoScenario** = **2**, tuto možnost nastavíte na **2** k *obnovení klienta paralelně*.
1. Stisknutím klávesy **F5** spusťte skript.

Skript obnoví databázi klienta (k databázi paralelní) do bodu v čase před odstranit události v předchozí části. Vytvoří druhý databáze, odstraní všechny existující katalogu metadata, která existuje v této databázi a přidá do katalogu v databázi *ContosoConcertHall\_staré* položku.

Ukázkový skript se otevře stránka události v prohlížeči. Poznámka: z adresy URL: ```http://events.wtp.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` , to se zobrazuje data z obnovené databáze kde *_old* se přidá k názvu.

Posuňte se události uvedené v prohlížeči potvrďte, že byla obnovena události odstranit v předchozí části.

Všimněte si, že vystavení obnovené klienta, jako je nepravděpodobné, že se, jak by poskytnout přístup klientů k další klienta, s vlastní aplikaci události procházet lístky obnovit data, ale slouží k snadno objasnění vzoru obnovení.

Ve skutečnosti byste pravděpodobně pouze zachovat tato obnovená databáze za definované období. Po dokončení voláním můžete odstranit položku obnovené klienta *odebrat RestoredTenant.ps1* skriptu.

1. Nastavit **$DemoScenario** k **4** vyberte *klienta odebrat obnovit* scénář.
1. **Spuštění** **pomocí** **F5**
1. *ContosoConcertHall\_staré* položka je nyní odstraněny z katalogu. Pokračujte a zavřete stránku události pro tohoto klienta v prohlížeči.


## <a name="restore-a-tenant-in-place-replacing-the-existing-tenant-database"></a>Obnovení klienta v místě, nahraďte existující databáze klienta

Toto cvičení obnoví Contoso vzájemné součinnosti Hall klienta k bodu v čase před událost byla odstraněna. *Obnovení TenantInPlace* skript obnoví aktuální databáze klienta pro novou databázi přejdete do předchozího bodu v čase a odstraní původní databázi. Tento vzor obnovení je nejvhodnější pro obnovení z poškození závažné dat jako může být důležité datových ztrát klienta bude muset přizpůsobit.

1. Otevřete **ukázku RestoreTenant.ps1** souboru v integrovaném Skriptovacím prostředí PowerShell
1. Nastavit **$DemoScenario** k **5** vyberte *obnovení klienta v případě místní*.
1. Spuštění pomocí **F5**.

Skript obnoví databázi klienta k bodu pět minut, než událost byla odstraněna. Dělá to pomocí první trvá Contoso vzájemné součinnosti Hall klienta do offline režimu, nejsou k dispozici žádné další aktualizace na data. Paralelní databáze je pak vytvořena obnovení z bodu obnovení a s názvem s časovým razítkem zajistit, že název databáze není v konfliktu s existující databáze název klienta. V dalším kroku staré databáze klienta se odstraní a obnovené databáze je přejmenována na původní název databáze. Nakonec Contoso vzájemné součinnosti Hall je uvést do režimu online pro povolení přístupu aplikace k obnovené databáze.

Úspěšně jste obnovili databázi do bodu v čase před událost byla odstraněna. Otevře stránku události, takže potvrďte poslední událost byla obnovena.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]

> * Obnovení databáze do paralelní databáze (-souběžného)
> * Obnovení databáze na místě

[Správa schématu databáze klienta](sql-database-saas-tutorial-schema-management.md)

## <a name="additional-resources"></a>Další zdroje

* Další [návodů, které stavět na adresář Wingtip SaaS aplikace](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Přehled kontinuity podnikových procesů s Azure SQL Database](sql-database-business-continuity.md)
* [Další informace o zálohování databáze SQL](sql-database-automated-backups.md)
