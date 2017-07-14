1. V novém okně se přihlaste k webu [Azure Portal](https://portal.azure.com/).
2. V levém podokně klikněte na **Nový**, potom na **Databáze** a nakonec na **Databáze Azure Cosmos**.
   
   ![Podokno databází portálu Azure Portal](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. V okně **Nový účet** zadejte požadovanou konfiguraci pro účet databáze Azure Cosmos. 

    V databázi Azure Cosmos můžete vybrat jeden ze čtyř programovacích modelů: Gremlin (graf), MongoDB, SQL (DocumentDB) a Tabulka (klíč-hodnota). 
    
    V tomto článku rychlého zprovoznění programujeme s využitím rozhraní DocumentDB API, takže při vyplňování formuláře vyberte možnost **SQL (DocumentDB)**. Pokud ale máte data grafu pro aplikaci sociálních médií, data typu klíč/hodnota (tabulka) nebo data migrovaná z aplikace MongoDB, je dobré si uvědomit, že databáze Azure Cosmos může poskytnout vysoce dostupnou a globálně distribuovanou platformu databázové služby pro všechny důležité podnikové aplikace.

    V okně **Nový účet** vyplňte pole podle informací v následujícím snímku obrazovky (slouží pouze jako vodítko). Při vytváření účtu vyberte jedinečné hodnoty, které neodpovídají těm na snímku obrazovky. 
 
    ![Okno Nový v databázi Azure Cosmos](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    ID|*Jedinečná hodnota*|Jedinečný název který identifikuje váš účet databáze Azure Cosmos. K zadanému ID se připojí řetězec *documents.azure.com*, čímž vznikne váš identifikátor URI, takže zadejte jedinečné, ale snadno rozpoznatelné ID. Toto ID může obsahovat pouze malá písmena, číslice a znak spojovníku (-) a musí se skládat ze 3 až 50 znaků.
    Rozhraní API|SQL (DocumentDB)|Dál v tomto článku budeme programovat za použití [rozhraní DocumentDB API](../articles/documentdb/documentdb-introduction.md).|
    Předplatné|*Vaše předplatné*|Předplatné Azure, se kterým chcete účet databáze Azure Cosmos používat. 
    Skupina prostředků|*Stejná hodnota jako ID*|Nový název skupiny prostředků pro váš účet. V zájmu jednoduchosti můžete použít název, který se shoduje s vaším ID. 
    Umístění|*Oblast nejbližší vašim uživatelům*|Zeměpisné umístění, ve kterém chcete účet databáze Azure Cosmos hostovat. Vyberte umístění, které je vašim uživatelům nejbližší, abyste jim zajistili nejrychlejší přístup k datům.
4. Kliknutím na **Vytvořit** vytvořte účet.
5. Na horním panelu nástrojů klikněte na **Oznámení** a sledujte proces nasazení.

    ![Podokno Oznámení portálu Azure Portal](./media/cosmos-db-create-dbaccount-graph/azure-documentdb-nosql-notification.png)

6.  Po dokončení nasazení otevřete nový účet na dlaždici **Všechny prostředky**. 

    ![Účet DocumentDB na dlaždici Všechny prostředky](./media/cosmos-db-create-dbaccount/all-resources.png)
 
