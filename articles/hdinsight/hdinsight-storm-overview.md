---
title: "Úvod do Apache Stormu v HDInsight | Dokumentace Microsoftu"
description: "Seznámíte se s úvodem do Apache Storm v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 72d54080-1e48-4a5e-aa50-cce4ffc85077
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/11/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 0cf2d7f4cbbed730d690693fd006665355155c22
ms.lasthandoff: 03/21/2017


---
# <a name="introduction-to-apache-storm-on-hdinsight-real-time-analytics-for-hadoop"></a>Úvod do Apache Storm v HDInsight: analýzy v reálném čase pro Hadoop

Apache Storm v HDInsight umožňuje vytvářet distribuovaná analytická řešení v reálném čase v Azure.

Apache Storm je distribuovaný open source výpočetní systém odolný proti chybám, který umožňuje zpracovávat data v reálném čase pomocí Hadoop. Řešení Storm také zajišťují garantované zpracování data se schopností opakování dat, která nebyla úspěšně zpracována na první pokus.

## <a name="why-use-storm-on-hdinsight"></a>Proč používat Storm v HDInsight

Apache Storm v HDInsight představuje spravovaný cluster integrovaný do prostředí Azure. Storm a další komponenty Hadoop v HDInsight jsou založené na Hortonworks Data Platform (HDP) a operační systém clusteru je Ubuntu (distribuce Linuxu). Tato konfigurace zajišťuje platformu, která je kompatibilní s oblíbenými nástroji a službami v ekosystému Hadoop.

> [!IMPORTANT]
> HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v článku [Vyřazení služby HDInsight pro Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

Apache Storm pro HDInsight poskytuje následující klíčové výhody:

* Funguje jako spravovaná služby se smlouvou SLA s 99,9% dostupností.

* Snadné přizpůsobení pomocí skriptů spouštěných na clusteru během nebo po jeho vytvoření. Další informace obsahuje článek [Přizpůsobení clusterů HDInsight pomocí skriptových akcí](hdinsight-hadoop-customize-cluster-linux.md).

* Můžete použít jazyk podle vašeho výběru: Komponenty Storm je možné psát v různých jazycích, jako je například **Java**, **C#** nebo **Python**.
  
  * Integrace sady Visual Studio a HDInsight pro vývoj, správu a monitorování topologií C#. Další informace naleznete v tématu [Vývoj topologií C# Storm pomocí sady HDInsight Tools pro Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

  * Podporuje rozhraní Java **Trident**. Toto rozhraní podporuje vytváření topologií Storm, které podporují „přesně jedno“ zpracování zpráv, „transakční“ trvalé uchovávání datového skladu a sadu běžných operací pro analýzy datového proudu.

* Snadné škálování clusteru nahoru a dolů: přidávání a odebírání pracovních uzlů bez jakéhokoli dopadu na činnost topologií Storm.

* Integrace s následujícími službami Azure:
  
    * Event Hubs
    * Virtual Network
    * Databáze SQL
    * Azure Storage
    * DocumentDB
  
  * Bezpečná kombinace schopností více clusterů HDInsight pomocí sítě Azure Virtual Network: vytvářejte analytické kanály, které používají HDInsight, HBase nebo clustery Hadoop.

Seznam společností, které používají pro svá řešení pro analýzu v reálném čase Apache Storm naleznete v tématu [Společnosti využívající Apache Storm](https://storm.apache.org/documentation/Powered-By.html).

Chcete-li začít používat Storm, otevřete část [Začínáme pracovat se Storm v HDInsight][gettingstarted].

### <a name="ease-of-creation"></a>Snadné vytvoření

V clusteru HDInsight můžete zřídit nové Storm během několika minut. Zadejte název clusteru, velikost, účet správce a účet úložiště. Azure vytvoří cluster, včetně vzorových topologií a řídicího panelu webové správy.

> [!NOTE]
> Clustery Storm můžete také vytvářet pomocí [Azure CLI](../cli-install-nodejs.md) nebo [Azure PowerShell](/powershell/azureps-cmdlets-docs).

Během 15 minut od odeslání žádosti budete mít nový cluster Storm spuštěný a připravený pro první analytický kanál v reálném čase.

### <a name="ease-of-use"></a>Snadné používání

* __Připojení Secure Shell:__ hlavní uzly clusteru HDInsight jsou přístupné z internetu prostřednictvím SSH. SSH umožňuje spouštění příkazů přímo na clusteru.

  Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

* __Webové připojení:__ Clustery HDInsight poskytují webové uživatelské rozhraní Ambari. Webové uživatelské rozhraní Ambari umožňuje snadno monitorovat, konfigurovat a spravovat služby ve vašem clusteru. Storm v HDInsight také poskytuje uživatelské rozhraní Storm UI, se kterým můžete monitorovat a spravovat spuštěné topologie Storm ze svého prohlížeče.

  Další informace obsahují články [Správa služby HDInsight pomocí webového rozhraní Ambari](hdinsight-hadoop-manage-ambari.md) a [Monitorování a správa pomocí Storm UI](hdinsight-storm-deploy-monitor-topology-linux.md#monitor-and-manage-using-the-storm-ui).

* __PowerShell a příkazový řádek Azure:__ Jak prostředí Azure PowerShell, tak příkazový řádek Azure CLI poskytují nástroje, které můžete použít z klientského systému při práci se službou HDInsight a dalšími službami Azure.

* __Integrace se sadou Visual Studio:__ Modul plug-in Nástroje Data Lake pro Visual Studio obsahuje šablony projektů pro vytváření topologií Storm v jazyce C# a nástroje pro monitorování Storm v HDInsight. Můžete vytvářet, nasazovat, monitorovat a spravovat topologie v jazyce C# z prostředí sady Visual Studio.

  Další informace naleznete v tématu [Vývoj topologií C# Storm pomocí sady HDInsight Tools pro Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

* __Integrace s dalšími službami Azure__

  * Pokud vyvíjíte v jazyce __Java__, Microsoft pro integraci s dalšími službami Azure využívá existující komponenty Storm, pokud je to možné. V některých případech mohou být vyžadovány součásti nebo řešení specifické pro danou službu.

    * __Azure Data Lake Store:__ Topologie založené na jazyce Java můžou využít přístup ke službě Data Lake Store prostřednictvím boltu Storm HDFS se schématem URI `adl://`. Příklad použití boltu Storm HDFS naleznete v části [Používání Azure Data Lake Store pomocí Apache Storm v HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-write-data-lake-store).

    * __Azure Storage__ (při použití jako úložiště pro HDInsight): Topologie založené na jazyce Java můžou využít přístup ke službě Azure Storage prostřednictvím boltu Storm HDFS se schématem identifikátoru URI `wasb://`.

    * __Azure Event Hubs:__ Přístup je možný prostřednictvím součástí EventHubSpout a EventHubBolt od Microsoftu. Tyto součásti jsou napsané v jazyce Java a poskytují se jako samostatné soubory .jar.

    Další informace o vývoji řešení v jazyce Java najdete v článku [Vývoj topologií v jazyce Java pro Apache Storm ve službě HDInsight](hdinsight-storm-develop-java-topology.md).

  * Pro vývoj v __C#__, můžete obvykle použít .NET SDK pro službu Azure. V některých případech může sada SDK záviset na architektuře, která není dostupná pro systém Linux (hostitelský operační systém pro HDInsight 3.4 a vyšší). V takovém případě můžete v rámci vašeho řešení C# použít komponenty napsané v Javě.

    * Příklady pro práci s __SQL DB__, __DocumentDB__, __EventHub__ a __HBase__ jsou zahrnuty jako šablony v sadě Azure Data Lake Tools pro Visual Studio. Další informace najdete v článku [Vývoj topologií v jazyce C# pro Storm v HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).

    * __Azure Event Hubs:__ Příklad použití komponent v jazyce Java v rámci řešení v C# najdete v článku [Zpracování událostí z Azure Event Hub pomocí Stormu ve službě HDInsight (C#)](hdinsight-storm-develop-csharp-event-hub-topology.md).

    Další informace o vývoji řešení v jazyce C# najdete v článku [Vývoj topologií v jazyce C# pro Apache Storm ve službě HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).

### <a name="reliability"></a>Spolehlivost

Apache Storm vždy zaručuje, že příchozí zprávy budou plně zpracovány, i když je analýza dat rozdělena na stovky uzlů.

**Uzel Nimbus** poskytuje podobné funkce jako Hadoop JobTracker a přiřazuje úlohy jiným uzlům v clusteru prostřednictvím nástroje **Zookeeper**. Uzly zookeeper poskytují koordinaci pro cluster a usnadňují komunikace mezi procesem Nimbus a **Supervisor** na pracovních uzlech. Pokud dojde k selhání jednoho uzlu zpracování, uzel Nimbus je informován a přiřadí úlohu a přidružená data do jiného uzlu.

Výchozí konfigurace pro Apache Storm může obsahovat pouze jeden uzel Nimbus. Storm v HDInsight využívá dva uzly Nimbus. V případě selhání primárního uzlu se cluster HDInsight přepne na sekundární uzel a primární uzel se obnoví.

![Graf nimbusu, zookeeper a supervisor](./media/hdinsight-storm-overview/nimbus.png)

### <a name="scale"></a>Měřítko

Ačkoli můžete určit počet uzlů v clusteru během vytváření, můžete chtít zvětšit nebo zmenšit cluster tak, aby odpovídal pracovnímu vytížení. Všechny clustery HDInsight umožňují změnit počet uzlů v clusteru, i v průběhu zpracování dat.

> [!NOTE]
> Abyste mohli využívat nové uzly přidané prostřednictvím škálování, budete muset vyrovnat topologie spuštěné před zvýšením velikosti clusteru.

### <a name="support"></a>Podpora

Storm v HDInsight obsahuje nepřetržitou plnou podporu na úrovni rozlehlé sítě. Storm v HDInsight má také SLA 99,9 %. To znamená, že zaručujeme, že cluster bude mít externí konektivitu alespoň 99,9 % času.

## <a name="common-use-cases-for-real-time-analytics"></a>Běžné případy použití pro analýzu v reálném čase

Následují některé obvyklé scénáře, pro které můžete použít Apache Storm v HDInsight. Informace o reálných scénářích najdete v tématu [Jak společnosti využívají Storm](https://storm.apache.org/documentation/Powered-By.html).

* Internet věcí (IoT)
* Odhalování podvodů
* Sociální analýzy
* Extrakce, transformace, načítání (ETL)
* Monitorování sítě
* Hledání
* Mobile engagement

## <a name="how-is-data-in-hdinsight-storm-processed"></a>Jak se zpracovávají data v HDInsight Storm

Apache Storm spustí **topologie** místo úloh MapReduce, se kterými jste se mohli seznámit v HDInsight nebo Hadoop. Cluster Storm v HDInsight obsahuje dva typy uzlů: hlavní uzly, které využívají **Nimbus** a pracovní uzly, které využívají **Supervisor**.

* **Nimbus:** podobný jako JobTracker v Hadoop, je zodpovědný za distribuci kódu v rámci celého clusteru, přiřazování úkolů k virtuálním počítačům a monitorování pro případ selhání. HDInsight nabízí dva uzly Nimbus, takže neexistuje žádný jediný bod selhání Storm v HDInsight
* **Supervisor:** supervizor pro každý pracovní uzel je zodpovědný za spouštění a zastavování **pracovních procesů** na uzlu.
* **Pracovní proces:** spouští podmnožinu **topologie**. Spuštěná topologie se distribuuje do mnoha pracovních procesů v rámci clusteru.
* **Topologie:** definuje výpočetní graf, který zpracovává data **datových proudů**. Na rozdíl od úloh MapReduce topologie běží, dokud je nezastavíte.
* **Datový proud:** nevázaná kolekce **záznamů**. Datové proudy vznikají pomocí funkcí **spouts** a **bolts** a spotřebovávají se pomocí funkcí **bolts**.
* **Záznam:** pojmenovaný seznam dynamicky zadávaných hodnot.
* **Spout:** spotřebovává data ze zdroje dat a vysílá jeden nebo více **datových proudů**.
  
  > [!NOTE]
  > Data se často načítají z fronty, například Kafka nebo Azure Event Hubs. Pokud dojde k výpadku, fronta zajistí přetrvání dat.

* **Bolt:** spotřebovává **datové proudy**, provádí zpracování na **záznamech** a může vysílat **datové proudy**. Funkce Bolts jsou také zodpovědné za zápis dat do externího úložiště, například front, HDInsight, HBase, objektů blob nebo jiná datová úložiště.
* **Apache Thrift:** framework software pro vývoj škálovatelných služeb mezi jazyky. Umožňuje vytvářet služby, které pracují mezi jazyky C++, Java, Python, PHP, Ruby, Erlangovo, Perl, Haskell, C#, kakao, JavaScript, Node.js, Smalltalk a dalšími jazyky.

Další informace o součástech Storm naleznete v tématu [Kurz Storm][apachetutorial] na apache.org.

## <a name="what-programming-languages-can-i-use"></a>Jaké programovací jazyky mohu použít

### <a name="c35"></a>C&#35;

Sada Data Lake Tools pro Visual Studio umožňuje vývojářům .NET navrhovat a implementovat topologii v jazyce C#. Můžete také vytvářet hybridní topologie, které využívají součásti Java a C#.

Další informace naleznete v tématu [Vývoj topologií C# pro Apache Storm v HDInsight pomocí sady Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

### <a name="java"></a>Java

Většina příkladů v jazyce Java, na které narazíte, je v prostém jazyce Java nebo Trident. Trident má vysokou úroveň abstrakce, která usnadňuje provádění akcí, například slučování, agregací, seskupování a filtrování. Trident však funguje na dávky záznamů, zatímco hrubé řešení Java zpracovává jeden záznam za druhým.

Další informace o Trident naleznete v tématu [Kurz Trident](https://storm.apache.org/documentation/Trident-tutorial.html) na apache.org.

Příklady topologií Java a Trident naleznete v tématu [Seznam příkladů topologie Storm](hdinsight-storm-example-topology.md) nebo počáteční příklady Storm v clusteru HDInsight.

Úvodní příklady pro Storm jsou umístěné v adresáři **/usr/hdp/current/storm-client/contrib/storm-starter** ve vašem clusteru HDInsight.

### <a name="python"></a>Python

Příklad použití komponent v Pythonu najdete v tématu [Vývoj topologií Storm pomocí Pythonu v HDInsight](hdinsight-storm-develop-python-topology.md).

## <a name="what-are-some-common-development-patterns"></a>Jaké jsou některé běžné vývojové vzory

### <a name="guaranteed-message-processing"></a>Zaručené zpracování zprávy

Storm můžete poskytovat různé úrovně zaručeného zpracování zprávy. Například základní aplikace Storm může zaručit alespoň jedno zpracování a Trident může zaručit přesně jedno zpracování.

Další informace naleznete v tématu [Záruky na zpracování dat](https://storm.apache.org/about/guarantees-data-processing.html) na webu apache.org.

### <a name="ibasicbolt"></a>IBasicBolt

Běžný je vzor čtení vstupní řazené kolekce členů, generování nuly nebo dalších řazených kolekcí členů a následné okamžité potvrzení vstupní řazené kolekce členů na konci metody provedení. Storm pro automatizaci tohoto vzoru poskytuje rozhraní [IBasicBolt](https://storm.apache.org/apidocs/backtype/storm/topology/IBasicBolt.html).

### <a name="joins"></a>Spojení

Způsob spojení datových proudů se v jednotlivých aplikacích liší. Například může spojit každý záznam z různých datových proudů do jednoho nového datového proudu nebo můžete spojit pouze dávky záznamů pro konkrétní okno. V obou případech můžete spojování provést pomocí příkazu [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29), což je způsob definice, jak jsou záznamy směrovány do funkcí bolts.

V následujícím příkladu Java se fieldsGrouping využívá ke směrování záznamů, které pocházejí z komponenty „1“, „2“ a „3“ do funkce bolt **MyJoiner**.

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

### <a name="batching"></a>Dávkování

Storm poskytuje interní časovací mechanismus známý jako „odškrtávání řazené kolekce členů“, který můžete použít ke generování dávky každých X sekund.

Příklad použití odškrtávaných záznamů v součásti v C# najdete v souboru [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java).

Pokud používáte Trident, je založen na zpracování dávek záznamů.

### <a name="caching"></a>Ukládání do mezipaměti

Ukládání do mezipaměti se často používá jako mechanismus pro urychlení zpracování, protože udržuje často používané prostředky v paměti. Protože se topologie distribuuje mezi více uzlů a v rámci každého uzlu se distribuuje více procesů, měli byste zvážit použití příkazu [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29). Příkaz `fieldsGrouping` zajišťuje, že řazené kolekce členů obsahující pole, která se používají pro vyhledávání v mezipaměti, se vždy přesměrují do stejného procesu. Díky této funkci seskupování předejdete duplikaci položek mezipaměti napříč procesy.

### <a name="streaming-top-n"></a>Streamování horních N

Pokud vaše topologie závisí na výpočtu hodnoty „horních N“, doporučujeme vám vypočítat hodnotu horních N paralelně a pak sloučit výstup z těchto výpočtů do globální hodnoty. To lze provést pomocí příkazu [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) směrujícího hodnotu podle pole pro paralelní zpracování a pak směrovat na bolt, který globálně určí hodnotu horních N.

Příklad výpočtu hodnoty „horních N“ najdete v části [RollingTopWords](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/RollingTopWords.java).

## <a name="what-type-of-logging-does-storm-use"></a>Jaký typ protokolování používá Storm

Storm používá Apache Log4j k ukládání informací. Ve výchozím nastavení se zaznamenává velké množství dat a může být obtížné informace roztřídit. Konfigurační soubor protokolování můžete zahrnout jako součást topologie Storm k řízení chování protokolování.

Topologii příkladu, která ukazuje, jak nakonfigurovat protokolování, najdete v části [Počet slov na základě jazyka Java](hdinsight-storm-develop-java-topology.md) pro Storm v HDInsight.

## <a name="next-steps"></a>Další kroky

Další informace o řešení pro analýzu v reálném čase s Apache Storm v HDInsight:

* [Začínáme se Stormem v HDInsight][gettingstarted]
* [Příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md

