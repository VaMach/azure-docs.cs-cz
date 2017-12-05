---
title: "Použití MapReduce a Curl s Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Naučte se vzdáleně spouštět úlohy MapReduce s Hadoop v HDInsight pomocí Curl."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: bc6daf37-fcdc-467a-a8a8-6fb2f0f773d1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/04/2017
ms.author: larryfr
ms.openlocfilehash: dd3e5904ee21ee74da5adaa65abd7865a82c8b36
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a>Spuštění úloh MapReduce s Hadoop v HDInsight pomocí REST

Naučte se používat rozhraní REST API WebHCat ke spuštění úloh MapReduce systému Hadoop v clusteru HDInsight. Curl se používá k ukazují, jak můžete pracovat s HDInsight pomocí nezpracovaná požadavků HTTP ke spuštění úloh MapReduce.

> [!NOTE]
> Pokud jste již obeznámeni s pomocí serverů se systémem Linux Hadoop, ale jsou pro vás nové do HDInsight, najdete v článku [co potřebujete vědět o systémem Linux Hadoop v HDInsight](../hdinsight-hadoop-linux-information.md) dokumentu.


## <a id="prereq"></a>Požadavky

* Na clusteru HDInsight Hadoop
* [Curl](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Spuštění úloh MapReduce pomocí Curl

> [!NOTE]
> Pokud používáte Curl nebo jinou komunikaci REST s WebHCat, je třeba ověřit žádosti zadáním uživatelského jména správce clusteru HDInsight a hesla. Název clusteru musí používat jako součást identifikátoru URI, který se používá k odesílání požadavků na server.
>
> Pro příkazy v této části nahraďte **správce** uživatelem pro ověření clusteru. Nahraďte **CLUSTERNAME** názvem vašeho clusteru. Po zobrazení výzvy zadejte heslo pro uživatelský účet.
>
> Rozhraní API REST je zabezpečená pomocí [ověřování přístupu k základní](http://en.wikipedia.org/wiki/Basic_access_authentication). Vždy doporučujeme provádět požadavky pomocí protokolu HTTPS k zajištění, že vaše přihlašovací údaje jsou bezpečně odeslat na server.


1. Z příkazového řádku použijte následující příkaz k ověření, zda se můžete připojit ke clusteru HDInsight:

    ```bash
    curl -u admin -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Můžete zobrazit odpověď podobná následujícím kódu JSON:

        {"status":"ok","version":"v1"}

    Parametry použité v tomto příkazu jsou následující:

   * **-u**: Určuje uživatelské jméno a heslo použité k ověření žádosti
   * **-G**: označuje, že tato operace je požadavek GET.

   Na začátek identifikátor URI **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, je stejný pro všechny požadavky.

2. Chcete-li odeslat úlohu MapReduce, použijte následující příkaz:

    ```bash
    curl -u admin -d user.name=admin -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar
    ```

    Konec identifikátor URI (/ mapreduce/jar) informuje WebHCat, že tento požadavek spustí úlohu MapReduce z třídy v souboru jar. Parametry použité v tomto příkazu jsou následující:

   * **-d**: `-G` se nepoužívá, takže požadavek výchozí metodu POST. `-d`Určuje datových hodnot, které se odesílají s požadavkem.
    * **User.Name**: uživatel, který spouští příkaz
    * **JAR**: umístění na soubor jar, který obsahuje třídu být spustili
    * **Třída**: třídu, která obsahuje logiku MapReduce
    * **arg**: argumenty, které mají být předána do úlohy MapReduce. V tomto případě, vstupní textového souboru a adresáře, které se používají pro výstup

   Tento příkaz by měla vrátit ID úlohy, který slouží ke kontrole stavu úlohy:

       {"id":"job_1415651640909_0026"}

3. Pokud chcete zkontrolovat stav úlohy, použijte následující příkaz:

    ```bash
    curl -G -u admin -d user.name=admin https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Nahraďte **JOBID** se hodnota vrácená v předchozím kroku. Například, pokud byl návratovou hodnotu `{"id":"job_1415651640909_0026"}`, pak by byl JOBID `job_1415651640909_0026`.

    Pokud tato úloha skončí, vrácená stav `SUCCEEDED`.

   > [!NOTE]
   > Tento požadavek Curl vrátí dokumentu JSON s informacemi o úloze. Jq se používá k načtení jenom hodnotu stavu.

4. Pokud se změnila stav úlohy na `SUCCEEDED`, můžete načíst výsledky úlohy z Azure Blob storage. `statusdir` Parametr, který je předán s dotaz obsahuje umístění výstupního souboru. V tomto příkladu je umístění `/example/curl`. Tuto adresu ukládá výstup úlohy do úložiště výchozí clustery na `/example/curl`.

Můžete zobrazit seznam a stáhnout tyto soubory pomocí [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Další informace o práci s objekty BLOB z příkazového řádku Azure najdete v tématu [použití Azure CLI 2.0 s Azure Storage](../../storage/common/storage-azure-cli.md#create-and-manage-blobs) dokumentu.

## <a id="nextsteps"></a>Další kroky

Obecné informace o úloh MapReduce v HDInsight:

* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)

Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)
* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)

Další informace o rozhraní REST, který se používá v tomto článku najdete v tématu [WebHCat odkaz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).