<properties
   pageTitle="Používání klíčů SSH s Hadoop na linuxových clusterech ze systému Windows | Microsoft Azure"
   description="Zjistěte, jak můžete vytvářet a používat klíče SSH k ověřování clusterů v systému Linux HDInsight. Připojování clusterů z klientů se systémem Windows pomocí klienta PuTTY SSH."
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
   ms.date="03/25/2016"
   ms.author="larryfr"/>

#Použití SSH se systémem Linux Hadoop v HDInsight ze systému Windows

> [AZURE.SELECTOR]
- [Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
- [Linux, Unix, OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) umožňuje vzdáleně provádět různé operace v clusterech služby HDInsight v Linuxu pomocí rozhraní příkazového řádku. Tento dokument obsahuje informace o připojení k HDInsight z klientů se systémem Windows pomocí klienta PuTTY SSH.

> [AZURE.NOTE] Postup v tomto článku předpokládá, že používáte klienta se systémem Windows. Pokud používáte klienta OS X, Linux a Unix, naleznete postup v části [Použití SSH se systémem Linux Hadoop v HDInsight z OS X, Linux a Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

##Předpoklady

* **PuTTY** a **PuTTYGen** pro klienty se systémem Windows. Tyto nástroje jsou k dispozici na adrese [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

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

1. Otevřete PuTTYGen.

2. Pro pole **Typ klíče pro generování** vyberte možnost **RSA SSH-2** a pak klikněte na tlačítko **Generovat**.

    ![Rozhraní PuTTYGen](./media/hdinsight-hadoop-linux-use-ssh-windows/puttygen.png)

3. Pohybujte myši v oblasti pod indikátorem průběhu, dokud se pole nevyplní. Pohyb myši vygeneruje náhodná data, která se použijí ke generování klíče.

    ![hýbání myší okolo](./media/hdinsight-hadoop-linux-use-ssh-windows/movingmouse.png)

    Po vygenerování klíče se zobrazí veřejný klíč.

4. Pro dodatečné zabezpečení můžete zadat přístupové heslo do pole **přístupové heslo klíče** a pak zadejte stejnou hodnotu do pole **Potvrdit heslo**.

    ![přístupové heslo](./media/hdinsight-hadoop-linux-use-ssh-windows/key.png)

    > [AZURE.NOTE] Důrazně doporučujeme pro klíč použít zabezpečené přístupové heslo. Pokud však heslo zapomenete, neexistuje žádný způsob, jak jej obnovit.

5. Klikněte na tlačítko **Uložit privátní klíč** a uložte klíč do souboru **.ppk**. Klíč se použije k ověření clusteru HDInsight se systémem Linux.

    > [AZURE.NOTE] Tento klíč musíte uložit na bezpečném místě, jelikož ho lze použít k přístupu na váš clusteru HDInsight se systémem Linux.

6. Klikněte na tlačítko **Uložit veřejný klíč** a uložte klíč jako soubor **.txt**. Tento postup umožňuje opakovaně v budoucnosti používat veřejný klíč, když vytvoříte další clustery se systémem Linux HDInsight.

    > [AZURE.NOTE] Veřejný klíč se také zobrazí v horní části PuTTYGen. Můžete kliknout pravým tlačítkem na toto pole, zkopírovat hodnotu a pak ji vložit do formuláře při vytváření clusteru pomocí portálu Azure.

##Vytvoření clusteru HDInsight se systémem Linux

Při vytváření clusteru HDInsight se systémem Linux je nutné zadat veřejný klíč, který jste vytvořili dříve. V klientech se systémem Windows existují dva způsoby vytvoření clusteru HDInsight se systémem Linux:

* **Portál Azure** – používá k vytvoření clusteru webový portál.

* **Azure CLI pro Mac, Linux a Windows** – používá příkazy příkazového řádku k vytvoření clusteru.

Každá z těchto metod bude vyžadovat veřejný klíč. Úplné informace týkající se vytvoření clusteru HDInsight se systémem Linux naleznete v části [Clustery HDInsight se systémem Linux](hdinsight-hadoop-provision-linux-clusters.md).

###Portál Azure

Pokud používáte [portál Azure][preview-portal] k vytvoření clusteru HDInsight se systémem Linux, je nutné zadat **Uživatelské jméno SSH** a vybrat, zda chcete zadat **HESLO** nebo **VEŘEJNÝ KLÍČ SSH**.

Pokud vyberete **VEŘEJNÝ KLÍČ SSH**, můžete buď vložit veřejný klíč (zobrazený v části __Veřejný klíč pro vložení do pole klíčů oprávnění\_OpenSSH__ v PuttyGen,) do pole __SSH PublicKey__ nebo vybrat možnost __Vybrat soubor__ a procházet a vybrat soubor, který obsahuje veřejný klíč.

![Obrázek formuláře požadujícího veřejný klíč](./media/hdinsight-hadoop-linux-use-ssh-windows/ssh-key.png)

Tento postup vytvoří přihlášení pro určeného uživatele a umožňuje ověření heslem nebo ověření pomocí klíče SSH.

###Rozhraní příkazového řádku Azure pro Mac, Linux a Windows

[Azure CLI pro Mac, Linux a Windows](../xplat-cli-install.md) můžete použít k vytvoření nového clusteru pomocí příkazu `azure hdinsight cluster create`.

Další informace o použití tohoto příkazu naleznete v tématu [Vytvoření Hadoop Linux clusterů v nástroji HDInsight pomocí vlastních možností](hdinsight-hadoop-provision-linux-clusters.md).

##Připojení ke clusteru HDInsight se systémem Linux

1. Otevřete PuTTY.

    ![rozhraní putty](./media/hdinsight-hadoop-linux-use-ssh-windows/putty.png)

2. Pokud jste zadali klíče SSH při vytváření uživatelského účtu, je třeba provést následující krok a vybrat privátní klíč pro použití při ověřování clusteru:

    V poli **Kategorie** rozbalte položku **Připojení**, rozbalte položku **SSH** a vyberte možnost **Auth**. Nakonec klikněte na tlačítko **Procházet** a vyberte soubor .ppk, který obsahuje soukromý klíč.

    ![rozhraní putty, vyberte privátní klíč](./media/hdinsight-hadoop-linux-use-ssh-windows/puttykey.png)

3. V poli **Kategorie**, vyberte možnost **Relace**. Na obrazovce **Základní možnosti pro relaci PuTTY** zadejte adresu serveru HDInsight v SSH do pole **Název hostitele (nebo IP adresa)**. Existují dvě možné adresy SSH, které můžete použít při připojování ke clusteru:

    * __Adresa hlavního uzlu__: pro připojení k hlavnímu uzlu clusteru použijte název clusteru a pak adresu **-ssh.azurehdinsight.net**. Například **mycluster-ssh.azurehdinsight.net**.
    
    * __Adresa okrajového uzlu__: Pokud se připojujete k serveru R na clusteru HDInsight, můžete se připojit k okrajovému uzlu serveru R pomocí adresy __RServer.CLUSTERNAME.ssh.azurehdinsight.net__, kde CLUSTERNAME představuje název clusteru. Například __RServer.mycluster.ssh.azurehdinsight.net__.

    ![rozhraní putty se zadanou adresou ssh](./media/hdinsight-hadoop-linux-use-ssh-windows/puttyaddress.png)

4. Chcete-li uložit informace o připojení pro budoucí použití, zadejte název tohoto připojení v části **Uložit relací**, a pak klikněte na tlačítko **Uložit**. Připojení se přidá do seznamu uložených relací.

5. Klikněte na tlačítko **Otevřít** a připojte se ke clusteru.

    > [AZURE.NOTE] Pokud je to poprvé, kdy se připojujete ke clusteru, zobrazí se výstraha zabezpečení. To je normální. Vyberte možnost **Ano** pro ukládání do mezipaměti klíče RSA2 serveru a pokračujte.

6. Po zobrazení výzvy zadejte uživatele, kterého jste zadali při vytvoření clusteru. Pokud jste pro uživatele zadali heslo, budete také vyzváni k jeho zadání.

> [AZURE.NOTE] Výše uvedené kroky předpokládají, že používáte port 22, který se připojí k hlavnímu uzlu 0 v clusteru HDInsight. Pokud používáte port 23, připojíte se k hlavnímu uzlu 1. Další informace o hlavních uzlech naleznete v tématu [Dostupnost a spolehlivost Hadoop clusterů v HDInsight](hdinsight-high-availability-linux.md).

###Připojování k pracovním uzlům

Pracovní uzly nejsou přímo dostupné z oblasti mimo datové centrum Azure, ale jsou dostupné z hlavního uzlu clusteru pomocí protokolu SSH.

Pokud jste zadali klíč SSH při vytváření uživatelského účtu, musíte provést následující kroky a použít privátní klíč pro ověřování do clusteru, pokud se chcete připojit k pracovním uzlům.

1. Nainstalujte Pageant z adresy [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). Tento nástroj se používá pro ukládání klíčů SSH pro PuTTY do mezipaměti.

2. Spusťte modul Pageant. Minimalizuje se na ikonu na stavovém panelu. Klikněte pravým tlačítkem myši na ikonu a vyberte možnost **Přidat klíč**.

    ![přidání klíče](./media/hdinsight-hadoop-linux-use-ssh-windows/addkey.png)

3. Jakmile se zobrazí dialogové okno Procházet, vyberte soubor .ppk, který obsahuje klíč, a pak klikněte na tlačítko **Otevřít**. Tento postup přidá klíč do modulu Pageant, který ho poskytne PuTTY při připojování ke clusteru.

    > [AZURE.IMPORTANT] Pokud jste použili klíče SSH k zabezpečení účtu, musíte dokončit předchozí postup, než bude možné se připojit k pracovním uzlům.

4. Otevřete PuTTY.

5. Pokud použijete klíč SSH k ověření, v části **Kategorie** rozbalte položku **Připojení**, rozbalte položku **SSH** a pak vyberte možnost **Auth**.

    V části **Parametry ověřování** povolte možnost **Povolit agenta předávání**. Tento postup umožní PuTTY automaticky předat ověřování certifikátu prostřednictvím připojení k hlavnímu uzlu clusteru při připojování k pracovním uzlům.

    ![povolit přesměrování agenta](./media/hdinsight-hadoop-linux-use-ssh-windows/allowforwarding.png)

6. Připojte se ke clusteru, jak je uvedeno výše. Pokud použijete klíč SSH pro ověřování, nemusíte klíč vybírat – k ověření clusteru se použije klíč SSH přidaný do modulu Pageant.

7. Po navázání připojení použijte následující postup k načtení seznamu uzlů v clusteru. Nahraďte *ADMINPASSWORD* heslem pro váš účet správce clusteru. Nahraďte *CLUSTERNAME* názvem vašeho clusteru.

        curl --user admin:ADMINPASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/hosts

    Tato možnost vrátí informace ve formátu JSON pro uzly v clusteru, včetně `host_name`, který obsahuje plně kvalifikovaný název domény (FQDN) pro každý uzel. Následuje příklad položky `host_name` vrácené příkazem **curl**:

        "host_name" : "workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net"

8. Jakmile máte seznam uzlů pracovního procesu, ke kterému se chcete připojit, použijte následující příkaz z relace PuTTY k otevření připojení do pracovního uzlu:

        ssh USERNAME@FQDN

    Nahraďte položku *USERNAME* svým uživatelským jménem SSH a *FQDN* plně kvalifikovaným názvem domény pracovního uzlu. Například, `workernode0.workernode-0-e2f35e63355b4f15a31c460b6d4e1230.j1.internal.cloudapp.net`.

    > [AZURE.NOTE] Pokud použijete heslo k ověřování relace SSH, zobrazí se výzva k opětovnému zadání hesla. Pokud používáte klíč SSH, je třeba připojení dokončit bez zobrazování výzev.

9. Po vytvoření relace se řádek pro relaci PuTTY změní z `username@hn0-clustername` na `username@wn0-clustername`, což značí, že jste připojeni k pracovnímu uzlu. Všechny příkazy, které spustíte v tomto okamžiku, bude spouštěny v pracovním uzlu.

10. Po dokončení provádění akcí v pracovním uzlu použijte příkaz `exit` k ukončení relace k uzlu pracovníka. Tím se vrátíte do řádku `username@hn0-clustername`.

##Přidat další účty

Pokud potřebujete přidat další účty do clusteru, postupujte následovně:

1. Vygenerujte nový veřejný klíč a soukromý klíč pro nový uživatelský účet, jak je popsáno výše.

2. Z relace SSH do clusteru přidejte nového uživatele pomocí následujícího příkazu:

        sudo adduser --disabled-password <username>

    Vytvoří se nový uživatelský účet, ale se zakázaným ověřováním pomocí hesla.

3. Vytvořte adresář a soubory pro uložení klíče pomocí následujících příkazů:

        sudo mkdir -p /home/<username>/.ssh
        sudo touch /home/<username>/.ssh/authorized_keys
        sudo nano /home/<username>/.ssh/authorized_keys

4. Když se otevře editor nano, zkopírujte a vložte obsah veřejného klíče pro nový uživatelský účet. Nakonec použijte zkratku **Ctrl-X**, soubor uložte a zavřete editor.

    ![obrázek nano editoru s ukázkovým klíčem](./media/hdinsight-hadoop-linux-use-ssh-windows/nano.png)

5. Pro změnu vlastnictví složky .ssh a obsahu na nový uživatelský účet použijte následující příkaz:

        sudo chown -hR <username>:<username> /home/<username>/.ssh

6. Nyní byste měli být schopni ověřit server pomocí nového uživatelského účtu a soukromého klíče.

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



<!--HONumber=Jun16_HO2-->


