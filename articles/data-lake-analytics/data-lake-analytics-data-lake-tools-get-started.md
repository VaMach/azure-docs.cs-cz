---
title: "Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio | Dokumentace Microsoftu"
description: "Naučte se nainstalovat nástroje Data Lake pro Visual Studio a vyvíjet a testovat skripty U-SQL."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/28/2017
ms.author: saveenr, yanacai
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: 401e2d84e5e2eb9f66a16b299fbb93bd1943e04b
ms.contentlocale: cs-cz
ms.lasthandoff: 06/30/2017


---
<a id="develop-u-sql-scripts-by-using-data-lake-tools-for-visual-studio" class="xliff"></a>

# Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Naučte se používat sadu Visual Studio k vytváření účtů Azure Data Lake Analytics, definování úloh v [U-SQL](data-lake-analytics-u-sql-get-started.md) a odesílání úloh do služby Data Lake Analytics. Další informace o Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).


<a id="prerequisites" class="xliff"></a>

## Požadavky

* **Visual Studio:** Podporovány jsou všechny edice kromě Express.
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* Sada **Microsoft Azure SDK pro .NET** verze 2.7.1 nebo novější.  Nainstalujte ji pomocí [Instalačního programu webové platformy](http://www.microsoft.com/web/downloads/platform.aspx).
* **Účet Data Lake Analytics**. Informace o vytvoření účtu najdete v tématu [Začínáme s Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-get-started-portal.md).

<a id="install-azure-data-lake-tools-for-visual-studio" class="xliff"></a>

## Instalace nástrojů Azure Data Lake pro Visual Studio 

Stáhněte nástroje Azure Data Lake pro Visual Studio z webu [Download Center](http://aka.ms/adltoolsvs) a nainstalujte je. Po instalaci si všimněte, že:
* Uzel **Průzkumník serveru** > **Azure** obsahuje uzel **Data Lake Analytics**. 
* Nabídka **Nástroje** obsahuje položku **Data Lake**.

<a id="connect-to-an-azure-data-lake-analytics-account" class="xliff"></a>

## Připojení k účtu Azure Data Lake Analytics

1. Otevřete sadu Visual Studio.
2. Otevřete Průzkumníka serveru výběrem **Zobrazení** > **Průzkumník serveru**.
3. Klikněte pravým tlačítkem na **Azure**. Pak vyberte **Připojit k předplatnému Microsoft Azure** a postupujte podle pokynů.
4. V Průzkumníku serveru vyberte **Azure** > **Data Lake Analytics**. Zobrazí se seznam vašich účtů Data Lake Analytics.


<a id="write-your-first-u-sql-script" class="xliff"></a>

## Napsání prvního skriptu U-SQL

Následující text je jednoduchý skript U-SQL. Definuje malou datovou sadu a zapíše ji do výchozího úložiště Data Lake Store jako soubor s názvem `/data.csv`.

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

<a id="submit-a-data-lake-analytics-job" class="xliff"></a>

### Odeslání úlohy Data Lake Analytics

1. Vyberte **Soubor** > **Nový** > **Projekt**.

2. Vyberte typ **Projekt U-SQL** a pak klikněte na **OK**. Visual Studio vytvoří řešení se souborem **Script.usql**.

3. Vložte předchozí skript do okna **Script.usql**.

4. V levém horním rohu okna **Script.usql** zadejte účet Data Lake Analytics.

    ![Odeslání projektu U-SQL sady Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

5. V levém horním rohu okna **Script.usql** vyberte **Odeslat**.
6. Zkontrolujte **Účet Analytics** a pak vyberte **Odeslat**. Po dokončení odeslání jsou výsledky odeslání dostupné ve výsledcích nástrojů Data Lake pro Visual Studio.

    ![Odeslání projektu U-SQL sady Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)
7. Pokud chcete zobrazit nejnovější stav úlohy a aktualizovat obrazovku, klikněte na **Aktualizovat**. Když se úloha úspěšně dokončí, zobrazí se **Graf úlohy**, **Operace s metadaty**, **Historie stavů**, **Diagnostika**:

    ![Graf výkonu úlohy U-SQL Visual Studio Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * **Souhrn úlohy** zobrazuje souhrn úlohy.   
   * **Podrobnosti o úloze** zobrazují konkrétnější informace o úloze, včetně skriptu, prostředků a vrcholů.
   * **Graf úlohy** vizualizuje průběh úlohy.
   * **Operace s metadaty** zobrazují všechny akce provedené s katalogem U-SQL.
   * **Data** zobrazují všechny vstupy a výstupy.
   * **Diagnostika** poskytuje pokročilé analýzy spouštění úlohy a optimalizace výkonu.

<a id="to-check-job-state" class="xliff"></a>

### Postup kontroly stavu úlohy

1. V Průzkumníku serveru vyberte **Azure** > **Data Lake Analytics**. 
2. Rozbalte název účtu Data Lake Analytics.
3. Dvakrát klikněte na **Úlohy**.
4. Vyberte úlohu, kterou jste dříve odeslali.

<a id="to-see-the-output-of-a-job" class="xliff"></a>

### Zobrazení výstupu úlohy

1. V Průzkumníku serveru přejděte na úlohu, kterou jste odeslali.
2. Klikněte na kartu **Data**.
3. Na kartě **Výstupy úlohy** vyberte soubor `"/data.csv"`.

<a id="next-steps" class="xliff"></a>

## Další kroky

* Začínáme se službou Data Lake Analytics pomocí webu [Azure Portal](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) 
* [Ladění kódu C# v úlohách U-SQL](data-lake-analytics-debug-u-sql-jobs.md)
* [Použití nástrojů Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)

