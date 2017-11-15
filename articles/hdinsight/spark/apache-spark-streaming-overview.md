---
title: "Co je vysílání datového proudu Spark v Azure HDInsight | Microsoft Docs"
description: "Jak používat aplikace vysílání datového proudu Spark v clusterech HDInsight Spark."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2017
ms.author: nitinme
ms.openlocfilehash: 4699673255706a69d597ce28b47ab05a5f8eb957
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/09/2017
---
# <a name="overview-of-spark-streaming"></a>Přehled vysílání datového proudu Spark

Vysílání datového proudu Spark poskytuje datový proud zpracování dat v HDInsight Spark clusterů s záruku, že všechny vstupní událost zpracovány právě jednou, i v případě, že dojde k selhání uzlu. Datového proudu Spark se dlouho běžící úlohy, která přijímá vstupní data z široké škály zdrojů, včetně Azure Event Hubs, Azure IoT Hub, Kafka, Flume, Twitter, ZeroMQ, nezpracovaná sockets TCP, nebo z monitorování HDFS systémy. Na rozdíl od výhradně událostmi řízené procesu datového proudu Spark dávek vstupní data do časových oken, jako je například 2 druhý řez a pak transformuje jednotlivých dávek dat pomocí mapy, snížit, připojení a extrahovat operace. Zapíše pak Transformovaná data systémy, databáze, řídicí panely a konzole.

![Zpracování s HDInsight a vysílání datového proudu Spark datového proudu](./media/apache-spark-streaming-overview/hdinsight-spark-streaming.png)

Vysílání datového proudu Spark aplikace musí počkat několik zlomků druhou shromažďovat každý *micro batch* událostí před odesláním této batch zpracování. Naproti tomu založeného na událostech aplikace zpracovává všechny události okamžitě. Vysílání datového proudu Spark latence je obvykle v rámci několik sekund. Výhody tohoto přístupu micro batch jsou efektivnější zpracování dat a jednodušší agregační výpočty.

## <a name="introducing-the-dstream"></a>Představení DStream

Vysílání datového proudu Spark představuje nepřetržitý proud příchozí použití dat *diskretizovaný datového proudu* názvem DStream. DStream lze vytvořit ze vstupního zdroje jako Event Hubs nebo Kafka, nebo použití transformací pro jiné DStream.

DStream poskytuje abstrakční vrstvu data nezpracované události. 

Spustit s jedinou událost, vyslovení teplotě čtení z připojených termostatu. Když tato událost přijde aplikace vysílání datového proudu Spark, události je uložen v spolehlivě, tedy se replikují na víc uzlů. Odolnost proti chybám zajistí, že selhání žádné jeden uzel nebude vést ke ztrátě události. Spark core používá datová struktura, která distribuuje data mezi několika uzly v clusteru, kde každý uzel obecně udržuje vlastní data v paměti pro nejlepší výkon. Tato struktura dat se nazývá *odolné distribuované datovou sadu* (RDD).

Každý RDD představuje shromážděné přes názvem batch interval v časovém uživatelem definované události. Protože v každém intervalu batch uplyne, se vytvářejí nové RDD obsahující všechna data z tohoto intervalu. Spojité sady RDDs se shromažďují do DStream. Například pokud batch interval je 1 sekunda dlouhý, vaše DStream vysílá dávce každý druhý obsahující jeden RDD, která obsahuje všechna data požity během této druhé. Při zpracování DStream, teploty události se zobrazí v jednom z těchto dávek. K vysílání datového proudu Spark aplikaci zpracovává balíků, které obsahují události a nakonec na data uložená v každé RDD funguje.

![Příklad DStream s událostmi teploty ](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-example.png)

## <a name="structure-of-a-spark-streaming-application"></a>Struktura vysílání datového proudu Spark aplikace
Aplikace se vysílání datového proudu Spark je dlouho běžící aplikace, která přijímá data ze zdrojů ingestování, platí transformací pro zpracování dat a pak přenáší data do jednoho nebo více cílových míst. Struktura vysílání datového proudu Spark aplikace má dvě hlavní části. Nejprve definovat kde data pocházejí z jaké zpracování udělat na data a kde by měli přejít výsledky. Za druhé aplikaci spouštíte dobu neurčitou, čeká na signál k zastavení.

Tady je příklad jednoduchou aplikaci, která přijímá řádku textu přes TCP soketu a udává počet, kolikrát se zobrazí jednotlivých slov.

### <a name="define-the-application"></a>Definování aplikace

Definici aplikace logiky zahrnuje čtyři kroky:

1. Vytvořte StreamingContext.
2. Vytvoření DStream z StreamingContext.
3. Transformace aplikovat DStream.
4. Vypsání výsledků.

Tato definice je statická a zpracování žádná data, dokud nespustíte aplikace.

#### <a name="create-a-streamingcontext"></a>Vytvoření StreamingContext
Vytvoření StreamingContext z SparkContext, která odkazuje na clusteru. Při vytváření StreamingContext, zadejte velikost dávky v sekundách, například:

    val ssc = new StreamingContext(spark, Seconds(1))

#### <a name="create-a-dstream"></a>Vytvoření DStream
Pomocí StreamingContext instance, kterou jste vytvořili, vytvořte vstupní DStream pro vstupní zdroj. V takovém případě jsme se díváte pro vzhled nové soubory ve výchozím úložišti připojeném ke clusteru HDInsight.

    val lines = ssc.textFileStream("/uploads/2017/01/")

#### <a name="apply-transformations"></a>Použití transformací
Zpracování implementaci a použití transformace na DStream. Naše aplikace přijímá jeden řádek textu v čase ze souboru, rozdělí každý řádek na slova a pak používá vzor map-reduce počítat počet, který se zobrazí jednotlivých slov.

    val words = lines.flatMap(_.split(" "))
    val pairs = words.map(word => (word, 1))
    val wordCounts = pairs.reduceByKey(_ + _)

#### <a name="output-results"></a>Výstup výsledků
Vysunout výsledky transformace na cílové systémy použitím výstupních operací. V takovém případě ukážeme výsledek každé spuštění prostřednictvím výpočet ve výstupu konzoly.

    wordCounts.print()

### <a name="run-the-application"></a>Spuštění aplikace
Spuštění aplikace streamování a spustit, dokud neobdrží signál k ukončení.

    ssc.start()            
    ssc.awaitTermination()

Podrobnosti o rozhraní API datového proudu Spark, společně s zdroje událostí, transformace a výstupních operací podporuje najdete [Spark streamování Průvodce programováním](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html).

Následující ukázkové aplikace je samostatný, proto ji můžete spustit uvnitř [Poznámkový blok Jupyter](apache-spark-jupyter-notebook-kernels.md). V příkladu vytvoříme zdroj imitovaná data ve třídě DummySource, který produkuje hodnota čítače a aktuální čas v milisekundách každých pět sekund. Nemůžeme vytvořit nový objekt StreamingContext, který má batch intervalu 30 sekund. Pokaždé, když je vytvořen dávce, prozkoumá RDD vytváří, převede ji na Spark DataFrame a vytvoří dočasné tabulky přes DataFrame.

    class DummySource extends org.apache.spark.streaming.receiver.Receiver[(Int, Long)](org.apache.spark.storage.StorageLevel.MEMORY_AND_DISK_2) {

        /** Start the thread that simulates receiving data */
        def onStart() {
            new Thread("Dummy Source") { override def run() { receive() } }.start()
        }

        def onStop() {  }

        /** Periodically generate a random number from 0 to 9, and the timestamp */
        private def receive() {
            var counter = 0  
            while(!isStopped()) {
            store(Iterator((counter, System.currentTimeMillis)))
            counter += 1
            Thread.sleep(5000)
            }
        }
    }

    // A batch is created every 30 seconds
    val ssc = new org.apache.spark.streaming.StreamingContext(spark.sparkContext, org.apache.spark.streaming.Seconds(30))

    // Set the active SQLContext so that we can access it statically within the foreachRDD
    org.apache.spark.sql.SQLContext.setActive(spark.sqlContext)

    // Create the stream
    val stream = ssc.receiverStream(new DummySource())

    // Process RDDs in the batch
    stream.foreachRDD { rdd =>

        // Access the SQLContext and create a table called demo_numbers we can query
        val _sqlContext = org.apache.spark.sql.SQLContext.getOrCreate(rdd.sparkContext)
        _sqlContext.createDataFrame(rdd).toDF("value", "time")
            .registerTempTable("demo_numbers")
    } 

    // Start the stream processing
    ssc.start()

Potom jsme můžete dotazovat DataFrame pravidelně zobrazíte aktuální sadu hodnot přítomno v dávce. V tomto případě používáme následujícího dotazu SQL:

    %%sql
    SELECT * FROM demo_numbers

Výsledný výstup vypadá takto:

| hodnota | time |
| --- | --- |
|10 | 1497314465256 |
|11 | 1497314470272 |
|12 | 1497314475289 |
|13 | 1497314480310 |
|14 | 1497314485327 |
|15 | 1497314490346 |

Očekáváme, že zobrazíte šesti hodnoty, protože DummySource vytvoří hodnotu každých 5 sekund a jsme emitování dávky každých 30 sekund.

## <a name="sliding-windows"></a>Klouzavé windows
Pokud chcete provést agregační výpočtů na vaše DStream za nějaké časové období, například získat průměrnou teplotu přes poslední 2 sekundy, můžete použít posuvné okno operace součástí vysílání datového proudu Spark. Posuvné okno je definován jako mají určitou dobu trvání (do délky okna) a interval, během které obsah okna jsou vyhodnocovány (interval snímek).

Tyto klouzavé windows může dojít k překrytí, například můžete definovat okno s délkou 2 sekund, které snímky každou 1 sekundu. To znamená pokaždé, když provést výpočet agregace, okno bude obsahovat data z poslední 1 sekundu okna předchozí, a také žádná nová data v další 1 sekunda.

![Příklad počáteční okno s teploty události](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-window-01.png)

![Příklad okna s událostmi teploty po klouzavé](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-window-02.png)

V následujícím příkladu budeme aktualizovat kód, který používá DummySource ke shromáždění na listy do okna s po dobu jedné minuty a snímku jedné minuty.

    // A batch is created every 30 seconds
    val ssc = new org.apache.spark.streaming.StreamingContext(spark.sparkContext, org.apache.spark.streaming.Seconds(30))

    // Set the active SQLContext so that we can access it statically within the foreachRDD
    org.apache.spark.sql.SQLContext.setActive(spark.sqlContext)

    // Create the stream
    val stream = ssc.receiverStream(new DummySource())

    // Process batches in 1 minute windows
    stream.window(org.apache.spark.streaming.Minutes(1)).foreachRDD { rdd =>

        // Access the SQLContext and create a table called demo_numbers we can query
        val _sqlContext = org.apache.spark.sql.SQLContext.getOrCreate(rdd.sparkContext)
        _sqlContext.createDataFrame(rdd).toDF("value", "time")
        .registerTempTable("demo_numbers")
    } 

    // Start the stream processing
    ssc.start()

Po první minutu to předá 12 položky - šesti položky z každé dvě dávek shromážděné v okně.

| hodnota | time |
| --- | --- |
| 1 | 1497316294139 |
| 2 | 1497316299158
| 3 | 1497316304178
| 4 | 1497316309204
| 5 | 1497316314224
| 6 | 1497316319243
| 7 | 1497316324260
| 8 | 1497316329278
| 9 | 1497316334293
| 10 | 1497316339314
| 11 | 1497316344339
| 12 | 1497316349361

Posuvné okno funkce dostupné v rozhraní API vysílání datového proudu Spark zahrnují oken, countByWindow, reduceByWindow a countByValueAndWindow. Podrobnosti o těchto funkcích najdete v tématu [transformace na DStreams](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html#transformations-on-dstreams).

## <a name="checkpointing"></a>Vytváření kontrolních bodů
Zajistit odolnost proti chybám a odolnost proti chybám vysílání datového proudu Spark spoléhá na vytváření kontrolních bodů zajistit, že zpracování datového proudu můžete pokračovat bez přerušení, i při krátkodobém selhání uzlu. Spark v HDInsight, vytvoří kontrolní body do trvalého úložiště (Azure Storage nebo Data Lake Store). Tyto kontrolní body ukládat metadata o datových proudů aplikaci, například konfigurace, operace definované aplikace a všechny balíků, které byly zařazeny do fronty, ale ještě nebyla zpracována. V některých případech kontrolní body bude také obsahovat, uložení dat v RDDs tak, aby zkrátil čas potřebný k opětovnému sestavení stav data z co je součástí RDDs spravuje Spark.

## <a name="deploying-spark-streaming-applications"></a>Nasazení aplikací Spark streamování
Obvykle sestavit aplikaci místně vysílání datového proudu Spark a poté ji nasadit do Spark v HDInsight pomocí kopírování na soubor JAR, která obsahuje vaše aplikace a výchozí úložišti připojeném ke svému clusteru HDInsight. Pak můžete spustit aplikace pomocí rozhraní API REST LIVY dostupná z clusteru. Jedná se o operaci POST, kde text příspěvku obsahuje dokument JSON, který poskytuje cestu k JAR, název třídy, jejichž hlavní metoda definuje a spouští aplikace streamování a volitelně požadavky na prostředky (například počet ex úlohy ecutors, paměť a počet jader), a vyžaduje nastavení konfigurace kódu aplikace.

![Příklad okna s událostmi teploty po klouzavé](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-livy.png)

S požadavek GET na koncový bod LIVY můžete také zkontrolovat stav všech aplikací. Nakonec můžete ukončit běžící aplikaci po vydání požadavku na odstranění za provozu koncový bod. Informace o rozhraní API LIVY v [vzdálené úlohy se LIVY](apache-spark-livy-rest-interface.md)

## <a name="next-steps"></a>Další kroky

* [Vytvořit Apache Spark Cluster v HDInsight](../hdinsight-hadoop-create-linux-clusters-portal.md)
* [Průvodce programováním vysílání datového proudu Spark](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html)
* [Spuštění úlohy Spark vzdáleně pomocí LIVY](apache-spark-livy-rest-interface.md)