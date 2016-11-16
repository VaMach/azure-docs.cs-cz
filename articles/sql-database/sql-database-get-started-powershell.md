---
title: "Nové nastavení SQL Database pomocí PowerShellu | Dokumentace Microsoftu"
description: "Naučte se vytvářet databáze pomocí prostředí PowerShell. Běžných úlohy vytváření databází lze provádět pomocí rutin prostředí PowerShell."
keywords: "vytvoření nové databáze sql, nastavení databáze"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: 7d99869b-cec5-4583-8c1c-4c663f4afd4d
ms.service: sql-database
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: powershell
ms.workload: data-management
ms.date: 08/19/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 87e52fe29f659577d7dc0c9661ebde2c1c475cfc


---
# <a name="create-a-sql-database-and-perform-common-database-setup-tasks-with-powershell-cmdlets"></a>Vytvoření databáze SQL a provádění běžných úloh úvodního nastavení databáze pomocí rutin prostředí PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](sql-database-get-started.md)
> * [PowerShell](sql-database-get-started-powershell.md)
> * [C#](sql-database-get-started-csharp.md)
> 
> 

Naučte se vytvářet databáze SQL pomocí rutin PowerShell. (Potřebujete-li vytvářet elastické databáze, přečtěte si článek [Vytvoření nového fondu elastické databáze pomocí prostředí PowerShell](sql-database-elastic-pool-create-powershell.md).)

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="database-setup-create-a-resource-group-server-and-firewall-rule"></a>Vytvoření databáze: vytvoření skupiny prostředků, serveru a pravidla brány firewall
Jakmile máte přístup ke spouštění rutin ve vašem vybraném předplatném Azure, je dalším krokem stanovení skupiny prostředků, která bude obsahovat server s novou databází. Následující příklad můžete upravit tak, aby používal libovolné platné umístění. Spuštěním příkazu **(Get-AzureRmLocation | Where-Object { $_.Providers -eq "Microsoft.Sql" }).Location** získáte seznam platných umístění.

Spuštěním následujícího příkazu vytvoříte skupinu prostředků:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "westus"


### <a name="create-a-server"></a>Vytvoření serveru
Databáze SQL se vytvářejí na serverech služby Azure SQL Database. Server vytvoříte příkazem [New-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603715\(v=azure.300\).aspx). Název vašeho serveru musí být jedinečný mezi všemi servery SQL Azure Database. Pokud se již název používá, obdržíte chybu. Stojí také za zmínku, že dokončení tohoto příkazu může trvat i několik minut. Příkaz můžete upravit tak, aby používal jakékoli platné umístění, ale je třeba určit stejné umístění, které jste použili pro vytvoření skupiny prostředků v předchozím kroku.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "westus" -ServerVersion "12.0"

Při spuštění tohoto příkazu budete vyzváni k zadání uživatelského jména a hesla. Nezadávejte svoje přihlašovací údaje služby Azure. Místo toho zadejte uživatelské jméno a heslo pro vytvoření jako správce serveru. Skript v dolní části tohoto článku ukazuje, jak nastavit přihlašovací údaje serveru v kódu.

Po úspěšném vytvoření serveru se o něm zobrazí podrobnosti.

### <a name="configure-a-server-firewall-rule-to-allow-access-to-the-server"></a>Konfigurace pravidla brány firewall pro povolení přístupu k serveru
Pro přístup k serveru musíte vytvořit pravidlo brány firewall. Spusťte příkaz [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx) a nahraďte počáteční a koncovou IP adresu platnou hodnotou pro váš počítač.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

Po úspěšném vytvoření pravidla se zobrazí podrobnosti o něm.

Pokud chcete povolit jiné službě Azure přístup k serveru, přidejte pravidlo brány firewall a nastavte StartIpAddress i EndIpAddress na hodnotu 0.0.0.0. Toto pravidlo umožní přístup k serveru z jakéhokoli předplatného Azure.

Další informace najdete v tématu [Brána firewall služby Azure SQL Database](sql-database-firewall-configure.md).

## <a name="create-a-sql-database"></a>Vytvoření databáze SQL
Nyní máte skupinu prostředků, server a pravidlo brány firewall, můžete se tedy připojit k serveru.

Příkaz [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339\(v=azure.300\).aspx) vytvoří (prázdnou) databázi SQL s úrovní služeb Standard a s úrovní výkonu S1:

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


Po úspěšném vytvoření databáze se zobrazí podrobnosti o ní.

## <a name="create-a-sql-database-powershell-script"></a>Skript prostředí PowerShell pro vytvoření databáze SQL
Následující skript prostředí PowerShell vytvoří databázi SQL a všechny její závislé prostředky. Nahraďte všechny výskyty `{variables}` hodnotami specifickými pro vaše předplatné a prostředky (při nastavování hodnot odeberte závorky **{}**).

    # Sign in to Azure and set the subscription to work with
    $SubscriptionId = "{subscription-id}"

    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId

    # CREATE A RESOURCE GROUP
    $resourceGroupName = "{group-name}"
    $rglocation = "{Azure-region}"

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $rglocation

    # CREATE A SERVER
    $serverName = "{server-name}"
    $serverVersion = "12.0"
    $serverLocation = "{Azure-region}"

    $serverAdmin = "{server-admin}"
    $serverPassword = "{server-password}" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

    $sqlDbServer = New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds

    # CREATE A SERVER FIREWALL RULE
    $ip = (Test-Connection -ComputerName $env:COMPUTERNAME -Count 1 -Verbose).IPV4Address.IPAddressToString
    $firewallRuleName = '{rule-name}'
    $firewallStartIp = $ip
    $firewallEndIp = $ip

    $fireWallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName $firewallRuleName -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp


    # CREATE A SQL DATABASE
    $databaseName = "{database-name}"
    $databaseEdition = "{Standard}"
    $databaseSlo = "{S0}"

    $sqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $databaseEdition -RequestedServiceObjectiveName $databaseSlo


    # REMOVE ALL RESOURCES THE SCRIPT JUST CREATED
    #Remove-AzureRmResourceGroup -Name $resourceGroupName






## <a name="next-steps"></a>Další kroky
Po vytvoření databáze SQL a provedení běžných úloh její úvodního nastavení jste připraveni na další krok:

* [Správa SQL Database pomocí prostředí PowerShell](sql-database-manage-powershell.md)
* [Připojení k SQL Database přes SQL Server Management Studio a provedení ukázkového dotazu T-SQL](sql-database-connect-query-ssms.md)

## <a name="additional-resources"></a>Další zdroje
* [Rutiny Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx)
* [Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)




<!--HONumber=Nov16_HO2-->


