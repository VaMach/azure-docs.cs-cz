<properties 
   pageTitle="Začínáme s Azure Data Lake Analytics pomocí rozhraní příkazového řádku Azure | Microsoft Azure" 
   description="Naučte se používat rozhraní příkazového řádku Azure k vytvoření účtu Data Lake Store, vytvoření úlohy Data Lake Analytics pomocí U-SQL a odeslání úlohy. " 
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


# Kurz: Začínáme s Azure Data Lake Analytics pomocí rozhraní příkazového řádku Azure

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Naučte se používat rozhraní příkazového řádku Azure k vytváření účtů Azure Data Lake Analytics, definování úloh Data Lake Analytics v [U-SQL](data-lake-analytics-u-sql-get-started.md) a odesílání úloh do účtů Data Lake Analytics. Další informace o Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).

V tomto kurzu budete vyvíjet úlohu, která načte soubor hodnot oddělených tabulátory (TSV) a převede jej na soubor hodnot oddělených čárkami (CSV). Pokud chcete použít jiné podporované nástroje a absolvovat stejný kurz, klikněte na karty nahoře v této části.

##Požadavky

Je nutné, abyste před zahájením tohoto kurzu měli tyto položky:

- **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
- **Rozhraní příkazového řádku Azure**. Viz téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../xplat-cli-install.md).
    - Pokud chcete absolvovat tuto ukázku, stáhněte a nainstalujte **předběžnou verzi** [nástrojů příkazového řádku Azure](https://github.com/MicrosoftBigData/AzureDataLake/releases).
- **Ověřování**, a to pomocí následujícího příkazu:

        azure login
    Další informace týkající se ověřování pomocí pracovního nebo školního účtu najdete v tématu [Připojení k předplatnému Azure z rozhraní příkazového řádku Azure](../xplat-cli-connect.md).
- **Přepněte do režimu Azure Resource Manager**, a to pomocí následujícího příkazu:

        azure config mode arm
        
## Vytvoření účtu Data Lake Analytics

Je nutné, abyste před spuštěním jakékoli úlohy měli účet Data Lake Analytics. Pokud chcete vytvořit účet Data Lake Analytics, je nutné zadat tyto údaje:

- **Skupina prostředků Azure**: Musí být vytvořen účet Data Lake Analytics v rámci Skupiny prostředků Azure. [Azure Resource Manager](../resource-group-overview.md) umožňuje pracovat s prostředky v aplikaci jako se skupinou. Všechny prostředky pro aplikaci můžete nasadit, aktualizovat nebo odstranit v rámci jediné koordinované operace.  

    Pokud chcete zobrazit výčet skupin prostředků v předplatném:
    
        azure group list 
    
    Pokud chcete vytvořit novou skupinu prostředků:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Název účtu Data Lake Analytics**
- **Umístění**: jedno z datových center Azure podporující Data Lake Analytics.
- **Výchozí účet Data Lake**: každý účet Data Lake Analytics má výchozí účet Data Lake.

    Pokud chcete zobrazit seznam stávajícího účtu Data Lake:
    
        azure datalake store account list

    Pokud chcete vytvořit nový účet Data Lake:

        azure datalake store account create "<Data Lake Store Account Name>" "<Azure Location>" "<Resource Group Name>"

    > [AZURE.NOTE] Název účtu Data Lake musí obsahovat jenom malá písmena a číslice.



**Postup vytvoření účtu Data Lake Analytics**

        azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"

        azure datalake analytics account list
        azure datalake analytics account show "<Data Lake Analytics Account Name>"          

![Zobrazení účtu Data Lake Analytics](./media/data-lake-analytics-get-started-cli/data-lake-analytics-show-account-cli.png)

> [AZURE.NOTE] Název účtu Data Lake Analytics musí obsahovat jenom malá písmena a číslice.


## Nahrání dat do Data Lake Store

V tomto kurzu budete zpracovávat několik protokolů hledání.  Protokol hledání se dá uložit buď do úložiště Data Lake Store, nebo do úložiště objektů Azure Blob. 

Portál Azure poskytuje uživatelské rozhraní pro kopírování některých ukázkových datových souborů (včetně souboru protokolu hledání) do výchozího účtu Data Lake. Pokud chcete nahrát data do výchozího účtu Data Lake Store, informace najdete v části [Příprava zdrojových dat](data-lake-analytics-get-started-portal.md#prepare-source-data).

Pokud chcete nahrát soubory pomocí rozhraní příkazového řádku, použijte tento příkaz:

    azure datalake store filesystem import "<Data Lake Store Account Name>" "<Path>" "<Destination>"
    azure datalake store filesystem list "<Data Lake Store Account Name>" "<Path>"

Data Lake Analytics má také přístup k úložišti objektů Azure Blob.  Pokud chcete nahrát data do úložiště objektů Azure Blob, informace najdete v tématu [Použití rozhraní příkazového řádku Azure s Azure Storage](../storage/storage-azure-cli.md).

## Odesílání úloh Data Lake Analytics

Úlohy Data Lake Analytics se píšou v jazyce U-SQL. Další informace o U-SQL najdete v tématu [Začínáme s jazykem U-SQL](data-lake-analytics-u-sql-get-started.md) a [Referenční informace pro jazyk U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

**Postup vytvoření skriptu úlohy Data Lake Analytics**

- Vytvořte textový soubor s následujícím skriptem U-SQL a uložte ho do pracovní stanice:

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
    
    Pokud zdrojový soubor nekopírujete do jiného umístění, tyto dvě cesty neupravujte.  Data Lake Analytics vytvoří výstupní složku, pokud neexistuje.
    
    U souborů uložených ve výchozích účtech Data Lake je jednodušší použít relativní cesty. Můžete také použít absolutní cesty.  Například 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Pro přístup k souborům v propojených účtech Storage je nutné použít absolutní cesty.  V případě souborů uložených v propojeném účtu Azure Storage je syntaxe následující:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Kontejnery Azure Blob s přístupovými oprávněními veřejných objektů blob nebo veřejných kontejnerů nejsou aktuálně podporované.      

    
**Postup odeslání úlohy**


    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"
    
    
K zobrazení seznamu úloh, získání podrobností o úlohách a zrušení úloh můžete použít tyto příkazy:

    azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job Id>"
    azure datalake analytics job list "<Data Lake Analytics Account Name>"
    azure datalake analytics job show "<Data Lake Analytics Account Name>" "<Job Id>"

Po dokončení úlohy můžete pomocí následujících rutin zobrazit výpis souboru a stáhnout soubor:
    
    azure datalake store filesystem list "<Data Lake Store Account Name>" "/Output"
    azure datalake store filesystem export "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" "<Destination>"
    azure datalake store filesystem read "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" <Length> <Offset>

## Viz také

- Pokud chcete použít jiné podporované nástroje a zobrazit stejný kurz, klikněte na selektory karet v horní části stránky.
- Pokud chcete zobrazit komplexnější dotaz, přejděte k tématu [Analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Pokud chcete začít s vývojem aplikací U-SQL, přejděte k tématu [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Pokud se chcete naučit jazyk U-SQL, informace najdete v tématu [Začínáme s jazykem U-SQL Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
- Informace týkající se úloh správy najdete v tématu [Správa Azure Data Lake Analytics pomocí Portálu Azure](data-lake-analytics-manage-use-portal.md).
- Přehled Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).




<!--HONumber=Sep16_HO3-->


