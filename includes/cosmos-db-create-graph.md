Teď můžete pomocí nástroje Průzkumník dat na webu Azure Portal vytvořit databázi grafu. 

1. Na webu Azure Portal v levé navigační nabídce klikněte na **Průzkumník dat (Preview)**. 
2. V okně **Průzkumník dat (Preview)** klikněte na **Nový graf** a potom stránku vyplňte pomocí následujících informací.

    ![Průzkumník dat na portálu Azure Portal](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer.png)

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    ID databáze|sample-database|ID vaší nové databáze. Názvy databází musí mít délku 1 až 255 znaků a nesmí obsahovat znaky `/ \ # ?` ani koncové mezery.
    ID grafu|sample-graph|ID vašeho nového grafu. Názvy grafu mají stejné požadavky na znaky jako ID databází.
    Kapacita úložiště| 10 GB|Nechte zadanou výchozí hodnotu. Tohle je kapacita úložiště databáze.
    Propustnost|400 RU/s|Nechte zadanou výchozí hodnotu. Propustnost můžete později navýšit, pokud budete chtít snížit latenci.
    RU/min|Vypnuto|Nechte zadanou výchozí hodnotu. 
    Klíč oddílu|/userid|Klíč oddílu, který bude rovnoměrně distribuovat data do jednotlivých oddílů. Výběr správného klíče oddílu je důležitý pro vytvoření výkonného grafu. Další informace se dozvíte v tématu [Návrh a vytváření oddílů](../articles/cosmos-db/partition-data.md#designing-for-partitioning).

3. Po vyplnění formuláře klikněte na **OK**.