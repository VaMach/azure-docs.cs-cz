---
title: "Škálování Azure SQL database | Microsoft Docs"
description: "Jak používat ShardMapManager, klientské knihovny pro elastické databáze"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 0e9d647a-9ba9-4875-aa22-662d01283439
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 604690325fd755dcf5c997cc281fe9e5825c51a4
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="scale-out-databases-with-the-shard-map-manager"></a>Horizontální navýšení kapacity databáze pomocí Správce horizontálního oddílu mapy
Chcete-li snadno škálovat databáze na SQL Azure, použijte správce mapy horizontálního oddílu. Mapa správce horizontálního oddílu je speciální databáze, která uchovává globální mapování informace o všech horizontálních oddílů (databáze) v sadě horizontálního oddílu. Metadata umožňuje aplikaci připojit ke správné databázi na základě hodnotu **horizontálního dělení klíč**. Kromě toho každých horizontálního oddílu v sadě obsahuje mapy, které sledují místní sdílení dat (označované jako **shardlets**). 

![Správa mapování horizontálních](./media/sql-database-elastic-scale-shard-map-management/glossary.png)

Pochopení, jak se vytvářejí tyto mapy je nezbytné pro správu mapy horizontálního oddílu. To se provádí pomocí [ShardMapManager třída](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx), který se nachází v [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md) ke správě horizontálního oddílu map.  

## <a name="shard-maps-and-shard-mappings"></a>Mapování horizontálních a horizontálního oddílu mapy
Pro každý horizontálního oddílu je nutné vybrat typ mapy horizontálního oddílu k vytvoření. Výběr závisí na architektuře databáze: 

1. Jednoho klienta na databázi  
2. Několik klientů na databázi (dva typy):
   1. Seznam mapování
   2. Mapování rozsahu

Pro jednoho klienta modelu, vytvořit **seznamu mapování** horizontálního oddílu mapy. Model jednoho klienta přiřadí jednu databázi na každého klienta. Jedná se efektivní model pro vývojáře SaaS, protože ho zjednodušuje správu.

![Seznam mapování][1]

Víceklientského modelu přiřadí několik klientů jedné databáze (a skupin klientů, které můžete distribuovat mezi více databází). Pomocí tohoto modelu, pokud očekáváte, že každý klient k potřebami malá data. V tomto modelu jsme řadu klienty přiřadit k databázi pomocí **rozsah mapování**. 

![Mapování rozsahu][2]

Nebo můžete implementovat s použitím modelu víceklientské databáze *seznamu mapování* přiřadit více klientů pro jednu databázi. Například DB1 se používá k ukládání informací o id klienta 1 a 5 a DB2 ukládá data pro klienta 7 a klienta 10. 

![Vnořením klientů na jednom DB][3] 

### <a name="supported-net-types-for-sharding-keys"></a>Podporované typy .net pro klíče horizontálního dělení
Elastické škálování podpora následující rozhraní .net Framework typy jako horizontálního dělení klíče:

* celé číslo
* dlouhá
* Identifikátor GUID
* Byte]  
* Data a času
* Časový interval
* Datový typ DateTimeOffset

### <a name="list-and-range-shard-maps"></a>Seznam a rozsah horizontálního oddílu mapy
Mapování horizontálních se dá vytvořit pomocí **seznam jednotlivých horizontálního dělení hodnoty klíče**, nebo se dá vytvořit pomocí **rozsahy horizontálního dělení hodnoty klíče**. 

### <a name="list-shard-maps"></a>Seznam horizontálních mapy
**Horizontálních oddílů** obsahovat **shardlets** a mapování shardlets k horizontálních oddílů je možný díky mapu horizontálního oddílu. A **seznam horizontálních mapy** je přidružení mezi jednotlivé hodnoty klíče, které identifikují shardlets a databázemi, které slouží jako horizontálních oddílů.  **Seznam mapování** jsou klíčem explicitní a jiné hodnoty lze mapovat do stejné databáze. Například klíč 1 se mapuje na databázi A a B. databáze odkazovat na klíčové hodnoty 3 a 6

| Klíč | Umístění horizontálního oddílu |
| --- | --- |
| 1 |Database_A |
| 3 |Database_B |
| 4 |Database_C |
| 6 |Database_B |
| Tlačítka ... |Tlačítka ... |

### <a name="range-shard-maps"></a>Rozsah horizontálního oddílu mapy
V **rozsah horizontálního oddílu mapy**, klíče rozsah je popsán pár **[hodnota nízká, vysoká hodnota)** kde *nízká hodnota* je minimální klíč v rozsahu a *vysoké hodnoty* první hodnota vyšší než je rozsah. 

Například **[0, 100)** zahrnuje všechny celá čísla větší než nebo rovna 0 a menší než 100. Všimněte si, že více rozsahům může ukazovat na stejnou databázi a nesouvislý rozsahy jsou podporovány (například [100,200) a [400,600) i bodu c databáze v následujícím příkladu.)

| Klíč | Umístění horizontálního oddílu |
| --- | --- |
| [1,50) |Database_A |
| [50,100) |Database_B |
| [100,200) |Database_C |
| [400,600) |Database_C |
| Tlačítka ... |Tlačítka ... |

Každá z tabulky uvedené výše je koncepční příklad **ShardMap** objektu. Každý řádek je zjednodušená příklad jednotlivce **PointMapping** (pro mapu horizontálního oddílu seznamu) nebo **RangeMapping** (pro mapu horizontálního oddílu rozsah) objektu.

## <a name="shard-map-manager"></a>Správce horizontálního oddílu mapy
V knihovně klienta správce mapy horizontálního oddílu je kolekce horizontálního oddílu map. Data spravovaná přes **ShardMapManager** instance je udržováno na třech místech: 

1. **Globální horizontálního oddílu mapy (GSM)**: Zadejte databázi, která bude sloužit jako úložiště pro všechna její mapování horizontálních a mapování. Speciální tabulek a uložených procedur se automaticky vytvoří ke správě informací. To je obvykle s malou databází a lehkým získat přístup, a nesmí být použita pro ostatní požadavky aplikace. V tabulkách jsou ve speciální schématu s názvem **__ShardManagement**. 
2. **Místní ID horizontálního oddílu mapy (LSM)**: každou databázi, která zadáte jako horizontálního oddílu se mění tak, aby obsahovala několik malých tabulky a speciální uložené procedury, které obsahují a spravovat informace o mapování horizontálních specifické pro tento horizontálního oddílu. Tyto informace jsou redundantní s informacemi v GSM a umožní aplikaci ověřit informace uložené v mezipaměti horizontálních mapy bez uvedení žádné zatížení na GSM; aplikace používá LSM k určení, pokud je v mezipaměti mapování stále platný. V tabulkách odpovídající LSM na každý horizontálního oddílu jsou i ve schématu **__ShardManagement**.
3. **Mezipaměť aplikací**: každý přístup instance aplikace **ShardMapManager** objekt udržuje místní mezipaměť v paměti její mapování. Uloží informace o směrování, která nedávno byla načtena. 

## <a name="constructing-a-shardmapmanager"></a>Vytváření ShardMapManager
A **ShardMapManager** objektu je vytvořený [factory](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx) vzor. **[ShardMapManagerFactory.GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**  metoda přijímá pověření (včetně názvu serveru a název databáze, která uchovává GSM) ve formě **ConnectionString** a vrací instanci třídy **ShardMapManager**.  

**Poznámka:** **ShardMapManager** by měla být vytvořena instance pouze jednou v každé doméně aplikace v rámci kód inicializace pro aplikaci. Vytvoření další instance ShardMapManager do stejné domény aplikace, bude výsledkem vyšší paměť a využití CPU aplikace. A **ShardMapManager** může obsahovat libovolný počet horizontálních mapy. Mapu jeden horizontálního oddílu může být dostatečné pro mnoho aplikací, jsou časy při použití různé sady databází pro jiné schéma nebo pro jedinečné účely; v těchto případech může být vhodnější než více mapami horizontálního oddílu. 

V tomto kódu aplikace pokusí otevřít existující **ShardMapManager** s [TryGetSqlShardMapManager metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx).  Pokud objekty, které představují globální **ShardMapManager** (GSM) není dosud neexistuje v databázi, klientské knihovny vytvoří je existuje pomocí [CreateSqlShardMapManager metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx).

    // Try to get a reference to the Shard Map Manager 
     // via the Shard Map Manager database.  
    // If it doesn't already exist, then create it. 
    ShardMapManager shardMapManager; 
    bool shardMapManagerExists = ShardMapManagerFactory.TryGetSqlShardMapManager(
                                        connectionString, 
                                        ShardMapManagerLoadPolicy.Lazy, 
                                        out shardMapManager); 

    if (shardMapManagerExists) 
     { 
        Console.WriteLine("Shard Map Manager already exists");
    } 
    else
    {
        // Create the Shard Map Manager. 
        ShardMapManagerFactory.CreateSqlShardMapManager(connectionString);
        Console.WriteLine("Created SqlShardMapManager"); 

        shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(
            connectionString, 
            ShardMapManagerLoadPolicy.Lazy);

        // The connectionString contains server name, database name, and admin credentials 
        // for privileges on both the GSM and the shards themselves.
    } 

Jako alternativu můžete použít Powershell k vytvoření nového správce mapy horizontálního oddílu. Příkladem je k dispozici [zde](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="get-a-rangeshardmap-or-listshardmap"></a>Získání RangeShardMap nebo ListShardMap
Po vytvoření horizontálního oddílu mapa správce, můžete získat [RangeShardMap](https://msdn.microsoft.com/library/azure/dn807318.aspx) nebo [ListShardMap](https://msdn.microsoft.com/library/azure/dn807370.aspx) pomocí [TryGetRangeShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), [TryGetListShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), nebo [GetShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) metoda.

    /// <summary>
    /// Creates a new Range Shard Map with the specified name, or gets the Range Shard Map if it already exists.
    /// </summary>
    public static RangeShardMap<T> CreateOrGetRangeShardMap<T>(ShardMapManager shardMapManager, string shardMapName)
    {
        // Try to get a reference to the Shard Map.
        RangeShardMap<T> shardMap;
        bool shardMapExists = shardMapManager.TryGetRangeShardMap(shardMapName, out shardMap);

        if (shardMapExists)
        {
            ConsoleUtils.WriteInfo("Shard Map {0} already exists", shardMap.Name);
        }
        else
        {
            // The Shard Map does not exist, so create it
            shardMap = shardMapManager.CreateRangeShardMap<T>(shardMapName);
            ConsoleUtils.WriteInfo("Created Shard Map {0}", shardMap.Name);
        }

        return shardMap;
    } 

### <a name="shard-map-administration-credentials"></a>Pověření správce horizontálního oddílu mapy
Aplikace, které Správa a zpracování mapy horizontálního oddílu se liší od těch, které pomocí map horizontálního oddílu k připojení trasy. 

Ke správě horizontálního oddílu mapy (Přidat nebo změnit horizontálních oddílů, map horizontálního oddílu, mapování horizontálních, atd.) je třeba vytvořit instanci **ShardMapManager** pomocí **přihlašovací údaje, které mají pro čtení a zápis oprávnění v databázi GSM a na každou databázi, která slouží jako horizontálního oddílu**. Přihlašovací údaje musí povolovat zápisu tabulky v GSM i LSM informace ID horizontálního oddílu mapy zadat nebo změnit, stejně jako u vytváření tabulek LSM na nové horizontálních oddílů.  

V tématu [používá přihlašovací údaje pro přístup k klientské knihovny pro elastické databáze](sql-database-elastic-scale-manage-credentials.md).

### <a name="only-metadata-affected"></a>Vliv pouze metadata.
Metody používané pro vyplnění nebo změna **ShardMapManager** data nijak nemění data uživatele uložená v horizontálních oddílů sami. Například metody, jako **CreateShard**, **DeleteShard**, **UpdateMapping**atd vliv pouze metadata mapy horizontálního oddílu. Není odebrat, přidat nebo změnit uživatele data obsažená v horizontálních oddílů. Místo toho tyto metody jsou určeny k použití ve spojení s samostatné operace, které provedete na vytvoření nebo odebrat skutečná databáze, nebo že přesunout řádky z jednoho horizontálního oddílu do jiného znovu vyvážit horizontálně dělené prostředí.  ( **Rozdělení sloučení** nástroje dodávaná s nástroje elastické databáze využívá tato rozhraní API společně s Orchestrace vlastní přesun dat. mezi horizontálních oddílů.) V tématu [škálování pomocí nástroje elastické databáze rozdělení sloučení](sql-database-elastic-scale-overview-split-and-merge.md).

## <a name="populating-a-shard-map-example"></a>Sestavování příklad horizontálního oddílu mapy
V příkladu sekvenci operací k naplnění mapu konkrétní horizontálního oddílu jsou uvedeny níže. Kód provede tyto kroky: 

1. Nové mapování horizontálních je vytvořen v rámci správce mapy horizontálního oddílu. 
2. Metadata pro dva různé horizontálních oddílů je přidán do mapy horizontálního oddílu. 
3. Budou přidány různé klíče rozsah mapování a se zobrazí celkové obsah mapy horizontálního oddílu. 

Kód je napsán tak, aby metoda může být znovu, pokud dojde k chybě. Každý požadavek testuje, jestli horizontálního oddílu nebo mapování už existuje, před pokusem o jeho vytvoření. Kód předpokládá, že databáze s názvem **sample_shard_0**, **sample_shard_1** a **sample_shard_2** již byly vytvořeny na serveru odkazuje řetězec **shardServer**. 

    public void CreatePopulatedRangeMap(ShardMapManager smm, string mapName) 
        {            
            RangeShardMap<long> sm = null; 

            // check if shardmap exists and if not, create it 
            if (!smm.TryGetRangeShardMap(mapName, out sm)) 
            { 
                sm = smm.CreateRangeShardMap<long>(mapName); 
            } 

            Shard shard0 = null, shard1=null; 
            // Check if shard exists and if not, 
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetShard(new ShardLocation(
                                     shardServer, 
                                     "sample_shard_0"), 
                                     out shard0)) 
            { 
                Shard0 = sm.CreateShard(new ShardLocation(
                                            shardServer, 
                                            "sample_shard_0")); 
            } 

            if (!sm.TryGetShard(new ShardLocation(
                                    shardServer, 
                                    "sample_shard_1"), 
                                    out shard1)) 
            { 
                Shard1 = sm.CreateShard(new ShardLocation(
                                             shardServer, 
                                            "sample_shard_1"));  
            } 

            RangeMapping<long> rmpg=null; 

            // Check if mapping exists and if not,
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetMappingForKey(0, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                          new RangeMappingCreationInfo<long>
                          (new Range<long>(0, 50), 
                          shard0, 
                          MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(50, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(50, 100), 
                         shard1, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(100, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long>
                         (new Range<long>(100, 150), 
                         shard0, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(150, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(150, 200), 
                         shard1, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(200, out rmpg)) 
            { 
               sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(200, 300), 
                         shard0, 
                         MappingStatus.Online)); 
            } 

            // List the shards and mappings 
            foreach (Shard s in sm.GetShards()
                         .OrderBy(s => s.Location.DataSource)
                         .ThenBy(s => s.Location.Database))
            { 
               Console.WriteLine("shard: "+ s.Location); 
            } 

            foreach (RangeMapping<long> rm in sm.GetMappings()) 
            { 
                Console.WriteLine("range: [" + rm.Value.Low.ToString() + ":" 
                        + rm.Value.High.ToString()+ ")  ==>" +rm.Shard.Location); 
            } 
        } 

Jako alternativu můžete použít skripty prostředí PowerShell k dosažení stejného výsledku. Některé příklady prostředí PowerShell ukázkový jsou k dispozici [zde](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).     

Jakmile jsou vyplněna horizontálního oddílu mapy, data přístup k aplikacím můžou vytvořit nebo přizpůsobit pro práci s map. Naplnění nebo manipulace s map nemusí dojít znovu, dokud **mapy rozložení** musí změnit.  

## <a name="data-dependent-routing"></a>Směrování závislé na datech
Mapa správce horizontálního oddílu nejvíce použije v aplikacích, které vyžadují připojení databáze k provádění operací data specifická pro aplikaci. Tato připojení musí být přidruženy k databázi správné. To se označuje jako **závislé směrování dat**. Pro tyto aplikace vytvořte instanci objektu manager mapy horizontálního oddílu z objektu pro vytváření pomocí přihlašovacích údajů, které mají oprávnění jen pro čtení na databázi GSM. Jednotlivých požadavků pro pozdější připojení zadat přihlašovací údaje potřebné pro připojení k databázi odpovídající horizontálního oddílu.

Všimněte si, že tyto aplikace (pomocí **ShardMapManager** otevřít s přihlašovacími údaji jen pro čtení) nelze provádět změny pro mapování nebo mapy. Pro tyto potřeby vytvořte správu konkrétní aplikace nebo skripty prostředí PowerShell, které zadat přihlašovací údaje pro vyšší úrovní oprávnění, jak je popsáno výše. V tématu [používá přihlašovací údaje pro přístup k klientské knihovny pro elastické databáze](sql-database-elastic-scale-manage-credentials.md).

Další podrobnosti najdete v tématu [závislé směrování dat](sql-database-elastic-scale-data-dependent-routing.md). 

## <a name="modifying-a-shard-map"></a>Úprava horizontálního oddílu mapy
Mapování horizontálních lze změnit různými způsoby. Všechny z následujících metod upravit metadat, která popisují horizontálních oddílů a jejich mapování, ale nemění fyzicky dat v rámci horizontálních oddílů, ani se jejich vytvoření nebo odstranění skutečné databáze.  Některé operace, na mapě horizontálního oddílu popsané dál může musí být zajistí koordinaci se akce správy, který fyzicky přesunout data nebo který přidávat a odebírat databáze slouží jako horizontálních oddílů.

Tyto metody spolupracovat jako stavební bloky, které jsou k dispozici pro úpravy celkové distribuci dat ve vašem prostředí horizontálně dělené databáze.  

* Přidat nebo odebrat horizontálních oddílů: použijte  **[CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)**  a  **[DeleteShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard.aspx)**  z [Shardmap třída](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.aspx). 
  
    Server a databázi představující horizontálního oddílu cíl již musí existovat pro tyto operace provést. Tyto metody nemají žádný vliv na databáze, sami, jenom na metadata v mapě horizontálního oddílu.
* Vytvořit nebo odstranit body nebo rozsahy, které jsou namapované horizontálních oddílů: použijte  **[CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn841993.aspx)**,  **[DeleteMapping](https://msdn.microsoft.com/library/azure/dn824200.aspx)**  z [RangeShardMapping třída](https://msdn.microsoft.com/library/azure/dn807318.aspx), a  **[CreatePointMapping](https://msdn.microsoft.com/library/azure/dn807218.aspx)**  z [ListShardMap](https://msdn.microsoft.com/library/azure/dn842123.aspx)
  
    Mnoho různých bodů nebo rozsahy lze mapovat na stejné ID horizontálního oddílu. Tyto metody ovlivňují jenom metadata – neovlivňují všechna data, která může být již přítomny v horizontálních oddílů. Pokud data musí být odebrána z databáze, aby byl konzistentní s **DeleteMapping** operace, budete muset provést tyto operace samostatně, ale ve spojení s těmito metodami.  
* Pro existující rozsahy rozdělit na dvě nebo sloučení sousedící oblasti do jedné: použijte  **[SplitMapping](https://msdn.microsoft.com/library/azure/dn824205.aspx)**  a  **[MergeMappings](https://msdn.microsoft.com/library/azure/dn824201.aspx)**.  
  
    Všimněte si, že rozdělení a operace sloučení **neměňte horizontálního oddílu, na kterou jsou hodnoty klíče namapovaná**. Rozdělení stávající rozsah dělí na dvě části, ale ponechá i jako namapované na stejné ID horizontálního oddílu. Sloučení funguje na dva přiléhající rozsahy, které jsou už namapované stejné ID horizontálního oddílu, je slučování do jednoho rozsahu.  Přesun body nebo rozsahy, sami mezi horizontálních oddílů musí být koordinované pomocí **UpdateMapping** ve spojení s vlastní přesun dat.  Můžete použít **rozdělení či sloučení** službu, která je součástí nástrojů pro elastické databáze pro koordinaci změny mapování horizontálních s přesun dat potřeby pohyb. 
* Znovu mapy (nebo přesunout) jednotlivé body nebo rozsahy pro různé horizontálních oddílů: použijte  **[UpdateMapping](https://msdn.microsoft.com/library/azure/dn824207.aspx)**.  
  
    Vzhledem k tomu potřeba může data přesouvat z jednoho horizontálního oddílu na jiný s **UpdateMapping** operace, budete muset provést tento přesun samostatně, ale ve spojení s těmito metodami.
* Provést mapování online a offline: použijte  **[MarkMappingOffline](https://msdn.microsoft.com/library/azure/dn824202.aspx)**  a  **[MarkMappingOnline](https://msdn.microsoft.com/library/azure/dn807225.aspx)**  k řízení stavu online mapování. 
  
    Mapování horizontálních určité operace jsou povoleny pouze při mapování je ve stavu "offline", včetně **UpdateMapping** a **DeleteMapping**. Při mapování je offline, žádost dat závisí na základě klíče, součástí mapování vrátí chybu. Kromě toho když rozsah je nejprve do režimu offline, všechna připojení k ovlivněných horizontálního oddílu jsou automaticky ukončená prevence nekonzistentní nebo neúplné výsledky dotazů namířená proti rozsahy se změnil. 

Mapování jsou neměnné objekty v rozhraní .net.  Všechny výše uvedené metody, které změní mapování zneplatnit také všechny odkazy na ně ve vašem kódu. Usnadnění provádění pořadí operace, které změní stav mapování všechny metody, které změní mapování vrátí nový odkaz mapování, tak možné zřetězit operace. Například pokud chcete odstranit existující mapování v shardmap sm, který obsahuje klíč 25, můžete spustit následující: 

        sm.DeleteMapping(sm.MarkMappingOffline(sm.GetMappingForKey(25)));

## <a name="adding-a-shard"></a>Přidání horizontálního oddílu
Aplikace často potřebují jednoduše přidat nové horizontálních oddílů pro zpracování dat, která je očekávána z nových klíčů nebo rozsahy klíčů pro mapu horizontálního oddílu, který již existuje. Například aplikace horizontálně dělené podle ID klienta muset zřídit nové horizontálního oddílu pro nového klienta, nebo horizontálně dělené měsíční dat může být nutné nové horizontálních zřídit před zahájením každý nový měsíc. 

Pokud nový rozsah hodnot klíče již není součástí stávající mapování a žádné přesun dat je nezbytné, je velmi jednoduchý a přidat nové horizontálního oddílu přidružit nový klíč nebo rozsahu do této horizontálního oddílu. Podrobnosti o přidání nové horizontálních oddílů najdete v tématu [přidání nové horizontálního oddílu](sql-database-elastic-scale-add-a-shard.md).

Pro scénáře, které vyžadují přesun dat ale nástroj rozdělení sloučení je nezbytná k orchestraci přesun dat mezi horizontálních oddílů v kombinaci s aktualizací map nezbytné horizontálního oddílu. Podrobnosti o použití rozdělení sloučení yool najdete v tématu [Přehled rozdělení sloučení](sql-database-elastic-scale-overview-split-and-merge.md) 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-shard-map-management/listmapping.png
[2]: ./media/sql-database-elastic-scale-shard-map-management/rangemapping.png
[3]: ./media/sql-database-elastic-scale-shard-map-management/multipleonsingledb.png
