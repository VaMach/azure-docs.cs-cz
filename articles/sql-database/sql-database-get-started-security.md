<properties
    pageTitle="Kurz k SQL Database: Začínáme se zabezpečením"
    description="Naučte se, jak vytvářet uživatelské účty pro přístup k databázi a její správu."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/19/2016"
    ms.author="carlrab"/>

# Kurz k SQL Database: Vytvoření uživatelského účtu databáze SQL pro přístup k databázi a její správu pomocí portálu Azure


> [AZURE.SELECTOR]
- [Kurz Začínáme](sql-database-get-started-security.md)
- [Udělení přístupu](sql-database-manage-logins.md)

V tomto kurzu se naučíte, jak pomocí Portálu Azure:

- použít hlavní přihlášení na úrovni serveru k přihlášení k SQL Database,
- vytvořit uživatelský účet pro SQL Database,
- přidělit uživatelskému účtu SQL Database oprávnění dbo pro uživatelskou databázi,
- provést připojení k databázi SQL pomocí uživatelského účtu, který není hlavním přihlášením na úrovni serveru. 

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]


[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]


## Další kroky
Nyní jste dokončili kurz SQL Database – vytvořili jste uživatelský účet a přidělili mu oprávnění dbo. Jste tedy připraveni na další informace o [zabezpečení SQL Database](sql-database-manage-logins.md).





<!---HONumber=Aug16_HO4-->


