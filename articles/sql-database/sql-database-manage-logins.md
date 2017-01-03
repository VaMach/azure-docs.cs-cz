---
title: "Ověřování a autorizace ve službě SQL Database: udělení přístupu | Dokumentace Microsoftu"
description: "Informace o správě zabezpečení služby SQL Database – postupy správy přístupu k databázím a zabezpečení přihlašování prostřednictvím hlavního účtu na úrovni serveru"
keywords: "zabezpečení databáze SQL,správa zabezpečení databáze,zabezpečení přihlášení,zabezpečení databáze,přístup k databázi"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 0a65a93f-d5dc-424b-a774-7ed62d996f8c
ms.service: sql-database
ms.custom: authentication and authorization
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 09/14/2016
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: 498ee06fedf986a1b7fb4e362c4b5cb688f6a685
ms.openlocfilehash: 37cce678e0a9dfb081719cee4be7a04a838c1012


---
# <a name="sql-database-authentication-and-authorization-granting-access"></a>Ověřování a autorizace ve službě SQL Database: udělení přístupu
> [!div class="op_single_selector"]
> * [Kurz Začínáme](sql-database-get-started-security.md)
> * [Udělení přístupu](sql-database-manage-logins.md)
> 
> 

Zde můžete získat počáteční přehled o konceptech správy přístupů ve službě SQL Database pro správce i pro uživatele bez oprávnění správce a o rolích.

## <a name="unrestricted-administrative-accounts"></a>Neomezené účty pro správu
Existují dva možné účty pro správu s neomezenými oprávněními pro přístup k virtuální hlavní databázi a všem uživatelským databázím. Tyto účty se označují jako hlavní účty na úrovni serveru.

### <a name="azure-sql-database-subscriber-account"></a>Účet předplatitele služby Azure SQL Database
Při vytvoření logické instance služby SQL se vytvoří účet s podporou jediného přihlášení, takzvaný účet předplatitele služby SQL Database. Tento účet používá pro připojení ověřování SQL Serveru (uživatelské jméno a heslo). Tento účet je správcem v logické instanci na serveru a ve všech uživatelských databázích připojených k této instanci. Oprávnění účtu předplatitele není možné omezit. Existovat může jenom jeden z těchto účtů.

### <a name="azure-active-directory-administrator"></a>Správce služby Azure Active Directory
Jeden účet služby Azure Active Directory je možné nakonfigurovat jako účet správce. Tímto účtem může být účet jednotlivého uživatele služby Azure Active Directory (AD) nebo skupina účtů služby Azure AD obsahující více uživatelů služby Azure AD. Nakonfigurování správce služby Azure AD je nepovinné, pokud ale chcete při připojování ke službě SQL Database používat Ověřování systému Windows pro účty služby Azure AD, musí být správce služby Azure AD nakonfigurován. Další informace o konfigurování přístupu v Azure Active Directory najdete v tématech [Připojení ke službě SQL Database nebo SQL Data Warehouse pomocí ověřování služby Azure Active Directory](sql-database-aad-authentication.md) a [Podpora nástroje SSMS pro ověřování Azure AD MFA ve službě SQL Database a SQL Data Warehouse](sql-database-ssms-mfa-authentication.md).

### <a name="configuring-the-firewall"></a>Konfigurace brány firewall
Pokud je nakonfigurovaná brána firewall na úrovni serveru, může se účet předplatitele služby Azure SQL Database a účet služby Azure Active Directory připojit k hlavní databázi a všem uživatelským databázím. Bránu firewall úrovni serveru můžete nakonfigurovat na portálu. Po vytvoření připojení můžete konfigurovat další pravidla brány firewall na úrovni serveru pomocí příkazu Transact-SQL [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx). Další informace o konfigurování pravidel brány firewall najdete v tématu [Postup konfigurace brány firewall služby Azure SQL Database pomocí portálu Azure Portal](sql-database-configure-firewall-settings.md).

### <a name="administrator-access-path"></a>Cesta pro přístup správce
Pokud je brána firewall na úrovni serveru správně nakonfigurovaná, můžou se účty předplatitelů služby SQL Database a správci SQL Serveru ve službě Azure Active Directory připojit pomocí klientských nástrojů, jako jsou SQL Server Management Studio nebo SQL Server Data Tools. Jenom nejnovější nástroje poskytují všechny funkce a možnosti. Následující diagram znázorňuje typickou konfiguraci pro dva účty správce.
    ![Cesta pro přístup správce](./media/sql-database-manage-logins/1sql-db-administrator-access.png)

Při použití otevřeného portu brány firewall na úrovni serveru se můžou správci připojit k jakékoli databázi služby SQL Database.

### <a name="connecting-to-a-database-by-using-sql-server-management-studio"></a>Připojení k databázi pomocí aplikace SQL Server Management Studio
Návod, jak se připojit pomocí aplikace SQL Server Management Studio, najdete v tématu [Připojení ke službě SQL Database pomocí aplikace SQL Server Management Studio a spuštění ukázkového dotazu T-SQL](sql-database-connect-query-ssms.md).

> [!IMPORTANT]
> Doporučujeme vám vždy používat nejnovější verzi aplikace Management Studio, aby se zajistila synchronizovanost s aktualizacemi Microsoft Azure a SQL Database. [Aktualizovat aplikaci SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

## <a name="additional-special-accounts"></a>Další zvláštní účty
Služba SQL Database poskytuje ve virtuální hlavní databázi dvě role pro správu s omezením přístupu, do kterých můžete přidávat uživatelské účty.

### <a name="database-creators"></a>Autoři databází
Účty pro správu umožňují vytvářet nové databáze. Pokud chcete vytvořit další účet, který bude moci vytvářet databáze, musíte vytvořit uživatele v hlavní databázi a přidat ho do zvláštní databázové role **dbmanager**. Tímto uživatelem může být uživatel databáze s omezením nebo uživatel s přihlášením SQL Serveru ve virtuální hlavní databázi.

1. Pomocí účtu správce se připojte k virtuální hlavní databázi.
2. Volitelný krok: Vytvořte přihlášení s ověřováním SQL Serveru pomocí příkazu [CREATE LOGIN](https://msdn.microsoft.com/library/ms189751.aspx). Ukázka příkazu:
   
   ```
   CREATE LOGIN Mary WITH PASSWORD = '<strong_password>';
   ```
   
   > [!NOTE]
   > Při vytváření přihlášení nebo uživatele databáze s omezením použijte silné heslo. Další informace najdete v tématu [Silná hesla](https://msdn.microsoft.com/library/ms161962.aspx).
   > 
   > 
   
   Za účelem zvýšení výkonu se přihlášení (u hlavních účtů na úrovni serveru) dočasně ukládají do mezipaměti na úrovni databáze. Pokud chcete aktualizovat mezipaměť pro ověřování, podívejte se na informace v tématu [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx).
3. Ve virtuální hlavní databázi vytvořte uživatele pomocí příkazu [CREATE USER](https://msdn.microsoft.com/library/ms173463.aspx). Tímto uživatelem může být uživatel databáze s omezením s ověřováním služby Azure Active Directory (pokud jste nakonfigurovali prostředí s ověřováním pomocí služby Azure AD) nebo uživatel databáze s omezením s ověřováním SQL Serveru nebo uživatel s ověřováním SQL Serveru založeným na přihlášení s ověřováním SQL Serveru (vytvořený v předchozím kroku). Ukázky příkazů:
   
   ```
   CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
   CREATE USER Tran WITH PASSWORD = '<strong_password>';
   CREATE USER Mary FROM LOGIN Mary; 
   ```
4. Do databázové role **dbmanager** přidejte nového uživatele pomocí příkazu [ALTER ROLE](https://msdn.microsoft.com/library/ms189775.aspx). Ukázky příkazů:
   
   ```
   ALTER ROLE dbmanager ADD MEMBER Mary; 
   ALTER ROLE dbmanager ADD MEMBER [mike@contoso.com];
   ```
   
   > [!NOTE]
   > Dbmanager je databázová role ve virtuální hlavní databázi, takže do role dbmanager můžete přidat pouze uživatele. Do role na úrovni databáze není možné přidat přihlášení na úrovni serveru.
   > 
   > 
5. V případě potřeby nakonfigurujte bránu firewall na úrovni serveru, aby se mohl nový uživatel připojit.

Nový uživatel se nyní může připojit k virtuální hlavní databázi a může vytvářet nové databáze. Účet použitý k vytvoření databáze se stává vlastníkem databáze.

### <a name="login-managers"></a>Správci přihlášení
Pokud chcete, můžete použít stejný postup (vytvořit přihlášení a uživatele a přidat uživatele do role **loginmanager**) a povolit tak uživateli vytvářet nová přihlášení ve virtuální hlavní databázi. To obvykle není nutné, protože Microsoft doporučuje používat uživatele databáze s omezením, kteří jsou ověřování na úrovni databáze, místo používání uživatelů s ověřováním na základě přihlášení. Další informace najdete v tématu [Uživatelé databáze s omezením – zajištění přenositelnosti databáze](https://msdn.microsoft.com/library/ff929188.aspx).

## <a name="non-administrator-users"></a>Uživatelé bez oprávnění správce
Obecně platí, že účty bez oprávnění správce nepotřebují přístup k virtuální hlavní databázi. Uživatele databáze s omezením můžete vytvářet pomocí příkazu [CREATE USER (Transact-SQL)](https://msdn.microsoft.com/library/ms173463.aspx). Tímto uživatelem může být uživatel databáze s omezením s ověřováním služby Azure Active Directory (pokud jste nakonfigurovali prostředí s ověřováním pomocí služby Azure AD) nebo uživatel databáze s omezením s ověřováním SQL Serveru nebo uživatel s ověřováním SQL Serveru založeným na přihlášení s ověřováním SQL Serveru (vytvořený v předchozím kroku). Další informace najdete v tématu [Uživatelé databáze s omezením – zajištění přenositelnosti databáze](https://msdn.microsoft.com/library/ff929188.aspx). 

Pokud chcete vytvářet uživatele, připojte se k databázi a spusťte podobné příkazy jako v následujících příkladech:

```
CREATE USER Mary FROM LOGIN Mary; 
CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
```

Na začátku může uživatele vytvořit jenom jeden ze správců nebo vlastník databáze. Jestliže chcete autorizovat další uživatele, aby mohli vytvářet nové uživatele, udělte vybraným uživatelům oprávnění `ALTER ANY USER` pomocí příkazu, jako je například tento:

```
GRANT ALTER ANY USER TO Mary;
```

Pokud chcete dalším uživatelům umožnit úplnou kontrolu databáze, přidejte je do pevné databázové role **db_owner** pomocí příkazu `ALTER ROLE`.

> [!NOTE]
> Hlavní důvod pro vytváření uživatelů databáze s ověřováním na základě přihlášení nastává v případě, že máte uživatele s ověřováním SQL Serveru, kteří potřebují přístup k více databázím. Uživatelé s ověřováním na základě přihlášení jsou vázáni s určitým přihlášením a pouze s jedním heslem, které se pro toto přihlášení udržuje. Uživatelé databází s omezením jsou v jednotlivých databázích jednotlivými entitami a pro každého se v každé databázi udržuje vlastní heslo. To může být pro uživatele databází s omezením matoucí, pokud neudržují stejná hesla.
> 
> 

### <a name="configuring-the-database-level-firewall"></a>Konfigurace brány firewall na úrovni databáze
Uživatelé bez oprávnění správce by v rámci osvědčených postupů měli mít do databází, které používají, přístup pouze přes bránu firewall. Místo toho, abyste autorizovali jejich IP adresy pomocí brány firewall na úrovni serveru a umožnili jim tak přístup do všech databází, nakonfigurujte bránu firewall na úrovni databáze pomocí příkazu [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx). Bránu firewall na úrovni databáze nemůžete nakonfigurovat pomocí portálu.

### <a name="non-administrator-access-path"></a>Cesta pro přístup uživatelů bez oprávnění správce
Pokud je brána firewall na úrovni databáze správně nakonfigurovaná, můžou se uživatelé připojit pomocí klientských nástrojů, jako jsou SQL Server Management Studio nebo SQL Server Data Tools. Jenom nejnovější nástroje poskytují všechny funkce a možnosti. Následující diagram znázorňuje typickou cestu pro přístup uživatelů bez oprávnění správce.
![Cesta pro přístup uživatelů bez oprávnění správce](./media/sql-database-manage-logins/2sql-db-nonadmin-access.png)

## <a name="groups-and-roles"></a>Skupiny a role
Efektivní správa přístupů využívá oprávnění přiřazená skupinám a rolím, nikoliv jednotlivým uživatelům. Pokud například používáte ověřování pomocí služby Azure Active Directory:

* Přidejte uživatele služby Azure Active Directory do skupiny Azure Active Directory. Pro tuto skupinu vytvořte uživatele databáze s omezením. Jednoho nebo více uživatelů databáze přidejte do databázové role. Pak přiřaďte oprávnění pro tuto databázovou roli.

Pokud používáte ověřování SQL Serveru:

* V databázi vytvořte uživatele databáze s omezením. Jednoho nebo více uživatelů databáze přidejte do databázové role. Pak přiřaďte oprávnění pro tuto databázovou roli.

Mezi databázové role patří například předdefinované role **db_owner**, **db_ddladmin**, **db_datawriter**, **db_datareader**, **db_denydatawriter** a **db_denydatareader**. Role **db_owner** se obvykle používá k udělení úplných oprávnění pouze několika uživatelům. Ostatní pevné databázové role jsou užitečné pro rychlé vytvoření jednoduché databáze ve vývojovém prostředí, ale nedoporučují se pro většinu databází v produkčním prostředí. Pevná databázová role **db_datareader** například uděluje přístup pro čtení pro všechny tabulky v databázi, což je obvykle více, než je skutečně nezbytné. Je mnohem lepší vytvořit vlastní databázové role definované uživatelem pomocí příkazu [CREATE ROLE](https://msdn.microsoft.com/library/ms187936.aspx) a každé roli pečlivě udělit nejnižší oprávnění, které jsou nezbytná pro práci. Pokud je uživatel členem více rolí, všechna jejich oprávnění se agregují.

## <a name="permissions"></a>Oprávnění
Ve službě SQL Database je dostupných více než 100 oprávnění, která můžete jednotlivě přidělit nebo zamítnout. Mnohá z těchto oprávnění jsou vnořená. Oprávnění `UPDATE` pro schéma například zahrnuje oprávnění `UPDATE` pro každou tabulku v tomto schématu. Podobně jako ve většině systémů oprávnění má zamítnutí oprávnění přednost před udělením oprávnění a přepíše ho. Kvůli velkému počtu oprávnění a používání vnořených oprávnění může návrh vhodného systému oprávnění vyžadovat pečlivou studii, aby byla vaše databáze dobře chráněna. Začněte seznamem oprávnění podle tématu [Oprávnění (databázový stroj)](https://msdn.microsoft.com/library/ms191291.aspx) a prohlédněte si [plakát](http://go.microsoft.com/fwlink/?LinkId=229142) s přehledem oprávnění.

## <a name="next-steps"></a>Další kroky
[Zabezpečení služby SQL Database](sql-database-security.md)

[Vytvoření tabulky \(kurz\)](https://msdn.microsoft.com/library/ms365315.aspx)

[Vložení a aktualizace dat v tabulce \(kurz\)](https://msdn.microsoft.com/library/ms365309.aspx)

[Čtení dat v tabulce \(kurz\)](https://msdn.microsoft.com/library/ms365310.aspx)

[Vytváření zobrazení a uložených procedur](https://msdn.microsoft.com/library/ms365311.aspx)

[Udělení přístupu k databázovému objektu](https://msdn.microsoft.com/library/ms365327.aspx)

## <a name="additional-resources"></a>Další zdroje
[Zabezpečení služby SQL Database](sql-database-security.md)

[Security Center pro databázový stroj SQL Server a Azure SQL Database](https://msdn.microsoft.com/library/bb510589.aspx) 




<!--HONumber=Dec16_HO3-->


