---
title: "Ověřování SQL: Přístup, ověřování, brány firewall služby Azure SQL Database | Dokumentace Microsoftu"
description: "V tomto úvodním kurzu se naučíte, jak pomocí sady SQL Server Management Studio a Transact-SQL pracovat s pravidly brány firewall na úrovni serveru a databáze, ověřováním pomocí SQL, přihlašováním, uživateli a rolemi a jak pro databáze a servery Azure SQL Database přidělovat přístup a řízení."
keywords: 
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 67797b09-f5c3-4ec2-8494-fe18883edf7f
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: c70b3b23fa95af6614c34bd951943f0559409220
ms.openlocfilehash: cf43790c329ef156ae17579d2281c861533ec201


---
# <a name="sql-server-authentication-access-and-database-level-firewall-rules"></a>Ověřování SQL Serveru, přístup a pravidla brány firewall na úrovni databáze

V tomto kurzu se naučíte, jak pomocí sady SQL Server Management Studio pracovat s ověřováním pomocí SQL Serveru, přihlašováním, uživateli a databázovými rolemi, které udělují přístup a oprávnění pro databáze a servery Azure SQL Database. Po dokončení tohoto kurzu budete umět:

- Vytváření přihlašovacích údajů a uživatelů na základě ověřování pomocí SQL Serveru
- Přidání uživatelů k rolím a udělení oprávnění rolím
- Použití jazyka T-SQL k vytvoření pravidla brány firewall na úrovni serveru a databáze 
- Připojení uživatele ke konkrétní databázi pomocí SSMS
- Zobrazení uživatelských oprávnění v hlavní databázi a uživatelských databázích

**Časový odhad:** Dokončení tohoto kurzu trvá přibližně 45 minut (za předpokladu, že už máte splněné požadavky).

> [!NOTE]
> Tento kurz vám pomůže seznámit se s obsahem těchto okruhů: [Řízení a přístup k SQL Database](sql-database-control-access.md), [Přihlašování, uživatelé a databázové role](sql-database-manage-logins.md), [Objekty zabezpečení](https://msdn.microsoft.com/library/ms181127.aspx), [Databázové role](https://msdn.microsoft.com/library/ms189121.aspx) a [Pravidla brány firewall pro SQL Database](sql-database-firewall-configure.md). Kurz věnovaný ověřování pomocí Azure Active Directory najdete v tématu [Začínáme s ověřováním Azure AD](sql-database-control-access-aad-authentication-get-started.md).
>  

## <a name="prerequisites"></a>Požadavky

* **Účet Azure:** Potřebujete mít účet Azure. Můžete si [zdarma otevřít účet Azure](https://azure.microsoft.com/free/) nebo [aktivovat výhody pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/). 

* **Oprávnění pro vytváření v Azure:** Musíte být schopni připojit se k webu Azure Portal pomocí účtu, který je členem role přispěvatele nebo vlastníka předplatného. Další informace o řízení přístupu na základě role (RBAC) najdete v tématu [Začínáme se správou přístupu na webu Azure Portal](../active-directory/role-based-access-control-what-is.md).

* **SQL Server Management Studio:** Nejnovější verzi aplikace SQL Server Management Studio (SMSS) si můžete stáhnout a nainstalovat v části [Stažení aplikace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx). Při připojení ke službě Azure SQL Database vždycky používejte nejnovější verzi SSMS, protože se neustále vydávají nové funkce.

* **Dokončený základní kurz:** Dokončili jste kurz [Začínáme se servery, databázemi a pravidly brány firewall služby Azure SQL Database s využitím webu Azure Portal a aplikace SQL Server Management Studio](sql-database-get-started.md) nebo ekvivalentní [verzi tohoto kurzu pro prostředí PowerShell](sql-database-get-started-powershell.md). Pokud ne, dokončete tento požadovaný kurz nebo spusťte skript prostředí PowerShell na konci [verze tohoto kurzu pro prostředí PowerShell](sql-database-get-started-powershell.md) a teprve potom pokračujte.



## <a name="sign-in-to-the-azure-portal-using-your-azure-account"></a>Přihlášení k webu Azure Portal pomocí účtu Azure
Kroky v tomto postupu ukazují, jak se připojit k webu Azure Portal pomocí účtu Azure (https://account.windowsazure.com/Home/Index).

1. Otevřete prohlížeč, kterému dáváte přednost, a připojte se k [portálu Azure](https://portal.azure.com/).
2. Přihlaste se k webu [Portál Azure](https://portal.azure.com/).
3. Na **přihlašovací stránce** zadejte přihlašovací údaje ke svému předplatnému.
   
   ![Přihlášení](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="view-logical-server-security-information-in-the-azure-portal"></a>Zobrazení informací o zabezpečení logického serveru na webu Azure Portal

Kroky v tomto postupu vám ukážou, jak na webu Azure Portal zobrazit informace o konfiguraci zabezpečení logického serveru.

1. Otevřete okno **SQL Server** pro váš server a prohlédněte si informace na stránce **Přehled**.

   ![Účet správce serveru na webu Azure Portal](./media/sql-database-control-access-sql-authentication-get-started/sql_admin_portal.png)

2. Poznamenejte si jméno správce logického serveru. 

3. Pokud si nepamatujete heslo, kliknutím na tlačítko **resetovat heslo** nastavte nové heslo.

4. Pokud pro tento server potřebujete získat informace o připojení, klikněte na **Vlastnosti**.

## <a name="view-server-admin-permissions-using-ssms"></a>Zobrazit oprávnění správce serveru pomocí SSMS

Kroky v tomto postupu vám ukážou, jak zobrazit informace o účtu správce serveru a jeho oprávněních v hlavní databázi a uživatelských databázích.

1. Otevřete SQL Server Management Studio a připojte k serveru jako správce pomocí ověřování systému SQL Server a účtu správce serveru.

   ![Připojení k serveru](./media/sql-database-get-started/connect-to-server.png)

2. Klikněte na **Připojit**.

   ![Připojeno k serveru](./media/sql-database-get-started/connected-to-server.png)

3. V Průzkumníku objektů rozbalte **Zabezpečení**a potom rozbalte **Přihlášení**. Zobrazí se existující přihlášení pro váš server. Jediným přihlášením pro nový server je přihlášení pro účet správce serveru.

   ![Přihlašovací jméno správce serveru](./media/sql-database-control-access-sql-authentication-get-started/server_admin_login.png)

4. V Průzkumníku objektů postupně rozbalte **Databáze**, **Systémové databáze**, **Hlavní**, **Zabezpečení**a **Uživatelé**. Zobrazí se uživatelský účet vytvořený pro přihlášení správce serveru v této databázi.

   ![uživatelský účet hlavní databáze pro správce serveru](./media/sql-database-control-access-sql-authentication-get-started/master_database_user_account_for_server_admin.png)

   > [!NOTE]
   > Informace o ostatních zobrazených uživatelských účtech, které se zobrazují v uzlu Uživatelé, najdete v tématu [Objekty zabezpečení](https://msdn.microsoft.com/library/ms181127.aspx).
   >

5. V Průzkumníku objektů klikněte pravým tlačítkem na **hlavní** a potom klikněte na **Nový dotaz**. Otevře se okno dotazu připojené k hlavní databázi.
6. V okně dotazu spusťte následující dotaz, který vrací informace o uživateli spouštějícím tento dotaz. 

   ```
   SELECT USER;
   ```

   ![Výběr dotazu uživatele v hlavní databázi](./media/sql-database-control-access-sql-authentication-get-started/select_user_query_in_master_database.png)

7. V okně dotazu spusťte následující dotaz, který vrací informace o oprávnění uživatele **sqladmin** v hlavní databázi. 

   ```
   SELECT prm.permission_name
      , prm.class_desc
      , prm.state_desc
      , p2.name as 'Database role'
      , p3.name as 'Additional database role' 
   FROM sys.database_principals p
   JOIN sys.database_permissions prm
      ON p.principal_id = prm.grantee_principal_id
      LEFT JOIN sys.database_principals p2
      ON prm.major_id = p2.principal_id
      LEFT JOIN sys.database_role_members r
      ON p.principal_id = r.member_principal_id
      LEFT JOIN sys.database_principals p3
      ON r.role_principal_id = p3.principal_id
   WHERE p.name = 'sqladmin';
   ```

   ![oprávnění správce serveru v hlavní databázi](./media/sql-database-control-access-sql-authentication-get-started/server_admin_permissions_in_master_database.png)

   >[!NOTE]
   > Uživatel sqladmin má oprávnění pro připojení k hlavní databázi, vytvoření přihlášení a uživatelů, získávání informací z tabulky sys.sql_logins a přidání uživatelů do databázových rolí dbmanager a dbcreator. Tato oprávnění jsou nad rámec oprávnění udělovaných veřejné roli, ze které dědí oprávnění všichni uživatelé (třeba oprávnění pro výběr informací z konkrétních tabulek). Další informace najdete v tématu [Oprávnění](https://msdn.microsoft.com/library/ms191291.aspx).
   >

8. V Průzkumníku objektů postupně rozbalte **blankdb**, **Zabezpečení** a **Uživatelé**. Zobrazí se uživatelský účet vytvořený pro přihlášení správce serveru v této databázi (a v každé uživatelské databázi).

   ![Uživatelské účty v blankdb](./media/sql-database-control-access-sql-authentication-get-started/user_accounts_in_blankdb.png)

9. V Průzkumníku objektů klikněte pravým tlačítkem na **blankdb** a potom klikněte na **Nový dotaz**.

10. V okně dotazu spusťte následující dotaz, který vrací informace o uživateli spouštějícím tento dotaz.

   ```
   SELECT USER;
   ```

   ![Výběr dotazu uživatele v databázi blankdb](./media/sql-database-control-access-sql-authentication-get-started/select_user_query_in_blankdb_database.png)

11. V okně dotazu spusťte následující dotaz, který vrací informace o oprávnění uživatele dbo. 

   ```
   SELECT prm.permission_name
      , prm.class_desc
      , prm.state_desc
      , p2.name as 'Database role'
      , p3.name as 'Additional database role' 
   FROM sys.database_principals AS p
   JOIN sys.database_permissions AS prm
      ON p.principal_id = prm.grantee_principal_id
      LEFT JOIN sys.database_principals AS p2
      ON prm.major_id = p2.principal_id
      LEFT JOIN sys.database_role_members r
      ON p.principal_id = r.member_principal_id
      LEFT JOIN sys.database_principals AS p3
      ON r.role_principal_id = p3.principal_id
   WHERE p.name = 'dbo';
   ```

   ![Oprávnění správce serveru v databázi blankdb](./media/sql-database-control-access-sql-authentication-get-started/server_admin_permissions_in_blankdb_database.png)

   > [!NOTE]
   > Uživatel dbo je členem veřejné role a také pevné databázové role db_owner. Další informace najdete v tématu [Role na úrovni databáze](https://msdn.microsoft.com/library/ms189121.aspx).
   >

## <a name="create-a-new-user-with-select-permissions"></a>Vytvoření nového uživatele s oprávněními SELECT

Kroky v tomto postupu ukazují, jak vytvořit uživatele na úrovni databáze, otestovat výchozí oprávnění nového uživatele (prostřednictvím veřejné role), udělit uživateli oprávnění **SELECT**oprávnění a zobrazit tato změněná oprávnění.

> [!NOTE]
> Uživatelé na úrovni databáze se také označují jako [uživatelé s omezením](https://msdn.microsoft.com/library/ff929188.aspx) a zvyšují přenositelnost vaší databáze. Informace o výhodách přenositelnosti najdete v tématu věnovaném [konfiguraci a správě zabezpečení Azure SQL Database pro geografické obnovení nebo převzetí služeb při selhání na sekundární server](sql-database-geo-replication-security-config.md).
>

1. V Průzkumníku objektů klikněte pravým tlačítkem na **sqldbtutorialdb** a potom klikněte na **Nový dotaz**.
2. V tomto okně dotazu spusťte následující příkaz, který v databázi sqldbtutorialdb vytvoří uživatelský účet s názvem **user1**.

   ```
   CREATE USER user1
   WITH PASSWORD = 'p@ssw0rd';
   ```
   ![nový uživatel user1 sqldbtutorialdb](./media/sql-database-control-access-sql-authentication-get-started/new_user_user1_aw.png)

3. V okně dotazu spusťte následující dotaz, který vrací informace o oprávnění uživatele user1.

   ```
   SELECT prm.permission_name
      , prm.class_desc
      , prm.state_desc
      , p2.name as 'Database role'
      , p3.name as 'Additional database role' 
   FROM sys.database_principals AS p
   JOIN sys.database_permissions AS prm
      ON p.principal_id = prm.grantee_principal_id
      LEFT JOIN sys.database_principals AS p2
      ON prm.major_id = p2.principal_id
      LEFT JOIN sys.database_role_members r
      ON p.principal_id = r.member_principal_id
      LEFT JOIN sys.database_principals AS p3
      ON r.role_principal_id = p3.principal_id
   WHERE p.name = 'user1';
   ```

   ![Nová uživatelská oprávnění v uživatelské databázi](./media/sql-database-control-access-sql-authentication-get-started/new_user_permissions_in_user_database.png)

   > [!NOTE]
   > Nový uživatel v databázi má jenom oprávnění zděděná z veřejné role.
   >

4. Pomocí příkazu **EXECUTE AS USER** spusťte následující dotazy a pokuste se dotazovat na tabulku SalesLT.ProductCategory v databázi sqldbtutorialdb jako **user1** jenom s oprávněními zděděnými z veřejné role.

   ```
   EXECUTE AS USER = 'user1';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![Bez oprávnění SELECT](./media/sql-database-control-access-sql-authentication-get-started/no_select_permissions.png)

   > [!NOTE]
   > Ve výchozím nastavení veřejné role neudělují oprávnění **SELECT** pro uživatelské objekty.
   >

5. Spusťte následující příkaz, kterým uživateli **user1** udělíte oprávnění **SELECT** pro tabulku SalesLT.ProductCategory.

   ```
   GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to user1;
   ```

   ![Udělení oprávnění SELECT](./media/sql-database-control-access-sql-authentication-get-started/grant_select_permissions.png)

6. Spuštěním následujících dotazů se úspěšně dotazujte na tabulku SalesLT.ProductCategory v databázi sqldbtutorialdb jako **user1**.

   ```
   EXECUTE AS USER = 'user1';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![Oprávnění SELECT](./media/sql-database-control-access-sql-authentication-get-started/select_permissions.png)

## <a name="create-a-database-level-firewall-rule-using-t-sql"></a>Vytvoření pravidla brány firewall na úrovni databáze s použitím SQL

Kroky v tomto postupu ukazují, jak vytvořit pravidlo brány firewall na úrovni databáze pomocí systémové uložené procedury [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx). Pravidlo brány firewall na úrovni databáze umožňuje správci serveru, aby prostřednictvím brány firewall Azure SQL Database povolil uživatelům přístup jenom ke konkrétním databázím.

> [!NOTE]
> [Pravidla brány firewall na úrovni databáze](sql-database-firewall-configure.md) zvyšují přenositelnost vaší databáze. Informace o výhodách přenositelnosti najdete v tématu věnovaném [konfiguraci a správě zabezpečení Azure SQL Database pro geografické obnovení nebo převzetí služeb při selhání na sekundární server](sql-database-geo-replication-security-config.md).
>

> [!IMPORTANT]
> K otestování pravidel brány firewall na úrovni databáze se připojte z jiného počítače (nebo odstraňte pravidlo brány firewall na úrovni serveru na webu Azure Portal).
>

1. V počítači, pro který ještě nemáte pravidlo brány firewall na úrovni serveru, otevřete SQL Server Management Studio.

2. V okně **Připojit k serveru** zadejte název serveru a ověřovací informace pro připojení pomocí ověřování SQL Serveru s účtem **user1**. 
    
   ![Připojení jako user1 bez pravidla brány firewall&1;](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule1.png)

3. Klikněte na **Možnosti**, zadejte databázi, ke které se chcete připojit, a potom do rozevíracího seznamu **Připojit k databázi** na kartě **Vlastnosti připojení** zadejte **sqldbtutorialdb**.
   
   ![Připojení jako user1 bez pravidla brány firewall&2;](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule2.png)

4. Klikněte na **Připojit**. 

   Zobrazí se dialogové okno oznamující, že počítač, ze kterého se pokoušíte připojit k SQL Database, nemá pravidlo brány firewall, které přístup k databázi povoluje. 

   ![Připojení jako user1 bez pravidla brány firewall&4;](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule4.png)


5. Z tohoto dialogového okna zkopírujte IP adresu klienta, která se použije v kroku 8.
6. Kliknutím na **OK** zavřete dialogové okno chyby, ale dialogové okno **Připojit k serveru** nezavírejte.
7. Vraťte se k počítači, pro který už máte vytvořené pravidlo brány firewall na úrovni serveru. 
8. Připojte se k databázi sqldbtutorialdb v SSMS jako správce serveru a spuštěním následujícího příkazu vytvořte bránu firewall na úrovni databáze pomocí IP adresy (nebo rozsahu adres) z kroku 5.  

   ```
   EXEC sp_set_database_firewall_rule @name = N'sqldbtutorialdbFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
   ```

   ![přidání pravidla brány firewall](./media/sql-database-control-access-sql-authentication-get-started/user1_add_rule_aw.png)

9. Znovu přejděte k druhému počítači a kliknutím na **Připojit** v dialogovém okně **Připojit k serveru** se připojte k sqldbtutorialdb jako user1. 

   > [!NOTE]
   > Než se vytvořené pravidlo brány firewall na úrovni databáze aktivuje, může to trvat až 5 minut.
   >

10. Po úspěšném připojení rozbalte **Databáze** v Průzkumníku objektů. Všimněte si, že **user1** může zobrazit jenom databázi **sqldbtutorialdb**.

   ![Připojení jako user1 s pravidlem brány firewall&1;](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_rule1.png)

11. Postupně rozbalte **sqldbtutorialdb** a **Tabulky**. Všimněte si, že user1 má oprávnění k zobrazení jediné tabulky – **SalesLT.ProductCategory**. 

   ![Připojení jako user1 a zobrazení objektů&1;](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_view_objects1.png)

## <a name="create-a-new-user-as-dbowner-and-a-database-level-firewall-rule"></a>Vytvoření nového uživatele jako db_owner a pravidla brány firewall na úrovni databáze

Kroky v tomto postupu ukazují, jak v jiné databázi vytvořit uživatele s oprávněním databázové role db_owner databáze a jak pro tuto databázi vytvořit bránu firewall na úrovni databáze. Tento nový uživatel se členstvím v roli **db_owner** se bude moci připojit k této izolované databázi (a spravovat ji).

1. Přejděte k počítači s připojením k SQL Database pomocí účtu správce serveru.
2. Otevřete okno dotazu připojené k databázi **blankdb** a spuštěním následujícího příkazu vytvořte v databázi blankdb uživatele blankdbadmin.

   ```
   CREATE USER blankdbadmin
   WITH PASSWORD = 'p@ssw0rd';
   ```

3. Ve stejném okně dotazu následujícím příkazem přidejte uživatele blankdbadmin do databázové role db_owner. Tento uživatel teď může provádět všechny akce nezbytné pro správu databáze blankdb.

   ```
   ALTER ROLE db_owner ADD MEMBER blankdbadmin; 
   ```

4. Ve stejném okně dotazu spusťte následující příkaz, kterým vytvoříte pravidlo brány firewall na úrovni databáze: [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) s použitím IP adresy z kroku 4 předchozího postupu (nebo rozsah IP adres pro uživatele této databáze):

   ```
   EXEC sp_set_database_firewall_rule @name = N'blankdbFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
   ```

5. Přejděte k druhému počítači (pro který jste vytvořili pravidlo brány firewall na úrovni databáze) a připojte se k databázi blankdb pomocí uživatelského účtu blankdbadmin.
6. Otevřete okno dotazu připojené k databázi blankdb a spuštěním následujícího příkazu vytvořte v databázi blankdb uživatele blankdbuser1.

   ```
   CREATE USER blankdbuser1
   WITH PASSWORD = 'p@ssw0rd';
   ```
 
7. Podle potřeby vašeho výukového prostředí vytvořte další pravidlo brány firewall na úrovni databáze pro tohoto uživatele. Pokud jste ale vytvořili pravidlo brány firewall na úrovni databáze pomocí rozsahu IP adres, možná to nebude nutné.

## <a name="grant-dbmanager-permissions-and-create-a-server-level-firewall-rule"></a>Udělení oprávnění dbmanager a vytvoření pravidla brány firewall na úrovni serveru

Kroky v tomto postupu vám ukážou, jak v hlavní databázi vytvořit přihlášení a uživatele s oprávněním vytvářet a spravovat nové uživatelské databáze. Tyto kroky také ukazují, jak vytvořit další pravidlo brány firewall na úrovni serveru pomocí Transact-SQL a příkazu [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx). 

> [!IMPORTANT]
>První pravidlo brány firewall na úrovni serveru se musí vždycky vytvořit v Azure (na webu Azure Portal, pomocí PowerShellu nebo pomocí rozhraní REST API).
>

> [!IMPORTANT]
> Vytvoření přihlášení v hlavní databázi a vytvoření uživatelského účtu z přihlášení je nutné proto, aby správce serveru mohl delegovat oprávnění k vytvoření databáze na jiného uživatele. Vytvoření přihlášení a následné vytvoření uživatelů ale snižuje přenositelnost vašeho prostředí.
>

1. Přejděte k počítači s připojením k SQL Database pomocí účtu správce serveru.
2. Otevřete okno dotazu připojené k hlavní databázi a následujícím příkazem vytvořte v hlavní databázi přihlášení dbcreator.

   ```
   CREATE LOGIN dbcreator
   WITH PASSWORD = 'p@ssw0rd';
   ```

3. Ve stejném okně dotazu: 

   ```
   CREATE USER dbcreator
   FROM LOGIN dbcreator;
   ```

3. Ve stejném okně dotazu následujícím příkazem přidejte uživatele dbcreator do databázové role dbmanager. Tento uživatel teď můžete vytvářet nové uživatelské databáze a spravovat je.

   ```
   ALTER ROLE dbmanager ADD MEMBER dbcreator; 
   ```

4. Ve stejném okně dotazu vytvořte pravidlo brány firewall na úrovni serveru spuštěním příkazu [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx). Použijte přitom IP adresu odpovídající vašemu prostředí:

   ```
   EXEC sp_set_firewall_rule @name = N'dbcreatorFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
   ```

5. Přejděte k druhému počítači (pro který jste vytvořili pravidlo brány firewall na úrovni serveru) a připojte se k hlavní databázi pomocí uživatelského účtu dbcreator.
6. Otevřete okno dotazu připojené k hlavní databázi a následujícím příkazem vytvořte v databázi nazvanou foo.

   ```
   CREATE DATABASE FOO (EDITION = 'basic');
   ```
 7. Volitelně můžete tuto databázi odstranit (a ušetřit tak peníze) pomocí příkazu:

   ```
   DROP DATABASE FOO;
   ```

## <a name="complete-script"></a>Celý skript

Jestli chcete vytvořit přihlášení a uživatele, přidat je do rolí, udělit jim oprávnění a vytvořit pravidla brány firewall na úrovni databáze a serveru, spusťte následující příkazy v požadovaných databázích na vašem serveru.

### <a name="master-database"></a>hlavní databáze
Tyto příkazy spusťte v hlavní databázi pomocí účtu správce serveru a přidejte odpovídající IP adresy nebo rozsah.

```
CREATE LOGIN dbcreator WITH PASSWORD = 'p@ssw0rd';
CREATE USER dbcreator FROM LOGIN dbcreator;
ALTER ROLE dbmanager ADD MEMBER dbcreator;
EXEC sp_set_firewall_rule @name = N'dbcreatorFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
```

### <a name="sqldbtutorialdb-database"></a>databáze sqldbtutorialdb
Tyto příkazy spusťte v databázi sqldbtutorialdb pomocí účtu správce serveru a přidejte odpovídající IP adresy nebo jejich rozsah.

```
CREATE USER user1 WITH PASSWORD = 'p@ssw0rd';
GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to user1;
EXEC sp_set_database_firewall_rule @name = N'sqldbtutorialdbFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
```

### <a name="blankdb-database"></a>Databáze blankdb
Tyto příkazy spusťte v databázi blankdb pomocí účtu správce serveru a přidejte odpovídající IP adresy nebo rozsah.

```
CREATE USER blankdbadmin
   WITH PASSWORD = 'p@ssw0rd';
ALTER ROLE db_owner ADD MEMBER blankdbadmin;
EXEC sp_set_database_firewall_rule @name = N'blankdbFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
CREATE USER blankdbuser1
   WITH PASSWORD = 'p@ssw0rd';
```

## <a name="next-steps"></a>Další kroky
- Přehled řízení a přístupu pro SQL Database najdete v tématu věnovaném [řízení a přístupu k SQL Database](sql-database-control-access.md).
- Přehled přihlášení, uživatelů a databázových rolí ve službě SQL Database najdete v tématu věnovaném [přihlášením, uživatelům a databázovým rolím](sql-database-manage-logins.md).
- Další informace o objektech zabezpečení databáze najdete v tématu [Objekty zabezpečení](https://msdn.microsoft.com/library/ms181127.aspx).
- Další informace o databázových rolích najdete v tématu věnovaném [databázovým rolím](https://msdn.microsoft.com/library/ms189121.aspx).
- Další informace o pravidlech brány firewall pro SQL Database najdete v tématu [Pravidla brány firewall služby SQL Database](sql-database-firewall-configure.md).
- Kurz ověřování pomocí Azure Active Directory najdete v tématu [Ověřování a autorizace Azure AD](sql-database-control-access-aad-authentication-get-started.md).




<!--HONumber=Feb17_HO3-->


