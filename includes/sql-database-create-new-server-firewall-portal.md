
<!--
includes/sql-database-create-new-server-firewall-portal.md

Latest Freshness check:  2016-08-01 , rickbyh.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md
articles/sql-database/sql-database-configure-firewall-settings

-->
## Vytvoření nové brány firewall na úrovni serveru SQL Azure

Pomocí následujícího postupu vytvořte na portálu Azure pravidlo brány firewall na úrovni serveru, které umožní připojení z určité IP adresy (vašeho klientského počítače) nebo celého rozsahu IP adres k logickému serveru SQL. 

1. Pokud nejste připojení, připojte se k [portálu Azure](http://portal.azure.com).
2. Ve výchozím okně klikněte na **SQL Server**.

    ![Nová serverová brána firewall](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-1.png)

2. V okně SQL Server klikněte server SQL, na kterém chcete pravidlo brány firewall vytvořit. 

    ![Nová serverová brána firewall](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-2.png)
           
3. Zkontrolujte vlastnosti serveru.

    ![Nová serverová brána firewall](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-3.png)
      
4. V okně Nastavení klikněte na tlačítko **Brána firewall**.

    ![Nová serverová brána firewall](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-4.png)
    

    > [AZURE.NOTE] K oknu pro **nastavení firewallu** na úrovni serveru se dostanete také z okna Databáze.

5. Klikněte na **Přidat IP adresu klienta**. Azure potom vytvoří pravidlo pro IP adresu vašeho klienta.

      ![Nová serverová brána firewall](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-5.png)

6. Případně můžete kliknout na přidanou IP adresu a upravit adresu brány firewall. Tím povolíte přístup k rozsahu IP adres.

      ![Nová serverová brána firewall](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-6.png)
    
7. Klikněte na tlačítko **Uložit** a vytvořte tak pravidlo brány firewall na úrovni serveru.

     ![Nová serverová brána firewall](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-7.png)

    >[AZURE.IMPORTANT] IP adresa klienta se může občas změnit a vy pak nebudete mít přístup k serveru, dokud nevytvoříte nové pravidlo brány firewall. Svoji IP adresu můžete zkontrolovat pomocí aplikace [Bing](http://www.bing.com/search?q=my%20ip%20address). Potom můžete přidat jednu IP adresu nebo rozsah IP adres. Podrobnosti najdete v tématu [Správa nastavení brány firewall](sql-database-configure-firewall-settings.md#manage-existing-server-level-firewall-rules-through-the-azure-portal).



<!--HONumber=Aug16_HO4-->


