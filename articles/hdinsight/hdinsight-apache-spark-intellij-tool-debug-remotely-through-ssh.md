---
title: "Azure nástrojů pro IntelliJ: ladění Spark aplikací vzdáleně přes SSH | Microsoft Docs"
description: "Podrobné pokyny o tom, jak používat nástroje HDInsight v Azure nástrojů pro IntelliJ k ladění aplikací vzdáleně na HDInsight clustery prostřednictvím SSH"
keywords: "vzdálené ladění intellij, vzdálené ladění intellij, ssh, intellij, hdinsight, ladění intellij, ladění"
services: hdinsight
documentationcenter: 
author: jejiang
manager: DJ
editor: Jenny Jiang
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 08/24/2017
ms.author: Jenny Jiang
ms.openlocfilehash: 19053e31d6eb097bc91a04ef9c6af5772aaa16da
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="debug-spark-applications-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a>Ladění aplikací Spark v clusteru s Azure nástrojů HDInsight pro IntelliJ prostřednictvím SSH

Tento článek obsahuje podrobné pokyny o tom, jak používat nástroje HDInsight v Azure nástrojů pro IntelliJ k ladění aplikací vzdáleně v clusteru HDInsight. K ladění projektu, můžete také zobrazit [HDInsight Spark ladění aplikace s Azure Toolkit pro IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) videa.

**Požadavky**

* **Nástroje HDInsight v Azure nástrojů pro IntelliJ**. Tento nástroj je součástí sady nástrojů Azure pro IntelliJ. Další informace najdete v tématu [instalaci nástrojů Azure pro IntelliJ](https://docs.microsoft.com/en-us/azure/azure-toolkit-for-intellij-installation).
* **Azure nástrojů pro IntelliJ**. Tato sada nástrojů použijte k vytvoření aplikací Spark pro cluster služby HDInsight. Další informace, postupujte podle pokynů v [pomocí nástrojů Azure pro IntelliJ k vytvoření aplikací Spark pro cluster služby HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).
* **HDInsight SSH služby pomocí uživatelského jména a hesla správu**. Další informace najdete v tématu [připojení k HDInsight (Hadoop) pomocí protokolu SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) a [použití SSH tunelového propojení pro přístup k Ambari webové uživatelské rozhraní, JobHistory, NameNode, Oozie a jiné webové uživatelská](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel). 
 

## <a name="create-a-spark-scala-application-and-configure-it-for-remote-debugging"></a>Vytvoření aplikací Spark Scala a nakonfigurovat ji pro vzdálené ladění

1. Spusťte IntelliJ IDEA a pak vytvořte projekt. V **nový projekt** dialogové okno pole, postupujte takto:

   a. Vyberte **HDInsight**. 

   b. Vyberte Java nebo Scala šablony založené na vaši volbu. Vyberte jednu z těchto možností:

      - **Spark v HDInsight (Scala)**

      - **Spark v HDInsight (Java)**

      - **Spark v clusteru HDInsight spuštění ukázkové (Scala)**

      Tento příklad používá **Spark v HDInsight clusteru spustit ukázkový (Scala)** šablony.

   c. V **nástroj pro sestavení** vyberte některý z následujících akcí podle potřeby vašeho:

      - **Maven**, podpora Průvodce vytvoření projektu Scala

      -  **SBT**, ke správě závislosti a vytváření projektu Scala 

      ![Vytvoření projektu ladění](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-create-projectfor-debug-remotely.png)

   d. Vyberte **Další**.     
 
3. V dalším **nový projekt** okno, postupujte takto:

   ![Vyberte Spark SDK](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-new-project.png)

   a. Zadejte název projektu a umístění projektu.

   b. V **SDK projektu** rozevíracího seznamu vyberte **Java 1.8** pro **Spark 2.x** clusteru nebo vyberte **Java 1.7** pro **Spark 1.x**  clusteru.

   c. V **Spark verze** rozevíracího seznamu, Průvodce vytvořením projektu Scala integruje správná verze pro sadu SDK Spark a Scala SDK. Pokud verze clusteru spark je starší než 2.0, vyberte **Spark 1.x**. Jinak vyberte možnost **Spark 2.x.** Tento příklad používá **Spark bodu 2.0.2 (Scala 2.11.8)**.

   d. Vyberte **Finish** (Dokončit).

4. Vyberte **src** > **hlavní** > **scala** otevřete kódu v projektu. Tento příklad používá **SparkCore_wasbloTest** skriptu.

5. Pro přístup k **upravit konfigurace** nabídce vyberte ikonu v pravém horním rohu. Z této nabídky můžete vytvořit nebo upravit konfiguraci pro vzdálené ladění.

   ![Úprava konfigurací](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-edit-configurations.png) 

6. V **konfigurace spustit/Debug** dialogovém okně vyberte znaménko plus (**+**). Vyberte **odeslat úlohu Spark** možnost.

   ![Přidat novou konfiguraci](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-add-new-Configuration.png)
7. Zadejte informace pro **název**, **clusteru Spark**, a **hlavní název třídy**. Potom vyberte **pokročilou konfiguraci**. 

   ![Spuštění konfigurace ladění](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-run-debug-configurations.png)

8. V **Spark odeslání Upřesnit konfiguraci** dialogové okno, vyberte **Spark povolení vzdáleného ladění**. Zadejte uživatelské jméno SSH a pak zadejte heslo nebo použít soubor privátního klíče. Chcete-li uložit konfiguraci, vyberte **OK**.

   ![Povolení vzdáleného ladění Spark](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-enable-spark-remote-debug.png)

9. Konfigurace je nyní uložit s názvem, který jste zadali. Chcete-li zobrazit podrobnosti o konfiguraci, vyberte název konfigurace. Chcete-li provést změny, vyberte **upravit konfigurace**. 

10. Po dokončení nastavení konfigurace, můžete spustit projekt na vzdálený cluster nebo provést vzdálené ladění.

## <a name="learn-how-to-perform-remote-debugging"></a>Zjistěte, jak provést vzdálené ladění
### <a name="scenario-1-perform-remote-run"></a>Scénář 1: Provést vzdálené spuštění

V této části Ukážeme vám ladění ovladače a vykonavatelů.

    import org.apache.spark.{SparkConf, SparkContext}

    object LogQuery {
      val exampleApacheLogs = List(
        """10.10.10.10 - "FRED" [18/Jan/2013:17:56:07 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 315 "http://referall.com/" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.350 "-" - "" 265 923 934 ""
          | 62.24.11.25 images.com 1358492167 - Whatup""".stripMargin.lines.mkString,
        """10.10.10.10 - "FRED" [18/Jan/2013:18:02:37 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 306 "http:/referall.com" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.352 "-" - "" 256 977 988 ""
          | 0 73.23.2.15 images.com 1358492557 - Whatup""".stripMargin.lines.mkString
      )
      def main(args: Array[String]) {
        val sparkconf = new SparkConf().setAppName("Log Query")
        val sc = new SparkContext(sparkconf)
        val dataSet = sc.parallelize(exampleApacheLogs)
        // scalastyle:off
        val apacheLogRegex =
          """^([\d.]+) (\S+) (\S+) \[([\w\d:/]+\s[+\-]\d{4})\] "(.+?)" (\d{3}) ([\d\-]+) "([^"]+)" "([^"]+)".*""".r
        // scalastyle:on
        /** Tracks the total query count and number of aggregate bytes for a particular group. */
        class Stats(val count: Int, val numBytes: Int) extends Serializable {
          def merge(other: Stats): Stats = new Stats(count + other.count, numBytes + other.numBytes)
          override def toString: String = "bytes=%s\tn=%s".format(numBytes, count)
        }
        def extractKey(line: String): (String, String, String) = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              if (user != "\"-\"") (ip, user, query)
              else (null, null, null)
            case _ => (null, null, null)
          }
        }
        def extractStats(line: String): Stats = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              new Stats(1, bytes.toInt)
            case _ => new Stats(1, 0)
          }
        }
        
        dataSet.map(line => (extractKey(line), extractStats(line)))
          .reduceByKey((a, b) => a.merge(b))
          .collect().foreach{
          case (user, query) => println("%s\t%s".format(user, query))}

        sc.stop()
      }
    }


1. Nastavit body ukončování řádků a pak vyberte **ladění** ikonu.

   ![Vyberte ikonu pro ladění](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-icon.png)

2. Při spuštění programu dosáhne bodem ukončování řádků, uvidíte **ovladač** kartě a dvě **vykonavatele** karty v **ladicí program** podokně. Vyberte **obnovit Program** ikonu dál spuštěný kód, který pak dosáhne další zarážek a se zaměřuje na odpovídající **vykonavatele** kartě. Můžete zkontrolovat protokoly spouštění na odpovídající **konzoly** kartě.

   ![Karta ladění](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debugger-tab.png)

### <a name="scenario-2-perform-remote-debugging-and-bug-fixing"></a>Scénář 2: Proveďte vzdálené ladění a opravy chyb
V této části jsme ukazují, jak pomocí možnosti ladění IntelliJ pro jednoduché opravu dynamicky aktualizovat hodnotu proměnné. V následujícím příkladu kódu je vyvolána výjimka, protože cílový soubor již existuje.
  
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext

        object SparkCore_WasbIOTest {
          def main(arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkCore_WasbIOTest")
            val sc = new SparkContext(conf)
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            // Find the rows that have only one digit in the sixth column.
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)

            try {
              var target = "wasb:///HVACout2_testdebug1";
              rdd1.saveAsTextFile(target);
            } catch {
              case ex: Exception => {
                throw ex;
              }
            }
          }
        }


#### <a name="to-perform-remote-debugging-and-bug-fixing"></a>K provedení vzdálené ladění a opravy chyb
1. Nastavit dva body ukončování řádků a pak vyberte **ladění** ikonu pro spuštění vzdáleného ladění procesu.

2. Kód zastaví v prvním bodu ukončování řádků, a parametr a informace o proměnných jsou uvedeny v **proměnné** podokně. 

3. Vyberte **obnovit Program** ikonu pokračovat. Kód zastaví v druhém bodu. Výjimka je zachycena podle očekávání.

  ![Throw – chyba](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-throw-error.png) 

4. Vyberte **obnovit Program** ikonu znovu. **HDInsight Spark odeslání** okně se zobrazí chyba "Úloha spuštění se nepovedlo".

  ![Chyba odeslání](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-error-submission.png) 

5. Chcete-li dynamicky aktualizovat hodnotu proměnné pomocí IntelliJ ladění funkce, vyberte **ladění** znovu. **Proměnné** podokně se zobrazí znovu. 

6. Klikněte pravým tlačítkem na cíl na **ladění** a pak vyberte **nastavit hodnotu**. Potom zadejte novou hodnotu proměnné. Potom vyberte **Enter** uložte hodnotu. 

  ![Nastavte hodnotu](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-set-value.png) 

7. Vyberte **obnovit Program** ikonu pokračujte ke spuštění programu. Tentokrát je žádná výjimka zachycena. Uvidíte, že projekt úspěšně běží bez jakékoli výjimky.

  ![Ladění bez výjimky](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-without-exception.png)

## <a name="seealso"></a>Další kroky
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="demo"></a>Ukázka
* Vytvoření projektu Scala (video): [vytvoření aplikací Spark Scala](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Vzdálené ladění (video): [pomocí nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně v clusteru HDInsight](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: provádějte interaktivní analýzy dat pomocí Spark v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: používejte Spark v HDInsight pro analýzu stavební teploty pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight k sestavení aplikací datových proudů v reálném čase](hdinsight-apache-spark-eventhub-streaming.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Vytvoření a spouštění aplikací
* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření
* [Vytvoření aplikací Spark pro cluster služby HDInsight pomocí nástrojů Azure pro IntelliJ](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Použití nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně prostřednictvím sítě VPN](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Používat nástroje HDInsight pro vytvoření aplikací Spark v Azure nástrojů pro Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Spark pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků v clusteru Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)
