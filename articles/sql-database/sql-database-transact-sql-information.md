---
title: "Nepodporované příkazy jazyka T-SQL v Azure SQL Database | Dokumentace Microsoftu"
description: "Příkazy jazyka Transact-SQL, které služba Azure SQL Database plně nepodporuje"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: c05abd9e-28a7-4c97-9bdf-bc60d08fc92e
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 11/28/2016
ms.author: rick.byham@microsoft.com
translationtype: Human Translation
ms.sourcegitcommit: 7db5939c88045ab59d7d9fb4b58f07374d7bca01
ms.openlocfilehash: ebec6ca87cfd5e9d2c4ea726d5d31d20998ef33f


---
# <a name="azure-sql-database-transact-sql-differences"></a>Rozdíly u Transact-SQL ve službě Azure SQL Database   
Většinu funkcí jazyka Transact-SQL, na kterých závisejí aplikace, podporují jak Microsoft SQL Server, tak Azure SQL Database. Například hlavní součásti SQL, jako jsou datové typy, operátory, řetězce, aritmetické, logické a kurzorové funkce atd., fungují stejně jako v SQL Serveru.

## <a name="why-some-transact-sql-is-not-supported"></a>Proč nejsou některé příkazy jazyka Transact-SQL podporované
Azure SQL Database je navržená tak, aby izolovala funkce od závislostí na hlavní databázi a operačním systému. Proto je řada serverových aktivit pro službu SQL Database nevhodná. Příkazy jazyka Transact-SQL většinou nejsou k dispozici, pokud konfigurují možnosti serveru, součásti operačního systému nebo určují konfiguraci systému souborů. Pokud potřebujete funkce, které se nacházejí mimo uživatelskou databázi, nabízí SQL Database často odpovídající alternativu. Můžete také použít jinou funkci nebo službu Azure. 

Například skupina dostupnosti Always On je nahrazena aktivní geografickou replikací. Z uvedených důvodů nejsou podporované příkazy Transact-SQL, které se týkají skupin dostupnosti, ani zobrazení dynamické správy týkající se skupiny dostupnosti Always On.  

Seznam podporovaných a nepodporovaných funkcí služby SQL Database najdete v tématu [Požadavky, pokyny a funkce služby Azure SQL Database](sql-database-features.md).

Obecně platí, že SQL Database nepodporuje zastaralou syntaxi SQL Serveru.

## <a name="transact-sql-syntax-partially-supported-in-sql-database"></a>SQL Database podporuje částečně syntaxi jazyka Transact-SQL.
SQL Database podporuje některé (ale ne všechny) argumenty, které existují v odpovídajících příkazech jazyka Transact-SQL v SQL Serveru 2016. Například příkaz `CREATE PROCEDURE` je dostupný, ale všechny možnosti příkazu `CREATE PROCEDURE` dostupné nejsou. Popis celé syntaxe by byl nepřehledný a zbytečný. Podrobnosti o podporovaných částech každého příkazu najdete v tématech o syntaxi, na které zde odkazujeme.

- Databáze: [CREATE](https://msdn.microsoft.com/library/dn268335.aspx)/[ALTER DATABASE](https://msdn.microsoft.com/library/ms174269.aspx)   
- Funkce: [CREATE](https://msdn.microsoft.com/library/ms186755.aspx)/[ALTER FUNCTION](https://msdn.microsoft.com/library/ms186967.aspx)   
- Přihlášení: [CREATE](https://msdn.microsoft.com/library/ms189751.aspx)/[ALTER LOGIN](https://msdn.microsoft.com/library/ms189828.aspx)   
- Uložené procedury: [CREATE](https://msdn.microsoft.com/library/ms187926.aspx)/[ALTER PROCEDURE](https://msdn.microsoft.com/library/ms189762.aspx)   
- Tabulky: [CREATE](https://msdn.microsoft.com/library/dn305849.aspx)/[ALTER TABLE](https://msdn.microsoft.com/library/ms190273.aspx)   
- Typy (vlastní): [CREATE TYPE](https://msdn.microsoft.com/library/ms175007.aspx)   
- Uživatelé: [CREATE](https://msdn.microsoft.com/library/ms173463.aspx)/[ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx)   
- Zobrazení: [CREATE](https://msdn.microsoft.com/library/ms187956.aspx)/[ALTER VIEW](https://msdn.microsoft.com/library/ms173846.aspx)   

## <a name="transact-sql-syntax-not-supported-in-sql-database"></a>Syntaxe jazyka Transact-SQL nepodporovaná službou SQL Database   
Kromě příkazů jazyka Transact-SQL souvisejících s nepodporovanými funkcemi popsanými v tématu [Požadavky, pokyny a funkce služby Azure SQL Database](sql-database-features.md) nejsou podporované ani následující příkazy a jejich skupiny:
- Kolace systémových objektů
- Související s připojením: příkazy ENDPOINT, `ORIGINAL_DB_NAME`. Služba SQL Database nepodporuje ověřování Windows, ale podporuje podobné ověřování Azure Active Directory. Některé typy ověřování vyžadují nejnovější verzi SQL Server Management Studia (SSMS). Další informace najdete v tématu [Připojení k SQL Database nebo SQL Data Warehouse pomocí ověřování služby Azure Active Directory](sql-database-aad-authentication.md).
- Mezidatabázové dotazy, které používají tři nebo čtyři názvy částí (mezidatabázové dotazy jen pro čtení jsou podporované prostřednictvím [dotazů do Elastic Database](sql-database-elastic-query-overview.md)).
- Mezidatabázové řetězení vlastnictví, nastavení `TRUSTWORTHY`
- `DATABASEPROPERTY` Místo toho použijte `DATABASEPROPERTYEX`.
- `EXECUTE AS LOGIN` Místo toho použijte EXECUTE AS USER.
- Šifrování je podporované s výjimkou služby EKM (extensible key management).
- Stavy: událostí, oznámení událostí a oznámení dotazů
- Syntaxe spojená s umístěním databázových souborů, jejich velikostí a databázovými soubory, které automaticky spravuje Microsoft Azure.
- Syntaxe týkající se vysoké dostupnosti, která je spravovaná prostřednictvím účtu Microsoft Azure. Patří sem syntaxe zálohování, obnovení, Always On, zrcadlení databáze, přesouvání protokolu a režimů obnovení.
- Syntaxe, která závisí na čtečce protokolů, protože tato čtečka není v SQL Database k dispozici: nabízená replikace a Change Data Capture. SQL Database může být odběratelem článku nabízené replikace.
- Syntaxe, která závisí na službě SQL Server Agent nebo na databázi MSDB: výstrahy, operátory a servery centrální správy. Použijte raději skriptování, například v Azure PowerShellu.
- Funkce: `fn_get_sql`, `fn_virtualfilestats`, `fn_virtualservernodes`
- Globální dočasné tabulky
- Syntaxe spojená s nastavením serverového hardwaru: paměť, pracovní vlákna, spřažení procesorů, příznaků trasování atd. Místo ní použijte úrovně služby.
- `HAS_DBACCESS`
- `KILL STATS JOB`
- `OPENQUERY`, `OPENROWSET`, `OPENDATASOURCE`, `BULK INSERT` a názvy složené ze čtyř částí
- .NET Framework – [integrace modulu CLR do SQL Serveru](http://msdn.microsoft.com/library/ms254963.aspx)
- Sémantické vyhledávání
- Přihlašovací údaje serveru. Místo nich použijte přihlašovací údaje v oboru databáze.
- Serverové položky: serverové role, `IS_SRVROLEMEMBER`, `sys.login_token`. K dispozici nejsou příkazy `GRANT`, `REVOKE` a `DENY` serverových oprávnění, i když některé z nich jsou nahrazené oprávněními na úrovni databáze. Některá praktická serverová zobrazení dynamických zpráv (DMV) mají odpovídající databázová zobrazení dynamických zpráv.
- `SET REMOTE_PROC_TRANSACTIONS`
- `SHUTDOWN`
- `sp_addmessage`
- Možnosti `sp_configure` a `RECONFIGURE`. Některé možnosti jsou dostupné prostřednictvím příkazu [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).
- `sp_helpuser`
- `sp_migrate_user_to_contained`
- Auditování SQL Serveru. Místo něho použijte auditování SQL Database.
- Trasování SQL Serveru
- Příznaky trasování. Některé příznaky trasování byly přesunuty do kompatibilních režimů.
- Ladění Transact-SQL
- Aktivační události: serverové nebo přihlašovací aktivační události
- Příkaz `USE`: Pokud chcete změnit kontext databáze na jinou databázi, musíte k nové databázi vytvořit nové připojení.

## <a name="full-transact-sql-reference"></a>Kompletní reference k jazyku Transact-SQL
Další informace o syntaxi a používání jazyka Transact-SQL, včetně příkladů, najdete v tématu [Reference k jazyku Transact-SQL (databázový stroj)](https://msdn.microsoft.com/library/bb510741.aspx) v dokumentaci SQL Server Books Online. 

### <a name="about-the-applies-to-tags"></a>Informace o značkách „Platí pro“
Referenční zdroje k jazyku Transact-SQL obsahují témata, která se týkají SQL Serveru verze 2008 až po současnost. Pod názvem tématu je pruh ikon se čtyřmi platformami SQL Serveru a vyznačenou platností. Například skupiny dostupnosti byly zavedeny v SQL Serveru 2012. Téma [CREATE AVAILABILTY GROUP](https://msdn.microsoft.com/library/ff878399.aspx) uvádí, že příkaz platí pro **SQL Server (počínaje verzí 2012)**. Příkaz neplatí pro SQL Server 2008, SQL Server 2008 R2, Azure SQL Database, Azure SQL Data Warehouse ani pro Parallel Data Warehouse.

Někdy je možné v produktu použít obecný předmět tématu, i když jsou mezi jednotlivým produkty drobné rozdíly. Tam, kde je to možné, jsou rozdíly v tématu uvedené u středových bodů.



<!--HONumber=Jan17_HO1-->


