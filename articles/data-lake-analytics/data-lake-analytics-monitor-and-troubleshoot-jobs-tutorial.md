---
title: "Řešení potíží s úlohy Azure Data Lake Analytics pomocí portálu Azure | Microsoft Docs"
description: "Naučte se používat portál Azure k řešení potíží s úloh Data Lake Analytics. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: b7066d81-3142-474f-8a34-32b0b39656dc
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: b9c7453cc0a94f70d0098ed83e5f127832065a62
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-azure-data-lake-analytics-jobs-using-azure-portal"></a>Řešení potíží s úlohy Azure Data Lake Analytics pomocí portálu Azure
Naučte se používat portál Azure k řešení potíží s úloh Data Lake Analytics.

V tomto kurzu instalační program chybějící problém zdrojového souboru a pomocí webu Azure Portal k vyřešení tohoto problému.

## <a name="submit-a-data-lake-analytics-job"></a>Odeslání úlohy Data Lake Analytics

Odeslání následující úlohy U-SQL:

```
@searchlog =
   EXTRACT UserId          int,
           Start           DateTime,
           Region          string,
           Query           string,
           Duration        int?,
           Urls            string,
           ClickedUrls     string
   FROM "/Samples/Data/SearchLog.tsv1"
   USING Extractors.Tsv();

OUTPUT @searchlog   
   TO "/output/SearchLog-from-adls.csv"
   USING Outputters.Csv();
```
    
Zdrojový soubor definované ve skriptu je **/Samples/Data/SearchLog.tsv1**, kde by měly být **/Samples/Data/SearchLog.tsv**.


## <a name="troubleshoot-the-job"></a>Řešení potíží s úlohy

**Chcete-li zobrazit všechny úlohy**

1. Z portálu Azure klikněte na tlačítko **Microsoft Azure** v levém horním rohu.
2. Klikněte na dlaždici s názvem účtu Data Lake Analytics.  Úlohu souhrnu se zobrazí na **úlohy správy** dlaždici.

    ![Správa úloh Azure Data Lake Analytics](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    Úlohy správy poskytuje přehled o stavu úlohy. Všimněte si, že je neúspěšnou úlohu.
3. Klikněte **úlohy správy** dlaždice zobrazíte úlohy. Úlohy jsou rozdělené do **systémem**, **zařazeno ve frontě**, a **ukončeno**. Zobrazí se vaše neúspěšnou úlohu v **ukončeno** části. Použije se první z nich v seznamu. Pokud máte mnoho úloh, můžete kliknout na **filtru** můžete vyhledat úlohy.

    ![Filtrujte úlohy Azure Data Lake Analytics](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)
4. Klikněte na možnost neúspěšné úlohy ze seznamu otevřete nové okno Podrobnosti úlohy:

    ![Azure Data Lake Analytics se nezdařilo úlohy](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    Upozornění **odešlete znovu** tlačítko. Po vyřešení problému znovu odešlete úlohu.
5. Klikněte na tlačítko zvýrazněná část na předchozím snímku obrazovky Otevřít podrobnosti o chybě.  Zobrazí se něco podobného jako:

    ![Azure Data Lake Analytics se nezdařilo podrobnosti úlohy](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    Zde zjistíte, že zdrojová složka nebyla nalezena.
6. Klikněte na tlačítko **duplicitní skriptu**.
7. Aktualizace **FROM** cesta pro následující:

    "/ Samples/Data/SearchLog.tsv"
8. Klikněte na **Odeslat úlohu**.

## <a name="see-also"></a>Viz také
* [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Začínáme s Azure Data Lake Analytics pomocí Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Začínáme s Azure Data Lake Analytics a jazykem U-SQL pomocí sady Visual Studio](data-lake-analytics-u-sql-get-started.md)
* [Správa Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md)
