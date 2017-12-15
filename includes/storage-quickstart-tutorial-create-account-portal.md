## <a name="create-a-storage-account-using-the-azure-portal"></a>Vytvoření účtu úložiště pomocí webu Azure Portal

Nejprve vytvořte nový účet úložiště pro obecné účely, který použijete v tomto rychlém startu. 

1. Přejděte na [Azure Portal](https://portal.azure.com) a přihlaste se pomocí svého účtu Azure. 
2. V nabídce centra vyberte **Nový** > **Úložiště** > **Účet úložiště – Blob, File, Table, Queue**. 
3. Zadejte jedinečný název účtu úložiště. Při pojmenování účtu úložiště mějte na paměti tato pravidla:
    - Název musí být dlouhý 3 až 24 znaků.
    - Název může obsahovat pouze číslice a malá písmena.
4. Ujistěte se, že jsou nastavené následující výchozí hodnoty: 
    - **Model nasazení** je nastavený na **Resource Manager**.
    - **Druh účtu** je nastavený na **Pro obecné účely**.
    - **Výkon** je nastavená na úroveň **Standard**.
    - **Replikace** je nastavená na **Místně redundantní úložiště (LRS)**.
5. Vyberte své předplatné. 
6. V části **Skupina prostředků** vytvořte novou skupinu prostředků s jedinečným názvem. 
7. Vyberte **Umístění**, které se použije pro účet úložiště.
8. Zaškrtněte políčko **Připnout na řídicí panel** a kliknutím na **Vytvořit** vytvořte účet úložiště. 

Účet úložiště se po vytvoření připne na řídicí panel. Klikněte na něj a otevřete ho. V části **Nastavení** klikněte na **Přístupové klíče**. Vyberte primární přístupový klíč, zkopírujte do schránky přidružený **Připojovací řetězec** a vložte ho do textového editoru pro pozdější použití.
