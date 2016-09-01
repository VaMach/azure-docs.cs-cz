<properties
    pageTitle="Nové nastavení SQL Database pomocí PowerShellu | Microsoft Azure"
    description="Naučte se vytvářet nové databáze pomocí prostředí PowerShell. Běžných úlohy vytváření databází lze provádět pomocí rutin prostředí PowerShell."
    keywords="vytvoření nové databáze sql, nastavení databáze"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="hero-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="05/09/2016"
    ms.author="sstein"/>

# Vytvoření nové databáze SQL a provádění běžných úloh úvodního nastavení databáze pomocí rutin prostředí PowerShell


> [AZURE.SELECTOR]
- [Azure Portal](sql-database-get-started.md)
- [PowerShell](sql-database-get-started-powershell.md)
- [C#](sql-database-get-started-csharp.md)



Naučte se vytvářet nové databáze SQL pomocí rutin PowerShell. (Potřebujete-li vytvářet elastické databáze, přečtěte si článek [Vytvoření nového fondu elastické databáze pomocí prostředí PowerShell](sql-database-elastic-pool-create-powershell.md).)


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## Vytvoření databáze: vytvoření skupiny prostředků, serveru a pravidla brány firewall

Jakmile máte přístup ke spouštění rutin ve vašem vybraném předplatném Azure, je dalším krokem stanovení skupiny prostředků, která bude obsahovat server s novou databází. Následující příklad můžete upravit tak, aby používal libovolné platné umístění. Spuštěním příkazu **(Get-AzureRmLocation | Where-Object { $_.Providers -eq "Microsoft.Sql" }).Location** získáte seznam platných umístění.

Spuštěním následujícího příkazu vytvoříte novou skupinu prostředků:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "West US"

Po úspěšném vytvoření nové skupiny prostředků uvidíte následující informace: **ProvisioningState : Succeeded** (Stav zřizování: úspěch).


### Vytvoření serveru

Databáze SQL se vytvářejí na serverech služby Azure SQL Database. Nový server vytvoříte příkazem **New-AzureRmSqlServer**. Parametr *ServerName* nahraďte názvem vašeho serveru. Tento název musí být jedinečný pro všechny servery databáze SQL Azure. Pokud se již název používá, obdržíte chybu. Stojí také za zmínku, že dokončení tohoto příkazu může trvat i několik minut. Příkaz můžete upravit tak, aby používal jakékoli platné umístění, ale je třeba určit stejné umístění, které jste použili pro vytvoření skupiny prostředků v předchozím kroku.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "West US" -ServerVersion "12.0"

Při spuštění tohoto příkazu budete vyzváni k zadání uživatelského jména a hesla. Nezadávejte svoje přihlašovací údaje služby Azure. Místo toho zadejte uživatelské jméno a heslo, které chcete používat jako správce nového serveru.

Po úspěšném vytvoření serveru se o něm zobrazí podrobnosti.

### Konfigurace pravidla brány firewall pro povolení přístupu k serveru

Vytvořte pravidlo brány firewall pro přístup k serveru. Spusťte následující příkaz a nahraďte počáteční a koncovou IP adresu platnou hodnotou pro váš počítač.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

Po úspěšném vytvoření pravidla se zobrazí podrobnosti o něm.

Pokud chcete povolit jiné službě Azure přístup k serveru, přidejte pravidlo brány firewall a nastavte StartIpAddress i EndIpAddress na hodnotu 0.0.0.0. Pamatujte ale, že tím umožníte přístup k serveru z jakéhokoli předplatného Azure.

Další informace najdete v tématu [Brána firewall služby Azure SQL Database](sql-database-firewall-configure.md).


## Vytvoření nové databáze SQL

Nyní máte skupinu prostředků, server a pravidlo brány firewall, můžete se tedy připojit k serveru.

Následující příkaz vytvoří novou (prázdnou) databázi SQL s úrovní služeb Standard služby a s úrovní výkonu S1:


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


Po úspěšném vytvoření databáze se zobrazí podrobnosti o ní.

## Skript prostředí PowerShell pro vytvoření nové databáze SQL

Následuje skript prostředí PowerShell pro vytvoření nové databáze SQL:

    $SubscriptionId = "4cac86b0-1e56-bbbb-aaaa-000000000000"
    $ResourceGroupName = "resourcegroupname"
    $Location = "Japan West"

    $ServerName = "uniqueservername"

    $FirewallRuleName = "rule1"
    $FirewallStartIP = "192.168.0.0"
    $FirewallEndIp = "192.168.0.0"

    $DatabaseName = "database1"
    $DatabaseEdition = "Standard"
    $DatabasePerfomanceLevel = "S1"


    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionId

    $ResourceGroup = New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    $Server = New-AzureRmSqlServer -ResourceGroupName $ResourceGroupName -ServerName $ServerName -Location $Location -ServerVersion "12.0"

    $FirewallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $ResourceGroupName -ServerName $ServerName -FirewallRuleName $FirewallRuleName -StartIpAddress $FirewallStartIP -EndIpAddress $FirewallEndIp

    $SqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -Edition $DatabaseEdition -RequestedServiceObjectiveName $DatabasePerfomanceLevel

    $SqlDatabase



## Další kroky
Po vytvoření nové databáze SQL a provedení běžných úloh její úvodního nastavení jste připraveni na další krok:

- [Připojení k SQL Database přes SQL Server Management Studio a provedení ukázkového dotazu T-SQL](sql-database-connect-query-ssms.md).


## Další zdroje

- [Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)



<!---HONumber=Aug16_HO4-->


