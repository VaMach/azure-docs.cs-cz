---
title: "Použití klíče SSH pro HDInsight (Hadoop) z Windows, Linuxu, Unixu nebo OS X | Dokumentace Microsoftu"
description: " Pro přístup k systému HDInsight můžete použít Secure Shell (SSH). Tento dokument obsahuje informace o používání SSH pro přístup k systému HDInsight z klientů s Windows, Linuxem, Unixem nebo OS X.."
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
ms.date: 01/12/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 279990a67ae260b09d056fd84a12160150eb4539
ms.openlocfilehash: 37409ad3f50cdd4a7a384c96a57a35ef8c83fb8f


---
# <a name="use-ssh-with-hdinsight-hadoop-from-windows-linux-unix-or-os-x"></a>Použití klíče SSH pro HDInsight (Hadoop) z Windows, Linuxu, Unixu nebo OS X

> [!div class="op_single_selector"]
> * [PuTTY (Windows)](hdinsight-hadoop-linux-use-ssh-windows.md)
> * [SSH (Windows, Linux, Unix, OS X)](hdinsight-hadoop-linux-use-ssh-unix.md)

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) umožňuje přihlašovat se ke clusteru služby HDInsight v Linuxu a spouštět příkazy pomocí rozhraní příkazového řádku. Tento dokument poskytuje základní informace o SSH a konkrétní informace o použití SSH s HDInsight.

## <a name="what-is-ssh"></a>Co je SSH?

SSH je kryptografický síťový protokol, který umožňuje zabezpečeně komunikovat se vzdáleným serverem přes nezabezpečenou síť. SSH slouží k zajištění zabezpečeného přihlášení ke vzdálenému serveru prostřednictvím příkazového řádku. V tomto případě k hlavním uzlům nebo k hraničnímu uzlu clusteru HDInsight.

SSH můžete také využít k tunelování síťového provozu z vašeho klienta do clusteru HDInsight. Použití tunelu umožňuje přístup k těm službám v clusteru HDInsight, které nejsou přímo zveřejněné na internetu. Další informace o použití tunelování SSH s HDInsight najdete v tématu [Použití tunelování SSH s HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="ssh-clients"></a>Klienti SSH

Řada operačních systémů poskytuje funkce klienta SSH prostřednictvím nástrojů příkazového řádku `ssh` a `scp`.

* __ssh__: Obecný klient SSH, kterého je možné využít k navázání vzdálených relací příkazového řádku a vytváření tunelů.
* __scp__: Nástroj, který kopíruje soubory mezi místními a vzdálenými systémy s využitím protokolu SSH.

Systém Windows neposkytoval klienta SSH až do verze Windows 10 Anniversary Edition. Tato verze systému Windows zahrnuje funkci Bash on Windows 10 pro vývojáře, která poskytuje `ssh`, `scp` a další linuxové příkazy. Další informace o použití funkce Bash on Windows 10 najdete v tématu [Bash na Ubuntu ve Windows](https://msdn.microsoft.com/commandline/wsl/about).

Pokud používáte Windows a nemáte k funkci Bash on Windows 10 přístup, doporučujeme následující klienty SSH:

* [Git pro Windows](https://git-for-windows.github.io/): Poskytuje nástroje příkazového řádku `ssh` a `scp`.
* [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/): Poskytuje grafického klienta SSH.
* [MobaXterm](http://mobaxterm.mobatek.net/): Poskytuje grafického klienta SSH.
* [Cygwin](https://cygwin.com/): Poskytuje nástroje příkazového řádku `ssh` a `scp`.

> [!NOTE]
> Kroky v této dokumentaci předpokládají, že máte přístup k příkazu `ssh`. Pokud používáte klienta, jako je puTTY nebo MobaXterm, najdete ekvivalentní příkaz a parametry v dokumentaci k tomuto produktu.

## <a name="ssh-authentication"></a>Ověřování SSH

Připojení SSH je možné ověřit pomocí hesla nebo pomocí [kryptografie využívající veřejný klíč (https://en.wikipedia.org/wiki/Public-key_cryptography)](https://en.wikipedia.org/wiki/Public-key_cryptography). Použití klíče je nejbezpečnější, protože tato metoda není zranitelná vůči tolika útokům jako hesla. Vytvoření a správa klíčů je ale složitější než použití hesla.

Použití kryptografie využívající veřejného klíče vyžaduje vytvoření páru _veřejného_ a _privátního_ klíče.

* **Veřejný klíč** se načte do uzlů clusteru HDInsight nebo libovolné jiné služby, pro kterou chcete kryptografii využívající veřejného klíče použít.

* **Privátní klíč** je to, co se předkládá clusteru HDInsight při přihlášení pomocí klienta SSH, aby se ověřila vaše identita. Chraňte tento privátní klíč. Nesdílejte ho.

    Další úrovně zabezpečení můžete dosáhnout, pokud pro privátní klíč vytvoříte přístupové heslo. Toto heslo musíte zadat dřív, než je možné klíč použít.

### <a name="create-a-public-and-private-key"></a>Vytvoření veřejného a privátního klíče

Nástroj `ssh-keygen` představuje nejjednodušší způsob vytvoření páru veřejného a privátního klíče určeného pro použití s HDInsight. K vytvoření páru veřejného a privátního klíče určeného pro použití s HDInsight z příkazového řádku použijte tento příkaz:

> [!NOTE]
> Pokud používáte klienta SSH s grafickým uživatelským rozhraním, jako je MobaXTerm nebo puTTY, vyhledejte si popis vytvoření klíčů v dokumentaci k tomuto klientovi.

    ssh-keygen -t rsa -b 2048

Budete vyzváni k zadání následujících informací:

* Umístění souboru: Výchozí umístění je `~/.ssh/id_rsa`.

* Volitelné přístupové heslo: Pokud zadáte přístupové heslo, musíte ho při ověřování v clusteru HDInsight zadat znova.

> [!IMPORTANT]
> Přístupové heslo je heslem privátního klíče. Když privátní klíč použijete k ověřování, musíte nejdřív zadat přístupové heslo. Teprve potom je možné klíč použít. Pokud někdo získá váš privátní klíč, bez přístupového hesla ho nemůže použít.
>
> Pokud ale heslo zapomenete, neexistuje žádný způsob, jak ho resetovat nebo obnovit.

Po dokončení příkazu budete mít dva nové soubory:

* __id\_rsa__: Tento soubor obsahuje privátní klíč.

    > [!WARNING]
    > Přístup k tomuto souboru musíte omezit, abyste zabránili neoprávněnému přístupu ke službám zabezpečeným pomocí veřejného klíče.

* __id\_rsa.pub__: Tento soubor obsahuje veřejný klíč. Můžete ho využít při vytváření clusteru HDInsght.

    > [!NOTE]
    > Na tom, kdo má přístup k _veřejnému_ klíči, nezáleží. Veřejný klíč slouží sám o sobě jenom k ověření privátního klíče. Služby, jako je server SSH, využívají veřejný klíč k ověření vaší identity při ověřování pomocí privátního klíče.

## <a name="configure-ssh-on-hdinsight"></a>Konfigurace SSH v HDInsight

Když vytváříte linuxový cluster HDInsight, musíte poskytnout _uživatelské jméno SSH_ a _heslo_ nebo _veřejný klíč_. Během vytváření clusteru se tyto informace využijí k tomu, aby se vytvořila přihlášení k uzlům clusteru HDInsight. Heslo nebo veřejný klíč slouží k zabezpečení uživatelského účtu.

Další informace o konfiguraci SSH při vytváření clusteru najdete v těchto dokumentech:

* [Vytvoření HDInsight pomocí webu Azure Portal](hdinsight-hadoop-create-linux-clusters-portal.md)
* [Vytvoření HDInsight pomocí rozhraní příkazového řádku Azure](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
* [Vytvoření HDInsight pomocí Azure PowerShellu](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [Vytvoření HDInsight pomocí šablon Azure Resource Manageru](hdinsight-hadoop-create-linux-clusters-arm-templates.md)
* [Vytvoření HDInsight pomocí sady .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)
* [Vytvoření HDInsight pomocí REST](hdinsight-hadoop-create-linux-clusters-curl-rest.md)

### <a name="additional-ssh-users"></a>Další uživatelé SSH

Přestože je možné po vytvoření přidat do clusteru další uživatele SSH, nedoporučuje se to.

* Nové uživatele SSH byste museli ručně přidat do každého uzlu v clusteru.

* Noví uživatelé SSH mají k HDInsight stejný přístup jako výchozí uživatel. Neexistuje žádný způsob, jak omezit přístup k datům nebo úlohám v HDInsight na základě uživatelského účtu SSH.

Pokud chcete omezit přístup pro jednotlivé uživatele, musíte použít cluster HDInsight připojený k doméně. HDInsight připojený k doméně používá k řízení přístupu k prostředkům clusteru služby Active Directory.

Použití clusteru HDInsight připojeného k doméně umožňuje ověřovat pomocí služby Active Directory po připojení pomocí protokolu SSH. Pomocí SSH se může připojit několik uživatelů, kteří se po připojení ověří pomocí účtu Active Directory. Další informace najdete v tématu [HDInsight připojený k doméně](#domainjoined).

##<a name="a-idconnecta-connect-to-hdinsight"></a><a id="connect"></a> Připojení k HDInsight

Přestože je ve všech uzlech v clusteru HDInsight spuštěný server SSH, prostřednictvím veřejného internetu se můžete připojit jenom k hlavním nebo hraničním uzlům.

* Pro připojení k _hlavním uzlům_ použijte `CLUSTERNAME-ssh.azurehdinsight.net`, kde __CLUSTERNAME__ je název clusteru HDInsight. Připojením na portu 22 (výchozí pro SSH) se připojíte k primárnímu hlavnímu uzlu. Port 23 se připojuje k sekundárnímu hlavnímu uzlu.

* Pro připojení k _hraničnímu uzlu_ použijte `EDGENAME.CLUSTERNAME-ssh.azurehdinsight.net`, kde __EDGENAME__ je název hraničního uzlu a __CLUSTERNAME__ je název clusteru HDInsight. Při připojování k hraničnímu uzlu použijte port 22.

Následující příkazy ukazují, jak se připojit k hlavním a hraničním uzlům clusteru s názvem __myhdi__ pomocí uživatelského jména SSH nazvaného __sshuser__. Hraniční uzel má název __myedge__.

| Požadovaná akce... | Postup... |
| ----- | ----- |
| Připojení k primárnímu hlavnímu uzlu | `ssh sshuser@myhdi-ssh.azurehdinsight.net` |
| Připojení k sekundárnímu hlavnímu uzlu | `ssh -p 23 sshuser@myhdi-ssh.azurehdinsight.net` |
| Připojení k hraničnímu uzlu | `ssh sshuser@edge.myhdi-ssh.azurehdinsight.net` |

Pokud k zabezpečení účtu SSH používáte heslo, budete vyzváni k jeho zadání.

Pokud k zabezpečení účtu SSH používáte veřejný klíč, bude možná potřeba pomocí přepínače `-i` zadat cestu k odpovídajícímu privátnímu klíči. Použití přepínače `-i` ukazuje následující příklad:

    ssh -i /path/to/public.key sshuser@myhdi-ssh.azurehdinsight.net

### <a name="connect-to-other-nodes"></a>Připojení k ostatním uzlům

Pracovní uzly a uzly Zookeeper nejsou přímo dostupné z oblasti mimo cluster, ale je k nim možné získat přístup z hlavních nebo hraničních uzlů clusteru. Slouží k tomu tento obecný postup:

1. Pomocí SSH se připojte k hlavnímu nebo hraničnímu uzlu:

        ssh sshuser@myhdi-ssh.azurehdinsight.net

2. V rámci připojení SSH k hlavnímu nebo hraničnímu uzlu použijte příkaz `ssh` pro připojení k pracovnímu uzlu v clusteru:

        ssh sshuser@wn0-myhdi

    Pokud chcete načíst seznam pracovních uzlů v clusteru, příklad toho, jak načíst plně kvalifikovaný název domény uzlů clusteru, najdete v tématu [Správa HDInsight pomocí rozhraní Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes).

Pokud je účet SSH zabezpečený pomocí hesla, budete vyzváni k jeho zadání a připojení se naváže.

Pokud k ověření uživatelského účtu používáte klíč SSH, musíte ověřit, že v místním prostředí je nakonfigurované přesměrování agenta SSH.

> [!IMPORTANT]
> V následujících krocích se předpokládá využití systému založeného na Linuxu/UNIXu a funkce Bash on Windows 10. Pokud tyto kroky pro váš systém nefungují, možná budete nahlédnout do dokumentace k příslušnému klientovi SSH.

1. V textovém editoru otevřete `~/.ssh/config`. Pokud tento soubor neexistuje, můžete ho vytvořit tak, že na příkazovém řádku zadáte `touch ~/.ssh/config`.

2. Do souboru přidejte následující: Nahraďte *CLUSTERNAME* názvem clusteru HDInsight.

        Host CLUSTERNAME-ssh.azurehdinsight.net
          ForwardAgent yes

    Tato položka nakonfiguruje přesměrování agenta SSH pro váš cluster HDInsight.

3. Proveďte test přesměrování agenta SSH pomocí následujícího příkazu z terminálu:

        echo "$SSH_AUTH_SOCK"

    Tento příkaz by měl vrátit informace podobné následujícímu textu:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Když se nic nevrátí, znamená to, že nástroj `ssh-agent` není spuštěný. Přečtěte si informace o spouštěcích skriptech agenta v tématu [Použití nástroje ssh-agent s SSH (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) nebo v dokumentaci ke klientovi SSH vyhledejte konkrétní kroky k instalaci a konfiguraci `ssh-agent`.

4. Jakmile si ověříte, že  se **ssh-agent** spustil, podle následujícího postupu přidejte svůj privátní klíč SSH k agentovi:

        ssh-add ~/.ssh/id_rsa

    Pokud je privátní klíč uložen v jiném souboru, nahraďte `~/.ssh/id_rsa` cestou k souboru.

<a id="domainjoined"></a>
### <a name="domain-joined-hdinsight"></a>HDInsight připojený k doméně

[HDInsight s připojením k doméně](hdinsight-domain-joined-introduction.md) integruje protokol Kerberos s platformou Hadoop ve službě HDInsight. Protože uživatel SSH není uživatelem domény služby Active Directory, můžete spouštět příkazy Hadoop až po ověření pomocí služby Active Directory. K ověření relace SSH pomocí Active Directory použijte následující kroky:

1. Cluster HDInsight připojený k doméně připojte pomocí SSH, jak je uvedené v části [Připojení k HDInsight](#connect). Následující příkaz se například připojí ke clusteru HDInsight s názvem __myhdi__ pomocí uživatelského účtu SSH nazvaného __sshuser__.

        ssh sshuser@myhdi-ssh.azurehdinsight.net

2. K ověření pomocí hesla a uživatele domény použijte tento postup:

        kinit

     Po zobrazení výzvy zadejte uživatelské jméno domény a heslo pro uživatele domény.

    Další informace o konfiguraci uživatelů domény pro clustery HDInsight připojené k doméně najdete v tématu [Konfigurace clusterů HDInisight připojených do domény](hdinsight-domain-joined-configure.md).

Po ověření pomocí příkazu `kinit` můžete využít příkazy Hadoop, jako je `hdfs dfs -ls /` nebo `hive`.

## <a name="a-idtunnelassh-tunneling"></a><a id="tunnel"></a>Tunelování SSH

SSH lze použít k tunelování místních požadavků, například webových požadavků, do clusteru HDInsight. Požadavek bude poté směrován na požadovaný prostředek, jako kdyby pocházel z hlavního uzlu clusteru HDInsight.

> [!IMPORTANT]
> Tunelové propojení SSH představuje požadavek pro přístup k webu uživatelského rozhraní pro některé služby Hadoop. Například uživatelské rozhraní Historie úloh nebo uživatelské rozhraní Správce prostředků lze otevřít pouze pomocí tunelového propojení SSH.

Další informace o vytváření a používání tunelu SSH najdete v tématu [Používání tunelu propojení SSH pro přístup k webovému uživatelskému rozhraní Ambari, JobHistory, NameNode, Oozie a dalším webovým uživatelským rozhraním](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="next-steps"></a>Další kroky

Teď, když chápete, jak provádět ověření pomocí klíče SSH se dozvíte další informace o používání nástroje MapReduce s Hadoop v HDInsight.

* [Použití Hivu se službou HDInsight](hdinsight-use-hive.md)
* [Použití Pigu se službou HDInsight](hdinsight-use-pig.md)
* [Použití úloh MapReduce se službou HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/



<!--HONumber=Feb17_HO3-->


