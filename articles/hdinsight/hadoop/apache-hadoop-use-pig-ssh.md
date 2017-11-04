---
title: "Použijte Hadoop Pig pomocí protokolu SSH v clusteru HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak připojit do clusteru se systémem Linux Hadoop s SSH a pak použijte příkaz Pig ke spuštění příkazů Pig Latin interaktivně, nebo jako dávkovou úlohu."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b646a93b-4c51-4ba4-84da-3275d9124ebe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/03/2017
ms.author: larryfr
ms.openlocfilehash: be18f6db46285233e233c843dab1f389cd553e96
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Spuštění úlohy Pig na cluster se systémem Linux pomocí příkazu Pig (SSH)

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Zjistěte, jak interaktivně spouštět úlohy Pig ze připojení SSH ke svému clusteru HDInsight. Programovací jazyk Pig Latin můžete k popisu transformace, které se použijí u vstupních dat k vytvoření požadované výstup.

> [!IMPORTANT]
> Kroky v tomto dokumentu vyžadují cluster HDInsight se systémem Linux. HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="ssh"></a>Připojení pomocí protokolu SSH

Použití SSH se připojit ke svému clusteru HDInsight. Následující příklad se připojí ke clusteru s názvem **myhdinsight** jako účet s názvem **sshuser**:

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net

**Pokud jste zadali klíč certifikátu pro ověřování SSH** při vytváření clusteru HDInsight, možná muset zadat umístění privátní klíč klientského systému.

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Pokud jste zadali heslo pro ověřování SSH** při vytváření clusteru HDInsight, zadejte po zobrazení výzvy.

Další informace o používání SSH s HDInsight, naleznete v části [použití SSH s HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="pig"></a>Použijte příkaz Pig

1. Po připojení pomocí následujícího příkazu spusťte Pig rozhraní příkazového řádku (CLI):

        pig

    Po chvíli, měli byste vidět `grunt>` řádku.

2. Zadejte následující příkaz:

        LOGS = LOAD '/example/data/sample.log';

    Tento příkaz načte obsah souboru sample.log do PROTOKOLŮ. Pomocí následujícího příkazu můžete zobrazit obsah souboru:

        DUMP LOGS;

3. V dalším kroku transformaci dat s použitím regulárních výrazů k extrakci pouze úroveň protokolování z každý záznam pomocí následujícího příkazu:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    Můžete použít **DUMP** chcete zobrazit data po transformaci. V takovém případě použijte `DUMP LEVELS;`.

4. Pokračujte v použití transformací pomocí příkazy v následující tabulce:

    | Pig Latin – příkaz | Jaké jsou příkaz |
    | ---- | ---- |
    | `FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;` | Odebere řádky, které obsahují hodnotu null pro úroveň protokolu a ukládá výsledky do `FILTEREDLEVELS`. |
    | `GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;` | Skupiny řádků a úroveň protokolu a ukládá výsledky do `GROUPEDLEVELS`. |
    | `FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;` | Vytvoří sadu dat, která obsahuje každý jedinečný protokolu dojde k hodnota úrovně a jak často se. Datová sada ukládána do `FREQUENCIES`. |
    | `RESULT = order FREQUENCIES by COUNT desc;` | Řadí úrovní záznamu do protokolu podle počtu (sestupně) a ukládá do `RESULT`. |

    > [!TIP]
    > Použití `DUMP` zobrazíte výsledek transformace po dokončení každého kroku.

5. Můžete také uložit výsledky transformace pomocí `STORE` příkaz. Například následující příkaz uloží `RESULT` k `/example/data/pigout` adresář na výchozí úložiště pro cluster:

        STORE RESULT into '/example/data/pigout';

   > [!NOTE]
   > Jsou data uložena v adresáři zadané v souborech s názvem `part-nnnnn`. Pokud adresář již existuje, obdržíte chybu.

6. Chcete-li ukončit řádku grunt, zadejte následující příkaz:

        QUIT;

### <a name="pig-latin-batch-files"></a>Pig Latin dávkové soubory

Můžete také použít příkaz Pig ke spuštění Pig Latin obsaženého v načítaném souboru.

1. Po ukončení řádku grunt, použijte následující příkaz k kanálu STDIN do souboru s názvem `pigbatch.pig`. Tento soubor je vytvořen v domovském adresáři pro uživatelský účet SSH.

        cat > ~/pigbatch.pig

2. Zadejte nebo vložte následující řádky a potom pomocí kombinace kláves Ctrl + D po dokončení.

        LOGS = LOAD '/example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

3. Použijte následující příkaz ke spuštění `pigbatch.pig` souboru pomocí příkazu Pig.

        pig ~/pigbatch.pig

    Po dokončení úlohy batch, zobrazí se následující výstup:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <a id="nextsteps"></a>Další kroky

Obecné informace o Pig v HDInsight najdete v následujícím dokumentu:

* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)

Další informace o další způsoby, jak pracovat s Hadoop v HDInsight najdete v následujících dokumentech:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)
* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)
