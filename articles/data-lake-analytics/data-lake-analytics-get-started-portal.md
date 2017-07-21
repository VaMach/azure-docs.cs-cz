---
title: "Začínáme s Azure Data Lake Analytics pomocí portálu Azure Portal | Dokumentace Microsoftu"
description: "Naučte se používat Azure Portal k vytvoření účtu Data Lake Analytics, vytvoření úlohy Data Lake Analytics pomocí U-SQL a odeslání úlohy. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: edmaca
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: b004310a3b2667134c9ad890bd907dc8ae439dd9
ms.contentlocale: cs-cz
ms.lasthandoff: 06/16/2017


---
# Začínáme s Azure Data Lake Analytics s využitím webu Azure Portal
<a id="get-started-with-azure-data-lake-analytics-using-azure-portal" class="xliff"></a>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Naučte se používat Azure Portal k vytváření účtů Azure Data Lake Analytics, definování úloh v [U-SQL](data-lake-analytics-u-sql-get-started.md) a odesílání úloh do služby Data Lake Analytics. Další informace o Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).

## Požadavky
<a id="prerequisites" class="xliff"></a>

Než začnete tento kurz, musíte mít **předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

## Vytvoření účtu Data Lake Analytics
<a id="create-a-data-lake-analytics-account" class="xliff"></a>

Teď zároveň vytvoříte účet Data Lake Analytics a Data Lake Store.  Tento krok je jednoduchý a trvá jen asi 60 vteřin.

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. Klikněte na **Nový** >  **Inteligentní funkce a analýzy** > **Data Lake Analytics**.
3. Vyberte hodnoty pro následující položky:
   * **Název:** Pojmenujte svůj účet Data Lake Analytics (povolena jsou pouze malá písmena a číslice).
   * **Předplatné**: Zvolte předplatné Azure použité pro účet Analytics.
   * **Skupina prostředků**. Vyberte některou z existujících skupin prostředků Azure nebo vytvořte novou.
   * **Umístění**. Vyberte datové centrum Azure pro účet Data Lake Analytics.
   * **Data Lake Store**: Postupujte podle pokynů a vytvořte nový účet Data Lake Store nebo vyberte některý z existujících. 
4. Volitelně vyberte cenovou úroveň pro svůj účet Data Lake Analytics.
5. Klikněte na možnost **Vytvořit**. 


## Váš první skript U-SQL
<a id="your-first-u-sql-script" class="xliff"></a>

Následující text je velmi jednoduchý skript U-SQL. Celá jeho úloha spočívá v definování malé datové sady v rámci skriptu a následném zapsání této datové sady do výchozího úložiště Data Lake Store jako soubor s názvem `/data.csv`.

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

## Odeslání úlohy U-SQL
<a id="submit-a-u-sql-job" class="xliff"></a>

1. V účtu Data Lake Analytics klikněte na možnost **Nová úloha**.
2. Vložte výše uvedený text skriptu U-SQL. 
3. Klikněte na **Odeslat úlohu**.   
4. Počkejte, až se stav úlohy změní na **Úspěch**.
5. Pokud úloha nebyla úspěšná, přejděte na téma [Sledování úloh Data Lake Analytics a odstraňování potíží](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).
6. Klikněte na kartu **Výstup** a potom na `SearchLog-from-Data-Lake.csv`. 

## Viz také
<a id="see-also" class="xliff"></a>

* Pokud chcete začít s vývojem aplikací U-SQL, přejděte k tématu [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* Pokud se chcete naučit jazyk U-SQL, informace najdete v tématu [Začínáme s jazykem U-SQL Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informace týkající se úloh správy najdete v tématu [Správa služby Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).

