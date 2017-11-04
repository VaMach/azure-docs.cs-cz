---
title: "Analýza dat snímačů pomocí Apache Storm a HBase | Microsoft Docs"
description: "Zjistěte, jak se připojit k Apache Storm s virtuální sítí. Používat Storm pro zpracování dat snímačů z centra událostí s HBase a vizualizovat s D3.js."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a9a1ac8e-5708-4833-b965-e453815e671f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/19/2017
ms.author: larryfr
ms.openlocfilehash: 87c2aece68c5de06d683abf971b6c7ccf7f67a54
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>Analýza dat snímačů s Apache Storm, centra událostí a HBase v HDInsight (Hadoop)

Další informace o použití Apache Storm v HDInsight ke zpracování dat snímačů z centra událostí Azure. Data jsou pak uloženy do Apache HBase v HDInsight a detekují pomocí D3.js.

Šablony Azure Resource Manager v tomto dokumentu ukazuje, jak vytvořit několik prostředků Azure ve skupině prostředků. Šablona vytvoří virtuální síť Azure, dva clustery HDInsight (Storm a HBase) a webové aplikace Azure. Implementace node.js řídicího panelu webu v reálném čase je automaticky nasadí do webové aplikace.

> [!NOTE]
> Informace v tomto dokumentu a v příkladu v tomto dokumentu vyžadují HDInsight verze 3.6.
>
> HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="architecture"></a>Architektura

![diagram architektury](./media/apache-storm-sensor-data-analysis/devicesarchitecture.png)

V tomto příkladu se skládá z následujících součástí:

* **Azure Event Hubs**: obsahuje data, která se shromažďují ze senzorů.
* **Storm v HDInsight**: poskytuje v reálném čase zpracování dat z centra událostí.
* **HBase v HDInsight**: po zpracování pomocí Storm poskytuje trvalé úložiště dat typu NoSQL pro data.
* **Služba Azure virtuální sítě**: umožňuje zabezpečené komunikaci mezi Storm v HDInsight a HBase v HDInsight clustery.
  
  > [!NOTE]
  > Virtuální sítě je potřeba při použití Java HBase klientského rozhraní API. Ho nebude vystavena, přes bránu veřejné pro clustery HBase. Instalace clustery HBase a Storm do stejné virtuální síti umožňuje Storm cluster (nebo všechny ostatní systémy ve virtuální síti) přímý přístup k HBase pomocí rozhraní API klienta.

* **Řídicí panel webu**: příklad řídicího panelu, který grafy data v reálném čase.
  
  * Web je implementované v Node.js.
  * [Socket.IO](http://socket.io/) se používá pro komunikaci v reálném čase mezi topologie Storm a webu.
    
    > [!NOTE]
    > Pro komunikaci pomocí Socket.io je podrobností implementace. Můžete použít libovolnou architekturu komunikace, jako je například nezpracovaná Websocket nebo SignalR.

  * [D3.js](http://d3js.org/) se používá k graf data, která se posílá webu.

> [!IMPORTANT]
> Dva clustery jsou povinné, protože neexistuje žádná podporovaná metoda vytvoření jednoho clusteru HDInsight Storm a HBase.

Topologie čte data z centra událostí pomocí `org.apache.storm.eventhubs.spout.EventHubSpout` třídy a zápisu dat do HBase pomocí `org.apache.storm.hbase.bolt.HBaseBolt` třídy. Komunikace s webem se dá udělat pomocí [socket.io client.java](https://github.com/nkzawa/socket.io-client.java).

Následující diagram popisuje rozložení topologie:

![diagram topologie](./media/apache-storm-sensor-data-analysis/sensoranalysis.png)

> [!NOTE]
> Tento diagram je zjednodušený přehled topologie. Pro každý oddíl v Centru událostí je vytvořena instance jednotlivých součástí. Tyto instance jsou rozdělené mezi uzly v clusteru a data se směruje mezi nimi takto:
> 
> * Data z spout pro Analyzátor je skupinu s vyrovnáváním zatížení.
> * Data z analyzátor na řídicí panel a HBase se seskupují podle ID zařízení tak, aby zprávy ze stejného zařízení vždy tok stejné komponenty.

### <a name="topology-components"></a>Topologie součásti

* **Event Hub Spout**: spout je zadaný jako součást Apache Storm verze 0.10.0 nebo vyšší.
  
  > [!NOTE]
  > Spout centra událostí použité v tomto příkladu vyžaduje Storm v HDInsight clusteru verze 3.5 nebo 3.6.

* **ParserBolt.java**: data, která je vysílaných spout je nezpracovaném formátu JSON a současně je vygenerované příležitostně více než jednu událost. Touto funkcí bolt čte data vysílaných spout a analyzuje zpráva JSON. Bolt pak vysílá data jako řazené kolekce členů, která obsahuje více polí.
* **DashboardBolt.java**: Tato součást ukazuje, jak používat k odesílání dat v reálném čase do řídicího panelu webové Socket.io klientské knihovny pro jazyk Java.
* **Ne hbase.yaml**: definici topologie používá při spuštění v místním režimu. HBase součásti nepoužívá.
* **s hbase.yaml**: definici topologie používá při spuštění topologii v clusteru. Používá HBase součásti.
* **dev.Properties**: informace o konfiguraci funkcí spout centra událostí, HBase bolt a součástí řídicího panelu.

## <a name="prepare-your-environment"></a>Příprava prostředí

Než použijete tento příklad, musíte připravit vývojového prostředí. Musíte taky vytvořit centra událostí Azure, který je používán v tomto příkladu.

Je třeba nainstalovat na vývojového prostředí následující položky:

* [Node.js](http://nodejs.org/): slouží k náhledu řídicího panelu webové místně na vašem vývojovém prostředí.
* [Java a JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): použité k jejich vývoji topologie Storm.
* [Maven](http://maven.apache.org/what-is-maven.html): použít k sestavení a kompilaci projektu.
* [Git](http://git-scm.com/): používá ke stahování projektu z Githubu.
* **SSH** klienta: používá k připojení ke clusterům HDInsight se systémem Linux. Další informace najdete v tématu [Použití SSH se službou HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

Při vytváření centra událostí, použijte postup v [vytváření centra událostí](../../event-hubs/event-hubs-create.md) dokumentu.

> [!IMPORTANT]
> Uložení RootManageSharedAccessKey název centra událostí, obor názvů a klíč. Tyto informace slouží ke konfiguraci topologie Storm.

Není nutné cluster služby HDInsight. Kroky v tomto dokumentu zadejte šablonu Azure Resource Manager, která vytváří prostředky potřebné pro tento příklad. Šablona vytváří v následujících zdrojích informací:
 
* Virtuální síť Azure
* Storm v clusteru HDInsight (systémem Linux dva uzly pracovního procesu)
* Na clusteru HDInsight HBase (systémem Linux dva uzly pracovního procesu)
* Webové aplikace Azure, který je hostitelem webové řídicí panel

## <a name="download-and-configure-the-project"></a>Stáhněte si a konfigurace projektu

Použijte následující ke stažení projektu z Githubu.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

Po dokončení příkazu, máte následující adresářovou strukturu:

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal.
                no-hbase.yaml - topology definition without hbase components.
                with-hbase.yaml - topology definition with hbase components.
            src/main/java/com/microsoft/examples/bolts/
                ParserBolt.java - parses JSON data into tuples
                DashboardBolt.java - sends data over Socket.IO to the web dashboard.
        dashboard/nodejs/ - this is the node.js web dashboard.
        SendEvents/ - utilities to send fake sensor data.

> [!NOTE]
> Tento dokument není přejděte na podrobnosti o kód, který najdete v tomto příkladu. Kód, je však plně komentář.

Ke konfiguraci projektu ke čtení z centra událostí, otevřete `hdinsight-eventhub-example/TemperatureMonitor/dev.properties` souboru a přidejte informace vašeho centra událostí do následující řádky:

```bash
eventhub.policy.key: the_key_for_RootManageSharedAccessKey
eventhub.namespace: your_namespace_here
eventhub.name: your_event_hub_name
eventhub.partitions: 2
```

## <a name="compile-and-test-locally"></a>Kompilace a testování místně

> [!IMPORTANT]
> Místně pomocí topologie vyžaduje funkční Storm vývojové prostředí. Další informace najdete v tématu [nastavení vývojového prostředí Storm](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) na Apache.org.

Před testováním, musíte spustit řídicí panel k zobrazení výstupu topologie a generovat data uložit do centra událostí.

> [!IMPORTANT]
> Komponenta HBase Tato topologie není aktivní, při testování místně. Rozhraní API Java pro cluster HBase, není přístupná z mimo virtuální síť Azure, který obsahuje clustery.

### <a name="start-the-web-application"></a>Spuštění webové aplikace

1. Otevřete příkazový řádek a přejděte do adresáře `hdinsight-eventhub-example/dashboard`. Použijte následující příkaz k instalaci závislosti nutné webovou aplikací:
   
    ```bash
    npm install
    ```

2. Použijte následující příkaz pro spuštění webové aplikace:
   
    ```bash
    node server.js
    ```
   
    Zobrazí zpráva podobná následující text:
   
        Server listening at port 3000

3. Otevřete webový prohlížeč a zadejte `http://localhost:3000/` jako adresu. Zobrazí se stránka podobná na následujícím obrázku:
   
    ![webové řídicí panel](./media/apache-storm-sensor-data-analysis/emptydashboard.png)
   
    Nechte tato příkazového řádku otevřené. Po testování, pomocí kombinace kláves Ctrl-C zastavení webového serveru.

### <a name="generate-data"></a>Generování dat

> [!NOTE]
> Postup v této části použijte Node.js tak, aby bylo možné na jakékoli platformě. Další příklady jazyka najdete v článku `SendEvents` adresáře.

1. Otevřete nový řádek, prostředí nebo terminálu a přejděte do adresáře `hdinsight-eventhub-example/SendEvents/nodejs`. Pokud chcete nainstalovat závislosti potřebné pro aplikaci, použijte následující příkaz:

    ```bash
    npm install
    ```

2. Otevřete `app.js` soubor v textovém editoru a přidání informací o centra událostí, které jste dříve získali:
   
    ```javascript
    // ServiceBus Namespace
    var namespace = 'Your-eventhub-namespace';
    // Event Hub Name
    var hubname ='Your-eventhub-name';
    // Shared access Policy name and key (from Event Hub configuration)
    var my_key_name = 'RootManageSharedAccessKey';
    var my_key = 'Your-Key';
    ```
   
   > [!NOTE]
   > Tento příklad předpokládá, že používáte `RootManageSharedAccessKey` přístup vašeho centra událostí.

3. Chcete-li vložit nové položky v Centru událostí použijte následující příkaz:
   
    ```bash
    node app.js
    ```
   
    Zobrazí několik řádků výstupu, které obsahují data odeslaná do centra událostí:
   
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="build-and-start-the-topology"></a>Sestavte a spusťte topologie

1. Otevřete nový příkazový řádek a přejděte do adresáře `hdinsight-eventhub-example/TemperatureMonitor`. Pro sestavení a balíček topologii, použijte následující příkaz: 

    ```bash
    mvn clean package
    ```

2. Pokud chcete spustit topologii v místním režimu, použijte následující příkaz:

    ```bash
    storm jar target/TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local --filter dev.properties resources/no-hbase.yaml
    ```

    * `--local`Spustí topologii v místním režimu.
    * `--filter`používá `dev.properties` souboru k naplnění parametry v definici topologie.
    * `resources/no-hbase.yaml`používá `no-hbase.yaml` definice topologie.
 
   Po zahájení, topologii čte položky z centra událostí a odešle je na řídicí panel spuštěna na místním počítači. Měli byste vidět řádky na webový řídicím panelu nezobrazí, podobně jako na následujícím obrázku:
   
    ![řídicí panel s daty](./media/apache-storm-sensor-data-analysis/datadashboard.png)

2. Řídicí panel průběhu, použijte `node app.js` příkazu z předchozích kroků a odesílat nová data do centra událostí. Protože teploty hodnoty jsou náhodně vygenerované, by měl aktualizovat graf můžete zobrazit velké změny v teploty.
   
   > [!NOTE]
   > Musí být v **hdinsight-eventhub – příklad/SendEvents/Nodejs** directory při použití `node app.js` příkaz.

3. Po ověření, že aktualizace řídicího panelu, zastavte topologie pomocí kombinace kláves Ctrl + C. Ctrl + C můžete také zastavit místní webový server.

## <a name="create-a-storm-and-hbase-cluster"></a>Vytvoření clusteru Storm a HBase

Kroky v tomto tématu použijte [šablony Azure Resource Manageru](../../azure-resource-manager/resource-group-template-deploy.md) pro vytvoření clusteru služby Azure Virtual Network a Storm a HBase ve virtuální síti. Šablony také vytvoří Azure Web App a nasadí kopie řídicího panelu do ní.

> [!NOTE]
> Virtuální síť se používá, takže topologie spuštěné v clusteru Storm může komunikovat přímo s clusteru HBase pomocí rozhraní API HBase Java.

Šablony Resource Manageru v tomto dokumentu se nachází v kontejneru veřejného objektu blob na **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.

1. Klikněte na následující tlačítko a přihlaste se k Azure a otevřete šablonu Resource Manageru na portálu Azure.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet-3.6.json" target="_blank"><img src="./media/apache-storm-sensor-data-analysis/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Z **vlastní nasazení** části, zadejte následující hodnoty:
   
    ![Parametry HDInsight](./media/apache-storm-sensor-data-analysis/parameters.png)
   
   * **Základní název clusteru**: Tato hodnota se používá jako základní název pro Storm a HBase clusterů. Například zadáním **abc** vytvoří cluster Storm s názvem **storm abc** a cluster HBase s názvem **hbase abc**.
   * **Uživatelské jméno přihlášení clusteru**: uživatelské jméno správce pro clustery Storm a HBase.
   * **Heslo pro přihlášení clusteru**: uživatelské heslo správce pro clustery Storm a HBase.
   * **Uživatelské jméno SSH**: SSH, aby uživatel vytvořil pro clustery Storm a HBase.
   * **Heslo SSH**: heslo pro uživatele SSH pro clustery Storm a HBase.
   * **Umístění**: oblast, která clustery jsou vytvořeny v.
     
     Klikněte na možnost **OK** a uložte parametry.

3. Použití **Základy** části vytvořit skupinu prostředků nebo vyberte nějaký existující.
4. V **umístění skupiny prostředků** rozevírací nabídce vyberte stejné umístění jako jste vybrali pro **umístění** parametr ve **nastavení** části.
5. Přečtěte si podmínky a ujednání a pak vyberte **souhlasím s podmínkami a ujednáními výše uvedených**.
6. Nakonec zkontrolujte **připnout na řídicí panel** a pak vyberte **nákupu**. Chcete-li vytvořit clustery trvá asi 20 minut.

Po vytvoření prostředky, zobrazí se informace o skupině prostředků.

![Skupinu prostředků pro virtuální síť a clustery](./media/apache-storm-sensor-data-analysis/groupblade.png)

> [!IMPORTANT]
> Všimněte si, že jsou názvy clusterů HDInsight **storm BASENAME** a **hbase BASENAME**, kde BASENAME je jméno, které jste zadali v šabloně. Použít tyto názvy v pozdější fázi při připojení k clustery. Také Upozorňujeme, že je název řídicího panelu webu **basename – řídicí panel**. Tato hodnota se používá později v tomto dokumentu.

## <a name="configure-the-dashboard-bolt"></a>Konfigurace funkcí bolt řídicí panel

Pokud chcete odesílat data na řídicí panel nasadit jako webovou aplikaci, je třeba upravit následující řádek v `dev.properties`souboru:

```yaml
dashboard.uri: http://localhost:3000
```

Změna `http://localhost:3000` k `http://BASENAME-dashboard.azurewebsites.net` a soubor uložte. Nahraďte **BASENAME** s základní jméno, které jste zadali v předchozím kroku. Můžete taky Vybrat řídicí panel a zobrazit adresu URL předtím vytvořili skupinu prostředků.

## <a name="create-the-hbase-table"></a>Vytvoření tabulky HBase

K uložení dat v HBase, jsme musíte nejdřív vytvořit tabulku. Předem vytvořit prostředky, které Storm potřebuje k zápisu, jako pokusu o vytvoření prostředky z uvnitř Storm může způsobit topologie více instancí pokusu o vytvoření stejného zdroje. Vytvořit prostředky mimo topologie a používat Storm pro čtení/zápis a analýzy.

1. Použití SSH se připojit ke clusteru HBase pomocí SSH uživatele a heslo, které jste zadali v šabloně při vytváření clusteru. Například, pokud připojení pomocí `ssh` příkazu, použijte následující syntaxi:
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```
   
    Nahraďte `sshuser` uživatelským jménem SSH, které jste zadali při vytvoření clusteru. Nahraďte `clustername` s názvem clusteru HBase.

2. Z relace SSH spusťte prostředí HBase.
   
    ```bash
    hbase shell
    ```
   
    Jakmile prostředí načetl, zobrazí `hbase(main):001:0>` řádku.

3. Z prostředí HBase zadejte následující příkaz a vytvořte tabulku pro ukládání dat snímačů:
   
    ```hbase
    create 'SensorData', 'cf'
    ```

4. Ověřte, že tabulka byla vytvořena pomocí následujícího příkazu:
   
    ```hbase
    scan 'SensorData'
    ```
   
    Vrátí informace podobně jako v následujícím příkladu, označující, že jsou 0 řádky v tabulce.
   
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Zadejte `exit` ukončíte prostředí HBase:

## <a name="configure-the-hbase-bolt"></a>Konfigurace funkcí bolt HBase

Zapsat do HBase z clusteru Storm, je nutné zadat HBase bolt s podrobností konfigurace clusteru HBase.

1. K načtení Zookeeper kvora pro váš cluster HBase, použijte jednu z následujících příkladech:

    ```bash
    CLUSTERNAME='your_HDInsight_cluster_name'
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HBASE/components/HBASE_MASTER" | jq '.metrics.hbase.master.ZookeeperQuorum'
    ```

    > [!NOTE]
    > Parametr `your_HDInsight_cluster_name` nahraďte názvem vašeho clusteru HDInsight. Další informace o instalaci `jq` nástroj, najdete v části [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).
    >
    > Po zobrazení výzvy zadejte heslo pro přihlašovací jméno správce HDInsight.

    ```powershell
    $clusterName = 'your_HDInsight_cluster_name'
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HBASE/components/HBASE_MASTER" -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.metrics.hbase.master.ZookeeperQuorum
    ```

    > [!NOTE]
    > Nahraďte ' your_HDInsight_cluster_name s názvem clusteru HDInsight. Po zobrazení výzvy zadejte heslo pro přihlašovací jméno správce HDInsight.
    >
    > Tento příklad vyžaduje prostředí Azure PowerShell. Další informace o použití prostředí Azure PowerShell najdete v tématu [Začínáme s Azure PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6)

    Informace vrácené těchto příkladech je podobná následující text:

    `zk2-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk0-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk3-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181`

    Tyto informace slouží Storm ke komunikaci s clusterem HBase.

2. Upravit `dev.properties` souboru a přidejte informace o Zookeeper kvora do následujícího řádku:

    ```yaml
    hbase.zookeeper.quorum: your_hbase_quorum
    ```

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a>Vytvoření balíčku a nasadit řešení do HDInsight

Ve vašem vývojovém prostředí použijte následující kroky k nasazení topologie Storm ke clusteru storm.

1. Z `TemperatureMonitor` adresář, použijte následující příkaz k provedení nového sestavení a vytvoření JAR balíčků z projektu:
   
        mvn clean package
   
    Tento příkaz vytvoří soubor s názvem `TemperatureMonitor-1.0-SNAPSHOT.jar in the `cíl ' adresáři projektu.

2. Spojovací bod služby použít k nahrání `TemperatureMonitor-1.0-SNAPSHOT.jar` a `dev.properties` soubory do clusteru Storm. V následujícím příkladu nahraďte `sshuser` uživatele SSH, jste zadali při vytváření clusteru, a `clustername` s názvem vašeho clusteru Storm. Po zobrazení výzvy zadejte heslo pro uživatele SSH.
   
    ```bash
    scp target/TemperatureMonitor-1.0-SNAPSHOT.jar dev.properties sshuser@clustername-ssh.azurehdinsight.net:
    ```

   > [!NOTE]
   > Ho může trvat několik minut odesílat soubory.

    Další informace o používání `scp` a `ssh` příkazy s HDInsight, najdete v části [použití SSH s HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md)

3. Jakmile soubor byl odeslán, připojte se ke clusteru Storm pomocí protokolu SSH.
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Nahraďte `sshuser` s uživatelským jménem SSH. Nahraďte `clustername` s názvem clusteru Storm.

4. Chcete-li spustit topologii, použijte následující příkaz z relace SSH:
   
    ```bash
    storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote --filter dev.properties -R /with-hbase.yaml
    ```

    * `--remote`odešle topologii pro službu Nimbus, která distribuuje jej do nadřízeného uzly v clusteru.
    * `--filter`používá `dev.properties` souboru k naplnění parametry v definici topologie.
    * `-R /with-hbase.yaml`používá `with-hbase.yaml` topologie, které jsou součástí balíčku.

5. Po spuštění topologii, otevřete prohlížeč na web můžete publikovat na platformě Azure, potom použít `node app.js` příkaz, který odesílá data do centra událostí. Měli byste vidět řídicího panelu webové aktualizace pro zobrazení informací.
   
    ![řídicí panel](./media/apache-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>Zobrazení dat HBase

Použijte následující postup pro připojení k HBase a ověřte, že data byla zapsána na tabulky:

1. Použití SSH se připojit ke clusteru HBase.
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Nahraďte `sshuser` s uživatelským jménem SSH. Nahraďte `clustername` s názvem clusteru HBase.

2. Z relace SSH spusťte prostředí HBase.
   
    ```bash
    hbase shell
    ```
   
    Jakmile prostředí načetl, zobrazí `hbase(main):001:0>` řádku.

3. Zobrazení řádků z tabulky:
   
    ```hbase
    scan 'SensorData'
    ```
   
    Tento příkaz vrátí informace podobná následující text, označující, že je v tabulce data.
   
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds
   
   > [!NOTE]
   > Tato operace kontroly vrací maximálně 10 řádků z tabulky.

## <a name="delete-your-clusters"></a>Odstranění clusterů

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

Pokud chcete odstranit clustery, úložiště a webové aplikace v jednom okamžiku, odstraňte skupinu prostředků, která je obsahuje.

## <a name="next-steps"></a>Další kroky

Další příklady topologií Storm s HDInsight naleznete v tématu [příklad topologií pro Storm v HDInsight](apache-storm-example-topology.md)

Další informace o Apache Storm naleznete v tématu [Apache Storm](https://storm.incubator.apache.org/) lokality.

Další informace o HBase v HDInsight, najdete v článku [HBase s HDInsight přehled](../hbase/apache-hbase-overview.md).

Další informace o Socket.io najdete v tématu [socket.io](http://socket.io/) lokality.

Další informace o D3.js najdete v tématu [D3.js - dokumenty řízené daty](http://d3js.org/).

Informace o vytváření topologie v jazyce Java najdete v tématu [Java vývoj topologií pro Apache Storm v HDInsight](apache-storm-develop-java-topology.md).

Informace o vytváření topologie v rozhraní .NET najdete v tématu [vývoj topologií C# pro Apache Storm v HDInsight pomocí sady Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com
