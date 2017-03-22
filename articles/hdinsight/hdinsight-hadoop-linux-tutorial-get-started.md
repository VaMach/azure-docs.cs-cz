---
title: "Kurz Hadoopu: Začínáme používat Hadoop a Hive v HDInsight | Dokumentace Microsoftu"
description: "Sledujte tento kurz a začněte s Hadoopem v HDInsight. Naučte se vytvářet linuxové clustery a dotazovat data pomocí Hive."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6a12ed4c-9d49-4990-abf5-0a79fdfca459
ms.service: hdinsight
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: d038245747ff8cfd2c80d7a7e0f05373ccb1e5fe
ms.lasthandoff: 03/11/2017


---
# <a name="hadoop-tutorial-get-started-using-hadoop-in-hdinsight"></a>Kurz Hadoopu: Začínáme používat Hadoop v HDInsight

Naučte se vytvářet clustery [Hadoop](http://hadoop.apache.org/) v HDInsight a spouštět úlohy Hive v HDInsight. [Hive Apache](https://hive.apache.org/) je nejoblíbenější součástí ekosystému Hadoop. V současné době je součástí služby HDInsight šest různých typů clusterů: [Hadoop](hdinsight-hadoop-introduction.md), [Spark](hdinsight-apache-spark-overview.md), [HBase](hdinsight-hbase-overview.md), [Storm](hdinsight-storm-overview.md), [Interaktivní Hive (Preview)](hdinsight-hadoop-use-interactive-hive.md) a [R server](hdinsight-hadoop-r-server-overview.md).  Každý typ clusteru podporuje odlišnou sadu komponent. Všech šest typů clusteru podporuje Hive. Seznam podporovaných součásti v HDInsight naleznete v tématu [Co je nového ve verzích clusterů Hadoop poskytovaných v HDInsight?](hdinsight-component-versioning.md)  

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít:

* **Předplatné Azure**: pro vytvoření bezplatného zkušebního účet na jeden měsíc otevřete web [azure.microsoft.com/free](https://azure.microsoft.com/free).

### <a name="access-control-requirements"></a>Požadavky na řízení přístupu
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-cluster"></a>Vytvoření clusteru

Většina úloh Hadoop jsou dávkové úlohy. Vytvoříte cluster, spustíte některé úlohy a pak cluster odstraníte. V této části vytvoříte cluster Hadoop ve službě HDInsight pomocí [šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md). Šablona Resource Manageru je plně přizpůsobitelná a umožňuje snadné vytváření prostředků Azure, jako například HDInsight. Zkušenosti s šablonou Resource Manageru nejsou pro postup dle tohoto kurzu vyžadovány. Další metody vytváření clusterů a principy vlastnosti používaných v tomto kurzu, naleznete v části [Vytváření clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Pomocí selektoru v horní části stránky vyberte možnosti vytvoření clusteru.

Šablona Resource Manageru použitá v tomto kurzu je umístěná v [Githubu](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/). 

1. Klikněte na následující obrázek pro přihlášení do Azure a otevřete šablonu Resource Manageru na webu Azure Portal. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-ssh-password%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Zadejte nebo vyberte tyto hodnoty:
   
    ![šablona Resource Manageru pro zahájení práce s HDInsight Linux na portálu](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-arm-template-on-portal.png).
   
    * **Předplatné**: Vyberte své předplatné Azure.
    * **Skupina prostředků**: Vytvořte novou skupinu prostředků nebo vyberte existující.  Skupina prostředků je kontejner komponent Azure.  V tomto případě skupina prostředků obsahuje cluster HDInsight a závislý účet služby Azure Storage. 
    * **Umístění**: Vyberte umístění Azure, ve kterém chcete cluster vytvořit.  Pro dosažení lepšího výkonu zvolte co nejbližší umístění. 
    * **Typ clusteru**: Pro účely tohoto kurzu vyberte **hadoop**.
    * **Název clusteru:** Zadejte název clusteru Hadoop.
    * **Přihlašovací jméno a heslo clusteru**: výchozí přihlašovací jméno je **admin**.
    * **Uživatelské jméno a heslo SSH**: výchozí uživatelské jméno **sshuser**.  Můžete ho změnit. 
     
    Některé vlastnosti jsou v šabloně pevně kódované.  Takové hodnoty můžete konfigurovat v šabloně.

    * **Umístění**: Umístění clusteru i závislý účet úložiště používají stejné umístění jako skupina prostředků.
    * **Verze clusteru:** 3.5
    * **Typ operačního systému**: Linux
    * **Počet pracovních uzlů**: 2

     Každý cluster obsahuje závislost účtu Azure Storage. Obvykle se označuje jako výchozí účet úložiště. Cluster HDInsight a jeho výchozí účet úložiště musí být umístěny společně a nacházet se ve stejné oblasti Azure. Odstraněním clusterů nedojde k odstranění účtu úložiště. 
     
     Podrobnější vysvětlení těchto vlastností naleznete v tématu [Vytváření clusterů Hadoop ve službě HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

3. Vyberte **Souhlasím s podmínkami a ujednáními uvedenými nahoře** a **Připnout na řídicí panel** a pak klikněte na **Koupit**. Na řídicím panelu portálu by se měla zobrazit nová dlaždice s názvem **Nasazení šablony**. Vytvoření clusteru trvá přibližně 20 minut. Jakmile je cluster vytvořen, popis dlaždice se změní na zadaný název skupiny prostředků. Na portálu se v novém okně automaticky otevře skupina prostředků. V seznamu bude uvedený cluster i výchozí úložiště.
   
    ![Počáteční skupina prostředků HDInsight Linux](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-resource-group.png).

4. Kliknutím na název clusteru otevřete cluster v novém okně.

   ![Počáteční nastavení clusteru HDInsight Linux](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-cluster-settings.png)


## <a name="run-hive-queries"></a>Spuštění dotazů Hive
[Apache Hive](hdinsight-use-hive.md) je nejoblíbenější součástí používanou v HDInsight. Existuje mnoho způsobů spouštění úloh Hive v HDInsight. V tomto kurzu použijete zobrazení Ambari Hive z portálu ke spuštění několika úloh Hive. Další metody pro odesílání úloh Hive naleznete v části [Použití Hive v HDInsight](hdinsight-use-hive.md).

1. Klikněte na **Řídicí panel clusteru** uvedený na předchozím snímku obrazovky a pak klikněte na **Řídicí panel clusteru HDInsight**.  Můžete také přejít na adresu **https://&lt;název_clusteru>.azurehdinsight.net**, kde &lt;název_clusteru> je název clusteru vytvořeného v předchozí části pro otevření Ambari.
2. Zadejte uživatelské jméno a heslo Hadoop, které jste zadali v předchozí části. Výchozí uživatelské jméno **admin**.
3. Otevřete **Zobrazení Hive**, jak je znázorněno na následujícím snímku obrazovky:
   
    ![Výběr zobrazení Ambari](./media/hdinsight-hadoop-linux-tutorial-get-started/selecthiveview.png).
4. V části **Editor dotazů** stránky vložte následující příkazy HiveQL do pracovního listu:
   
        SHOW TABLES;
   
   > [!NOTE]
   > Hive vyžaduje středník.       
   > 
   > 
5. Klikněte na tlačítko **Spustit**. Pod Editorem dotazů se musí zobrazit část **Výsledky zpracování dotazu** a informace o úloze. 
   
    Po dokončení dotazu se v části **Výsledky zpracování dotazu** zobrazí výsledky operace. Zobrazí jedna tabulka s názvem **hivesampletable**. Tato vzorová tabulka Hive obsahuje všechny clustery HDInsight.
   
    ![Zobrazení Hive HDInsight](./media/hdinsight-hadoop-linux-tutorial-get-started/hiveview.png).
6. Opakujte kroky 4 a 5 a spusťte následující dotaz:
   
        SELECT * FROM hivesampletable;
   
   > [!TIP]
   > Všimněte si rozevíracího seznamu **Uložit výsledky** v pravé horní části **Výsledky zpracování dotazu**. Tuto část můžete použít pro stažení výsledků nebo pro jejich uložení do úložiště HDInsight jako soubor CSV.
   > 
   > 
7. Klikněte na tlačítko **Historie** a načtěte seznam úloh.

Po dokončení úlohy Hive můžete [Exportovat výsledky do databáze Azure SQL nebo databáze systému SQL Server](hdinsight-use-sqoop-mac-linux.md), můžete také [zobrazit výsledky pomocí aplikace Excel](hdinsight-connect-excel-power-query.md). Další informace o používání Hive v HDInsight naleznete v části [Použití Hive a HiveQL s Hadoop v HDInsight k analýze ukázkového souboru Apache log4j](hdinsight-use-hive.md).

## <a name="clean-up-the-tutorial"></a>Vyčistěte kurz
Po dokončení kurzu můžete cluster odstranit. Pomocí HDInsight jsou vaše data uložena v Azure Storage, takže můžete clusteru bezpečně odstranit, pokud není používán. Za cluster služby HDInsight se účtují poplatky, i když se nepoužívá. Vzhledem k tomu, že poplatky za cluster představují několikanásobek poplatků za úložiště, dává ekonomický smysl odstraňovat clustery, které nejsou používány. 

> [!NOTE]
> Pomocí [Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) můžete s HDInsight vytvářet clustery na vyžádání a nakonfigurovat nastavení TimeToLive na automatické mazání clusterů. 
> 
> 

**Postup odstranění clusteru a/nebo výchozího účtu úložiště**

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V ovládacím panelu portálu klikněte na dlaždici s názvem skupiny prostředků, které jste použili při vytvoření clusteru.
3. V okně prostředků klikněte na tlačítko **Odstranit** a odstraňte skupinu prostředků, která obsahuje clusteru a výchozí účet úložiště nebo klikněte na název clusteru v dlaždici **Prostředky** a pak klikněte na tlačítko **Odstranit** v okně clusteru. Vezměte na vědomí, že odstraněním skupiny prostředků odstraníte účet úložiště. Pokud chcete zachovat účet úložiště, zvolte odstranění samotného clusteru.

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili, jak vytvořit cluster HDInsight se systémem Linux pomocí šablony Resource Manageru, a jak provádět základní dotazy Hive.

Další informace o analýze dat pomocí HDInsight naleznete v následujících tématech:

* Další informace o používání Hivu se službou HDInsight, včetně postupu provádění dotazů Hivu ze sady Visual Studio, najdete v tématu [Použití Hivu se službou HDInsight][hdinsight-use-hive].
* Další informace o jazyce Pig používaném k transformaci dat najdete v tématu [Použití Pigu se službou HDInsight][hdinsight-use-pig].
* Další informace o MapReduce, způsobu psaní programů, které zpracovávají data v Hadoopu, najdete v tématu [Použití MapReduce se službou HDInsight][hdinsight-use-mapreduce].
* Další informace o použití nástrojů HDInsight pro Visual Studio k analýze dat na HDInsight naleznete v části [Začněte používat nástroje Visual Studio Hadoop pro HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md).

Pokud jste připraveni začít pracovat s vlastními daty a potřebujete vědět více o způsobu, jakým HDInsight ukládá data nebo jak načítá data do HDInsight, projděte si následující témata:

* Informace o tom, jak HDInsight používá Azure Storage, najdete v tématu [Používání Azure Storage s HDInsight](hdinsight-hadoop-use-blob-storage.md).
* Informace o tom, jak nahrát data do služby HDInsight, najdete v tématu [Nahrání dat do služby HDInsight][hdinsight-upload-data].

Pokud chcete získat další informace o vytváření a správě clusteru služby HDInsight, projděte si následující témata:

* Další informace o správě clusteru HDInsight se systémem Linux naleznete v části [Správa clusterů HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md).
* Další informace o možnostech, které můžete vybrat při vytváření clusteru služby HDInsight, naleznete v tématu [Vytváření HDInsight na Linuxu pomocí vlastních možností](hdinsight-hadoop-provision-linux-clusters.md).
* Je-li obeznámeni s Linux a Hadoop, ale chcete znát podrobnosti o Hadoop na HDInsight, prostudujte si část [Práce s HDInsight v systému Linux](hdinsight-hadoop-linux-information.md). Téma obsahuje informace o:
  
  * Adresách URL služeb hostovaných v clusteru, například Ambari a WebHCat
  * Umístění souborů Hadoop a příkladech v místním systému souborů
  * Používání Azure Storage (WASB) namísto HDFS jako výchozího datového úložiště

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md



