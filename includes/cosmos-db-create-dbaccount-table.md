1. V novém okně prohlížeče se přihlaste k webu [Azure Portal](https://portal.azure.com/).
2. V levé nabídce klikněte na **Vytvořit prostředek**, pak na **Databáze** a nakonec v části **Azure Cosmos DB** klikněte na **Vytvořit**. 
   
   ![Snímek obrazovky webu Azure Portal se zvýrazněním položek Další služby a Databáze Azure Cosmos](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-1.png)

3. Na stránce **Nový účet** zadejte nastavení nového účtu služby Azure Cosmos DB. 
 
    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    ID|*Zadejte jedinečný název*|Zadejte jedinečný název, který identifikuje tento účet služby Azure Cosmos DB. Jelikož je řetězec *documents.azure.com* připojený k ID, které poskytnete k vytvoření identifikátoru URI, použijte jedinečné, ale snadno rozpoznatelné ID.<br><br>Toto ID může obsahovat pouze malá písmena, číslice a znak spojovníku (-) a musí se skládat ze 3 až 50 znaků.
    Rozhraní API|Tabulka Azure|Rozhraní API určuje typ účtu, který se má vytvořit. Azure Cosmos DB poskytuje pro splnění požadavků vaší aplikace pět rozhraní API: SQL (databáze dokumentů), Gremlin (databáze grafu), MongoDB (databáze dokumentů), Tabulka Azure a Cassandra, z nichž každé v současné době vyžaduje samostatný účet.<br><br>Vyberte **Tabulka Azure**, protože v tomto rychlém startu vytváříte tabulku pracující s rozhraním Table API.<br><br>[Další informace o rozhraní Table API](../articles/cosmos-db/table-introduction.md) |
    Předplatné|*Zadejte stejný jedinečný název, jako jste zadali výše pro ID*|Vyberte předplatné Azure, které chcete pro tento účet služby Azure Cosmos DB použít. 
    Skupina prostředků|*Stejná hodnota jako ID*|Zadejte nový název skupiny prostředků pro váš účet. V zájmu jednoduchosti můžete použít název, který se shoduje s vaším ID. 
    Umístění|*Vyberte oblast nejbližší vašim uživatelům*|Vyberte zeměpisné umístění, ve kterém chcete účet služby Azure Cosmos DB hostovat. Použijte umístění, které je vašim uživatelům nejbližší, abyste jim zajistili nejrychlejší přístup k datům.
    Povolit geografickou redundanci| Ponechte prázdné | Tím se vytvoří replikovaná verze vaší databáze ve druhé (spárované) oblasti. Ponechte toto nastavení prázdné.  
    Připnutí na řídicí panel | Vyberte | Zaškrtněte toto políčko, aby se váš nový účet databáze přidal na řídicí panel portálu, kde k němu budete mít snadný přístup.

    Poté klikněte na možnost **Vytvořit**.  

    ![Snímek okna Nový v Azure Cosmos DB](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-2.png)

4. Vytvoření účtu trvá několik minut. Během vytváření účtu se na portálu zobrazí dlaždice **Nasazování služby Azure Cosmos DB**.

    ![Podokno Oznámení portálu Azure Portal](./media/cosmos-db-create-dbaccount-table/deploying-cosmos-db.png)

    Po vytvoření účtu se zobrazí stránka **Blahopřejeme! Váš účet služby Azure Cosmos DB byl vytvořen**.
