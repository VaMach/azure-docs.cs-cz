---
title: "Spustit úlohu Hadoop pomocí Azure Cosmos DB a HDInsight | Microsoft Docs"
description: "Naučte se spouštíte jednoduchou úlohu Hive, Pig a MapReduce s Azure Cosmos DB a Azure HDInsight."
services: cosmos-db
author: dennyglee
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 06f0ea9d-07cb-4593-a9c5-ab912b62ac42
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 01/19/2018
ms.author: denlee
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 181954e4657166db8aa94021ad093437d8c7abfd
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2018
---
# <a name="Azure Cosmos DB-HDInsight"></a>Spustit úlohu Apache Hive, Pig nebo Hadoop pomocí Azure Cosmos DB a HDInsight
V tomto kurzu se dozvíte, jak spustit [Apache Hive][apache-hive], [Apache Pig][apache-pig], a [Apache Hadoop] [ apache-hadoop] úloh MapReduce v Azure HDInsight s konektorem Hadoop Cosmos DB. Cosmos DB Hadoop konektor umožňuje Cosmos DB tak, aby fungoval jako zdroj a jímka pro úlohy Hive, Pig a MapReduce. V tomto kurzu použije Cosmos DB jako zdroj dat i v cílovém pro úlohy Hadoop.

> [!IMPORTANT] 
> Spark pro konektor Azure Cosmos DB možnost se doporučuje pro připojení Azure HDInsight k Azure Cosmos DB. Další informace najdete v tématu [urychlit analýzy velkých objemů dat v reálném čase s Spark pro konektor Azure Cosmos DB](spark-connector.md).

Po dokončení tohoto kurzu, budete moct odpovězte si na následující otázky:

* Jak načíst data z databáze Cosmos pomocí úlohy Hive, Pig nebo MapReduce?
* Jak ukládat data do databáze Cosmos. pomocí úlohy Hive, Pig nebo MapReduce?

Doporučujeme začít následujícím videem, kde spustíme prostřednictvím úlohy Hive pomocí Cosmos databáze a HDInsight.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Use-Azure-DocumentDB-Hadoop-Connector-with-Azure-HDInsight/player]
>
>

Pak se vraťte k tomuto článku, kterou budete dostávat všechny podrobnosti na jak spouštět úlohy analytics na vaše data Cosmos DB.

> [!TIP]
> Tento kurz předpokládá, že máte zkušenosti s jazykem Apache Hadoop, Hive a Pig. Pokud začínáte Apache Hadoop, Hive a Pig, doporučujeme, abyste navštívíte [dokumentaci Apache Hadoop][apache-hadoop-doc]. V tomto kurzu také předpokládá, že máte zkušenosti s Cosmos DB a máte účet Cosmos DB. Pokud jste novým uživatelem Cosmos DB nebo nemáte účet Cosmos DB, podrobnosti naleznete v našem [Začínáme] [ getting-started] stránky.
>
>

Nemáte čas k dokončení tohoto kurzu a chcete jen získat celé ukázkové skripty prostředí PowerShell pro Hive, Pig a MapReduce? Nejedná se o problém, je získání [sem][hdinsight-samples]. Stahování také obsahuje soubory hql, pig a java pro tyto ukázky.

## <a name="NewestVersion"></a>Nejnovější verze
<table border='1'>
    <tr><th>Verze konektoru Hadoop</th>
        <td>1.2.0</td></tr>
    <tr><th>Identifikátor Uri skriptu</th>
        <td>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</td></tr>
    <tr><th>Datum změny</th>
        <td>04/26/2016</td></tr>
    <tr><th>Podporované HDInsight verze</th>
        <td>3.1, 3.2</td></tr>
    <tr><th>Protokol změn</th>
        <td>Aktualizovat Azure Cosmos DB Java SDK pro 1.6.0</br>
            Přidaná podpora pro dělené kolekce jako zdroj a jímka</br>
        </td></tr>
</table>

## <a name="Prerequisites"></a>Požadavky
Než budete postupovat podle pokynů v tomto kurzu, ujistěte se, že máte následující:

* Účet Cosmos DB, databázi a kolekci s dokumenty uvnitř. Další informace najdete v tématu [Začínáme s Cosmos DB][getting-started]. Import ukázkových dat do účtu Cosmos DB s [nástroj pro import Cosmos DB][import-data].
* Propustnost. Čtení a zápisy z prostředí HDInsight započítají vůči vaší jednotek přiděleného žádosti pro kolekce.
* Kapacitu pro další uloženou proceduru v každém výstupní kolekce. Uložené procedury používají pro přenos výsledné dokumenty.
* Kapacity pro výsledný dokumenty z úloh Hive, Pig nebo MapReduce.
* [*Volitelné*] kapacity pro další kolekci.

> [!WARNING]
> Aby se zabránilo vytvoření nové kolekce během některé z úloh, můžete buď vytisknout výsledky do stdout, uložte si výstup do vašeho kontejneru WASB nebo zadejte již existující kolekci. V případě zadání existující kolekci, se vytvoří nové dokumenty v kolekci a stávající dokumenty bude mít vliv pouze pokud dojde ke konfliktu v *ID*. **Konektor s docházet ke konfliktům id automaticky přepíše stávající dokumenty**. Tuto funkci můžete vypnout pomocí nastavení upsert možnost na hodnotu false. Pokud je hodnota false upsert a dojde ke konfliktu, se nezdaří úlohy Hadoop; vytváření sestav chybu id konflikt.
>
>

## <a name="ProvisionHDInsight"></a>Krok 1: Vytvoření nového clusteru HDInsight
Tento kurz používá akce skriptu z portálu Azure k přizpůsobení clusteru HDInsight. V tomto kurzu budeme používat portál Azure k vytvoření clusteru HDInsight. Pokyny o tom, jak pomocí rutin prostředí PowerShell nebo sady SDK rozhraní .NET HDInsight, podívejte se [HDInsight přizpůsobit clustery pomocí akce skriptu] [ hdinsight-custom-provision] článku.

1. Přihlaste se k [portál Azure][azure-portal].
2. Klikněte na tlačítko **+ nový** horní levé navigaci, vyhledejte **HDInsight** v horním panelu vyhledávání v novém okně.
3. **HDInsight** publikováno **Microsoft** se zobrazí v horní části výsledky. Klikněte na něj a potom na **vytvořit**.
4. V novém clusteru HDInsight vytvořit okno, zadejte vaše **název clusteru** a vyberte **předplatné** chcete zřídit tento prostředek.

    <table border='1'>
        <tr><td>Název clusteru</td><td>Název clusteru.<br/>
Název serveru DNS musí spustit a končit znakem alpha číselné a může obsahovat pomlčky.<br/>
Pole musí být řetězec o délce 3 až 63 znaků.</td></tr>
        <tr><td>Název předplatného</td>
            <td>Pokud máte více než jedno předplatné Azure, vyberte odběr, který bude hostitelem clusteru HDInsight. </td></tr>
    </table>
5.Klikněte na tlačítko **vybrat typ clusteru** a nastavte následující vlastnosti do zadaných hodnot.

    <table border='1'>
        <tr><td>Typ clusteru</td><td><strong>Hadoop</strong></td></tr>
        <tr><td>Vrstvy clusteru</td><td><strong>Standard</strong></td></tr>
        <tr><td>Operační systém</td><td><strong>Windows</strong></td></tr>
        <tr><td>Verze</td><td>nejnovější verzi</td></tr>
    </table>

    Nyní, klikněte na tlačítko **vyberte**.

    ![Zadejte podrobnosti o počáteční clusteru Hadoop HDInsight][image-customprovision-page1]
6. Klikněte na **pověření** nastavit přihlašovací jméno a přihlašovací údaje vzdálený přístup. Zvolte vaše **uživatelské jméno přihlášení clusteru** a **clusteru heslo pro přihlášení**.

    Pokud chcete vzdálené do clusteru, vyberte *Ano* v dolní části okna a zadejte uživatelské jméno a heslo.
7. Klikněte na **zdroj dat** nastavení vaší primární umístění pro přístup k datům. Vyberte **metodu výběru** a zadejte již existující účet úložiště nebo vytvořte novou.
8. V okně stejné zadat **výchozí kontejner** a **umístění**. A klikněte na tlačítko **vyberte**.

   > [!NOTE]
   > Vyberte umístění blízko oblast účtu Cosmos DB pro lepší výkon
   >
   >
9. Klikněte na **cenová** vyberte počet a typ uzlů. Můžete ponechat výchozí konfigurace a později na škálovat počet uzlů pracovního procesu.
10. Klikněte na tlačítko **volitelné konfiguraci**, pak **skript akce** v okně volitelné konfiguraci.

     V akcí skriptů zadejte následující informace, chcete-li přizpůsobit clusteru HDInsight.

     <table border='1'>
         <tr><th>Vlastnost</th><th>Hodnota</th></tr>
         <tr><td>Název</td>
             <td>Zadejte název akce skriptu.</td></tr>
         <tr><td>Identifikátor URI skriptu</td>
             <td>Zadejte identifikátor URI skriptu, která je volána, chcete-li přizpůsobit clusteru.</br></br>
Zadejte: </br> <strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</strong>.</td></tr>
         <tr><td>Hlavní uzel</td>
             <td>Klikněte na zaškrtávací políčko pro spuštění skriptu prostředí PowerShell na hlavní uzel.</br></br>
             <strong>Zaškrtněte toto políčko</strong>.</td></tr>
         <tr><td>Worker</td>
             <td>Klikněte na zaškrtávací políčko pro spuštění skriptu prostředí PowerShell do pracovního uzlu.</br></br>
             <strong>Zaškrtněte toto políčko</strong>.</td></tr>
         <tr><td>Zookeeper</td>
             <td>Klikněte na zaškrtávací políčko pro spuštění skriptu prostředí PowerShell na Zookeeper.</br></br>
             <strong>Není potřeba</strong>.
             </td></tr>
         <tr><td>Parametry</td>
             <td>Zadejte parametry, pokud se vyžadují skriptem.</br></br>
             <strong>Žádné parametry potřeby</strong>.</td></tr>
     </table>
11.Buď vytvořit novou **skupiny prostředků** nebo použít existující skupinu prostředků v rámci svého předplatného Azure.
12. Teď, zkontrolujte **připnout na řídicí panel** sledování jeho nasazení a klikněte na tlačítko **vytvořit**!

## <a name="InstallCmdlets"></a>Krok 2: Instalace a konfigurace prostředí Azure PowerShell
1. Nainstalujte Azure PowerShell. Pokyny naleznete [sem][powershell-install-configure].

   > [!NOTE]
   > Jenom pro dotazy Hive, případně můžete použít HDInsight je online Editor Hive. Uděláte to tak, přihlaste se k [portálu Azure][azure-portal], klikněte na tlačítko **HDInsight** v levém podokně zobrazíte seznam clusterů HDInsight. Klikněte na cluster, který chcete spouštět dotazy Hive a pak klikněte na **dotazu konzoly**.
   >
   >
2. Otevřete Integrované skriptovací prostředí Azure PowerShell:

   * V počítači se systémem Windows 8 nebo Windows Server 2012 nebo vyšší můžete použít integrované hledání. Na obrazovce Start zadejte **prostředí powershell ise** a klikněte na tlačítko **Enter**.
   * V počítači se systémem starším než Windows 8 nebo Windows Server 2012 pomocí nabídky Start. Z nabídky Start, zadejte **příkazového řádku** do vyhledávacího pole a pak v seznamu výsledků, klikněte na tlačítko **příkazového řádku**. V příkazovém řádku zadejte **powershell_ise** a klikněte na tlačítko **Enter**.
3. Přidání účtu Azure.

   1. V podokně konzoly zadejte **Add-AzureAccount** a klikněte na tlačítko **Enter**.
   2. Zadejte e-mailová adresa spojená s předplatným Azure a klikněte na tlačítko **pokračovat**.
   3. Zadejte heslo pro vaše předplatné Azure.
   4. Klikněte na tlačítko **přihlášení**.
4. Následující diagram identifikuje důležité části prostředí Azure PowerShell skriptování.

    ![Diagram pro prostředí Azure PowerShell][azure-powershell-diagram]

## <a name="RunHive"></a>Krok 3: Spuštění úlohy Hive pomocí Azure Cosmos DB a HDInsight
> [!IMPORTANT]
> Všechny proměnné indikován < > musí být zadána pomocí nastavení konfigurace.
>
>

1. V podokně pro skript prostředí PowerShell nastavte následující proměnné.

        # Provide Azure subscription name, the Azure Storage account and container that is used for the default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide the HDInsight cluster name where you want to run the Hive job.
        $clusterName = "<HDInsightClusterName>"
2. <p>Začněme, vytváření řetězec vašeho dotazu. Jsme budete napsat dotaz Hive, které trvá vygenerované systémem časová razítka (_ts) a jedinečné ID (_rid) z Azure Cosmos DB kolekce všechny dokumenty, zaznamená všechny dokumenty podle počtu minut a pak uloží výsledky zpět do nové kolekce Azure Cosmos DB.</p>

    <p>Nejdříve vytvoříme tabulku Hive z našich kolekce Azure Cosmos DB. Přidejte následující fragment kódu do podokna skript prostředí PowerShell <strong>po</strong> fragmentu kódu od #1. Ujistěte se, zahrnete parametr dotazu volitelné oříznout dokumenty, které se právě _ts a _rid.</p>

   > [!NOTE]
   > **Pojmenování DocumentDB.inputCollections se jedná o chybu.** Ano, jsme povolit přidávání více kolekcí jako vstup: </br>
   >
   >

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> The collection names are separated without spaces, using only a single comma.

        # Create a Hive table using data from Azure Cosmos DB. Pass Azure Cosmos DB the query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

1. V dalším kroku vytvoříme pro kolekci výstupní tabulku Hive. Vlastnosti dokumentu výstup bude měsíc, den, hodinu, minutu a celkový počet výskytů.

   > [!NOTE]
   > **Ještě znovu pojmenování DocumentDB.outputCollections se jedná o chybu.** Ano, jsme povolit přidávání více kolekcí jako výstup: </br>
   > '*DocumentDB.outputCollections*'='*\<název kolekce DocumentDB výstupu 1\>*,*\<název kolekce DocumentDB výstupu 2\>*. </br> Názvy kolekce jsou oddělené bez mezer pouze jeden čárkou. </br></br>
   > Dokumenty bude distribuované kruhového dotazování napříč více kolekcí. Batch dokumentů se budou ukládat do jedné kolekce a potom druhý dávky dokumenty se uloží v další kolekce a tak dále.
   >
   >

       # Create a Hive table for the output data to Azure Cosmos DB.
       $queryStringPart2 = "drop table DocumentDB_analytics; " +
                             "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                             "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                             "tblproperties ( " +
                                 "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                 "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                 "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                 "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "
2. Nakonec umožňuje tally dokumenty měsíc, den, hodinu a minutu a vložit výsledky zpět do výstupní tabulku Hive.

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "
3. Přidejte následující fragment skriptu k vytvoření definice úlohy Hive z předchozího dotazu.

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    Můžete také použít-souboru přepínač tak, aby určete soubor skriptu HiveQL na HDFS.
4. Přidejte následující fragment uložte čas spuštění a odeslání úlohy Hive.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition
5. Přidejte následující příkaz a počkejte na dokončení úlohy Hive.

        # Wait for the Hive job to complete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600
6. Přidejte následující tisknout standardní výstupní zařízení a počáteční a koncový čas.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
7. **Spustit** nový skript! **Klikněte na tlačítko** tlačítko zelená execute.
8. Zkontrolujte výsledky. Přihlaste se k [portál Azure][azure-portal].

   1. Klikněte na tlačítko <strong>Procházet</strong> na levé straně panelu. </br>
   2. Klikněte na tlačítko <strong>všechno, co</strong> v pravém horním panelu Procházet. </br>
   3. Najít a klikněte na tlačítko <strong>Azure Cosmos DB účty</strong>. </br>
   4. Poté vyhledejte vaše <strong>Azure Cosmos DB účet</strong>, pak <strong>Azure Cosmos DB databáze</strong> a <strong>Azure Cosmos DB kolekce</strong> přidružené ke kolekci výstupu, zadané v dotazu Hive.</br>
   5. Nakonec klikněte na <strong>Průzkumníka dokumentů</strong> pod <strong>Developer Tools</strong>.</br></p>

   Zobrazí se výsledky dotazu Hive.

   ![Výsledky dotazu Hive][image-hive-query-results]

## <a name="RunPig"></a>Krok 4: Spuštění úlohy Pig pomocí Cosmos databáze a HDInsight
> [!IMPORTANT]
> Všechny proměnné indikován < > musí být zadána pomocí nastavení konfigurace.
>
>

1. V podokně pro skript prostředí PowerShell nastavte následující proměnné.

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want to run the Pig job.
        $clusterName = "Azure HDInsight Cluster Name"
2. <p>Začněme, vytváření řetězec vašeho dotazu. Jsme budete napsat dotaz Pig, které trvá vygenerované systémem časová razítka (_ts) a jedinečné ID (_rid) z Azure Cosmos DB kolekce všechny dokumenty, zaznamená všechny dokumenty podle počtu minut a pak uloží výsledky zpět do nové kolekce Azure Cosmos DB.</p>
    <p>Nejdřív načíst dokumenty z databáze Cosmos do HDInsight. Přidejte následující fragment kódu do podokna skript prostředí PowerShell <strong>po</strong> fragmentu kódu od #1. Nezapomeňte přidat dotaz na volitelný parametr dotazu DocumentDB oříznout naše dokumenty, které se právě _ts a _rid.</p>

   > [!NOTE]
   > Ano, jsme povolit přidávání více kolekcí jako vstup: </br>
   > '*\<Název kolekce DocumentDB vstup 1\>*,*\<název kolekce DocumentDB vstup 2\>*.</br> Názvy kolekce jsou oddělené bez mezer pouze jeden čárkou. </b>
   >
   >

    Dokumenty bude distribuované kruhového dotazování napříč více kolekcí. Batch dokumentů se budou ukládat do jedné kolekce a potom druhý dávky dokumenty se uloží v další kolekce a tak dále.

        # Load data from Cosmos DB. Pass the Azure Cosmos DB query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "
3. Dále umožňuje tally podle měsíc, den, hodinu, minutu a celkový počet výskytů dokumenty.

       # GROUP BY minute and COUNT entries for each.
       $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                           "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                           "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "
4. Umožňuje uložit nakonec výsledky do naší nové kolekce výstup.

   > [!NOTE]
   > Ano, jsme povolit přidávání více kolekcí jako výstup: </br>
   > '\<Název kolekce DocumentDB výstupu 1\>,\<název kolekce DocumentDB výstupu 2\>.</br> Názvy kolekce jsou oddělené bez mezer pouze jeden čárkou.</br>
   > Dokumenty bude distribuované kruhového dotazování napříč více kolekcí. Batch dokumentů se budou ukládat do jedné kolekce a potom druhý dávky dokumenty se uloží v další kolekce a tak dále.
   >
   >

        # Store output data to Cosmos DB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "
5. Přidejte následující fragment skriptu k vytvoření definice úlohy Pig z předchozího dotazu.

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    Můžete také použít-souboru přepínač tak, aby určete soubor skriptu Pig na HDFS.
6. Přidejte následující fragment uložte čas spuštění a odeslat úlohu Pig.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition
7. Přidejte následující příkaz a počkejte na dokončení úlohy Pig.

        # Wait for the Pig job to complete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600
8. Přidejte následující tisknout standardní výstupní zařízení a počáteční a koncový čas.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
9. **Spustit** nový skript! **Klikněte na tlačítko** tlačítko zelená execute.
10. Zkontrolujte výsledky. Přihlaste se k [portál Azure][azure-portal].

    1. Klikněte na tlačítko <strong>Procházet</strong> na levé straně panelu. </br>
    2. Klikněte na tlačítko <strong>všechno, co</strong> v pravém horním panelu Procházet. </br>
    3. Najít a klikněte na tlačítko <strong>Azure Cosmos DB účty</strong>. </br>
    4. Poté vyhledejte vaše <strong>Azure Cosmos DB účet</strong>, pak <strong>Azure Cosmos DB databáze</strong> a <strong>Azure Cosmos DB kolekce</strong> přidružené ke kolekci výstupu, zadané v dotazu Pig.</br>
    5. Nakonec klikněte na <strong>Průzkumníka dokumentů</strong> pod <strong>Developer Tools</strong>.</br></p>

    Zobrazí se výsledky dotazu Pig.

    ![Výsledky dotazu pig][image-pig-query-results]

## <a name="RunMapReduce"></a>Krok 5: Spustit úlohu MapReduce pomocí Azure Cosmos DB a HDInsight
1. V podokně pro skript prostředí PowerShell nastavte následující proměnné.

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name
2. Jsme budete spustit úlohu MapReduce, která sečte počet výskytů pro každou vlastnost dokumentu z kolekce Azure Cosmos DB. Přidejte tento fragment skriptu **po** výše uvedeném fragmentu.

        # Define the MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

   > [!NOTE]
   > TallyProperties v01.jar se dodává s vlastní instalace služby konektoru Hadoop DB Cosmos.
   >
   >
3. Přidáním následujícího příkazu se odeslat úlohu MapReduce.

        # Save the start time and submit the job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    Kromě definici úlohy MapReduce je také zadat název clusteru HDInsight, kde chcete spustit úlohu MapReduce a přihlašovací údaje. Start-AzureHDInsightJob je synchronního volání. Chcete-li zkontrolovat na dokončení úlohy, použijte *čekání AzureHDInsightJob* rutiny.
4. Přidejte následující příkaz a zkontrolujte všechny chyby se spustit úlohu MapReduce.

        # Get the job output and print the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
5. **Spustit** nový skript! **Klikněte na tlačítko** tlačítko zelená execute.
6. Zkontrolujte výsledky. Přihlaste se k [portál Azure][azure-portal].

   1. Klikněte na tlačítko <strong>Procházet</strong> na levé straně panelu.
   2. Klikněte na tlačítko <strong>všechno, co</strong> v pravém horním panelu Procházet.
   3. Najít a klikněte na tlačítko <strong>Azure Cosmos DB účty</strong>.
   4. Poté vyhledejte vaše <strong>Azure Cosmos DB účet</strong>, pak <strong>Azure Cosmos DB databáze</strong> a <strong>Azure Cosmos DB kolekce</strong> přidružené ke kolekci výstupu, zadané v úlohu MapReduce.
   5. Nakonec klikněte na <strong>Průzkumníka dokumentů</strong> pod <strong>Developer Tools</strong>.

      Zobrazí se výsledky úlohy MapReduce.

      ![MapReduce výsledky dotazu][image-mapreduce-query-results]

## <a name="NextSteps"></a>Další kroky
Blahopřejeme! Právě jste spustili vaše první úlohy Hive, Pig a MapReduce pomocí Azure Cosmos DB a HDInsight.

Máme open source naše konektor Hadoop. Pokud byste chtěli, můžete přispívat na [Githubu][github].

Další informace naleznete v následujících článcích:

* [Vývoj aplikace Java pomocí Azure Cosmos DB][sql-api-java-application]
* [Vývoj aplikací Java MapReduce pro Hadoop v HDInsight][hdinsight-develop-deploy-java-mapreduce]
* [Začínáme používat Hadoop s Hive v HDInsight k analýze používání mobilního telefonu][hdinsight-get-started]
* [Používání nástroje MapReduce s HDInsight][hdinsight-use-mapreduce]
* [Použití Hivu se službou HDInsight][hdinsight-use-hive]
* [Použití Pigu se službou HDInsight][hdinsight-use-pig]
* [Přizpůsobení clusterů HDInsight pomocí akce skriptu][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: sql-api-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[github]: https://github.com/Azure/azure-documentdb-hadoop
[sql-api-java-application]: sql-api-java-application.md
[import-data]: import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md
[hdinsight-develop-deploy-java-mapreduce]:../hdinsight/hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]:../hdinsight/hadoop/hdinsight-use-hive.md
[hdinsight-use-mapreduce]:../hdinsight/hadoop/hdinsight-use-mapreduce.md
[hdinsight-use-pig]:../hdinsight/hadoop/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0
