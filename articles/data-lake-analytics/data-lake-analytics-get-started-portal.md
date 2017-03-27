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
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 7f23ae904461e754b3871b33ca40c2ff4fcb99f0
ms.lasthandoff: 03/22/2017


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-portal"></a>Kurz: Začínáme s Azure Data Lake Analytics pomocí portálu Azure Portal
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Naučte se používat Azure Portal k vytváření účtů Azure Data Lake Analytics, definování úloh v [U-SQL](data-lake-analytics-u-sql-get-started.md) a odesílání úloh do služby Data Lake Analytics. Další informace o Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).

V tomto kurzu budete vyvíjet úlohu, která načte soubor hodnot oddělených tabulátory (TSV) a převede jej na soubor hodnot oddělených čárkami (CSV). Pokud chcete použít jiné podporované nástroje a absolvovat stejný kurz, klikněte na karty nahoře v této části. Po úspěšném vytvoření první úlohy můžete začít psát složitější transformace dat pomocí U-SQL.

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu musíte mít tyto položky:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-data-lake-analytics-account"></a>Vytvoření účtu Data Lake Analytics
Je nutné, abyste před spuštěním jakékoli úlohy měli účet Data Lake Analytics.

Každý účet Data Lake Analytics má závislost účtu Azure Data Lake Store.  Tento účet se nazývá výchozí účet Data Lake Store.  Účet Data Lake Store můžete vytvořit předem nebo při vytváření účtu Data Lake Analytics. V tomto kurzu vytvoříte účet Data Lake Store s účtem Data Lake Analytics.

**Vytvoření účtu Data Lake Analytics**

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. Klikněte na **Nový**, klikněte na **Inteligence a analýza** a pak klikněte na **Data Lake Analytics**.
3. Zadejte nebo vyberte tyto hodnoty:

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

   * **Název:** Pojmenujte svůj účet Data Lake Analytics (povolena jsou pouze malá písmena a číslice).
   * **Předplatné**: Zvolte předplatné Azure použité pro účet Analytics.
   * **Skupina prostředků**. Vyberte některou z existujících skupin prostředků Azure nebo vytvořte novou. Azure Resource Manager umožňuje pracovat s prostředky v aplikaci jako se skupinou. Další informace najdete v tématu [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).
   * **Umístění**. Vyberte datové centrum Azure pro účet Data Lake Analytics.
   * **Data Lake Store:** Klikněte na *Konfigurovat požadovaná nastavení*. Postupujte podle pokynů a vytvořte nový účet Data Lake Store nebo vyberte některý z existujících. Každý účet Data Lake Analytics má závislý účet Data Lake Store. Účet Data Lake Analytics a závislý účet Data Lake Store se musí nacházet ve stejném datovém centru Azure.
4. Vyberte si cenovou úroveň.  
5. Klikněte na možnost **Vytvořit**. Vrátíte se na domovskou obrazovku portálu, kde se zobrazí nová dlaždice s popiskem „Nasazování služby Azure Data Lake Analytics“. Procesu nasazení bude trvat několik minut, než vytvoří účet Data Lake Analytics. Jakmile je účet vytvořený, portál ho otevře v novém okně.

Po vytvoření účtu Data Lake Analytics můžete přidat další účty Data Lake Store a účty Azure Storage. Pokyny najdete v tématu [Správa zdrojů dat účtu Data Lake Analytics](data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

## <a name="prepare-source-data"></a>Příprava zdrojových dat
V tomto kurzu budete zpracovávat protokoly hledání.  Protokol hledání se dá uložit buď do úložiště Data Lake Store, nebo do úložiště Azure Blob Storage.

Azure Portal poskytuje uživatelské rozhraní pro kopírování ukázkových datových souborů (včetně souboru protokolu hledání) do výchozího účtu Data Lake Store.

**Kopírování ukázkových datových souborů**

1. Z portálu [Azure Portal](https://portal.azure.com) otevřete svůj účet Data Lake Analytics.  Vytvořte ho podle tématu [Správa účtů Data Lake Analytics](data-lake-analytics-get-started-portal.md#create-data-lake-analytics-account) a otevřete ho na portálu.
2. Rozbalte podokno **Základy** a klikněte na **Prozkoumat ukázkové skripty**. Otevře se další okno s názvem **Ukázkové skripty**.

    ![Ukázkový skript portálu Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-sample-scripts.png)
3. Kliknutím na **Ukázková data chybí** zkopírujte ukázkové datové soubory. Po dokončení portál zobrazí **Aktualizace ukázkových dat byla úspěšně dokončena**.
4. V okně účtu Data Lake Analytics klikněte nahoře na možnost **Průzkumník dat**.

    ![Tlačítko průzkumníku dat Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-data-explorer-button.png)

    Otevřou se dvě okna. Jedno je **Průzkumník dat** a druhé je výchozí účet Data Lake Store.
5. V okně výchozího účtu Data Lake Store rozbalte složku kliknutím na možnost **Ukázky** a potom rozbalte složku kliknutím na možnost **Data**. Zobrazí se tyto soubory a složky:

   * AmbulanceData/
   * AdsLog.tsv
   * SearchLog.tsv
   * version.txt
   * WebLog.log

     V tomto kurzu budete používat soubor SearchLog.tsv.

V praxi buď naprogramujete aplikace tak, aby zapsaly data do propojeného účtu úložiště, nebo nahrajete data. Informace týkající se nahrávání souborů najdete v části [Nahrání dat do Data Lake Store](data-lake-analytics-manage-use-portal.md#upload-data-to-adls) nebo [Nahrání dat do úložiště objektů blob](data-lake-analytics-manage-use-portal.md#upload-data-to-wasb).

## <a name="create-and-submit-data-lake-analytics-jobs"></a>Vytvoření a odeslání úloh Data Lake Analytics
Jakmile připravíte zdrojová data, můžete začít vyvíjet skript U-SQL.  

**Odeslání úlohy**

1. V okně účtu Data Lake Analytics na portálu klikněte na možnost **Nová úloha**.

    ![Tlačítko nové úlohy Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-new-job-button.png)

    Pokud není toto okno zobrazené, informace najdete v tématu [Otevření účtu Data Lake Analytics z portálu](data-lake-analytics-manage-use-portal.md#access-adla-account).
2. Zadejte **Název úlohy** a tento skript U-SQL:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();

        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    ![vytvoření úloh U-SQL Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-new-job.png)

    Tento skript U-SQL přečte zdrojový datový soubor pomocí **Extractors.Tsv()** a potom pomocí **Outputters.Csv()** vytvoří soubor .csv.

    Pokud zdrojový soubor nekopírujete do jiného umístění, tyto dvě cesty neupravujte.  Data Lake Analytics vytvoří výstupní složku, pokud neexistuje.  V tomto případě používáme jednoduché, relativní cesty.  

    U souborů uložených ve výchozích účtech Data Lake je jednodušší použít relativní cesty. Můžete také použít absolutní cesty.  Například

        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Další informace týkající se jazyka U-SQL najdete v tématu [Začínáme s jazykem U-SQL Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md) a [Referenční informace pro jazyk U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

1. Klikněte shora na možnost **Odeslat úlohu**.   
2. Počkejte, až se stav úlohy změní na **Úspěch**. Jak vidíte, dokončení úlohy trvalo asi jednu minutu.

    Pokud úloha nebyla úspěšná, informace najdete v tématu [Sledování úloh Data Lake Analytics a odstraňování potíží](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).
3. V dolní části okna klikněte na kartu **Výstup** a pak na **SearchLog-from-Data-Lake.csv**. Můžete stáhnout, přejmenovat nebo odstranit výstupní soubor nebo zobrazit jeho náhled.

    ![Vlastnosti výstupního souboru Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-output-file-properties.png)

## <a name="see-also"></a>Viz také
* Pokud chcete zobrazit komplexnější dotaz, přejděte k tématu [Analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Pokud chcete začít s vývojem aplikací U-SQL, přejděte k tématu [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* Pokud se chcete naučit jazyk U-SQL, informace najdete v tématu [Začínáme s jazykem U-SQL Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informace týkající se úloh správy najdete v tématu [Správa služby Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).
* Přehled Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).
* Pokud chcete použít jiné podporované nástroje a zobrazit stejný kurz, klikněte na selektory karet v horní části stránky.
* Pokud chcete protokolovat diagnostické informace, přečtěte si téma [Zobrazení protokolů diagnostiky pro Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).

