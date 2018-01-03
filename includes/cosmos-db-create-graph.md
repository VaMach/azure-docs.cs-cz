Teď můžete pomocí nástroje Průzkumník dat na webu Azure Portal vytvořit databázi grafu. 

1. Na portálu Azure, v nabídce na levé straně vyberte **Data Explorer (Preview)**.

2. V části **Data Explorer (Preview)**, vyberte **nový graf**. Pak zadejte na stránce pomocí následující informace:

    ![Průzkumník dat na portálu Azure Portal](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer.png)

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    ID databáze|sample-database|Jako název nové databáze zadejte *sample-database*. Názvy databází musí být v rozmezí 1 až 255 znaků a nesmí obsahovat `/ \ # ?` nebo koncové mezery.
    ID grafu|sample-graph|Jako název nové kolekce zadejte *sample-graph*. Názvy grafů mají stejné požadavky na znaky jako ID databází.
    Kapacita úložiště| 10 GB|Nechte zadanou výchozí hodnotu. Tohle je kapacita úložiště databáze.
    Propustnost|400 RU/s|Nechte zadanou výchozí hodnotu. Propustnost můžete později navýšit, pokud budete chtít snížit latenci.

3. Když je formulář vyplnit, vyberte **OK**.
