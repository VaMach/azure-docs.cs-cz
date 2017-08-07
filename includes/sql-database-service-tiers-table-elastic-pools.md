<!--
Used in:
sql-database-elastic-pool.md   
sql-database-resource-limits.md
sql-database-service-tiers.md  
-->
 
### <a name="basic-elastic-pool-limits"></a>Základní limity elastického fondu

| Velikost fondu (v eDTU)  | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Maximální velikost úložiště dat na fond* | 5 GB | 10 GB | 20 GB | 29 GB | 39 GB | 78 GB | 117 GB | 156 GB |
| Maximální úložiště vnitřní paměti OLTP na fond | – | Není dostupné. | Není dostupné. | Není dostupné. | Není dostupné. | Není dostupné. | Není dostupné. | Není dostupné. |
| Maximální počet databází pro každý fond | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Maximální počet souběžných pracovních procesů (požadavků) na fond | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Maximální počet souběžných přihlášení na fond | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Maximální počet souběžných relací na fond | 30000 | 30000 | 30000 | 30000 |30000 | 30000 | 30000 | 30000 |
| Minimální počet eDTU na databázi | 0,5 | 0,5 | 0,5 | 0,5 | 0,5 | 0,5 | 0,5 | 0,5 |
| Maximální počet eDTU na databázi | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| Maximální velikost úložiště dat na databázi | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 
||||||||

### <a name="standard-elastic-pool-limits"></a>Standardní limity elastického fondu

| Velikost fondu (v eDTU)  | **50** | **100** | **200**** | **300**** | **400**** | **800****| 
|:---|---:|---:|---:| ---: | ---: | ---: | 
| Maximální velikost úložiště dat na fond* | 50 GB| 100 GB| 200 GB | 300 GB| 400 GB | 800 GB | 
| Maximální úložiště vnitřní paměti OLTP na fond | – | Není dostupné. | Není dostupné. | Není dostupné. | Není dostupné. | Není dostupné. | 
| Maximální počet databází pro každý fond | 100 | 200 | 500 | 500 | 500 | 500 | 
| Maximální počet souběžných pracovních procesů (požadavků) na fond | 100 | 200 | 400 | 600 |  800 | 1600 |
| Maximální počet souběžných přihlášení na fond | 100 | 200 | 400 | 600 |  800 | 1600 |
| Maximální počet souběžných relací na fond | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
| Minimální počet eDTU na databázi** | 0, 10, 20, 50 | 0, 10, 20, 50, 100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| Maximální počet eDTU na databázi** | 10, 20, 50 | 10, 20, 50, 100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 | 
| Maximální velikost úložiště dat na databázi | 50 GB | 100 GB | 200 GB | 250 GB | 250 GB | 250 GB |
||||||||

### <a name="standard-elastic-pool-limits-continued"></a>Limity elastického fondu úrovně Standard (pokračování) 

| Velikost fondu (v eDTU)  |  **1 200**** | **1 600**** | **2 000**** | **2 500**** | **3 000**** |
|:---|---:|---:|---:| ---: | ---: |
| Maximální velikost úložiště dat na fond* | 1,2 TB | 1,6 TB | 2 TB | 2,4 TB | 2,9 TB | 
| Maximální úložiště vnitřní paměti OLTP na fond | – | Není dostupné. | Není dostupné. | Není dostupné. | Není dostupné. | 
| Maximální počet databází pro každý fond | 500 | 500 | 500 | 500 | 500 | 
| Maximální počet souběžných pracovních procesů (požadavků) na fond |  2400 | 3200 | 4000 | 5000 | 6000 |
| Maximální počet souběžných přihlášení na fond |  2400 | 3200 | 4000 | 5000 | 6000 |
| Maximální počet souběžných relací na fond | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Minimální počet eDTU na databázi** | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1 200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1 200, 1 600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1 200, 1 600, 2 000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1 200, 1 600, 2 000, 2 500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1 200, 1 600, 2 000, 2 500, 3 000 |
| Maximální počet eDTU na databázi** | 10, 20, 50, 100, 200, 300, 400, 800, 1 200 | 10, 20, 50, 100, 200, 300, 400, 800, 1 200, 1 600 | 10, 20, 50, 100, 200, 300, 400, 800, 1 200, 1 600, 2 000 | 10, 20, 50, 100, 200, 300, 400, 800, 1 200, 1 600, 2 000, 2 500 | 10, 20, 50, 100, 200, 300, 400, 800, 1 200, 1 600, 2 000, 2 500, 3 000 | 
| Maximální velikost úložiště dat na databázi | 250 GB | 250 GB | 250 GB | 250 GB | 250 GB | 
||||||||

### <a name="premium-elastic-pool-limits"></a>Prémiové limity elastického fondu

| Velikost fondu (v eDTU)  | **125** | **250** | **500** | **1000** | **1 500*****| 
|:---|---:|---:|---:| ---: | ---: | 
| Maximální velikost úložiště dat na fond* | 250 GB | 500 GB | 750 GB | 1 TB | 1,5 TB | 
| Maximální úložiště vnitřní paměti OLTP na fond | 1 GB| 2 GB| 4 GB| 10 GB| 12 GB| 
| Maximální počet databází pro každý fond | 50 | 100 | 100 | 100 | 100 |  
| Maximální počet souběžných pracovních procesů na fond (požadavky) | 200 | 400 | 800 | 1600 |  2400 | 
| Maximální počet souběžných přihlášení na fond | 200 | 400 | 800 | 1600 |  2400 |
| Maximální počet souběžných relací na fond | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Minimální počet eDTU na databázi | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1 000 | 0, 25, 50, 75, 125, 250, 500, 1 000 | 
| Maximální počet eDTU na databázi | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1 000 | 25, 50, 75, 125, 250, 500, 1 000 |
| Maximální velikost úložiště dat na databázi | 250 GB | 500 GB | 500 GB | 500 GB | 500 GB | 
||||||||

### <a name="premium-elastic-pool-limits-continued"></a>Limity elastického fondu úrovně Premium (pokračování) 

| Velikost fondu (v eDTU) | **2 000***** | **2 500***** | **3 000***** | **3 500***** | **4 000*****|
|:---|---:|---:|---:| ---: | ---: | 
| Maximální velikost úložiště dat na fond* | 2 TB | 2,5 TB | 3 TB | 3,5 TB | 4 TB |
| Maximální úložiště vnitřní paměti OLTP na fond | 16 GB | 20 GB | 24 GB | 28 GB | 32 GB |
| Maximální počet databází pro každý fond | 100 | 100 | 100 | 100 | 100 | 
| Maximální počet souběžných pracovních procesů (požadavků) na fond |  3200 | 4000 | 4800 | 5600 | 6400 |
| Maximální počet souběžných přihlášení na fond |  3200 | 4000 | 4800 | 5600 | 6400 |
| Maximální počet souběžných relací na fond | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Minimální počet eDTU na databázi | 0, 25, 50, 75, 125, 250, 500, 1 000, 1 750 | 0, 25, 50, 75, 125, 250, 500, 1 000, 1 750 | 0, 25, 50, 75, 125, 250, 500, 1 000, 1 750 | 0, 25, 50, 75, 125, 250, 500, 1 000, 1 750 |  0, 25, 50, 75, 125, 250, 500, 1 000, 1 750, 4 000 | 
| Maximální počet eDTU na databázi | 25, 50, 75, 125, 250, 500, 1 000, 1 750 | 25, 50, 75, 125, 250, 500, 1 000, 1 750 | 25, 50, 75, 125, 250, 500, 1 000, 1 750 | 25, 50, 75, 125, 250, 500, 1 000, 1 750 | 25, 50, 75, 125, 250, 500, 1 000, 1 750, 4 000 | 
| Maximální velikost úložiště dat na databázi | 500 GB | 500 GB | 500 GB | 500 GB | 500 GB | 
||||||||

### <a name="premium-rs-elastic-pool-limits"></a>Omezení elastického fondu Premium RS

| Velikost fondu (v eDTU)  | **125** | **250** | **500** | **1000** |
|:---|---:|---:|---:| ---: | ---: | 
| Maximální velikost úložiště dat na fond* | 250 GB| 500 GB | 750 GB | 750 GB |
| Maximální úložiště vnitřní paměti OLTP na fond | 1 GB | 2 GB | 4 GB | 10 GB |
| Maximální počet databází pro každý fond | 50 | 100 | 100 | 100 |
| Maximální počet souběžných pracovních procesů (požadavků) na fond | 200 | 400 | 800 | 1600 |
| Maximální počet souběžných přihlášení na fond | 200 | 400 | 800 | 1600 |
| Maximální počet souběžných relací na fond | 30000 | 30000 | 30000 | 30000 |
| Minimální počet eDTU na databázi | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1 000 |
| Maximální počet eDTU na databázi | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1 000 | 
| Maximální velikost úložiště dat na databázi | 250 GB | 500 GB | 500 GB | 500 GB | 
||||||||

> [!IMPORTANT]
>\* Databáze ve fondu sdílejí úložiště fondu, proto je velikost datového úložiště v elastickém fondu omezená na menší z hodnot zbývajícího úložiště fondu a maximální velikosti úložiště na databázi. 
>
>
>\*\* Minimální/maximální počet eDTU na databázi začínající na 200 eDTU a vyšší je ve verzi Public Preview.
>
>\*\*\* Výchozí maximální velikost úložiště dat na fond pro fondy Premium s 500 eDTU a více je 750 GB. Pokud chcete větší maximální velikost úložiště dat na fond Premium pro 1 000 a více eDTU, je nutné tuto velikost explicitně vybrat pomocí webu Azure Portal, [PowerShellu](../articles/sql-database/sql-database-elastic-pool.md#manage-sql-database-elastic-pools-using-powershell), [Azure CLI](../articles/sql-database/sql-database-elastic-pool.md#manage-sql-database-elastic-pools-using-the-azure-cli) nebo rozhraní [REST API](../articles/sql-database/sql-database-elastic-pool.md#manage-sql-database-elastic-pools-using-the-rest-api). Fondy Premium s větším úložištěm než 1 TB jsou aktuálně ve verzi Public Preview v následujících oblastech: USA – východ 2, USA – západ, USA (Gov) – Virginia, Západní Evropa, Německo – střed, Jihovýchodní Asie, Japonsko – východ, Austrálie – východ, Kanada – střed a Kanada – východ. Maximální velikost úložiště na fond pro všechny ostatní oblasti je aktuálně omezena na 750 GB.
>
