---
title: "Co jsou HDInsight, technologie Hadoop a clustery? – Azure | Dokumentace Microsoftu"
description: "Úvod do HDInsight, technologie Hadoop a jejích komponent včetně systémů Spark, Kafka, Hive a HBase pro analýzu velkých objemů dat."
keywords: "azure hadoop, hadoop azure, hadoop úvod, úvod hadoop, technologie hadoop, úvod do technologie hadoop, úvod k technologii hadoop, co je cluster hadoop, co je hadoop cluster, k čemu slouží hadoop"
services: hdinsight
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: e56a396a-1b39-43e0-b543-f2dee5b8dd3a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: cgronlun
ms.openlocfilehash: 00a8f14603bf05f013fadda1f1b80fb1de8f9a7c
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="introduction-to-azure-hdinsight-the-hadoop-technology-stack-and-hadoop-clusters"></a>Úvod do Azure HDInsight, technologie a clusterů Hadoop
 Tento článek obsahuje úvod do Azure HDInsight, cloudové distribuce založené na technologii Hadoop. Také se v něm dozvíte, co je cluster Hadoop a kdy ho můžete použít. 

## <a name="what-is-hdinsight-and-the-hadoop-technology-stack"></a>Co je HDInsight a technologie Hadoop? 
Azure HDInsight je cloudová distribuce komponent Hadoop z platformy [Hortonworks Data Platform (HDP)](https://hortonworks.com/products/data-center/hdp/). Původní open source architekturou pro distribuované zpracování a analýzu velkých datových sad v počítačových clusterech byl systém [Apache Hadoop](http://hadoop.apache.org/). 


Součástí technologie Hadoop je potřebný software a nástroje, jako je Apache Hive, HBase, Spark, Kafka a řada dalších. Komponenty technologie Hadoop dostupné ve službě HDInsight najdete v tématu [Dostupné komponenty a verze ve službě HDInsight][component-versioning]. Další informace o platformě Hadoop v prostředí HDInsight najdete v tématu [Funkce Azure pro HDInsight](https://azure.microsoft.com/services/hdinsight/).

## <a name="what-is-a-hadoop-cluster-and-when-do-you-use-it"></a>Co je cluster Hadoop a kdy se používá?
*Hadoop* je také typ clusteru, který obsahuje:

* YARN pro plánování úloh a správu prostředků
* MapReduce pro paralelní zpracování
* Distribuovaný souborový systém Hadoop (HDFS)
  
Clustery Hadoop se nejčastěji používají pro dávkové zpracování uložených dat. Jiné druhy clusterů ve službě HDInsight mají další funkce: Spark je stále oblíbenější díky rychlejšímu zpracování v paměti. Podrobnosti najdete v tématu [Typy clusterů ve službě HDInsight](#overview).

## <a name="what-is-big-data"></a>Co jsou velké objemy dat?
Velké objemy dat označují všechny rozsáhlé soubory s digitálními informacemi, jako je například:

* Data senzorů v průmyslových zařízeních
* Údaje o aktivitě zákazníka na webové stránce
* Informační kanál sítě Twitter

Velké objemy dat se shromažďují v narůstajícím množství, vyšší rychlostí a ve větší pestrosti formátů. Může se to týkat historických dat (tj. uložených) nebo dat v reálném čase (tj. streamovaných ze zdroje). 

## <a name="overview"></a>Typy clusterů ve službě HDInsight
HDInsight zahrnuje specifické typy clusterů a možnosti přizpůsobení clusterů, jako je například přidávání komponent, nástrojů a jazyků.

### <a name="spark-kafka-interactive-query-hbase-customized-and-other-cluster-types"></a>Spark, Kafka, Interactive Query, HBase, přizpůsobené clustery a další typy clusterů
HDInsight nabízí následující typy clusteru:

* **[Apache Hadoop](https://wiki.apache.org/hadoop):** Používá [HDFS](#hdfs), správu prostředků [YARN](#yarn) a jednoduchý programovací model [MapReduce](#mapreduce) pro paralelní zpracování a analýzu dat dávek.
* **[Apache Spark](http://spark.apache.org/)**: rozhraní pro paralelní zpracování, které podporuje  zpracování v paměti pro zvýšení výkonu aplikací pro analýzu velkých objemů dat, úlohy Spark pro SQL, streamování dat a strojové učení. Přečtěte si téma [Co je Apache Spark v prostředí HDInsight?](../spark/apache-spark-overview.md)
* **[Apache HBase](http://hbase.apache.org/)**: Databáze NoSQL postavená na systému Hadoop, která umožňuje náhodný přístup a zajišťuje velkou konzistenci pro velké objemy nestrukturovaných a částečně strukturovaných dat – potenciálně až miliardy řádků krát miliony sloupců. Přečtěte si téma [Co je HBase v HDInsight?](../hbase/apache-hbase-overview.md)
* **[Microsoft R Server](https://msdn.microsoft.com/microsoft-r/rserver)**: Server pro hostování a správu paralelních, distribuovaných procesů R. Poskytuje datovým vědcům, statistikům a programátorům v R přístup ke škálovatelným, distribuovaným analytickým metodám služby HDInsight na vyžádání. Viz [Přehled R Serveru ve službě HDInsight](../r-server/r-server-overview.md).
* **[Apache Storm](https://storm.incubator.apache.org/)**: distribuovaný výpočetní systém v reálném čase pro rychlé zpracování velkých streamů dat. Storm je poskytován jako spravovaný cluster v prostředí HDInsight. Viz [Analýza dat snímačů v reálném čase pomocí nástrojů Storm a Hadoop](../storm/apache-storm-sensor-data-analysis.md).
* **[Apache Interactive Query ve verzi Preview (také označované LLAP (Live Long and Process)):](https://cwiki.apache.org/confluence/display/Hive/LLAP)** Ukládání do mezipaměti pro interaktivnější a rychlejší dotazy Hive. Viz [Použití Interactive Query ve službě HDInsight](../interactive-query/apache-interactive-query-get-started.md).
* **[Apache Kafka](https://kafka.apache.org/)**: Open source platforma sloužící k vytváření aplikací a kanálů pro streamovaná data. Kafka také poskytuje funkce propojující fronty zpráv, pomocí kterých můžete publikovat datové streamy a přihlašovat se k jejich odběru. Viz [Úvod k Apache Kafka ve službě HDInsight](../kafka/apache-kafka-introduction.md).

Clustery můžete konfigurovat také pomocí následujících metod:
* **[Clustery připojené k doméně ve verzi Preview](../domain-joined/apache-domain-joined-introduction.md)**: Cluster připojený k doméně služby Active Directory umožňující řízení přístupu a poskytující zásady správného řízení pro data.
* **[Vlastní clustery s akcemi skriptů](../hdinsight-hadoop-customize-cluster-linux.md)**: Clustery se skripty, které se spouštějí během zřizování a instalují další komponenty.

### <a name="example-cluster-customization-scripts"></a>Příklady skriptů pro přizpůsobení clusterů
Akce skriptů jsou skripty Bash v systému Linux, které se spouštějí během zřizování clusteru a lze je použít k instalaci dalších komponent clusteru. 

Následují příklady skriptů poskytovaných týmem HDInsight:

* **[Hue](../hdinsight-hadoop-hue-linux.md)**: Sada webových aplikací používaných pro interakci s clusterem. Pouze clustery v Linuxu.
* **[Giraph](../hdinsight-hadoop-giraph-install-linux.md)**: Zpracování grafů pro modelování vztahů mezi věcmi nebo osobami.
* **[Solr](../hdinsight-hadoop-solr-install-linux.md)**: Vyhledávací podniková platforma, která umožňuje fulltextové vyhledávání dat.

Informace o vývoji vlastních akcí skriptů naleznete v tématu [Vývoj akcí skriptů v prostředí HDInsight](../hdinsight-hadoop-script-actions-linux.md).

## <a name="components-and-utilities-on-hdinsight-clusters"></a>Komponenty a nástroje v clusterech HDInsight
Clustery prostředí HDInsight obsahují následující součásti a nástroje:

* **[Ambari](#ambari)**: zřizování, správa, monitorování a nástroje clusterů.
* **[Avro](#avro)** (knihovna Microsoft .NET pro Avro): serializaci dat pro prostředí Microsoft .NET. 
* **[Hive &amp; HCatalog](#hive)**: Dotazování typu strukturovaného dotazovacího jazyka (SQL) a vrstva správy tabulek a ukládání.
* **[Mahout](#mahout)**: pro škálovatelné aplikace strojového učení.
* **[MapReduce](#mapreduce)**: starší verze rozhraní pro distribuované zpracování a správu prostředků systému Hadoop. Viz [YARN](#yarn).
* **[Oozie](#oozie)**: řízení pracovních postupů.
* **[Phoenix](#phoenix)**: vrstva relační databáze nad HBase.
* **[Pig](#pig)**: jednodušší skriptování pro transformace prostředí MapReduce.
* **[Sqoop](#sqoop)**: import a export dat.
* **[Tez](#tez)**: umožňuje efektivní provádění datově náročných procesů ve velkém rozsahu.
* **[YARN](#yarn)**: Správa prostředků, která je součástí hlavní knihovny systému Hadoop.
* **[ZooKeeper](#zookeeper)**: koordinace procesů v distribuovaných systémech.

> [!NOTE]
> Informace o konkrétních komponentách a informace o verzi najdete v tématu [Komponenty a verze systému Hadoop v prostředí HDInsight][component-versioning].
>
>

### <a name="ambari"></a>Ambari
Apache Ambari slouží k zřizování, správě a monitoringu clusterů systému Apache Hadoop. Obsahuje intuitivní sadu nástrojů pro operátory a výkonnou sadu rozhraní API, které překonávají složitost systému Hadoop a zjednodušují operace s clustery. Clustery HDInsight v Linuxu nabízí webové uživatelské rozhraní Ambari i rozhraní Ambari REST API. Zobrazení Ambari v clusterech prostředí HDInsight umožňují funkce zásuvných uživatelských rozhraní.
Viz [Správa clusterů HDInsight pomocí zobrazení Ambari](../hdinsight-hadoop-manage-ambari.md) a <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Rozhraní API v zobrazení Apache Ambari – referenční informace</a>.

### <a name="avro"></a>Avro (knihovna rozhraní Microsoft .NET pro Avro)
V knihovně Microsoft .NET pro Avro implementuje Apache Avro kompaktní binární formát pro předávání dat pro serializaci prostředí Microsoft .NET. Definuje jazykově nezávislé schéma, aby data sériovaná v jednom jazyce bylo možné číst v jiném jazyce. Podrobné informace o formátu lze nalézt v odkazu <a target=_"blank" href="http://avro.apache.org/docs/current/spec.html">Apache Avro Specification</a>. Formát souborů Avro podporuje distribuovaný programovací model MapReduce: Soubory jsou „dělitelné“, což znamená, že v nich můžete vyhledat libovolný bod a začít se čtením dat od konkrétního bloku. Postup naleznete v tématu [Serializace dat pomocí knihovny Microsoft .NET pro Avro](apache-hadoop-dotnet-avro-serialization.md). Do budoucna se plánuje podpora clusterů založených na Linuxu.

### <a name="hdfs"></a>HDFS
Distribuovaný souborů systém Hadoop (HDFS) je spolu s nástroji YARN a MapReduce základem technologie Hadoop. Jde o standardní systém souborů pro clustery Hadoop v HDInsight. Přečtěte si téma [Dotazování dat z úložiště kompatibilního se systémem HDFS](../hdinsight-hadoop-use-blob-storage.md).

### <a name="hive"></a>Hive &amp; HCatalog
<a target="_blank" href="http://hive.apache.org/">Apache Hive</a> je software datového skladu postavený na platformě Hadoop, který umožňuje dotazování a správu rozsáhlých datových sad v distribuovaném úložišti pomocí jazyka typu SQL nazývaného HiveQL. Hive je stejně jako Pig abstrakce vycházející z nástroje MapReduce a překládá dotazy do řady úloh MapReduce. Hive má blíže k systému pro správu relačních databází a používá se u strukturovanějších dat. Pro nestrukturovaná data je vhodnější Pig. Viz [Použití nástroje Hive se systémem Hadoop v prostředí HDInsight](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> je vrstva správy úložiště a tabulek pro systém Hadoop, která poskytuje relační zobrazení dat. V HCatalogu můžete číst a zapisovat soubory v libovolném formátu, který je použitelný pro Hive SerDe (serializátor-deserializátor).

### <a name="mahout"></a>Mahout
<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> představuje knihovnu algoritmů strojového učení, které se dají spouštět v systému Hadoop. S využitím principů statistiky učí aplikace strojového učení systémy učit se z dat a využívat dřívější výstupy ke zjištění budoucího chování. Viz [Generování doporučení pomocí nástroje Mahout v systému Hadoop](../hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
MapReduce je starší verze softwarového rozhraní pro Hadoop, určeného k psaní aplikací pro paralelní dávkové zpracování velkých sad dat. Úloha MapReduce rozdělí rozsáhlé datové sady a uspořádá data pro zpracování do párů hodnot klíčů. Úlohy MapReduce je možné spustit na technologii [YARN](#yarn). Další informace najdete na wikiwebu systému Hadoop v tématu <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a>.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> je systém koordinace pracovních postupů, který spravuje úlohy platformy Hadoop. Je integrován do zásobníku Hadoop a podporuje úlohy systému Hadoop pro MapReduce, Pig, Hive a Sqoop. Lze jej také použít při plánování úloh pro konkrétní systém, jako jsou programy v jazyce Java nebo skripty prostředí. Viz [Použití Oozie se systémem Hadoop](../hdinsight-use-oozie-linux-mac.md).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> je vrstva relační databáze nad HBase. Phoenix zahrnuje ovladač JDBC, který umožňuje přímé dotazování a správu tabulek SQL. Phoenix přeloží dotazy a další příkazy do nativních volání rozhraní API typu NoSQL, místo použití prostředí MapReduce, což umožní rychlejší aplikace nad uloženými daty NoSQL. Viz [Použití nástrojů Apache Phoenix a SQuirreL s clustery HBase](../hdinsight-hbase-phoenix-squirrel.md).

### <a name="pig"></a>Pig
<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a> je vysokoúrovňová platforma, která umožňuje provádět komplexní transformace MapReduce na rozsáhlých datových sadách pomocí jednoduchého skriptovacího jazyka nazvaného Pig Latin. Pig překládá skripty jazyka Pig Latin, aby je bylo možno spustit v systému Hadoop. Pig Latin můžete rozšířit vytvořením uživatelsky definovaných funkcí (UDF). Viz [Použití Pig se systémem Hadoop](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> je nástroj pro co nejefektivnější hromadný přenos dat mezi platformou Hadoop a relačními databázemi jako SQL nebo dalšími strukturovanými datovými úložišti. Viz [Použití nástroje Sqoop se systémem Hadoop](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> je aplikační rozhraní založené na technologii Hadoop YARN, které vytváří komplexní acyklické grafy obecného zpracování dat. Je to pružnější a výkonnější nástupce rozhraní MapReduce, který umožňuje efektivní spouštění datově náročných procesů, například Hive, ve velkém rozsahu. Viz [„Použití rozhraní Apache Tez pro zlepšení výkonu“ v tématu Použití Hive a HiveQL](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>YARN
Apache YARN je nová generace prostředí MapReduce (MapReduce 2.0 nebo MRv2), která podporuje scénáře zpracování dat nad rámec dávkového zpracování prostředí MapReduce s větší škálovatelností a zpracováním v reálném čase. YARN poskytuje správu prostředků a distribuované aplikační rozhraní. Úlohy MapReduce je možné spustit na technologii YARN. Přečtěte si téma <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (YARN)</a>.

### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> koordinuje procesy ve velkých distribuovaných systémech pomocí sdíleného hierarchického oboru názvů datových registrů (znodes). Znodes obsahují malé množství metainformací potřebných ke koordinaci procesů: stav, umístění, konfigurace atd. Podívejte se na příklad uzlu [ZooKeeper s clusterem HBase a vrstvou Apache Phoenix](../hbase/apache-hbase-phoenix-squirrel-linux.md). 

## <a name="programming-languages-on-hdinsight"></a>Programovací jazyky v prostředí HDInsight
Clustery – Spark, HBase, Kafka, Hadoop a další – podporují řadu programovacích jazyků, ale některé nejsou ve výchozím nastavení nainstalované. Pro knihovny, moduly nebo balíčky, které nejsou ve výchozím nastavení nainstalovány, [použijte akci skriptu pro instalaci součásti](../hdinsight-hadoop-script-actions-linux.md). 

### <a name="default-programming-language-support"></a>Výchozí podpora programovacích jazyků
Ve výchozím nastavení podporují clustery prostředí HDInsight tyto jazyky:

* Java
* Python

Další jazyky lze nainstalovat pomocí [příkazů skriptů](../hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>Jazyky Java virtual machine (JVM)
Řadu jiných jazyků než Java je možné spouštět v prostředí Java Virtual Machine (JVM); spuštění některých z těchto jazyků však může vyžadovat další součásti nainstalované v clusteru.

Clustery prostředí HDInsight podporují následující jazyky založené na JVM:

* Clojure
* Jython (Python pro jazyk Java)
* Scala

### <a name="hadoop-specific-languages"></a>Jazyky pro Hadoop
Clustery HDInsight podporují následující jazyky, které jsou určené konkrétně pro technologii Hadoop:

* Pig Latin pro úlohy Pig
* HiveQL pro úlohy Hive a SparkSQL

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard a HDInsight Premium
HDInsight nabízí cloud pro velké objemy dat ve dvou kategoriích, Standard a Premium. HDInsight Standard poskytuje cluster v podnikovém měřítku, který mohou organizace použít ke spouštění úloh s velkými objemy dat. HDInsight Premium vychází z funkcí úrovně Standard a poskytuje rozšířené analytické a bezpečnostní možnosti pro cluster prostředí HDInsight. Další informace viz [Azure HDInsight Premium](../hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)

## <a name="microsoft-business-intelligence-and-hdinsight"></a>Microsoft business intelligence a HDInsight
Známé nástroje business intelligence (BI) načítají, analyzují a vykazují data integrovaná v prostředí HDInsight buď pomocí doplňku Power Query, nebo ovladače Microsoft Hive ODBC Driver:

* [Připojení aplikace Excel k systému Hadoop pomocí Power Query:](apache-hadoop-connect-excel-power-query.md) Zjistěte, jak připojit Excel k účtu služby Azure Storage, kde se ukládají data z vašeho clusteru HDInsight, pomocí doplňku Microsoft Power Query pro Excel. Jsou vyžadovány pracovní stanice Windows. 
* [Připojení aplikace Excel k systému Hadoop pomocí ovladače ODBC Microsoft Hive](apache-hadoop-connect-excel-hive-odbc-driver.md): Zjistěte, jak importovat data z prostředí HDInsight pomocí ovladače ODBC Microsoft Hive. Jsou vyžadovány pracovní stanice Windows. 
* [Cloudová platforma Microsoft](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Získejte informace o službě Power BI pro Office 365, stáhněte si zkušební verzi serveru SQL Server a nastavte si SharePoint Server 2013 a SQL Server BI.
* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx)
* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx)


## <a name="next-steps"></a>Další kroky

* [Začínáme se systémem Hadoop v prostředí HDInsight](apache-hadoop-linux-tutorial-get-started.md): Rychlý kurz zřizování clusterů systému Hadoop HDInsight a spuštění ukázkových dotazů nástroje Hive.
* [Začínáme se Spark v prostředí HDInsight](../spark/apache-spark-jupyter-spark-sql.md): Rychlý kurz vytvoření clusteru Spark a spouštění interaktivních dotazů Spark SQL.
* [Použití R Serveru v prostředí HDInsight](../r-server/r-server-get-started.md): Zahájení práce s R Serverem v prostředí HDInsight Premium.
* [Zřízení clusterů HDInsight](../hdinsight-hadoop-provision-linux-clusters.md): Naučte se zřizovat clustery Hadoop ve službě HDInsight prostřednictvím webu Azure Portal, prostředí Azure CLI nebo Azure PowerShellu.


[component-versioning]: ../hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/