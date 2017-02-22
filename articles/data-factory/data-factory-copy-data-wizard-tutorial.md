---
title: "Kurz: Vytvoření kanálu pomocí průvodce kopírováním | Dokumentace Microsoftu"
description: "V tomto kurzu vytvoříte kanál služby Azure Data Factory s aktivitou kopírování pomocí průvodce kopírováním podporovaného službou Data Factory"
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b87afb8e-53b7-4e1b-905b-0343dd096198
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/02/2017
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: fbf77e9848ce371fd8d02b83275eb553d950b0ff
ms.openlocfilehash: 5a50f583831b398ae22416e7ade23c33846de55c


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-data-factory-copy-wizard"></a>Kurz: Vytvoření kanálu s aktivitou kopírování pomocí průvodce kopírováním služby Data Factory.
> [!div class="op_single_selector"]
> * [Přehled a požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Šablona Azure Resource Manageru](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

**Průvodce kopírováním** služby Azure Data Factory vám umožní snadno a rychle vytvořit kanál, který implementuje scénář příjmu/pohybu dat. Proto doporučujeme použít průvodce jako první krok k vytvoření ukázkového kanálu pro scénář pohybu dat. Tento návod ukazuje, jak vytvořit objekt pro vytváření dat Azure, spustit Průvodce kopírováním a projít posloupností kroků poskytnutí podrobností o vašem scénáři příjmu/pohybu dat. Po dokončení kroků v průvodci se automaticky vytvoří kanál s aktivitou kopírování pro kopírování dat z úložiště Azure Blob Storage do Azure SQL Database. Podrobnosti o aktivitě kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md). 

## <a name="prerequisites"></a>Požadavky
- Projděte si [Přehled a požadavky kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md), kde najdete informace o kurzu, a proveďte **nutné** kroky.


## <a name="create-data-factory"></a>Vytvoření objektu pro vytváření dat
V tomto kroku vytvoříte pomocí webu Azure Portal objekt pro vytváření dat Azure s názvem **ADFTutorialDataFactory**.

1. Po přihlášení na webu [Azure Portal](https://portal.azure.com) klikněte v levém horním rohu na možnost **NOVÝ**, klikněte na možnost **Inteligence a analýza** a pak klikněte na možnost **Objekt pro vytváření dat**. 
   
   ![Nový -> Objekt pro vytváření dat](./media/data-factory-copy-data-wizard-tutorial/new-data-factory-menu.png)
2. V okně **Nový objekt pro vytváření dat**:
   
   1. Do pole **Název** zadejte **ADFTutorialDataFactory**.
       Název objektu pro vytváření dat Azure musí být globálně jedinečný. Pokud se zobrazí chyba **Název objektu pro vytváření dat ADFTutorialDataFactory není k dispozici**, změňte název objektu pro vytváření dat (třeba na váš_název_ADFTutorialDataFactory) a zkuste to znovu. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.  
      
       ![Název objektu pro vytváření dat není k dispozici](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-not-available.png)
      
      > [!NOTE]
      > Název objektu pro vytváření dat se může v budoucnu zaregistrovat jako název DNS, takže pak bude veřejně viditelný.
      > 
      > 
   2. Vyberte své **předplatné** Azure.
   3. Pro skupinu prostředků proveďte jeden z následujících kroků: 
      
      - Vyberte možnost **Použít existující** a vyberte existující skupinu prostředků.
      - Vyberte možnost **Vytvořit nový** a zadejte název pro skupinu prostředků.
         
          Některé kroky v tomto kurzu vychází z předpokladu, že pro skupinu prostředků použijete název **ADFTutorialResourceGroup**. Informace o skupinách prostředků najdete v článku [Použití skupin prostředků ke správě prostředků Azure](../azure-resource-manager/resource-group-overview.md).
   4. Vyberte **umístění** pro příslušný objekt pro vytváření dat.
   5. Zaškrtněte políčko **Připnout na řídicí panel** v dolní části okna.  
   6. Klikněte na možnost **Vytvořit**.
      
       ![Okno Nový objekt pro vytváření dat](media/data-factory-copy-data-wizard-tutorial/new-data-factory-blade.png)            
3. Po vytvoření se zobrazí okno **Objekt pro vytváření dat**, jak je znázorněno na následujícím obrázku:
   
   ![Domovská stránka objektu pro vytváření dat](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-home-page.png)

## <a name="launch-copy-wizard"></a>Spuštění průvodce kopírováním
1. Na domovské stránce objektu pro vytváření dat klikněte na dlaždici **Kopírovat data**. Spustí se **průvodce kopírováním**. 
   
   > [!NOTE]
   > Pokud zjistíte, že se webový prohlížeč zasekl ve fázi „Autorizace…“, zakažte / zrušte zaškrtnutí políčka **Block third party cookies and site data** (Blokovat data souborů cookie a webů třetích stran) nebo nechte toto nastavení povolené a vytvořte výjimku pro **login.microsoftonline.com**. Potom zkuste průvodce znovu spustit.
   > 
   > 
2. Na stránce **Vlastnosti**:
   
   1. Jako **Název úlohy** zadejte **CopyFromBlobToAzureSql**.
   2. Zadejte **popis** (volitelné).
   3. Změňte položky **Datum a čas zahájení** a **Datum a čas ukončení** tak, aby datum ukončení bylo nastaveno na dnešek a datum zahájení bylo pět dnů před dneškem.  
   4. Klikněte na **Další**.  
      
      ![Nástroj pro kopírování – stránka Vlastnosti](./media/data-factory-copy-data-wizard-tutorial/copy-tool-properties-page.png) 
3. Na stránce **Source data store** (Zdrojové úložiště dat) klikněte na dlaždici **Azure Blob Storage**. Tato stránka slouží k zadání zdrojového úložiště dat pro úlohu kopírování. Můžete použít existující propojenou službu úložiště dat nebo zadat nové úložiště dat. Pokud chcete použít stávající propojenou službu, klikněte na **Z EXISTUJÍCÍCH PROPOJENÝCH SLUŽEB** a vyberte požadovanou propojenou službu. 
   
    ![Nástroj pro kopírování – stránka zdrojového úložiště dat](./media/data-factory-copy-data-wizard-tutorial/copy-tool-source-data-store-page.png)
4. Na stránce **Specify the Azure Blob storage account** (Zadejte účet Azure Blob Storage):
   
   1. Do pole **Název propojené služby** zadejte **AzureStorageLinkedService**.
   2. Ujistěte se, že je pro položku **Metoda výběru účtu** vybrána možnost **Z předplatných Azure**.
   3. Vyberte své **předplatné** Azure.  
   4. V seznamu účtů úložiště Azure dostupných ve zvoleném předplatném vyberte požadovaný **účet úložiště Azure**. Také si můžete zvolit, že chcete zadat nastavení účtu úložiště ručně. Stačí u položky **Account selection method** (Metoda výběru účtu) vybrat možnost **Enter manually ** (Zadat ručně) a potom kliknout na **Další**. 
      
      ![Nástroj pro kopírování – zadání účtu Azure Blob Storage](./media/data-factory-copy-data-wizard-tutorial/copy-tool-specify-azure-blob-storage-account.png)
5. Na stránce **Choose the input file or folder** (Zvolte vstupní soubor nebo složku):
   
   1. Přejděte na složku **adftutorial**.
   2. Vyberte soubor **emp.txt** a klikněte na **Vybrat**.
   3. Klikněte na **Další**. 
      
      ![Nástroj pro kopírování – volba vstupního souboru nebo složky](./media/data-factory-copy-data-wizard-tutorial/copy-tool-choose-input-file-or-folder.png)
6. Na stránce **Zvolte vstupní soubor nebo složku** klikněte na **Další**. Nezaškrtávejte políčko **Binární kopie**. 
   
    ![Nástroj pro kopírování – volba vstupního souboru nebo složky](./media/data-factory-copy-data-wizard-tutorial/chose-input-file-folder.png) 
7. Na stránce **Nastavení formátu souboru** jsou uvedeny oddělovače a schéma, které je automaticky zjištěno průvodcem při analýze souboru. Můžete také zadat oddělovače ručně, pokud chcete, aby Průvodce kopírováním zastavil automatické zjišťování, nebo pokud je chcete přepsat. Po zkontrolování oddělovačů a náhledu dat klikněte na **Další**. 
   
    ![Nástroj pro kopírování – nastavení formátu souboru](./media/data-factory-copy-data-wizard-tutorial/copy-tool-file-format-settings.png)  
8. Na stránce Cílové úložiště dat vyberte možnost **Azure SQL Database** a potom klikněte na **Další**.
   
    ![Nástroj pro kopírování – volba cílového úložiště](./media/data-factory-copy-data-wizard-tutorial/choose-destination-store.png)
9. Na stránce **Specify the Azure SQL database** (Určete databázi SQL Azure):
   
   1. Do pole **Název připojení** zadejte hodnotu **AzureSqlLinkedService**.
   2. Ujistěte se, že je pro položku **Metoda výběru serveru/databáze** vybrána možnost **Z předplatných Azure**.
   3. Vyberte své **předplatné** Azure.  
   4. Vyberte možnosti v polích **Název serveru** a **Databáze**.
   5. Zadejte **Uživatelské jméno** a **Heslo**.
   6. Klikněte na **Další**.  
      
      ![Nástroj pro kopírování – určení Azure SQL Database](./media/data-factory-copy-data-wizard-tutorial/specify-azure-sql-database.png)
10. Na stránce **Mapování tabulek** vyberte v rozevíracím seznamu poli **Cíl** možnost **emp** a potom klikněte na **šipku dolů** (volitelné). Tím zobrazíte schéma a náhled dat.
    
     ![Nástroj pro kopírování – mapování tabulek](./media/data-factory-copy-data-wizard-tutorial/copy-tool-table-mapping-page.png) 
11. Na stránce **Schema mapping** (Mapování schématu) klikněte na tlačítko **Další**.
    
    ![Nástroj pro kopírování – mapování schématu](./media/data-factory-copy-data-wizard-tutorial/schema-mapping-page.png)
12. Na stránce **Nastavení výkonu** klikněte na **Další**. 
    
    ![Nástroj pro kopírování – nastavení výkonu](./media/data-factory-copy-data-wizard-tutorial/performance-settings.png)
13. Na stránce **Souhrn** zkontrolujte informace a klikněte na **Dokončit**. Průvodce v objektu pro vytváření dat (ze kterého jste průvodce kopírováním spustili) vytvoří dvě propojené služby, dvě datové sady (vstupní a výstupní) a jeden kanál. 
    
    ![Nástroj pro kopírování – nastavení výkonu](./media/data-factory-copy-data-wizard-tutorial/summary-page.png)

## <a name="launch-monitor-and-manage-application"></a>Spuštění aplikace pro monitorování a správu
1. Na stránce **Nasazení** klikněte na následující odkaz: `Click here to monitor copy pipeline`.
   
   ![Nástroj pro kopírování – nasazení bylo úspěšné](./media/data-factory-copy-data-wizard-tutorial/copy-tool-deployment-succeeded.png)  
2. Pomocí pokynů v tématu [Monitorování a správa kanálu pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) zjistíte, jak můžete vytvořený kanál monitorovat. Kliknutím na ikonu **Aktualizovat** v seznamu **OKNA AKTIVIT** zobrazíte řez. 
   
   ![Monitorovací aplikace](./media/data-factory-copy-data-wizard-tutorial/monitoring-app.png) 
   
   
   Kliknutím na tlačítko **Aktualizovat** v seznamu **OKNA AKTIVITY** ve spodní části zobrazte nejnovější stav. Zobrazení se neobnovuje automaticky. 

> [!NOTE]
> Datový kanál v tomto kurzu kopíruje data ze zdrojového úložiště dat do cílového úložiště dat. Neprovádí transformaci vstupních dat, aby vytvořil výstupní data. Kurz předvádějící způsoby transformace dat pomocí Azure Data Factory najdete v tématu popisujícím [kurz vytvoření prvního kanálu, který umožňuje transformovat data pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).
> 
> Dvě aktivity můžete zřetězit (spustit jednu aktivitu po druhé) nastavením výstupní datové sady jedné aktivity jako vstupní datové sady druhé aktivity. Podrobné informace najdete v tématu s popisem [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md).

## <a name="see-also"></a>Viz také
| Téma | Popis |
|:--- |:--- |
| [Kanály](data-factory-create-pipelines.md) |Tento článek vám pomůže pochopit kanály a aktivity ve službě Azure Data Factory a porozumět tomu, jak se dají ve vaší situaci nebo firmě použít k sestavení kompletních pracovních postupů založených na datech. |
| [Datové sady](data-factory-create-datasets.md) |Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) |Tento článek vysvětluje aspekty plánování a provádění aplikačního modelu služby Azure Data Factory. |




<!--HONumber=Feb17_HO1-->


