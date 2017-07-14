---
title: "Představujeme Spark v Azure HDInsight | Dokumentace Microsoftu"
description: "Tento článek představuje Spark ve službě HDInsight a různé scénáře, ve kterých můžete použít cluster Sparku ve službě HDInsight."
keywords: "co je apache spark, cluster spark, představujeme spark, spark ve službě hdinsight"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 82334b9e-4629-4005-8147-19f875c8774e
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/12/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 80be19618bd02895d953f80e5236d1a69d0811af
ms.openlocfilehash: acb80aa98cc978a906ccd6e4b4132a439e505bc8
ms.contentlocale: cs-cz
ms.lasthandoff: 06/07/2017


---
# Představujeme Spark ve službě HDInsight
<a id="introduction-to-spark-on-hdinsight" class="xliff"></a>

Tento článek obsahuje úvod do Sparku ve službě HDInsight. <a href="http://spark.apache.org/" target="_blank">Apache Spark</a> je opensource paralelní framework pro zpracování, který podporuje zpracování v paměti pro zvýšení výkonu velkých objemů dat analytických aplikací. Cluster Spark v prostředí HDInsight je kompatibilní s úložištěm Azure Storage (WASB) a také se službou Azure Data Lake Store, takže svá stávající data uložená v Azure můžete v tomto clusteru snadno zpracovat.

Když vytvoříte cluster Spark v HDInsight, vytvoříte výpočetní prostředky Azure s nainstalovaným a nakonfigurovaným Sparkem. Vytvoření clusteru Spark v HDInsight trvá pouze asi deset minut. Data, která se zpracují, jsou uložená v úložišti Azure Storage nebo ve službě Azure Data Lake Store. Další informace najdete v tématu [Použití Azure Storage s HDInsight](hdinsight-hadoop-use-blob-storage.md).

**Pokud chcete ve službě HDInsight vytvořit cluster Spark**, přečtěte si téma [Rychlý start: Vytvoření clusteru Spark v HDInsight a spuštění interaktivního dotazu pomocí Jupyteru](hdinsight-apache-spark-jupyter-spark-sql.md).


## Co je Apache Spark ve službě Azure HDInsight?
<a id="what-is-apache-spark-on-azure-hdinsight" class="xliff"></a>
Clustery Spark v prostředí HDInsight nabízejí plně spravovanou službu Spark. Tady najdete výhody, které přináší vytvoření clusteru Spark v HDInsight.

| Funkce | Popis |
| --- | --- |
| Snadné vytváření clusterů Spark |Během několika minut můžete vytvořit nový cluster Spark v HDInsight pomocí portálu Azure Portal, Azure PowerShellu nebo sady SDK rozhraní .NET HDInsight. Viz [Začínáme s clusterem Spark v HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Snadné používání |Součástí clusteru Spark v prostředí HDInsight jsou poznámkové bloky Jupyter a Zeppelin. Můžete je použít pro interaktivní zpracování dat a vizualizaci.|
| Rozhraní REST API |Clustery Spark v HDInsight zahrnují [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server), server úloh Spart založený na rozhraní REST API, který slouží ke vzdálenému odesílání a monitorování úloh. |
| Podpora pro Azure Data Lake Store | Cluster Spark ve službě HDInsight můžete nakonfigurovat tak, aby používal Azure Data Lake Store jako další úložiště i jako primární úložiště (pouze s clustery HDInsight 3.5). Další informace o Data Lake Store naleznete v tématu [Přehled o Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md). |
| Integrace se službami Azure |Cluster Spark v HDInsight se dodává s konektorem k Azure Event Hubs. Zákazníci mohou vytvářet aplikace streamování pomocí Event Hubs navíc ke službě [Kafka](http://kafka.apache.org/), která je již k dispozici jako součást Spark. |
| Podpora pro R Server | R Server můžete nastavit v clusteru HDInsight Spark ke spuštění distribuovaných R výpočtů s rychlostmi dohodnutými s clusterem Spark. Další informace naleznete v tématu [Začínáme používat R Server v HDInsight](hdinsight-hadoop-r-server-get-started.md). |
| Integrace v prostředí IDE třetích stran | Služba HDInsight přináší moduly plug-in pro prostředí IDE, jako je IntelliJ IDEA a Eclipse, díky kterým můžete vytvářet a odesílat aplikace do clusteru Spart v HDInsight. Další informace najdete v tématu [Použití sady Azure Toolkit pro IntelliJ IDEA](hdinsight-apache-spark-intellij-tool-plugin.md) a [Použití sady Azure Toolkit pro Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md).|
| Počet souběžných dotazů |Clustery Spark v HDInsight podporují souběžné dotazy. To umožňuje sdílení stejných prostředků clusteru pro více dotazů od jednoho uživatele nebo více dotazů od různých uživatelů a aplikací. |
| Ukládání do mezipaměti na SSD |Data do mezipaměti můžete ukládat volitelně buď do paměti nebo na SSD disky připojené k uzlům clusteru. Ukládání do paměti poskytuje nejlepší výkon dotazů, ale může být nákladné; ukládání na SSD poskytuje skvělou možnost pro zlepšení výkonu dotazů, aniž by bylo nutné vytvořit cluster velikosti, která je potřeba pro umístění celé datové sady do paměti. |
| Integrace s nástroji BI |Clustery Spark pro HDInsight nabízí konektory pro nástroje BI, například [Power BI](http://www.powerbi.com/) a [Tableau](http://www.tableau.com/products/desktop) pro analýzu dat. |
| Předem zavedené knihovny Anaconda |Clustery Spark na HDInsight přichází s předinstalovanými knihovnami Anaconda. [Anaconda](http://docs.continuum.io/anaconda/) poskytuje téměř 200 knihoven pro machine learning, analýzy dat, vizualizace atd. |
| Škálovatelnost |Ačkoli můžete určit počet uzlů v clusteru během vytváření, můžete chtít zvětšit nebo zmenšit cluster tak, aby odpovídal pracovnímu vytížení. Všechny clustery HDInsight umožňují změnit počet uzlů v clusteru. Navíc clustery Spark můžete vyřadit bez ztráty dat, protože všechna data jsou uložená v Azure Storage nebo ve službě Data Lake Store. |
| Nepřetržitá podpora |Clustery Spark v HDInsight přináší nepřetržitou podporu napříč celou podnikovou sítí a SLA, která zajišťuje 99,9% dostupnost. |

## Jaké jsou případy použití pro Spark v HDInsight?
<a id="what-are-the-use-cases-for-spark-on-hdinsight" class="xliff"></a>
Clustery Spark v HDInsight podporují následující klíčové scénáře.

### Interaktivní analýzu dat a BI
<a id="interactive-data-analysis-and-bi" class="xliff"></a>
[Podívejte se na kurz](hdinsight-apache-spark-use-bi-tools.md)

Apache Spark v HDInsight ukládá data do úložiště Azure Storage nebo služby Azure Data Lake Store. Obchodní specialisté a osoby provádějící klíčová rozhodnutí analyzují a vytváří z těchto dat sestavy a používají Microsoft Power BI pro vytváření interaktivních sestav z analyzovaných dat. Analytici mohou začínat z nestrukturovaných / částečně strukturovaných dat v úložišti clusteru, definovat schéma pro data s využitím poznámkových bloků a následně vytvořit modely dat pomocí Microsoft Power BI. Clustery Spark v HDInsight podporují také různé nástroje třetích stran pro BI, například Tableau. Představují tak ideální platformu pro analytiky dat, obchodní specialisty a osoby provádějící klíčová rozhodnutí.

### Spark Machine Learning
<a id="spark-machine-learning" class="xliff"></a>
[Podívejte se na kurz: předpovídání teplot sestavení pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Podívejte se na kurz: předpovídání výsledků kontroly potravin](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

Systém Apache Spark je vybavený knihovnou [MLlib](http://spark.apache.org/mllib/) pro strojové učení, jejímž základem je Spark a kterou můžete používat z clusteru Spark v HDInsight. Cluster Spark v HDInsight obsahuje Anacondu, distribuci jazyka Python s různými balíčky pro strojové učení. Spojte tyto možnosti s vestavěnou podporou pro poznámkové bloky Jupyter a Zeppelin a máte nejmodernější prostředí pro tvorbu machine learning aplikací.

### Vysílání datových proudů a analýza dat v reálném čase ve Sparku
<a id="spark-streaming-and-real-time-data-analysis" class="xliff"></a>
[Podívejte se na kurz](hdinsight-apache-spark-eventhub-streaming.md)

Clustery Spark v HDInsight nabízí bohatou podporu pro vytváření řešení pro analýzu v reálném čase. Zatímco Spark již obsahuje konektory pro načítání dat z mnoha zdrojů, například soketů Kafka, Flume, Twitter, ZeroMQ nebo TCP, Spark v HDInsight přidává prvotřídní podporu pro příjem dat z Azure Event Hubs. Event Hubs je nejpoužívanější službou řazení front v Azure. Díky připravené podpoře pro službu Event Hubs představují clustery Spark v HDInsight ideální platformu pro vytvoření kanálu k analýze dat v reálném čase.

## <a name="next-steps"></a>Jaké součásti jsou zahrnuté v clusteru Spark?
Součástí clusterů Spark v HDInsight jsou následující komponenty, které jsou v nich k dispozici ve výchozím nastavení.

* [Spark Core](https://spark.apache.org/docs/1.5.1/). Obsahuje Spark Core, Spark SQL, rozhraní API pro vysílání datového proudu Spark, GraphX a MLlib.
* [Anaconda](http://docs.continuum.io/anaconda/)
* [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
* [Poznámkový blok Jupyter](https://jupyter.org)
* [Poznámkový blok Zeppelin](http://zeppelin-project.org/)

Clustery Spark v HDInsight také poskytují [ovladač ODBC](http://go.microsoft.com/fwlink/?LinkId=616229) pro připojení ke clusterům Spark v HDInsight z nástrojů BI, například Microsoft Power BI a Tableau.

## Kde mám začít?
<a id="where-do-i-start" class="xliff"></a>
Začněte tak, že vytvoříte cluster Spark v HDInsight. Přečtěte si téma [Rychlý začátek: Vytvoření clusteru Spark v HDInsight v Linuxu a spuštění interaktivního dotazu pomocí Jupyteru](hdinsight-apache-spark-jupyter-spark-sql.md). 

## Další kroky
<a id="next-steps" class="xliff"></a>
### Scénáře
<a id="scenarios" class="xliff"></a>
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](hdinsight-apache-spark-eventhub-streaming.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### Vytvoření a spouštění aplikací
<a id="create-and-run-applications" class="xliff"></a>
* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)

### Nástroje a rozšíření
<a id="tools-and-extensions" class="xliff"></a>
* [Modul plug-in nástroje HDInsight pro IntelliJ IDEA pro vytvoření a odesílání aplikací Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Použití modulu plug-in nástroje HDInsight pro IntelliJ IDEA pro vzdálené ladění aplikací Spark](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### Správa prostředků
<a id="manage-resources" class="xliff"></a>
* [Správa prostředků v clusteru Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)
* [Známé problémy systému Apache Spark v Azure HDInsight](hdinsight-apache-spark-known-issues.md)

