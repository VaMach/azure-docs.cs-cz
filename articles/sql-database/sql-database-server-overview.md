---
title: "Přehled logického serveru Azure SQL Database | Dokumentace Microsoftu"
description: "Tato stránka obsahuje důležité informace a pokyny pro práci s logickými SQL servery Azure."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: servers
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/28/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: 17e2830dceeaa313dd0fd7d406bf68a75b6f900e


---
# <a name="azure-sql-database-logical-servers"></a>Logické servery Azure SQL Database

Toto téma obsahuje důležité informace a pokyny pro práci s logickými SQL servery Azure. Informace o databázích SQL Azure najdete v tématu [Databáze SQL](sql-database-overview.md).

## <a name="what-is-an-azure-sql-database-logical-server"></a>Co je logický server Azure SQL Database?
Logický server Azure SQL Database funguje jako centrální bod pro správu více databází. Ve službě SQL Database je server logická konstrukce, která se liší od instance SQL Serveru, kterou můžete znát z místního prostředí. Služba SQL Database zejména neposkytuje žádnou záruku ohledně umístění databází ve vztahu k jejich logickým serverům a nezveřejňuje žádné funkce ani možnosti přístupu na úrovni instance. Další informace o logických SQL serverech Azure najdete v tématu [Logické servery](sql-database-server-overview.md). 

Logický server Azure Database:

- Je vytvořen v rámci předplatného Azure, ale je možné ho i s obsaženými prostředky přenést do jiného předplatného.
- Je nadřazeným prostředkem pro databáze, elastické fondy a datové sklady.
- Poskytuje obor názvů pro databáze, elastické fondy a datové sklady.
- Je logický kontejner se silnou sémantikou životního cyklu – odstraníte server a ten odstraní databáze, elastické fondy a datové sklady, které obsahuje.
- Podílí se na řízení přístupu na základě role (RBAC) v Azure – databáze a elastické fondy na serveru dědí přístupová práva od serveru.
- Je nejvyšším prvkem identity databází a elastických fondů pro účely správy prostředků v Azure (viz schéma adresy URL pro databáze a fondy).
- Uspořádává prostředky v oblasti.
- Poskytuje koncový bod připojení pro přístup k databázi (<serverName>.database.windows.net).
- Poskytuje přístup k metadatům, která se vztahují k obsaženým prostředkům, přes zobrazení dynamických zpráv díky připojení k hlavní databázi. 
- Poskytuje obor pro zásady správy, které se vztahují na jeho databáze: přihlašování, brána firewall, auditování, detekce hrozeb atd. 
- Je omezený kvótou nadřazeného předplatného (šest serverů na předplatné – [viz Limity předplatného](../azure-subscription-service-limits.md)).
- Poskytuje obor pro kvóty databáze a DTU pro prostředky, které obsahuje (například 45 000 DTU ve verzi V12).
- Je oborem správy verzí pro možnosti povolené u obsažených prostředků (nejnovější verze je V12).
- Hlavní přihlášení na úrovni serveru můžou spravovat všechny databáze na serveru.
- Může obsahovat přihlašovací údaje podobné těm, která se místně používají v instancích SQL Serveru, a udělit jim přístup k jedné nebo několika databázím na serveru spolu s omezenými právy pro správu. Další informace najdete v tématu [Přihlašovací údaje](sql-database-manage-logins.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-sql-database-logical-server"></a>Jak se připojím k logickému serveru Azure SQL Database a provedu ověření?

- **Ověřování a autorizace**: Azure SQL Database podporuje ověřování pomocí SQL a pomocí Azure Active Directory (s určitými omezeními – informace k ověřování najdete v tématu [Připojení ke službě SQL Database a ověření pomocí Azure Active Directory](sql-database-aad-authentication.md)). K databázím SQL Azure se můžete připojit a provést ověření přes hlavní databázi serveru. Nebo se můžete připojit přímo k uživatelské databázi. Další informace najdete v tématu [Správa databází a přihlašovacích údajů ve službě Azure SQL Database](sql-database-manage-logins.md). Ověřování systému Windows se nepodporuje. 
- **TDS**: Microsoft Azure SQL Database podporuje klienty protokolu TDS (Tabular Data Stream) ve verzi 7.3 nebo novější.
- **TCP/IP**: Jsou povolena pouze připojení přes protokol TCP/IP.
- **Brána firewall služby SQL Database**: Brána firewall služby SQL Database pomáhá chránit vaše data tím, že brání veškerému přístupu k vašemu databázovému serveru a jeho databázím, dokud neurčíte, které počítače k tomu mají oprávnění. Viz téma [Brány firewall](sql-database-firewall-configure.md).

## <a name="what-collations-are-supported"></a>Jaké kolace se podporují?

Výchozí databázová kolace, kterou služba Microsoft Azure SQL Database (včetně hlavní databáze) používá, je **SQL_LATIN1_GENERAL_CP1_CI_AS**, kde **LATIN1_GENERAL** znamená jazyk Angličtina (Spojené státy), **CP1** značí znakovou sadu 1252, **CI** znamená, že se nerozlišují malá a velká písmena, a **AS** značí rozlišování diakritiky. Není doporučeno po vytvoření měnit kolaci databází ve verzi V12. Další informace o kolacích najdete v článku [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

## <a name="what-are-the-naming-requirements-for-database-objects"></a>Jaké jsou požadavky na pojmenování pro databázové objekty?

Názvy všech nových objektů musí splňovat pravidla SQL Serveru pro identifikátory. Další informace najdete v článku [Identifikátory](https://msdn.microsoft.com/library/ms175874.aspx).

## <a name="what-features-are-supported"></a>Jaké funkce jsou podporovány?

Informace o podporovaných funkcích najdete v tématu [Funkce](sql-database-features.md). Přečtěte si také téma [Odlišnosti jazyka Transact-SQL ve službě Azure SQL Database](sql-database-transact-sql-information.md), kde najdete další informace o důvodech nedostatečné podpory určitých typů zařízení.

## <a name="how-do-i-manage-a-logical-server"></a>Jak můžu spravovat logický server?

Logické servery Azure SQL Database můžete spravovat několika způsoby:
- [Azure Portal](sql-database-manage-portal.md)
- [PowerShell](sql-database-manage-powershell.md)
- [REST](/rest/api/sql/)

## <a name="next-steps"></a>Další kroky

- Informace o službě Azure SQL Database najdete v tématu [Co je SQL Database?](sql-database-technical-overview.md)
- Informace o podporovaných funkcích najdete v tématu [Funkce](sql-database-features.md).
- Přehled databází SQL Azure najdete v tématu [Přehled databáze SQL](sql-database-overview.md).
- Informace o podpoře a odlišnostech jazyka Transact-SQL najdete v tématu [Odlišnosti jazyka Transact-SQL ve službě Azure SQL Database](sql-database-transact-sql-information.md).
- Informace o konkrétních kvótách a omezeních prostředků závisí na vaší **úrovni služeb**. Přehled úrovní služeb najdete v tématu [Úrovně služeb SQL Database](sql-database-service-tiers.md).
- Přehled funkcí zabezpečení najdete v tématu [Přehled zabezpečení služby Azure SQL Database](sql-database-security-overview.md).
- Informace o dostupnosti a podpoře ovladačů pro službu SQL Database najdete v tématu [Knihovny pro připojení ke službě SQL Database a serveru SQL Server](sql-database-libraries.md).




<!--HONumber=Dec16_HO4-->


