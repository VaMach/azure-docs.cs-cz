---
title: "Přehled: nástroje pro správu služby SQL Database | Dokumentace Microsoftu"
description: "Porovnání nástrojů a možností pro správu služby Azure SQL Database"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 37767380-975f-4dee-a28d-80bc2036dda3
ms.service: sql-database
ms.custom: overview
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/24/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 73cc9d1d02ed915466f0eac2d9a42fe8cf9e2fb9
ms.openlocfilehash: 32d0a1fd8671bbd9d450711dd686ca49c2178c16


---
# <a name="overview-management-tools-for-sql-database"></a>Přehled: nástroje pro správu služby SQL Database
Toto téma popisuje a porovnává nástroje a možnosti pro správu databází služby Azure SQL Database a pomůže vám vybrat nejvhodnější nástroj pro vaše úlohy, vaši firmu a pro vás. Výběr nejvhodnějšího nástroje závisí na tom, kolik databází budete spravovat, jaké úlohy budete provádět a jak často je budete provádět.

## <a name="azure-portal"></a>Azure Portal
[Azure Portal](https://portal.azure.com) je webová aplikace, ve které můžete vytvářet, aktualizovat a odstraňovat databáze a logické servery a sledovat činnost databází. Je to skvělý nástroj, pokud s Azure začínáte, spravujete jenom několik databází nebo potřebuje něco udělat rychle.

Další informace o používání tohoto portálu najdete v tématu [Správa databází služby SQL Database pomocí portálu Azure Portal](sql-database-manage-portal.md).

## <a name="sql-server-management-studio-and-sql-server-data-tools-in-visual-studio"></a>SQL Server Management Studio a SQL Server Data Tools ve Visual Studiu
SQL Server Management Studio (SSMS) a SQL Server Data Tools (SSDT) jsou klientské nástroje určené ke spouštění na počítači, který používáte pro správu a vývoj databáze v cloudu. Pokud vyvíjíte aplikace a znáte Visual Studio nebo jiná integrovaná vývojová prostředí (IDE), [vyzkoušejte SSDT ve Visual Studiu](https://msdn.microsoft.com/library/mt204009.aspx). Mnoho správců databází zná nástroj SSMS, který je možné používat u databází služby Azure SQL Database. [Stáhněte si nejnovější verzi SSMS](https://msdn.microsoft.com/library/mt238290) a při práci se službou Azure SQL Database vždy používejte nejnovější verzi. Další informace o správě databází služby Azure SQL Database pomocí SSMS najdete v tématu [Správa databází služby SQL Database pomocí SSMS](sql-database-manage-azure-ssms.md).

> [!IMPORTANT]
> Vždy používejte nejnovější verzi nástrojů [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290) a [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) – nástroje tak budou synchronizovány s aktualizacemi služeb Microsoft Azure a SQL Database.
>  

## <a name="powershell"></a>PowerShell
Ke správě databází a elastických fondů a k automatizaci nasazení prostředků Azure můžete použít PowerShell. Microsoft doporučuje tento nástroj pro správu velkého počtu databází a k automatizaci nasazení a změn prostředků v produkčním prostředí.

Další informace najdete v tématu [Správa služby SQL Database pomocí PowerShellu](sql-database-manage-powershell.md)

## <a name="elastic-database-tools"></a>Nástroje pro elastické databáze
Nástroje pro elastické databáze slouží například k provádění následujících akcí: 

* Spuštění skriptu T-SQL pro sadu databází pomocí [elastické úlohy](sql-database-elastic-jobs-overview.md)
* Přesunutí databází založených na modelu s více tenanty do modelu s jedním tenantem pomocí [nástroje pro dělení a slučování](sql-database-elastic-scale-overview-split-and-merge.md)
* Správa databází založených na modelu s jedním tenantem nebo více tenanty pomocí [klientské knihovny pro elastické škálování](sql-database-elastic-database-client-library.md)

## <a name="additional-resources"></a>Další zdroje
* [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)
* [Azure Automation](https://azure.microsoft.com/documentation/services/automation/)
* [Azure Scheduler](https://azure.microsoft.com/documentation/services/scheduler/)




<!--HONumber=Jan17_HO1-->


