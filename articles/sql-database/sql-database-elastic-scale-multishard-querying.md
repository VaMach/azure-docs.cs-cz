---
title: "Dotaz horizontálně dělené databází Azure SQL | Microsoft Docs"
description: "Spuštění dotazů napříč horizontálních oddílů pomocí klientské knihovny elastické databáze."
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: a4379c15-f213-4026-ab6f-a450ee9d5758
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2016
ms.author: torsteng
ms.openlocfilehash: 67bcb3c7fe33341103f28bc70e8cc2acbb924cae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="multi-shard-querying"></a>Víc horizontálních dotazování
## <a name="overview"></a>Přehled
Pomocí [nástroje elastické databáze](sql-database-elastic-scale-introduction.md), můžete vytvořit řešení horizontálně dělené databáze. **Dotazování víc horizontálních** se používá pro úlohy, jako je kolekce nebo generování sestav dat vyžadující spuštění dotazu roztahovány napříč několika horizontálních oddílů. (K kontrastu [závislé na data směrování](sql-database-elastic-scale-data-dependent-routing.md), který provede všechny práci na jednom horizontálního oddílu.) 

1. Získání [ **RangeShardMap** ](https://msdn.microsoft.com/library/azure/dn807318.aspx) nebo [ **ListShardMap** ](https://msdn.microsoft.com/library/azure/dn807370.aspx) pomocí [ **TryGetRangeShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), [ **TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), nebo [ **GetShardMap** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) metoda. V tématu [ **vytváření ShardMapManager** ](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) a [ **získat RangeShardMap nebo ListShardMap**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. Vytvoření  **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)**  objektu.
3. Vytvoření  **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**. 
4. Nastavte  **[Vlastnost CommandText](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)**  k příkazu T-SQL.
5. Spusťte příkaz voláním  **[ExecuteReader metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**.
6. Zobrazit výsledky pomocí  **[MultiShardDataReader třída](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**. 

## <a name="example"></a>Příklad
Následující kód ukazuje použití více horizontálního oddílu dotazování pomocí danou **ShardMap** s názvem *myShardMap*. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
    { 
    using (MultiShardCommand cmd = conn.CreateCommand())
           { 
            cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
            cmd.CommandType = CommandType.Text; 
            cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
            cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

            using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
                { 
                    while (sdr.Read())
                        { 
                            var c1Field = sdr.GetString(0); 
                            var c2Field = sdr.GetFieldValue<int>(1); 
                            var c3Field = sdr.GetFieldValue<Int64>(2);
                        } 
                 } 
           } 
    } 


Klíčovým rozdílem je, vytváření připojení více horizontálního oddílu. Kde **SqlConnection** funguje na jedné databáze, **MultiShardConnection** trvá ***kolekce horizontálních oddílů*** jako vstup. Naplnění kolekce horizontálních oddílů z mapy horizontálního oddílu. Pak je spustit dotaz na kolekce horizontálních oddílů pomocí **UNION ALL** sémantiku ke kompilaci jeden celkový výsledek. Volitelně lze přidat název horizontálního oddílu, kde řádek pochází z výstupu pomocí **ExecutionOptions** na příkaz Vlastnosti. 

Všimněte si volání **myShardMap.GetShards()**. Tato metoda načte všechny horizontálních oddílů z mapování horizontálních a poskytuje snadný způsob, jak spustit dotaz napříč všechny příslušné databáze. Kolekce horizontálních oddílů pro více horizontálního oddílu dotazu může být přesnějších další provedením dotaz LINQ nad shromažďováním vrácená z volání **myShardMap.GetShards()**. V kombinaci se zásadou částečné výsledky aktuální možnosti v dotazování víc horizontálních byl navržen tak, aby dobře hodí desítkami až stovky horizontálních oddílů.

Omezení s více horizontálních dotazování právě nedostatek ověření horizontálních oddílů a shardlets, který je dotazován. Při závislé na data směrování ověřuje, že dané horizontálního oddílu je součástí mapy horizontálního oddílu v době dotazování, neprovádějte dotazy víc horizontálních této kontroly. To může vést k víc horizontálních dotazy spuštěnými na databáze, které byly odebrány z mapy horizontálního oddílu.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Víc horizontálních dotazy a operace sloučení rozdělení
Dotazy víc horizontálních Neověřovat, zda shardlets v databázi předmětem dotazu se účastní probíhající operace sloučení rozdělení. (Viz [škálování pomocí nástroje elastické databáze rozdělení sloučení](sql-database-elastic-scale-overview-split-and-merge.md).) To může vést k nekonzistenci kde řádky ze stejné shardlet zobrazit pro více databází ve stejném dotazu více horizontálního oddílu. Mějte na paměti tyto omezení a zvažte vyprázdnění probíhající operace sloučení rozdělení a změny horizontálního oddílu mapy při provádění více horizontálních dotazy.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Viz také
**[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)**  třídy a metody.

Spravovat pomocí horizontálních oddílů [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md). Zahrnuje obor názvů s názvem [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) , který poskytuje možnost používat víc horizontálních oddílů pomocí jeden dotaz a výsledek. Dotazování abstrakce zajišťuje v kolekci horizontálních oddílů. Obsahuje také zásady alternativní spouštění, zejména částečné výsledky, jak nakládat s chybami při dotazování přes mnoho horizontálních oddílů.  

