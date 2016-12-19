---
title: "Úvod k platformě Hadoop – Co je Hadoop ve službě HDInsight? | Dokumentace Microsoftu"
description: "Podívejte se na úvod k platformě Hadoop (distribuované zpracování a analýza velkých objemů dat) a součástem ekosystému Hadoop v cloudu ve službě HDInsight."
keywords: "analýzy velkých objemů dat, úvod k platformě hadoop, co je hadoop, technologická skupina hadoop, ekosystém hadoop"
services: hdinsight
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: e56a396a-1b39-43e0-b543-f2dee5b8dd3a
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/21/2016
ms.author: cgronlun
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 66200ad8688928c368146a177130f6e2fad4849b


---
# <a name="an-introduction-to-the-hadoop-ecosystem-on-azure-hdinsight"></a>Úvod k platformě Hadoop ve službě Azure HDInsight
 Tento článek obsahuje úvod k platformě Hadoop ve službě Azure HDInsight, jejímu ekosystému a velkým objemům dat. Přečtěte si informace o součástech platformy Hadoop, běžné terminologii a scénářích pro analýzy velkých objemů dat.

## <a name="what-is-hadoop-on-hdinsight"></a>Co je Hadoop ve službě HDInsight?
Hadoop označuje ekosystém open source softwaru, který představuje platformu pro distribuované zpracování, ukládání a analýzu velkých datových sad v clusterech počítačů. Azure HDInsight zpřístupňuje součásti platformy Hadoop z distribuce platformy **Hortonworks Data Platform (HDP)** dostupné v cloudu a nasazuje a zřizuje spravované clustery s vysokou spolehlivostí a dostupností.  

Apache Hadoop byl původně projekt open source pro zpracování velkých objemů dat. Na něj navázal vývoj souvisejícího softwaru a nástrojů považovaných za součást technologické skupiny Hadoop, včetně Apache Hive, Apache HBase, Apache Spark a mnoha dalších. Podrobnosti najdete v části [Přehled ekosystému Hadoop ve službě HDInsight](#overview).

## <a name="what-is-big-data"></a>Co jsou velké objemy dat?
Velký objem dat znamená jakýkoliv rozsáhlý soubor digitálních informací, od textů na Twitteru přes data ze snímačů průmyslových zařízení až po informace o prohlížení a nákupech zákazníků na webu. Velké objemy se mohou týkat historických dat (tj. uložených) nebo dat v reálném čase (tj. streamovaných přímo ze zdroje). Velké objemy dat se shromažďují v neustále narůstajícím množství, stále vyšší rychlostí a ve stále větší pestrosti formátů.

Aby bylo možné z velkých objemů dat získat použitelné informace nebo vhled do problematiky, musíte shromažďovat relevantní data a pokládat správné otázky. Je třeba také zajistit, aby byla data přístupná, vyčištěná, zanalyzovaná a prezentovaná užitečným způsobem. A zde vám právě může pomoci analýza velkých objemů dat systému Hadoop v prostředí HDInsight.

## <a name="a-nameoverviewaoverview-of-the-hadoop-ecosystem-in-hdinsight"></a><a name="overview"></a>Přehled ekosystému Hadoop ve službě HDInsight
HDInsight je cloudová distribuce rychle se rozrůstající technologické skupiny Apache Hadoop v Microsoft Azure pro analýzu velkých objemů dat. Zahrnuje implementace Apache Spark, HBase, Storm, Pig, Hive, Sqoop, Oozie, Ambari a další. HDInsight se také integruje s nástroji obchodních informací (BI), jako jsou Power BI, Excel, analytické služby SQL serveru a služby generování sestav SQL serveru.

### <a name="hadoop-hbase-spark-storm-and-customized-clusters"></a>Hadoop, HBase, Spark, Storm a přizpůsobené clustery
HDInsight poskytuje konfigurace clusterů pro Apache Hadoop, Spark, HBase nebo Storm. Nebo můžete [přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).

* **Hadoop**: poskytuje spolehlivé úložiště dat se systémem souborů [HDFS](#hdfs) a jednoduchý programovací model [MapReduce](#mapreduce) pro souběžné zpracování a analýzu dat.
* **<a target="_blank" href="http://spark.apache.org/">Apache Spark</a>**: rozhraní pro paralelní zpracování, které podporuje  zpracování v paměti pro zvýšení výkonu aplikací pro analýzu velkých objemů dat, úlohy Spark pro SQL, streamování dat a strojové učení. Viz [Přehled: co je Apache Spark v prostředí HDInsight?](hdinsight-apache-spark-overview.md)
* **<a target="_blank" href="http://hbase.apache.org/">HBase</a>**: databáze NoSQL postavená na platformě Hadoop, která umožňuje náhodný přístup a zajišťuje velkou konzistenci pro velké objemy nestrukturovaných a polostrukturovaných dat – případně až miliardy řádků krát miliony sloupců. Viz [Přehled databáze HBase v prostředí HDInsight](hdinsight-hbase-overview.md).
* **<a  target="_blank" href="https://storm.incubator.apache.org/">Apache Storm</a>**: distribuovaný výpočetní systém v reálném čase pro rychlé zpracování velkých streamů dat. Storm je poskytován jako spravovaný cluster v prostředí HDInsight. Viz [Analýza dat snímačů v reálném čase pomocí nástrojů Storm a Hadoop](hdinsight-storm-sensor-data-analysis.md).

### <a name="example-customization-scripts"></a>Příklad skriptů přizpůsobení
Akce skriptů jsou skripty, které běží při zřizování clusteru a lze je použít k instalaci dalších součástí clusteru. Pro clustery v systému Linux jde o skripty Bash.

Následují příklady skriptů poskytovaných týmem HDInsight:

* [Hue](hdinsight-hadoop-hue-linux.md): Sada webových aplikací používaných pro interakci s clusterem. Pouze clustery v Linuxu.
* [Giraph](hdinsight-hadoop-giraph-install-linux.md): Zpracování grafů pro modelování vztahů mezi věcmi nebo osobami.
* [R](hdinsight-hadoop-r-scripts-linux.md): Open-source jazyk a prostředí pro statistické výpočty používané pro strojové učení.
* [Solr](hdinsight-hadoop-solr-install-linux.md): Vyhledávací platforma v podnikovém měřítku, která umožňuje fulltextové vyhledávání dat.

Informace o vývoji vlastních akcí skriptů naleznete v tématu [Vývoj akcí skriptů v prostředí HDInsight](hdinsight-hadoop-script-actions-linux.md).

## <a name="what-are-the-hadoop-components-and-utilities"></a>Jaké jsou součásti a nástroje platformy Hadoop?
Clustery prostředí HDInsight obsahují následující součásti a nástroje.

* **[Ambari](#ambari)**: zřizování, správa, monitorování a nástroje clusterů.
* **[Avro](#avro)** (knihovna Microsoft .NET pro Avro): serializaci dat pro prostředí Microsoft .NET.
* **[Hive & HCatalog](#hive)**: dotazování typu strukturovaného dotazovacího jazyka (SQL) a vrstva správy tabulek a ukládání.
* **[Mahout](#mahout)**: pro škálovatelné aplikace strojového učení.
* **[MapReduce](#mapreduce)**: starší verze rozhraní pro distribuované zpracování a správu prostředků systému Hadoop. Viz [YARN](#yarn), rozhraní prostředků nové generace.
* **[Oozie](#oozie)**: řízení pracovních postupů.
* **[Phoenix](#phoenix)**: vrstva relační databáze nad HBase.
* **[Pig](#pig)**: jednodušší skriptování pro transformace prostředí MapReduce.
* **[Sqoop](#sqoop)**: import a export dat.
* **[Tez](#tez)**: umožňuje efektivní provádění datově náročných procesů ve velkém rozsahu.
* **[YARN](#yarn)**: součást hlavní knihovny systému Hadoop a nová generace softwarového rozhraní MapReduce.
* **[ZooKeeper](#zookeeper)**: koordinace procesů v distribuovaných systémech.

> [!NOTE]
> Informace o konkrétní součásti a informace o verzi najdete v tématu [Komponenty systému Hadoop, správa verzí a nabídky služeb v prostředí HDInsight][component-versioning].
> 
> 

### <a name="a-nameambariaambari"></a><a name="ambari"></a>Ambari
Apache Ambari slouží k zřizování, správě a monitoringu clusterů systému Apache Hadoop. Obsahuje intuitivní sadu nástrojů pro operátory a výkonnou sadu rozhraní API, které překonávají složitost systému Hadoop a zjednodušují operace s clustery. Clustery HDInsight založené na Linuxu poskytují jak webové rozhraní Ambari, tak Ambari REST API, zatímco clustery založené na Windows poskytují podmnožinu rozhraní REST API. Zobrazení Ambari v clusterech prostředí HDInsight umožňují funkce zásuvných uživatelských rozhraní.

Viz [Správa clusterů prostředí HDInsight pomocí nástroje Ambari](hdinsight-hadoop-manage-ambari.md) (pouze Linux), [Monitorování clusterů systému Hadoop v prostředí HDInsight pomocí nástroje Ambari API](hdinsight-monitor-use-ambari-api.md) a <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">odkazy na Apache Ambari API</a>.

### <a name="a-nameavroaavro-microsoft-net-library-for-avro"></a><a name="avro"></a>Avro (knihovna rozhraní Microsoft .NET pro Avro)
V knihovně Microsoft .NET pro Avro implementuje Apache Avro kompaktní binární formát pro předávání dat pro serializaci prostředí Microsoft .NET. Používá <a target="_blank" href="http://www.json.org/">JavaScript Object Notation (JSON)</a> pro definování jazykově nezávislého schématu, které poskytuje vzájemnou funkční spolupráci jazyka, což znamená, že data serializovaná v jednom jazyce lze číst v jiném. Podrobné informace o formátu lze nalézt v odkazu <a target=_"blank" href="http://avro.apache.org/docs/current/spec.html">Apache Avro Specification</a>.
Formát souborů nástroje Avro podporuje distribuovaný programovací model MapReduce. Soubory jsou „rozdělitelné“, což znamená, že můžete vyhledat libovolný bod v souboru a začít číst od určitého bloku. Postup naleznete v tématu [Serializace dat pomocí knihovny Microsoft .NET pro Avro](hdinsight-dotnet-avro-serialization.md).

### <a name="a-namehdfsahdfs"></a><a name="hdfs"></a>HDFS
Hadoop Distributed File System (HDFS) je distribuovaný systém souborů, který je spolu s prostředím MapReduce a YARN jádrem ekosystému Hadoop. HDFS je standardní systém souborů pro clustery Hadoop v HDInsight.

### <a name="a-namehiveahive-hcatalog"></a><a name="hive"></a>Hive & HCatalog
<a target="_blank" href="http://hive.apache.org/">Apache Hive</a> je software datového skladu postavený na platformě Hadoop, který umožňuje dotazování a správu rozsáhlých datových sad v distribuovaném úložišti pomocí jazyka typu SQL nazývaného HiveQL. Hive, stejně jako Pig, je abstrakcí prostředí MapReduce. Při spuštění Hive překládá dotazy do řady úloh MapReduce. Hive je koncepčně blíže systému správy relačních databází než Pig a je proto vhodnější pro použití s více strukturovanými daty. Pro nestrukturovaná data je vhodnější Pig. Viz [Použití nástroje Hive se systémem Hadoop v prostředí HDInsight](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> je vrstva tabulky a správy ukládání pro platformu Hadoop, která předkládá uživatelům relační zobrazení dat. V nástroji HCatalog můžete číst a zapisovat soubory v libovolném formátu, pro který lze napsat Hive SerDe (serializátor-deserializátor).

### <a name="a-namemahoutamahout"></a><a name="mahout"></a>Mahout
<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> je škálovatelná knihovna algoritmů strojového učení, které běží na platformě Hadoop. S využitím principů statistiky učí aplikace strojového učení systémy učit se z dat a využívat dřívější výstupy ke zjištění budoucího chování. Viz [Generování doporučení pomocí nástroje Mahout v systému Hadoop](hdinsight-mahout.md).

### <a name="a-namemapreduceamapreduce"></a><a name="mapreduce"></a>MapReduce
MapReduce je starší verze softwarového rozhraní pro Hadoop, určeného k psaní aplikací pro paralelní dávkové zpracování velkých sad dat. Úloha MapReduce rozdělí rozsáhlé datové sady a uspořádá data pro zpracování do párů hodnot klíčů.

[YARN](#yarn) je správce prostředků a aplikační rozhraní systému Hadoop nové generace a označuje se jako MapReduce 2.0. Úlohy MapReduce je možné spustit na technologii YARN.

Další informace o prostředí MapReduce naleznete v tématu <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> na stránce Hadoop Wiki.

### <a name="a-nameoozieaoozie"></a><a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> je systém koordinace pracovních postupů, který spravuje úlohy platformy Hadoop. Je integrován do zásobníku Hadoop a podporuje úlohy systému Hadoop pro MapReduce, Pig, Hive a Sqoop. Lze jej také použít při plánování úloh pro konkrétní systém, jako jsou programy v jazyce Java nebo skripty prostředí. Viz [Použití Oozie se systémem Hadoop](hdinsight-use-oozie.md).

### <a name="a-namephoenixaphoenix"></a><a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> je vrstva relační databáze nad HBase. Phoenix zahrnuje ovladač JDBC, který umožňuje uživatelům přímé dotazování a správu tabulek SQL. Phoenix přeloží dotazy a další příkazy do nativních volání rozhraní API typu NoSQL, místo použití prostředí MapReduce, což umožní rychlejší aplikace nad uloženými daty NoSQL. Viz [Použití nástrojů Apache Phoenix a SQuirreL s clustery HBase](hdinsight-hbase-phoenix-squirrel.md).

### <a name="a-namepigapig"></a><a name="pig"></a>Pig
<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a> je vysokoúrovňová platforma, která umožňuje provádět komplexní transformace MapReduce na velmi rozsáhlých datových sadách pomocí jednoduchého skriptovacího jazyka nazvaného Pig Latin. Pig překládá skripty jazyka Pig Latin, aby je bylo možno spustit v systému Hadoop. Pig Latin můžete rozšířit vytvořením uživatelsky definovaných funkcí (UDF). Viz [Použití Pig se systémem Hadoop](hdinsight-use-pig.md).

### <a name="a-namesqoopasqoop"></a><a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> je nástroj pro co možná nejefektivnější hromadný přenos dat mezi platformou Hadoop a relačními databázemi jako SQL nebo dalšími strukturovanými datovými úložišti. Viz [Použití nástroje Sqoop se systémem Hadoop](hdinsight-use-sqoop.md).

### <a name="a-nametezatez"></a><a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> je aplikační rozhraní založené na technologii Hadoop YARN, které vytváří komplexní acyklické grafy obecného zpracování dat. Je to pružnější a výkonnější nástupce rozhraní MapReduce, který umožňuje efektivní spouštění datově náročných procesů, například Hive, ve velkém rozsahu. Viz [„Použití rozhraní Apache Tez pro zlepšení výkonu“ v tématu Použití Hive a HiveQL](hdinsight-use-hive.md#usetez).

### <a name="a-nameyarnayarn"></a><a name="yarn"></a>YARN
Apache YARN je nová generace prostředí MapReduce (MapReduce 2.0 nebo MRv2), která podporuje scénáře zpracování dat nad rámec dávkového zpracování prostředí MapReduce s větší škálovatelností a zpracováním v reálném čase. YARN poskytuje správu prostředků a distribuované aplikační rozhraní. Úlohy MapReduce je možné spustit na technologii YARN.

Další informace o prostředí YARN naleznete v části <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Nová generace prostředí MapReduce systému Apache Hadoop (YARN)</a>.

### <a name="a-namezookeeperazookeeper"></a><a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> koordinuje procesy ve velkých distribuovaných systémech prostřednictvím sdíleného hierarchického oboru názvů datových registrů (znodes). Znodes obsahují malé množství metainformací potřebných ke koordinaci procesů: stav, umístění, konfigurace atd.

## <a name="programming-languages-on-hdinsight"></a>Programovací jazyky v prostředí HDInsight
Clustery prostředí HDInsight – Hadoop, HBase, Storm a Spark – podporují řadu programovacích jazyků, některé však nejsou ve výchozím nastavení nainstalovány. Pro knihovny, moduly nebo balíčky, které nejsou ve výchozím nastavení nainstalovány, použijte akci skriptu pro instalaci součásti. Viz [Vývoj akcí skriptů v prostředí HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="default-programming-language-support"></a>Výchozí podpora programovacích jazyků
Ve výchozím nastavení podporují clustery prostředí HDInsight tyto jazyky:

* Java
* Python

Další jazyky lze nainstalovat pomocí akcí skriptů: [Vývoj akcí skriptů v prostředí HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>Jazyky Java virtual machine (JVM)
Řadu jiných jazyků než Java lze spouštět s využitím nástroje Java virtual machine (JVM); spuštění některých z těchto jazyků však může vyžadovat další součásti nainstalované v clusteru.

Clustery prostředí HDInsight podporují následující jazyky založené na JVM:

* Clojure
* Jython (Python pro jazyk Java)
* Scala

### <a name="hadoop-specific-languages"></a>Jazyky pro Hadoop
Clustery HDInsight podporují následující jazyky, které jsou určené konkrétně pro ekosystém Hadoop:

* Pig Latin pro úlohy Pig
* HiveQL pro úlohy Hive a SparkSQL

## <a name="a-nameadvantageaadvantages-of-hadoop-in-the-cloud"></a><a name="advantage"></a>Výhody platformy Hadoop v cloudu
Jako součást cloudového ekosystému Azure nabízí systém Hadoop v prostředí HDInsight řadu výhod, mezi něž patří:

* Automatické zřizování clusterů systému Hadoop. Vytváření clusterů prostředí HDInsight je mnohem snazší než ruční konfigurace clusterů systému Hadoop. Podrobnosti najdete v tématu [Zřizování clusterů systému Hadoop v prostředí HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
* Moderní součásti systému Hadoop. Podrobnosti najdete v tématu [Komponenty systému Hadoop, správa verzí a nabídky služeb v prostředí HDInsight][component-versioning].
* Vysoká dostupnost a spolehlivost clusterů. Podrobnosti viz [Dostupnost a spolehlivost clusterů systému Hadoop v prostředí HDInsight](hdinsight-high-availability-linux.md).
* Efektivní a ekonomické ukládání dat umožňuje Azure Blob, volba kompatibilní se systémem Hadoop. Podrobnosti viz [Použití úložiště Azure Blob se systémem Hadoop v prostředí HDInsight](hdinsight-hadoop-use-blob-storage.md).
* Integrace s dalšími službami Azure, včetně [Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/) a [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).
* Další velikosti a typy virtuálních počítačů pro spuštění clusterů HDInsight. Podrobnosti najdete v tématu [Komponenty systému Hadoop, správa verzí a nabídky služeb v prostředí HDInsight][component-versioning].
* Škálování clusterů. Škálování clusterů umožňuje změnit počet uzlů spuštěného clusteru v prostředí HDInsight bez nutnosti jej odstranit nebo znovu vytvořit.
* Podpora služby Virtual Network. Clustery prostředí HDInsight lze použít se službou Azure Virtual Network pro podporu izolace cloudových prostředků nebo hybridních scénářů, propojujících tyto cloudové prostředky s těmi, které jsou ve vašem datovém centru.
* Nízké vstupní náklady Spusťte si [bezplatnou zkušební verzi](https://azure.microsoft.com/free/) nebo se podívejte na [podrobnosti o cenách prostředí HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

Další informace o výhodách platformy Hadoop v prostředí HDInsight najdete v tématu [Funkce Azure pro HDInsight][marketing-page].

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard a HDInsight Premium
HDInsight nabízí cloud pro velké objemy dat ve dvou kategoriích, Standard a Premium. HDInsight Standard poskytuje cluster v podnikovém měřítku, který mohou organizace použít ke spouštění úloh s velkými objemy dat. HDInsight Premium z toho vychází a poskytuje rozšířené analytické a bezpečnostní možnosti pro cluster prostředí HDInsight. Další informace viz [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)

## <a name="a-idresourcesaresources-for-learning-more-about-big-data-analysis-hadoop-and-hdinsight"></a><a id="resources"></a>Zdroje dalších informací o analýze velkých objemů dat, systému Hadoop a službě HDInsight
Poznatky z tohoto úvodu do systému Hadoop v cloudu a analýzy velkých objemů dat si můžete doplnit prostřednictvím níže uvedených zdrojů.

### <a name="hadoop-documentation-for-hdinsight"></a>Dokumentace systému Hadoop pro HDInsight
* [Dokumentace prostředí HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/): Stránka dokumentace pro Hadoop v prostředí Azure HDInsight s odkazy na články, videa a další materiály.
* [Začínáme se systémem Hadoop v prostředí HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): Rychlý kurz zřizování clusterů systému Hadoop HDInsight a spuštění ukázkových dotazů nástroje Hive.
* [Začínáme se Spark v prostředí HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md): Rychlý kurz vytvoření clusteru Spark a spouštění interaktivních dotazů Spark SQL.
* [Použití R Serveru v prostředí HDInsight](hdinsight-hadoop-r-server-get-started.md): Zahájení práce s R Serverem v prostředí HDInsight Premium.
* [Zřízení clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Naučte se zřizovat clustery Hadoop ve službě HDInsight prostřednictvím webu Azure Portal, prostředí Azure CLI nebo Azure PowerShellu.

### <a name="apache-hadoop"></a>Apache Hadoop
* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: Přečtěte si další informace o softwarové knihovně Apache Hadoop, platformě, která umožňuje distribuované zpracování rozsáhlých datových sad napříč clustery počítačů.
* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">HDFS</a>: Přečtěte si další informace o architektuře a designu distribuovaného systému souborů Hadoop, systému primárního úložiště používaného aplikacemi Hadoop.
* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">Kurz MapReduce</a>: Přečtěte si další informace o programovací platformě pro psaní aplikací Hadoop umožňujících rychlé paralelní zpracování velkých objemů dat na velkých clusterech výpočetních uzlů.

### <a name="microsoft-business-intelligence"></a>Microsoft business intelligence
Známé nástroje business intelligence (BI) – například aplikace Excel, PowerPivot, SQL Server Analysis Services a SQL Server Reporting Services – slouží k načítání, analýze a generování sestav dat integrovaných v prostředí HDInsight pomocí doplňku Power Query nebo ovladače ODBC Microsoft Hive.

Tyto nástroje BI vám mohou pomoci při analýze velkých objemů dat:

* [Připojení aplikace Excel k systému Hadoop pomocí Power Query](hdinsight-connect-excel-power-query.md): Zjistěte, jak připojit Excel k účtu úložiště Azure, kde se ukládají data přidružená k vašemu clusteru prostředí HDInsight pomocí aplikace Microsoft Power Query pro Excel. Jsou vyžadovány pracovní stanice Windows. Funguje s clusterem založeným na systému Windows nebo Linux.
* [Připojení aplikace Excel k systému Hadoop pomocí ovladače ODBC Microsoft Hive](hdinsight-connect-excel-hive-odbc-driver.md): Zjistěte, jak importovat data z prostředí HDInsight pomocí ovladače ODBC Microsoft Hive. Jsou vyžadovány pracovní stanice Windows. Funguje s clusterem založeným na systému Windows nebo Linux.
* [Cloudová platforma Microsoft](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Získejte informace o službě Power BI pro Office 365, stáhněte si zkušební verzi serveru SQL Server a nastavte si SharePoint Server 2013 a SQL Server BI.
* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx).
* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx).

[marketing-page]: https://azure.microsoft.com/services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/



<!--HONumber=Dec16_HO1-->


