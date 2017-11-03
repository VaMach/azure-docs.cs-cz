---
title: "Příklad víceklientského aplikace Azure SQL Database - Wingtip SaaS | Microsoft Docs"
description: "Další informace pomocí víceklientské ukázkovou aplikaci, která používá databázi SQL Azure, například adresář Wingtip SaaS"
keywords: kurz k sql database
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: sstein
ms.openlocfilehash: 46c9a3eadc2c23959b4d08649c6c0215d44b493e
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="introduction-to-a-sql-database-multi-tenant-saas-app-example"></a>Úvod do příklad aplikace SaaS více klientů databáze SQL

*Wingtip SaaS* aplikace je víceklientské ukázkovou aplikaci, která demonstruje jedinečných výhod databáze SQL. Aplikace používá vzorec SaaS aplikace databází na tenanta k poskytování služeb více tenantům. Tato aplikace je určená k prezentují funkce databáze SQL Azure, které umožňují SaaS scénáře, včetně několika vzory návrhu a správu SaaS. Pro rychlé zprovoznění, nasadí aplikaci Wingtip SaaS za méně než pět minut!

Jsou k dispozici v aplikaci zdrojový kód a správu skriptů [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) úložiště github. Ke spouštění skriptů, [Learning moduly složka pro stahování](#download-and-unblock-the-wingtip-saas-scripts) do místního počítače.

## <a name="sql-database-wingtip-saas-tutorials"></a>Kurzy SaaS Wingtip databáze SQL

Po nasazení aplikace, prozkoumejte následující kurzy, které sestavení po počátečním nasazení. Tyto kurzy prozkoumejte běžných vzorů SaaS, které využít integrované funkce SQL Database, SQL Data Warehouse a jinými službami Azure. Kurzy zahrnují skripty prostředí PowerShell, s podrobné vysvětlení, které výrazně zjednodušit pochopení a implementace stejné vzorů správu SaaS ve svých aplikacích.


| Kurz | Popis |
|:--|:--|
|[Nasazení a prozkoumejte Wingtip SaaS aplikace](sql-database-saas-tutorial.md)| **ZAČNĚTE ZDE!** Nasazení a prozkoumejte aplikace Wingtip SaaS k předplatnému Azure. |
|[Zřizování a katalog klientů](sql-database-saas-tutorial-provision-and-catalog.md)| Zjistěte, jak se aplikace připojí klientům pomocí databáze katalogu a jak katalogu mapuje klienty na svá data. |
|[Sledování a správa výkonu](sql-database-saas-tutorial-performance-monitoring.md)| Zjistěte, jak používat monitorování funkcí služby SQL Database a jak nastavit upozornění při překročení prahové hodnoty výkonu. |
|[Monitorování s analýzy protokolů (OMS)](sql-database-saas-tutorial-log-analytics.md) | Další informace o použití [analýzy protokolů](../log-analytics/log-analytics-overview.md) monitorování velké množství prostředků, napříč více fondů. |
|[Obnovení jednoho klienta](sql-database-saas-tutorial-restore-single-tenant.md)| Zjistěte, jak k obnovení databázi klienta do předchozího bodu v čase. Kroky k obnovení databázi paralelní ponechat stávající databázi klienta online, jsou také zahrnuté. |
|[Správa schématu klienta](sql-database-saas-tutorial-schema-management.md)| Zjistěte, jak aktualizovat schéma a aktualizovat referenční data napříč všichni klienti Wingtip SaaS. |
|[Spustit analytics ad-hoc](sql-database-saas-tutorial-adhoc-analytics.md) | Vytvoření databáze analýzy ad-hoc a spusťte v reálném čase distribuované dotazy mezi všechny klienty.  |
|[Spusťte klienta analytics](sql-database-saas-tutorial-tenant-analytics.md) | Extrahování dat klienta do služby analytics databáze nebo data warehouse ke spuštění v režimu offline analytické dotazy. |



## <a name="application-architecture"></a>Architektura aplikace

Aplikace Wingtip SaaS používá model databáze za klienta a elastické fondy SQL Pokud chcete maximalizovat efektivitu. Pro zřizování a mapování klientům svá data se používá databázi katalogu. Základní aplikace Wingtip SaaS používá fond s tři klienty ukázka plus databáze katalogu. Dokončení řadu SaaS Wingtip kurzy za následek doplňky vyhledá nasazení se zavedením analytické databáze mezidatabázové Správa schématu, atd.


![Architektura Wingtip SaaS](media/sql-database-wtp-overview/app-architecture.png)


Při průchodu přes kurzů k a práci s aplikací, je potřeba zaměřit se na vzory SaaS, které se vztahují na datové vrstvě. Jinými slovy je třeba se zaměřit na datovou vrstvu a ne na analýzu aplikace jako takové. Principy provádění těchto SaaS vzory je klíč k implementaci tyto vzory ve svých aplikacích při zvažování veškeré nezbytné úpravy pro vaše konkrétní obchodní požadavky.

## <a name="download-and-unblock-the-wingtip-saas-scripts"></a>Stáhněte si a odblokování skripty Wingtip SaaS

Spustitelný soubor obsah (skripty, knihovny DLL) mohou být blokovány Windows, když jsou soubory zip stažené z externího zdroje a rozbalené. Při extrahování skripty ze souboru zip, ***použijte následující postup, chcete-li odblokovat soubor .zip před extrahování***. Tím se zajistí, že je povoleno spustit skripty.

1. Přejděte do [úložiště github Wingtip SaaS](https://github.com/Microsoft/WingtipSaaS).
1. Klikněte na tlačítko **klonovat nebo stáhnout**.
1. Klikněte na tlačítko **stáhnout ZIP** a soubor uložte.
1. Klikněte pravým tlačítkem myši **WingtipSaaS-master.zip** soubor a vyberte **vlastnosti**.
1. Na **Obecné** vyberte **Odblokovat**.
1. Klikněte na **OK**.
1. Extrahujte soubory.

Skripty, které jsou umístěné v *... \\WingtipSaaS hlavní\\Learning moduly* složky.


## <a name="working-with-the-wingtip-saas-powershell-scripts"></a>Práce s skriptů prostředí PowerShell Wingtip SaaS

Chcete-li maximální využití ukázku ponořte do zadané skripty. Použití zarážek a krok prostřednictvím skriptů, prozkoumání podrobnosti o tom, jak jsou implementované různé vzorce SaaS. Snadno procházet postupně zadaný skriptech a modulech pro nejlepší pochopení, doporučujeme používat [prostředí PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).

### <a name="update-the-configuration-file-for-your-deployment"></a>Aktualizovat konfigurační soubor pro vaše nasazení

Upravit **UserConfig.psm1** soubor s hodnotou prostředků skupiny a uživatele, který nastavíte během nasazení:

1. Otevřete *prostředí PowerShell ISE* a zatížení... \\Učení moduly\\*UserConfig.psm1* 
1. Aktualizace *ResourceGroupName* a *název* s konkrétními hodnotami pro vaše nasazení (na řádky 10 a 11 pouze).
1. Uložte změny!

Tyto hodnoty nastavení tady jednoduše nebude nutné aktualizovat tyto hodnoty specifické pro nasazení v každé skriptu.

### <a name="execute-scripts-by-pressing-f5"></a>Provádění skriptů stisknutím klávesy F5

Používat několik skriptů *$PSScriptRoot* k procházení složek, a *$PSScriptRoot* je Vyhodnocená jenom když jsou spouštět skripty, stisknutím klávesy **F5**.  Zvýraznění a spuštění výběr (**F8**) může vést k chybám, takže stiskněte **F5** při spouštění skriptů.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Procházení skriptů k vyzkoušení implementace

Nejlepší způsob, jak porozumět skripty je procházení je chcete zobrazit, co dělají. Podívejte se na zahrnutou **Demo -** skripty, které jsou k dispozici snadno postupujte podle pracovní postup vysoké úrovně. **Demo -** skripty zobrazit kroky potřebné k provedení každý úkol, takže nastavit zarážky a přejít k podrobnostem hlubší do jednotlivých volání zobrazíte podrobnosti implementace pro různé vzorce SaaS.

Tipy pro zkoumání a procházení skriptů prostředí PowerShell:

* Otevřete **Demo -** skripty v integrovaném Skriptovacím prostředí PowerShell.
* Spustit nebo pokračovat s **F5** (pomocí **F8** se nedoporučuje, protože *$PSScriptRoot* při spuštění výběr skriptu, nebude hodnocen).
* Pokud chcete umístit zarážky, klikněte na řádek nebo ho vyberte a stiskněte klávesu **F9**.
* Funkci nebo volání skriptu můžete vynechat stisknutím klávesy **F10**.
* Na funkci nebo volání skriptu můžete přejít stisknutím klávesy **F11**.
* Aktuální funkci nebo vlání skriptu můžete ukončit stisknutím kombinace **Shift + F11**.


## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Prozkoumání databázového schématu a spouštění dotazů SQL pomocí SSMS

Použití [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) k připojení a procházet aplikačních serverů a databází.

Nasazení původně má dva servery SQL Database pro připojení k – *tenants1 -&lt;uživatele&gt;*  serveru a *katalogu -&lt;uživatele&gt;*  Server. Aby se zajistilo úspěšné ukázku připojení, mají oba servery [pravidlo brány firewall](sql-database-firewall-configure.md) povolení všechny IP adresy prostřednictvím.


1. Otevřete *SSMS* a připojte se k serveru *tenants1-&lt;User&gt;.database.windows.net*.
1. Klikněte na **Připojit** > **Databázový stroj...**:

   ![katalogový server](media/sql-database-wtp-overview/connect.png)

1. Přihlašovací údaje pro ukázku jsou: Přihlašovací jméno = *developer*, Heslo = *P@ssword1*

   ![připojení](media\sql-database-wtp-overview\tenants1-connect.png)

1. Opakujte kroky 2–3 a připojte se k serveru *catalog-&lt;User&gt;.database.windows.net*.

Po úspěšném připojení byste měli vidět oba servery. Seznam databází se může lišit v závislosti na klienty, kterou jste zřídili:

![průzkumník objektů](media/sql-database-wtp-overview/object-explorer.png)



## <a name="next-steps"></a>Další kroky

[Nasazení aplikace Wingtip SaaS](sql-database-saas-tutorial.md)
