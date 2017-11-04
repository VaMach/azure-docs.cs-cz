---
title: "Data závislé směrování s Azure SQL Database | Microsoft Docs"
description: "Jak používat třídu ShardMapManager v aplikacích .NET pro data závislé na směrování, funkce horizontálně dělené databází ve službě Azure SQL Database"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: cad09e15-5561-4448-aa18-b38f54cda004
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: ddove
ms.openlocfilehash: 2246dd12b922fcbc2e2b58890b3d56253810849c
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="data-dependent-routing"></a>Směrování závislé na datech
**Data závislé směrování** je možnost používat data v dotazu pro směrování požadavku k příslušné databázi. Toto je základní vzor při práci s horizontálně dělené databáze. Kontext požadavku může také použít pro směrování požadavku, zvlášť pokud klíč horizontálního dělení není část dotazu. Každé specifického dotazu nebo transakce v aplikaci pomocí závislé směrování dat je omezen na přístup k jedné databáze na základě požadavku. Pro nástroje Azure SQL Database elastické tento směrování se provádí pomocí  **[ShardMapManager třída](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)**  v aplikacích ADO.NET.

Aplikace není nutné sledovat různé připojovací řetězce nebo DB umístění přidružené k jiné řezy dat v horizontálně dělené prostředí. Místo toho [správce mapy horizontálního oddílu](sql-database-elastic-scale-shard-map-management.md) otevře připojení k databázím správné v případě potřeby na základě dat v mapě horizontálního oddílu a hodnota klíče horizontálního dělení, který je cílem dané aplikace požadavek. Klíč je obvykle *customer_id*, *tenant_id*, *date_key*, nebo některé konkrétní identifikátor, který je základní parametr žádosti databáze). 

Další informace najdete v tématu [škálování SQL serveru se Škálováním s závislé směrování dat](https://technet.microsoft.com/library/cc966448.aspx).

## <a name="download-the-client-library"></a>Stáhnout klientské knihovny
Třída získáte nainstalovat [klientské knihovny pro elastické databáze](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). 

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a>Použití ShardMapManager v závislou aplikaci dat směrování
Aplikace by měl vytvořit instanci **ShardMapManager** při inicializaci pomocí volání objektu pro vytváření  **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**. V tomto příkladu jak **ShardMapManager** a konkrétní **ShardMap** ji obsahující jsou inicializovány. Tento příklad ukazuje GetSqlShardMapManager a [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) metody.

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, 
                      ShardMapManagerLoadPolicy.Lazy);
    RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 

### <a name="use-lowest-privilege-credentials-possible-for-getting-the-shard-map"></a>Použít pověření nejnižší oprávnění možné pro získání ID horizontálního oddílu mapy
Pokud aplikace není manipulace s samotná mapa horizontálního oddílu, přihlašovací údaje použité metoda objektu factory musí mít oprávnění stačí jen pro čtení **globální horizontálního oddílu mapy** databáze. Tyto přihlašovací údaje se obvykle liší od přihlašovací údaje používané k otevření připojení do správce mapy horizontálního oddílu. Viz také [používá přihlašovací údaje pro přístup k klientské knihovny pro elastické databáze](sql-database-elastic-scale-manage-credentials.md). 

## <a name="call-the-openconnectionforkey-method"></a>Volání metody OpenConnectionForKey
**[ShardMap.OpenConnectionForKey metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)**  vrátí připravená pro vydání příkazy k příslušné databázi na základě hodnoty z připojení k ADO.Net **klíč** parametr. Informace ID horizontálního oddílu je uložené v mezipaměti v aplikace **ShardMapManager**, takže tyto požadavky nejsou obvykle zahrnují vyhledávání v databázi proti **globální mapy horizontálního oddílu** databáze. 

    // Syntax: 
    public SqlConnection OpenConnectionForKey<TKey>(
        TKey key,
        string connectionString,
        ConnectionOptions options
    )


* **Klíč** parametr se používá jako klíč vyhledávání do mapy horizontálního oddílu určit příslušné databáze pro požadavek. 
* **ConnectionString** slouží k předávání pouze uživatelská pověření pro požadované připojení. Žádný název databáze nebo název serveru jsou zahrnuté v tomto *connectionString* vzhledem k tomu, že metoda určí databáze a serveru pomocí **ShardMap**. 
* **[ConnectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)**  musí být nastavena na **ConnectionOptions.Validate** Pokud prostředí, kde horizontálního oddílu mapuje může změnit a řádky může přesunout do jiné databáze na základě těchto rozdělení nebo sloučení operací. To zahrnuje stručný dotazu mapu místní horizontálního oddílu na cílové databáze (ne do globální horizontálních map) před doručením připojení k aplikaci. 

Selže-li ověření vůči mapování horizontálních místní (což znamená, že mezipaměť je nesprávný), Manager mapy horizontálního oddílu se bude dotazovat mapy globální horizontálních získat nové správnou hodnotu pro vyhledávání, aktualizace mezipaměti a získat a vrátit na příslušnou databázi připojení. 

Použití **ConnectionOptions.None** pouze když změny mapování horizontálních nejsou očekáván aplikace je online. V takovém případě uložené v mezipaměti hodnoty lze předpokládat, že vždycky být správná a mohou být velmi odezvy ověření volání na cílovou databázi bezpečně přeskočeny. Který snižuje zatížení databáze. **ConnectionOptions** také lze nastavit prostřednictvím hodnotu v konfiguračním souboru k označení, zda se očekává horizontálního dělení změny nebo není během v časovém intervalu.  

Tento příklad používá hodnotu celé číslo klíče **CustomerID**pomocí **ShardMap** objekt s názvem **customerShardMap**.  

    int customerId = 12345; 
    int newPersonId = 4321; 

    // Connect to the shard for that customer ID. No need to call a SqlConnection 
    // constructor followed by the Open method.
    using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, 
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
    { 
        // Execute a simple command. 
        SqlCommand cmd = conn.CreateCommand(); 
        cmd.CommandText = @"UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID"; 

        cmd.Parameters.AddWithValue("@customerID", customerId); 
        cmd.Parameters.AddWithValue("@newPersonID", newPersonId); 
        cmd.ExecuteNonQuery(); 
    }  

**OpenConnectionForKey** metoda vrátí nové již otevřete připojení ke správné databázi. Připojení v tímto způsobem nadále využívat všech výhod ADO.Net sdružování připojení. Tak dlouho, dokud transakce a požadavky lze splnit jednu horizontálního oddílu současně, to by měl být pouze změny potřebné v aplikaci už používá ADO.Net. 

**[OpenConnectionForKeyAsync metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)**  je také dostupná v případě, že vaše aplikace umožňuje použití asynchronní programování s ADO.Net. Její chování je závislé směrování ekvivalentní ADO data. NET na  **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)**  metoda.

## <a name="integrating-with-transient-fault-handling"></a>Integrace s přechodná chyba zpracování
Osvědčeným postupem při vývoji aplikací přístup dat v cloudu, je zajistit, že přechodných zachytila aplikaci a že operace, které jsou opakovat několikrát před vyvolání k chybě. Přechodná chyba zpracování pro cloudové aplikace je popsána v [přechodných chyb](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx). 

Přechodná chyba zpracování mohou existovat vedle sebe přirozeně pomocí vzoru Data závislé směrování. Klíčovým požadavkem je opakovat, včetně celého datového přístupu požadavek **pomocí** blok, který získat připojení směrování závislé na data. V předchozím příkladu by mohla být přepsána takto (Poznámka zvýrazněná změnit). 

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a>Příklad: závislé směrování s přechodná chyba zpracování dat
<pre><code>int customerId = 12345; 
int newPersonId = 4321; 

<span style="background-color:  #FFFF00">Configuration.SqlRetryPolicy.ExecuteAction(() =&gt; </span> 
<span style="background-color:  #FFFF00">    { </span>
        // Connect to the shard for a customer ID. 
        using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId,  
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
        { 
            // Execute a simple command 
            SqlCommand cmd = conn.CreateCommand(); 

            cmd.CommandText = @&quot;UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID&quot;; 

            cmd.Parameters.AddWithValue(&quot;@customerID&quot;, customerId); 
            cmd.Parameters.AddWithValue(&quot;@newPersonID&quot;, newPersonId); 
            cmd.ExecuteNonQuery(); 

            Console.WriteLine(&quot;Update completed&quot;); 
        } 
<span style="background-color:  #FFFF00">    }); </span> 
</code></pre>


Balíčky, které jsou nezbytné k implementaci přechodná chyba zpracování se stáhnou automaticky při sestavování ukázkovou aplikaci elastické databáze. Balíčky jsou k dispozici samostatně na [Enterprise Library - přechodné chyby zpracování bloku aplikace](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/). Použijte verzi 6.0 nebo novější. 

## <a name="transactional-consistency"></a>Transakční konzistence
Transakční vlastnosti jsou zaručena bezpečnost pro všechny operace místní horizontálního oddílu. Například spuštění transakce odeslat prostřednictvím závislé na data směrování v rámci oboru horizontálního oddílu cíl pro připojení. V současné době neexistují žádné možnosti zadaná pro zapsání několik připojení do transakce a proto nejsou žádné záruky transakcí pro operace prováděné napříč horizontálních oddílů.

## <a name="next-steps"></a>Další kroky
Odpojit horizontálního oddílu, nebo znovu připojit horizontálního oddílu, najdete v části [pomocí třídy RecoveryManager opravit problémy horizontálního oddílu mapy](sql-database-elastic-database-recovery-manager.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

