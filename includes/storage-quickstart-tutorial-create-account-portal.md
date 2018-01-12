## <a name="create-a-storage-account-by-using-the-azure-portal"></a>Vytvořit účet úložiště pomocí portálu Azure

Nejprve vytvořte nový účet úložiště pro obecné účely, který použijete v tomto rychlém startu. 

1. Přejděte na [portál Azure](https://portal.azure.com/#create/Microsoft.StorageAccount-ARM) a přihlaste se pomocí účtu Azure. 
2. Zadejte jedinečný název účtu úložiště. Při pojmenování účtu úložiště mějte na paměti tato pravidla:
    - Název musí být 3 až 24 znaků.
    - Název může obsahovat jenom číslice a malá písmena.
3. Ujistěte se, že jsou nastavené následující výchozí hodnoty: 
    - **Model nasazení** je nastaven na **Resource Manager**.
    - **Druh účtu** je nastavený na **Pro obecné účely**.
    - **Výkon** je nastavená na úroveň **Standard**.
    - **Replikace** je nastavená na **Místně redundantní úložiště (LRS)**.
4. Vyberte své předplatné. 
5. V části **Skupina prostředků** vytvořte novou skupinu prostředků s jedinečným názvem. 
6. Vyberte umístění, které chcete použít pro váš účet úložiště.
7. Vyberte **připnout na řídicí panel** a vyberte **vytvořit** k vytvoření účtu úložiště. 

Po vytvoření účtu úložiště je připnutá na řídicí panel. Vyberte, ho otevřete. V části **nastavení**, vyberte **přístupové klíče**. Vyberte primární klíč a zkopírujte do schránky řetězec přidružené připojení. Vložte řetězec do textového editoru pro pozdější použití.
