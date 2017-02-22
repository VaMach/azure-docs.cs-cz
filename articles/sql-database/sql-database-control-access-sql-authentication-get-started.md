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
ms.date: 01/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 356cc4c6d8e25d36880e4b12bf471326e61990c3
ms.openlocfilehash: 275a33567fa1472573bc8abc87948ad306e853f0


---
# <a name="sql-database-tutorial-sql-server-authentication-logins-and-user-accounts-database-roles-permissions-server-level-firewall-rules-and-database-level-firewall-rules"></a>Kurz k SQL Database: Ověřování pomocí SQL Serveru, přihlašovací údaje a uživatelské účty, databázové role, oprávnění, pravidla brány firewall na úrovni serveru a pravidla brány firewall na úrovni databáze
V tomto úvodním kurzu se naučíte, jak pomocí sady SQL Server Management Studio pracovat s ověřováním pomocí SQL Serveru, přihlašováním, uživateli a databázovými rolemi, které udělují přístup a oprávnění pro databáze a servery Azure SQL Database. Naučíte se tyto postupy:

- Zobrazení uživatelských oprávnění v hlavní databázi a uživatelských databázích
- Vytváření přihlašovacích údajů a uživatelů na základě ověřování pomocí SQL Serveru
- Udělení oprávnění pro uživatele na úrovni serveru nebo konkrétních databází
- Přihlášení k uživatelské databázi jako jiný uživatel než správce
- Vytvoření pravidel brány firewall na úrovni databáze pro uživatele databází
- Vytvoření pravidel brány firewall na úrovni serveru pro správce serverů

**Časový odhad:** Dokončení tohoto kurzu trvá přibližně 45 minut (za předpokladu, že už máte splněné požadavky).

## <a name="prerequisites"></a>Požadavky

* Potřebujete mít účet Azure. Můžete si [zdarma otevřít účet Azure](/pricing/free-trial/?WT.mc_id=A261C142F) nebo [aktivovat výhody pro předplatitele sady Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* Musíte být schopni připojit se k webu Azure Portal pomocí účtu, který je členem role přispěvatele nebo vlastníka předplatného. Další informace o řízení přístupu na základě role (RBAC) najdete v tématu [Začínáme se správou přístupu na webu Azure Portal](../active-directory/role-based-access-control-what-is.md).

* Dokončili jste kurz [Začínáme se servery, databázemi a pravidly brány firewall služby Azure SQL Database s využitím webu Azure Portal a aplikace SQL Server Management Studio](sql-database-get-started.md) nebo ekvivalentní [verzi tohoto kurzu pro prostředí PowerShell](sql-database-get-started-powershell.md). Pokud ne, dokončete tento požadovaný kurz nebo spusťte skript prostředí PowerShell na konci [verze tohoto kurzu pro prostředí PowerShell](sql-database-get-started-powershell.md) a teprve potom pokračujte.

> [!NOTE]
> Tento kurz vám pomůže seznámit se s obsahem těchto výukových okruhů: [Řízení a přístup k SQL Database](sql-database-control-access.md), [Přihlašování, uživatelé a databázové role](sql-database-manage-logins.md), [Objekty zabezpečení](https://msdn.microsoft.com/library/ms181127.aspx), [Databázové role](https://msdn.microsoft.com/library/ms189121.aspx) a [Pravidla brány firewall pro SQL Database](sql-database-firewall-configure.md).
>  

## <a name="sign-in-to-the-azure-portal-using-your-azure-account"></a>Přihlášení k webu Azure Portal pomocí účtu Azure
Pomocí [stávajícího předplatného](https://account.windowsazure.com/Home/Index) se k webu Azure Portal připojíte následujícím postupem.

1. Otevřete prohlížeč, kterému dáváte přednost, a připojte se k [portálu Azure](https://portal.azure.com/).
2. Přihlaste se k webu [Portál Azure](https://portal.azure.com/).
3. Na **přihlašovací stránce** zadejte přihlašovací údaje ke svému předplatnému.
   
   ![Přihlášení](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="view-information-about-the-security-configuration-for-your-logical-server"></a>Zobrazení informací o konfiguraci zabezpečení logického serveru

V této části kurzu zobrazíte na webu Azure Portal informace o konfiguraci zabezpečení logického serveru.

1. Otevřete okno **SQL Server** pro váš logický server a prohlédněte si informace na stránce **Přehled**.

   ![Účet správce serveru na webu Azure Portal](./media/sql-database-control-access-sql-authentication-get-started/sql_admin_portal.png)

2. Poznamenejte si název účtu správce serveru pro logický server. Pokud si nepamatujete heslo, kliknutím na tlačítko **resetovat heslo** nastavte nové heslo.

> [!NOTE]
> Pokud chcete zkontrolovat informace o připojení pro tento server, přejděte k tématu [Zobrazení nebo aktualizace nastavení serveru](sql-database-view-update-server-settings.md). Pro tuto řadu kurzů se jako plně kvalifikovaný název serveru používá sqldbtutorialserver.database.windows.net.
>

## <a name="connect-to-sql-server-using-sql-server-management-studio-ssms"></a>Připojení k SQL serveru pomocí aplikace SQL Server Management Studio (SSMS)

1. Pokud jste to ještě neudělali, stáhněte si nejnovější verzi SMSS v části [Stažení aplikace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) a nainstalujte ji. Když bude k dispozici pro stažení novější verze, aplikace SSMS zobrazí upozornění, abyste mohli používat aktuální verzi.

2. Po instalaci zadejte do vyhledávacího pole ve Windows text **Microsoft SQL Server Management Studio** a klikněte na **Enter**. Otevře se SSMS.

   ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)

3. V dialogovém okně **Připojení k serveru** zadejte informace potřebné k připojení k SQL serveru pomocí ověřování SQL Serveru a účet správce serveru.

   ![Připojení k serveru](./media/sql-database-get-started/connect-to-server.png)

4. Klikněte na **Připojit**.

   ![Připojeno k serveru](./media/sql-database-get-started/connected-to-server.png)

## <a name="view-the-server-admin-account-and-its-permissions"></a>Zobrazení účtu správce serveru a jeho oprávnění 
V této části kurzu zobrazíte informace o účtu správce serveru a jeho oprávněních v hlavní databázi a uživatelských databázích.

1. V Průzkumníku objektů rozbalte **Zabezpečení** a potom **Přihlášení**, zobrazí se existující přihlášení pro váš server Azure SQL Database. Všimněte si, že se zobrazí přihlašovací údaje pro účet správce serveru zadaný během zřizování – sqladmin pro tuto řadu kurzů.

   ![Přihlašovací jméno správce serveru](./media/sql-database-control-access-sql-authentication-get-started/server_admin_login.png)

2. V Průzkumníku objektů postupně rozbalte **Databáze**, **Systémové databáze**, **hlavní**, **Zabezpečení** a **Uživatelé**. Všimněte si, že v hlavní databázi byl vytvořen uživatelský účet pro přihlášení správce serveru, a to se stejným jménem, jaké má jeho uživatelský účet (jména se sice nemusí shodovat, ale je to doporučený postup).

   ![uživatelský účet hlavní databáze pro správce serveru](./media/sql-database-control-access-sql-authentication-get-started/master_database_user_account_for_server_admin.png)

   > [!NOTE]
   > Informace o ostatních zobrazených uživatelských účtech najdete v tématu [Objekty zabezpečení](https://msdn.microsoft.com/library/ms181127.aspx).
   >

3. V Průzkumníku objektů klikněte pravým tlačítkem na **hlavní** a potom klikněte na **Nový dotaz**. Otevře se okno dotazu připojené k hlavní databázi.
4. V okně dotazu spusťte následující dotaz, který vrací informace o uživateli spouštějícím tento dotaz. Všimněte si, že jako uživatelský účet spouštějící tento dotaz se vrátí sqladmin (když dále v tomto postupu zadáme dotaz na uživatelskou databázi, uvidíme jiný výsledek).

   ```
   SELECT USER;
   ```

   ![Výběr dotazu uživatele v hlavní databázi](./media/sql-database-control-access-sql-authentication-get-started/select_user_query_in_master_database.png)

5. V okně dotazu spusťte následující dotaz, který vrací informace o oprávnění uživatele sqladmin. Všimněte si, že uživatel sqladmin má oprávnění pro připojení k hlavní databázi, vytvoření přihlášení a uživatelů, získávání informací z tabulky sys.sql_logins a přidání uživatelů do databázových rolí dbmanager a dbcreator. Tato oprávnění jsou nad rámec oprávnění udělovaných veřejné roli, ze které dědí oprávnění všichni uživatelé (třeba oprávnění pro výběr informací z konkrétních tabulek). Další informace najdete v tématu [Oprávnění](https://msdn.microsoft.com/library/ms191291.aspx).

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

6. V Průzkumníku objektů postupně rozbalte **blankdb**, **Zabezpečení** a **Uživatelé**. Všimněte si, že v této databázi není žádný uživatelský účet s názvem sqladmin.

   ![Uživatelské účty v blankdb](./media/sql-database-control-access-sql-authentication-get-started/user_accounts_in_blankdb.png)

7. V Průzkumníku objektů klikněte pravým tlačítkem na **blankdb** a potom klikněte na **Nový dotaz**.

8. V okně dotazu spusťte následující dotaz, který vrací informace o uživateli spouštějícím tento dotaz. Všimněte si, že jako uživatelský účet spouštějící tento dotaz se vrátí dbo (ve výchozím nastavení se v každé uživatelské databázi přihlášení správce serveru mapuje na uživatelský účet dbo).

   ```
   SELECT USER;
   ```

   ![Výběr dotazu uživatele v databázi blankdb](./media/sql-database-control-access-sql-authentication-get-started/select_user_query_in_blankdb_database.png)

9. V okně dotazu spusťte následující dotaz, který vrací informace o oprávnění uživatele dbo. Všimněte si, že dbo je členem veřejné role a také pevné databázové role db_owner. Další informace najdete v tématu [Role na úrovni databáze](https://msdn.microsoft.com/library/ms189121.aspx).

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

10. Pokud chcete, můžete předchozí tři kroky zopakovat pro uživatelskou databázi AdventureWorksLT.

## <a name="create-a-new-user-in-the-adventureworkslt-database-with-select-permissions"></a>Vytvoření nového uživatele v databázi AdventureWorksLT s oprávněními SELECT

V této části kurzu vytvoříte uživatelský účet v databázi AdventureWorksLT, otestujete oprávnění tohoto uživatele jako člena veřejné role, udělíte mu oprávnění SELECT a potom oprávnění tohoto uživatele otestujete znovu.

> [!NOTE]
> Uživatelé na úrovni databáze ([uživatelé s omezením](https://msdn.microsoft.com/library/ff929188.aspx)) zvyšují přenositelnost databáze. Této vlastnosti se budeme věnovat v budoucích kurzech.
>

1. V Průzkumníku objektů klikněte pravým tlačítkem na **AdventureWorksLT** a potom klikněte na **Nový dotaz**. Otevře se okno dotazu připojené k databázi AdventureWorksLT.
2. Spusťte následující příkaz, který v databázi AdventureWorksLT vytvoří uživatelský účet s názvem user1.

   ```
   CREATE USER user1
   WITH PASSWORD = 'p@ssw0rd';
   ```
   ![nový uživatel user1 databáze AdventureWorksLT](./media/sql-database-control-access-sql-authentication-get-started/new_user_user1_aw.png)

3. V okně dotazu spusťte následující dotaz, který vrací informace o oprávnění uživatele user1. Všimněte si, že jediná oprávnění, která user1 má, jsou oprávnění zděděná z veřejné role.

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

4. Spusťte následující dotazy, které se pokusí dotazovat na tabulku v databázi AdventureWorksLT jako uživatel user1.

   ```
   EXECUTE AS USER = 'user1';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![Bez oprávnění SELECT](./media/sql-database-control-access-sql-authentication-get-started/no_select_permissions.png)

5. Spusťte následující příkaz, kterým uživateli user1 udělíte oprávnění SELECT pro tabulku ProductCategory ve schématu SalesLT.

   ```
   GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to user1;
   ```

   ![Udělení oprávnění SELECT](./media/sql-database-control-access-sql-authentication-get-started/grant_select_permissions.png)

6. Spusťte následující dotazy, které se pokusí dotazovat na tabulku v databázi AdventureWorksLT jako uživatel user1.

   ```
   EXECUTE AS USER = 'user1';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![Oprávnění SELECT](./media/sql-database-control-access-sql-authentication-get-started/select_permissions.png)

## <a name="create-a-database-level-firewall-rule-for-an-adventureworkslt-database-user"></a>Vytvoření pravidla brány firewall na úrovni databáze pro uživatele databáze AdventureWorksLT

V této části kurzu zaprotokolujete pokus o přihlášení z počítače s jinou IP adresou, jako správce serveru vytvoříte pravidlo brány firewall na úrovni databáze a pak se pomocí tohoto nového pravidla brány firewall na úrovni databáze přihlásíte. 

> [!NOTE]
> [Pravidla brány firewall na úrovni databáze](sql-database-firewall-configure.md) zvyšují přenositelnost databáze. Této vlastnosti se budeme věnovat v budoucích kurzech.
>

1. V jiném počítači, pro který jste ještě nevytvořili pravidlo brány firewall na úrovni serveru, otevřete SQL Server Management Studio.

   > [!IMPORTANT]
   > Vždy používejte nejnovější verzi SSMS, která je dostupná na stránce pro [stažení SQL Server Management Studia](https://msdn.microsoft.com/library/mt238290.aspx). 
   >

2. V okně **Připojit k serveru** zadejte název serveru a ověřovací informace pro připojení pomocí ověřování SQL Serveru s účtem user1. 
    
   ![Připojení jako user1 bez pravidla brány firewall 1](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule1.png)

3. Klikněte na **Možnosti**, ke které se chcete připojit, a potom do rozevíracího seznamu **Připojit k databázi** na kartě **Vlastnosti připojení** zadejte **AdventureWorksLT**.
   
   ![Připojení jako user1 bez pravidla brány firewall 2](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule2.png)

4. Klikněte na **Připojit**. Zobrazí se dialogové okno oznamující, že počítač, ze kterého se pokoušíte připojit k SQL Database, nemá pravidlo brány firewall, které přístup k databázi povoluje. Dialogové okno, které se zobrazí, má dvě varianty. Závisí na krocích, které jste s branami firewall už provedli, ale obvykle se zobrazí první uvedené okno.

   ![Připojení jako user1 bez pravidla brány firewall 3](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule3.png)

   ![Připojení jako user1 bez pravidla brány firewall 4](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule4.png)

   > [!NOTE]
   > Nejnovější verze aplikace SSMS obsahuje funkci, která přispěvatelům a vlastníkům předplatného umožňuje přihlášení do Microsoft Azure a vytvoření pravidla brány firewall na úrovni serveru.
   > 

4. Zkopírujte z tohoto dialogového okna IP adresu klienta, která se použije v kroku 7.
5. Klikněte na **Zrušit**, ale dialogové okno **Připojit k serveru** nezavírejte.
6. Vraťte se k počítači, pro který už máte vytvořené pravidlo brány firewall na úrovni serveru, a připojte se k serveru pomocí účtu správce serveru.
7. Jako správce serveru připojený k databázi AdventureWorksLT spusťte v novém okně dotazu následující příkaz pro vytvoření brány firewall na úrovni databáze: [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx). Využijte přitom IP adresu z kroku 4:

   ```
   EXEC sp_set_database_firewall_rule @name = N'AdventureWorksLTFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
   ```

   ![přidání pravidla brány firewall](./media/sql-database-control-access-sql-authentication-get-started/user1_add_rule_aw.png)

8. Znovu přejděte k druhému počítači a kliknutím na **Připojit** v dialogovém okně **Připojit k serveru** se připojte k AdventureWorksLT jako user1. 

   ![Připojení jako user1 s pravidlem brány firewall 1](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_rule1.png)

9. V Průzkumníku objektů postupně rozbalte **Databáze**, **AdventureWorksLT** a nakonec **Tabulky**. Všimněte si, že user1 má oprávnění k zobrazení jediné tabulky – **SalesLT.ProductCategory**. 

   ![Připojení jako user1 a zobrazení objektů 1](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_view_objects1.png)

10. V Průzkumníku objektů klikněte pravým tlačítkem na **SalesLT.ProductCategory** a potom klikněte na **Select Top 1000 Rows** (Vybrat prvních 1000 řádků).   

   ![dotaz query1 uživatele user1](./media/sql-database-control-access-sql-authentication-get-started/user1_query1.png)

   ![výsledky dotazu query1 uživatele user1](./media/sql-database-control-access-sql-authentication-get-started/user1_query1_results.png)

## <a name="create-a-new-user-in-the-blankdb-database-with-dbowner-database-role-permissions-and-a-database-level-firewall-rule"></a>Vytvoření nového uživatele v databázi blankdb s oprávněním databázové role db_owner a s pravidlem brány firewall na úrovni databáze

V této části kurzu vytvoříte s použitím účtu správce serveru nového uživatele v databázi blankdb s oprávněním databázové role db_owner a s pravidlem brány firewall na úrovni databáze. 

1. Přejděte k počítači s připojením k SQL Database pomocí účtu správce serveru.
2. Otevřete okno dotazu připojené k databázi blankdb a spuštěním následujícího příkazu vytvořte v databázi blankdb uživatele blankdbadmin.

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
 
7. Podle potřeby vašeho výukového prostředí vytvořte další pravidlo brány firewall na úrovni databáze pro tohoto uživatele. 

## <a name="create-a-new-login-and-user-in-the-master-database-with-dbmanager-permissions-and-create-a-server-level-firewall-rule"></a>Vytvoření nového přihlášení a uživatele s oprávněním dbmanager v hlavní databázi a vytvoření nového pravidla brány firewall na úrovni serveru

V této části kurzu vytvoříte v hlavní databázi přihlášení a uživatele s oprávněním vytvářet a spravovat nové uživatelské databáze. Vytvoříte také další pravidlo brány firewall na úrovni serveru pomocí Transact-SQL a příkazu [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx).

> [!NOTE]
> Vytvoření přihlášení v hlavní databázi a vytvoření uživatelského účtu z přihlášení je nutné proto, aby vlastník účtu správce serveru mohl delegovat oprávnění k vytvoření databáze na jiného uživatele. Vytvořením přihlášení a uživatele z přihlášení se však sníží přenositelnost vašeho prostředí. Tyto důsledky prozkoumáme v dalších kurzech – včetně toho, jak s nimi počítat při plánování zotavení po havárii.
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

4. Ve stejném okně dotazu vytvořte pravidlo brány firewall na úrovni serveru příkazem [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) s použitím IP adresy odpovídající vašemu prostředí:

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

### <a name="adventureworkslt-database"></a>Databáze AdventureWorksLT
Tyto příkazy spusťte v databázi AdventureWorksLT pomocí účtu správce serveru a přidejte odpovídající IP adresy nebo rozsah.

```
CREATE USER user1 WITH PASSWORD = 'p@ssw0rd';
GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to user1;
EXEC sp_set_database_firewall_rule @name = N'AdventureWorksLTFirewallRule', 
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
- Kurz k ověřování pomocí Azure Active Directory najdete v článku [Kurz k SQL Database: Ověřování AAD, přihlašovací údaje a uživatelské účty, databázové role, oprávnění, pravidla brány firewall na úrovni serveru a pravidla brány firewall na úrovni databáze](sql-database-control-access-sql-authentication-get-started.md).




<!--HONumber=Jan17_HO3-->


