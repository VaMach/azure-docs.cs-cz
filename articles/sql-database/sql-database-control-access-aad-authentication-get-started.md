---
title: "Ověřování AAD: Přístup, ověřování, brány firewall služby Azure SQL Database | Dokumentace Microsoftu"
description: "V tomto úvodním kurzu se naučíte, jak pomocí sady SQL Server Management Studio a Transact-SQL pracovat s pravidly brány firewall na úrovni serveru a databáze, ověřováním pomocí Azure Active Directory, přihlašováním, uživateli a rolemi a jak pro databáze a servery Azure SQL Database přidělovat přístup a řízení."
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
ms.sourcegitcommit: 7d061c083b23de823d373c30f93cccfe1c856ba3
ms.openlocfilehash: 8a6dc7d3dca80782a55e13b53180b1542b61544b


---
# <a name="azure-ad-authentication-access-and-database-level-firewall-rules"></a>Ověřování Azure AD, přístup a pravidla brány firewall na úrovni databáze
V tomto kurzu se naučíte, jak pomocí sady SQL Server Management Studio pracovat s ověřováním pomocí Azure Active Directory, přihlašováním, uživateli a databázovými rolemi, které udělují přístup a oprávnění pro databáze a servery Azure SQL Database. Naučíte se tyto postupy:

- Zobrazení uživatelských oprávnění v hlavní databázi a uživatelských databázích
- Vytváření přihlašovacích údajů a uživatelů na základě ověřování pomocí Azure Active Directory
- Udělení oprávnění pro uživatele na úrovni serveru nebo konkrétních databází
- Přihlášení k uživatelské databázi jako jiný uživatel než správce
- Vytvoření pravidel brány firewall na úrovni databáze pro uživatele databází
- Vytvoření pravidel brány firewall na úrovni serveru pro správce serverů

**Časový odhad:** Dokončení tohoto kurzu trvá přibližně 45 minut (za předpokladu, že už máte splněné požadavky).

## <a name="prerequisites"></a>Požadavky

* Potřebujete mít účet Azure. Můžete si [zdarma otevřít účet Azure](https://azure.microsoft.com/free/) nebo [aktivovat výhody pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/). 

* Musíte být schopni připojit se k webu Azure Portal pomocí účtu, který je členem role přispěvatele nebo vlastníka předplatného. Další informace o řízení přístupu na základě role (RBAC) najdete v tématu [Začínáme se správou přístupu na webu Azure Portal](../active-directory/role-based-access-control-what-is.md).

* Dokončili jste kurz [Začínáme se servery, databázemi a pravidly brány firewall služby Azure SQL Database s využitím webu Azure Portal a aplikace SQL Server Management Studio](sql-database-get-started.md) nebo ekvivalentní [verzi tohoto kurzu pro prostředí PowerShell](sql-database-get-started-powershell.md). Pokud ne, dokončete tento požadovaný kurz nebo spusťte skript prostředí PowerShell na konci [verze tohoto kurzu pro prostředí PowerShell](sql-database-get-started-powershell.md) a teprve potom pokračujte.

   > [!NOTE]
   > Dokončení souvisejícího kurzu pro ověřování SQL Serveru ([Ověřování SQL, přihlašovací údaje a uživatelské účty, databázové role, oprávnění, pravidla brány firewall na úrovni serveru a pravidla brány firewall na úrovni databáze](sql-database-control-access-sql-authentication-get-started.md)) není povinné, ale jsou v něm popsaná některá témata, kterým se v tomto kurzu už znovu nevěnujeme. Pokud jste už na stejných počítačích (se stejnými IP adresami) dokončili tento související kurz, postupy v tomto kurzu, které souvisejí s branami firewall na úrovni serveru a databáze, se nevyžadují a jsou z tohoto důvodu označené jako nepovinné. Také snímky obrazovky v tomto kurzu předpokládají, že jste tento související kurz dokončili. 
   >

* Už jste vytvořili a naplnili Azure Active Directory. Další informace najdete v tématech [Integrování místních identit do služby Azure Active Directory](../active-directory/active-directory-aadconnect.md), [Přidání vlastního názvu domény do Azure AD](../active-directory/active-directory-add-domain.md), [Microsoft Azure podporuje federaci s Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Správa adresáře služby Azure AD](https://msdn.microsoft.com/library/azure/hh967611.aspx), [Správa služby Azure AD pomocí rozhraní Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) a [Porty a protokoly, které vyžaduje hybridní identita](../active-directory/active-directory-aadconnect-ports.md).

> [!NOTE]
> Tento kurz vám pomůže seznámit se s obsahem těchto výukových okruhů: [Řízení a přístup k SQL Database](sql-database-control-access.md), [Přihlašování, uživatelé a databázové role](sql-database-manage-logins.md), [Objekty zabezpečení](https://msdn.microsoft.com/library/ms181127.aspx), [Databázové role](https://msdn.microsoft.com/library/ms189121.aspx), [Pravidla brány firewall SQL Database](sql-database-firewall-configure.md) a [Ověřování pomocí Azure Active Directory](sql-database-aad-authentication.md). 
>  

## <a name="sign-in-to-the-azure-portal-using-your-azure-account"></a>Přihlášení k webu Azure Portal pomocí účtu Azure
Pomocí [stávajícího předplatného](https://account.windowsazure.com/Home/Index) se k webu Azure Portal připojíte následujícím postupem.

1. Otevřete prohlížeč, kterému dáváte přednost, a připojte se k [portálu Azure](https://portal.azure.com/).
2. Přihlaste se k webu [Portál Azure](https://portal.azure.com/).
3. Na **přihlašovací stránce** zadejte přihlašovací údaje ke svému předplatnému.
   
   ![Přihlášení](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="provision-an-azure-active-directory-admin-for-your-sql-logical-server"></a>Zřízení správce služby Azure Active Directory pro logický server SQL

V této části kurzu zobrazíte na webu Azure Portal informace o konfiguraci zabezpečení logického serveru.

1. Otevřete okno **SQL Server** pro váš logický server a prohlédněte si informace na stránce **Přehled**. Všimněte si, že správce Azure Active Directory není nakonfigurovaný.

   ![Účet správce serveru na webu Azure Portal](./media/sql-database-control-access-aad-authentication-get-started/sql_admin_portal.png)

2. Kliknutím na **Nenakonfigurováno** v podokně **Základy** otevřete okno **Správce Active Directory**.

   ![Okno AAD](./media/sql-database-control-access-aad-authentication-get-started/aad_blade.png)

3. Kliknutím na **Nastavit správce** otevřete okno **Přidat správce**. Vyberte účet skupiny nebo uživatele Active Directory jako správce Active Directory pro váš server.

   ![Výběr účtu správce AAD](./media/sql-database-control-access-aad-authentication-get-started/aad_admin.png)

4. Klikněte na **Vybrat** a potom klikněte na **Uložit**.

   ![Uložení vybraného účtu správce AAD](./media/sql-database-control-access-aad-authentication-get-started/aad_admin_save.png)

> [!NOTE]
> Pokud chcete zkontrolovat informace o připojení pro tento server, přejděte k tématu [Správa serverů](sql-database-manage-servers-portal.md). Pro tuto řadu kurzů se jako plně kvalifikovaný název serveru používá sqldbtutorialserver.database.windows.net.
>

## <a name="connect-to-sql-server-using-sql-server-management-studio-ssms"></a>Připojení k SQL serveru pomocí aplikace SQL Server Management Studio (SSMS)

1. Pokud jste to ještě neudělali, stáhněte si nejnovější verzi SMSS v části [Stažení aplikace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) a nainstalujte ji. Když bude k dispozici pro stažení novější verze, aplikace SSMS zobrazí upozornění, abyste mohli používat aktuální verzi.

2. Po instalaci zadejte do vyhledávacího pole ve Windows text **Microsoft SQL Server Management Studio** a klikněte na **Enter**. Otevře se SSMS.

   ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)

3. V dialogovém okně **Připojit k serveru** vyberte jednu z metod ověřování služby Active Directory a potom zadejte příslušné ověřovací informace. Informace o volbě metody najdete v tématech [Ověřování pomocí Azure Active Directory](sql-database-aad-authentication.md) a [Podpora nástroje SSMS pro ověřování Azure AD MFA](sql-database-ssms-mfa-authentication.md).

   ![Připojení k serveru pomocí AAD](./media/sql-database-control-access-aad-authentication-get-started/connect_to_server_with_aad.png)

4. Pomocí ověřování SQL Serveru a účtu správce serveru zadejte informace potřebné pro připojení k SQL serveru.

5. Klikněte na **Připojit**.

   ![Připojení k serveru pomocí AAD](./media/sql-database-control-access-aad-authentication-get-started/connected_to_server_with_aad.png)

## <a name="view-the-server-admin-account-and-its-permissions"></a>Zobrazení účtu správce serveru a jeho oprávnění 
V této části kurzu zobrazíte informace o účtu správce serveru a jeho oprávněních v hlavní databázi a uživatelských databázích.

1. V Průzkumníku objektů postupně rozbalte **Databáze**, **Systémové databáze**, **hlavní**, **Zabezpečení** a **Uživatelé**. Všimněte si, že v hlavní databázi se pro správce Active Directory vytvořil uživatelský účet. Dál si všimněte, že se pro uživatelský účet správce Active Directory nevytvořilo přihlášení.

   ![Uživatelský účet hlavní databáze pro správce AAD](./media/sql-database-control-access-aad-authentication-get-started/master_database_user_account_for_AAD_admin.png)

   > [!NOTE]
   > Informace o ostatních zobrazených uživatelských účtech najdete v tématu [Objekty zabezpečení](https://msdn.microsoft.com/library/ms181127.aspx).
   >

2. V Průzkumníku objektů klikněte pravým tlačítkem na **hlavní** a potom klikněte na **Nový dotaz**. Otevře se okno dotazu připojené k hlavní databázi.
3. V okně dotazu spusťte následující dotaz, který vrací informace o uživateli spouštějícím tento dotaz. Všimněte si, že jako uživatelský účet spouštějící tento dotaz se vrátí user@microsoft.com (když dále v tomto postupu zadáme dotaz na uživatelskou databázi, zobrazí se jiný výsledek).

   ```
   SELECT USER;
   ```

   ![Výběr dotazu uživatele v hlavní databázi](./media/sql-database-control-access-aad-authentication-get-started/select_user_query_in_master_database.png)

4. V okně dotazu spusťte následující dotaz, který vrací informace o oprávnění uživatele s právy pro správu Active Directory. Všimněte si, že uživatel s právy pro správu Active Directory má oprávnění pro připojení k hlavní databázi, vytvoření přihlášení a uživatelů, výběru informací z tabulky sys.sql_logins a přidání uživatelů do databázových rolí dbmanager a dbcreator. Tato oprávnění jsou nad rámec oprávnění udělovaných veřejné roli, ze které dědí oprávnění všichni uživatelé (třeba oprávnění pro výběr informací z konkrétních tabulek). Další informace najdete v tématu [Oprávnění](https://msdn.microsoft.com/library/ms191291.aspx).

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
   WHERE p.name = 'user@microsoft.com';
   ```

   ![Oprávnění správce AAD v hlavní databázi](./media/sql-database-control-access-aad-authentication-get-started/aad_admin_permissions_in_master_database.png)

6. V Průzkumníku objektů postupně rozbalte **blankdb**, **Zabezpečení** a **Uživatelé**. Všimněte si, že v této databázi není žádný uživatelský účet s názvem user@microsoft.com.

   ![Uživatelské účty v blankdb](./media/sql-database-control-access-aad-authentication-get-started/user_accounts_in_blankdb.png)

7. V Průzkumníku objektů klikněte pravým tlačítkem na **blankdb** a potom klikněte na **Nový dotaz**.

8. V okně dotazu spusťte následující dotaz, který vrací informace o uživateli spouštějícím tento dotaz. Všimněte si, že jako uživatelský účet spouštějící tento dotaz se vrátí dbo (ve výchozím nastavení se v každé uživatelské databázi přihlášení správce serveru mapuje na uživatelský účet dbo).

   ```
   SELECT USER;
   ```

   ![Výběr dotazu uživatele v databázi blankdb](./media/sql-database-control-access-aad-authentication-get-started/select_user_query_in_blankdb_database.png)

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

   ![Oprávnění správce serveru v databázi blankdb](./media/sql-database-control-access-aad-authentication-get-started/aad_admin_permissions_in_blankdb_database.png)

10. Pokud chcete, můžete předchozí tři kroky zopakovat pro uživatelskou databázi AdventureWorksLT.

## <a name="create-a-new-user-in-the-adventureworkslt-database-with-select-permissions"></a>Vytvoření nového uživatele v databázi AdventureWorksLT s oprávněními SELECT

V této části kurzu vytvoříte uživatelský účet v databázi AdventureWorksLT na základě názvu objektu zabezpečení uživatele Azure AD nebo zobrazovaného názvu skupiny Azure AD, otestujete oprávnění tohoto uživatele jako člena veřejné role, udělíte mu oprávnění SELECT a potom oprávnění tohoto uživatele otestujete znovu.

> [!NOTE]
> Uživatelé na úrovni databáze ([uživatelé s omezením](https://msdn.microsoft.com/library/ff929188.aspx)) zvyšují přenositelnost databáze. Této vlastnosti se budeme věnovat v budoucích kurzech.
>

1. V Průzkumníku objektů klikněte pravým tlačítkem na **AdventureWorksLT** a potom klikněte na **Nový dotaz**. Otevře se okno dotazu připojené k databázi AdventureWorksLT.
2. Spusťte následující příkaz, který v databázi AdventureWorksLT vytvoří uživatelský účet pro uživatele v doméně Microsoft s názvem aaduser1.

   ```
   CREATE USER [aaduser1@microsoft.com]
   FROM EXTERNAL PROVIDER;
   ```
   ![Nový uživatel aaduser1@microsoft.com databáze AdventureWorksLT](./media/sql-database-control-access-aad-authentication-get-started/new_user_aaduser1@microsoft.com_aw.png)

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
   WHERE p.name = 'aaduser1@microsoft.com';
   ```

   ![Nová uživatelská oprávnění v uživatelské databázi](./media/sql-database-control-access-aad-authentication-get-started/new_user_permissions_in_user_database.png)

4. Spusťte následující dotazy, které se pokusí dotazovat na tabulku v databázi AdventureWorksLT jako uživatel user1.

   ```
   EXECUTE AS USER = 'aaduser1@microsoft.com';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![Bez oprávnění SELECT](./media/sql-database-control-access-aad-authentication-get-started/no_select_permissions.png)

5. Spusťte následující příkaz, kterým uživateli user1 udělíte oprávnění SELECT pro tabulku ProductCategory ve schématu SalesLT.

   ```
   GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to [aaduser1@microsoft.com];
   ```

   ![Udělení oprávnění SELECT](./media/sql-database-control-access-aad-authentication-get-started/grant_select_permissions.png)

6. Spusťte následující dotazy, které se pokusí dotazovat na tabulku v databázi AdventureWorksLT jako uživatel user1.

   ```
   EXECUTE AS USER = 'aaduser1@microsoft.com';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![Oprávnění SELECT](./media/sql-database-control-access-aad-authentication-get-started/select_permissions.png)

## <a name="create-a-database-level-firewall-rule-for-adventureworkslt-database-users"></a>Vytvoření pravidla brány firewall na úrovni databáze pro uživatele databáze AdventureWorksLT

> [!NOTE]
> Tento postup není nutné dokončit, pokud jste dokončili ekvivalentní postup v souvisejícím kurzu pro ověřování SQL Serveru, [ověřování a autorizace SQL](sql-database-control-access-sql-authentication-get-started.md) a učíte se s použitím stejného počítače se stejnou IP adresou.
>

V této části kurzu se pokusíte přihlásit pomocí nového uživatelského účtu z počítače s jinou IP adresou, jako správce serveru vytvoříte pravidlo brány firewall na úrovni databáze a úspěšně se pomocí tohoto nového pravidla brány firewall na úrovni databáze přihlásíte. 

> [!NOTE]
> [Pravidla brány firewall na úrovni databáze](sql-database-firewall-configure.md) zvyšují přenositelnost databáze. Této vlastnosti se budeme věnovat v budoucích kurzech.
>

1. V jiném počítači, pro který jste ještě nevytvořili pravidlo brány firewall na úrovni serveru, otevřete SQL Server Management Studio.

   > [!IMPORTANT]
   > Vždy používejte nejnovější verzi SSMS, která je dostupná na stránce pro [stažení SQL Server Management Studia](https://msdn.microsoft.com/library/mt238290.aspx). 
   >

2. V okně **Připojit k serveru** zadejte název serveru a ověřovací informace pro připojení pomocí ověřování SQL Serveru s účtem aaduser1@microsoft.com. 
    
   ![Připojení jako aaduser1@microsoft.com bez pravidla brány firewall&1;](./media/sql-database-control-access-aad-authentication-get-started/connect_aaduser1_no_rule1.png)

3. Klikněte na **Možnosti**, ke které se chcete připojit, a potom do rozevíracího seznamu **Připojit k databázi** na kartě **Vlastnosti připojení** zadejte **AdventureWorksLT**.
   
   ![Připojení jako aaduser1 bez pravidla brány firewall&2;](./media/sql-database-control-access-aad-authentication-get-started/connect_aaduser1_no_rule2.png)

4. Klikněte na **Připojit**. Zobrazí se dialogové okno oznamující, že počítač, ze kterého se pokoušíte připojit k SQL Database, nemá pravidlo brány firewall, které přístup k databázi povoluje. Dialogové okno, které se zobrazí, má dvě varianty. Závisí na krocích, které jste s branami firewall už provedli, ale obvykle se zobrazí první uvedené okno.

   ![Připojení jako user1 bez pravidla brány firewall&3;](./media/sql-database-control-access-aad-authentication-get-started/connect_aaduser1_no_rule3.png)

   ![Připojení jako user1 bez pravidla brány firewall&4;](./media/sql-database-control-access-aad-authentication-get-started/connect_aaduser1_no_rule4.png)

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

   ![Přidání pravidla brány firewall na úrovni databáze&4;](./media/sql-database-control-access-aad-authentication-get-started/aaduser1_add_rule_aw.png)

8. Znovu přejděte k druhému počítači a kliknutí na **Připojit** v dialogovém okně **Připojit k serveru** se připojte k AdventureWorksLT jako aaduser1. 

9. V Průzkumníku objektů postupně rozbalte **Databáze**, **AdventureWorksLT** a nakonec **Tabulky**. Všimněte si, že user1 má oprávnění k zobrazení jediné tabulky – **SalesLT.ProductCategory**. 

10. V Průzkumníku objektů klikněte pravým tlačítkem na **SalesLT.ProductCategory** a potom klikněte na **Select Top 1000 Rows** (Vybrat prvních 1000 řádků).   

## <a name="next-steps"></a>Další kroky
- Přehled řízení a přístupu pro SQL Database najdete v tématu věnovaném [řízení a přístupu k SQL Database](sql-database-control-access.md).
- Přehled přihlášení, uživatelů a databázových rolí ve službě SQL Database najdete v tématu věnovaném [přihlášením, uživatelům a databázovým rolím](sql-database-manage-logins.md).
- Další informace o objektech zabezpečení databáze najdete v tématu [Objekty zabezpečení](https://msdn.microsoft.com/library/ms181127.aspx).
- Další informace o databázových rolích najdete v tématu věnovaném [databázovým rolím](https://msdn.microsoft.com/library/ms189121.aspx).
- Další informace o pravidlech brány firewall pro SQL Database najdete v tématu [Pravidla brány firewall služby SQL Database](sql-database-firewall-configure.md).




<!--HONumber=Feb17_HO3-->


