---
title: "Azure SQL Data Warehouse – kurz Začínáme | Dokumentace Microsoftu"
description: "V tomto kurzu se naučíte, jak zřídit službu Azure SQL Data Warehouse a nahrát do ní data. Také se seznámíte se základními informacemi o škálování, pozastavení a ladění."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: barbkess
ms.assetid: 52DFC191-E094-4B04-893F-B64D5828A900
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 01/26/2017
ms.author: elbutter;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2c88c1abd2af7a1ca041cd5003fd1f848e1b311c
ms.openlocfilehash: 12f72e76ee991dfb701637847f2e406cd0f8c449
ms.lasthandoff: 02/03/2017


---
# <a name="get-started-with-sql-data-warehouse"></a>Začínáme s SQL Data Warehouse

V tomto kurzu se dozvíte, jak zřídit službu Azure SQL Data Warehouse a nahrát do ní data. Také se seznámíte se základními informacemi o škálování, pozastavení a ladění. Po dokončení kurzu budete připraveni na dotazování a prozkoumávání svého datového skladu.

**Odhadovaný čas dokončení:** Toto je ucelený kurz s ukázkovým kódem, který po splnění požadavků zabere přibližně 30 minut. 

## <a name="prerequisites"></a>Požadavky

Kurz předpokládá znalost základních konceptů služby SQL Data Warehouse. Pokud potřebujete úvodní informace, přečtěte si téma [Co je SQL Data Warehouse?](sql-data-warehouse-overview-what-is.md) 

### <a name="sign-up-for-microsoft-azure"></a>Přihlášení k Microsoft Azure
Pokud ještě nemáte účet Microsoft Azure, musíte si jeden zaregistrovat, abyste mohli tuto službu používat. Pokud již máte účet, tento krok přeskočte. 

1. Přejděte na stránky účtu [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/).
2. Vytvořte si bezplatný účet Azure, nebo si účet zakupte.
3. Postupujte podle zobrazených pokynů.

### <a name="install-appropriate-sql-client-drivers-and-tools"></a>Instalace odpovídajících ovladačů a nástrojů klienta SQL

Většina nástrojů klienta SQL se může připojit ke službě SQL Data Warehouse pomocí JDBC, ODBC nebo ADO.NET. Z důvodu velkého počtu funkcí T-SQL, které SQL Data Warehouse podporuje, nejsou některé klientské aplikace s SQL Data Warehouse plně kompatibilní.

Pokud používáte operační systém Windows, doporučujeme použít buď sadu [Visual Studio], nebo aplikaci [SQL Server Management Studio].

[!INCLUDE [Create a new logical server](../../includes/sql-data-warehouse-create-logical-server.md)] 

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="create-a-sql-data-warehouse"></a>Vytvoření SQL Data Warehouse

SQL Data Warehouse je zvláštním typem databáze, která je navržena pro výkonné paralelní zpracování. Databáze je distribuovaná mezi více uzlů a dotazy zpracovává paralelně. SQL Data Warehouse má řídicí uzel, který orchestruje aktivitu všech uzlů. V samotných uzlech slouží ke správě dat služba SQL Database.  

> [!NOTE]
> Vytvoření služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.  Další informace najdete v tématu [SQL Data Warehouse – ceny](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).
>

### <a name="create-a-data-warehouse"></a>Vytvoření datového skladu

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Klikněte na **Nový** > **Databáze** > **SQL Data Warehouse**.

    ![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png)
    ![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)

3. Zadání podrobností o nasazení

    **Název databáze:** Výběr je zcela na vás. Pokud máte více datových skladů, doporučujeme do názvu zahrnout podrobnosti, jako je oblast a prostředí, například *mydw-westus-1-test*.

    **Předplatné:** Vaše předplatné Azure

    **Skupina prostředků:** Vytvořte skupinu prostředků nebo vyberte existující.
    > [!NOTE]
    > Skupiny prostředků jsou užitečné pro správu prostředků, jako je například určování rozsahu řízení přístupu a nasazení podle šablony. Další informace o skupinách prostředků Azure a osvědčených postupech si přečtete [zde](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).

    **Zdroj:** Prázdná databáze

    **Server:** Vyberte server, který jste vytvořili v části [Požadavky].

    **Kolace:** Ponechte výchozí kolaci SQL_Latin1_General_CP1_CI_AS.

    **Vybrat výkon:** Doporučujeme začít se standardním nastavením 400DWU.

4. Zvolte **Připnout na řídicí panel**
    ![Připnout na řídicí panel](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png).

5. Opřete se a počkejte, než se datový sklad nasadí! Je běžné, že tento proces trvá několik minut. Portál vás upozorní, jakmile bude váš datový sklad připraven k použití. 

## <a name="connect-to-sql-data-warehouse"></a>Připojení k SQL Data Warehouse

Tento kurz pro připojení k datovému skladu používá aplikaci SQL Server Management Studio (SSMS). Ke službě SQL Data Warehouse se můžete připojit prostřednictvím těchto podporovaných konektorů: ADO.NET, JDBC, ODBC a PHP. Mějte na paměti, že funkce nástrojů, které nejsou podporované Microsoftem, můžou být omezené.


### <a name="get-connection-information"></a>Získání informací o připojení

Chcete-li se připojit ke svému datovému skladu, je nutné se připojit přes logický SQL Server, který jste vytvořili v části [Požadavky].

1. Vyberte svůj datový sklad z řídicího panelu nebo jej vyhledejte ve vašich prostředcích.

    ![Řídicí panel SQL Data Warehouse](./media/sql-data-warehouse-get-started-tutorial/sql-dw-dashboard.png)

2. Vyhledejte úplný název pro logický SQL server.

    ![Výběr názvu serveru](./media/sql-data-warehouse-get-started-tutorial/select-server.png)

3. Otevřete SSMS a pomocí průzkumníku objektů se připojte k tomuto serveru prostřednictvím přihlašovacích údajů správce serveru, které jste vytvořili v části [Požadavky].

    ![Připojení přes SSMS](./media/sql-data-warehouse-get-started-tutorial/ssms-connect.png)

Pokud všechno proběhne správně, měli byste být teď připojeni ke svému logickému SQL Serveru. Vzhledem k tomu, že jste přihlášeni jako správce serveru, můžete se připojit k libovolné databázi hostované na daném serveru, a to včetně hlavní databáze. 

Existuje pouze jeden účet správce serveru a ze všech uživatelů má nejvíce oprávnění. Dejte pozor, aby ve vaší organizaci neznalo heslo správce příliš mnoho lidí. 

Můžete mít také účet správce Azure Active Directory. Podrobnosti zde neposkytujeme. Další informace o použití ověřování pomocí Azure Active Directory najdete v tématu [Ověřování pomocí Azure AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).

Dále se budeme věnovat vytváření dalších přihlašovacích údajů a uživatelů.


## <a name="create-a-database-user"></a>Vytvoření uživatele databáze

V tomto kroku vytvoříte uživatelský účet pro přístup k datovému skladu. Také vám ukážeme, jak dát takovému uživateli možnost spouštět dotazy s velkým množstvím paměťových a procesorových prostředků.

### <a name="notes-about-resource-classes-for-allocating-resources-to-queries"></a>Poznámky k třídám prostředků pro přidělování prostředků dotazům

- Z důvodu zabezpečení vašich dat nepoužívejte účet správce serveru ke spouštění dotazů na produkčních databázích. Ze všech uživatelů má nejvíce oprávnění a jeho použití k provádění operací na uživatelských datech tato data ohrožuje. Navíc vzhledem k tomu, že je správce serveru určen k provádění operací správy, spouští operace pouze s malým množstvím přidělených paměťových a procesorových prostředků. 

- SQL Data Warehouse používá předdefinované databázové role, označované jako třídy prostředků, k přidělování různých množství paměťových a procesorových prostředků a slotů souběžnosti jednotlivým uživatelům. Každý uživatel může patřit do malé, střední, velké nebo velmi velké třídy prostředků. Třída prostředků uživatele určuje, jaké prostředky má uživatel k dispozici pro spouštění dotazů a operací načítání.

- Z důvodu optimalizace komprese dat uživatel obvykle potřebuje načítání s velkým nebo velmi velkým přidělením prostředků. Další informace o třídách prostředků najdete [zde](./sql-data-warehouse-develop-concurrency.md#resource-classes):

### <a name="create-an-account-that-can-control-a-database"></a>Vytvoření účtu, který může řídit databázi

Vzhledem k tomu, že jste aktuálně přihlášeni jako správce serveru, máte oprávnění vytvářet účty a uživatele.

1. Pomocí SSMS nebo jiného klienta dotazů otevřete nový dotaz pro **master**.

    ![Nový dotaz na Master](./media/sql-data-warehouse-get-started-tutorial/query-on-server.png)

    ![Nový dotaz na Master1](./media/sql-data-warehouse-get-started-tutorial/query-on-master.png)

2. V okně dotazu spusťte tento příkaz T-SQL, který vytvoří účet MedRCLogin a uživatele LoadingUser. Tento účet se může připojit k logickému SQL serveru.

    ```sql
    CREATE LOGIN MedRCLogin WITH PASSWORD = 'a123reallySTRONGpassword!';
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

3. Nyní pomocí dotazu na *databázi SQL Data Warehouse* vytvořte uživatele databáze na základě účtu, který jste vytvořili pro přístup a provádění operací na databázi.

    ```sql
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

4. Udělte uživateli databáze oprávnění řídit databázi s názvem NYT. 

    ```sql
    GRANT CONTROL ON DATABASE::[NYT] to LoadingUser;
    ```
    > [!NOTE]
    > Pokud název databáze obsahuje pomlčky, uzavřete ho do hranatých závorek. 
    >

### <a name="give-the-user-medium-resource-allocations"></a>Udělení středního přidělení prostředků uživateli

1. Spuštěním tohoto příkazu T-SQL udělejte z uživatele člena střední třídy prostředků, která má název mediumrc. 

    ```sql
    EXEC sp_addrolemember 'mediumrc', 'LoadingUser';
    ```
    > [!NOTE]
    > Kliknutím [sem](sql-data-warehouse-develop-concurrency.md#resource-classes) zjistíte další informace o souběžnosti a třídách prostředků! 
    >

2. Připojení k logickému serveru pomocí nových přihlašovacích údajů

    ![Přihlášení pomocí nových přihlašovacích údajů](./media/sql-data-warehouse-get-started-tutorial/new-login.png)


## <a name="load-data-from-azure-blob-storage"></a>Načtení dat z Azure Blob Storage

Nyní jste připraveni k načtení dat do datového skladu. Tento krok ukazuje, jak načíst data taxislužby města New York z veřejného úložiště objektů blob v Azure. 

- Běžný způsob, jak načíst data do služby SQL Data Warehouse, je nejprve přesunout data do Azure Blob Storage, a pak je načíst do datového skladu. Pro snadnější pochopení načítání jsou data taxislužby města New York již hostována ve veřejném úložišti objektů blob v Azure. 

- Informace o přesunu dat do Azure Blob Storage nebo jejich načtení přímo ze zdroje do služby SQL Data Warehouse najdete pro budoucí použití v části s [přehledem načítání](sql-data-warehouse-overview-load.md).


### <a name="define-external-data"></a>Definování externích dat

1. Vytvořte hlavní klíč. Pro každou databázi je nutné vytvořit hlavní klíč pouze jednou. 

    ```sql
    CREATE MASTER KEY;
    ```

2. Zadejte umístění objektu blob Azure, který obsahuje data taxislužby.  

    ```sql
    CREATE EXTERNAL DATA SOURCE NYTPublic
    WITH
    (
        TYPE = Hadoop,
        LOCATION = 'wasbs://2013@nytpublic.blob.core.windows.net/'
    );
    ```

3. Definujte formáty externích souborů.

    Příkaz ```CREATE EXTERNAL FILE FORMAT``` slouží k určení formátu souborů obsahujících externí data. Obsahují text oddělený jedním nebo několika znaky, kterým se říká oddělovače. Pro účely ukázky jsou data taxislužby uložena jako nekomprimovaná data a jako komprimovaná data ve formátu GZip.

    Spuštěním těchto příkazů T-SQL nadefinujte dva různé formáty: nekomprimovaný a komprimovaný.

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

4.  Vytvořte schéma pro váš formát externích souborů. 

    ```sql
    CREATE SCHEMA ext;
    ```
5. Vytvořte externí tabulky. Tyto tabulky odkazují na data uložená v Azure Blob Storage. Spuštěním následujících příkazů T-SQL vytvořte několik externích tabulek odkazujících na objekt blob Azure, který jsme dříve definovali v externím zdroji dat.

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
    )
    ;
        
    
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
    )
    ;
    
    
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
    )
    ;
    
    CREATE EXTERNAL TABLE [ext].[Weather]
    (
        [DateID] int NOT NULL,
        [GeographyID] int NOT NULL,
        [PrecipitationInches] float NOT NULL,
        [AvgTemperatureFahrenheit] float NOT NULL
    )
    WITH
    (
        LOCATION = 'Weather2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    ```

### Import the data from Azure blob storage.

SQL Data Warehouse supports a key statement called CREATE TABLE AS SELECT (CTAS). This statement creates a new table based on the results of a select statement. The new table has the same columns and data types as the results of the select statement.  This is an elegant way to import data from Azure blob storage into SQL Data Warehouse.

1. Run this script to import your data.

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

2. View your data as it loads.

   You’re loading several GBs of data and compressing it into highly performant clustered columnstore indexes. Run the following query that uses a dynamic management views (DMVs) to show the status of the load. After starting the query, grab a coffee and a snack while SQL Data Warehouse does some heavy lifting.
    
    ```sql
    SELECT
        r.command,
        s.request_id,
        r.status,
        count(distinct input_name) as nbr_files,
        sum(s.bytes_processed)/1024/1024 as gb_processed
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

3. View all system queries.

    ```sql
    SELECT * FROM sys.dm_pdw_exec_requests;
    ```

4. Enjoy seeing your data nicely loaded into your Azure SQL Data Warehouse.

    ![See Data Loaded](./media/sql-data-warehouse-get-started-tutorial/see-data-loaded.png)


## Improve query performance

There are several ways to improve query performance and to achieve the high-speed performance that SQL Data Warehouse is designed to provide.  

### See the effect of scaling on query performance 

One way to improve query performance is to scale resources by changing the DWU service level for your data warehouse. Each service level costs more, but you can scale back or pause resources at any time. 

In this step, you compare performance at two different DWU settings.

First, let's scale the sizing down to 100 DWU so we can get an idea of how one compute node might perform on its own.

1. Go to the portal and select your SQL Data Warehouse.

2. Select scale in the SQL Data Warehouse blade. 

    ![Scale DW From portal](./media/sql-data-warehouse-get-started-tutorial/scale-dw.png)

3. Scale down the performance bar to 100 DWU and hit save.

    ![Scale and save](./media/sql-data-warehouse-get-started-tutorial/scale-and-save.png)

4. Wait for your scale operation to finish.

    > [!NOTE]
    > Queries cannot run while changing the scale. Scaling **kills** your currently running queries. You can restart them when the operation is finished.
    >
    
5. Do a scan operation on the trip data, selecting the top million entries for all the columns. If you're eager to move on quickly, feel free to select fewer rows. Take note of the time it takes to run this operation.

    ```sql
    SELECT TOP(1000000) * FROM dbo.[Trip]
    ```
6. Scale your data warehouse back to 400 DWU. Remember, each 100 DWU is adding another compute node to your Azure SQL Data Warehouse.

7. Run the query again! You should notice a significant difference. 

> [!NOTE]
> Since SQL Data Warehouse uses massively parallel processing. Queries that scan or perform analytic functions on millions of rows experience the true power of
> Azure SQL Data Warehouse.
>

### See the effect of statistics on query performance

1. Run a query that joins the Date table with the Trip table

    ```sql
    SELECT TOP (1000000) 
        dt.[DayOfWeek],
        tr.[MedallionID],
        tr.[HackneyLicenseID],
        tr.[PickupTimeID],
        tr.[DropoffTimeID],
        tr.[PickupGeographyID],
        tr.[DropoffGeographyID],
        tr.[PickupLatitude],
        tr.[PickupLongitude],
        tr.[PickupLatLong],
        tr.[DropoffLatitude],
        tr.[DropoffLongitude],
        tr.[DropoffLatLong],
        tr.[PassengerCount],
        tr.[TripDurationSeconds],
        tr.[TripDistanceMiles],
        tr.[PaymentType],
        tr.[FareAmount],
        tr.[SurchargeAmount],
        tr.[TaxAmount],
        tr.[TipAmount],
        tr.[TollsAmount],
        tr.[TotalAmount]
    FROM [dbo].[Trip] as tr
        JOIN dbo.[Date] as dt
        ON  tr.DateID = dt.DateID
    ```

    This query takes a while because SQL Data Warehouse has to shuffle data before it can perform the join. Joins do not have to shuffle data if they are designed to join data in the same way it is distributed. That's a deeper subject. 

2. Statistics make a difference. 
3. Run this statement to create statistics on the join columns.

    ```sql
    CREATE STATISTICS [dbo.Date DateID stats] ON dbo.Date (DateID);
    CREATE STATISTICS [dbo.Trip DateID stats] ON dbo.Trip (DateID);
    ```

    > [!NOTE]
    > SQL DW does not automatically manage statistics for you. Statistics are important for query
    > performance and it is highly recommended you create and update statistics.
    > 
    > **You gain the most benefit by having statistics on columns involved in joins, columns
    > used in the WHERE clause and columns found in GROUP BY.**
    >

3. Run the query from Prerequisites again and observe any performance differences. While the differences in query performance will not be as drastic as scaling up, you should notice a  speed-up. 

## Next steps

You're now ready to query and explore. Check out our best practices or tips.

If you're done exploring for the day, make sure to pause your instance! In production, you can experience enormous 
savings by pausing and scaling to meet your business needs.

![Pause](./media/sql-data-warehouse-get-started-tutorial/pause.png)

## Useful readings

[Concurrency and Workload Management][]

[Best practices for Azure SQL Data Warehouse][]

[Query Monitoring][]

[Top 10 Best Practices for Building a Large Scale Relational Data Warehouse][]

[Migrating Data to Azure SQL Data Warehouse][]

[Concurrency and Workload Management]: sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example
[Best practices for Azure SQL Data Warehouse]: sql-data-warehouse-best-practices.md#hash-distribute-large-tables
[Query Monitoring]: sql-data-warehouse-manage-monitor.md
[Top 10 Best Practices for Building a Large Scale Relational Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2013/09/16/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse/
[Migrating Data to Azure SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/



[!INCLUDE [Additional Resources](../../includes/sql-data-warehouse-article-footer.md)]

<!-- Internal Links -->
[Prerequisites]: sql-data-warehouse-get-started-tutorial.md#prerequisites

<!--Other Web references-->
[Visual Studio]: https://www.visualstudio.com/
[SQL Server Management Studio]: https://msdn.microsoft.com/en-us/library/mt238290.aspx

