---
title: "Vytváření clusterů systému Hadoop pomocí webového prohlížeče - Azure HDInsight | Microsoft Docs"
description: "Naučte se vytvářet clustery Hadoop, HBase, Storm a Spark v Linux pro HDInsight pomocí webového prohlížeče a portálu Azure preview."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 697278cf-0032-4f7c-b9b2-a84c4347659e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: c9a6040f7115917219143cd4bc8d0e3ac87370cf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-linux-based-clusters-in-hdinsight-using-the-azure-portal"></a>Vytvořit clustery se systémem Linux v HDInsight pomocí portálu Azure
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Portál Azure je nástroj pro správu webových služeb a prostředků, které jsou hostované v cloudu Microsoft Azure. V tomto článku se dozvíte, jak k vytvoření clusterů HDInsight se systémem Linux pomocí portálu.

## <a name="prerequisites"></a>Požadavky
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Moderní webový prohlížeč**. Portál Azure používá HTML5 a Javascript a nemusí fungovat správně starší verze webových prohlížečů.

## <a name="create-clusters"></a>Vytváření clusterů
Portál Azure zpřístupní většinu vlastností clusteru. Pomocí šablony Azure Resource Manager, můžete skrýt spoustu dalších podrobností. Další informace najdete v tématu [vytvořit systémem Linux Hadoop clusterů v HDInsight pomocí šablony Azure Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Klikněte na tlačítko  **+** , klikněte na tlačítko **Intelligence + analýzy**a potom klikněte na **HDInsight**.
   
    ![Vytvoření nového clusteru na portálu Azure](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster.png "vytvoření nového clusteru na portálu Azure")

3. V **HDInsight** okně klikněte na tlačítko **vlastní (velikost, nastavení, aplikace)**, klikněte na tlačítko **Základy**a potom zadejte následující informace.

    ![Vytvoření nového clusteru na portálu Azure](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-basics.png "vytvoření nového clusteru na portálu Azure")

    * Zadejte **Název clusteru**: Tento název musí být globálně jedinečný.

    * Z **předplatné** rozevíracího seznamu, vyberte předplatné Azure, který se použije pro cluster.

    * Klikněte na tlačítko **clusteru typu**a potom vyberte:
   
        * **Typ clusteru**: Pokud nevíte, vyberte možnost **Hadoop**. Jedná se o nejoblíbenější typ clusteru.
     
            > [!IMPORTANT]
            > HDInsight clustery mohou mít různé typy, které odpovídají zatížení nebo technologie, která clusteru je přizpůsobená pro. Neexistuje žádná podporovaná metoda pro vytvoření clusteru, který kombinuje více typů, jako je například Storm a HBase v jednom clusteru. 
            > 
            > 
        
        * **Operační systém**: Vyberte **Linux**.
        
        * **Verze**: Pokud nevíte, použijte výchozí verzi. Další informace najdete v článku [Verze clusterů HDInsight](hdinsight-component-versioning.md).
        * **Úroveň clusteru**: Azure HDInsight nabízí cloud pro velké objemy dat ve dvou kategoriích, Standard a Premium. Další informace najdete v článku [Úrovně clusterů](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers).

    * Pro **uživatelské jméno přihlášení clusteru** a **clusteru přihlašovacího hesla**, zadejte uživatelské jméno a heslo pro uživatele správce.

    * Zadejte **uživatelské jméno SSH** a pokud budete chtít mít heslo SSH stejné jako heslo správce jste dřív zadali, vyberte **použijte stejné heslo jako přihlašovací údaje clusteru** zaškrtávací políčko. Pokud ne, zadejte buď **heslo** nebo **veřejný klíč**, který se používá k ověření uživatele SSH. Doporučujeme používat veřejný klíč. Klikněte v dolní části na tlačítko **Vybrat** a uložte konfiguraci přihlašovacích údajů.
   
        Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

    * Pro **skupiny prostředků**, zadejte, zda chcete vytvořit novou skupinu prostředků nebo použijte existující.

    * Zadejte datového centra **umístění** kde bude vytvořen cluster.

    * Klikněte na **Další**.

4. Na **úložiště** okno, zadejte, zda chcete jako výchozí úložiště Azure Storage (WASB) nebo Data Lake Store. Podívejte se na tabulce pro další informace.

    ![Vytvoření nového clusteru na portálu Azure](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-storage.png "vytvoření nového clusteru na portálu Azure")

    | Úložiště                                      | Popis |
    |----------------------------------------------|-------------|
    | **Objektů BLOB služby Azure Storage jako výchozí úložiště**   | <ul><li>Pro **primárního úložiště typu**, vyberte **Azure Storage**. Potom pro **metodu výběru**, můžete zvolit **mé odběry** Pokud chcete zadat účet úložiště, který je součástí vašeho předplatného Azure a potom vyberte účet úložiště. Jinak, klikněte na tlačítko **přístupový klíč** a zadejte informace pro účet úložiště, který chcete vybrat z mimo vašeho předplatného Azure.</li><li>Pro **výchozí kontejner**, můžete přejít s výchozím názvem kontejneru navrhovaná službou portálu nebo zadat vlastní.</li><li>Pokud používáte WASB jako výchozí úložiště, můžete (volitelně) klepnutím **další účty úložiště** k určení dalších účtů úložiště pro přidružení ke clusteru. V **klíčů k úložišti Azure** okně klikněte na tlačítko **přidejte klíč k úložišti**, a potom můžete zadat účet úložiště z vašich předplatných Azure nebo z jiných předplatných (tím, že přístupový klíč účtu úložiště).</li><li>Pokud používáte WASB jako výchozí úložiště, můžete (volitelně) klepnutím **Data Lake Store přístup** k určení Azure Data Lake Store jako další úložiště. Další informace najdete v tématu [vytvoření clusteru HDInsight s Data Lake Store pomocí portálu Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</li></ul> |
    | **Azure Data Lake Store jako výchozí úložiště** | Pro **primárního úložiště typu**, vyberte **Data Lake Store** a pak naleznete v článku [vytvoření clusteru HDInsight s Data Lake Store pomocí portálu Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) pokyny. |
    | **Externí metaúložiště**                      | Volitelně můžete zadat databázi SQL pro uložení Hive a Oozie metadata spojená s clusterem. Pro **vyberte databázi SQL pro Hive** vyberte databázi SQL a zadejte uživatelské jméno a heslo pro databázi. Opakujte tyto kroky pro Oozie metadata.<br><br>Některé aspekty při používání Azure SQL database pro metaúložiště. <ul><li>Databáze Azure SQL použitý k metaúložiště musí umožňovat připojení k jiným službám Azure, včetně Azure HDInsight. Na řídicím panelu Azure SQL database, na pravé straně klikněte na název serveru. Je to server, na kterém je spuštěna instance databáze SQL. Jakmile jste na zobrazení serveru, klikněte na **konfigurace**a pak pro **služeb Azure**, klikněte na tlačítko **Ano**a pak klikněte na **Uložit**.</li><li>Při vytváření metaúložiště, nepoužívejte název databáze, který obsahuje pomlčky nebo spojovníky, protože to může způsobit selhání procesu vytváření clusteru.</li></ul>                                                                                                                                                                       |

    Klikněte na **Další**. 

    > [!WARNING]
    > Použití dalšího účtu úložiště v jiném umístění, než je cluster HDInsight, není podporováno.

5. Volitelně klikněte na **aplikace** k instalaci aplikací, které fungují s clustery HDInsight. Tyto aplikace mohou být vytvořeny společností Microsoft, nezávislými dodavateli softwaru (ISV) nebo vámi samotnými. Další informace najdete v tématu [aplikace nainstalovat HDInsight](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation).


6. Klikněte na tlačítko **velikost clusteru** zobrazíte informace o uzly, které se vytvoří pro tento cluster. Nastavte počet uzlů pracovního procesu, které potřebujete pro cluster. V okně se zobrazí odhadované náklady na provoz clusteru.
   
    ![Uzel cenové úrovně okno](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-nodes.png "zadejte počet uzlů clusteru")
   
   > [!IMPORTANT]
   > Pokud máte v plánu na víc než 32 uzlů pracovního procesu, při vytváření clusteru nebo škálování clusteru po vytvoření, je nutné vybrat velikost hlavního uzlu s alespoň s 8 jádry a 14GB paměti ram.
   > 
   > Další informace o velikosti uzlu a souvisejících nákladů, najdete v části [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).
   > 
   > 
   
   Klikněte na tlačítko **Další** uzlu ceny konfiguraci uložte.

7. Klikněte na tlačítko **upřesňující nastavení** nakonfigurovat další volitelné nastavení, například pomocí **akcí skriptů** k přizpůsobení clusteru a nainstalujte vlastní součásti nebo připojení **virtuální sítě**. Podívejte se na tabulce pro další informace.

    ![Uzel cenové úrovně okno](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-advanced.png "zadejte počet uzlů clusteru")

    | Možnost | Popis |
    |--------|-------------|
    | **Akce skriptu** | Tuto možnost použijte, pokud chcete použít vlastní skript k přizpůsobení cluster, při vytváření clusteru. Další informace o akcí skriptů naleznete v tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md). |
    | **Virtual Network** | Vyberte virtuální síť Azure a podsíť, pokud chcete umístit do clusteru do virtuální sítě. Informace o používání HDInsight s virtuální síť, včetně určité požadavky na konfiguraci pro virtuální síť, najdete v článku [HDInsight rozšířit možnosti pomocí Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md). |

    Klikněte na **Další**.

8. Na **Souhrn** okně zkontrolujte zadané informace, které jste dřív zadali a potom klikněte na **vytvořit**.

    ![Uzel cenové úrovně okno](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-summary.png "zadejte počet uzlů clusteru")
    
    > [!NOTE]
    > To bude trvat nějakou dobu clusteru má být vytvořen, obvykle přibližně 15 minut. Použít na dlaždici na úvodní panel, nebo **oznámení** položku na levé straně stránky zjistíte v procesu zřizování.
    > 
    > 
12. Po dokončení procesu vytváření, klikněte na dlaždici pro cluster z úvodního panelu v okně clusteru. V okně clusteru obsahuje následující informace.
    
    ![Okno clusteru](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-completed.png "vlastnosti clusteru")
    
    Následující informace vám pomůžou pochopit ikony v horní části tohoto okna.
    
    * **Přehled** kartě jsou uvedeny všechny základní informace o clusteru, například název, patří do skupiny prostředků, umístění, operační systém, adresa URL pro řídicí panel clusteru atd.
    * **Řídicí panel** vás přesměruje na portálu Ambari, který je přidružen ke clusteru.
    * **Secure Shell**: informace potřebné pro přístup ke clusteru pomocí protokolu SSH.
    * **Škálování clusteru** umožňuje zvýšit počet uzlů pracovního procesu, které jsou přidruženy ke clusteru.
    * **Odstranit**: Odstraní clusteru HDInsight.
    

## <a name="customize-clusters"></a>Přizpůsobení clusterů
* V tématu [clusterů HDInsight přizpůsobit pomocí Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).
* V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>Odstranění clusteru
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Řešení potíží

Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Další kroky
Teď, když jste úspěšně vytvořili clusteru služby HDInsight, použijte následující postupy se dozvíte, jak pracovat s clusteru:

### <a name="hadoop-clusters"></a>Clustery Hadoop
* [Použití Hivu se službou HDInsight](hdinsight-use-hive.md)
* [Použití Pigu se službou HDInsight](hdinsight-use-pig.md)
* [Používání nástroje MapReduce s HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>Clustery HBase
* [Začínáme s HBase v HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Vývoj aplikací v jazyce Java pro HBase v HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Clustery Storm
* [Vývoj topologie Java pro Storm v HDInsight](hdinsight-storm-develop-java-topology.md)
* [Použití komponent, Python v Storm v HDInsight](hdinsight-storm-develop-python-topology.md)
* [Nasazení a monitorování topologie se Storm v HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Clustery Spark
* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](hdinsight-apache-spark-eventhub-streaming.md)

