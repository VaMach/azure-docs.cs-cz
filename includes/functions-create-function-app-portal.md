1. Klikněte **nový** nalezeno tlačítko v levém horním rohu portálu Azure, pak vyberte **výpočetní** > **aplikaci funkce**. 

    ![Vytvoření aplikace pro funkce na portálu Azure](./media/functions-create-function-app-portal/function-app-create-flow.png)

2. Použijte nastavení aplikace funkce uvedeného v následující tabulce bitovou kopii.

    ![Definovat nové nastavení aplikace – funkce](./media/functions-create-function-app-portal/function-app-create-flow2.png)

    | Nastavení      | Navrhovaná hodnota  | Popis                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Název aplikace** | Globálně jedinečný název | Název identifikující novou aplikaci Function App. Platnými znaky jsou `a-z`, `0-9`, a `-`.  | 
    | **Předplatné** | Vaše předplatné | Předplatné, pod kterým je vytvoření této nové funkce aplikace. | 
    | **[Skupina prostředků](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Název nové skupiny prostředků, ve které chcete vytvořit aplikaci Function App. | 
    | **OS** | Windows | Hostování bez serveru je aktuálně k dispozici pouze při spuštění v systému Windows. Linux hostování, najdete v části [vytvoření první funkce systémem Linux pomocí příkazového řádku Azure CLI](../articles/azure-functions/functions-create-first-azure-function-azure-cli-linux.md). |
    | **[Plán hostování](../articles/azure-functions/functions-scale.md)** |   Plán Consumption | Plán hostování, který určuje způsob přidělování prostředků aplikaci Function App. Ve výchozím **plánu Consumption** se prostředky přidávají dynamicky podle požadavků příslušných funkcí. V tomto [bez serveru](https://azure.microsoft.com/overview/serverless-computing/) hostování, platíte jenom čas spuštění funkcí.   |
    | **Umístění** | Západní Evropa | Vyberte [oblast](https://azure.microsoft.com/regions/) okolo vás nebo v blízkosti dalších služeb funkcí přístup. |
    | **[Účet úložiště](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account)** |  Globálně jedinečný název |  Název nového účtu úložiště, který bude aplikace Function App používat. Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena. Můžete taky použít existující účet. |

1. Po kliknutí na **Vytvořit** se nová aplikace Function App zřídí a nasadí. Kliknutím na ikonu oznámení v pravém horním rohu na portálu můžete sledovat stav nasazení. 

    ![Definovat nové nastavení aplikace – funkce](./media/functions-create-function-app-portal/function-app-create-notification.png)

    Kliknutím na tlačítko **přejděte k prostředku** přejdete do nové aplikace funkce.
