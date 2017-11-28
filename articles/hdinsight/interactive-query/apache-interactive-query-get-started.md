---
title: "Použijte interaktivní dotazu s Azure HDInsight | Microsoft Docs"
description: "Další informace o použití interaktivní dotazu (Hive LLAP) s HDInsight."
keywords: 
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0957643c-4936-48a3-84a3-5dc83db4ab1a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: jgao
ms.openlocfilehash: f53234ddd8362e74c421c18c8d500308f5a232a7
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="use-interactive-query-with-hdinsight"></a>Použijte interaktivní dotazu s HDInsight
Interaktivní dotazu (také nazývané Hive LLAP nebo [Live dlouhé a proces](https://cwiki.apache.org/confluence/display/Hive/LLAP)) je Azure HDInsight [clusteru typu](../hdinsight-hadoop-provision-linux-clusters.md#cluster-types). Interaktivní Query podporuje ukládání do mezipaměti v paměti, takže dotazů Hive rychlejší a víc interaktivní. 

Cluster služby interaktivní dotazu se liší od clusteru Hadoop. Obsahuje pouze službu Hive. 

> [!NOTE]
> Mají přístup ke službě Hive v clusteru interaktivní dotazu pouze prostřednictvím zobrazení Ambari Hive, Beeline a ovladač Microsoft Hive Open Database Connectivity (Hive ODBC). Nelze k němu přístup prostřednictvím konzoly nástroje Hive, Templeton, nástroje příkazového řádku Azure (Azure CLI) nebo Azure PowerShell. 
> 
> 

## <a name="create-an-interactive-query-cluster"></a>Vytvoření clusteru interaktivní dotazu
Informace o vytváření clusteru HDInsight naleznete v tématu [vytvoření Hadoop clusterů v HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Vyberte typ clusteru interaktivní dotazu.

## <a name="execute-hive-queries-from-interactive-query"></a>Spuštění dotazů Hive z interaktivní dotazu
Spuštění dotazů Hive, máte následující možnosti:

* Pomocí Power BI

    V tématu [vizualizovat velké objemy dat s Power BI v Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md).

* Použití Zeppelin

    V tématu [Zeppelin používá ke spouštění dotazů Hive v Azure HDInsight ](../hdinsight-connect-hive-zeppelin.md).

* Použití Visual Studia

    V tématu [připojení k Azure HDInsight a spouštět dotazy Hive pomocí nástrojů Data Lake pro Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-hive-queries).

* Pomocí kódu v sadě Visual Studio

    V tématu [použití Visual Studio Code Hive, LLAP nebo pySpark](../hdinsight-for-vscode.md).
* Spusťte Hive pomocí zobrazení Ambari Hive.
  
    V tématu [použití zobrazení Hive se systémem Hadoop v prostředí Azure HDInsight](../hadoop/apache-hadoop-use-hive-ambari-view.md).
* Spusťte Hive pomocí Beeline.
  
    V tématu [používání Hive s Hadoop v HDInsight s Beeline](../hadoop/apache-hadoop-use-hive-beeline.md).
  
    Můžete Beeline z hlavního uzlu nebo z prázdné hraniční uzel. Doporučujeme používat Beeline z prázdné hraniční uzel. Informace o vytváření clusteru služby HDInsight pomocí prázdné hraniční uzel, najdete v článku [použít prázdný edge uzly v HDInsight](../hdinsight-apps-use-edge-node.md).
* Spusťte Hive pomocí Hive ODBC.
  
    V tématu [připojit Excel k systému Hadoop pomocí ovladače Microsoft Hive ODBC](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

Najít připojovací řetězec připojení k databázi v jazyce Java (JDBC):

1. Přihlaste se k Ambari pomocí následující adresu URL: https://\<název clusteru\>. AzureHDInsight.net.
2. V nabídce vlevo vyberte **Hive**.
3. Zkopírujte adresu URL, vyberte ikonu schránky:
   
   ![JDBC LLAP interaktivních dotazů HDInsight Hadoop](./media/apache-interactive-query-get-started/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="next-steps"></a>Další kroky

* Zjistěte, jak [vytvářet interaktivní dotazu clusterů v HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).
* Zjistěte, jak [vizualizovat velké objemy dat s Power BI v Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md).
* Zjistěte, jak [použití Zeppelin ke spouštění dotazů Hive v Azure HDInsight ](../hdinsight-connect-hive-zeppelin.md).
* Zjistěte, jak [spouštění dotazů Hive pomocí nástrojů Data Lake pro Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-hive-queries).
* Zjistěte, jak [používat nástroje HDInsight pro Visual Studio Code](../hdinsight-for-vscode.md).
* Zjistěte, jak [pomocí zobrazení Hive Hadoop v HDInsight](../hadoop/apache-hadoop-use-hive-ambari-view.md)
* Zjistěte, jak [Beeline používat k odesílání dotazů Hive v HDInsight](../hadoop/apache-hadoop-use-hive-beeline.md).
* Zjistěte, jak [připojení aplikace Excel k systému Hadoop pomocí ovladače Microsoft Hive ODBC](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

