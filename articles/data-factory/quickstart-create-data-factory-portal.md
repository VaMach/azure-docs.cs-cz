---
title: "Vytvoření datové továrny Azure pomocí uživatelského rozhraní služby Azure Data Factory | Dokumentace Microsoftu"
description: "V tomto kurzu se dozvíte, jak vytvořit datovou továrnu s kanálem, který kopíruje data z jedné složky do jiné ve službě Azure Blob Storage."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/16/2018
ms.author: jingwang
ms.openlocfilehash: ebce4e0d137d2e56cc914efad357593af7abb255
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="create-a-data-factory-using-the-azure-data-factory-ui"></a>Vytvoření datové továrny pomocí uživatelského rozhraní služby Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Verze 2 – Preview](quickstart-create-data-factory-portal.md)

Tento rychlý start popisuje vytvoření a monitorování datové továrny pomocí uživatelského rozhraní služby Azure Data Factory. Kanál, který vytvoříte v této datové továrně, **kopíruje** data z jedné složky do jiné složky v úložišti objektů blob Azure. Kurz předvádějící způsoby **transformace** dat pomocí Azure Data Factory najdete v tématu [Kurz: Transformace dat pomocí Sparku](tutorial-transform-data-spark-portal.md). 


> [!NOTE]
> Pokud se službou Azure Data Factory začínáte, přečtěte si téma [Seznámení se službou Azure Data Factory](data-factory-introduction.md), než s tímto rychlým startem začnete. 
>
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, která je všeobecně dostupná (GA), prostudujte si [Kurz služby Data Factory verze 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

### <a name="video"></a>Video 
Toto video vám pomůže seznámit se s uživatelským rozhraním služby Data Factory: 
>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Visually-build-pipelines-for-Azure-Data-Factory-v2/Player]

## <a name="create-a-data-factory"></a>Vytvoření datové továrny

1. Přejděte na [Azure Portal](https://portal.azure.com). 
2. V nabídce vlevo klikněte na **Nový**, klikněte na **Data + analýzy** a pak na **Data Factory**. 
   
   ![Nový -> Objekt pro vytváření dat](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)
2. Na stránce **Nová datová továrna** jako **název** zadejte **ADFTutorialDataFactory**. 
      
     ![Stránka Nová datová továrna](./media/quickstart-create-data-factory-portal/new-azure-data-factory.png)
 
   Název objektu pro vytváření dat Azure musí být **globálně jedinečný**. Pokud se zobrazí následující chyba pole názvu, změňte název datové továrny (například na vaše_jméno_ADFTutorialDataFactory). Pravidla pojmenování artefaktů služby Data Factory najdete v článku [Data Factory – pravidla pojmenování](naming-rules.md).
  
     ![Název není dostupný – chyba](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
3. Vyberte své **předplatné** Azure, ve kterém chcete vytvořit datovou továrnu. 
4. Pro **Skupinu prostředků** proveďte jeden z následujících kroků:
     
      - Vyberte **Použít existující** a z rozevíracího seznamu vyberte existující skupinu prostředků. 
      - Vyberte **Vytvořit novou** a zadejte název skupiny prostředků.   
         
    Informace o skupinách prostředků najdete v článku [Použití skupin prostředků ke správě prostředků Azure](../azure-resource-manager/resource-group-overview.md).  
4. Jako **verzi** vyberte **V2 (Preview)**.
5. Vyberte **umístění** pro objekt pro vytváření dat. V rozevíracím seznamu se zobrazí pouze umístění podporovaná službou Data Factory. Úložiště dat (Azure Storage, Azure SQL Database atd.) a výpočetní prostředí (HDInsight atd.) používané datovou továrnou můžou být v jiných umístěních.
6. Zaškrtněte **Připnout na řídicí panel**.     
7. Klikněte na možnost **Vytvořit**.
8. Na řídicím panelu vidíte následující dlaždice se statusem: **Nasazování datové továrny**. 

    ![nasazování dlaždice datové továrny](media//quickstart-create-data-factory-portal/deploying-data-factory.png)
9. Po vytvoření se zobrazí stránka **Datová továrna**, jak je znázorněno na obrázku.
   
    ![Domovská stránka objektu pro vytváření dat](./media/quickstart-create-data-factory-portal/data-factory-home-page.png)
10. Kliknutím na dlaždici **Vytvořit a monitorovat** otevřete na samostatné kartě aplikaci uživatelského rozhraní služby Azure Data Factory. 
11. Na stránce Začínáme přepněte na levém panelu na kartu **Upravit**, jak je znázorněno na následujícím obrázku: 

    ![Stránka Začínáme](./media/quickstart-create-data-factory-portal/get-started-page.png)

## <a name="create-azure-storage-linked-service"></a>Vytvoření propojené služby Azure Storage
V tomto kroku vytvoříte propojenou službu, která propojí váš účet služby Azure Storage s datovou továrnou. Tato propojená služba má informace o připojení, které služba Data Factory používá pro připojení za běhu.

2. Klikněte na **Připojení** a pak klikněte na tlačítko **Nové** na řídicím panelu. 

    ![Nové připojení](./media/quickstart-create-data-factory-portal/new-connection-button.png)    
3. Na stránce **Nová propojená služba** vyberte **Azure Blob Storage** a klikněte na **Pokračovat**. 

    ![Výběr služby Azure Storage](./media/quickstart-create-data-factory-portal/select-azure-storage.png)
4. Na stránce **Nová propojená služba** proveďte následující kroky: 

    1. Jako **Název** zadejte **AzureStorageLinkedService**.
    2. Jako **Název účtu úložiště** vyberte svůj účet služby Azure Storage.
    3. Klikněte na **Test připojení** a potvrďte, že se služba Data Factory dokáže připojit k účtu úložiště. 
    4. Kliknutím na **Uložit** propojenou službu uložte. 

        ![Nastavení propojené služby Azure Storage](./media/quickstart-create-data-factory-portal/azure-storage-linked-service.png) 
5. Ověřte, že se služba **AzureStorageLinkedService** zobrazí v seznamu propojených služeb. 

    ![Propojená služba Azure Storage v seznamu](./media/quickstart-create-data-factory-portal/azure-storage-linked-service-in-list.png)

## <a name="create-datasets"></a>Vytvoření datových sad
V tomto kroku vytvoříte dvě datové sady:**InputDataset** a **OutputDataset**. Tyto datové sady jsou typu **AzureBlob**. Odkazují na **propojenou službu Azure Storage**, kterou jste vytvořili v předchozím kroku. 

Vstupní datová sada představuje zdrojová data ve vstupní složce. V definici vstupní datové sady určíte kontejner objektů blob (**adftutorial**), složku (**input**) a soubor (**emp.txt**) obsahující zdrojová data. 

Výstupní datová sada představuje data kopírovaná do cíle. V definici výstupní datové sady určíte kontejner objektů blob (**adftutorial**), složku (**output**) a soubor, do kterého se data kopírují. Ke každému spuštění kanálu je přidružené jedinečné ID, ke kterému je možný přístup před systémovou proměnnou **u RunId**. Název výstupního souboru se dynamicky vyhodnocuje na základě ID spuštění kanálu.   

V nastavení propojené služby jste zadali účet úložiště Azure obsahující zdrojová data. V nastavení zdrojové datové sady určíte, kde přesně se data nacházejí (kontejner objektů blob, složka a soubor). V nastavení datové sady jímky určíte, kam se data kopírují (kontejner objektů blob, složka a soubor). 
 
1. Klikněte na tlačítko **+ (plus)** a vyberte **Datová sada**.

    ![Nabídka Nová datová sada](./media/quickstart-create-data-factory-portal/new-dataset-menu.png)
2. Na stránce **Nová datová sada** vyberte **Azure Blob Storage** a klikněte na **Dokončit**. 

    ![Výběr služby Azure Blob Storage](./media/quickstart-create-data-factory-portal/select-azure-blob-storage.png)
3. V okně **Vlastnosti** datové sady jako **Název** zadejte **InputDataset**. 

    ![Obecná nastavení datové sady](./media/quickstart-create-data-factory-portal/dataset-general-page.png)
4. Přepněte na kartu **Připojení** a proveďte následující kroky: 

    1. Jako propojenou službu vyberte **AzureStorageLinkedService**. 
    2. Klikněte na tlačítko **Procházet** vedle pole **Cesta k souboru**. 
        ![Vyhledání vstupního souboru](./media/quickstart-create-data-factory-portal/file-path-browse-button.png)
    3. V okně **Zvolte soubor nebo složku** přejděte do složky **input** kontejneru **adftutorial**, vyberte soubor **emp.txt** a klikněte na **Dokončit**.

        ![Vyhledání vstupního souboru](./media/quickstart-create-data-factory-portal/choose-file-folder.png)
    4. (volitelné) Kliknutím na **Náhled dat** zobrazíte náhled dat v souboru emp.txt.     
5. Pomocí stejného postupu vytvořte výstupní datovou sadu.  

    1. Klikněte na tlačítko **+ (plus)** v levém podokně a vyberte **Datová sada**.
    2. Na stránce **Nová datová sada** vyberte **Azure Blob Storage** a klikněte na **Dokončit**.
    3. Jako název zadejte **OutputDataset**.
    4. Jako složku zadejte **adftutorial/output**. Aktivita kopírování vytvoří výstupní složku, pokud neexistuje. 
    5. Jako název souboru zadejte `@CONCAT(pipeline().RunId, '.txt')`. Při každém spuštění kanálu se ke spuštění kanálu přidruží jedinečné ID. Výraz vytvoří název výstupního souboru zřetězením ID spuštění kanálu a přípony **.txt**. Seznam podporovaných systémových proměnných a výrazů najdete v tématech [Systémové proměnné](control-flow-system-variables.md) a [Jazyk výrazů](control-flow-expression-language-functions.md).

        ![Nastavení výstupní datové sady](./media/quickstart-create-data-factory-portal/output-dataset-settings.png)

## <a name="create-a-pipeline"></a>Vytvoření kanálu 
V tomto kroku vytvoříte a ověříte kanál s aktivitou **Kopírování**, která používá vstupní a výstupní datovou sadu. Aktivita kopírování kopíruje data ze souboru zadaného v nastavení vstupní datové sady do souboru zadaného v nastavení výstupní datové sady. Pokud vstupní datová sada určuje pouze složku (bez názvu souboru), aktivita kopírování zkopíruje do cíle všechny soubory ve zdrojové složce. 

1. Klikněte na tlačítko **+ (plus)** a vyberte **Kanál**. 

    ![Nabídka Nový kanál](./media/quickstart-create-data-factory-portal/new-pipeline-menu.png)
2. V okně **Vlastnosti** jako **Název** zadejte **CopyPipeline**. 

    ![Obecná nastavení kanálu](./media/quickstart-create-data-factory-portal/pipeline-general-settings.png)
3. Na panelu nástrojů **Aktivity** rozbalte **Tok dat** a přetáhněte aktivitu **Kopírování** z panelu nástrojů **Aktivity** na plochu návrháře kanálu. Na panelu nástrojů **Aktivity** můžete aktivity také vyhledávat. Jako **název** zadejte **CopyFromBlobToBlob**.

    ![Obecná nastavení aktivity kopírování](./media/quickstart-create-data-factory-portal/copy-activity-general-settings.png)
4. V nastavení aktivity kopírování přepněte na kartu **Zdroj** a jako **zdrojovou datovou sadu** vyberte **InputDataset**.

    ![Nastavení zdroje aktivity kopírování](./media/quickstart-create-data-factory-portal/copy-activity-source-settings.png)    
5. V nastavení aktivity kopírování přepněte na kartu **Jímka** a jako **datovou sadu jímky** vyberte **OutputDataset**.

    ![Nastavení jímky aktivity kopírování](./media/quickstart-create-data-factory-portal/copy-activity-sink-settings.png)    
7. Klikněte na **Ověřit** a ověřte nastavení kanálu. Ověřte úspěšné ověření kanálu. Pokud chcete zavřít výstup ověřování, klikněte na tlačítko s **šipkami doprava** (>>). 

    ![Ověření kanálu](./media/quickstart-create-data-factory-portal/pipeline-validate-button.png)

## <a name="test-run-the-pipeline"></a>Testovací spuštění kanálu
V tomto kroku provedete testovací spuštění kanálu před tím, než ho nasadíte do služby Data Factory. 

1. Na panelu nástrojů pro kanál klikněte na **Testovací běh**. 
    
    ![Testovací běhy kanálu](./media/quickstart-create-data-factory-portal/pipeline-test-run.png)
2. Ověřte, že se na kartě **Výstup** v nastavení kanálu zobrazí stav spuštění kanálu. 

    ![Výstup testovacího běhu](./media/quickstart-create-data-factory-portal/test-run-output.png)    
3. Ověřte, že složka **output** kontejneru **adftutorial** obsahuje výstupní soubor. Pokud výstupní složka neexistuje, služba Data Factory ji automaticky vytvoří. 
    
    ![Ověření výstupu](./media/quickstart-create-data-factory-portal/verify-output.png)


## <a name="trigger-the-pipeline-manually"></a>Ruční aktivace kanálu
V tomto kroku nasadíte entity (propojené služby, datové sady a kanály) do služby Azure Data Factory. Pak ručně aktivujete spuštění kanálu. Entity můžete publikovat také do vlastního úložiště VSTS GIT, čemuž se věnuje [jiný kurz](tutorial-copy-data-portal.md?#configure-code-repository).

1. Před aktivací kanálu je nutné publikovat entity do služby Data Factory. Pokud je chcete publikovat, v levém podokně klikněte na **Publikovat**. 

    ![Tlačítko Publikovat](./media/quickstart-create-data-factory-portal/publish-button.png)
2. Pokud chcete kanál aktivovat ručně, klikněte na **Aktivovat** na panelu nástrojů a pak vyberte **Aktivovat**. 
    
    ![Aktivovat](./media/quickstart-create-data-factory-portal/pipeline-trigger-now.png)

## <a name="monitor-the-pipeline"></a>Monitorování kanálu

1. Vlevo přepněte na kartu **Monitorování**. Pomocí tlačítka **Aktualizovat** seznam aktualizujte.

    ![Monitorování kanálu](./media/quickstart-create-data-factory-portal/monitor-trigger-now-pipeline.png)
2. Klikněte na odkaz **Zobrazit spuštění aktivit** ve sloupci **Akce**. Na této stránce se zobrazí stav spuštění aktivity kopírování. 

    ![Spuštění aktivity kanálu](./media/quickstart-create-data-factory-portal/pipeline-activity-runs.png)
3. Pokud chcete zobrazit podrobnosti o operaci kopírování, klikněte na odkaz **Podrobnosti** (obrázek brýlí) ve sloupci **Akce**. Podrobnosti o vlastnostech najdete v tématu [Přehled aktivity kopírování](copy-activity-overview.md). 

    ![Podrobnosti o aktivitě kopírování](./media/quickstart-create-data-factory-portal/copy-operation-details.png)
4. Ověřte, že složka **output** obsahuje nový soubor. 
5. Ze zobrazení **Spuštění aktivit** zpět na zobrazení **Spuštění kanálu** můžete přepnout kliknutím na odkaz **Kanály**. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>Aktivace kanálu podle plánu
Tento krok je v tomto kurzu volitelný. Můžete vytvořit **aktivační událost plánovače** a naplánovat pravidelné spouštění kanálu (každou hodinu, každý den atd.). V tomto kroku vytvoříte aktivační událost, která se bude spouštět každou minutu až do data a času, které jste zadali jako koncové datum. 

1. Přepněte na kartu **Upravit**. 

    ![Přepnutí na kartu Upravit](./media/quickstart-create-data-factory-portal/switch-edit-tab.png)
1. V nabídce klikněte na **Aktivační událost** a pak klikněte na **Nová / upravit**. 

    ![Nabídka Nová aktivační událost](./media/quickstart-create-data-factory-portal/new-trigger-menu.png)
2. Na stránce **Přidat aktivační události** klikněte na **Zvolit aktivační událost...** a pak na **Nová**. 

    ![Přidat aktivační události – nová aktivační událost](./media/quickstart-create-data-factory-portal/add-trigger-new-button.png)
3. Na stránce **Nová aktivační událost** v poli **Konec** vyberte **K datu**, zadejte koncový čas pár minut od aktuálního času a klikněte na **Použít**. Každé spuštění kanálu je zpoplatněno, proto zadejte koncový čas pouze pár minut po času zahájení. Ujistěte se, že se jedná o stejný den. Zajistěte však, aby měl kanál mezi časem publikování a koncovým časem dostatek času na spuštění. Aktivační událost nabývá účinnosti po publikování řešení do služby Data Factory, a ne při uložení aktivační události v uživatelském rozhraní. 

    ![Nastavení aktivační události](./media/quickstart-create-data-factory-portal/trigger-settings.png)
4. Na stránce **Nová aktivační událost** zaškrtněte možnost **Aktivováno** a klikněte na **Další**. 

    ![Nastavení aktivační události – tlačítko Další](./media/quickstart-create-data-factory-portal/trigger-settings-next.png)
5. Na stránce **Nová aktivační událost** si přečtěte zprávu upozornění a klikněte na **Dokončit**.

    ![Nastavení aktivační události – tlačítko Dokončit](./media/quickstart-create-data-factory-portal/new-trigger-finish.png)
6. Kliknutím na **Publikovat** publikujte změny do služby Data Factory. 

    ![Tlačítko Publikovat](./media/quickstart-create-data-factory-portal/publish-2.png)
8. Vlevo přepněte na kartu **Monitorování**. Kliknutím na **Aktualizovat** seznam aktualizujte. Uvidíte, že se kanál spustí každou minutu od času publikování až do koncového času. Všimněte si hodnot ve sloupci **Aktivoval(a)**. Ruční spuštění aktivační události proběhlo v kroku, který jste provedli dříve (**Aktivovat**). 

    ![Monitorování aktivovaných spuštění](./media/quickstart-create-data-factory-portal/monitor-triggered-runs.png)
9. Kliknutím na šipku dolů vedle **Spuštění kanálu** přepněte na zobrazení **Spuštění aktivační události**. 

    ![Monitorování spuštění aktivační události](./media/quickstart-create-data-factory-portal/monitor-trigger-runs.png)    
10. Ověřte, že se ve složce **output** vytvořil **výstupní soubor** pro každé spuštění kanálu až do zadaného koncového data a času. 

## <a name="next-steps"></a>Další kroky
Kanál v této ukázce kopíruje data z jednoho umístění do jiného umístění v úložišti objektů blob v Azure. Projděte si [kurzy](tutorial-copy-data-portal.md), kde se dozvíte o použití služby Data Factory ve více scénářích. 