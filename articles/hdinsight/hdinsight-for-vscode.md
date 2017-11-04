---
title: "Nástroje Azure HDInsight - použití Visual Studio Code Hive, LLAP nebo pySpark | Microsoft Docs"
description: "Další informace o použití nástroje Azure HDInsight pro Visual Studio Code k vytvoření, odeslání dotazů a skriptů."
Keywords: "VScode, nástroje Azure HDInsight, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, interaktivní Hive, interaktivní dotazu"
services: HDInsight
documentationcenter: 
author: jejiang
manager: 
editor: jgao
tags: azure-portal
ms.assetid: 
ms.service: HDInsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/27/2017
ms.author: jejiang
ms.openlocfilehash: 9c1d0e0520df30306c1647cf1f3ec86c8a4fd8f5
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="use-azure-hdinsight-tool-for-visual-studio-code"></a>Pomocí nástroje Azure HDInsight pro Visual Studio Code

Další informace o použití nástroje Azure HDInsight pro Visual Studio Code (VSCode) k vytvoření, odeslání úloh Hive batch, interaktivních dotazů Hive a skripty, pySpark. Nástroje Azure HDInsight lze nainstalovat na platformách podporovaných aplikací VSCode včetně Windows, Linux a systému MacOS. Můžete najít požadavky pro různé platformy.


## <a name="prerequisites"></a>Požadavky

Následující položky jsou požadovány pro dokončení tohoto článku:

- HDInsight cluster.  Vytvoření clusteru s podporou naleznete v tématu [Začínáme s HDInsight]( hdinsight-hadoop-linux-tutorial-get-started.md).
- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).
- [Mono](http://www.mono-project.com/docs/getting-started/install/). Mono je pouze požadované pro systémy Linux a systému MacOS.

## <a name="install-the-hdinsight-tools"></a>Instalace nástrojů HDInsight
   
Po dokončení instalace požadované součásti, můžete nainstalovat nástroje Azure HDInsight pro VSCode. 

**K nástrojům nainstalovat Azure HDInsight**

1. Otevřete **Visual Studio Code**.
2. Klikněte na tlačítko **rozšíření** v levém podokně. Zadejte **HDInsight** do vyhledávacího pole.
3. Klikněte na tlačítko **nainstalovat** vedle **nástroje Azure HDInsight**. Za několik sekund **nainstalovat** tlačítko se změní na **opětovného načtení**.
4. Klikněte na tlačítko **opětovného načtení** aktivovat **nástroje Azure HDInsight** rozšíření.
5. Klikněte na tlačítko **opětovného načtení okno** k potvrzení. Můžete zobrazit **nástroje Azure HDInsight** v podokně rozšíření.

   ![HDInsight pro Visual Studio kód Python instalace](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

## <a name="open-hdinsight-workspace"></a>Otevřete pracovní prostor pro HDInsight

V VSCode potřebovat vytvořit pracovní prostor, aby mohli připojit k Azure.

**Otevřete pracovní prostor**

1. Z **soubor** nabídky, klikněte na tlačítko **otevřít složku**, zadat stávající složku nebo vytvořit novou složku jako pracovní složky. Složka se zobrazí v levém podokně.
2. V levém podokně klikněte na **nový soubor** Ikona vedle pracovní složky.

   ![Nový soubor](./media/hdinsight-for-vscode/new-file.png)
3. Pojmenujte nový soubor s .hql (dotazů Hive) nebo přípona souboru .py (Spark skript). Upozornění **XXXX_hdi_settings.json** konfigurační soubor je automaticky přidán do pracovní složky.
4. Otevřete **XXXX_hdi_settings.json** z **EXPLORER**, nebo klikněte pravým tlačítkem na editor skriptů a vyberte **nastavení konfigurace**. Přihlášení položku, výchozího clusteru a parametry pro odeslání úlohy, můžete nakonfigurovat, jak je znázorněno v ukázce v souboru. Můžete také ponechat ostatní parametry prázdné.

## <a name="connect-to-azure"></a>Připojení k Azure

Před skripty můžete odeslat ke clusterům HDInsight z VSCode, je nutné připojit ke svému účtu Azure.

**Pro připojení k Azure**

1. Pokud nemáte, vytvořte nové pracovní složky a nový soubor skriptu.
2. Klikněte pravým tlačítkem na editor skriptů a potom vyberte **HDInsight: přihlášení** v místní nabídce. Můžete také stisknout **CTRL + SHIFT + P** a zadáním **HDInsight: přihlášení**.

    ![Nástroje HDInsight pro Visual Studio Code přihlášení](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)
3. Postupujte podle pokynů přihlášení **výstup** podokně k přihlášení.

    **Azure:** ![nástroje HDInsight pro Visual Studio Code přihlašovací informace](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-Azurelogin-info.png)

    Po připojení se na stavovém řádku v levém dolním rohu okna VSCode zobrazí název účtu Azure. 

    > [!NOTE]
    > Otevřete prohlížeč s privátním režimu nebo incognito z důvodu problému známé Azure authentication. Pokud váš účet Azure má dva faktory, které jsou povolené, se doporučuje použít ověřování phone místo kódu Pin.
  

4. Klikněte pravým tlačítkem na skript upravit otevřete v místní nabídce. V místní nabídce můžete provádět následující úlohy:

    - odhlášení
    - Seznam clustery
    - Nastavení výchozího clusteru
    - Odeslání interaktivních dotazů Hive
    - Odeslání dávky skriptu Hive
    - Odeslání interaktivních dotazů PySpark
    - Odeslání PySpark dávkového skriptu
    - Konfigurace sady

## <a name="list-hdinsight-clusters"></a>Clustery HDInsight seznamu

Chcete-li otestovat připojení, můžete vytvořit seznam clusterům HDInsight:

**Seznam clusterů HDInsight v rámci vašeho předplatného Azure**
1. Otevřete pracovní prostor a připojit se k Azure. V tématu [prostoru otevřete HDInsight](#open-hdinsight-workspace) a [připojit k Azure](#connect-to-azure).
2. Klikněte pravým tlačítkem na editor skriptů a potom vyberte **HDInsight: Cluster seznamu** v místní nabídce. 
3. Clustery Hive a Spark se zobrazí v **výstup** podokně.

    ![Výchozí konfigurace clusteru sady](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-default-cluster"></a>Nastavení výchozího clusteru
1. Otevřete pracovní prostor a připojit se k Azure. V tématu [prostoru otevřete HDInsight](#open-hdinsight-workspace) a [připojit k Azure](#connect-to-azure).
2. Klikněte pravým tlačítkem na editor skriptů a pak klikněte na **HDInsight: nastavení clusteru výchozí**. 
3. Vyberte cluster jako výchozího clusteru pro aktuální soubor skriptu. Automaticky aktualizovat konfigurační soubor nástroje **XXXX_hdi_settings.json**. 

   ![Výchozí konfigurace clusteru sady](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-azure-environment"></a>Nastavení prostředí Azure 
1. Příkaz palety otevřít stisknutím kombinace kláves **CTRL + SHIFT + P**.
2. Zadejte **HDInsight: nastavte prostředí Azure**.
3. Vyberte jeden ze způsobů z Azure a AzureChina jako váš výchozí položku přihlášení.
4. Naše nástroj mezitím, již uložen, co jste vybrali výchozí položku přihlášení do **XXXX_hdi_settings.json**. Aktualizovány také přímo na tento konfigurační soubor. 

   ![výchozí přihlášení položku konfigurace sady](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="submit-interactive-hive-queries"></a>Odeslání interaktivních dotazů Hive

Nástroje HDInsight pro VSCode umožňují odesílání interaktivních dotazů Hive ke clusterům HDInsight interaktivní dotazu.

1. Pokud nemáte, vytvořte nové pracovní složky a nový soubor skriptu Hive.
2. Připojit k účtu Azure a pak nakonfigurujte výchozího clusteru, pokud jste tak dosud neučinili.
3. Zkopírujte a vložte následující kód do souboru Hive a pak ho uložit.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Klikněte pravým tlačítkem na editor skriptů a pak klikněte na **HDInsight: Hive interaktivní** odeslat dotaz. Nástroje lze také odeslat blok kódu místo souboru celou skriptu pomocí místní nabídky. Brzy NATO výsledku dotazu je zobrazuje novou kartu:

   ![interaktivní výsledek Hive](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - **VÝSLEDKY** panely: můžete uložit celý výsledek jako sdílený svazek clusteru, JSON, aplikace EXCEL na místní cestu nebo stačí vybrat více řádků.
    - **ZPRÁVY** panely: Kliknutím na tlačítko **řádku** číslo, přejde na první řádek spouštění skriptu.

Porovnání se [dávková úloha Hive](#submit-hive-batch-scripts), interaktivní dotaz trvá mnohem méně času.

## <a name="submit-hive-batch-scripts"></a>Odeslání dávky skripty Hive

1. Pokud nemáte, vytvořte nové pracovní složky a nový soubor skriptu Hive.
2. Připojit k účtu Azure a pak nakonfigurujte výchozího clusteru, pokud jste tak dosud neučinili.
3. Zkopírujte a vložte následující kód do souboru Hive a pak ho uložit.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Klikněte pravým tlačítkem na editor skriptů a pak klikněte na **HDInsight: Hive Batch** se odeslat úlohu Hive. 
4. Vyberte cluster, ve které chcete odeslat.  

    Po odeslání úlohy Hive, se zobrazí informace o úspěšné odeslání a jobid v **výstup** panelu. A otevře se **webový PROHLÍŽEČ** protokolu událostí v reálném čase úlohy a stav ukazuje.

   ![odeslat výsledek úlohy Hive](./media/hdinsight-for-vscode/submit-Hivejob-result.png)

Porovnání se [odesílání interaktivních dotazů Hive](#submit-interactive-hive-queries), dávková úloha trvá mnohem delší dobu.

## <a name="submit-interactive-pyspark-queries"></a>Odeslání interaktivních dotazů PySpark
Nástroje HDInsight pro VSCode také umožňují odeslání interaktivních dotazů PySpark ke clusterům Spark.
1. Pokud nemáte vytvořte nový pracovní složky a nový soubor skriptu s příponou .py.
2. Pokud jste tak dosud neučinili, připojte k účtu Azure.
3. Zkopírujte a vložte následující kód do souboru skriptu:
   ```python
   from operator import add
   lines = spark.read.text("/HdiSamples/HdiSamples/FoodInspectionData/README").rdd.map(lambda r: r[0])
   counters = lines.flatMap(lambda x: x.split(' ')) \
                .map(lambda x: (x, 1)) \
                .reduceByKey(add)

   coll = counters.collect()
   sortedCollection = sorted(coll, key = lambda r: r[1], reverse = True)

   for i in range(0, 5):
        print(sortedCollection[i])
   ```
4. Zvýrazněte tyto skripty a klikněte pravým tlačítkem na editor skriptů a pak klikněte na **HDInsight: PySpark interaktivní**.
5. Klikněte na následující **nainstalovat** tlačítko, pokud jste nenainstalovali **Python** rozšíření v VSCode.
    ![HDInsight pro Visual Studio kód Python instalace](./media/hdinsight-for-vscode/hdinsight-vscode-install-python.png)

6. Nastavení prostředí python ve vašem systému Pokud nemáte ho nainstalujete. 
   - Pro systém windows, stáhněte a nainstalujte [Python](https://www.python.org/downloads/). Zkontrolujte, zda je `Python` a `pip` ve vašem systému CESTU.
   - Instrukce pro systému MacOS a Linux, najdete v části [nastavit až PySpark interaktivní prostředí pro Visual Studio Code](set-up-pyspark-interactive-environment.md).
7. Vyberte cluster, který odeslat dotaz PySpark. Brzy NATO výsledku dotazu je zobrazuje právo novou kartu:

   ![odeslání úlohy výsledek python](./media/hdinsight-for-vscode/pyspark-interactive-result.png) 
8. Naše nástroj podporuje také dotaz **SQL klauzule**.

   ![odeslání úlohy python výsledek](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png) odeslání stav se zobrazí na levé straně stavového řádku dolní při spuštění dotazů. Jiné dotazy nelze odeslat, pokud je stav **jádra PySpark (zaneprázdněn)**, jinak, na kterém běží je zablokování.
9. Naše clusterů umožňuje udržovat relaci. Například **= 100**, již zachovat tuto relaci v clusteru, teď můžete spustit pouze **tisku** do clusteru.
 

## <a name="submit-pyspark-batch-job"></a>Odeslání PySpark dávkovou úlohu

1. Pokud nemáte vytvořte nový pracovní složky a nový soubor skriptu s příponou .py.
2. Pokud jste tak dosud neučinili, připojte k účtu Azure.
3. Zkopírujte a vložte následující kód do souboru skriptu:

    ```python
    from __future__ import print_function
    import sys
    from operator import add
    from pyspark.sql import SparkSession
    if __name__ == "__main__":
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
    
        lines = spark.read.text('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv').rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' '))\
                    .map(lambda x: (x, 1))\
                    .reduceByKey(add)
        output = counts.collect()
        for (word, count) in output:
            print("%s: %i" % (word, count))
        spark.stop()
    ```
4. Klikněte pravým tlačítkem na editor skriptů a pak klikněte na **HDInsight: PySpark Batch**. 
5. Vyberte cluster odešlete úlohu PySpark. 

   ![odeslání úlohy výsledek python](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Po odeslání úlohy python, odeslání protokolů se zobrazí v **výstup** okno v VSCode. **Adresa URL uživatelského rozhraní Spark** a **adresa URL uživatelského rozhraní Yarn** jsou také uvedeny. Adresu URL můžete otevřít ve webovém prohlížeči a sledovat stav úlohy.


## <a name="additional-features"></a>Další funkce

HDInsight pro VSCode podporuje následující funkce:

- **Automatické dokončování IntelliSense**. Návrhy jsou odebrány kolem – klíčové slovo, metoda, proměnné, atd. Různé ikony představují různé typy objektů:

    ![Nástroje HDInsight pro Visual Studio kódu IntelliSense typy objektů](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **Chyba značky IntelliSense**. Služba jazyka podtrhne úpravy chyby skriptu Hive.     
- **Syntaxe označuje**. Služba jazyka používá jinou barvu odlišit od proměnné, klíčová slova, datový typ, funkce atd. 

    ![Nástroje HDInsight pro Visual Studio Code syntaxe označuje](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Další kroky

### <a name="demo"></a>Ukázka
* HDInsight pro VScode: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření

* [Použití nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně prostřednictvím sítě VPN](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně přes SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Používat nástroje HDInsight pro vytvoření aplikací Spark v Azure nástrojů pro Eclipse](spark/apache-spark-eclipse-tool-plugin.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Vizualizovat data Hive s Microsoft Power BI v Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md).
* [Nastavení PySpark interaktivní prostředí pro Visual Studio Code](set-up-pyspark-interactive-environment.md)
* [Použití Zeppelin ke spouštění dotazů Hive v Azure HDInsight](./hdinsight-connect-hive-zeppelin.md)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](spark/apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikace datových proudů v reálném čase](spark/apache-spark-eventhub-streaming.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](spark/apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Vytváření a spouštění aplikací
* [Vytvoření samostatné aplikace pomocí Scala](spark/apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](spark/apache-spark-livy-rest-interface.md)

### <a name="managing-resources"></a>Správa prostředků
* [Správa prostředků v clusteru Apache Spark v Azure HDInsight](spark/apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](spark/apache-spark-job-debugging.md)



