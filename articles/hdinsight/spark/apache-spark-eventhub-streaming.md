---
title: "Použití Apache Spark streamování pomocí Event Hubs v Azure HDInsight | Microsoft Docs"
description: "Sestavení Apache Spark streamování ukázku odesílat datový proud do centra událostí Azure a pak přijímat události v clusteru HDInsight Spark pomocí scala aplikace."
keywords: "Apache spark streamování, vysílání datového proudu spark, ukázka spark, apache spark streamování například ukázka azure centra událostí, spark ukázka"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 68894e75-3ffa-47bd-8982-96cdad38b7d0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: d0678388fea79797c3cb4cd84deeab827981ebff
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/09/2017
---
# <a name="apache-spark-streaming-process-data-from-azure-event-hubs-with-spark-cluster-on-hdinsight"></a>Apache Spark streamování: clusteru zpracování dat z Azure Event Hubs s Spark v HDInsight

V tomto článku vytvořit Apache Spark streamování vzorku, který zahrnuje následující kroky:

1. Použijete samostatné aplikace k ingestování zpráv do centra událostí Azure.

2. S dva různé přístupy můžete načítat zprávy z centra událostí v reálném čase pomocí aplikace běžící v clusteru Spark v Azure HDInsight.

3. Vytvoření datových proudů analytické kanály pro uložení dat do jiného úložiště systémů nebo získáte přehledy z dat za chodu.

## <a name="prerequisites"></a>Požadavky

* Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="spark-streaming-concepts"></a>Vysílání datového proudu Spark koncepty

Podrobné vysvětlení vysílání datového proudu Spark, najdete v článku [vysílání datového proudu přehled Apache Spark](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview). HDInsight přináší stejné součásti pro datové proudy pro cluster Spark v Azure.  

## <a name="what-does-this-solution-do"></a>Jakým způsobem tohoto řešení?

V tomto článku Pokud chcete vytvořit vysílání datového proudu příklad Spark, proveďte následující kroky:

1. Vytvoření centra událostí Azure, která bude přijímat události datového proudu.

2. Spusťte místní samostatná aplikace, která generuje události a doručí do centra událostí Azure. Ukázkové aplikace, k tomu je publikována v [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).

3. Vzdálené spuštění aplikace na streamování cluster Spark, který čte streamování událostí z centra událostí Azure a provádět různé zpracování dat nebo analýzu.

## <a name="create-an-azure-event-hub"></a>Vytvoření centra událostí Azure

1. Přihlaste se k [portálu Azure](https://ms.portal.azure.com)a klikněte na tlačítko **nový** v levém horním rohu obrazovky.

2. Klikněte na **Internet věcí** a pak na **Event Hubs**.

    ![Centra událostí vytvořit pro příklad vysílání datového proudu Spark](./media/apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "centra událostí vytvořit pro příklad vysílání datového proudu Spark")

3. V okně **Vytvořit obor názvů** zadejte název oboru názvů. Vyberte cenovou úroveň (Basic nebo Standard). Zvolte také předplatné Azure, skupinu prostředků a umístění, ve kterém se má prostředek vytvořit. Kliknutím na **Vytvořit** vytvoříte obor názvů.

      ![Zadejte název centra událostí příklad vysílání datového proudu Spark](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "zadejte název centra událostí příklad vysílání datového proudu Spark")

    > [!NOTE]
    > Měli byste vybrat stejné **umístění** jako cluster Apache Spark v HDInsight a snižuje tak latenci a náklady.
    >
    >

4. V seznamu oborů názvů Event Hubs klikněte na nově vytvořený obor názvů.      


5. V okně obor názvů, klikněte na tlačítko **Event Hubs**a potom klikněte na **+ centra událostí** k vytvoření nového centra událostí.
   
    ![Centra událostí vytvořit pro příklad vysílání datového proudu Spark](./media/apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "centra událostí vytvořit pro příklad vysílání datového proudu Spark")

6. Zadejte název pro vaše Centrum událostí, nastavte počet oddílů na 10 a uchování zpráv na 1. Abyste mohli Ponechejte zbývající jako výchozí a pak klikněte na tlačítko jsme nejsou archivace zpráv v tomto řešení **vytvořit**.
   
    ![Zadejte podrobnosti o události rozbočovače pro příklad vysílání datového proudu Spark](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "zadejte podrobnosti o události rozbočovače pro příklad vysílání datového proudu Spark")

7. Nově vytvořený Centrum událostí je uvedena v okně centra událostí.
    
     ![Zobrazit centra událostí pro vysílání datového proudu příklad Spark](./media/apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "Centrum událostí zobrazení vysílání datového proudu příklad Spark")

8. Po návratu do okna oboru názvů (ne v okně konkrétního centra událostí) klikněte na **Zásady sdíleného přístupu** a poté klikněte na **RootManageSharedAccessKey**.
    
     ![Nastavení centra událostí zásad pro vysílání datového proudu příklad Spark](./media/apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "zásady nastavení centra událostí pro vysílání datového proudu příklad Spark")

9. Klikněte na tlačítko Kopírovat zkopírovat **RootManageSharedAccessKey** primární klíč a připojovací řetězec do schránky. Uložte do použít později v tomto kurzu.
    
     ![Zobrazit centra událostí zásad klíče pro vysílání datového proudu příklad Spark](./media/apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "centra událostí zobrazení zásad klíče pro streamování příklad Spark")

## <a name="send-messages-to-azure-event-hub-using-a-sample-scala-application"></a>Odeslání zprávy do centra událostí Azure pomocí Scala ukázkové aplikace

V této části použijte samostatná místní Scala aplikace, která generuje datového proudu událostí a odešle ji do centra událostí Azure, kterou jste vytvořili dříve. Tato aplikace je k dispozici na webu GitHub na [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer). Zde postup předpokládá, že jste již forked toto úložiště GitHub.

1. Ujistěte se, že máte k dispozici následující nainstalovaná na počítači, kde spuštění této aplikace.

    * Java Development kit Oracle. Můžete nainstalovat z [zde](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
    * Apache Maven. Si můžete stáhnout z [zde](https://maven.apache.org/download.cgi). Pokyny k instalaci Maven jsou k dispozici [zde](https://maven.apache.org/install.html).

2. Otevřete příkazový řádek a přejděte do umístění, které jste naklonovali úložiště GitHub pro ukázkovou aplikaci Scala a spusťte následující příkaz pro sestavení aplikace.

        mvn package

3. Výstup jar pro aplikaci, **com-microsoft-azure-eventhubs-client-example-0.2.0.jar**, se vytvořil v rámci **/target** adresáře. Tato JAR později v tomto článku použijete k testování úplného řešení.

## <a name="create-application-to-receive-messages-from-event-hub-into-a-spark-cluster"></a>Vytvoření aplikace pro příjem zpráv z centra událostí do clusteru Spark 

Máme dva přístupy k vysílání datového proudu Spark a Azure Event Hubs, na základě příjemce připojení a připojení na základě přímo DStream připojit. Na základě přímo DStream byla zavedená v ledna 2017 v 2.0.3 vydání. Je předpokládané nahradit původní připojení na základě příjemce, protože to je další původce a efektivní prostředků. Další informace uvedené v [https://github.com/hdinsight/spark-eventhubs](https://github.com/hdinsight/spark-eventhubs). Přímé DStream podporuje pouze Spark 2.0 +.

### <a name="build-applications-with-the-dependency-to-spark-eventhubs-connector"></a>Vytváření aplikací s závislostí do spark eventhubs konektoru

Pracovní verze Spark EventHubs jsme také bude publikovat na webu GitHub. Pokud chcete použít pracovní verze Spark EventHubs, první krok je určit Githubu jako zdrojové úložiště přidáním následující položku na pom.xml:

```xml
<repository>
      <id>spark-eventhubs</id>
      <url>https://raw.github.com/hdinsight/spark-eventhubs/maven-repo/</url>
      <snapshots>
        <enabled>true</enabled>
        <updatePolicy>always</updatePolicy>
      </snapshots>
</repository>
```

Potom můžete přidat následující závislost do projektu provést předběžné verze.

Závislost maven

```xml
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11 -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spark-streaming-eventhubs_2.11</artifactId>
    <version>2.0.4</version>
</dependency>
```

SBT závislostí

```
// https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11
libraryDependencies += "com.microsoft.azure" % "spark-streaming-eventhubs_2.11" % "2.0.4"
```

### <a name="direct-dstream-connection"></a>Přímé připojení DStream

Jar předem připraveného souboru, který obsahuje příklady použití přímé DStream lze stáhnout v [http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar](http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar).

Na soubor jar obsahuje tři příklady, jejichž zdrojového kódu jsou k dispozici na [https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream](https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream).

Pořízení [WindowingWordCount](https://github.com/hdinsight/spark-eventhubs/blob/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream/WindowingWordCount.scala) jako příklad:

```scala
private def createStreamingContext(
  sparkCheckpointDir: String,
  batchDuration: Int,
  namespace: String,
  eventHunName: String,
  eventhubParams: Map[String, String],
  progressDir: String) = {
val ssc = new StreamingContext(new SparkContext(), Seconds(batchDuration))
ssc.checkpoint(sparkCheckpointDir)
val inputDirectStream = EventHubsUtils.createDirectStreams(
  ssc,
  namespace,
  progressDir,
  Map(eventHunName -> eventhubParams))

inputDirectStream.map(receivedRecord => (new String(receivedRecord.getBody), 1)).
  reduceByKeyAndWindow((v1, v2) => v1 + v2, (v1, v2) => v1 - v2, Seconds(batchDuration * 3),
    Seconds(batchDuration)).print()

ssc
}

def main(args: Array[String]): Unit = {

if (args.length != 8) {
  println("Usage: program progressDir PolicyName PolicyKey EventHubNamespace EventHubName" +
    " BatchDuration(seconds) Spark_Checkpoint_Directory maxRate")
  sys.exit(1)
}

val progressDir = args(0)
val policyName = args(1)
val policykey = args(2)
val namespace = args(3)
val name = args(4)
val batchDuration = args(5).toInt
val sparkCheckpointDir = args(6)
val maxRate = args(7)

val eventhubParameters = Map[String, String] (
  "eventhubs.policyname" -> policyName,
  "eventhubs.policykey" -> policykey,
  "eventhubs.namespace" -> namespace,
  "eventhubs.name" -> name,
  "eventhubs.partition.count" -> "32",
  "eventhubs.consumergroup" -> "$Default",
  "eventhubs.maxRate" -> s"$maxRate"
)

val ssc = StreamingContext.getOrCreate(sparkCheckpointDir,
  () => createStreamingContext(sparkCheckpointDir, batchDuration, namespace, name,
    eventhubParameters, progressDir))

ssc.start()
ssc.awaitTermination()
}
```

V předchozím příkladu `eventhubParameters` jsou specifické pro jednu instanci EventHubs parametry a budete muset předejte jej `createDirectStreams` rozhraní API, která vytvoří přímé DStream objekt mapování do oboru názvů služby Event Hubs. Prostřednictvím přímé DStream objekt můžete volat jakéhokoli DStream rozhraní API poskytované framework API vysílání datového proudu Spark. V tomto příkladu jsme vypočítat frekvenci jednotlivých slov v rámci poslední 3 malých batch intervalů.

### <a name="receiver-based-connection"></a>Na základě příjemce připojení

Spark, streamování ukázková aplikace napsané v jazyce Scala, která přijímá události a směrovat na různé cíle, je k dispozici na [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples). Použijte následující postup a aktualizovat aplikaci pro konfiguraci centra událostí vytvořit jar výstup.

1. Spusťte IntelliJ IDEA a úvodní obrazovka Vyberte **rezervovat z verzí** a pak klikněte na **Git**.
   
    ![Apache Spark streamování příklad - get zdroje z Git](./media/apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark streamování příklad - get zdroje z Git")

2. V **klonování úložiště** dialogovém okně zadejte adresu URL do úložiště Git clone z, zadejte adresář pro klonování, a potom klikněte na **klon**.
   
    ![Apache Spark streamování příklad - klonování z Git](./media/apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark streamování příklad - klonování z Git")
3. Postupujte podle pokynů, dokud je zcela klonovat projektu. Stiskněte klávesu **Alt + 1** otevřete **zobrazení projektu**. Měl by vypadat jako následující.
   
    ![Apache Spark streamování příklad: zobrazení projektu](./media/apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark streamování příklad: zobrazení projektu")
4. Ujistěte se, že je s Java8 zkompilován kódu aplikace. Aby, klikněte na tlačítko **soubor**, klikněte na tlačítko **strukturu projektu**a na **projektu** , zkontrolujte, zda projekt jazyka level nastavená na **8 - Lambdas, typu poznámky atd**.
   
    ![Apache Spark streamování příklad - Set kompilátoru](./media/apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark streamování příklad - Set kompilátoru")
5. Otevřete **pom.xml** a zkontrolujte, zda je správná verze Spark. V části `<properties>` uzlu, vyhledejte následující fragment kódu a ověřit verzi Spark.

        <scala.version>2.11.8</scala.version>
        <scala.compat.version>2.11.8</scala.compat.version>
        <scala.binary.version>2.11</scala.binary.version>
        <spark.version>2.0.0</spark.version>

6. Aplikace vyžaduje závislostí jar, nazývá **jar ovladač JDBC**. To se vyžaduje k zápisu zprávy přijaté z centra událostí do Azure SQL database. Můžete si stáhnout tuto jar (v4.1 nebo novější) z [zde](https://msdn.microsoft.com/sqlserver/aa937724.aspx). Přidáte odkaz na tento jar do projektu knihovny. Proveďte následující kroky:
     
     1. Z okna IntelliJ IDEA, kdy máte aplikaci otevřít, klikněte na tlačítko **soubor**, klikněte na tlačítko **strukturu projektu**a potom klikněte na **knihovny**. 
     2. Klikněte na ikonu Přidat (![ikonu Přidat](./media/apache-spark-eventhub-streaming/add-icon.png)), klikněte na tlačítko **Java**a pak přejděte do umístění, kam jste stáhli jar ovladač JDBC. Postupujte podle pokynů pro přidání na soubor jar do projektu knihovny.

         ![přidejte chybějící závislosti](./media/apache-spark-eventhub-streaming/add-missing-dependency-jars.png "přidejte chybějící závislost JAR")
     3. Klikněte na tlačítko **Použít**.

7. Vytvoření výstupního souboru jar. Proveďte následující kroky.

   1. V **strukturu projektu** dialogové okno, klikněte na tlačítko **artefakty** a pak klikněte na plus symbol. V dialogovém okně automaticky otevírané okno klikněte na **JAR**a potom klikněte na **z modulů se závislostmi**.      
       
       ![Apache Spark streamování příklad - vytvořit JAR](./media/apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "Apache Spark streamování příklad - vytvořit JAR")
   2. V **vytvořit JAR z modulů** dialogové okno pole, klikněte na tlačítko se třemi tečkami (![třemi tečkami](./media/apache-spark-eventhub-streaming/ellipsis.png)) proti **hlavní třídy**.
   3. V **vyberte třídu hlavní** dialogové okno pole, vyberte některou z dostupných tříd a pak klikněte na tlačítko **OK**.
      
       ![Apache Spark streamování příklad - vyberte třídu pro jar](./media/apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark streamování příklad - vyberte třídu pro jar")
   4. V **vytvořit JAR z modulů** dialogové okno pole, ujistěte se, že možnost **extrahovat k cíli JAR** je vybrána a potom klikněte na **OK**. Tím se vytvoří jeden JAR s všechny závislosti.
      
       ![Apache Spark streamování příklad - vytvořit jar z modulů](./media/apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "Apache Spark streamování příklad - vytvořit jar z modulů")
   5. **Výstup rozložení** karta Vypíše seznam všech JAR, které jsou součástí na projekt Maven. Můžete vybrat a odstranit ty, na kterém aplikace Scala má žádné přímé závislost. Pro aplikaci tady vytváříme, můžete odebrat všechny uzly s výjimkou poslední (**spark streamování data trvalost příklady zkompilovat výstup**). Vyberte JAR odstranit a potom klikněte na **odstranit** ikona (![odstranit ikonu](./media/apache-spark-eventhub-streaming/delete-icon.png)).
      
       ![Apache Spark streamování Příklad - odstranění extrahovat JAR](./media/apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark streamování Příklad - odstranění extrahovat JAR")
      
       Zajistěte, aby **sestavení na Ujistěte se,** je políčko zaškrtnuto, který zajistí, aby jar vytvořit pokaždé, když je vytvořené nebo aktualizované projektu. Klikněte na tlačítko **Použít**.
   6. V **výstup rozložení** kartě přímo v dolní části **dostupné elementy** pole, máte jar SQL JDBC, který jste dříve přidali do projektu knihovny. Je nutné přidat do **výstup rozložení** kartě. Klikněte pravým tlačítkem na soubor jar a pak klikněte na tlačítko **extrahovat do kořenové výstup**.
      
       ![Apache Spark streamování příklad - extract závislostí jar](./media/apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark streamování příklad - jar závislostí extrakce")  
      
       **Výstup rozložení** karta by teď měl vypadat takto.
      
       ![Apache Spark streamování příklad - karta finální výstup](./media/apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark streamování příklad - karta finální výstup")        
      
       V **strukturu projektu** dialogové okno, klikněte na tlačítko **použít** a pak klikněte na **OK**.    
   7. V řádku nabídek klikněte na **sestavení**a potom klikněte na **zkontrolujte projektu**. Můžete také kliknout na **sestavení artefaktů** vytvořit jar. Výstup jar se vytvořil v rámci **\classes\artifacts**.
      
       ![Apache Spark streamování příklad – výstupní JAR](./media/apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "streamování příklad Apache Spark – výstupní JAR")

## <a name="run-the-application-remotely-on-a-spark-cluster-using-livy"></a>Vzdálené spuštění aplikace na clusteru Spark pomocí Livy

V tomto článku pomocí Livy ke spuštění Apache Spark streamování aplikací vzdáleně na clusteru Spark. Podrobné informace o tom, jak používat Livy s clusterem HDInsight Spark, naleznete v části [clusteru odeslání úloh vzdáleně Apache Spark v Azure HDInsight](apache-spark-livy-rest-interface.md). Předtím, než můžete začít spouštět vysílání datového proudu aplikací Spark, existuje několik věcí, které byste měli udělat:

1. Spustit místní samostatnou aplikaci generovat události a odesílat do centra událostí. Uděláte to tak, použijte následující příkaz:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "mysbnamespace" --eventhubs-name "myeventhub" --policy-name "mysendpolicy" --policy-key "<policy key>" --message-length 32 --thread-count 32 --message-count -1

2. Zkopírujte streamování jar (**spark streamování data trvalost examples.jar**) do úložiště objektů Azure Blob, který je přidružen ke clusteru. Díky tomu jar pro Livy dostupné. Můžete použít [ **AzCopy**](../../storage/common/storage-use-azcopy.md), nástroj příkazového řádku, tak. Existuje mnoho dalších klientů, které můžete použít k nahrání data. Můžete najít další informace o nich v [nahrávání dat pro úlohy Hadoop do HDInsight](../hdinsight-upload-data.md).
3. Nainstalujte na počítače, kde běží tyto aplikace z CURL. Používáme CURL k vyvolání Livy koncových bodů ke spouštění úloh na dálku.

### <a name="run-the-spark-streaming-application-to-receive-the-events-into-an-azure-storage-blob-as-text"></a>Spustit Spark streamování aplikace na příjem událostí do objektu Blob úložiště Azure jako text

Otevřete příkazový řádek, přejděte do adresáře, kam jste nainstalovali CURL a spusťte následující příkaz (uživatelské jméno a heslo a cluster nahraďte název):

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputBlob.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Parametry v souboru **inputBlob.txt** jsou definovány takto:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsEventCount", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Dejte nám vědět, co parametry ve vstupním souboru jsou:

* **soubor** je cesta k souboru aplikace jar na účet úložiště Azure přidružený ke clusteru.
* **Název třídy** je název třídy ve jar.
* **argumentů** je seznam argumentů, které vyžadují třídy
* **numExecutors** je počet jader používané Spark ke spuštění streamování aplikace. To by měl být vždy alespoň dvakrát počet oddílů centra událostí.
* **executorMemory**, **executorCores**, **driverMemory** jsou parametry sloužící k přidělování požadované prostředky streamování aplikace.

> [!NOTE]
> Není nutné k vytvoření výstupní složky (EventCheckpoint, EventCount/EventCount10), které se používají jako parametry. Je vytváří streamování aplikace.
>
>

Když spustíte příkaz, byste měli vidět výstup takto:

    < HTTP/1.1 201 Created
    < Content-Type: application/json; charset=UTF-8
    < Location: /18
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Tue, 01 Dec 2015 05:39:10 GMT
    < Content-Length: 37
    <
    {"id":1,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

Poznamenejte si ID dávky v poslední řádek výstupu (v tomto příkladu je '1'). Pokud chcete ověřit, že aplikace bude spuštěna úspěšně, můžete se podívat na účtu úložiště Azure, který je přidružen ke clusteru a měli byste vidět **/EventCount/EventCount10** složku vytvořit existuje. Tato složka by měla obsahovat objekty BLOB, které jsou zaznamenány počet událostí, které jsou zpracovány v časovém období zadaná pro parametr **batch interval v sekundách**.

Vysílání datového proudu aplikací Spark budou nadále spustit, dokud jste ho ukončit. Uděláte to tak, použijte následující příkaz:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/1"

### <a name="run-the-applications-to-receive-the-events-into-an-azure-storage-blob-as-json"></a>Spuštění aplikace, které chcete přijímat události do objektu Blob úložiště Azure jako JSON
Otevřete příkazový řádek, přejděte do adresáře, kam jste nainstalovali CURL a spusťte následující příkaz (uživatelské jméno a heslo a cluster nahraďte název):

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputJSON.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Parametry v souboru **inputJSON.txt** jsou definovány takto:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureBlobAsJSON", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-store-folder", "/EventStore10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Parametry jsou podobná zadaný pro výstup textu, v předchozím kroku. Znovu není potřeba vytvořit výstupní složky (EventCheckpoint, EventCount/EventCount10), které se používají jako parametry. Je vytváří streamování aplikace.

 Po spuštění příkazu, můžete se podívat na účtu úložiště Azure, který je přidružen ke clusteru a měli byste vidět **/EventStore10** složku vytvořit existuje. Otevřít libovolný soubor s předponou **část -** a měli byste vidět událostí zpracovaných ve formátu JSON.

### <a name="run-the-applications-to-receive-the-events-into-a-hive-table"></a>Spuštění aplikace, které chcete přijímat události do tabulky Hive
Spustit vysílání datového proudu aplikace, která datové proudy událostí do tabulky Hive Spark budete potřebovat některé další součásti. Jsou to:

* datanucleus rozhraní api jdo 3.2.6.jar
* datanucleus. rdbms 3.2.9.jar
* datanucleus – základní – 3.2.10.jar
* Hive-site.xml

**.Jar** soubory jsou k dispozici v clusteru HDInsight Spark na `/usr/hdp/current/spark-client/lib`. **Hive-site.xml** je k dispozici na `/usr/hdp/current/spark-client/conf`.

Můžete použít [WinScp](http://winscp.net/eng/download.php) zkopírujte tyto soubory z clusteru do místního počítače. Potom můžete nástroje pro kopírování těchto souborů přes do účtu úložiště, který je přidružen ke clusteru. Další informace o tom, jak nahrání souborů do účtu úložiště najdete v tématu [nahrávání dat pro úlohy Hadoop do HDInsight](../hdinsight-upload-data.md).

Jakmile jste zkopírovali přes souborů do účtu úložiště Azure, otevřete příkazový řádek, přejděte do adresáře, kam jste nainstalovali CURL a spusťte následující příkaz (uživatelské jméno a heslo a cluster nahraďte název):

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputHive.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Parametry v souboru **inputHive.txt** jsou definovány takto:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToHiveTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-hive-table", "EventHiveTable10" ], "jars":["wasb:///example/jars/datanucleus-api-jdo-3.2.6.jar", "wasb:///example/jars/datanucleus-rdbms-3.2.9.jar", "wasb:///example/jars/datanucleus-core-3.2.10.jar"], "files":["wasb:///example/jars/hive-site.xml"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Parametry jsou podobná zadaný pro výstup textu, v předchozích krocích. Znovu, není potřeba vytvořit výstupní složky (EventCheckpoint, EventCount/EventCount10) nebo výstupní tabulku Hive (EventHiveTable10), které se používají jako parametry. Je vytváří streamování aplikace. Všimněte si, že **JAR** a **soubory** možnost zahrne cesty k souborům .jar a hive-site.xml, který jste zkopírovali do účtu úložiště.

Chcete-li ověřit, že byl úspěšně vytvořen tabulku hive, použijte [zobrazení Ambari Hive](../hadoop/apache-hadoop-use-hive-ambari-view.md). Můžete spustit dotaz SELECT existuje, chcete-li zobrazit obsah v tabulce.

    SELECT * FROM eventhivetable10 LIMIT 10;

Zobrazený výstup by měl vypadat asi takto:

    ZN90apUSQODDTx7n6Toh6jDbuPngqT4c
    sor2M7xsFwmaRW8W8NDwMneFNMrOVkW1
    o2HcsU735ejSi2bGEcbUSB4btCFmI1lW
    TLuibq4rbj0T9st9eEzIWJwNGtMWYoYS
    HKCpPlWFWAJILwR69MAq863nCWYzDEw6
    Mvx0GQOPYvPR7ezBEpIHYKTKiEhYammQ
    85dRppSBSbZgThLr1s0GMgKqynDUqudr
    5LAWkNqorLj3ZN9a2mfWr9rZqeXKN4pF
    ulf9wSFNjD7BZXCyunozecov9QpEIYmJ
    vWzM3nvOja8DhYcwn0n5eTfOItZ966pa
    Time taken: 4.434 seconds, Fetched: 10 row(s)


### <a name="run-the-applications-to-receive-the-events-into-an-azure-sql-database-table"></a>Spuštění aplikace, které chcete přijímat události do tabulky databáze Azure SQL
Před spuštěním tohoto kroku, ujistěte se, že máte k Azure SQL database vytvořit. Pokyny najdete v tématu [vytvoření databáze SQL v minutách](../../sql-database/sql-database-get-started-portal.md). K dokončení této části, potřebujete hodnoty pro název databáze, název databázového serveru a přihlašovací údaje správce databáze jako parametry. Není nutné, když vytvoření databázové tabulky. Vysílání datového proudu aplikací Spark vytvoří, které pro vás.

Otevřete příkazový řádek, přejděte do adresáře, kam jste nainstalovali CURL a spusťte následující příkaz:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputSQL.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Parametry v souboru **inputSQL.txt** jsou definovány takto:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureSQLTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--sql-server-fqdn", "<database-server-name>.database.windows.net", "--sql-database-name", "mysparkdatabase", "--database-username", "sparkdbadmin", "--database-password", "<put-password-here>", "--event-sql-table", "EventContent" ], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Pokud chcete ověřit, že aplikace bude spuštěna úspěšně, můžete připojit k službě Azure SQL database pomocí SQL Server Management Studio. Pokyny o tom, jak to udělat najdete v tématu [připojit k SQL Database přes SQL Server Management Studio](../../sql-database/sql-database-connect-query-ssms.md). Po připojení k databázi, můžete přejít na **EventContent** tabulku, která byla vytvořena streamování aplikací. Můžete spustit Rychlý dotaz pro získání dat z tabulky. Spusťte následující dotaz:

    SELECT * FROM EventCount

Zobrazený výstup by měl vypadat přibližně takto:

    00046b0f-2552-4980-9c3f-8bba5647c8ee
    000b7530-12f9-4081-8e19-90acd26f9c0c
    000bc521-9c1b-4a42-ab08-dc1893b83f3b
    00123a2a-e00d-496a-9104-108920955718
    0017c68f-7a4e-452d-97ad-5cb1fe5ba81b
    001KsmqL2gfu5ZcuQuTqTxQvVyGCqPp9
    001vIZgOStka4DXtud0e3tX7XbfMnZrN
    00220586-3e1a-4d2d-a89b-05c5892e541a
    0029e309-9e54-4e1b-84be-cd04e6fce5ec
    003333cf-874f-4045-9da3-9f98c2b4ea49
    0043c07e-8d73-420a-9af7-1fcb94575356
    004a11a9-0c2c-4bc0-a7d5-2e0ebd947ab9


## <a name="seealso"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](apache-spark-overview.md)
* [Návrh připojení na základě příjemce a přímé DStream](https://www.slideshare.net/NanZhu/seattle-sparkmeetup032317)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin](apache-spark-machine-learning-mllib-ipython.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](../hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Vytvoření a spouštění aplikací
* [Vytvoření samostatné aplikace pomocí Scala](../hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření
* [Modul plug-in nástroje HDInsight pro IntelliJ IDEA pro vytvoření a odesílání aplikací Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Použití modulu plug-in nástroje HDInsight pro IntelliJ IDEA pro vzdálené ladění aplikací Spark](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků v clusteru Apache Spark v Azure HDInsight](apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: ../hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]: ../storage-create-storage-account/
