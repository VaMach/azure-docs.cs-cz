<properties
    pageTitle="Sestavení prvního objektu pro vytváření dat (REST) | Microsoft Azure"
    description="V tomto kurzu vytvoříte pomocí rozhraní REST API služby Data Factory ukázkový kanál služby Azure Data Factory."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"
/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/16/2016"
    ms.author="spelluru"/>

# Sestavení prvního objektu pro vytváření dat Azure pomocí rozhraní REST API služby Data Factory
> [AZURE.SELECTOR]
- [Přehled kurzu](data-factory-build-your-first-pipeline.md)
- [Pomocí editoru služby Data Factory](data-factory-build-your-first-pipeline-using-editor.md)
- [Pomocí prostředí PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Pomocí sady Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [Pomocí šablony Resource Manageru](data-factory-build-your-first-pipeline-using-arm.md)
- [Pomocí rozhraní REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

V tomto článku se dozvíte, jak vytvořit první objekt pro vytváření dat Azure pomocí rozhraní REST API služby Data Factory.

## Požadavky

- Pročtěte si článek [Přehled kurzu](data-factory-build-your-first-pipeline.md). Tento článek vám pomůže pochopit základní koncepty ve službě Azure Data Factory. 
- Nainstalujte na svůj počítač nástroj [Curl](https://curl.haxx.se/dlwiz/). Pomocí nástroje CURL a příkazů REST vytvoříte objekt pro vytváření dat. 
- Postupujte podle pokynů v [tomto článku](../resource-group-create-service-principal-portal.md) a proveďte následující kroky: 
    1. V Azure Active Directory vytvořte webovou aplikaci s názvem **ADFGetStartedApp**.
    2. Získejte **ID klienta** a **tajný klíč**. 
    3. Získejte **ID tenanta**. 
    4. Přiřaďte aplikaci **ADFGetStartedApp** k roli **Přispěvatel Data Factory**.  
- Nainstalujte [Azure PowerShell](../powershell-install-configure.md).  
- Spusťte **PowerShell** a potom spusťte následující příkaz. Nechte prostředí Azure PowerShell otevřené až do konce tohoto kurzu. Pokud ho zavřete a znovu otevřete, bude potřeba tyto příkazy spustit znovu.
    1. Spusťte příkaz **Login-AzureRmAccount** a zadejte uživatelské jméno a heslo, které používáte k přihlášení na webu Azure Portal.  
    2. Spuštěním příkazu **Get-AzureRmSubscription** zobrazte všechna předplatná pro tento účet.
    3. Spuštěním rutiny **Get-AzureRmSubscription -SubscriptionName NazevPredplatnehoAzure | Set-AzureRmContext** vyberte předplatné, se kterým chcete pracovat. Místo **NazevPredplatnehoAzure** uveďte název svého předplatného Azure. 
3. Spuštěním následujícího příkazu v PowerShellu vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup**.  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Některé kroky v tomto kurzu vychází z předpokladu, že používáte skupinu prostředků s názvem ADFTutorialResourceGroup. Pokud používáte jinou skupinu prostředků, použijte v postupech v tomto kurzu místo skupiny ADFTutorialResourceGroup název vaší skupiny prostředků.

## Vytvoření definic JSON
Vytvořte následující soubory JSON ve složce, ve které je umístěn soubor curl.exe. 

### datafactory.json 

    {  
        "name": "FirstDataFactoryREST",  
        "location": "WestUS"
    }  

### azurestoragelinkedservice.json
> [AZURE.IMPORTANT] Položky **accountname** a **accountkey** nahraďte názvem svého účtu Azure Storage a jeho klíčem. Informace o tom, jak získat přístupový klíč k úložišti, najdete v článku o [zobrazení, kopírování a opětovném vytváření přístupových klíčů úložiště](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }


### hdinsightondemandlinkedservice.json

    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.2",
                "clusterSize": 1,
                "timeToLive": "00:30:00",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }

Následující tabulka obsahuje popis vlastností použitých v tomto fragmentu kódu JSON:

| Vlastnost | Popis |
| :------- | :---------- |
| Version | Určuje, že vytvářený cluster HDInsight má mít verzi 3.2. | 
| ClusterSize | Velikost clusteru HDInsight. | 
| TimeToLive | Určuje dobu nečinnosti před odstraněním clusteru HDInsight. |
| linkedServiceName | Určuje účet úložiště, který se používá k ukládání protokolů generovaných clusterem HDInsight |

Je třeba počítat s následujícím: 

- Pomocí výše uvedeného kódu JSON služba Data Factory vytvoří cluster HDInsight **se systémem Windows**. Můžete ale také vytvořit cluster HDInsight **se systémem Linux**. Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). 
- Místo clusteru HDInsight na vyžádání můžete použít také **vlastní cluster HDInsight**. Podrobnosti najdete v tématu [Propojená služba HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).
- Cluster HDInsight vytvoří **výchozí kontejner** ve službě Blob Storage, kterou jste určili v kódu JSON (**linkedServiceName**). Při odstranění clusteru HDInsight neprovede odstranění tohoto kontejneru. Toto chování je záměrné. Díky propojené službě HDInsight na vyžádání se cluster HDInsight vytvoří pokaždé, když je potřeba zpracovat určitý řez, pokud neexistuje aktivní cluster (**timeToLive**), a po dokončení zpracování se zase odstraní.

    Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů. Pokud je nepotřebujete k řešení potíží s úlohami, můžete je odstranit, abyste snížili náklady na úložiště. Názvy těchto kontejnerů používají následující formát: „adf**název_vašeho_objektu_pro_vytváření_dat**-**název_propojené_služby**-razítko_data_a_času“. K odstranění kontejnerů ze služby Azure Blob Storage můžete použít nástroje, jako je třeba [Průzkumník úložišť od Microsoftu](http://storageexplorer.com/).

Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). 

### inputdataset.json

    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


Kód JSON určuje datovou sadu s názvem **AzureBlobInput**, která představuje vstupní data pro aktivitu v kanálu. Kromě toho určuje, že se vstupní data nachází v kontejneru objektů blob s názvem **adfgetstarted** ve složce s názvem **inputdata**.

Následující tabulka obsahuje popis vlastností použitých v tomto fragmentu kódu JSON:

| Vlastnost | Popis |
| :------- | :---------- |
| type | Vlastnost type je nastavená na hodnotu AzureBlob, protože se data nachází ve službě Azure Blob Storage. |  
| linkedServiceName | Odkazuje na službu StorageLinkedService, kterou jste vytvořili předtím. |
| fileName | Tato vlastnost je nepovinná. Pokud ji vynecháte, vyberou se všechny soubory v cestě folderPath. V tomto případě se zpracovává jenom soubor input.log. |
| type | Soubory protokolů jsou v textovém formátu, takže použijeme hodnotu TextFormat. | 
| columnDelimiter | Sloupce v souborech protokolu jsou oddělené znakem čárky (,) |
| frequency/interval | Frekvence je nastavená na hodnotu Month (Měsíc) a interval je 1, takže vstupní řezy jsou dostupné jednou za měsíc. | 
| external | Pokud vstupní data nevygenerovala služba Data Factory, je tato vlastnost nastavená na hodnotu true. | 

### outputdataset.json

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adfgetstarted/partitioneddata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            }
        }
    }

Kód JSON definuje datovou sadu s názvem **AzureBlobOutput**, která představuje výstupní data pro aktivitu v kanálu. Kromě toho určuje, že se mají výsledky ukládat do kontejneru objektů blob s názvem **adfgetstarted** do složky s názvem **partitioneddata**. Oddíl **availability** určuje, že se výstupní sada vytváří jednou měsíčně.

### pipeline.json
> [AZURE.IMPORTANT] Nahraďte **storageaccountname** názvem vašeho účtu služby Azure Storage. 


    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [{
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [{
                    "name": "AzureBlobInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }],
            "start": "2016-07-10T00:00:00Z",
            "end": "2016-07-11T00:00:00Z",
            "isPaused": false
        }
    }

V tomto fragmentu kódu JSON vytváříte kanál sestávající z jediné aktivity, která zpracovává data v clusteru HDInsight pomocí skriptu Hive.

Soubor skriptu Hive **partitionweblogs.hql** je uložený v účtu služby Azure Storage (který určuje služba scriptLinkedService s názvem **StorageLinkedService**) a ve složce **script** v kontejneru **adfgetstarted**.

Oddíl **defines** určuje nastavení běhového prostředí, které se předá skriptu Hive jako konfigurační hodnoty Hive (např. ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).

Vlastnosti kanálu **start** a **end** určují období aktivity kanálu.

V kódu JSON aktivity určujete, že má skript Hive běžet ve výpočetní službě určené vlastností **linkedServiceName**, tedy **HDInsightOnDemandLinkedService**.

> [AZURE.NOTE] Podrobnosti o vlastnostech JSON použitých ve výše uvedeném příkladu najdete v článku [Anatomie kanálu](data-factory-create-pipelines.md#anatomy-of-a-pipeline). 

## Nastavení globálních proměnných

V prostředí Azure PowerShell spusťte následující příkazy (po nahrazení ukázkových hodnot vašimi vlastními):

> [AZURE.IMPORTANT] Postup získání ID klienta, tajného klíče klienta, ID tenanta a ID předplatného naleznete v sekci [Požadavky](#prerequisites).   

    $client_id = "<client ID of application in AAD>"
    $client_secret = "<client key of application in AAD>"
    $tenant = "<Azure tenant ID>";
    $subscription_id="<Azure subscription ID>";

    $rg = "ADFTutorialResourceGroup"
    $adf = "FirstDataFactoryREST"



## Ověření pomocí ADD

    $cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
    $responseToken = Invoke-Command -scriptblock $cmd;
    $accessToken = (ConvertFrom-Json $responseToken).access_token;
    
    (ConvertFrom-Json $responseToken) 



## Vytvoření objektu pro vytváření dat

V tomto kroku vytvoříte službu Azure Data Factory s názvem **FirstDataFactoryREST**. Objekt pro vytváření dat může mít jeden nebo víc kanálů. Kanál může obsahovat jednu nebo víc aktivit. Může obsahovat třeba aktivitu kopírování, která slouží ke kopírování dat ze zdrojového do cílového úložiště dat, a aktivitu HDInsight Hive pro spuštění skriptu Hive, který umožňuje transformovat vstupní data na výstupní data produktu. Spuštěním následujícího příkazu vytvořte objekt pro vytváření dat: 

1. Přiřaďte příkaz k proměnné s názvem **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
2. Příkaz spusťte pomocí **Invoke-Command**.

        $results = Invoke-Command -scriptblock $cmd;
3. Zkontrolujte výsledky. Pokud byl objekt pro vytváření dat vytvořen úspěšně, v části **výsledky** se zobrazí JSON pro příslušný objekt pro vytváření dat. V opačném případě se zobrazí chybová zpráva.  

        $results

Je třeba počítat s následujícím:
 
- Název objektu pro vytváření dat Azure musí být globálně jedinečný. Pokud se ve výsledcích zobrazí tato chyba: **Název objektu pro vytváření dat „FirstDataFactoryREST“ není k dispozici**, změňte název (například váš_název_FirstDataFactoryREST) v souboru JSON a ve výše uvedeném příkazu. Při provádění postupů v tomto kurzu používejte tento název místo názvu **FirstDataFactoryREST**. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.
- Instance služby Data Factory můžete vytvářet jenom tehdy, když jste přispěvatelem/správcem předplatného Azure.
- Název objektu pro vytváření dat se může v budoucnu zaregistrovat jako název DNS, takže pak bude veřejně viditelný.
- Pokud se zobrazí chyba „**Pro předplatné není zaregistrované používání oboru názvů Microsoft.DataFactory**“, proveďte některý z těchto kroků a znovu zkuste název publikovat: 

    - V prostředí Azure PowerShell zaregistrujte zprostředkovatele služby Data Factory pomocí následujícího příkazu. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Spuštěním následujícího příkazu si můžete ověřit, jestli je zprostředkovatel služby Data Factory zaregistrovaný. 
    
            Get-AzureRmResourceProvider
    - Přihlaste se na web [Azure Portal ](https://portal.azure.com) pomocí předplatného Azure a přejděte do okna Objekt pro vytváření dat nebo na webu Azure Portal vytvořte objekt pro vytváření dat. Zprostředkovatel se při takovém postupu zaregistruje automaticky.

Před vytvořením kanálu je nejdřív potřeba vytvořit několik entit služby Data Factory. Nejdřív vytvoříte propojené služby, které spojí úložiště dat / výpočetní služby s vaším úložištěm dat, definujete vstupní a výstupní datové sady reprezentující data v propojených úložištích dat a potom vytvoříte kanál s aktivitou, která tyto datové sady používá. 

## Vytvoření propojených služeb 
V tomto kroku propojíte svůj účet služby Azure Storage a cluster Azure HDInsight na vyžádání s objektem pro vytváření dat. Účet služby Azure Storage v této ukázce obsahuje vstupní a výstupní data pro kanál. Propojená služba HDInsight slouží v této ukázce ke spuštění skriptu Hive určeného v aktivitě kanálu. 

### Vytvoření propojené služby Azure Storage
V tomto kroku propojíte se svým objektem pro vytváření dat svůj účet služby Azure Storage. V tomto kurzu použijete tento účet služby Azure Storage taky k uložení vstupních/výstupních dat a souboru skriptu HQL.

1. Přiřaďte příkaz k proměnné s názvem **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
2. Příkaz spusťte pomocí **Invoke-Command**.
 
        $results = Invoke-Command -scriptblock $cmd;
3. Zkontrolujte výsledky. Pokud byla propojená služba vytvořena úspěšně, v části **výsledky** se zobrazí JSON pro tuto propojenou službu. V opačném případě se zobrazí chybová zpráva.
  
        $results

### Vytvoření propojené služby Azure HDInsight
V tomto kroku propojíte se svým objektem pro vytváření dat cluster HDInsight na vyžádání. Cluster HDInsight se automaticky vytvoří za běhu, a až dokončí zpracování, po určité zadané době nečinnosti se odstraní. Místo clusteru HDInsight na vyžádání můžete použít také vlastní cluster HDInsight. Podrobnosti najdete v tématu [Propojené výpočetní služby](data-factory-compute-linked-services.md).  

1. Přiřaďte příkaz k proměnné s názvem **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
2. Příkaz spusťte pomocí **Invoke-Command**.

        $results = Invoke-Command -scriptblock $cmd;
3. Zkontrolujte výsledky. Pokud byla propojená služba vytvořena úspěšně, v části **výsledky** se zobrazí JSON pro tuto propojenou službu. V opačném případě se zobrazí chybová zpráva.  

        $results

## Vytvoření datových sad
V tomto kroku vytvoříte datové sady, které představují vstupní a výstupní data pro zpracování Hive. Tyto datové sady odkazují na službu **StorageLinkedService**, kterou už jste v tomto kurzu vytvořili. Propojená služba odkazuje na účet služby Azure Storage a datové sady určují kontejner, složku a název souboru v úložišti, který obsahuje vstupní a výstupní data.   

### Vytvoření vstupní datové sady
V tomto kroku vytvoříte vstupní datovou sadu, která bude představovat vstupní data ve službě Azure Blob Storage.

1. Přiřaďte příkaz k proměnné s názvem **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
2. Příkaz spusťte pomocí **Invoke-Command**.

        $results = Invoke-Command -scriptblock $cmd;
3. Zkontrolujte výsledky. Pokud byla datová sada vytvořena úspěšně, v části **výsledky** se zobrazí JSON pro tuto datovou sadu. V opačném případě se zobrazí chybová zpráva.
  
        $results
### Vytvoření výstupní datové sady
V tomto kroku vytvoříte výstupní datovou sadu, která bude představovat výstupní data ve službě Azure Blob Storage.

1. Přiřaďte příkaz k proměnné s názvem **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
2. Příkaz spusťte pomocí **Invoke-Command**.

        $results = Invoke-Command -scriptblock $cmd;
3. Zkontrolujte výsledky. Pokud byla datová sada vytvořena úspěšně, v části **výsledky** se zobrazí JSON pro tuto datovou sadu. V opačném případě se zobrazí chybová zpráva.
  
        $results 

## Vytvoření kanálu
V tomto kroku vytvoříte svůj první kanál s aktivitou **HDInsightHive**. Vstupní řez je dostupný jednou měsíčně (frequency: Month, interval: 1), výstupní řez se vytváří také jednou měsíčně a vlastnost scheduler pro aktivitu je také nastavena na jednou měsíčně (viz níže). Nastavení výstupní datové sady a vlastnosti scheduler se musí shodovat. V současnosti určuje plán výstupní datová sada, takže musíte výstupní datovou sadu vytvořit i v případě, že aktivita nevytváří žádný výstup. Pokud aktivita nemá žádný vstup, vstupní datovou sadu vytvářet nemusíte.  

Ujistěte se, že se ve složce **adfgetstarted/inputdata** ve službě Azure Blob Storage nachází soubor **input.log**, a spuštěním následujícího příkazu kanál nasaďte. Časy **start** a **end** jsou nastavené na minulost a vlastnost **isPaused** má hodnotu false, takže se kanál (aktivita v kanálu) spustí hned po nasazení. 

1. Přiřaďte příkaz k proměnné s názvem **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
2. Příkaz spusťte pomocí **Invoke-Command**.

        $results = Invoke-Command -scriptblock $cmd;
3. Zkontrolujte výsledky. Pokud byla datová sada vytvořena úspěšně, v části **výsledky** se zobrazí JSON pro tuto datovou sadu. V opačném případě se zobrazí chybová zpráva.  

        $results
5. Úspěšně jste vytvořili první kanál pomocí prostředí Azure PowerShell, blahopřejeme!

## Monitorování kanálu
V tomto kroku použijete rozhraní REST API služby Data Factory k monitorování řezů vytvářených kanálem.

    $ds ="AzureBlobOutput"

    $cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

    $results2 = Invoke-Command -scriptblock $cmd;

    IF ((ConvertFrom-Json $results2).value -ne $NULL) {
        ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
    } else {
            (convertFrom-Json $results2).RemoteException
    }

Spouštějte Invoke-Command a další příkaz, dokud se u řezu neobjeví stav **Připraveno** nebo **Nezdařilo se**. Pokud je řez ve stavu Připraveno, zkontrolujte, jestli se ve složce **partitioneddata** v kontejneru **adfgetstarted** ve službě Blob Storage nachází výstupní data.  Upozorňujeme, že vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá.

![Výstupní data](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [AZURE.IMPORTANT] Po úspěšném zpracování řezu se vstupní soubor odstraní. Pokud tedy chcete spustit řez znovu nebo si znovu projít tento kurz, načtěte vstupní soubor (input.log) do složky inputdata v kontejneru adfgetstarted.

Azure Portal můžete použít také k monitorování řezů a odstraňování jakýchkoli potíží. Přečtěte si podrobnosti o [monitorování kanálů pomocí webu Azure Portal](data-factory-build-your-first-pipeline-using-editor.md##monitor-pipeline).  

## Souhrn 
V tomto kurzu jste vytvořili objekt pro zpracování dat Azure, který zpracovává data pomocí skriptu Hive v clusteru HDInsight Hadoop. Pomocí editoru služby Data Factory na webu Azure Portal jste provedli tyto kroky:  

1.  Vytvořili jste **objekt pro vytváření dat** Azure.
2.  Vytvořili jste dvě **propojené služby**:
    1.  Propojená služba **Azure Storage** spojuje úložiště objektů blob Azure, které obsahuje vstupní/výstupní soubory, s objektem pro vytváření dat.
    2.  Propojená služba na vyžádání **Azure HDInsight** spojuje cluster na vyžádání HDInsight Hadoop s objektem pro vytváření dat. Služba Azure Data Factory vytvoří cluster HDInsight Hadoop ve chvíli, kdy je potřeba zpracovat vstupní data a vygenerovat výstupní data. 
3.  Vytvořili jste dvě **datové sady**, které popisují vstupní a výstupní data aktivity HDInsight Hive v kanálu. 
4.  Vytvořili jste **kanál** s aktivitou **HDInsight Hive**. 

## Další kroky
V tomto článku jste vytvořili kanál s aktivitou transformace (aktivita HDInsight), která v clusteru Azure HDInsight na vyžádání spouští skript Hive. Pokud chcete zjistit, jak pomocí aktivity kopírování zkopírovat data z Azure Blob do Azure SQL, projděte si článek [Kurz: Kopírování dat z Azure Blob do Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## Viz také
| Téma | Popis |
| :---- | :---- |
| [Rozhraní REST API služby Data Factory – referenční informace](https://msdn.microsoft.com/library/azure/dn906738.aspx) |  Tady najdete rozsáhlou dokumentaci o rutinách služby Data Factory. |
| [Aktivity transformace dat](data-factory-data-transformation-activities.md) | Tento článek obsahuje seznam aktivit transformace dat (třeba transformaci HDInsight Hive, kterou jste použili v tomto kurzu) podporovaných službou Azure Data Factory. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) | Tento článek vysvětluje aspekty plánování a provádění aplikačního modelu služby Azure Data Factory. |
| [Kanály](data-factory-create-pipelines.md) | Tento článek vám pomůže pochopit kanály a aktivity ve službě Azure Data Factory a porozumět tomu, jak se dají ve vaší situaci nebo firmě použít k sestavení kompletních pracovních postupů založených na datech. |
| [Datové sady](data-factory-create-datasets.md) | Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory.
| [Monitorování a správa kanálů pomocí oken webu Azure Portal](data-factory-monitor-manage-pipelines.md) | Tento článek popisuje, jak monitorovat, spravovat a ladit kanály pomocí oken webu Azure Portal. |
| [Monitorování a správa kanálů pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) | Tento článek popisuje, jak monitorovat, spravovat a ladit kanály pomocí aplikace pro monitorování a správu. 




<!---HONumber=Aug16_HO4-->


