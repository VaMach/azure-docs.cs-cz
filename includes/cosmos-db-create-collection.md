Teď můžete pomocí Průzkumníka dat vytvořit kolekci a přidat data do databáze. 

1. Na webu Azure Portal v levé navigační nabídce klikněte na **Průzkumník dat (Preview)**. 

2. V okně **Průzkumník dat (Preview)** klikněte na **Nová kolekce** a potom zadejte následující informace:

    ![Okno Průzkumník dat na webu Azure Portal](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    ID databáze|Úlohy|ID vaší nové databáze. Názvy databází musí mít délku 1 až 255 znaků a nesmí obsahovat znaky /, \\, #, ? ani koncové mezery.
    ID kolekce|Items|ID nové kolekce. Názvy kolekcí mají stejné požadavky na znaky jako ID databází.
    Kapacita úložiště| Pevná (10 GB)|Použijte výchozí hodnotu. Tohle je kapacita úložiště databáze.
    Propustnost|400 RU|Použijte výchozí hodnotu. Pokud budete chtít snížit latenci, můžete propustnost později navýšit.
    RU/min|Vypnuto|Nechte zadanou výchozí hodnotu. Když bude později potřeba zpracovat nárazové úlohy, můžete pak funkci [RU/min](../articles/cosmos-db/request-units-per-minute.md) zapnout.
    Klíč oddílu|/kategorie|Klíč oddílu, který rovnoměrně distribuuje data do jednotlivých oddílů. Výběr správného klíče oddílu je důležitý pro vytvoření výkonné kolekce. Další informace se dozvíte v tématu [Návrh a vytváření oddílů](../articles/cosmos-db/partition-data.md#designing-for-partitioning).    
3. Po vyplnění formuláře klikněte na **OK**.