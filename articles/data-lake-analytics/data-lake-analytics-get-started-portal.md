<properties 
   pageTitle="Začínáme s Azure Data Lake Analytics pomocí Portálu Azure | Azure" 
   description="Naučte se používat Portál Azure k vytvoření účtu Data Lake Analytics, vytvoření úlohy Data Lake Analytics pomocí U-SQL a odeslání úlohy. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# Kurz: Začínáme s Azure Data Lake Analytics pomocí Portálu Azure

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Naučte se používat web Azure Portal k vytváření účtů Azure Data Lake Analytics, definování úloh Data Lake Analytics v [U-SQL](data-lake-analytics-u-sql-get-started.md) a odesílání úloh do účtů Data Lake Analytics. Další informace o Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).

V tomto kurzu budete vyvíjet úlohu, která načte soubor hodnot oddělených tabulátory (TSV) a převede jej na soubor hodnot oddělených čárkami (CSV). Pokud chcete použít jiné podporované nástroje a absolvovat stejný kurz, klikněte na karty nahoře v této části. Po úspěšném vytvoření první úlohy můžete začít psát složitější transformace dat pomocí U-SQL.

##Požadavky

Je nutné, abyste před zahájením tohoto kurzu měli tyto položky:

- **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

##Vytvoření účtu Data Lake Analytics

Je nutné, abyste před spuštěním jakékoli úlohy měli účet Data Lake Analytics.

Každý účet Data Lake Analytics má závislost účtu [Azure Data Lake Store]().  Tento účet se nazývá výchozí účet Data Lake Store.  Účet Data Lake Store můžete vytvořit předem nebo při vytváření účtu Data Lake Analytics. V tomto kurzu vytvoříte účet Data Lake Store s účtem Data Lake Analytics.

**Postup vytvoření účtu Data Lake Analytics**

1. Přihlaste se na nový [Portál Azure Classic](https://portal.azure.com).
2. Klikněte na možnost **Nový**, klikněte na možnost **Data + analýzy** a potom klikněte na možnost **Data Lake Analytics**.
6. Zadejte nebo vyberte tyto položky:

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **Název**: Zadejte název účtu Analytics.
    - **Data Lake Store**: Každý účet Data Lake Analytics má závislý účet Data Lake Store. Účet Data Lake Analytics a závislý účet Data Lake Store se musí nacházet ve stejném datovém centru Azure. Postupujte podle pokynů a vytvořte nový účet Data Lake Store nebo vyberte některý z existujících.
    - **Předplatné**: Zvolte předplatné Azure použité pro účet Analytics.
    - **Skupina prostředků**. Vyberte některou z existujících skupin prostředků Azure nebo vytvořte novou. Azure Resource Manager (ARM) umožňuje pracovat s prostředky v aplikaci jako se skupinou. Další informace najdete v tématu [Přehled Azure Resource Manageru](resource-group-overview.md). 
    - **Umístění**. Vyberte datové centrum Azure pro účet Data Lake Analytics. 
7. Vyberte položku **Připnout na Úvodní panel**. Je to nezbytné pro absolvování tohoto kurzu.
8. Klikněte na možnost **Vytvořit**. Tím přejdete na Úvodní panel portálu. Na Úvodní panel se přidá nová dlaždice s popiskem „Nasazení Azure Data Lake Analytics“. Vytvoření účtu Data Lake Analytics chvíli trvá. Jakmile je účet vytvořený, portál ho otevře v novém okně portálu.

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)


Po vytvoření účtu Data Lake Analytics můžete přidat další účty Data Lake Store a účty Azure Storage. Pokyny najdete v tématu [Správa zdrojů dat účtu Data Lake Analytics](data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

##Příprava zdrojových dat

V tomto kurzu budete zpracovávat několik protokolů hledání.  Protokol hledání se dá uložit buď do úložiště Data Lake Store, nebo do úložiště objektů Azure Blob. 

Portál Azure poskytuje uživatelské rozhraní pro kopírování některých ukázkových datových souborů (včetně souboru protokolu hledání) do výchozího účtu Data Lake.

**Postup kopírování ukázkových datových souborů**

1. Na webu Azure Portal klikněte v levém horním rohu na možnost **Microsoft Azure**.
2. Klikněte na dlaždici s názvem účtu Data Lake Analytics.  Byla sem připnuta při vytvoření účtu.
Pokud zde účet není připnutý, přejděte k tématu [Otevření účtu Data Lake Analytics z portálu](data-lake-analytics-manage-use-portal.md#access-adla-account) a otevřete účet.
3. Rozbalte podokno **Základy** a klikněte na možnost **Prozkoumat ukázkové úlohy**. Otevře se další okno s názvem **Ukázkové úlohy**.
4. Klikněte na možnost **Kopírovat ukázková data** a potvrďte kliknutím na tlačítko **OK**.
5. Klikněte na položku **Oznámení**, což je ikona ve tvaru zvonu. Zobrazí se protokol s informací **Aktualizace ukázkových dat je dokončená**. Klikněte kamkoli mimo podokno oznámení, a tím ho zavřete.
7. V okně účtu Data Lake Analytics klikněte nahoře na možnost **Průzkumník dat**. 

    ![Tlačítko průzkumníku dat Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-data-explorer-button.png)

    Otevřou se dvě okna. Jedno je **Průzkumník dat** a druhé je výchozí účet Data Lake Store.
8. V okně výchozího účtu Data Lake Store rozbalte složku kliknutím na možnost **Ukázky** a potom rozbalte složku kliknutím na možnost **Data**. Zobrazí se tyto soubory a složky:

    - AmbulanceData/
    - AdsLog.tsv
    - SearchLog.tsv
    - version.txt
    - WebLog.log
    
    V tomto kurzu budete používat soubor SearchLog.tsv.

V praxi buď naprogramujete aplikace tak, aby zapsaly data do propojeného účtu úložiště, nebo nahrajete data. Informace týkající se nahrávání souborů najdete v části [Nahrání dat do Data Lake Store](data-lake-analytics-manage-use-portal.md#upload-data-to-adls) nebo [Nahrání dat do úložiště objektů blob](data-lake-analytics-manage-use-portal.md#upload-data-to-wasb).

##Vytvoření a odeslání úloh Data Lake Analytics

Jakmile připravíte zdrojová data, můžete začít vyvíjet skript U-SQL.  

**Postup odeslání úlohy**

1. V okně účtu Data Lake Analytics na portálu klikněte na možnost **Nová úloha**. 

    ![Tlačítko nové úlohy Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-new-job-button.png)

    Pokud není toto okno zobrazené, informace najdete v tématu [Otevření účtu Data Lake Analytics z portálu](data-lake-analytics-manage-use-portal.md#access-adla-account).
4. Zadejte **Název úlohy** a tento skript U-SQL:

    ![vytvoření úloh U-SQL Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-new-job.png)

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

    Tento skript U-SQL přečte zdrojový datový soubor pomocí **Extractors.Tsv()** a potom pomocí **Outputters.Csv()** vytvoří soubor .csv. 
    
    Pokud zdrojový soubor nekopírujete do jiného umístění, tyto dvě cesty neupravujte.  Data Lake Analytics vytvoří výstupní složku, pokud neexistuje.  V tomto případě používáme jednoduché, relativní cesty.  
    
    U souborů uložených ve výchozích účtech Data Lake je jednodušší použít relativní cesty. Můžete také použít absolutní cesty.  Například 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
      

    Další informace týkající se jazyka U-SQL najdete v tématu [Začínáme s jazykem U-SQL Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md) a [Referenční informace pro jazyk U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).
     
5. Klikněte shora na možnost **Odeslat úlohu**. Otevře se nové podokno Podrobnosti úlohy. V záhlaví se zobrazuje stav úlohy.   
6. Počkejte, až se stav úlohy změní na **Úspěch**. Když se úloha dokončí, portál otevře nové okno s podrobnostmi o úloze:

    ![Podrobnosti úlohy Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-job-completed.png)

    Na předchozím snímku obrazovky vidíte, že dokončení úlohy (od Odesláno po Dokončeno) trvalo asi 1,5 minuty.
    
    Pokud úloha nebyla úspěšná, informace najdete v tématu [Sledování úloh Data Lake Analytics a odstraňování potíží](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorials.md).

7. V dolní části okna **Podrobnosti úlohy** klikněte na název úlohy v souboru **SearchLog-from-Data-Lake.csv**. Můžete stáhnout, přejmenovat nebo odstranit výstupní soubor nebo zobrazit jeho náhled.

    ![Vlastnosti výstupního souboru Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-output-file-properties.png)
8. Kliknutím na položku **Náhled** zobrazte výstupní soubor.

    ![Náhled výstupního souboru úlohy Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-job-output-preview.png)

##Viz také

- Pokud chcete zobrazit komplexnější dotaz, přejděte k tématu [Analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Pokud chcete začít s vývojem aplikací U-SQL, přejděte k tématu [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Pokud se chcete naučit jazyk U-SQL, informace najdete v tématu [Začínáme s jazykem U-SQL Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
- Informace týkající se úloh správy najdete v tématu [Správa Azure Data Lake Analytics pomocí Portálu Azure](data-lake-analytics-manage-use-portal.md).
- Přehled Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).
- Pokud chcete použít jiné podporované nástroje a zobrazit stejný kurz, klikněte na selektory karet v horní části stránky.
- Pokud chcete protokolovat diagnostické informace, přečtěte si téma [Zobrazení protokolů diagnostiky pro Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).



<!--HONumber=Sep16_HO3-->


