---
title: "Začínáme s Apache Storm v prostředí Azure HDInsight | Dokumentace Microsoftu"
description: "Začněte s analýzami velkých objemů dat pomocí Apache Storm a ukázek Storm Starter na HDInsight se systémem Linux. Naučte se používat Storm pro zpracování dat v reálném čase."
keywords: "apache storm,kurz apache storm,analýza velkého objemu dat,storm starter"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: d710dcac-35d1-4c27-a8d6-acaf8146b485
ms.service: hdinsight
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/17/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: cfaade8249a643b77f3d7fdf466eb5ba38143f18
ms.openlocfilehash: 4950dbe528290c7d839c97cc8770db4ae0ec08c6
ms.lasthandoff: 03/01/2017

---
#<a name="get-started-with-the-storm-starter-samples-for-big-data-analytics-on-linux-based-hdinsight"></a>Začínáme s ukázkami Storm Starter pro analýzy velkých objemů dat v linuxovém HDInsightu

Apache Storm je škálovatelný výpočetní systém v reálném čase odolný proti chybám, distribuovaný určený pro zpracování datových proudů. Pomocí Storm v Azure HDInsight můžete vytvořit cloudový cluster Storm, který bude provádět analýzy velkých objemů dat v reálném čase.

> [!IMPORTANT]
> HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v článku [Vyřazení služby HDInsight pro Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Znalost SSH a SCP**. Další informace o použití SSH a SCP v prostředí HDInsight naleznete v následujících dokumentech:
  
    * [Použití SSH se systémem Linux Hadoop ve službě HDInsight z prostředí Bash ve Windows 10, Linuxu, Unixu nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * [Použití SSH (PuTTY) se systémem Linux Hadoop ve službě HDInsight ze systému Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

### <a name="access-control-requirements"></a>Požadavky na řízení přístupu

[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-a-storm-cluster"></a>Vytvoření clusteru Storm

Pomocí následujících kroků můžete vytvořit Storm na clusteru HDInsight:

1. Na webu [Azure Portal](https://portal.azure.com) vyberte **+ NOVÉ**, **Inteligentní funkce a analýzy** a pak **HDInsight**.
   
    ![Vytvoření clusteru HDInsight](./media/hdinsight-apache-storm-tutorial-get-started-linux/create-hdinsight.png)

2. V okně **Základy** zadejte následující informace:

    * **Název clusteru:** Název clusteru HDInsight.
    * **Předplatné:** Vyberte předplatné, které chcete použít.
    * **Uživatelské jméno přihlášení clusteru** a **Heslo přihlášení clusteru**: Přihlašovací údaje pro přístup ke clusteru pomocí protokolu HTTPS. Tyto přihlašovací údaje se používají i pro přístup ke službám, jako jsou webové uživatelské rozhraní Ambari nebo REST API.
    * **Uživatelské jméno Secure Shell (SSH:)** Přihlašovací údaje používané pro přístup ke clusteru přes SSH. Ve výchozím nastavení je heslo stejné jako pro přihlášení ke clusteru.
    * **Skupina prostředků:** Skupina prostředků, ve které se cluster vytváří.
    * **Umístění:** Oblast Azure, ve které se cluster vytváří.
   
    ![Výběr předplatného](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-basic-configuration.png)

3. Vyberte **Typ clusteru** a pak v okně **Konfigurace clusteru** zadejte tyto hodnoty:
   
    * **Typ clusteru:** Storm

    * **Operační systém:** Linux

    * **Verze:** Storm 1.0.1 (HDI 3.5)

    * **Úroveň clusteru:** Standard
     
    Nakonec uložte nastavení tlačítkem **Vybrat**.
     
    ![Výběr typu clusteru](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-cluster-type.png)

4. Po výběru typu clusteru použijte tlačítko __Vybrat__ k výběru typu clusteru. Dále stisknutím tlačítka __Další__ dokončete základní konfiguraci.

5. V okně **Úložiště** vyberte nebo vytvořte účet úložiště. Pro ukázkový postup v tomto dokumentu ponechte všechna ostatní pole v tomto okně na výchozích hodnotách. Stisknutím tlačítka __Další__ uložte konfiguraci úložiště.

    ![Nastavení účtu úložiště pro HDInsight](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-storage-account.png)

6. V okně **Souhrn** zkontrolujte konfiguraci clusteru. Pomocí odkazů __Upravit__ opravte případná chybná nastavení. Nakonec stisknutím tlačítka Vytvořit cluster vytvořte.
   
    ![Souhrn konfigurace clusteru](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-configuration-summary.png)
   
    > [!NOTE]
    > Vytvoření clusteru trvá přibližně 20 minut.

## <a name="run-a-storm-starter-sample-on-hdinsight"></a>Spustit ukázku Storm Starter v HDInsight

1. Připojte se ke clusteru HDInsight pomocí protokolu SSH:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Pokud jste použili heslo k zabezpečení uživatelského účtu SSH, zobrazí se výzva k jeho zadání. Pokud jste použili veřejný klíč, bude pravděpodobně muset použít parametr `-i` k určení odpovídajícího privátního klíče. Například, `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
   
    Další informace o použití SSH se systémem Linux HDInsight naleznete v následujících článcích:
   
    * [Použití SSH se systémem Linux Hadoop ve službě HDInsight z prostředí Bash ve Windows 10, Linuxu, Unixu nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Použití SSH (PuTTY) se systémem Linux Hadoop ve službě HDInsight ze systému Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Následující příkaz použijte ke spuštění ukázkové topologie:
   
        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology wordcount
   
    > [!NOTE]
    > V dřívějších verzích služby HDInsight je název třídy topologie `storm.starter.WordCountTopology` místo `org.apache.storm.starter.WordCountTopology`.
   
    Tento příkaz spustí ukázkovou topologii WordCount v clusteru s popisným názvem „wordcount“. Bude náhodně generovat věty a počítat výskyt jednotlivých slov v těchto větách.
   
    > [!NOTE]
    > Při odesílání vlastních topologií do clusteru je před použitím příkazu `storm` nutné nejprve zkopírovat soubor jar obsahující cluster. Ke zkopírování tohoto souboru použijte příkaz `scp`. Například `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`.
    > 
    > Příklad WordCount a další příklady starter storm jsou již zahrnuty v clusteru na `/usr/hdp/current/storm-client/contrib/storm-starter/`.

Pokud si chcete prohlédnout zdrojové kódy příkladů starter storm, najdete je na [https://github.com/apache/storm/tree/1.0.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.0.x-branch/examples/storm-starter). Tento odkaz je pro Storm 1.0.x, který je součástí služby HDInsight 3.5. Pro ostatní verze Stormu použijte tlačítko __Větev__ v horní části stránky a vyberte jinou verzi Stormu.

## <a name="monitor-the-topology"></a>Monitorování topologie

Uživatelské rozhraní Storm poskytuje webové rozhraní pro práci se spuštěnými topologiemi a je součástí clusteru HDInsight.

Ke sledování topologie pomocí uživatelského rozhraní Storm použijte následující kroky:

1. Pokud chcete zobrazit uživatelské rozhraní Storm, ve webovém prohlížeči otevřete stránku https://CLUSTERNAME.azurehdinsight.net/stormui. Nahraďte **CLUSTERNAME** názvem vašeho clusteru.
    
    > [!NOTE]
    > Pokud budete vyzváni k zadání uživatelského jména a hesla, zadejte správce clusteru (admin) a heslo použité při vytvoření clusteru.

2. V části **Souhrn topologie**, vyberte položku **wordcount** ve sloupci **Název**. Zobrazí se další informace o topologii.
    
    ![Řídicí panel Storm s informace o topologii WordCount Storm Starter.](./media/hdinsight-apache-storm-tutorial-get-started-linux/topology-summary.png)
    
    Tato stránka přináší následující informace:
    
    * **Statistiky topologie** – základní informace o výkonu topologie uspořádané do časových oken.
     
        > [!NOTE]
        > Výběrem konkrétního časového okna změníte časové okno informací zobrazených v dalších částech stránky.

    * **Funkce Spouts** – základní informace o funkcích spouts, včetně poslední chyby vrácené každou funkcí spout.
    
    * **Funkce Bolts** – základní informace o funkcích bolts.
    
    * **Topologie konfigurace** – podrobné informace o konfiguraci topologie.
     
    Tato stránka také obsahuje akce, které můžete provést na topologii:
   
    * **Aktivovat** – obnoví zpracování deaktivované topologie.
    
    * **Deaktivovat** – pozastaví spuštěné topologie.
    
    * **Znovu vyvážit** – upraví paralelismus topologii. Po změně počtu uzlů v clusteru musíte znovu vyvážit spuštěné topologie. Nové vyvážení upraví paralelismus, aby se vykompenzovalo zvýšení nebo snížení počtu uzlů v clusteru. Další informace naleznete v části [Pochopení paralelismu topologie Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).
    
    * **Ukončit** – ukončí topologii Storm po zadaném časovém limitu.

3. Na této stránce vyberte položku z oddílu **Spouts** nebo **Bolts**. Zobrazí se informace o vybrané komponentě.
   
    ![Řídicí panel Storm s informacemi o vybraných součástech.](./media/hdinsight-apache-storm-tutorial-get-started-linux/component-summary.png)
   
    Tato stránka obsahuje následující informace:
   
    * **Statistiky funkcí Spout/Bolt** – základní informace o výkonu komponenty uspořádané do časových oken.
     
        > [!NOTE]
        > Výběrem konkrétního časového okna změníte časové okno informací zobrazených v dalších částech stránky.
     
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
   
    V tomto příkladu se slovo **seven** vyskytlo 1493957krát. Tolikrát bylo toto slovo zjištěno od spuštění této topologie.

## <a name="stop-the-topology"></a>Zastavení topologie

Vraťte se na stránku **Souhrn topologie**, kde naleznete topologii počtu slov a pak vyberte tlačítko **Zastavit** z oddílu **Topologie akce**. Po zobrazení výzvy zadejte hodnotu 10 jako počet sekund, po které se má počkat před zastavením topologie. Po uplynutí časového limitu se topologie už při návštěvě oddílu **Uživatelské rozhraní Storm** řídicího panelu nezobrazí.

## <a name="delete-the-cluster"></a>Odstranění clusteru

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a id="next"></a>Další kroky

V tomto kurzu Apache Storm jste se naučili základy práce se Stormem v HDInsightu. Dále se naučíte, jak [Vyvíjet topologie založené na jazyce Java pomocí nástroje Maven](hdinsight-storm-develop-java-topology.md).

Pokud jste již obeznámeni s vývojem topologií založených na jazyce Java a chcete nasadit existující topologie do HDInsight, naleznete postup v části [Nasazení a správa topologií Apache Storm v HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md).

Pokud jste vývojář .NET, můžete s použitím sady Visual Studio vytvořit topologie C# nebo hybridní topologie C#/Java. Další informace najdete v tématu [Vývoj topologií C# pro Apache Storm ve službě HDInsight pomocí nástrojů Hadoop pro Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Příklady topologií, které se dají použít se systémem Storm ve službě HDInsight:

* [Příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[azureportal]: https://manage.windowsazure.com/
[hdinsight-provision]: hdinsight-provision-clusters.md
[preview-portal]: https://portal.azure.com/

