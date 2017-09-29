### <a name="create-a-new-logical-sql-server-in-the-azure-portal"></a>Vytvoření nového logického SQL serveru na webu Azure Portal

1. Klikněte na **Nový**, vyhledejte **logical server** a stiskněte **Enter**.

    ![vyhledání logického serveru](./media/sql-data-warehouse-create-logical-server/search-logical-server.png)
2. Vyberte **SQL Server (logický server)**. 

    ![výběr logického serveru](./media/sql-data-warehouse-create-logical-server/select-logical-server.png)
  
3. Kliknutím na **Vytvořit** otevřete nové okno SQL Server (logický server).

   <kbd> ![otevření okna logického serveru](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png) </kbd> <kbd>![okno logického serveru](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png) </kbd>
  
3. Do pole pro název serveru v okně SQL Server (logický server) zadejte platný název pro nový logický server. Zelená značka zaškrtnutí označuje, že jste zadali platný název.
    
    ![Název nového serveru](./media/sql-data-warehouse-create-logical-server/new-name-logical-server.png)

    > [!IMPORTANT]
    > Plně kvalifikovaný název nového serveru bude <název_serveru>.database.windows.net.
    >
    
4. Do textového pole Přihlášení správce serveru zadejte uživatelské jméno pro 	účet ověřování SQL pro tento server. Toto přihlášení se označuje jako přihlášení objektu zabezpečení serveru. Zelená značka zaškrtnutí označuje, že jste zadali platný název.
    
    ![Přihlášení správce SQL](./media/sql-data-warehouse-create-logical-server/sql-admin-login.png)
5. Do textových polí **Heslo** a **Potvrzení hesla** zadejte heslo pro přihlašovací účet objektu zabezpečení serveru. Zelená značka zaškrtnutí označuje, že jste zadali platné heslo.
    
    ![Heslo správce SQL](./media/sql-data-warehouse-create-logical-server/sql-admin-password.png)
6. Vyberte předplatné, ve kterém máte oprávnění k vytváření objektů.

    ![předplatné](./media/sql-data-warehouse-create-logical-server/subscription.png)
7. V textovém poli Skupina prostředků vyberte **Vytvořit nový** a potom do textového pole skupiny prostředků zadejte platný název pro novou skupinu prostředků (můžete také použít existující skupinu prostředků, pokud jste si už nějakou vytvořili). Zelená značka zaškrtnutí označuje, že jste zadali platný název.

    ![Nová skupina prostředků](./media/sql-data-warehouse-create-logical-server/new-resource-group.png)

8. V textovém poli **Umístění** vyberte datacentrum odpovídající vašemu umístění, třeba Austrálie – východ.
    
    ![Umístění serveru](./media/sql-data-warehouse-create-logical-server/server-location.png)
    
    > [!TIP]
    > V tomto okně nejde měnit zaškrtnutí políčka **Povolit službám Azure přístup k serveru**. Toto nastavení můžete změnit v okně brány firewall serveru. Další informace najdete v článku [Začínáme se zabezpečením](../articles/sql-database/sql-database-manage-servers-portal.md).
    >
    
9. Klikněte na možnost **Vytvořit**.

    ![Tlačítko Vytvořit](./media/sql-data-warehouse-create-logical-server/create.png)

