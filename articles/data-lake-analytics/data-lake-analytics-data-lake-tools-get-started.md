<properties
   pageTitle="Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio | Azure"
   description="Naučte se nainstalovat nástroje Data Lake pro Visual Studio a vyvíjet a testovat skripty U-SQL. "
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# Kurz: Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Naučte se nainstalovat nástroje Data Lake pro Visual Studio a používat tyto nástroje k psaní a testování skriptů U-SQL.

U-SQL je mimořádně škálovatelný a vysoce rozšiřitelný jazyk pro přípravu, transformaci a analýzu všech dat v úložišti Data Lake i mimo něj. Další informace najdete v tématu [Odkaz na U-SQL] (http://go.microsoft.com/fwlink/p/?LinkId=691348).


###Požadavky

- **Visual Studio 2015, Visual Studio 2013 Update 4 nebo Visual Studio 2012. Jsou podporovány edice Enterprise (Ultimate nebo Premium), Professional a Community; není podporována edice Express. Visual Studio „15“ není aktuálně podporováno, ale pracujeme na tom.**
- **Sada Microsoft Azure SDK pro .NET verze 2.7.1 nebo vyšší**.  Nainstalujte ji pomocí [Instalačního programu webové platformy](http://www.microsoft.com/web/downloads/platform.aspx).
- **[Nástroje Data Lake pro Visual Studio](http://aka.ms/adltoolsvs)**.

    Po instalaci nástrojů Data Lake pro Visual Studio se v Průzkumníku serveru pod uzlem „Azure“ zobrazí uzel „Data Lake Analytics“ (Průzkumníka serveru můžete otevřít stisknutím kombinace kláves Ctrl+Alt+S).

- **Projděte si následující dvě části tématu [Začínáme s Azure Data Lake Analytics pomocí Portálu Azure](data-lake-analytics-get-started-portal.md)**.

    - [Vytvoření účtu Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md#create_adl_analytics_account).
    - [Nahrání souboru SearchLog.tsv do výchozího účtu Data Lake Storage](data-lake-analytics-get-started-portal.md#update-data-to-the-default-adl-storage-account).

    Pro usnadnění práce najdete v tématu [Ukázkový Appx-A prostředí PowerShell pro přípravu kurzu](data-lake-analytics-data-lake-tools-get-started.md#appx-a-powershell-sample-for-preparing-the-tutorial) ukázkový skript prostředí PowerShell pro vytvoření analytické služby Data Lake a nahrání zdrojového datového souboru.

    Nástroje Data Lake nepodporují vytváření účtů Data Lake Analytics. Účet je proto nutné vytvořit pomocí Portálu Azure, prostředí Azure PowerShell, sady .NET SDK nebo rozhraní Azure CLI. Pokud chcete spustit úlohu Data Lake Analytics, budete potřebovat data. Ačkoli nástroje Data Lake podporují nahrávání dat, pro větší přehlednost tohoto kurzu nahrajete ukázková data pomocí portálu.

## Připojení k Azure

**Postup připojení k Data Lake Analytics**

1. Otevřete sadu Visual Studio.
2. V nabídce **Zobrazení** klikněte na tlačítko **Průzkumník serveru** a otevřete Průzkumníka serveru. Můžete také stisknout kombinaci kláves **[CTRL]+[ALT]+S**.
3. Klikněte pravým tlačítkem na **Azure**, klikněte na položku „Připojit k předplatnému Microsoft Azure“ a postupujte podle pokynů.
4. V **Průzkumníku serveru** rozbalte položku **Azure** a pak rozbalte položku **Data Lake Analytics**. Zobrazí se seznam účtů Data Lake Analytics, pokud nějaké máte. Účty Data Lake Analytics nelze vytvářet ze sady Visual Studio. Pokud chcete vytvořit účet, informace najdete v tématu [Začínáme s Azure Data Lake Analytics pomocí Portálu Azure](data-lake-analytics-get-started-portal.md) nebo [Začínáme s Azure Data Lake Analytics pomocí Azure PowerShell](data-lake-analytics-get-started-powershell.md).

## Nahrání zdrojových datových souborů

Nějaká data jste již v tomto kurzu nahráli v části **Požadavky**.  

Pokud chcete použít vlastní data, níže jsou uvedeny postupy pro nahrání dat z nástrojů Data Lake.

**Postup nahrání souborů do závislého účtu Azure Data Lake**

1. V **Průzkumníku serveru** rozbalte položku **Azure**, rozbalte položku **Data Lake Analytics**, rozbalte účet Data Lake Analytics a rozbalte položku **Účty Storage**. Zobrazí se výchozí účet Data Lake Storage, propojené účty Data Lake Storage a propojené účty Azure Storage. Výchozí účet Data Lake má popisek „Výchozí účet úložiště“.
2. Klikněte pravým tlačítkem na výchozí účet Data Lake Storage a potom klikněte na položku **Explorer**.  Otevře se podokno Nástroje Data Lake pro Průzkumníka Visual Studio.  Na levé straně je stromové zobrazení a na pravé straně je zobrazení obsahu.
3. Přejděte do složky, kam chcete nahrát soubory,
4. Klikněte pravým tlačítkem na libovolné prázdné místo a pak klikněte na položku **Nahrát**.

    ![Projekt U-SQL Visual Studio U-SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-upload-files.png)

**Postup nahrání souborů do propojeného účtu úložiště Azure Blob**

1. V **Průzkumníku serveru** rozbalte položku **Azure**, rozbalte položku **Data Lake Analytics**, rozbalte účet Data Lake Analytics a rozbalte položku **Účty Storage**. Zobrazí se výchozí účet Data Lake Storage, propojené účty Data Lake Storage a propojené účty Azure Storage.
2. Rozbalte úložiště Azure Storage.
3. Klikněte pravým tlačítkem na kontejner, kam chcete nahrát soubory, a pak klikněte na možnost **Explorer**. Pokud nemáte kontejner, musíte ho nejdřív vytvořit pomocí Portálu Azure, prostředí Azure PowerShell nebo jiných nástrojů.
4. Přejděte do složky, kam chcete nahrát soubory,
5. Klikněte pravým tlačítkem na libovolné prázdné místo a pak klikněte na položku **Nahrát**.

## Vývoj skriptů U-SQL

Úlohy Data Lake Analytics se píšou v jazyce U-SQL. Další informace o U-SQL najdete v tématu [Začínáme s jazykem U-SQL](data-lake-analytics-u-sql-get-started.md) a [Referenční informace pro jazyk U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

**Postup vytvoření a odeslání úlohy Data Lake Analytics**

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

    >[AZURE.NOTE] Kontejnery Azure Blob s přístupovými oprávněními veřejných objektů blob nebo veřejných kontejnerů nejsou aktuálně podporované.  

    Všimněte si těchto funkcí:

    - **IntelliSense**

        Automaticky se vyplní název a zobrazí se členové Sad řádků, Tříd, Databází, Schémat a Objektů definovaných uživatelem (UDO).

        Technologie IntelliSense pro entity katalogu (Databáze, Schémata, Tabulky, UDO atd.) se vztahuje k výpočetnímu účtu. Aktuální aktivní výpočetní účet, databázi a schéma můžete zkontrolovat v horním panelu nástrojů a můžete je přepnout pomocí rozevíracích seznamů.

    - **Rozbalení sloupců ***

        Klikněte vpravo od * a pod * se zobrazí modré podtržení. Umístěte myš na modré podtržení a klikněte na šipku dolů.
        ![Nástroje Data Lake Visual Studio, rozbalení *](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-expand-asterisk.png)

        Klikněte na položku **Rozbalit sloupce** a nástroj nahradí znak * názvy sloupců.

    - **Automatický formát**

        Uživatelé můžou změnit odsazení skriptu U-SQL na základě struktury kódu v nabídce Upravit -> Rozšířené:

        - Formátovat dokument (Ctrl+E, D): Naformátuje celý dokument.   
        - Formátovat výběr (Ctrl+K, Ctrl+F): Naformátuje výběr. Pokud není nic vybráno, tato zkratka naformátuje řádek, ve kterém se nachází kurzor.  

        Veškerá pravidla formátování lze konfigurovat v nabídce Nástroje -> Možnosti -> Textový editor - > SIP -> Formátování.  
    - **Inteligentní odsazení**

        Nástroje Data Lake pro Visual Studio dokážou během psaní skriptů automaticky odsazovat výrazy. Tato funkce je ve výchozím nastavení zakázána a uživatelé ji musí povolit zaškrtnutím možnosti U-SQL -> Možnosti a nastavení -> Přepínače -> Povolit inteligentní odsazení.

    - **Přechod k definici a nalezení všech odkazů**

        Klikněte pravým tlačítkem na název Sady řádků / parametru / sloupce / UDO atd. a poté můžete přejít k příslušné definici kliknutím na položku Přejít k definici (F12). Kliknutím na položku Najít všechny odkazy (Shift+F12) se zobrazí všechny odkazy.

    - **Vložení cesty Azure**

        Díky nástrojům Data Lake si už při psaní skriptu nemusíte pamatovat cestu k souboru Azure a ručně ji zadat, ale můžete využít snadnější způsob: klikněte v editoru pravým tlačítkem myši a klikněte na položku Vložit cestu Azure. Přejděte k požadovanému souboru v dialogovém okně prohlížeče objektů blob Azure. Klikněte na tlačítko **OK**. Cesta k souboru se vloží do kódu.

5. Zadejte účet Data Lake Analytics, Databázi a Schéma. Můžete vybrat možnost **(místní)** a pro účely testování spustit skript místně. Další informace najdete v tématu [Místní spuštění U-SQL](#run-u-sql-locally).

    ![Odeslání projektu U-SQL sady Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

    Další informace najdete v tématu [Použití katalogu U-SQL](data-lake-analytics-use-u-sql-catalog.md).

5. V **Průzkumníku řešení** klikněte pravým tlačítkem na položku **Script.usql** a pak klikněte na možnost **Sestavit skript**. Ověřte výsledek v podokně Výstup.
6. V **Průzkumníku řešení** klikněte pravým tlačítkem na položku **Script.usql** a pak klikněte na položku **Odeslat skript**. Můžete také kliknout na tlačítko **Odeslat** v podokně Script.usql.  Viz předchozí snímek obrazovky.  Pokud chcete při odeslání použít rozšířené možnosti, klikněte na šipku dolů vedle tlačítka Odeslat:
7. Zadejte **Název úlohy**, ověřte **Účet Analytics** a klikněte na tlačítko **Odeslat**. Jakmile je odeslání hotové, v okně Výsledky nástrojů Data Lake pro Visual Studio jsou dostupné výsledky odeslání a odkaz na úlohu.

    ![Odeslání projektu U-SQL sady Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)

8. Pokud chcete zobrazit nejnovější stav úlohy a aktualizovat obrazovku, je nutné kliknout na tlačítko Aktualizovat. Když se úloha úspěšně dokončí, zobrazí se **Graf úlohy**, **Operace s metadaty**, **Historie stavů**, **Diagnostika**:

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
2. Klikněte na úlohu, jejíž stav chcete zobrazit.

**Postup zobrazení výstupu úlohy**

1. V **Průzkumníku serveru** rozbalte položku **Azure**, rozbalte položku **Data Lake Analytics**, rozbalte účet Data Lake Analytics, rozbalte položku **Účty Storage**, klikněte pravým tlačítkem na výchozí účet Data Lake Storage a potom klikněte na položku **Explorer**.
2.  Dvojím kliknutím na možnost **výstup** otevřete složku.
3.  Dvakrát klikněte na položku **SearchLog-From-adltools.csv**.


###Přehrávání úloh

Přehrání úlohy umožňuje sledovat průběh provádění úlohy a vizuálně detekovat kritická místa a anomálie výkonu. Tato funkce se dá použít před dokončením provádění úlohy (tj. po dobu, kdy je úloha aktivně spuštěná) i po dokončení provádění. Přehrávání během provádění úlohy umožňuje přehrát průběh až do aktuálního okamžiku.

**Postup zobrazení průběhu provádění úlohy**  

1. Klikněte v pravém horním rohu na možnost **Profil zatížení**. Viz předchozí snímek obrazovky.
2. Klikněte v levém dolním rohu na tlačítko Přehrát a překontrolujte průběh provádění úlohy.
3. Přehrávání můžete kdykoli pozastavit kliknutím na tlačítko **Pozastavit** a můžete také přímo přetáhnout indikátor průběhu na konkrétní pozici.


###Heat mapa

Nástroje Data Lake pro Visual Studio poskytují v zobrazení úlohy barevné překryvné vrstvy vybrané uživatelem, které znázorňují průběh, vstup/výstup dat, dobu provádění nebo propustnost vstupu/výstupu u každé fáze. Uživatelé díky tomu mohou přímo a intuitivně najít potenciální problémy a distribuci vlastností úlohy. V rozevíracím seznamu můžete zvolit zdroj dat, který chcete zobrazit.  

## Místní spuštění U-SQL

Pomocí místního spuštění U-SQL v sadě Visual Studio můžete provádět tyto úkony:

- Místní spuštění skriptů U-SQL společně se sestaveními C#.
- Místní ladění sestavení C#.
- Vytvoření, odstranění nebo zobrazení místních databází, sestavení, schémat a tabulek v Průzkumníku serveru úplně stejně, jako u služby Azure Data Lake Analytics.

V sadě Visual Studio se zobrazí *Místní* účet a instalační program vytvoří složku *DataRoot* umístěnou v *C:\LocalRunRoot*. Složka DataRoot se použije takto:

- Ukládání metadat, včetně tabulek, databází, TVF atd.
- U konkrétního skriptu: Pokud vstupní/výstupní cesta odkazuje na relativní cestu, vyhledáme složku DataRoot (a cestu ke skriptu, pokud se jedná o vstup).
- Složka DataRoot NEBUDE odkazována, pokud se pokoušíte registrovat sestavení a použít relativní cestu (podrobnější informace najdete v části „Použití sestavení při místním spuštění“).

Toto video ukazuje funkci místního spuštění U-SQL:

>[AZURE.VIDEO usql-localrun]

### Známé problémy a omezení

- Místní spuštění U-SQL nepodporuje místní dotazování na sady souborů. Viz téma [Sady souborů U-SQL](https://msdn.microsoft.com/library/azure/mt621294.aspx). Toto se bude v budoucnu řešit.
- Pomalý výkon z důvodu nízkého paralelismu, protože plány úloh jsou prováděny sériově v jediném procesu.
- Místní spuštění nezobrazuje grafy úloh v sadě Visual Studio. Toto se bude v budoucnu řešit.
- V Průzkumníku serveru nejde vytvořit tabulku, databázi atd. pro místní účet.
- Když se odkazuje na relativní cestu:

    - ve vstupu skriptu (EXTRACT * FROM „/cesta/abc“) – prohledá se cesta DataRoot i cesta ke skriptu.
    - ve výstupu skriptu (OUTPUT TO „cesta/abc“) – cesta DataRoot se použije jako výstupní složka.
    - v registraci sestavení (CREATE ASSEMBLY xyz FROM „/cesta/abc“) – prohledá se cesta ke skriptu, ale ne DataRoot.
    - v registrovaném TVF, zobrazení nebo jiných entitách metadat – prohledá se cesta DataRoot, ale ne cesta ke skriptu.

    U skriptů spuštěných ve službě Data Lake se jako kořenová složka použije výchozí účet úložiště a odpovídajícím způsobem se prohledá.

### Místní testování skriptů U-SQL
Pokyny týkající se vývoje skriptů U-SQL najdete v tématu [Vývoj skriptů U-SQL](#develop-and-test-u-sql-scripts). Pokud chcete místně sestavovat a spouštět skripty U-SQL, v rozevíracím seznamu clusteru vyberte položku **(Místní)** a klikněte na možnost **Odeslat**. Ujistěte se, zda odkazujete na správná data – můžete buď odkazovat na absolutní cestu, nebo umístit data pod složku DataRoot.

![Místní odeslání projektu U-SQL sady Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-local-run.png)

Můžete také kliknout pravým tlačítkem na skript a potom v kontextové nabídce kliknout na možnost **Spustit místní plán** nebo aktivovat místní spuštění stisknutím kláves **CTRL+F5**.

### Použití sestavení při místním spuštění

Přizpůsobené soubory C# se dají spouštět dvěma způsoby:

- Napište sestavení do souboru kódu a sestavení se automaticky zaregistrují a odpojí po dokončení skriptu.
- Vytvořte projekt sestavení C# a zaregistrujte výstupní knihovnu DLL k místnímu účtu pomocí skriptu podobného tomu, který je uvedený níže. Všimněte si, že cesta je relativní vzhledem ke skriptu, a ne ke složce DataRoot.

![Použití sestavení při místním spuštění U-SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-local-run-assembly.png)

### Místní ladění skriptů a sestavení C#

Sestavení C# můžete ladit, aniž byste je odeslali a zaregistrovali k službě Azure Data Lake Analytics. V souboru kódu i v odkazovaném projektu C# můžete nastavit zarážky.

**Postup ladění místního kódu v souboru kódu**
1.  Nastavte zarážky v souboru kódu.
2.  Stiskněte klávesu **F5** a místně laďte skript.

Následující postup funguje pouze v sadě Visual Studio 2015. Ve starší sadě Visual Studio je pravděpodobně nutné ručně přidat soubory PDB.

**Postup ladění místního kódu v odkazovaném projektu C#**
1.  Vytvořte projekt sestavení C# a sestavte jej tak, aby generoval výstupní knihovnu DLL.
2.  Zaregistruje knihovnu DLL pomocí příkazu U-SQL:

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
3.  Nastavte zarážky v kódu C#.
4.  Stiskněte klávesu **F5** a laďte skript, a to s místním odkazem na knihovnu DLL jazyka C#.  

##Viz také

Pokud chcete začít s Data Lake Analytics pomocí různých nástrojů, projděte si témata:

- [Začínáme s Data Lake Analytics pomocí Portálu Azure](data-lake-analytics-get-started-portal.md)
- [Začínáme s Data Lake Analytics pomocí Azure PowerShell](data-lake-analytics-get-started-powershell.md)
- [Začínáme s Data Lake Analytics pomocí sady .NET SDK](data-lake-analytics-get-started-net-sdk.md)

Další témata týkající se vývoje:

- [Analýza weblogů pomocí Data Lake Analytics](data-lake-analytics-analyze-weblogs.md)
- [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Začínáme s jazykem U-SQL Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md)
- [Vývoj uživatelem definovaných operátorů U-SQL pro úlohy Data Lake Analytics](data-lake-analytics-u-sql-develop-user-defined-operators.md)

##Ukázka Appx-A PowerShell pro přípravu kurzu

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



<!---HONumber=Aug16_HO4-->


