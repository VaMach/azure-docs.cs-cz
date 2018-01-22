---
title: "Kopírování dat pomocí nástroje pro kopírování dat Azure | Dokumentace Microsoftu"
description: "Vytvořte datovou továrnu Azure a pak pomocí nástroje pro kopírování dat zkopírujte data z jedné složky v úložišti objektů blob v Azure do jiné."
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
ms.openlocfilehash: c17e738e3db18503f7cdda24a5f01ceb78e4e6a3
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="use-copy-data-tool-to-copy-data"></a>Kopírování dat pomocí nástroje pro kopírování dat 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Verze 2 – Preview](quickstart-create-data-factory-copy-data-tool.md)

V tomto rychlém startu použijete Azure Portal k vytvoření datové továrny. Pak pomocí nástroje pro kopírování dat vytvoříte kanál, který kopírujte data z jedné složky v úložišti objektů blob v Azure do jiné. 

> [!NOTE]
> Pokud se službou Azure Data Factory začínáte, přečtěte si téma [Seznámení se službou Azure Data Factory](data-factory-introduction.md), než s tímto rychlým startem začnete. 
>
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [úvod do služby Data Factory verze 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

## <a name="create-a-data-factory"></a>Vytvoření datové továrny

1. V nabídce vlevo klikněte na **Nový**, klikněte na **Data + analýzy** a pak na **Data Factory**. 
   
   ![Nový -> Objekt pro vytváření dat](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory-menu.png)
2. Na stránce **Nová datová továrna** jako **název** zadejte **ADFTutorialDataFactory**. 
      
     ![Stránka Nová datová továrna](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory.png)
 
   Název objektu pro vytváření dat Azure musí být **globálně jedinečný**. Pokud se zobrazí následující chyba pole názvu, změňte název datové továrny (například na vaše_jméno_ADFTutorialDataFactory) a zkuste to znovu. Pravidla pojmenování artefaktů služby Data Factory najdete v článku [Data Factory – pravidla pojmenování](naming-rules.md).
  
     ![Název není dostupný – chyba](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
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

    ![nasazování dlaždice datové továrny](media/quickstart-create-data-factory-copy-data-tool/deploying-data-factory.png)
9. Po vytvoření se zobrazí stránka **Datová továrna**, jak je znázorněno na obrázku.
   
   ![Domovská stránka objektu pro vytváření dat](./media/quickstart-create-data-factory-copy-data-tool/data-factory-home-page.png)
10. Kliknutím na dlaždici **Vytvořit a monitorovat** otevřete na samostatné kartě uživatelské rozhraní služby Azure Data Factory. 

## <a name="launch-copy-data-tool"></a>Spuštění nástroje pro kopírování dat

1. Na stránce Začínáme klikněte na dlaždici **Kopírovat data** a spusťte nástroj pro kopírování dat. 

   ![Dlaždice nástroje pro kopírování dat](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-tile.png)
2. Na stránce **Vlastnosti** nástroje pro kopírování dat klikněte na **Další**. Na této stránce můžete zadat název a popis kanálu. 

    ![Nástroj pro kopírování dat – stránka Vlastnosti](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-properties-page.png)
3. Na stránce **Zdrojové úložiště dat** vyberte **Azure Blob Storage** a klikněte na **Další**.

    ![Stránka Zdrojové úložiště dat](./media/quickstart-create-data-factory-copy-data-tool/source-data-store-page.png)
4. Na stránce **Zadejte účet služby Azure Blob Storage** vyberte z rozevíracího seznamu **název svého účtu úložiště** a klikněte na Další. 

    ![Zadání účtu úložiště objektů blob](./media/quickstart-create-data-factory-copy-data-tool/specify-blob-storage-account.png)
5. Na stránce **Zvolte vstupní soubor nebo složku** proveďte následující kroky:

    1. Přejděte do složky **adftutorial/input**. 
    2. Vyberte soubor **emp.txt**.
    3. Klikněte na **Zvolit**. Tento krok můžete přeskočit dvojím kliknutím na soubor **emp.txt**. 
    4. Klikněte na **Další**. 

    ![Zvolte vstupní soubor nebo složku](./media/quickstart-create-data-factory-copy-data-tool/choose-input-file-folder.png)
6. Na stránce **Nastavení formátu souboru** si všimněte, že nástroj automaticky rozpozná oddělovače sloupců a řádků, a klikněte na **Další**. Na této stránce si také můžete prohlédnou data a schéma vstupních dat. 

    ![Stránka Nastavení formátu souboru](./media/quickstart-create-data-factory-copy-data-tool/file-format-settings-page.png)
7. Na stránce **Cílové úložiště dat** vyberte **Azure Blob Storage** a klikněte na **Další**. 

    ![Stránka Cílové úložiště dat](./media/quickstart-create-data-factory-copy-data-tool/destination-data-store-page.png)    
8. Na stránce **Zadejte účet služby Azure Blob Storage** vyberte svůj účet služby Azure Blob Storage a klikněte na **Další**. 

    ![Stránka Zadejte úložiště dat jímky](./media/quickstart-create-data-factory-copy-data-tool/specify-sink-blob-storage-account.png)
9. Na stránce **Zvolte výstupní soubor nebo složku** proveďte následující kroky: 

    1. Jako **cestu ke složce** zadejte **adftutorial/output**.
    2. Jako **název souboru** zadejte **emp.txt**. 
    3. Klikněte na **Další**. 

    ![Zvolte výstupní soubor nebo složku](./media/quickstart-create-data-factory-copy-data-tool/choose-output-file-folder.png) 
10. Na stránce **Nastavení formátu souboru**  klikněte na **Další**. 

    ![Stránka Nastavení formátu souboru](./media/quickstart-create-data-factory-copy-data-tool/file-format-settings-output-page.png)
11. Na stránce **Nastavení** klikněte na **Další**. 

    ![Stránka Upřesnit nastavení](./media/quickstart-create-data-factory-copy-data-tool/advanced-settings-page.png)
12. Na stránce **Souhrn** zkontrolujte všechna nastavení a klikněte na Další. 

    ![Stránka souhrnu](./media/quickstart-create-data-factory-copy-data-tool/summary-page.png)
13. Na stránce **Nasazení dokončeno** klikněte na **Monitorovat** a začněte monitorovat vytvořený kanál. 

    ![Stránka Nasazení](./media/quickstart-create-data-factory-copy-data-tool/deployment-page.png)
14. Aplikace se přepne na kartu **Monitorování**. Na této stránce se zobrazí stav kanálu. Kliknutím na **Aktualizovat** seznam aktualizujte. 
    
    ![Stránka Monitorování spuštění kanálu](./media/quickstart-create-data-factory-copy-data-tool/monitor-pipeline-runs-page.png)
15. Klikněte na odkaz **Zobrazit spuštění aktivit** ve sloupci Akce. Kanál má pouze jednu aktivitu typu **Kopírování**. 

    ![Stránka Spuštění aktivit](./media/quickstart-create-data-factory-copy-data-tool/activity-runs.png)
16. Pokud chcete zobrazit podrobnosti o operaci kopírování, klikněte na odkaz **Podrobnosti** (obrázek brýlí) ve sloupci **Akce**. Podrobnosti o vlastnostech najdete v tématu [Přehled aktivity kopírování](copy-activity-overview.md). 

    ![Podrobnosti o aktivitě kopírování](./media/quickstart-create-data-factory-copy-data-tool/copy-operation-details.png)
17. Ověřte, že se ve složce **output** kontejneru **adftutorial** vytvořil soubor **emp.txt**. Pokud složka output neexistuje, služba Data Factory ji automaticky vytvoří. 
18. Přepněte na kartu **Upravit**, kde můžete upravit propojené služby, datové sady a kanály. Další informace o jejich úpravách v uživatelském rozhraní služby Data Factory najdete v tématu [Vytvoření datové továrny pomocí webu Azure Portal](quickstart-create-data-factory-portal.md).

    ![Karta Upravit](./media/quickstart-create-data-factory-copy-data-tool/edit-tab.png)

## <a name="next-steps"></a>Další kroky
Kanál v této ukázce kopíruje data z jednoho umístění do jiného umístění v úložišti objektů blob v Azure. Projděte si [kurzy](tutorial-copy-data-portal.md), kde se dozvíte o použití služby Data Factory ve více scénářích. 