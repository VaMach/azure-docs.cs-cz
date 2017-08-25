Teď můžete pomocí Průzkumníka dat vytvořit tabulku a přidat data do databáze. 

1. Na webu Azure Portal v navigační nabídce klikněte na **Průzkumník dat (Preview)**. 
2. V okně Průzkumníka dat klikněte na **Nová tabulka** a potom stránku vyplňte pomocí následujících informací.

    ![Průzkumník dat na webu Azure Portal](./media/cosmos-db-create-table/azure-cosmosdb-data-explorer.png)

    Nastavení|Navrhovaná hodnota|Popis
    ---|---|---
    ID tabulky|sample-table|ID vaší nové tabulky. Názvy tabulek mají stejné požadavky na znaky jako ID databází. Názvy databází musí mít délku 1 až 255 znaků a nesmí obsahovat znaky `/ \ # ?` ani koncové mezery.
    Kapacita úložiště| 10 GB|Nechte zadanou výchozí hodnotu. Tohle je kapacita úložiště databáze.
    Propustnost|400 RU/s|Nechte zadanou výchozí hodnotu. [Propustnost](../articles/cosmos-db/request-units.md) můžete později navýšit, pokud budete chtít snížit latenci.

3. Po vyplnění formuláře klikněte na **OK**.