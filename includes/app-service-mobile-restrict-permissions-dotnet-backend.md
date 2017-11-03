
Ve výchozím nastavení můžete rozhraní API v back-end mobilní aplikace volá anonymně. Dále je třeba omezit přístup jenom ověřené klienty.  

* **Node.js zpět ukončení (prostřednictvím portálu Azure)** :  

    V nastavení mobilní aplikace, klikněte na tlačítko **snadno tabulky** a vyberte tabulku. Klikněte na tlačítko **změnit oprávnění**, vyberte **ověřený přístup pouze** pro všechna oprávnění a pak klikněte na tlačítko **Uložit**.
* **Rozhraní .NET back-end (C#)**:  

    V projektu serveru, přejděte na **řadiče** > **TodoItemController.cs**. Přidat `[Authorize]` atribut **TodoItemController** třídy následujícím způsobem. Chcete-li omezit přístup jenom na konkrétní metody, můžete taky použít tento atribut pouze na tyto metody místo třídy. Znovu publikujte serverový projekt.

        [Authorize]
        public class TodoItemController : TableController<TodoItem>

* **Back-end Node.js (prostřednictvím kódu Node.js)** :  

    K ověřování vyžadovat pro přístup k tabulce, přidejte následující řádek do skriptu serveru Node.js:

        table.access = 'authenticated';

    Další podrobnosti najdete v tématu [postupy: ověřování vyžadovat pro přístup k tabulkám](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Další stahování projektu pro rychlý start kód z vaší lokality najdete v tématu [postupy: stažení projektu pro rychlý start kódu Node.js back-end pomocí Git](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).
