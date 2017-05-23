1. Klikněte na tlačítko **Nový** v levém horním rohu webu Azure Portal.

1. Klikněte na **Vypočítat** > **Function App** a vyberte svoje **Předplatné**. Potom použijte nastavení aplikace Function App uvedené v tabulce.

    ![Vytvoření aplikace Function App na webu Azure Portal](./media/functions-create-function-app-portal/function-app-create-flow.png)

    | Nastavení      | Navrhovaná hodnota  | Popis                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Název aplikace** | Globálně jedinečný název | Název identifikující novou aplikaci Function App. | 
    | **[Skupina prostředků](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Název nové skupiny prostředků, ve které chcete vytvořit aplikaci Function App. | 
    | **[Plán hostování](../articles/azure-functions/functions-scale.md)** |   Plán Consumption | Plán hostování, který určuje způsob přidělování prostředků aplikaci Function App. Ve výchozím **plánu Consumption** se prostředky přidávají dynamicky podle požadavků příslušných funkcí. Platíte jenom dobu, kdy jsou funkce spuštěné.   |
    | **Umístění** | Západní Evropa | Vyberte umístění ve vaší blízkosti nebo v blízkosti jiných služeb, ke kterým budou funkce získávat přístup. |
    | **[Účet úložiště](../articles/storage/storage-create-storage-account.md#create-a-storage-account)** |  Globálně jedinečný název |  Název nového účtu úložiště, který bude aplikace Function App používat. Můžete taky použít existující účet. |

1. Po kliknutí na **Vytvořit** se nová aplikace Function App zřídí a nasadí.