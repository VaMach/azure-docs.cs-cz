<properties 
    pageTitle="Kurz: Vytvoření kanálu pomocí průvodce kopírováním" 
    description="V tomto kurzu vytvoříte kanál služby Azure Data Factory s aktivitou kopírování pomocí průvodce kopírováním podporovaného službou Data Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="05/16/2016" 
    ms.author="spelluru"/>

# Kurz: Vytvoření kanálu s aktivitou kopírování pomocí průvodce kopírováním služby Data Factory.
> [AZURE.SELECTOR]
- [Přehled kurzu](data-factory-get-started.md)
- [Pomocí editoru služby Data Factory](data-factory-get-started-using-editor.md)
- [Pomocí prostředí PowerShell](data-factory-monitor-manage-using-powershell.md)
- [Pomocí sady Visual Studio](data-factory-get-started-using-vs.md)
- [Pomocí průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md)

V tomto kurzu vytvoříte v objektu pro vytváření dat kanál s aktivitou kopírování pomocí průvodce kopírováním služby Data Factory. Nejdřív pomocí webu Azure Portal vytvoříte objekt pro vytváření dat a potom pomocí průvodce kopírováním vytvoříte pro službu Data Factory propojené služby, datové sady a kanál s aktivitou kopírování, která zkopíruje data ze Azure Blob Storage do Azure SQL Database.

> [AZURE.IMPORTANT] Než se pustíte do tohoto kurzu, přečtěte si článek [Přehled kurzu](data-factory-get-started.md) a proveďte nutné kroky.

## Vytvoření objektu pro vytváření dat
V tomto kroku vytvoříte pomocí webu Azure Portal objekt pro vytváření dat Azure s názvem **ADFTutorialDataFactory**.

1.  Po přihlášení na web [Azure Portal](https://portal.azure.com) klikněte v levém horním rohu na položku **+ NOVÝ**, v okně **Vytvořit** vyberte **Analýza dat** a v okně **Analýza dat** klikněte na **Objekt pro vytváření dat**. 

    ![Nový -> Objekt pro vytváření dat](./media/data-factory-copy-data-wizard-tutorial/new-data-factory-menu.png)

6. V okně **Nový objekt pro vytváření dat**:
    1. Do pole **Název** zadejte **ADFTutorialDataFactory**. 
    
        ![Okno Nový objekt pro vytváření dat](./media/data-factory-copy-data-wizard-tutorial/getstarted-new-data-factory.png)
    2. Klikněte na **NÁZEV SKUPINY PROSTŘEDKŮ** a postupujte takto:
        1. Klikněte na **Vytvořit novou skupinu prostředků**.
        2. V okně **Vytvořit skupinu prostředků** zadejte do pole **Název** skupiny prostředků název **ADFTutorialResourceGroup** a klikněte na **OK**. 

            ![Vytvoření skupiny prostředků](./media/data-factory-copy-data-wizard-tutorial/create-new-resource-group.png)

        Některé kroky v tomto kurzu vychází z předpokladu, že pro skupinu prostředků použijete název **ADFTutorialResourceGroup**. Informace o skupinách prostředků najdete v článku [Použití skupin prostředků ke správě prostředků Azure](../resource-group-overview.md).  
7. Všimněte si, že v okně **Nový objekt pro vytváření dat** je vybraná možnost **Add to Startboard** (Připnout na Úvodní panel).
8. V okně **Nový objekt pro vytváření dat** klikněte na **Vytvořit**.

    Název objektu pro vytváření dat Azure musí být globálně jedinečný. Pokud se zobrazí chyba **Název objektu pro vytváření dat ADFTutorialDataFactory není k dispozici**, změňte název objektu pro vytváření dat (třeba na váš_název_ADFTutorialDataFactory) a zkuste to znovu. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.  
     
    ![Název objektu pro vytváření dat není k dispozici](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-not-available.png)
    
    > [AZURE.NOTE] Název objektu pro vytváření dat se může v budoucnu zaregistrovat jako název DNS, takže pak bude veřejně viditelný.  

9. Klikněte na položku **OZNÁMENÍ** vlevo a podívejte se na případná oznámení procesu vytváření. Pokud máte otevřené okno **OZNÁMENÍ**, zavřete ho kliknutím na tlačítko **X**. 
10. Po vytvoření se zobrazí okno **OBJEKT PRO VYTVÁŘENÍ DAT**, jak vidíte níže.

    ![Domovská stránka objektu pro vytváření dat](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-home-page.png)

## Vytvoření kanálu

1. Na domovské stránce objektu pro vytváření dat klikněte na dlaždici **Kopírovat data**. Spustí se **průvodce kopírováním**. 

    > [AZURE.NOTE] Pokud zjistíte, že se webový prohlížeč zasekl ve fázi „Autorizace…“, zakažte / zrušte zaškrtnutí políčka **Block third party cookies and site data** (Blokovat data souborů cookie a webů třetích stran) nebo nechte toto nastavení povolené a vytvořte výjimku pro **login.microsoftonline.com**. Potom zkuste průvodce znovu spustit.
2. Na stránce **Vlastnosti**:
    1. Jako **Název úlohy** zadejte **CopyFromBlobToAzureSql**.
    2. Zadejte **popis** (volitelné).
    3. Všimněte si položek **Datum a čas zahájení** a **Datum a čas ukončení**. Změňte **Datum a čas ukončení** na den následující po položce **Datum a čas zahájení**. 
    3. Klikněte na **Další**.  

    ![Nástroj pro kopírování – stránka Vlastnosti](./media/data-factory-copy-data-wizard-tutorial/copy-tool-properties-page.png) 
3. Na stránce **Source data store** (Zdrojové úložiště dat) klikněte na dlaždici **Azure Blob Storage**. Tato stránka slouží k zadání zdrojového úložiště dat pro úlohu kopírování. Můžete použít existující propojenou službu úložiště dat nebo zadat nové úložiště dat. Pokud chcete použít existující propojenou službu, klikněte na **FROM EXISTING LINKED SERVICES** (Z existujících propojených služeb) a vyberte požadovanou propojenou službu. 

    ![Nástroj pro kopírování – stránka zdrojového úložiště dat](./media/data-factory-copy-data-wizard-tutorial/copy-tool-source-data-store-page.png)
5. Na stránce **Specify the Azure Blob storage account** (Zadejte účet Azure Blob Storage):
    1. Do pole **Název propojené služby** zadejte **AzureStorageLinkedService**.
    2. Ujistěte se, že je u položky **Account selection method** (Metoda výběru účtu) vybraná možnost **From Azure subscriptions** (Z předplatných Azure). 
    3. V seznamu účtů úložiště Azure dostupných ve zvoleném předplatném vyberte požadovaný **účet úložiště Azure**. Také si můžete zvolit, že chcete zadat nastavení účtu úložiště ručně. Stačí u položky **Account selection method** (Metoda výběru účtu) vybrat možnost **Enter manually ** (Zadat ručně) a potom kliknout na **Další**. 

    ![Nástroj pro kopírování – zadání účtu Azure Blob Storage](./media/data-factory-copy-data-wizard-tutorial/copy-tool-specify-azure-blob-storage-account.png)
6. Na stránce **Choose the input file or folder** (Zvolte vstupní soubor nebo složku):
    1. Přejděte na složku **adftutorial**.
    2. Vyberte soubor **emp.txt** a klikněte na **Vybrat**.
    3. Klikněte na **Další**. 

    ![Nástroj pro kopírování – volba vstupního souboru nebo složky](./media/data-factory-copy-data-wizard-tutorial/copy-tool-choose-input-file-or-folder.png)
7. Na stránce **File format settings** (Nastavení formátu souboru) vyberte **výchozí** hodnoty a klikněte na **Další**.

    ![Nástroj pro kopírování – nastavení formátu souboru](./media/data-factory-copy-data-wizard-tutorial/copy-tool-file-format-settings.png)  
8. Na stránce cílového úložiště dat klikněte na dlaždici **Azure SQL Database** a potom klikněte na **Další**.
9. Na stránce **Specify the Azure SQL database** (Určete databázi SQL Azure):
    1. Do pole **Název propojené služby** zadejte **AzureSqlLinkedService**. 
    2. Ujistěte se, že je položka **Server/database selection method** (Metoda výběru serveru/databáze) nastavená na hodnotu **From Azure subscriptions** (Z předplatných Azure).
    3. Vyberte možnosti v polích **Název serveru** a **Databáze**.
    4. Zadejte **Uživatelské jméno** a **Heslo**.
    5. Klikněte na **Další**.  
9. Na stránce **Mapování tabulek** vyberte v rozevíracím seznamu poli **Cíl** možnost **emp** a potom klikněte na **šipku dolů** (volitelné). Tím zobrazíte schéma a náhled dat.

    ![Nástroj pro kopírování – mapování tabulek](./media/data-factory-copy-data-wizard-tutorial/copy-tool-table-mapping-page.png) 
10. Na stránce **Schema mapping** (Mapování schématu) klikněte na tlačítko **Další**.
11. Na stránce **Souhrn** zkontrolujte informace a klikněte na **Dokončit**. Tím se v objektu pro vytváření dat (ze kterého jste průvodce kopírováním spustili) vytvoří dvě propojené služby, dvě datové sady (vstupní a výstupní) a jeden kanál. 
12. Na stránce **Nasazení bylo úspěšné** klikněte na **Click here to monitor copy pipeline** (Kliknutím sem můžete monitorovat kanál kopírování).

    ![Nástroj pro kopírování – nasazení bylo úspěšné](./media/data-factory-copy-data-wizard-tutorial/copy-tool-deployment-succeeded.png)  
13. V článku [Monitorování a správa kanálů pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) získáte informace o tom, jak můžete vytvořený kanál monitorovat.

    ![Monitorovací aplikace](./media/data-factory-copy-data-wizard-tutorial/monitoring-app.png) 
 

## Viz také
| Téma | Popis |
| :---- | :---- |
| [Aktivity přesunu dat](data-factory-data-movement-activities.md) | Tento článek obsahuje podrobné informace o aktivitě kopírování, kterou jste v tomto kurzu použili. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) | Tento článek vysvětluje aspekty plánování a provádění aplikačního modelu služby Azure Data Factory. |
| [Kanály](data-factory-create-pipelines.md) | Tento článek vám pomůže pochopit kanály a aktivity ve službě Azure Data Factory a porozumět tomu, jak se dají ve vaší situaci nebo podniku použít k sestavení kompletních pracovních postupů založených na datech. |
| [Datové sady](data-factory-create-datasets.md) | Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory.
| [Monitorování a správa kanálů pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) | Tento článek popisuje, jak monitorovat, spravovat a ladit kanály pomocí aplikace pro monitorování a správu. 


<!--HONumber=Jun16_HO2-->


