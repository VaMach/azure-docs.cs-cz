<properties 
    pageTitle="Přehled Apache Spark v HDInsight | Microsoft Azure" 
    description="Úvod do Apache Spark v HDInsight a scénářů, ve kterých se má použít Spark v HDInsight v aplikacích." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# Přehled: Apache Spark v Linux HDInsight
 
<a href="http://spark.apache.org/" target="_blank">Apache Spark</a> je opensource paralelní framework pro zpracování, který podporuje zpracování v paměti pro zvýšení výkonu velkých objemů dat analytických aplikací. Modul zpracování Spark je postaven pro rychlost, snadné použití a sofistikované analýzy. Možnosti výpočtů Spark v paměti představují dobrou volbu pro iterativní algoritmy ve výpočtech machine learning a grafů. Spark je také kompatibilní s úložištěm Azure Blob (WASB), takže existující data uložená v Azure lze snadno zpracovat prostřednictvím Spark.

Když vytvoříte cluster Spark v HDInsight, vytvoříte výpočetní prostředky Azure s nainstalovaným a nakonfigurovaným Spark. Vytvoření clusteru Spark v HDInsight trvá pouze asi deset minut. Data ke zpracování se ukládají do úložiště Azure Blob. Viz [Použití Azure Blob Storage s HDInsight][hdinsight-storage].

![Apache Spark v Azure HDInsight](./media/hdinsight-apache-spark-overview/hdispark.architecture.png  "Apache Spark on Azure HDInsight")


**Chcete začít pracovat s Apache Spark v Azure HDInsight?** Viz [Rychlý začátek: Vytvořte cluster Spark na systému HDInsight Linux a spusťte ukázkové aplikace pomocí Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md).

>[AZURE.NOTE] Seznam známých problémů a omezení v aktuální verzi najdete v části [Známé problémy Apache Spark v Azure HDInsight (Linux)](hdinsight-apache-spark-jupyter-spark-sql.md).


## Proč používat Spark v Azure HDInsight? 

Azure HDInsight nabízí plně spravovanou službu Spark. Výhody použití Spark v HDInsight jsou:

| Funkce                             | Popis       |
|-------------------------------------|-------------------|
| Snadné vytváření clusterů            | Během několika minut můžete vytvořit nový cluster Spark v HDInsight pomocí portálu pro správu Azure, Azure PowerShell nebo sady SDK rozhraní .NET HDInsight. Viz [Začínáme s clusterem Spark v HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Snadné používání                     | Spark v clusterech HDInsight zahrnuje předem konfigurované poznámkové bloky Jupyter. Můžete je použít pro interaktivní zpracování dat a vizualizaci. Adresy URL jsou https://CLUSTERNAME.azurehdinsight.net/jupyter. Nahraďte __CLUSTERNAME__ názvem clusteru Spark HDInsight.|
| Rozhraní REST API                       | Spark v HDInsight zahrnuje [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server), rozhraní REST API založené na serveru úloh Spark pro vzdálené odesílání a sledování spuštěných úloh. |
| Podpora pro Azure Data Lake Store | Spark v HDInsight můžete nakonfigurovat k používání Azure Data Lake Store jako další úložiště. Další informace o Data Lake Store naleznete v tématu [Přehled o Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
| Integrace se službami Azure | Spark v HDInsight se dodává s konektorem k Azure Event Hubs. Zákazníci mohou vytvářet aplikace streamování pomocí Event Hubs navíc ke službě [Kafka](http://kafka.apache.org/), která je již k dispozici jako součást Spark. |
| Podpora pro R Server  | R Server můžete nastavit v clusteru HDInsight Spark ke spuštění distribuovaných R výpočtů s rychlostmi dohodnutými s clusterem Spark. Další informace naleznete v tématu [Začínáme používat R Server v HDInsight](hdinsight-hadoop-r-server-get-started.md).   |
| Integrace s IntelliJ IDEA  | Modul plug-in HDInsight pro IntelliJ můžete použít k vytvoření a odeslání aplikací do clusteru HDInsight Spark. Další informace naleznete v tématu [Používání modulu plug-in nástroje HDInsight pro vytvoření aplikací Spark pro cluster HDInsight Spark Linux](hdinsight-apache-spark-intellij-tool-plugin.md). |
| Počet souběžných dotazů              | Spark v HDInsight podporuje souběžné dotazy. To umožňuje sdílení stejných prostředků clusteru pro více dotazů od jednoho uživatele nebo více dotazů od různých uživatelů a aplikací. |
| Ukládání do mezipaměti na SSD                 | Data do mezipaměti můžete ukládat volitelně buď do paměti nebo na SSD disky připojené k uzlům clusteru. Ukládání do paměti poskytuje nejlepší výkon dotazů, ale může být nákladné; ukládání na SSD poskytuje skvělou možnost pro zlepšení výkonu dotazů, aniž by bylo nutné vytvořit cluster velikosti, která je potřeba pro umístění celé datové sady do paměti.|
| Integrace s nástroji BI       | Spark pro HDInsight nabízí konektory pro nástroje BI, například [Power BI](http://www.powerbi.com/) a [Tableau](http://www.tableau.com/products/desktop) pro analýzu dat.|
| Předem zavedené knihovny Anaconda        | Clustery Spark na HDInsight přichází s předinstalovanými knihovnami Anaconda. [Anaconda](http://docs.continuum.io/anaconda/) poskytuje téměř 200 knihoven pro machine learning, analýzy dat, vizualizace atd.|
| Škálovatelnost                     | Ačkoli můžete určit počet uzlů v clusteru během vytváření, můžete chtít zvětšit nebo zmenšit cluster tak, aby odpovídal pracovnímu vytížení. Všechny clustery HDInsight umožňují změnit počet uzlů v clusteru. Navíc clustery Spark můžete vyřadit bez ztráty dat, vzhledem k tomu, že jsou všechna data uložena do Azure Blob Storage. |
| Nepřetržitá podpora                    | Spark v HDInsight přináší nepřetržitou podporu rozlehlé sítě a SLA 99,9 % doby provozu.|



## Jaké jsou případy použití pro Spark v HDInsight?

Apache Spark v HDInsight umožňuje následující klíčové scénáře.

### Interaktivní analýzu dat a BI

[Podívejte se do kurzu](hdinsight-apache-spark-use-bi-tools.md)

Apache Spark v HDInsight ukládá data do objektů Blob Azure. Obchodní specialisté a osoby provádějící klíčová rozhodnutí analyzují a vytváří z těchto dat sestavy a používají Microsoft Power BI pro vytváření interaktivních sestav z analyzovaných dat. Analytici mohou začínat z nestrukturovaných / částečně strukturovaných dat v úložišti Azure, definovat schéma pro data s využitím poznámkových bloků a následně vytvořit modely dat pomocí Microsoft Power BI. Spark v HDInsight podporuje také různé nástroje třetích stran BI například Tableau, Qlikview a Lumira SAP, což z něj činí ideální platformu pro analytiky dat, obchodní specialisty a osoby provádějící klíčová rozhodnutí.

### Iterační Machine Learning

[Podívejte se na kurz: předpovídání teplot sestavení pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Podívejte se na kurz: předpovídání výsledků kontroly potravin](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

Apache Spark se dodává s [MLlib](http://spark.apache.org/mllib/), knihovnou machine learning postavenou na Spark. Kromě toho také Spark v HDInsight obsahuje Anacondu, distribuci jazyka Python s různými balíčky pro machine learning. Spojte tyto možnosti s vestavěnou podporou pro poznámkové bloky Jupyter a máte nejmodernější prostředí pro tvorbu machine learning aplikací.  

### Vysílání datových proudů a analýza dat v reálném čase

[Podívejte se do kurzu](hdinsight-apache-spark-eventhub-streaming.md)

Analýza dat v reálném čase se používá pro scénáře od snížení času pro data insight zpracováním dat tak jak jsou po sestavení skutečného řešení datového proudu. Spark v HDInsight nabízí bohatou podporu pro vytváření řešení pro analýzu v reálném čase. Zatímco Spark již obsahuje konektory pro načítání dat z mnoha zdrojů, například soketů Kafka, Flume, Twitter, ZeroMQ nebo TCP, Spark v HDInsight přidává prvotřídní podporu pro příjem dat z Azure Event Hubs. Event Hubs je nejpoužívanější službou řazení front v Azure. Připravená podpora pro Event Hubs činí z Spark v HDInsight ideální platformu pro sestavování analytického kanálu v reálném čase.

##<a name="next-steps"></a>Jaké součásti jsou zahrnuty jako součást clusteru Spark?

Spark v HDInsight zahrnuje následující součásti, které jsou k dispozici v clusterech ve výchozím nastavení.

- [Spark Core](https://spark.apache.org/docs/1.5.1/). Obsahuje Spark Core, Spark SQL, rozhraní API pro vysílání datového proudu Spark, GraphX a MLlib.
- [Anaconda](http://docs.continuum.io/anaconda/)
- [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
- [Poznámkový blok Jupyter](https://jupyter.org)

Spark v HDInsight také poskytuje [ovladač ODBC](http://go.microsoft.com/fwlink/?LinkId=616229) pro konektivitu ke clusterům Spark v HDInsight z nástrojů BI, například Microsoft Power BI a Tableau.

## Kde mám začít?

Začněte vytvořením clusteru Spark na systému HDInsight Linux. Viz [Rychlý začátek: Vytvořte cluster Spark na systému HDInsight Linux a spusťte ukázkové aplikace pomocí Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md). 

## Další kroky

### Scénáře

* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Spark v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)

* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu stavební teploty pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů ve skutečném čase](hdinsight-apache-spark-eventhub-streaming.md)

* [Analýza protokolu webu pomocí Spark v HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### Vytvoření a spouštění aplikací

* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Vzdálené spouštění úloh na clusteru Spark pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)

### Nástroje a rozšíření

* [Modul plug-in nástroje HDInsight pro IntelliJ IDEA pro vytvoření a odesílání aplikací Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Použití modulu plug-in nástroje HDInsight pro IntelliJ IDEA pro vzdálené ladění aplikací Spark](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Použití poznámkových bloků Zeppelin s clusterem Spark v HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Spark pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Použijte externí balíčky s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Nainstalujte do počítače Jupyter a připojte ho ke clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### Správa prostředků

* [Správa prostředků v clusteru Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Sledování a ladění úloh spuštěných v clusteru serveru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md



<!--HONumber=sep16_HO2-->


