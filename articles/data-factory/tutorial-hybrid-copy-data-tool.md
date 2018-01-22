---
title: "Kopírování místních dat pomocí nástroje pro kopírování dat Azure | Dokumentace Microsoftu"
description: "Vytvořte datovou továrnu Azure a pak pomocí nástroje pro kopírování dat zkopírujte data z místní databáze SQL Serveru do úložiště objektů blob v Azure."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/04/2018
ms.author: jingwang
ms.openlocfilehash: aba8b13d47b2b9236a0d5cc868e53780e894c406
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage-by-using-copy-data-tool"></a>Kopírování dat z místní databáze SQL Serveru do úložiště objektů blob v Azure pomocí nástroje pro kopírování dat
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Verze 2 – Preview](tutorial-hybrid-copy-data-tool.md)

V tomto kurzu pomocí webu Azure Portal vytvoříte datovou továrnu. Pak pomocí nástroje pro kopírování dat vytvoříte kanál, který kopírujte data z místní databáze SQL Serveru do úložiště objektů blob v Azure.

> [!NOTE]
> - Pokud se službou Azure Data Factory začínáte, přečtěte si téma [Seznámení se službou Azure Data Factory](introduction.md).
>
> - Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [úvod do služby Data Factory verze 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

V tomto kurzu provedete následující kroky:

> [!div class="checklist"]
> * Vytvoření datové továrny
> * Vytvoření kanálu pomocí nástroje pro kopírování dat
> * Monitorování spuštění aktivit a kanálu

## <a name="prerequisites"></a>Požadavky
### <a name="azure-subscription"></a>Předplatné Azure
Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

### <a name="azure-roles"></a>Role Azure
Pro vytvoření instancí datové továrny musí mít uživatelský účet, který použijete pro přihlášení k Azure, přiřazenou roli *přispěvatel* nebo *vlastník* předplatného Azure nebo musí být jeho *správcem*. 

Pokud chcete zobrazit oprávnění, která v předplatném máte, přejděte na web Azure Portal, v pravém horním rohu vyberte své uživatelské jméno a pak vyberte **Oprávnění**. Pokud máte přístup k několika předplatným, vyberte odpovídající předplatné. Ukázku pokynů pro přidání uživatele k roli najdete v článku věnovaném [přidání rolí](../billing/billing-add-change-azure-subscription-administrator.md).

### <a name="sql-server-2014-2016-and-2017"></a>SQL Server 2014, 2016 a 2017
V tomto kurzu použijete místní databázi SQL Serveru jako *zdrojové* úložiště dat. Kanál v datové továrně, který vytvoříte v tomto kurzu, kopíruje data z této místní databáze SQL Serveru (zdroj) do úložiště objektů blob v Azure (jímka). Ve své databázi SQL Serveru pak vytvoříte tabulku **emp** a vložíte do ní několik ukázkových záznamů. 

1. Spusťte aplikaci SQL Server Management Studio. Pokud na vašem počítači ještě není nainstalovaná, přejděte na stránku pro [stažení aplikace SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). 

2. Pomocí svých přihlašovacích údajů se přihlaste ke své instanci SQL Serveru. 

3. Vytvořte ukázkovou databázi. Ve stromovém zobrazení klikněte pravým tlačítkem na **Databáze** a pak vyberte **Nová databáze**. 
 
4. V okně **Nová databáze** zadejte název databáze a pak vyberte **OK**. 

5. Vytvořte tabulku **emp** a vložte do ní nějaká ukázková data spuštěním následujícího skriptu dotazu proti databázi: Ve stromovém zobrazení klikněte pravým tlačítkem na databázi, kterou jste vytvořili, a pak vyberte **Nový dotaz**.

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO
    
    INSERT INTO emp (FirstName, LastName) VALUES ('John', 'Doe')
    INSERT INTO emp (FirstName, LastName) VALUES ('Jane', 'Doe')
    GO
    ```

### <a name="azure-storage-account"></a>Účet služby Azure Storage
V tomto kurzu použijete účet úložiště Azure (konkrétně úložiště objektů blob v Azure) pro obecné účely jako cílové úložiště dat nebo úložiště dat jímky. Pokud nemáte účet úložiště Azure pro obecné účely, přečtěte si téma [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account), kde najdete pokyny k jeho vytvoření. Kanál v datové továrně, který vytvoříte v tomto kurzu, kopíruje data z místní databáze SQL Serveru (zdroj) do tohoto úložiště objektů blob v Azure (jímka). 

#### <a name="get-storage-account-name-and-account-key"></a>Získání názvu a klíče účtu úložiště
V tomto kurzu použijete název a klíč svého účtu úložiště Azure. Získejte název a klíč vašeho účtu úložiště pomocí následujícího postupu: 

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com) pomocí svého uživatelského jména a hesla Azure. 

2. V levém podokně vyberte **Další služby**, proveďte filtrování pomocí klíčového slova **úložiště** a pak vyberte **Účty úložiště**.

    ![Vyhledání účtu úložiště](media/tutorial-hybrid-copy-powershell/search-storage-account.png)

3. V seznamu účtů úložiště vyfiltrujte váš účet úložiště (pokud je to potřeba) a pak vyberte váš účet úložiště. 

4. V okně **Účet úložiště** vyberte **Přístupové klíče**.

    ![Získání názvu a klíče účtu úložiště](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)

5. Zkopírujte hodnoty polí **Název účtu úložiště** a **klíč1** a vložte je do Poznámkového bloku nebo jiného editoru pro pozdější použití v rámci kurzu. 

#### <a name="create-the-adftutorial-container"></a>Vytvoření kontejneru adftutorial 
V této části vytvoříte ve svém úložišti objektů blob v Azure kontejner objektů blob s názvem **adftutorial**. 

1. V okně **Účet úložiště** přepněte na **Přehled** a pak vyberte **Objekty blob**. 

    ![Výběr možnosti Objekty blob](media/tutorial-hybrid-copy-powershell/select-blobs.png)

2. V okně **Služba Blob** vyberte **Kontejner**. 

    ![Tlačítko pro přidání kontejneru](media/tutorial-hybrid-copy-powershell/add-container-button.png)

3. V okně **Nový kontejner** do pole **Název** zadejte **adftutorial** a pak vyberte **OK**. 

    ![Zadání názvu kontejneru](media/tutorial-hybrid-copy-powershell/new-container-dialog.png)

4. V seznamu kontejnerů vyberte **adftutorial**.  

    ![Výběr kontejneru](media/tutorial-hybrid-copy-powershell/seelct-adftutorial-container.png)

5. Okno **Kontejner** pro **adftutorial** nechte otevřené. Použijete ji k ověření výstupu na konci tohoto kurzu. Data Factory v tomto kontejneru vytvoří výstupní složku automaticky, takže ji nemusíte vytvářet.

    ![Okno Kontejner](media/tutorial-hybrid-copy-powershell/container-page.png)


## <a name="create-a-data-factory"></a>Vytvoření datové továrny

1. V nabídce vlevo klikněte na **Nový**, klikněte na **Data + analýzy** a pak na **Data Factory**. 
   
   ![Nový -> Objekt pro vytváření dat](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory-menu.png)
2. Na stránce **Nová datová továrna** jako **název** zadejte **ADFTutorialDataFactory**. 
      
     ![Stránka Nová datová továrna](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory.png)
 
   Název objektu pro vytváření dat Azure musí být **globálně jedinečný**. Pokud se zobrazí následující chyba pole názvu, změňte název datové továrny (například na vaše_jméno_ADFTutorialDataFactory). Pravidla pojmenování artefaktů služby Data Factory najdete v článku [Data Factory – pravidla pojmenování](naming-rules.md).
  
     ![Stránka Nová datová továrna](./media/tutorial-hybrid-copy-data-tool/name-not-available-error.png)
3. Vyberte své **předplatné** Azure, ve kterém chcete vytvořit datovou továrnu. 
4. Pro **Skupinu prostředků** proveďte jeden z následujících kroků:
     
      - Vyberte **Použít existující** a z rozevíracího seznamu vyberte existující skupinu prostředků. 
      - Vyberte **Vytvořit novou** a zadejte název skupiny prostředků.   
         
      Informace o skupinách prostředků najdete v článku [Použití skupin prostředků ke správě prostředků Azure](../azure-resource-manager/resource-group-overview.md).  
4. Jako **verzi** vyberte **V2 (Preview)**.
5. Vyberte **umístění** pro objekt pro vytváření dat. V rozevíracím seznamu se zobrazí pouze podporovaná umístění. Úložiště dat (Azure Storage, Azure SQL Database atd.) a výpočetní prostředí (HDInsight atd.) používané datovou továrnou můžou být v jiných umístěních nebo oblastech.
6. Zaškrtněte **Připnout na řídicí panel**.     
7. Klikněte na možnost **Vytvořit**.
8. Na řídicím panelu vidíte následující dlaždice se statusem: **Nasazování datové továrny**. 

    ![nasazování dlaždice datové továrny](media/tutorial-hybrid-copy-data-tool/deploying-data-factory.png)
9. Po vytvoření se zobrazí stránka **Datová továrna**, jak je znázorněno na obrázku.
   
   ![Domovská stránka objektu pro vytváření dat](./media/tutorial-hybrid-copy-data-tool/data-factory-home-page.png)
10. Kliknutím na dlaždici **Vytvořit a monitorovat** otevřete na samostatné kartě uživatelské rozhraní služby Azure Data Factory. 

## <a name="use-copy-data-tool-to-create-a-pipeline"></a>Vytvoření kanálu pomocí nástroje pro kopírování dat

1. Na stránce Začínáme klikněte na dlaždici **Kopírovat data** a spusťte nástroj pro kopírování dat. 

   ![Dlaždice nástroje pro kopírování dat](./media/tutorial-hybrid-copy-data-tool/copy-data-tool-tile.png)
2. Na stránce **Vlastnosti** nástroje pro kopírování dat jako **Název úlohy** zadejte **CopyFromOnPremSqlToAzureBlobPipeline** a klikněte na **Další**. Nástroj pro kopírování dat vytvoří kanál s názvem, který zadáte do tohoto pole. 
    
   ![Stránka Vlastnosti](./media/tutorial-hybrid-copy-data-tool/properties-page.png)
3. Na stránce **Zdrojové úložiště dat** vyberte **SQL Server** a klikněte na **Další**. Možná se budete muset posunout dolů, aby se v seznamu zobrazil **SQL Server**. 

   ![Stránka Zdrojové úložiště dat](./media/tutorial-hybrid-copy-data-tool/select-source-data-store.png)
4. Jako **Název připojení** zadejte **SqlServerLinkedService** a klikněte na odkaz **Vytvořit prostředí Integration Runtime**. Musíte vytvořit místní prostředí Integration Runtime, stáhnout ho na svůj počítač a zaregistrovat ho ve službě Data Factory. Místní prostředí Integration Runtime kopíruje data mezi vaším místním prostředím a cloudem Azure.

   ![Odkaz Vytvořit prostředí Integration Runtime](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-link.png)
5. V dialogovém okně **Vytvořit prostředí Integration Runtime** do pole **Název** zadejte **TutorialIntegrationRuntime** a klikněte na **Vytvořit**. 

   ![Dialogové okno Vytvořit prostředí Integration Runtime](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-dialog.png)
6. Klikněte na **Spustit expresní instalaci na tomto počítači**. Tato akce nainstaluje prostředí Integration Runtime na vašem počítači a zaregistruje ho ve službě Data Factory. Případně můžete využít možnost ruční instalace a stáhnout instalační soubor, spustit ho a použít klíč k registraci prostředí Integration Runtime. 

    ![Odkaz Vytvořit prostředí Integration Runtime](./media/tutorial-hybrid-copy-data-tool/launch-express-setup-link.png)
7. Spusťte staženou aplikaci. V okně se zobrazí **stav** expresní instalace. 

    ![Stav expresní instalace](./media/tutorial-hybrid-copy-data-tool/express-setup-status.png)
8. Ověřte, že je v poli **Prostředí Integration Runtime** vybraný modul **TutorialIntegrationRuntime**.

    ![Vybrané prostředí Integration Runtime](./media/tutorial-hybrid-copy-data-tool/integration-runtime-selected.png)
9. V části **Zadejte místní databázi SQL Serveru** proveďte následující kroky: 

    1. Jako **Název připojení** zadejte **OnPremSqlLinkedService**.
    2. Jako **Název serveru** zadejte název vašeho místního SQL Serveru.
    3. Jako **Název databáze** zadejte název vaší místní databáze. 
    4. Jako **Typ ověřování** vyberte odpovídající typ ověřování.
    5. Jako **Uživatelské jméno** zadejte jméno uživatele s přístupem k místnímu SQL Serveru.
    6. Zadejte **heslo** pro tohoto uživatele. 
10. Na stránce **Vyberte tabulky, ze kterých se mají kopírovat data, nebo použijte vlastní dotaz** vyberte v seznamu tabulku **[dbo].[emp]** a klikněte na **Další**. 

    ![Výběr tabulky emp](./media/tutorial-hybrid-copy-data-tool/select-emp-table.png)
11. Na stránce **Cílové úložiště dat** vyberte **Azure Blob Storage** a klikněte na **Další**.

    ![Výběr služby Azure Blob Storage](./media/tutorial-hybrid-copy-data-tool/select-destination-data-store.png)
12. Na stránce **Zadejte účet služby Azure Blob Storage** proveďte následující kroky: 

    1. Jako **Název připojení** zadejte **AzureStorageLinkedService**.
    2. Jako **Název účtu úložiště** vyberte z rozevíracího seznamu svůj účet úložiště Azure. 
    3. Klikněte na **Další**.

        ![Výběr služby Azure Blob Storage](./media/tutorial-hybrid-copy-data-tool/specify-azure-blob-storage-account.png)
13. Na stránce **Zvolte výstupní soubor nebo složku** proveďte následující kroky: 
    
    1. Jako **cestu ke složce** zadejte **adftutorial/fromonprem**. Kontejner **adftutorial** jste vytvořili jako součást požadavků. Pokud výstupní složka neexistuje, služba Data Factory ji automaticky vytvoří. Můžete také použít tlačítko **Procházet** a přejít k úložišti objektů blob a jeho kontejnerům a složkám. Všimněte si, že ve výchozím nastavení je název výstupního souboru nastavený na **dbo.emp**.
        
        ![Zvolte výstupní soubor nebo složku](./media/tutorial-hybrid-copy-data-tool/choose-output-file-folder.png)
14. Na stránce **Nastavení formátu souboru**  klikněte na **Další**. 

    ![Stránka Nastavení formátu souboru](./media/tutorial-hybrid-copy-data-tool/file-format-settings-page.png)
15. Na stránce **Nastavení** klikněte na **Další**. 

    ![Stránka Nastavení](./media/tutorial-hybrid-copy-data-tool/settings-page.png)
16. Na stránce **Souhrn** zkontrolujte hodnoty všech nastavení a klikněte na **Další**. 

    ![Stránka souhrnu](./media/tutorial-hybrid-copy-data-tool/summary-page.png)
17. Na stránce **Nasazení** klikněte na **Monitorovat** a začněte monitorovat vytvořený kanál nebo úlohu.

    ![Stránka Nasazení](./media/tutorial-hybrid-copy-data-tool/deployment-page.png)
18. Na kartě **Monitorování** můžete zobrazit stav kanálu, který jste vytvořili. Pomocí odkazů ve sloupci **Akce** můžete zobrazit spuštění aktivit související se spuštěním kanálu nebo spustit kanál znovu. 

    ![Spuštění kanálu](./media/tutorial-hybrid-copy-data-tool/monitor-pipeline-runs.png)
19. Klikněte na odkaz **Zobrazit spuštění aktivit** ve sloupci **Akce** a zobrazte spuštění aktivit související se spuštěním kanálu. Pokud chcete zobrazit podrobnosti o operaci kopírování, klikněte na odkaz **Podrobnosti** (ikona brýlí) ve sloupci **Akce**. Zpět na zobrazení spuštění kanálu můžete přepnout kliknutím na **Kanály** v horní části.

    ![Spuštění aktivit](./media/tutorial-hybrid-copy-data-tool/monitor-activity-runs.png)
20. Ověřte, že složka **fromonprem** kontejneru **adftutorial** obsahuje výstupní soubor.   
 
    ![Výstupní objekt blob](./media/tutorial-hybrid-copy-data-tool/output-blob.png)
21. Kliknutím na kartu **Upravit** na levé straně přepněte do režimu úprav. Pomocí editoru můžete aktualizovat propojené služby, datové sady a kanály vytvořené nástrojem. Kliknutím na **Kód** zobrazíte kód JSON přidružený k entitě otevřené v editoru. Podrobnosti o úpravách těchto entit v uživatelském rozhraní služby Data Factory najdete ve [verzi tohoto kurzu pro Azure Portal](tutorial-copy-data-portal.md).

    ![Karta Upravit](./media/tutorial-hybrid-copy-data-tool/edit-tab.png)


## <a name="next-steps"></a>Další kroky
Kanál v této ukázce kopíruje data z místní databáze SQL Serveru do úložiště objektů blob v Azure. Naučili jste se tyto postupy: 

> [!div class="checklist"]
> * Vytvoření datové továrny
> * Vytvoření kanálu pomocí nástroje pro kopírování dat
> * Monitorování spuštění aktivit a kanálu

Seznam úložišť dat podporovaných službou Data Factory najdete v tématu popisujícím [podporovaná úložiště dat](copy-activity-overview.md#supported-data-stores-and-formats).

Pokud se chcete dozvědět víc o hromadném kopírování dat ze zdroje do cíle, přejděte k následujícímu kurzu:

> [!div class="nextstepaction"]
>[Hromadné kopírování dat](tutorial-bulk-copy-portal.md)