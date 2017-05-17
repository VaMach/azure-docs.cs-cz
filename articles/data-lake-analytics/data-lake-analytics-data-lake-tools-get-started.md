---
title: "Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio | Dokumentace Microsoftu"
description: "Naučte se nainstalovat nástroje Data Lake pro Visual Studio a vyvíjet a testovat skripty U-SQL. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/06/2017
ms.author: edmaca, yanacai
ms.translationtype: Human Translation
ms.sourcegitcommit: 0b53a5ab59779dc16825887b3c970927f1f30821
ms.openlocfilehash: c26ac89bd7ef494331ba309aacf87de03506ac4c
ms.contentlocale: cs-cz
ms.lasthandoff: 04/07/2017


---
# <a name="tutorial-develop-u-sql-scripts-using-data-lake-tools-for-visual-studio"></a>Kurz: Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Vytvořte a otestujte skripty U-SQL s využitím nástrojů Data Lake pro Visual Studio.

U-SQL je mimořádně škálovatelný a vysoce rozšiřitelný jazyk pro přípravu, transformaci a analýzu všech dat v úložišti Data Lake i mimo něj. Další informace najdete v tématu [Referenční informace o U-SQL](http://go.microsoft.com/fwlink/p/?LinkId=691348).

## <a name="prerequisites"></a>Požadavky
* **Visual Studio 2017 (v rámci úlohy úložiště dat a zpracování), Visual Studio 2015 Update 3, Visual Studio 2013 Update 4 nebo Visual Studio 2012. Jsou podporovány edice Enterprise (Ultimate nebo Premium), Professional a Community; není podporována edice Express.**
* **Sada Microsoft Azure SDK pro .NET verze 2.7.1 nebo vyšší**.  Nainstalujte ji pomocí [Instalačního programu webové platformy](http://www.microsoft.com/web/downloads/platform.aspx).
* **[Nástroje Data Lake pro Visual Studio](http://aka.ms/adltoolsvs)**.

    Po instalaci nástrojů Data Lake pro Visual Studio se v Průzkumníku serveru pod uzlem Azure zobrazí uzel Data Lake Analytics (Průzkumníka serveru otevřete stisknutím kombinace kláves Ctrl+Alt+S).

* **Účet Data Lake Analytics a ukázková data**. Nástroje Data Lake nepodporují vytváření účtů Data Lake Analytics. Vytvořte účet pomocí webu Azure Portal, Azure PowerShellu, sady .NET SDK nebo rozhraní příkazového řádku Azure CLI.
Pro usnadnění práce najdete v tématu [Ukázku Appx-A prostředí PowerShell pro přípravu kurzu](data-lake-analytics-data-lake-tools-get-started.md#appx-a-powershell-sample-for-preparing-the-tutorial) ukázkový skript prostředí PowerShell pro vytvoření služby Data Lake Analytics a nahrání zdrojového datového souboru.

    Volitelně si můžete projít následující dvě části tématu [Začínáme s Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-get-started-portal.md) a vytvořit si účet a nahrát data ručně:

    1. [Vytvoření účtu Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md#create-data-lake-analytics-account).
    2. [Nahrání souboru SearchLog.tsv do výchozího účtu Data Lake Storage](data-lake-analytics-get-started-portal.md#prepare-source-data).

## <a name="connect-to-azure"></a>Připojení k Azure
**Připojení ke službě Data Lake Analytics**

1. Otevřete sadu Visual Studio.
2. V nabídce **Zobrazení** klikněte na tlačítko **Průzkumník serveru** a otevřete Průzkumníka serveru. Můžete také stisknout kombinaci kláves **[CTRL]+[ALT]+S**.
3. Klikněte pravým tlačítkem na **Azure**, klikněte na položku „Připojit k předplatnému Microsoft Azure“ a postupujte podle pokynů.
4. V **Průzkumníku serveru** rozbalte položku **Azure** a pak rozbalte položku **Data Lake Analytics**. Zobrazí se seznam účtů Data Lake Analytics, pokud nějaké máte. Účty Data Lake Analytics nelze vytvářet ze sady Visual Studio. Pokud chcete vytvořit účet, informace najdete v tématu [Začínáme s Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-get-started-portal.md) nebo [Začínáme s Azure Data Lake Analytics pomocí Azure PowerShell](data-lake-analytics-get-started-powershell.md).

## <a name="upload-source-data-files"></a>Nahrání zdrojových datových souborů
Nějaká data jste již v tomto kurzu nahráli v části **Požadavky**.  

Chcete-li použít vlastní data, postupujte podle těchto kroků a nahrajte data z nástrojů Data Lake.

**Nahrání souborů do závislého účtu Azure Data Lake**

1. V **Průzkumníku serveru** rozbalte položku **Azure**, rozbalte položku **Data Lake Analytics**, rozbalte účet Data Lake Analytics a rozbalte položku **Účty Storage**. Zobrazí se výchozí účet Data Lake Storage, propojené účty Data Lake Storage a propojené účty Azure Storage. Výchozí účet Data Lake má popisek „Výchozí účet úložiště“.
2. Klikněte pravým tlačítkem na výchozí účet Data Lake Storage a potom klikněte na položku **Explorer**.  Otevře se podokno Nástroje Data Lake pro Průzkumníka Visual Studio.  Na levé straně je stromové zobrazení a na pravé straně je zobrazení obsahu.
3. Přejděte do složky, kam chcete nahrát soubory,
4. Klikněte pravým tlačítkem na libovolné prázdné místo a pak klikněte na položku **Nahrát**.

    ![Projekt U-SQL Visual Studio U-SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-upload-files.png)

**Nahrání souborů do propojeného účtu úložiště Azure Blob**

1. V **Průzkumníku serveru** rozbalte položku **Azure**, rozbalte položku **Data Lake Analytics**, rozbalte účet Data Lake Analytics a rozbalte položku **Účty Storage**. Zobrazí se výchozí účet Data Lake Storage, propojené účty Data Lake Storage a propojené účty Azure Storage.
2. Rozbalte úložiště Azure Storage.
3. Klikněte pravým tlačítkem na kontejner, kam chcete nahrát soubory, a pak klikněte na možnost **Explorer**. Pokud nemáte kontejner, musíte ho nejdřív vytvořit pomocí Portálu Azure, prostředí Azure PowerShell nebo jiných nástrojů.
4. Přejděte do složky, kam chcete nahrát soubory,
5. Klikněte pravým tlačítkem na libovolné prázdné místo a pak klikněte na položku **Nahrát**.

## <a name="develop-u-sql-scripts"></a>Vývoj skriptů U-SQL
Úlohy Data Lake Analytics se píšou v jazyce U-SQL. Další informace o U-SQL najdete v tématu [Začínáme s jazykem U-SQL](data-lake-analytics-u-sql-get-started.md) a [Referenční informace pro jazyk U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

**Vytvoření a odeslání úlohy Data Lake Analytics**

1. V nabídce **Soubor** klikněte na položku **Nový** a potom klikněte na položku **Projekt**.
2. Vyberte typ **Projekt U-SQL**.

    ![Nový projekt U-SQL Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. Klikněte na tlačítko **OK**. Visual Studio vytvoří řešení se souborem **Script.usql**.
4. Do souboru **Script.usql** zadejte následující skript:

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

        @res =
            SELECT *
            FROM @searchlog;        

        OUTPUT @res   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Tento skript U-SQL přečte zdrojový datový soubor pomocí **Extractors.Tsv()** a potom pomocí **Outputters.Csv()** vytvoří soubor .csv.

    Pokud jste zdrojový soubor nezkopírovali do jiného umístění, tyto dvě cesty neupravujte.  Data Lake Analytics vytvoří výstupní složku, pokud neexistuje.

    U souborů uložených ve výchozích účtech Data Lake je jednodušší použít relativní cesty. Můžete také použít absolutní cesty.  Například

        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Pro přístup k souborům v propojených účtech Storage je nutné použít absolutní cesty.  V případě souborů uložených v propojeném účtu Azure Storage je syntaxe následující:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

   > [!NOTE]
   > Kontejnery Azure Blob s přístupovými oprávněními veřejných objektů blob nebo veřejných kontejnerů nejsou aktuálně podporované.  
   >
   >

    Všimněte si těchto funkcí:

   * **IntelliSense**

       Automaticky se vyplní název a zobrazí se členové Sad řádků, Tříd, Databází, Schémat a Objektů definovaných uživatelem (UDO).

       Technologie IntelliSense pro entity katalogu (Databáze, Schémata, Tabulky, UDO atd.) se vztahuje k výpočetnímu účtu. Aktuální aktivní výpočetní účet, databázi a schéma můžete zkontrolovat v horním panelu nástrojů a můžete je přepnout pomocí rozevíracích seznamů.
   * **Rozbalení * sloupců**

       Klikněte vpravo od * a pod * se zobrazí modré podtržení. Umístěte myš na modré podtržení a klikněte na šipku dolů.
       ![Nástroje Data Lake Visual Studio, rozbalení *](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-expand-asterisk.png)

       Klikněte na položku **Rozbalit sloupce** a nástroj nahradí znak * názvy sloupců.
   * **Automatický formát**

       Uživatelé můžou změnit odsazení skriptu U-SQL na základě struktury kódu v nabídce Upravit -> Rozšířené:

     * Formátovat dokument (Ctrl+E, D): Naformátuje celý dokument.   
     * Formátovat výběr (Ctrl+K, Ctrl+F): Naformátuje výběr. Pokud není nic vybráno, tato zkratka naformátuje řádek, ve kterém se nachází kurzor.  

       Veškerá pravidla formátování lze konfigurovat v nabídce Nástroje -> Možnosti -> Textový editor - > SIP -> Formátování.  
   * **Inteligentní odsazení**

       Nástroje Data Lake pro Visual Studio dokážou během psaní skriptů automaticky odsazovat výrazy. Tato funkce je ve výchozím nastavení zakázána a uživatelé ji musí povolit zaškrtnutím možnosti U-SQL -> Možnosti a nastavení -> Přepínače -> Povolit inteligentní odsazení.
   * **Přechod k definici a nalezení všech odkazů**

       Klikněte pravým tlačítkem na název Sady řádků / parametru / sloupce / UDO atd. a poté můžete přejít k příslušné definici kliknutím na položku Přejít k definici (F12). Kliknutím na položku Najít všechny odkazy (Shift+F12) se zobrazí všechny odkazy.
   * **Vložení cesty Azure**

       Díky nástrojům Data Lake si už při psaní skriptu nemusíte pamatovat cestu k souboru Azure a ručně ji zadat, ale můžete využít snadnější způsob: klikněte v editoru pravým tlačítkem myši a klikněte na položku Vložit cestu Azure. Přejděte k požadovanému souboru v dialogovém okně prohlížeče objektů blob Azure. Klikněte na tlačítko **OK**. Cesta k souboru se vloží do kódu.
5. Zadejte účet Data Lake Analytics, Databázi a Schéma. Můžete vybrat možnost **(místní)** a pro účely testování spustit skript místně. Další informace najdete v tématu [Místní spuštění U-SQL](#run-u-sql-locally).

    ![Odeslání projektu U-SQL sady Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

    Další informace najdete v tématu [Použití katalogu U-SQL](data-lake-analytics-use-u-sql-catalog.md).
6. V **Průzkumníku řešení** klikněte pravým tlačítkem na položku **Script.usql** a pak klikněte na možnost **Sestavit skript**. Ověřte výsledek v podokně Výstup.
7. V **Průzkumníku řešení** klikněte pravým tlačítkem na položku **Script.usql** a pak klikněte na položku **Odeslat skript**. Můžete také kliknout na tlačítko **Odeslat** v podokně Script.usql.  Viz předchozí snímek obrazovky.  Pokud chcete při odeslání použít rozšířené možnosti, klikněte na šipku dolů vedle tlačítka Odeslat:
8. Zadejte **Název úlohy**, ověřte **Účet Analytics** a klikněte na tlačítko **Odeslat**. Jakmile je odeslání hotové, v okně Výsledky nástrojů Data Lake pro Visual Studio jsou dostupné výsledky odeslání a odkaz na úlohu.

    ![Odeslání projektu U-SQL sady Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)
9. Pokud chcete zobrazit nejnovější stav úlohy a aktualizovat obrazovku, je nutné kliknout na tlačítko Aktualizovat. Když se úloha úspěšně dokončí, zobrazí se **Graf úlohy**, **Operace s metadaty**, **Historie stavů**, **Diagnostika**:

    ![Graf výkonu úlohy U-SQL Visual Studio Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * Souhrn úlohy. Zobrazuje souhrnné informace o aktuální úloze, jako je např.: Stav, Průběh, Doba provádění, Název modulu runtime, Odesílatel atd.   
   * Podrobnosti úlohy. Jsou dostupné podrobné informace o této úloze, včetně skriptu, prostředku nebo Zobrazení provádění vrcholů.
   * Graf úlohy. Jsou dostupné čtyři grafy k vizualizaci informací úlohy: Průběh, Přečtená data, Zapsaná data, Doba provádění, Průměrná doba provádění na jeden uzel, Propustnost vstupu, Propustnost výstupu.
   * Operace s metadaty. Zobrazuje všechny operace s metadaty.
   * Historie stavů.
   * Diagnostika. Nástroje Data Lake pro Visual Studio budou automaticky diagnostikovat provádění úlohy. Pokud při úlohách nastanou chyby nebo problémy s výkonem, zobrazí se výstrahy. Další informace najdete v části Diagnostika úlohy (odkaz bude doplněn).

**Postup kontroly stavu úlohy**

1. V Průzkumníku serveru rozbalte položku **Azure**, rozbalte položku **Data Lake Analytics** a rozbalte název účtu Data Lake Analytics.
2. Dvojím kliknutím na možnost **Úlohy** zobrazte seznam úloh.
3. Klikněte na úlohu, jejíž stav chcete zobrazit.

**Postup zobrazení výstupu úlohy**

1. V **Průzkumníku serveru** rozbalte položku **Azure**, rozbalte položku **Data Lake Analytics**, rozbalte účet Data Lake Analytics, rozbalte položku **Účty Storage**, klikněte pravým tlačítkem na výchozí účet Data Lake Storage a potom klikněte na položku **Explorer**.
2. Dvojím kliknutím na možnost **výstup** otevřete složku.
3. Dvakrát klikněte na položku **SearchLog-From-adltools.csv**.

### <a name="job-playback"></a>Přehrávání úloh
Přehrání úlohy umožňuje sledovat průběh provádění úlohy a vizuálně detekovat kritická místa a anomálie výkonu. Tato funkce se dá použít před dokončením provádění úlohy (tj. po dobu, kdy je úloha aktivně spuštěná) i po dokončení provádění. Přehrávání během provádění úlohy umožňuje přehrát průběh až do aktuálního okamžiku.

**Postup zobrazení průběhu provádění úlohy**  

1. Klikněte v pravém horním rohu na možnost **Profil zatížení**. Viz předchozí snímek obrazovky.
2. Klikněte v levém dolním rohu na tlačítko Přehrát a překontrolujte průběh provádění úlohy.
3. Přehrávání můžete kdykoli pozastavit kliknutím na tlačítko **Pozastavit** a můžete také přímo přetáhnout indikátor průběhu na konkrétní pozici.

### <a name="heat-map"></a>Heat mapa
Nástroje Data Lake pro Visual Studio poskytují v zobrazení úlohy barevné překryvné vrstvy vybrané uživatelem, které znázorňují průběh, vstup/výstup dat, dobu provádění nebo propustnost vstupu/výstupu u každé fáze. Uživatelé díky tomu mohou přímo a intuitivně najít potenciální problémy a distribuci vlastností úlohy. V rozevíracím seznamu můžete zvolit zdroj dat, který chcete zobrazit.  

## <a name="run-u-sql-locally"></a>Místní spuštění U-SQL

Nástroje Azure Data Lake pro Visual Studio a sadu Azure Data Lake U-SQL SDK můžete použít k místnímu spouštění úloh U-SQL na pracovní stanici, stejně jako byste je spouštěli ve službě Azure Data Lake. Tyto dvě místně spouštěné funkce vám šetří čas při testování a ladění úloh U-SQL. 

* [Testování a ladění úloh U-SQL pomocí místního spuštění a sady Azure Data Lake U-SQL SDK](data-lake-analytics-data-lake-tools-local-run.md)


## <a name="see-also"></a>Viz také
Pokud chcete začít s Data Lake Analytics pomocí různých nástrojů, projděte si témata:

* [Začínáme se službou Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-get-started-portal.md)
* [Začínáme se službou Data Lake Analytics pomocí Azure PowerShellu](data-lake-analytics-get-started-powershell.md)
* [Začínáme se službou Data Lake Analytics pomocí sady .NET SDK](data-lake-analytics-get-started-net-sdk.md)
* [Ladění kódu C# v úlohách U-SQL](data-lake-analytics-debug-u-sql-jobs.md)

Pokud se chcete naučit pracovat s nástroji Data Lake pro Visual Studio Code, přečtěte si téma [Použití nástrojů Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).

Další témata týkající se vývoje:

* [Analýza webových protokolů pomocí služby Data Lake Analytics](data-lake-analytics-analyze-weblogs.md)
* [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Začínáme s jazykem U-SQL v Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md)
* [Vývoj uživatelem definovaných operátorů U-SQL pro úlohy Data Lake Analytics](data-lake-analytics-u-sql-develop-user-defined-operators.md)

## <a name="appx-a-powershell-sample-for-preparing-the-tutorial"></a>Ukázka Appx-A PowerShell pro přípravu kurzu
Následující skript prostředí PowerShell vám připraví účet Azure Data Lake Analytics a zdrojová data tak, abyste mohli přejít rovnou k části [Vývoj skriptů U-SQL](data-lake-analytics-data-lake-tools-get-started.md#develop-u-sql-scripts).

    #region - used for creating Azure service names
    $nameToken = "<Enter an alias>"
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion

    #region - service names
    $resourceGroupName = $namePrefix + "rg"
    $dataLakeStoreName = $namePrefix + "adas"
    $dataLakeAnalyticsName = $namePrefix + "adla"
    $location = "East US 2"
    #endregion


    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - Create an Azure Data Lake Analytics service account
    Write-Host "Create a resource group ..." -ForegroundColor Green
    New-AzureRmResourceGroup `
        -Name  $resourceGroupName `
        -Location $location

    Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
    New-AzureRmDataLakeStoreAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeStoreName `
        -Location $location

    Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
    New-AzureRmDataLakeAnalyticsAccount `
        -Name $dataLakeAnalyticsName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultDataLake $dataLakeStoreName

    Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
    Get-AzureRmDataLakeAnalyticsAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeAnalyticsName  
    #endregion

    #region - prepare the source data
    Write-Host "Import the source data ..."  -ForegroundColor Green
    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file.
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.

    # Create the temp location  
    New-Item -Path $localFolder -ItemType Directory -Force

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

    Write-Host "List the source data ..."  -ForegroundColor Green
    Get-AzureRmDataLakeStoreChildItem -Account $dataLakeStoreName -Path  "/Samples/Data/"
    #endregion

