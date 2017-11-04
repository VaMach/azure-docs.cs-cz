---
title: "Správa prostředků v clusteru Apache Spark v Azure HDInsight | Microsoft Docs"
description: "Další informace o použití spravovat prostředky pro clustery Spark v Azure HDInsight pro dosažení vyššího výkonu."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9da7d4e3-458e-4296-a628-77b14643f7e4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: 24f6ce9c22aff727ef597cd7bed0c15b260b8aa0
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="manage-resources-for-apache-spark-cluster-on-azure-hdinsight"></a>Správa prostředků v clusteru Apache Spark v Azure HDInsight 

V tomto článku zjistíte, jak pro přístup k rozhraní jako uživatelské rozhraní Ambari, YARN uživatelského rozhraní, a Server historie Spark přidruženého k vašemu clusteru Spark. Můžete také informace o tom, jak ladit konfigurace clusteru pro optimální výkon.

**Požadavky:**

* Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="how-do-i-launch-the-ambari-web-ui"></a>Jak spuštění webové uživatelské rozhraní Ambari?
1. Z portálu [Azure Portal](https://portal.azure.com/) z úvodního panelu klikněte na dlaždici pro váš cluster Spark (pokud je připnutý na úvodní panel). Můžete také přejít na cluster pod položkou **Procházet vše** > **Clustery HDInsight**.
2. Pro váš cluster Spark klikněte na tlačítko **řídicí panel**. Po zobrazení výzvy zadejte přihlašovací údaje správce pro Spark cluster.

    ![Spusťte Ambari](./media/apache-spark-resource-manager/hdinsight-launch-cluster-dashboard.png "spuštění Správce prostředků")
3. Mělo by se zobrazit webového uživatelského rozhraní Ambari, jak je znázorněno na snímku obrazovky.

    ![Webovému uživatelskému rozhraní Ambari](./media/apache-spark-resource-manager/ambari-web-ui.png "webovému uživatelskému rozhraní Ambari")   

## <a name="how-do-i-launch-the-spark-history-server"></a>Jak spuštění serveru historie Spark?
1. Z portálu [Azure Portal](https://portal.azure.com/) z úvodního panelu klikněte na dlaždici pro váš cluster Spark (pokud je připnutý na úvodní panel).
2. V okně clusteru pod **rychlé odkazy**, klikněte na tlačítko **řídicí panel clusteru**. V **řídicí panel clusteru** okně klikněte na tlačítko **Spark historie serveru**.

    ![Spark historie serveru](./media/apache-spark-resource-manager/launch-history-server.png "Spark historie serveru")

    Po zobrazení výzvy zadejte přihlašovací údaje správce pro Spark cluster.

## <a name="how-do-i-launch-the-yarn-ui"></a>Jak mohu spustit uživatelské rozhraní Yarn?
Monitorování aplikací, které jsou aktuálně spuštěny v clusteru Spark můžete pomocí uživatelského rozhraní YARN.

1. V okně clusteru, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **YARN**.

    ![Spustit uživatelské rozhraní YARN](./media/apache-spark-resource-manager/launch-yarn-ui.png)

   > [!TIP]
   > Alternativně můžete také spustit uživatelské rozhraní YARN z uživatelského rozhraní Ambari. Chcete-li spustit uživatelské rozhraní Ambari, v okně clusteru, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **řídicí panel clusteru HDInsight**. V uživatelském rozhraní Ambari, klikněte na **YARN**, klikněte na tlačítko **rychlé odkazy**, klikněte na tlačítko active Resource Manager a pak klikněte na **uživatelského rozhraní správce prostředků**.
   >
   >

## <a name="what-is-the-optimum-cluster-configuration-to-run-spark-applications"></a>Jaká je optimální clusteru konfigurace ke spouštění aplikací Spark?
Tři klíčové parametry, které lze použít pro konfigurace Spark v závislosti na požadavcích aplikace jsou `spark.executor.instances`, `spark.executor.cores`, a `spark.executor.memory`. Vykonavatele je proces spuštění pro aplikaci Spark. Se spustí v pracovním uzlu a zodpovídá provádět úlohy pro aplikaci. Výchozí počet velikost vykonavatele pro každý cluster a vykonavatelů je vypočítáváno na počet uzlů pracovního procesu a velikost uzlu pracovníka. Tyto informace jsou uloženy v `spark-defaults.conf` o hlavních uzlech clusteru.

Tři konfigurační parametry lze nastavit na úrovni clusteru (pro všechny aplikace, které běží na clusteru) nebo lze zadat pro každou jednotlivých aplikací.

### <a name="change-the-parameters-using-ambari-ui"></a>Změňte parametry, pomocí uživatelského rozhraní Ambari
1. Z uživatelského rozhraní Ambari, klikněte na tlačítko **Spark**, klikněte na tlačítko **Contigs**a potom rozbalte **vlastní spark – výchozí**.

    ![Nastavení parametrů pomocí nástroje Ambari](./media/apache-spark-resource-manager/set-parameters-using-ambari.png)
2. Výchozí hodnoty jsou vhodné mít 4 Spark aplikace v clusteru současně spustit. Tyto hodnoty můžete změnit z uživatelského rozhraní, jak je uvedeno níže.

    ![Nastavení parametrů pomocí nástroje Ambari](./media/apache-spark-resource-manager/set-executor-parameters.png)
3. Klikněte na tlačítko **Uložit** se uložit změny konfigurace. V horní části stránky zobrazí se výzva k restartování všechny ovlivněné služby. Klikněte na tlačítko **restartujte**.

    ![Restartujte služby](./media/apache-spark-resource-manager/restart-services.png)

### <a name="change-the-parameters-for-an-application-running-in-jupyter-notebook"></a>Změňte parametry pro aplikace běžící v poznámkového bloku Jupyter
Pro aplikace běžící v poznámkového bloku Jupyter, můžete použít `%%configure` magic provádět změny konfigurace. V ideálním případě je nutné provést tyto změny na začátku aplikace před spuštěním vaše první buňky kódu. To zajistí, že konfigurace je použití s Livy relací, když se vytvoří. Pokud chcete změnit konfiguraci v pozdější fázi v aplikaci, musíte použít `-f` parametr. Však se tím způsobem, budou ztraceny všechny průběh v aplikaci.

Následující fragment kódu ukazuje, jak změnit konfiguraci pro aplikace běžící v Jupyter.

    %%configure
    {"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}

Parametry konfigurace musí být předán jako řetězec formátu JSON a musí být na další řádek po magic, jak je znázorněno v příkladu sloupec.

### <a name="change-the-parameters-for-an-application-submitted-using-spark-submit"></a>Změny, které parametry pro aplikaci odesílat pomocí spark odeslání
Následující příkaz je příklad toho, jak změnit parametry konfigurace pro aplikaci batch, které je odeslána pomocí `spark-submit`.

    spark-submit --class <the application class to execute> --executor-memory 3072M --executor-cores 4 –-num-executors 10 <location of application jar file> <application parameters>

### <a name="change-the-parameters-for-an-application-submitted-using-curl"></a>Změňte parametry pro aplikaci odesílat pomocí cURL
Následující příkaz je příklad toho, jak změnit parametry konfigurace pro aplikaci batch, které je odeslána pomocí pomocí cURL.

    curl -k -v -H 'Content-Type: application/json' -X POST -d '{"file":"<location of application jar file>", "className":"<the application class to execute>", "args":[<application parameters>], "numExecutors":10, "executorMemory":"2G", "executorCores":5' localhost:8998/batches

### <a name="how-do-i-change-these-parameters-on-a-spark-thrift-server"></a>Změna těchto parametrů na serveru Spark Thrift
Spark Thrift Server poskytuje JDBC nebo ODBC přístup ke clusteru Spark a slouží k dotazů Spark SQL služby. Nástroje, jako Power BI, Tableau atd. použijte protokol ODBC ke komunikaci se serverem Spark Thrift na provedení dotazů Spark SQL jako aplikace Spark. Při vytvoření clusteru Spark spuštění dvě instance serveru Spark Thrift jednu na každý hlavního uzlu. Každý Server Spark Thrift se zobrazí jako aplikace Spark v uživatelském rozhraní YARN.

Spark Thrift Server používá přidělení dynamické vykonavatele Spark a proto `spark.executor.instances` nepoužívá. Místo toho používá serveru Spark Thrift `spark.dynamicAllocation.minExecutors` a `spark.dynamicAllocation.maxExecutors` k určení počtu prováděcího modulu. Parametry konfigurace `spark.executor.cores` a `spark.executor.memory` slouží k úpravě vykonavatele velikost. Tyto parametry můžete změnit, jak je znázorněno v následujícím postupu.

* Rozbalte **Advanced spark thrift-sparkconf** kategorie se aktualizovat parametry `spark.dynamicAllocation.minExecutors`, `spark.dynamicAllocation.maxExecutors`, a `spark.executor.memory`.

    ![Konfigurace serveru Spark thrift](./media/apache-spark-resource-manager/spark-thrift-server-1.png)    
* Rozbalte **vlastní spark-thrift-sparkconf** kategorii k aktualizaci parametr `spark.executor.cores`.

    ![Konfigurace serveru Spark thrift](./media/apache-spark-resource-manager/spark-thrift-server-2.png)

### <a name="how-do-i-change-the-driver-memory-of-the-spark-thrift-server"></a>Změna ovladačů paměti serveru Spark Thrift
Paměť ovladač serveru Spark Thrift je nakonfigurován tak, aby 25 % velikost paměti RAM hlavního uzlu, za předpokladu, že je větší než 14 GB je celková velikost paměti RAM hlavního uzlu. Změna konfigurace paměti ovladačů, můžete použít rozhraní Ambari, jak je uvedeno níže.

* Z uživatelského rozhraní Ambari, klikněte na tlačítko **Spark**, klikněte na tlačítko **konfigurací**, rozbalte položku **Advanced spark env**a pak zadejte hodnotu pro **spark_thrift_cmd_opts**.

    ![Konfigurace serveru Spark thrift paměti RAM](./media/apache-spark-resource-manager/spark-thrift-server-ram.png)

## <a name="i-do-not-use-bi-with-spark-cluster-how-do-i-take-the-resources-back"></a>S clusterem Spark nepoužívám BI. Jak můžu zpět trvat prostředky?
Vzhledem k tomu, že používáme dynamické přidělování Spark, jenom prostředky, které se spotřebovávají thrift serveru jsou prostředky pro dvě aplikace předlohy. Uvolnění těchto prostředků je potřeba zastavit službu Thrift Server běžící v clusteru.

1. V uživatelském rozhraní Ambari, v levém podokně klikněte na **Spark**.
2. Na další stránce klikněte na tlačítko **Spark Thrift servery**.

    ![Restartujte thrift server](./media/apache-spark-resource-manager/restart-thrift-server-1.png)
3. Měli byste vidět dvě headnodes, na kterých běží serveru Spark Thrift. Klikněte na jednu z headnodes.

    ![Restartujte thrift server](./media/apache-spark-resource-manager/restart-thrift-server-2.png)
4. Další stránka obsahuje seznam všech služeb spuštěných na tomto headnode. V seznamu klikněte na tlačítko rozevíracího seznamu vedle serveru Spark Thrift a pak klikněte na tlačítko **Zastavit**.

    ![Restartujte thrift server](./media/apache-spark-resource-manager/restart-thrift-server-3.png)
5. Tento postup opakujte u dalších headnode také.

## <a name="my-jupyter-notebooks-are-not-running-as-expected-how-can-i-restart-the-service"></a>Moje poznámkové bloky Jupyter neběží podle očekávání. Jak je možné restartovat službu?
Spuštění webové uživatelské rozhraní Ambari, jak je uvedeno výše. V levém navigačním podokně klikněte na tlačítko **Jupyter**, klikněte na tlačítko **služby akce**a potom klikněte na **restartujte všechny**. Tato akce spustí službu Jupyter na všechny headnodes.

    ![Restart Jupyter](./media/apache-spark-resource-manager/restart-jupyter.png "Restart Jupyter")

## <a name="how-do-i-know-if-i-am-running-out-of-resources"></a>Jak poznám, že pokud používám mimo prostředky?
Jako v příkladu nahoře, spustíte uživatelské rozhraní Yarn. V tabulce clusteru metriky na obrazovce, zkontrolujte hodnoty **paměti používá** a **celkové paměti** sloupce. Pokud jsou velmi podobné 2 hodnoty, nemusí být dostatek prostředků ke spuštění na další aplikaci. Totéž platí i pro **VCores používá** a **VCores celkem** sloupce. Také v hlavní zobrazení, pokud je aplikace zůstanou ve **platných** stavu a není přechod do **systémem** ani **se nezdařilo** stavu, může se také jednat znamená, že ho není dostatek prostředků ke spuštění načtení.

    ![Resource Limit](./media/apache-spark-resource-manager/resource-limit.png "Resource Limit")

## <a name="how-do-i-kill-a-running-application-to-free-up-resource"></a>Jak ukončit spuštěné aplikace pro uvolnění prostředků?
1. V uživatelském rozhraní Yarn v levém panelu klikněte na tlačítko **systémem**. V seznamu spuštěných aplikací určit aplikace pro ukončeny a klikněte na **ID**.

    ![Příkaz kill App1](./media/apache-spark-resource-manager/kill-app1.png "Kill App1")

2. Klikněte na tlačítko **ukončení aplikace** v pravém horním rohu, pak klikněte na **OK**.

    ![Příkaz kill počítači App2](./media/apache-spark-resource-manager/kill-app2.png "Kill počítači App2")

## <a name="see-also"></a>Viz také
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](apache-spark-job-debugging.md)

### <a name="for-data-analysts"></a>Pro analytiky dat

* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin](apache-spark-machine-learning-mllib-ipython.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](apache-spark-custom-library-website-log-analysis.md)
* [Analýza dat telemetrie Application Insights pomocí Sparku v HDInsight](apache-spark-analyze-application-insight-logs.md)
* [Použití Caffe v Azure HDInsight Spark pro distribuované hloubkové learning](apache-spark-deep-learning-caffe.md)

### <a name="for-spark-developers"></a>Pro vývojáře, Spark

* [Vytvoření samostatné aplikace pomocí Scala](apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](apache-spark-livy-rest-interface.md)
* [Modul plug-in nástroje HDInsight pro IntelliJ IDEA pro vytvoření a odesílání aplikací Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](apache-spark-eventhub-streaming.md)
* [Použití modulu plug-in nástroje HDInsight pro IntelliJ IDEA pro vzdálené ladění aplikací Spark](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark](apache-spark-jupyter-notebook-install-locally.md)
