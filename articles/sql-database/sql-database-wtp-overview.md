---
title: "Ukázková aplikace s více tenanty využívající službu Azure SQL Database | Dokumentace Microsoftu"
description: "Seznámení s ukázkovou aplikací Wingtip Tickets (WTP) využívající službu Azure SQL Database"
keywords: kurz k sql database
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 265eab8104d8af7c510a88dffb9d70a2b3b37631
ms.contentlocale: cs-cz
ms.lasthandoff: 05/12/2017


---
# <a name="introduction-to-the-wingtip-tickets-platform-wtp-sample-saas-application"></a>Úvod do ukázkové SaaS aplikace WTP (Wingtip Tickets Platform)

SaaS aplikace WTP (Wingtip Tickets Platform) je ukázková aplikace s více tenanty, která předvádí jedinečné výhody SQL Database. Aplikace používá vzorec SaaS aplikace databází na tenanta k poskytování služeb více tenantům. Aplikace WTP je navržená tak, aby ukazovala funkce služby Azure SQL Database, která umožňuje scénáře SaaS, včetně vzorců návrhu a správy SaaS. [Nasazení aplikace WTP netrvá ani pět minut](sql-database-saas-tutorial.md), takže se můžete rychle pustit do práce.

Po nasazení aplikace WTP si projděte [sadu kurzů](#sql-database-saas-tutorials), které se zobrazí po prvním nasazení. Každý kurz se zaměřuje na typické úlohy, které jsou implementované v aplikacích SaaS. Úlohy se implementují podle vzorců SaaS, které využívají integrované funkce SQL Database. Uvedené vzorce zahrnují zřizování nových tenantů, obnovení databází tenantů, spouštění distribuovaných vzorců napříč všemi tenanty a implementace změn schématu napříč všemi databázemi tenantů. Každý kurz obsahuje skripty pro opakované použití s podrobnými vysvětleními, která výrazně zjednodušují pochopení a implementují stejné vzorce správy SaaS ve vašich aplikacích.

I když je aplikace WTP svým způsobem úplná a přesvědčivá jako ukázková aplikace, je důležité zaměřit se na klíčové vzorce Saas, protože souvisí s datovou vrstvou. Jinými slovy je třeba se zaměřit na datovou vrstvu a ne na analýzu aplikace jako takové. Znalost implementace těchto základních vzorců SaaS je klíčem k implementaci těchto vzorců v aplikacích se zohledněním jakýchkoli modifikací nezbytných pro vaše konkrétní pracovní požadavky.



## <a name="application-architecture"></a>Architektura aplikace

Aplikace WTP používá model databáze na tenanta a využívá elastické fondy SQL k maximální účinnosti.
Použití katalogu tenanta pro zřizování správy a možností připojení.
Integrované monitorování aplikace, fondu a databáze a výstrahy (OMS).
Schéma napříč tenanty a správa referenčních dat (úlohy elastické databáze).
Dotazy mezi tenanty, provozní analýzy (elastický dotaz).
Rozšíření dosahu s využitím geograficky distribuovaných dat.
Kontinuita obchodních procesů, Zotavení jednoho tenanta (PITR) DR na škále (geografické obnovení, geografická replikace, automatické-DR) Samoobslužná správa tenanta (prostřednictvím rozhraní API pro správu) PITR pro zotavení z operací, které poškozují samy sebe.

Základní aplikace Wingtip používá fond se třemi ukázkovými tenanty a databází katalogu.

![Architektura WTP](media/sql-database-wtp-overview/wtp-architecture.png)


## <a name="sql-database-wtp-saas-tutorials"></a>Kurzy SQL Database WTP SaaS

Následující kurzy vycházejí z počátečního nasazení [ukázkové Saas aplikace WTP (Wingtip Tickets Platform)](sql-database-saas-tutorial.md):

| Oblast | Popis | Umístění skriptu |
|:--|:--|:--|
|[Kurz Zřízení tenantů a jejich přidání do katalogu](sql-database-saas-tutorial-provision-and-catalog.md)| Zřízení nových tenantů a jejich registrace v katalogu | [Skripty na githubu](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Provision%20and%20Catalog) |
|[Kurz Sledování a správa výkonu](sql-database-saas-tutorial-performance-monitoring.md)| Monitorování a správa výkonu databáze a fondu | [Skripty na githubu](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Performance%20Monitoring%20and%20Management) |
|[Kurz Obnovení jednoho tenanta](sql-database-saas-tutorial-restore-single-tenant.md)| Obnovení databází tenantů | [Skripty na githubu](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Business%20Continuity%20and%20Disaster%20Recovery/RestoreTenant) |
|[Kurz Správa schémat tenantů](sql-database-saas-tutorial-schema-management.md)| Provádění dotazů mezi všemi klienty  | [Skripty na githubu](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Schema%20Management) |
|[Kurz Spuštění analýzy ad-hoc](sql-database-saas-tutorial-adhoc-analytics.md) | Vytvoření analytické databáze ad hoc a spuštění dotazů ve všech tenantech  | [Skripty na githubu](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Operational%20Analytics/Adhoc%20Analytics) |
|[Kurz Správa pomocí Log Analytics (OMS)](sql-database-saas-tutorial-log-analytics.md) | Konfigurace a prozkoumání Log Analytics | [Skripty na githubu](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Performance%20Monitoring%20and%20Management/LogAnalytics) |
|[Kurz Spuštění analýzy tenanta](sql-database-saas-tutorial-tenant-analytics.md) | Nastavení a spouštění analytických dotazů tenanta | [Skripty na githubu](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Operational%20Analytics/Tenant%20Analytics) |

## <a name="get-the-wingtip-application-scripts"></a>Získání skriptů aplikace Wingtip

Skripty a zdrojový kód aplikace Wingtip Tickets jsou k dispozici v úložišti GitHubu [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). Soubory se skripty jsou ve [složce Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Stáhněte si složku **Learning Modules** do svého místního počítače. Dejte pozor, abyste zachovali strukturu složky.

## <a name="working-with-the-wtp-powershell-scripts"></a>Práce se skripty WTP PowerShellu

Výhody práce s aplikací WTP plynou z podrobného prozkoumání poskytnutých skriptů a zkoušení, jak se implementují různé vzorce SaaS.

Pokud chcete zobrazit poskytnuté skripty a moduly a projít si je, abyste jim lépe porozuměli, podívejte se do části [ISE Windows PowerShellu](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise). Protože většina skriptů s předponou *Demo-* obsahuje proměnné, které můžete před provedením změnit, použití PowerShell ISE zjednodušuje práci s těmito skripty.

Ke každému nasazení aplikace WTP existuje soubor **UserConfig.psm1** obsahující dva parametry pro nastavení hodnot skupiny prostředků a uživatelského jména, které definujete během nasazení. Po nasazení upravte modul **UserConfig.psm1** nastavením parametrů _ResourceGroupName_ a _Name_. Tyto hodnoty používají jiné skripty k úspěšnému spouštění, takže se doporučuje nastavit je po dokončení nasazení.



### <a name="execute-scripts-by-pressing-f5"></a>Provádění skriptů stisknutím klávesy F5

Řada skriptů používá k povolení navigace ve složkách proměnnou *$PSScriptRoot*, která se vyhodnocuje jenom při provedení skriptu klávesou **F5**.  Zvýraznění a spuštění výběru (**F8**) může vést k chybám, takže se při spouštění skriptů WTP doporučuje stisknout klávesu **F5**.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Procházení skriptů k vyzkoušení implementace

Skutečná hodnota procházení skriptů spočívá v tom, že zjistíte, k čemu slouží. Podívejte se na skripty _Demo-_ na první úrovni, které poskytují snadno čitelný pracovní postup vysoké úrovně, který znázorňuje kroky požadované pro dosažení jednotlivých úkolů. Když pak dále procházíte jednotlivá volání, uvidíte podrobnosti o implementaci pro různé vzorce SaaS.

Tipy pro práci se skripty [PowerShellu a jejich ladění](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise):

* Otevřete a nakonfigurujte skripty demo- v ISE v PowerShellu.
* Proveďte skript nebo pokračujte v jeho provádění klávesou **F5**. Použití klávesy **F8** se nedoporučuje, protože se *$PSScriptRoot* při spuštění výběrů skriptů nevyhodnocuje.
* Pokud chcete umístit zarážky, klikněte na řádek nebo ho vyberte a stiskněte klávesu **F9**.
* Funkci nebo volání skriptu můžete vynechat stisknutím klávesy **F10**.
* Na funkci nebo volání skriptu můžete přejít stisknutím klávesy **F11**.
* Aktuální funkci nebo vlání skriptu můžete ukončit stisknutím kombinace **Shift + F11**.




## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Prozkoumání databázového schématu a spouštění dotazů SQL pomocí SSMS

K připojení a procházení serverů a databází WTP použijte [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

Ukázková aplikace WTP má na začátku dva servery SQL Database ke kterým se připojuje. Jsou to server *tenants1* a server *catalog*:


1. Otevřete *SSMS* a připojte se k serveru *tenants1-&lt;User&gt;.database.windows.net*.
2. Klikněte na **Připojit** > **Databázový stroj...**:

   ![katalogový server](media/sql-database-wtp-overview/connect.png)

1. Přihlašovací údaje pro ukázku jsou: Přihlašovací jméno = *developer*, Heslo = *P@ssword1*

   ![připojení](media\sql-database-wtp-overview\tenants1-connect.png)

1. Opakujte kroky 2–3 a připojte se k serveru *catalog-&lt;User&gt;.database.windows.net*.

Po úspěšném připojení byste měli vidět oba servery. Podle toho, kolik tenantů jste zřídili, můžete vidět více nebo méně databází:

![průzkumník objektů](media/sql-database-wtp-overview/object-explorer.png)



## <a name="next-steps"></a>Další kroky

[Nasazení ukázkové SaaS aplikace Wingtip Tickets](sql-database-saas-tutorial.md)
