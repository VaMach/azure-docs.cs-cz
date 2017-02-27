---
title: "Rychlý start: První Azure SQL Database | Dokumentace Microsoftu"
description: "Naučíte se vytvářet logický server služby SQL Database, pravidlo brány firewall na úrovni serveru a databáze na webu Azure Portal. Také se naučíte používat SQL Server Management Studio s Azure SQL Database."
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
ms.date: 02/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 166a9d7032bb75188a790bea1724aefd194dcefa
ms.openlocfilehash: 36afd5c8bccb080ae3aaf1b4975d317b9087a3b3


---
# <a name="create-connect-to-and-query-your-first-azure-sql-databases-in-the-azure-portal-and-using-ssms"></a>Vytvoření prvních databází SQL Azure, připojení k nim a jejich dotazování na webu Azure Portal a pomocí SSMS

V tomto kurzu se naučíte vytvářet databáze SQL Azure, připojovat se k nim a dotazovat je na webu Azure Portal a pomocí aplikace SQL Server Management Studio. Po dokončení tohoto kurzu:

* Budete mít vytvořenou skupinu prostředků obsahující logický server, pravidlo brány firewall na úrovni serveru a dvě databáze.
* Budete umět zobrazit vlastnosti serveru a databáze na webu Azure Portal a pomocí aplikace SQL Server Management Studio.
* Budete vědět, jak dotazovat databázi na webu Azure Portal a pomocí aplikace SQL Server Management Studio.

**Časový odhad:** Tento kurz trvá přibližně 30 minut (za předpokladu, že už máte splněné požadavky).

> [!TIP]
> Můžete se také naučit vytvářet databáze SQL Azure, připojovat se k nim a dotazovat je pomocí [PowerShellu](sql-database-get-started-powershell.md) nebo [jazyka C#](sql-database-get-started-csharp.md).
>

> [!NOTE]
> Tento kurz vám pomůže seznámit se s obsahem těchto výukových okruhů: [Přehled serveru SQL Database](sql-database-server-overview.md), [přehled SQL Database](sql-database-overview.md) a [přehled pravidel brány firewall pro Azure SQL Database](sql-database-firewall-configure.md). Přehled služby SQL Database najdete v tématu [Co je SQL Database?](sql-database-technical-overview.md).
>  

## <a name="prerequisites"></a>Požadavky

* **Účet Azure:** Můžete si [zdarma otevřít účet Azure](https://azure.microsoft.com/free/) nebo [aktivovat výhody pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/). 

* **Oprávnění pro vytváření v Azure:** Musíte být schopni připojit se k webu Azure Portal pomocí účtu, který je členem role přispěvatele nebo vlastníka předplatného. Další informace o řízení přístupu na základě role (RBAC) najdete v tématu [Začínáme se správou přístupu na webu Azure Portal](../active-directory/role-based-access-control-what-is.md).

* **SQL Server Management Studio:** Nejnovější verzi aplikace SQL Server Management Studio (SMSS) si můžete stáhnout a nainstalovat v části [Stažení aplikace SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx). Při připojení ke službě Azure SQL Database vždycky používejte nejnovější verzi SSMS, protože se neustále vydávají nové funkce.

### <a name="sign-in-to-the-azure-portal"></a>Přihlášení k webu Azure Portal

Kroky v tomto postupu ukazují, jak se připojit k webu Azure Portal pomocí [účtu Azure](https://account.windowsazure.com/Home/Index).

1. Otevřete prohlížeč, kterému dáváte přednost, a připojte se k [portálu Azure](https://portal.azure.com/).
2. Přihlaste se k webu [Portál Azure](https://portal.azure.com/).
3. Na **přihlašovací stránce** zadejte přihlašovací údaje ke svému předplatnému.
   
   ![Přihlášení](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="create-a-new-logical-sql-server"></a>Vytvoření nového logického SQL Serveru

Kroky v tomto postupu ukazují, jak na webu Azure Portal vytvořit logický server v oblasti podle vašeho výběru. Logický server je objekt, ve kterém vytváříte databáze SQL a pravidla brány firewall, pomocí kterých povolíte uživatelům připojení přes bránu firewall služby Azure SQL Database. 

1. Klikněte na **Nový**, zadejte **sql server** a potom klikněte na **ENTER**.

    ![Logický SQL server](./media/sql-database-get-started/logical-sql-server.png)
2. Klikněte na **SQL Server (logický server)**.
   
    ![Vytvoření logického SQL serveru](./media/sql-database-get-started/create-logical-sql-server.png)
3. Kliknutím na **Vytvořit** otevřete nové okno SQL Server (pouze logický server).

    ![Nový logický SQL server](./media/sql-database-get-started/new-logical-sql-server.png)
3. Do textového pole **Název serveru** zadejte platný název pro nový logický server. Zelená značka zaškrtnutí označuje, že jste zadali platný název.
    
    ![Název nového serveru](./media/sql-database-get-started/new-server-name.png)

    > [!IMPORTANT]
    > Plně kvalifikovaný název nového serveru musí být globálně jedinečný a ve formátu: **<název_serveru>.database.windows.net**. Tento plně kvalifikovaný název serveru použijete později v tomto kurzu pro připojení k serveru a databázím.
    >
    
4. Do textového pole **Účet správce serveru** zadejte uživatelské jméno přihlašovacích údajů ověřování SQL pro tento server. Tyto přihlašovacích údaje se označují jako přihlašovacích údaje objektu zabezpečení serveru. Zelená značka zaškrtnutí označuje, že jste zadali platný název.
    
    ![Přihlášení správce SQL](./media/sql-database-get-started/sql-admin-login.png)
5. Do textových polí **Heslo** a **Potvrzení hesla** zadejte heslo pro přihlašovací účet objektu zabezpečení serveru. Zelená značka zaškrtnutí označuje, že jste zadali platné heslo.
    
    ![Heslo správce SQL](./media/sql-database-get-started/sql-admin-password.png)
6. V rozevíracím seznamu **Předplatné** vyberte předplatné, ve kterém máte oprávnění k vytváření objektů.

    ![předplatné](./media/sql-database-get-started/subscription.png)
7. Pod textovým polem **Skupina prostředků** vyberte **Vytvořit novou** a zadejte platný název nové skupiny prostředků. Zelená značka zaškrtnutí označuje, že jste zadali platný název.

    ![Nová skupina prostředků](./media/sql-database-get-started/new-resource-group.png)

8. V textovém poli **Umístění** vyberte datové centrum, ve kterém chcete vytvořit logický server.
    
    ![Umístění serveru](./media/sql-database-get-started/server-location.png)
    
    > [!TIP]
    > V tomto okně nejde měnit zaškrtnutí políčka **Povolit službám Azure přístup k serveru**. Toto nastavení můžete změnit v okně brány firewall serveru. Další informace najdete v článku [Začínáme se zabezpečením](sql-database-control-access-sql-authentication-get-started.md).
    >
    
9. Zaškrtněte políčko **Připnout na řídicí panel**.

10. Kliknutím na **Vytvořit** nasaďte tento skript do Azure a vytvořte logický server.

    ![Tlačítko Vytvořit](./media/sql-database-get-started/create.png)

11. Po vytvoření serveru zkontrolujte vlastnosti serveru, které se zobrazí ve výchozím nastavení. 

    ![Okno SQL Server](./media/sql-database-get-started/sql-server-blade.png)
12. Kliknutím na **Vlastnosti** zobrazíte další vlastnosti logického SQL serveru.

    ![Vlastnosti sql serveru](./media/sql-database-get-started/sql-server-properties.png)
13. Zkopírujte si do schránky celý plně kvalifikovaný název serveru. Použijete ho v tomto kurzu o chvíli později.

    ![Celý název sql serveru](./media/sql-database-get-started/sql-server-full-name.png)

## <a name="create-a-server-level-firewall-rule"></a>Vytvoření pravidla brány firewall na úrovni serveru

Kroky v tomto postupu ukazují, jak vytvořit pravidlo brány firewall na úrovni serveru na webu Azure Portal. Brána firewall služby Azure SQL Database ve výchozím nastavení brání externímu připojení k logickému serveru a jeho databázím. Abyste se mohli k serveru připojit, je nutné vytvořit pravidlo brány firewall pro IP adresu počítače, ze kterého se v dalším postupu budete připojovat. Další informace najdete v tématu [Přehled pravidel brány firewall služby Azure SQL Database](sql-database-firewall-configure.md).

1. V okně serveru SQL klikněte na **Brána firewall**. Otevře se okno Brána firewall pro váš server. Všimněte si, že se zobrazila IP adresa vašeho klientského počítače.

    ![Brána firewall SQL Serveru](./media/sql-database-get-started/sql-server-firewall.png)

2. Klikněte na **Přidat IP adresu klienta** na panelu nástrojů a vytvořte pro vaši aktuální IP adresu pravidlo brány firewall.

    ![Přidat IP adresu klienta](./media/sql-database-get-started/add-client-ip.png)

    > [!NOTE]
    > Pravidlo brány firewall můžete vytvořit pro jedinou IP adresu nebo pro celý rozsah IP adres. Otevření brány firewall umožňuje uživatelům a správcům SQL přihlásit se k jakékoli databázi na serveru, pro který mají platné přihlašovací údaje.
    >

4. Kliknutím na **Uložit** na panelu nástrojů uložte toto pravidlo brány firewall na úrovni serveru a potom kliknutím na **OK** zavřete dialogové okno s oznámením úspěchu.

    ![úspěch](./media/sql-database-get-started/save-firewall-rule.png)

## <a name="connect-to-the-server-with-ssms"></a>Připojení k serveru pomocí SSMS

Kroky v tomto postupu ukazují, jak se připojit k logickému SQL serveru pomocí aplikace SQL Server Management Studio. SSMS je primárním nástrojem, pomocí kterého správci databází spravují databáze a servery SQL.

1. Otevřete aplikaci SQL Server Management Studio (zadejte do vyhledávacího pole ve Windows text **Microsoft SQL Server Management Studio** a stiskněte **Enter**. Otevře se SSMS).

    ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)
3. V dialogovém okně **Připojit k serveru** zadejte plně kvalifikovaný název serveru z předchozího postupu, vyberte Ověřování SQL Serveru a potom zadejte přihlašovací jméno a heslo, které jste určili během zřizování serveru.

    ![Připojení k serveru](./media/sql-database-get-started/connect-to-server.png)
4. Klikněte na **Připojit**. Naváže se připojení a v SSMS se otevře Průzkumník objektů.

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
    > Pokud chcete začít se zabezpečením SQL, přečtěte si téma [Začínáme s ověřováním SQL](sql-database-control-access-sql-authentication-get-started.md).
    >

## <a name="create-a-database-with-sample-data"></a>Vytvoření databáze s ukázkovými daty

Kroky v tomto postupu ukazují, jak na webu Azure Portal vytvořit databázi s ukázkovými daty přidruženou k logickému serveru, který jste vytvořili dříve. 

1. Na webu Azure Portal klikněte na **Databáze SQL** ve výchozím okně.

    ![Databáze SQL](./media/sql-database-get-started/new-sql-database.png)
2. V okně Databáze SQL klikněte na **Přidat**. 

    ![Přidání databáze SQL](./media/sql-database-get-started/add-sql-database.png)

    ![Okno Databáze SQL](./media/sql-database-get-started/sql-database-blade.png)
3. Do textového pole **Název databáze** zadejte platný název databáze.

    ![Název databáze SQL](./media/sql-database-get-started/sql-database-name.png)
4. V části **Vybrat zdroj** vyberte **Ukázka (AdventureWorksLT)**.
   
    ![adventure works lt](./media/sql-database-get-started/adventureworkslt.png)
5. V části **Server** ověřte, že je vybraný váš server. Všimněte si také, že databázi je možné na server přidat jako izolovanou databázi (to je výchozí nastavení), nebo do elastického fondu. Další informace o elastických fondech najdete v části věnované [elastickým fondům](sql-database-elastic-pool.md).

6. V části **Cenová úroveň** změňte cenovou úroveň na **Basic** a klikněte na **Vybrat**. Cenovou úroveň můžete zvýšit v případě potřeby později, ale pro účely výuky doporučujeme využít nejnižší úroveň.

    ![Cenová úroveň](./media/sql-database-get-started/pricing-tier.png)
7. Zaškrtněte políčko **Připnout na řídicí panel** a potom klikněte na **Vytvořit**.

    ![Tlačítko Vytvořit](./media/sql-database-get-started/create.png)

8. Po vytvoření databáze zobrazte její podrobnosti na webu Azure Portal. Další kurzy vám pomohou se s možnostmi, které jsou v tomto okně dostupné, seznámit blíž. 

    ![Nové okno ukázkové databáze](./media/sql-database-get-started/new-sample-db-blade.png)

## <a name="query-the-database-in-the-azure-portal"></a>Dotazování databáze na webu Azure Portal

Kroky v tomto postupu ukazují, jak přímo dotazovat databázi na webu Azure Portal. 

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
7. Když jste ověřeni, zadejte do okna dotazu následující dotaz a potom klikněte na **Spustit**.

   ```select * from sys.objects```

    ![dotaz v editoru dotazů](./media/sql-database-get-started/query-editor-query.png)

8. Zkontrolujte výsledky dotazu v podokně **Výsledky**.

    ![výsledky editoru dotazů](./media/sql-database-get-started/query-editor-results.png)

## <a name="query-the-database-with-ssms"></a>Dotazování databáze pomocí SSMS

Kroky v tomto postupu ukazují, jak se připojit k databázi pomocí aplikace SQL Server Management Studio a následně dotazovat ukázková data pro zobrazení objektů v databázi.

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

Kroky v tomto postupu ukazují, jak vytvořit novou databázi pomocí aplikace SQL Server Management Studio.

1. V Průzkumníku objektů klikněte pravým tlačítkem na **Databáze** a potom klikněte na **Nová databáze**.

    ![Nová prázdná databáze pomocí SSMS](./media/sql-database-get-started/new-blank-database-ssms.png)

2. V dialogovém okně **Nová databáze** zadejte název databáze do textového pole Název databáze. 

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

Pokud připojení ke službě Azure SQL Database selže, zobrazí se chybové zprávy. Problémy s připojením může způsobovat rekonfigurace databáze SQL Azure, nastavení brány firewall, vypršení časového limitu připojení nebo nesprávné přihlašovací údaje. Nástroj Poradce při potížích s připojením najdete na stránce [Řešení potíží s připojením ve službě Microsoft Azure SQL Database](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database).

## <a name="delete-a-single-database-in-the-azure-portal"></a>Odstranění izolované databáze na webu Azure Portal

Kroky v tomto postupu ukazují, jak odstranit izolovanou databázi pomocí webu Azure Portal.

1. V okně Databáze SQL na webu Azure Portal klikněte na databázi, kterou chcete odstranit. 
2.  U vybrané databáze SQL klikněte na **Odstranit**.

    ![delete-database](./media/sql-database-get-started/delete-database.png)
2. Kliknutím na **Ano** potvrďte, že chcete databázi trvale odstranit.

    ![delete-database-yes](./media/sql-database-get-started/delete-database-yes.png)

> [!TIP]
> Během doby uchování vaší databáze ji můžete obnovit z automatických záloh spouštěných službou (za předpokladu, že neodstraníte službu samotnou). Databáze edice Basic můžete obnovit do sedmi dnů. Databáze všech ostatních edic můžete obnovit do 35 dnů. Pokud odstraníte službu samotnou, nebude možné obnovit server ani žádnou z jeho odstraněných databází. Další informace o zálohování databáze najdete v tématu [Další informace o zálohování SQL Database](sql-database-automated-backups.md) a informace o obnovení databáze ze zálohy najdete v tématu [Obnovení databáze](sql-database-recovery-using-backups.md). Článek o obnovení odstraněné databáze obsahuje téma [Obnovení odstraněné databáze SQL Azure – Azure Portal](sql-database-restore-deleted-database-portal.md).
>


## <a name="next-steps"></a>Další kroky
Tento kurz jste nyní dokončili, ale k dispozici je celá řada dalších kurzů, které staví na tom, co jste se v tomto kurzu naučili. 

- Pokud chcete začít kurz ověřování SQL Serveru, přejděte k tématu [Ověřování a autorizace SQL](sql-database-control-access-sql-authentication-get-started.md).
- Pokud chcete začít kurz ověřování Azure Active Directory, přejděte k tématu [Ověřování a autorizace AAD](sql-database-control-access-aad-authentication-get-started.md).
* Pokud chcete zadat dotaz na ukázkovou databázi na webu Azure Portal, přejděte k tématu [Public Preview: Interaktivní práce s dotazy pro databáze SQL](https://azure.microsoft.com/updates/azure-sql-database-public-preview-t-sql-editor/).
* Pokud znáte Excel, zjistěte, jak se [připojit k databázi SQL v Azure pomocí aplikace Excel](sql-database-connect-excel.md).
* Jste-li připraveni na psaní kódu, zvolte si programovací jazyk v tématu [Knihovny pro připojení ke službě SQL Database a systému SQL Server](sql-database-libraries.md).
* Pokud chcete do Azure přesunout databáze z místního systému SQL Server, přečtěte si téma [Migrace databáze do služby SQL Database](sql-database-cloud-migrate.md).
* Chcete-li do nové tabulky načíst data ze souboru CSV pomocí nástroje příkazového řádku BCP, více zjistíte v tématu [Načtení dat do služby SQL Database ze souboru CSV pomocí BCP](sql-database-load-from-csv-with-bcp.md).
* Pokud chcete začít vytvářet tabulky a další objekty, přečtěte si v článku [Vytvoření tabulky](https://msdn.microsoft.com/library/ms365315.aspx) téma věnované postupu při vytváření tabulek.

## <a name="additional-resources"></a>Další zdroje

- Technický přehled najdete v tématu [Co je SQL Database?](sql-database-technical-overview.md)
- Informace o cenách najdete v tématu [Ceny Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).




<!--HONumber=Feb17_HO3-->


