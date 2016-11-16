---
title: "Kurz k SQL Database: Začínáme se zabezpečením"
description: "Naučte se, jak vytvářet uživatelské účty pro přístup k databázi a její správu."
keywords: 
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 67797b09-f5c3-4ec2-8494-fe18883edf7f
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/17/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: a3bd8ac5466ae67df0a1865509c8fb897d1374ec


---
# <a name="sql-database-tutorial-create-sql-database-user-accounts-to-access-and-manage-a-database"></a>Kurz k SQL Database: Vytvoření uživatelského účtu databáze SQL pro přístup k databázi a její správu
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

## <a name="next-steps"></a>Další kroky
Nyní jste dokončili kurz SQL Database – vytvořili jste uživatelský účet a přidělili mu oprávnění dbo. Jste tedy připraveni na další informace o [zabezpečení SQL Database](sql-database-manage-logins.md).




<!--HONumber=Nov16_HO2-->


