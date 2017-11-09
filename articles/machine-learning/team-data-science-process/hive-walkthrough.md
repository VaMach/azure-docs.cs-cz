---
title: "Prozkoumejte data v clusteru Hadoop a vytváření modelů v Azure Machine Learning | Microsoft Docs"
description: "Pomocí procesu Team dat vědecké účely začátku do konce scénář nasazení clusteru služby HDInsight Hadoop pro sestavení a nasazení modelu použití veřejně dostupné datové sady."
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e9e76c91-d0f6-483d-bae7-2d3157b86aa0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 1be39ab258235740c7e0875a5c0c29ee4a665a71
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/09/2017
---
# <a name="the-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a>Proces Team dat. vědecké účely v akci: použití Azure HDInsight Hadoop clusterů
V tomto návodu použijeme [tým datové vědy procesu (TDSP)](overview.md) začátku do konce scénář pomocí [clusteru Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) k uložení, prozkoumejte a funkci pracovníka data z veřejně k dispozici [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datové sady a dolů ukázková data. Modely dat jsou integrované s Azure Machine Learning pro zpracování více třídami a binární klasifikace a regrese prediktivní úlohy.

Návod, který ukazuje způsob zpracování větší datovou sadu (1 terabajt) pro podobné scénáře pomocí clusterů HDInsight Hadoop pro zpracování dat najdete v tématu [Team Data vědecké účely proces – pomocí Azure HDInsight Hadoop clusterů v datové sadě 1 TB](hive-criteo-walkthrough.md) .

Je také možné použít IPython poznámkového bloku k provádění úloh uvedené návod použití datové sady 1 TB. Uživatelé, kteří by chtěli opakujte tento postup by se měli obrátit [Criteo návod pomocí připojení Hive ODBC](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) tématu.

## <a name="dataset"></a>Popis NYC taxíkem služebních cest datové sady
Data NYC taxíkem cesty je přibližně 20GB komprimované hodnot oddělených čárkami (CSV) souborů (nekomprimovaným ~ 48GB), skládající se z více než 173 milionů jednotlivých cest a tarify placené pro každou cestu. Každý záznam cestě zahrnuje číslo licence vyzvednutí a odkládací umístění a čas, anonymizovaná hackerský (ovladač) a číslo Medailon (taxi na jedinečné id). Data obsahuje všechny služebních cest v roku 2013 a je dostupné pro každý měsíc následující dvě datové sady:

1. CSV soubory 'trip_data' obsahují podrobnosti o cestě, například na počtu cestujících, vyzvednutí a dropoff body, doba trvání cesty a délka cesty. Tady je několik ukázkových záznamů:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. CSV soubory 'trip_fare' obsahují podrobnosti o tarif placené pro každou cestu, například typ platby, velikost tarif, příplatek a daně, tipy a mýtné a celkovou velikost placené. Tady je několik ukázkových záznamů:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Jedinečný klíč pro připojení k cestě\_dat a cesty\_tarif se skládá z pole: medailonu, hackerský\_licence a vyzvednutí\_data a času.

Chcete-li získat všechny podrobnosti relevantní pro konkrétní služební cestě, je dostatečná spojení s tři klíče: "medailonu", "zabezpečení\_licenční" a "vyzvednutí\_data a času".

Jsme popisují některé další podrobnosti dat, když jsme jejich uložení do tabulek Hive za chvíli.

## <a name="mltasks"></a>Příklady předpovědi úlohy
Když blíží data, určení druh předpovědi, které chcete, aby na základě jeho analýzy pomáhá vysvětlení úloh, které je potřeba zahrnout do procesu.
Tady jsou tři příklady předpovědi problémy, které jsme adresy v tomto návodu, jejichž formulování je založena na *tip\_velikost*:

1. **Binární klasifikace**: předpovědět, zda byl tip placené cesty, tj. *tip\_velikost* větší než $0 je pozitivní příklad, při *tip\_velikost* $ 0 je záporný příklad.
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0
2. **Více třídami klasifikace**: K předpovědi rozsahu tip objemu placené pro cestu. Jsme rozdělit *tip\_velikost* do pěti přihrádek nebo třídy:
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0 and tip_amount <= $5
        Class 2: tip_amount > $5 and tip_amount <= $10
        Class 3: tip_amount > $10 and tip_amount <= $20
        Class 4: tip_amount > $20
3. **Úloha regrese**: K předvídání množství tip placené cesty.  

## <a name="setup"></a>Nastavení clusteru HDInsight Hadoop pro pokročilou analýzu
> [!NOTE]
> Obvykle se jedná **správce** úloh.
> 
> 

Můžete nastavit prostředí Azure pro pokročilou analýzu využívajícího clusteru služby HDInsight v tři kroky:

1. [Vytvoření účtu úložiště](../../storage/common/storage-create-storage-account.md): Tento účet úložiště se používá pro ukládání dat v Azure Blob Storage. Data použitá v clusterech HDInsight se také nachází zde.
2. [Přizpůsobení clusterů systému Azure HDInsight Hadoop pro pokročilé analýzy proces a technologie](customize-hadoop-cluster.md). Tento krok vytvoří cluster s 64-bit Anaconda Python 2.7 nainstalované na všech uzlech služby Azure HDInsight Hadoop. Existují dva důležité kroky pamatovat při přizpůsobení clusteru HDInsight.
   
   * Mějte na paměti propojení účtu úložiště vytvořeném v kroku 1 k vašemu clusteru HDInsight při jejím vytváření. Tento účet úložiště se používá pro přístup k datům, která je zpracovat v rámci clusteru.
   * Po vytvoření clusteru, povolte vzdálený přístup k hlavnímu uzlu clusteru. Přejděte na **konfigurace** a klikněte na **povolit vzdálené**. Tento krok určuje uživatelská pověření použitá pro vzdálené přihlášení.
3. [Vytvořit pracovní prostor služby Azure Machine Learning](../studio/create-workspace.md): Tento Azure Machine Learning prostoru slouží k vytvoření modely machine learning. Tato úloha je určeno po dokončení počáteční data zkoumání a dolů vzorkování pomocí clusteru HDInsight.

## <a name="getdata"></a>Získat data z veřejné zdroje
> [!NOTE]
> Obvykle se jedná **správce** úloh.
> 
> 

Chcete-li získat [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datové sady z veřejného umístění, můžete použít některou z metod popsaných v [přesun dat do a z Azure Blob Storage](move-azure-blob.md) kopírovat data do vašeho počítače.

Zde jsme popisují, jak pomocí nástroje AzCopy pro přenos souborů, který obsahuje data. Stáhněte a nainstalujte AzCopy postupujte podle pokynů v [Začínáme pomocí příkazového řádku Azcopy](../../storage/common/storage-use-azcopy.md).

1. Z okna příkazového řádku, vydávání následující příkazy AzCopy nahrazení *< path_to_data_folder >* s požadované cílové umístění:

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. Po dokončení kopírování celkem 24 komprimované soubory jsou ve složce data vybrali. Rozbalte stažené soubory do stejného adresáře na místním počítači. Poznamenejte si složky, kde jsou umístěny nekomprimovaných souborů. Tato složka se označuje jako *< cesta\_k\_unzipped_data\_soubory\>*  je následující.

## <a name="upload"></a>Nahrát data do výchozího kontejneru clusteru Azure HDInsight Hadoop
> [!NOTE]
> Obvykle se jedná **správce** úloh.
> 
> 

V následujících příkazech AzCopy, nahraďte následující parametry skutečné hodnoty, které jste zadali při vytváření clusteru Hadoop a rozbalení datové soubory.

* ***& č. 60; path_to_data_folder >*** adresáři (spolu s cesta) na počítači, které obsahují rozbalené datové soubory  
* ***& č. 60; název účtu úložiště clusteru Hadoop >*** účtu úložiště přidruženého k vašemu clusteru HDInsight
* ***& č. 60; výchozí kontejner clusteru Hadoop >*** výchozí kontejner používá váš cluster. Všimněte si, zda je název kontejneru výchozí obvykle stejný název jako samotného clusteru. Například pokud clusteru se nazývá "abc123.azurehdinsight.net", je výchozí kontejner abc123.
* ***& č. 60; klíč účtu úložiště >*** klíč pro účet úložiště používá váš cluster

Z příkazového řádku nebo okno prostředí Windows PowerShell v počítači spusťte následující dva příkazy AzCopy.

Tento příkaz odesílá data na cestě ***nyctaxitripraw*** adresář ve výchozím kontejneru clusteru Hadoop.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Tento příkaz odešle data tarif ***nyctaxifareraw*** adresář ve výchozím kontejneru clusteru Hadoop.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

Data by teď ve službě Azure Blob Storage a připravené k využívat v rámci clusteru HDInsight.

## <a name="#download-hql-files"></a>Přihlaste se k hlavnímu uzlu clusteru Hadoop a a připravit pro analýzu nahodilého dat
> [!NOTE]
> Obvykle se jedná **správce** úloh.
> 
> 

Pro přístup k hlavnímu uzlu clusteru pro analýzu nahodilého dat a dolů vzorkování dat, postupujte podle pokynů uvedených v [přístup hlavního uzlu Hadoop clusteru](customize-hadoop-cluster.md).

V tomto návodu budeme používat hlavně dotazy, které jsou napsané v [Hive](https://hive.apache.org/), jako SQL dotazovací jazyk, k provedení explorations předběžné data. Dotazy Hive jsou uložené v souborech .hql. Jsme pak dolů ukázkové tato data, která má být použit v rámci Azure Machine Learning pro vytváření modelů.

Příprava clusteru pro analýzu nahodilého dat, můžeme stáhnout soubory .hql obsahuje relevantní skripty Hive z [githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) do místního adresáře (C:\temp) z hlavního uzlu. Chcete-li to provést, otevřete **příkazového řádku** z v rámci hlavního uzlu clusteru a vydejte následující dva příkazy:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Tyto dva příkazy stáhne všechny soubory .hql potřebné v tomto návodu k místnímu adresáři ***C:\temp &#92;*** v hlavního uzlu.

## <a name="#hive-db-tables"></a>Vytvoření databáze Hive a tabulky rozdělena na oddíly po měsících
> [!NOTE]
> Obvykle se jedná **správce** úloh.
> 
> 

Nyní jsme připraveni k vytváření tabulek Hive pro naší datové sadě taxíkem NYC.
V hlavního uzlu clusteru Hadoop, otevřete ***Hadoop příkazového řádku*** na ploše hlavního uzlu a zadejte adresář Hive zadáním příkazu

    cd %hive_home%\bin

> [!NOTE]
> **Spusťte všechny příkazy Hive v tomto návodu z výše uvedených Hive bin / directory řádku. To se postará o všech problémech, cesta automaticky. Používáme podmínky "Hive directory výzva", "Hive bin / directory řádku" a "Hadoop příkazový řádek" zcela zaměnitelným významem v tomto návodu.**
> 
> 

Z adresáře řádku Hive zadejte následující příkaz v Hadoop příkazový řádek z hlavního uzlu odeslat dotaz Hive k vytvoření databáze Hive a tabulek:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Tady je obsah ***C:\temp\sample\_hive\_vytvořit\_db\_a\_tables.hql*** souboru, který vytvoří databázi Hive ***nyctaxidb*** a tabulky ***cestě*** a ***tarif***.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Tento skript Hive vytvoří dvě tabulky:

* "cesty" tabulka obsahuje podrobnosti o cestě každý pravé (podrobnosti o ovladači, výstupní čas, vzdálenost cesty a časy)
* "tarif" tabulka obsahuje podrobnosti o tarif (tarif částka tip, mýtné a příplatky).

Pokud potřebujete žádné další pomoc s tyto postupy nebo chcete prozkoumat alternativní těch, najdete v části [dotazů odeslání Hive přímo z Hadoop příkazového řádku ](move-hive-tables.md#submit).

## <a name="#load-data"></a>Načtení dat do tabulek Hive podle oddílů
> [!NOTE]
> Obvykle se jedná **správce** úloh.
> 
> 

Datová sada taxíkem NYC má přirozené dělení po měsících, které jsou používány za účelem povolení rychlejší zpracování a dotazů. Dole uvedené příkazy Powershellu (vystavené Hive directory pomocí **Hadoop příkazového řádku**) načíst data do tabulek Hive "cesty" a "tarif" rozdělena na oddíly po měsících.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

*Ukázka\_hive\_načíst\_data\_podle\_partitions.hql* soubor obsahuje následující **NAČÍST** příkazy.

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Všimněte si, že počet dotazů Hive, kterou tady používáme v procesu zkoumání zahrnují vyhledávání na právě jeden oddíl nebo na pouze několik oddílů. Ale tyto dotazy, které by bylo možné spustit napříč celou daty.

### <a name="#show-db"></a>Zobrazit databází v clusteru HDInsight Hadoop
Chcete-li zobrazit databáze vytvořené v clusteru HDInsight Hadoop v okně příkazového řádku Hadoop, spusťte následující příkaz v Hadoop příkazového řádku:

    hive -e "show databases;"

### <a name="#show-tables"></a>Zobrazení tabulek Hive v databázi nyctaxidb
Pokud chcete zobrazit tabulky v databázi nyctaxidb, spusťte následující příkaz v Hadoop příkazového řádku:

    hive -e "show tables in nyctaxidb;"

Může potvrdit, že v tabulkách jsou rozdělena na oddíly pomocí příkazu níže:

    hive -e "show partitions nyctaxidb.trip;"

Očekávaný výstup je zobrazena níže:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

Podobně je zajištěno, že tarif tabulka je rozdělena na oddíly pomocí příkazu níže:

    hive -e "show partitions nyctaxidb.fare;"

Očekávaný výstup je zobrazena níže:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="#explore-hive"></a>Zkoumání dat a funkce analýzy v Hive
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Zkoumání dat a funkce technici úlohy pro data načtena do tabulek Hive můžete udělat pomocí dotazů Hive. Zde jsou příklady takových úloh, že jsme vás provede procesem v této části:

* Zobrazte prvních 10 záznamů v obou tabulkách.
* Prozkoumejte data distribuce několik polí v různých časových oken.
* Prozkoumejte data quality polí zeměpisné šířky a délky.
* Generovat binární a více třídami klasifikační štítky na základě **tip\_velikost**.
* Vygenerujte funkce computing vzdálenosti přímé cesty.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Zkoumání: Zobrazte prvních 10 záznamy v cestě tabulky
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Pokud chcete zobrazit, jak vypadá data, jsme zkontrolujte 10 záznamy ze všech tabulek. Spusťte odděleně následující dva dotazy z příkazového řádku adresář Hive v konzole nástroje příkazového řádku Hadoop kontrola záznamy.

Pokud chcete získat prvních 10 záznamy v tabulce "cesty" z první měsíc:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

Pokud chcete získat "tarif" top 10 záznamy v tabulce z první měsíc:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Je často užitečné záznamy uložíte do souboru pro pohodlný zobrazení. Malé změny na výše uvedeném dotazu toho dosahuje:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Zkoumání: Zobrazte počet záznamů v každé 12 oddílů
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Týkající se pokynů počet cest se liší v kalendářním roce. Seskupení podle měsíce umožňuje zobrazit, jak vypadá této distribuce služebních cest.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

To nám dává výstup:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

Zde je první sloupec v měsíci a druhá počet cest pro daný měsíc.

Celkový počet záznamů jsme také můžete počítat v naší datové sadě cestě po vydání následujícího příkazu příkazového řádku adresář Hive.

    hive -e "select count(*) from nyctaxidb.trip;"

Dostaneme:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Pomocí příkazů, které jsou podobné těm, které jsou pro sadu dat služební cestě, jsme vydávat dotazy Hive z příkazového řádku Hive adresář pro datovou sadu tarif ověření počet záznamů.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

To nám dává výstup:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Všimněte si, že pro obě sady dat se vrátí přesně stejný počet služebních cest za měsíc. To poskytuje první ověření správně načíst data.

Počítání celkový počet záznamů v sadě dat tarif lze provést pomocí příkazu z příkazového řádku adresář Hive níže:

    hive -e "select count(*) from nyctaxidb.fare;"

Dostaneme:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Celkový počet záznamů v obou tabulkách je také stejné. To poskytuje druhé ověření správně načíst data.

### <a name="exploration-trip-distribution-by-medallion"></a>Zkoumání: Cestě distribuce podle Medailon
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Tento příklad identifikuje Medailon (taxi čísla) s více než 100 služebních cest v daném časovém období. Dotaz výhody před přístupem dělenou tabulku vzhledem k tomu, že je podmíněno proměnnou oddílu **měsíc**. Výsledky dotazu se zapisují do místního souboru queryoutput.tsv v `C:\temp` z hlavního uzlu.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Tady je obsah *ukázka\_hive\_cestě\_počet\_podle\_medallion.hql* souboru kontroly.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Medailon v datové sadě taxíkem NYC identifikuje jedinečný soubor cab. Abychom mohli identifikovat, které soubory CAB jsou "zaneprázdněný" žádostí, které z nich větší než počet cest provedené v konkrétním časovém období. Následující příklad určuje soubory CAB, provedené víc než sto cest v první tři měsíce a uloží do místního souboru C:\temp\queryoutput.tsv výsledky dotazu.

Tady je obsah *ukázka\_hive\_cestě\_počet\_podle\_medallion.hql* souboru kontroly.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Na řádku adresář Hive vydejte následující příkaz:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Zkoumání: Cestě distribuce podle Medailon a hack_license
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Při prohlížení datové sady, často chceme Zkontrolujte počet výskytů co skupin hodnot. Tato část uveďte příklad toho, jak to udělat pro soubory CAB a ovladače.

*Ukázka\_hive\_cestě\_počet\_podle\_Medailon\_license.hql* skupin souborů tarif datové sady na "Medailon" a "hack_license" a Vrátí počet každé kombinaci. Níže jsou jeho obsah.

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Tento dotaz vrací určitý ovladač kombinací seřazené podle sestupné počet cest a souborů cab.

Z adresáře řádku Hive spusťte příkaz:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

Výsledky dotazu jsou zapisovány do místního souboru C:\temp\queryoutput.tsv.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a>Zkoumání: Vyhodnocování kvality dat. kontrolou neplatné délky nebo zeměpisnou šířku záznamů
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Běžné cílem analýzy nahodilého dat je odstraňování plevele se neplatný nebo chybných záznamů. Příklad v této části určuje, zda buď zeměpisné šířky nebo délky pole obsahovat hodnotu daleko mimo oblast NYC. Vzhledem k tomu, že je pravděpodobné, že tyto záznamy mají hodnoty chybné zeměpisné šířky, chceme je vyloučit z všechna data, která se má použít pro modelování.

Tady je obsah *ukázka\_hive\_kvality\_assessment.hql* souboru kontroly.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


Z adresáře řádku Hive spusťte příkaz:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

*-S* argument zahrnuté v tomto příkazu potlačí výtisk obrazovky stav úloh Hive mapy nebo snižte. To je užitečné, protože umožňuje na obrazovce tiskové dotazu výstupu podregistru srozumitelnější.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Zkoumání: Binární třída distribuce cestě tipy
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Pro problém binární klasifikace uvedených v [příklady úloh předpovědi](hive-walkthrough.md#mltasks) části, je užitečné vědět, zda byl zadán tip, nebo ne. Toto rozdělení tipy je binární:

* Zadaný Tip (třída 1, tip\_částka > $0)  
* žádné tip (třída 0, tip\_velikost = 0).

*Ukázka\_hive\_vysypávány\_frequencies.hql* tomu souboru vidíte níže.

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

Z adresáře řádku Hive spusťte příkaz:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Zkoumání: Třída distribuce do více třídami nastavení
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Pro více třídami klasifikace problému uvedených v [příklady úloh předpovědi](hive-walkthrough.md#mltasks) části tato datová sada také slouží k přirozené klasifikace, kde bychom rádi předpovědi množství tipy zadána. Přihrádek jsme můžete použít k definování rozsahy tip v dotazu. Třída distribuce pro různé tip rozsahy získáte používáme *ukázka\_hive\_tip\_rozsah\_frequencies.hql* souboru. Níže jsou jeho obsah.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

Z konzoly Hadoop příkazového řádku spusťte následující příkaz:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a>Zkoumání: Výpočetní přímé vzdálenost mezi dvěma umístěními zeměpisné šířky
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Míru přímé vzdálenost umožní nám zjistit nesoulad mezi ním vzdálenost skutečné cesty. Tato funkce jsme stimulují tak, že odkazuje osobní může být méně pravděpodobné, že tip, pokud jejich rozmyslete si, že ovladač má záměrně provedenou je mnohem delší trasy.

Zobrazíte porovnání mezi vzdálenost skutečné cesty a [Haversine vzdálenost](http://en.wikipedia.org/wiki/Haversine_formula) mezi dvěma body zeměpisné šířky (vzdálenost "dobře kruh"), používáme trigonometrické funkce dostupné v rámci Hive, tedy:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

Ve výše uvedené dotazu R je radius zemském povrchu v miles a platformy je převést na radiánech. Všimněte si, že body zeměpisné šířky jsou "filtrovány" odebrat hodnoty, které jsou daleko od oblasti NYC.

V takovém případě jsme naše výsledky zapisovat do adresář s názvem "queryoutputdir". Sekvence příkazů ukazuje následující obrázek nejprve vytvoří tento výstup adresář a potom spustí příkaz Hive.

Z adresáře řádku Hive spusťte příkaz:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


Výsledky dotazu se zapisují do 9 Azure blob ***queryoutputdir/000000\_0*** k ***queryoutputdir/000008\_0*** v kontejneru výchozí clusteru Hadoop.

Pokud chcete zobrazit velikost jednotlivých objektů BLOB, jsme spusťte následující příkaz z příkazového řádku adresář Hive:

    hdfs dfs -ls wasb:///queryoutputdir

Chcete-li zobrazit obsah daného souboru, vyslovte 000000\_0, používáme na Hadoop `copyToLocal` příkaz proto.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> `copyToLocal`může být velmi pomalé pro velké soubory a nedoporučuje se používat pro použití s nimi.  
> 
> 

Hlavní výhodou, že máte tato data jsou umístěny v objektu blob Azure je, že jsme může zkoumat data v rámci pomocí Azure Machine Learning [importovat Data] [ import-data] modulu.

## <a name="#downsample"></a>Dolů modely ukázkových dat a sestavení v Azure Machine Learning
> [!NOTE]
> To je obvykle **vědecký pracovník Data** úloh.
> 
> 

Po z analytické fáze nahodilého dat jsme nyní připraveni na dolů ukázková data pro vytváření modelů v Azure Machine Learning. V této části ukážeme, jak použít dotaz Hive na dolů ukázková data, která je potom k němu přistupovat z [importovat Data] [ import-data] modulu v Azure Machine Learning.

### <a name="down-sampling-the-data"></a>Dolů vzorkování dat
Existují dva kroky v tomto postupu. Nejdřív jsme připojit **nyctaxidb.trip** a **nyctaxidb.fare** tabulky na tři klíče, které jsou k dispozici ve všech záznamech: "medailonu", "zabezpečení\_licence", a "vyzvednutí\_data a času". Pak se vygeneruje štítek binární klasifikace **vysypávány** a klasifikaci s více třída popisek **tip\_– třída**.

Abyste mohli použít na nabídku vzorků dat přímo z [importovat Data] [ import-data] modulu v Azure Machine Learning, je nutné ukládání výsledků dotazu na výše uvedené na interní tabulku Hive. V co následuje můžeme vytvořit vnitřní tabulku Hive a naplnit její obsah se připojené k a dolů jen Vzorkovaná data.

Dotaz se použije standardní funkce Hive přímo ke generování hodinu, den, týden roku, den v týdnu (1 zastupuje pondělí a 7 zastupuje neděle) od "vyzvednutí\_data a času" pole a přímé vzdálenost mezi vyzvednutí a dropoff umístění. Uživatelé mohou odkazovat na [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) pro úplný seznam takové funkce.

Dotaz pak dolů ukázky data tak, aby výsledky dotazu, můžete začlenit do Azure Machine Learning Studio. Pouze asi 1 % původní datové sady je importovat do nástroje Studio.

Níže jsou obsah *ukázka\_hive\_Příprava\_pro\_aml\_full.hql* soubor, který připraví dat pro model vytváření v Azure Machine Learning.

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of the join into the above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

Spuštění tohoto dotazu z příkazového řádku adresář Hive:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Nyní je k dispozici vnitřní tabulku "nyctaxidb.nyctaxi_downsampled_dataset", který je přístupný pomocí [importovat Data] [ import-data] modulu z Azure Machine Learning. Kromě toho můžeme použít tuto datovou sadu pro vytváření modelů Machine Learning.  

### <a name="use-the-import-data-module-in-azure-machine-learning-to-access-the-down-sampled-data"></a>Použít modul importovat Data v Azure Machine Learning pro přístup k dolů jen Vzorkovaná data
Jako požadavky na vystavování Hive dotazy v [importovat Data] [ import-data] modul Azure Machine Learning, potřebujeme přístup do Azure Machine Learning prostoru a přístup k přihlašovacím údajům clusteru a jeho přidružený účet úložiště.

Některé podrobnosti o [importovat Data] [ import-data] modulu a parametry pro vstup:

**Identifikátor URI serveru HCatalog**: Pokud je název clusteru abc123, to je jednoduše: https://abc123.azurehdinsight.net

**Název uživatelského účtu Hadoop**: uživatelské jméno zvolené pro cluster (**není** uživatelské jméno vzdáleného přístupu)

**Heslo účtu pož Hadoop**: heslo zvolené pro cluster (**není** heslo vzdáleného přístupu)

**Umístění výstupu dat**: to je zvolen jako Azure.

**Název účtu úložiště Azure**: název výchozí účet úložiště, které jsou přidruženy ke clusteru.

**Název kontejneru Azure**: Toto je výchozí název kontejneru pro cluster a je obvykle stejný jako název clusteru. Pro cluster s názvem "abc123" jde jenom abc123.

> [!IMPORTANT]
> **Všechny tabulky chceme dotazování pomocí [importovat Data] [ import-data] modulu v Azure Machine Learning musí být interní tabulku.** Tip pro určení toho, jestli tabulka T v databázi D.db interní tabulku je následující.
> 
> 

Na řádku adresář Hive vydejte příkaz:

    hdfs dfs -ls wasb:///D.db/T

Pokud v tabulce je interní tabulku a je naplněna, musí zde zobrazí její obsah. Zjistit, zda tabulka je interní tabulku je pomocí Průzkumníka úložiště Azure. Použije ji k přejděte do kontejneru výchozí název clusteru a potom filtrovat podle názvu tabulky. Pokud tabulka a její obsah objeví, potvrdíte, že je interní tabulku.

Zde je snímek dotaz Hive a [importovat Data] [ import-data] modul:

![Dotaz Hive pro Import dat modul](./media/hive-walkthrough/1eTYf52.png)

Poznámka: od našich nižší jen Vzorkovaná data nachází ve výchozím kontejneru, výsledný dotaz Hive z Azure Machine Learning je velmi jednoduchý a je jen "vybrat * z nyctaxidb.nyctaxi\_po převzorkování dolů\_data".

Datová sada může nyní použije jako výchozí bod pro vytváření modelů Machine Learning.

### <a name="mlmodel"></a>Vytvářet modely v Azure Machine Learning
Jsme je nyní možné přejít k vytváření modelů a modelu nasazení v [Azure Machine Learning](https://studio.azureml.net). Data jsou připravena k nám pro použití v adresách identifikované výše předpovědi problémy:

**1. Binární klasifikace**: K předvídání, jestli tip byl placené cesty.

**Použít student:** logistic regression Two-class

a. U tohoto problému našeho štítku cíl (nebo třída) je "vysypávány". Naše původní datové sady vzorků nižší má několik sloupců, které jsou nevracení cíl tohoto experimentu klasifikace. Konkrétně: tip\_třídy, tip\_velikostí a celkový počet\_částka zobrazení informace o cílové štítek, který není k dispozici na testování čas. Jsme tyto sloupce odebrat z aspekt pomocí [výběr sloupců v datové sadě] [ select-columns] modulu.

Snímek níže ukazuje naše experimentu předpovědět, zda byl placené tip pro danou cestu.

![Experiment snímku](./media/hive-walkthrough/QGxRz5A.png)

b. Pro tento experiment naše distribuce popisek cíl byly přibližně 1:1.

Snímek níže znázorňuje rozdělení tipu popisky třídy pro problém binární klasifikace.

![Distribuce popisků tip – třída](./media/hive-walkthrough/9mM4jlD.png)

V důsledku toho jsme získat AUC 0.987, jak je znázorněno na obrázku níže.

![Hodnota AUC](./media/hive-walkthrough/8JDT0F8.png)

**2. Více třídami klasifikace**: K předpovědi rozsahu tip objemu placené pro cestu pomocí dříve definovaných tříd.

**Použít student:** více třídami logistic regression

a. Pro tento problém popisek naše cíle (nebo třída) je "tip\_třída" které můžete provést jednu z pěti hodnot (0,1,2,3,4). Jako v případě binární klasifikace máme několik sloupců, které jsou nevracení cíl tohoto experimentu. Konkrétně: vysypávány, tip\_velikost, celkový počet\_částka zobrazení informace o cílové štítek, který není k dispozici na testování čas. Jsme odebrat tyto sloupce pomocí [výběr sloupců v datové sadě] [ select-columns] modulu.

Snímek níže ukazuje naše experimentu k předpovědi, které koš tip je pravděpodobné, že tak, aby spadal (třída 0: tip = $0, 1 – třída: tip > $0 a tip < = 5, 2 – třída: tip > $5 a tip < = 10, třídy 3: tip > $10 a tip < = 20 Třída 4: tip > 20)

![Experiment snímku](./media/hive-walkthrough/5ztv0n0.png)

Nyní ukážeme, jak vypadá naše skutečné testovací třída distribuční. Vidíte, že třída 0 a 1 třídy jsou běžně se vyskytujícím, ostatní třídy vyskytují jen vzácně.

![Testování distribuční – třída](./media/hive-walkthrough/Vy1FUKa.png)

b. Pro tento experiment používáme nedorozuměním matice podívat se na naše přesnostmi předpovědi. Příklad najdete níž.

![Matice nedorozuměním](./media/hive-walkthrough/cxFmErM.png)

Všimněte si, že naše třída přesnostmi na běžně se vyskytujícím třídy jsou sice velmi dobré, modelu neprovádí dobrou práci "vzdělávání" na vzácnějších třídy.

**3. Úloha regrese**: K předvídání množství tip placené cesty.

**Použít student:** Boosted rozhodovací strom

a. Pro tento problém popisek naše cíle (nebo třída) je "tip\_velikost". Naše nevracení cíl v tomto případě jsou: vysypávány, tip\_třída, celkový počet\_velikost; tyto proměnné odhalit informace o velikosti tip pro, který je obvykle dostupná na testování čas. Jsme odebrat tyto sloupce pomocí [výběr sloupců v datové sadě] [ select-columns] modulu.

Belows snímku ukazuje naše experimentu k předvídání množství dané špičky.

![Experiment snímku](./media/hive-walkthrough/11TZWgV.png)

b. V případě problémů regrese měříme přesnostmi naše předpovědí prohlížením kvadratická chyba v jsou předpovědi, koeficient spolehlivosti a podobně. Ukážeme tyto níže.

![Předpověď statistiky](./media/hive-walkthrough/Jat9mrz.png)

Vidíme, že o koeficient spolehlivosti je 0.709, zdání přibližně 71 % odchylku vysvětluje naše koeficienty modelu.

> [!IMPORTANT]
> Můžete najít další informace o Azure Machine Learning a jak přístup a použít jej [co je Machine Learning](../studio/what-is-machine-learning.md). Je velmi užitečná prostředku pro přehrávání s spoustu experimenty Machine Learning v Azure Machine Learning [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/). Galerie vysvětluje škálu experimenty a poskytuje důkladné Úvod do řadu funkcí služby Azure Machine Learning.
> 
> 

## <a name="license-information"></a>Informace o licenci
Tento ukázkový postup a jeho doprovodné skriptů jsou sdíleny společností Microsoft v rámci licencí MIT. Zkontrolujte prosím soubor LICENSE.txt v adresáři uvedeném ukázkovém kódu na Githubu další podrobnosti.

## <a name="references"></a>Odkazy
• [Stránce pro stažení Andrés Monroy NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC Taxi cestě dat podle Whong Jan](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC taxíkem a Limousine Komise výzkum a statistiky](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

[2]: ./media/hive-walkthrough/output-hive-results-3.png
[11]: ./media/hive-walkthrough/hive-reader-properties.png
[12]: ./media/hive-walkthrough/binary-classification-training.png
[13]: ./media/hive-walkthrough/create-scoring-experiment.png
[14]: ./media/hive-walkthrough/binary-classification-scoring.png
[15]: ./media/hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
