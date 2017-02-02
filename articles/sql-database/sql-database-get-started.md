---
title: "Azure Portal: Začínáme s Azure SQL Database | Dokumentace Microsoftu"
description: "Naučíte se vytvářet logický server SQL Database, pravidlo brány firewall na úrovni serveru a databáze pomocí webu Azure Portal. Dál se naučíte k dotazování využívat aplikaci SQL Server Management Studio."
keywords: "kurz k sql database, vytvoření databáze sql"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: single databases
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/23/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2a85b3dc1078bad9e5e2fc0ce0bec7e994b29150
ms.openlocfilehash: 6da0bb371336e0d9662a7fd36187cdf4317c22ea


---
# <a name="sql-database-tutorial-get-started-with-azure-sql-database-servers-databases-and-firewall-rules-using-the-azure-portal-and-sql-server-management-studio"></a>Kurz k SQL Database: Začínáme se servery, databázemi a pravidly brány firewall služby Azure SQL Database s využitím webu Azure Portal a aplikace SQL Server Management Studio

V tomto úvodním kurzu se naučíte, jak pomocí portálu Azure Portal provádět tyto akce:

* Vytvoření nové skupiny prostředků Azure
* Vytvoření logického SQL serveru Azure
* Zobrazení vlastností logického SQL serveru Azure
* Vytvoření pravidla brány firewall na úrovni serveru
* Vytvoření ukázkové databáze Adventure Works LT jako izolované databáze
* Zobrazení vlastností ukázkové databáze Adventure Works LT v Azure

V tomto kurzu můžete také využít nejnovější verzi aplikace SQL Server Management Studio pro:

* Připojení k logickému serveru a jeho hlavní databázi
* Dotaz na hlavní databázi
* Připojení k ukázkové databázi
* Dotaz na ukázkovou databázi

Po dokončení tohoto kurzu budete mít ukázkovou databázi a prázdnou databázi, která je spuštěná ve skupině prostředků Azure a připojená k logickému serveru. Dál budete mít nakonfigurované pravidlo brány firewall na úrovni serveru, které objektu zabezpečení na úrovni serveru umožňuje přihlášení k serveru ze zadaná IP adresy (nebo rozsahu IP adres). 

**Časový odhad**: Tento kurz trvá přibližně 30 minut (za předpokladu, že už máte splněné požadavky).

> [!TIP]
> Stejné úlohy můžete v úvodním kurzu provádět i pomocí jazyka [C#](sql-database-get-started-csharp.md) nebo prostředí [PowerShell](sql-database-get-started-powershell.md).
>

## <a name="prerequisites"></a>Požadavky

* Potřebujete mít účet Azure. Můžete si [zdarma otevřít účet Azure](/pricing/free-trial/?WT.mc_id=A261C142F) nebo [aktivovat výhody pro předplatitele sady Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* Musíte být schopni připojit se k webu Azure Portal pomocí účtu, který je členem role přispěvatele nebo vlastníka předplatného. Další informace o řízení přístupu na základě role (RBAC) najdete v tématu [Začínáme se správou přístupu na webu Azure Portal](../active-directory/role-based-access-control-what-is.md).

> [!NOTE]
> Tento kurz vám pomůže seznámit se s obsahem těchto výukových okruhů: [Přehled serveru SQL Database](sql-database-server-overview.md), [přehled SQL Database](sql-database-overview.md) a [přehled pravidel brány firewall pro Azure SQL Database](sql-database-firewall-configure.md).
>  


### <a name="sign-in-to-the-azure-portal-using-your-azure-account"></a>Přihlášení k webu Azure Portal pomocí účtu Azure
Pomocí [stávajícího předplatného](https://account.windowsazure.com/Home/Index) se k webu Azure Portal připojíte následujícím postupem.

1. Otevřete prohlížeč, kterému dáváte přednost, a připojte se k [portálu Azure](https://portal.azure.com/).
2. Přihlaste se k webu [Portál Azure](https://portal.azure.com/).
3. Na **přihlašovací stránce** zadejte přihlašovací údaje ke svému předplatnému.
   
   ![Přihlášení](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="create-a-new-logical-sql-server-in-the-azure-portal"></a>Vytvoření nového logického SQL serveru na webu Azure Portal

1. Klikněte na **Nový**, zadejte **sql server** a potom klikněte na **ENTER**.

    ![Logický SQL server](./media/sql-database-get-started/logical-sql-server.png)
2. Klikněte na **SQL Server (logický server)**.
   
    ![Vytvoření logického SQL serveru](./media/sql-database-get-started/create-logical-sql-server.png)
3. Kliknutím na **Vytvořit** otevřete nové okno SQL Server (logický server).

    ![Nový logický SQL server](./media/sql-database-get-started/new-logical-sql-server.png)
3. Do textového pole Název serveru zadejte platný název pro nový logický server. Zelená značka zaškrtnutí označuje, že jste zadali platný název.
    
    ![Název nového serveru](./media/sql-database-get-started/new-server-name.png)

    > [!IMPORTANT]
    > Plně kvalifikovaný název nového serveru bude <název_serveru>.database.windows.net.
    >
    
4. Do textového pole Přihlášení správce serveru zadejte uživatelské jméno pro 	účet ověřování SQL pro tento server. Toto přihlášení se označuje jako přihlášení objektu zabezpečení serveru. Zelená značka zaškrtnutí označuje, že jste zadali platný název.
    
    ![Přihlášení správce SQL](./media/sql-database-get-started/sql-admin-login.png)
5. Do textových polí **Heslo** a **Potvrzení hesla** zadejte heslo pro přihlašovací účet objektu zabezpečení serveru. Zelená značka zaškrtnutí označuje, že jste zadali platné heslo.
    
    ![Heslo správce SQL](./media/sql-database-get-started/sql-admin-password.png)
6. Vyberte předplatné, ve kterém máte oprávnění k vytváření objektů.

    ![předplatné](./media/sql-database-get-started/subscription.png)
7. V textovém poli Skupina prostředků vyberte **Vytvořit nový** a potom do textového pole skupiny prostředků zadejte platný název pro novou skupinu prostředků (můžete také použít existující skupinu prostředků, pokud jste si už nějakou vytvořili). Zelená značka zaškrtnutí označuje, že jste zadali platný název.

    ![Nová skupina prostředků](./media/sql-database-get-started/new-resource-group.png)

8. V textovém poli **Umístění** vyberte datacentrum odpovídající vašemu umístění, třeba Austrálie – východ.
    
    ![Umístění serveru](./media/sql-database-get-started/server-location.png)
    
    > [!TIP]
    > V tomto okně nejde měnit zaškrtnutí políčka **Povolit službám Azure přístup k serveru**. Toto nastavení můžete změnit v okně brány firewall serveru. Další informace najdete v článku [Začínáme se zabezpečením](sql-database-control-access-sql-authentication-get-started.md).
    >
    
9. Klikněte na možnost **Vytvořit**.

    ![Tlačítko Vytvořit](./media/sql-database-get-started/create.png)

## <a name="view-the-logical-sql-server-properties-in-the-azure-portal"></a>Zobrazení vlastností logického SQL serveru na webu Azure Portal

1. Na webu Azure Portal klikněte na **Další služby**.

    ![Další služby](./media/sql-database-get-started/more-services.png)
2. Do textového pole Filtr zadejte **SQL** a potom u serverů SQL klikněte na hvězdičku. Nastavíte tak servery SQL v Azure jako oblíbené položky. 

    ![nastavení oblíbených položek](./media/sql-database-get-started/favorite.png)
3. Ve výchozím okně klikněte na **SQL servery**. Otevře se seznam serverů SQL ve vašem předplatném Azure. 

    ![Nový SQL Server](./media/sql-database-get-started/new-sql-server.png)

4. Kliknutím na nový SQL server zobrazíte jeho vlastnosti na webu Azure Portal. Další kurzy vám pomohou se s možnostmi, které jsou v tomto okně dostupné, seznámit blíž.

    ![Okno SQL Server](./media/sql-database-get-started/sql-server-blade.png)
5. V části Nastavení klikněte na tlačítko **Vlastnosti**. Zobrazí se různé vlastnosti logického SQL serveru.

    ![Vlastnosti sql serveru](./media/sql-database-get-started/sql-server-properties.png)
6. Zkopírujte si do schránky celý plně kvalifikovaný název serveru. Použijete ho v tomto kurzu o chvíli později.

    ![Celý název sql serveru](./media/sql-database-get-started/sql-server-full-name.png)

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Vytvoření pravidla brány firewall na úrovni serveru na webu Azure Portal

1. V okně SQL Server v části Nastavení klikněte na **Brána firewall**. Otevře se okno Brána firewall pro SQL Server.

    ![Brána firewall SQL Serveru](./media/sql-database-get-started/sql-server-firewall.png)

2. Zkontrolujte zobrazenou IP adresu klienta a pomocí prohlížeče podle vašeho výběru ověřte, že je to vaše IP adresa na internetu (zadejte dotaz Jaká je moje IP adresa). Čas od času si adresy z různých důvodů neodpovídají.

    ![Vaše IP adresa](./media/sql-database-get-started/your-ip-address.png)

3. Za předpokladu, že se IP adresy shodují, klikněte na panelu nástrojů na **Přidat IP adresu klienta**.

    ![Přidat IP adresu klienta](./media/sql-database-get-started/add-client-ip.png)

    > [!NOTE]
    > Bránu firewall služby SQL Database na serveru můžete otevřít pro jednu IP adresu nebo pro celý rozsah adres. Otevření brány firewall umožňuje uživatelům a správcům SQL přihlásit se k jakékoli databázi na serveru, ke kterému mají platné přihlašovací údaje.
    >

4. Kliknutím na **Uložit** na panelu nástrojů uložte toto pravidlo brány firewall na úrovni serveru a potom klikněte na **OK**.

    ![Přidat IP adresu klienta](./media/sql-database-get-started/save-firewall-rule.png)

## <a name="connect-to-sql-server-using-sql-server-management-studio-ssms"></a>Připojení k SQL serveru pomocí aplikace SQL Server Management Studio (SSMS)

1. Pokud jste to ještě neudělali, stáhněte si nejnovější verzi SMSS v části [Stažení aplikace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) a nainstalujte ji. Když bude k dispozici pro stažení novější verze, aplikace SSMS zobrazí upozornění, abyste mohli používat aktuální verzi.

2. Po instalaci zadejte do vyhledávacího pole ve Windows text **Microsoft SQL Server Management Studio** a klikněte na **Enter**. SSMS se otevře:

    ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)
3. V dialogovém okně Připojení k serveru zadejte informace potřebné k připojení k SQL serveru pomocí ověřování SQL Serveru.

    ![Připojení k serveru](./media/sql-database-get-started/connect-to-server.png)
4. Klikněte na **Připojit**.

    ![Připojeno k serveru](./media/sql-database-get-started/connected-to-server.png)
5. V Průzkumníku objektů postupně rozbalte **Databáze**, **Systémové databáze** a **hlavní**. Zobrazí se objekty v hlavní databázi.

    ![hlavní databáze](./media/sql-database-get-started/master-database.png)
6. Klikněte pravým tlačítkem na **hlavní** a potom klikněte na **Nový dotaz**.

    ![Dotaz na hlavní databázi](./media/sql-database-get-started/query-master-database.png)

8. Do okna dotazu zadejte následující dotaz:

   ```select * from sys.objects```

9.  Na panelu nástrojů klikněte na **Spustit**. Zobrazí se seznam všech systémových objektů v hlavní databázi.

    ![Dotaz na systémové objekty hlavní databáze](./media/sql-database-get-started/query-master-database-system-objects.png)

    > [!NOTE]
    > Pokud chcete prozkoumat zabezpečení SQL, přečtěte se téma [Začínáme se zabezpečením SQL](sql-database-control-access-sql-authentication-get-started.md).
    >

## <a name="create-new-database-in-the-azure-portal-using-adventure-works-lt-sample"></a>Vytvoření nové databáze na webu Azure Portal pomocí ukázky Adventure Works LT

1. Na webu Azure Portal klikněte na **Databáze SQL** ve výchozím okně.

    ![Databáze SQL](./media/sql-database-get-started/new-sql-database.png)
2. V okně Databáze SQL klikněte na **Přidat**.

    ![Přidání databáze SQL](./media/sql-database-get-started/add-sql-database.png)
3. V okně Databáze SQL zkontrolujte informace, které se dokončily za vás.

    ![Okno Databáze SQL](./media/sql-database-get-started/sql-database-blade.png)
4. Zadejte platný název databáze.

    ![Název databáze SQL](./media/sql-database-get-started/sql-database-name.png)
5. V části Vybrat zdroj klikněte na **Ukázka** a potom v části Vybrat ukázku klikněte na **AdventureWorksLT [V12]**.
   
    ![adventure works lt](./media/sql-database-get-started/adventureworkslt.png)
6. V části Server zadejte přihlašovací jméno a heslo správce serveru.

    ![Přihlašovací údaje serveru](./media/sql-database-get-started/server-credentials.png)

    > [!NOTE]
    > Databázi je možné na server přidat jako izolovanou databázi (to je výchozí nastavení), nebo do elastického fondu. Další informace o elastických fondech najdete v části věnované [elastickým fondům](sql-database-elastic-pool.md).
    >

7. V části Cenová úroveň změňte cenovou úroveň na **Basic**. (Cenovou úroveň můžete zvýšit v případě potřeby později, ale pro účely výuky doporučujeme využít nejnižší úroveň.)

    ![Cenová úroveň](./media/sql-database-get-started/pricing-tier.png)
8. Klikněte na možnost **Vytvořit**.

    ![Tlačítko Vytvořit](./media/sql-database-get-started/create.png)

## <a name="view-database-properties-in-the-azure-portal"></a>Zobrazení vlastností databáze na webu Azure Portal

1. V okně Databáze SQL klikněte na novou databázi. Zobrazíte tak její vlastnosti na webu Azure Portal. Další kurzy vám pomohou se s možnostmi, které jsou v tomto okně dostupné, seznámit blíž. 

    ![Nové okno ukázkové databáze](./media/sql-database-get-started/new-sample-db-blade.png)
2. Kliknutím na **Vlastnosti** zobrazíte další informace o databázi.

    ![Vlastnosti nové ukázkové databáze](./media/sql-database-get-started/new-sample-db-properties.png)

3. Klikněte na tlačítko **Zobrazit databázové připojovací řetězce**.

    ![Připojovací řetězce nové ukázkové databáze](./media/sql-database-get-started/new-sample-db-connection-strings.png)
4. Klikněte na **Přehled** a potom klikněte na název serveru v podokně Essentials.
    
    ![Podokno Essentials nové ukázkové databáze](./media/sql-database-get-started/new-sample-db-essentials-pane.png)
5. V podokně Essentials pro váš server se zobrazuje nově přidaná databáze.

    ![Nová ukázková databáze v okně Essentials](./media/sql-database-get-started/new-sample-db-server-essentials-pane.png)

## <a name="connect-and-query-sample-database-using-sql-server-management-studio"></a>Připojení ukázkové databáze a zpracování dotazů pomocí aplikace SQL Server Management Studio

1. Přepněte do aplikace SQL Server Management Studio a v Průzkumníku objektů klikněte na **Databáze**. Potom na panelu nástrojů klikněte na **Obnovit**. Zobrazí se ukázková databáze.

    ![Nová ukázková databáze pomocí SSMS](./media/sql-database-get-started/new-sample-db-ssms.png)
2. V Průzkumníku objektů rozbalte novou databázi. Zobrazí se její objekty.

    ![Objekty nové ukázkové databáze pomocí SSMS](./media/sql-database-get-started/new-sample-db-objects-ssms.png)
3. Pravým tlačítkem klikněte na ukázkovou databázi a potom klikněte na **Nový dotaz**.

    ![Nový dotaz pro ukázkovou databázi pomocí SSMS](./media/sql-database-get-started/new-sample-db-query-ssms.png)
4. Do okna dotazu zadejte následující dotaz:

   ```select * from sys.objects```
   
9.  Na panelu nástrojů klikněte na **Spustit**. Zobrazí se seznam všech systémových objektů v ukázkové databázi.

    ![Systémové objekty nové ukázkové databáze pomocí SSMS](./media/sql-database-get-started/new-sample-db-query-objects-ssms.png)

## <a name="create-a-new-blank-database-using-sql-server-management-studio"></a>Vytvoření nové prázdné databáze pomocí aplikace SQL Server Management Studio

1. V Průzkumníku objektů klikněte pravým tlačítkem na **Databáze** a potom klikněte na **Nová databáze**.

    ![Nová prázdná databáze pomocí SSMS](./media/sql-database-get-started/new-blank-database-ssms.png)

    > [!NOTE]
    > Můžete taky nechat SSMS vytvořit za vás databázový skript a vytvořit novou databázi pomocí jazyka Transact-SQL.
    >

2. V dialogovém okně Nová databáze zadejte název databáze do textového pole Název databáze. 

    ![Název nové prázdné databáze pomocí SSMS](./media/sql-database-get-started/new-blank-database-name-ssms.png)

3. V dialogovém okně Nová databáze klikněte na **Možnosti** a potom změňte edici na **Basic**.

    ![Možnosti nové prázdné databáze pomocí SSMS](./media/sql-database-get-started/new-blank-database-options-ssms.png)

    > [!TIP]
    > Projděte si v tomto dialogovém okně další možnosti, které je možné upravit pro Azure SQL Database. Další informace o těchto možnostech najdete v tématu [Vytvoření databáze](https://msdn.microsoft.com/library/dn268335.aspx).
    >

4. Kliknutím na **OK** vytvoříte prázdnou databázi.
5. Po dokončení aktualizujte uzel Databáze v Průzkumníku objektů. Zobrazí se nově vytvořená prázdná databáze. 

    ![Nová prázdná databáze v Průzkumníku objektů](./media/sql-database-get-started/new-blank-database-object-explorer.png)

> [!TIP]
> Během výuky můžete ušetřit tím, že odstraníte databáze, které nepoužíváte. Databáze edice Basic můžete obnovit do sedmi dnů. Neodstraňujte ale server. Pokud to uděláte, nebude možné obnovit server ani žádnou z jeho odstraněných databází.
>


## <a name="next-steps"></a>Další kroky
Tento kurz jste nyní dokončili, ale k dispozici je celá řada dalších kurzů, které staví na tom, co jste se v tomto kurzu naučili. 

* Pokud se chcete začít seznamovat se zabezpečením Azure SQL Database, přečtěte si téma [Začínáme se zabezpečením](sql-database-control-access-sql-authentication-get-started.md).
* Pokud znáte Excel, zjistěte, jak se [připojit k databázi SQL v Azure pomocí aplikace Excel](sql-database-connect-excel.md).
* Jste-li připraveni na psaní kódu, zvolte si programovací jazyk v tématu [Knihovny pro připojení ke službě SQL Database a systému SQL Server](sql-database-libraries.md).
* Pokud chcete do Azure přesunout databáze z místního systému SQL Server, přečtěte si téma [Migrace databáze do služby SQL Database](sql-database-cloud-migrate.md).
* Chcete-li do nové tabulky načíst data ze souboru CSV pomocí nástroje příkazového řádku BCP, více zjistíte v tématu [Načtení dat do služby SQL Database ze souboru CSV pomocí BCP](sql-database-load-from-csv-with-bcp.md).
* Pokud chcete začít vytvářet tabulky a další objekty, přečtěte si v článku [Vytvoření tabulky](https://msdn.microsoft.com/library/ms365315.aspx) téma věnované postupu při vytváření tabulek.

## <a name="additional-resources"></a>Další zdroje

- Technický přehled najdete v tématu [Co je SQL Database?](sql-database-technical-overview.md).
- Informace o cenách najdete v tématu [Ceny Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).




<!--HONumber=Jan17_HO3-->


