<properties
    pageTitle="Kurz Apache Storm: Začínáme s linuxovým Storm v HDInsight | Microsoft Azure"
    description="Začněte s analýzami velkých objemů dat pomocí Apache Storm a ukázek Storm Starter na HDInsight se systémem Linux. Naučte se používat Storm pro zpracování dat v reálném čase."
    keywords="apache storm,kurz apache storm,analýza velkého objemu dat,storm starter"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="paulettm"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/16/2016"
   ms.author="larryfr"/>


# Kurz Apache Storm: Začněte s ukázkami Storm Starter pro analýzy velkých objemů dat v HDInsight

Apache Storm je škálovatelný výpočetní systém v reálném čase odolný proti chybám, distribuovaný určený pro zpracování datových proudů. Pomocí Storm v Azure HDInsight můžete vytvořit cloudový cluster Storm, který bude provádět analýzy velkých objemů dat v reálném čase.

> [AZURE.NOTE] Kroky v tomto článku vytváří cluster HDInsight se systémem Linux. Postup vytvoření Storm se systémem Windows v clusteru HDInsight, naleznete v části [Kurz Apache Storm: Začínáme s ukázkami používání analýzy dat Storm Starter v HDInsight](hdinsight-apache-storm-tutorial-get-started.md)

## Než začnete

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Musíte úspěšně dokončit tento kurz Apache Storm:

- **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Znalost SSH a SCP**. Další informace o použití SSH a SCP s HDInsight naleznete v následujících kapitolách:

    - **Klienti Linux a Unix OS X**: viz [Používání SSH se systémem Linux Hadoop v HDInsight ze systému Linux, OS X nebo Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    - **Klienti Windows**: viz [Použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

## Vytvoření clusteru Storm

V této části vytvoříte cluster HDInsight verze 3.2 (Storm verze 0.9.3) pomocí šablony Azure Resource Manageru. Informace o verzích HDInsight a jejich SLA naleznete v tématu [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md). Ostatní metody tvorby clusteru najdete v části [Tvorba clusterů HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Kliknutím na následující obrázek otevřete šablonu na portálu Azure Portal.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-storm-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Tato šablona se nachází v kontejneru veřejného objektu blob *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-storm-cluster-in-hdinsight.json*. 
   
2. Z okna Parametry zadejte následující údaje:

    - **Název clusteru**: Zadejte název pro cluster Hadoop, který chcete vytvořit.
    - **Přihlašovací jméno a heslo clusteru**: výchozí přihlašovací jméno je admin.
    - **Uživatelské jméno a heslo SSH**.
    
    Zapište tyto hodnoty.  Ty budete potřebovat později v kurzu.

    > [AZURE.NOTE] SSH se používá pro vzdálený přístup ke clusteru HDInsight pomocí příkazového řádku. Uživatelské jméno a heslo, které zde můžete použít, se používá při připojování ke clusteru prostřednictvím SSH. Uživatelské jméno SSH musí být také jedinečné, jelikož vytvoří uživatelský účet na všech uzlech clusteru HDInsight. Následují některé názvy účtů vyhrazené pro služby v clusteru, které nelze použít jako uživatelské jméno SSH:
    >
    > root, hdiuser, storm, hbase, ubuntu, zookeeper, hdfs, yarn, mapred, hbase, hive, oozie, falcon, sqoop, admin, tez, hcat, hdinsight-zookeeper.

    > Další informace o použití SSH s HDInsight naleznete v následujících článcích:

    > * [Použití SSH se systémem Linux Hadoop v HDInsight z OS X, Linux a Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    > * [Použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    
3. Klikněte na možnost **OK** a uložte parametry.

4. Z okna **Vlastní nasazení** klikněte na rozevírací pole **Skupina prostředků** a pak klikněte na tlačítko **Nový** a vytvořte novou skupinu prostředků. Skupina prostředků je kontejner, který seskupuje cluster, účet závislého úložiště a další propojené prostředky skupin.

5. Klikněte na tlačítko **Smluvní podmínky** a pak klikněte na tlačítko **Vytvořit**.

6. Klikněte na **Vytvořit**. Zobrazí se nová dlaždice s názvem Odeslání nasazení pro šablonu nasazení. Vytvoření clusteru a databáze SQL trvá přibližně 20 minut.


##Spustit ukázku Storm Starter v HDInsight

Ukázky [storm-starter](https://github.com/apache/storm/tree/master/examples/storm-starter) jsou zahrnuty v clusteru HDInsight. V následujících krocích spustíte příklad WordCount.

1. Připojte se ke clusteru HDInsight pomocí protokolu SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Pokud jste použili heslo k zabezpečení účtu uživatele SSH, budete vyzváni k jeho zadání. Pokud jste použili veřejný klíč, bude pravděpodobně muset použít parametr `-i` k určení odpovídajícího privátního klíče. Například, `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
        
    Další informace o použití SSH se systémem Linux HDInsight naleznete v následujících článcích:
    
    * [Použití SSH se systémem Linux Hadoop v HDInsight ze systému OS X, Linux a Unix.](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows](hdinsight-hadoop-linux-use-ssh-windows)

2. Následující příkaz použijte ke spuštění ukázkové topologie:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-0.10.0.2.4.2.4-5.jar storm.starter.WordCountTopology wordcount
        
    > [AZURE.NOTE] Část `0.10.0.2.4.2.4-5` názvu souboru se při aktualizaci HDInsight o novější verze Stormu může změnit.

    Tato akce spustí ukázkovou topologii WordCount v clusteru s popisným názvem „wordcount“. Bude náhodně generovat věty a počítat výskyt jednotlivých slov v těchto větách.

    > [AZURE.NOTE] Při odesílání topologie do clusteru je nutné nejprve zkopírovat soubor jar obsahující cluster před použitím příkazu `storm`. Můžete to provést pomocí příkazu `scp` z klienta, které se soubor nachází. Například: `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > Příklad WordCount a další příklady starter storm jsou již zahrnuty v clusteru na `/usr/hdp/current/storm-client/contrib/storm-starter/`.

##Monitorování topologie

Uživatelské rozhraní Storm poskytuje webové rozhraní pro práci se spuštěnými topologiemi a je součástí clusteru HDInsight.

Ke sledování topologie pomocí uživatelského rozhraní Storm použijte následující kroky:

1. Otevřete webový prohlížeč na https://CLUSTERNAME.azurehdinsight.net/stormui, kde __CLUSTERNAME__ je název clusteru. Otevře se uživatelské rozhraní Storm.

    > [AZURE.NOTE] Pokud budete vyzváni k zadání uživatelského jména a hesla, zadejte správce clusteru (admin) a heslo použité při vytvoření clusteru.

2. V části **Souhrn topologie**, vyberte položku **wordcount** ve sloupci **Název**. Tato akce zobrazí další informace o topologii.

    ![Řídicí panel Storm s informace o topologii WordCount Storm Starter.](./media/hdinsight-apache-storm-tutorial-get-started-linux/topology-summary.png)

    Tato stránka přináší následující informace:

    * **Statistiky topologie** – základní informace o výkonu topologie uspořádané do časových oken.

        > [AZURE.NOTE] Výběrem konkrétního časového okna změníte časové okno informací zobrazených v dalších částech stránky.

    * **Funkce Spouts** – základní informace o funkcích spouts, včetně poslední chyby vrácené každou funkcí spout.

    * **Funkce Bolts** – základní informace o funkcích bolts.

    * **Topologie konfigurace** – podrobné informace o konfiguraci topologie.

    Tato stránka také obsahuje akce, které můžete provést na topologii:

    * **Aktivovat** – obnoví zpracování deaktivované topologie.

    * **Deaktivovat** – pozastaví spuštěné topologie.

    * **Znovu vyvážit** – upraví paralelismus topologii. Po změně počtu uzlů v clusteru musíte znovu vyvážit spuštěné topologie. To umožňuje topologii upravovat paralelismus za účelem kompenzace zvýšení nebo snížení počtu uzlů v clusteru. Další informace naleznete v části [Pochopení paralelismu topologie Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

    * **Ukončit** – ukončí topologii Storm po zadaném časovém limitu.

3. Na této stránce vyberte položku z oddílu **Spouts** nebo **Bolts**. Zobrazí se informace o vybrané součásti.

    ![Řídicí panel Storm s informacemi o vybraných součástech.](./media/hdinsight-apache-storm-tutorial-get-started-linux/component-summary.png)

    Tato stránka obsahuje následující informace:

    * **Statistiky funkcí Spout/Bolt** – základní informace o výkonu komponenty uspořádané do časových oken.

        > [AZURE.NOTE] Výběrem konkrétního časového okna změníte časové okno informací zobrazených v dalších částech stránky.

    * **Statistiky vstupu** (pouze funkce bolt) – informace o komponentech, které vytváří dat využívaná funkcí bolt.

    * **Statististiky výstupu** – informace o datech vysílaných touto funkcí bolt.

    * **Prováděcí moduly** – informace o instancích této komponenty.

    * **Chyby** – chyby vytvořené touto komponentou.

4. Chcete-li zobrazit podrobnosti pro konkrétní instanci komponenty, při zobrazení podrobností o funkcích spout nebo bolt vyberte položku ze sloupce **Port** v oddílu **Vykonavatelé**.

        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]

    Z těchto dat můžete uvidíte, že k výskytu slova **sedm** došlo 1 493 957krát. Tolikrát bylo zjištěno od spuštění této topologie.

##Zastavení topologie

Vraťte se na stránku **Souhrn topologie**, kde naleznete topologii počtu slov a pak vyberte tlačítko **Zastavit** z oddílu **Topologie akce**. Po zobrazení výzvy zadejte hodnotu 10 jako počet sekund, po které se má počkat před zastavením topologie. Po uplynutí časového limitu se již topologie nezobrazí při návštěvě oddílu **Uživatelské rozhraní Storm** řídicího panelu.

##Odstranění clusteru

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a id="next"></a>Další kroky

V tomto kurzu Apache Storm jste využili Storm Starter, abyste se naučili vytvářet Storm na clusteru HDInsight a používat ovládací panel Storm k nasazení, monitorování a správě topologie Storm. Dále se naučíte, jak [Vyvíjet topologie založené na jazyce Java pomocí nástroje Maven](hdinsight-storm-develop-java-topology.md).

Pokud jste již obeznámeni s vývojem topologií založených na jazyce Java a chcete nasadit existující topologie do HDInsight, naleznete postup v části [Nasazení a správa topologií Apache Storm v HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md).

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[azureportal]: https://manage.windowsazure.com/
[hdinsight-provision]: hdinsight-provision-clusters.md
[preview-portal]: https://portal.azure.com/


<!--HONumber=Aug16_HO4-->


