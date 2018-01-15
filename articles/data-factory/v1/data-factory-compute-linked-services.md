---
title: "Výpočetní prostředí podporovaných službou Azure Data Factory | Microsoft Docs"
description: "Další informace o výpočetní prostředí, které můžete použít v Azure Data Factory kanály (například Azure HDInsight) k datům transformace nebo proces."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 6877a7e8-1a58-4cfb-bbd3-252ac72e4145
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/01/2017
ms.author: shlo
robots: noindex
ms.openlocfilehash: 7733ea111de896ab0f825c85b89be25ebafdbd85
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>Výpočetní prostředí podporovaných službou Azure Data Factory
> [!NOTE]
> Tento článek se týká verze 1 služby Data Factory, která je obecně dostupná (GA). Pokud používáte verze 2 služby Data Factory, který je ve verzi preview, najdete v části [výpočetní propojené služby v V2](../compute-linked-services.md).

Tento článek vysvětluje různé výpočetní prostředí, které můžete použít k datům procesu nebo transformace. Obsahuje také podrobnosti o různých konfiguracích (na vyžádání oproti přineste si vlastní) podporovaných službou Data Factory při konfiguraci propojených služeb propojení tyto výpočetní prostředí s objektem pro vytváření dat Azure.

Následující tabulka obsahuje seznam výpočetních prostředích nepodporuje objekt pro vytváření dat a aktivity, které můžete spustit na ně. 

| Výpočetní prostředí                      | activities                               |
| ---------------------------------------- | ---------------------------------------- |
| [Cluster HDInsight na vyžádání](#azure-hdinsight-on-demand-linked-service) nebo [vlastní cluster HDInsight](#azure-hdinsight-linked-service) | [DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [streamování Hadoop](data-factory-hadoop-streaming-activity.md) |
| [Azure Batch](#azure-batch-linked-service) | [DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) | [Aktivity Machine Learning: Dávkové spouštění a Aktualizace prostředku](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics-linked-service) | [U-SQL Data Lake Analytics](data-factory-usql-activity.md) |
| [Azure SQL](#azure-sql-linked-service), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-linked-service), [systému SQL Server](#sql-server-linked-service) | [Uložená procedura](data-factory-stored-proc-activity.md) |

## <a name="supported-hdinsight-versions-in-azure-data-factory"></a>Podporované verze HDInsight v Azure Data Factory
Azure HDInsight podporuje více verzích clusterů Hadoop, které lze nasadit kdykoli. Každou verzi volbu vytvoří na konkrétní verzi rozdělení Hortonworks Data Platform (HDP) a sadu součástí, které jsou obsaženy v rámci této distribuce. Microsoft udržuje aktualizuje seznam podporovaných verzích HDInsight poskytovat nejnovější komponenty ekosystém Hadoop a opravy. Podrobné informace najdete v tématu [podporované HDInsight verze](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

> [!IMPORTANT]
> HDInsight se systémem Linux verze 3.3 vyřazenou na 31 července 2017. Data Factory v1 propojené služby HDInsight na vyžádání zákazníkům byly zadány až 15 prosinec 2017, testování a provést upgrade na novější verzi HDInsight. Na 31 července 2018 vyřadí HDInsight se systémem Windows.
>
> 

**Co se stane po datu vyřazení** 

Po 15. prosince 2017:

- Už nebudete moct vytvořit HDInsight se systémem Linux verze 3.3 (nebo starší verze) clusterů pomocí propojená služba HDInsight na vyžádání v Azure Data Factory v1. 
- Pokud [osType nebo vlastnost Version](https://docs.microsoft.com/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) nejsou explicitně zadané v existující definice JSON propojené služby HDInsight na vyžádání v1 Azure Data Factory, výchozí hodnota se změní z **verze = 3.1, osType = Windows** k **verze =[nejnovější verze HDI výchozí](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#hadoop-components-available-with-different-hdinsight-versions), osType = Linux**.

Po 31. července 2018:

- Už nebudete moct vytvořit žádné verzi clustery HDInsight se systémem Windows pomocí propojená služba HDInsight na vyžádání v Azure Data Factory v1. 

 **Doporučené akce** 

- Aktualizace [osType nebo vlastnost Version](https://docs.microsoft.com/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) ovlivněné definic propojená služba HDInsight na vyžádání v1 Azure Data Factory pro HDInsight se systémem Linux novější verze (3.6 HDInsight) a tak se mohou používat nejnovější Hadoop ekosystém součástí a opravy. 
- Před testovacího 15 prosince 2017 Azure Data Factory V1 Hive, Pig, MapReduce a Hadoop streamování aktivity, které odkazují ovlivněné propojené služby, abyste měli jistotu, jsou kompatibilní s novým *osType* nebo  *Verze* výchozí hodnota (verze = 3.6, osType = Linux) nebo explicitní HDInsight verze a osType při upgradu. Další informace o kompatibilitě, přečtěte si [migrace z clusteru HDInsight se systémem Windows do clusteru se systémem Linux](https://docs.microsoft.com/azure/hdinsight/hdinsight-migrate-from-windows-to-linux) a [co jsou komponent systému Hadoop a verze, které jsou k dispozici v prostředí HDInsight?](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#hortonworks-release-notes-associated-with-hdinsight-versions) webové stránky dokumentace. 
- Explicitně nastavte osType Windows před 15. prosince 2017, pokud chcete pokračovat v používání služby Azure Data Factory v1On vyžádání HDInsight propojené k vytvoření clusterů HDInsight se systémem Windows. Ale doporučujeme před 31 července 2018 migrace na clustery HDInsight se systémem Linux. 
- Aktualizujte definice DotNet vlastní aktivity JSON propojené služby Azure Batch místo toho chcete použít, pokud používáte propojená služba HDInsight na vyžádání k provádění Azure Data Factory v1DotNet vlastní aktivity. Další informace na [použít vlastní aktivity v kanálu objekt pro vytváření dat Azure](https://docs.microsoft.com/azure/data-factory/v1/data-factory-use-custom-activities) webová stránka dokumentace. 

>[!Note]
>Pro zákazníky, kteří používají vaše stávající přineste si vlastní clusteru (BYOC) propojená služba HDInsight v Azure Data Factory v1 nebo uživatelů, kteří používají BYOC a HDInsightLinked služba na vyžádání v Azure Data Factory v2 podporovat nejnovější verzi zásad ofAzure HDInsight clustery je již vynucené, proto není vyžadována žádná akce. 
>
> 


## <a name="on-demand-compute-environment"></a>Na vyžádání výpočetním prostředí
V tomto typu konfigurace je plně spravovaná výpočetní prostředí pomocí služby Azure Data Factory. Je vytvořeno automaticky pomocí služby Data Factory předtím, než je úloha odeslána zpracování dat a odebrat při dokončení úlohy. Můžete vytvořit propojená služba na vyžádání výpočetní prostředí, konfiguraci a řídit granulární nastavení pro provádění úlohy, správu clusteru a zavádění akce.

> [!NOTE]
> V konfiguraci na vyžádání v současné době podporuje pouze pro clustery služby Azure HDInsight.
>
> 

## <a name="azure-hdinsight-on-demand-linked-service"></a>Propojená služba Azure HDInsight na vyžádání
Služba Azure Data Factory můžete automaticky vytvoření clusteru HDInsight se systémem Windows nebo Linux na vyžádání pro zpracování dat. Cluster se vytvoří ve stejné oblasti jako účet úložiště (vlastnost linkedServiceName v kódu JSON) přidružen ke clusteru.

Vezměte na vědomí následující **důležité** body o HDInsight na vyžádání propojené služby:

* Nevidíte clusteru HDInsight na vyžádání vytvořit ve vašem předplatném Azure. Služba Azure Data Factory slouží ke správě clusteru HDInsight na vyžádání vaším jménem.
* Protokoly pro úlohy, které se spouštějí v clusteru HDInsight na vyžádání se zkopírují do účtu úložiště přidruženého ke clusteru HDInsight. Tyto protokoly můžete přistupovat z portálu Azure v **podrobnosti o spuštění aktivit** okno. V tématu [monitorování a Správa kanálů](data-factory-monitor-manage-pipelines.md) článku.
* Musíte platit pouze za čas, kdy se HDInsight cluster je nahoru a spuštěné úlohy.

> [!IMPORTANT]
> Obvykle trvá **20 minut** nebo více ke zřízení clusteru Azure HDInsight na vyžádání.
>
> 

### <a name="example"></a>Příklad:
Následující kód JSON určuje základě Linux na vyžádání propojené služby HDInsight. Služba Data Factory automaticky vytvoří **systémem Linux** clusteru HDInsight se při zpracování dat řezu. 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.6",
            "osType": "Linux",
            "clusterSize": 1,
            "timeToLive": "00:05:00",            
            "linkedServiceName": "AzureStorageLinkedService"
        }
    }
}
```

> [!IMPORTANT]
> Cluster HDInsight vytvoří **výchozí kontejner** ve službě Blob Storage, kterou jste určili v kódu JSON (**linkedServiceName**). Při odstranění clusteru HDInsight neprovede odstranění tohoto kontejneru. Toto chování je záměrné. Díky propojené službě HDInsight na vyžádání se cluster HDInsight vytvoří pokaždé, když je potřeba zpracovat určitý řez, pokud neexistuje aktivní cluster (**timeToLive**), a po dokončení zpracování se zase odstraní. 
>
> Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů. Pokud je nepotřebujete k řešení potíží s úlohami, můžete je odstranit, abyste snížili náklady na úložiště. Názvy těchto kontejnerů se řídí vzorem: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`. K odstranění kontejnerů ze služby Azure Blob Storage můžete použít nástroje, jako je třeba [Průzkumník úložišť od Microsoftu](http://storageexplorer.com/).
>
> 

### <a name="properties"></a>Vlastnosti
| Vlastnost                     | Popis                              | Požaduje se |
| ---------------------------- | ---------------------------------------- | -------- |
| type                         | Vlastnost typu musí být nastavená na **HDInsightOnDemand**. | Ano      |
| Parametr ClusterSize                  | Počet uzlů pracovního procesu nebo data v clusteru. Vytvoření clusteru HDInsight s 2 hlavních uzlech spolu s počtem uzlů pracovního procesu, který jste zadali pro tuto vlastnost. Uzly jsou velikosti Standard_D3, který má 4 jádra, 4 pracovní uzly clusteru trvá 24 jader (4\*4 = 16 jader pro uzly pracovního procesu, plus 2\*4 = 8 jader pro head uzly). V tématu [vytvořit systémem Linux Hadoop clusterů v HDInsight](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) podrobnosti o Standard_D3 vrstvy. | Ano      |
| TimeToLive                   | Povolené doby nečinnosti pro cluster HDInsight na vyžádání. Určuje, jak dlouho clusteru HDInsight na vyžádání zůstane aktivní po dokončení činnosti spustit, pokud nejsou žádné aktivní úlohy v clusteru.<br/><br/>Například pokud spuštění aktivity trvá 6 minut a timetolive nastavena na 5 minut, clusteru zůstává aktivní po dobu 5 minut po spuštění 6 minut zpracování aktivity. Pokud se okno 6 minut proveden jiné aktivity při spuštění, je zpracován stejného clusteru.<br/><br/>Vytvoření clusteru HDInsight na vyžádání je náročná operace (může trvat), takže použití tohoto nastavení podle potřeby ke zlepšení výkonu služby data factory pomocí opakovaného použití clusteru HDInsight na vyžádání.<br/><br/>Pokud hodnota timetolive nastavíte na 0, odstranění clusteru ihned po dokončení spuštění aktivity. Vzhledem k tomu, pokud jste nastavili na vysokou hodnotu, může zůstat nečinnosti zbytečně výsledkem vysoké náklady na clusteru. Proto je důležité nastavit odpovídající hodnotu na základě potřeb.<br/><br/>Pokud je hodnota vlastnosti timetolive správně nastavena, více kanálů sdílet instanci clusteru HDInsight na vyžádání. | Ano      |
| verze                      | Verze clusteru HDInsight, naleznete [podporované HDInsight verze](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#supported-hdinsight-versions) pro povolené verzích HDInsight. Pokud není zadaný, použije [nejnovější verze HDI výchozí](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#hadoop-components-available-with-different-hdinsight-versions). | Ne       |
| linkedServiceName            | Propojená služba má být používána clusteru na vyžádání pro ukládání a zpracování dat Azure Storage. HDInsight cluster vytvoří ve stejné oblasti jako účet úložiště Azure.<p>V současné době nelze vytvořit cluster HDInsight na vyžádání, který používá jako úložiště Azure Data Lake Store. Pokud chcete uložit výsledek data z HDInsight zpracování v Azure Data Lake Store, pomocí aktivity kopírování zkopírovat data z Azure Blob Storage do Azure Data Lake Store. </p> | Ano      |
| additionalLinkedServiceNames | Určuje, že další účty úložiště pro HDInsight propojená služba tak, aby služba Data Factory můžete zaregistrovat vaším jménem. Tyto účty úložiště musí být ve stejné oblasti jako cluster HDInsight, který je vytvořen ve stejné oblasti jako účet úložiště určeného linkedServiceName. | Ne       |
| osType                       | Typ operačního systému. Povolené hodnoty jsou: systémy Linux a Windows. Pokud není zadaný, použije se ve výchozím nastavení Linux.  <br/>Důrazně recommand pomocí Linux na bázi clustery HDInsight jako datum vyřazení pro HDInsight v systému Windows je 31 července 2018. | Ne       |
| hcatalogLinkedServiceName    | Název Azure SQL propojené služby, které odkazují na databázi HCatalog. Cluster HDInsight na vyžádání je vytvořená pomocí Azure SQL database jako metaúložiště. | Ne       |

#### <a name="additionallinkedservicenames-json-example"></a>Příklad additionalLinkedServiceNames JSON

```json
"additionalLinkedServiceNames": [
    "otherLinkedServiceName1",
    "otherLinkedServiceName2"
  ]
```

### <a name="advanced-properties"></a>Rozšířené vlastnosti
Můžete také zadat následující vlastnosti pro podrobné konfiguraci clusteru HDInsight na vyžádání.

| Vlastnost               | Popis                              | Požaduje se |
| :--------------------- | :--------------------------------------- | :------- |
| coreConfiguration      | Určuje parametry konfigurace jádra (jako základní site.xml) pro vytvoření clusteru HDInsight. | Ne       |
| hBaseConfiguration     | Určuje parametry konfigurace HBase (hbase-site.xml) pro HDInsight cluster. | Ne       |
| hdfsConfiguration      | Určuje parametry konfigurace HDFS (hdfs-site.xml) pro HDInsight cluster. | Ne       |
| hiveConfiguration      | Určuje parametry konfigurace hive (hive-site.xml) pro HDInsight cluster. | Ne       |
| mapReduceConfiguration | Určuje parametry konfigurace MapReduce (mapred-site.xml) pro HDInsight cluster. | Ne       |
| oozieConfiguration     | Určuje parametry konfigurace Oozie (oozie-site.xml) pro HDInsight cluster. | Ne       |
| stormConfiguration     | Určuje parametry konfigurace Storm (storm-site.xml) pro HDInsight cluster. | Ne       |
| yarnConfiguration      | Určuje parametry konfigurace Yarn (yarn-site.xml) pro HDInsight cluster. | Ne       |

#### <a name="example--on-demand-hdinsight-cluster-configuration-with-advanced-properties"></a>Příklad – konfigurace clusteru HDInsight na vyžádání pomocí rozšířených vlastností

```json
{
  "name": " HDInsightOnDemandLinkedService",
  "properties": {
    "type": "HDInsightOnDemand",
    "typeProperties": {
      "version": "3.6",
      "osType": "Linux",
      "clusterSize": 16,
      "timeToLive": "01:30:00",
      "linkedServiceName": "adfods1",
      "coreConfiguration": {
        "templeton.mapper.memory.mb": "5000"
      },
      "hiveConfiguration": {
        "templeton.mapper.memory.mb": "5000"
      },
      "mapReduceConfiguration": {
        "mapreduce.reduce.java.opts": "-Xmx4000m",
        "mapreduce.map.java.opts": "-Xmx4000m",
        "mapreduce.map.memory.mb": "5000",
        "mapreduce.reduce.memory.mb": "5000",
        "mapreduce.job.reduce.slowstart.completedmaps": "0.8"
      },
      "yarnConfiguration": {
        "yarn.app.mapreduce.am.resource.mb": "5000",
        "mapreduce.map.memory.mb": "5000"
      },
      "additionalLinkedServiceNames": [
        "datafeeds",
        "adobedatafeed"
      ]
    }
  }
}
```

### <a name="node-sizes"></a>Velikost uzlu
Zadávat lze velikosti head, data a uzly zookeeper s následujícími vlastnostmi: 

| Vlastnost          | Popis                              | Požaduje se |
| :---------------- | :--------------------------------------- | :------- |
| headNodeSize      | Určuje velikost hlavního uzlu. Výchozí hodnota je: Standard_D3. Najdete v článku **určení velikosti uzlu** podrobnosti. | Ne       |
| dataNodeSize      | Určuje velikost uzlu data. Výchozí hodnota je: Standard_D3. | Ne       |
| zookeeperNodeSize | Určuje velikost uzlu správce Zoo. Výchozí hodnota je: Standard_D3. | Ne       |

#### <a name="specifying-node-sizes"></a>Určení velikosti uzlu
Najdete v článku [velikosti virtuálních počítačů](../../virtual-machines/linux/sizes.md) článku řetězcové hodnoty je třeba zadat vlastností uvedených v předchozí části. Hodnoty musí odpovídat **rutiny & rozhraní API** popsána v článku. Jak můžete vidět v následujícím článku, má datový uzel velikosti velké (výchozí) 7 GB paměti, která nemusí být dostatečně vhodné pro váš scénář. 

Pokud chcete vytvořit D4 velikost head uzlů a uzlů pracovního procesu, zadejte **Standard_D4** jako hodnota vlastnosti headNodeSize a dataNodeSize. 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Pokud zadáte chybnou hodnotu pro tyto vlastnosti, může se zobrazit následující **Chyba:** vytvoření clusteru se nezdařilo. Výjimka: Operaci vytvoření clusteru nelze dokončit. Operace se nezdařila, kód chyby je 400. Zanechaný stav clusteru: Chyba. Zpráva: 'PreClusterCreationValidationFailure'. Jakmile se zobrazí tato chyba, ujistěte se, že používáte **RUTINY & rozhraní API** názvem z tabulky v [velikosti virtuálních počítačů](../../virtual-machines/linux/sizes.md) článku.  

> [!NOTE]
> Azure Data Factory aktuálně nepodporuje clusterů HDInsight pomocí Azure Data Lake Store jako primární úložiště. Používání Azure Storage jako primární úložiště pro clustery služby HDInsight. 
>
> 


## <a name="bring-your-own-compute-environment"></a>Výpočetní prostředí
V tomto typu konfigurace uživatelé mohou registrovat stávající výpočetní prostředí jako propojené služby ve službě Data Factory. Výpočetní prostředí spravuje uživatele a služba Data Factory používá, je spuštění aktivity.

Tento typ konfigurace je podporována pro následující výpočetních prostředích:

* Azure HDInsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Databáze Azure SQL, Azure SQL DW, systému SQL Server

## <a name="azure-hdinsight-linked-service"></a>Propojená služba Azure HDInsight
Můžete vytvořit propojené služby Azure HDInsight k registraci vlastní cluster HDInsight s Data Factory.

### <a name="example"></a>Příklad:

```json
{
  "name": "HDInsightLinkedService",
  "properties": {
    "type": "HDInsight",
    "typeProperties": {
      "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
      "userName": "admin",
      "password": "<password>",
      "linkedServiceName": "MyHDInsightStoragelinkedService"
    }
  }
}
```

### <a name="properties"></a>Vlastnosti
| Vlastnost          | Popis                              | Požaduje se |
| ----------------- | ---------------------------------------- | -------- |
| type              | Vlastnost typu musí být nastavená na **HDInsight**. | Ano      |
| clusterUri        | Identifikátor URI clusteru HDInsight.        | Ano      |
| uživatelské jméno          | Zadejte jméno uživatele, který se má použít pro připojení k existujícímu clusteru HDInsight. | Ano      |
| heslo          | Zadejte heslo pro uživatelský účet.   | Ano      |
| linkedServiceName | Název úložiště Azure, propojené služby, která odkazuje na Azure blob storage používaný v clusteru HDInsight. <p>V současné době nelze zadat, že Azure Data Lake Store propojené služby pro tuto vlastnost. Pokud HDInsight cluster má přístup do Data Lake Store, k datům v Azure Data Lake Store můžete přistupovat z Hive nebo Pig skriptů. </p> | Ano      |

## <a name="azure-batch-linked-service"></a>Propojená služba Azure Batch
Můžete vytvořit služby Azure Batch propojené můžete zaregistrovat fondu služby Batch virtuálních počítačů (VM) pro služby data factory. Můžete spustit pomocí Azure Batch nebo Azure HDInsight vlastní aktivity .NET.

Najdete v následujících tématech, pokud začínáte používat službu Azure Batch:

* [Základy Azure Batch](../../batch/batch-technical-overview.md) Přehled služby Azure Batch.
* [Nové AzureBatchAccount](https://msdn.microsoft.com/library/mt125880.aspx) rutiny k vytvoření účtu Azure Batch (nebo) [portál Azure](../../batch/batch-account-create-portal.md) vytvoření účtu Azure Batch pomocí portálu Azure. V tématu [pomocí prostředí PowerShell ke správě účtu Azure Batch](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) téma pro podrobné pokyny k použití rutiny.
* [Nový-AzureBatchPool](https://msdn.microsoft.com/library/mt125936.aspx) rutiny vytvoření fondu Azure Batch.

### <a name="example"></a>Příklad:

```json
{
  "name": "AzureBatchLinkedService",
  "properties": {
    "type": "AzureBatch",
    "typeProperties": {
      "accountName": "<Azure Batch account name>",
      "accessKey": "<Azure Batch account key>",
      "poolName": "<Azure Batch pool name>",
      "linkedServiceName": "<Specify associated storage linked service reference here>"
    }
  }
}
```

Připojit "**.\< Název oblasti\>**"na název účtu batch pro **accountName** vlastnost. Příklad:

```json
"accountName": "mybatchaccount.eastus"
```

Další možností je zajistit batchUri koncový bod, jak znázorňuje následující ukázka:

```json
"accountName": "adfteam",
"batchUri": "https://eastus.batch.azure.com",
```

### <a name="properties"></a>Vlastnosti
| Vlastnost          | Popis                              | Požaduje se |
| ----------------- | ---------------------------------------- | -------- |
| type              | Vlastnost typu musí být nastavená na **AzureBatch**. | Ano      |
| název účtu       | Název účtu Azure Batch.         | Ano      |
| accessKey         | Přístupový klíč pro účet Azure Batch.  | Ano      |
| poolName          | Název fondu virtuálních počítačů.    | Ano      |
| linkedServiceName | Název úložiště Azure propojená služba přidruženého k této službě Azure Batch propojený. Tato propojená služba se používá pro pracovní soubory potřebné ke spuštění aktivity a ukládání protokoly spuštění aktivity. | Ano      |

## <a name="azure-machine-learning-linked-service"></a>Propojená služba Azure Machine Learning
Vytváření služby Azure Machine Learning propojené k registraci dávce Machine Learning vyhodnocovací koncový bod pro služby data factory.

### <a name="example"></a>Příklad:

```json
{
  "name": "AzureMLLinkedService",
  "properties": {
    "type": "AzureML",
    "typeProperties": {
      "mlEndpoint": "https://[batch scoring endpoint]/jobs",
      "apiKey": "<apikey>"
    }
  }
}
```

### <a name="properties"></a>Vlastnosti
| Vlastnost   | Popis                              | Požaduje se |
| ---------- | ---------------------------------------- | -------- |
| Typ       | Vlastnost typu musí být nastavená na: **AzureML**. | Ano      |
| mlEndpoint | Adresu URL dávkového vyhodnocování.                   | Ano      |
| apiKey     | Rozhraní API modelu publikované prostoru.     | Ano      |

## <a name="azure-data-lake-analytics-linked-service"></a>Propojená služba Azure Data Lake Analytics
Vytvoříte **Azure Data Lake Analytics** propojená služba Azure Data Lake Analytics výpočetní služby s objektem pro vytváření dat Azure. Data Lake Analytics U-SQL aktivitu v kanálu odkazuje na tato propojená služba. 

Následující tabulka obsahuje popis obecné vlastnosti používané v definici JSON. Dále můžete mezi instanční objekt a ověření přihlašovacích údajů uživatele.

| Vlastnost                 | Popis                              | Požaduje se                                 |
| ------------------------ | ---------------------------------------- | ---------------------------------------- |
| **Typ**                 | Vlastnost typu musí být nastavená na: **AzureDataLakeAnalytics**. | Ano                                      |
| **název účtu**          | Název účtu Azure Data Lake Analytics.  | Ano                                      |
| **dataLakeAnalyticsUri** | Identifikátor URI služby Azure Data Lake Analytics.           | Ne                                       |
| **ID předplatného**       | Id předplatného Azure                    | Ne (když není určeno, předplatné objektu pro vytváření dat se používá). |
| **Název skupiny prostředků**    | Název skupiny prostředků Azure.                | Ne (když není určeno, skupinu prostředků objektu pro vytváření dat se používá). |

### <a name="service-principal-authentication-recommended"></a>Objekt zabezpečení ověřování služby (doporučeno)
Pokud chcete použít ověřování hlavní služby, zaregistrujte entitu aplikace v Azure Active Directory (Azure AD) a jí udělit přístup k Data Lake Store. Podrobné pokyny najdete v tématu [Service-to-service ověřování](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Poznamenejte si následující hodnoty, které můžete použít k definování propojené služby:
* ID aplikace
* Klíč aplikace 
* ID tenanta

Použijte objekt zabezpečení ověřování služby tak, že zadáte následující vlastnosti:

| Vlastnost                | Popis                              | Požaduje se |
| :---------------------- | :--------------------------------------- | :------- |
| **servicePrincipalId**  | Zadejte ID aplikace klienta.     | Ano      |
| **servicePrincipalKey** | Zadejte klíč aplikace.           | Ano      |
| **klienta**              | Zadejte informace o klienta (název nebo klienta domény ID) v rámci které se nachází aplikace. Můžete ji načíst podržením ukazatele myši v pravém horním rohu portálu Azure. | Ano      |

**Příkladu: Ověření objektu službu**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

### <a name="user-credential-authentication"></a>Ověření pověření uživatele
Alternativně můžete použít ověřování pověření uživatele pro Data Lake Analytics zadáním následujících vlastností:

| Vlastnost          | Popis                              | Požaduje se |
| :---------------- | :--------------------------------------- | :------- |
| **autorizace** | Klikněte **Autorizovat** tlačítko v editoru služby Data Factory a zadejte svoje přihlašovací údaje, který přiřazuje URL pro autorizaci automaticky generovaný této vlastnosti. | Ano      |
| **ID relace**     | ID relace OAuth z autorizační relace OAuth. Každé ID relace je jedinečné a může být použit pouze jednou. Toto nastavení se automaticky generuje při pomocí editoru služby Data Factory. | Ano      |

**Příklad: Ověření pověření uživatele**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a>Vypršení platnosti tokenu
Autorizační kód, který jste vygenerovali pomocí **Autorizovat** tlačítko vyprší po určité době. Pro dobu vypršení platnosti pro různé typy uživatelských účtů najdete v následující tabulce. Mohou se zobrazit následující chyby zprávy při ověřování **platnost tokenu vyprší**: přihlašovací údaje chyby operace: invalid_grant - AADSTS70002: Chyba při ověřování přihlašovacích údajů. AADSTS70008: Předloženému udělení přístupu je prošlý nebo odvolat. ID trasování: ID korelace d18629e8-af88-43c5-88e3-d8419eb1fca1: časové razítko fac30a0c-6be6-4e02-8d69-a776d2ffefd7: 2015-12-15 21:09:31Z

| Typ uživatele                                | Platnost vyprší po                            |
| :--------------------------------------- | :--------------------------------------- |
| Uživatelské účty, které nejsou spravované přes Azure Active Directory (@hotmail.com, @live.comatd.) | 12 hodin                                 |
| Účty uživatelů spravované pomocí Azure Active Directory (AAD) | 14 dnů po poslední řez spustit. <br/><br/>90 dnů, pokud řezu založeného na základě OAuth propojené služby používá alespoň jednou za 14 dní. |

Vyhněte se/vyřešit tuto chybu, opětovné pověření pomocí **Authorize** tlačítko při **platnost tokenu vyprší** a znovu nasaďte propojené služby. Můžete také vygenerovat hodnoty pro **sessionId** a **autorizace** vlastnosti programově pomocí kódu následujícím způsobem:

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

V tématu [azuredatalakestorelinkedservice třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), a [AuthorizationSessionGetResponse třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) témata podrobnosti o třídy objektu pro vytváření dat používá v kódu. Přidat odkaz na: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll pro třídu WindowsFormsWebAuthenticationDialog. 

## <a name="azure-sql-linked-service"></a>Propojená služba Azure SQL
Vytvoření služby Azure SQL propojené a použít je s [aktivity uložené procedury](data-factory-stored-proc-activity.md) k vyvolání uložené procedury z objektu pro vytváření dat kanál. V tématu [konektor služby Azure SQL](data-factory-azure-sql-connector.md#linked-service-properties) článku podrobnosti o této propojené službě.

## <a name="azure-sql-data-warehouse-linked-service"></a>Azure SQL datového skladu propojené služby
Vytvoření služby Azure SQL Data Warehouse propojené a použít je s [aktivity uložené procedury](data-factory-stored-proc-activity.md) k vyvolání uložené procedury z objektu pro vytváření dat kanál. V tématu [konektor služby Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) článku podrobnosti o této propojené službě.

## <a name="sql-server-linked-service"></a>Propojená služba systému SQL Server
Vytvoření služby SQL serveru propojená a použít je s [aktivity uložené procedury](data-factory-stored-proc-activity.md) k vyvolání uložené procedury z objektu pro vytváření dat kanál. V tématu [systému SQL Server konektoru](data-factory-sqlserver-connector.md#linked-service-properties) článku podrobnosti o této propojené službě.

