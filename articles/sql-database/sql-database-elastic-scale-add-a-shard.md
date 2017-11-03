---
title: "Přidání horizontálního oddílu pomocí nástroje elastické databáze | Microsoft Docs"
description: "Nastavit jak přidat nové horizontálních oddílů horizontálního oddílu pomocí rozhraní API elastické škálování."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 62a349db-bebe-406f-a120-2f1986f2b286
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 7432bf00aa4d97d8dbc4b92a6a836698ee2b84d7
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="adding-a-shard-using-elastic-database-tools"></a>Přidání horizontálního oddílu pomocí nástroje elastické databáze
## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Chcete-li přidat horizontálního oddílu pro nový rozsah nebo klíč
Aplikace často potřebují jednoduše přidat nové horizontálních oddílů pro zpracování dat, která je očekávána z nových klíčů nebo rozsahy klíčů pro mapu horizontálního oddílu, který již existuje. Například aplikace horizontálně dělené podle ID klienta muset zřídit nové horizontálního oddílu pro nového klienta, nebo horizontálně dělené měsíční dat může být nutné nové horizontálních zřídit před zahájením každý nový měsíc. 

Pokud nový rozsah hodnot klíče již není součástí existující mapování, je velmi jednoduchý a přidat nové horizontálního oddílu přidružit nový klíč nebo rozsahu do této horizontálního oddílu. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Příklad: Přidání horizontálního oddílu a její rozsah do existující mapu horizontálního oddílu
Této ukázce se používá [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\)) metody a vytvoří instanci [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.)třídy. V ukázce níže, databáze s názvem **sample_shard_2** a všechny objekty nezbytné schématu do něj byly vytvořeny pro uložení rozsah [300, 400).  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


Jako alternativu můžete použít Powershell k vytvoření nového správce mapy horizontálního oddílu. Příkladem je k dispozici [zde](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Chcete-li přidat horizontálního oddílu pro prázdný součástí stávající rozsah
V některých případech je již namapován rozsah horizontálního oddílu a částečně vyplněnou data, ale teď chcete nadcházející data přesměrováni na různých horizontálního oddílu. Například můžete horizontálního oddílu podle rozsahu den a muset horizontálního oddílu již přidělené 50 dnů, ale dne 24 chcete zobrazovat v různých horizontálního oddílu budoucí data. Elastické databáze [nástroji pro sloučení rozdělení](sql-database-elastic-scale-overview-split-and-merge.md) mohou provést tuto operaci, ale pokud přesun dat není potřebné (například data pro rozsah dnů [25, 50), tj., den 25 včetně na 50 vylučují, ještě neexistuje) můžete to provést zcela přímo pomocí rozhraní API pro správu horizontálního oddílu mapy.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Příklad: rozdělení rozsah a přiřazování prázdný část horizontálních se nově přidané
Databáze s názvem "sample_shard_2" a všechny objekty nezbytné schématu do něj byly vytvořeny.  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 

        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**Důležité**: použijte tento postup, jen pokud jste si jisti, že rozsah aktualizované mapování je prázdný.  Výše uvedené metody Neprovádět kontrolu dat pro rozsah přesouvání, proto je nejlepší zahrnout kontroly v kódu.  Pokud řádky existují v rozsahu přesouvání, nebude odpovídat distribuční skutečná data mapy aktualizované horizontálního oddílu. Použití [nástroji pro sloučení rozdělení](sql-database-elastic-scale-overview-split-and-merge.md) k provedení operace místo v těchto případech.  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

