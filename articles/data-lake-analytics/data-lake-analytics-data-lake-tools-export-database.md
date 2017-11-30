---
title: "Exportování databází U-SQL pomocí nástrojů Azure Data Lake pro Visual Studio | Microsoft Docs"
description: "Další informace o použití nástroje Azure Data Lake pro Visual Studio na export databáze. U-SQL a naimportujte ho do místního účtu ve stejnou dobu."
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: 
editor: 
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/27/2017
ms.author: yanacai
ms.openlocfilehash: 9f11e07f7fbaf2b708d6fbc8a746238745132bda
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="how-to-export-u-sql-database"></a>Jak na export databáze. U-SQL

V tomto dokumentu se dozvíte, jak používat [nástrojů Azure Data Lake pro Visual Studio](http://aka.ms/adltoolsvs) na export databáze. U-SQL jako jednoho skriptu U-SQL a stažené prostředky. Import vyexportované databáze místní účet je podporováno také v rámci jednoho procesu.

Zákazníci obvykle udržovat více prostředí pro vývoj, testování a provozním. Těchto prostředí jsou hostované na místní účet v místním počítači vývojáře a účet Azure Data Lake Analytics v Azure. Při vývoji a ladění U-SQL dotazy na vývojová a testovací prostředí, je běžné, že vývojáři muset znovu vytvořit, které se nacházejí v produkční databázi. **Databáze Průvodce exportem** pomáhá zrychlit tohoto procesu. Pomocí Průvodce může klonovat vývojáři existující databáze prostředí a ukázkových dat do jiných účtů Azure Data Lake Analytics.

## <a name="export-steps"></a>Postup exportu

### <a name="step-1-right-click-the-database-in-server-explorer-and-click-export"></a>Krok 1: Klikněte pravým tlačítkem na databázi v Průzkumníku serveru a klikněte na tlačítko "Export..."

Všechny účty Azure Data Lake Analytics, zda máte oprávnění pro jsou uvedeny v Průzkumníku serveru. Rozbalte ten obsahuje databáze, kterou chcete exportovat a klikněte pravým tlačítkem na databázi zvolit **exportovat...** . Pokud nenajdete v místní nabídce, budete muset [aktualizaci nástroj na lasted](http://aka.ms/adltoolsvs).

![Export databáze nástroje data Lake Analytics](./media/data-lake-analytics-data-lake-tools-export-database/export-database.png)

### <a name="step-2-configure-the-objects-you-want-to-export"></a>Krok 2: Konfigurace objekty, které chcete provést export

Někdy je databáze velká, ale bude třeba jen malou část toho ji a potom můžete nakonfigurovat podmnožinu objektů, které chcete exportovat v průvodci export. Všimněte si, že dokončení akce exportu spuštěním úlohy U-SQL, a proto export z účtu Azure způsobuje náklady.

![Data Lake Analytics nástroje pro Export databáze Průvodce](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard.png)

### <a name="step-3-check-the-objects-list-and-more-configurations"></a>Krok 3: Kontrola seznamu objektů a další konfigurace

V tomto kroku můžete zkontrolovat double vybrané objekty v horní části dialogového okna. Pokud existují nějaké chyby, klikněte na tlačítko předchozí přejít zpět a objekty, které chcete exportovat znovu nakonfigurovat.

Můžete také provést další konfigurace o export cíl, popisy těchto konfigurací jsou uvedeny v následující tabulce:

|Konfigurace|Popis|
|-------------|-----------|
|Název cílového|Tento název označuje, kam chcete uložit vyexportované databáze prostředků, jako je sestavení, další soubory a ukázková data. Složka s tímto názvem bude vytvořen v kořenové složce vaše místní data.|
|Adresáři projektu|Tato cesta definuje, kam chcete uložit exportovaný skript U-SQL, která zahrnuje všechny definice objektu databáze.|
|Pouze schéma|Výběrem této možnosti má za následek pouze definice databáze a prostředky (jako je sestavení a další soubory) která je exportována.|
|Schéma a Data|Výběrem této možnosti výsledkem definic databáze, prostředky a dat pro export. Exportují se horních N řádků tabulky.|
|Importovat do místní databáze automaticky|Kontroluje, že tato konfigurace znamená vyexportované databáze budou importovány do místní databáze automaticky po dokončení exportu.|

![Konfigurace databáze Průvodce nástroje pro Export data Lake Analytics](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-configuration.png)

### <a name="step-4-check-the-export-results"></a>Krok 4: Kontrola výsledků exportu

Po všech těchto nastavení a export průběh najdete v Průvodci vyexportovaných výsledků z okna protokolu. Prostřednictvím protokolu označeny červenou obdélníku v pájecí – snímek obrazovky můžete najít umístění exportovaných U-SQL skriptů a databáze prostředků včetně sestavení a další soubory ukázková data.

![Data Lake Analytics nástroje pro Export databáze Průvodce byl dokončen](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-completed.png)

## <a name="how-to-import-the-exported-database-to-local-account"></a>Postup importování vyexportované databáze místní účet

Kontroluje nejpohodlnější způsob k provedení této import **importovat do místní databáze automaticky** během exportu průběh v kroku 3. Pokud jste zapomněli Uděláte to tak, můžete najít exportovaný skript U-SQL přes protokol exportu a spusťte skript U-SQL místně pro importování databáze do místní účet.

## <a name="how-to-import-the-exported-database-to-azure-data-lake-analytics-account"></a>Postup importování vyexportované databáze k účtu Azure Data Lake Analytics

K importu databázi na jiný účet Azure Data Lake Analytics, budete potřebovat dva kroky:

1. Nahrajte exportovaných prostředků, včetně sestavení a další soubory ukázková data do výchozího účtu Azure Data Lake Store, které chcete importovat do účtu Azure Data Lake Analytics. Můžete najít složku exportovaný prostředků v kořenové složce místní data a nahrát celou složku do kořenového adresáře výchozí účet úložiště.
2. Odešlete exportovaný skript U-SQL k účtu Azure Data Lake Analytics, že které chcete importovat databázi, aby po dokončení odesílání.

## <a name="known-limitation"></a>Známá omezení

Aktuálně Pokud jste vybrali **schéma a Data** v Průvodci se nástroj spouští úlohy U-SQL pro export dat uložených v tabulkách. To je důvod, proč data export proces může být pomalé a způsobit náklady. 

## <a name="next-steps"></a>Další kroky

* [Pochopení U-SQL databáze](https://msdn.microsoft.com/library/azure/mt621299.aspx) 
* [Postup testování a ladění úloh U-SQL pomocí místní spuštění a sadu SDK Azure Data Lake U-SQL](data-lake-analytics-data-lake-tools-local-run.md)


