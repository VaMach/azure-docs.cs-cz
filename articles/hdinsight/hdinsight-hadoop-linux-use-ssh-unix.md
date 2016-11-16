---
title: "Použití klíče SSH se systémem Linux Hadoop z Linuxu, Unixu nebo OS X | Dokumentace Microsoftu"
description: " Do systému HDInsight, založeného na Linuxu, můžete přistupovat pomocí systém protokolu Secure Shell (SSH). Dokument poskytuje informace o používání SSH s HDInsight od klientů OS X, Linux a Unix."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: a6a16405-a4a7-4151-9bbf-ab26972216c5
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/13/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 476d9ce8b64f3442031310bd9170c682a9940b2b


---
# <a name="use-ssh-with-linuxbased-hadoop-on-hdinsight-from-linux-unix-or-os-x"></a>Použití SSH se systémem Linux Hadoop v HDInsight z OS X, Linux a Unix
> [!div class="op_single_selector"]
> * [Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
> * [Linux, Unix, OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
> 
> 

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) umožňuje vzdáleně provádět různé operace v clusterech služby HDInsight v Linuxu pomocí rozhraní příkazového řádku. Dokument poskytuje informace o používání SSH s HDInsight od klientů OS X, Linux a Unix.

> [!NOTE]
> Postup v tomto článku předpokládá, že používáte klienta se systémem Linux, Unix, nebo OS X. Tyto kroky může provést u klienta s Windows, pokud jste nainstalovali balíček, který poskytuje `ssh` a `ssh-keygen`, jako je například [Bash na Ubuntu ve Windows](https://msdn.microsoft.com/commandline/wsl/about).
> 
> Pokud nemáte v klientu Windows nainstalované SSH, postupujte podle kroků v tématu [Použití SSH s linuxovým systémem HDInsight (Hadoop) z Windows](hdinsight-hadoop-linux-use-ssh-windows.md), kde najdete informace o instalaci a používání PuTTY.
> 
> 

## <a name="prerequisites"></a>Požadavky
* **ssh-keygen** a **ssh** pro klienty OS X, Linux a Unix. Tyhle nástroje se obvykle poskytují s operačním systémem nebo jsou k dispozici v systému pro správu balíčků.
* Moderní webový prohlížeč, který podporuje HTML5.

NEBO

* [Azure CLI](../xplat-cli-install.md).
  
    [!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

## <a name="what-is-ssh"></a>Co je SSH?
SSH je nástroj pro přihlašování a vzdálené spouštění příkazů na vzdáleném serveru. U služby HDInsight se systémem Linux vytváří SSH šifrované připojení k hlavnímu uzlu clusteru a poskytuje příkazový řádek, který použijete k zadávání příkazů. Příkazy jsou poté provedeny přímo na serveru.

### <a name="ssh-user-name"></a>Uživatelské jméno SSH
Uživatelské jméno SSH je jméno sloužící k ověření clusteru HDInsight. Když zadáte uživatelské jméno SSH při vytváření clusteru, vytvoří se tento uživatel na všech uzlech v clusteru. Jakmile je cluster vytvořen, můžete použít toto uživatelské jméno pro připojení k hlavním uzlům clusteru HDInsight. Z hlavních uzlů se poté můžete připojit k jednotlivým pracovním uzlům.

### <a name="ssh-password-or-public-key"></a>Heslo nebo veřejný klíč SSH
Uživatel SSH může pro ověření použít buď heslo nebo veřejný klíč. Heslo představuje vymyšlený textový řetězec, zatímco veřejný klíč je součástí páru kryptografických klíčů vygenerovaných k vaší jednoznačné identifikaci.

Klíč je bezpečnější než heslo, ale vyžaduje další kroky pro vygenerování klíče a vy musíte zachovat soubory obsahující klíč v zabezpečeném umístění. Pokud někdo získá přístup k souborům klíčů, získá přístup k vašemu účtu. Případně pokud ztratíte soubory klíčů, nebudete se moci přihlásit ke svému účtu.

Pár klíčů se skládá z veřejného klíče (která se odesílá na server HDInsight) a soukromého klíče (který je uložen v klientském počítači). Při připojování k serveru HDInsight pomocí SSH použije klient SSH privátní klíč ve vašem počítači k ověření na serveru.

## <a name="create-an-ssh-key"></a>Vytvoření klíče SSH
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
     
     > [!NOTE]
     > Důrazně doporučujeme pro klíč použít zabezpečené přístupové heslo. Pokud však heslo zapomenete, neexistuje žádný způsob, jak jej obnovit.
     > 
     > 
     
     Po dokončení příkazu budete mít dva nové soubory, privátní klíč (například **id\_rsa**) a veřejný klíč (například **id\_rsa.pub**).

## <a name="create-a-linuxbased-hdinsight-cluster"></a>Vytvoření clusteru HDInsight se systémem Linux
Při vytváření clusteru HDInsight se systémem Linux je nutné zadat veřejný klíč, který jste vytvořili dříve. V klientech se systémem Linux, Unix nebo OS X existují dva způsoby vytvoření clusteru HDInsight:

* **Azure Portal** – používá k vytvoření clusteru webový portál.
* **Azure CLI pro Mac, Linux a Windows** – používá příkazy příkazového řádku k vytvoření clusteru.

Každá z těchto metod bude vyžadovat heslo nebo veřejný klíč. Úplné informace týkající se vytvoření clusteru HDInsight se systémem Linux naleznete v části [Clustery HDInsight se systémem Linux](hdinsight-hadoop-provision-linux-clusters.md).

### <a name="azure-portal"></a>Azure Portal
Pokud používáte web [Azure Portal][preview-portal] k vytvoření clusteru HDInsight se systémem Linux, musíte zadat **UŽIVATELSKÉ JMÉNO SSH** a vybrat, jestli chcete zadat **HESLO** nebo **VEŘEJNÝ KLÍČ SSH**.

Když vyberete **VEŘEJNÝ KLÍČ SSH**, můžete buď vložit veřejný klíč (uložený v souboru s příponou **.pub**) do pole **SSH PublicKey**, nebo vybrat možnost **Vybrat soubor** a procházet a vybrat soubor, který obsahuje veřejný klíč.

![Obrázek formuláře požadujícího veřejný klíč](./media/hdinsight-hadoop-linux-use-ssh-unix/ssh-key.png)

> [!NOTE]
> Soubor s klíčem je textový soubor. Obsah by se měl zobrazit přibližně takto:
> 
> ```
> ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCelfkjrpYHYiks4TM+r1LVsTYQ4jAXXGeOAF9Vv/KGz90pgMk3VRJk4PEUSELfXKxP3NtsVwLVPN1l09utI/tKHQ6WL3qy89WVVVLiwzL7tfJ2B08Gmcw8mC/YoieT/YG+4I4oAgPEmim+6/F9S0lU2I2CuFBX9JzauX8n1Y9kWzTARST+ERx2hysyA5ObLv97Xe4C2CQvGE01LGAXkw2ffP9vI+emUM+VeYrf0q3w/b1o/COKbFVZ2IpEcJ8G2SLlNsHWXofWhOKQRi64TMxT7LLoohD61q2aWNKdaE4oQdiuo8TGnt4zWLEPjzjIYIEIZGk00HiQD+KCB5pxoVtp user@system
> ```
> 
> 

Tím dojde k vytvoření přihlášení pro zadaného uživatele pomocí hesla a veřejného klíče, který zadáte.

### <a name="azure-commandline-interface-for-mac-linux-and-windows"></a>Rozhraní příkazového řádku Azure pro Mac, Linux a Windows
[Azure CLI pro Mac, Linux a Windows](../xplat-cli-install.md) můžete použít k vytvoření nového clusteru pomocí příkazu `azure hdinsight cluster create`.

Další informace o použití tohoto příkazu naleznete v tématu [Vytvoření Hadoop Linux clusterů v nástroji HDInsight pomocí vlastních možností](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="connect-to-a-linuxbased-hdinsight-cluster"></a>Připojení ke clusteru HDInsight se systémem Linux
Z relace terminálové služby se připojte pomocí příkazu SSH k hlavnímu uzlu clusteru poskytnutím adresy a uživatelského jména:

* **Adresa SSH** - Existují dvě adresy, které slouží k připojení ke clusteru pomocí protokolu SSH:
  
  * **Připojení k hlavnímu uzlu**: Název clusteru, za nímž následuje **-ssh.azurehdinsight.net**. Například **mycluster-ssh.azurehdinsight.net**.
  * **Připojit k uzlu edge**: Když je váš cluster R serverem v HDInsight, cluster bude také obsahovat hraniční uzel, na který lze přistupovat pomocí **RServer.CLUSTERNAME.ssh.azurehdinsight.net**, kde **CLUSTERNAME** je název clusteru.
* **Uživatelské jméno** - Uživatelské jméno SSH, které jste zadali při vytvoření clusteru.

Následující příklad se připojí k primárnímu hlavnímu uzlu clusteru **mycluster** jako uživatel **me**:

    ssh me@mycluster-ssh.azurehdinsight.net

Když jste použili heslo k uživatelskému účtu, zobrazí se výzva k jeho zadání.

Když jste použili klíč SSH, který je zabezpečen pomocí přístupového hesla, zobrazí se výzva k zadání přístupového hesla. V opačném případě se SSH pokusí provést automatické ověření pomocí jednoho z místních privátních klíčů vašeho klienta.

> [!NOTE]
> Pokud SSH neověřuje automaticky se správným privátním klíčem, použijte parametr **-i** a zadejte cestu k privátnímu klíči. Následující příklad načte privátní klíč z `~/.ssh/id_rsa`:
> 
> `ssh -i ~/.ssh/id_rsa me@mycluster-ssh.azurehdinsight.net`
> 
> 

Pokud se připojujete pomocí adresy pro hlavní uzel a neurčíte port, SSH určí jako výchozí port 22, který se připojí k primárnímu hlavnímu uzlu v clusteru HDInsight. Použijete-li port 23, připojíte se k sekundárnímu hlavnímu uzlu. Další informace o hlavních uzlech najdete v tématu [Dostupnost a spolehlivost clusterů Hadoop ve službě HDInsight](hdinsight-high-availability-linux.md).

### <a name="connect-to-worker-nodes"></a>Připojování k pracovním uzlům
Pracovní uzly nejsou přímo dostupné z oblasti mimo datové centrum Azure, ale jsou dostupné z hlavního uzlu clusteru přes SSH.

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

> [!IMPORTANT]
> Když k ověření svého účtu používáte klíč SSH, musíte dokončit předchozí kroky a ověřit, jestli funguje přesměrování agenta.
> 
> 

1. Připojte se ke clusteru HDInsight pomocí protokolu SSH podle dříve uvedeného postupu.
2. Jakmile se připojíte, použijte následující postup k načtení seznamu uzlů v clusteru. Nahraďte *ADMINPASSWORD* heslem pro váš účet správce clusteru. Nahraďte *CLUSTERNAME* názvem vašeho clusteru.
   
        curl --user admin:ADMINPASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/hosts
   
    Tato možnost vrátí informace ve formátu JSON pro uzly v clusteru, včetně `host_name`, který obsahuje plně kvalifikovaný název domény (FQDN) pro každý uzel. Následuje příklad položky `host_name` vrácené příkazem **curl**:
   
        "host_name" : "workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net"
3. Jakmile máte seznam uzlů pracovního procesu, ke kterému se chcete připojit, použijte následující příkaz z relace SSH k otevření připojení do pracovního uzlu:
   
        ssh USERNAME@FQDN
   
    Nahraďte položku *USERNAME* svým uživatelským jménem SSH a *FQDN* plně kvalifikovaným názvem domény pracovního uzlu. Například, `workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net`.
   
   > [!NOTE]
   > Pokud použijete heslo k ověřování relace SSH, zobrazí se výzva k opětovnému zadání hesla. Pokud používáte klíč SSH, je třeba připojení dokončit bez zobrazování výzev.
   > 
   > 
4. Po vytvoření relace se výzva terminálu změní z `username@hn#-clustername` na `username@wk#-clustername`, což značí, že jste připojeni k pracovnímu uzlu. Všechny příkazy, které spustíte v tomhle okamžiku, se spustí v pracovním uzlu.
5. Po dokončení provádění akcí v pracovním uzlu použijte příkaz `exit` k ukončení relace k uzlu pracovníka. Tím se vrátíte do řádku `username@hn#-clustername`.

## <a name="connect-to-a-domainjoined-hdinsight-cluster"></a>Připojení ke clusteru HDInsight připojenému do domény
[HDInsight s připojením k doméně](hdinsight-domain-joined-introduction.md) integruje protokol Kerberos s platformou Hadoop ve službě HDInsight. Protože uživatel SSH není uživatelem domény služby Active Directory, nemůže tento uživatelský účet spustit příkazy Hadoop přímo z prostředí SSH v clusteru připojeném k doméně. Je potřeba nejdříve spustit *kinit*. 

**Spuštění dotazů Hive v clusteru HDInsight připojeném k doméně pomocí protokolu SSH**

1. Připojte se ke clusteru HDInsight připojenému do domény pomocí protokolu SSH.  Pokyny najdete v tématu [Připojení ke clusteru HDInsight se systémem Linux](#connect-to-a-linux-based-hdinsight-cluster).
2. Spusťte kinit. Zobrazí se výzva k zadání doménového uživatelského jména a hesla uživatele domény. Další informace o konfiguraci uživatelů domény pro clustery HDInsight připojené k doméně najdete v tématu [Konfigurace clusterů HDInisight připojených do domény](hdinsight-domain-joined-configure.md).
   
    ![Kinit připojený k doméně Hadoop ve službě HDInsight](./media/hdinsight-hadoop-linux-use-ssh-unix/hdinsight-domain-joined-hadoop-kinit.png)
3. Otevřete konzolu Hive tak, že zadáte:
   
        hive
   
    Pak můžete spustit příkazy Hive.

## <a name="add-more-accounts"></a>Přidat další účty
1. Vygenerujte nový veřejný klíč a privátní klíč pro nový uživatelský účet, jak je popsáno v části  [Vytvoření klíče SSH](#create-an-ssh-key-optional).
   
   > [!NOTE]
   > Privátní klíč by se měl generovat buď na klientovi, kterého uživatel použije pro připojení ke clusteru, nebo by se měl po vytvoření na tohoto klienta bezpečně přenést.
   > 
   > 
2. Z relace SSH do clusteru přidejte nového uživatele pomocí následujícího příkazu:
   
        sudo adduser --disabled-password <username>
   
    Vytvoří se nový uživatelský účet, ale se zakázaným ověřováním pomocí hesla.
3. Vytvořte adresář a soubory pro uložení klíče pomocí následujících příkazů:
   
        sudo mkdir -p /home/<username>/.ssh
        sudo touch /home/<username>/.ssh/authorized_keys
        sudo nano /home/<username>/.ssh/authorized_keys
4. Když se otevře editor nano, zkopírujte a vložte obsah veřejného klíče pro nový uživatelský účet. Nakonec použijte zkratku **Ctrl-X**, soubor uložte a zavřete editor.
   
    ![obrázek nano editoru s ukázkovým klíčem](./media/hdinsight-hadoop-linux-use-ssh-unix/nano.png)
5. Pro změnu vlastnictví složky .ssh a obsahu na nový uživatelský účet použijte následující příkaz:
   
        sudo chown -hR <username>:<username> /home/<username>/.ssh
6. Nyní byste měli být schopni ověřit server pomocí nového uživatelského účtu a soukromého klíče.

## <a name="a-idtunnelassh-tunneling"></a><a id="tunnel"></a>Tunelování SSH
SSH lze použít k tunelování místních požadavků, například webových požadavků, do clusteru HDInsight. Požadavek bude poté směrován na požadovaný prostředek, jako kdyby pocházel z hlavního uzlu clusteru HDInsight.

> [!IMPORTANT]
> Tunelové propojení SSH představuje požadavek pro přístup k webu uživatelského rozhraní pro některé služby Hadoop. Například uživatelské rozhraní Historie úloh nebo uživatelské rozhraní Správce prostředků lze otevřít pouze pomocí tunelového propojení SSH.
> 
> 

Další informace o vytváření a používání tunelového propojení SSH naleznete v tématu [Používání tunelového propojení SSH pro přístup k webovému uživatelskému rozhraní Ambari, ResourceManager, JobHistory, NameNode, Oozie a jiným webovým uživatelským rozhraním](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="next-steps"></a>Další kroky
Teď, když chápete, jak provádět ověření pomocí klíče SSH se dozvíte další informace o používání nástroje MapReduce s Hadoop v HDInsight.

* [Použití Hivu se službou HDInsight](hdinsight-use-hive.md)
* [Použití Pigu se službou HDInsight](hdinsight-use-pig.md)
* [Použití úloh MapReduce se službou HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/



<!--HONumber=Nov16_HO2-->


