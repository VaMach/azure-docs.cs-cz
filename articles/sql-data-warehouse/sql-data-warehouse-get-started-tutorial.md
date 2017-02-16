---
title: "Azure SQL Data Warehouse – Kurz Začínáme | Microsoft Docs"
description: "Kurz Začínáme s Azure SQL Data Warehouse"
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
ms.date: 12/21/2016
ms.author: elbutter
translationtype: Human Translation
ms.sourcegitcommit: 5bb75bf36892c737568b8129b30bb30b02d01bf2
ms.openlocfilehash: 1227903652a944d9d144917922fe36a4f149f820


---
# <a name="get-started-with-sql-data-warehouse"></a>Začínáme s SQL Data Warehouse

Kurz Začínáme s Azure SQL Data Warehouse V tomto kurzu se naučíte základy zřizování a načítání dat do SQL Data Warehouse a také se dozvíte některé základní informace o škálování, pozastavení a ladění. 

**Odhadovaný čas dokončení:** 75 minut

## <a name="prerequisites"></a>Požadavky


### <a name="sign-up-for-microsoft-azure"></a>Přihlášení k Microsoft Azure
Pokud ještě nemáte účet Microsoft Azure, musíte si jeden zaregistrovat, abyste mohli tuto službu používat. Pokud již máte účet, tento krok přeskočte. 

1. Přejděte na stránky účtu [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/).
2. Vytvořte si bezplatný účet Azure, nebo si účet zakupte.
3. Postupujte podle zobrazených pokynů.

### <a name="install-appropriate-sql-client-driver-and-tools"></a>Instalace odpovídajícího ovladače a nástrojů klienta SQL

Většina nástrojů klienta SQL se může připojit k Azure SQL Data Warehouse pomocí JDBC, ODBC nebo ADO.net. Z důvodu složitosti produktu a velkého počtu funkcí T-SQL, které SQL Data Warehouse podporuje, nemusí být každá klientská aplikace s SQL Data Warehouse plně kompatibilní.

Pokud používáte operační systém Windows, doporučujeme použít buď [Visual Studio], nebo [SQL Server Management Studio].


[!INCLUDE [Create a new logical server](../../includes/sql-data-warehouse-create-logical-server.md)] 

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="create-an-azure-sql-data-warehouse"></a>Vytvoření Azure SQL Data Warehouse

> [!NOTE]
> Vytvoření služby SQL Data Warehouse může znamenat, že se vám začne fakturovat nová služba.  Další informace najdete v tématu [SQL Data Warehouse – ceny](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).
>


### <a name="create-a-sql-data-warehouse"></a>Vytvoření SQL Data Warehouse
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Klikněte na **Nový** > **Databáze** > **SQL Data Warehouse**.

    ![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png)
    ![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)

3. Zadání podrobností o nasazení

    **Název databáze:** Výběr je zcela na vás. Pokud máte více instancí SQL DW, doporučujeme do názvu zahrnout podrobnosti, jako je oblast, prostředí apod., například *mydw-westus-1-test*.

    **Předplatné:** Vaše předplatné Azure

    **Skupina prostředků:** Vytvořte novou (nebo použijte existující, pokud máte v úmyslu používat Azure SQL Data Warehouse s jinými službami).
    > [!NOTE]
    > Služby v rámci skupiny prostředků musí mít stejný životní cyklus. Skupiny prostředků jsou užitečné pro správu prostředků, jako je například určování rozsahu řízení přístupu a nasazení podle šablony. Další informace o skupinách prostředků Azure a osvědčených postupech si přečtete [zde](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).
    >

    **Zdroj:** Prázdná databáze

    **Server:** Vyberte server, který jste vytvořili v části [Požadavky].

    **Kolace:** Ponechte výchozí kolaci SQL_Latin1_General_CP1_CI_AS.

    **Vybrat výkon:** Doporučujeme použít standardní nastavení 400DWU.

4. Zvolte **Připnout na řídicí panel**
    ![Připnout na řídicí panel](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png).

5. Opřete se a počkejte, než se Azure SQL Data Warehouse nasadí. Je běžné, že tento proces trvá několik minut. Portál vás na dokončení nasazení instanci upozorní. 

## <a name="connect-to-azure-sql-data-warehouse-through-sql-server-logical-server"></a>Připojení k Azure SQL Data Warehouse pomocí SQL Serveru (logický server)

Tento kurz pro připojení k SQL Data Warehouse používá SQL Server Management Studio. Lze použít i další nástroje pro připojení k SQL Data Warehouse pomocí našich podporovaných konektorů: ADO.NET, JDBC, ODBC a PHP. Mějte na paměti, že funkce nástrojů, které nejsou podporované Microsoftem, můžou být omezené.


### <a name="get-connection-information"></a>Získání informací o připojení

Chcete-li se připojit k SQL Data Warehouse, je nutné se připojit přes SQL Server (logický server), který jste vytvořili v části [Požadavky].

1. Vyberte svou službu SQL Data Warehouse z řídicího panelu nebo ji vyhledejte ve vašich prostředcích.

    ![Řídicí panel SQL Data Warehouse](./media/sql-data-warehouse-get-started-tutorial/sql-dw-dashboard.png)

2. Vyhledejte úplný název pro logický server.

    ![Výběr názvu serveru](./media/sql-data-warehouse-get-started-tutorial/select-server.png)

3. Otevřete SSMS a pomocí průzkumníku objektů se připojte k tomuto serveru prostřednictvím přihlašovacích údajů, které jste vytvořili v části [Požadavky].

    ![Připojení přes SSMS](./media/sql-data-warehouse-get-started-tutorial/ssms-connect.png)

Pokud všechno proběhne správně, měli byste být teď připojení ke své instanci SQL Serveru (logický server). Můžete použít přihlašovací údaje serveru k ověření a přihlášení k libovolné databázi na daném serveru jako vlastník databáze. Obvyklým postupem ale je vytvořit pro každou databázi samostatné přihlašovací údaje. Postup vytvoření uživatele najdete v části [Vytvoření uživatele pro SQL Data Warehouse](./sql-data-warehouse-get-started-tutorial.md#create-a-user-for-sql-data-warehouse). 

## <a name="create-a-user-for-sql-data-warehouse"></a>Vytvoření uživatele pro SQL Data Warehouse

### <a name="why-create-a-separate-user"></a>Proč vytvářet samostatného uživatele?

K vytvoření nového uživatele pro SQL Data Warehouse používáme připojení k SQL Serveru (logický server) s přihlašovacími údaji serveru z předchozího kroku. Existují dva hlavní důvody, proč můžete chtít vytvořit samostatné přihlašovací údaje pro SQL DW.

1.  Uživatelé ve vaší organizaci by měli používat jiný účet k ověření. Tímto způsobem můžete omezit oprávnění udělená aplikaci a snížit rizika škodlivých aktivit.

2. Ve výchozím nastavení používá přihlášení správce serveru, pomocí kterého jste aktuálně připojení, menší třídu prostředků. Třídy prostředků pomáhají řídit přidělování paměti a cyklů procesoru pro dotaz. Uživatelům ve třídě **smallrc** je dáno menší množství paměti a můžou využít výhod vyšší souběžnosti. Naproti tomu uživatelům přiřazeným ke třídě **xlargerc** je dáno velké množství paměti, a proto může být souběžně spuštěno méně jejich dotazů. Aby se data načítala způsobem, který nejlépe optimalizuje kompresi, je potřeba se ujistit, že uživatel načítající data je součástí větší třídy prostředků. Další informace o třídách prostředků najdete [zde](./sql-data-warehouse-develop-concurrency.md#resource-classes):

### <a name="creating-a-user-of-a-larger-resource-class"></a>Vytvoření uživatele větší třídy prostředků

1. Vytvořte nový dotaz na **hlavní** databázi vašeho serveru.

    ![Nový dotaz na Master](./media/sql-data-warehouse-get-started-tutorial/query-on-server.png)

    ![Nový dotaz na Master1](./media/sql-data-warehouse-get-started-tutorial/query-on-master.png)

2. Vytvořte přihlášení a uživatele serveru.

    ```sql
    CREATE LOGIN XLRCLOGIN WITH PASSWORD = 'a123reallySTRONGpassword!';
    CREATE USER LoadingUser FOR LOGIN XLRCLOGIN;
    ```

3. Vytvořte nového uživatele databáze na základě přihlášení serveru.
    ```sql
    CREATE USER LoadingUser FOR LOGIN XLRCLOGIN;
    ```

4. Udělte uživateli oprávnění k řízení databáze.
    ```sql
    GRANT CONTROL ON DATABASE::[NYT] to LoadingUser;
    ```
    > [!NOTE]
    > Pokud název databáze obsahuje pomlčky, uzavřete ho do hranatých závorek. 
    >

5. Přidejte uživatele databáze do role třídy prostředků **xlargerc**.
    ```sql
    EXEC sp_addrolememeber 'xlargerc', 'LoadingUser';
    ```

6. Přihlaste se k vaší databáze pomocí nových přihlašovacích údajů.

    ![Přihlášení pomocí nových přihlašovacích údajů](./media/sql-data-warehouse-get-started-tutorial/new-login.png)


## <a name="loading-data"></a>Načítání dat

### <a name="defining-external-data"></a>Definování externích dat
1. Definujte externí zdroj dat.

    ```sql
    CREATE EXTERNAL DATA SOURCE NYTPublic
    WITH
    (
    TYPE = Hadoop
    , LOCATION = 'wasbs://2013@nytpublic.blob.core.windows.net/'
    );
    ```


2. Definujte formáty externích souborů.

    Příkaz ```CREATE EXTERNAL FILE FORMAT``` se používá k určení formátu externích dat, ze kterých provádíte načítání. Pro veřejná data taxislužby města New York jsme použili dva formáty pro ukládání dat do Azure Blob Storage.

    ```sql
    CREATE EXTERNAL FILE FORMAT uncompressedcsv
    WITH
    ( FORMAT_TYPE = DELIMITEDTEXT
    , FORMAT_OPTIONS ( FIELD_TERMINATOR = ','
    , STRING_DELIMITER = ''
    , DATE_FORMAT = ''
    , USE_TYPE_DEFAULT = False
    )
    );

    CREATE EXTERNAL FILE FORMAT compressedcsv
    WITH
    ( FORMAT_TYPE = DELIMITEDTEXT
    , FORMAT_OPTIONS ( FIELD_TERMINATOR = '|'
    , STRING_DELIMITER = ''
    , DATE_FORMAT = ''
    , USE_TYPE_DEFAULT = False
    )
    , DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
    );
    ```

3.  Vytvořte schéma pro váš formát externích souborů.

    ```sql
    CREATE SCHEMA ext;
    GO
    ```

4. Vytvořte externí tabulky. Tyto tabulky odkazují na data uložená v HDFS nebo Azure Blob Storage. 

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
    LOCATION = 'Date'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
    )
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
    LOCATION = 'Geography'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0 
    )
    ;
    CREATE EXTERNAL TABLE [ext].[HackneyLicense]
    (
    [HackneyLicenseID] int NOT NULL,
    [HackneyLicenseBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
    [HackneyLicenseCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
    LOCATION = 'HackneyLicense'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
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
    LOCATION = 'Medallion'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
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
    LOCATION = 'Time'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
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
    LOCATION = 'Trip2013'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = compressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
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
    LOCATION = 'Weather2013'
    , DATA_SOURCE = NYTPublic
    , FILE_FORMAT = uncompressedcsv
    , REJECT_TYPE = value
    , REJECT_VALUE = 0
    )
    ;
    ```

### <a name="create-table-as-select-ctas"></a>CREATE TABLE AS SELECT (CTAS)

5. Načtěte data z externí tabulky do vaší instanci SQL Data Warehouse. 
    ```sql
    CREATE TABLE [dbo].[Date]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Date]
    OPTION (LABEL = 'CTAS : Load [dbo].[Date]')
    ;
    CREATE TABLE [dbo].[Geography]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Geography]
    OPTION (LABEL = 'CTAS : Load [dbo].[Geography]')
    ;
    CREATE TABLE [dbo].[HackneyLicense]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[HackneyLicense]
    OPTION (LABEL = 'CTAS : Load [dbo].[HackneyLicense]')
    ;
    CREATE TABLE [dbo].[Medallion]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Medallion]
    OPTION (LABEL = 'CTAS : Load [dbo].[Medallion]')
    ;
    CREATE TABLE [dbo].[Time]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Time]
    OPTION (LABEL = 'CTAS : Load [dbo].[Time]')
    ;
    CREATE TABLE [dbo].[Weather]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Weather]
    OPTION (LABEL = 'CTAS : Load [dbo].[Weather]')
    ;
    CREATE TABLE [dbo].[Trip]
    WITH
    ( DISTRIBUTION = ROUND_ROBIN
    , CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT *
    FROM [ext].[Trip]
    OPTION (LABEL = 'CTAS : Load [dbo].[Trip]')
    ;
    ```

    > [!NOTE]
    > Provedete načtení několika GB dat a jejich kompresi do vysoce výkonných indexů Columnstore clusteru. Spusťte následující dotaz DMV, a zatímco Azure SQL Data Warehouse bude dělat namáhavou práci, udělejte si přestávku na kávu.
    >

6. Vytvořte nový dotaz a sledujte pomocí tohoto zobrazení dynamické správy (DMV), jak vaše data přibývají.

    ```sql
    SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files,
    sum(s.bytes_processed)/1024/1024 as gb_processed
    FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
    on r.request_id = s.request_id
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
    nbr_files desc, gb_processed desc;
    ```

7. Zobrazte všechny systémové dotazy.

    ```sql
    SELECT * FROM sys.dm_pdw_exec_requests;
    ```

8. Užívejte si pohled na to, jak se data do Azure SQL Data Warehouse krásně načítají.

    ![Zobrazení načtených data](./media/sql-data-warehouse-get-started-tutorial/see-data-loaded.png)



## <a name="querying-data"></a>Dotazování na data 

### <a name="scan-query-with-scaling"></a>Zkoumání dotazu pomocí škálování

Udělejte si představu o tom, jak škálování ovlivňuje rychlost vašich dotazů.

Než začneme, vertikálně snížíme kapacitu pro naši operaci na 100 DWU, abychom získali představu, jak může pracovat samostatný výpočetní uzel.

1. Přejděte na portál a vyberte instanci SQL Data Warehouse.

2. Vyberte horizontální snížení kapacity v okně SQL Data Warehouse. 

    ![Škálování DW z portálu](./media/sql-data-warehouse-get-started-tutorial/scale-dw.png)

3. Vertikálně snižte kapacitu pomocí pruhu výkonu na 100 DWU a stiskněte tlačítko Uložit.

    ![Škálování a uložení](./media/sql-data-warehouse-get-started-tutorial/scale-and-save.png)

4. Počkejte na dokončení operace škálování.

    > [!NOTE]
    > Uvědomte si prosím, že operace škálování **ukončí** vaše aktuálně spuštěné dotazy a zabrání spuštění nových.
    >
    
5. Proveďte operaci prohledávání dat o cestách tak, že vyberete prvních milion položek pro všechny sloupce. Pokud chcete postupovat rychleji, klidně vyberte menší počet řádků.

    ```sql
    SELECT TOP(1000000) * FROM dbo.[Trip]
    ```

Poznamenejte si dobu potřebnou k provedení této operace.

6. Vertikálně navyšte kapacitu instance na 400 DWU. Pamatujte si, že každých 100 DWU přidává do Azure SQL Data Warehouse další výpočetní uzel.

7. Spusťte dotaz znovu. Měli byste zaznamenat značný rozdíl. 

> [!NOTE]
> Řešení Azure SQL Data Warehouse je postaveno na platformě MPP (Massively Parallel Processing). Dotazy a operace, které mohou provádět práci paralelně na různých uzlech, využívají skutečný potenciál Azure SQL Data Warehouse.
>

### <a name="join-query-with-statistics"></a>Dotaz spojení se statistikami

1. Spusťte dotaz, který spojuje tabulku Date s tabulkou Trip.

    ```sql
    SELECT TOP (1000000) dt.[DayOfWeek]
    ,tr.[MedallionID]
    ,tr.[HackneyLicenseID]
    ,tr.[PickupTimeID]
    ,tr.[DropoffTimeID]
    ,tr.[PickupGeographyID]
    ,tr.[DropoffGeographyID]
    ,tr.[PickupLatitude]
    ,tr.[PickupLongitude]
    ,tr.[PickupLatLong]
    ,tr.[DropoffLatitude]
    ,tr.[DropoffLongitude]
    ,tr.[DropoffLatLong]
    ,tr.[PassengerCount]
    ,tr.[TripDurationSeconds]
    ,tr.[TripDistanceMiles]
    ,tr.[PaymentType]
    ,tr.[FareAmount]
    ,tr.[SurchargeAmount]
    ,tr.[TaxAmount]
    ,tr.[TipAmount]
    ,tr.[TollsAmount]
    ,tr.[TotalAmount]
    FROM [dbo].[Trip] as tr
    join
    dbo.[Date] as dt
    on tr.DateID = dt.DateID
    ```

    Podle očekávání dotaz trvá mnohem déle, když přesouváte data mezi uzly, zejména v případě takovéhoto scénáře spojení.

2. Podívejme se, jak se to změní, když vytvoříme statistiky pro spojovaný sloupec spuštěním následujícího kódu:

    ```sql
    CREATE STATISTICS [dbo.Date DateID stats] ON dbo.Date (DateID);
    CREATE STATISTICS [dbo.Trip DateID stats] ON dbo.Trip (DateID);
    ```

    > [!NOTE]
    > SQL DW nespravuje statistiky automaticky. Statistiky jsou důležité pro výkon dotazů a důrazně se doporučuje statistiky vytvářet a aktualizovat.
    > 
    > **Nejvíce výhod získáte tak, že budete mít statistiky pro sloupce používané ve spojeních, sloupce používané v klauzuli WHERE a sloupce používané v příkazu GROUP BY.**
    >

3. Znovu spusťte dotaz z části Požadavky a sledujte rozdíly ve výkonu. I když rozdíly ve výkonu dotazů nebudou tak drastické jako při vertikálním navýšení kapacity, měli byste zaznamenat viditelné zrychlení. 

## <a name="next-steps"></a>Další kroky

Teď jste připravení na dotazování a prozkoumávání. Vyzkoušejte si naše osvědčené postupy a tipy.

Pokud jste už pro dnešek se zkoumáním hotovi, nezapomeňte svoji instanci pozastavit. V produkčním prostředí můžete pozastavením a škálováním podle svých obchodních potřeb zaznamenat značné úspory.

![Pozastavení](./media/sql-data-warehouse-get-started-tutorial/pause.png)

## <a name="useful-readings"></a>Užitečné informace k přečtení

[Souběžnost a správa úloh]

[Osvědčené postupy pro službu Azure SQL Data Warehouse]

[Monitorování dotazů]

[10 nejlepších osvědčených postupů pro sestavení rozsáhlého relačního datového skladu]

[Migrace dat do Azure SQL Data Warehouse]


[Souběžnost a správa úloh]: sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example
[Osvědčené postupy pro službu Azure SQL Data Warehouse]: sql-data-warehouse-best-practices.md#hash-distribute-large-tables
[Monitorování dotazů]: sql-data-warehouse-manage-monitor.md
[10 nejlepších osvědčených postupů pro sestavení rozsáhlého relačního datového skladu]: https://blogs.msdn.microsoft.com/sqlcat/2013/09/16/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse/
[Migrace dat do Azure SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/



[!INCLUDE [Additional Resources](../../includes/sql-data-warehouse-article-footer.md)]

<!-- Internal Links -->
[Požadavky]: sql-data-warehouse-get-started-tutorial.md#prerequisites

<!--Other Web references-->
[Visual Studio]: https://www.visualstudio.com/
[SQL Server Management Studio]: https://msdn.microsoft.com/en-us/library/mt238290.aspx



<!--HONumber=Jan17_HO2-->


