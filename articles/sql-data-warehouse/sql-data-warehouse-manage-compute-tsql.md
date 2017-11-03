---
title: "Pozastavit, obnovit a škálovat pomocí T-SQL v Azure SQL Data Warehouse | Microsoft Docs"
description: "Příkaz Transact-SQL (T-SQL) úlohy škálovatelný výkon úpravou Dwu. Uložte náklady škálování zpět v době mimo špičku."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: a970d939-2adf-4856-8a78-d4fe8ab2cceb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 03/30/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: 9221d72ecf8ab2ba8b04e4bc97eeef7157817cca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a>Spravovat výpočetní výkon v Azure SQL Data Warehouse (T-SQL)
> [!div class="op_single_selector"]
> * [Přehled](sql-data-warehouse-manage-compute-overview.md)
> * [Azure Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a>Aktuální nastavení DWU zobrazení
Chcete-li zobrazit aktuální nastavení DWU pro vaše databáze:

1. Otevřete Průzkumník objektů systému SQL Server v sadě Visual Studio.
2. Připojení k databázi hlavní přidružené k logickému serveru služby SQL Database.
3. Vyberte ze zobrazení dynamické správy sys.database_service_objectives. Zde naleznete příklad: 

```sql
SELECT
    db.name [Database]
,   ds.edition [Edition]
,   ds.service_objective [Service Objective]
FROM
    sys.database_service_objectives ds
JOIN
    sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Škálování výpočetní kapacity
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Chcete-li změnit jednotkami Dwu:

1. Připojení k databázi hlavní přidružené k logické databáze SQL serveru.
2. Použití [ALTER DATABASE] [ ALTER DATABASE] příkaz TSQL. Následující příklad nastaví DW1000 cíle na úrovni služby pro databázi MySQLDW. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state-and-operation-progress"></a>Kontrola stavu a operaci průběhu databáze

1. Připojení k databázi hlavní přidružené k logické databáze SQL serveru.
2. Odesílání dotazu pro kontrolu stavu databáze

```sql
SELECT *
FROM
sys.databases
```

3. Odesílání dotazu a zkontrolujte stav operace

```sql
SELECT *
FROM
    sys.dm_operation_status
WHERE
    resource_type_desc = 'Database'
AND 
    major_resource_id = 'MySQLDW'
```

Tato DMV vrátí informace o různé operace správy v SQL Data Warehouse například operaci a stav operace, který bude buď IN_PROGRESS nebo byla DOKONČENA.



<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Další kroky
Další úlohy správy, najdete v části [přehled správy][Management overview].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute power overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
