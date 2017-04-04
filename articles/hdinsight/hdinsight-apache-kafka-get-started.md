---
title: "Začínáme s Apache Kafka v prostředí HDInsight | Dokumentace Microsoftu"
description: "Přečtěte si základní informace o vytváření a používání platforem Kafka v prostředí HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 43585abf-bec1-4322-adde-6db21de98d7f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/14/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 1c7a95354358d91859ce9d2019cef93b6662811f
ms.lasthandoff: 03/25/2017

---
# <a name="get-started-with-apache-kafka-preview-on-hdinsight"></a>Začínáme s Apache Kafka (Preview) v prostředí HDInsight

[Apache Kafka](https://kafka.apache.org) je open source distribuovaná streamovací platforma, která je dostupná pro HDInsight. Často se používá jako zprostředkovatel zpráv, protože nabízí funkce podobné frontě pro publikování a odběr zpráv. V tomto dokumentu si ukážeme, jak vytvořit cluster HDInsight se systémem Kafka a jak potom odesílat a přijímat data z aplikace v Javě.

> [!NOTE]
> Aktuálně jsou pro HDInsight dostupné dvě verze Kafka: 0.9.0 (HDInsight 3.4) a 0.10.0 (HDInsight 3.5). Postupy v tomto dokumentu platí pro HDInsight 3.5.

## <a name="prerequisite"></a>Požadavek

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Pro úspěšné dokončení tohoto kurzu Apache Kafka budete potřebovat následující:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Znalost SSH a SCP**. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

* [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) nebo ekvivalentní, například OpenJDK.

* [Apache Maven](http://maven.apache.org/) 

### <a name="access-control-requirements"></a>Požadavky na řízení přístupu

[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-a-kafka-cluster"></a>Vytvoření clusteru Kafka

Pomocí následujících kroků můžete vytvořit systém Kafka na clusteru HDInsight:

1. Na webu [Azure Portal](https://portal.azure.com) vyberte **+ NOVÉ**, **Inteligentní funkce a analýzy** a pak **HDInsight**.
   
    ![Vytvoření clusteru HDInsight](./media/hdinsight-apache-kafka-get-started/create-hdinsight.png)

2. V okně **Základy** zadejte následující informace:

    * **Název clusteru:** Název clusteru HDInsight.
    * **Předplatné:** Vyberte předplatné, které chcete použít.
    * **Uživatelské jméno přihlášení clusteru** a **Heslo přihlášení clusteru**: Přihlašovací údaje pro přístup ke clusteru pomocí protokolu HTTPS. Tyto přihlašovací údaje se používají i pro přístup ke službám, jako jsou webové uživatelské rozhraní Ambari nebo REST API.
    * **Uživatelské jméno Secure Shell (SSH:)** Přihlašovací údaje používané pro přístup ke clusteru přes SSH. Ve výchozím nastavení je heslo stejné jako pro přihlášení ke clusteru.
    * **Skupina prostředků:** Skupina prostředků, ve které se cluster vytváří.
    * **Umístění:** Oblast Azure, ve které se cluster vytváří.
   
    ![Výběr předplatného](./media/hdinsight-apache-kafka-get-started/hdinsight-basic-configuration.png)

3. Vyberte **Typ clusteru** a pak v okně **Konfigurace clusteru** zadejte tyto hodnoty:
   
    * **Typ clusteru:** Kafka

    * **Verze:** Kafka 0.10.0 (HDI 3.5)

    * **Úroveň clusteru:** Standard
     
    Nakonec uložte nastavení tlačítkem **Vybrat**.
     
    ![Výběr typu clusteru](./media/hdinsight-apache-kafka-get-started/set-hdinsight-cluster-type.png)

    > [!NOTE]
    > Pokud vaše předplatné Azure nemá přístup k verzi Kafka Preview, zobrazí se pokyny, jak tento přístup získat. Zobrazí se obrazovka s pokyny podobná této:
    >
    > ![Zpráva o verzi Preview: Pokud chcete nasadit spravovaný cluster Apache Kafka v HDInsight, pošlete nám e-mail se žádostí o přístup k verzi Preview.](./media/hdinsight-apache-kafka-get-started/no-kafka-preview.png)

4. Po výběru typu clusteru použijte tlačítko __Vybrat__ k výběru typu clusteru. Dále stisknutím tlačítka __Další__ dokončete základní konfiguraci.

5. V okně **Úložiště** vyberte nebo vytvořte účet úložiště. Pro ukázkový postup v tomto dokumentu ponechte všechna ostatní pole v tomto okně na výchozích hodnotách. Stisknutím tlačítka __Další__ uložte konfiguraci úložiště.

    ![Nastavení účtu úložiště pro HDInsight](./media/hdinsight-apache-kafka-get-started/set-hdinsight-storage-account.png)

6. V okně **Souhrn** zkontrolujte konfiguraci clusteru. Pomocí odkazů __Upravit__ opravte případná chybná nastavení. Nakonec stisknutím tlačítka Vytvořit cluster vytvořte.
   
    ![Souhrn konfigurace clusteru](./media/hdinsight-apache-kafka-get-started/hdinsight-configuration-summary.png)
   
    > [!NOTE]
    > Vytvoření clusteru trvá přibližně 20 minut.

## <a name="connect-to-the-cluster"></a>Připojení ke clusteru

Z klienta se připojte ke clusteru pomocí SSH. Pokud používáte systém Linux, Unix, MacOS nebo prostředí Bash ve Windows 10, použijte následující příkaz:

```ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net```

Místo **SSHUSER** použijte uživatelské jméno SSH zadané při vytváření clusteru. Místo **CLUSTERNAME** zadejte název vašeho clusteru.

Po zobrazení výzvy zadejte heslo, které jste pro účet SSH nastavili.

Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

##<a id="getkafkainfo"></a>Získání informací o hostiteli Zookeeper a Broker

Při práci s platformou Kafka musíte znát dvě hodnoty hostitelů; hostitele *Zookeeper* a hostitele *Broker*. Tito hostitelé se používají s rozhraním Kafka API s mnohými z nástrojů, které se s tímto systémem dodávají.

Podle následujícího postupu vytvoříte proměnné prostředí s informacemi o hostitelích. Tyto proměnné prostředí se používají v dalším postupu v tomto dokumentu.

1. Připojte se ke clusteru přes SSH a následujícím příkazem nainstalujte nástroj `jq`. Tento nástroj slouží k analýze dokumentů JSON a je užitečný při načítání informací hostitele zprostředkovatele:
   
    ```bash
    sudo apt -y install jq
    ```

2. Použitím následujících příkazů nastavte proměnné prostředí s informace získanými z Ambari. Místo __KAFKANAME__ zadejte název vašeho clusteru Kafka. Místo __PASSWORD__ zadejte heslo (správce) pro určené při vytváření clusteru.

    ```bash
    export KAFKAZKHOSTS=`curl --silent -u admin:'PASSWORD' -G http://headnodehost:8080/api/v1/clusters/KAFKANAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")'`

    export KAFKABROKERS=`curl --silent -u admin:'PASSWORD' -G http://headnodehost:8080/api/v1/clusters/KAFKANAME/services/HDFS/components/DATANODE | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")'`

    echo '$KAFKAZKHOSTS='$KAFKAZKHOSTS
    echo '$KAFKABROKERS='$KAFKABROKERS
    ```

    Následující text uvádí příklad obsahu proměnné `$KAFKAZKHOSTS`:
   
    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk3-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`
   
    Následující text uvádí příklad obsahu proměnné `$KAFKABROKERS`:
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`
   
    > [!WARNING]
    > Nespoléhejte na to, že informace vrácené v této relaci budou vždy přesné. Při škálování clusteru můžou být zprostředkovatelé přidáni nebo odebráni. Pokud dojde k chybě a dojde k nahrazení uzlu, může se změnit název hostitele pro uzel. 
    > 
    > Pokud chcete mít jistotu, že jsou informace platné, načtěte informace o hostitelích Zookeeper a Broker až krátce před jejich použitím.

## <a name="create-a-topic"></a>Vytvoření tématu

Kafka ukládá datové proudy do kategorií označovaných jako *témata*. V okně SSH připojenému k hlavnímu uzlu clusteru připojení použijte skript pro vytvoření tématu, dodaný se systémem Kafka:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
```

Tento příkaz se připojí k hostiteli Zookeeper s použitím informací uložených v proměnné `$KAFKAZKHOSTS` a vytvoří téma Kafka s názvem **test**. Vytvoření tématu můžete ověřit použitím následujícího skriptu pro vypsání témat:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
```

Výstup tohoto příkazu vypíše seznam témat Kafka, který obsahuje i téma **test**.

## <a name="produce-and-consume-records"></a>Produkce a konzumace záznamů

Kafka ukládá *záznamy* v tématech. Záznamy jsou vytvářeny *producenty* a spotřebovávány *konzumenty*. Producenti načítají záznamy ze *zprostředkovatelů* Kafka. Každý pracovní uzel v clusteru HDInsight je zprostředkovatelem Kafka.

Následujícím postupem uložíte záznamy do dříve vytvořeného tématu test a pak je načtete pomocí konzumenta:

1. V relaci SSH použijte skript pro zápis záznamu do tématu, dodaný se systémem Kafka:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    Po spuštění tohoto příkazu se nevrátíte do příkazového řádku. Místo toho zadejte několik textových zpráv a pak ukončete odesílání zpráv do tématu stisknutím **Ctrl+C**. Každý řádek se odešle jako samostatný záznam.

2. Ke čtení záznamů z tématu použijte skript dodaný se systémem Kafka:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --topic test --from-beginning
    ```
   
    Ten načte záznamy z tématu a zobrazí je. Parametr `--from-beginning` způsobí, že konzument začne načítat od začátku datového proudu a zpracuje tak všechny záznamy.

3. Konzumenta zastavíte stisknutím __Ctrl+C__.

## <a name="producer-and-consumer-api"></a>Rozhraní API pro producenta a konzumenta

Produkovat a konzumovat záznamy můžete také pomocí [rozhraní API systému Kafka](http://kafka.apache.org/documentation#api). Ke stažení a sestavení producenta a konzumenta v jazyce Java použijte následující postup:

1. Stáhněte si příklady ze stránky [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started). Příklad producenta/konzumenta najdete v projektu v adresáři `Producer-Consumer`. Tento příklad obsahuje následující třídy:
   
    * **Run** – spustí producenta nebo konzumenta.

    * **Producer** – uloží do tématu 1 000 000 záznamů.

    * **Consumer** – čte záznamy z tématu.

2. Na příkazové řádku ve vašem vývojovém prostředí přejděte do adresáře `Producer-Consumer` tohoto příkladu a pak zadáním následujícího příkazu vytvořte balíček jar:
   
    ```
    mvn clean package
    ```
   
    Tento příkaz vytvoří adresář s názvem `target`, který bude obsahovat soubor s názvem `kafka-producer-consumer-1.0-SNAPSHOT.jar`.

3. Pomocí následujících příkazů zkopírujte soubor `kafka-producer-consumer-1.0-SNAPSHOT.jar` do vašeho clusteru HDInsight:
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    Místo **SSHUSER** použijte jméno uživatele SSH pro váš cluster a místo **CLUSTERNAME** zadejte název clusteru. Po zobrazení výzvy zadejte heslo uživatele SSH.

4. Až příkaz `scp` dokončí kopírování souboru, připojte se ke clusteru pomocí SSH a pak následujícím příkazem zapište záznamy do tématu test, které jste vytvořili dříve.
   
    ```bash
    ./kafka-producer-consumer.jar producer $KAFKABROKERS
    ```
   
    Tento program spustí producenta a zapíše záznamy. Zobrazí se počitadlo jako ukazatel průběhu.

    > [!NOTE]
    > Pokud se zobrazí chyba oprávnění, pomocí následujícího příkazu nastavte soubor jako spustitelný: ```chmod +x kafka-producer-consumer.jar```

5. Po skončení procesu zadejte následující příkaz pro čtení záznamů:
   
    ```bash
    ./kafka-producer-consumer.jar consumer $KAFKABROKERS
    ```
   
    Zobrazí se počet načtených záznamů spolu s celkovým počtem. Je možné, že uvidíte víc než 1 000 000 záznamů, protože jste už do tématu odeslali několik záznamů pomocí skriptu v jednom z předchozích kroků.

6. Konzumenta ukončíte stisknutím __Ctrl+C__.

### <a name="multiple-consumers"></a>Víc současných konzumentů

Důležitou koncepcí platformy Kafka je, že konzumenti můžou při čtení záznamů používat skupiny konzumentů (definované pomocí ID skupiny). Výsledkem použití skupiny s více konzumenty je vyvážení zatížení při čtení záznamů z tématu. Každý konzument ze skupiny obdrží určitou část záznamů. Pokud chcete vidět tento proces v akci, použijte následující postup:

1. Otevřete novou relaci SSH ke clusteru, abyste mohli používat dvě současně. V obou relacích spusťte konzumenta se stejným ID skupiny pomocí příkazu:
   
    ```bash
    ./kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
    ```

    > [!NOTE]
    > Protože jde o novou relaci SSH, musíte pomocí příkazů popsaných v části [Získání informací o hostiteli Zookeeper a Broker](#getkafkainfo) nastavit proměnnou `$KAFKABROKERS`.

2. Sledujte, jak v každé relaci narůstá počet načtených záznamů. Celkový počet v obou relacích by měl být stejný jako v předchozím příkladu s jedním konzumentem.

Konzumace klienty ze stejné skupiny se realizuje rozdělením tématu na oddíly. Téma `test` vytvořené dříve má osm oddílů. Pokud otevřete osm relací SSH a v každé relaci spustíte konzumenta, bude každý z nich číst záznamy z jednoho oddílu tématu.

> [!IMPORTANT]
> Ve skupině příjemců nemůže být víc instancí konzumentů než má téma oddílů. V tomto příkladu může skupina obsahovat nejvýše 8 spotřebitelů, protože to je počet oddílů tématu. Můžete také mít víc skupin konzumentů, každou s maximálně 8 konzumenty.

Záznamy se v systému Kafka ukládají v pořadí, ve kterém je oddíl přijme. Pro dosažení doručování záznamů ve správném pořadí *v rámci oddílu* vytvořte skupinu příjemců, ve které bude počet instancí konzumentů odpovídat počtu oddílů. Pro dosažení doručování záznamů ve správném pořadí *v rámci tématu* vytvořte skupinu obsahující pouze jednu instanci konzumenta.

## <a name="streaming-api"></a>API pro streamování

Rozhraní API pro streamování bylo do platformy Kafka přidáno ve verzi 0.10.0; starší verze nechávají zpracování datových proudů na Apache Spark nebo Storm.

1. Pokud jste to už neudělali, stáhněte si do svého vývojového prostředí příklady ze stránky [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started). Příklad streamování najdete v projektu v adresáři `streaming`.
   
    Tento projekt obsahuje pouze jednu třídu, `Stream`, která čte záznamy z dříve vytvořeného tématu `test`. Počítá přečtená slova a odesílá každé slovo a aktuální počet do tématu s názvem `wordcounts`. Téma `wordcounts` vytvoříme později v této části.

2. Na příkazovém řádku ve vašem vývojovém prostředí přejděte do adresáře `Streaming` a pak zadáním následujícího příkazu vytvořte balíček jar:
   
    ```
    mvn clean package
    ```
   
    Tento příkaz vytvoří adresář s názvem `target`, který bude obsahovat soubor s názvem `kafka-streaming-1.0-SNAPSHOT.jar`.

3. Pomocí následujících příkazů zkopírujte soubor `kafka-streaming-1.0-SNAPSHOT.jar` do vašeho clusteru HDInsight:
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    Místo **SSHUSER** použijte jméno uživatele SSH pro váš cluster a místo **CLUSTERNAME** zadejte název clusteru. Po zobrazení výzvy zadejte heslo uživatele SSH.

4. Až příkaz `scp` dokončí kopírování souboru, připojte se ke clusteru pomocí SSH a pak následujícím příkazem vytvořte téma `wordcounts`:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS
    ```

5. Potom spusťte streamovací proces pomocí následujícího příkazu:
   
    ```bash
    ./kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS 2>/dev/null &
    ```
   
    Tento příkaz spustí streamovací proces na pozadí.

6. K odesílání zpráv do tématu `test` použijte následující příkaz. Tyto zprávy jsou zpracovány příkladem streamování:
   
    ```bash
    ./kafka-producer-consumer.jar producer $KAFKABROKERS &>/dev/null &
    ```

7. Následujícím příkazem zobrazíte výstup zapisovaný do tématu `wordcounts` streamovacím procesem:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --topic wordcounts --from-beginning --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
    ```
   
    > [!NOTE]
    > Pro zobrazení dat musíte konzumentovi říct, aby vypsal klíč a deserializátor, který se použije pro klíč a hodnotu. Název klíče je slovo a hodnota klíče obsahuje počet.
   
    Výstup se bude podobat následujícímu:
   
        dwarfs  13635
        ago     13664
        snow    13636
        dwarfs  13636
        ago     13665
        a       13803
        ago     13666
        a       13804
        ago     13667
        ago     13668
        jumped  13640
        jumped  13641
        a       13805
        snow    13637
   
    > [!NOTE]
    > Počet se zvýší pokaždé, když se načte nové slovo.

7. Stisknutím __Ctrl+C__ ukončete konzumenta a pak pomocí příkazu `fg` přeneste streamovací úlohu z pozadí zpět na popředí. Ukončete i ji stisknutím __Ctrl+C__.

## <a name="delete-the-cluster"></a>Odstranění clusteru

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Další kroky

V tomto dokumentu jste se naučili základy práce s platformou Apache Kafka v HDInsight. Další informace o práci s platformou Kafka najdete v těchto zdrojích:

* [Dokumentace Apache Kafka](http://kafka.apache.org/documentation.html) na webu kafka.apache.org.
* [Vytvoření repliky Kafka ve službě HDInsight pomocí MirrorMakeru](hdinsight-apache-kafka-mirroring.md)
* [Použití Apache Stormu se systémem Kafka ve službě HDInsight](hdinsight-apache-storm-with-kafka.md)
* [Použití Apache Sparku se systémem Kafka ve službě HDInsight](hdinsight-apache-spark-with-kafka.md)


