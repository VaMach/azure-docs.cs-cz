---
title: "Powershell: Konfigurace pravidel brány firewall služby SQL Database | Dokumentace Microsoftu"
description: "Zjistěte, jak pomocí Powershellu nakonfigurovat pravidla brány firewall na úrovni serveru pro IP adresy, ze kterých se přistupuje k databázím SQL Azure."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 30dcea72-61c1-48b6-8e1d-b1db2eb61567
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/09/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 86bc7d89bb5725add8ba05b6f0978467147fd3ca
ms.openlocfilehash: d80bd1fbb5cdb0492e521a4d600f657fac0e3325


---
# <a name="configure-azure-sql-database-server-level-firewall-rules-by-using-powershell"></a>Konfigurace pravidel brány firewall na úrovni serveru služby Azure SQL Database pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [Přehled](sql-database-firewall-configure.md)
> * [Azure Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [REST API](sql-database-configure-firewall-settings-rest.md)
> 
> 

Azure SQL Database používá pravidla brány firewall k umožnění připojení k vašim serverům a databázím. Nastavení brány firewall můžete definovat na úrovni serveru nebo databáze pro hlavní nebo uživatelskou databázi na serveru služby SQL Database a selektivně tak umožnit přístup k dané databázi.

> [!IMPORTANT]
> Pokud chcete umožnit aplikacím z Azure připojení k vašemu databázovému serveru, musí být povolená připojení Azure. Další informace o pravidlech brány firewall a povolení připojení z Azure najdete v tématu [Brána firewall služby Azure SQL Database](sql-database-firewall-configure.md). Pokud provádíte připojení v rámci cloudu Azure, možná bude nutné otevřít některé další porty TCP. Další informace najdete v části „SQL Database verze V12: Vnější vs. vnitřní“ tématu [Porty nad 1433 pro technologii ADO.NET 4.5 a službu SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md).
> 
> 

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-server-firewall-rules"></a>Vytvoření pravidel brány firewall serveru
Pravidla brány firewall na úrovni serveru je možné vytvářet, aktualizovat a odstraňovat pomocí Azure PowerShellu.

Pokud chcete vytvořit nové pravidlo brány firewall na úrovni serveru, spusťte rutinu [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx). Následující příklad povolí rozsah IP adres na serveru Contoso.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -ServerName 'Contoso' -FirewallRuleName "ContosoFirewallRule" -StartIpAddress '192.168.1.1' -EndIpAddress '192.168.1.10'        

Pokud chcete upravit stávající pravidlo brány firewall na úrovni serveru, spusťte rutinu [Set-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603789\(v=azure.300\).aspx). Následující příklad změní rozsah přípustných IP adres pro pravidlo s názvem ContosoFirewallRule.

    Set-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -StartIPAddress 192.168.1.4 -EndIPAddress 192.168.1.10 -FirewallRuleName 'ContosoFirewallRule' -ServerName 'Contoso'

Pokud chcete odstranit stávající pravidlo brány firewall na úrovni serveru, spusťte rutinu [Remove-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603588\(v=azure.300\).aspx). Následující příklad odstraní pravidlo s názvem ContosoFirewallRule.

    Remove-AzureRmSqlServerFirewallRule -FirewallRuleName 'ContosoFirewallRule' -ServerName 'Contoso'


## <a name="manage-firewall-rules-by-using-powershell"></a>Správa pravidel brány firewall pomocí prostředí PowerShell
Ke správě pravidel brány firewall můžete použít také prostředí PowerShell. Další informace najdete v následujících tématech:

* [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx)
* [Remove-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603588\(v=azure.300\).aspx)
* [Set-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603789\(v=azure.300\).aspx)
* [Get-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603586\(v=azure.300\).aspx)

## <a name="next-steps"></a>Další kroky
Informace o tom, jak pomocí jazyka Transact SQL vytvořit pravidla brány firewall na úrovni serveru a databáze, najdete v článku [Konfigurace pravidel brány firewall na úrovni serveru a databáze služby Azure SQL Database pomocí jazyka T-SQL](sql-database-configure-firewall-settings-tsql.md).

V těchto článcích najdete informace o vytváření pravidel brány firewall na úrovni serveru dalšími metodami:

* [Konfigurace pravidel brány firewall na úrovni serveru služby Azure SQL Database pomocí webu Azure Portal](sql-database-configure-firewall-settings.md)
* [Konfigurace pravidel brány firewall na úrovni serveru služby Azure SQL Database pomocí rozhraní REST API](sql-database-configure-firewall-settings-rest.md)

Kurz týkající se vytvoření databáze najdete v článku [Vytvoření databáze SQL během několika minut pomocí webu Azure Portal](sql-database-get-started.md).
S připojováním k databázi SQL Azure z open source aplikací nebo aplikací třetích stran vám pomůžou [Ukázky kódu pro rychlý start klientů se službou SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx).
S pochopením přecházení do databází vám pomůže článek [Správa přístupu k databázi a zabezpečení přihlášení](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## <a name="additional-resources"></a>Další zdroje
* [Zabezpečení databáze](sql-database-security-overview.md)
* [Security Center pro databázový stroj SQL Server a Azure SQL Database](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->



<!--HONumber=Jan17_HO1-->


