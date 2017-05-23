Teď můžete pomocí Průzkumníka dat vytvořit kontejner grafu a přidat data do databáze. 

1. Na webu Azure Portal v navigační nabídce klikněte na **Průzkumník dat**. 
2. V okně Průzkumníka dat klikněte na **Nová kolekce** a potom stránku vyplňte pomocí následujících informací.

    ![Průzkumník dat na webu Azure Portal](./media/cosmosdb-create-graph/azure-cosmosdb-data-explorer.png)

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    ID databáze|sample-database|ID vaší nové databáze. Názvy databází musí mít délku 1 až 255 znaků a nesmí obsahovat znaky `/ \ # ?` ani koncové mezery.
    ID kolekce|sample-graph|ID nové kolekce. Názvy kolekcí mají stejné požadavky na znaky jako ID databází.
    Kapacita úložiště| 10 GB|Nechte zadanou výchozí hodnotu. Tohle je kapacita úložiště databáze.
    Propustnost|400 RU/s|Nechte zadanou výchozí hodnotu. Propustnost můžete později navýšit, pokud budete chtít snížit latenci.
    Klíč oddílu|/userid|Klíč oddílu, který bude rovnoměrně distribuovat data do jednotlivých oddílů. Výběr správného klíče oddílu je důležitý pro vytvoření výkonné kolekce. Další informace se dozvíte v tématu [Návrh a vytváření oddílů](../articles/cosmos-db/partition-data.md#designing-for-partitioning).

3. Po vyplnění formuláře klikněte na **OK**.