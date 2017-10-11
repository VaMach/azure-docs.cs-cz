---
title: "Apache Sqoop se systémem Hadoop - Azure HDInsight | Microsoft Docs"
description: "Další informace o použití Apache Sqoop k importu a exportu mezi systémem Hadoop v HDInsight a Azure SQL Database."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
author: Blackmist
tags: azure-portal
keywords: hadoop sqoop, sqoop
ms.assetid: 303649a5-4be5-4933-bf1d-4b232083c354
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: larryfr
ms.openlocfilehash: 35dcbb91e6af1480685c9fd5b829c54277c1c605
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-hadoop-on-hdinsight-and-sql-database"></a>Použití Apache Sqoop k importu a exportu dat mezi systémem Hadoop v HDInsight a databáze SQL

[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Další informace o použití Apache Sqoop k importu a exportu mezi clusteru Hadoop v prostředí Azure HDInsight a databáze Azure SQL Database nebo Microsoft SQL Server. Kroky v tomto dokumentu pomocí `sqoop` příkaz přímo z headnode clusteru Hadoop. Použití SSH k připojení k hlavnímu uzlu a spusťte příkazy v tomto dokumentu.

> [!IMPORTANT]
> Kroky v tomto dokumentu fungovat jenom s clustery HDInsight, které používají systém Linux. HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="install-freetds"></a>Nainstalujte FreeTDS

1. Použití SSH se připojit ke clusteru HDInsight. Například následující příkaz připojí k primární headnode clusteru s názvem `mycluster`:

    ```bash
    ssh CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Použijte následující příkaz k instalaci FreeTDS:

    ```bash
    sudo apt --assume-yes install freetds-dev freetds-bin
    ```

    FreeTDS se používá v několika krocích pro připojení k databázi SQL.

## <a name="create-the-table-in-sql-database"></a>Vytvořit v tabulce databáze SQL

> [!IMPORTANT]
> Pokud používáte HDInsight cluster a databázi SQL vytvořit v [vytvoření clusteru a databáze SQL](hdinsight-use-sqoop.md), přeskočte postup v této části. Databáze a tabulky, které byly vytvořeny jako součást kroky v [vytvoření clusteru a databáze SQL](hdinsight-use-sqoop.md) dokumentu.

1. Z relace SSH použijte následující příkaz pro připojení k databázi SQL serveru.

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Zobrazí se výstup podobný následujícímu:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

2. Na `1>` výzva, zadejte následující dotaz:

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
    GO
    ```

    Když `GO` příkaz je zadán, jsou vyhodnocovány předchozí příkazy. Nejdřív **mobiledata** tabulka bude vytvořena a potom clusterovaný index se přidá k němu (vyžadována databáze SQL.)

    Chcete-li ověřit, zda byl vytvořen v tabulce použijte následující dotaz:

    ```sql
    SELECT * FROM information_schema.tables
    GO
    ```

    Zobrazí výstup podobný následujícímu:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

3. Zadejte `exit` na `1>` výzvy ukončete nástroj tsql.

## <a name="sqoop-export"></a>Sqoop exportu

1. Z připojení SSH do clusteru použijte následující příkaz k ověření, že Sqoop vidí vaší databázi SQL:

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> -P
    ```
    Po zobrazení výzvy zadejte heslo pro přihlášení k databázi SQL.

    Tento příkaz vrátí seznam databází, včetně **sqooptest** databáze, kterou jste vytvořili dříve.

2. Export dat z **hivesampletable** k **mobiledata** tabulky, použijte následující příkaz:

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P --table 'mobiledata' --export-dir 'wasb:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1
    ```

    Tento příkaz nastaví Sqoop se připojit k **sqooptest** databáze. Sqoop exportuje data z **wasb: / / / hive nebo skladu/hivesampletable** k **mobiledata** tabulky.

    > [!IMPORTANT]
    > Použití `wasb:///` Pokud výchozí úložiště pro cluster se účet úložiště Azure. Použití `adl:///` Pokud je Azure Data Lake Store.

3. Po dokončení příkazu pro připojení k databázi pomocí TSQL použijte následující příkaz:

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P -p 1433 -D sqooptest
    ```

    Po připojení, použijte následující příkazy k ověření, že se data se exportují do **mobiledata** tabulky:

    ```sql
    SET ROWCOUNT 50;
    SELECT * FROM mobiledata
    GO
    ```

    Měli byste vidět seznam dat v tabulce. Typ `exit` ukončete nástroj tsql.

## <a name="sqoop-import"></a>Sqoop import

1. Použijte následující příkaz pro import dat z **mobiledata** do tabulky v databázi SQL, **wasb: / / / kurzy/usesqoop/importeddata** v HDInsight:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

    Pole v datech jsou oddělených tabulátorem a řádky se ukončila příkazem znak nového řádku.

2. Po dokončení importu použijte následující příkaz, který seznamu se data v adresáři nové:

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="using-sql-server"></a>Pomocí SQL serveru

Můžete taky Sqoop k importu a exportu dat z SQL serveru, buď ve vašem datovém centru, nebo na virtuální počítač hostovaný v Azure. Rozdíly mezi použitím SQL Database a SQL Server jsou:

* HDInsight a SQL Server musí být ve stejné virtuální síti Azure.

    Příklad, naleznete v části [HDInsight připojit k místní síti](./connect-on-premises-network.md) dokumentu.

    Další informace o používání HDInsight s virtuální síť Azure, najdete v článku [rozšíření prostředí HDInsight pomocí Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentu. Další informace o Azure Virtual Network, najdete v článku [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md) dokumentu.

* SQL Server musí nakonfigurovat pro povolení ověřování SQL. Další informace najdete v tématu [volba režimu ověřování](https://msdn.microsoft.com/ms144284.aspx) dokumentu.

* Budete muset nakonfigurovat SQL Server tak, aby přijímal vzdálená připojení. Další informace najdete v tématu [řešení potíží s připojením k databázovému stroji systému SQL Server](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) dokumentu.

* Vytvořte **sqooptest** databáze serveru SQL Server pomocí nástroj, jako třeba **SQL Server Management Studio** nebo **tsql**. Kroky pro použití Azure CLI fungovat pouze pro databázi SQL Azure.

    Pomocí následujících příkazů Transact-SQL můžete vytvořit **mobiledata** tabulky:

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    ```

* Při připojování k systému SQL Server z prostředí HDInsight, budete muset použít IP adresu serveru SQL Server. Například:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

## <a name="limitations"></a>Omezení

* Hromadné export - s Linuxovým systémem HDInsight, Sqoop konektor umožňuje exportovat data do systému Microsoft SQL Server nebo Azure SQL Database v současné době nepodporuje hromadné vložení.

* Dávkování - s HDInsight se systémem Linux, při použití `-batch` přepnout při vložení, Sqoop umožňuje více vloží místo dávkování operace insert.

## <a name="next-steps"></a>Další kroky

Nyní jste se naučili postup použití nástroje Sqoop. Další informace naleznete v tématu:

* [Použijte Oozie s HDInsight][hdinsight-use-oozie]: použití Sqoop akce v pracovním postupu Oozie.
* [Analýza dat zpoždění letu pomocí HDInsight][hdinsight-analyze-flight-data]: použití Hive k analýze letu zpoždění dat a pak pomocí Sqoop exportovat data do Azure SQL database.
* [Nahrání dat do HDInsight][hdinsight-upload-data]: Najít další metody pro odesílání dat do HDInsight nebo Azure Blob storage.

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
