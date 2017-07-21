---
title: Co jsou Azure HDInsight, technologie Hadoop a clustery? | Dokumentace Microsoftu
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
ms.date: 05/11/2017
ms.author: cgronlun
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: 7d1f52550ca2b50e9536606d0f0099f4bf0f1e5e
ms.contentlocale: cs-cz
ms.lasthandoff: 05/18/2017


---
# Úvod do Azure HDInsight, technologie a clusterů Hadoop
<a id="introduction-to-azure-hdinsight-the-hadoop-technology-stack-and-hadoop-clusters" class="xliff"></a>
 Tento článek obsahuje úvod do Azure HDInsight, cloudové distribuce založené na technologii Hadoop. Také se v něm dozvíte, co je cluster Hadoop a kdy ho můžete použít. 

## Co je HDInsight a technologie Hadoop?
<a id="what-is-hdinsight-and-the-hadoop-technology-stack" class="xliff"></a> 
Azure HDInsight je cloudová distribuce komponent Hadoop z platformy **Hortonworks Data Platform (HDP)**. Původní open source architekturou pro distribuované zpracování a analýzu velkých datových sad v počítačových clusterech byl systém [Apache Hadoop](http://hadoop.apache.org/). 

Díky prostředí HDInsight se technologie Hadoop snáze používají, protože přinášejí:

*    Jednodušší instalaci a konfiguraci. Viz [Vytváření clusterů Hadoop ve službě HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
*    Vysokou dostupnost a spolehlivost. Viz [Dostupnost a spolehlivost služby HDInsight](hdinsight-high-availability-linux.md).
*    Zabezpečení a zásady správného řízení díky integraci se službou Active Directory. Viz [Clustery připojené k doméně](hdinsight-domain-joined-introduction.md).
*    Dynamické škálování bez přerušení úloh
*   Aktualizace komponent a aktuální verze Přečtěte si téma [Komponenty a verze systému Hadoop ve prostředí HDInsight][component-versioning].
*   Integrace s dalšími službami Azure, včetně [Web Apps](https://docs.microsoft.com/azure/app-service-web/) a [SQL Database](https://docs.microsoft.com/azure/sql-database/)

Součástí technologie Hadoop je potřebný software a nástroje, jako je Apache Hive, HBase, Spark, Kafka a řada dalších. Další informace o platformě Hadoop v prostředí HDInsight najdete v tématu [Funkce Azure pro HDInsight](https://azure.microsoft.com/services/hdinsight/).

## Co je cluster Hadoop a kdy se používá?
<a id="what-is-a-hadoop-cluster-and-when-do-you-use-it" class="xliff"></a>
Pojem *Hadoop* také označuje typ clusteru, který má tyto vlastnosti:

* Distribuovaný souborový systém Hadoop (HDFS)
* YARN pro plánování úloh a správu prostředků
* MapReduce pro paralelní zpracování
  
Clustery Hadoop se nejčastěji používají pro dávkové zpracování uložených dat. Jiné druhy clusterů v prostředí HDInsight mají další funkce, například rychlejší zpracování dat v paměti nebo zpracování dat při streamování fronty zpráv. Podrobnosti najdete v tématu [Typy clusterů ve službě HDInsight](#overview).

## Co jsou velké objemy dat?
<a id="what-is-big-data" class="xliff"></a>
Velké objemy dat označují všechny rozsáhlé soubory s digitálními informacemi, jako je například:

* Informační kanál sítě Twitter
* Data senzorů v průmyslových zařízeních
* Údaje o aktivitě zákazníka na webové stránce

Velké objemy se mohou týkat historických dat (tj. uložených) nebo dat v reálném čase (tj. streamovaných ze zdroje). Velké objemy dat se shromažďují v neustále narůstajícím množství, stále vyšší rychlostí a ve stále větší pestrosti formátů.

## <a name="overview"></a>Typy clusterů ve službě HDInsight
HDInsight je cloudová distribuce rychle se rozrůstající technologické skupiny Apache Hadoop v Microsoft Azure pro analýzu velkých objemů dat. Její součástí jsou specifické typy clusterů a možnosti přizpůsobení clusterů, jako je například přidávání komponent, nástrojů a jazyků.

### Spark, Kafka, interaktivní Hive, HBase, přizpůsobené clustery a další typy clusterů
<a id="spark-kafka-interactive-hive-hbase-customized-and-other-cluster-types" class="xliff"></a>
HDInsight nabízí následující typy clusteru:

* **[Apache Hadoop](https://wiki.apache.org/hadoop)**: Používá [HDFS](#hdfs), správu prostředků [YARN](#yarn) a jednoduchý programovací model [MapReduce](#mapreduce) pro paralelní zpracování a analýzu dat.
* **[Apache Spark](http://spark.apache.org/)**: rozhraní pro paralelní zpracování, které podporuje  zpracování v paměti pro zvýšení výkonu aplikací pro analýzu velkých objemů dat, úlohy Spark pro SQL, streamování dat a strojové učení. Přečtěte si téma [Co je Apache Spark v prostředí HDInsight?](hdinsight-apache-spark-overview.md)
* **[Apache HBase](http://hbase.apache.org/)**: Databáze NoSQL postavená na systému Hadoop, která umožňuje náhodný přístup a zajišťuje velkou konzistenci pro velké objemy nestrukturovaných a částečně strukturovaných dat – potenciálně až miliardy řádků krát miliony sloupců. Přečtěte si téma [Co je HBase v HDInsight?](hdinsight-hbase-overview.md)
* **[Microsoft R Server](https://msdn.microsoft.com/microsoft-r/rserver)**: Server pro hostování a správu paralelních, distribuovaných procesů R. Poskytuje datovým vědcům, statistikům a programátorům v R přístup ke škálovatelným, distribuovaným analytickým metodám služby HDInsight na vyžádání. Viz [Přehled R Serveru ve službě HDInsight](hdinsight-hadoop-r-server-overview.md).
* **[Apache Storm](https://storm.incubator.apache.org/)**: distribuovaný výpočetní systém v reálném čase pro rychlé zpracování velkých streamů dat. Storm je poskytován jako spravovaný cluster v prostředí HDInsight. Viz [Analýza dat snímačů v reálném čase pomocí nástrojů Storm a Hadoop](hdinsight-storm-sensor-data-analysis.md).
* **[Apache Interactive Hive ve verzi Preview (také označované LLAP (Live Long and Process))](https://cwiki.apache.org/confluence/display/Hive/LLAP)**: Ukládání do mezipaměti pro interaktivní a rychlejší dotazy Hivu. Viz [Použití Interactive Hivu ve službě HDInsight](hdinsight-hadoop-use-interactive-hive.md).
* **[Apache Kafka](https://kafka.apache.org/)**: Open source platforma sloužící k vytváření aplikací a kanálů pro streamovaná data. Kafka také poskytuje funkce propojující fronty zpráv, pomocí kterých můžete publikovat datové streamy a přihlašovat se k jejich odběru. Viz [Úvod k Apache Kafka ve službě HDInsight](hdinsight-apache-kafka-introduction.md).
* **[Clustery připojené k doméně ve verzi Preview](hdinsight-domain-joined-introduction.md)**: Cluster připojený k doméně služby Active Directory umožňující řízení přístupu a poskytující zásady správného řízení pro data.
* **[Vlastní clustery s akcemi skriptů](hdinsight-hadoop-customize-cluster-linux.md)**: Clustery se skripty, které se spouštějí během zřizování a instalují další komponenty.

### Příklady skriptů pro přizpůsobení clusterů
<a id="example-cluster-customization-scripts" class="xliff"></a>
Akce skriptů jsou skripty Bash v systému Linux, které se spouštějí během zřizování clusteru a lze je použít k instalaci dalších komponent clusteru. 

Následují příklady skriptů poskytovaných týmem HDInsight:

* **[Hue](hdinsight-hadoop-hue-linux.md)**: Sada webových aplikací používaných pro interakci s clusterem. Pouze clustery v Linuxu.
* **[Giraph](hdinsight-hadoop-giraph-install-linux.md)**: Zpracování grafů pro modelování vztahů mezi věcmi nebo osobami.
* **[Solr](hdinsight-hadoop-solr-install-linux.md)**: Vyhledávací podniková platforma, která umožňuje fulltextové vyhledávání dat.

Informace o vývoji vlastních akcí skriptů naleznete v tématu [Vývoj akcí skriptů v prostředí HDInsight](hdinsight-hadoop-script-actions-linux.md).

## Komponenty a nástroje v clusterech HDInsight
<a id="components-and-utilities-on-hdinsight-clusters" class="xliff"></a>
Clustery prostředí HDInsight obsahují následující součásti a nástroje:

* **[Ambari](#ambari)**: zřizování, správa, monitorování a nástroje clusterů.
* **[Avro](#avro)** (knihovna Microsoft .NET pro Avro): serializaci dat pro prostředí Microsoft .NET. 
* **[Hive & HCatalog](#hive)**: Dotazování typu strukturovaného dotazovacího jazyka (SQL) a vrstva správy tabulek a ukládání.
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
Viz [Správa clusterů HDInsight pomocí zobrazení Ambari](hdinsight-hadoop-manage-ambari.md) a <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Rozhraní API v zobrazení Apache Ambari – referenční informace</a>.

### <a name="avro"></a>Avro (knihovna rozhraní Microsoft .NET pro Avro)
V knihovně Microsoft .NET pro Avro implementuje Apache Avro kompaktní binární formát pro předávání dat pro serializaci prostředí Microsoft .NET. Definuje jazykově nezávislé schéma, aby data sériovaná v jednom jazyce bylo možné číst v jiném jazyce. Podrobné informace o formátu lze nalézt v odkazu <a target=_"blank" href="http://avro.apache.org/docs/current/spec.html">Apache Avro Specification</a>. Formát souborů Avro podporuje distribuovaný programovací model MapReduce: Soubory jsou „dělitelné“, což znamená, že v nich můžete vyhledat libovolný bod a začít se čtením dat od konkrétního bloku. Postup naleznete v tématu [Serializace dat pomocí knihovny Microsoft .NET pro Avro](hdinsight-dotnet-avro-serialization.md). Do budoucna se plánuje podpora clusterů založených na Linuxu.

### <a name="hdfs"></a>HDFS
Distribuovaný souborů systém Hadoop (HDFS) je spolu s nástroji YARN a MapReduce základem technologie Hadoop. Jde o standardní systém souborů pro clustery Hadoop v HDInsight. Přečtěte si téma [Dotazování dat z úložiště kompatibilního se systémem HDFS](hdinsight-hadoop-use-blob-storage.md).

### <a name="hive"></a>Hive & HCatalog
<a target="_blank" href="http://hive.apache.org/">Apache Hive</a> je software datového skladu postavený na platformě Hadoop, který umožňuje dotazování a správu rozsáhlých datových sad v distribuovaném úložišti pomocí jazyka typu SQL nazývaného HiveQL. Hive je stejně jako Pig abstrakce vycházející z nástroje MapReduce a překládá dotazy do řady úloh MapReduce. Hive má blíže k systému pro správu relačních databází a používá se u strukturovanějších dat. Pro nestrukturovaná data je vhodnější Pig. Viz [Použití nástroje Hive se systémem Hadoop v prostředí HDInsight](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> je vrstva správy úložiště a tabulek pro systém Hadoop, která poskytuje relační zobrazení dat. V nástroji HCatalog můžete číst a zapisovat soubory v libovolném formátu, pro který lze napsat Hive SerDe (serializátor-deserializátor).

### <a name="mahout"></a>Mahout
<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> představuje knihovnu algoritmů strojového učení, které se dají spouštět v systému Hadoop. S využitím principů statistiky učí aplikace strojového učení systémy učit se z dat a využívat dřívější výstupy ke zjištění budoucího chování. Viz [Generování doporučení pomocí nástroje Mahout v systému Hadoop](hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
MapReduce je starší verze softwarového rozhraní pro Hadoop, určeného k psaní aplikací pro paralelní dávkové zpracování velkých sad dat. Úloha MapReduce rozdělí rozsáhlé datové sady a uspořádá data pro zpracování do párů hodnot klíčů. Úlohy MapReduce je možné spustit na technologii [YARN](#yarn). Další informace najdete na wikiwebu systému Hadoop v tématu <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a>.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> je systém koordinace pracovních postupů, který spravuje úlohy platformy Hadoop. Je integrován do zásobníku Hadoop a podporuje úlohy systému Hadoop pro MapReduce, Pig, Hive a Sqoop. Lze jej také použít při plánování úloh pro konkrétní systém, jako jsou programy v jazyce Java nebo skripty prostředí. Viz [Použití Oozie se systémem Hadoop](hdinsight-use-oozie-linux-mac.md).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> je vrstva relační databáze nad HBase. Phoenix zahrnuje ovladač JDBC, který umožňuje přímé dotazování a správu tabulek SQL. Phoenix přeloží dotazy a další příkazy do nativních volání rozhraní API typu NoSQL, místo použití prostředí MapReduce, což umožní rychlejší aplikace nad uloženými daty NoSQL. Viz [Použití nástrojů Apache Phoenix a SQuirreL s clustery HBase](hdinsight-hbase-phoenix-squirrel.md).

### <a name="pig"></a>Pig
<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a> je vysokoúrovňová platforma, která umožňuje provádět komplexní transformace MapReduce na rozsáhlých datových sadách pomocí jednoduchého skriptovacího jazyka nazvaného Pig Latin. Pig překládá skripty jazyka Pig Latin, aby je bylo možno spustit v systému Hadoop. Pig Latin můžete rozšířit vytvořením uživatelsky definovaných funkcí (UDF). Viz [Použití Pig se systémem Hadoop](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> je nástroj pro co nejefektivnější hromadný přenos dat mezi platformou Hadoop a relačními databázemi jako SQL nebo dalšími strukturovanými datovými úložišti. Viz [Použití nástroje Sqoop se systémem Hadoop](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> je aplikační rozhraní založené na technologii Hadoop YARN, které vytváří komplexní acyklické grafy obecného zpracování dat. Je to pružnější a výkonnější nástupce rozhraní MapReduce, který umožňuje efektivní spouštění datově náročných procesů, například Hive, ve velkém rozsahu. Viz [„Použití rozhraní Apache Tez pro zlepšení výkonu“ v tématu Použití Hive a HiveQL](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>YARN
Apache YARN je nová generace prostředí MapReduce (MapReduce 2.0 nebo MRv2), která podporuje scénáře zpracování dat nad rámec dávkového zpracování prostředí MapReduce s větší škálovatelností a zpracováním v reálném čase. YARN poskytuje správu prostředků a distribuované aplikační rozhraní. Úlohy MapReduce je možné spustit na technologii YARN. Přečtěte si téma <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (YARN)</a>.

### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> koordinuje procesy ve velkých distribuovaných systémech pomocí sdíleného hierarchického oboru názvů datových registrů (znodes). Znodes obsahují malé množství metainformací potřebných ke koordinaci procesů: stav, umístění, konfigurace atd. Podívejte se na příklad uzlu [ZooKeeper s clusterem HBase a vrstvou Apache Phoenix](hdinsight-hbase-phoenix-squirrel-linux.md). 

## Programovací jazyky v prostředí HDInsight
<a id="programming-languages-on-hdinsight" class="xliff"></a>
Clustery – Spark, HBase, Kafka, Hadoop a další – podporují řadu programovacích jazyků, ale některé nejsou ve výchozím nastavení nainstalované. Pro knihovny, moduly nebo balíčky, které nejsou ve výchozím nastavení nainstalovány, [použijte akci skriptu pro instalaci součásti](hdinsight-hadoop-script-actions-linux.md). 

### Výchozí podpora programovacích jazyků
<a id="default-programming-language-support" class="xliff"></a>
Ve výchozím nastavení podporují clustery prostředí HDInsight tyto jazyky:

* Java
* Python

Další jazyky lze nainstalovat pomocí [příkazů skriptů](hdinsight-hadoop-script-actions-linux.md).

### Jazyky Java virtual machine (JVM)
<a id="java-virtual-machine-jvm-languages" class="xliff"></a>
Řadu jiných jazyků než Java lze spouštět s využitím nástroje Java virtual machine (JVM); spuštění některých z těchto jazyků však může vyžadovat další součásti nainstalované v clusteru.

Clustery prostředí HDInsight podporují následující jazyky založené na JVM:

* Clojure
* Jython (Python pro jazyk Java)
* Scala

### Jazyky pro Hadoop
<a id="hadoop-specific-languages" class="xliff"></a>
Clustery HDInsight podporují následující jazyky, které jsou určené konkrétně pro technologii Hadoop:

* Pig Latin pro úlohy Pig
* HiveQL pro úlohy Hive a SparkSQL

## HDInsight Standard a HDInsight Premium
<a id="hdinsight-standard-and-hdinsight-premium" class="xliff"></a>
HDInsight nabízí cloud pro velké objemy dat ve dvou kategoriích, Standard a Premium. HDInsight Standard poskytuje cluster v podnikovém měřítku, který mohou organizace použít ke spouštění úloh s velkými objemy dat. HDInsight Premium vychází z funkcí úrovně Standard a poskytuje rozšířené analytické a bezpečnostní možnosti pro cluster prostředí HDInsight. Další informace viz [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)

## Microsoft business intelligence a HDInsight
<a id="microsoft-business-intelligence-and-hdinsight" class="xliff"></a>
Známé nástroje business intelligence (BI) načítají, analyzují a vykazují data integrovaná v prostředí HDInsight buď pomocí doplňku Power Query, nebo ovladače Microsoft Hive ODBC Driver:

* [Připojení aplikace Excel k systému Hadoop pomocí Power Query](hdinsight-connect-excel-power-query.md): Zjistěte, jak připojit Excel k účtu úložiště Azure, kde se ukládají data přidružená k vašemu clusteru prostředí HDInsight pomocí aplikace Microsoft Power Query pro Excel. Jsou vyžadovány pracovní stanice Windows. 
* [Připojení aplikace Excel k systému Hadoop pomocí ovladače ODBC Microsoft Hive](hdinsight-connect-excel-hive-odbc-driver.md): Zjistěte, jak importovat data z prostředí HDInsight pomocí ovladače ODBC Microsoft Hive. Jsou vyžadovány pracovní stanice Windows. 
* [Cloudová platforma Microsoft](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Získejte informace o službě Power BI pro Office 365, stáhněte si zkušební verzi serveru SQL Server a nastavte si SharePoint Server 2013 a SQL Server BI.
* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx)
* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx)


## Další kroky
<a id="next-steps" class="xliff"></a>

* [Začínáme se systémem Hadoop v prostředí HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): Rychlý kurz zřizování clusterů systému Hadoop HDInsight a spuštění ukázkových dotazů nástroje Hive.
* [Začínáme se Spark v prostředí HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md): Rychlý kurz vytvoření clusteru Spark a spouštění interaktivních dotazů Spark SQL.
* [Použití R Serveru v prostředí HDInsight](hdinsight-hadoop-r-server-get-started.md): Zahájení práce s R Serverem v prostředí HDInsight Premium.
* [Zřízení clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Naučte se zřizovat clustery Hadoop ve službě HDInsight prostřednictvím webu Azure Portal, prostředí Azure CLI nebo Azure PowerShellu.


[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/
