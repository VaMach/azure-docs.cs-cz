---
title: "Řešení úložiště Azure pro R serverem v HDInsight - Azure | Microsoft Docs"
description: "Další informace o možnostech jiného úložiště k dispozici uživatelům s R serverem v HDInsight"
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 1cf30096-d3ca-45ea-b526-aa3954402f66
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: aafcc818af4c6e5d141d3633b31b913802a21752
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="azure-storage-solutions-for-r-server-on-hdinsight"></a>Řešení úložiště Azure pro R serverem v HDInsight

Microsoft R serverem v HDInsight obsahuje celou řadu řešení úložiště k uchování dat, kódu nebo objektů, které obsahují výsledky z analýzy. Mezi ně patří následující možnosti:

- [Objekt Blob systému Azure](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake Storage](https://azure.microsoft.com/services/data-lake-store/)
- [Úložiště Azure File](https://azure.microsoft.com/services/storage/files/)

Máte také možnost přístupu k několika účtům Azure storage nebo kontejnerů k vašemu clusteru HDI. Úložiště Azure File je možnost vhodná datová úložiště pro použití na hraniční uzel, který umožňuje připojení Azure úložné sdílené složky, například Linux systému souborů. Ale sdílené složky Azure File můžete připojit a používat libovolný systém, který nemá podporovaný operační systém, například Windows nebo Linux. 

Při vytváření clusteru Hadoop v HDInsight, můžete zadat buď **úložiště Azure** účet nebo **úložiště Data Lake store**. Kontejner konkrétní úložiště z tohoto účtu obsahuje systému souborů pro cluster, který vytvoříte (například Hadoop Distributed File System). Další informace a pokyny najdete v tématu:

- [Používání Azure storage s HDInsight](../hdinsight-hadoop-use-blob-storage.md)
- [Použití Data Lake Store s Azure HDInsight clustery](../hdinsight-hadoop-use-data-lake-store.md). 

Další informace o řešení úložiště Azure najdete v tématu [Úvod do Microsoft Azure Storage](../../storage/common/storage-introduction.md). 

Informace o výběru nejvhodnější možnosti úložiště pro váš scénář, najdete v části [rozhodování o použití objektů BLOB služby Azure, Azure soubory nebo datové disky Azure](../../storage/common/storage-decide-blobs-files-disks.md) 


## <a name="use-azure-blob-storage-accounts-with-r-server"></a>Účty úložiště objektů Blob v Azure pomocí R Server

V případě potřeby můžete přistupovat k vašemu clusteru HDI více účtů úložiště Azure nebo kontejnerů. Uděláte to tak, je třeba zadat další úložiště účtů v uživatelském rozhraní, při vytváření clusteru a potom postupujte podle těchto kroků používat s R Server.

> [!WARNING]
> Z důvodů výkonu se HDInsight cluster vytvoří ve stejném datovém centru jako účet primárního úložiště, který určíte. Použití účtu úložiště v jiném umístění než HDInsight cluster není podporováno.

1. Vytvoření clusteru HDInsight s názvem účtu úložiště **storage1** výchozí kontejner s názvem **container1**.
2. Zadejte účet úložiště, názvem **storage2**.  
3. Zkopírujte soubor mycsv.csv do adresáře/Share a provádět analýzy na daný soubor.  

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal myscsv.scv /share  

4. V kódu jazyka R, nastavte na název uzlu **výchozím** a nastavte adresáře a souboru ke zpracování.  

        myNameNode <- "default"
        myPort <- 0

        #Location of the data:  
        bigDataDirRoot <- "/share"  

        #Define Spark compute context:
        mySparkCluster <- RxSpark(consoleOutput=TRUE)

        #Set compute context:
        rxSetComputeContext(mySparkCluster)

        #Define the Hadoop Distributed File System (HDFS) file system:
        hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

        #Specify the input file to analyze in HDFS:
        inputFile <-file.path(bigDataDirRoot,"mycsv.csv")

Všechny adresáře a souboru odkazy přejděte na účet úložiště wasb://container1@storage1.blob.core.windows.net. Toto je **výchozí účet úložiště** který je spojen s clusterem HDInsight.

Nyní předpokládejme, že chcete zpracovat soubor s názvem mySpecial.csv, který je umístěný v /private adresář **container2** v **storage2**.

Ve vašem kódu R bodu název uzlu odkazu na **storage2** účet úložiště.


    myNameNode <- "wasb://container2@storage2.blob.core.windows.net"
    myPort <- 0

    #Location of the data:
    bigDataDirRoot <- "/private"

    #Define Spark compute context:
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    #Set compute context:
    rxSetComputeContext(mySparkCluster)

    #Define HDFS file system:
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    #Specify the input file to analyze in HDFS:
    inputFile <-file.path(bigDataDirRoot,"mySpecial.csv")

Všechny adresáře a souboru odkazy nyní přejděte na účet úložiště wasb://container2@storage2.blob.core.windows.net. Toto je **název uzlu** který jste zadali.

Budete muset nakonfigurovat User/RevoShare/<SSH username> v **storage2** následujícím způsobem:


    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>



## <a name="use-an-azure-data-lake-store-with-r-server"></a>Použití Azure Data Lake store s R Server

Pokud chcete používat s vaším účtem HDInsight ukládá Data Lake, budete muset poskytnout vašeho clusteru přístup k každý Azure Data Lake store, kterou chcete použít. Pokyny o tom, jak pomocí portálu Azure k vytvoření clusteru HDInsight pomocí účtu Azure Data Lake Store jako výchozí úložiště nebo jako další úložiště najdete v tématu [vytvoření clusteru HDInsight s Data Lake Store pomocí portálu Azure](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

Pak použijete úložišti ve vašem skriptu R mnohem stejně, jako jste účet sekundární úložiště Azure, jak je popsáno v předchozím postupu.

### <a name="add-cluster-access-to-your-azure-data-lake-stores"></a>Přístup ke clusteru přidat do vašeho úložiště Azure Data Lake
Máte přístup k úložiště Data Lake store pomocí objektu služby Azure Active Directory (Azure AD), který je spojen s clusteru HDInsight.

Chcete-li přidat objektu služby Azure AD:

1. Při vytváření clusteru HDInsight, vyberte **identita AAD clusteru** z **zdroj dat** kartě.

2. V **identita AAD clusteru** dialogovém **vyberte objekt služby AD**, vyberte **vytvořit nový**.

Po zadejte název objektu služby a vytvořit heslo pro něj, klikněte na tlačítko **Správa přístupu ADLS** instanční objekt přidružit vašeho úložiště Data Lake.

Je také možné přidat do úložiště Data Lake pro jeden nebo více po vytvoření clusteru přístup ke clusteru. Otevřete položku portál Azure pro Data Lake store a přejděte na **Průzkumníku dat > přístup > Přidat**. 

### <a name="how-to-access-the-data-lake-store-from-r-server"></a>Jak získat přístup ze serveru R úložiště Data Lake store

Jakmile jste dali přístup do úložiště Data Lake store, můžete použít úložiště v R Server v HDInsight způsob, jakým byste účet sekundární úložiště Azure. Jediným rozdílem je, že předpona **wasb: / /** změny **adl: / /** následujícím způsobem:


    # Point to the ADL store (e.g. ADLtest)
    myNameNode <- "adl://rkadl1.azuredatalakestore.net"
    myPort <- 0

    # Location of the data (assumes a /share directory on the ADL account)
    bigDataDirRoot <- "/share"  

    # Define Spark compute context
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    # Set compute context
    rxSetComputeContext(mySparkCluster)

    # Define HDFS file system
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    # Specify the input file in HDFS to analyze
    inputFile <-file.path(bigDataDirRoot,"AirlineDemoSmall.csv")

    # Create factors for days of the week
    colInfo <- list(DayOfWeek = list(type = "factor",
               levels = c("Monday", "Tuesday", "Wednesday", "Thursday",
                          "Friday", "Saturday", "Sunday")))

    # Define the data source
    airDS <- RxTextData(file = inputFile, missingValueString = "M",
                    colInfo  = colInfo, fileSystem = hdfsFS)

    # Run a linear regression
    model <- rxLinMod(ArrDelay~CRSDepTime+DayOfWeek, data = airDS)


Konfigurace účtu úložiště Data Lake s RevoShare adresáře a přidejte ukázkový soubor .csv z předchozího příkladu se používají následující příkazy:


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/AirlineDemoSmall.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-r-server"></a>Používání Azure File storage s R Server

Je také možnost vhodná datová úložiště pro použití v uzlu edge volat – Azure Files ((https://azure.microsoft.com/services/storage/files/). Umožňuje připojit Azure úložné sdílené složky systému souborů Linux. Tato možnost může být užitečný pro ukládání datové soubory, skripty R a objektů výsledků, které může být potřeba později, zejména v případě, že má smysl pro systém nativní souborů na uzlu edge namísto HDFS. 

Hlavní výhodou soubory Azure je, sdílené složky můžete připojit a používat systémem, který má podporovaný operační systém, například Windows nebo Linux. Například můžete použít jiný cluster HDInsight s někým ve vašem týmu, virtuální počítač Azure nebo i v místním systému. Další informace naleznete v tématu:

- [Jak používat Azure File Storage s Linuxem](../../storage/files/storage-how-to-use-files-linux.md)
- [Jak používat Azure File storage ve Windows](../../storage/files/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a>Další kroky

Teď, když znáte možnosti úložiště Azure, pomocí následujících odkazů ke zjišťování způsob přenosu dat vědecké účely úlohy provádějí s R serverem v HDInsight.

* [Přehled R serverem v HDInsight](r-server-overview.md)
* [Začínáme s serveru R na Hadoop](r-server-get-started.md)
* [Možnosti výpočetního kontextu pro R Server ve službě HDInsight](r-server-compute-contexts.md)

