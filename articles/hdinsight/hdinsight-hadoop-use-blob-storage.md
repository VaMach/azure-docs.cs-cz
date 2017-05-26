---
title: "Dotazy na data z HDFS kompatibilního úložiště Azure | Dokumentace Microsoftu"
description: "Zjistěte, jak zadávat dotazy na data ze služby Azure Storage a Azure Data Lake Store pro ukládání výsledků analýzy."
keywords: blob storage, hdfs, structured data, unstructured data, data lake store, Hadoop input, Hadoop output, hadoop storage, hdfs input, hdfs output, hdfs storage, wasb azure
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1d2e65f2-16de-449e-915f-3ffbc230f815
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/27/2017
ms.author: jgao
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: bc7707f3bbb6639699826550f39876d0096d6c03
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017


---
# <a name="use-hdfs-compatible-storage-with-hadoop-in-hdinsight"></a>Použití HDFS kompatibilního úložiště se systémem Hadoop ve službě HDInsight

Pokud chcete analyzovat data v clusteru HDInsight, můžete je ukládat ve službě Azure Storage, Azure Data Lake Store nebo v obou. Obě možnosti ukládání umožňují bezpečné odstranění clusterů HDInsight, které se používají pro výpočty, aniž by se ztratila uživatelská data.

Hadoop podporuje hodnoty výchozího systému souborů. Výchozí systém souborů znamená výchozí schéma a autoritu. Lze ho také použít k vyřešení relativní cesty. Během procesu vytváření clusteru HDInsight můžete jako výchozí systém souborů zadat kontejner objektů blob ve službě Azure Storage. U služby HDInsight 3.5 můžete jako výchozí systém souborů vybrat službu Azure Storage nebo Azure Data Lake Store.

V tomto článku zjistíte, jak tyto dvě možnosti úložiště fungují s clustery HDInsight. Informace o vytvoření clusteru HDInsight najdete v tématu [Začínáme se službou HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

## <a name="using-azure-storage-with-hdinsight-clusters"></a>Použití služby Azure Storage s clustery HDInsight

Azure Storage je robustní řešení úložiště pro obecné účely, které se jednoduše integruje se službou HDInsight. HDInsight může jako výchozí systém souborů pro cluster používat kontejner objektů blob ve službě Azure Storage. Prostřednictvím rozhraní systému souborů Hadoop DFS (HDFS) může celá sada komponent ve službě HDInsight pracovat přímo se strukturovanými nebo nestrukturovanými daty uloženými jako objekty blob.

> [!WARNING]
> Při vytváření účtu služby Azure Storage je k dispozici několik možností. Následující tabulka poskytuje informace o podporovaných možnostech se službou HDInsight:
> 
> | Typ účtu úložiště | Úroveň úložiště | Podporováno se službou HDInsight |
> | ------- | ------- | ------- |
> | Účet služby Storage pro obecné účely | Standard | __Ano__ |
> | &nbsp; | Premium | Ne |
> | Účet služby Blob Storage | Hot | Ne |
> | &nbsp; | Cool | Ne |

### <a name="hdinsight-storage-architecture"></a>Architektura úložiště HDInsight
Následující diagram představuje abstraktní zobrazení architektury úložiště HDInsight:

![Clustery Hadoop používají rozhraní API HDFS pro přístup a ukládání strukturovaných i nestrukturovaných dat do služby Blob Storage.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "Architektura HDInsight Storage")

Služba HDInsight poskytuje přístup do systému souborů DFS, který je místně připojen k výpočetním uzlům. Tento systém souborů je přístupný pomocí plně kvalifikovaného identifikátoru URI, například:

    hdfs://<namenodehost>/<path>

Služba HDInsight navíc poskytuje možnost přístupu k datům uloženým ve službě Azure Storage. Syntaxe je:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

Při použití účtu Azure Storage s clustery HDInsight je potřeba zvážit tyto aspekty.

* **Kontejnery v účtech úložiště, které jsou připojeny ke clusteru:** Vzhledem k tomu, že název účtu a klíč jsou během vytváření přidružené  ke clusteru, máte plný přístup k objektům blob v těchto kontejnerech.

* **Veřejné kontejnery nebo veřejné objekty blob v účtech úložiště, které NEJSOU připojené ke clusteru:** Máte oprávnění jen pro čtení objektů blob v kontejnerech.
  
  > [!NOTE]
  > Veřejné kontejnery umožňují získat seznam všech objektů blob, které jsou k dispozici v tomto kontejneru a získat metadata kontejneru. Veřejné objekty blob umožňují přístup k objektům blob jenom v případě, že znáte přesnou adresu URL. Další informace najdete v tématu <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">Omezení přístupu ke kontejnerům a objektům blob</a>.
  > 
  > 
* **Privátní kontejnery v účtech úložiště, které NEJSOU připojené ke clusteru:** Nemůžete získat přístup k objektům blob, dokud nedefinujete účet úložiště při odesílání úlohy WebHCat. To se vysvětluje dále v tomhle článku.

Účty úložiště, které se definují v procesu vytváření a jejich klíče jsou uloženy v %HADOOP_HOME%/conf/core-site.xml na uzlech clusteru. Výchozím chováním služby HDInsight je používání účtů úložiště, které jsou definovány v souboru core-site.xml. Nedoporučuje se upravovat přímo soubor core-site.xml, protože hlavní uzel (master) clusteru může být kdykoli obnoven z image nebo migrován a změny tohoto souboru se neuchovají.

Více úloh WebHCat, včetně Hive, MapReduce, streamování Hadoop a Pig, může obsahovat popis účtů úložiště a spojených metadat. (To aktuálně funguje pro Pig s účty úložiště, ale ne pro metadata.) V části tohoto článku  [Přístup k objektům blob pomocí Azure PowerShell](#powershell) se nachází příklad této funkce. Více informací najdete v části [Použití clusteru HDInsight s alternativními účty úložiště a metaúložišti](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Objekty blob lze použít pro strukturovaná i nestrukturovaná data. Kontejnery objektů blob ukládají data jako páry klíč/hodnota a neexistuje žádná hierarchie adresářů. V názvu klíče se dá použít lomítko (/), aby název klíče připomínal  cestu k souboru. Klíč k objektu blob může být například *input/log1.txt*. Žádný skutečný *vstupní* adresář neexistuje, ale vzhledem k lomítku v názvu klíče tento název připomíná zobrazení cesty k souboru.

### <a id="benefits"></a>Výhody služby Azure Storage
Předpokládaná výkonová náročnost společně umístěných výpočetních clusterů a prostředků úložiště je zmírněna tím, že výpočetní clustery jsou vytvořeny blízko prostředků účtu úložiště uvnitř oblasti Azure, kde vysokorychlostní síť umožňuje velmi efektivní přístup výpočetních uzlů k datům ve službě Azure Storage.

S ukládáním dat ve službě Azure Storage namísto HDFS je spojeno několik výhod:

* **Opakované použití dat a sdílení:** data v HDFS se nachází uvnitř výpočetního clusteru. Jenom aplikace, které mají přístup k výpočetnímu clusteru, můžou používat data pomocí rozhraní API HDFS. Data ve službě Azure Storage jsou přístupná prostřednictvím rozhraní API HDFS nebo prostřednictvím [rozhraní REST API služby Blob Storage][blob-storage-restAPI]. Proto s větším počtem aplikací (včetně jiných clusterů HDInsight) a nástrojů  se dají vytvářet a využívat data.
* **Archivace dat:** Ukládání dat ve službě Azure Storage umožňuje bezpečné odstranění clusterů HDInsight, které jsou používány pro výpočty, aniž by se ztratila uživatelská data.
* **Náklady na úložiště dat:** Ukládání dat v systému souborů DFS je z dlouhodobého hlediska dražší než ukládání dat ve službě Azure Storage, protože náklady na výpočetní cluster jsou vyšší než náklady na službu Azure Storage. Navíc se data nemusí nahrávat znovu pro každou generaci výpočetních clusterů, náklady na nahrávání dat jsou tak nižší.
* **Elastické škálování:** I když HDFS poskytuje škálovaný systém souborů, škála se určuje podle počtu uzlů, které vytvoříte pro svůj cluster. Změna škálování může být složitější než využití elastického škálování, které je automaticky k dispozici ve službě Azure Storage.
* **Geografická replikace:** Službu Azure Storage je možné geograficky replikovat. I když to přináší geografické obnovení a redundanci dat, převzetí služeb při selhání do geograficky replikovaného umístění vážně ovlivňuje výkon a může vést k dalším nákladům. Doporučujeme proto geografickou replikaci dobře zvážit a zvolit jen v případě, že hodnota dat je vyšší než náklady na celou operaci.

Některé úlohy a balíčky MapReduce můžou vytvořit mezilehlé výsledky, které ve službě Azure Storage ve skutečnosti uložit nechcete. V takovém případě můžete zvolit k uložení dat do místní HDFS. Ve skutečnosti služba HDInsight používá DFS pro některé z těchto mezilehlých výsledků v úlohách Hive a jiných procesech.

> [!NOTE]
> Většina příkazů HDFS (například <b>ls</b>, <b>copyFromLocal</b> a <b>mkdir</b>) bude i nadále fungovat podle očekávání. Jenom příkazy, které jsou specifické pro nativní implementaci HDFS (což se označuje jako DFS), jako je například <b>fschk</b> a <b>dfsadmin</b>, se budou chovat ve službě Azure Storage odlišně.
> 
> 

### <a name="create-blob-containers"></a>Vytvoření kontejnerů objektů Blob
K použití objektů blob je třeba nejprve vytvořit [Účet služby Azure Storage][azure-storage-create]. V rámci tohoto procesu zadáte oblast Azure, ve které se účet úložiště vytvoří. Účet úložiště a clusteru musí být uloženy ve stejné oblasti. Databáze serveru SQL metaúložiště Hive a databáze serveru SQL metaúložiště Oozie musí být také umístěny ve stejné oblasti.

Bez ohledu na svoje umístění patří každý objekt blob, který vytvoříte, do kontejneru v účtu úložiště Azure. Tento kontejner může být existující objekt blob, který se vytvořil mimo HDInsight, nebo to může být kontejner, který se vytvořil pro cluster služby HDInsight.

Výchozí kontejner objektu blob ukládá konkrétní informace, jako je historie úlohy a protokoly. Výchozí kontejner objektu Blob nesdílejte s více clustery služby HDInsight. Může dojít k poškození historie úlohy. Doporučujeme použít jiný kontejner pro každý cluster a umístit sdílená data na propojený účet úložiště, zadaný v nasazení všech příslušných clusterů, nikoli na výchozí účet úložiště. Další informace o konfiguraci propojených účtů úložiště najdete v tématu [Tvorba clusterů HDInsight][hdinsight-creation]. Nicméně, po odstranění původního clusteru HDInsight můžete znovu použít výchozí kontejner úložiště. Pro clustery HBase můžete zachovat schéma a data tabulky HBase vytvořením nového clusteru HBase pomocí výchozího kontejneru objektů blob, který je používán odstraněným clusterem HBase.

#### <a name="using-the-azure-portal"></a>Použití webu Azure Portal
Při vytváření clusteru HDInsight z portálu máte možnost (jak je vidět níže) zadat podrobnosti účtu úložiště. Můžete také určit, jestli chcete ke clusteru přidružit další účet úložiště, a pokud ano, zvolit jako další úložiště službu Data Lake Store nebo další Azure Storage Blob.

![Zdroj dat pro vytvoření hadoopu HDInsight](./media/hdinsight-hadoop-use-blob-storage/hdinsight.provision.data.source.png)

> [!WARNING]
> Použití dalšího účtu úložiště v jiném umístění, než je cluster HDInsight, není podporováno.

#### <a name="using-azure-cli"></a>Použití Azure CLI
[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

Pokud máte [nainstalováno a nakonfigurováno rozhraní příkazového řádku Azure CLI](../cli-install-nodejs.md), následující příkaz lze použít k účtu úložiště a kontejneru.

    azure storage account create <storageaccountname> --type LRS

> [!NOTE]
> Parametr `--type` určuje, jak bude účet úložiště replikován. Další informace najdete v tématu [Replikace Azure Storage](../storage/storage-redundancy.md). Nepoužívejte ZRS, protože nepodporuje objekt blob, soubor,  tabulku nebo frontu stránky.
> 
> 

Budete vyzváni k zadání geografické oblasti, ve které se vytvoří účet úložiště. Účet úložiště byste měli vytvořit ve stejné oblasti, kterou chcete použít k vytvoření clusteru služby HDInsight.

Po vytvoření účtu úložiště použijte následující příkaz k načtení klíčů účtu úložiště:

    azure storage account keys list <storageaccountname>

Když chcete vytvořit kontejner, použijte následující příkaz:

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

#### <a name="using-azure-powershell"></a>Použití Azure Powershell
Pokud jste [nainstalovali a nakonfigurovali Azure PowerShell][powershell-install], můžete použít následující z příkazového řádku Azure PowerShell k vytvoření účtu úložiště a kontejneru:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"

    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"

    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID

    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location

    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS 

    # Create default blob containers
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

### <a name="address-files-in-azure-storage"></a>Adresování souborů ve službě Azure Storage
Schéma identifikátoru URI pro přístup k souborům ve službě Azure Storage ze služby HDInsight je:

    wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>

Schéma identifikátoru URI poskytuje nezašifrovaný přístup (s předponou *wasb:*) a zašifrovaný přístup SSL (s *wasbs*). Doporučujeme používat *wasbs* kdykoli je to možné, i v případě přístupu k datům, umístěným uvnitř stejné oblasti v Azure.

&lt;BlobStorageContainerName&gt; označuje název kontejneru objektů blob ve službě Azure Storage.
&lt;StorageAccountName&gt; identifikuje název účtu úložiště Azure. Vyžaduje se plně kvalifikovaný název domény (FQDN).

Pokud nebyl zadán &lt;BlobStorageContainerName&gt; ani &lt;StorageAccountName&gt;, použije se výchozí systém souborů. Pro soubory ve výchozím systému souborů můžete použít relativní cestu nebo absolutní cestu. Například soubor *hadoop-mapreduce-examples.jar*, který se dodává s clustery HDInsight, lze odkazovat pomocí jedné z následujících akcí:

    wasbs://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
    wasbs:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> V clusterech HDInsight verze 2.1 a 1.6. je název souboru <i>hadoop-examples.jar</i>
> 
> 

&lt;Cesta&gt; je název cesty HDFS souboru nebo adresáře. Vzhledem k tomu, že kontejnery ve službě Azure Storage jsou jednoduše úložiště párů klíč-hodnota, neexistuje žádný opravdový hierarchický systém souborů. Lomítko ( / ) uvnitř klíče objektu blob se považuje za oddělovač adresářů. Například název objektu blob pro *hadoop-mapreduce-examples.jar* je:

    example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> Při práci s objekty blob mimo HDInsight většina nástrojů nerozpozná formát WASB a místo toho očekávají základní formát cesty, jako je například `example/jars/hadoop-mapreduce-examples.jar`.
> 
> 

### <a name="access-blobs-using-azure-cli"></a>Přístup k objektům blob pomocí Azure CLI
Pomocí následujícího příkazu můžete zobrazit seznam příkazů týkajících se objektu blob:

    azure storage blob

**Příklad použití Azure CLI pro nahrání souboru**

    azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Příklad použití Azure CLI pro stažení souboru**

    azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>

**Příklad použití Azure CLI pro odstranění souboru**

    azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Příklad použití Azure CLI pro vytvoření seznamu souborů**

    azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>

### <a name="access-blobs-using-azure-powershell"></a>Přístup k objektům blob pomocí Azure PowerShell
> [!NOTE]
> Příkazy v této části jsou ukázkami základních příkladů použití prostředí PowerShell pro přístup k datům, uloženým v objektech blob. Obsáhlejší a plnohodnotný příklad, přizpůsobený pro práci s HDInsight, najdete v části [Nástroje HDInsight](https://github.com/Blackmist/hdinsight-tools).
> 
> 

Pomocí následujícího příkazu můžete zobrazit seznam rutin týkajících se objektu blob:

    Get-Command *blob*

![Seznam rutin prostředí PowerShell týkajících se objektu blob.][img-hdi-powershell-blobcommands]

#### <a name="upload-files"></a>Nahrání souborů
Viz [Nahrání dat do služby HDInsight][hdinsight-upload-data].

#### <a name="download-files"></a>Stažení souborů
Následující skript stáhne objekt blob bloku do aktuální složky. Před spuštěním skriptu změňte adresář na složku, ke které máte oprávnění k zápisu.

    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<AzureStorageAccountName>"   # The storage account used for the default file system specified at creation.
    $containerName = "<BlobStorageContainerName>"  # The default file system container has the same name as the cluster.
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.

    # Use Add-AzureAccount if you haven't connected to your Azure subscription
    Login-AzureRmAccount 
    Select-AzureRmSubscription -SubscriptionID "<Your Azure Subscription ID>"

    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force

    Write-Host "List the downloaded file ..." -ForegroundColor Green
    cat "./$blob"

Při poskytnutí názvu skupiny prostředků a názvu clusteru můžete použít následující kód:

    $resourceGroupName = "<AzureResourceGroupName>"
    $clusterName = "<HDInsightClusterName>"
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.

    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 

    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob $blob -Context $storageContext -Force

#### <a name="delete-files"></a>Odstranění souborů
    Remove-AzureStorageBlob -Container $containerName -Context $storageContext -blob $blob

#### <a name="list-files"></a>Zobrazení souborů
    Get-AzureStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"

#### <a name="run-hive-queries-using-an-undefined-storage-account"></a>Spuštění dotazů Hive pomocí nedefinovaného účtu úložiště
Tento příklad ukazuje, jak zobrazit obsah složky z účtu úložiště, které není definováno během procesu vytváření.
$clusterName = „<HDInsightClusterName>“

    $undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
    $undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

    $undefinedStorageKey = Get-AzureStorageKey $undefinedStorageAccount | %{ $_.Primary }

    Use-AzureRmHDInsightCluster $clusterName

    $defines = @{}
    $defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

    Invoke-AzureRmHDInsightHiveJob -Defines $defines -Query "dfs -ls wasbs://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"


### <a name="using-additional-storage-accounts"></a>Použití dalších účtů úložiště

Při vytváření clusteru HDInsight zadáváte účet služby Azure Storage, který k němu chcete přidružit. Kromě tohoto účtu úložiště můžete během procesu vytváření nebo až po vytvoření clusteru přidat další účty úložiště ze stejného předplatného Azure nebo různých předplatných Azure. Pokyny pro přidání dalších účtů úložiště najdete v tématu [Vytváření clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]
> Použití dalšího účtu úložiště v jiném umístění, než je cluster HDInsight, není podporováno.

## <a name="using-azure-data-lake-store-with-hdinsight-clusters"></a>Použití služby Azure Data Lake Store s clustery HDInsight

Clustery HDInsight můžou službu Azure Data Lake Store využívat dvěma způsoby:

* Azure Data Lake Store jako výchozí úložiště, nebo
* Azure Data Lake Store jako další úložiště, přičemž Azure Storage Blob je výchozí úložiště.

> [!NOTE]
> Ke službě Azure Data Lake Store se vždy přistupuje prostřednictvím zabezpečeného kanálu, takže se nepoužívá název schématu systému souborů `adls`. Vždy používáte `adl`.
> 
> 

### <a name="using-azure-data-lake-store-as-default-storage"></a>Použití služby Azure Data Lake Store jako výchozího úložiště

Když je služba HDInsight nasazená se službou Azure Data Lake Store jako výchozím úložištěm, soubory související s clusterem se ukládají do úložiště Azure Data Lake v tomto umístění:

    adl://mydatalakestore/<cluster_root_path>/

kde `<cluster_root_path>` je název složky, kterou vytvoříte ve službě Azure Data Lake Store. Pokud pro každý cluster zadáte kořenovou cestu, můžete stejný účet Azure Data Lake Store použít pro více než jeden cluster. Takže máte nastavení, kde:

* Cluster1 může používat cestu `adl://mydatalakestore/cluster1storage`.
* Cluster2 může používat cestu `adl://mydatalakestore/cluster2storage`.

Všimněte si, že oba clustery používají stejný účet Data Lake Store **mydatalakestore**. Každý cluster má přístup k vlastnímu kořenovému systému souborů ve službě Data Lake Store. Prostředí nasazení na webu Azure Portal vás zvláště vyzývá, abyste pro kořenovou cestu používali název složky ve formátu například **/clustery/\<název_clusteru>**.

#### <a name="accessing-files-from-the-cluster"></a>Přístup k souborům z clusteru

Existuje několik způsobů, jak můžete přistupovat k souborům ve službě Azure Data Lake Store z clusteru HDInsight.

* **Pomocí plně kvalifikovaného názvu**. S tímto přístupem zadáváte úplnou cestu k souboru, ke kterému chcete získat přístup.

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/<file_path>

* **Pomocí zkráceného formátu cesty**. S tímto přístupem nahradíte část cesty až ke kořenu clusteru za adl:///. Takže ve výše uvedeném příkladu můžete nahradit `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` za `adl:///`.

        adl:///<file path>

* **Pomocí relativní cesty**. S tímto přístupem zadáváte pouze relativní cestu k souboru, ke kterému chcete získat přístup. Například pokud je úplná cesta k souboru následující:

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/example/data/sample.log

    Ke stejnému souboru sample.log můžete přistupovat pomocí této relativní cesty:

        /example/data/sample.log

### <a name="using-azure-data-lake-store-as-additional-storage"></a>Použití služby Azure Data Lake Store jako dalšího úložiště

Službu Data Lake Store můžete také použít jako další úložiště. Výchozím úložištěm clusteru v takových případech může být účet Azure Data Lake Store nebo Azure Storage Blob. Pokud spouštíte úlohy HDInsight s daty uloženými ve službě Azure Data Lake Store jako dalším úložišti, musíte použít plně kvalifikovanou cestu k souborům. Například:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Všimněte si, že teď v adrese URL není **cluster_root_path**. Je to proto, že služba Data Lake Store v tomto případě není výchozím úložištěm, takže stačí zadat pouze cestu k souborům.


### <a name="creating-hdinsight-clusters-with-access-to-data-lake-store"></a>Vytváření clusterů HDInsight s přístupem ke službě Data Lake Store

Na následujících odkazech najdete podrobné pokyny k vytvoření clusterů HDInsight s přístupem ke službě Data Lake Store.

* [Pomocí portálu](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)
* [Pomocí PowerShellu (se službou Data Lake Store jako výchozím úložištěm)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [Pomocí PowerShellu (se službou Data Lake Store jako dalším úložištěm)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [Pomocí šablon Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)


## <a name="next-steps"></a>Další kroky
V tomto článku jste zjistili, jak používat HDFS kompatibilní službu Azure Storage a Azure Data Lake Store se službou HDInsight. To umožňuje vytvářet škálovatelná a dlouhodobá řešení pro získávání archivovaných dat a používat službu HDInsight k odemčení informací uvnitř uložených strukturovaných a nestrukturovaných dat.

Další informace naleznete v tématu:

* [Začínáme se službou Azure HDInsight][hdinsight-get-started]
* [Začínáme se službou Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md)
* [Nahrání dat do služby HDInsight][hdinsight-upload-data]
* [Použití Hivu se službou HDInsight][hdinsight-use-hive]
* [Použití Pigu se službou HDInsight][hdinsight-use-pig]
* [Použití sdílených přístupových podpisů služby Azure Storage k omezení přístupu k datům pomocí HDInsight][hdinsight-use-sas]

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: ../storage/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  

