1. V nové okno prohlížeče, přihlaste se k [portál Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **nové** > **databáze** > **Azure Cosmos DB**.
   
   ![Podokno databází portálu Azure Portal](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-1.png)

3. V **nový účet** zadejte nastavení pro nový účet Azure Cosmos DB. 
 
    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    ID|*Zadejte jedinečný název*|Zadejte jedinečný název pro identifikaci tohoto účtu Azure Cosmos DB. Protože *documents.azure.com* se připojí k ID, které poskytnete vytvořit váš kontaktní bod, jedinečné, ale osobní ID využívají.<br><br>Toto ID může obsahovat pouze malá písmena, číslice a znak spojovníku (-) a musí se skládat ze 3 až 50 znaků.
    Rozhraní API|Cassandra|Rozhraní API Určuje typ účtu chcete-li vytvořit. Poskytuje Azure Cosmos DB pět rozhraní API pro vyhovuje potřebám vaší aplikace: SQL (databáze dokumentu), Gremlin (grafu databáze), MongoDB (databáze dokumentu), Azure Table a Cassandra, každý, které aktuálně vyžadují samostatný účet. <br><br>Vyberte **Cassandra** vzhledem k tomu, že v tento rychlý start sloupec celou databázi, která je dotazovatelný pomocí syntaxe CQL vytváříte.<br><br>Pokud se v seznamu nezobrazí Cassandra (wide sloupec), pak budete muset [požádat o připojení](../articles/cosmos-db/cassandra-introduction.md#sign-up-now) programu preview Cassandra rozhraní API.<br><br> [Další informace o rozhraní API Cassandra](../articles/cosmos-db/cassandra-introduction.md)|
    Předplatné|*Vaše předplatné*|Vyberte předplatné Azure, který chcete použít pro tento účet Azure Cosmos DB. 
    Skupina prostředků|*Zadejte stejný jedinečný název, který výše uvedeného v ID*|Zadejte nový název skupiny prostředků pro váš účet. V zájmu jednoduchosti můžete použít název, který se shoduje s vaším ID. 
    Umístění|*Vyberte oblast nejbližší uživatelům.*|Vyberte zeměpisné umístění, ve kterém k hostování účtu Azure Cosmos DB. Použijte umístění, které je nejblíže k uživatelům poskytnout nejrychlejší přístup k datům.
    Připnutí na řídicí panel | Vyberte | Zaškrtněte toto políčko, aby nový databázový účet se přidá do řídicího panelu portálu pro snadný přístup.

    Poté klikněte na **Vytvořit**.

    ![Okno Nový účet pro službu Azure Cosmos DB](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-2.png)

4. Vytvoření účtu trvá několik minut. Během vytváření účtu se zobrazí řídicí panel portálu **nasazení Azure DB Cosmos** dlaždici.

    ![Na portálu Azure dlaždici oznámení](./media/cosmos-db-create-dbaccount-cassandra/deploying-cosmos-db.png)

    Po vytvoření účtu **Blahopřejeme! Byl vytvořen účet Azure Cosmos DB** zobrazí se stránka. 

