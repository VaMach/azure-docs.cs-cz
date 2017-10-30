---
title: "Nasazení modelu pro služby Azure Machine Learning (verze Preview) | Dokumentace Microsoftu"
description: "V tomto kurzu na pokračování se dozvíte, jak komplexně používat služby Azure Machine Learning (verze Preview). Toto je 3. část na téma nasazení modelu."
services: machine-learning
author: raymondl
ms.author: raymondl, aashishb
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: hero-article
ms.date: 09/27/2017
ms.openlocfilehash: 56a79906a0f43f06d35db703d641f547e7bdf868
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/20/2017
---
# <a name="classifying-iris-part-3-deploy-a-model"></a>Klasifikace Iris, část 3: Nasazení modelu
Služby Azure Machine Learning (verze Preview) představují integrované, komplexní řešení datové vědy a pokročilé analýzy pro profesionální datové vědce, které slouží k přípravě dat, vývoji experimentů a nasazování modelů na úrovni cloudu.

Tento kurz je třetí částí z třídílné série. V této části kurzu se naučíte, jak pomocí služeb Azure Machine Learning (verze Preview) provádět následující kroky:

> [!div class="checklist"]
> * Vyhledání souboru modelu
> * Vygenerování hodnoticího skriptu a souboru schématu
> * Příprava prostředí
> * Vytvoření webové služby v reálném čase
> * Spuštění webové služby v reálném čase
> * Zkoumání výstupních dat objektů blob 

 Tento kurz používá kvůli zjednodušení nadčasovou [datovou sadu Iris](https://en.wikipedia.org/wiki/iris_flower_data_set). Snímky obrazovky jsou specifické pro systém Windows, ale prostředí v systému macOS je téměř shodné.

## <a name="prerequisites"></a>Požadavky
Měli byste si nejdříve projít první dva kurzy z této série:

1. Postupujte podle [kurzu přípravy dat](tutorial-classifying-iris-part-1.md) a vytvořte prostředky služby Azure Machine Learning a nainstalujte aplikaci Azure Machine Learning Workbench.

2. Postupujte podle [kurzu sestavení modelu](tutorial-classifying-iris-part-2.md) a vytvořte ve službě Azure Machine Learning logistický regresní model.

3. Potřebujete nainstalovaný a místně spuštěný modul Docker. Alternativně můžete nasazení provést do clusteru Azure Container Service v Azure.

## <a name="download-the-model-pickle-file"></a>Stažení souboru pickle modelu
V předchozí části kurzu se skript **iris_sklearn.py** spouštěl v aplikaci Azure Machine Learning Workbench lokálně. Tento krok zajistil serializaci logistického regresního modelu pomocí oblíbeného balíčku serializace objektů Python **[pickle](https://docs.python.org/2/library/pickle.html)**. 

1. Spusťte aplikaci **Azure Machine Learning Workbench** a otevřete projekt **myIris**, který jste vytvořili v předchozí části této série kurzů.

2. Jakmile se projekt otevře, klikněte v aplikaci Azure Machine Learning Workbench na tlačítko **Soubory** (ikona složky) na levém panelu nástrojů a otevřete seznam souborů ve složce projektu.

3. Vyberte soubor **iris_sklearn.py** a v aplikaci Workbench se na nové kartě textového editoru otevře kód Python.

4. Podívejte se do souboru **iris_sklearn.py**, abyste věděli, kde se soubor pickle vygeneroval. Pomocí kombinace kláves CTRL+F otevřete dialogové okno hledání a potom v kódu Python vyhledejte slovo **pickle**.

   Tento fragment kódu ukazuje, jak se vygeneroval výstupní soubor pickle. Všimněte si, že výstupní soubor pickle má na disku název **model.pkl**. 

   ```python
   print("Export the model to model.pkl")
   f = open('./outputs/model.pkl', 'wb')
   pickle.dump(clf1, f)
   f.close()
   ```

5. Vyhledejte soubor pickle modelu ve výstupních souborech některého z předchozích spuštění.
   
   Po spuštění skriptu **iris_sklearn.py** se soubor modelu zapsal do složky **outputs** s názvem **model.pkl**. Tato složka se nachází v prostředí pro spuštění, které pro spuštění skriptu vyberete, a ne v místní složce projektu. 
   
   - K vyhledání souboru použijte aplikaci Azure Machine Learning Workbench a kliknutím na tlačítko **Spuštění** (ikona hodin) na levém panelu nástrojů otevřete seznam **Všechna spuštění**.  
   - Otevře se karta **Všechna spuštění**. V tabulce spuštění vyberte některé z nedávných spuštění s cílem **local** a názvem skriptu **iris_sklearn.py**. 
   - Otevře se stránka **vlastností spuštění**. V pravém horním rohu stránky si všimněte části **Výstupy**. 
   - Stáhněte soubor pickle tím, že zaškrtnete políčko vedle souboru **model.pkl** a kliknete na tlačítko **Stáhnout**. Uložte soubor do kořenové složky projektu. Budete ho potřebovat v dalších krocích.

   ![Stažení souboru pickle](media/tutorial-classifying-iris/download_model.png)

   Další informace o složce **outputs** najdete v článku [Čtení a zapisování velkých datových souborů](how-to-read-write-files.md).

## <a name="get-scoring-and-schema-files"></a>Získání souborů vyhodnocení a schématu
Pokud chcete se souborem modelu nasadit také webovou službu, potřebujete hodnoticí skript a volitelně také schéma vstupních dat webové služby. Hodnoticí skript načte soubor **model.pkl** z aktuální složky a na jeho základě vytvoří nově předpokládanou třídu Iris.  

1. Spusťte aplikaci **Azure Machine Learning Workbench** a otevřete projekt **myIris**, který jste vytvořili v předchozí části této série kurzů.

2. Jakmile se projekt otevře, klikněte v aplikaci Azure Machine Learning Workbench na tlačítko **Soubory** (ikona složky) na levém panelu nástrojů a otevřete seznam souborů ve složce projektu.

3. Vyberte soubor **iris_score.py**. Otevře se skript jazyka Python. Tento soubor se používá jako soubor vyhodnocení.

   ![Soubor vyhodnocení](media/tutorial-classifying-iris/model_data_collection.png)

4. Pokud chcete získat soubor schématu, spusťte skript. Na příkazovém řádku vyberte prostředí **local** a skript **iris_score.py** a potom klikněte na tlačítko **Spustit**. 

5. Tento skript vytvoří ve složce **outputs** soubor JSON, ve kterém je zaznamenané schéma vstupních dat vyžadované modelem.

6. Všimněte si panelu úloh na pravé straně okna aplikace Machine Learning Workbench. Počkejte, až se u nejnovější úlohy **iris\_score.py** zobrazí zelený stav **Dokončeno**. Potom klikněte na hypertextový odkaz **iris\_score.py [1]** u nejnovějšího spuštění úlohy a prohlédněte si podrobnosti o spuštění skriptu **iris_score.py**. 

7. Na stránce vlastností spuštění v části **Výstupy** vyberte nově vytvořený soubor **service_schema.json**. **Zkontrolujte** tento soubor a potom klikněte na **Stáhnout**. Uložte soubor do kořenové složky projektu.

8. Vraťte se na předchozí kartu, kde máte otevřený skript **iris_score.py**. 

   Všimněte si, že se používá shromažďování dat, které umožňuje zaznamenat vstupy modelu a předpovědi z webové služby. Pro shromažďování dat jsou zajímavé především následující body:

9. Projděte si kód v horní části třídy importu souborů ModelDataCollector, který obsahuje funkci shromažďování dat modelu:

   ```python
   from azureml.datacollector import ModelDataCollector
   ```

10. Projděte si následující řádky kódu ve funkci **init()**, která vytváří instanci ModelDataCollector:

   ```python
   global inputs_dc, prediction_dc
   inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
   prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")`
   ```

11. Projděte si následující řádky kódu ve funkci **run(input_df**, která shromažďuje data vstupu a předpovědí:

   ```python
   global clf2, inputs_dc, prediction_dc
   inputs_dc.collect(input_df)
   prediction_dc.collect(pred)
   ```

Teď můžete začít připravovat své prostředí na zprovoznění modelu.

## <a name="prepare-to-operationalize-locally"></a>Příprava na místní zprovoznění
Pomocí nasazení v _místním režimu_ proveďte spuštění v kontejnerech Docker v místním počítači.

_Místní režim_ můžete použít pro vývoj a testování. K provedení následujících kroků pro zprovoznění modelu je potřeba, aby byl modul Docker spuštěný místně. Pokud chcete zobrazit nápovědu k příkazům, můžete na konci příkazů použít příznak `-h`.

>[!NOTE]
>Pokud nemáte modul Docker v místním počítači, můžete místo toho v Azure vytvořit cluster k nasazení. Jenom po tomto kurzu nezapomeňte cluster odstranit, aby vám zbytečně nenabíhaly poplatky.

1. V aplikaci Microsoft Azure Machine Learning Workbench otevřete rozhraní příkazového řádku. V nabídce Soubor klikněte na **Otevřít příkazový řádek**.

   Příkazový řádek se otevře v aktuálním umístění složky projektu **c:\temp\myIris>**.

2. Ujistěte se, že je ve vašem předplatném zaregistrovaný poskytovatel prostředků Azure **Microsoft.ContainerRegistry**. Tohoto poskytovatele prostředků zaregistrujte dřív, než v kroku 3 vytvoříte prostředí. To, jestli je už zaregistrovaný, můžete zkontrolovat pomocí tohoto příkazu:
   ``` 
   az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table 
   ``` 

   Zobrazený výstup by měl vypadat přibližně takto: 
   ```
   Provider                                  Status 
   --------                                  ------
   Microsoft.Authorization                   Registered 
   Microsoft.ContainerRegistry               Registered 
   microsoft.insights                        Registered 
   Microsoft.MachineLearningExperimentation  Registered 
   ... 
   ```
   
   Pokud není poskytovatel **Microsoft.ContainerRegistry** zaregistrovaný, můžete ho zaregistrovat pomocí následujícího příkazu:
   ``` 
   az provider register --namespace Microsoft.ContainerRegistry 
   ```
   Registrace může trvat několik minut a vy můžete zkontrolovat její stav pomocí výše uvedeného příkazu **az provider list** nebo tohoto příkazu:
   ``` 
   az provider show -n Microsoft.ContainerRegistry 
   ``` 

   Ve třetím řádku výstupu se zobrazí **"registrationState": "Registrace"**. Chvíli počkejte a potom příkaz show opakujte, dokud se ve výstupu nezobrazí **"registrationState": "Registrováno"**.

3. Vytvořte prostředí. Tento krok je potřeba spustit jednou pro každé prostředí (například jednou pro vývojové prostředí a jednou pro produkční prostředí). Pro první prostředí použijte _místní režim_. (Později můžete zkusit v tomto příkazu použít přepínač `-c` nebo `--cluster` a vytvořit prostředí v _režimu clusteru_.)

   ```azurecli
   az ml env setup -n <new deployment environment name> --location <e.g. eastus2>
   ```
   
   Podle pokynů na obrazovce vytvořte účet úložiště pro ukládání obrázků Dockeru, službu ACR (Azure Container Registry) pro zobrazení seznamu imagí Dockeru a účet služby AppInsight pro shromažďování telemetrických dat. Pokud jste použili přepínač `-c`, vytvoří se také cluster služby ACS (Azure Container Service).
   
   Název clusteru nabízí způsob, jak identifikovat příslušné prostředí. Umístění by mělo být stejné jako umístění účtu pro správu modelu vytvořeného na webu Azure Portal.

4. Vytvořte účet pro správu modelu (tento krok je jednorázový).  
   ```azurecli
   az ml account modelmanagement create --location <e.g. eastus2> -n <new model management account name> -g <existing resource group name> --sku-name S1
   ```
   
5. Nastavte účet pro správu modelu.  
   ```azurecli
   az ml account modelmanagement set -n <youracctname> -g <yourresourcegroupname>
   ```

6. Nastavte prostředí.
Po vytvoření nastavte pomocí následujícího příkazu proměnné prostředí nutné k zprovoznění. Použijte stejný název prostředí, který jste už dřív použili v kroku 4. Použijte stejný název skupiny prostředků, který byl výstupem v příkazovém okně při dokončení procesu instalace.
   ```azurecli
   az ml env set -n <deployment environment name> -g <existing resource group name>
   ```

7. Pokud chcete zkontrolovat, jestli jste zprovozněné prostředí pro nasazení místní webové služby nastavili správně, zadejte následující příkaz:

   ```azurecli
   az ml env show
   ```

Teď můžete vytvořit webovou službu v reálném čase.

>[!NOTE]
>Svůj účet a prostředí pro správu modelů můžete znovu použít k dalším nasazením webových služeb. Nemusíte je vytvářet pro každou webovou službu zvlášť. K účtu nebo prostředí může být přiřazeno několik webových služeb.

## <a name="create-a-real-time-web-service-in-one-command"></a>Vytvoření webové služby v reálném čase pomocí jednoho příkazu
1. Pomocí následujícího příkazu vytvořte webovou službu v reálném čase:

   ```azurecli
   az ml service create realtime -f iris_score.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true 
   ```
   Vygeneruje se ID webové služby, které můžete použít později.

   Pro příkaz **az ml service create realtime** se používají následující přepínače:
   * -n: název aplikace, musí obsahovat jenom malá písmena
   * -f: název souboru hodnoticího skriptu
   * --model-file: soubor modelu, v tomto případě se jedná o soubor pickle model.pkl
   * -r: typ modelu, v tomto případě model python
   * --collect-model-data true: povolí shromažďování dat

   >[!IMPORTANT]
   >Název služby (a zároveň název nové image Dockeru) musí obsahovat jenom malá písmena, jinak dojde k chybě. 

2. Když příkaz spustíte, do účtu úložiště, který jste vytvořili v rámci vytváření prostředí, se nahraje model a soubor vyhodnocení. Proces nasazení vytvoří image Dockeru obsahující váš model, schéma a soubor vyhodnocení a předá je do registru ACR: **\<název_ACR\>.azureacr.io/\<název_image\>:\<verze\>**. 

   Potom tuto image přetáhne do místního počítače a na základě této image spustí kontejner Docker. Pokud je vaše prostředí nakonfigurované v režimu clusteru, kontejner Docker se místo toho nasadí do clusteru ACS Kubernetes.

   V rámci nasazení se v místním počítači vytvoří koncový bod HTTP REST pro webovou službu. Po několika minutách by se měl příkaz dokončit a zobrazit zprávu o úspěšném provedení. Webová služba je připravená k použití!

3. Spuštěný kontejner Docker můžete zobrazit pomocí příkazu **docker ps**:
   ```azurecli
   docker ps
   ```

## <a name="create-a-real-time-web-service-using-separate-commands"></a>Vytvoření webové služby v reálném čase pomocí samostatných příkazů
Jako alternativu k příkazu **az ml service create realtime** uvedenému výše můžete také provést jednotlivé kroky samostatně. Nejdříve zaregistrujte model, potom vytvořte manifest, sestavte image Dockeru a vytvořte webovou službu. Tento přístup vám zajistí větší flexibilitu v každém kroku. Navíc můžete použít entity vygenerované v předchozím kroku a další entity vytvářet jenom v případě potřeby.

1. Zaregistrujte model zadáním názvu souboru pickle.

   ```azurecli
   az ml model register --model model.pkl --name model.pkl
   ```
   Tento příkaz vygeneruje ID modelu.

2. Vytvoření manifestu

   K vytvoření manifestu použijte tento příkaz a zadejte výstup ID modelu z předchozího kroku:

   ```azurecli
   az ml manifest create --manifest-name <new manifest name> -f iris_score.py -r python -i <model ID> -s service_schema.json
   ```
   Tento příkaz vygeneruje ID manifestu.

3. Vytvoření image Dockeru

   K vytvoření image Dockeru použijte tento příkaz a zadejte hodnotu výstupu ID manifestu z předchozího kroku:

   ```azurecli
   az ml image create -n irisimage --manifest-id <manifest ID>
   ```
   Tento příkaz vygeneruje ID image Dockeru.
   
4. Vytvoření služby

   K vytvoření služby použijte uvedený příkaz a zadejte výstup ID image z předchozího kroku:

   ```azurecli
   az ml service create realtime --image-id <image ID> -n irisapp --collect-model-data true
   ```
   Tento příkaz vygeneruje ID webové služby.

Teď můžete webovou službu spustit.

## <a name="run-the-real-time-web-service"></a>Spuštění webové služby v reálném čase

Spuštěnou webovou službu **irisapp** otestujete tak, že jí předáte zakódovaný záznam JSON obsahující pole čtyř náhodných čísel.

1. Při vytváření webové služby se používala ukázková data. Při spuštění v místním režimu můžete volat příkaz **az ml service show realtime**. Toto volání načte ukázkový příkaz pro spuštění, který je vhodný k otestování služby. Tím také získáte hodnoticí adresu URL, pomocí které můžete službu začlenit do vlastní aplikace:

   ```azurecli
   az ml service show realtime -i <web service ID>
   ```

2. Pokud chcete službu otestovat, spusťte vrácený příkaz ke spuštění služby.

   ```azurecli
   az ml service run realtime -i irisapp -d "{\"input_df\": [{\"petal width\": 0.25, \"sepal length\": 3.0, \"sepal width\": 3.6, \"petal length\": 1.3}]}"
   ```
   Výstup je **„2“**, což je předpokládaná třída. (Váš výsledek se může lišit.) 

3. Pokud chcete službu spustit mimo rozhraní příkazového řádku, musíte získat klíče pro ověření:

   ```azurecli
   az ml service keys realtime -i <web service ID>
   ```

## <a name="view-the-collected-data-in-azure-blob-storage"></a>Zobrazení shromážděných dat v úložišti objektů blob v Azure

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

2. Vyhledejte své účty úložiště. Pokud to chcete udělat, klikněte na **Další služby**.

3. Do pole hledání zadejte **Účty úložiště** a stiskněte klávesu **Enter**.

4. Na stránce hledání **Účty úložiště** vyberte prostředek **Účet úložiště** odpovídající vašemu prostředí. 

   > [!TIP]
   > Pokud chcete určit, který účet úložiště se používá: Otevřete aplikaci Azure Machine Learning Workbench, vyberte projekt, na kterém pracujete, a z nabídky **Soubor** vyberte příkazový řádek. Na příkazovém řádku zadejte `az ml env show -v` a podívejte se na hodnotu *storage_account*. Toto je název účtu úložiště.

5. Po otevření stránky **Účet úložiště** klikněte na položku **Kontejnery** v seznamu vlevo. Najděte kontejner s názvem **modeldata**. 
 
   Pokud se nezobrazí žádná data, možná budete muset počkat až 10 minut od prvního požadavku na webovou službu, aby se do účtu úložiště začala šířit data.

   Data budou téci do objektů blob s následující cestou kontejneru:

   ```
   /modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv
   ```

6. Tato data můžete využívat z objektů blob Azure. Existuje řada různých nástrojů používajících software společnosti Microsoft a opensourcových nástrojů, jako třeba:

   - Azure ML Workbench: Otevřete soubor CSV v aplikaci Azure ML Workbench tak, že přidáte soubor CSV jako zdroj dat. 
   - Excel: Otevřete denní soubory CSV jako tabulku.
   - [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/): Vytvářejte grafy na základě dat získaných z dat ve formátu CSV v objektech blob.
   - [Hive](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started): Načtěte data ve formátu CSV do tabulky hive a spouštějte dotazy SQL přímo na objektech blob.
   - [Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-overview): Vytvořte strukturu dataframe s velkou částí dat ve formátu CSV.

      ```python
      var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
      ```


## <a name="next-steps"></a>Další kroky
V této třetí části třídílné série kurzů jste se naučili, jak pomocí služeb Azure Machine Learning provádět tyto úlohy:
> [!div class="checklist"]
> * Vyhledání souboru modelu
> * Vygenerování hodnoticího skriptu a souboru schématu
> * Příprava prostředí
> * Vytvoření webové služby v reálném čase
> * Spuštění webové služby v reálném čase
> * Zkoumání výstupních dat objektů blob 

Úspěšně jste spustili cvičný skript v různých výpočetních prostředích, vytvořili model, serializovali jej a zprovoznili jej prostřednictvím webové služby založené na Dockeru. 

Teď jste připravení na provádění pokročilé přípravy dat:
> [!div class="nextstepaction"]
> [Pokročilá příprava dat](tutorial-bikeshare-dataprep.md)

