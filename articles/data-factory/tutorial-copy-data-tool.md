---
title: "Kopírování dat pomocí nástroje pro kopírování dat Azure | Dokumentace Microsoftu"
description: "Vytvořte datovou továrnu Azure a pak pomocí nástroje pro kopírování dat zkopírujte data ze služby Azure Blob Storage do služby Azure SQL Database."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: a8c7035ebf462bb168147e9d8eb60742333ce8b8
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="copy-data-from-azure-blob-to-azure-sql-database-using-copy-data-tool"></a>Kopírování dat z objektu blob Azure do služby Azure SQL Database pomocí nástroje pro kopírování dat
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Verze 2 – Preview](tutorial-copy-data-tool.md)

V tomto kurzu pomocí webu Azure Portal vytvoříte datovou továrnu. Pak pomocí nástroje pro kopírování dat vytvoříte kanál, který kopírujte data z úložiště objektů blob v Azure do databáze SQL Azure. 

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

* **Předplatné Azure**. Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.
* **Účet služby Azure Storage**. Úložiště objektů blob použijete jako **zdrojové** úložiště dat. Pokud nemáte účet úložiště Azure, přečtěte si článek [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account), kde najdete kroky pro jeho vytvoření.
* **Azure SQL Database**. Tuto databázi použijete jako úložiště dat **jímky**. Pokud Azure SQL Database nemáte, přečtěte si článek věnovaný [vytvoření databáze Azure SQL](../sql-database/sql-database-get-started-portal.md), kde najdete kroky pro její vytvoření.

### <a name="create-a-blob-and-a-sql-table"></a>Vytvoření objektu blob a tabulky SQL

Teď připravte objekt blob Azure a Azure SQL Database pro tento kurz provedením následující kroků:

#### <a name="create-a-source-blob"></a>Vytvoření zdrojového objektu blob

1. Spusťte Poznámkový blok. Zkopírujte následující text a uložte ho na disk jako soubor **inputEmp.txt**.

    ```
    John|Doe
    Jane|Doe
    ```

2. Pomocí nástrojů, jako je [Průzkumník služby Azure Storage](http://storageexplorer.com/), vytvořte kontejner **adfv2tutorial** a odešlete soubor **inputEmp.txt** do tohoto kontejneru.

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

2. Povolte službám Azure přístup k SQL serveru. Zkontrolujte, že nastavení **Povolit přístup ke službám Azure** je pro váš server SQL Azure zapnuté, aby služba Data Factory mohla na server SQL Azure zapisovat data. Pokud chcete toto nastavení ověřit a zapnout, proveďte následující kroky:

    1. Klikněte na **Další služby** na levé straně a potom klikněte na **Servery SQL**.
    2. Vyberte svůj server a v části **NASTAVENÍ** klikněte na **Brána firewall**.
    3. Na stránce **Nastavení brány firewall** klikněte na **ZAPNUTO** u možnosti **Povolit přístup ke službám Azure**.

## <a name="create-a-data-factory"></a>Vytvoření datové továrny

1. V nabídce vlevo klikněte na **Nový**, klikněte na **Data + analýzy** a pak na **Data Factory**. 
   
   ![Nový -> Objekt pro vytváření dat](./media/tutorial-copy-data-tool/new-azure-data-factory-menu.png)
2. Na stránce **Nová datová továrna** jako **název** zadejte **ADFTutorialDataFactory**. 
      
     ![Stránka Nová datová továrna](./media/tutorial-copy-data-tool/new-azure-data-factory.png)
 
   Název objektu pro vytváření dat Azure musí být **globálně jedinečný**. Pokud se zobrazí následující chyba pole názvu, změňte název datové továrny (například na vaše_jméno_ADFTutorialDataFactory). Pravidla pojmenování artefaktů služby Data Factory najdete v článku [Data Factory – pravidla pojmenování](naming-rules.md).
  
     ![Stránka Nová datová továrna](./media/tutorial-copy-data-tool/name-not-available-error.png)
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

    ![nasazování dlaždice datové továrny](media/tutorial-copy-data-tool/deploying-data-factory.png)
9. Po vytvoření se zobrazí stránka **Datová továrna**, jak je znázorněno na obrázku.
   
   ![Domovská stránka objektu pro vytváření dat](./media/tutorial-copy-data-tool/data-factory-home-page.png)
10. Kliknutím na dlaždici **Vytvořit a monitorovat** otevřete na samostatné kartě uživatelské rozhraní služby Azure Data Factory. 

## <a name="use-copy-data-tool-to-create-a-pipeline"></a>Vytvoření kanálu pomocí nástroje pro kopírování dat

1. Na stránce Začínáme klikněte na dlaždici **Kopírovat data** a spusťte nástroj pro kopírování dat. 

   ![Dlaždice nástroje pro kopírování dat](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
2. Na stránce **Vlastnosti** nástroje pro kopírování dat jako **Název úlohy** zadejte **CopyFromBlobToSqlPipeline** a klikněte na **Další**. Uživatelské rozhraní služby Data Factory vytvoří kanál s názvem, který zadáte jako název úlohy. 

    ![Nástroj pro kopírování dat – stránka Vlastnosti](./media/tutorial-copy-data-tool/copy-data-tool-properties-page.png)
3. Na stránce **Zdrojové úložiště dat** vyberte **Azure Blob Storage** a klikněte na **Další**. Zdrojová data se nacházejí v úložišti objektů blob v Azure. 

    ![Stránka Zdrojové úložiště dat](./media/tutorial-copy-data-tool/source-data-store-page.png)
4. Na stránce **Zadejte účet služby Azure Blob Storage** proveďte následující kroky: 
    1. Jako **název připojení** zadejte **AzureStorageLinkedService**.
    2. Z rozevíracího seznamu vyberte **název vašeho účtu úložiště**.
    3. Klikněte na **Další**. 

        ![Zadání účtu úložiště objektů blob](./media/tutorial-copy-data-tool/specify-blob-storage-account.png)

        Propojená služba propojuje úložiště dat nebo výpočetní prostředí s datovou továrnou. V tomto případě vytvoříte propojenou službu Azure Storage, která propojí váš účet služby Azure Storage s úložištěm dat. Propojená služba obsahuje informace o připojení, které služba Data Factory používá pro připojení k úložišti objektů blob za běhu. Datová sada určuje kontejner, složku a soubor (volitelné) obsahující zdrojová data. 
5. Na stránce **Zvolte vstupní soubor nebo složku** proveďte následující kroky:
    
    1. Přejděte do složky **adfv2tutorial/input**.
    2. Vyberte soubor **inputEmp.txt**.
    3. Klikněte na **Zvolit**. Případně můžete na soubor **inputEmp.txt** dvakrát kliknout. 
    4. Klikněte na **Další**. 

    ![Zvolte vstupní soubor nebo složku](./media/tutorial-copy-data-tool/choose-input-file-folder.png)
6. Na stránce **Nastavení formátu souboru** si všimněte, že nástroj automaticky rozpozná oddělovače sloupců a řádků, a klikněte na **Další**. Na této stránce si také můžete prohlédnou data a schéma vstupních dat. 

    ![Stránka Nastavení formátu souboru](./media/tutorial-copy-data-tool/file-format-settings-page.png)
7. Na stránce **Cílové úložiště dat** vyberte **Azure SQL Database** a klikněte na **Další**. 

    ![Stránka Cílové úložiště dat](./media/tutorial-copy-data-tool/destination-data-storage-page.png)
8. Na stránce **Zadejte databázi SQL Azure** proveďte následující kroky: 

    1. Jako **Název připojení** zadejte **AzureSqlDatabaseLinkedService**. 
    2. Jako **Název serveru** vyberte váš server SQL Azure.
    3. Jako **Název databáze** vyberte vaši databázi SQL Azure.
    4. Jako **Uživatelské jméno** zadejte jméno uživatele.
    5. Jako **Heslo** zadejte heslo tohoto uživatele.
    6. Klikněte na **Další**. 

        ![Zadání databáze SQL Azure](./media/tutorial-copy-data-tool/specify-azure-sql-database.png)

        Datová sada musí být přidružená k propojené službě. Propojená služba obsahuje připojovací řetězec, který služba Data Factory používá pro připojení k databázi SQL Azure za běhu. Datová sada určuje kontejner, složku a soubor (volitelné), do kterého se data kopírují.
9.  Na stránce **Mapování tabulek** vyberte **[dbo].[emp]** a klikněte na **Další**. 

    ![Stránka Mapování tabulek](./media/tutorial-copy-data-tool/table-mapping-page.png)
10. Na stránce **Mapování schématu** si všimněte mapování prvního a druhého sloupce ve vstupním souboru na sloupce **FirstName** (Jméno) a **LastName** (Příjmení) tabulky **emp**. 

    ![Stránka Mapování schématu](./media/tutorial-copy-data-tool/schema-mapping-page.png)
11. Na stránce **Nastavení** klikněte na **Další**. 

    ![Stránka Nastavení](./media/tutorial-copy-data-tool/settings-page.png)
12. Na stránce **Souhrn** zkontrolujte nastavení a klikněte na **Další**.

    ![Stránka souhrnu](./media/tutorial-copy-data-tool/summary-page.png)
13. Na stránce **Nasazení** klikněte na **Monitorovat** a začněte monitorovat kanál (úlohu).

    ![Stránka Nasazení](./media/tutorial-copy-data-tool/deployment-page.png)
14. Všimněte si, že je vlevo automaticky vybraná karta **Monitorování**. Ve sloupci **Akce** se zobrazí odkazy na zobrazení podrobností o spuštění aktivity a na opětovné spuštění kanálu. Kliknutím na **Aktualizovat** seznam aktualizujte. 

    ![Monitorování spuštění kanálu](./media/tutorial-copy-data-tool/monitor-pipeline-runs.png)
15. Pokud chcete zobrazit spuštění aktivit související se spuštěním kanálu, klikněte na odkaz **Zobrazit spuštění aktivit** ve sloupci **Akce**. Kanál obsahuje pouze jednu aktivitu (aktivita kopírování), takže se zobrazí pouze jedna položka. Pokud chcete zobrazit podrobnosti o operaci kopírování, klikněte na odkaz **Podrobnosti** (ikona brýlí) ve sloupci **Akce**. Pokud chcete přepnout zpět na zobrazení spuštění kanálu, klikněte na odkaz **Kanály** v horní části. Pokud chcete zobrazení aktualizovat, klikněte na **Aktualizovat**. 

    ![Monitorování spuštění aktivit](./media/tutorial-copy-data-tool/monitor-activity-runs.png)
16. Kliknutím na kartu **Upravit** na levé straně přepněte do režimu úprav. Pomocí editoru můžete aktualizovat propojené služby, datové sady a kanály vytvořené nástrojem. Kliknutím na **Kód** zobrazíte kód JSON přidružený k entitě otevřené v editoru. Podrobnosti o úpravách těchto entit v uživatelském rozhraní služby Data Factory najdete ve [verzi tohoto kurzu pro Azure Portal](tutorial-copy-data-portal.md).

    ![Karta Editor](./media/tutorial-copy-data-tool/edit-tab.png)
17. Ověřte vložení dat do tabulky **emp** ve vaší databázi SQL Azure. 

    ![Ověření výstupu SQL](./media/tutorial-copy-data-tool/verify-sql-output.png)

## <a name="next-steps"></a>Další kroky
Kanál v této ukázce kopíruje data z úložiště objektů blob v Azure do databáze SQL Azure. Naučili jste se tyto postupy: 

> [!div class="checklist"]
> * Vytvoření datové továrny
> * Vytvoření kanálu pomocí nástroje pro kopírování dat
> * Monitorování spuštění aktivit a kanálu

Pokud se chcete dozvědět víc o kopírování dat z místního prostředí do cloudu, přejděte k následujícímu kurzu: 

> [!div class="nextstepaction"]
>[Kopírování dat z místního prostředí do cloudu](tutorial-hybrid-copy-data-tool.md)