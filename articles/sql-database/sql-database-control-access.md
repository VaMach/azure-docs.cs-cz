---
title: "Řízení přístupu ke službě Azure SQL Database | Dokumentace Microsoftu"
description: "Informace o řízení přístupu ke službě Microsoft Azure SQL Database."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 8e71b04c-bc38-4153-8f83-f2b14faa31d9
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 10/18/2016
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: f4712d70c0323e607ddcc021809f8097a621730d
ms.openlocfilehash: 82508d01f9f1aaee11ebb5a5f936dfc71ace9cad


---
# <a name="azure-sql-database-access-control"></a>Řízení přístupu ke službě Azure SQL Database
Služba SQL Database zajišťuje zabezpečení řízením přístupu pomocí pravidel brány firewall omezujících připojení podle IP adresy, ověřovacích mechanismů vyžadujících po uživatelích prokázání identity a autorizačních mechanismů omezujících uživatelům přístup ke konkrétním akcím a datům. 

> [!IMPORTANT]
> Přehled funkcí zabezpečení služby SQL Database najdete v [přehledu zabezpečení SQL](sql-database-security-overview.md).
>

## <a name="firewall-and-firewall-rules"></a>Brána firewall a pravidla brány firewall
Microsoft Azure SQL Database poskytuje relační databázovou službu pro aplikace Azure a další internetové aplikace. Brána firewall pomáhá chránit vaše data tím, že brání veškerému přístupu k vašemu databázovému serveru, dokud neurčíte, které počítače k tomu mají oprávnění. Brána firewall uděluje přístup k databázím v závislosti na zdrojové IP adrese každého požadavku. Další informace najdete v tématu [Přehled pravidel brány firewall služby Azure SQL Database](sql-database-firewall-configure.md).

Služba Azure SQL Database je dostupná jenom prostřednictvím portu TCP 1433. Pokud chcete z počítače získat přístup ke službě SQL Database, ověřte, že brána firewall vašeho klientského počítače umožňuje odchozí komunikaci TCP přes port TCP 1433. Pokud ho jiné aplikace nepotřebují, zablokujte na portu TCP 1433 příchozí připojení. 

V rámci připojovacího procesu budou připojení virtuálních počítačů Azure přesměrována na jinou IP adresu a port, které jsou pro každou roli pracovního procesu jedinečné. Číslo portu je v rozsahu 11000 až 11999. Další informace o portech TCP najdete v tématu [Porty s větším číslem než 1433 pro ADO.NET 4.5 a službu SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="authentication"></a>Ověřování

SQL Database podporuje dva typy ověřování:

* **Ověřování SQL,** které používá uživatelské jméno a heslo. Když jste vytvářeli logický server databáze, zadali jste uživatelské jméno a heslo účtu „server admin“. Tyto přihlašovací údaje můžete použít k ověření a přihlášení k libovolné databázi na daném serveru jako vlastník databáze neboli „dbo“. 
* **Ověřování pomocí Azure Active Directory,** které používá identity spravované v Azure Active Directory a je podporované u spravovaných a integrovaných domén. [Kdykoliv to půjde](https://msdn.microsoft.com/library/ms144284.aspx), použijte ověřování pomocí Active Directory (integrované zabezpečení). Pokud chcete k ověřování použít Azure Active Directory, musíte vytvořit jiného správce serveru, který se jmenuje „Azure AD admin“ a který smí spravovat uživatele a skupiny služby Azure AD. Tento správce také smí provádět všechny operace jako běžný správce serveru. Postup vytvoření účtu správce služby Azure AD, který umožňuje ověřování Azure Active Directory, najdete v tématu [Připojení ke službě SQL Database s ověřením přes Azure Active Directory](sql-database-aad-authentication.md).

Databázový stroj zavře připojení, které jsou neaktivní déle než 30 minut. Připojení se musí znovu přihlásit, aby bylo možné ho použít. Trvale aktivní připojení ke službě SQL Database vyžadují opakovanou autorizaci (prováděnou databázovým strojem) nejméně každých 10 hodin. Databázový stroj se pokusí při opakované autorizaci použít původně zadané heslo, aby nebyl nutný zásah uživatele. Při resetování hesla ve službě SQL Database se kvůli zachování výkonu připojení znovu neověřuje, i když je důvodem resetování sdružování připojení. Toto chování se liší od místního SQL Serveru. Pokud se heslo od původního ověření připojení změnilo, musí se připojení ukončit a vytvořit nové připojení pomocí nového hesla. Uživatel s oprávněním KILL DATABASE CONNECTION může připojení ke službě SQL Database výslovně ukončit příkazem [KILL](https://msdn.microsoft.com/library/ms173730.aspx).

Uživatelské účty je možné vytvořit v hlavní databázi a udělit jim oprávnění ve všech databázích na serveru, nebo je lze vytvořit v samotné databázi (takové účty se nazývají uživatelé s omezením). Informace o vytváření a správě přihlašování najdete v tématu [Správa přihlašování](sql-database-manage-logins.md). Pokud chcete zlepšit přenositelnost a škálovatelnost, používejte uživatele databáze s omezením. Další informace o uživatelích s omezením najdete v tématech [Uživatelé databáze s omezením – vytvoření přenosné databáze](https://msdn.microsoft.com/library/ff929188.aspx), [CREATE USER (Transact-SQL)](https://technet.microsoft.com/library/ms173463.aspx) a [Databáze s omezením](https://technet.microsoft.com/library/ff929071.aspx).

Doporučený postup je, aby aplikace k ověření používala jiný účet – omezíte tak oprávnění udělené aplikaci a snížíte riziko škodlivých aktivit v případě ohrožení kódu aplikace útoky v podobě injektáže SQL. Doporučený postup je vytvořit [uživatele databáze s omezením](https://msdn.microsoft.com/library/ff929188), který umožňuje ověřit aplikaci přímo v databázi. Pokud chcete vytvořit uživatele databáze s omezením, který používá ověřování SQL, připojte se k uživatelské databázi jako správce serveru a spusťte následující příkaz T-SQL:

## <a name="authorization"></a>Autorizace

Autorizace určuje, co může uživatel provádět ve službě Azure SQL Database. Tyto možnosti jsou dané [členstvím v databázových rolích](https://msdn.microsoft.com/library/ms189121) a [oprávněními k databázi na úrovni objektů](https://msdn.microsoft.com/library/ms191291.aspx) vašeho účtu. Doporučený postup je udělit uživatelům co nejmenší možná oprávnění. Účet správce serveru, který používáte k připojení, je členem skupiny db_owner. Tato skupina může s databází provádět všechny operace. Tento účet uložte kvůli nasazení upgradovaných schémat a dalším možnostem správy. Použijte účet „ApplicationUser“, který má omezenější oprávnění a umožňuje připojit se z aplikace k databázi s nejnižšími oprávněními, jaké aplikace potřebuje. Další informace najdete v tématu [Správa přihlašování](sql-database-manage-logins.md).

K hlavní databázi většinou potřebují mít přístup jenom správci. Pro rutinní přístup ke každé uživatelské databázi můžete použít databázové uživatele s omezením, kteří nejsou správci, ale jsou v každé databázi vytvoření. Když použijete tyto databázové uživatele s omezením, nepotřebujete v hlavní databázi vytvářet přihlášení. Další informace najdete v tématu [Uživatelé databáze s omezením – zajištění přenositelnosti databáze](https://msdn.microsoft.com/library/ff929188.aspx).

Kromě toho můžete tyto funkce použít k omezení nebo zvýšení oprávnění.

* K dočasnému bezpečnému zvýšení oprávnění můžete použít [zosobnění](https://msdn.microsoft.com/library/vstudio/bb669087) a [přihlašování k modulům](https://msdn.microsoft.com/library/bb669102).
* K omezení řádků, ke kterým má uživatel přístup, můžete použít [zabezpečení na úrovni řádku](https://msdn.microsoft.com/library/dn765131).
* K omezení rizika ohrožení citlivých dat můžete použít [maskování dat](sql-database-dynamic-data-masking-get-started.md).
* K omezení akcí, které je možné s databází provádět, můžete použít [uložené procedury](https://msdn.microsoft.com/library/ms190782).

## <a name="next-steps"></a>Další kroky

- Přehled všech funkcí zabezpečení služby SQL Database najdete v [přehledu zabezpečení SQL](sql-database-security-overview.md).
- Další informace o pravidlech brány firewall najdete v tématu [Brána firewall služby Azure SQL Database](sql-database-firewall-configure.md).
- Informace o uživateli a přihlašování najdete v tématu [Správa přihlašování](sql-database-manage-logins.md). 
- Diskuzi o používání funkcí ochrany dat ve službě SQL Database najdete v tématu [Ochrana a zabezpečení dat](sql-database-protect-data.md).
- Diskuzi o proaktivním monitorování najdete v tématech [Začínáme s auditováním služby SQL Database](sql-database-auditing-get-started.md) a [Začínáme s detekcí hrozeb služby SQL Database](sql-database-threat-detection-get-started.md).




<!--HONumber=Dec16_HO4-->


