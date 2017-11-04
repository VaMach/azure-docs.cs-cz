---
title: "Použití Apache Phoenix a SQuirreL s Azure HDInsight se systémem Windows | Microsoft Docs"
description: "Zjistěte, jak používat Apache Phoenix v HDInsight a jak nainstalovat a nakonfigurovat SQuirreL na pracovní stanici pro připojení k cluster HBase v HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1a756e98-75c1-44cd-a178-c5119683b7b7
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 64a4c5b158ebe0119f2f0133587a743fd2dbf0ff
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="use-apache-phoenix-and-squirrel-with-windows-based-hbase-clusters-in-hdinsight"></a>Použití nástrojů Apache Phoenix a SQuirreL s clustery založené na Windows HBase v HDInsight
Další informace o použití [Apache Phoenix](http://phoenix.apache.org/) v HDInsight a jak nainstalovat a nakonfigurovat SQuirreL na pracovní stanici pro připojení k cluster HBase v HDInsight. Další informace o Phoenix najdete v tématu [Phoenix za 15 minut nebo méně](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Gramatika Phoenix, najdete v části [Phoenix gramatika](http://phoenix.apache.org/language/index.html).

> [!NOTE]
> Informace o verzi Phoenix v HDInsight, naleznete v části [co je nového ve verzích clusterů systému Hadoop poskytovaných prostředím HDInsight?](hdinsight-component-versioning.md).
>

> [!IMPORTANT]
> Kroky v tomto dokumentu fungovat pouze pro clustery HDInsight se systémem Windows. HDInsight je k dispozici pouze v systému Windows verze nižší než HDInsight 3.4. HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Informace o používání Phoenix na HDInsight se systémem Linux najdete v tématu [clusterů použití nástrojů Apache Phoenix se systémem Linux HBase v HDInsight](hbase/apache-hbase-phoenix-squirrel-linux.md).
>



## <a name="use-sqlline"></a>Použití SQLLine
[SQLLine](http://sqlline.sourceforge.net/) je nástroj příkazového řádku pro provedení SQL.

### <a name="prerequisites"></a>Požadavky
Než budete moci použít SQLLine, musíte mít následující:

* **Cluster HBase v HDInsight**. Informace o zřizování HBase clusteru najdete v tématu [začít pracovat s Apache HBase v HDInsight][hdinsight-hbase-get-started].
* **Připojte se ke clusteru HBase pomocí protokolu vzdálené plochy**. Pokyny najdete v tématu [Správa clusterů systému Hadoop v HDInsight pomocí portálu Azure Classic][hdinsight-manage-portal].

**Chcete-li zjistit název hostitele**

1. Otevřete **Hadoop příkazového řádku** z plochy.
2. Spusťte následující příkaz k získání příponu DNS:

        ipconfig

    Zapište **příponu DNS specifickou pro připojení**. Například *myhbasecluster.f5.internal.cloudapp.net*. Jakmile se připojíte ke clusteru HBase, musíte připojit k Zookeepers pomocí plně kvalifikovaného názvu domény. Každý cluster HDInsight má 3 Zookeepers. Jsou *zookeeper0*, *zookeeper1*, a *zookeeper2*. Plně kvalifikovaný název domény se něco podobného jako *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.

**Chcete-li použít SQLLine**

1. Otevřete **Hadoop příkazového řádku** z plochy.
2. Spusťte následující příkazy otevřete SQLLine:

        cd %phoenix_home%\bin
        sqlline.py [The FQDN of one of the Zookeepers]

    ![HDInsight hbase phoenix sqlline][hdinsight-hbase-phoenix-sqlline]

    Funkce použité v ukázce:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

Další informace najdete v tématu [SQLLine ruční](http://sqlline.sourceforge.net/#manual) a [Phoenix gramatika](http://phoenix.apache.org/language/index.html).

## <a name="use-squirrel"></a>Použití SQuirreL
[SQuirreL klienta SQL](http://squirrel-sql.sourceforge.net/) je grafické Java program, který vám umožní zobrazit struktura databáze kompatibilní JDBC, procházet data v tabulkách, vydávat příkazy SQL atd. Slouží k připojení k serveru Apache Phoenix v HDInsight.

V této části se dozvíte, jak nainstalovat a nakonfigurovat SQuirreL na pracovní stanici se připojit ke clusteru HBase v HDInsight prostřednictvím sítě VPN.

### <a name="prerequisites"></a>Požadavky
Než budete postupovat podle pokynů, musí mít následující:

* Cluster HBase nasadit do virtuální sítě Azure s virtuálním počítačem DNS.  Pokyny najdete v tématu [vytvořte HBase clusterů ve virtuální síti Azure][hdinsight-hbase-provision-vnet].

* Získáte přípona DNS podle připojení clusteru cluster HBase. Chcete-li ho RDP do clusteru a poté spusťte příkaz IPConfig.  Přípona DNS je stejný jako:

        myhbase.b7.internal.cloudapp.net
* Stáhněte a nainstalujte [Microsoft Visual Studio Express pro Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) na pracovní stanici. Budete potřebovat makecert z balíček, který chcete vytvořit svůj certifikát.  
* Stáhněte a nainstalujte [prostředí Java Runtime](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) na pracovní stanici.  SQuirreL SQL klientská verze 3.0 nebo vyšší vyžaduje prostředí JRE 1.6 nebo vyšší verze.  

### <a name="configure-a-point-to-site-vpn-connection-to-the-azure-virtual-network"></a>Konfigurace připojení typu Point-to-Site VPN do virtuální síť Azure
Související se situací konfiguraci připojení point-to-site VPN jsou 3 kroky:

1. [Konfigurace virtuální sítě a brány dynamického směrování](#Configure-a-virtual-network-and-a-dynamic-routing-gateway)
2. [Vytvoření certifikáty](#Create-your-certificates)
3. [Konfigurace klienta VPN](#Configure-your-VPN-client)

V tématu [konfigurace připojení typu Point-to-Site VPN k virtuální síti Azure](../vpn-gateway/vpn-gateway-point-to-site-create.md) Další informace.

#### <a name="configure-a-virtual-network-and-a-dynamic-routing-gateway"></a>Konfigurace virtuální sítě a brány dynamického směrování
Zajistit, když máte zřízenou cluster HBase v virtuální sítě Azure (viz požadavky na této části). Dalším krokem je konfigurace připojení typu point-to-site.

**Ke konfiguraci připojení point-to-site**

1. Přihlaste se k [portál Azure Classic][azure-portal].
2. Na levé straně klikněte na tlačítko **sítě**.
3. Klikněte na virtuální síť, které jste vytvořili (viz [HBase zřizování clusterů ve virtuální síti Azure][hdinsight-hbase-provision-vnet]).
4. Klikněte na tlačítko **konfigurace** shora.
5. V **připojení point-to-site** vyberte **konfiguraci připojení point-to-site**.
6. Konfigurace **počáteční IP** a **CIDR** k určení rozsahu IP adres, ze kterého klienti sítě VPN obdrží IP adresu při připojení. Rozsah se nesmí překrývat s žádným z rozsahů ve vaší místní sítí a virtuální síť Azure, které se budou připojovat k. Například. Pokud jste vybrali 10.0.0.0/20 pro virtuální síť, můžete vybrat 10.1.0.0/24 pro klienta adresní prostor. Najdete v článku [připojeníPoint-To-Site] [ vnet-point-to-site-connectivity] stránka Další informace.
7. V části prostory adres virtuální sítě, klikněte na tlačítko **přidat podsíť brány**.
8. Klikněte na tlačítko **Uložit** v dolní části stránky.
9. Klikněte na tlačítko **Ano** pro potvrzení změny. Počkejte, dokud systém dokončil provádění změn, než budete pokračovat k dalšímu postupu.

**K vytvoření brány dynamického směrování**

1. Z klasického portálu Azure klikněte na tlačítko **řídicí panel** z horní části stránky.
2. Klikněte na tlačítko **vytvořit BRÁNU** v dolní části stránky.
3. Klikněte na tlačítko **Ano** k potvrzení. Počkejte na vytvoření brány.
4. Klikněte na tlačítko **řídicí panel** shora.  Zobrazí se visual diagram virtuální sítě:

    ![Virtuální diagram point-to-site virtuální síť Azure][img-vnet-diagram]

    Diagram znázorňuje 0 připojení klientů. Když provedete připojení k virtuální síti, číslo se aktualizují na jednu.

#### <a name="create-your-certificates"></a>Vytvoření certifikáty
Jeden způsob, jak vytvořit certifikát X.509 je pomocí nástroje vytvoření certifikátu (makecert.exe), která se dodává s [Microsoft Visual Studio Express pro Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).

**Vytvořit certifikát podepsaný svým držitelem kořenové**

1. Pracovní stanice otevřete okno příkazového řádku.
2. Přejděte do složky nástroje Visual Studio.
3. Následující příkaz v následujícím příkladu vytvoříte a instalace kořenového certifikátu v úložišti osobních certifikátů na pracovní stanici a také vytvořit odpovídající soubor .cer, který později nahrajete do portálu Azure Classic.

        makecert -sky exchange -r -n "CN=HBaseVnetVPNRootCertificate" -pe -a sha1 -len 2048 -ss My "C:\Users\JohnDole\Desktop\HBaseVNetVPNRootCertificate.cer"

    Přejděte do adresáře, který chcete soubor .cer umístěný v, kde HBaseVnetVPNRootCertificate je název, který chcete použít pro certifikát.

    Nemáte zavřete příkazový řádek.  Budete ho potřebovat v dalším postupu.

   > [!NOTE]
   > Vzhledem k tomu, že jste vytvořili kořenový certifikát, ze kterého se budou generovat klientské certifikáty, měli byste tento certifikát exportovat i s jeho privátním klíčem, a uložit jej na bezpečné místo, odkud ho bude možné obnovit.
   >
   >

**Vytvoření certifikátu klienta**

* Při použití stejné příkazového řádku (musí být na stejném počítači, kde jste vytvořili kořenový certifikát. Klientský certifikát musí být vygenerovaný z kořenového certifikátu), spusťte následující příkaz:

          makecert.exe -n "CN=HBaseVnetVPNClientCertificate" -pe -sky exchange -m 96 -ss My -in "HBaseVnetVPNRootCertificate" -is my -a sha1

    HBaseVnetVPNRootCertificate je název kořenového certifikátu.  Musí odpovídat názvu kořenový certifikát.  

    Kořenový certifikát a certifikát klienta jsou uložené v úložišti osobních certifikátů v počítači. Certmgr.msc použijte k ověření.

    ![Certifikát sítě VPN point-to-site virtuální síť Azure][img-certificate]

    Klientský certifikát musí být nainstalován na každém počítači, který chcete připojit k virtuální síti. Doporučujeme vám vytvořit jedinečný klientský certifikát pro každý počítač, který chcete připojit k virtuální síti. Chcete-li exportovat klientské certifikáty, použijte certmgr.msc.

**Nahrát kořenový certifikát na portálu Azure Classic**

1. Z klasického portálu Azure klikněte na tlačítko **sítě** na levé straně.
2. Klikněte na virtuální síť, kde je nasazen HBase cluster na.
3. Klikněte na tlačítko **certifikáty** shora.
4. Klikněte na tlačítko **NAHRÁT** z dolní části a zadejte soubor kořenového certifikátu jste vytvořili v postupu před posledním. Počkejte, dokud získali certifikát importován.
5. Klikněte na tlačítko **řídicí panel** nahoře.  Virtuální diagram zobrazuje stav.

#### <a name="configure-your-vpn-client"></a>Konfigurace klienta VPN
**Ke stažení a instalaci balíčku klienta VPN**

1. Na stránce řídicího PANELU ve virtuální síti, v části rychlého přehledu, klikněte na možnost **stažení balíčku klienta VPN 64-bit** nebo **stažení balíčku klienta VPN 32-bit** založené na pracovní stanici operačního systému verze.
2. Klikněte na tlačítko **spustit** k instalaci balíčku.
3. Do příkazového řádku zabezpečení klikněte na tlačítko **více informací o**a potom klikněte na **přesto spustit**.
4. Klikněte na tlačítko **Ano** dvakrát.

**Pro připojení k síti VPN**

1. Na ploše pracovní stanice klikněte na ikonu sítě na hlavním panelu úloh. Zobrazí se připojení VPN s názvem vaší virtuální sítě.
2. Klikněte na název připojení VPN.
3. Klikněte na **Připojit**.

**K testování VPN připojení a domény překlad IP adresy**

* Z pracovní stanice, otevřete příkazový řádek a ping, jeden z následujících názvů zadána přípona DNS HBase cluster je myhbase.b7.internal.cloudapp.net:

        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        headnode0.myhbase.b7.internal.cloudapp.net
        headnode1.myhbase.b7.internal.cloudapp.net
        workernode0.myhbase.b7.internal.cloudapp.net

### <a name="install-and-configure-squirrel-on-your-workstation"></a>Instalace a konfigurace SQuirreL na pracovní stanici
**Chcete-li nainstalovat SQuirreL**

1. Stáhněte si soubor jar klienta SQuirreL SQL z [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).
2. Otevření a spuštění na soubor jar. Vyžaduje [prostředí Java Runtime](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).
3. Klikněte na tlačítko **Další** dvakrát.
4. Zadejte cestu, kde máte oprávnění k zápisu a pak klikněte na **Další**.

  > [!NOTE]
  > Výchozí instalační složka je ve složce C:\Program Files\squirrel-sql-3.6.  Aby bylo možné zapsat do této cesty, instalační program musí mít oprávnění správce. Otevřete příkazový řádek jako správce, přejděte do složky Bin přejít na jazyce Java a spusťte:
  >
  >     Java.exe-jar [cestu na soubor jar SQuirreL]
5. Klikněte na tlačítko **OK** pro potvrzení vytvoření cílový adresář.
6. Výchozí nastavení je instalace základní a standardní balíčky.  Klikněte na **Další**.
7. Klikněte na tlačítko **Další** dvakrát a potom klikněte na **provádí**.

**K instalaci ovladačů Phoenix**

Soubor jar phoenix ovladačů se nachází v clusteru HBase. Cesta je podobný následujícímu podle verze:

    C:\apps\dist\phoenix-4.0.0.2.1.11.0-2316\phoenix-4.0.0.2.1.11.0-2316-client.jar
Je potřeba ho zkopírujte do pracovní stanice v [Instalační složka SQuirreL] / lib cestu.  Nejjednodušší způsob je pro připojení RDP do clusteru a potom pomocí souboru kopírování a vkládání (CTRL + C a CTRL + V) a zkopírujte ho do pracovní stanice.

**Chcete-li přidat ovladač Phoenix SQuirreL**

1. Otevřete SQuirreL SQL klienta z pracovní stanice.
2. Klikněte **ovladač** karty na levé straně.
3. Z **ovladače** nabídky, klikněte na tlačítko **nový ovladač**.
4. Zadejte následující informace:

   * **Název**: Phoenix
   * **Příklad adresy URL**: jdbc:phoenix:zookeeper2.contoso-hbase-eu.f5.internal.cloudapp.net
   * **Název třídy**: org.apache.phoenix.jdbc.PhoenixDriver

     > [!WARNING]
     > Uživatel v adrese URL příklad všechny malá písmena. Můžete použít jejich úplné zookeeper kvora v případě, že jeden z nich je vypnutý.  Názvy hostitelů jsou zookeeper0, zookeeper1 a zookeeper2.
     >
     >

     ![HDInsight HBase Phoenix SQuirreL ovladačů][img-squirrel-driver]
5. Klikněte na **OK**.

**Chcete-li vytvořit alias do clusteru HBase**

1. SQuirreL, klikněte **aliasy** karty na levé straně.
2. Z **aliasy** nabídky, klikněte na tlačítko **nový Alias**.
3. Zadejte následující informace:

   * **Název**: název clusteru HBase nebo libovolný název, který preferujete.
   * **Ovladač**: Phoenix.  Toto musí odpovídat názvu ovladače, kterou jste vytvořili v posledním postupu.
   * **Adresa URL**: adresa URL se zkopíruje z konfiguraci ovladačů. Zkontrolujte, zda uživatel všechny malá písmena.
   * **Uživatelské jméno**: může být jakýkoli text.  Vzhledem k tomu, že používáte připojení VPN, uživatelské jméno se nepoužívá.
   * **Heslo**: může být jakýkoli text.

     ![HDInsight HBase Phoenix SQuirreL ovladačů][img-squirrel-alias]
4. Klikněte na tlačítko **Test**.
5. Klikněte na **Připojit**. Pokud se provede připojení, SQuirreL vypadá takto:

    ![HBase Phoenix SQuirreL][img-squirrel]

**Při spuštění testu**

1. Klikněte na tlačítko **SQL** kartě právo vedle **objekty** kartě.
2. Zkopírujte a vložte následující kód:

        CREATE TABLE IF NOT EXISTS us_population (state CHAR(2) NOT NULL, city VARCHAR NOT NULL, population BIGINT  CONSTRAINT my_pk PRIMARY KEY (state, city))
3. Klikněte na tlačítko spustit.

    ![HBase Phoenix SQuirreL][img-squirrel-sql]
4. Přepnout zpět **objekty** kartě.
5. Rozbalte název aliasu a poté rozbalte **tabulky**.  Zobrazí se nové tabulky uvedené v části.

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili, jak používat Apache Phoenix v HDInsight.  Další informace najdete v tématu

* [Přehled HBase ve službě HDInsight][hdinsight-hbase-overview]: HBase je NoSQL open source databáze Apache postavená na systému Hadoop, která poskytuje náhodný přístup a silnou konzistenci pro velké objemy nestrukturovaných a částečně strukturovaných dat.
* [Zřídit clustery HBase v Azure Virtual Network][hdinsight-hbase-provision-vnet]: integrace virtuální sítě, clustery HBase můžete nasadit do stejné virtuální síti jako aplikace tak, aby aplikace může komunikovat přímo s HBase.
* [Konfigurace replikace HBase v HDInsight](hbase/apache-hbase-replication.md): Naučte se konfigurovat replikace HBase v rámci dvou datových centrech Azure.


[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]:hbase/apache-hbase-provision-vnet.md
[hdinsight-hbase-overview]:hbase/apache-hbase-overview.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png
