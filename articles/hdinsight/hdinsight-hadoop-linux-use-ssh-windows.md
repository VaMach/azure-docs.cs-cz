---
title: "Použití SSH s clustery HDInsight pomocí PuTTY ze systému Windows | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvářet a používat klíče SSH k ověřování v clusterech HDInsight se systémem Linux z klientů se systémem Windows pomocí klienta SSH PuTTY."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 639328ca-d800-4fa9-97ed-5664477b88cd
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: b1806950581e0adbeec52839f12c70599d28100d
ms.lasthandoff: 03/21/2017

---
# <a name="use-ssh-with-hdinsight-hadoop-from-putty-on-windows"></a>Použití SSH s clustery HDInsight (Hadoop) pomocí PuTTY ze systému Windows

> [!div class="op_single_selector"]
> * [PuTTY (Windows)](hdinsight-hadoop-linux-use-ssh-windows.md)
> * [SSH (Windows, Linux, Unix, OS X)](hdinsight-hadoop-linux-use-ssh-unix.md)

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) umožňuje vzdáleně provádět různé operace v clusterech služby HDInsight v Linuxu pomocí rozhraní příkazového řádku. Tento dokument obsahuje informace o připojení k HDInsight z klientů se systémem Windows pomocí klienta PuTTY SSH.

> [!NOTE]
> Postup v tomto článku předpokládá, že používáte systém Windows s klientem SSH PuTTY. Pokud používáte systém Linux, Unix, OS X nebo Windows, který umožňuje použití příkazu `ssh`, přečtěte si téma [Použití SSH se systémem Linux Hadoop ve službě HDInsight ze systému Linux, Unix a OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="prerequisites"></a>Požadavky

* **PuTTY** a **PuTTYGen** pro klienty se systémem Windows. Tyto nástroje jsou k dispozici na adrese [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
* Moderní webový prohlížeč, který podporuje HTML5.

## <a name="what-is-ssh"></a>Co je SSH?

SSH je nástroj pro přihlašování a vzdálené spouštění příkazů na vzdáleném serveru. Se systémem Linux HDInsight SSH vytvoří šifrované připojení k hlavnímu uzlu clusteru a poskytuje příkazový řádek, který použijete k zadávání příkazů. Příkazy jsou poté provedeny přímo na serveru.

Systém Windows v minulosti klienta SSH neposkytoval. PuTTY je grafický klient SSH, kterého lze nainstalovat v systému Windows.

### <a name="ssh-user-name"></a>Uživatelské jméno SSH

Uživatelské jméno SSH je jméno sloužící k ověření clusteru HDInsight. Když zadáte uživatelské jméno SSH při vytváření clusteru, vytvoří se tento uživatel na všech uzlech v clusteru. Jakmile je cluster vytvořen, můžete použít toto uživatelské jméno pro připojení k hlavním uzlům clusteru HDInsight. Z hlavních uzlů se pak můžete připojit k jednotlivým pracovním uzlům.

### <a name="ssh-password-or-public-key"></a>Heslo nebo veřejný klíč SSH

Uživatel SSH může pro ověření použít buď heslo nebo veřejný klíč. Heslo představuje vymyšlený textový řetězec, zatímco veřejný klíč je součástí páru kryptografických klíčů vygenerovaných k vaší jednoznačné identifikaci.

Klíč je bezpečnější než heslo, ale vyžaduje další kroky pro vygenerování klíče a vy musíte zachovat soubory obsahující klíč v zabezpečeném umístění. Pokud někdo získá přístup k souborům klíčů, získá přístup k vašemu účtu. Případně pokud ztratíte soubory klíčů, nebudete se moci přihlásit ke svému účtu.

Pár klíčů se skládá z veřejného klíče (která se odesílá na server HDInsight) a soukromého klíče (který je uložen v klientském počítači). Při připojování k serveru HDInsight pomocí SSH použije klient SSH privátní klíč ve vašem počítači k ověření na serveru.

## <a name="create-an-ssh-key"></a>Vytvoření klíče SSH

Následující informace použijte, pokud plánujete používat klíče SSH s vaším clusterem. Pokud plánujete použít heslo, můžete tuto část přeskočit.

1. Otevřete PuTTYGen.

2. Pro pole **Typ klíče pro generování** vyberte možnost **RSA SSH-2** a pak klikněte na tlačítko **Generovat**.
   
    ![Rozhraní PuTTYGen](./media/hdinsight-hadoop-linux-use-ssh-windows/puttygen.png)

3. Pohybujte myši v oblasti pod indikátorem průběhu, dokud se pole nevyplní. Pohyb myši vygeneruje náhodná data, která se použijí ke generování klíče.
   
    ![hýbání myší okolo](./media/hdinsight-hadoop-linux-use-ssh-windows/movingmouse.png)
   
    Po vygenerování klíče se zobrazí veřejný klíč.

4. Pro dodatečné zabezpečení můžete zadat přístupové heslo do pole **přístupové heslo klíče** a pak zadejte stejnou hodnotu do pole **Potvrdit heslo**.
   
    ![přístupové heslo](./media/hdinsight-hadoop-linux-use-ssh-windows/key.png)
   
   > [!NOTE]
   > Důrazně doporučujeme pro klíč použít zabezpečené přístupové heslo. Pokud však heslo zapomenete, neexistuje žádný způsob, jak jej obnovit.

5. Klikněte na tlačítko **Uložit privátní klíč** a uložte klíč do souboru **.ppk**. Klíč se použije k ověření clusteru HDInsight se systémem Linux.
   
   > [!NOTE]
   > Tento klíč musíte uložit na bezpečném místě, jelikož ho lze použít k přístupu na váš clusteru HDInsight se systémem Linux.

6. Klikněte na tlačítko **Uložit veřejný klíč** a uložte klíč jako soubor **.txt**. Tento postup umožňuje opakovaně v budoucnosti používat veřejný klíč, když vytvoříte další clustery se systémem Linux HDInsight.
   
   > [!NOTE]
   > Veřejný klíč se také zobrazí v horní části PuTTYGen. Můžete kliknout pravým tlačítkem na toto pole, zkopírovat hodnotu a pak ji vložit do formuláře při vytváření clusteru pomocí portálu Azure.

## <a name="create-a-linux-based-hdinsight-cluster"></a>Vytvoření clusteru HDInsight se systémem Linux

Při vytváření clusteru HDInsight se systémem Linux je nutné zadat veřejný klíč, který jste vytvořili dříve. V klientech se systémem Windows existují dva způsoby vytvoření clusteru HDInsight se systémem Linux:

* **Portál Azure** – používá k vytvoření clusteru webový portál.

* **Azure CLI pro Mac, Linux a Windows** – používá příkazy příkazového řádku k vytvoření clusteru.

Každá z těchto metod bude vyžadovat veřejný klíč. Úplné informace týkající se vytvoření clusteru HDInsight se systémem Linux naleznete v části [Clustery HDInsight se systémem Linux](hdinsight-hadoop-provision-linux-clusters.md).

### <a name="azure-portal"></a>Azure Portal

Pokud používáte web [Azure Portal][preview-portal] k vytvoření clusteru HDInsight se systémem Linux, je nutné zadat **Uživatelské jméno SSH** a vybrat, zda chcete zadat **HESLO** nebo **VEŘEJNÝ KLÍČ SSH**.

Pokud vyberete **VEŘEJNÝ KLÍČ SSH**, můžete buď vložit veřejný klíč (zobrazený v části **Veřejný klíč pro vložení do pole klíčů oprávnění\_OpenSSH** v PuttyGen,) do pole **SSH PublicKey** nebo vybrat možnost **Vybrat soubor** a procházet a vybrat soubor, který obsahuje veřejný klíč.

![Obrázek formuláře požadujícího veřejný klíč](./media/hdinsight-hadoop-linux-use-ssh-windows/ssh-key.png)

Tento postup vytvoří přihlášení pro určeného uživatele a umožňuje ověření heslem nebo ověření pomocí klíče SSH.

### <a name="azure-command-line-interface-for-mac-linux-and-windows"></a>Rozhraní příkazového řádku Azure pro Mac, Linux a Windows

[Azure CLI pro Mac, Linux a Windows](../cli-install-nodejs.md) můžete použít k vytvoření nového clusteru pomocí příkazu `azure hdinsight cluster create`.

Další informace o použití tohoto příkazu naleznete v tématu [Vytvoření Hadoop Linux clusterů v nástroji HDInsight pomocí vlastních možností](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="connect-to-a-linux-based-hdinsight-cluster"></a>Připojení ke clusteru HDInsight se systémem Linux

1. Otevřete PuTTY.
   
    ![rozhraní putty](./media/hdinsight-hadoop-linux-use-ssh-windows/putty.png)
2. Pokud jste zadali klíče SSH při vytváření uživatelského účtu, je třeba provést následující krok a vybrat privátní klíč pro použití při ověřování clusteru:
   
    V poli **Kategorie** rozbalte položku **Připojení**, rozbalte položku **SSH** a vyberte možnost **Auth**. Nakonec klikněte na tlačítko **Procházet** a vyberte soubor .ppk, který obsahuje soukromý klíč.
   
    ![rozhraní putty, vyberte privátní klíč](./media/hdinsight-hadoop-linux-use-ssh-windows/puttykey.png)

3. V poli **Kategorie**, vyberte možnost **Relace**. Na obrazovce **Základní možnosti pro relaci PuTTY** zadejte adresu serveru HDInsight v SSH do pole **Název hostitele (nebo IP adresa)**. Existují dvě možné adresy SSH, které můžete použít při připojování ke clusteru:

    ![rozhraní putty se zadanou adresou ssh](./media/hdinsight-hadoop-linux-use-ssh-windows/puttyaddress.png)

    * **Adresa hlavního uzlu**: Chcete-li se připojit k hlavnímu uzlu clusteru, použijte název clusteru následovaný **-ssh.azurehdinsight.net**. Například **mycluster-ssh.azurehdinsight.net**.
   
    * **Adresa okrajového uzlu**: Pokud se připojujete k serveru R na clusteru HDInsight, můžete se připojit k okrajovému uzlu serveru R pomocí adresy **RServer.CLUSTERNAME.ssh.azurehdinsight.net**, kde CLUSTERNAME představuje název clusteru. Například **RServer.mycluster.ssh.azurehdinsight.net**.
     

4. Chcete-li uložit informace o připojení pro budoucí použití, zadejte název tohoto připojení v části **Uložit relací**, a pak klikněte na tlačítko **Uložit**. Připojení se přidá do seznamu uložených relací.
5. Klikněte na tlačítko **Otevřít** a připojte se ke clusteru.
   
   > [!NOTE]
   > Pokud je to poprvé, kdy se připojujete ke clusteru, zobrazí se výstraha zabezpečení. To je normální. Vyberte možnost **Ano** pro ukládání do mezipaměti klíče RSA2 serveru a pokračujte.

6. Po zobrazení výzvy zadejte uživatele, kterého jste zadali při vytvoření clusteru. Pokud jste pro uživatele zadali heslo, budete také vyzváni k jeho zadání.

> [!NOTE]
> Výše uvedené kroky předpokládají, že používáte port 22, který se připojí k primárnímu hlavnímu uzlu v clusteru HDInsight. Použijete-li port 23, připojíte se k sekundárnímu hlavnímu uzlu. Další informace o hlavních uzlech naleznete v tématu [Dostupnost a spolehlivost Hadoop clusterů v HDInsight](hdinsight-high-availability-linux.md).

### <a name="connect-to-worker-nodes"></a>Připojování k pracovním uzlům

Pracovní uzly nejsou přímo dostupné z oblasti mimo datové centrum Azure, ale jsou dostupné z hlavního uzlu clusteru pomocí protokolu SSH.

Pokud jste zadali klíč SSH při vytváření uživatelského účtu, musíte provést následující kroky a použít privátní klíč pro ověřování do clusteru, pokud se chcete připojit k pracovním uzlům.

1. Nainstalujte Pageant z adresy [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). Tento nástroj se používá pro ukládání klíčů SSH pro PuTTY do mezipaměti.

2. Spusťte modul Pageant. Minimalizuje se na ikonu na stavovém panelu. Klikněte pravým tlačítkem myši na ikonu a vyberte možnost **Přidat klíč**.
   
    ![přidání klíče](./media/hdinsight-hadoop-linux-use-ssh-windows/addkey.png)

3. Jakmile se zobrazí dialogové okno Procházet, vyberte soubor .ppk, který obsahuje klíč, a pak klikněte na tlačítko **Otevřít**. Tento postup přidá klíč do modulu Pageant, který ho poskytne PuTTY při připojování ke clusteru.
   
   > [!IMPORTANT]
   > Pokud jste použili klíče SSH k zabezpečení účtu, musíte dokončit předchozí postup, než bude možné se připojit k pracovním uzlům.

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
    
    > [!NOTE]
    > Pokud použijete heslo k ověřování relace SSH, zobrazí se výzva k opětovnému zadání hesla. Pokud používáte klíč SSH, je třeba připojení dokončit bez zobrazování výzev.

9. Po vytvoření relace se řádek pro relaci PuTTY změní z `username@hn#-clustername` na `username@wn#-clustername`, což značí, že jste připojeni k pracovnímu uzlu. Všechny příkazy, které spustíte v tomto okamžiku, bude spouštěny v pracovním uzlu.

10. Po dokončení provádění akcí v pracovním uzlu použijte příkaz `exit` k ukončení relace k uzlu pracovníka. Tím se vrátíte do řádku `username@hn#-clustername`.

## <a name="add-more-accounts"></a>Přidat další účty

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

## <a id="tunnel"></a>Tunelování SSH

SSH lze použít k tunelování místních požadavků, například webových požadavků, do clusteru HDInsight. Požadavek bude poté směrován na požadovaný prostředek, jako kdyby pocházel z hlavního uzlu clusteru HDInsight.

> [!IMPORTANT]
> Tunelové propojení SSH představuje požadavek pro přístup k webu uživatelského rozhraní pro některé služby Hadoop. Například uživatelské rozhraní Historie úloh nebo uživatelské rozhraní Správce prostředků lze otevřít pouze pomocí tunelového propojení SSH.

Další informace o vytváření a používání tunelového propojení SSH naleznete v tématu [Používání tunelového propojení SSH pro přístup k webovému uživatelskému rozhraní Ambari, ResourceManager, JobHistory, NameNode, Oozie a jiným webovým uživatelským rozhraním](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="next-steps"></a>Další kroky

Teď, když chápete, jak provádět ověření pomocí klíče SSH se dozvíte další informace o používání nástroje MapReduce s Hadoop v HDInsight.

* [Použití Hivu se službou HDInsight](hdinsight-use-hive.md)
* [Použití Pigu se službou HDInsight](hdinsight-use-pig.md)
* [Použití úloh MapReduce se službou HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/

