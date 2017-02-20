---
title: "Rychlý start: První Azure SQL Database | Dokumentace Microsoftu"
description: "Naučíte se vytvářet logický server SQL Database, pravidlo brány firewall na úrovni serveru a databáze pomocí webu Azure Portal. Také se naučíte používat SQL Server Management Studio s Azure SQL Database."
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
ms.date: 02/04/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 6453cca9f876e6c363fbed463263c0f9684a3e70
ms.openlocfilehash: b838974de06ecbc751254064e2310df51c450086


---
# <a name="quick-start-tutorial-your-first-azure-sql-database"></a>Rychlý úvodní kurz: První Azure SQL Database

V tomto rychlém úvodním kurzu se naučíte:

* [Vytvoření nového logického serveru](sql-database-get-started.md#create-a-new-logical-sql-server) 
* [Zobrazení vlastností logického serveru](sql-database-get-started.md#view-the-logical-server-properties) 
* [Vytvoření pravidla brány firewall na úrovni serveru](sql-database-get-started.md#create-a-server-level-firewall-rule) 
* [Připojení k serveru pomocí SSMS](sql-database-get-started.md#connect-to-the-server-with-ssms) 
* [Vytvoření databáze s ukázkovými daty](sql-database-get-started.md#create-a-database-with-sample-data) 
* [Zobrazení vlastností databáze](sql-database-get-started.md#view-the-database-properties) 
* [Dotazování databáze na webu Azure Portal](sql-database-get-started.md#query-the-database-in-the-azure-portal) 
* [Připojení a dotazování databáze pomocí SSMS](sql-database-get-started.md#connect-and-query-the-database-with-ssms) 
* [Vytvoření prázdné databáze pomocí SSMS](sql-database-get-started.md#create-a-blank-database-with-ssms) 
* [Řešení potíží s připojením](sql-database-get-started.md#troubleshoot-connectivity) 
* [Odstranění databáze](sql-database-get-started.md#delete-a-single-database) 


V tomto rychlém úvodním kurzu vytvoříte ukázkovou databázi a prázdnou databázi, která je spuštěná ve skupině prostředků Azure a připojená k logickému serveru. Dále vytvoříte dvě pravidla brány firewall na úrovni serveru nakonfigurovaná tak, aby umožňovala objektu zabezpečení na úrovni serveru přihlášení k serveru ze dvou zadaných IP adres. Nakonec se naučíte dotazovat databázi na webu Azure Portal a připojovat se a dotazovat pomocí aplikace SQL Server Management Studio. 

**Časový odhad:** Tento kurz trvá přibližně 30 minut (za předpokladu, že už máte splněné požadavky).

> [!TIP]
> Stejné úlohy můžete provádět i pomocí jazyka [C#](sql-database-get-started-csharp.md) nebo [PowerShellu](sql-database-get-started-powershell.md).
>

## <a name="prerequisites"></a>Požadavky

* Potřebujete mít účet Azure. Můžete si [zdarma otevřít účet Azure](/pricing/free-trial/?WT.mc_id=A261C142F) nebo [aktivovat výhody pro předplatitele sady Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* Musíte být schopni připojit se k webu Azure Portal pomocí účtu, který je členem role přispěvatele nebo vlastníka předplatného. Další informace o řízení přístupu na základě role (RBAC) najdete v tématu [Začínáme se správou přístupu na webu Azure Portal](../active-directory/role-based-access-control-what-is.md).

> [!NOTE]
> Tento rychlý úvodní kurz vám pomůže seznámit se s obsahem těchto výukových okruhů: [Přehled serveru SQL Database](sql-database-server-overview.md), [přehled SQL Database](sql-database-overview.md) a [přehled pravidel brány firewall pro Azure SQL Database](sql-database-firewall-configure.md).
>  


### <a name="sign-in-to-the-azure-portal-with-your-azure-account"></a>Přihlášení k webu Azure Portal pomocí účtu Azure
Pomocí [účtu Azure](https://account.windowsazure.com/Home/Index) se k webu Azure Portal připojíte následujícím postupem.

1. Otevřete prohlížeč, kterému dáváte přednost, a připojte se k [portálu Azure](https://portal.azure.com/).
2. Přihlaste se k webu [Portál Azure](https://portal.azure.com/).
3. Na **přihlašovací stránce** zadejte přihlašovací údaje ke svému předplatnému.
   
   ![Přihlášení](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="create-a-new-logical-sql-server"></a>Vytvoření nového logického SQL Serveru

Podle kroků v tomto postupu vytvořte nový logický server pomocí webu Azure Portal v oblasti podle svého výběru.

1. Klikněte na **Nový**, zadejte **sql server** a potom klikněte na **ENTER**.

    ![Logický SQL server](./media/sql-database-get-started/logical-sql-server.png)
2. Klikněte na **SQL Server (logický server)**.
   
    ![Vytvoření logického SQL serveru](./media/sql-database-get-started/create-logical-sql-server.png)
3. Kliknutím na **Vytvořit** otevřete nové okno SQL Server (logický server).

    ![Nový logický SQL server](./media/sql-database-get-started/new-logical-sql-server.png)
3. Do textového pole Název serveru zadejte platný název pro nový logický server. Zelená značka zaškrtnutí označuje, že jste zadali platný název.
    
    ![Název nového serveru](./media/sql-database-get-started/new-server-name.png)

    > [!IMPORTANT]
    > Plně kvalifikovaný název nového serveru má formát: <název_serveru>.database.windows.net.
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

## <a name="view-the-logical-server-properties"></a>Zobrazení vlastností logického serveru

Podle kroků v tomto postupu zobrazte vlastnosti serveru pomocí webu Azure Portal. V následujícím postupu budete pro připojení k serveru potřebovat plně kvalifikovaný název tohoto serveru. 

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

## <a name="create-a-server-level-firewall-rule"></a>Vytvoření pravidla brány firewall na úrovni serveru

Podle kroků v tomto postupu vytvořte pomocí webu Azure Portal nové pravidlo brány firewall na úrovni serveru, které vám v dalším postupu umožní připojit se k serveru pomocí SQL Server Management Studia.

1. V okně SQL Server v části Nastavení klikněte na **Brána firewall**. Otevře se okno Brána firewall pro SQL Server.

    ![Brána firewall SQL Serveru](./media/sql-database-get-started/sql-server-firewall.png)

2. Na panelu nástrojů klikněte na **Přidat IP adresu klienta**.

    ![Přidat IP adresu klienta](./media/sql-database-get-started/add-client-ip.png)

    > [!NOTE]
    > Bránu firewall služby SQL Database na serveru můžete otevřít pro jednu IP adresu nebo pro celý rozsah adres. Otevření brány firewall umožňuje uživatelům a správcům SQL přihlásit se k jakékoli databázi na serveru, ke kterému mají platné přihlašovací údaje.
    >

4. Kliknutím na **Uložit** na panelu nástrojů uložte toto pravidlo brány firewall na úrovni serveru a potom klikněte na **OK**.

    ![Přidat IP adresu klienta](./media/sql-database-get-started/save-firewall-rule.png)

## <a name="connect-to-the-server-with-ssms"></a>Připojení k serveru pomocí SSMS

Podle kroků v tomto postupu se připojte k logickému serveru SQL pomocí SQL Server Management Studia.

1. Pokud jste to ještě neudělali, stáhněte si nejnovější verzi SMSS v části [Stažení aplikace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) a nainstalujte ji. Když bude k dispozici pro stažení novější verze, aplikace SSMS zobrazí upozornění, abyste mohli používat aktuální verzi.

2. Po instalaci zadejte do vyhledávacího pole ve Windows text **Microsoft SQL Server Management Studio** a klikněte na **Enter**. SSMS se otevře:

    ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)
3. V dialogovém okně Připojení k serveru zadejte informace potřebné k připojení k SQL Serveru pomocí ověřování SQL Serveru.

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

## <a name="create-a-database-with-sample-data"></a>Vytvoření databáze s ukázkovými daty

Podle kroků v tomto postupu vytvořte databázi s ukázkovými daty pomocí webu Azure Portal. Tuto databázi vytvoříte připojenou k logickému serveru, který jste dříve vytvořili. Pokud v oblasti, ve které jste vytvořili váš server, není k dispozici úroveň služeb Basic, odstraňte server a znovu ho vytvořte v jiné oblasti. Pokyny k odstranění najdete v posledním postupu tohoto kurzu.

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

## <a name="view-the-database-properties"></a>Zobrazení vlastností databáze

Podle kroků v tomto postupu dáte dotaz na databázi pomocí webu Azure Portal.

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

## <a name="query-the-database-in-the-azure-portal"></a>Dotazování databáze na webu Azure Portal

Podle kroků v tomto postupu dáte dotaz na databázi pomocí editoru dotazů na webu Azure Portal. Dotaz zobrazí objekty v databázi.

1. V okně Databáze SQL klikněte v panelu nástrojů na **Nástroje**.

    ![nástroje](./media/sql-database-get-started/tools.png)
2. V okně Nástroje klikněte na **Editor dotazů (Preview)**.

    ![editor dotazů](./media/sql-database-get-started/query-editor.png)
3. Klikněte na zaškrtávací políčko a potvrďte, že jste vzali na vědomí, že editor dotazů je ve verzi Preview, a pak klikněte na **OK**.
4. V okně **Editor dotazů** klikněte na **Přihlásit**.

    ![okno editoru dotazů](./media/sql-database-get-started/query-editor-blade.png)
5. Zkontrolujte typ autorizace a přihlašovací jméno a potom zadejte heslo pro přihlášení. 

    ![přihlášení k editoru dotazů](./media/sql-database-get-started/query-editor-login.png)
6. Kliknutím na **OK** se pokuste přihlásit.
7. Jakmile se zobrazí chyba přihlášení s oznámením, že váš klient nemá oprávnění k přihlášení z důvodu absence pravidla brány firewall pro IP adresu vašeho klienta, zkopírujte IP adresu vašeho klienta v okně chyby a v okně SQL Serveru pro tuto databázi vytvořte pravidlo brány firewall na úrovni serveru.

    ![chyba editoru dotazů](./media/sql-database-get-started/query-editor-error.png)
8. Opakujte předchozích 6 kroků a přihlaste se k vaší databázi.
9. Když jste ověřeni, do okna dotazu zadejte následující dotaz:

   ```select * from sys.objects```

    ![dotaz v editoru dotazů](./media/sql-database-get-started/query-editor-query.png)
10.  Klikněte na **Run** (Spustit).
11. Zkontrolujte výsledky dotazu v podokně **Výsledky**.

    ![výsledky editoru dotazů](./media/sql-database-get-started/query-editor-results.png)

## <a name="connect-and-query-the-database-with-ssms"></a>Připojení a dotazování databáze pomocí SSMS

Podle kroků v tomto postupu se připojte k databázi pomocí SQL Server Management Studia a potom zadejte dotaz na ukázková data, abyste zobrazili objekty v databázi.

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

## <a name="create-a-blank-database-with-ssms"></a>Vytvoření prázdné databáze pomocí SSMS

Podle kroků v tomto postupu vytvořte novou databázi na logickém serveru pomocí SQL Server Management Studia.

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

## <a name="troubleshoot-connectivity"></a>Řešení potíží s připojením

> [!IMPORTANT]
> Pokud máte problémy s připojením, přečtěte si téma [Problémy s připojením](sql-database-troubleshoot-common-connection-issues.md).
> 

## <a name="delete-a-single-database"></a>Odstranění izolované databáze

Podle kroků v tomto postupu odstraníte izolovanou databázi pomocí webu Azure Portal.

1. V okně webu Azure Portal pro vaši databázi SQL klikněte na **Odstranit**.

    ![delete-database](./media/sql-database-get-started/delete-database.png)
2. Kliknutím na **Ano** potvrďte, že chcete databázi trvale odstranit.

    ![delete-database-yes](./media/sql-database-get-started/delete-database-yes.png)

> [!TIP]
> Během doby uchování pro vaši databázi ji můžete obnovit z automatických záloh spouštěných službou. Databáze edice Basic můžete obnovit do sedmi dnů. Neodstraňujte ale server. Pokud to uděláte, nebude možné obnovit server ani žádnou z jeho odstraněných databází. Další informace o zálohování databáze najdete v tématu [Další informace o zálohování SQL Database](sql-database-automated-backups.md) a informace o obnovení databáze ze zálohy najdete v tématu [Obnovení databáze](sql-database-recovery-using-backups.md). Článek o obnovení odstraněné databáze obsahuje téma [Obnovení odstraněné databáze SQL Azure – Azure Portal](sql-database-restore-deleted-database-portal.md).
>


## <a name="next-steps"></a>Další kroky
Tento kurz jste nyní dokončili, ale k dispozici je celá řada dalších kurzů, které staví na tom, co jste se v tomto kurzu naučili. 

- Pokud chcete začít kurz ověřování SQL Serveru, přejděte k tématu [Ověřování a autorizace SQL](sql-database-control-access-sql-authentication-get-started.md).
- Pokud chcete začít kurz ověřování Azure Active Directory, přejděte k tématu [Ověřování a autorizace AAD](sql-database-control-access-aad-authentication-get-started.md).
* Pokud chcete zadat dotaz na ukázkovou databázi na webu Azure Portal, přejděte k tématu [Public Preview: Interaktivní práce s dotazy pro databáze SQL](https://azure.microsoft.com/en-us/updates/azure-sql-database-public-preview-t-sql-editor/).
* Pokud znáte Excel, zjistěte, jak se [připojit k databázi SQL v Azure pomocí aplikace Excel](sql-database-connect-excel.md).
* Jste-li připraveni na psaní kódu, zvolte si programovací jazyk v tématu [Knihovny pro připojení ke službě SQL Database a systému SQL Server](sql-database-libraries.md).
* Pokud chcete do Azure přesunout databáze z místního systému SQL Server, přečtěte si téma [Migrace databáze do služby SQL Database](sql-database-cloud-migrate.md).
* Chcete-li do nové tabulky načíst data ze souboru CSV pomocí nástroje příkazového řádku BCP, více zjistíte v tématu [Načtení dat do služby SQL Database ze souboru CSV pomocí BCP](sql-database-load-from-csv-with-bcp.md).
* Pokud chcete začít vytvářet tabulky a další objekty, přečtěte si v článku [Vytvoření tabulky](https://msdn.microsoft.com/library/ms365315.aspx) téma věnované postupu při vytváření tabulek.

## <a name="additional-resources"></a>Další zdroje

- Technický přehled najdete v tématu [Co je SQL Database?](sql-database-technical-overview.md)
- Informace o cenách najdete v tématu [Ceny Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).




<!--HONumber=Feb17_HO2-->


