<properties
    pageTitle="Co je Hadoop v cloudu? Úvod do prostředí HDInsight | Microsoft Azure"
    description="Co je Hadoop v cloudu a jak je spravován v prostředí HDInsight? Úvod do komponent systému Hadoop a analýzy velkých objemů dat."
    keywords="big data analysis,introduction to hadoop,what is hadoop,hadoop in the cloud"
    services="hdinsight"
    documentationCenter=""
    authors="cjgronlund"
    manager="paulettm"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="03/29/2016"
   ms.author="cgronlun"/>


# Co je Hadoop v cloudu? Úvod do komponent systému Hadoop v prostředí HDInsight pro analýzu velkých objemů dat

iSeznamte se se systémem Hadoop, jeho ekosystémem a velkými objemy dat v prostředí Azure HDInsight: Co je Hadoop v prostředí HDInsight a jaké jsou komponenty systému Hadoop, obvyklá terminologie a scénáře analýzy velkých objemů dat? Také se dozvíte o kurzech systému Hadoop, dokumentaci a prostředcích pro používání systému Hadoop v cloudu v prostředí HDInsight.

## Co je Hadoop v cloudu v prostředí HDInsight?

Azure HDInsight nasazuje a zřizuje spravované clustery systému Apache Hadoop v cloudu, což představuje softwarové rozhraní určené pro zpracování, analýzu a generování sestav velkých objemů dat s vysokou spolehlivostí a dostupností. HDInsight využívá distribuci systému Hadoop **Hortonworks Data Platform (HDP)**. Systémem Hadoop se často myslí celý ekosystém součástí systému Hadoop, což zahrnuje i Apache HBase, Apache Spark a Apache Storm, jakož i další technologie shrnuté pod pojmem Hadoop. Podrobnosti jsou uvedeny níže v části [Přehled ekosystému Hadoop v prostředí HDInsight](#overview).


## Co jsou velké objemy dat?
Velkými objemy dat se myslí data shromažďovaná v neustále narůstajícím množství, stále vyšší rychlostí a ve stále větší pestrosti nestrukturovaných formátů a proměnlivých sémantických kontextů.

Velký objem dat znamená jakýkoliv rozsáhlý soubor digitálních informací, od textů na Twitteru přes data ze snímačů průmyslových zařízení až po informace o prohlížení a nákupech zákazníků v online katalogu. Velké objemy se mohou týkat historických dat (tj. uložených) nebo dat v reálném čase (tj. streamovaných přímo ze zdroje).

Aby vám velké objemy dat poskytly využitelné informace nebo vhled, musíte nejen shromáždit relevantní data a klást správné otázky, ale data musí být také přístupná, vyčištěná, analyzovaná a potom prezentovaná užitečným způsobem. A zde vám právě může pomoci analýza velkých objemů dat systému Hadoop v prostředí HDInsight.


## <a name="overview"></a>Přehled ekosystému Hadoop v prostředí HDInsight

HDInsight je cloudová implementace rychle se zvětšujícího zásobníku technologie Apache Hadoop v Microsoft Azure, která je vhodným řešením pro analýzu velkých objemů dat. Zahrnuje implementace Apache Spark, HBase, Storm, Pig, Hive, Sqoop, Oozie, Ambari a další. HDInsight se také integruje s nástroji obchodních informací (BI), jako jsou Power BI, Excel, analytické služby SQL serveru a služby generování sestav SQL serveru.

### Clustery v Linuxu

Azure HDInsight nasazuje a zřizuje clustery systému Hadoop v cloudu v systému **Linux**. Podrobnosti jsou v následující tabulce.

Kategorie | Hadoop v systému Linux
---------| -------------------
**Operační systém clusteru** | Ubuntu 12.04 Long Term Support (LTS)
**Typ clusteru** | Hadoop, Spark, HBase, Storm
**Nasazení** | Portál Azure, Azure CLI, Azure PowerShell
**Uživatelské rozhraní clusteru** | Ambari
**Vzdálený přístup** | Secure Shell (SSH), REST API, ODBC, JDBC



### Hadoop, HBase, Spark, Storm a přizpůsobené clustery

HDInsight poskytuje konfigurace clusterů pro Apache Hadoop, Spark, HBase nebo Storm. Nebo můžete [přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).

* **Hadoop** (úlohy dotazů): poskytuje spolehlivé úložiště dat se systémem souborů [HDFS](#HDFS) a jednoduchý programovací model [MapReduce](#mapreduce) pro souběžné zpracování a analýzu dat.

* **<a target="_blank" href="http://spark.apache.org/">Apache Spark</a>**: rozhraní pro paralelní zpracování, které podporuje  zpracování v paměti pro zvýšení výkonu aplikací pro analýzu velkých objemů dat, úlohy Spark pro SQL, streamování dat a strojové učení. Viz [Přehled: co je Apache Spark v prostředí HDInsight?](hdinsight-apache-spark-overview.md)

* **<a target="_blank" href="http://hbase.apache.org/">HBase</a>** (úlohy NoSQL): databáze NoSQL postavená na systému Hadoop, která umožňuje náhodný přístup a velkou konzistenci pro velké objemy nestrukturovaných a polostrukturovaných dat – případně až miliardy řádků krát miliony sloupců. Viz [Přehled databáze HBase v prostředí HDInsight](hdinsight-hbase-overview.md).

* **<a  target="_blank" href="https://storm.incubator.apache.org/">Apache Storm</a>** (úlohy streamování): distribuovaný výpočetní systém v reálném čase pro rychlé zpracování velkých datových proudů. Storm je poskytován jako spravovaný cluster v prostředí HDInsight. Viz [Analýza dat snímačů v reálném čase pomocí nástrojů Storm a Hadoop](hdinsight-storm-sensor-data-analysis.md).

#### Příklad skriptů přizpůsobení

Akce skriptů jsou skripty, které běží při zřizování clusteru a lze je použít k instalaci dalších součástí clusteru. Pro clustery v systému Linux jde o skripty Bash.

Následují příklady skriptů poskytovaných týmem HDInsight:

* [Hue](hdinsight-hadoop-hue-linux.md): Sada webových aplikací používaných pro interakci s clusterem. Pouze clustery v Linuxu.

* [Giraph](hdinsight-hadoop-giraph-install-linux.md): Zpracování grafů pro modelování vztahů mezi věcmi nebo osobami.

* [R](hdinsight-hadoop-r-scripts-linux.md): Open-source jazyk a prostředí pro statistické výpočty používané pro strojové učení.

* [Solr](hdinsight-hadoop-solr-install-linux.md): Vyhledávací platforma v podnikovém měřítku, která umožňuje fulltextové vyhledávání dat.

Informace o vývoji vlastních akcí skriptů naleznete v tématu [Vývoj akcí skriptů v prostředí HDInsight](hdinsight-hadoop-script-actions-linux.md).

## HDInsight Standard a HDInsight Premium

HDInsight nabízí cloud pro velké objemy dat ve dvou kategoriích, Standard a Premium. HDInsight Standard poskytuje cluster v podnikovém měřítku, který mohou organizace použít ke spouštění úloh s velkými objemy dat. HDInsight Premium z toho vychází a poskytuje rozšířené analytické a bezpečnostní možnosti pro cluster prostředí HDInsight. Další informace viz [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)

## Jako jsou komponenty a nástroje systému Hadoop?

Clustery prostředí HDInsight obsahují následující součásti a nástroje.

* **[Ambari](#ambari)**: zřizování, správa, monitorování a nástroje clusterů.

* **[Avro](#avro)** (knihovna Microsoft .NET pro Avro): serializaci dat pro prostředí Microsoft .NET.

* **[Hive & HCatalog](#hive)**: dotazování typu strukturovaného dotazovacího jazyka (SQL) a vrstva správy tabulek a ukládání.

* **[Mahout](#mahout)**: strojové učení.

* **[MapReduce](#mapreduce)**: starší verze rozhraní pro distribuované zpracování a správu prostředků systému Hadoop. Viz [YARN](#yarn), rozhraní prostředků nové generace.

* **[Oozie](#oozie)**: řízení pracovních postupů.

* **[Phoenix](#phoenix)**: vrstva relační databáze nad HBase.

* **[Pig](#pig)**: jednodušší skriptování pro transformace prostředí MapReduce.

* **[Sqoop](#sqoop)**: import a export dat.

* **[Tez](#tez)**: umožňuje efektivní provádění datově náročných procesů ve velkém rozsahu.

* **[YARN](#yarn)**: součást hlavní knihovny systému Hadoop a nová generace softwarového rozhraní MapReduce.

* **[ZooKeeper](#zookeeper)**: koordinace procesů v distribuovaných systémech.

> [AZURE.NOTE] Informace o konkrétní součásti a informace o verzi naleznete v tématu [Co je nového ve verzích clusterů systému Hadoop poskytovaných v prostředí HDInsight?][component-versioning]

### <a name="ambari"></a>Ambari

Apache Ambari slouží k zřizování, správě a monitoringu clusterů systému Apache Hadoop. Obsahuje intuitivní sadu nástrojů pro operátory a výkonnou sadu rozhraní API, které překonávají složitost systému Hadoop a zjednodušují operace s clustery. Clustery HDInsight založené na Linuxu poskytují jak webové rozhraní Ambari, tak Ambari REST API, zatímco clustery založené na Windows poskytují podmnožinu rozhraní REST API. Zobrazení Ambari v clusterech prostředí HDInsight umožňují funkce zásuvných uživatelských rozhraní.

Viz [Správa clusterů prostředí HDInsight pomocí nástroje Ambari](hdinsight-hadoop-manage-ambari.md) (pouze Linux), [Monitorování clusterů systému Hadoop v prostředí HDInsight pomocí nástroje Ambari API](hdinsight-monitor-use-ambari-api.md) a <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">odkazy na Apache Ambari API</a>.

### <a name="avro"></a>Avro (knihovna rozhraní Microsoft .NET pro Avro)

V knihovně Microsoft .NET pro Avro implementuje Apache Avro kompaktní binární formát pro předávání dat pro serializaci prostředí Microsoft .NET. Používá <a target="_blank" href="http://www.json.org/">JavaScript Object Notation (JSON)</a> pro definování jazykově nezávislého schématu, které poskytuje vzájemnou funkční spolupráci jazyka, což znamená, že data serializovaná v jednom jazyce lze číst v jiném. Podrobné informace o formátu lze nalézt v odkazu <a target=_"blank" href="http://avro.apache.org/docs/current/spec.html">Apache Avro Specification</a>.
Formát souborů nástroje Avro podporuje distribuovaný programovací model MapReduce. Soubory jsou „rozdělitelné“, což znamená, že můžete vyhledat libovolný bod v souboru a začít číst od určitého bloku. Postup naleznete v tématu [Serializace dat pomocí knihovny Microsoft .NET pro Avro](hdinsight-dotnet-avro-serialization.md).


### <a name="hdfs"></a>HDFS

Hadoop Distributed File System (HDFS) je distribuovaný systém souborů, který je spolu s prostředím MapReduce a YARN jádrem ekosystému Hadoop. HDFS je standardní systém souborů pro clustery Hadoop v HDInsight.

### <a name="hive"></a>Hive & HCatalog

<a target="_blank" href="http://hive.apache.org/">Apache Hive</a> je software datového skladu postavený na systému Hadoop, který umožňuje dotazování a správu rozsáhlých datových sad v distribuovaném úložišti pomocí jazyka typu SQL nazývaného HiveQL. Hive, stejně jako Pig, je abstrakcí prostředí MapReduce. Při spuštění Hive překládá dotazy do řady úloh MapReduce. Hive je koncepčně blíže systému správy relačních databází než Pig a je proto vhodnější pro použití s více strukturovanými daty. Pro nestrukturovaná data je vhodnější Pig. Viz [Použití nástroje Hive se systémem Hadoop v prostředí HDInsight](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> je vrstva tabulky a správy ukládání pro Hadoop, která předkládá uživatelům relační zobrazení dat. V nástroji HCatalog můžete číst a zapisovat soubory v libovolném formátu, pro který lze napsat Hive SerDe (serializátor-deserializátor).

### <a name="mahout"></a>Mahout

<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> je škálovatelná knihovna algoritmů strojového učení, které běží v systému Hadoop. S využitím principů statistiky učí aplikace strojového učení systémy učit se z dat a využívat dřívější výstupy ke zjištění budoucího chování. Viz [Generování doporučení pomocí nástroje Mahout v systému Hadoop](hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
MapReduce je starší verze softwarového rozhraní pro Hadoop, určeného k psaní aplikací pro paralelní dávkové zpracování velkých sad dat. Úloha MapReduce rozdělí rozsáhlé datové sady a uspořádá data pro zpracování do párů hodnot klíčů.

[YARN](#yarn) je správce prostředků a aplikační rozhraní systému Hadoop nové generace a označuje se jako MapReduce 2.0. Úlohy MapReduce lze spustit na rozhraní YARN.

Další informace o prostředí MapReduce naleznete v tématu <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> na stránce Hadoop Wiki.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> je systém koordinace pracovních postupů, který spravuje úlohy systému Hadoop. Je integrován do zásobníku Hadoop a podporuje úlohy systému Hadoop pro MapReduce, Pig, Hive a Sqoop. Lze jej také použít při plánování úloh pro konkrétní systém, jako jsou programy v jazyce Java nebo skripty prostředí. Viz [Použití časového nástroje Oozie Coordinator se systémem Hadoop](hdinsight-use-oozie-coordinator-time.md).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> je vrstva relační databáze nad HBase. Phoenix zahrnuje ovladač JDBC, který umožňuje uživatelům přímé dotazování a správu tabulek SQL. Phoenix přeloží dotazy a další příkazy do nativních volání rozhraní API typu NoSQL, místo použití prostředí MapReduce, což umožní rychlejší aplikace nad uloženými daty NoSQL. Viz [Použití nástrojů Apache Phoenix a SQuirreL s clustery HBase](hdinsight-hbase-phoenix-squirrel.md).


### <a name="pig"></a>Pig
<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a> je vysokoúrovňová platforma, která umožňuje provádět komplexní transformace MapReduce na velmi rozsáhlých datových sadách pomocí jednoduchého skriptovacího jazyka nazvaného Pig Latin. Pig překládá skripty jazyka Pig Latin, aby je bylo možno spustit v systému Hadoop. Pig Latin můžete rozšířit vytvořením uživatelsky definovaných funkcí (UDF). Viz [Použití platformy Pig se systémem Hadoop k analýze souboru protokolu Apache](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> je nástroj pro co možná nejefektivnější hromadný přenos dat mezi systémem Hadoop a relačními databázemi jako SQL nebo dalšími strukturovanými datovými úložišti . Viz [Použití nástroje Sqoop se systémem Hadoop](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> je aplikační rozhraní založené na systému Hadoop YARN, které provádí komplexní acyklické grafy obecného zpracování dat. Je to pružnější a výkonnější nástupce rozhraní MapReduce, který umožňuje efektivní spouštění datově náročných procesů, například Hive, ve velkém rozsahu. Viz [„Použití rozhraní Apache Tez pro zlepšení výkonu“ v tématu Použití Hive a HiveQL](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>YARN
Apache YARN je nová generace prostředí MapReduce (MapReduce 2.0 nebo MRv2), která podporuje scénáře zpracování dat nad rámec dávkového zpracování prostředí MapReduce s větší škálovatelností a zpracováním v reálném čase. YARN poskytuje správu prostředků a distribuované aplikační rozhraní. Úlohy MapReduce lze spustit na rozhraní YARN.

Další informace o prostředí YARN naleznete v části <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Nová generace prostředí MapReduce systému Apache Hadoop (YARN)</a>.


### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> koordinuje procesy ve velkých distribuovaných systémech prostřednictvím sdíleného hierarchického oboru názvů datových registrů (znodes). Znodes obsahují malé množství metainformací potřebných ke koordinaci procesů: stav, umístění, konfigurace atd.

## Programovací jazyky v prostředí HDInsight

Clustery prostředí HDInsight – Hadoop, HBase, Storm a Spark – podporují řadu programovacích jazyků, některé však nejsou ve výchozím nastavení nainstalovány. Pro knihovny, moduly nebo balíčky, které nejsou ve výchozím nastavení nainstalovány, použijte akci skriptu pro instalaci součásti. Viz [Vývoj akcí skriptů v prostředí HDInsight](hdinsight-hadoop-script-actions-linux.md).

### Výchozí podpora programovacích jazyků

Ve výchozím nastavení podporují clustery prostředí HDInsight tyto jazyky:

* Java

* Python

Další jazyky lze nainstalovat pomocí akcí skriptů: [Vývoj akcí skriptů v prostředí HDInsight](hdinsight-hadoop-script-actions-linux.md).

### Jazyky Java virtual machine (JVM)

Řadu jiných jazyků než Java lze spouštět s využitím nástroje Java virtual machine (JVM); spuštění některých z těchto jazyků však může vyžadovat další součásti nainstalované v clusteru.

Clustery prostředí HDInsight podporují následující jazyky založené na JVM:

* Clojure

* Jython (Python pro jazyk Java)

* Scala

### Jazyky pro Hadoop

Clustery HDInsight poskytují podporu pro následující jazyky, které jsou určeny konkrétně pro ekosystém Hadoop:

* Pig Latin pro úlohy Pig

* HiveQL pro úlohy Hive a SparkSQL


## <a name="advantage"></a>Výhody systému Hadoop v cloudu

Jako součást cloudového ekosystému Azure nabízí systém Hadoop v prostředí HDInsight řadu výhod, mezi něž patří:

* Automatické zřizování clusterů systému Hadoop. Vytváření clusterů prostředí HDInsight je mnohem snazší než ruční konfigurace clusterů systému Hadoop. Podrobnosti najdete v tématu [Zřizování clusterů systému Hadoop v prostředí HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

* Moderní součásti systému Hadoop. Podrobnosti najdete v tématu [Co je nového ve verzích clusterů systému Hadoop poskytovaných prostředím HDInsight?][component-versioning].

* Vysoká dostupnost a spolehlivost clusterů.  Do clusterů systému Hadoop nasazených v prostředí HDInsight byl pro zvýšení dostupnosti služby přidán druhý hlavní uzel. Standardní implementace clusterů systému Hadoop obvykle mívají jeden hlavní uzel. HDInsight odstraní tento jediný bod selhání přidáním sekundárního hlavního uzlu. Přechodem na novou konfiguraci clusteru HA se cena clusteru nezmění, pokud zákazníci nevytvářejí clustery s mimořádně velkým hlavním uzlem místo uzlu  standardní velikosti.

    Podrobnosti viz [Dostupnost a spolehlivost clusterů systému Hadoop v prostředí HDInsight](hdinsight-high-availability-linux.md).

* Efektivní a ekonomické ukládání dat umožňuje Azure Blob, volba kompatibilní se systémem Hadoop. Podrobnosti viz [Použití úložiště Azure Blob se systémem Hadoop v prostředí HDInsight](hdinsight-hadoop-use-blob-storage.md).

* Integrace s dalšími službami Azure, včetně [Web Apps](../documentation/services/app-service/web/) a [SQL Database](../documentation/services/sql-database/).

* Další velikosti virtuálních počítačů. Clustery prostředí HDInsight jsou k dispozici pro různé typy a velikosti virtuálních počítačů. Clustery prostředí HDInsight mohou nyní využívat velikosti A2 až A7, vytvořené pro obecné účely; uzly série D-Series vybavené disky bez mechanických částí (SSD) a o 60 procent rychlejšími procesory; a velikosti A8 a A9, které mají podporu InfiniBand pro rychlou práci v síti. Zákazníci řešení Apache HBase v prostředí Azure HDInsight mohou využít větších možností konfigurace D-Series pro zvýšení výkonu. Zákazníci řešení Apache Storm v prostředí Azure HDInsight mohou také těžit z rozšířené paměti pro nahrávání velkých referenčních datových sad, jakož i rychlejších procesorů pro vyšší propustnost.

* Škálování clusterů. Škálování clusterů umožňuje změnit počet uzlů spuštěného clusteru v prostředí HDInsight bez nutnosti jej odstranit nebo znovu vytvořit.

* Podpora služby Virtual Network. Clustery prostředí HDInsight lze použít se službou Azure Virtual Network pro podporu izolace cloudových prostředků nebo hybridních scénářů, propojujících tyto cloudové prostředky s těmi, které jsou ve vašem datovém centru.

* Nízké vstupní náklady Spusťte si [bezplatnou zkušební verzi](/pricing/free-trial/) nebo se podívejte na [podrobnosti o cenách prostředí HDInsight](/pricing/details/hdinsight/).


Další informace o výhodách systému Hadoop v prostředí HDInsight najdete na stránce o [funkcích Azure pro HDInsight][marketing-page].



## <a id="resources"></a>Zdroje dalších informací o analýze velkých objemů dat, systému Hadoop a prostředí HDInsight

Poznatky z tohoto úvodu do systému Hadoop v cloudu a analýzy velkých objemů dat si můžete doplnit prostřednictvím níže uvedených zdrojů.


### Dokumentace systému Hadoop pro HDInsight

* [Dokumentace prostředí HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/): Stránka dokumentace pro Azure HDInsight s odkazy na články, videa a další materiály.

* [Začínáme s prostředím HDInsight v Linuxu](hdinsight-hadoop-linux-tutorial-get-started.md): Rychlý kurz zřizování clusterů systému Hadoop HDInsight v systému Linux a spuštění ukázkových dotazů nástroje Hive.

* [Začínáme s nástrojem Storm v Linuxu v prostředí HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md): Rychlý kurz zřizování clusterů systému Storm v prostředí HDInsight a spuštění ukázkových topologií systému Storm.

* [Zřízení prostředí HDInsight v Linuxu](hdinsight-hadoop-provision-linux-clusters.md): Naučte se, jak zřídit cluster systému Hadoop HDInsight v Linuxu prostřednictvím portálu Azure, Azure CLI nebo Azure PowerShell.

* [Práce s prostředím HDInsight v Linuxu](hdinsight-hadoop-linux-information.md): Získejte některé rychlé tipy pro práci s clustery systému Hadoop v Linuxu zřízenými prostřednictvím Azure.

* [Správa clusterů HDInsight pomocí nástroje Ambari](hdinsight-hadoop-manage-ambari.md): Zjistěte, jak sledovat a spravovat své clustery systému Hadoop v prostředí HDInsight v Linuxu pomocí rozhraní Ambari Web nebo Ambari REST API.


### Apache Hadoop

* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: Další informace o knihovně softwaru Apache Hadoop, rozhraní, které umožňuje distribuované zpracování rozsáhlých datových sad napříč clustery počítačů.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">HDFS</a>: Další informace o architektuře a designu distribuovaného systému souborů Hadoop, systému primárního úložiště používaného aplikacemi Hadoop.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">Kurz MapReduce</a>: Další informace o programovacím rozhraní pro psaní aplikací Hadoop, které rychle paralelně zpracovávají velké objemy dat. na velkých clusterech výpočetních uzlů.

### Databáze SQL v Azure

* [Azure SQL Database](/documentation/services/sql-database/): Dokumentace, kurzy a videa pro službu SQL Database.

* [Služba SQL Database na portálu Azure](../sql-database/sql-database-manage-portal.md): Jednoduchý a snadno použitelný nástroj pro správu databáze SQL v cloudu.

* [Adventure Works pro službu SQL Database](http://msftdbprodsamples.codeplex.com/releases/view/37304): Stránka pro stažení ukázkové databáze SQL.

### Microsoft business intelligence (pro HDInsight ve Windows)

Známé nástroje business intelligence (BI) – například aplikace Excel, PowerPivot, SQL Server Analysis Services a SQL Server Reporting Services – slouží k načítání, analýze a generování sestav dat integrovaných v prostředí HDInsight pomocí doplňku Power Query nebo ovladače ODBC Microsoft Hive.

Tyto nástroje BI vám mohou pomoci při analýze velkých objemů dat:

* [Připojení aplikace Excel k systému Hadoop pomocí Power Query](hdinsight-connect-excel-power-query.md): Zjistěte, jak připojit Excel k účtu úložiště Azure, kde se ukládají data přidružená k vašemu clusteru prostředí HDInsight pomocí aplikace Microsoft Power Query pro Excel.

* [Připojení aplikace Excel k systému Hadoop pomocí ovladače ODBC Microsoft Hive](hdinsight-connect-excel-hive-ODBC-driver.md): Zjistěte, jak importovat data z prostředí HDInsight pomocí ovladače ODBC Microsoft Hive.

* [Cloudová platforma Microsoft](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Získejte informace o službě Power BI pro Office 365, stáhněte si zkušební verzi serveru SQL Server a nastavte si SharePoint Server 2013 a SQL Server BI.

* <a target="_blank" href="http://msdn.microsoft.com/library/hh231701.aspx">Další informace o službě SQL Server Analysis Services</a>.

* <a target="_blank" href="http://msdn.microsoft.com/library/ms159106.aspx">Další informace o službě SQL Server Reporting Services</a>.


### Vyzkoušejte řešení Hadoop pro analýzu velkých objemů dat (pro HDInsight v systému Windows)

Pomocí analýzy velkých objemů dat ve vaší organizaci získáte přehled o svém podnikání. Zde je několik příkladů:

* [Analýza dat snímačů VZT](hdinsight-hive-analyze-sensor-data.md): Zjistěte, jak analyzovat data snímačů pomocí nástroje Hive se systémem HDInsight (Hadoop) a potom si data prohlédnout v aplikaci Microsoft Excel. V této ukázce použijete Hive ke zpracování historických dat vytvořených systémy VZT pro zjištění, které systémy nedokáží spolehlivě udržovat nastavenou teplotu.

* [Analýza protokolů webových stránek nástrojem Hive se systémem HDInsight](hdinsight-hive-analyze-website-log.md): Zjistěte, jak použít HiveQL v prostředí HDInsight k analýze protokolů webových stránek pro získání přehledu o četnosti návštěv z externích webových stránek za den a souhrnu chyb na webových stránkách, se kterými se uživatelé setkávají.

* [Analýza dat snímačů v reálném čase pomocí nástrojů Storm a HBase v prostředí HDInsight (Hadoop)](hdinsight-storm-sensor-data-analysis.md): Naučte se vytvářet řešení, které použije cluster systému Storm v prostředí HDInsight ke zpracování dat snímačů z Azure Event Hubs a potom zobrazí zpracovaná data snímačů jako informace téměř v reálném čase na webovém řídicím panelu.


[marketing-page]: ../services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/



<!--HONumber=Jun16_HO2-->


