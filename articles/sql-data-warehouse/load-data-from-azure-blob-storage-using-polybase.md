---
title: "Načítání dat Polybase - Azure Blob Storage do Azure SQL Data Warehouse | Microsoft Docs"
description: "Kurz, který používá portál Azure a SQL Server Management Studio načtení New Yorku Taxicab dat z Azure blob storage do Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: 
author: ckarst
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-data-warehouse
ms.custom: mvc,develop data warehouses
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 11/17/2017
ms.author: cakarst
ms.reviewer: barbkess
ms.openlocfilehash: 64315945d977ba912634eb626491a4513def1556
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="use-polybase-to-load-data-from-azure-blob-storage-to-azure-sql-data-warehouse"></a>Načtení dat z Azure blob storage do Azure SQL Data Warehouse pomocí PolyBase

PolyBase je standard načítání technologie pro získávání dat do SQL Data Warehouse. V tomto kurzu použijete k načtení New Yorku Taxicab dat z Azure blob storage do Azure SQL Data Warehouse PolyBase. Tento kurz používá [portál Azure](https://portal.azure.com) a [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) (SSMS) na: 

> [!div class="checklist"]
> * Vytvoření data warehouse na portálu Azure
> * Nastavit pravidlo brány firewall na úrovni serveru, na portálu Azure
> * Připojení do datového skladu pomocí SSMS
> * Vytvoření uživatele určené pro načítání dat
> * Vytvoření externí tabulky pro data v Azure blob storage
> * Použijte příkaz funkce CTAS T-SQL načítat data do datového skladu
> * Zobrazit průběh data, jako je načítání
> * Vytvoření statistiky pro nově načtená data

Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.

## <a name="before-you-begin"></a>Než začnete

Než začnete tento kurz, stáhněte a nainstalujte nejnovější verzi [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) (SSMS).


## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal

Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).

## <a name="create-a-blank-sql-data-warehouse"></a>Vytvořit prázdný datový sklad SQL

Datový sklad SQL Azure se vytvoří s definovanou sadou [výpočetních prostředků](performance-tiers.md). Databáze se vytvoří v rámci [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md) a na [logickém serveru SQL Azure](../sql-database/sql-database-features.md). 

Postupujte podle těchto kroků můžete vytvořit prázdnou SQL data warehouse. 

1. Klikněte na tlačítko **Nový** v levém horním rohu webu Azure Portal.

2. Na stránce **Nový** vyberte **Databáze** a v části **Doporučené** na stránce **Nový** vyberte **SQL Data Warehouse**.

    ![vytvoření datového skladu](media/load-data-from-azure-blob-storage-using-polybase/create-empty-data-warehouse.png)

3. Do formuláře služby SQL Data Warehouse zadejte následující informace:   

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Název databáze** | mySampleDataWarehouse | Platné názvy databází najdete v tématu [Identifikátory databází](/sql/relational-databases/databases/database-identifiers). | 
   | **Předplatné** | Vaše předplatné  | Podrobnosti o vašich předplatných najdete v tématu [Předplatná](https://account.windowsazure.com/Subscriptions). |
   | **Skupina prostředků** | myResourceGroup | Platné názvy skupin prostředků najdete v tématu [Pravidla a omezení pojmenování](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Výběr zdroje** | Prázdnou databázi | Určuje, že chcete vytvořit prázdnou databázi. Poznámka: datový sklad je jedním z typů databáze.|

    ![vytvoření datového skladu](media/load-data-from-azure-blob-storage-using-polybase/create-data-warehouse.png)

4. Klikněte na **Server** a vytvořte a nakonfigurujte nový server pro novou databázi. Do **formuláře Nový server** zadejte následující informace: 

    | Nastavení | Navrhovaná hodnota | Popis | 
    | ------- | --------------- | ----------- |
    | **Název serveru** | Libovolný globálně jedinečný název | Platné názvy serverů najdete v tématu [Pravidla a omezení pojmenování](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). | 
    | **Přihlašovací jméno správce serveru** | Libovolné platné jméno | Platná přihlašovací jména najdete v tématu [Identifikátory databází](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).|
    | **Heslo** | Libovolné platné heslo | Heslo musí mít alespoň osm znaků a musí obsahovat znaky ze tří z následujících kategorií: velká písmena, malá písmena, číslice a jiné než alfanumerické znaky. |
    | **Umístění** | Libovolné platné umístění | Informace o oblastech najdete v tématu [Oblasti služeb Azure](https://azure.microsoft.com/regions/). |

    ![vytvoření databázového serveru](media/load-data-from-azure-blob-storage-using-polybase/create-database-server.png)

5. Klikněte na **Vybrat**.

6. Klikněte na tlačítko **úroveň výkonu** k určení, zda datový sklad je optimalizován pro pružnost nebo výpočetní a počet datových skladů jednotky. 

7. V tomto kurzu vyberte **optimalizované pro pružnost** vrstvy služeb. Posuvník je ve výchozím nastavení nastavený na hodnotu **DW400**.  Zkuste jeho posouváním hodnotu zvýšit a snížit a podívejte se, jak funguje. 

    ![konfigurace výkonu](media/load-data-from-azure-blob-storage-using-polybase/configure-performance.png)

8. Klikněte na tlačítko **Použít**.
9. Na stránce SQL Data Warehouse, vyberte **kolace** pro prázdnou databázi. V tomto kurzu použijte výchozí hodnotu. Další informace o kolacích najdete v tématu [kolace](/sql/t-sql/statements/collations.md)

11. Po vyplnění formuláře pro SQL Database klikněte na **Vytvořit** a databázi zřiďte. Zřizování trvá několik minut. 

    ![kliknutí na Vytvořit](media/load-data-from-azure-blob-storage-using-polybase/click-create.png)

12. Na panelu nástrojů klikněte na **Oznámení** a sledujte proces nasazení.
    
     ![oznámení](media/load-data-from-azure-blob-storage-using-polybase/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Vytvoření pravidla brány firewall na úrovni serveru

Služba SQL Data Warehouse vytvoří bránu firewall na úrovni serveru, aby zabránila externím aplikacím a nástrojům v připojení k serveru nebo ke kterékoli databázi na serveru. Pokud chcete umožnit připojení, můžete přidat pravidla brány firewall, která povolí připojení z konkrétních IP adres.  Postupujte podle těchto pokynů a vytvořte [pravidlo brány firewall na úrovni serveru](../sql-database/sql-database-firewall-configure.md) pro IP adresu vašeho klienta. 

> [!NOTE]
> SQL Data Warehouse komunikuje přes port 1433. Pokud se pokoušíte připojit z podnikové sítě, nemusí být odchozí provoz přes port 1433 bránou firewall vaší sítě povolený. Pokud je to tak, nebudete se moct připojit k serveru Azure SQL Database, dokud vaše IT oddělení neotevře port 1433.
>

1. Po dokončení nasazení klikněte na **Databáze SQL** z nabídky na levé straně a klikněte na **mySampleDatabase** na stránce **Databáze SQL**. Otevře se stránka s přehledem pro vaši databázi, na které se zobrazí plně kvalifikovaný název serveru (například **mynewserver-20171113.database.windows.net**) a možnosti pro další konfiguraci. 

2. Zkopírujte tento plně kvalifikovaný název serveru, abyste ho mohli použít pro připojení k serveru a jeho databázím v následujících rychlých startech. Potom klikněte na název serveru a otevřete nastavení serveru.

    ![vyhledání názvu serveru](media/load-data-from-azure-blob-storage-using-polybase/find-server-name.png) 

3. Klikněte na název serveru a otevřete nastavení serveru.

    ![nastavení serveru](media/load-data-from-azure-blob-storage-using-polybase/server-settings.png) 

5. Klikněte na **Zobrazit nastavení brány firewall**. Otevře se stránka **Nastavení brány firewall** pro server služby SQL Database. 

    ![pravidlo brány firewall serveru](media/load-data-from-azure-blob-storage-using-polybase/server-firewall-rule.png) 

4. Klikněte na **Přidat IP adresu klienta** na panelu nástrojů a přidejte svoji aktuální IP adresu do nového pravidla brány firewall. Pravidlo brány firewall může otevřít port 1433 pro jednu IP adresu nebo rozsah IP adres.

5. Klikněte na **Uložit**. Vytvoří se pravidlo brány firewall na úrovni serveru pro vaši aktuální IP adresu, které otevře port 1433 na logickém serveru.

6. Klikněte na **OK** a pak zavřete stránku **Nastavení brány firewall**.

Pomocí této IP adresy se teď můžete připojit k serveru SQL a jeho datovým skladům. Připojení funguje z aplikace SQL Server Management Studio nebo jiného nástroje podle vašeho výběru. Při připojování použijte účet správce serveru, který jste předtím vytvořili.  

> [!IMPORTANT]
> Standardně je přístup přes bránu firewall služby SQL Database povolený pro všechny služby Azure. Pokud chcete bránu firewall zakázat pro všechny služby Azure, klikněte na této stránce na **VYPNUTO** pak klikněte na **Uložit**.

## <a name="get-the-fully-qualified-server-name"></a>Získání plně kvalifikovaného názvu serveru

Na webu Azure Portal získejte plně kvalifikovaný název vašeho serveru SQL. Plně kvalifikovaný název budete později používat při připojování k serveru.

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).
2. V nabídce vlevo vyberte **SQL Database** a na stránce **Databáze SQL** klikněte na vaši databázi. 
3. V podokně **Základy** na stránce webu Azure Portal pro vaši databázi vyhledejte a potom zkopírujte **Název serveru**. V tomto příkladu je plně kvalifikovaný název mynewserver-20171113.database.windows.net. 

    ![informace o připojení](media/load-data-from-azure-blob-storage-using-polybase/find-server-name.png)  

## <a name="connect-to-the-server-as-server-admin"></a>Připojení k serveru jako správce serveru

V této části se pomocí aplikace [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms.md) (SSMS) naváže připojení k serveru SQL Azure.

1. Otevřete SQL Server Management Studio.

2. V dialogovém okně **Připojení k serveru** zadejte následující informace:

    | Nastavení      | Navrhovaná hodnota | Popis | 
    | ------------ | --------------- | ----------- | 
    | Typ serveru | Databázový stroj | Tato hodnota se vyžaduje. |
    | Název serveru | Plně kvalifikovaný název serveru | Název musí být přibližně takto: **mynewserver 20171113.database.windows.net**. |
    | Authentication | Ověřování SQL Serveru | Ověřování SQL je jediný typ ověřování, který jsme v tomto kurzu nakonfigurovali. |
    | Přihlásit | Účet správce serveru | Jedná se o účet, který jste zadali při vytváření serveru. |
    | Heslo | Heslo pro účet správce serveru | Jedná se o heslo, které jste zadali při vytváření serveru. |

    ![Připojení k serveru](media/load-data-from-azure-blob-storage-using-polybase/connect-to-server.png)

4. Klikněte na **Připojit**. V aplikaci SSMS se otevře okno Průzkumníka objektů. 

5. V Průzkumníku objektů rozbalte **Databáze**. Potom rozbalte **systémové databáze** a **hlavní** zobrazit objekty v hlavní databázi.  Rozbalte položku **mySampleDatabase** zobrazit objekty v nové databáze.

    ![databázové objekty](media/load-data-from-azure-blob-storage-using-polybase/connected.png) 

## <a name="create-a-user-for-loading-data"></a>Vytvořte uživatele pro načítání dat

Účet správce serveru slouží k provádění operací správy a není vhodná pro spuštění dotazů na data uživatele. Načítání dat je operace náročné na paměť. [Maximální hodnoty paměti](performance-tiers.md#memory-maximums) jsou definovány podle [úroveň výkonu](performance-tiers.md), a [Třída prostředků](resource-classes-for-workload-management.md). 

Je nejvhodnější vytvořit přihlášení a uživatele, který je vyhrazen pro načítání dat. Pak přidejte uživatele načítání do [Třída prostředků](resource-classes-for-workload-management.md) umožňující přidělení odpovídající maximální velikost paměti.

Vzhledem k tomu, že jste aktuálně připojeni jako správce serveru, můžete vytvořit přihlášení a uživatele. Pomocí těchto kroků můžete vytvořit přihlášení a uživatele volat **LoaderRC20**. Pak přiřaďte uživatele do **staticrc20** Třída prostředků. 

1.  V aplikaci SSMS, klikněte pravým tlačítkem na **hlavní** zobrazit rozevírací nabídce, a vyberte **nový dotaz**. Otevře se nové okno dotazu.

    ![Nový dotaz v předloze](media/load-data-from-azure-blob-storage-using-polybase/create-loader-login.png)

2. V okně dotazu zadejte tyto příkazy T-SQL a vytvořte přihlášení a uživatele s názvem LoaderRC20, nahraďte vlastní heslo pro 'a123STRONGpassword!'. 

    ```sql
    CREATE LOGIN LoaderRC20 WITH PASSWORD = 'a123STRONGpassword!';
    CREATE USER LoaderRC20 FOR LOGIN LoaderRC20;
    ```

3. Klikněte na tlačítko **Spustit**.

4. Klikněte pravým tlačítkem na **mySampleDataWarehouse**a zvolte **nový dotaz**. Otevře okno Nový dotaz.  

    ![Nový dotaz na ukázkové datového skladu](media/load-data-from-azure-blob-storage-using-polybase/create-loading-user.png)
 
5. Zadejte následující příkazy T-SQL vytvořte uživatele databáze s názvem LoaderRC20 pro LoaderRC20 přihlášení. Druhý řádek uděluje oprávnění pro řízení na nový datový sklad nového uživatele.  Tato oprávnění jsou podobná nastavit uživatele vlastník databáze. Ve třetím řádku přidá nový uživatel jako člen skupiny staticrc20 [Třída prostředků](resource-classes-for-workload-management.md).

    ```sql
    CREATE USER LoaderRC20 FOR LOGIN LoaderRC20;
    GRANT CONTROL ON DATABASE::[mySampleDataWarehouse] to LoaderRC20;
    EXEC sp_addrolemember 'staticrc20', 'LoaderRC20';
    ```

6. Klikněte na tlačítko **Spustit**.

## <a name="connect-to-the-server-as-the-loading-user"></a>Připojení k serveru jako uživatel načítání

Prvním krokem k načítání dat je přihlášení jako LoaderRC20.  

1. V Průzkumníku objektů, klikněte **připojit** rozevírací nabídku a vyberte **databázový stroj**. **Připojit k serveru** zobrazí se dialogové okno.

    ![Připojení k nové přihlašovací údaje](media/load-data-from-azure-blob-storage-using-polybase/connect-as-loading-user.png)

2. Zadejte název plně kvalifikovaný serveru a zadejte **LoaderRC20** jako přihlášení.  Zadejte heslo pro LoaderRC20.

3. Klikněte na **Připojit**.

4. Při připojení je připraven, zobrazí se dvě připojení serveru v Průzkumníku objektů. Jedno připojení jako správce serveru a jedno připojení jako MedRCLogin.

    ![Připojení je úspěšné](media/load-data-from-azure-blob-storage-using-polybase/connected-as-new-login.png)

## <a name="create-external-tables-for-the-sample-data"></a>Vytvoření externí tabulky pro ukázková data

Jste připraveni k zahájení procesu načítání dat do nového datového skladu. V tomto kurzu se dozvíte, jak používat [Polybase](/sql/relational-databases/polybase/polybase-guide.md) načíst New Yorku taxíkem souboru cab data z objektu blob úložiště Azure. Informace o přesunu dat do Azure Blob Storage nebo jejich načtení přímo ze zdroje do služby SQL Data Warehouse najdete pro budoucí použití v části s [přehledem načítání](sql-data-warehouse-overview-load.md).

Spusťte následující příkaz SQL skriptů zadejte informace o datech, které chcete načíst. Tyto informace zahrnují, kde jsou data uložena, formát obsah data a definice tabulky pro data. 

1. V předchozí části jste přihlášení do datového skladu jako LoaderRC20. V aplikaci SSMS, klikněte pravým tlačítkem na připojení LoaderRC20 a vyberte **nový dotaz**.  Zobrazí se nové okno dotazu. 

    ![Načítání nové okno dotazu](media/load-data-from-azure-blob-storage-using-polybase/new-loading-query.png)

2. Porovnejte vaše okno dotazu na předchozí obrázek.  Ověřte, používá jako LoaderRC20 a provádění dotazů na databázi MySampleDataWarehouse vaší nové okno dotazu. K provedení všech kroků načítání použijte toto okno dotazu.

3. Vytvoření hlavního klíče pro databázi MySampleDataWarehouse. Pro každou databázi je nutné vytvořit hlavní klíč pouze jednou. 

    ```sql
    CREATE MASTER KEY;
    ```

4. Spusťte následující [vytvořit externí zdroj dat](/sql/t-sql/statements/create-external-data-source-transact-sql.md) příkaz určete umístění objektu Azure blob. Toto je umístění dat taxíkem externí soubor cab.  Chcete-li spustit příkaz, který jste připojí do okna dotazu, zvýrazněte příkazy, které chcete spustit a klikněte na tlačítko **Execute**.

    ```sql
    CREATE EXTERNAL DATA SOURCE NYTPublic
    WITH
    (
        TYPE = Hadoop,
        LOCATION = 'wasbs://2013@nytaxiblob.blob.core.windows.net/'
    );
    ```

5. Spusťte následující [vytvořit EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql.md) příkaz T-SQL určete formátování vlastnosti a možnosti pro externí datový soubor. Tento příkaz určuje externích dat je uloženo jako text a hodnoty jsou odděleny kanálu ('| ') znaků. Externí soubor je komprimován s Gzip. 

    ```sql
    CREATE EXTERNAL FILE FORMAT uncompressedcsv
    WITH (
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( 
            FIELD_TERMINATOR = ',',
            STRING_DELIMITER = '',
            DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        )
    );
    CREATE EXTERNAL FILE FORMAT compressedcsv
    WITH ( 
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( FIELD_TERMINATOR = '|',
            STRING_DELIMITER = '',
        DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        ),
        DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
    );
    ```

6.  Spusťte následující [CREATE SCHEMA](/sql/t-sql/statements/create-schema-transact-sql.md) příkaz Vytvořit schéma pro vaše formát externích souborů. Schéma poskytuje způsob, jak uspořádat externí tabulky, které se chystáte vytvořit.

    ```sql
    CREATE SCHEMA ext;
    ```

7. Vytvořte externí tabulky. V tabulce, definice, které jsou uloženy v SQL Data Warehouse, ale v tabulkách referenční data, která je uložená v úložišti objektů blob Azure. Spuštěním následujících příkazů T-SQL vytvořte několik externích tabulek odkazujících na objekt blob Azure, který jsme dříve definovali v externím zdroji dat.

    ```sql
    CREATE EXTERNAL TABLE [ext].[Date] 
    (
        [DateID] int NOT NULL,
        [Date] datetime NULL,
        [DateBKey] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DaySuffix] varchar(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeek] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfQuarter] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfYear] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfMonth] varchar(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Month] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Quarter] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [QuarterName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Year] char(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [YearName] char(7) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthYear] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MMYYYY] char(6) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FirstDayOfMonth] date NULL,
        [LastDayOfMonth] date NULL,
        [FirstDayOfQuarter] date NULL,
        [LastDayOfQuarter] date NULL,
        [FirstDayOfYear] date NULL,
        [LastDayOfYear] date NULL,
        [IsHolidayUSA] bit NULL,
        [IsWeekday] bit NULL,
        [HolidayUSA] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Date',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    ); 
    CREATE EXTERNAL TABLE [ext].[Geography]
    (
        [GeographyID] int NOT NULL,
        [ZipCodeBKey] varchar(10) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [County] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [City] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [State] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Country] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [ZipCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Geography',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0 
    );      
    CREATE EXTERNAL TABLE [ext].[HackneyLicense]
    (
        [HackneyLicenseID] int NOT NULL,
        [HackneyLicenseBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HackneyLicenseCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'HackneyLicense',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    );
    CREATE EXTERNAL TABLE [ext].[Medallion]
    (
        [MedallionID] int NOT NULL,
        [MedallionBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [MedallionCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Medallion',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;  
    CREATE EXTERNAL TABLE [ext].[Time]
    (
        [TimeID] int NOT NULL,
        [TimeBKey] varchar(8) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HourNumber] tinyint NOT NULL,
        [MinuteNumber] tinyint NOT NULL,
        [SecondNumber] tinyint NOT NULL,
        [TimeInSecond] int NOT NULL,
        [HourlyBucket] varchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [DayTimeBucketGroupKey] int NOT NULL,
        [DayTimeBucket] varchar(100) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL
    )
    WITH
    (
        LOCATION = 'Time',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    );
    CREATE EXTERNAL TABLE [ext].[Trip]
    (
        [DateID] int NOT NULL,
        [MedallionID] int NOT NULL,
        [HackneyLicenseID] int NOT NULL,
        [PickupTimeID] int NOT NULL,
        [DropoffTimeID] int NOT NULL,
        [PickupGeographyID] int NULL,
        [DropoffGeographyID] int NULL,
        [PickupLatitude] float NULL,
        [PickupLongitude] float NULL,
        [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DropoffLatitude] float NULL,
        [DropoffLongitude] float NULL,
        [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [PassengerCount] int NULL,
        [TripDurationSeconds] int NULL,
        [TripDistanceMiles] float NULL,
        [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FareAmount] money NULL,
        [SurchargeAmount] money NULL,
        [TaxAmount] money NULL,
        [TipAmount] money NULL,
        [TollsAmount] money NULL,
        [TotalAmount] money NULL
    )
    WITH
    (
        LOCATION = 'Trip2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = compressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    );
    CREATE EXTERNAL TABLE [ext].[Weather]
    (
        [DateID] int NOT NULL,
        [GeographyID] int NOT NULL,
        [PrecipitationInches] float NOT NULL,
        [AvgTemperatureFahrenheit] float NOT NULL
    )
    WITH
    (
        LOCATION = 'Weather',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    ```

8. V Průzkumníku objektů rozbalte mySampleDataWarehouse zobrazíte seznam externí tabulky, který jste právě vytvořili.

    ![Zobrazení externí tabulky](media/load-data-from-azure-blob-storage-using-polybase/view-external-tables.png)

## <a name="load-the-data-into-your-data-warehouse"></a>Načíst data do datového skladu

Tato část používá externí tabulky vámi zadaný načíst ukázková data z Azure Storage Blob do SQL Data Warehouse.  

> [!NOTE]
> V tomto kurzu načte data přímo do konečné tabulky. V produkčním prostředí obvykle použijete CREATE TABLE AS SELECT se načte do pracovní tabulky. Když data jsou v přípravné tabulce, můžete provést všechny potřebné transformace. Pokud chcete data v přípravné tabulce připojit k provozní tabulky, můžete použít úlohy INSERT... Příkaz SELECT. Další informace najdete v tématu [vložení dat do tabulky produkční](guidance-for-loading-data.md#inserting-data-into-a-production-table).
> 

Tento skript využívá [vytvořit tabulku AS vyberte funkce CTAS ()](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md) příkaz T-SQL k načítání dat z Azure Storage Blob do nové tabulky v datovém skladu. Funkce CTAS vytvoří novou tabulku na základě výsledků příkazu select. Nová tabulka obsahuje stejné sloupce a datové typy jako výsledky příkazu SELECT. Pokud příkaz select vybere z externí tabulku, SQL Data Warehouse naimportuje data do relační tabulky v datovém skladu. 

1. Spusťte následující skript, který chcete načíst data do nové tabulky v datovém skladu.

    ```sql
    CREATE TABLE [dbo].[Date]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Date]
    OPTION (LABEL = 'CTAS : Load [dbo].[Date]')
    ;
    CREATE TABLE [dbo].[Geography]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT * FROM [ext].[Geography]
    OPTION (LABEL = 'CTAS : Load [dbo].[Geography]')
    ;
    CREATE TABLE [dbo].[HackneyLicense]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[HackneyLicense]
    OPTION (LABEL = 'CTAS : Load [dbo].[HackneyLicense]')
    ;
    CREATE TABLE [dbo].[Medallion]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Medallion]
    OPTION (LABEL = 'CTAS : Load [dbo].[Medallion]')
    ;
    CREATE TABLE [dbo].[Time]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Time]
    OPTION (LABEL = 'CTAS : Load [dbo].[Time]')
    ;
    CREATE TABLE [dbo].[Weather]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Weather]
    OPTION (LABEL = 'CTAS : Load [dbo].[Weather]')
    ;
    CREATE TABLE [dbo].[Trip]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Trip]
    OPTION (LABEL = 'CTAS : Load [dbo].[Trip]')
    ;
    ```

2. Zobrazte data během načítání. Provedete načtení několika GB dat a jejich kompresi do vysoce výkonných clusterovaných indexů columnstore. Spuštěním následujícího dotazu, který používá zobrazení dynamické správy, zobrazíte stav načítání. Po spuštění dotazu si udělejte přestávku na kávu, zatímco SQL Data Warehouse bude dělat namáhavou práci.

    ```sql
    SELECT
        r.command,
        s.request_id,
        r.status,
        count(distinct input_name) as nbr_files,
        sum(s.bytes_processed)/1024/1024/1024 as gb_processed
    FROM 
        sys.dm_pdw_exec_requests r
        INNER JOIN sys.dm_pdw_dms_external_work s
        ON r.request_id = s.request_id
    WHERE
        r.[label] = 'CTAS : Load [dbo].[Date]' OR
        r.[label] = 'CTAS : Load [dbo].[Geography]' OR
        r.[label] = 'CTAS : Load [dbo].[HackneyLicense]' OR
        r.[label] = 'CTAS : Load [dbo].[Medallion]' OR
        r.[label] = 'CTAS : Load [dbo].[Time]' OR
        r.[label] = 'CTAS : Load [dbo].[Weather]' OR
        r.[label] = 'CTAS : Load [dbo].[Trip]'
    GROUP BY
        r.command,
        s.request_id,
        r.status
    ORDER BY
        nbr_files desc, 
        gb_processed desc;
    ```

3. Zobrazte všechny systémové dotazy.

    ```sql
    SELECT * FROM sys.dm_pdw_exec_requests;
    ```

4. Získejte zobrazuje data vhodně načíst do datového skladu.

    ![Zobrazit načíst tabulky](media/load-data-from-azure-blob-storage-using-polybase/view-loaded-tables.png)

## <a name="create-statistics-on-newly-loaded-data"></a>Vytvoření statistiky pro nově načtená data

SQL Data Warehouse nevytváří ani neaktualizuje statistiku automaticky. Pro dosažení vysokého výkonu dotazu je proto důležité vytvořit statistiku pro každý sloupec každé tabulky po prvním načtení. Důležité je také aktualizovat statistiku po důležitých změnách v datech.

Spusťte tyto příkazy k vytvoření statistiky pro sloupce, které se mohou být použity ve spojeních.

    ```sql
    CREATE STATISTICS [dbo.Date DateID stats] ON dbo.Date (DateID);
    CREATE STATISTICS [dbo.Trip DateID stats] ON dbo.Trip (DateID);
    ```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Vám budou účtovány výpočetní prostředky a data, která načíst do datového skladu. Ty se účtují samostatně. 

- Pokud chcete zachovat data v úložišti, můžete pozastavit výpočetní prostředky v době, kdy datový sklad nepoužíváte. Pomocí pozastavení výpočetní bude pouze zdarma pro úložiště dat a můžete obnovit výpočetní vždy, když budete chtít pracovat s daty.
- Pokud chcete zamezit budoucím poplatkům, můžete datový sklad odstranit. 

Pomocí tohoto postupu podle potřeby vyčistěte prostředky.

1. Přihlaste se k [portál Azure](https://portal.azure.com), klikněte na váš datový sklad.

    ![Vyčištění prostředků](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. Pokud chcete pozastavit výpočetní prostředky, klikněte na tlačítko **Pozastavit**. Pokud datový sklad je pozastavena, zobrazí se **spustit** tlačítko.  Pokud chcete obnovit výpočetní prostředky, klikněte na **Spustit**.

3. Chcete-li odebrat datového skladu, takže vám nebude nic účtováno výpočtů nebo úložiště, klikněte na tlačítko **odstranit**.

4. Pokud chcete odstranit server SQL, který jste vytvořili, klikněte na **mynewserver-20171113.database.windows.net**, jak je znázorněno na předchozím obrázku, a pak klikněte na **Odstranit**.  Buďte opatrní s jako odstranění serveru se odstraní všechny databáze, které jsou přiřazené k serveru.

5. Pokud chcete odebrat skupinu prostředků, klikněte na **myResourceGroup** a pak klikněte na **Odstranit skupinu prostředků**.

## <a name="next-steps"></a>Další kroky 
V tomto kurzu jste zjistili, jak vytvořit datový sklad a vytvořte uživatele pro načítání dat. Vytvoří externí tabulky můžete definovat strukturu dat uložených v Azure Storage Blob a pak použít příkaz PolyBase CREATE TABLE AS SELECT k načtení dat do datového skladu. 

Jste to udělali tyto věci:
> [!div class="checklist"]
> * Vytvoření datového skladu na portálu Azure
> * Nastavit pravidlo brány firewall na úrovni serveru, na portálu Azure
> * Připojení do datového skladu pomocí SSMS
> * Vytvořit uživatele určené pro načítání dat
> * Vytvoření externí tabulky pro data v Azure Storage Blob
> * Použít příkaz funkce CTAS T-SQL načítat data do datového skladu
> * Zobrazit průběh data, jako je načítání
> * Vytvoření statistiky pro nově načtená data

Přechodu na Přehled migrace se dozvíte, jak migrovat existující databázi do SQL Data Warehouse.

> [!div class="nextstepaction"]
>[Zjistěte, jak migrovat existující databázi do SQL Data Warehouse](sql-data-warehouse-overview-migrate.md)
