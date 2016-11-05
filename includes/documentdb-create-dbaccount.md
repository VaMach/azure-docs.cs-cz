1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).
2. Na panelu vlevo klikněte na **Nový**, na **Data + úložiště** a nakonec na **DocumentDB (NoSQL)**.
   
   ![Snímek obrazovky webu Azure Portal se zvýrazněním položek Další služby a DocumentDB (NoSQL)](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)  
3. V okně **Nový účet** zadejte požadovanou konfiguraci účtu DocumentDB.
   
    ![Snímek obrazovky s oknem Nový DocumentDB](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)
   
   * Do pole **ID** zadejte název pro identifikaci účtu DocumentDB.  Po ověření **ID** se v poli **ID** zobrazí zelená značka zaškrtnutí. Hodnota **ID** se v rámci identifikátoru URI stane názvem hostitele. **ID** smí obsahovat jenom malá písmena, číslice a znak spojovníku a musí se skládat ze 3 až 50 znaků. Všimněte si, že ke zvolenému názvu koncového bodu se připojí text *documents.azure.com* Výsledkem bude koncový bod účtu DocumentDB.
   * V poli **NoSQL API** vyberte **DocumentDB**.  
   * V poli **Předplatné** vyberte předplatné Azure, se kterým chcete tento účet DocumentDB používat. Pokud má váš účet jenom jedno předplatné, ve výchozím nastavením se vybere tento účet.
   * V poli **Skupina prostředků** vyberte nebo vytvořte skupinu prostředků pro účet DocumentDB.  Ve výchozím nastavení se vytvoří nová skupina prostředků. Další informace najdete v článku [Použití webu Azure Portal ke správě prostředků Azure](../articles/azure-portal/resource-group-portal.md).
   * K určení zeměpisného umístění, ve kterém chcete účet DocumentDB hostovat, použijte **Umístění**. 
4. Po nakonfigurování možností nového účtu DocumentDB klikněte na **Vytvořit**. Ke kontrole stavu nasazení použijte centrum oznámení.  
   
   ![Rychlé vytváření databází – snímek obrazovky s centrem oznámení, na kterém je vidět vytváření účtu DocumentDB](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-4.png)  
   
   ![Snímek obrazovky s centrem oznámení, na kterém je vidět úspěšné vytvoření účtu DocumentDB a jeho nasazení do skupiny prostředků – oznámení online tvůrce databází](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-5.png)
5. Účet DocumentDB je po vytvoření připravený k použití s výchozím nastavením. Výchozí konzistence účtu DocumentDB je nastavená na možnost **Relace**.  Výchozí konzistenci můžete upravit kliknutím na **Výchozí konzistence** v nabídce prostředků. Další informace o úrovních konzistence, které nabízí DocumentDB, najdete v tématu [Úrovně konzistence v DocumentDB](../articles/documentdb/documentdb-consistency-levels.md).
   
   ![Snímek obrazovky okna Skupina prostředků – začátek vývoje aplikace](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-6.png)  
   
   ![Snímek obrazovky okna Úroveň konzistence – konzistence relace](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-7.png)  

[Postup: Vytvoření účtu DocumentDB]: #Howto
[Další kroky]: #NextSteps
[documentdb-manage]:../articles/documentdb/documentdb-manage.md


<!--HONumber=Sep16_HO3-->


