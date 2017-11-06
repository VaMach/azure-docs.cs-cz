---
title: "Azure SQL Data synchronizovat osvědčené postupy | Microsoft Docs"
description: "Další informace osvědčené postupy pro konfiguraci a spuštění synchronizaci dat SQL Azure"
services: sql-database
ms.date: 11/2/2017
ms.topic: article
ms.service: sql-database
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 6101dfa4bc74acf5045975f6513886fa135fe833
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="best-practices-for-sql-data-sync"></a>Osvědčené postupy pro synchronizaci dat SQL 

Tento článek popisuje osvědčené postupy pro synchronizaci dat SQL (Preview).

## <a name="security-and-reliability"></a>Zabezpečení a spolehlivost

### <a name="client-agent"></a>Klientský Agent

-   Nainstalujte agenta klienta pomocí nejnižší oprávnění účtu s přístupem k síti služby.

-   Je nejvhodnější Pokud agenta klienta je nainstalovaná na samostatném počítači z počítače systému SQL Server na místě.

-   Neregistrovat místní databázi s více než jeden agentem.

-   I když synchronizuje různých tabulek pro různé synchronizace skupiny.

-   Registrace místní databázi s více klientských agentů představuje problémy při odstraňování jedné ze skupin synchronizace.

### <a name="database-accounts-with-least-required-privilege"></a>Databáze účtů s nejnižší požadovaná oprávnění

-   **Pro nastavení synchronizace**. Vytvořit nebo Alter Table, Alter Database, vytvořit proceduru, vyberte / Alter schématu, vytvoření uživatelsky definovaný typ..

-   **Pro účely trvalé synchronizace**. Vyberte / Vložit / aktualizovat / odstranit v tabulkách vybrány pro synchronizaci a na synchronizovat metadata a tabulky pro sledování, oprávnění spouštět na uložené procedury vytvořených službou, oprávnění ke spuštění na typech uživatelem definovaná tabulka.

-   **Pro jeho rušení**. Příkaz ALTER na tabulky součástí synchronizace, vyberte / Delete pro tabulky, synchronizovat Metadata, ovládací prvek v synchronizaci sledování tabulky, uložené procedury a uživatelem definované typy sledování synchronizace.

**Jak můžete použít tyto informace při pouze jeden přihlašovací údaje pro databázi ve skupině synchronizace?**

-   Změnit pověření pro různé fáze (například credential1 pro instalaci a credential2 pro probíhající).

-   Změnit oprávnění přihlašovací údaje (tedy změnit oprávnění po synchronizaci nastavení).

## <a name="locate-hub"></a>Umístění databáze rozbočovače

### <a name="enterprise-to-cloud-scenario"></a>Scénář Enterprise cloudu

Pro zajištění minimální latence, zachovat databázi rozbočovače blízko největší koncentrace provozu databáze synchronizace skupiny.

### <a name="cloud-to-cloud-scenario"></a>Scénář cloudu cloudu

-   Pokud jsou všechny databáze ve skupině synchronizace v jedné datové centrum, rozbočovače musí nacházet ve stejném datovém centru. Tato konfigurace snižuje latenci a náklady na přenos dat mezi datovými centry.

-   Pokud databáze ve skupině synchronizace jsou v několika datových centrech, rozbočovače musí nacházet ve stejném datovém centru jako většinu databáze a provozu databáze.

### <a name="mixed-scenarios"></a>Smíšená scénáře

Předchozí pokyny se vztahují na složitějších konfigurací synchronizace skupiny.

## <a name="database-considerations-and-constraints"></a>Informace se databází a omezení

### <a name="sql-database-instance-size"></a>Velikost instance databáze SQL

Když vytvoříte novou instanci databáze SQL, nastavte maximální velikost tak, aby vždycky je větší než databázi, kterou nasazujete. Pokud není nastaveno maximální velikost větší než nasazené databáze, dojde k selhání synchronizace. Když neexistuje žádné automatického zvětšování – můžete provést příkaz ALTER DATABASE a zvyšte velikost databáze po jeho vytvoření. Zajistěte, aby že zůstat v omezení velikosti instance databáze SQL.

> [!IMPORTANT]
> Synchronizaci dat SQL ukládá další metadata s každou databázi. Ujistěte se, že účet pro tato metadata při výpočtu potřeba místo. Množství přidali, se řídí režijní náklady na šířku tabulky (například úzké tabulky vyžadují další režii) a objemu provozu.

## <a name="table-considerations-and-constraints"></a>Aspekty tabulky a omezení

### <a name="selecting-tables"></a>Výběr tabulek

Ne všechny tabulky v databázi musí být v [skupiny synchronizace](#sync-group). Výběr tabulky, které pokud chcete zahrnout do skupiny synchronizace a které chcete vyloučit (nebo zahrnout do jiné skupiny synchronizace) může ovlivnit efektivitu a nákladů. Zahrnete pouze ty tabulky do skupiny synchronizace, že obchodní potřeby vyžádání a tabulky, na kterých jsou závislé.

### <a name="primary-keys"></a>Primární klíče

Každá tabulka ve skupině synchronizace musí mít primární klíč. Službu synchronizaci dat SQL (Preview) se nepodařilo synchronizovat všechny tabulky, který nemá primární klíč.

Před distribucí do produkčního prostředí, testování výkonu počátečních a pokračujících synchronizace.

## <a name="provisioning-destination-databases"></a>Zřizování cílové databáze

Náhled synchronizaci dat SQL (Preview) poskytuje základní databáze automatické zřizování.

Tato část popisuje omezení synchronizaci dat SQL (Preview) je zřizování.

### <a name="auto-provisioning-limitations"></a>Automatické zřizování omezení

Toto jsou omezení synchronizaci dat SQL (Preview) automatické zřizování.

-   Pouze vybrané sloupce jsou vytvořeny v cílové tabulce.
Proto pokud některé sloupce nejsou součástí skupiny synchronizace tyto sloupce nejsou zřízené v cílových tabulkách.

-   Indexy jsou vytvářeny pouze pro vybrané sloupce.
Pokud má index tabulky zdrojového sloupce, které nejsou součástí skupiny synchronizace tyto indexy nejsou zřízené v cílových tabulkách.

-   Indexy na sloupce typu XML nejsou zřízené.

-   ZKONTROLUJTE, že omezení není zřízený.

-   Existující aktivační události na zdrojové tabulky nejsou zřízené.

-   Zobrazení a uložených procedur nejsou vytvořeny v cílové databázi.

### <a name="recommendations"></a>Doporučení

-   Pomocí funkce automatického zřizování pouze k vyzkoušení modulů pro službu.

-   V produkčním prostředí by měl zřídit schéma databáze.

## <a name="avoid-a-slow-and-costly-initial-synchronization"></a>Vyhněte se pomalé a nákladná počáteční synchronizaci

Tato část popisuje počáteční synchronizace skupiny synchronizace a co můžete dělat předejdete počáteční synchronizaci více, než by měl trvá déle, než nezbytné a nákladů.

### <a name="how-initial-synchronization-works"></a>Jak počáteční synchronizace funguje

Když vytvoříte skupinu synchronizace, spusťte s daty v pouze jednu databázi. Pokud máte data ve více databází, synchronizaci dat SQL (Preview). každý řádek považuje za konflikt, který potřebuje řešení. Toto řešení konfliktů způsobí, že počáteční synchronizace se pomalu, přejděte do několik měsíců, v závislosti na velikosti databáze trvá několik dní.

Kromě toho pokud jsou databáze v různých datových centrech, nákladů na počáteční synchronizace jsou vyšší než je nezbytné, vzhledem k tomu, že každý řádek musí projít mezi různých datových střediscích.

### <a name="recommendation"></a>Doporučení

Vždy, když možné spustit s daty v jenom jedna z databází synchronizace skupiny.

## <a name="design-to-avoid-synchronization-loops"></a>Návrh, aby se zabránilo synchronizace smyčky

Smyčku synchronizace nastává kvůli tomu, aby každé změně v jedné databáze se replikuje prostřednictvím databází ve skupině synchronizace pravidelného a do nekonečna jsou cyklické odkazy v rámci synchronizace skupiny. Chcete vyhnout smyčky synchronizace, protože snížit výkon a může značně zvýšit náklady.

## <a name="avoid-out-of-date-databases-and-sync-groups"></a>Vyhněte se zastaralé databáze a synchronizaci skupin

Synchronizace skupiny nebo databáze v rámci skupiny synchronizace se může stát zastaralé. Pokud je stav skupiny synchronizace "zastaralé", přestane fungovat. Pokud je stav do databáze "zastaralé", data mohou být ztracena. Je nejvhodnější vyhnout těchto situacích než nutné obnovit z nich.

### <a name="avoid-out-of-date-databases"></a>Vyhněte se zastaralé databáze

Do databáze je nastaven stav zastaralé po provedení offline pro 45 dnů nebo déle. Nepoužívejte zastaralý stav na databázi tím zajistí, že žádná z databází není offline 45 dní nebo více.

### <a name="avoid-out-of-date-sync-groups"></a>Vyhněte se zastaralé synchronizace skupiny

Skupiny synchronizace stav nastaven na zastaralé nepodaří-li všechny změny v rámci skupiny pro synchronizaci se rozšíří na zbytek skupiny synchronizace pro 45 dnů nebo déle. Nepoužívejte zastaralý stav na skupinu synchronizace pravidelně kontrolou skupiny synchronizace historie protokolu. Zkontrolujte, zda jsou vyřešeny všechny konflikty a změny se úspěšně rozšíří v rámci databáze synchronizace skupiny.

Skupiny synchronizace se pravděpodobně nepodaří použít ke změně důvodů:

-   Schéma nekompatibilita mezi tabulkami.

-   Nekompatibilita data mezi tabulkami.

-   Vložení řádků s hodnotou null ve sloupci, který nepovoluje hodnoty null.

-   Probíhá aktualizace řádek hodnotu, která porušuje omezení cizího klíče.

Můžete zabránit zastaralé synchronizace skupin podle:

-   Aktualizujte schéma tak, aby povolit hodnoty obsažené v řádcích se nezdařilo.

-   Aktualizujte hodnoty cizího klíče zahrnout hodnoty obsažené v řádcích se nezdařilo.

-   Aktualizujte hodnoty dat v neúspěšné řádku, který má být kompatibilní se schématem nebo cizí klíče v cílové databázi.

## <a name="avoid-deprovisioning-issues"></a>Vyhněte se zrušení zřízení problémy

Za určitých okolností rušení registrace databáze s klientským agentem může způsobit selhání synchronizace.

### <a name="scenario"></a>Scénář

1. Synchronizace skupiny A někdo vytvořil v instanci SQL databáze a místní databázi systému SQL Server, který je přidružen ke místní agent 1.

2. Stejnou místní databázi není registrována s místní agent 2 (Tento agent není přidružené k žádné skupině sync).

3. Zrušení registrace místní databázi z místní agent 2 odebere tabulky sledování nebo metadat pro synchronizaci skupiny A pro místní databázi.

4. Nyní skupiny A operací synchronizace nezdaří s následující chybou – "aktuální operaci nelze dokončit, protože databáze není zřízený pro synchronizaci nebo nemáte oprávnění k synchronizaci konfigurace tabulky."

### <a name="solution"></a>Řešení

Vyhněte situaci zcela tím, že nikdy zaregistrujete databáze s více než jeden agentem.

Pokud chcete obnovit z této situaci:

1. Odeberte z každé synchronizace skupiny, které patří do databáze.

2. Přidejte databázi zpět do každé skupiny synchronizace, který právě odebrali z.

3. Nasaďte každou skupinu ovlivněných synchronizace, (která zřizuje databáze).

## <a name="handling-changes-that-fail-to-propagate"></a>Zpracování změny, které se nepodařilo rozšířit

### <a name="reasons-that-changes-fail-to-propagate"></a>Z důvodů, které se nepodařilo rozšířit změny

Změny můžou selhat potřebný k šíření z mnoha důvodů. Některé příčiny by byl:

-   Nekompatibilita schématu nebo datového typu.

-   Došlo k pokusu o vložení v nevynulovatelném sloupci hodnotu null.

-   Narušení omezení cizího klíče.

### <a name="what-happens-when-changes-fail-to-propagate"></a>Co se stane, pokud se nedaří rozšíří změny?

-   Synchronizace skupiny ukazují, že je ve stavu varování.

-   Podrobnosti najdete v protokolu uživatelského rozhraní portálu prohlížeč.

-   Pokud není problém vyřešen 45 dní, stane aktuální databázi.

> [!NOTE]
> Tyto změny nikdy rozšíří. K opětovnému vytvoření skupiny synchronizace je jediným způsobem, jak obnovit.

### <a name="recommendation"></a>Doporučení

Monitorování stavu skupiny synchronizace a databáze pravidelně přes rozhraní portálu a protokolu.

## <a name="modifying-your-sync-group"></a>Úprava skupiny synchronizace

Nepokoušejte se odebrat databázi ze skupiny, synchronizaci a pak upravte skupiny synchronizace bez nasazení první z nich změny.

Nejprve odeberte databázi ze skupiny pro synchronizaci. Potom nasazení příslušné změny a počkat na jeho rušení pro dokončení. Po dokončení této operace, můžete upravit skupiny synchronizace a změny nasazení.

Pokud se pokusíte odebrat databázi a pak upravte skupinu synchronizace bez první nasazení změny, jeden nebo jiná operace selže a rozhraní portálu může dojít k nekonzistentnímu stavu. V takovém případě můžete obnovit stránku k obnovení stavu.
