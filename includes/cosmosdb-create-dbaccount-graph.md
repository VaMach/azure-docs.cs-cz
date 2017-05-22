1. V novém okně se přihlaste k webu [Azure Portal](https://portal.azure.com/).
2. V nabídce vlevo klikněte na **Nový**, potom na **Databáze** a nakonec na **Databáze Azure Cosmos**.
   
   ![Snímek obrazovky webu Azure Portal se zvýrazněním položek Další služby a Databáze Azure Cosmos](./media/cosmosdb-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-1.png)

3. V okně **Nový účet** zadejte požadovanou konfiguraci účtu databáze Azure Cosmos. 

    V databázi Azure Cosmos můžete vybrat jeden ze čtyř programovacích modelů: Gremlin (graf), MongoDB, SQL (DocumentDB) a Tabulka (klíč-hodnota).  
       
    V tomto rychlém startu budeme programovat s využitím rozhraní Graph API, takže při vyplňování formuláře vyberete možnost **Gremlin (graph)** (Gremlin (graf)). Pokud ale máte data dokumentu z aplikace katalogu, data typu klíč/hodnota (tabulka) nebo data migrovaná z aplikace MongoDB, je dobré si uvědomit, že databáze Azure Cosmos může poskytnout vysoce dostupnou a globálně distribuovanou platformu databázové služby pro všechny důležité podnikové aplikace.

    Vyplňte okno Nový účet a jako vodítko při tom použijte informace na snímku obrazovky. Při vytváření svého účtu budete vybírat jedinečné hodnoty, které tedy nebudou přesně odpovídat snímku obrazovky. 
 
    ![Snímek okna Nový v databázi Azure Cosmos](./media/cosmosdb-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-2.png)

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    ID|*Jedinečná hodnota*|Jedinečný název, pomocí kterého chcete identifikovat účet databáze Azure Cosmos. K zadanému ID se připojí řetězec *documents.azure.com*, čímž vznikne váš identifikátor URI, takže zadejte jedinečné, ale snadno rozpoznatelné ID. ID smí obsahovat jenom malá písmena, číslice a znak spojovníku a musí se skládat ze 3 až 50 znaků.
    Rozhraní API|Gremlin (graph) (Gremlin (graf))|Dál v tomto článku budeme programovat za použití [rozhraní Graph API](../articles/cosmos-db/graph-introduction.md).|
    Předplatné|*Vaše předplatné*|Předplatné Azure, se kterým chcete účet databáze Azure Cosmos používat. 
    Skupina prostředků|*Stejná hodnota jako ID*|Nový název skupiny prostředků pro váš účet. V zájmu jednoduchosti můžete použít název, který se shoduje s vaším ID. 
    Umístění|*Oblast nejbližší vašim uživatelům*|Zeměpisné umístění, ve kterém chcete účet databáze Azure Cosmos hostovat. Vyberte umístění, které má nejblíž k vašim uživatelům, abyste jim zajistili nejrychlejší přístup k datům.
    Plán služby App Service pro rozhraní Graph API|Vytvořit nový – výchozí S1|Vyberte výpočetní prostředek pro provádění dotazů grafy Gremlin.

4. Kliknutím na **Vytvořit** vytvořte účet.
5. Na panelu nástrojů klikněte na **Oznámení** a sledujte proces nasazení.

    ![Oznámení o zahájení nasazení](./media/cosmosdb-create-dbaccount-graph/azure-documentdb-nosql-notification.png)

6.  Po dokončení nasazení otevřete nový účet na dlaždici Všechny prostředky. 

    ![Účet DocumentDB na dlaždici Všechny prostředky](./media/cosmosdb-create-dbaccount-graph/azure-documentdb-all-resources.png)