<properties
    pageTitle="Úvod do Apache Storm v HDInsight | Microsoft Azure"
    description="Získejte Úvod do Apache Storm a naučte se, jak můžete používat Storm v HDInsight k sestavení řešení pro analýzu dat v reálném čase v cloudu."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="03/18/2016"
   ms.author="larryfr"/>

#Úvod do Apache Storm v HDInsight: analýzy v reálném čase pro Hadoop

Apache Storm v HDInsight vám umožní vytvářet distribuovaná analytická řešení v reálném čase v prostředí Azure pomocí [Apache Hadoop](http://hadoop.apache.org).

##Co je to Apache Storm?

Apache Storm je distribuovaný open source výpočetní systém odolný proti chybám, který umožňuje zpracovávat data v reálném čase pomocí Hadoop. Řešení Storm také zajišťují garantované zpracování data se schopností opakování dat, která nebyla úspěšně zpracována na první pokus.

##Proč používat Storm v HDInsight?

Apache Storm v HDInsight představuje spravovaný cluster integrovaný do prostředí Azure. Poskytuje následující klíčové výhody:

* Funguje jako spravovaná služby s dobou provozu SLA 99,9 %

* Využívá jazyk podle vašeho výběru: poskytuje podporu pro komponenty Storm napsané v jazyce **Java**, **C#** a **Python**

    * Podporuje směs programovacích jazyků: čtení dat v jazyce Java a následné zpracování pomocí jazyka C#
    
        > [AZURE.NOTE] Topologie C# jsou podporovány pouze na clusterech HDInsight se systémem Windows.

    * Použijte rozhraní **Trident** Java k vytváření topologie Storm, které podporují „přesně jedno“ zpracování zpráv, stálost ukládání „transakcí“ a sadu běžných operací analýz datového proudu

* Obsahuje integrované funkce škálování nahoru a dolů: škálování clusteru HDInsight bez jakéhokoli dopadu na spouštění topologií Storm

* Integrace s jinými službami Azure včetně Event Hub, Azure Virtual Network, SQL Database, úložiště objektů Blob a DocumentDB.

    * Kombinace možností více clusterů HDInsight pomocí sítě Azure Virtual Network: vytvářejte analytické kanály, které používají HDInsight, HBase nebo clustery Hadoop

Seznam společností, které používají pro svá řešení pro analýzu v reálném čase Apache Storm naleznete v tématu [Společnosti využívající Apache Storm](https://storm.apache.org/documentation/Powered-By.html).

Chcete-li začít používat Storm, otevřete část [Začínáme pracovat se Storm v HDInsight][gettingstarted].

###Snadné zřizování

V clusteru HDInsight můžete zřídit nové Storm během několika minut. Zadejte název clusteru, velikost, účet správce a účet úložiště. Azure vytvoří cluster, včetně vzorových topologií a řídicího panelu webové správy.

> [AZURE.NOTE] Clustery Storm můžete také vytvářet pomocí [Azure CLI](../xplat-cli-install.md) nebo [Azure PowerShell](../powershell-install-configure.md).

Během 15 minut od odeslání žádosti budete mít nový cluster Storm spuštěný a připravený pro první analytický kanál v reálném čase.

###Snadné používání

__U clusterů Storm se systémem Linux v HDInsight__ se můžete připojit ke clusteru pomocí SSH a použít příkaz `storm` ke spuštění s správě topologií. Kromě toho můžete ke sledování služby Storm použít Ambari a uživatelské rozhraní Storm ke sledování a správě spuštěných topologií.

Další informace o práci s clustery Storm se systémem Linux naleznete v části [Začínáme s Apache Storm v HDInsight se systémem Linux](hdinsight-apache-storm-tutorial-get-started-linux.md).

__U clusterů Storm se systémem Windows v HDInsight__ umožňují nástroje HDInsight pro Visual Studio vytvářet C# a hybridní C# nebo Java topologie a odesílat je na cluster Storm v HDInsight.  

![Vytvoření projektu Storm](./media/hdinsight-storm-overview/createproject.png)

Nástroje HDInsight pro Visual Studio také obsahují rozhraní, které umožňuje sledovat a spravovat topologie Storm v clusteru.

![Správa Storm](./media/hdinsight-storm-overview/stormview.png)

Příklad použití nástrojů HDInsight k vytvoření aplikace Storm najdete v tématu [Vývoj topologií C# Storm pomocí nástrojů HDInsight pro Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Další informace o nástrojích HDInsight pro Visual Studio naleznete v tématu [Začínáme pomocí nástrojů HDInsight pro Visual Studio](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md).

Každý cluster Storm v HDInsight také obsahuje webový řídicí panel Storm, který umožňuje odeslání, sledování a správu topologií Storm spuštěných v clusteru.

![Řídicí panel Storm](./media/hdinsight-storm-overview/dashboard.png)

Další informace o používání řídicího panelu Storm naleznete v tématu [Nasazení a správa topologií Apache Storm v HDInsight](hdinsight-storm-deploy-monitor-topology.md).

Storm v HDInsight také poskytuje snadnou integraci s Azure Event Hubs přes **Event Hub Spout**. Nejnovější verzi této součásti je k dispozici na adrese [https://github.com/hdinsight/hdinsight-storm-examples/tree/master/lib/eventhubs](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/lib/eventhubs). Další informace o použití této součásti najdete v následujících dokumentech.

* [Vývoj topologie C#, která používá Azure Event Hubs](hdinsight-storm-develop-csharp-event-hub-topology.md)

* [Vývoj topologie Java, která používá Azure Event Hubs](hdinsight-storm-develop-java-event-hub-topology.md)

###Spolehlivost

Apache Storm vždy zaručuje, že příchozí zprávy budou plně zpracovány, i když je analýza dat rozdělena na stovky uzly.

**Uzel Nimbus** poskytuje podobné funkce jako Hadoop JobTracker a přiřazuje úlohy jiným uzlům v clusteru prostřednictvím nástroje **Zookeeper**. Uzly zookeeper poskytují koordinaci pro cluster a usnadňují komunikace mezi procesem Nimbus a **Supervisor** na pracovních uzlech. Pokud dojde k selhání jednoho uzlu zpracování, uzel Nimbus je informován a přiřadí úlohu a přidružená data do jiného uzlu.

Výchozí konfigurace pro Apache Storm může obsahovat pouze jeden uzel Nimbus. Storm v HDInsight využívá dva uzly Nimbus. V případě selhání primárního uzlu se cluster HDInsight přepne na sekundární uzel a primární uzel se obnoví.

![Graf nimbusu, zookeeper a supervisor](./media/hdinsight-storm-overview/nimbus.png)

###Měřítko

Ačkoli můžete určit počet uzlů v clusteru během vytváření, můžete chtít zvětšit nebo zmenšit cluster tak, aby odpovídal pracovnímu vytížení. Všechny clustery HDInsight umožňují změnit počet uzlů v clusteru, i v průběhu zpracování dat.

> [AZURE.NOTE] Abyste mohli využívat nové uzly, které se přidávají prostřednictvím škálování, budete muset vyrovnat topologie spuštěné před zvýšením velikosti clusteru.

###Podpora

Storm v HDInsight obsahuje nepřetržitou plnou podporu na úrovni rozlehlé sítě. Storm v HDInsight má také SLA 99,9 %. To znamená, že zaručujeme, že cluster bude mít externí konektivitu alespoň na úrovni 99,9 % času.

##Běžné případy použití pro analýzu v reálném čase

Následují některé obvyklé scénáře, pro které můžete použít Apache Storm v HDInsight. Pro informace o scénářích reálného světa si přečtěte téma [Jak společnosti využívají Storm](https://storm.incubator.apache.org/documentation/Powered-By.html).

* Internet věcí (IoT)
* Odhalování podvodů
* Sociální analýzy
* Extrakce, transformace, načítání (ETL)
* Monitorování sítě
* Hledání
* Mobile engagement

##Jak se zpracovávají data v HDInsight Storm?

Apache Storm spustí **topologie** místo úloh MapReduce, se kterými jste se mohli seznámit v HDInsight nebo Hadoop. Cluster Storm v HDInsight obsahuje dva typy uzlů: hlavní uzly, které využívají **Nimbus** a pracovní uzly, které využívají **Supervisor**.

* **Nimbus**: podobný jako JobTracker v Hadoop, je zodpovědný za distribuci kódu v rámci celého clusteru, přiřazování úkolů k virtuálním počítačům a monitorování pro případ selhání. HDInsight nabízí dva uzly Nimbus, takže neexistuje žádný jediný bod selhání Storm v HDInsight

* **Supervisor**: supervizor pro každý pracovní uzel je zodpovědný za spouštění a zastavování **pracovních procesů** na uzlu.

* **Pracovní proces**: spouští podmnožinu **topologie**. Spuštěná topologie se distribuuje do mnoha pracovních procesů v rámci clusteru.

* **Topologie**: definuje výpočetní graf, který zpracovává data **datových proudů**. Na rozdíl od úloh MapReduce topologie běží, dokud je nezastavíte.

* **Datový proud**: nevázaná kolekce **záznamů**. Datové proudy vznikají pomocí funkcí **spouts** a **bolts** a spotřebovávají se pomocí funkcí **bolts**.

* **Záznam**: pojmenovaný seznam dynamicky zadávaných hodnot.

* **Spout**: spotřebovává data ze zdroje dat a vysílá jeden nebo více **datových proudů**.

    > [AZURE.NOTE] V mnoha případech se data načítají z fronty, například Kafka, fronty Azure Service Bus nebo Event Hubs. Fronty zajišťují, že data přetrvají v případě výpadku.

* **Bolt**: spotřebovává **datové proudy**, provádí zpracování na **záznamech** a může vysílat **datové proudy**. Funkce Bolts jsou také zodpovědné za zápis dat do externího úložiště, například front, HDInsight, HBase, objektů blob nebo jiná datová úložiště.

* **Apache Thrift**: framework software pro vývoj škálovatelných služeb mezi jazyky. Umožňuje vytvářet služby, které pracují mezi jazyky C++, Java, Python, PHP, Ruby, Erlangovo, Perl, Haskell, C#, kakao, JavaScript, Node.js, Smalltalk a dalšími jazyky.

    * **Nimbus** je služba Thrift a **topologie** představuje definici Thrift, takže je možné vyvíjet topologie pomocí různých programovacích jazyků.

Další informace o součástech Storm naleznete v tématu [Kurz Storm][apachetutorial] na apache.org.


##Jaké programovací jazyky mohu použít?

Storm v clusteru HDInsight poskytuje podporu jazyků C#, Java a Python.

### C&#35;

Nástroje HDInsight pro Visual Studio umožňují vývojářům rozhraní .NET navrhovat a implementovat topologii v jazyce C#. Můžete také vytvářet hybridní topologie, které využívají součásti Java a C#.

Další informace naleznete v tématu [Vývoj topologií C# pro Apache Storm v HDInsight pomocí sady Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

###Java

Většina příkladů Java, na které narazíte, bude v prostém jazyce Java nebo Trident. Trident má vysokou úroveň abstrakce, která usnadňuje provádění akcí, například slučování, agregací, seskupování a filtrování. Trident však funguje na dávky záznamů, zatímco hrubé řešení Java zpracovává jeden záznam za druhým.

Další informace o Trident naleznete v tématu [Kurz Trident](https://storm.incubator.apache.org/documentation/Trident-tutorial.html) na apache.org.

Příklady topologií Java a Trident naleznete v tématu [Seznam příkladů topologie Storm](hdinsight-storm-example-topology.md) nebo počáteční příklady Storm v clusteru HDInsight.

Počáteční příklady Storm jsou umístěny v adresáři __ /usr/hdp/current/storm-client/contrib/storm-starter__ v clusterech se systémem Linux a adresáři **%storm_home%\contrib\storm-starter** v clusterech se systémem Windows.

##Jaké jsou některé běžné vývojové vzory?

###Zaručené zpracování zprávy

Storm můžete poskytovat různé úrovně zaručeného zpracování zprávy. Například základní aplikace Storm může zaručit alespoň jedno zpracování  a Trident může zaručit přesně jedno zpracování.

Další informace naleznete v tématu [Záruky na zpracování dat](https://storm.apache.org/about/guarantees-data-processing.html) na webu apache.org.

###IBasicBolt

Vzor čtení vstupního záznamu, generování nuly nebo dalších záznamů a velmi běžná je potom okamžitá kreativní oprava vstupního záznamu na konci metody provedení a Storm poskytuje rozhraní [IBasicBolt](https://storm.apache.org/apidocs/backtype/storm/topology/IBasicBolt.html) pro automatizaci tohoto vzoru.

###Spojení

Spojování dvou datových proudů se liší mezi aplikacemi. Například může spojit každý záznam z různých datových proudů do jednoho nového datového proudu nebo můžete spojit pouze dávky záznamů pro konkrétní okno. V obou případech můžete spojování provést pomocí příkazu [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29), což je způsob definice, jak jsou záznamy směrovány do funkcí bolts.

V následujícím příkladu Java se fieldsGrouping využívá ke směrování záznamů, které pocházejí z komponenty „1“, „2“ a „3“ do funkce bolt **MyJoiner**.

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

###Dávkování

Dávkování lze provést několika způsoby. Základní topologie Storm Java mohou využít jednoduchý čítač na počet dávek X záznamů před generováním nebo použít interní mechanismus časování známý jako „odškrtávaný záznam“ a vygenerovat dávky každých X sekund.

Příklad použití odškrtávaného záznamu naleznete v části [Analýza dat snímače pomocí Storm a HBase v HDInsight](hdinsight-storm-sensor-data-analysis.md).

Pokud používáte Trident, je založen na zpracování dávek záznamů.

###Ukládání do mezipaměti

Ukládání do mezipaměti se často používá jako mechanismus pro urychlení zpracování, protože udržuje často používané prostředky v paměti. Jelikož je topologie distribuována mezi více uzly a více procesy v každém uzlu, měli byste zvážit použití příkazu [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) a zajistit, že záznamy obsahující pole, které se používají pro vyhledávání v mezipaměti, jsou vždy směrovány do stejného procesu. Tím předejdete duplikace položek mezipaměti napříč procesy.

###Streamování horních N

Pokud vaše topologie závisí na výpočtu hodnoty „top N“, například top 5 trendů na Twitteru, doporučujeme vám vypočítat nejvyšší hodnotu N paralelně a pak sloučit výstup z těchto výpočtů do globální hodnoty. To lze provést pomocí [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) a nasměrovat hodnotu podle pole na paralelní funkce bolts (které rozdělují data podle hodnoty pole) a pak směrovat na funkce bolt, které globálně určují hodnotu hlavních rolí.

Příklad naleznete v části [RollingTopWords](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/RollingTopWords.java).

##Další kroky

Další informace o řešení pro analýzu v reálném čase s Apache Storm v HDInsight:

* [Začínáme se Storm v HDInsight][gettingstarted]

* [Příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.incubator.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.incubator.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md



<!--HONumber=Jun16_HO2-->


