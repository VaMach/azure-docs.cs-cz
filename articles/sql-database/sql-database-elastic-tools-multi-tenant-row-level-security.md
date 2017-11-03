---
title: "Víceklientské aplikací pomocí nástroje elastické databáze a zabezpečení na úrovni řádků"
description: "Používejte nástroje elastické databáze pomocí zabezpečení na úrovni řádků pro vytvoření aplikace s vysoce škálovatelné datové vrstvy."
metakeywords: azure sql database elastic tools multi tenant row level security rls
services: sql-database
documentationcenter: 
manager: jhubbard
author: tmullaney
ms.assetid: e72d3cfe-e9be-4326-b776-9c6d96c0a18e
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: thmullan;torsteng
ms.openlocfilehash: 4b4ec29bb53bdce413dadf7cb0751433b9ca686c
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a>Víceklientské aplikací pomocí nástroje elastické databáze a zabezpečení na úrovni řádků
[Nástroje elastické databáze](sql-database-elastic-scale-get-started.md) a [nízkoúrovňového zabezpečení (RLS)](https://msdn.microsoft.com/library/dn765131) nabízejí výkonnou sadu funkcí pro flexibilní a efektivní škálování datovou vrstvu víceklientské aplikace s Azure SQL Database. Další informace najdete v tématu [návrhová schémata pro víceklientské aplikace SaaS ve službě Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) Další informace. 

Tento článek ukazuje, jak použít tyto technologie společně k vytvoření aplikace s vysoce škálovatelné datové vrstvy, která podporuje více klientů horizontálních oddílů, pomocí **ADO.NET SqlClient** nebo **Entity Framework**.  

* **Nástroje elastické databáze** umožňuje vývojářům škálovat vrstvu data aplikace prostřednictvím horizontálního dělení standardní postupy, pomocí sady knihovny .NET a šablony služeb Azure. Správa horizontálních oddílů s použitím elastické databáze klientské knihovny pomáhá zautomatizovat a zjednodušit řadu infrastruktury úlohy, které jsou obvykle spojené s horizontálního dělení. 
* **Zabezpečení na úrovni řádků** vývojářům umožňuje ukládání dat pro více klientů ve stejné databázi pomocí zásad zabezpečení pomocí filtrů řádky, které nepatří do klienta provádění dotazu. Centralizuje logiku přístup s RLS v databázi, nikoli v aplikaci, zjednodušuje údržbu a snižuje riziko chyby, jako základu kódu aplikace zvětšování. 

Používání těchto funkcí společně, aplikace mohou těžit z výhod zvýšení úspory a efektivitu nákladů uložením dat pro více klientů ve stejné ID horizontálního oddílu databázi. Ve stejnou dobu má aplikace stále flexibilitu a nabídnout izolovaný, jednoho klienta horizontálních oddílů pro "premium" klienty, kteří vyžadují přísnější záruky výkonu vzhledem k tomu, že více klientů horizontálních oddílů není zaručeno distribuce rovna prostředků mezi klienty.  

Stručně řečeno, elastického databáze klientské knihovny [závislé na data směrování](sql-database-elastic-scale-data-dependent-routing.md) rozhraní API pro klienty automaticky připojit k databázi správný horizontálních obsahující jejich horizontálního dělení klíč (obecně "TenantId"). Po připojení, zásady zabezpečení RLS v rámci databáze zajišťuje, aby klienti můžete pouze přístup k řádky, které obsahují jejich TenantId. Předpokládá se, že všechny tabulky obsahovat sloupec TenantId k označení, které řádky patří do každého klienta. 

![Architektura aplikace blogu][1]

## <a name="download-the-sample-project"></a>Stáhněte si ukázkový projekt
### <a name="prerequisites"></a>Požadavky
* Použijte sadu Visual Studio (2012 nebo vyšší) 
* Vytvořte tři databáze SQL Azure 
* Stáhněte si ukázkový projekt: [elastické databáze nástroje pro Azure SQL - víceklientské horizontálních oddílů](http://go.microsoft.com/?linkid=9888163)
  * Zadejte informace pro vaše databáze na začátku **Program.cs** 

Rozšiřuje popsané v tomto projektu [elastické databáze nástroje pro Azure SQL - Entity Framework integrace](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) přidáním podpory pro víc klientů horizontálních databáze. Sestavuje jednoduché konzolové aplikace pro vytvoření blogy a příspěvky, s čtyři klientů a dvě databáze víceklientské horizontálního oddílu, jak je znázorněno na předchozím obrázku. 

Sestavte a spusťte aplikaci. Tato bootstraps správce mapy horizontálního oddílu nástroje elastické databáze a spustí následující testy: 

1. Pomocí rozhraní Entity Framework a LINQ, vytvořte nový blog a následně se zobrazí všechny blogy pro každého klienta
2. Pomocí ADO.NET SqlClient, zobrazte všechny blogy pro klienta
3. Pokuste se vložit blogu pro chybný klienta k ověření, že je vyvolána chyba  

Všimněte si, že vzhledem k tomu, že RLS dosud nebyla spuštěna v databázích horizontálního oddílu, každý z těchto testů zjistí problém: Klienti mohou vidět blogy, který nepatří do nich a aplikace není zabránila vkládání blogu pro chybný klienta. Zbývající část tohoto článku popisuje, jak vyřešit tyto problémy vynucování izolace klienta s RLS. Existují dva kroky: 

1. **Aplikační vrstvě**: Upravit kód aplikace do vždy aktuální TenantId SESSION_CONTEXT po otevření připojení. Ukázkový projekt má neudělali. 
2. **Datová vrstva**: vytvoření zásady zabezpečení RLS v každé databázi horizontálního oddílu filtrování řádků podle TenantId uložené v SESSION_CONTEXT. Musíte to provést pro každý z vaší horizontálního oddílu databáze, jinak hodnota řádků v víceklientské nefiltrují horizontálních oddílů. 

## <a name="step-1-application-tier-set-tenantid-in-the-sessioncontext"></a>Krok 1) aplikační vrstvě: nastavte TenantId v SESSION_CONTEXT
Po připojení k databázi horizontálního oddílu pomocí klientské knihovny elastické databáze závislé na data směrování API, stále potřebám aplikace s oznámením databáze, které TenantId používá toto připojení tak, aby zásady zabezpečení RLS můžete filtrovat řádky, které patří do ostatních klientů. Je doporučeným způsobem předejte tyto informace uložit aktuální TenantId pro toto připojení v [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx). (Poznámka: můžete alternativně použít [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), ale SESSION_CONTEXT je lepší volbou protože je jednodušší použít, vrátí hodnotu NULL ve výchozím nastavení a podporuje páry klíč hodnota.)

### <a name="entity-framework"></a>Entity Framework
Pro aplikace používající rozhraní Entity Framework, je nejjednodušší způsob nastavit SESSION_CONTEXT v rámci ElasticScaleContext přepsání popsané v [závislé na Data směrování pomocí EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext). Před vrácením připojení zprostředkované přes závislé na data směrování, vytvoření a provedení SqlCommand, který nastaví 'TenantId' v SESSION_CONTEXT k shardingKey zadaný pro toto připojení. Tímto způsobem, stačí jednou napsat kód, chcete-li nastavit SESSION_CONTEXT. 

```
// ElasticScaleContext.cs 
// ... 
// C'tor for data-dependent routing. This call opens a validated connection routed to the proper 
// shard by the shard map manager. Note that the base class c'tor call fails for an open connection 
// if migrations need to be done and SQL credentials are used. This is the reason for the  
// separation of c'tors into the DDR case (this c'tor) and the internal c'tor for new shards. 
public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
    : base(OpenDDRConnection(shardMap, shardingKey, connectionStr), true /* contextOwnsConnection */)
{
}

public static SqlConnection OpenDDRConnection(ShardMap shardMap, T shardingKey, string connectionStr)
{
    // No initialization
    Database.SetInitializer<ElasticScaleContext<T>>(null);

    // Ask shard map to broker a validated connection for the given key
    SqlConnection conn = null;
    try
    {
        conn = shardMap.OpenConnectionForKey(shardingKey, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey to enable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", shardingKey);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
} 
// ... 
```

Nyní SESSION_CONTEXT automaticky nastavena zadaný TenantId vždy, když je volána ElasticScaleContext: 

```
// Program.cs 
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))   
    {     
        var query = from b in db.Blogs
                    orderby b.Name
                    select b;

        Console.WriteLine("All blogs for TenantId {0}:", tenantId);     
        foreach (var item in query)     
        {       
            Console.WriteLine(item.Name);     
        }   
    } 
}); 
```

### <a name="adonet-sqlclient"></a>ADO.NET SqlClient
Doporučený postup pro aplikace pomocí ADO.NET SqlClient, je vytvoření funkce obálku kolem ShardMap.OpenConnectionForKey(), který automaticky nastaví 'TenantId' v SESSION_CONTEXT na správné TenantId před vrácením připojení. Aby se zajistilo, že SESSION_CONTEXT je vždycky nastavený, spustíte pouze připojení pomocí této funkce obálku.

```
// Program.cs
// ...

// Wrapper function for ShardMap.OpenConnectionForKey() that automatically sets SESSION_CONTEXT with the correct
// tenantId before returning a connection. As a best practice, you should only open connections using this 
// method to ensure that SESSION_CONTEXT is always set before executing a query.
public static SqlConnection OpenConnectionForTenant(ShardMap shardMap, int tenantId, string connectionStr)
{
    SqlConnection conn = null;
    try
    {
        // Ask shard map to broker a validated connection for the given key
        conn = shardMap.OpenConnectionForKey(tenantId, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey to enable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", tenantId);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
}

// ...

// Example query via ADO.NET SqlClient
// If row-level security is enabled, only Tenant 4's blogs are listed
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (SqlConnection conn = OpenConnectionForTenant(sharding.ShardMap, tenantId4, connStrBldr.ConnectionString))
    {
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"SELECT * FROM Blogs";

        Console.WriteLine("--\nAll blogs for TenantId {0} (using ADO.NET SqlClient):", tenantId4);
        SqlDataReader reader = cmd.ExecuteReader();
        while (reader.Read())
        {
            Console.WriteLine("{0}", reader["Name"]);
        }
    }
});

```

## <a name="step-2-data-tier-create-row-level-security-policy"></a>Krok 2) Data vrstvy: vytvoření zásad zabezpečení na úrovni řádků
### <a name="create-a-security-policy-to-filter-the-rows-each-tenant-can-access"></a>Vytvoření zásady zabezpečení pro filtrování řádky, které můžete přístup každý klient
Teď, když aplikace je před odesláním dotazu nastavení SESSION_CONTEXT s aktuální TenantId, zásady zabezpečení RLS můžete filtrovat dotazy a vyloučit řádky, které mají různé TenantId.  

RLS je implementovaná v T-SQL: uživatelem definované funkce definuje logiku přístupu a zásady zabezpečení sváže tuto funkci Libovolný počet tabulek. Pro tento projekt funkce ověří, že aplikaci (namísto jiný uživatel SQL) je připojen k databázi, a že, TenantId, jsou uložena v SESSION_CONTEXT odpovídá TenantId daného řádku. Predikát filtru umožňuje řádky, které splňují tyto podmínky pro ve filtru pro dotazy SELECT, UPDATE a DELETE; a predikát block brání řádků, které porušují tyto podmínky z vložené nebo aktualizované. Pokud SESSION_CONTEXT nebyla nastavena, vrátí hodnotu NULL a žádné řádky jsou viditelné nebo možné vložit. 

Pokud chcete povolit RLS, spustit následující T-SQL na všechny horizontálních oddílů pomocí Visual Studio (SSDT), aplikace SSMS nebo ve skriptu PowerShell, který je zahrnutý v projektu (nebo pokud používáte [elastické databáze úlohy](sql-database-elastic-jobs-overview.md), které můžete použít k automatizaci provádění tohoto T-SQL na všechny horizontálních oddílů): 

```
CREATE SCHEMA rls -- separate schema to organize RLS objects 
GO

CREATE FUNCTION rls.fn_tenantAccessPredicate(@TenantId int)     
    RETURNS TABLE     
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult          
        WHERE DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- the user in your application’s connection string (dbo is only for demo purposes!)         
        AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
GO

CREATE SECURITY POLICY rls.tenantAccessPolicy
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts
GO 
```

> [!TIP]
> Pro složitější projekty, které je třeba přidat predikát na stovky tabulky můžete pomocná uložené procedury, která automaticky generuje zásadu zabezpečení přidání predikát ve všech tabulkách ve schématu. Další informace najdete v tématu [použití zabezpečení na úrovni řádků pro všechny tabulky – Pomocník skriptu (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).  
> 
> 

Pokud spustíte ukázkovou aplikaci znovu, nyní tenantům v pouze řádky, které patří k nim. Kromě toho aplikace nelze vložit řádky, které patří do klientů jiného, než jaké aktuálně připojené k databázi horizontálního oddílu a nemůže se aktualizovat viditelné řádky, které mají různé TenantId. Pokud se aplikace pokusí udělat, je vyvolána DbUpdateException.

Pokud později na přidáte nové tabulky, změnit zásady zabezpečení a přidejte filtr a blokovat predikáty v nové tabulce: 

```
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable
GO 
```

### <a name="add-default-constraints-to-automatically-populate-tenantid-for-inserts"></a>Přidat výchozí omezení automatické vyplnění TenantId pro vložení
Můžete vložit výchozího omezení na každou tabulku automaticky naplnit hodnoty TenantId s hodnotou, které jsou aktuálně uloženy ve SESSION_CONTEXT při vložení řádků. Například: 

```
-- Create default constraints to auto-populate TenantId with the value of SESSION_CONTEXT for inserts 
ALTER TABLE Blogs     
    ADD CONSTRAINT df_TenantId_Blogs      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO

ALTER TABLE Posts     
    ADD CONSTRAINT df_TenantId_Posts      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO 
```

Aplikaci teď není nutné zadávat TenantId při vložení řádků: 

```
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))
    {
        var blog = new Blog { Name = name }; // default constraint sets TenantId automatically     
        db.Blogs.Add(blog);     
        db.SaveChanges();   
    } 
}); 
```

> [!NOTE]
> Pokud používáte výchozí omezení pro projekt Entity Framework, se doporučuje nezahrnujte do datového modelu EF sloupci TenantId. Je to proto, že automaticky zadat výchozí hodnoty, které potlačí výchozí omezení vytvořené v T-SQL, které používají SESSION_CONTEXT dotazy na Entity Framework. V projektu vzorku použít výchozí omezení, například doporučujeme odebrat TenantId z DataClasses.cs (a spuštění Add-Migration v konzole Správce balíčků) a zkontrolujte, zda pole existuje pouze v tabulkách databáze pomocí T-SQL. Tímto způsobem EF automaticky zadat nesprávné výchozí hodnoty při vkládání dat. 
> 
> 

### <a name="optional-enable-a-superuser-to-access-all-rows"></a>(Volitelné) Povolit "superuživatele" pro přístup k všechny řádky
Některé aplikace může chtít vytvořit "superuživatele" kdo má přístup všechny řádky, například za účelem povolení generování sestav mezi všechny klienty na všechny horizontálních oddílů, nebo k provádění operací rozdělení či sloučení na horizontálních oddílů, které vyžadují přesunutí řádků klienta mezi databázemi. Chcete-li povolit, měli byste vytvořit nového uživatele SQL ("superuživatel" v tomto příkladu) v každé databázi horizontálního oddílu. Potom změňte zásady zabezpečení s novou predikátem funkci, která umožňuje tento uživatel pro přístup k všechny řádky:

```
-- New predicate function that adds superuser logic
CREATE FUNCTION rls.fn_tenantAccessPredicateWithSuperUser(@TenantId int)
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult 
        WHERE 
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- note, should not be dbo!
            AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
        ) 
        OR
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('superuser')
        )
GO

-- Atomically swap in the new predicate function on each table
ALTER SECURITY POLICY rls.tenantAccessPolicy
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts
GO
```


### <a name="maintenance"></a>Údržby
* **Přidání nové horizontálních oddílů**: spuštění skriptu T-SQL povolit RLS na všechny nové horizontálních oddílů, jinak nejsou filtrovány dotazy na tyto horizontálních oddílů.
* **Přidání nové tabulky**: přidejte nefiltrují predikát filtru a blokování do zásad zabezpečení na všechny horizontálních oddílů vždy, když je vytvořena nová tabulka, jinak hodnota dotazy na novou tabulku. To je možné automatizovat pomocí aktivační událost jazyka DDL, jak je popsáno v [použití zabezpečení na úrovni řádků automaticky do nově vytvořené tabulky (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).

## <a name="summary"></a>Souhrn
Nástroje elastické databáze a zabezpečení na úrovni řádků může být společně slouží jako škálování aplikace datové vrstvy s podporou pro oba více klientů a jednoho klienta horizontálních oddílů. Víceklientské horizontálních oddílů lze použít k ukládání dat efektivněji (obzvláště v případech, kdy velký počet klientů, které mají jenom pár řádků dat), při jednoho klienta horizontálních oddílů lze použít pro podporu klientů premium s přísnější výkon a izolace požadavky.  Další informace najdete v tématu [zabezpečení na úrovni řádků odkaz](https://msdn.microsoft.com/library/dn765131). 

## <a name="additional-resources"></a>Další zdroje
* [Co je Azure elastickém fondu?](sql-database-elastic-pool.md)
* [Horizontální navýšení kapacity s Azure SQL Database](sql-database-elastic-scale-introduction.md)
* [Vzory návrhu pro aplikace SaaS s více tenanty využívající Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Ověřování ve víceklientských aplikacích s využitím Azure AD a OpenID Connect](../guidance/guidance-multitenant-identity-authenticate.md)
* [Aplikace Tailspin Surveys](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a>Otázky a žádosti o funkce
Máte dotazy, oslovení nám na [fórum SQL Database](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) a pro žádosti o funkce, přidejte je do [fóru pro zpětnou vazbu databáze SQL](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->


