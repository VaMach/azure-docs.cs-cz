1. V novém okně se přihlaste k webu [Azure Portal](https://portal.azure.com/).
2. V nabídce vlevo klikněte na **Nový**, potom na **Databáze** a nakonec v části **Databáze Azure Cosmos** klikněte na **Vytvořit**.
   
   ![Snímek obrazovky webu Azure Portal se zvýrazněním položek Další služby a Databáze Azure Cosmos](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)

3. V okně **Nový účet** zadejte požadovanou konfiguraci účtu databáze Azure Cosmos. 

    S Azure Cosmos databáze, můžete vybrat jednu z čtyři programovací modely: Gremlin (grafu), MongoDB, SQL a tabulky (klíč hodnota). 
       
    V tomto rychlém startu budeme programovat s využitím rozhraní API MongoDB, takže při vyplňování formuláře vyberete možnost **MongoDB**. Pokud ale máte data grafu pro aplikaci sociálních médií, data dokumentu z aplikace katalogu nebo data typu klíč/hodnota (tabulka), je dobré si uvědomit, že databáze Azure Cosmos může poskytnout vysoce dostupnou a globálně distribuovanou platformu databázové služby pro všechny důležité podnikové aplikace.

    Vyplňte okno **Nový účet** a jako vodítko při tom použijte informace uvedené v tabulce.
 
    ![Snímek okna Nový v databázi Azure Cosmos](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-2.png)
   
    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    ID|*Jedinečná hodnota*|Jedinečný název, pomocí kterého chcete identifikovat účet databáze Azure Cosmos. K zadanému ID se připojí řetězec *documents.azure.com*, čímž vznikne váš identifikátor URI, takže zadejte jedinečné, ale snadno rozpoznatelné ID. ID smí obsahovat jenom malá písmena, číslice a znak spojovníku a musí se skládat ze 3 až 50 znaků.
    Rozhraní API|MongoDB|Rozhraní API Určuje typ účtu chcete-li vytvořit. Poskytuje Azure Cosmos DB pět rozhraní API pro vyhovuje potřebám vaší aplikace: SQL (databáze dokumentu), Gremlin (grafu databáze), MongoDB (databáze dokumentu), Azure Table a Cassandra, každý, které aktuálně vyžadují samostatný účet. <br><br>Vyberte **MongoDB** vzhledem k tomu, že v tento rychlý start vytváříte dokumentu databázi, která je dotazovatelný pomocí MongoDB.<br><br>[Další informace o rozhraní API MongoDB](../articles/cosmos-db/mongodb-introduction.md)|
    Předplatné|*Vaše předplatné*|Předplatné Azure, se kterým chcete účet databáze Azure Cosmos používat. 
    Skupina prostředků|*Stejná hodnota jako ID*|Nový název skupiny prostředků pro váš účet. V zájmu jednoduchosti můžete použít název, který se shoduje s vaším ID. 
    Umístění|*Oblast nejbližší vašim uživatelům*|Zeměpisné umístění, ve kterém chcete účet databáze Azure Cosmos hostovat. Vyberte umístění, které má nejblíž k vašim uživatelům, abyste jim zajistili nejrychlejší přístup k datům.

4. Kliknutím na **Vytvořit** vytvořte účet.
5. Na panelu nástrojů klikněte na **Oznámení** a sledujte proces nasazení.

    ![Oznámení o zahájení nasazení](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-nosql-notification.png)

6.  Po dokončení nasazení otevřete nový účet na dlaždici Všechny prostředky. 

    ![Účet Azure Cosmos DB na dlaždici všechny prostředky](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-all-resources.png)
