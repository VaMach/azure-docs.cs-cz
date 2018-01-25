---
title: "Kopírování dat z SQL Serveru do úložiště objektů blob pomocí Azure Data Factory | Dokumentace Microsoftu"
description: "Zjistěte, jak kopírovat data z místního úložiště dat do cloudu Azure s využitím místního prostředí Integration Runtime ve službě Azure Data Factory."
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
ms.date: 01/11/2018
ms.author: jingwang
ms.openlocfilehash: 64cd758e2f40ff2b18abbff1194a7e57389d8a54
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2018
---
# <a name="tutorial-copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage"></a>Kurz: Kopírování dat z místní databáze SQL Serveru do úložiště objektů blob v Azure
V tomto kurzu pomocí uživatelského rozhraní služby Azure Data Factory vytvoříte kanál datové továrny, který kopíruje data z místní databáze SQL Serveru do úložiště objektů blob v Azure. Vytvoříte a použijete místní prostředí Integration Runtime, které přesouvá data mezi místním a cloudovým úložištěm dat. 

> [!NOTE]
> Tento článek se týká verze 2 služby Azure Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [dokumentaci služby Data Factory verze 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Tento článek neposkytuje podrobný úvod do služby Data Factory. Další informace najdete v tématu [Úvod do Azure Data Factory](introduction.md). 

V tomto kurzu budete provádět následující kroky:

> [!div class="checklist"]
> * Vytvoření datové továrny
> * Vytvořte místní prostředí Integration Runtime.
> * Vytvoření propojených služeb SQL Server a Azure Storage 
> * Vytvoření datových sad SQL Serveru a Azure Blob
> * Vytvoření kanálu s aktivitou kopírování pro přesun dat
> * Zahájení spuštění kanálu
> * Monitorování spuštění kanálu

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

5. Vytvořte tabulku **emp** a vložte do ní nějaká ukázková data spuštěním následujícího skriptu dotazu proti databázi:

   ```
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

6. Ve stromovém zobrazení klikněte pravým tlačítkem na databázi, kterou jste vytvořili, a pak vyberte **Nový dotaz**.

### <a name="azure-storage-account"></a>Účet služby Azure Storage
V tomto kurzu použijete účet úložiště Azure (konkrétně úložiště objektů blob v Azure) pro obecné účely jako cílové úložiště dat nebo úložiště dat jímky. Pokud nemáte účet úložiště Azure pro obecné účely, přečtěte si téma [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account). Kanál v datové továrně, který vytvoříte v tomto kurzu, kopíruje data z místní databáze SQL Serveru (zdroj) do tohoto úložiště objektů blob v Azure (jímka). 

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
V tomto kroku vytvoříte datovou továrnu a spustíte uživatelské rozhraní služby Azure Data Factory, ve kterém vytvoříte v této datové továrně kanál. 

1. V nabídce vlevo klikněte na **Nový**, klikněte na **Data + analýzy** a pak na **Data Factory**. 
   
   ![Nový -> Objekt pro vytváření dat](./media/tutorial-hybrid-copy-portal/new-azure-data-factory-menu.png)
2. Na stránce **Nová datová továrna** jako **název** zadejte **ADFTutorialDataFactory**. 
      
     ![Stránka Nová datová továrna](./media/tutorial-hybrid-copy-portal/new-azure-data-factory.png)
 
   Název objektu pro vytváření dat Azure musí být **globálně jedinečný**. Pokud se zobrazí následující chyba pole názvu, změňte název datové továrny (například na vaše_jméno_ADFTutorialDataFactory). Pravidla pojmenování artefaktů služby Data Factory najdete v článku [Data Factory – pravidla pojmenování](naming-rules.md).
  
     ![Stránka Nová datová továrna](./media/tutorial-hybrid-copy-portal/name-not-available-error.png)
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

    ![nasazování dlaždice datové továrny](media/tutorial-hybrid-copy-portal/deploying-data-factory.png)
9. Po vytvoření se zobrazí stránka **Datová továrna**, jak je znázorněno na obrázku.
   
   ![Domovská stránka objektu pro vytváření dat](./media/tutorial-hybrid-copy-portal/data-factory-home-page.png)
10. Kliknutím na dlaždici **Vytvořit a monitorovat** otevřete na samostatné kartě uživatelské rozhraní služby Azure Data Factory. 


## <a name="create-a-pipeline"></a>Vytvoření kanálu

1. Na stránce **Začínáme** klikněte na **Vytvořit kanál**. Automaticky se pro vás vytvoří kanál. Kanál se zobrazí ve stromovém zobrazení a otevře se jeho editor. 

   ![Stránka Začínáme](./media/tutorial-hybrid-copy-portal/get-started-page.png)
2. Na kartě **Obecné** v dolní části okna **Vlastnosti** jako **Název** zadejte **SQLServerToBlobPipeline**.

   ![Název kanálu](./media/tutorial-hybrid-copy-portal/pipeline-name.png)
2. Na panelu nástrojů **Aktivity** rozbalte **Tok dat** a přetáhněte aktivitu **Kopírování** na plochu návrháře kanálu. Nastavte název aktivity na **CopySqlServerToAzureBlobActivity**.

   ![Název aktivity](./media/tutorial-hybrid-copy-portal/copy-activity-name.png)
3. V okně Vlastnosti přepněte na kartu **Zdroj** a klikněte na **+ Nový**.

   ![Nová zdrojová datová sada – tlačítko](./media/tutorial-hybrid-copy-portal/source-dataset-new-button.png)
4. V okně **Nová datová sada** vyhledejte **SQL Server**, vyberte **SQL Server** a klikněte na **Dokončit**. Zobrazí se nová karta s názvem **SqlServerTable1**. Datová sada **SqlServerTable1** se zobrazí také ve stromovém zobrazení na levé straně. 

   ![Výběr SQL Serveru](./media/tutorial-hybrid-copy-portal/select-sql-server.png)
5. Na kartě **Obecné** v okně Vlastnosti jako **Název** zadejte **SqlServerDataset**.
    
   ![Zdrojová datová sada – název](./media/tutorial-hybrid-copy-portal/source-dataset-name.png)
6. Přepněte na kartu **Připojení** a klikněte na **+ Nové**. V tomto kroku vytvoříte připojení ke zdrojovému úložišti dat (databáze SQL Serveru). 

   ![Zdrojová datová sada – tlačítko Nové připojení](./media/tutorial-hybrid-copy-portal/source-connection-new-button.png)
7. V okně **Nová propojená služba** klikněte na **Nové prostředí Integration Runtime**. V této části vytvoříte místní prostředí Integration Runtime a přidružíte ho k místnímu počítači s databází SQL Serveru. Místní prostředí Integration Runtime je komponenta, která kopíruje data z databáze SQL Serveru ve vašem počítači do úložiště objektů blob v Azure. 

   ![Nové prostředí Integration Runtime](./media/tutorial-hybrid-copy-portal/new-integration-runtime-button.png)
8. V okně **Instalace prostředí Integration Runtime** vyberte **Privátní síť** a klikněte na **Další**. 

   ![Výběr privátní sítě](./media/tutorial-hybrid-copy-portal/select-private-network.png)
9. Zadejte název prostředí Integration Runtime a klikněte na **Další**.  
    
   ![Prostředí Integration Runtime – název](./media/tutorial-hybrid-copy-portal/integration-runtime-name.png)
10. Klikněte na text **Kliknutím sem spustíte expresní instalaci pro tento počítač** v části **Možnost 1: Expresní instalace**. 

   ![Kliknutí na odkaz na expresní instalaci](./media/tutorial-hybrid-copy-portal/click-exress-setup.png)
11. V okně **Expresní instalace Integration Runtime (v místním prostředí)** klikněte na **Zavřít**. 

   ![Instalace prostředí Integration Runtime – úspěch](./media/tutorial-hybrid-copy-portal/integration-runtime-setup-successful.png)
12. V okně **Instalace prostředí Integration Runtime** ve webovém prohlížeči klikněte na **Dokončit**. 

   ![Instalace prostředí Integration Runtime – dokončení](./media/tutorial-hybrid-copy-portal/click-finish-integration-runtime-setup.png)
13. V okně **Nová propojená služba** proveďte následující kroky:

    1. Jako **Název** zadejte **SqlServerLinkedService**.
    2. Ověřte, že se místní prostředí Integration Runtime, které jste vytvořili dříve, zobrazí v části **Připojení prostřednictvím prostředí Integration Runtime**.
    3. Jako **Název serveru** zadejte název vašeho SQL Serveru. 
    4. Do pole **Název databáze** zadejte název databáze s tabulkou **emp**. 
    5. Vyberte odpovídající **typ ověřování**, který má služba Data Factory používat pro připojení k vaší databázi SQL Serveru. 
    6. Zadejte **uživatelské jméno** a **heslo**. Pokud v názvu uživatelského účtu nebo serveru potřebujete použít zpětné lomítko (\\), vložte před něj řídicí znak (\\). Použijte například *mydomain\\\\myuser*. 
    7. Klikněte na **Test připojení**. Proveďte tento krok, abyste potvrdili, že se služba Data Factory může připojit k vaší databázi SQL Serveru pomocí místního prostředí Integration Runtime, které jste vytvořili. 
    8. Pokud chcete propojenou službu uložit, klikněte na **Uložit**.

       ![Propojená služba SQL Serveru – nastavení](./media/tutorial-hybrid-copy-portal/sql-server-linked-service-settings.png)
14. Měli byste se vrátit do okna s otevřenou zdrojovou datovou sadou. V části **Připojení** v okně **Vlastnosti** proveďte následující kroky: 

    1. Ověřte, že se jako **Propojená služba** zobrazuje **SqlServerLinkedService**. 
    2. Jako **Tabulka** vyberte **[dbo].[emp]**.

        ![Zdrojová datová sada – informace o připojení](./media/tutorial-hybrid-copy-portal/source-dataset-connection.png)
15. Přepněte na kartu s kanálem SQLServerToBlobPipeline (nebo) klikněte na kanál **SQLServerToBlobPipeline** ve stromovém zobrazení. 

    ![Karta Kanál](./media/tutorial-hybrid-copy-portal/pipeliene-tab.png)
16. V okně **Vlastnosti** přepněte na kartu **Jímka** a klikněte na **+ Nová**. 

    ![Datová sada jímky – tlačítko Nová](./media/tutorial-hybrid-copy-portal/sink-dataset-new-button.png)
17. V okně **Nová datová sada** vyberte **Azure Blob Storage** a klikněte na **Dokončit**. Otevře se nová karta pro tuto datovou sadu. Datová sada se zobrazí také ve stromovém zobrazení. 

    ![Výběr služby Azure Blob Storage](./media/tutorial-hybrid-copy-portal/select-azure-blob-storage.png)
18. Jako **Název** zadejte **AzureBlobDataset**.

    ![Datová sada jímky – název](./media/tutorial-hybrid-copy-portal/sink-dataset-name.png)
19. V okně **Vlastnosti** přepněte na kartu **Připojení** a v části **Propojená služba** klikněte na **+ Nová**. 

    ![Nová propojená služba – tlačítko](./media/tutorial-hybrid-copy-portal/new-storage-linked-service-button.png)
20. V okně **Nová propojená služba** proveďte následující kroky:

    1. Jako **Název** zadejte **AzureStorageLinkedService**.
    2. Jako **Název účtu úložiště** vyberte svůj účet úložiště Azure. 
    3. Otestujte připojení k vašemu účtu úložiště Azure kliknutím na **Test připojení**.
    4. Klikněte na **Uložit**.

        ![Propojená služba Azure Storage – nastavení](./media/tutorial-hybrid-copy-portal/azure-storage-linked-service-settings.png) 
21.  Měli byste se vrátit do okna s otevřenou datovou sadou jímky. Na kartě **Připojení** proveďte následující kroky: 

        1. Ověřte, že jako **Propojená služba** je vybraná služba **AzureStorageLinkedService**.
        2. Do části **složka** v **cestě k souboru** zadejte **adftutorial/fromonprem**. Pokud výstupní složka v kontejneru adftutorial neexistuje, služba Data Factory ji automaticky vytvoří.
        3. Do části **název souboru** v **cestě k souboru** zadejte `@CONCAT(pipeline().RunId, '.txt')`.

            ![Datová sada jímky – připojení](./media/tutorial-hybrid-copy-portal/sink-dataset-connection.png)
22. Přepněte na kartu s otevřeným kanálem (nebo) klikněte na **kanál** ve **stromovém zobrazení**. Ověřte, že jako **Datová sada jímky** je vybraná datová sada **AzureBlobDataset**. 

    ![Vybraná datová sada jímky ](./media/tutorial-hybrid-copy-portal/sink-dataset-selected.png)
23. Pokud chcete ověřit nastavení kanálu, klikněte na Ověřit na panelu nástrojů pro kanál. Zavřete **sestavu ověření kanálu** kliknutím na **X** v pravém rohu. 

    ![Ověření kanálu](./media/tutorial-hybrid-copy-portal/validate-pipeline.png)
1. Publikujte entity, které jste vytvořili ve službě Azure Data Factory, kliknutím na **Publikovat**.

    ![Tlačítko Publikovat](./media/tutorial-hybrid-copy-portal/publish-button.png)
24. Počkejte, dokud se nezobrazí překryvné okno **Publikování proběhlo úspěšně**. Stav publikování můžete také zkontrolovat kliknutím na odkaz **Zobrazit oznámení** na levé straně. Zavřete okno oznámení kliknutím na **X**. 

    ![Publikování proběhlo úspěšně](./media/tutorial-hybrid-copy-portal/publishing-succeeded.png)
    

## <a name="trigger-a-pipeline-run"></a>Aktivace spuštění kanálu
Klikněte na **Aktivační událost** na panelu nástrojů pro kanál a pak klikněte na **Aktivovat**.

![Aktivovat](./media/tutorial-hybrid-copy-portal/trigger-now.png)

## <a name="monitor-the-pipeline-run"></a>Monitorování spuštění kanálu

1. Přepněte na kartu **Monitorování**. Zobrazí se kanál, který jste ručně aktivovali v předchozím kroku. 

    ![Spuštění kanálu](./media/tutorial-hybrid-copy-portal/pipeline-runs.png)
2. Pokud chcete zobrazit spuštění aktivit související se spuštěním kanálu, klikněte na odkaz **Zobrazit spuštění aktivit** ve sloupci **Akce**. Zobrazí se pouze spuštění aktivit, protože kanál obsahuje pouze jednu aktivitu. Pokud chcete zobrazit podrobnosti o operaci kopírování, klikněte na odkaz **Podrobnosti** (ikona brýlí) ve sloupci **Akce**. Zpět na zobrazení spuštění kanálu můžete přepnout kliknutím na **Kanály** v horní části.

    ![Spuštění aktivit](./media/tutorial-hybrid-copy-portal/activity-runs.png)

## <a name="verify-the-output"></a>Ověření výstupu
Kanál v kontejneru objektů blob `adftutorial` automaticky vytvoří výstupní složku *fromonprem*. Zkontrolujte, že výstupní složka obsahuje soubor *dbo.emp.txt*. 

1. Na webu Azure Portal v okně kontejneru **adftutorial** vyberte **Obnovit**. Zobrazí se výstupní složka.

    ![Vytvořená výstupní složka](media/tutorial-hybrid-copy-portal/fromonprem-folder.png)
2. V seznamu složek vyberte `fromonprem`. 
3. Potvrďte, že se zobrazuje soubor s názvem `dbo.emp.txt`.

    ![Výstupní soubor](media/tutorial-hybrid-copy-portal/fromonprem-file.png)


## <a name="next-steps"></a>Další kroky
Kanál v této ukázce kopíruje data z jednoho umístění do jiného v úložišti objektů blob v Azure. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření datové továrny
> * Vytvořte místní prostředí Integration Runtime.
> * Vytvoření propojených služeb SQL Server a Azure Storage 
> * Vytvoření datových sad SQL Serveru a Azure Blob
> * Vytvoření kanálu s aktivitou kopírování pro přesun dat
> * Zahájení spuštění kanálu
> * Monitorování spuštění kanálu

Seznam úložišť dat podporovaných službou Data Factory najdete v tématu popisujícím [podporovaná úložiště dat](copy-activity-overview.md#supported-data-stores-and-formats).

Pokud se chcete dozvědět víc o hromadném kopírování dat ze zdroje do cíle, přejděte k následujícímu kurzu:

> [!div class="nextstepaction"]
>[Hromadné kopírování dat](tutorial-bulk-copy-portal.md)
