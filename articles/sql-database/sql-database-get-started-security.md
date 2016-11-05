---
title: 'Kurz k SQL Database: Začínáme se zabezpečením'
description: Naučte se, jak vytvářet uživatelské účty pro přístup k databázi a její správu.
keywords: ''
services: sql-database
documentationcenter: ''
author: CarlRabeler
manager: jhubbard
editor: ''

ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/17/2016
ms.author: carlrab

---
# Kurz k SQL Database: Vytvoření uživatelského účtu databáze SQL pro přístup k databázi a její správu
> [!div class="op_single_selector"]
> * [Kurz Začínáme](sql-database-get-started-security.md)
> * [Udělení přístupu](sql-database-manage-logins.md)
> 
> 

V tomto kurzu se naučíte používat SQL Server Management Studio (SSMS), když chcete:

* Přihlásit se k SQL Database pomocí hlavního přihlášení na úrovni serveru.
* Vytvořit uživatelský účet pro SQL Database.
* Udělit uživateli SQL Database [oprávnění db_owner](https://msdn.microsoft.com/library/ms189121.aspx#Anchor_0).
* Připojit se k SQL Database pomocí uživatelského účtu, který není hlavním přihlášením na úrovni serveru.

[!INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

[!INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]

[!INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]

[!INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]

[!INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]

## Další kroky
Nyní jste dokončili kurz SQL Database – vytvořili jste uživatelský účet a přidělili mu oprávnění dbo. Jste tedy připraveni na další informace o [zabezpečení SQL Database](sql-database-manage-logins.md).

<!--HONumber=Sep16_HO3-->


