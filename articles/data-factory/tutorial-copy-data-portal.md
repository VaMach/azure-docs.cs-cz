---
title: "Vytvoření kanálu Data Factory pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "Tento kurz obsahuje podrobné pokyny k vytvoření datové továrny s kanálem pomocí webu Azure Portal. Kanál pomocí aktivity kopírování kopíruje data z úložiště objektů blob v Azure do databáze SQL Azure. "
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: 7486e7c6816538fc120fd0b0a8bea0b006fb21f0
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="copy-data-from-azure-blob-to-azure-sql-database-using-azure-data-factory"></a>Kopírování dat z objektu blob Azure do Azure SQL Database pomocí Azure Data Factory
V tomto kurzu vytvoříte datovou továrnu pomocí uživatelského rozhraní služby Azure Data Factory. Kanál v této datové továrně kopíruje data ze služby Azure Blob Storage do služby Azure SQL Database. Schéma konfigurace v tomto kurzu se vztahuje na kopírování z úložiště dat založeného na souborech do relačního úložiště dat. Seznam úložišť dat, která jsou podporovaná jako zdroje a jímky, najdete v tabulce [podporovaných úložišť dat](copy-activity-overview.md#supported-data-stores-and-formats).

> [!NOTE]
> - Pokud se službou Azure Data Factory začínáte, přečtěte si téma [Seznámení se službou Azure Data Factory](introduction.md).
>
> - Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [úvod do služby Data Factory verze 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

V tomto kurzu provedete následující kroky:

> [!div class="checklist"]
> * Vytvoření datové továrny
> * Vytvoření kanálu s aktivitou kopírování
> * Testovací spuštění kanálu
> * Ruční aktivace kanálu
> * Aktivace kanálu podle plánu
> * Monitorování spuštění aktivit a kanálu

## <a name="prerequisites"></a>Požadavky
* **Předplatné Azure**. Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.
* **Účet služby Azure Storage**. Úložiště objektů blob použijete jako **zdrojové** úložiště dat. Pokud nemáte účet úložiště Azure, přečtěte si článek [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account), kde najdete kroky pro jeho vytvoření.
* **Azure SQL Database**. Tuto databázi použijete jako úložiště dat **jímky**. Pokud Azure SQL Database nemáte, přečtěte si článek věnovaný [vytvoření databáze Azure SQL](../sql-database/sql-database-get-started-portal.md), kde najdete kroky pro její vytvoření.

### <a name="create-a-blob-and-a-sql-table"></a>Vytvoření objektu blob a tabulky SQL

Teď připravte objekt blob Azure a Azure SQL Database pro tento kurz provedením následující kroků:

#### <a name="create-a-source-blob"></a>Vytvoření zdrojového objektu blob

1. Spusťte Poznámkový blok. Zkopírujte následující text a uložte ho na disk jako soubor **emp.txt**.

    ```
    John,Doe
    Jane,Doe
    ```

2. Ve svém úložišti objektů blob v Azure vytvořte kontejner **adftutorial**. V tomto kontejneru vytvořte složku **input**. Pak do složky **input** nahrajte soubor **emp.txt**. K provedení těchto úloh použijte Azure Portal nebo nástroj, jako je například [Průzkumník služby Azure Storage](http://storageexplorer.com/).

#### <a name="create-a-sink-sql-table"></a>Vytvoření tabulky SQL jímky

1. Pomocí následujícího skriptu SQL vytvořte tabulku **dbo.emp** v Azure SQL Database.

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

2. Povolte službám Azure přístup k SQL serveru. Zkontrolujte, že nastavení **Povolit přístup ke službám Azure** je pro server SQL Azure **ZAPNUTÉ**, aby služba Data Factory mohla na server Azure SQL zapisovat data. Pokud chcete toto nastavení ověřit a zapnout, proveďte následující kroky:

    1. Klikněte na **Další služby** na levé straně a potom klikněte na **Servery SQL**.
    2. Vyberte svůj server a v části **NASTAVENÍ** klikněte na **Brána firewall**.
    3. Na stránce **Nastavení brány firewall** klikněte na **ZAPNUTO** u možnosti **Povolit přístup ke službám Azure**.

## <a name="create-a-data-factory"></a>Vytvoření datové továrny
V tomto kroku vytvoříte datovou továrnu a spustíte uživatelské rozhraní služby Azure Data Factory, ve kterém vytvoříte v této datové továrně kanál. 

1. V nabídce vlevo klikněte na **Nový**, klikněte na **Data + analýzy** a pak na **Data Factory**. 
   
   ![Nový -> Objekt pro vytváření dat](./media/tutorial-copy-data-portal/new-azure-data-factory-menu.png)
2. Na stránce **Nová datová továrna** jako **název** zadejte **ADFTutorialDataFactory**. 
      
     ![Stránka Nová datová továrna](./media/tutorial-copy-data-portal/new-azure-data-factory.png)
 
   Název objektu pro vytváření dat Azure musí být **globálně jedinečný**. Pokud se zobrazí následující chyba pole názvu, změňte název datové továrny (například na vaše_jméno_ADFTutorialDataFactory). Pravidla pojmenování artefaktů služby Data Factory najdete v článku [Data Factory – pravidla pojmenování](naming-rules.md).
  
     ![Stránka Nová datová továrna](./media/tutorial-copy-data-portal/name-not-available-error.png)
3. Vyberte své **předplatné** Azure, ve kterém chcete vytvořit datovou továrnu. 
4. Pro **Skupinu prostředků** proveďte jeden z následujících kroků:
     
      - Vyberte **Použít existující** a z rozevíracího seznamu vyberte existující skupinu prostředků. 
      - Vyberte **Vytvořit novou** a zadejte název skupiny prostředků.   
         
        Informace o skupinách prostředků najdete v článku [Použití skupin prostředků ke správě prostředků Azure](../azure-resource-manager/resource-group-overview.md).  
4. Jako **verzi** vyberte **V2 (Preview)**.
5. Vyberte **umístění** pro objekt pro vytváření dat. V rozevíracím seznamu se zobrazí pouze podporovaná umístění. Úložiště dat (Azure Storage, Azure SQL Database atd.) a výpočetní prostředí (HDInsight atd.) používané datovou továrnou mohou být v jiných oblastech.
6. Zaškrtněte **Připnout na řídicí panel**.     
7. Klikněte na možnost **Vytvořit**.      
8. Na řídicím panelu vidíte následující dlaždice se statusem: **Nasazování datové továrny**. 

    ![nasazování dlaždice datové továrny](media/tutorial-copy-data-portal/deploying-data-factory.png)
9. Po vytvoření se zobrazí stránka **Datová továrna**, jak je znázorněno na obrázku.
   
   ![Domovská stránka objektu pro vytváření dat](./media/tutorial-copy-data-portal/data-factory-home-page.png)
10. Kliknutím na dlaždici **Vytvořit a monitorovat** otevřete na samostatné kartě uživatelské rozhraní služby Azure Data Factory.

## <a name="create-a-pipeline"></a>Vytvoření kanálu
V tomto kroku vytvoříte v datové továrně kanál s aktivitou kopírování. Aktivita kopírování kopíruje data ze služby Azure Blob Storage do služby Azure SQL Database. V [kurzu Rychlý start](quickstart-create-data-factory-portal.md) jste vytvořili kanál pomocí tohoto postupu:

1. Vytvoření propojené služby 
2. Vytvoření vstupní a výstupní datové sady
3. Následné vytvoření kanálu

V tomto kurzu začnete vytvořením kanálu a propojené služby a datové sady vytvoříte, když je budete potřebovat ke konfiguraci kanálu. 

1. Na stránce Začínáme klikněte na dlaždici **Vytvořit kanál**. 

   ![Dlaždice Vytvořit kanál](./media/tutorial-copy-data-portal/create-pipeline-tile.png)
3. V okně **Vlastnosti** kanálu nastavte **název** kanálu na **CopyPipeline**.

    ![Název kanálu](./media/tutorial-copy-data-portal/pipeline-name.png)
4. Na panelu nástrojů **Aktivity** rozbalte kategorii **Tok dat** a přetáhněte aktivitu **Kopírování** z panelu nástrojů na plochu návrháře kanálu. 

    ![Přetažení aktivity kopírování](./media/tutorial-copy-data-portal/drag-drop-copy-activity.png)
5. Na kartě **Obecné** v okně **Vlastnosti** zadejte jako název aktivity **CopyFromBlobToSql**.

    ![Název aktivity](./media/tutorial-copy-data-portal/activity-name.png)
6. Přepněte na kartu **Zdroj**. Kliknutím na **+ Nová** vytvořte zdrojovou datovou sadu. 

    ![Nabídka Nová zdrojová datová sada](./media/tutorial-copy-data-portal/new-source-dataset-button.png)
7. V okně **Nová datová sada** vyberte **Azure Blob Storage** a klikněte na **Dokončit**. Zdrojem dat je úložiště objektů blob v Azure, takže jako zdrojovou datovou sadu vyberete službu Azure Blob Storage. 

    ![Výběr služby Azure Blob Storage](./media/tutorial-copy-data-portal/select-azure-storage.png)
8. V aplikaci se otevře nová **karta** s názvem **AzureBlob1**.

    ![Karta AzureBlob1 ](./media/tutorial-copy-data-portal/new-tab-azure-blob1.png)        
9. Na kartě **Obecné** v dolní části okna **Vlastnosti** jako **název** zadejte **SourceBlobDataset**.

    ![Název datové sady](./media/tutorial-copy-data-portal/dataset-name.png)
10. V okně Vlastnosti přepněte na kartu **Připojení**.   

    ![Karta Připojení](./media/tutorial-copy-data-portal/source-dataset-connection-tab.png)
11. Klikněte na **+ Nová** vedle textového pole **Propojená služba**. Propojená služba propojuje úložiště dat nebo výpočetní prostředí s datovou továrnou. V tomto případě vytvoříte propojenou službu Azure Storage, která propojí váš účet služby Azure Storage s úložištěm dat. Propojená služba obsahuje informace o připojení, které služba Data Factory používá pro připojení k úložišti objektů blob za běhu. Datová sada určuje kontejner, složku a soubor (volitelné) obsahující zdrojová data. 

    ![Tlačítko Nová propojená služba](./media/tutorial-copy-data-portal/source-dataset-new-linked-service-button.png)
12. V okně **Nová propojená služba** proveďte následující kroky: 

    1. Do pole **Název** zadejte **AzureStorageLinkedService**. 
    2. V poli **Název účtu úložiště** vyberte svůj účet úložiště Azure.
    3. Klikněte na **Test připojení** a otestujte připojení k účtu služby Azure Storage.  
    4. Kliknutím na **Uložit** propojenou službu uložte.

        ![Nová propojená služba Azure Storage](./media/tutorial-copy-data-portal/new-azure-storage-linked-service.png)
13. Vedle pole **Cesta k souboru** klikněte na **Procházet**.  

    ![Tlačítko pro procházení souborů](./media/tutorial-copy-data-portal/file-browse-button.png)
14. Přejděte do složky **adftutorial/input**, vyberte soubor **emp.txt** a klikněte na **Dokončit**. Případně můžete na soubor emp.txt dvakrát kliknout. 

    ![Výběr vstupního souboru](./media/tutorial-copy-data-portal/select-input-file.png)
15. Ověřte, že **Formát souboru** je nastavený na **Textový formát** a **oddělovač sloupců** je nastavený na **Čárka (`,`)**. Pokud se ve zdrojovém souboru používají jiné oddělovače řádků a sloupců, můžete vedle pole **Formát souboru** kliknout na **Rozpoznat formát textu**. Nástroj pro kopírování dat pro vás automaticky rozpozná formát souboru a oddělovače. Tyto hodnoty však můžete přepsat. Kliknutím na **Náhled dat** můžete zobrazit náhled dat na této stránce.

    ![Rozpoznat formát textu](./media/tutorial-copy-data-portal/detect-text-format.png)
17. V okně Vlastnosti přepněte na kartu **Schéma** a klikněte na **Importovat schéma**. Všimněte si, že aplikace ve zdrojovém souboru rozpoznala dva sloupce. Tady importujete schéma, abyste mohli namapovat sloupce ze zdrojového úložiště dat na sloupce v úložišti dat jímky. Pokud nepotřebujete mapovat sloupce, můžete tento krok přeskočit. Pro účely tohoto kurzu schéma naimportujte.

    ![Rozpoznání schématu zdroje](./media/tutorial-copy-data-portal/detect-source-schema.png)  
19. Teď přepněte na **kartu s kanálem** nebo klikněte na kanál ve **stromovém zobrazení** vlevo.  

    ![Karta Kanál](./media/tutorial-copy-data-portal/pipeline-tab.png)
20. V okně Vlastnosti ověřte, že je v poli Zdrojová datová sada vybraná datová sada **SourceBlobDataset**. Kliknutím na **Náhled dat** můžete zobrazit náhled dat na této stránce. 
    
    ![Zdrojová datová sada](./media/tutorial-copy-data-portal/source-dataset-selected.png)
21. Přepněte na kartu **Jímka** a kliknutím na **Nová** vytvořte datovou sadu jímky. 

    ![Nabídka Nová datová sada jímky](./media/tutorial-copy-data-portal/new-sink-dataset-button.png)
22. V okně **Nová datová sada** vyberte **Azure SQL Database** a klikněte na **Dokončit**. V tomto kurzu kopírujete data do databáze SQL Azure. 

    ![Výběr služby Azure SQL Database](./media/tutorial-copy-data-portal/select-azure-sql-database.png)
23. Na kartě **Obecné** v okně Vlastnosti nastavte název na **OutputSqlDataset**. 
    
    ![Název výstupní datové sady](./media/tutorial-copy-data-portal/output-dataset-name.png)
24. Přepněte na kartu **Připojení** a v části **Propojená služba** klikněte na **Nová**. Datová sada musí být přidružená k propojené službě. Propojená služba obsahuje připojovací řetězec, který služba Data Factory používá pro připojení k databázi SQL Azure za běhu. Datová sada určuje kontejner, složku a soubor (volitelné), do kterého se data kopírují. 
    
    ![Tlačítko Nová propojená služba](./media/tutorial-copy-data-portal/new-azure-sql-database-linked-service-button.png)       
25. V okně **Nová propojená služba** proveďte následující kroky: 

    1. Do pole **Název** zadejte **AzureSqlDatabaseLinkedService**. 
    2. V poli **Název serveru** vyberte váš server SQL Azure.
    4. V poli **Název databáze** vyberte vaši databázi SQL Azure. 
    5. Do pole **Uživatelské jméno** zadejte jméno uživatele. 
    6. Do pole **Heslo** zadejte heslo pro tohoto uživatele. 
    7. Klikněte na **Test připojení** a otestujte připojení.
    8. Kliknutím na **Uložit** propojenou službu uložte. 
    
        ![Nová propojená služba Azure SQL Database](./media/tutorial-copy-data-portal/new-azure-sql-linked-service-window.png)

26. Jako **Tabulka** vyberte **[dbo].[emp]**. 

    ![Výběr tabulky emp](./media/tutorial-copy-data-portal/select-emp-table.png)
27. Přepněte na kartu **Schéma** a klikněte na Importovat schéma. 

    ![Import schématu cíle](./media/tutorial-copy-data-portal/import-destination-schema.png)
28. Vyberte sloupec **ID** a klikněte na **Odstranit**. Sloupec ID je v databázi SQL sloupcem identity, takže aktivita kopírování nemusí do tohoto sloupce vkládat žádná dat.

    ![Odstranění sloupce ID](./media/tutorial-copy-data-portal/delete-id-column.png)
30. Přepněte na kartu s **kanálem** a ověřte, že je jako **Datová sada jímky** vybraná datová sada **OutputSqlDataset**.

    ![Karta Kanál](./media/tutorial-copy-data-portal/pipeline-tab-2.png)        
31. V dolní části okna Vlastnosti přepněte na kartu **Mapování** a klikněte na **Importovat schémata**. Všimněte si mapování prvního a druhého sloupce ve zdrojovém souboru na pole **FirstName** (Jméno) a **LastName** (Příjmení) v databázi SQL.

    ![Mapování schémat](./media/tutorial-copy-data-portal/map-schemas.png)
33. Ověřte kanál kliknutím na tlačítko **Ověřit**. Kliknutím na **šipku doprava** zavřete okno ověřování.

    ![Výstup ověření kanálu](./media/tutorial-copy-data-portal/pipeline-validation-output.png)   
34. Klikněte na tlačítko **Kód** v pravém rohu. Zobrazí se kód JSON přidružený k příslušnému kanálu. 

    ![Tlačítko Kód](./media/tutorial-copy-data-portal/code-button.png)
35. Zobrazí se kód JSON podobný následujícímu fragmentu kódu:  

    ```json
    {
        "name": "CopyPipeline",
        "properties": {
            "activities": [
                {
                    "name": "CopyFromBlobToSql",
                    "type": "Copy",
                    "dependsOn": [],
                    "policy": {
                        "timeout": "7.00:00:00",
                        "retry": 0,
                        "retryIntervalInSeconds": 20
                    },
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "SqlSink",
                            "writeBatchSize": 10000
                        },
                        "enableStaging": false,
                        "parallelCopies": 0,
                        "cloudDataMovementUnits": 0,
                        "translator": {
                            "type": "TabularTranslator",
                            "columnMappings": "Prop_0: FirstName, Prop_1: LastName"
                        }
                    },
                    "inputs": [
                        {
                            "referenceName": "SourceBlobDataset",
                            "type": "DatasetReference",
                            "parameters": {}
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "OutputSqlDataset",
                            "type": "DatasetReference",
                            "parameters": {}
                        }
                    ]
                }
            ]
        }
    }
    ```

## <a name="test-run-the-pipeline"></a>Testovací spuštění kanálu
Před publikováním artefaktů (propojené služby, databáze a kanál) do služby Data Factory (nebo) vlastního úložiště VSTS GIT můžete otestovat spuštění kanálu. 

1. Pokud chcete otestovat spuštění kanálu, klikněte na **Testovací běh** na panelu nástrojů. Na kartě **Výstup** v dolní části okna se zobrazí stav spuštění kanálu. 

    ![Tlačítko Testovací běh](./media/tutorial-copy-data-portal/test-run-output.png)
2. Ověřte, že se data ze zdrojového souboru vložila do cílové databáze SQL. 

    ![Ověření výstupu SQL](./media/tutorial-copy-data-portal/verify-sql-output.png)
3. V levém podokně klikněte na **Publikovat**. Tato akce publikuje vytvořené entity (propojené služby, datové sady a kanály) do služby Azure Data Factory.

    ![Tlačítko Publikovat](./media/tutorial-copy-data-portal/publish-button.png)
4. Počkejte, dokud se nezobrazí zpráva **Publikování proběhlo úspěšně**. Pokud chcete zobrazit zprávy oznámení, klikněte na bočním panelu vlevo na kartu **Zobrazit oznámení**. Zavřete okno oznámení kliknutím na **X**.

    ![Zobrazit oznámení](./media/tutorial-copy-data-portal/show-notifications.png)

## <a name="configure-code-repository"></a>Konfigurace úložiště kódu
Kód přidružený k artefaktům datové továrny můžete publikovat do úložiště kódu Visual Studio Team Services (VSTS). V tomto kroku vytvoříte úložiště kódu. 

Pokud nechcete pracovat s úložištěm kódu VSTS, můžete tento krok přeskočit a pokračovat v publikování do služby Data Factory jako v předchozím kroku. 

1. Klikněte na službu **Data Factory** v levém rohu (nebo) šipku dolů vedle jí a pak klikněte na **Konfigurace úložiště kódu**. 

    ![Tlačítko Kód](./media/tutorial-copy-data-portal/configure-code-repository-button.png)
2. Na stránce **Nastavení úložiště** proveďte následující kroky: 
    1. V poli **Typ úložiště** vyberte **Visual Studio Team Services Git**.
    2. V poli **Účet služby Visual Studio Team Services** vyberte svůj účet VSTS.
    3. V poli **Název projektu** vyberte projekt ve vašem účtu VSTS.
    4. Jako **název úložiště Git**, které se přidruží k vaší datové továrně, zadejte **Tutorial2**. 
    5. Ověřte, že je vybraná možnost **Importovat do úložiště stávající prostředky Data Factory**. 
    6. Kliknutím na **Uložit** nastavení uložte. 

        ![Nastavení úložiště](./media/tutorial-copy-data-portal/repository-settings.png)
3. Ověřte, že je vybrané úložiště **VSTS GIT**.

    ![Vybrané úložiště VSTS GIT](./media/tutorial-copy-data-portal/vsts-git-selected.png)
4. Na samostatné kartě webového prohlížeče přejděte do úložiště **Tutorial2**, kde se zobrazí dvě větve: **master** a **adf_publish**.

    ![Větve master a adf_publish](./media/tutorial-copy-data-portal/initial-branches-vsts-git.png)
5. Ověřte, že **soubory JSON** pro entity služby Data Factory jsou ve větvi **master**.

    ![Soubory ve větvi master](./media/tutorial-copy-data-portal/master-branch-files.png)
6. Ověřte, že **soubory JSON** ještě nejsou ve větvi **adf_publish**. 

    ![Soubory ve větvi adf_publish](./media/tutorial-copy-data-portal/adf-publish-files.png)
7. Přidejte **popis** **kanálu** a klikněte na tlačítko **Uložit** na panelu nástrojů. 

    ![Přidání popisu kanálu](./media/tutorial-copy-data-portal/pipeline-description.png)
8. Teď by se v úložišti **Tutorial2** měla zobrazit **větev** s vaším uživatelským jménem. Provedená změna je ve vaší vlastní větvi, ne ve větvi master. Publikovat můžete pouze entity z větve master.

    ![Vaše větev](./media/tutorial-copy-data-portal/your-branch.png)
9. Najeďte myší na tlačítko **Synchronizovat** (ještě na něj neklikejte), vyberte možnost **Potvrdit změny** a kliknutím na tlačítko **Synchronizovat** proveďte synchronizaci změn s větví **master**. 

    ![Potvrzení a synchronizace změn](./media/tutorial-copy-data-portal/commit-and-sync.png)
9. V okně Synchronizace změn proveďte následující akce: 

    1. Ověřte, že se v seznamu aktualizovaných kanálů zobrazí kanál **CopyPipeline**.
    2. Ověřte, že je vybraná možnost **Po synchronizaci publikovat změny**. Pokud zrušíte zaškrtnutí této možnosti, provede se synchronizace změn ve vaší větvi s větví master, ale ne jejich publikování do služby Data Factory. Můžete je publikovat později pomocí tlačítka **Publikovat**. Pokud tuto možnost zaškrtnete, změny se nejprve synchronizují do větve master a pak se publikují do služby Data Factory.
    3. Klikněte na **Synchronizovat**. 

    ![Okno Synchronizace změn](./media/tutorial-copy-data-portal/sync-your-changes.png)
10. Teď se ve větvi **adf_publish** úložiště **Tutorial2** zobrazují soubory. V této větvi najdete také šablonu Azure Resource Manageru pro vaše řešení Data Factory.  

    ![Soubory ve větvi adf_publish](./media/tutorial-copy-data-portal/adf-publish-files-after-publish.png)


## <a name="trigger-the-pipeline-manually"></a>Ruční aktivace kanálu
V tomto kroku ručně aktivujete kanál, který jste publikovali v minulém kroku. 

1. Klikněte na **Aktivační událost** na panelu nástrojů a pak klikněte na **Aktivovat**. 

    ![Nabídka Aktivovat](./media/tutorial-copy-data-portal/trigger-now-menu.png)
2. Vlevo přepněte na kartu **Monitorování**. Zobrazí se stav ručně aktivovaného spuštění kanálu. Pomocí odkazů ve sloupci Akce můžete zobrazit podrobnosti o konkrétní aktivitě nebo spustit kanál znovu.

    ![Monitorování spuštění kanálu](./media/tutorial-copy-data-portal/monitor-pipeline.png)
3. Pokud chcete zobrazit spuštění aktivit související se spuštěním kanálu, klikněte na odkaz **Zobrazit spuštění aktivit** ve sloupci **Akce**. Tento příklad obsahuje pouze jednu aktivitu, takže se v seznamu zobrazí pouze jedna položka. Pokud chcete zobrazit podrobnosti o operaci kopírování, klikněte na odkaz **Podrobnosti** (ikona brýlí) ve sloupci **Akce**. Pokud chcete přepnout zpět na zobrazení **Spuštění kanálu**, můžete kliknout na odkaz **Kanály** v horní části. Pokud chcete zobrazení aktualizovat, klikněte na **Aktualizovat**.

    ![Zobrazení spuštění aktivit](./media/tutorial-copy-data-portal/view-activity-runs.png)
4. Ověřte, že se do tabulky **emp** v databázi SQL Azure přidaly další dva řádky. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>Aktivace kanálu podle plánu
V tomto plánu vytvoříte pro kanál aktivační událost plánovače. Tato aktivační událost spouští kanál podle zadaného plánu (každou hodinu, každý den atd.). V tomto příkladu nastavíte aktivační událost tak, aby se spouštěla každou minutu až do uplynutí koncového data a času. 

1. Vlevo přepněte na kartu **Upravit**. 

    ![Karta Upravit](./media/tutorial-copy-data-portal/edit-tab.png)
2. Klikněte na **Aktivační událost** a vyberte **Nová / upravit**. Pokud kanál není aktivní, přepněte do něj. 

    ![Nabídka Nová / upravit aktivační událost](./media/tutorial-copy-data-portal/trigger-new-edit-menu.png)
3. V okně **Přidat aktivační události** klikněte na **Zvolit aktivační událost...** a pak na **+ Nová**. 

    ![Přidat aktivační události – nová aktivační událost](./media/tutorial-copy-data-portal/add-trigger-new-button.png)
4. V okně **Nová aktivační událost** proveďte následující kroky: 

    1. Nastavte **název** na **RunEveryMinute**.
    2. Jako **Konec** vyberte **K datu**. 
    3. Klikněte na rozevírací seznam pro možnost **K datu**.
    4. Vyberte **aktuální den**. Ve výchozím nastavení je koncový den nastavený na další den. 
    5. Aktualizujte část **minuty** tak, aby byla pár minut po aktuálním datu a času. Aktivační událost se aktivuje pouze po publikování změn. Proto pokud zadáte čas jenom pár minut od aktuálního času a během této doby změny nepublikujete, spuštění aktivační události se nezobrazí.  
    6. Klikněte na tlačítko **Použít**. 

        ![Nastavení vlastností aktivační události](./media/tutorial-copy-data-portal/set-trigger-properties.png)
    7. Zaškrtněte možnost **Aktivováno**. Pomocí tohoto zaškrtávacího políčka můžete aktivační událost deaktivovat a aktivovat ji později.
    8. Klikněte na **Další**.

        ![Aktivovaná aktivační událost – další](./media/tutorial-copy-data-portal/trigger-activiated-next.png)

    > [!IMPORTANT]
    > Každé spuštění kanálu je zpoplatněno. Proto nastavte koncové datum odpovídajícím způsoben. 
6. Na stránce **Parametry spuštění aktivační události** zkontrolujte upozornění a klikněte na **Dokončit**. Kanál v tomto příkladu nepřijímá žádné parametry. 

    ![Parametry kanálu](./media/tutorial-copy-data-portal/trigger-pipeline-parameters.png)
7. Kliknutím na **Publikovat** publikujte změny do úložiště. Aktivační událost se ve skutečnost neaktivuje, dokud úspěšně neproběhne publikování. 

    ![Publikování aktivační události](./media/tutorial-copy-data-portal/publish-trigger.png) 
8. Vlevo přepněte na kartu **Monitorování**, kde se zobrazí aktivovaná spuštění kanálu. 

    ![Aktivovaná spuštění kanálu](./media/tutorial-copy-data-portal/triggered-pipeline-runs.png)    
9. Pokud chcete přepnout ze zobrazení spuštění kanálu na zobrazení spuštění aktivační události, klikněte na Spuštění kanálu a vyberte Spuštění aktivační události.
    
    ![Nabídka Spuštění aktivační události](./media/tutorial-copy-data-portal/trigger-runs-menu.png)
10. V seznamu se zobrazí spuštění aktivační události. 

    ![Seznam spuštění aktivační události](./media/tutorial-copy-data-portal/trigger-runs-list.png)
11. Ověřte, že se až do uplynutí zadaného koncového času budou do tabulky **emp** vkládat dva řádky za minutu (pro každé spuštění kanálu). 

## <a name="next-steps"></a>Další kroky
Kanál v této ukázce kopíruje data z jednoho umístění do jiného umístění v úložišti objektů blob v Azure. Naučili jste se tyto postupy: 

> [!div class="checklist"]
> * Vytvoření datové továrny
> * Vytvoření kanálu s aktivitou kopírování
> * Testovací spuštění kanálu
> * Ruční aktivace kanálu
> * Aktivace kanálu podle plánu
> * Monitorování spuštění aktivit a kanálu


Pokud se chcete dozvědět víc o kopírování dat z místního prostředí do cloudu, přejděte k následujícímu kurzu: 

> [!div class="nextstepaction"]
>[Kopírování dat z místního prostředí do cloudu](tutorial-hybrid-copy-data-tool.md)
