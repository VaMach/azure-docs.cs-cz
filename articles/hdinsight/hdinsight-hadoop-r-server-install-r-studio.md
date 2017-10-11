---
title: Nainstalujte Rstudia s R serverem v HDInsight - Azure | Microsoft Docs
description: Postup instalace Rstudia s R serverem v HDInsight.
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 918abb0d-8248-4bc5-98dc-089c0e007d49
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: 416420d855505508735ebd8526e93efdb230ad53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="installing-rstudio-with-r-server-on-hdinsight"></a>Instalace Rstudia s R serverem v HDInsight

Tento článek popisuje postup instalace community (zdarma) verze [Rstudia Server](https://www.rstudio.com/products/rstudio-server/) na hraničního uzlu clusteru pomocí vlastního skriptu. Rstudia Server poskytuje IDE se založené na prohlížeči pro vzdálené klienty a se často používá v systému Linux. Existuje více integrované vývojové prostředí (integrovaného vývojového prostředí) k dispozici pro R dnes, včetně:

- Společnosti Microsoft [R Tools pro Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS) 
- [Rstudia serveru](https://www.rstudio.com/products/rstudio-server/) 
- Walware je na základě Eclipse [StatET](http://www.walware.de/goto/statet).

Výhodou nainstalovat Server Rstudia hraničního uzlu clusteru HDInsight je, že zajišťuje úplné prostředí IDE pro vývoj a spouštění skriptů R s R Server v clusteru. Tato konfigurace může být výrazně zvýšit produktivitu, než výchozí použití R konzoly.

> [!NOTE]
> Postup popsaný v tomto článku je pouze v případě, že jste nevybrali pro instalaci edice community Rstudia serveru při zřizování clusteru. Pokud jste přidali při zřizování, pak pro přístup k ní kliknete na **řídicí panely serveru R** v položce portál Azure pro váš cluster, klikněte na dlaždici **R Studio Server** dlaždici. 

Pokud byste radši chtěli použít komerčně licencovanou verzi Pro Rstudia serveru, postupujte podle pokynů pro instalaci z [Rstudia Server](https://www.rstudio.com/products/rstudio/download-server/).

> [!NOTE]
> Pokud používáte cluster služby HDInsight, pro který byla nainstalována pomocí R [nainstalovat akce skriptu R](hdinsight-hadoop-r-scripts-linux.md), kroky v tomto dokumentu nebude fungovat správně, protože vyžadují serveru R na clusteru HDInsight.
>
> 

## <a name="prerequisites"></a>Požadavky

* Cluster Azure HDInsight s nainstalovaným serverem R. Pokyny najdete v tématu [začít pracovat s R Server v clusterech HDInsight](hdinsight-hadoop-r-server-get-started.md).
* Klientem SSH. Pro distribuce operačních systémů Linux a Unix nebo OS X Macintosh `ssh` příkaz je součástí operačního systému. Pro systém Windows, doporučujeme, abyste [emulaci](http://www.redhat.com/services/custom/cygwin/) s [OpenSSH možnost](https://www.youtube.com/watch?v=CwYSvvGaiWU), nebo [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  

## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a>Nainstalujte Rstudia na cluster pomocí vlastního skriptu

Postup je následující:

1. Identifikujte hraničního uzlu clusteru. Pro cluster služby HDInsight s R Server následuje zásady vytváření názvů pro hlavní uzel a hraniční uzel.
   * Hlavní uzel`CLUSTERNAME-ssh.azurehdinsight.net`
   * Hraniční uzel`CLUSTERNAME-ed-ssh.azurehdinsight.net` 

2. SSH do hraničního uzlu clusteru pomocí vzoru pro pojmenovávání zadané v kroku 1. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Jakmile se připojíte, stát kořenovou uživatel v clusteru. V této relaci SSH použijte následující příkaz:

        sudo su -

4. Stáhněte si vlastní skript k instalaci Rstudia. Použijte následující příkaz:

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Změna oprávnění u souboru vlastní skript a spusťte skript. Použijte následující příkazy:

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. Pokud jste použili heslo k SSH při vytváření clusteru HDInsight s R Server, můžete tento krok přeskočit a přejít na další. Pokud jste použili klíče SSH místo toho k vytvoření clusteru, musíte nastavit heslo pro vaše uživatele SSH. Toto heslo je nutné při připojování k Rstudia. Spusťte následující příkazy:

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:


7. Po zobrazení výzvy k **aktuální Kerberos heslo**, stiskněte klávesu **ENTER**.  Všimněte si, že je třeba nahradit `USERNAME` s uživatelem SSH pro váš cluster HDInsight. Pokud je heslo úspěšně nastavená, zobrazí se následující zpráva:

        passwd: password updated successfully

    Ukončení relace SSH.

8. Vytvoření tunelu SSH do clusteru pomocí mapování `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` v clusteru HDInsight na klientský počítač. Tunelové propojení SSH je třeba vytvořit před otevřením novou relaci prohlížeče.

   * U klienta Linux nebo klienta Windows s [emulaci](http://www.redhat.com/services/custom/cygwin/), otevřete relaci terminálu a zadejte následující příkaz:

             ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

       Nahraďte **uživatelské jméno** s uživatelem SSH pro váš HDInsight cluster a nahraďte **CLUSTERNAME** s názvem clusteru HDInsight.
       Můžete také použít klíč SSH, nikoli heslo přidáním `-i id_rsa_key`.        
   * Pokud klient systému Windows pomocí klienta PuTTY pak

     1. Otevřete PuTTY a zadejte informace o připojení.
     2. V **kategorie** nalevo dialogového okna, rozbalte položku **připojení**, rozbalte položku **SSH**a potom vyberte **tunely**.
     3. Zadejte následující informace na **možnosti řízení SSH port předávání** formuláře:

        * **Zdrojový port** – port na straně klienta, který chcete přesměrovat. Například **8787**.
        * **Cílový** -na cíl, který musí být mapován na místním klientském počítači. Například **localhost:8787**.

            ![Vytvoření tunelu SSH](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "vytvoření tunelu SSH")

     4. Klikněte na tlačítko **přidat** přidejte nastavení a potom klikněte na **otevřete** otevřít připojení SSH.
     5. Pokud budete vyzváni, přihlaste se k serveru k zahájení relace SSH a povolte tunelové propojení.

9. Otevřete webový prohlížeč a zadejte následující adresu URL na port, které jste zadali pro tunelové propojení:

        http://localhost:8787/ 

10. Zobrazí se výzva k zadání SSH uživatelské jméno a heslo pro připojení ke clusteru. Pokud jste použili klíč SSH při vytváření clusteru, je nutné zadat heslo, které jste vytvořili v kroku 5.

    ![Připojení k R Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "vytvoření tunelu SSH")

11. K ověření, zda Rstudia instalace byla úspěšná, můžete spustit test skript, který spouští úlohy MapReduce a Spark na základě R na clusteru. Chcete-li stáhnout zkušební skript spustit v Rstudia, přejděte zpět ke konzole SSH a zadejte následující příkazy:

    *    Pokud jste vytvořili clusteru Hadoop s R, použijte tento příkaz:

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r
    *    Když vytvoříte Spark cluster s R, použijte tento příkaz:

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

12. V Rstudia najdete v části testovací skript, který jste stáhli. Poklikáním soubor otevřete, vyberte obsah souboru a pak klikněte na tlačítko **spustit**. Měli byste vidět výstup v **konzoly** podokně:

   ![Testování instalace](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "testování instalace")

Další možností je `source(testhdi.r)` nebo `source(testhdi_spark.r)` se spustit skript.

## <a name="see-also"></a>Viz také

* [Výpočetní kontextu možnosti pro R Server v clusterech prostředí HDInsight](hdinsight-hadoop-r-server-compute-contexts.md)
* [Možnosti služby Azure Storage pro R Server ve službě HDInsight](hdinsight-hadoop-r-server-storage.md)

