V tomto kurzu sestavíte svou první datovou továrnu Azure s datovým kanálem, který zpracovává data pomocí skriptu Hive v clusteru Azure HDInsight (Hadoop). 

Tento článek neposkytuje koncepční přehled služby Azure Data Factory. Koncepční přehled služby najdete v článku [Úvod do Azure Data Factory](../articles/data-factory/data-factory-introduction.md).

## Co je popsáno v tomto kurzu? 
**Azure Data Factory** umožňuje vytvářet úkoly **přesouvání** a **zpracování** dat jako pracovní postupy řízené daty (také nazývané datové kanály). Naučíte se vytvořit svůj první datový kanál s úkolem zpracování dat (nebo transformace dat). Tento úkol používá cluster Azure HDInsight pro transformaci a analýzu webových protokolů.  

V tomto kurzu budete provádět následující kroky:

1.  Vytvoření **datové továrny**. Datová továrna může obsahovat jeden nebo více datových kanálů, které přesouvají a zpracovávají data. 
2.  Vytvoření **propojených služeb**. Vytvoříte propojenou službu, která spojuje úložiště dat nebo výpočetní službu s datovou továrnou. Úložiště dat, jako například Azure Storage, uchovává vstupní a výstupní data aktivit v kanálu. Výpočetní služba, jako například Azure HDInsight, zpracovává nebo transformuje data.    
3.  Vytvoření vstupní a výstupní **datové sady**. Vstupní datová sada představuje vstup pro aktivitu v kanálu a výstupní datová sada představuje výstup pro aktivitu.
3.  Vytvoření **kanálu**. Kanál může mít jednu nebo více aktivit (příklady: aktivita kopírování, aktivita HDInsight Hive). Tento příklad používá aktivitu HDInsight Hive, která spouští skript Hive. Skript nejprve vytvoří externí tabulku odkazující na nezpracovaná data webového protokolu uložená v Azure Blob Storage a pak nezpracovaná data rozdělí podle roku a měsíce.
 
![Zobrazení diagramu v kurzu služby Data Factory](./media/data-factory-tutorial-prerequisites/data-factory-tutorial-diagram-view.png)


V tomto kurzu adfgetstarted (kontejner) => inputdata (složka) obsahuje jeden soubor s názvem input.log. Tento soubor protokolu obsahuje položky za tři měsíce: leden, únor a březen 2014. Zde jsou řádky vzorku pro každý měsíc ve vstupním souboru. 

    2014-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
    2014-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
    2014-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871

Při zpracování souboru pomocí kanálu s aktivitou HDInsight Hive aktivita spustí v klastru HDInsight skript Hive, který rozdělí vstupní data podle roku a měsíce. Skript vytvoří tři výstupní složky, které obsahují soubor s položkami za jednotlivé měsíce.  

    adfgetstarted/partitioneddata/year=2014/month=1/000000_0
    adfgetstarted/partitioneddata/year=2014/month=2/000000_0
    adfgetstarted/partitioneddata/year=2014/month=3/000000_0

Z řádků vzorku uvedených výše je první (s datem 2014-01-01) zapsán do souboru 000000_0 ve složce month=1. Podobně je druhý řádek zapsán do souboru ve složce month=2 a třetí řádek je zapsán do souboru ve složce month=3.  


## Požadavky
Je nutné, abyste před zahájením tohoto kurzu splňovali následující požadavky:

1.  **Předplatné Azure** – Pokud nemáte předplatné Azure, můžete si během několika minut vytvořit bezplatný zkušební účet. Postup při vytváření bezplatného zkušebního účtu najdete v článku [Bezplatná zkušební verze](https://azure.microsoft.com/pricing/free-trial/).

2.  **Úložiště Azure** – Pro ukládání dat v rámci tohoto kurzu budete používat účet úložiště Azure. Pokud nemáte účet úložiště Azure, přečtěte si článek [Vytvoření účtu úložiště](../articles/storage/storage-create-storage-account.md#create-a-storage-account). Po vytvoření účtu úložiště je třeba získat klíč účtu, který se používá pro přístup k účtu. Přečtěte si článek [Zobrazení, kopírování a opětovné vygenerování přístupových klíčů k úložišti](../articles/storage/storage-create-storage-account.md#view-and-copy-storage-access-keys).

## Odeslání souborů pro kurz do Azure Storage
Před zahájením kurzu je třeba připravit úložiště Azure se soubory potřebnými pro kurz.

V této části provedete následující operace:

2. Odeslání souboru dotazu Hive (HQL) do složky **script** kontejneru **adfgetstarted**.
3. Odeslání vstupního souboru do složky **inputdata** kontejneru **adfgetstarted**. 

### Vytvoření souboru skriptu HQL 

1. Spusťte **Poznámkový blok** a vložte následující skript HQL. Tento skript Hive vytvoří dvě tabulky: **WebLogsRaw** a **WebLogsPartitioned**. V nabídce klikněte na **Soubor** a vyberte **Uložit jako**. Přejděte do složky **C:\adfgetstarted** na pevném disku. V poli **Všechny soubory (*.*)**vyberte**Uložit jako typ**  . Do pole **partitionweblogs.hql** zadejte **Název souboru**. Ověřte, že v poli **Kódování** v dolní části dialogového okna je nastaveno **ANSI**. Pokud ne, nastavte **ANSI**.  

        set hive.exec.dynamic.partition.mode=nonstrict;
        
        DROP TABLE IF EXISTS WebLogsRaw; 
        CREATE TABLE WebLogsRaw (
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        LINES TERMINATED BY '\n' 
        tblproperties ("skip.header.line.count"="2");
        
        LOAD DATA INPATH '${hiveconf:inputtable}' OVERWRITE INTO TABLE WebLogsRaw;
        
        DROP TABLE IF EXISTS WebLogsPartitioned ; 
        create external table WebLogsPartitioned (  
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        partitioned by ( year int, month int)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
        STORED AS TEXTFILE 
        LOCATION '${hiveconf:partitionedtable}';
        
        INSERT INTO TABLE WebLogsPartitioned  PARTITION( year , month) 
        SELECT
          date,
          time,
          ssitename,
          csmethod,
          csuristem,
          csuriquery,
          sport,
          susername,
          cipcsUserAgent,
          csCookie,
          csReferer,
          cshost,
          scstatus,
          scsubstatus,
          scwin32status,
          scbytes,
          csbytes,
          timetaken,
          year(date),
          month(date)
        FROM WebLogsRaw

Za běhu předává aktivita Hive v kanálu Data Factory hodnoty pro parametry inputtable a partitionedtable, jak je znázorněno v následujícím fragmentu kódu. Položka storageaccountname odpovídá názvu vašeho účtu úložiště Azure. 

        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
 
### Vytvoření ukázkového vstupního souboru
Prostřednictvím Poznámkového bloku vytvořte ve složce **c:\adfgetstarted** soubor s názvem **input.log** a s následujícím obsahem: 

    #Software: Microsoft Internet Information Services 8.0
    #Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step5.png X-ARR-LOG-ID=9b0c14b1-434d-495a-9b0d-46775194257b 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 37931 871 32
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step6.png X-ARR-LOG-ID=f99cff81-ec38-4a13-b2fe-21b10adca996 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 27146 871 47
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step1.png X-ARR-LOG-ID=af94d3b4-8e05-4871-82c4-638f866d4e83 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 66259 871 140
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47

### Odeslání vstupního souboru a souboru HQL do Azure Blob Storage

Tato část obsahuje pokyny k používání nástroje **AzCopy** ke kopírování souborů do Azure Blob Storage. K provedení tohoto úkolu můžete použít libovolný nástroj dle vlastního výběru (příklad: [Microsoft Azure Storage Explorer](http://storageexplorer.com/), [CloudXPlorer od společnosti ClumsyLeaf Software](http://clumsyleaf.com/products/cloudxplorer)).   
     
2. Příprava úložiště Azure pro kurz:
    1. Stáhněte si [nejnovější verzi nástroje **AzCopy**](http://aka.ms/downloadazcopy) nebo [nejnovější verzi Preview](http://aka.ms/downloadazcopypr). Pokyny k používání nástroje najdete v článku [Použití nástroje AzCopy](../articles/storage/storage-use-azcopy.md).
    2. Přidejte umístění nástroje AzCopy do systémové cesty spuštěním následujícího příkazu na příkazovém řádku. 
    
            set path=%path%;C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy

    3. Přejděte do složky c:\adfgetstarted a spusťte následující příkaz. Tento příkaz odešle soubor **input.log** do účtu úložiště (kontejner **adfgetstarted** a složka **inputdata**). Položku **Název účtu úložiště** nahraďte názvem účtu úložiště a položku **Klíč úložiště** nahraďte přístupovým klíčem pro účet.

            AzCopy /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/inputdata /DestKey:<storagekey>  /Pattern:input.log

        > [AZURE.NOTE] Tento příkaz vytvoří kontejner s názvem **adfgetstarted** v Azure Blob Storage a zkopíruje soubor **input.log** z místní jednotky do složky **inputdata** v kontejneru. 
    
    5. Po úspěšném odeslání souboru se zobrazí výstup nástroje AzCopy podobný následujícímu.
    
            Finished 1 of total 1 file(s).
            [2015/12/16 23:07:33] Transfer summary:
            -----------------
            Total files transferred: 1
            Transfer successfully:   1
            Transfer skipped:        0
            Transfer failed:         0
            Elapsed time:            00.00:00:01
    1. Spuštěním následujícího příkazu odešlete soubor **partitionweblogs.hql** do složky y**script** kontejneru **adfgetstarted**. Zde je příslušný příkaz: 
    
            AzCopy /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/script /DestKey:<storagekey>  /Pattern:partitionweblogs.hql




<!--HONumber=sep16_HO2-->


