---
title: "Apache Spark strukturovaných streamování s Kafka - Azure HDInsight | Microsoft Docs"
description: "Naučte se používat Apache Spark streamování (DStream) k získání dat do nebo z Apache Kafka. V tomto příkladu stream dat pomocí poznámkového bloku Jupyter z Spark v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/07/2017
ms.author: larryfr
ms.openlocfilehash: 39734153a757d4665f2a15d3af244d2fab9ad9d7
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="use-spark-structured-streaming-with-kafka-on-hdinsight"></a>Používejte Spark strukturovaná streamování s Kafka v HDInsight

Naučte se používat Spark strukturovaných streamování číst data z Apache Kafka v Azure HDInsight.

Vysílání datového proudu strukturovaná Spark je modul zpracování datového proudu, který je založený na Spark SQL. Umožňuje express streamování výpočty stejná jako výpočetní batch na statických dat. Další informace o strukturovaných streamování najdete v tématu [strukturovaných streamování Průvodce programováním [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) na Apache.org.

> [!IMPORTANT]
> Tento příklad používá Spark 2.1 na HDInsight 3.6. Strukturované Streaming se považuje za __alpha__ 2.1 Spark.
>
> Kroky v tomto dokumentu vytvořte skupinu prostředků Azure, která obsahuje oba Spark v HDInsight a Kafka na clusteru HDInsight. Tyto clustery jsou obě nachází v rámci virtuální síť Azure, což umožňuje clusteru Spark přímo komunikovat s Kafka clusteru.
>
> Po dokončení kroků v tomto dokumentu, nezapomeňte odstranit clustery nadbytečné náklady.

## <a name="create-the-clusters"></a>Vytváření clusterů

Apache Kafka v HDInsight neposkytuje přístup k zprostředkovatelé Kafka prostřednictvím veřejného Internetu. Všechno, co komunikuje se Kafka musí být ve stejné virtuální síti Azure jako uzly v clusteru Kafka. V tomto příkladu jsou Kafka i Spark clusterů umístěné v virtuální sítě Azure. Následující diagram znázorňuje tok komunikace mezi clustery:

![Diagram clustery Spark a Kafka v virtuální sítě Azure](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Kafka služby je omezený na komunikaci v rámci virtuální sítě. Jiné služby v clusteru, například SSH a Ambari, jsou přístupné přes internet. Další informace o veřejné porty, které jsou k dispozici s HDInsight naleznete v tématu [porty a identifikátory URI používají v prostředí HDInsight](hdinsight-hadoop-port-settings-for-services.md).

Když vytvoříte virtuální síť Azure, Kafka, a clustery Spark ručně, je jednodušší použít šablonu Azure Resource Manager. Použijte následující kroky k nasazení virtuální sítě Azure, Kafka a clustery k předplatnému Azure z Spark.

1. Na následující tlačítko použijte pro přihlášení do Azure a otevřete šablonu na portálu Azure.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Šablona Azure Resource Manager je umístěna ve **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.

    Tato šablona vytváří v následujících zdrojích informací:

    * Kafka v clusteru HDInsight 3.6.
    * Spark v HDInsight 3.6 clusteru.
    * Virtuální síť Azure, který obsahuje clusterů HDInsight.

    > [!IMPORTANT]
    > Strukturované streamování poznámkového bloku použitý v tomto příkladu vyžaduje Spark v HDInsight 3.6. Pokud používáte starší verzi Spark v HDInsight, zobrazí se chyba při použití poznámkového bloku.

2. Následující informace slouží k naplnění položek na **vlastní nasazení** části:
   
    ![HDInsight vlastní nasazení](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * **Skupina prostředků**: vytvoření skupiny nebo vyberte nějaký existující. Tato skupina obsahuje clusteru HDInsight.

    * **Umístění**: Vyberte umístění geograficky blízko vás.

    * **Základní název clusteru**: Tato hodnota se používá jako základní název pro Spark a Kafka clusterů. Například zadáním **hdi** vytvoří Spark clusteru s názvem spark hdi__ a Kafka clusteru s názvem **kafka hdi**.

    * **Uživatelské jméno přihlášení clusteru**: uživatelské jméno správce pro clustery Spark a Kafka.

    * **Heslo pro přihlášení clusteru**: uživatelské heslo správce pro clustery Spark a Kafka.

    * **Uživatelské jméno SSH**: SSH, aby uživatel vytvořil pro clustery Spark a Kafka.

    * **Heslo SSH**: heslo pro uživatele SSH pro clustery Spark a Kafka.

3. Pro čtení **podmínky a ujednání**a potom vyberte **souhlasím s podmínkami a ujednáními výše uvedených**.

4. Nakonec zkontrolujte **připnout na řídicí panel** a pak vyberte **nákupu**. Chcete-li vytvořit clustery trvá asi 20 minut.

Po vytvoření prostředky se zobrazí souhrnná stránka.

![Informace o skupině prostředků pro virtuální síť a clustery](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> Všimněte si, že jsou názvy clusterů HDInsight **spark BASENAME** a **kafka BASENAME**, kde BASENAME je jméno, které jste zadali v šabloně. Názvy těchto používat v dalších krocích při připojování k clustery.

## <a name="get-the-kafka-brokers"></a>Získat Kafka zprostředkovatelé

Kód v tomto příkladu se připojí k Kafka zprostředkovatele hostitele v clusteru Kafka. Pokud chcete najít adresy dvou zprostředkovatele hostitelů Kafka, použijte následující příklad PowerShell nebo Bash:

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
$clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$brokerHosts = $respObj.host_components.HostRoles.host_name[0..1]
($brokerHosts -join ":9092,") + ":9092"
```

```bash
curl -u admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
```

Po zobrazení výzvy zadejte heslo pro účet Clusterové přihlášení (správce)

> [!NOTE]
> Tento příklad předpokládá, že `$CLUSTERNAME` tak, aby obsahovala název clusteru Kafka.
>
> Tento příklad používá [jq](https://stedolan.github.io/jq/) nástroj analyzovat data z dokumentu JSON.

Výstup se bude podobat následujícímu:

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

Tyto informace uložte, protože se používá v následujících částech tohoto dokumentu.

## <a name="get-the-notebooks"></a>Získat poznámkových bloků

Kód pro tento příklad popsané v tomto dokumentu je k dispozici na [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).

## <a name="upload-the-notebooks"></a>Nahrát poznámkových bloků

Nahrát poznámkových bloků z projektu do vaší Spark v clusteru HDInsight pomocí následujících kroků:

1. Ve webovém prohlížeči připojte do poznámkového bloku Jupyter v clusteru Spark. V následující adresu URL, nahraďte `CLUSTERNAME` s názvem vaší __Spark__ clusteru:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Po zobrazení výzvy zadejte přihlašovací clusteru (správce) a heslo použité při vytváření clusteru.

2. Použijte v horní pravé části stránky se __nahrát__ tlačítko Odeslat __datového proudu-Tweetů-To_Kafka.ipynb__ souboru do clusteru. Vyberte __otevřete__ spusťte.

    ![Pomocí tlačítka nahrávání vyberte a odeslat Poznámkový blok](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![Vyberte soubor KafkaStreaming.ipynb](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. Najít __datového proudu-Tweetů-To_Kafka.ipynb__ položku v seznamu poznámkových bloků a vyberte __nahrát__ tlačítko vedle ní.

    ![Pokud chcete nahrát poznámkového bloku, použijte tlačítko nahrávání KafkaStreaming.ipynb položku](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)

4. Opakujte kroky 1 – 3 načíst __Spark-strukturovaná-streamování-z – Kafka.ipynb__ poznámkového bloku.

## <a name="load-tweets-into-kafka"></a>Načtení tweetů do Kafka

Jakmile soubory byly odeslány, vyberte __datového proudu-Tweetů-To_Kafka.ipynb__ záznam, tím otevřete Poznámkový blok. Postupujte podle kroků v poznámkovém bloku a načte tweetů do Kafka.

## <a name="process-tweets-using-spark-structured-streaming"></a>Proces tweetů pomocí Spark strukturovaných streamování

Na domovské stránce poznámkového bloku Jupyter, vyberte __Spark-strukturovaná-streamování-z – Kafka.ipynb__ položku. Postupujte podle kroků v poznámkovém bloku se načíst tweetů z Kafka pomocí Spark strukturovaných streamování.

## <a name="next-steps"></a>Další postup

Teď, když jste se naučili použití Spark strukturovaných streamování, najdete v následujících dokumentech Další informace o práci s Spark a Kafka:

* [Jak používat Spark streamování (DStream) s Kafka](hdinsight-apache-spark-with-kafka.md).
* [Začněte s Poznámkový blok Jupyter a Spark v HDInsight](spark/apache-spark-jupyter-spark-sql.md)