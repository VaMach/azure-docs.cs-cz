---
title: "Azure PowerShell: Vytvoření databáze SQL | Dokumentace Microsoftu"
description: "Naučíte se vytvářet logický server služby SQL Database, pravidlo brány firewall na úrovni serveru a databáze na webu Azure Portal."
keywords: "kurz k sql database, vytvoření databáze sql"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: carlrab
ms.translationtype: Human Translation
ms.sourcegitcommit: 8be2bcb9179e9af0957fcee69680ac803fd3d918
ms.openlocfilehash: e43f2f6bb6d4f7e280f8874c31ec79c01b54a84b
ms.contentlocale: cs-cz
ms.lasthandoff: 06/23/2017

---

# Vytvoření izolované databáze SQL Azure pomocí PowerShellu
<a id="create-a-single-azure-sql-database-using-powershell" class="xliff"></a>

PowerShell slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. Tento průvodce podrobně uvádí, jak pomocí PowerShellu nasadit databázi SQL Azure ve [skupině prostředků Azure](../azure-resource-manager/resource-group-overview.md) na [logický server Azure SQL Database](sql-database-features.md).

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

Tento kurz vyžaduje modul Azure PowerShell verze 4.0 nebo novější. Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps). 

## Přihlaste se k Azure.
<a id="log-in-to-azure" class="xliff"></a>

Přihlaste se k předplatnému Azure pomocí příkazu [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) a postupujte podle pokynů na obrazovce.

```powershell
Add-AzureRmAccount
```

## Vytvoření proměnných
<a id="create-variables" class="xliff"></a>

V tomto rychlém startu definujte proměnné, které se použijí ve skriptech.

```powershell
# The data center and resource name for your resources
$resourcegroupname = "myResourceGroup"
$location = "WestEurope"
# The logical server name: Use a random value or replace with your own value (do not capitalize)
$servername = "server-$(Get-Random)"
# Set an admin login and password for your database
# The login information for the server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# The ip address range that you want to allow to access your server - change as appropriate
$startip = "0.0.0.0"
$endip = "0.0.0.0"
# The database name
$databasename = "mySampleDatabase"
```

## Vytvoření skupiny prostředků
<a id="create-a-resource-group" class="xliff"></a>

Vytvořte [skupinu prostředků Azure](../azure-resource-manager/resource-group-overview.md) pomocí příkazu [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina. Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v umístění `westeurope`.

```powershell
New-AzureRmResourceGroup -Name $resourcegroupname -Location $location
```
## Vytvoření logického serveru
<a id="create-a-logical-server" class="xliff"></a>

Vytvořte [logický server Azure SQL Database](sql-database-features.md) pomocí příkazu [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver). Logický server obsahuje soubor databází spravovaných jako skupina. Následující příklad vytvoří ve skupině prostředků náhodně pojmenovaný server s přihlašovacím jménem správce `ServerAdmin` a heslem `ChangeYourAdminPassword1`. Podle potřeby tyto předdefinované hodnoty nahraďte.

```powershell
New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## Konfigurace pravidla brány firewall serveru
<a id="configure-a-server-firewall-rule" class="xliff"></a>

Vytvořte [pravidlo brány firewall na úrovni serveru služby Azure SQL Database](sql-database-firewall-configure.md) pomocí příkazu [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule). Pravidlo brány firewall na úrovni serveru umožňuje externí aplikaci, jako je SQL Server Management Studio nebo nástroj SQLCMD, připojení k databázi SQL přes bránu firewall služby SQL Database. V následujícím příkladu je brána firewall otevřená pouze pro ostatní prostředky Azure. Pokud chcete povolit externí připojení, změňte IP adresu na příslušnou adresu pro vaše prostředí. Chcete-li otevřít všechny IP adresy, použijte jako počáteční IP adresu 0.0.0.0 a jako koncovou adresu 255.255.255.255.

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> SQL Database komunikuje přes port 1433. Pokud se pokoušíte připojit z podnikové sítě, nemusí být odchozí provoz přes port 1433 bránou firewall vaší sítě povolený. Pokud je to tak, nebudete se moct připojit k serveru Azure SQL Database, dokud vaše IT oddělení neotevře port 1433.
>

## Vytvoření databáze s ukázkovými daty na serveru
<a id="create-a-database-in-the-server-with-sample-data" class="xliff"></a>

Vytvořte na serveru databázi s [úrovní výkonu S0](sql-database-service-tiers.md) pomocí příkazu [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase). Následující příklad vytvoří databázi s názvem `mySampleDatabase` a načte do této databáze ukázková data AdventureWorksLT. Nahraďte podle potřeby tyto předdefinované hodnoty (další rychlé starty v této kolekci jsou postavené na tomto rychlém startu).

```powershell
New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -SampleName "AdventureWorksLT" `
    -RequestedServiceObjectiveName "S0"
```

## Vyčištění prostředků
<a id="clean-up-resources" class="xliff"></a>

Další rychlé starty v této kolekci jsou postavené na tomto rychlém startu. 

> [!TIP]
> Pokud chcete pokračovat v práci s dalšími rychlými starty, nevyčišťujte prostředky vytvořené v rámci tohoto rychlého startu. Pokud pokračovat nechcete, pomocí následujících kroků odstraňte všechny prostředky vytvořené tímto rychlým startem na webu Azure Portal.
>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## Další kroky
<a id="next-steps" class="xliff"></a>

Teď, když máte databázi, můžete se k ní připojit a provádět dotazování pomocí vašich oblíbených nástrojů. Další informace získáte výběrem vašeho nástroje níže:

- [SQL Server Management Studio](sql-database-connect-query-ssms.md)
- [Visual Studio Code](sql-database-connect-query-vscode.md)
- [.NET](sql-database-connect-query-dotnet.md)
- [PHP](sql-database-connect-query-php.md)
- [Node.js](sql-database-connect-query-nodejs.md)
- [Java](sql-database-connect-query-java.md)
- [Python](sql-database-connect-query-python.md)
- [Ruby](sql-database-connect-query-ruby.md)


