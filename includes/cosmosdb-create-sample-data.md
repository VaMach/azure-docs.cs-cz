Teď můžete do nové kolekce přidávat data pomocí Průzkumníka dat.

1. V Průzkumníku dat se nová databáze zobrazí v podokně Kolekce. Rozbalte databázi **Items**, rozbalte kolekci **ToDoList**, klikněte na **Dokumenty** a potom klikněte na **Nové dokumenty**. 

   ![Vytváření nových dokumentů v Průzkumníku dat na webu Azure Portal](./media/cosmosdb-create-sample-data/azure-cosmosdb-data-explorer-emulator-new-document.png)
  
2. Teď přidejte do kolekce několik nových dokumentů s následující strukturou, kde jako ID v jednotlivých dokumentech vložíte jedinečné hodnoty a podle potřeby změníte další vlastnosti. Nové dokumenty můžou mít jakoukoli strukturu, protože databáze Azure Cosmos neuplatňuje pro data žádné schéma.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries."
     }
     ```

     Teď můžete k načítání dat používat dotazy v Průzkumníku dat. Ve výchozím nastavení používá Průzkumník dat pro načtení všech dokumentů v kolekci příkaz SELECT * FROM c. Ten můžete změnit na příkaz SELECT * FROM c ORDER BY c.name ASC, který vrátí všechny dokumenty ve vzestupném abecedním pořadí podle vlastnosti name. 
 
     Průzkumník dat můžete použít taky k vytváření uložených procedur, funkcí UDF a aktivačních událostí pro provádění obchodní logiky na straně serveru. Průzkumník dat zpřístupní všechna integrovaná programová data v rozhraních API, ale zajistí jednoduchý přístup k vašim datům na webu Azure Portal.