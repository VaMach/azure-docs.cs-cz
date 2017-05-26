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
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/31/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 90043b27360cfea4235c4401bcba9e6a254b8627
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017


---
# <a name="introduction-to-apache-storm-on-hdinsight-real-time-analytics-for-hadoop"></a>Úvod do Apache Storm v HDInsight: analýzy v reálném čase pro Hadoop

S použitím Apache Storm v Azure HDInsight můžete vytvářet řešení distribuované analýzy v reálném čase.

Storm je distribuovaný výpočetní systém typu open source. Je odolný vůči poruchám. Prostřednictvím Stormu můžete zpracovávat data v reálném čase s použitím Hadoopu. Řešení Storm také zajišťují garantované zpracování data se schopností opakování dat, která nebyla úspěšně zpracována na první pokus.

## <a name="how-does-storm-work"></a>Jak Storm funguje?

Storm spouští topologie místo úloh MapReduce, se kterými jste se mohli seznámit v HDInsight nebo Hadoop. Topologie se skládají z několika součástí, které jsou uspořádány do orientovaného acyklického grafu (DAG). Následující diagram znázorňuje tok dat mezi součástmi v topologii pro základní počet slov:

![Příklad uspořádání součástí v topologii Storm](./media/hdinsight-storm-overview/wordcount-topology.png)

* Součásti spout přenášejí data do topologie. Vysílají do topologie jeden nebo více datových proudů.

* Součásti bolt zpracovávají datové proudy vyslané ze spoutů nebo z jiných boltů. Bolty mohou volitelně vysílat do topologie nové datové proudy. Bolty také odpovídají za zápis dat do trvalého úložiště, například HDFS nebo HBase.

## <a name="why-use-storm-on-hdinsight"></a>Proč používat Storm v HDInsight?

Storm pro HDInsight poskytuje následující klíčové výhody:

* Funguje jako spravovaná služby se smlouvou SLA s 99,9% dostupností.

* Podporuje snadné přizpůsobení prostřednictvím spouštění skriptů v clusteru během nebo po jeho vytvoření. Další informace obsahuje článek [Přizpůsobení clusterů HDInsight pomocí skriptových akcí](hdinsight-hadoop-customize-cluster-linux.md).

* Využívá různé jazyky. Součásti Stormu můžete psát v jazyce, který vám vyhovuje, například Java, C# nebo Python.

    * Integruje Visual Studio a HDInsight pro vývoj, správu a monitorování topologií C#. Další informace naleznete v tématu [Vývoj topologií C# Storm pomocí sady HDInsight Tools pro Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

    * Podporuje rozhraní Java Trident. Můžete vytvářet topologie Storm, které podporují právě jedno zpracování zpráv, transakční trvalé uchovávání datového skladu a sadu běžných operací pro analýzy datového proudu.

*  Umožňuje snadné vertikální navyšování i snižování kapacity. Můžete přidávat a odebírat pracovní uzly bez jakéhokoli dopadu na činnost topologií Storm.

* Umožňuje integraci s následujícími službami Azure:

    * Azure Event Hubs

    * Azure Virtual Network

    * Azure SQL Database

    * Azure Storage

    * Azure Cosmos DB

* Bezpečně kombinuje možnosti více clusterů HDInsight s použitím virtuální sítě. Můžete vytvářet analytické kanály, které používají clustery HDInsight, HBase nebo Hadoop.

Seznam společností, které používají pro svá řešení pro analýzu v reálném čase Storm, najdete v tématu [Společnosti využívající Apache Storm](https://storm.apache.org/documentation/Powered-By.html).

Chcete-li začít používat Storm, otevřete část [Začínáme pracovat se Storm v HDInsight][gettingstarted].

## <a name="ease-of-creation"></a>Snadné vytvoření

V HDInsight můžete zřídit nový cluster Storm během několika minut. Další informace týkající se vytvoření clusteru Storm naleznete v článku [Začínáme se Stormem v HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).

## <a name="ease-of-use"></a>Snadné používání

* __Připojení Secure Shell (SSH):__ Hlavní uzly clusteru HDInsight jsou přístupné z internetu prostřednictvím SSH. Příkazy můžete spouštět přímo v clusteru prostřednictvím SSH.

  Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

* __Webové připojení:__ Clustery HDInsight poskytují webové uživatelské rozhraní Ambari. Webové uživatelské rozhraní Ambari umožňuje snadno monitorovat, konfigurovat a spravovat služby v clusteru. Storm v HDInsight také poskytuje uživatelské rozhraní Storm. Prostřednictvím uživatelského rozhraní Storm můžete monitorovat a spravovat spuštěné topologie Storm z prohlížeče.

  Další informace obsahují články [Správa služby HDInsight pomocí webového rozhraní Ambari](hdinsight-hadoop-manage-ambari.md) a [Monitorování a správa pomocí Storm UI](hdinsight-storm-deploy-monitor-topology-linux.md#monitor-and-manage-storm-ui).

* __Prostředí PowerShell a rozhraní příkazového řádku Azure:__ Prostředí PowerShell i rozhraní příkazového řádku Azure poskytují nástroje příkazového řádku, které můžete používat z klientského systému při práci se službou HDInsight a dalšími službami Azure.

* __Integrace se sadou Visual Studio:__ Nástroje Azure Data Lake pro Visual Studio obsahují šablony projektů pro vytváření topologií Storm v jazyce C# s použitím rozhraní SCP.Net. Nástroje Data Lake poskytují také nástroje pro nasazení, monitorování a správu řešení se Stormem v HDInsight.

  Další informace naleznete v tématu [Vývoj topologií C# Storm pomocí sady HDInsight Tools pro Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="integration-with-other-azure-services"></a>Integrace s dalšími službami Azure

* __Azure Data Lake Store:__ Příklad použití Data Lake Store se Stormem naleznete v části [Používání Azure Data Lake Store pomocí Apache Storm v HDInsight](hdinsight-storm-write-data-lake-store.md).

* __Event Hubs:__ Příklad použití Event Hubs se Stormem najdete v následujících dokumentech:

    * [Vývoj topologie založené na jazyce Java pro Storm v HDInsight](hdinsight-storm-develop-java-topology.md)

    * [Zpracování událostí z Azure Event Hubs se Stormem v HDInsight (C#)](hdinsight-storm-develop-csharp-event-hub-topology.md)

* __SQL Database__, __Azure Cosmos DB__, __Event Hubs__ a __HBase__: Příklady šablon jsou součástí nástrojů Data Lake pro Visual Studio. Další informace najdete v článku [Vývoj topologií v jazyce C# pro Storm v HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="reliability"></a>Spolehlivost

Storm zaručuje, že příchozí zprávy budou vždy plně zpracovány, i když je analýza dat rozdělena do stovek uzlů.

Uzel Nimbus poskytuje funkce podobné Hadoop JobTracker a přiřazuje úlohy jiným uzlům v clusteru prostřednictvím nástroje Zookeeper. Uzly Zookeeper poskytují koordinaci pro cluster a usnadňují komunikaci mezi procesy Nimbus a Supervisor v pracovních uzlech. Pokud dojde k selhání jednoho uzlu zpracování, uzel Nimbus je informován a přiřadí úlohu a přidružená data do jiného uzlu.

Výchozí konfigurace pro Storm obsahuje pouze jeden uzel Nimbus. Storm v HDInsight využívá dva uzly Nimbus. V případě selhání primárního uzlu se cluster HDInsight přepne na sekundární uzel a primární uzel se obnoví. Následující diagram znázorňuje tok konfigurace úlohy pro Storm v HDInsight:

![Graf nimbusu, zookeeper a supervisor](./media/hdinsight-storm-overview/nimbus.png)

## <a name="scale"></a>Měřítko

Ačkoli lze určit počet uzlů v clusteru během vytváření, může být vhodné zvětšit nebo zmenšit cluster tak, aby odpovídal úloze. Počet uzlu ve všech clusterech HDInsight je možné změnit i v průběhu zpracování dat.

> [!NOTE]
> Abyste mohli využívat nové uzly přidané prostřednictvím škálování, budete muset vyrovnat topologie spuštěné před zvýšením velikosti clusteru.

## <a name="support"></a>Podpora

Storm v HDInsight obsahuje nepřetržitou plnou podporu na úrovni rozlehlé sítě. Storm v HDInsight má také SLA 99,9 %. To znamená, že zaručujeme, že cluster bude mít externí konektivitu alespoň 99,9 % času.

Další informace najdete v článku [Podpora Azure](https://azure.microsoft.com/support/options/).

## <a name="common-use-cases"></a>Běžné případy použití

Následují některé obvyklé scénáře, pro které můžete použít Storm v HDInsight. 

* Internet věcí (IoT)
* Odhalování podvodů
* Sociální analýzy
* Extrakce, transformace a načítání (ETL)
* Monitorování sítě
* Hledání
* Mobile engagement

Informace o reálných scénářích najdete v dokumentu [Jak společnosti využívají Storm](https://storm.apache.org/documentation/Powered-By.html).

## <a name="development"></a>Vývoj

Nástroje Data Lake Tools pro Visual Studio umožňují vývojářům .NET navrhovat a implementovat topologie v jazyce C#. Můžete také vytvářet hybridní topologie, které využívají součásti Java a C#.

Další informace najdete v článku [Vývoj topologií C# pro Storm v HDInsight pomocí sady Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Můžete také vyvíjet řešení v jazyce Java s použitím rozhraní IDE podle svého výběru. Další informace najdete v článku [Vývoj topologií v jazyce Java pro Storm v HDInsight](hdinsight-storm-develop-java-topology.md).

K vývoji součástí Storm je možné použít i jazyk Python. Další informace najdete v článku [Vývoj topologií Storm pomocí Pythonu v HDInsight](hdinsight-storm-develop-python-topology.md).

## <a name="common-development-patterns"></a>Běžné vývojové vzory

### <a name="guaranteed-message-processing"></a>Zaručené zpracování zprávy

Storm můžete poskytovat různé úrovně zaručeného zpracování zprávy. Například základní aplikace Storm může zaručit alespoň jedno zpracování a Trident může zaručit přesně jedno zpracování.

Další informace naleznete v tématu [Záruky na zpracování dat](https://storm.apache.org/about/guarantees-data-processing.html) na webu apache.org.

### <a name="ibasicbolt"></a>IBasicBolt

Běžný je vzor čtení vstupní řazené kolekce členů, generování nuly nebo dalších řazených kolekcí členů a následné okamžité potvrzení vstupní řazené kolekce členů na konci metody provedení. Storm pro automatizaci tohoto vzoru poskytuje rozhraní [IBasicBolt](https://storm.apache.org/apidocs/backtype/storm/topology/IBasicBolt.html).

### <a name="joins"></a>Spojení

Způsob spojení datových proudů se v jednotlivých aplikacích liší. Můžete například spojovat jednotlivé řazené kolekce členů z různých datových proudů do jednoho nového datového proudu nebo můžete spojovat pouze dávky řazených kolekcí členů pro konkrétní okno. V obou případech můžete spojování provést pomocí příkazu [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29), což je způsob definice, jak jsou záznamy směrovány do funkcí bolts.

V následujícím příkladu Java se příkaz fieldsGrouping využívá ke směrování řazených kolekcí členů, které pocházejí z komponent „1“, „2“ a „3“, do boltu MyJoiner:

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

### <a name="batches"></a>Dávky

Storm poskytuje interní časovací mechanismus známý jako „odškrtávání řazené kolekce členů“, který můžete použít ke generování dávky každých X sekund.

Příklad použití odškrtávaní řazených kolekcí členů v součásti v C# najdete v souboru [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java).

Trident je založen na zpracování dávek řazených kolekcí členů.

### <a name="caches"></a>Caches

Ukládání do mezipaměti se často používá jako mechanismus pro urychlení zpracování, protože udržuje často používané prostředky v paměti. Protože se topologie distribuuje mezi více uzlů a v rámci každého uzlu se distribuuje více procesů, měli byste zvážit použití příkazu [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29). Pomocí příkazu `fieldsGrouping` můžete zajistit, aby se řazené kolekce členů obsahující pole, která se používají pro vyhledávání v mezipaměti, vždy směrovaly do stejného procesu. Díky této funkci seskupování předejdete duplikaci položek mezipaměti napříč procesy.

### <a name="stream-top-n"></a>Datový proud „horních N“

Pokud vaše topologie závisí na výpočtu hodnoty „horních N“, počítejte hodnotu „horních N“ paralelně. Poté je třeba výstup z těchto výpočtů sloučit do globální hodnoty. Tuto operaci lze provést pomocí příkazu [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) směrujícího hodnotu podle pole pro paralelní zpracování a pak směrovat na bolt, který globálně určí hodnotu „horních N“.

Příklad výpočtu hodnoty „horních N“ najdete v příkladu [RollingTopWords](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/RollingTopWords.java).

## <a name="logging"></a>Protokolování

Storm používá Apache Log4j k ukládání informací. Ve výchozím nastavení se zaznamenává velké množství dat a může být obtížné informace roztřídit. Konfigurační soubor protokolování můžete zahrnout jako součást topologie Storm k řízení chování protokolování.

Topologii příkladu, která ukazuje, jak nakonfigurovat protokolování, najdete v části [Počet slov na základě jazyka Java](hdinsight-storm-develop-java-topology.md) pro Storm v HDInsight.

## <a name="next-steps"></a>Další kroky

Další informace o řešení pro analýzu v reálném čase se Stormem v HDInsight:

* [Začínáme se Stormem v HDInsight][gettingstarted]
* [Příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md

