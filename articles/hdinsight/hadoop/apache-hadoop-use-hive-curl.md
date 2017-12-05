---
title: "Používání Hadoop Hive s Curl v HDInsight - Azure | Microsoft Docs"
description: "Naučte se vzdáleně odesílání úloh Pig do HDInsight pomocí Curl."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6ce18163-63b5-4df6-9bb6-8fcbd4db05d8
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/04/2017
ms.author: larryfr
ms.openlocfilehash: 07386c2fbb5fe4ce4fe1ca6844f6308cf59cbfb4
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="run-hive-queries-with-hadoop-in-hdinsight-using-rest"></a>Spouštění dotazů Hive se systémem Hadoop v HDInsight pomocí REST

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Naučte se používat rozhraní REST API WebHCat ke spouštění dotazů Hive se systémem Hadoop v clusteru Azure HDInsight.

[Curl](http://curl.haxx.se/) se používá k ukazují, jak můžete pracovat s HDInsight pomocí nezpracované požadavky HTTP. [Jq](http://stedolan.github.io/jq/) nástroj se používá ke zpracování dat JSON vrácená z požadavky REST.

> [!NOTE]
> Pokud jste obeznámeni s pomocí serverů se systémem Linux Hadoop, ale jsou nové do HDInsight, najdete v článku [co potřebujete vědět o Hadoop v HDInsight se systémem Linux](../hdinsight-hadoop-linux-information.md) dokumentu.

## <a id="curl"></a>Spouštění dotazů Hive

> [!NOTE]
> Pokud používáte cURL nebo jinou komunikaci REST s WebHCat, je třeba ověřit žádosti zadáním uživatelského jména a hesla pro správce clusteru HDInsight.
>
> Pro příkazy v této části nahraďte **správce** uživatelem pro ověření clusteru. Nahraďte **CLUSTERNAME** názvem vašeho clusteru. Po zobrazení výzvy zadejte heslo pro uživatelský účet.
>
> Rozhraní API REST je zabezpečeno pomocí [základního ověřování](http://en.wikipedia.org/wiki/Basic_access_authentication). K zajištění, že vaše přihlašovací údaje jsou bezpečně odeslat na server, vždy proveďte požadavky pomocí Secure HTTP (HTTPS).

1. Z příkazového řádku použijte následující příkaz k ověření, zda se můžete připojit ke clusteru HDInsight:

    ```bash
    curl -u admin -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Můžete zobrazit odpověď podobná následující text:

        {"status":"ok","version":"v1"}

    Parametry použité v tomto příkazu jsou následující:

    * **-u** – uživatelské jméno a heslo použité pro ověření žádosti.
    * **-G** – označuje, že tento požadavek je operaci GET.

   Na začátek adresu URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, je stejný pro všechny požadavky. Cesta **/status**, označuje, že požadavek je vrátit stav WebHCat (také známé jako Templeton) pro server. Můžete také vyžadovat verzi Hive pomocí následujícího příkazu:

    ```bash
    curl -u admin -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive
    ```

    Tento požadavek vrátí odpověď podobná následující text:

        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}

2. Následující informace vám pomůžou vytvořit tabulku s názvem **log4jLogs**:

    ```bash
    curl -u admin -d user.name=admin -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    Použité s touto žádostí následující parametry:

   * **-d** – od `-G` se nepoužívá, použije se výchozí hodnota žádost na metodu POST. `-d`Určuje datových hodnot, které se odesílají s požadavkem.

     * **User.Name** -uživatel, který spouští příkaz.
     * **spuštění** -příkazy HiveQL k provedení.
     * **statusdir** -adresáře, který stav pro tuto úlohu je zapsán do.

   Tyto příkazy provádět následující akce:
   
   * **VYŘAĎTE tabulku** – Pokud tabulka již existuje, je odstraněn.
   * **Vytvoření externí tabulky** -vytvoří novou tabulku "externí" v Hive. Externí tabulky uložit pouze definici tabulky Hive. Data je ponechán v původním umístění.

     > [!NOTE]
     > Externí tabulky by měl být použit při očekáváte, že v základních datech aktualizovat externího zdroje. Například automatizované data nahrát procesu nebo jiná operace MapReduce.
     >
     > Vyřazení externí tabulku nemá **není** odstranit data, jenom definici tabulky.

   * **Řádek formátu** – jak data formátována. Pole v každém protokolu jsou oddělené mezerou.
   * **ULOŽENÉ umístění textový soubor AS** – se uloží data (například/datový adresář) a která je uložena jako text.
   * **Vyberte** -počet všech řádků vybere kde sloupec **t4** obsahuje hodnotu **[Chyba]**. Tento příkaz vrátí hodnotu **3** jsou tři řádky, které obsahují tuto hodnotu.

     > [!NOTE]
     > Všimněte si, že jsou nahrazovány mezery mezi příkazy HiveQL `+` znak při použití s Curl. Hodnoty v uvozovkách, které obsahují mezery, jako je například oddělovač, by neměl být nahrazen `+`.

   * **INPUT__FILE__NAME jako '% 25.log'** – tento příkaz omezí výsledky vyhledávání na používat jenom soubory, které končí na. log.

     > [!NOTE]
     > `%25` Je kódovaná adresou URL formuláře %, je skutečný stav `like '%.log'`. % Musí být adresa URL kódování, jako je považován za zvláštní znak v adresách URL.

   Tento příkaz vrátí ID úlohy, který slouží ke kontrole stavu úlohy.

       {"id":"job_1415651640909_0026"}

3. Pokud chcete zkontrolovat stav úlohy, použijte následující příkaz:

    ```bash
    curl -G -u admin -d user.name=admin https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Nahraďte **JOBID** se hodnota vrácená v předchozím kroku. Například, pokud byl návratovou hodnotu `{"id":"job_1415651640909_0026"}`, pak **JOBID** by `job_1415651640909_0026`.

    Pokud se úloha dokončí, je stav **úspěšné**.

   > [!NOTE]
   > Tento požadavek Curl vrátí dokument JavaScript Object Notation (JSON) s informace o úloze. Jq se používá k načtení jenom hodnotu stavu.

4. Jakmile se stav úlohy se změnila na **úspěšné**, můžete načíst výsledky úlohy z Azure Blob storage. `statusdir` Parametr předaný s dotaz obsahuje umístění výstupního souboru; v tomto případě **/příklad/curl**. Tato adresa ukládá výstup v **příklad/curl** adresáře v úložišti výchozí clustery.

    Můžete zobrazit seznam a stáhnout tyto soubory pomocí [rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli). Další informace o používání rozhraní příkazového řádku Azure s Azure Storage najdete v tématu [použití Azure CLI 2.0 s Azure Storage](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli#create-and-manage-blobs) dokumentu.

5. Použít následující příkazy k vytvoření nové "interní" tabulku s názvem **errorLogs**:

    ```bash
    curl -u admin -d user.name=admin -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    Tyto příkazy provádět následující akce:

   * **Vytvoření tabulka není v případě existuje** -vytvoří tabulku, pokud ještě neexistuje. Tento příkaz vytvoří interní tabulku, která je uložena v datovém skladu Hive. Tato tabulka spravuje Hive.

     > [!NOTE]
     > Na rozdíl od externích tabulek se odstraní vyřazení interní tabulku v základních datech.

   * **ULOŽENÉ ORC AS** – ukládá data ve formátu optimalizované řádek sloupcovém (ORC). ORC je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.
   * **PŘEPSAT INSERT... Vyberte** -vybere řádky z **log4jLogs** tabulku, která obsahují **[Chyba]**, pak vkládá data do **errorLogs** tabulky.
   * **Vyberte** -vybere všechny řádky z nové **errorLogs** tabulky.

6. Použijte úlohu s ID vrátil a zkontrolujte stav úlohy. Jakmile ho proběhla úspěšně, použijte rozhraní příkazového řádku Azure jak je popsáno dříve ke stažení a zobrazit výsledky. Výstup by měl obsahovat tři řádky, které obsahují **[Chyba]**.

## <a id="nextsteps"></a>Další kroky

Obecné informace o Hive s HDInsight:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)

Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:

* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)
* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)

Pokud používáte s Hive Tez, najdete v následujících dokumentech pro ladění informace:

* [Použití zobrazení Ambari Tez na HDInsight se systémem Linux](../hdinsight-debug-ambari-tez-view.md)

Další informace o rozhraní API REST v tomto dokumentu najdete v tématu [WebHCat odkaz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) dokumentu.

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


