---
title: "Přehled databází SQL Azure | Dokumentace Microsoftu"
description: "Tato stránka obsahuje přehled databází SQL Azure."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: single databases
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/28/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 145cdc5b686692b44d2c3593a128689a56812610
ms.openlocfilehash: 6d5f4640556f98f9601139c318ffc578e9df2539


---
# <a name="azure-sql-database-overview"></a>Přehled databází SQL Azure
Toto téma obsahuje přehled databází SQL Azure. Informace o logických SQL serverech Azure najdete v tématu [Logické servery](sql-database-server-overview.md).

## <a name="what-is-azure-sql-database"></a>Co je databáze SQL Azure?
Každá databáze ve službě Azure SQL Database je přidružená k logickému serveru. Databáze může být:

- Samostatná databáze s [vlastní sadou prostředků](sql-database-what-is-a-dtu.md#what-are-database-transaction-units-dtus) (DTU)
- Součástí [elastického fondu](sql-database-elastic-pool.md), který [sdílí sadu prostředků](sql-database-what-is-a-dtu.md#what-are-elastic-database-transaction-units-edtus) (eDTU)
- Součástí [sady horizontálně dělených databází s horizontálním navýšením kapacity](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling); tyto databáze můžou být buď izolované, nebo součástí fondu
- Součástí sady databází, které se podílejí na [vzoru návrhu SaaS pro více klientů](sql-database-design-patterns-multi-tenancy-saas-applications.md); tyto databáze můžou být buď izolované, nebo součástí fondu (nebo obojí) 

## <a name="how-do-i-connect-and-authenticate-to-an-azure-sql-database"></a>Jak se připojím k databázi SQL Azure a provedu ověření?

- **Ověřování a autorizace**: Azure SQL Database podporuje ověřování pomocí SQL a pomocí Azure Active Directory (s určitými omezeními – informace k ověřování najdete v tématu [Připojení ke službě SQL Database a ověření pomocí Azure Active Directory](sql-database-aad-authentication.md)). K databázím SQL Azure se můžete připojit a provést ověření přes hlavní databázi serveru. Nebo se můžete připojit přímo k uživatelské databázi. Další informace najdete v tématu [Správa databází a přihlašovacích údajů ve službě Azure SQL Database](sql-database-manage-logins.md). Ověřování systému Windows se nepodporuje. 
- **TDS**: Microsoft Azure SQL Database podporuje klienty protokolu TDS (Tabular Data Stream) ve verzi 7.3 nebo novější.
- **TCP/IP**: Jsou povolena pouze připojení přes protokol TCP/IP.
- **Brána firewall služby SQL Database**: Brána firewall služby SQL Database pomáhá chránit vaše data tím, že brání veškerému přístupu k vašemu databázovému serveru a jeho databázím, dokud neurčíte, které počítače k tomu mají oprávnění. Viz téma [Brány firewall](sql-database-firewall-configure.md).

## <a name="what-collations-are-supported"></a>Jaké kolace se podporují?
Výchozí databázová kolace, kterou služba Microsoft Azure SQL Database používá, je **SQL_LATIN1_GENERAL_CP1_CI_AS**, kde **LATIN1_GENERAL** znamená jazyk Angličtina (Spojené státy), **CP1** značí znakovou sadu 1252, **CI** znamená, že se nerozlišují malá a velká písmena, a **AS** značí rozlišování diakritiky. Pro databáze V12 není možné kolaci změnit. Další informace o nastavení kolace najdete v článku [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="what-are-the-naming-requirements-for-database-objects"></a>Jaké jsou požadavky na pojmenování pro databázové objekty?

Názvy všech nových objektů musí splňovat pravidla SQL Serveru pro identifikátory. Další informace najdete v článku [Identifikátory](https://msdn.microsoft.com/library/ms175874.aspx).

## <a name="what-features-are-supported-by-azure-sql-databases"></a>Jaké funkce databáze SQL Azure podporují?

Informace o podporovaných funkcích najdete v tématu [Funkce](sql-database-features.md). Přečtěte si také téma [Odlišnosti jazyka Transact-SQL ve službě Azure SQL Database](sql-database-transact-sql-information.md), kde najdete další informace o důvodech nedostatečné podpory určitých typů zařízení.

## <a name="how-do-i-manage-an-azure-sql-database"></a>Jak můžu spravovat databázi SQL Azure?

Logické servery Azure SQL Database můžete spravovat několika způsoby:
- [Azure Portal](sql-database-manage-portal.md)
- [PowerShell](sql-database-manage-powershell.md)
- [Transact-SQL](sql-database-manage-azure-ssms.md)
- [REST](/rest/api/sql/)

## <a name="next-steps"></a>Další kroky

- Informace o službě Azure SQL Database najdete v tématu [Co je SQL Database?](sql-database-technical-overview.md)
- Informace o podporovaných funkcích najdete v tématu [Funkce](sql-database-features.md).
- Přehled logických SQL serverů Azure najdete v tématu [Přehled logického serveru SQL Database](sql-database-server-overview.md).
- Informace o podpoře a odlišnostech jazyka Transact-SQL najdete v tématu [Odlišnosti jazyka Transact-SQL ve službě Azure SQL Database](sql-database-transact-sql-information.md).
- Informace o konkrétních kvótách a omezeních prostředků závisí na vaší **úrovni služeb**. Přehled úrovní služeb najdete v tématu [Úrovně služeb SQL Database](sql-database-service-tiers.md).
- Pokyny pro zabezpečení najdete v tématu [Omezení a pokyny pro zabezpečení služby Azure SQL Database](sql-database-security-guidelines.md).
- Informace o dostupnosti a podpoře ovladačů pro službu SQL Database najdete v tématu [Knihovny pro připojení ke službě SQL Database a serveru SQL Server](sql-database-libraries.md).




<!--HONumber=Dec16_HO2-->


