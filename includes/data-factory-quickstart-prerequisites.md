## <a name="prerequisites"></a>Požadavky

### <a name="azure-subscription"></a>Předplatné Azure
Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

### <a name="azure-roles"></a>Role Azure
Pro vytvoření instancí Data Factory musí být uživatelský účet, který použijete pro přihlášení k Azure, členem rolí **přispěvatel** nebo **vlastník** nebo **správcem** předplatného Azure. Na webu Azure Portal klikněte na **uživatelské jméno** v pravém horním rohu a vyberte **Oprávnění**. Zobrazí se oprávnění, která v příslušném předplatném máte. Pokud máte přístup k několika předplatným, vyberte odpovídající předplatné. Ukázku pokynů pro přidání uživatele k roli najdete v článku věnovaném [přidání rolí](../articles/billing/billing-add-change-azure-subscription-administrator.md).

### <a name="azure-storage-account"></a>Účet služby Azure Storage
V tomto rychlém startu použijete účet služby Azure Storage (konkrétně služby Blob Storage) pro obecné účely jako **zdrojové** i **cílové úložiště dat**. Pokud nemáte účet úložiště Azure pro obecné účely, přečtěte si téma popisující [Vytvoření účtu úložiště](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account). 

#### <a name="get-storage-account-name-and-account-key"></a>Získání názvu a klíče účtu úložiště
V tomto rychlém startu použijete název a klíč svého účtu úložiště Azure. Následující postup předvádí kroky k získání názvu a klíče vašeho účtu úložiště. 

1. Spusťte webový prohlížeč a přejděte na [Azure Portal](https://portal.azure.com). Přihlaste se pomocí svého uživatelského jména a hesla Azure. 
2. V nabídce vlevo klikněte na **Další služby >**, použijte filtr s klíčovým slovem **úložiště** a vyberte **Účty úložiště**.

    ![Vyhledání účtu úložiště](media/data-factory-quickstart-prerequisites/search-storage-account.png)
3. V seznamu účtů úložiště vyfiltrujte váš účet úložiště (pokud je to potřeba) a pak vyberte **váš účet úložiště**. 
4. Na stránce **Účet úložiště** vyberte v nabídce **Přístupové klíče**.

    ![Získání názvu a klíče účtu úložiště](media/data-factory-quickstart-prerequisites/storage-account-name-key.png)
5. Zkopírujte do schránky hodnoty z polí **Název účtu úložiště** a **klíč1**. Vložte je do Poznámkového bloku nebo jiného editoru a uložte je. Použijete je později v tomto rychlém startu.   

#### <a name="create-input-folder-and-files"></a>Vytvoření vstupní složky a souborů
V této části vytvoříte ve svém úložišti objektů blob v Azure kontejner objektů blob s názvem **adftutorial**. Potom v kontejneru vytvoříte složku **input** a nahrajete do ní ukázkový soubor. 

1. Na stránce **Účet úložiště** přepněte na **Přehled** a potom klikněte na **Objekty blob**. 

    ![Výběr možnosti Objekty blob](media/data-factory-quickstart-prerequisites/select-blobs.png)
2. Na stránce **Blob Service** klikněte na panelu nástrojů na **+ Kontejner**. 

    ![Tlačítko pro přidání kontejneru](media/data-factory-quickstart-prerequisites/add-container-button.png)    
3. V dialogovém okně **Nový kontejner** jako název zadejte **adftutorial** a klikněte na **OK**. 

    ![Zadání názvu kontejneru](media/data-factory-quickstart-prerequisites/new-container-dialog.png)
4. V seznamu kontejnerů klikněte na **adftutorial**. 

    ![Výběr kontejneru](media/data-factory-quickstart-prerequisites/seelct-adftutorial-container.png)
1. Na stránce **Kontejner** klikněte na panelu nástrojů na **Nahrát**.  

    ![Tlačítko Nahrát](media/data-factory-quickstart-prerequisites/upload-toolbar-button.png)
6. Na stránce **Nahrát objekt blob** klikněte na **Upřesnit**.

    ![Kliknutí na odkaz Upřesnit](media/data-factory-quickstart-prerequisites/upload-blob-advanced.png)
7. Spusťte **Poznámkový blok** a vytvořte soubor nazvaný **emp.txt** s následujícím obsahem: Uložte ho do složky **c:\ADFv2QuickStartPSH**. Pokud složka **ADFv2QuickStartPSH** neexistuje, vytvořte ji.
    
    ```
    John, Doe
    Jane, Doe
    ```    
8. Na webu Azure Portal na stránce **Nahrát objekt blob** vyberte **emp.txt** v poli **Soubory**. 
9. Jako hodnotu pole **Nahrát do složky** zadejte **input**. 

    ![Nastavení pro nahrání objektu blob](media/data-factory-quickstart-prerequisites/upload-blob-settings.png)    
10. Potvrďte, že je nastavená složka **input** a soubor **emp.txt** a klikněte na **Nahrát**.
11. Měli byste vidět soubor **emp.txt** a stav nahrávání v seznamu. 
12. Zavřete okno **Nahrát objekt blob** kliknutím na **X** v rohu. 

    ![Zavření okna Nahrát objekt blob](media/data-factory-quickstart-prerequisites/close-upload-blob.png)
1. Stránku **kontejneru** nechte otevřenou. Použijete ji k ověření výstupu na konci tohoto rychlého startu.