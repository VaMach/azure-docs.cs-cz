---
title: "REST API: Pravidla brány firewall služby Azure SQL Database na úrovni serveru | Dokumentace Microsoftu"
description: "Zjistěte, jak pomocí rozhraní REST API nakonfigurovat pravidla brány firewall na úrovni serveru pro IP adresy, ze kterých se přistupuje k databázím Azure SQL."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: fc874f3c-c623-4924-8cb7-32a8c31e666c
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
ms.openlocfilehash: 6ac9bdf34e8cf0a8ca8e06ae3ed6cd2dd1cf408b


---
# <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>Konfigurace pravidel brány firewall na úrovni serveru služby Azure SQL Database pomocí rozhraní REST API
> [!div class="op_single_selector"]
> * [Přehled](sql-database-firewall-configure.md)
> * [Azure Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [REST API](sql-database-configure-firewall-settings-rest.md)
> 
> 

Microsoft Azure SQL Database používá pravidla brány firewall k umožnění připojení k vašim serverům a databázím. Nastavení brány firewall můžete definovat na úrovni serveru nebo databáze pro hlavní nebo uživatelskou databázi na serveru služby Azure SQL Database a selektivně tak umožnit přístup k dané databázi.

> [!IMPORTANT]
> Pokud chcete umožnit aplikacím z Azure připojení k vašemu databázovému serveru, musí být povolená připojení Azure. Další informace o pravidlech brány firewall a povolení připojení z Azure najdete v tématu [Brána firewall služby Azure SQL Database](sql-database-firewall-configure.md). Pokud provádíte připojení v rámci cloudu Azure, možná bude nutné otevřít některé další porty TCP. Další informace najdete v části **SQL Database verze V12: Vnější vs. vnitřní** tématu [Porty nad 1433 pro technologii ADO.NET 4.5 a službu SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md).
> 
> 

## <a name="manage-server-level-firewall-rules-through-rest-api"></a>Správa pravidel brány firewall na úrovni serveru přes rozhraní REST API
1. Správa pravidel brány firewall přes rozhraní REST API musí být ověřená. Informace najdete v [Průvodce vývojáře k ověřování pomocí rozhraní API Azure Resource Manageru](../azure-resource-manager/resource-manager-api-authentication.md).
2. Pravidla na úrovni serveru můžete vytvořit, aktualizovat nebo odstranit pomocí rozhraní REST API.
   
    Pokud chcete vytvořit nebo aktualizovat pravidlo brány firewall na úrovni serveru, proveďte metodu PUT následujícím způsobem:
   
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
   
    Text žádosti
   
        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 

    Pokud chcete odstranit existující pravidlo brány firewall na úrovni serveru, proveďte metodu DELETE následujícím způsobem:

        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>Správa pravidel brány firewall pomocí rozhraní REST API
* [Vytvoření nebo aktualizace pravidla brány firewall](https://msdn.microsoft.com/library/azure/mt445501.aspx)
* [Odstranění pravidla brány firewall](https://msdn.microsoft.com/library/azure/mt445502.aspx)
* [Získání pravidla brány firewall](https://msdn.microsoft.com/library/azure/mt445503.aspx)
* [Výpis všech pravidel brány firewall](https://msdn.microsoft.com/library/azure/mt604478.aspx)

## <a name="next-steps"></a>Další kroky
Informace o tom, jak pomocí jazyka Transact SQL vytvořit pravidla brány firewall na úrovni serveru a databáze, najdete v článku [Konfigurace pravidel brány firewall na úrovni serveru a databáze služby Azure SQL Database pomocí jazyka T-SQL](sql-database-configure-firewall-settings-tsql.md). 

V těchto článcích najdete informace o vytváření pravidel brány firewall na úrovni serveru dalšími metodami: 

* [Konfigurace pravidel brány firewall na úrovni serveru služby Azure SQL Database pomocí webu Azure Portal](sql-database-configure-firewall-settings.md)
* [Konfigurace pravidel brány firewall na úrovni serveru služby Azure SQL Database pomocí prostředí PowerShell](sql-database-configure-firewall-settings-powershell.md)

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


