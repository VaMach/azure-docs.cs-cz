1. V **Průzkumníku řešení** klikněte pravým tlačítkem na požadovaný projekt a vyberte **Publikovat**. Zvolte **vytvořit nový** a potom **publikování**. 

    ![Publikování a vytvoření nové aplikace funkcí](./media/functions-vstools-publish/functions-vstools-publish-new-function-app.png)

2. Pokud jste již nepřipojili Visual Studio k účtu Azure, vyberte **přidat účet...** .  

3. V dialogovém okně **Vytvořit službu App Service** použijte nastavení **Hostování**, jak je uvedeno v následující tabulce: 

    ![Místní modul runtime Azure](./media/functions-vstools-publish/functions-vstools-publish.png)

    | Nastavení      | Navrhovaná hodnota  | Popis                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Název aplikace** | Globálně jedinečný název | Název jednoznačně identifikující novou aplikaci funkcí. |
    | **Předplatné** | Vaše předplatné | Předplatné Azure, které se má použít. |
    | **[Skupina prostředků](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Název skupiny prostředků, ve které chcete vytvořit aplikaci funkcí. Zvolte **nový** vytvořit novou skupinu prostředků.|
    | **[Plán služby App Service](../articles/azure-functions/functions-scale.md)** | Plán Consumption | Ujistěte se, že jste vybrali **spotřeba** pod **velikost** po kliknutí na tlačítko **nový** k vytvoření nového plánu. Také vyberte **umístění** v [oblast](https://azure.microsoft.com/regions/) okolo vás nebo v blízkosti dalších služeb funkcí přístup.  |

    >[!NOTE]
    >Functions runtime je požadován účet úložiště Azure. Z toho důvodu se vytvoří nový účet úložiště Azure za vás při vytvoření aplikace pro funkce.

4. Klikněte na tlačítko **vytvořit** vytvořte aplikaci funkce a související prostředky v Azure s těmito nastaveními a nasazení projektu kódu funkce. 

5. Po dokončení nasazení poznamenejte **adresa URL webu** hodnotu, která je adresa funkce aplikace v Azure.

    ![Místní modul runtime Azure](./media/functions-vstools-publish/functions-vstools-publish-profile.png)
