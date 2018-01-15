---
title: "Použití Apache Spark strukturovaná streamování pomocí Event Hubs v Azure HDInsight | Microsoft Docs"
description: "Sestavení Apache Spark streamování ukázku odesílat datový proud do centra událostí Azure a pak přijímat události v clusteru HDInsight Spark pomocí scala aplikace."
keywords: "Apache spark streamování, vysílání datového proudu spark, ukázka spark, apache spark streamování například ukázka azure centra událostí, spark ukázka"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jgao
ms.openlocfilehash: e0486d2c5f78da1d1e4a12703f120eccef43c305
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/12/2018
---
# <a name="apache-spark-structured-streaming-on-hdinsight-to-process-events-from-event-hubs"></a>Apache Spark strukturovaných streamování na HDInsight ke zpracování událostí ze služby Event Hubs

V tomto článku se dozvíte ke zpracování v reálném čase telemetrických dat pomocí Spark strukturovaných streamování. K tomu proveďte následující postup vysoké úrovně:

1. Zkompilování a spuštění na místní pracovní stanici ukázkovou aplikaci producent událost, která generuje události odesílat do centra událostí.
2. Použití [Spark prostředí](apache-spark-shell.md) definovat a spustit jednoduchou aplikaci strukturované datové proudy Spark.

## <a name="prerequisites"></a>Požadavky

* Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](apache-spark-jupyter-spark-sql.md).

* Na obor názvů Azure Event Hubs. V tématu [vytvoření oboru názvů Azure Event Hubs](apache-spark-eventhub-streaming.md#create-an-azure-event-hub) Další informace.

* Java Development kit Oracle. Můžete nainstalovat z [zde](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

* Apache Maven. Si můžete stáhnout z [zde](https://maven.apache.org/download.cgi). Pokyny k instalaci Maven jsou k dispozici [zde](https://maven.apache.org/install.html).

## <a name="build-configure-and-run-the-event-producer"></a>Vytvoření, konfiguraci a spuštění producent událostí
V této úloze naklonujte ukázkovou aplikaci, která vytvoří náhodných událostí a odesílá je do nakonfigurovaných centra událostí. Tato ukázková aplikace je k dispozici na webu GitHub na [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer).

1. Ujistěte se, že máte nainstalované nástroje git. Můžete stáhnout z [GIT stáhne](http://www.git-scm.com/downloads). 
2. Otevřete příkazový řádek nebo prostředí terminálu a spusťte následující příkaz z adresáře zvoleného klonovat projektu.
        
        git clone https://github.com/hdinsight/eventhubs-sample-event-producer.git eventhubs-client-sample

3. Tím se vytvoří novou složku s názvem eventhubs klienta sample. V rámci prostředí nebo příkazového řádku přejděte do této složky.
4. Spusťte Maven k sestavení aplikace pomocí následujícího příkazu:

          mvn package

5. V rámci prostředí nebo příkazového řádku přejděte do adresáře cíl, který je vytvořen a obsahuje soubor ``com-microsoft-azure-eventhubs-client-example-0.2.0.jar``.
6. Dále musíte vytvořit příkazový řádek pro spouštění událostí producent Centrum událostí. To provedete v hodnoty v příkazu nahraďte následujícím způsobem:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.azure.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "<namespace>" --eventhubs-name "hub1" --policy-name "RootManageSharedAccessKey" --policy-key "<policyKey>" --message-length 32 --thread-count 1 --message-count -1

7. Například dokončení příkazu by vypadat podobně jako následující:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.azure.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "sparkstreaming" --eventhubs-name "hub1" --policy-name "RootManageSharedAccessKey" --policy-key "2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU=" --message-length 32 --thread-count 1 --message-count -1

8. Spuštění příkazu a pokud vaše konfigurace je správná za chvíli zobrazí výstup týkající se událostí, které odesílá do vašeho centra událostí, podobný následujícímu:

        Map('policyKey -> 2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU, 'eventhubsName -> hub1, 'policyName -> RootManageSharedAccessKey, 'eventhubsNamespace -> sparkstreaming, 'messageCount -> -1, 'messageLength -> 32, 'threadCount -> 1)
        Events per thread: -1 (-1 for unlimited)
        10 > Sun Jun 18 11:32:58 PDT 2017 > 1024 > 1024 > Sending event: ZZ93958saG5BUKbvUI9wHVmpuA2TrebS
        10 > Sun Jun 18 11:33:46 PDT 2017 > 2048 > 2048 > Sending event: RQorGRbTPp6U2wYzRSnZUlWEltRvTZ7R
        10 > Sun Jun 18 11:34:33 PDT 2017 > 3072 > 3072 > Sending event: 36Eoy2r8ptqibdlfCYSGgXe6ct4AyOX3
        10 > Sun Jun 18 11:35:19 PDT 2017 > 4096 > 4096 > Sending event: bPZma9V0CqOn6Hj9bhrrJT0bX2rbPSn3
        10 > Sun Jun 18 11:36:06 PDT 2017 > 5120 > 5120 > Sending event: H2TVD77HNTVyGsVcj76g0daVnYxN4Sqs

9. Ponechte běží producent událostí, když budete pokračovat k kroky.

## <a name="run-spark-shell-on-your-hdinsight-cluster"></a>Spusťte prostředí Spark v HDInsight clusteru
V této úloze bude SSH do hlavního uzlu clusteru HDInsight, spusťte prostředí Spark a spusťte aplikaci vysílání datového proudu Spark, která načítá a zpracovává události ze služby Event Hubs. 

Pomocí tohoto bodu musí být připravené clusteru HDInsight. Pokud ne, budete muset počkat, dokud nedokončí zřizování. Až bude připravený, pokračujte následující kroky:

1. Připojte ke clusteru HDInsight pomocí SSH. Pokyny najdete v tématu [připojení k HDInsight pomocí SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

5. Aplikace, kterou vytvoříte vyžaduje balíček Spark streamování Event Hubs. Ke spuštění Spark prostředí tak, aby je automaticky načte tuto závislost z [Maven centrální](https://search.maven.org), ujistěte se, zadejte přepínač balíčky s souřadnice Maven následujícím způsobem:

        spark-shell --packages "com.microsoft.azure:spark-streaming-eventhubs_2.11:2.1.5"

6. Po dokončení prostředí Spark načítání, měli byste vidět:

        Welcome to
            ____              __
            / __/__  ___ _____/ /__
            _\ \/ _ \/ _ `/ __/  '_/
        /___/ .__/\_,_/_/ /_/\_\   version 2.1.1.2.6.2.3-1
            /_/
                
        Using Scala version 2.11.8 (OpenJDK 64-Bit Server VM, Java 1.8.0_151)
        Type in expressions to have them evaluated.
        Type :help for more information.

        scala> 

7. Zkopírujte následující fragment kódu do textového editoru a upravit ho tak, aby má klíč zásad a Namespace nastavit jako vhodné pro vaše Centrum událostí.

        val eventhubParameters = Map[String, String] (
            "eventhubs.policyname" -> "RootManageSharedAccessKey",
            "eventhubs.policykey" -> "<policyKey>",
            "eventhubs.namespace" -> "<namespace>",
            "eventhubs.name" -> "hub1",
            "eventhubs.partition.count" -> "2",
            "eventhubs.consumergroup" -> "$Default",
            "eventhubs.progressTrackingDir" -> "/eventhubs/progress",
            "eventhubs.sql.containsProperties" -> "true"
            )
            
8. Pokud se podíváte na váš koncový bod kompatibilní s EventHub v následující podobě, rámci, který čte `iothub-xxxxxxxxxx` je název vaší EventHub-compatible Namespace a lze použít pro `eventhubs.namespace`. Pole `SharedAccessKeyName` lze použít pro `eventhubs.policyname`, a `SharedAccessKey` pro `eventhubs.policykey`: 

        Endpoint=sb://iothub-xxxxxxxxxx.servicebus.windows.net/;SharedAccessKeyName=xxxxx;SharedAccessKey=xxxxxxxxxx 

9. Vložte fragment upravené do scala čekání > řádku a stiskněte klávesu návratový. Zobrazený výstup by měl vypadat přibližně takto:

        scala> val eventhubParameters = Map[String, String] (
            |       "eventhubs.policyname" -> "RootManageSharedAccessKey",
            |       "eventhubs.policykey" -> "2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU",
            |       "eventhubs.namespace" -> "sparkstreaming",
            |       "eventhubs.name" -> "hub1",
            |       "eventhubs.partition.count" -> "2",
            |       "eventhubs.consumergroup" -> "$Default",
            |       "eventhubs.progressTrackingDir" -> "/eventhubs/progress",
            |       "eventhubs.sql.containsProperties" -> "true"
            |     )
        eventhubParameters: scala.collection.immutable.Map[String,String] = Map(eventhubs.sql.containsProperties -> true, eventhubs.name -> hub1, eventhubs.consumergroup -> $Default, eventhubs.partition.count -> 2, eventhubs.progressTrackingDir -> /eventhubs/progress, eventhubs.policykey -> 2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU, eventhubs.namespace -> hdiz-docs-eventhubs, eventhubs.policyname -> RootManageSharedAccessKey)

10. V dalším kroku začnete vytvářet Spark strukturovaných streamování dotazu se zadat zdroj. Vložte následující údaje do prostředí Spark a stiskněte klávesu návratový.

        val inputStream = spark.readStream.
        format("eventhubs").
        options(eventhubParameters).
        load()

11. Zobrazený výstup by měl vypadat přibližně takto:

        inputStream: org.apache.spark.sql.DataFrame = [body: binary, offset: bigint ... 5 more fields]

12. V dalším kroku napsat dotaz tak, aby se zapíše svůj výstup do konzoly. To lze proveďte tak, že zkopírujete následující do prostředí Spark a stisknutím návratový.

        val streamingQuery1 = inputStream.writeStream.
        outputMode("append").
        format("console").start().awaitTermination()

13. Měli byste vidět některé dávky začínat výstup podobný následujícímu

        -------------------------------------------
        Batch: 0
        -------------------------------------------
        [Stage 0:>                                                          (0 + 2) / 2]

14. Následují výstup výsledky zpracování jednotlivých microbatch události. 

        -------------------------------------------
        Batch: 0
        -------------------------------------------
        17/06/18 18:57:39 WARN TaskSetManager: Stage 1 contains a task of very large size (419 KB). The maximum recommended task size is 100 KB.
        +--------------------+------+---------+------------+---------+------------+----------+
        |                body|offset|seqNumber|enqueuedTime|publisher|partitionKey|properties|
        +--------------------+------+---------+------------+---------+------------+----------+
        |[7B 22 74 65 6D 7...|     0|        0|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   112|        1|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   224|        2|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   336|        3|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   448|        4|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   560|        5|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   672|        6|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   784|        7|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   896|        8|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1008|        9|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1120|       10|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1232|       11|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1344|       12|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1456|       13|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1568|       14|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1680|       15|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1792|       16|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1904|       17|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  2016|       18|  1497734889|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  2128|       19|  1497734889|     null|        null|     Map()|
        +--------------------+------+---------+------------+---------+------------+----------+
        only showing top 20 rows

15. Jako nové události přicházejí od výrobce událostí, jsou zpracovávány tímto dotazem strukturovaných streamování.
16. Ujistěte se, že po dokončení spuštění této ukázce odstranění clusteru HDInsight.



## <a name="see-also"></a>Další informace najdete v tématech

* [Přehled vysílání datového proudu Spark](apache-spark-streaming-overview.md)













