---
title: "Pravidla brány firewall na úrovni serveru a databáze služby Azure SQL Database pomocí jazyka T-SQL | Dokumentace Microsoftu"
description: "Zjistěte, jak nakonfigurovat bránu firewall pro IP adresy, ze kterých se přistupuje k databázím SQL Azure."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
ms.assetid: 71e692a1-5e2f-4a18-a6d6-527b849cf68e
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/30/2016
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: a49bc9cd8fb50dcc753fbb04bf7f2d96cf79f3ab


---
# <a name="configure-azure-sql-database-server-level-and-database-level-firewall-rules-using-t-sql"></a>Konfigurace pravidel brány firewall na úrovni serveru a databáze služby Azure SQL Database pomocí jazyka T-SQL
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

## <a name="server-level-firewall-rules"></a>Pravidla brány firewall na úrovni serveru
Pouze hlavní přihlášení na úrovni serveru nebo správce Azure Active Directory může vytvořit pravidlo brány firewall na úrovni serveru pomocí jazyka Transact-SQL.

1. Otevřete okno dotazu a připojte se k virtuální hlavní databázi pomocí aplikace SQL Server Management Studio.
2. Z okna dotazu je možné pravidla brány firewall na úrovni serveru vybírat, vytvářet, aktualizovat a odstraňovat.
3. Pokud chcete vytvořit nebo aktualizovat pravidla brány firewall na úrovni serveru, spusťte uloženou proceduru `sp_set_firewall_rule`. Následující příklad povolí rozsah IP adres na serveru Contoso.<br/>Začněte tím, že zobrazíte existující pravidla.
   
        SELECT * FROM sys.firewall_rules ORDER BY name;
   
    Potom přidejte pravidlo brány firewall.
   
        EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
            @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
   
    Pokud chcete odstranit pravidlo brány firewall na úrovni serveru, spusťte uloženou proceduru sp_delete_firewall_rule. Následující příklad odstraní pravidlo s názvem ContosoFirewallRule.
   
        EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
   
   Další informace o těchto uložených procedurách najdete v článcích [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) a [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx).

## <a name="database-level-firewall-rules"></a>Pravidla brány firewall na úrovni databáze
Pouze uživatel databáze s oprávněním **CONTROL** k databázi (například vlastník databáze) může vytvořit pravidlo brány firewall na úrovni databáze.

1. Po vytvoření brány firewall na úrovni serveru pro vaši IP adresu otevřete přes portál Classic nebo aplikaci SQL Server Management Studio okno dotazu.
2. Připojte se k databázi, pro kterou chcete vytvořit pravidlo brány firewall na úrovni databáze.
   
    Pokud chcete vytvořit nové nebo aktualizovat stávající pravidlo brány firewall na úrovni databáze, spusťte uloženou proceduru `sp_set_database_firewall_rule`. Následující příklad vytvoří nové pravidlo brány firewall s názvem ContosoFirewallRule.
   
        EXEC sp_set_database_firewall_rule @name = N'ContosoFirewallRule', 
            @start_ip_address = '192.168.1.11', @end_ip_address = '192.168.1.11'
   
    Pokud chcete odstranit stávající pravidlo brány firewall na úrovni databáze, spusťte uloženou proceduru `sp_delete_database_firewall_rule`. Následující příklad odstraní pravidlo s názvem ContosoFirewallRule.
   `
   
        EXEC sp_delete_database_firewall_rule @name = N'ContosoFirewallRule'

Další informace o těchto uložených procedurách najdete v článcích [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) a [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx).

## <a name="next-steps"></a>Další kroky
V těchto článcích najdete informace o vytváření pravidel brány firewall na úrovni serveru dalšími metodami: 

* [Konfigurace pravidel brány firewall na úrovni serveru služby Azure SQL Database pomocí webu Azure Portal](sql-database-configure-firewall-settings.md)
* [Konfigurace pravidel brány firewall na úrovni serveru služby Azure SQL Database pomocí prostředí PowerShell](sql-database-configure-firewall-settings-powershell.md)
* [Konfigurace pravidel brány firewall na úrovni serveru služby Azure SQL Database pomocí rozhraní REST API](sql-database-configure-firewall-settings-rest.md)

Kurz týkající se vytvoření databáze najdete v článku [Vytvoření databáze SQL během několika minut pomocí webu Azure Portal](sql-database-get-started.md).
S připojováním k databázi SQL Azure z open source aplikací nebo aplikací třetích stran vám pomůžou [Ukázky kódu pro rychlý start klientů se službou SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx).
S pochopením přecházení do databází vám pomůže článek [Správa přístupu k databázi a zabezpečení přihlášení](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## <a name="additional-resources"></a>Další zdroje
* [Zabezpečení databáze](sql-database-security-overview.md)
* [Security Center pro databázový stroj SQL Server a Azure SQL Database](https://msdn.microsoft.com/library/bb510589)




<!--HONumber=Dec16_HO4-->


