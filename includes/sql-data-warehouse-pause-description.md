
<!--
includes/sql-data-warehouse-include-pause-description.md

Latest Freshness check:  2016-04-22 , barbkess.

As of circa 2016-04-22, the following topics might include this include:
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-powershell.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-rest-api.md

-->
Abyste ušetřili náklady, můžete pozastavit a obnovit výpočetní prostředky na vyžádání. Například pokud nebudete používat databázi v noci a o víkendech, můžete pozastavit tyto v době a obnovit během dne. Zatímco databáze byla pozastavena, se nezapočítávají pro Dwu.

Při přesunutí ukazatele myši databáze:

* Paměťovou a výpočetní prostředky se vrátí do fondu dostupné prostředky v datovém centru
* DWU náklady jsou nula po dobu trvání pozastavení.
* Úložiště dat není ovlivněná a vaše data zůstává beze změn. 
* SQL Data Warehouse zruší všechny spuštěné nebo zařazené ve frontě operace.

