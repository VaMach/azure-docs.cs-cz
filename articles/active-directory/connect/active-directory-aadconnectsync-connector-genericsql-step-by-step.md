---
title: "Obecné konektor SQL krok za krokem | Microsoft Docs"
description: "Tento článek je proti prostřednictvím jednoduchý systém HR podrobné pomocí obecné konektoru SQL."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 28c1cc60-24fd-4d0d-a36d-b4aba6de86e7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 3fdc1b405b95180d031aa4ad45b406f7fc149d8f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="generic-sql-connector-step-by-step"></a>Podrobný postup pro obecný konektor SQL
Toto téma je podrobný průvodce. Vytvoří databáze HR jednoduchého ukázkového a použít jej pro import někteří uživatelé a jejich členství ve skupině.

## <a name="prepare-the-sample-database"></a>Příprava ukázkové databáze
Na serveru systémem SQL Server, spusťte skript SQL v nalezen [příloha A](#appendix-a). Tento skript vytvoří ukázkovou databázi s názvem GSQLDEMO. Objektový model pro vytvořené databáze vypadá na tomto obrázku:  
![Objektový Model](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/objectmodel.png)

Také vytvořte uživatele, které chcete použít pro připojení k databázi. V tomto návodu je uživatel názvem FABRIKAM\SQLUser a umístěné v doméně.

## <a name="create-the-odbc-connection-file"></a>Vytvoření souboru připojení rozhraní ODBC
Obecné konektor SQL je pomocí rozhraní ODBC pro připojení ke vzdálenému serveru. Nejprve musíme vytvořit soubor s informacemi o připojení ODBC.

1. Spusťte nástroj pro správu rozhraní ODBC na serveru:  
   ![ODBC](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc.png)
2. Vyberte kartu **souborové DSN**. Klikněte na tlačítko **přidat...** .  
   ![ODBC1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc1.png)
3. Funguje out-of-box ovladačů, přesně, takže vyberte ho a klikněte na tlačítko **Další >**.  
   ![ODBC2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc2.png)
4. Pojmenujte soubor, jako například **GenericSQL**.  
   ![ODBC3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc3.png)
5. Klikněte na **Dokončit**.  
   ![ODBC4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc4.png)
6. Čas ke konfiguraci připojení. Zadejte zdroj dat funkční popis a zadejte název serveru se systémem SQL Server.  
   ![ODBC5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc5.png)
7. Vyberte způsob ověřování SQL. V tomto případě používáme ověřování systému Windows.  
   ![ODBC6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc6.png)
8. Zadejte název ukázkové databáze **GSQLDEMO**.  
   ![ODBC7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc7.png)
9. Udržování všechno výchozí na této obrazovce. Klikněte na **Dokončit**.  
   ![ODBC8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc8.png)
10. Chcete-li ověřit, všechno funguje podle očekávání, klikněte na tlačítko **Test zdroje dat**.  
    ![ODBC9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc9.png)
11. Ujistěte se, že test je úspěšné.  
    ![ODBC10](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc10.png)
12. Nyní měli být viditelné v souborové DSN ODBC konfigurační soubor.  
    ![ODBC11](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc11.png)

Nyní je k dispozici soubor budeme potřebovat a můžete začít vytvářet konektor.

## <a name="create-the-generic-sql-connector"></a>Vytvořit konektor obecné SQL
1. V uživatelském rozhraní Synchronization Service Manager vyberte **konektory** a **vytvořit**. Vyberte **obecné SQL (Microsoft)** a pojmenujte ho popisný název.  
   ![Connector1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector1.png)
2. Vyhledejte soubor DSN, který jste vytvořili v předchozí části a nahrajte na server. Zadejte pověření pro připojení k databázi.  
   ![Connector2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector2.png)
3. V tomto návodu budeme jsou a usnadnit tak nám a Řekněme, že existují dva typy objektů, **uživatele** a **skupiny**.
   ![Connector3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector3.png)
4. Pro vyhledání atributů, chceme konektor ke zjištění těchto atributů prohlížením samotné tabulky. Vzhledem k tomu **uživatelé** je vyhrazené slovo v SQL, je potřeba zadat v hranaté závorky [].  
   ![Connector4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector4.png)
5. Čas k definování ukotvení atribut a atribut rozlišující název. Pro **uživatelé**, používáme kombinace uživatelského jména dva atributy a EmployeeID. Pro **skupiny**, používáme GroupName (ne realistické ve skutečném, ale pro tento postup funguje).
   ![Connector5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector5.png)
6. Ne všechny typy atributů lze zjistit v databázi SQL. Typ atributu odkaz na konkrétní nelze. Typ objektu skupiny je nutné změnit ID vlastníka a MemberID, chcete-li.  
   ![Connector6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector6.png)
7. Atributy, které jsme vybrali jako atributy typu odkaz v předchozím kroku vyžaduje typ objektu tyto hodnoty jsou odkaz na. V našem případě typ objektu uživatele.  
   ![Connector7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector7.png)
8. Na stránce globální parametry vyberte **vodoznak** jako strategie delta. Také zadejte ve formátu data a času **rrrr MM-dd hh: mm:**.
   ![Connector8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector8.png)
9. Na **konfigurace oddílů a hierarchií** vyberte oba typy objektů.
   ![Connector9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector9.png)
10. Na **vyberte typy objektů** a **vybrat atributy**, vyberte typy objektů a všech atributů. Na **konfigurace kotvy** klikněte na tlačítko **Dokončit**.

## <a name="create-run-profiles"></a>Vytvoření profilů spuštění
1. V uživatelském rozhraní Synchronization Service Manager vyberte **konektory**, a **konfigurovat profily spuštění**. Klikněte na tlačítko **nový profil**. Začneme s **úplný Import**.  
   ![Runprofile1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile1.png)
2. Vyberte typ **úplný Import (jenom fázi)**.  
   ![Runprofile2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile2.png)
3. Vyberte oddíl, **OBJEKT = uživatele**.  
   ![Runprofile3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile3.png)
4. Vyberte **tabulky** a typ **[uživatelé]**. Přejděte do části Typ objektu s více hodnotami a zadejte data jako na následujícím obrázku. Vyberte **Dokončit** uložit v kroku.  
   ![Runprofile4a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4a.png)  
   ![Runprofile4b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4b.png)  
5. Vyberte **nový krok**. Tentokrát vyberte **OBJEKT = skupiny**. Na poslední stránce použijte konfiguraci jako na následujícím obrázku. Klikněte na **Dokončit**.  
   ![Runprofile5a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5a.png)  
   ![Runprofile5b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5b.png)  
6. Volitelné: Pokud chcete, můžete nakonfigurovat další profilů spuštění. V tomto návodu se používá pouze úplný Import.
7. Klikněte na tlačítko **OK** pro dokončení změny profilů spuštění.

## <a name="add-some-test-data-and-test-the-import"></a>Přidat některé testovacích dat a testování importu
Vyplňte nějaká testovací data v ukázkové databázi. Až budete připravení, vyberte **spustit** a **úplný import**.

Zde je uživatel s dvě telefonní čísla a skupiny se některé členy.  
![cs1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs1.png)  
![CS2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs2.png)  

## <a name="appendix-a"></a>Příloha A
**Skript SQL k vytvoření ukázkové databáze**

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
