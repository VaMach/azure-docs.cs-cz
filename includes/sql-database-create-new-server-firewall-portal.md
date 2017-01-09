
<!--
includes/sql-database-create-new-server-firewall-portal.md

Latest Freshness check:  2016-11-28 , rickbyh.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started.md
articles/sql-database/sql-database-configure-firewall-settings
articles/sql-data-warehouse-get-started-provision.md

-->
## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Vytvoření pravidla brány firewall na úrovni serveru na webu Azure Portal

1. V okně SQL Server v části Nastavení klikněte na **Brána firewall**. Otevře se okno Brána firewall pro SQL Server.

    ![Brána firewall SQL Serveru](../articles/sql-database/media/sql-database-get-started/sql-server-firewall.png)
    
2. Zkontrolujte zobrazenou IP adresu klienta a pomocí prohlížeče podle vašeho výběru ověřte, že je to vaše IP adresa na internetu (zadejte dotaz Jaká je moje IP adresa). Čas od času si adresy z různých důvodů neodpovídají.

    ![Vaše IP adresa](../articles/sql-database/media/sql-database-get-started/your-ip-address.png)

3. Za předpokladu, že se IP adresy shodují, klikněte na panelu nástrojů na **Přidat IP adresu klienta**.

    ![Přidat IP adresu klienta](../articles/sql-database/media/sql-database-get-started/add-client-ip.png)

    > [!NOTE]
    > Bránu firewall služby SQL Database na serveru můžete otevřít pro jednu IP adresu nebo pro celý rozsah adres. Otevření brány firewall umožňuje uživatelům a správcům SQL přihlásit se k jakékoli databázi na serveru, ke kterému mají platné přihlašovací údaje.
    >

4. Kliknutím na **Uložit** na panelu nástrojů uložte toto pravidlo brány firewall na úrovni serveru a potom klikněte na **OK**.

    ![Přidat IP adresu klienta](../articles/sql-database/media/sql-database-get-started/save-firewall-rule.png)

> [!Tip]
> Kurz najdete v článku [Kurz k SQL Database: Vytvoření serveru, pravidla brány firewall na úrovni serveru, ukázkové databáze a pravidla brány firewall na úrovni databáze a připojení k SQL Serveru](../articles/sql-database/sql-database-get-started.md).    
>


<!--HONumber=Jan17_HO1-->


