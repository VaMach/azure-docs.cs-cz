<properties
    pageTitle="Kurz Linux: Začínáme s Hadoop a Hive | Microsoft Azure"
    description="Sledujte tento kurz Linuxu a začněte s Hadoop v HDInsight. Naučte se vytvářet Linuxové clustery a dotazovat data pomocí Hive."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/14/2016"
    ms.author="jgao"/>


# Kurz Hadoopu: začněte používat systém Hadoop pro Linux v HDInsight

> [AZURE.SELECTOR]
- [Založený na Linuxu](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Založený na systému Windows](hdinsight-hadoop-tutorial-get-started-windows.md)

Naučte se vytvářet linuxové clustery [Hadoop](http://hadoop.apache.org/) v HDInsight a spouštět úlohy Hive v HDInsight. [Hive Apache](https://hive.apache.org/) je nejoblíbenější součástí ekosystému Hadoop. V současnosti se HDInsight dodává se 4 různými typy clusterů: [Hadoop](hdinsight-hadoop-introduction.md), [Spark](hdinsight-apache-spark-overview.md), [HBase](hdinsight-hbase-overview.md) a [Storm](hdinsight-storm-overview.md).  Každý typ clusteru podporuje odlišnou sadu komponent. Všechny 4 typy clusteru podporují Hive. Seznam podporovaných součásti v HDInsight naleznete v tématu [Co je nového ve verzích clusterů Hadoop poskytovaných v HDInsight?](hdinsight-component-versioning.md)  

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## Požadavky

Než začnete tento kurz, musíte mít:

- **Předplatné Azure**: pro vytvoření bezplatného zkušebního účet na jeden měsíc otevřete web [azure.microsoft.com/free](https://azure.microsoft.com/free).

### Požadavky na řízení přístupu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## Vytvoření clusteru

Většina úloh Hadoop jsou dávkové úlohy. Vytvoříte cluster, spustíte některé úlohy a pak cluster odstraníte. V této části vytvoříte linuxový cluster Hadoop v HDInsight pomocí [šablony Azure Resource Manageru](../resource-group-template-deploy.md). Šablona Resource Manageru je plně přizpůsobitelná a umožňuje snadné vytváření prostředků Azure, jako například HDInsight. Zkušenosti s šablonou Resource Manageru nejsou pro postup dle tohoto kurzu vyžadovány. Další metody vytváření clusterů a principy vlastnosti používaných v tomto kurzu, naleznete v části [Vytváření clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Šablona Resource Manageru používaná v tomto kurzu se nachází ve veřejném kontejneru objektu blob [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight.json). 

1. Klikněte na následující obrázek pro přihlášení do Azure a otevřete šablonu Resource Manageru na portálu Azure. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hadoop-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Z okna **Parametry** zadejte následující údaje:

    ![šablona Resource Manageru pro zahájení práce s HDInsight Linux na portálu](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-arm-template-on-portal.png).

    - **Název clusteru**: Zadejte název pro cluster Hadoop, který chcete vytvořit.
    - **Přihlašovací jméno a heslo clusteru**: výchozí přihlašovací jméno je **admin**.
    - **Uživatelské jméno a heslo SSH**: výchozí uživatelské jméno **sshuser**.  Můžete ho změnit. 
    
    Další parametry jsou volitelné podle tohoto kurzu. Můžete je nechat, jak jsou. 
    
    Každý cluster obsahuje závislost účtu úložiště Azure Blob. Obvykle se označuje jako výchozí účet úložiště. Cluster HDInsight a jeho výchozí účet úložiště musí být umístěny společně a nacházet se ve stejné oblasti Azure. Odstraněním clusterů nedojde k odstranění účtu úložiště. V šabloně je výchozí název účtu úložiště definován jako název clusteru s připojeným názvem „úložiště“. 
    
3. Klikněte na možnost **OK** a uložte parametry.
4. V okně **Vlastní nasazení** zadejte **Nový název skupiny prostředků**, chcete-li vytvořit novou skupinu prostředků.  Skupina prostředků je kontejner, který seskupuje cluster, účet závislého úložiště a další prostředky. Umístění skupiny prostředků se může lišit od umístění v clusteru.
5. Klikněte na tlačítko **Smluvní podmínky** a pak klikněte na tlačítko **Vytvořit**.
6. Ověřte zaškrtnutí políčka **Kód pin pro řídicí panel** a pak klikněte na tlačítko **Vytvořit**. Zobrazí se nová dlaždice s názvem **Nasazení šablony**. Vytvoření clusteru trvá přibližně 20 minut. 
7.  Jakmile je cluster vytvořen, popis dlaždice se změní na zadaný název skupiny prostředků. A portálu automaticky otevře dvě okna s clusterem a nastavením clusteru. 

    ![Počáteční nastavení clusteru HDInsight Linux](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-cluster-settings.png).

    Existují dva uvedené prostředky, cluster a výchozí účet úložiště.

##Spuštění dotazů Hive

[Apache Hive](hdinsight-use-hive.md) je nejoblíbenější součástí používanou v HDInsight. Existuje mnoho způsobů spouštění úloh Hive v HDInsight. V tomto kurzu použijete zobrazení Ambari Hive z portálu ke spuštění několika úloh Hive. Další metody pro odesílání úloh Hive naleznete v části [Použití Hive v HDInsight](hdinsight-use-hive.md).

1. Přejděte na adresu **https://&lt;ClusterName>.azurehdinsight.net**, kde &lt;ClusterName> je název clusteru vytvořeného v předchozí části pro otevření Ambari.
2. Zadejte uživatelské jméno a heslo Hadoop, které jste zadali v předchozí části. Výchozí uživatelské jméno **admin**.
3. Otevřete **Zobrazení Hive**, jak je znázorněno na následujícím snímku obrazovky:

    ![Výběr zobrazení Ambari](./media/hdinsight-hadoop-linux-tutorial-get-started/selecthiveview.png).
4. V části __Editor dotazů__ stránky vložte následující příkazy HiveQL do pracovního listu:

        SHOW TABLES;

    >[AZURE.NOTE] Hive vyžaduje středník.       
        
5. Klikněte na tlačítko __Spustit__. Pod Editorem dotazů se musí zobrazit část __Výsledky zpracování dotazu__ a informace o úloze. 

    Po dokončení dotazu se v části __Výsledky zpracování dotazu__ zobrazí výsledky operace. Zobrazí jedna tabulka s názvem **hivesampletable**. Tato vzorová tabulka Hive obsahuje všechny clustery HDInsight.

    ![Zobrazení Hive HDInsight](./media/hdinsight-hadoop-linux-tutorial-get-started/hiveview.png).

6. Opakujte kroky 4 a 5 a spusťte následující dotaz:

        SELECT * FROM hivesampletable;

    > [AZURE.TIP] Všimněte si rozevíracího seznamu __Uložit výsledky__ v pravé horní části __Výsledky zpracování dotazu__. Tuto část můžete použít pro stažení výsledků nebo pro jejich uložení do úložiště HDInsight jako soubor CSV.

7. Klikněte na tlačítko **Historie** a načtěte seznam úloh.

Po dokončení úlohy Hive můžete [Exportovat výsledky do databáze Azure SQL nebo databáze systému SQL Server](hdinsight-use-sqoop-mac-linux.md), můžete také [zobrazit výsledky pomocí aplikace Excel](hdinsight-connect-excel-power-query.md). Další informace o používání Hive v HDInsight naleznete v části [Použití Hive a HiveQL s Hadoop v HDInsight k analýze ukázkového souboru Apache log4j](hdinsight-use-hive.md).

##Vyčistěte kurz

Po dokončení kurzu můžete cluster odstranit. Pomocí HDInsight jsou vaše data uložena v Azure Storage, takže můžete clusteru bezpečně odstranit, pokud není používán. Za cluster služby HDInsight se účtují poplatky, i když se nepoužívá. Vzhledem k tomu, že poplatky za cluster představují několikanásobek poplatků za úložiště, dává ekonomický smysl odstraňovat clustery, které nejsou používány. 

>[AZURE.NOTE] Pomocí [Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) můžete s HDInsight vytvářet clustery na vyžádání a nakonfigurovat nastavení TimeToLive na automatické mazání clusterů. 

**Postup odstranění clusteru a/nebo výchozího účtu úložiště**

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. V ovládacím panelu portálu klikněte na dlaždici s názvem skupiny prostředků, které jste použili při vytvoření clusteru.
3. V okně prostředků klikněte na tlačítko **Odstranit** a odstraňte skupinu prostředků, která obsahuje clusteru a výchozí účet úložiště nebo klikněte na název clusteru v dlaždici **Prostředky** a pak klikněte na tlačítko **Odstranit** v okně clusteru. Vezměte na vědomí, že odstraněním skupiny prostředků odstraníte účet úložiště. Pokud chcete zachovat účet úložiště, zvolte odstranění samotného clusteru.

## Další kroky

V tomto kurzu jste se naučili, jak vytvořit cluster HDInsight se systémem Linux pomocí šablony Resource Manageru, a jak provádět základní dotazy Hive.

Další informace o analýze dat pomocí HDInsight naleznete v následujících tématech:

- Další informace o používání Hive s HDInsight, včetně postupu, jak provádět dotazy Hive ze sady Visual Studio naleznete v části [Používání Hive s HDInsight][hdinsight-use-hive].

- Další informace o jazyce Pig používaném k transformaci dat naleznete v části [Používání Pig s HDInsight][hdinsight-use-pig].

- Další informace o MapReduce, způsobu jak psát programy, které zpracovávají data v Hadoop, naleznete v části [Používání MapReduce s HDInsight][hdinsight-use-mapreduce].

- Další informace o použití nástrojů HDInsight pro Visual Studio k analýze dat na HDInsight naleznete v části [Začněte používat nástroje Visual Studio Hadoop pro HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md).

Pokud jste připraveni začít pracovat s vlastními daty a potřebujete vědět více o způsobu, jakým HDInsight ukládá data nebo jak načítá data do HDInsight, projděte si následující témata:

- Informace o používání úložiště objektů blob Azure HDInsight naleznete v tématu [Používání úložiště Azure Blob s HDInsight](hdinsight-hadoop-use-blob-storage.md).

- Informace o tom, jak nahrát data do HDInsight naleznete v tématu [Odesílání dat do HDInsight][hdinsight-upload-data].

Pokud chcete získat další informace o vytváření a správě clusteru služby HDInsight, projděte si následující témata:

- Další informace o správě clusteru HDInsight se systémem Linux naleznete v části [Správa clusterů HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md).

- Další informace o možnostech, které můžete vybrat při vytváření clusteru služby HDInsight, naleznete v tématu [Vytváření HDInsight na Linuxu pomocí vlastních možností](hdinsight-hadoop-provision-linux-clusters.md).

- Je-li obeznámeni s Linux a Hadoop, ale chcete znát podrobnosti o Hadoop na HDInsight, prostudujte si část [Práce s HDInsight v systému Linux](hdinsight-hadoop-linux-information.md). Téma obsahuje informace o:

    * Adresách URL služeb hostovaných v clusteru, například Ambari a WebHCat
    * Umístění souborů Hadoop a příkladech v místním systému souborů
    * Používání Azure Storage (WASB) namísto HDFS jako výchozího datového úložiště


[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[powershell-download]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[powershell-install-configure]: powershell-install-configure.md
[powershell-open]: powershell-install-configure.md#Install

[img-hdi-dashboard]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.png
[img-hdi-dashboard-query-select]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.png
[img-hdi-dashboard-query-select-result]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.png
[img-hdi-dashboard-query-select-result-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.output.png
[img-hdi-dashboard-query-browse-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.browse.output.png
[image-hdi-clusterstatus]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.ClusterStatus.png
[image-hdi-gettingstarted-powerquery-importdata]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData.png
[image-hdi-gettingstarted-powerquery-importdata2]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData2.png



<!--HONumber=Sep16_HO5-->


