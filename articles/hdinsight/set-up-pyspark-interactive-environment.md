---
title: "Nastavení nástroje Azure HDInsight - PySpark interaktivní prostředí pro Visual Studio Code | Microsoft Docs"
description: "Další informace o použití nástroje Azure HDInsight pro Visual Studio Code k vytvoření, odeslání dotazů a skriptů."
Keywords: "VScode, nástroje Azure HDInsight, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, interaktivní Hive, interaktivní dotazu"
services: HDInsight
documentationcenter: 
author: jejiang
manager: 
editor: 
tags: azure-portal
ms.assetid: 
ms.service: HDInsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/27/2017
ms.author: jejiang
ms.openlocfilehash: 839f379228322eb2da0ff61609634bf1f86e4bb3
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="set-up-pyspark-interactive-environment-for-visual-studio-code"></a>Nastavení PySpark interaktivní prostředí pro Visual Studio Code

Následující kroky ukazují, jak nainstalovat balíčky pythonu při spuštění **HDInsight: PySpark interaktivní**.


## <a name="set-up-pyspark-interactive-environment-on-macos-and-linux"></a>Nastavení PySpark interaktivní prostředí v systému MacOS a Linux
Je třeba pomocí příkazu **pip3** pro následující kroky, pokud je **python 3.x**.
1. Zajistěte, aby **Python** a **pip** nainstalována.
 
    ![verze pip Python](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

2.  Nainstalujte Jupyter
    ```
    sudo pip install jupyter
    ```
    +  Možná se následující chybová zpráva přijdete na Linuxu a systému MacOS:

        ![error1](./media/set-up-pyspark-interactive-environment/error1.png)
        ```Resolve:
        sudo pip uninstall asyncio
        sudo pip install trollies
        ```

    + Nainstalovat libkrb5 dev(For Linux only), možná zobrazí následující chybová zpráva:

        ![error2](./media/set-up-pyspark-interactive-environment/error2.png)
        ```Resolve:
        sudo apt-get install libkrb5-dev 
        ```

3. Nainstalujte sparkmagic
   ```
   sudo pip install sparkmagic
   ```

4. Ujistěte se, že ipywidgets je správně nainstalován spuštěním:
   ```
   sudo jupyter nbextension enable --py --sys-prefix widgetsnbextension
   ```
   ![Nainstalujte jádrech obálky](./media/set-up-pyspark-interactive-environment/ipywidget-enable.png)
 

5. Nainstalujte jádrech obálku. Spustit **pip zobrazit sparkmagic** a zobrazuje cestu této sparkmagic je nainstalovaná. 

    ![sparkmagic umístění](./media/set-up-pyspark-interactive-environment/sparkmagic-location.png)
   
6. Přejděte do umístění a spusťte:

   ```Python2
   sudo jupyter-kernelspec install sparkmagic/kernels/pysparkkernel   
   ```
   ```Python3
   sudo jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel
   ```

   ![instalace kernelspec jupyter](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-install.png)
7. Kontrola stavu instalace: 

    ```
    jupyter-kernelspec list
    ```
    ![seznam kernelspec jupyter](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-list.png)

    Pro dostupné jádra: **python2** a **pysparkkernel** odpovídají **python 2.x**, **python3** a  **pyspark3kernel** odpovídají **python 3.x**. 

8. Restartujte VScode a zpět na editor skriptů systémem **HDInsight: PySpark interaktivní**.

## <a name="next-steps"></a>Další kroky

### <a name="demo"></a>Ukázka
* HDInsight pro VScode: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření
* [Pomocí nástroje Azure HDInsight pro Visual Studio Code](hdinsight-for-vscode.md)
* [Použití nástrojů Azure pro IntelliJ k vytvoření a odeslání aplikací Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Použití nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně přes SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Použití nástrojů Azure pro IntelliJ k ladění aplikací Spark vzdáleně prostřednictvím sítě VPN](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Používat nástroje HDInsight pro vytvoření aplikací Spark v Azure nástrojů pro Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalace Jupyteru do počítače a připojení ke clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)
* [Vizualizovat data Hive s Microsoft Power BI v Azure HDInsight](./hdinsight-connect-hive-power-bi.md).
* [Použití Zeppelin ke spouštění dotazů Hive v Azure HDInsight ](./hdinsight-connect-hive-zeppelin.md).
