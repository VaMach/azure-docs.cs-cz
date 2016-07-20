1.  Přihlaste se k online [portálu Microsoft Azure](https://portal.azure.com/).
2.  Na panelu vlevo klikněte na **Nový**, potom na **Data + úložiště** a nakonec na **Azure DocumentDB**.

    ![Snímek obrazovky portálu Azure při vytváření databáze, zvýrazněné tlačítko Nový, v okně Vytvořit možnost Data + úložiště a v okně Data + úložiště možnost Azure DocumentDB](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)  

3. V okně **Nový účet DocumentDB** zadejte požadovanou konfiguraci účtu DocumentDB.

    ![Snímek obrazovky s oknem Nový DocumentDB](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)


    - Do pole **ID** zadejte název pro identifikaci účtu DocumentDB.  Po ověření **ID** se v poli **ID** zobrazí zelená značka zaškrtnutí. Hodnota **ID** se v rámci identifikátoru URI stane názvem hostitele. **ID** může obsahovat pouze malá písmena, číslice a znak spojovníku a musí se skládat ze 3 až 50 znaků. Všimněte si, že ke zvolenému názvu koncového bodu se připojí text *documents.azure.com* a výsledkem bude koncový bod účtu DocumentDB.

    - V případě možnosti **Předplatné** vyberte požadované předplatné, se kterým chcete účet DocumentDB používat. Pokud má váš účet jenom jedno předplatné, bude ve výchozím nastavením vybrán tento účet.

    - Ve **skupině prostředků** vyberte nebo vytvořte skupinu prostředků pro účet DocumentDB.  Ve výchozím nastavení bude vybrána existující skupina prostředků v rámci předplatného Azure.  Samozřejmě si můžete také vytvořit novou skupinu prostředků, do které účet DocumentDB přidáte. Další informace najdete v článku [Použití Portálu Azure ke správě prostředků Azure](resource-group-portal.md).

    - K určení zeměpisného umístění, ve kterém chcete účet DocumentDB hostovat, použijte nabídku **Umístění**.   

4.  Po nakonfigurování možností nového účtu DocumentDB klikněte na **Vytvořit**.  Vytvoření účtu DocumentDB může několik minut trvat.  Pokud chcete zkontrolovat stav, můžete průběh sledovat na úvodním panelu.  
    ![Snímek obrazovky s dlaždicí Vytváření na úvodním panelu – Online tvůrce databází](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-3.png)  

    Popřípadě můžete průběh sledovat v centru oznámení.  

    ![Rychlé vytváření databází – snímek obrazovky s centrem oznámení, na kterém je vidět vytváření účtu DocumentDB](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-4.png)  

    ![Snímek obrazovky s centrem oznámení, na kterém je vidět úspěšné vytvoření účtu DocumentDB a jeho nasazení do skupiny prostředků – oznámení Online tvůrce databází](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-5.png)

5.  Účet DocumentDB je po vytvoření připravený k použití s výchozím nastavením na online portálu. Všimněte si, že výchozí konzistence účtu DocumentDB je nastavená na možnost **Relace**.  Výchozí nastavení konzistence můžete upravit kliknutím na ikonu **Nastavení** na horním příkazovém řádku a potom na položku **Výchozí konzistence**, která se nachází v okně **Všechna nastavení** v části **Funkce**.

    ![Snímek obrazovky okna Skupina prostředků – začátek vývoje aplikace](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-6.png)  

    ![Snímek obrazovky okna Úroveň konzistence – konzistence relace](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-7.png)  

[Postup: Vytvoření účtu DocumentDB]: #Howto
[Další kroky]: #NextSteps
[documentdb-manage]:../articles/documentdb/documentdb-manage.md



<!--HONumber=Jun16_HO2-->


