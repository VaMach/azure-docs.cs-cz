Teď můžete pomocí nástroje Průzkumník dat na webu Azure Portal vytvořit databázi a kolekci. 

1. Klikněte na tlačítko **Průzkumníku dat** > **nové kolekce**. 
    
    **Přidat kolekce** oblasti se zobrazí v pravém, budete muset přejděte přímo k jeho zobrazení.

    ![Portál Azure Průzkumníku dat, okno Přidat kolekce](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

2. V **Přidání kolekce** zadejte nastavení pro nové kolekce.

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    ID databáze|Úlohy|Zadejte *úlohy* jako název nové databáze. Názvy databází musí mít délku 1 až 255 znaků a nesmí obsahovat znaky /, \\, #, ? ani koncové mezery.
    ID kolekce|Items|Zadejte *položky* jako název nové kolekce. ID kolekce mají stejné požadavky znak jako názvy databází.
    Kapacita úložiště| Pevná (10 GB)|Změňte hodnotu na **Fixed (10 GB)**. Tato hodnota je kapacita úložiště databáze.
    Propustnost|400 RU|Změňte propustnosti na 400 jednotek žádosti za sekundu (RU/s). Kapacita úložiště musí být nastavena na **Fixed (10 GB)** Chcete-li nastavit propustnost 400 RU/s. Pokud budete chtít snížit latenci, můžete propustnost později navýšit. 
    Klíč oddílu|/kategorie|Zadejte */category* jako klíč oddílu. Klíč oddílu distribuuje data rovnoměrně pro každý oddíl v databázi. Další informace o oddílech najdete v tématu [návrh a vytváření oddílů](../articles/cosmos-db/partition-data.md#designing-for-partitioning).

    Klikněte na **OK**.

    Průzkumník dat zobrazí nové databáze a kolekce.

    ![Portál Azure Průzkumníku dat znázorňující nové databáze a kolekce.](./media/cosmos-db-create-collection/azure-cosmos-db-new-collection.png)