---
title: "Začínáme s R Serverem ve službě HDInsight | Dokumentace Microsoftu"
description: "Naučíte se vytvořit Apache Spark v clusteru HDInsight, který obsahuje R Server, a potom do clusteru odeslat skript R."
services: HDInsight
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: b5e111f3-c029-436c-ba22-c54a4a3016e3
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/28/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 2c9877f84873c825f96b62b492f49d1733e6c64e
ms.openlocfilehash: 383a325bfd2620f6c4fd25ce2f3a66522131efef
ms.lasthandoff: 03/15/2017


---
# <a name="get-started-using-r-server-on-hdinsight"></a>Začínáme používat R Server ve službě HDInsight
HDInsight zahrnuje možnost integrace R Serveru do clusteru HDInsight. Díky tomu můžou skripty R používat Spark a MapReduce ke spouštění distribuovaných výpočtů. V tomto dokumentu se naučíte vytvořit R Server v clusteru HDInsight a následně spustit skript R, který ukazuje použití Sparku k distribuovaným výpočtům jazyka R.

## <a name="prerequisites"></a>Požadavky
* **Předplatné Azure:** Než začnete tento kurz, musíte mít předplatné Azure. Další informace najdete na stránce [Získat bezplatnou zkušební verzi Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Klient Secure Shell (SSH):** Klient SSH slouží k vzdálenému připojení ke clusteru HDInsight a spouštění příkazů přímo v clusteru. Systémy Linux, Unix a OS X poskytují klienta SSH prostřednictvím příkazu `ssh`. Pro systémy Windows doporučujeme [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

  * **Klíče SSH (volitelné):** Účet SSH používaný k připojení ke clusteru můžete zabezpečit pomocí hesla nebo veřejného klíče. Použití hesla je jednodušší a umožňuje začít bez nutnosti vytvářet pár veřejného a privátního klíče. Použití klíče je ale bezpečnější.

      Postupy v tomto dokumentu předpokládají použití hesla. Informace o vytvoření a používání klíčů SSH se službou HDInsight najdete v těchto dokumentech:

    * [Použití SSH se službou HDInsight z klientů systémů Linux, Unix a OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Použití SSH se službou HDInsight z klientů systému Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

### <a name="access-control-requirements"></a>Požadavky na řízení přístupu
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-the-cluster"></a>Vytvoření clusteru
> [!NOTE]
> Kroky v tomto dokumentu vás provedou vytvořením R Serveru v clusteru HDInsight s použitím základních informací o konfiguraci. Informace o dalších nastaveních konfigurace clusteru (jako je přidání dalších účtů úložiště, používání služby Azure Virtual Network nebo vytvoření metastoru pro Hive) najdete v tématu popisujícím [vytvoření clusterů HDInsight se systémem Linux](hdinsight-hadoop-provision-linux-clusters.md). Informace o vytvoření R Serveru pomocí šablony Azure Resource Manageru najdete v tématu [Nasazení clusteru HDInsight s R Serverem](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).
>
> 

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

2. Vyberte **NOVÝ**, **Intelligence a analýza** a pak **HDInsight**.

    ![Obrázek vytváření nového clusteru](./media/hdinsight-getting-started-with-r/newcluster.png)

3. V prostředí **Rychlé vytvoření** zadejte název clusteru do pole **Název clusteru**. Pokud máte více předplatných Azure, pomocí položky **Předplatné** vyberte předplatné, které chcete použít.

    ![Výběr názvu clusteru a předplatného](./media/hdinsight-getting-started-with-r/clustername.png)

4. Vyberte **Typ clusteru**. Otevře se okno **Konfigurace clusteru**. V okně **Konfigurace clusteru** vyberte následující možnosti:

   * **Typ clusteru:** R Server
   * **Verze:** Vyberte verzi R Serveru, která se má v clusteru nainstalovat. Vyberte nejnovější verzi, abyste získali nejnovější funkce. V případě potřeby jsou k dispozici i další verze pro zajištění kompatibility. Poznámky ke všem dostupným verzím jsou k dispozici [tady](https://msdn.microsoft.com/en-us/microsoft-r/notes/r-server-notes).
   * **Komunitní verze RStudia pro R Server:** Toto integrované vývojové prostředí (IDE) v prohlížeči se ve výchozím nastavení nainstaluje na hraničním uzlu.  Pokud si jeho instalaci nepřejete, zrušte zaškrtnutí tohoto políčka. Pokud zvolíte jeho instalaci, adresu URL pro přístup k přihlašovací stránce RStudio Serveru najdete po vytvoření clusteru v okně aplikace portálu pro váš cluster.

   Pro další možnosti ponechte výchozí hodnoty a pomocí tlačítka **Uložit** uložte typ clusteru.
   
   ![Snímek obrazovky okna Typ clusteru](./media/hdinsight-getting-started-with-r/clustertypeconfig.png)
         
5. Zadejte **Uživatelské jméno přihlášení clusteru** a **Heslo přihlášení clusteru**.

   Zadejte **Uživatelské jméno SSH**.  SSH se používá pro vzdálené připojení ke clusteru pomocí klienta **Secure Shell (SSH)**. Uživatele SSH můžete zadat v tomto dialogovém okně nebo po vytvoření clusteru (karta Konfigurace pro cluster). R Server je nakonfigurovaný, aby očekával **uživatelské jméno SSH** „remoteuser“.  **Pokud použijete jiné uživatelské jméno, po vytvoření clusteru budete muset provést dodatečný krok.**
   
   Ponechte zaškrtnuté políčko **Použít stejné heslo jako pro přihlášení ke clusteru** pro použití typu ověřování **HESLO**, pokud nedáváte přednost použití veřejného klíče.  Pokud budete chtít mít přístup k R Serveru v clusteru přes vzdáleného klienta, jako je RTVS, RStudio nebo jiné integrované vývojové prostředí (IDE) počítače, budete potřebovat pár veřejného a privátního klíče. Pokud instalujete komunitní verzi RStudio Serveru, budete muset zvolit heslo SSH.     
   
   Pokud chcete vytvořit a používat pár veřejného a privátního klíče, zrušte zaškrtnutí políčka **Použít stejné heslo jako pro přihlášení ke clusteru**, pak vyberte **VEŘEJNÝ KLÍČ** a postupujte následovně.  Tyto pokyny předpokládají, že máte nainstalované prostředí Cygwin, ve kterém je dostupný příkaz ssh-keygen nebo ekvivalentní příkaz.
   
   * Vygenerujte na svém přenosném počítači z příkazového řádku pár veřejného a privátního klíče:
   
   `ssh-keygen -t rsa -b 2048`

   * Postupujte na příkazovém řádku podle zobrazených pokynů, pojmenujte soubor klíče a pak zadejte přístupové heslo pro zvýšení zabezpečení. Vaše obrazovka by měla vypadat přibližně takto:

   ![Příkazový řádek SSH v systému Windows](./media/hdinsight-getting-started-with-r/sshcmdline.png)
   
   * Tím se vytvoří soubor privátního klíče a soubor veřejného klíče s názvem <název_souboru_privátního_klíče>.pub, například furiosa a furiosa.pub.
   
   ![Adresář SSH](./media/hdinsight-getting-started-with-r/dir.png)

   * Pak zadejte soubor veřejného klíče (*.pub) během přiřazování přihlašovacích údajů clusteru HDInsight, a nakonec potvrďte vaši skupinu prostředků a oblast a vyberte**Další**.
   
   ![Okno Přihlašovací údaje](./media/hdinsight-getting-started-with-r/publickeyfile.png)  
   
   * Změňte na svém přenosném počítači oprávnění k souboru privátního klíče.
   
   `chmod 600 <private-key-filename>`
   
   * Použijte soubor privátního klíče spolu s SSH ke vzdálenému přihlášení
   
   `ssh –i <private-key-filename> remoteuser@<hostname public ip>`
   
   nebo jako součást definice výpočetního kontextu Hadoop Spark pro R Server na klientovi (informace o použití Microsoft R Serveru jako klienta Hadoop najdete v části [Vytvoření výpočetního kontextu pro Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark) v on-line [dokumentu Začínáme se ScaleR v Apache Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started)).
   
6. Rychlé vytvoření vás přesměruje do okna **Úložiště**, kde vyberete nastavení účtu úložiště, které se má použít pro primární umístění systému souboru HDFS používaného clusterem. Vyberte nový nebo existující účet služby Azure Storage nebo existující účet Data Lake Store.

   1. Pokud vyberete účet služby Azure Storage, můžete vybrat existující účet úložiště tak, že vyberete možnost **Vybrat účet úložiště** a pak vyberete účet. Nebo můžete vytvořit nový účet pomocí odkazu **Vytvořit nový** v části **Vyberte účet úložiště**.

      > [!NOTE]
      > Pokud vyberete **Nový**, musíte zadat název pro nový účet úložiště. Pokud je název přijat, zobrazí se zelené zaškrtnutí.

      Pro **Výchozí kontejner** se ve výchozím nastavení použije název clusteru. Ponechte tuto hodnotu.

      Pokud jste vybrali možnost nového účtu úložiště, zobrazí se výzva k výběru **Umístění**, kde vyberete, v jaké oblasti se má účet úložiště vytvořit.  
   
         ![Okno zdroje dat](./media/hdinsight-getting-started-with-r/datastore.png)  
   
      > [!IMPORTANT]
      > Výběrem umístění pro výchozí zdroj dat se nastaví také umístění clusteru HDInsight. Cluster i výchozí zdroj dat musí být ve stejné oblasti.

   2. Pokud vyberete možnost použití existujícího účtu Data Lake Store, pak vyberte účet úložiště Azure Data Lake Store, který se má použít, a přidáním identity ADD do svého clusteru umožněte přístup k úložišti. Další informace o tomto procesu najdete v tématu popisujícím [vytvoření clusteru HDInsight se službou Data Lake Store pomocí webu Azure Portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal).

   Pomocí tlačítka **Vybrat** uložte konfiguraci zdroje dat.


7. Zobrazí se okno **Souhrn**, ve kterém můžete ověřit všechna nastavení. Tady můžete změnou hodnoty **Velikost clusteru** upravit počet serverů v clusteru a také můžete zadat **Akce skriptů**, které chcete spustit. Pokud si nejste jisti, že potřebujete větší cluster, ponechte výchozí počet pracovních uzlů, tedy `4`. V okně se zobrazí odhadované náklady na provoz clusteru.
   
   ![souhrn clusteru](./media/hdinsight-getting-started-with-r/clustersummary.png)

   > [!NOTE]
   > V případě potřeby můžete velikost clusteru změnit později na webu Azure Portal (Cluster -> Nastavení -> Škálování clusteru) a navýšit nebo snížit tak počet pracovních uzlů.  To může být užitečné ke snížení kapacity clusteru, když se zrovna nepoužívá, nebo ke zvýšení kapacity pro splnění požadavků větších úloh.
   >
   > 

    Mezi faktory, které byste měli vzít v úvahu při nastavování velikosti clusteru, datových uzlů a hraničních uzlů, patří:  

   * Výkon distribuovaných analýz R Serveru ve Sparku je v případě velkých dat úměrný počtu pracovních uzlů.  

   * Výkon analýz R Serveru lineárně klesá s velikostí analyzovaných dat. Například:  

     * Pro malá až středně velká data bude výkon nejlepší při analýze v místním výpočetním kontextu na hraničním uzlu.  Další informace o scénářích, kdy místní výpočetní kontext a výpočetní kontext Spark pracují nejlépe, najdete v tématu Možnosti výpočetního kontextu pro R Server ve službě HDInsight.<br>
     * Pokud se přihlásíte k hraničnímu uzlu a spustíte svůj skript R, všechny funkce kromě funkcí ScaleR rx se spustí <strong>místně</strong> na hraničním uzlu, proto by velikost paměti a počet jader na hraničním uzlu měly být nastavené odpovídajícím způsobem. Totéž platí, pokud používáte R Server ve službě HDInsight jako vzdálený výpočetní kontext ze svého přenosného počítače.

     ![Okno s cenovými úrovněmi uzlů](./media/hdinsight-getting-started-with-r/pricingtier.png)

     Pomocí tlačítka **Vybrat** uložte konfiguraci cen uzlů.

   Všimněte si, že je uvedený také odkaz na **stažení šablony a parametrů**. Po kliknutí na tento odkaz se zobrazí skripty, které můžete použít k automatizaci vytváření clusteru s použitím vybrané konfigurace. Po vytvoření clusteru jsou tyto skripty k dispozici také v položce pro váš cluster na webu Azure Portal.

   > [!NOTE]
   > Vytvoření clusteru chvíli trvá, obvykle přibližně 20 minut. Proces vytváření můžete zkontrolovat v dlaždici na Úvodním panelu nebo v položce **Oznámení** na levé straně stránky.
   >
   > 

## <a name="connect-to-rstudio-server"></a>Připojení k RStudio Serveru

Pokud jste se rozhodli do instalace zahrnout komunitní verzi RStudio Serveru, přístup k přihlašovací stránce RStudia můžete získat dvěma způsoby.

1. Buď přejděte na následující adresu URL (kde **NÁZEV_CLUSTERU** je název clusteru, který jste vytvořili): 

    https://**NÁZEV_CLUSTERU**.azurehdinsight.net/rstudio/

2. Nebo otevřete položku pro váš cluster na webu Azure Portal, vyberte rychlý odkaz Řídicí panel R Serveru a pak vyberte Řídicí panel RStudia:

     ![Přístup k řídicímu panelu RStudia](./media/hdinsight-getting-started-with-r/rstudiodashboard1.png)
     
     ![Přístup k řídicímu panelu RStudia](./media/hdinsight-getting-started-with-r/rstudiodashboard2.png)

   > [!IMPORTANT]
   > Bez ohledu na zvolenou metodu bude při prvním přihlášení potřeba dvojí ověření.  Při prvním ověření zadejte ID uživatele a heslo správce clusteru. Až se zobrazí druhá výzva, zadejte ID uživatele a heslo SSH. Při dalším přihlašování už budete potřebovat jenom heslo a ID uživatele SSH. 

## <a name="connect-to-the-r-server-edge-node"></a>Připojení k hraničnímu uzlu R Serveru
Připojte se pomocí SSH k hraničnímu uzlu R Serveru v clusteru HDInsight:

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> Adresu `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` můžete vyhledat také na webu Azure Portal tak, že vyberete cluster a pak **Všechna nastavení**, **Aplikace** a **R Server**. Zobrazí se informace o koncovém bodu SSH pro hraniční uzel.
>
> ![Obrázek koncového bodu SSH pro hraniční uzel](./media/hdinsight-getting-started-with-r/sshendpoint.png)
>
> 

Pokud jste použili heslo k zabezpečení uživatelského účtu SSH, budete vyzváni k jeho zadání. Pokud jste použili veřejný klíč, bude pravděpodobně muset použít parametr `-i` k určení odpovídajícího privátního klíče. Například, `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`.

Další informace o použití SSH se systémem Linux HDInsight najdete v následujících článcích:

* [Použití SSH se systémem Linux Hadoop ve službě HDInsight ze systému OS X, Linux a Unix](hdinsight-hadoop-linux-use-ssh-unix.md).
* [Použití SSH se systémem Linux Hadoop ve službě HDInsight ze systému Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

Po připojení se dostanete na příkazový řádek, který bude vypadat nějak takto:

`username@ed00-myrser:~$`

## <a name="use-the-r-console"></a>Použití konzoly R

1. Z relace SSH pomocí následujícího příkazu spusťte konzolu R.  
   
   ```
   R

   You will see output similar to the following.
   R version 3.2.2 (2015-08-14) -- "Fire Safety"
   Copyright (C) 2015 The R Foundation for Statistical Computing
   Platform: x86_64-pc-linux-gnu (64-bit)
   
   R is free software and comes with ABSOLUTELY NO WARRANTY.
   You are welcome to redistribute it under certain conditions.
   Type 'license()' or 'licence()' for distribution details.
   
   Natural language support but running in an English locale
   
   R is a collaborative project with many contributors.
   Type 'contributors()' for more information and
   'citation()' on how to cite R or R packages in publications.
   
   Type 'demo()' for some demos, 'help()' for on-line help, or
   'help.start()' for an HTML browser interface to help.
   Type 'q()' to quit R.
       
   Microsoft R Server version 8.0: an enhanced distribution of R
   Microsoft packages Copyright (C) 2016 Microsoft Corporation
   
   Type 'readme()' for release notes.
   >
   ```
   
2. Na příkazovém řádku `>` můžete zadat kód R. R Server obsahuje balíčky umožňující snadnou interakci se systémem Hadoop a spouštění distribuovaných výpočtů. Například pomocí následujícího příkazu zobrazíte kořen výchozího systému souborů pro cluster HDInsight.

`rxHadoopListFiles("/")`

Můžete také použít adresování ve stylu WASB.

`rxHadoopListFiles("wasbs:///")`

## <a name="using-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a>Použití R Serveru ve službě HDInsight ze vzdálené instance Microsoft R Serveru nebo klienta Microsoft R Client
Podle předchozí části popisující použití párů veřejného a privátního klíče pro přístup ke clusteru je možné nastavit přístup k výpočetnímu kontextu HDInsight Hadoop Spark ze vzdálené instance Microsoft R Serveru nebo klienta Microsoft R Client spuštěné na počítači nebo přenosném počítači (informace o použití Microsoft R Serveru jako klienta Hadoop najdete v části [Vytvoření výpočetního kontextu pro Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark) v on-line [průvodci Začínáme s RevoScaleR Hadoop Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started)).  Abyste to mohli provést, při definování výpočetního kontextu RxSpark na svém přenosném počítači budete muset zadat následující možnosti: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches a sshProfileScript. Například:

```
    myNameNode <- "default"
    myPort <- 0 
    
    mySshHostname  <- 'rkrrehdi1-ed-ssh.azurehdinsight.net'  # HDI secure shell hostname
    mySshUsername  <- 'remoteuser'# HDI SSH username
    mySshSwitches  <- '-i /cygdrive/c/Data/R/davec'   # HDI SSH private key
    
    myhdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep="/")
    myShareDir <- paste("/var/RevoShare" , mySshUsername, sep="/")
    
    mySparkCluster <- RxSpark(
      hdfsShareDir = myhdfsShareDir,
      shareDir     = myShareDir,
      sshUsername  = mySshUsername,
      sshHostname  = mySshHostname,
      sshSwitches  = mySshSwitches,
      sshProfileScript = '/etc/profile',
      nameNode     = myNameNode,
      port         = myPort,
      consoleOutput= TRUE
    )
```


## <a name="use-a-compute-context"></a>Použití výpočetního kontextu
Výpočetní kontext vám umožňuje řídit, zda se výpočty budou provádět místně na hraničním uzlu, nebo budou distribuovány napříč uzly v clusteru HDInsight.

1. V RStudio Serveru nebo konzole R (v rámci relace SSH) pomocí následujícího postupu načtěte ukázková data do výchozího úložiště pro službu HDInsight.
   
   ```
   # Set the HDFS (WASB) location of example data
   bigDataDirRoot <- "/example/data"
   # create a local folder for storaging data temporarily
   source <- "/tmp/AirOnTimeCSV2012"
   dir.create(source)
   # Download data to the tmp folder
   remoteDir <- "http://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
   download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
   download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
   download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
   download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
   download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
   download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
   download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
   download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
   download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
   download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
   download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
   download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))
   # Set directory in bigDataDirRoot to load the data into
   inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012") 
   # Make the directory
   rxHadoopMakeDir(inputDir)
   # Copy the data from source to input
   rxHadoopCopyFromLocal(source, bigDataDirRoot)
   ```
   
2. Nyní vytvoříme některé informace o datech a nadefinujeme dva zdroje dat, abychom s daty mohli pracovat.
   
   ```
   # Define the HDFS (WASB) file system
   hdfsFS <- RxHdfsFileSystem()
   # Create info list for the airline data
   airlineColInfo <- list(
         DAY_OF_WEEK = list(type = "factor"),
         ORIGIN = list(type = "factor"),
         DEST = list(type = "factor"),
         DEP_TIME = list(type = "integer"),
         ARR_DEL15 = list(type = "logical"))
   
   # get all the column names
   varNames <- names(airlineColInfo)
   
   # Define the text data source in hdfs
   airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)
   # Define the text data source in local system
   airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)
   
   # formula to use
   formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"
   ```
   
3. Nad daty spustíme logistickou regresi využívající místní výpočetní kontext.
   
   ```
   # Set a local compute context
   rxSetComputeContext("local")
   # Run a logistic regression
   system.time(
       modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
   )
   # Display a summary 
   summary(modelLocal)
   ```
   
   Zobrazený výstup by měl končit řádky, které vypadají přibližně takto:
   
   ```
   Data: airOnTimeDataLocal (RxTextData Data Source)
   File name: /tmp/AirOnTimeCSV2012
   Dependent variable(s): ARR_DEL15
   Total independent variables: 634 (Including number dropped: 3)
   Number of valid observations: 6005381
   Number of missing observations: 91381
   -2*LogLikelihood: 5143814.1504 (Residual deviance on 6004750 degrees of freedom)
   
   Coefficients:
                     Estimate Std. Error z value Pr(>|z|)
     (Intercept)   -3.370e+00  1.051e+00  -3.208  0.00134 **
     ORIGIN=JFK     4.549e-01  7.915e-01   0.575  0.56548
     ORIGIN=LAX     5.265e-01  7.915e-01   0.665  0.50590
     ......
     DEST=SHD       5.975e-01  9.371e-01   0.638  0.52377
     DEST=TTN       4.563e-01  9.520e-01   0.479  0.63172
     DEST=LAR      -1.270e+00  7.575e-01  -1.676  0.09364 .
     DEST=BPT         Dropped    Dropped Dropped  Dropped
     ---
     Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
   
     Condition number of final variance-covariance matrix: 11904202
     Number of iterations: 7
   ```
   
4. Nyní spustíme stejnou logistickou regresi využívající kontext Spark. Kontext Spark bude distribuovat zpracování do všech pracovních uzlů v clusteru HDInsight.
   
   ```
   # Define the Spark compute context 
   mySparkCluster <- RxSpark()
   # Set the compute context 
   rxSetComputeContext(mySparkCluster)
   # Run a logistic regression 
   system.time(  
       modelSpark <- rxLogit(formula, data = airOnTimeData)
   )
   # Display a summary
   summary(modelSpark)
   ```

   
   > [!NOTE]
   > Distribuovat výpočty napříč uzly clusteru můžete také pomocí MapReduce. Další informace o výpočetním kontextu najdete v tématu [Možnosti výpočetního kontextu pro R Server ve službě HDInsight](hdinsight-hadoop-r-server-compute-contexts.md).
   >
   >
   
## <a name="distribute-r-code-to-multiple-nodes"></a>Distribuování kódu R do více uzlů
S R Serverem můžete snadno vzít existující kód R a spustit ho napříč několika uzly v clusteru pomocí příkazu `rxExec`. To je užitečné při uklízení parametrů nebo provádění simulací. Následuje příklad použití příkazu `rxExec`.

`rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )`

Pokud stále používáte kontext Spark nebo MapReduce, příkaz vrátí hodnoty nodename (název uzlu) všech pracovních uzlů, ve kterých je spuštěný kód `(Sys.info()["nodename"])`. Například v clusteru se čtyřmi uzly se může zobrazit výstup, který vypadá nějak takto.


```
$rxElem1
    nodename
"wn3-myrser"
    
$rxElem2
    nodename
"wn0-myrser"
    
$rxElem3
    nodename
"wn3-myrser"
    
$rxElem4
    nodename
"wn3-myrser"
```

## <a name="accessing-data-in-hive-and-parquet"></a>Přístup k datům v Hive a Parquet
Nová funkce, která je k dispozici v R Serveru 9.0 a novějším, umožňuje přímý přístup k datům v Hive a Parquet pro použití ve funkcích ScaleR ve výpočetním kontextu Spark. Tyto možnosti jsou dostupné prostřednictvím nových funkcí zdroje dat ScaleR s názvem RxHiveData a RxParquetData. Funkce pomocí Spark SQL načtou data přímo do struktury DataFrame ve Sparku, aby je ScaleR mohl analyzovat.  

Následuje ukázkový kód pro použití těchto nových funkcí: 



```
#..create a Spark compute context

myHadoopCluster <- rxSparkConnect(reset = TRUE)
```


```
#..retrieve some sample data from Hive and run a model 

hiveData <- RxHiveData("select * from hivesampletable", 
                 colInfo = list(devicemake = list(type = "factor")))
rxGetInfo(hiveData, getVarInfo = TRUE)

rxLinMod(querydwelltime ~ devicemake, data=hiveData)
```

```
#..retrieve some sample data from Parquet and run a model 

rxHadoopMakeDir('/share')
rxHadoopCopyFromLocal(file.path(rxGetOption('sampleDataDir'), 'claimsParquet/'), '/share/')
pqData <- RxParquetData('/share/claimsParquet',
                 colInfo = list(
            age    = list(type = "factor"),
           car.age = list(type = "factor"),
              type = list(type = "factor")
         ) )
rxGetInfo(pqData, getVarInfo = TRUE)

rxNaiveBayes(type ~ age + cost, data = pqData)
```


``` 
#..check on Spark data objects, cleanup, and close the Spark session 

lsObj <- rxSparkListData() # two data objs are cached
lsObj
rxSparkRemoveData(lsObj)
rxSparkListData() # it should show empty list
rxSparkDisconnect(myHadoopCluster)
```

Další informace o používání těchto nových funkcí najdete v on-line nápovědě v R Serveru pomocí příkazů ?RxHivedata a ?RxParquetData.  


## <a name="install-r-packages"></a>Instalace balíčků R
Chcete-li nainstalovat na hraniční uzel další balíčky, můžete přímo v konzole R použít příkaz `install.packages()`, zatímco jste k hraničnímu uzlu připojeni přes SSH. Pokud však potřebujete balíčky R nainstalovat na pracovní uzly clusteru, musíte použít akci skriptu.

Akce skriptů jsou skripty Bash, které se používají k provádění změn konfigurace clusteru HDInsight nebo k instalaci dalšího softwaru. V tomto případě k instalaci dalších balíčků R. Chcete-li nainstalovat další balíčky pomocí akce skriptu, použijte následující postup.

> [!IMPORTANT]
> Instalace dalších balíčků R pomocí akcí skriptů je možná jedině po vytvoření clusteru. Akci skriptu nelze použít během vytváření clusteru, protože se skript spoléhá na úplnou instalaci a konfiguraci R Serveru.
>
> 

1. Na webu [Azure Portal](https://portal.azure.com) vyberte svůj R Server v clusteru HDInsight.
   
2. Novou akci skriptu odešlete tak, že v okně **Nastavení** vyberete **Akce skriptů** a pak **Odeslat novou**.
   
   ![Obrázek okna Akce skriptů](./media/hdinsight-getting-started-with-r/scriptaction.png)
   
3. V okně **Odeslat akci skriptu** zadejte následující informace.
   
   * **Název:** Popisný název k identifikaci tohoto skriptu.
   
   * **Identifikátor URI skriptu Bash:** `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`
   
   * **Hlavní:** Toto políčko by mělo být **nezaškrtnuté**.
   
   * **Pracovní:** Toto políčko by mělo být **zaškrtnuté**.
   
   * **Hraniční uzly:** Toto políčko by mělo být **nezaškrtnuté**.
   
   * **Zookeeper:** Toto políčko by mělo být **nezaškrtnuté**.
   
   * **Parametry:** Balíčky R určené k instalaci. Například `bitops stringr arules`.
   
   * **Zachovat tento skript...:** Toto políčko by mělo být **zaškrtnuté**.  
   
   > [!NOTE]
   > 1. Ve výchozím nastavení se všechny balíčky R instalují ze snímku úložiště Microsoft MRAN, který je konzistentní s nainstalovanou verzí R Serveru.  Chcete-li nainstalovat novější verze balíčků, vystavujete se riziku nekompatibility, můžete to však provést zadáním `useCRAN` jako prvního prvku seznamu balíčků, např. `useCRAN bitops, stringr, arules`.  
   > 2. Některé balíčky R budou vyžadovat další linuxové systémové knihovny. Pro usnadnění práce jsme předinstalovali potřebné závislosti pro 100 nejoblíbenějších balíčků R. Pokud ale balíčky R, které instalujete, vyžadují ještě další knihovny, budete si muset stáhnout základní skript, které zde používáme, a přidat další kroky k instalaci příslušných systémových knihoven. Upravený skript je pak nutné odeslat do veřejného kontejneru objektů blob v úložišti Azure, a k instalaci balíčků použít upravený skript.
   >    Další informace o vývoji akcí skriptů najdete v tématu [Vývoj akcí skriptů](hdinsight-hadoop-script-actions-linux.md).  
   >
   >
   
   ![Přidání akce skriptu](./media/hdinsight-getting-started-with-r/submitscriptaction.png)
   
4. Vyberte **Vytvořit** a spusťte skript. Po dokončení skriptu budou balíčky R k dispozici na všech pracovních uzlech.
   
## <a name="using-microsoft-r-server-operationalization"></a>Použití operacionalizace Microsoft R Serveru
Po dokončení modelování vašich dat můžete model operacionalizovat za účelem provádění předpovědí. Chcete-li nakonfigurovat operacionalizaci Microsoft R Serveru, proveďte následující kroky.

Nejprve se přes SSH připojte k hraničnímu uzlu. Například, ```ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net```.

Jakmile používáte SSH, přejděte do následujícího adresáře a pomocí příkazu sudo dotnet spusťte soubor .dll, jak je znázorněno níže.

```
   cd /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil
   sudo dotnet Microsoft.DeployR.Utils.AdminUtil.dll
```

Pokud chcete nakonfigurovat operacionalizaci Microsoft R Serveru s použitím jednotné konfigurace, postupujte následovně:

* Vyberte „1. Configure R Server for Operationalization“ (Konfigurovat R Server pro operacionalizaci).
* Vyberte „A. One-box (web + compute nodes)“ (Jednotná konfigurace webu a výpočetních uzlů).
* Zadejte heslo uživatele **admin**.

![jednotná konfigurace operacionalizace](./media/hdinsight-hadoop-r-server-get-started/admin-util-one-box-.png)

Volitelně můžete provést diagnostické kontroly spuštěním diagnostického testu, jak je znázorněno níže.

* Vyberte „6. Run diagnostic tests“ (Spustit diagnostické testy).
* Vyberte „A. Test configuration“ (Testovat konfiguraci).
* Zadejte uživatelské jméno admin a heslo, které jste zadali v jednom z předchozích kroků konfigurace.
* Ověřte, že se zobrazí „Overall Health: pass“ (Celkový stav: v pořádku)
* Ukončete nástroj pro správu
* Ukončete připojení SSH

![Diagnostika operacionalizace](./media/hdinsight-hadoop-r-server-get-started/admin-util-diagnostics.png)

V této fázi je konfigurace operacionalizace dokončena. Nyní se můžete pomocí balíčku mrsdeploy na vašem klientovi RClient připojit k operacionalizaci na hraničním uzlu, a začít používat funkce operacionalizace, například [vzdálené spouštění](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) a [webové služby](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette). V závislosti na tom, jestli je váš cluster nastavený ve virtuální síti, může být potřeba nastavit přesměrování portu tunelovým propojením přes přihlášení SSH, jak je vysvětleno níže:

### <a name="rserver-cluster-on-virtual-network"></a>Cluster R Serveru ve virtuální síti

Ověřte, že je na hraničním uzlu povolený provoz přes port 12800. Tímto způsobem můžete hraniční uzel použít pro připojení k funkci operacionalizace.

```
library(mrsdeploy)

remoteLogin(
    deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
    username = "admin",
    password = "xxxxxxx"
)
```

Pokud se metoda remoteLogin() nemůže připojit k hraničnímu uzlu, ale můžete se k němu připojit přes SSH, budete muset ověřit, jestli je správně nastavené pravidlo pro povolení provozu na portu 12800. Pokud bude tento problém přetrvávat, můžete jako alternativní řešení použít nastavení přesměrování portu tunelovým propojením přes SSH.

### <a name="rserver-cluster-not-set-up-on-virtual-network"></a>Cluster R Serveru nastavený mimo virtuální síť

Pokud váš cluster není nastavený ve virtuální síti NEBO máte potíže s připojením přes virtuální síť, můžete použít přesměrování portu tunelovým propojením přes SSH, jak je znázorněno níže:

```
ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net
```

V PuTTY to také můžete nastavit.

![připojení ssh v putty](./media/hdinsight-hadoop-r-server-get-started/putty.png)

Jakmile máte aktivní relaci SSH, provoz z portu 12800 vašeho počítače se prostřednictvím relace SSH přesměruje na port 12800 hraničního uzlu. Nezapomeňte v metodě remoteLogin() použít adresu `127.0.0.1:12800`. Tím se přihlásíte k operacionalizaci hraničního uzlu prostřednictvím přesměrování portu.

```
library(mrsdeploy)

remoteLogin(
    deployr_endpoint = "http://127.0.0.1:12800",
    username = "admin",
    password = "xxxxxxx"
)
```

## <a name="how-to-scale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a>Jak je možné škálovat výpočetní uzly operacionalizace Microsoft R Serveru na pracovní uzly HDInsight?
 
 
### <a name="decommission-the-worker-nodes"></a>Vyřazení pracovních uzlů z provozu
Microsoft R Server v současné době není spravován přes YARN. Pokud se pracovní uzly nevyřadí z provozu, správce prostředků YARN nebude fungovat podle očekávání, protože nebude vědět o prostředcích, které si server zabírá. Abyste tomu zabránili, doporučujeme vyřadit z provozu pracovní uzly, na které chcete škálovat výpočetní uzly.
 
Postup vyřazení pracovních uzlů z provozu:
 
* Přihlaste se ke konzole Ambari clusteru HDInsight a klikněte na kartu hosts (hostitelé).
* Vyberte pracovní uzly určené k vyřazení z provozu, klikněte na Actions (Akce) > Selected Hosts (Vybraní hostitelé) > Hosts (Hostitelé) a klikněte na Turn ON Maintenance Mode (Zapnout režim údržby). Například na následujícím obrázku jsme vybrali k vyřazení z provozu uzly wn3 a wn4.  
   
   ![vyřazení pracovních uzlů z provozu](./media/hdinsight-hadoop-r-server-get-started/get-started-operationalization.png)  
   
* Vyberte Actions (Akce) > Selected Hosts (Vybraní hostitelé) > DataNodes (Datové uzly) a klikněte na Decommission (Vyřadit z provozu).
* Vyberte Actions (Akce) > Selected Hosts (Vybraní hostitelé) > NodeManagers (Správci uzlů) a klikněte na Decommission (Vyřadit z provozu).
* Vyberte Actions (Akce) > Selected Hosts (Vybraní hostitelé) > DataNodes (Datové uzly) a klikněte na Stop (Zastavit).
* Vyberte Actions (Akce) > Selected Hosts (Vybraní hostitelé) > NodeManagers (Správci uzlů) a klikněte na Stop (Zastavit).
* Vyberte Actions (Akce) > Selected Hosts (Vybraní hostitelé) > Hosts (Hostitelé) a klikněte na Stop All Components (Zastavit všechny komponenty).
* Zrušte výběr pracovních uzlů a vyberte hlavní uzly.
* Vyberte Actions (Akce) > Selected Hosts (Vybraní hostitelé) > Hosts (Hostitelé) a klikněte na Restart All Components (Restartovat všechny komponenty).
 
 
###    <a name="configure-compute-nodes-on-each-decommissioned-worker-nodes"></a>Konfigurace výpočetních uzlů na všech vyřazených pracovních uzlech
 
* Přihlaste se přes SSH do každého vyřazeného pracovního uzlu.
* Spusťte nástroj pro správu pomocí příkazu `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.
* Zadáním „1“ vyberte možnost „1. Configure R Server for Operationalization“ (Konfigurovat R Server pro operacionalizaci).
* Zadáním „c“ vyberte možnost „C. Compute node“ (Výpočetní uzel). Tím se na pracovním uzlu nakonfiguruje výpočetní uzel.
* Ukončete nástroj pro správu
 
### <a name="add-compute-nodes-details-on-web-node"></a>Přidání podrobností o výpočetních uzlech do webového uzlu
Jakmile budou všechny vyřazené pracovní uzly nakonfigurované tak, aby na nich běžely výpočetní uzly, přejděte zpět do hraničního uzlu a přidejte IP adresy vyřazených pracovních uzlů do konfigurace webového uzlu Microsoft R Serveru:
 
* Připojte se přes SSH k hraničnímu uzlu.
* Spusťte `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`.
* Vyhledejte část „URIs“ (Identifikátory URI) a přidejte IP adresy a podrobnosti o portu pracovního uzlu.

![příkazový řádek vyřazení pracovních uzlů z provozu](./media/hdinsight-hadoop-r-server-get-started/get-started-op-cmd.png)

## <a name="next-steps"></a>Další kroky
Teď, když chápete, jak vytvořit nový cluster HDInsight obsahující R Server, a rozumíte základům používání konzoly R z relace SSH, si můžete přečíst následující témata popisující další způsoby práce s R Serverem ve službě HDInsight.

* [Přidání RStudio Serveru do služby HDInsight (pokud nebyl nainstalován během vytváření clusteru)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Možnosti výpočetního kontextu pro R Server ve službě HDInsight](hdinsight-hadoop-r-server-compute-contexts.md)
* [Možnosti služby Azure Storage pro R Server ve službě HDInsight](hdinsight-hadoop-r-server-storage.md)


