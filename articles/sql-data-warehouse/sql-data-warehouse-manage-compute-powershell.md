---
title: "Spravovat výpočetní výkon v Azure SQL Data Warehouse (PowerShell) | Microsoft Docs"
description: "Úkoly prostředí PowerShell pro správu výpočetního výkonu. Škálovat výpočetní prostředky úpravou Dwu. Nebo, pozastavení a obnovení výpočetní prostředky, abyste ušetřili náklady."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 8354a3c1-4e04-4809-933f-db414a8c74dc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 6a185d96447c2e1b0b463439dd062081e783da5f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>Spravovat výpočetní výkon v Azure SQL Data Warehouse (PowerShell)
> [!div class="op_single_selector"]
> * [Přehled](sql-data-warehouse-manage-compute-overview.md)
> * [Azure Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

## <a name="before-you-begin"></a>Než začnete
### <a name="install-the-latest-version-of-azure-powershell"></a>Nainstalujte nejnovější verzi prostředí Azure PowerShell
> [!NOTE]
> Pomocí prostředí Azure PowerShell s SQL Data Warehouse, je nutné prostředí Azure PowerShell verze 1.0.3 nebo novější.  K ověření vaší aktuální verzí, spusťte příkaz **Get-Module - ListAvailable-Name Azure**. Můžete nainstalovat nejnovější verzi z [instalačního programu webové platformy Microsoft][Microsoft Web Platform Installer].  Další informace najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell][How to install and configure Azure PowerShell].
>
> 

### <a name="get-started-with-azure-powershell-cmdlets"></a>Začínáme s rutinami prostředí Azure PowerShell
Abyste mohli začít:

1. Otevřete prostředí Azure PowerShell.
2. Do příkazového řádku prostředí PowerShell spusťte tyto příkazy a přihlaste se k Azure Resource Manager a vybrat své předplatné.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Škálování výpočetní výkon
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Chcete-li změnit jednotkami Dwu, použijte [Set-AzureRmSqlDatabase] [ Set-AzureRmSqlDatabase] rutiny prostředí PowerShell. Následující příklad nastaví DW1000 cíle na úrovni služby pro databázi MySQLDW, který je hostován na serveru MyServer.

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Pozastavit výpočetní
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Chcete-li pozastavit databázi, použijte [Suspend-AzureRmSqlDatabase] [ Suspend-AzureRmSqlDatabase] rutiny. Následující příklad pozastaví databáze s názvem Database02 hostovaná na serveru s názvem Server01. Server je ve skupině prostředků Azure s názvem ResourceGroup1.

> [!NOTE]
> Všimněte si, že pokud je váš server foo.database.windows.net, použijte "foo" jako parametr ServerName - rutin prostředí PowerShell.
>
> 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
Variace, načte tento další příklad databázi do $database objektu. Je následně prostřednictvím kanálu předá objekt, který má [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]. Výsledky jsou uloženy v resultDatabase objektu. Poslední příkaz zobrazí výsledky.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Obnovit výpočetní
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Chcete-li spustit databázi, použijte [Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] rutiny. Následující příklad spustí databáze s názvem Database02 hostovaná na serveru s názvem Server01. Server je ve skupině prostředků Azure s názvem ResourceGroup1.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

Variace, načte tento další příklad databázi do $database objektu. Je následně prostřednictvím kanálu předá objekt, který má [Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] a ukládá výsledky do $resultDatabase. Poslední příkaz zobrazí výsledky.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state"></a>Zkontrolujte stav databáze

Jak je znázorněno v předchozích příkladech, můžete použít jednu [Get-AzureRmSqlDatabase] [ Get-AzureRmSqlDatabase] rutiny získat informace o databázi, a tím Kontrola stavu, ale také použít jako argument. 

```powershell
Get-AzureRmSqlDatabase [-ResourceGroupName] <String> [-ServerName] <String> [[-DatabaseName] <String>]
 [-InformationAction <ActionPreference>] [-InformationVariable <String>] [-Confirm] [-WhatIf]
 [<CommonParameters>]
```

Které budou jako výsledek v něco 

```powershell   
ResourceGroupName             : nytrg
ServerName                    : nytsvr
DatabaseName                  : nytdb
Location                      : West US
DatabaseId                    : 86461aae-8e3d-4ded-9389-ac9d4bc69bbb
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1General_CP1CI_AS
CatalogCollation              :
MaxSizeBytes                  : 32212254720
Status                        : Online
CreationDate                  : 10/26/2016 4:33:14 PM
CurrentServiceObjectiveId     : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
CurrentServiceObjectiveName   : System2
RequestedServiceObjectiveId   : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           : 1/1/0001 12:00:00 AM
```

Kde je můžete pak zkontrolovat *stav* databáze. V takovém případě se zobrazí, že tato databáze je online. 

Když spustíte tento příkaz, měli byste obdržet hodnotou stavu buď Online, pozastavení, obnovování, škálování a pozastaveno.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Další kroky
Další úlohy správy, najdete v části [přehled správy][Management overview].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Get-AzureRmSqlDatabase]: /powershell/servicemanagement/azure.sqldatabase/v1.6.1/get-azuresqldatabase

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
