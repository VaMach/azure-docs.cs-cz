<properties
   pageTitle="Použití klíče SSH se systémem Linux Hadoop z OS X, Linux nebo Unix | Microsoft Azure"
   description=" Do systému HDInsight, založeného na Linuxu, můžete přistupovat pomocí systém protokolu Secure Shell (SSH). Dokument poskytuje informace o používání SSH s HDInsight od klientů OS X, Linux a Unix."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="06/14/2016"
   ms.author="larryfr"/>

#Použití SSH se systémem Linux Hadoop v HDInsight ze systému OS X, Linux a Unix.

> [AZURE.SELECTOR]
- [Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
- [Linux, Unix, OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) umožňuje vzdáleně provádět různé operace v clusterech služby HDInsight v Linuxu pomocí rozhraní příkazového řádku. Dokument poskytuje informace o používání SSH s HDInsight od klientů OS X, Linux a Unix.

> [AZURE.NOTE] Postup v tomto článku předpokládá, že používáte klienta se systémem Linux, Unix, nebo OS X. Protože tyhle kroky se dají provést u klienta se systémem Windows, pokud jste nainstalovali balíček, který poskytuje `ssh` a `ssh-keygen` (například Git pro Windows), doporučujeme, aby klienti systému Windows postupovali podle kroků v části [Použití SSH se systémem Linux HDInsight (Hadoop) ze systému Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

##Požadavky

* **ssh-keygen** a **ssh** pro klienty OS X, Linux a Unix. Tyhle nástroje se obvykle poskytují s operačním systémem nebo jsou k dispozici v systému pro správu balíčků.

* Moderní webový prohlížeč, který podporuje HTML5.

NEBO

* [Azure CLI](../xplat-cli-install.md).

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

##Co je SSH?

SSH je nástroj pro přihlašování a vzdálené spouštění příkazů na vzdáleném serveru. Se systémem Linux HDInsight SSH vytvoří šifrované připojení k hlavnímu uzlu clusteru a poskytuje příkazový řádek, který použijete k zadávání příkazů. Příkazy jsou poté provedeny přímo na serveru.

###Uživatelské jméno SSH

Uživatelské jméno SSH je jméno sloužící k ověření clusteru HDInsight. Když zadáte uživatelské jméno SSH při vytváření clusteru, vytvoří se tento uživatel na všech uzlech v clusteru. Jakmile je cluster vytvořen, můžete použít toto uživatelské jméno pro připojení k hlavním uzlům clusteru HDInsight. Z hlavních uzlů se pak můžete připojit k jednotlivým pracovním uzlům.

###Heslo nebo veřejný klíč SSH

Uživatel SSH může pro ověření použít buď heslo nebo veřejný klíč. Heslo představuje vymyšlený textový řetězec, zatímco veřejný klíč je součástí páru kryptografických klíčů vygenerovaných k vaší jednoznačné identifikaci.

Klíč je bezpečnější než heslo, ale vyžaduje další kroky pro vygenerování klíče a vy musíte zachovat soubory obsahující klíč v zabezpečeném umístění. Pokud někdo získá přístup k souborům klíčů, získá přístup k vašemu účtu. Případně pokud ztratíte soubory klíčů, nebudete se moci přihlásit ke svému účtu.

Pár klíčů se skládá z veřejného klíče (která se odesílá na server HDInsight) a soukromého klíče (který je uložen v klientském počítači). Při připojování k serveru HDInsight pomocí SSH použije klient SSH privátní klíč ve vašem počítači k ověření na serveru.

##Vytvoření klíče SSH

Následující informace použijte, pokud plánujete používat klíče SSH s vaším clusterem. Pokud plánujete použít heslo, můžete tuto část přeskočit.

1. Otevřete relaci terminálové služby a pomocí následujícího příkazu zkontrolujte, jestli máte nějaké existující klíče SSH:

        ls -al ~/.ssh

    Vyhledejte následující soubory v seznamu adresářů. Jedná se o běžné názvy veřejných klíčů SSH.

    * id\_dsa.pub
    * id\_ecdsa.pub
    * id\_ed25519.pub
    * id\_rsa.pub

2. Když nechcete použít existující soubor nebo nemáte žádné existující klíče SSH, vygenerujte nový soubor pomocí následujícího postupu:

        ssh-keygen -t rsa

    Budete vyzváni k zadání následujících informací:

    * Umístění souboru – výchozí umístění je ~/.ssh/id\_rsa.
    * Přístupové heslo – zobrazí se výzva k jeho opětovnému zadání.

        > [AZURE.NOTE] Důrazně doporučujeme pro klíč použít zabezpečené přístupové heslo. Pokud však heslo zapomenete, neexistuje žádný způsob, jak jej obnovit.

    Po dokončení příkazu budete mít dva nové soubory, privátní klíč (například **id\_rsa**) a veřejný klíč (například **id\_rsa.pub**).

##Vytvoření clusteru HDInsight se systémem Linux

Při vytváření clusteru HDInsight se systémem Linux je nutné zadat veřejný klíč, který jste vytvořili dříve. V klientech se systémem Linux, Unix nebo OS X existují dva způsoby vytvoření clusteru HDInsight:

* **Portál Azure** – používá k vytvoření clusteru webový portál.

* **Azure CLI pro Mac, Linux a Windows** – používá příkazy příkazového řádku k vytvoření clusteru.

Každá z těchto metod bude vyžadovat heslo nebo veřejný klíč. Úplné informace týkající se vytvoření clusteru HDInsight se systémem Linux naleznete v části [Clustery HDInsight se systémem Linux](hdinsight-hadoop-provision-linux-clusters.md).

###Azure Portal

Pokud používáte [portál Azure][preview-portal] k vytvoření clusteru HDInsight se systémem Linux, musíte zadat **UŽIVATELSKÉ JMÉNO SSH** a vybrat, jestli chcete zadat **HESLO** nebo **VEŘEJNÝ KLÍČ SSH**.

Když vyberete **VEŘEJNÝ KLÍČ SSH**, můžete buď vložit veřejný klíč (uložený v souboru s příponou **.pub**) do pole __SSH PublicKey__, nebo vybrat možnost __Vybrat soubor__ a procházet a vybrat soubor, který obsahuje veřejný klíč.

![Obrázek formuláře požadujícího veřejný klíč](./media/hdinsight-hadoop-linux-use-ssh-unix/ssh-key.png)

> [AZURE.NOTE] Soubor s klíčem je textový soubor. Obsah by se měl zobrazit přibližně takto:
> ```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCelfkjrpYHYiks4TM+r1LVsTYQ4jAXXGeOAF9Vv/KGz90pgMk3VRJk4PEUSELfXKxP3NtsVwLVPN1l09utI/tKHQ6WL3qy89WVVVLiwzL7tfJ2B08Gmcw8mC/YoieT/YG+4I4oAgPEmim+6/F9S0lU2I2CuFBX9JzauX8n1Y9kWzTARST+ERx2hysyA5ObLv97Xe4C2CQvGE01LGAXkw2ffP9vI+emUM+VeYrf0q3w/b1o/COKbFVZ2IpEcJ8G2SLlNsHWXofWhOKQRi64TMxT7LLoohD61q2aWNKdaE4oQdiuo8TGnt4zWLEPjzjIYIEIZGk00HiQD+KCB5pxoVtp user@system
> ```

Tím dojde k vytvoření přihlášení pro zadaného uživatele pomocí hesla a veřejného klíče, který zadáte.

###Rozhraní příkazového řádku Azure pro Mac, Linux a Windows

[Azure CLI pro Mac, Linux a Windows](../xplat-cli-install.md) můžete použít k vytvoření nového clusteru pomocí příkazu `azure hdinsight cluster create`.

Další informace o použití tohoto příkazu naleznete v tématu [Vytvoření Hadoop Linux clusterů v nástroji HDInsight pomocí vlastních možností](hdinsight-hadoop-provision-linux-clusters.md).

##Připojení ke clusteru HDInsight se systémem Linux

Z relace terminálové služby se připojte pomocí příkazu SSH k hlavnímu uzlu clusteru poskytnutím adresy a uživatelského jména:

* **Adresa SSH** - Existují dvě adresy, které slouží k připojení ke clusteru pomocí protokolu SSH:

    * **Připojte se k hlavnímu uzlu**: Název clusteru, za nímž následuje **-ssh.azurehdinsight.net**. Například **mycluster-ssh.azurehdinsight.net**.
    
    * **Připojit k uzlu edge**: Když je váš cluster R serverem v HDInsight, cluster bude také obsahovat hraniční uzel, na který lze přistupovat pomocí **RServer.CLUSTERNAME.ssh.azurehdinsight.net**, kde __CLUSTERNAME__ je název clusteru.

* **Uživatelské jméno** - Uživatelské jméno SSH, které jste zadali při vytvoření clusteru.

Následující příklad se připojí k hlavního uzlu 0 v **mycluster** jako uživatel **me**:

    ssh me@mycluster-ssh.azurehdinsight.net

Když jste použili heslo k uživatelskému účtu, zobrazí se výzva k jeho zadání.

Když jste použili klíč SSH, který je zabezpečen pomocí přístupového hesla, zobrazí se výzva k zadání přístupového hesla. V opačném případě se SSH pokusí provést automatické ověření pomocí jednoho z místních privátních klíčů vašeho klienta.

> [AZURE.NOTE] Pokud SSH neověřuje automaticky se správným privátním klíčem, použijte parametr **-i** a zadejte cestu k privátnímu klíči. Následující příklad načte privátní klíč z `~/.ssh/id_rsa`:
>
> `ssh -i ~/.ssh/id_rsa me@mycluster-ssh.azurehdinsight.net`

Pokud se připojujete pomocí adresy pro hlavní uzel a není určen port, SSH určí jako výchozí port 22, který se připojí k hlavnímu uzlu 0 v clusteru HDInsight. Když používáte port 23, budete se připojovat k hlavnímu uzlu 1. Další informace o hlavních uzlech naleznete v tématu [Dostupnost a spolehlivost Hadoop clusterů v HDInsight](hdinsight-high-availability-linux.md).

###Připojování k pracovním uzlům

Pracovní uzly nejsou přímo dostupné z oblasti mimo datové centrum Azure, ale jsou dostupné z hlavního uzlu clusteru pomocí protokolu SSH.

Když k ověření uživatelského účtu používáte klíč SSH, musíte u svého klienta dokončit následující kroky:

1. V textovém editoru otevřete `~/.ssh/config`. Pokud tenhle soubor neexistuje, můžete ho vytvořit zadáním `touch ~/.ssh/config` v terminálu.

2. Do souboru přidejte následující: Nahraďte *CLUSTERNAME* názvem clusteru HDInsight.

        Host CLUSTERNAME-ssh.azurehdinsight.net
          ForwardAgent yes

    Tím se nakonfiguruje přesměrování agenta SSH pro váš cluster HDInsight.

3. Proveďte test přesměrování agenta SSH pomocí následujícího příkazu z terminálu:

        echo "$SSH_AUTH_SOCK"

    Měla by se vrátit informace podobná této:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Když se nic nevrátí, znamená to, že **agent-ssh** se nespustil. Konkrétní kroky instalace a konfigurace pro **ssh-agent** najdete v dokumentaci operačního systému , nebo v tématu [Using ssh-agent with ssh](http://mah.everybody.org/docs/ssh).

4. Jakmile si ověříte, že  se **ssh-agent** spustil, podle následujícího postupu přidejte svůj privátní klíč SSH k agentovi:

        ssh-add ~/.ssh/id_rsa

    Pokud je privátní klíč uložen v jiném souboru, nahraďte `~/.ssh/id_rsa` cestou k souboru.

Pomocí následujících kroků se připojte k pracovním uzlům pro váš cluster.

> [AZURE.IMPORTANT] Když k ověření svého účtu používáte klíč SSH, musíte dokončit předchozí kroky a ověřit, jestli funguje přesměrování agenta.

1. Připojte se ke clusteru HDInsight pomocí protokolu SSH podle dříve uvedeného postupu.

2. Jakmile se připojíte, použijte následující postup k načtení seznamu uzlů v clusteru. Nahraďte *ADMINPASSWORD* heslem pro váš účet správce clusteru. Nahraďte *CLUSTERNAME* názvem vašeho clusteru.

        curl --user admin:ADMINPASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/hosts

    Tato možnost vrátí informace ve formátu JSON pro uzly v clusteru, včetně `host_name`, který obsahuje plně kvalifikovaný název domény (FQDN) pro každý uzel. Následuje příklad položky `host_name` vrácené příkazem **curl**:

        "host_name" : "workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net"

3. Jakmile máte seznam uzlů pracovního procesu, ke kterému se chcete připojit, použijte následující příkaz z relace SSH k otevření připojení do pracovního uzlu:

        ssh USERNAME@FQDN

    Nahraďte položku *USERNAME* svým uživatelským jménem SSH a *FQDN* plně kvalifikovaným názvem domény pracovního uzlu. Například, `workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net`.

    > [AZURE.NOTE] Pokud použijete heslo k ověřování relace SSH, zobrazí se výzva k opětovnému zadání hesla. Pokud používáte klíč SSH, je třeba připojení dokončit bez zobrazování výzev.

4. Po vytvoření relace se výzva terminálu změní z `username@hn0-clustername` na `username@wk0-clustername`, což značí, že jste připojeni k pracovnímu uzlu. Všechny příkazy, které spustíte v tomhle okamžiku, se spustí v pracovním uzlu.

4. Po dokončení provádění akcí v pracovním uzlu použijte příkaz `exit` k ukončení relace k uzlu pracovníka. Tím se vrátíte do řádku `username@hn0-clustername`.

##Přidat další účty

1. Vygenerujte nový veřejný klíč a privátní klíč pro nový uživatelský účet, jak je popsáno v části  [Vytvoření klíče SSH](#create-an-ssh-key-optional).

    > [AZURE.NOTE] Privátní klíč by se měl generovat buď na klientovi, kterého uživatel použije pro připojení ke clusteru, nebo by se měl po vytvoření na tohoto klienta bezpečně přenést.

1. Z relace SSH do clusteru přidejte nového uživatele pomocí následujícího příkazu:

        sudo adduser --disabled-password <username>

    Vytvoří se nový uživatelský účet, ale se zakázaným ověřováním pomocí hesla.

2. Vytvořte adresář a soubory pro uložení klíče pomocí následujících příkazů:

        sudo mkdir -p /home/<username>/.ssh
        sudo touch /home/<username>/.ssh/authorized_keys
        sudo nano /home/<username>/.ssh/authorized_keys

3. Když se otevře editor nano, zkopírujte a vložte obsah veřejného klíče pro nový uživatelský účet. Nakonec použijte zkratku **Ctrl-X**, soubor uložte a zavřete editor.

    ![obrázek nano editoru s ukázkovým klíčem](./media/hdinsight-hadoop-linux-use-ssh-unix/nano.png)

4. Pro změnu vlastnictví složky .ssh a obsahu na nový uživatelský účet použijte následující příkaz:

        sudo chown -hR <username>:<username> /home/<username>/.ssh

5. Nyní byste měli být schopni ověřit server pomocí nového uživatelského účtu a soukromého klíče.

##<a id="tunnel"></a>Tunelování SSH

SSH lze použít k tunelování místních požadavků, například webových požadavků, do clusteru HDInsight. Požadavek bude poté směrován na požadovaný prostředek, jako kdyby pocházel z hlavního uzlu clusteru HDInsight.

> [AZURE.IMPORTANT] Tunelové propojení SSH představuje požadavek pro přístup k webu uživatelského rozhraní pro některé služby Hadoop. Například uživatelské rozhraní Historie úloh nebo uživatelské rozhraní Správce prostředků lze otevřít pouze pomocí tunelového propojení SSH.

Další informace o vytváření a používání tunelového propojení SSH naleznete v tématu [Používání tunelového propojení SSH pro přístup k webovému uživatelskému rozhraní Ambari, ResourceManager, JobHistory, NameNode, Oozie a jiným webovým uživatelským rozhraním](hdinsight-linux-ambari-ssh-tunnel.md).

##Další kroky

Teď, když chápete, jak provádět ověření pomocí klíče SSH se dozvíte další informace o používání nástroje MapReduce s Hadoop v HDInsight.

* [Použijte Hive s HDInsight](hdinsight-use-hive.md)

* [Použijte Pig s HDInsight](hdinsight-use-pig.md)

* [Použijte úlohy MapReduce s HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/



<!---HONumber=Aug16_HO4-->


