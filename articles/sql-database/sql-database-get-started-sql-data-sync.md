---
title: "Začínáme s Azure synchronizaci dat SQL (Preview) | Microsoft Docs"
description: "Tento kurz vám pomůže začít pracovat s synchronizaci dat SQL Azure (Preview)."
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: a295a768-7ff2-4a86-a253-0090281c8efa
ms.service: sql-database
ms.custom: load & move data
ms.workload: Active
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: douglasl
ms.reviewer: douglasl
ms.openlocfilehash: 64b95142eb5b7086b070585a359bf81e7397c3e5
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="getting-started-with-azure-sql-data-sync-preview"></a>Začínáme s synchronizaci dat Azure SQL (Preview)
V tomto kurzu zjistěte, jak nastavit synchronizaci dat SQL Azure tak, že vytvoříte skupinu hybridních synchronizace, která obsahuje instance Azure SQL Database a SQL Server. Do nové skupiny synchronizace plně konfigurována a synchronizuje podle plánu, který nastavíte.

Tento kurz předpokládá, že máte alespoň zkušenosti s SQL Database a SQL Server. 

Přehled synchronizaci dat SQL najdete v tématu [synchronizaci dat](sql-database-sync-data.md).

Pro dokončení příklady prostředí PowerShell, které ukazují, jak nakonfigurovat synchronizaci dat SQL, najdete v následujících článcích:
-   [Pomocí prostředí PowerShell k synchronizaci mezi více databází Azure SQL](scripts/sql-database-sync-data-between-sql-databases.md)
-   [Synchronizace mezi databáze SQL Azure a místní databáze SQL serveru pomocí prostředí PowerShell](scripts/sql-database-sync-data-between-azure-onprem.md)

## <a name="step-1---create-sync-group"></a>Krok 1 – Vytvoření skupiny synchronizace

### <a name="locate-the-data-sync-settings"></a>Najít nastavení synchronizace dat

1.  V prohlížeči přejděte na portál Azure.

2.  Na portálu vyhledejte vaší databáze SQL z řídicího panelu nebo na ikonu databází SQL na panelu nástrojů.

    ![Seznam databází Azure SQL](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3.  Na **databází SQL** okně vyberte existující databázi SQL, který chcete použít jako databázi rozbočovače pro synchronizaci dat. Otevře se okno databáze SQL.

4.  V okně databáze SQL pro vybranou databázi, vyberte **synchronizace do jiné databáze**. Otevře se okno synchronizaci dat.

    ![Synchronizaci další možnost databáze](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a>Vytvořit novou skupinu synchronizace

1.  V okně synchronizaci dat vyberte **nové synchronizace skupiny**. **Nové synchronizace skupiny** otevře se okno s krokem 1 **vytvořit skupinu synchronizace**, zvýrazněné. **Vytvořit skupinu synchronizace dat** také otevře se okno.

2.  Na **vytvořit skupinu synchronizace dat** okně provádět následující akce:

    1.  V **název skupiny synchronizace** pole, zadejte název nové skupiny synchronizace.

    2.  V **Metadata databáze Sync** zvolte, zda chcete vytvořit novou databázi (doporučeno) nebo použijte existující databázi.

        > [!NOTE]
        > Společnost Microsoft doporučuje, že můžete vytvořit nový, prázdný databázi chcete použít jako databázi synchronizace metadat. Synchronizaci dat vytváří tabulky v této databázi a spustí Časté úlohy. Tato databáze je automaticky sdílen jako databázi Metadata synchronizace pro všechny skupiny synchronizace ve vybrané oblasti. Metadata databáze Sync nebo jeho název nelze změnit bez vyřadíte.

        Pokud jste zvolili **novou databázi**, vyberte **vytvořit novou databázi.** **SQL Database** otevře se okno. Na **SQL Database** okno, název a nakonfigurujte novou databázi. Potom vyberte **OK**.

        Pokud jste zvolili **použít existující databázi**, vyberte databázi ze seznamu.

    3.  V **automatické synchronizace** část, vyberte nejdřív **na** nebo **vypnout**.

        Pokud jste zvolili **na**v **frekvence synchronizace** , zadejte číslo a vyberte sekund, minut, hodin nebo dnů.

        ![Zadejte četnost synchronizace](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

    4.  V **řešení konfliktů** vyberte "Rozbočovače wins" nebo "Člen wins."

        ![Zadejte způsob řešení konfliktů](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

    5.  Vyberte **OK** a počkejte na novou skupinu synchronizace vytvoření a nasazení.

## <a name="step-2---add-sync-members"></a>Krok 2 – přidání členů synchronizace

Po nové synchronizace skupiny se vytvoří a nasadí, krok 2, **přidat členy synchronizace**, je označený na **nové synchronizace skupiny** okno.

V **rozbočovače databáze** zadejte existující přihlašovací údaje pro databázi SQL server, na kterém je umístěna databáze rozbočovače. Nezadávejte *nové* přihlašovacích údajů v této části.

![Rozbočovače databáze byl přidán do skupiny synchronizace](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

## <a name="add-an-azure-sql-database"></a>Přidat k Azure SQL Database

V **databázi člena** část, volitelně přidejte databáze SQL Azure do skupiny synchronizace výběrem **přidat databázi Azure**. **Konfigurace databáze Azure** otevře se okno.

Na **konfigurace databáze Azure** okně provádět následující akce:

1.  V **název člena synchronizace** pole, zadejte název nového člena synchronizace. Tento název se liší od názvu samotná databáze.

2.  V **předplatné** pole, vyberte přidružené předplatné Azure pro účely fakturace.

3.  V **Azure SQL Server** pole, vyberte existující databázový server SQL.

4.  V **Azure SQL Database** pole, vyberte existující databázi SQL.

5.  V **synchronizace pokynů** pole, vyberte obousměrné synchronizace, do rozbočovače nebo z rozbočovače.

    ![Přidání nového člena synchronizace databáze SQL](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6.  V **uživatelské jméno** a **heslo** pole, zadejte existující přihlašovací údaje pro databázi SQL server, na kterém je umístěna databáze člena. Nezadávejte *nové* přihlašovacích údajů v této části.

7.  Vyberte **OK** a počkejte na vytvoření a nasazení nového člena synchronizace.

    ![Byl přidán nový člen synchronizace databáze SQL](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

## <a name="add-an-on-premises-sql-server-database"></a>Přidat místní databázi systému SQL Server

V **databázi člena** část, volitelně přidat místní SQL Server do skupiny synchronizace výběrem **přidat místní databázi**. **Nakonfigurovat místní** otevře se okno.

Na **nakonfigurovat místní** okně provádět následující akce:

1.  Vyberte **zvolte bránu agenta synchronizace**. **Vyberte agenta synchronizace** otevře se okno.

    ![Zvolte bránu agenta synchronizace](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2.  Na **zvolte bránu agenta synchronizace** okně vyberte, zda chcete použít existující agenta nebo vytvořit nový agent.

    Pokud jste zvolili **existující agenty**, vyberte existující agenta ze seznamu.

    Pokud jste zvolili **vytvořit nový agent**, provádět následující akce:

    1.  Stáhněte si klientský software agenta synchronizace z uvedeného odkazu a nainstalujte ji na počítači, kde je umístěný SQL Server.
 
        > [!IMPORTANT]
        > Budete muset otevřít odchozí port TCP 1433 v bráně firewall umožníte komunikaci se serverem pro agenta klienta.


    2.  Zadejte název pro agenta.

    3.  Vyberte **vytvořit a vygenerovat klíč**.

    4.  Klíč agenta zkopírujte do schránky.
        
        ![Vytvoření nového agenta synchronizace](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

    5.  Vyberte **OK** zavřete **vyberte agenta synchronizace** okno.

    6.  Na počítači serveru SQL najít a spuštění aplikace synchronizace agenta klienta.

        ![Data synchronizovat klientskou aplikaci pro agenta](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    7.  V aplikaci agenta synchronizace, vyberte **odeslání klíč agenta**. **Konfigurace databáze synchronizace metadat** otevře se dialogové okno.

    8.  V **konfigurace databáze synchronizace metadat** dialogové okno, vložte klíč agenta zkopírovaných z portálu Azure. Zadejte také existující přihlašovací údaje pro server Azure SQL Database, na kterém je umístěna databáze metadat. (Pokud jste vytvořili novou databázi metadat, na stejném serveru jako databázi centra je tato databáze.) Vyberte **OK** a počkejte na dokončení konfigurace.

        ![Zadejte přihlašovací údaje pro klíč a server agenta](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        >   [!NOTE] 
        >   Pokud dojde k chybě brány firewall v tomto okamžiku, budete muset vytvořit pravidlo brány firewall na platformě Azure, které chcete povolit příchozí přenosy z počítače systému SQL Server. Pravidlo můžete vytvořit ručně na portálu, ale může být snadněji vytvořit v serveru SQL Server Management Studio (SSMS). V aplikaci SSMS pokuste se připojit k databázi rozbočovače na Azure. Zadejte jeho název jako \<hub_database_name\>. database.windows.net. Postupujte podle kroků v dialogovém okně nakonfigurovat pravidlo brány firewall Azure. Pak se vraťte do agenta synchronizace klienta aplikace.

    9.  V aplikaci agenta synchronizace klienta, klikněte na tlačítko **zaregistrovat** k registraci do databáze SQL serveru s agentem. **Konfigurace serveru SQL Server** otevře se dialogové okno.

        ![Přidejte a nakonfigurujte databázi systému SQL Server](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    10. V **konfigurace serveru SQL Server** dialogovém okně vyberte, jestli se mají připojit pomocí ověřování systému SQL Server nebo ověřování systému Windows. Pokud jste vybrali ověřování SQL serveru, zadejte existující přihlašovací údaje. Zadejte název serveru SQL Server a název databáze, který chcete synchronizovat. Vyberte **testovací připojení** můžete zkontrolovat nastavení. Potom vyberte **Uložit**. V seznamu se zobrazí registrované databáze.

        ![Databáze systému SQL Server je teď zaregistrovaný.](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    11. Teď můžete zavřít aplikaci synchronizace agenta klienta.

    12. Na portálu na **nakonfigurovat místní** vyberte **vyberte databázi.** **Vybrat databázi** otevře se okno.

    13. Na **vybrat databázi** okno v **název člena synchronizace** pole, zadejte název nového člena synchronizace. Tento název se liší od názvu samotná databáze. Vyberte databázi ze seznamu. V **synchronizace pokynů** pole, vyberte obousměrné synchronizace, do rozbočovače nebo z rozbočovače.

        ![Vyberte v místní databázi](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

    14. Vyberte **OK** zavřete **vybrat databázi** okno. Potom vyberte **OK** zavřete **nakonfigurovat místní** okno a počkejte na vytvoření a nasazení nového člena synchronizace. Nakonec klikněte na **OK** zavřete **vyberte synchronizace členů** okno.

        ![V místní databázi přidán do skupiny synchronizace](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3.  Pro připojení k synchronizaci dat SQL a místní agent, přidejte uživatelské jméno k roli `DataSync_Executor`. Synchronizaci dat vytvoří tuto roli na instanci systému SQL Server.

## <a name="step-3---configure-sync-group"></a>Krok 3: Konfigurace skupiny synchronizace

Po nové členy skupiny synchronizace se vytváří a nasazují, krok 3 **skupiny synchronizace konfigurace**, je označený na **nové synchronizace skupiny** okno.

1.  Na **tabulky** okně, vyberte databázi ze seznamu synchronizačních členy skupiny a potom vyberte **aktualizace schématu**.

2.  Ze seznamu dostupných tabulek vyberte tabulky, které chcete synchronizovat.

    ![Vyberte tabulky, které chcete synchronizovat](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3.  Ve výchozím nastavení jsou vybrány všechny sloupce v tabulce. Pokud nechcete, aby k synchronizaci všech sloupců, zakažte políčko pro sloupce, které nechcete synchronizovat. Ujistěte se, že nechte vybraný sloupec primárního klíče.

    ![Vyberte pole pro synchronizaci](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4.  Nakonec vyberte **Uložit**.

## <a name="next-steps"></a>Další kroky
Blahopřejeme. Vytvořili jste skupinu synchronizace, která obsahuje instance databáze SQL a databáze SQL serveru.

Další informace o SQL Database a synchronizaci dat SQL najdete v tématu:

-   [Stáhněte si dokumentaci rozhraní API REST synchronizaci dat SQL](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)
-   [Databáze SQL – přehled](sql-database-technical-overview.md)
-   [Správa životního cyklu databáze](https://msdn.microsoft.com/library/jj907294.aspx)
