<properties 
    pageTitle="Kurz: Vytvoření kanálu s aktivitou kopírování pomocí editoru služby Data Factory" 
    description="V tomto kurzu vytvoříte kanál služby Azure Data Factory s aktivitou kopírování pomocí editoru služby Data Factory na webu Azure Portal." 
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
    ms.date="08/01/2016" 
    ms.author="spelluru"/>

# Kurz: Vytvoření kanálu s aktivitou kopírování pomocí editoru služby Data Factory
> [AZURE.SELECTOR]
- [Přehled kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Pomocí editoru služby Data Factory](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Pomocí prostředí PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Pomocí sady Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [Pomocí rozhraní REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [Pomocí průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md)


Tento kurz obsahuje následující kroky:

Krok | Popis
-----| -----------
[Vytvoření služby Azure Data Factory](#create-data-factory) | V tomto kroku vytvoříte objekt pro vytváření dat Azure s názvem **ADFTutorialDataFactory**.  
[Vytvoření propojených služeb](#create-linked-services) | V tomto kroku vytvoříte dvě propojené služby: **AzureStorageLinkedService** a **AzureSqlLinkedService**. AzureStorageLinkedService propojuje službu Azure Storage a AzureSqlLinkedService propojuje službu Azure SQL Database k objektu ADFTutorialDataFactory. Vstupní data pro kanál se nachází v kontejneru objektů blob v Azure Blob Storage a výstupní data budou uložena v tabulce v Azure SQL Database. Proto přidáváte tyto dvě úložiště dat jako propojené služby objektu pro vytváření dat.      
[Vytvoření vstupní a výstupní datové sady](#create-datasets) | V předchozím kroku jste vytvořili propojené služby, které odkazují na úložiště dat, která obsahují vstupní a výstupní data. V tomto kroku nadefinujete dvě tabulky objektu pro vytváření dat, **EmpTableFromBlob** a **EmpSQLTable**, které představují vstupní a výstupní data uložená v úložištích dat. Pro EmpTableFromBlob zadáte kontejner objektů blob, který obsahuje objekt blob se zdrojovými daty, a pro EmpSQLTable zadáte tabulku SQL, do které se uloží výstupní data. Zadejte také další vlastnosti, jako je například struktura dat, dostupnost dat a další. 
[Vytvoření kanálu](#create-pipeline) | V tomto kroku vytvoříte v objektu ADFTutorialDataFactory kanál s názvem **ADFTutorialPipeline**. Kanál má **aktivitu kopírování**, která kopíruje vstupní data z objektu blob Azure do výstupní tabulky Azure SQL. Aktivita kopírování provádí přesun dat ve službě Azure Data Factory a aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelným způsobem. Podrobnosti o aktivitě kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md). 
[Monitorování kanálu](#monitor-pipeline) | V tomto kroku budete monitorovat řezy vstupní a výstupní tabulky pomocí webu Azure Portal.

> [AZURE.IMPORTANT] 
> Než se pustíte do tohoto kurzu, přečtěte si článek [Přehled kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) a proveďte nutné kroky.

## Vytvoření objektu pro vytváření dat
V tomto kroku vytvoříte pomocí webu Azure Portal objekt pro vytváření dat Azure s názvem **ADFTutorialDataFactory**.

1.  Po přihlášení na web [Azure Portal][azure-portal] klikněte v levém dolním rohu na položku **NOVÝ**, v okně **Vytvořit** vyberte **Analýza dat** a v okně **Analýza dat** klikněte na **Objekt pro vytváření dat**. 

    ![Nový -> Objekt pro vytváření dat][image-data-factory-new-datafactory-menu]    

6. V okně **Nový objekt pro vytváření dat**:
    1. Do pole **Název** zadejte **ADFTutorialDataFactory**. 
    
        ![Okno Nový objekt pro vytváření dat][image-data-factory-getstarted-new-data-factory-blade]
    2. Klikněte na **NÁZEV SKUPINY PROSTŘEDKŮ** a postupujte takto:
        1. Klikněte na **Vytvořit novou skupinu prostředků**.
        2. V okně **Vytvořit skupinu prostředků** zadejte do pole **Název** skupiny prostředků název **ADFTutorialResourceGroup** a klikněte na **OK**. 

            ![Vytvoření skupiny prostředků][image-data-factory-create-resource-group]

        Některé kroky v tomto kurzu vychází z předpokladu, že pro skupinu prostředků použijete název **ADFTutorialResourceGroup**. Informace o skupinách prostředků najdete v článku [Použití skupin prostředků ke správě prostředků Azure](../resource-group-overview.md).  
7. Všimněte si, že v okně **Nový objekt pro vytváření dat** je vybraná možnost **Add to Startboard** (Připnout na Úvodní panel).
8. V okně **Nový objekt pro vytváření dat** klikněte na **Vytvořit**.

    Název objektu pro vytváření dat Azure musí být globálně jedinečný. Pokud se zobrazí chyba **Název objektu pro vytváření dat ADFTutorialDataFactory není k dispozici**, změňte název objektu pro vytváření dat (třeba na váš_název_ADFTutorialDataFactory) a zkuste to znovu. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.  
     
    ![Název objektu pro vytváření dat není k dispozici][image-data-factory-name-not-available]
    
    > [AZURE.NOTE] Název objektu pro vytváření dat se může v budoucnu zaregistrovat jako název DNS, takže pak bude veřejně viditelný.  
    > 
    > Instance služby Data Factory můžete vytvářet jenom tehdy, když jste přispěvatelem/správcem předplatného Azure.

9. Klikněte na položku **OZNÁMENÍ** vlevo a podívejte se na případná oznámení procesu vytváření. Pokud máte otevřené okno **OZNÁMENÍ**, zavřete ho kliknutím na tlačítko **X**. 
10. Po vytvoření se zobrazí okno **OBJEKT PRO VYTVÁŘENÍ DAT**, jak vidíte níže.

    ![Domovská stránka objektu pro vytváření dat][image-data-factory-get-stated-factory-home-page]

## Vytvoření propojených služeb
Propojené služby propojují úložiště dat nebo výpočetní služby s objektem pro vytváření dat Azure. Úložištěm dat může být Azure Storage, Azure SQL Database nebo místní databáze SQL Serveru.

V tomto kroku vytvoříte dvě propojené služby: **AzureStorageLinkedService** a **AzureSqlLinkedService**. Propojená služba AzureStorageLinkedService propojuje účet Azure Storage a AzureSqlLinkedService propojuje službu Azure SQL Database s objektem **ADFTutorialDataFactory**. Později v tomto kurzu vytvoříte kanál, který kopíruje data z kontejneru objektů blob ve službě AzureStorageLinkedService do tabulky SQL ve službě AzureSqlLinkedService.

### Vytvoření propojené služby pro účet úložiště Azure
1.  V okně **OBJEKT PRO VYTVÁŘENÍ DAT** klikněte na dlaždici **Vytvořit a nasadit**, abyste spustili **Editor** pro objekt pro vytváření dat.

    ![Dlaždice Vytvořit a nasadit][image-author-deploy-tile] 

     
5. V **editoru ** klikněte na panelu nástrojů na tlačítko **Nové datové úložiště** a z rozevírací nabídky vyberte **Úložiště Azure**. V pravém podokně by se měla zobrazit šablona JSON pro vytvoření propojené služby Azure Storage. 

    ![Tlačítko Nové datové úložiště v editoru][image-editor-newdatastore-button]
    
6. Položky **accountname** a **accountkey** nahraďte názvem účtu úložiště Azure a jeho klíčem. 

    ![Editor – Blob Storage – JSON](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-json.png)    
    
    Podrobné informace o vlastnostech JSON najdete v tématu [JSON Scripting Reference](http://go.microsoft.com/fwlink/?LinkId=516971) (Referenční příručka skriptování JSON).

6. Službu AzureStorageLinkedService nasadíte kliknutím na **Nasadit** na panelu nástrojů. Potvrďte, že se v záhlaví okna zobrazila zpráva **ÚSPĚŠNÉ VYTVOŘENÍ PROPOJENÉ SLUŽBY**.

    ![Editor – Blob Storage – nasazení][image-editor-blob-storage-deploy]

### Vytvoření propojené služby pro Azure SQL Database
1. V **editoru služby Data Factory** klikněte na panelu nástrojů na tlačítko **Nové datové úložiště** a z rozevírací nabídky vyberte **Azure SQL Database**. V pravém podokně by se měla zobrazit šablona JSON pro vytvoření propojené služby Azure SQL.

    ![Editor – Azure SQL – nastavení][image-editor-azure-sql-settings]

2. Položky **servername**, **databasename**, **username@servername** a **password** nahraďte názvem serveru SQL Azure, názvem databáze, uživatelským účtem a heslem. 
3. Službu AzureSqlLinkedService vytvoříte a nasadíte kliknutím na **Nasadit** na panelu nástrojů. 
   

## Vytvoření datových sad
V předchozím kroku jste vytvořili propojené služby **AzureStorageLinkedService** a **AzureSqlLinkedService**, abyste propojili účet úložiště Azure a Azure SQL Database k objektu pro vytváření dat: **ADFTutorialDataFactory**. V tomto kroku nadefinujete dvě tabulky objektu pro vytváření dat, **EmpTableFromBlob** a **EmpSQLTable**. Ty představují vstupní a výstupní data uložená v úložištích dat, na která odkazují objekty AzureStorageLinkedService a AzureSqlLinkedService. Pro EmpTableFromBlob zadáte kontejner objektů blob, který obsahuje objekt blob se zdrojovými daty, a pro EmpSQLTable zadáte tabulku SQL, do které se uloží výstupní data. 

### Vytvoření vstupní datové sady 
Tabulka je obdélníková datová sada a má schéma. V tomto kroku vytvoříte tabulku s názvem **EmpBlobTable**, která odkazuje na kontejner objektů blob ve službě Azure Storage reprezentované propojenou službou **AzureStorageLinkedService**.

1. V **editoru** služby Data Factory klikněte na panelu nástrojů na tlačítko **Nová datová sada** a v rozevírací nabídce klikněte na **Tabulka objektů blob**. 
2. Nahraďte kód JSON v pravém podokně následujícím fragmentem kódu JSON: 

        {
          "name": "EmpTableFromBlob",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "fileName": "emp.txt",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

        
     Je třeba počítat s následujícím: 
    
    - Vlastnost **type** datové sady je nastavená na **AzureBlob**.
    - Vlastnost **linkedServiceName** je nastavená na **AzureStorageLinkedService**. Tuto propojenou službu jste vytvořili v kroku 2.
    - Vlastnost **folderPath** je nastavená na kontejner **adftutorial**. Můžete taky zadat název objektu blob ve složce. Pokud neurčíte název objektu blob, budou za vstupní data považována data ze všech objektů blob v kontejneru.  
    - Vlastnost **type** formátu je nastavená na **TextFormat**.
    - V textovém souboru existují dvě pole, **FirstName** a **LastName**, oddělená čárkou (**columnDelimiter**). 
    - Vlastnost **availability** je nastavená na **hourly** (**frequency** je nastavená na**hour** a **interval** je nastavená na **1**), takže služba Data Factory bude každou hodinu hledat vstupní data v kořenové složce v zadaném kontejneru objektů blob (**adftutorial**). 
    

    Pokud neurčíte **fileName** pro **vstupní** **tabulku**, všechny soubory nebo objekty blob ze vstupní složky (**folderPath**) se považují za vstupy. Pokud zadáte fileName v kódu JSON, bude se za vstup považovat jenom zadaný soubor nebo objekt blob.
 
    Pokud nezadáte **fileName** pro **výstupní tabulku**, generované soubory v **folderPath** se pojmenují podle následujícího formátu: Data.&lt;identifikátor GUID\&gt;.txt (například: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt).

    Pokud chcete nastavit **folderPath** a **fileName** dynamicky podle času **SliceStart**, použijte vlastnost **partitionedBy**. V následujícím příkladu folderPath používá rok, měsíc a den z vlastnosti SliceStart (čas zahájení zpracování řezu) a fileName používá hodinu z vlastnosti SliceStart. Pokud například začne být řez vytvářen v době 2014-10-20T08:00:00, vlastnost folderName je nastavená na wikidatagateway/wikisampledataout/2014/10/20 a vlastnost fileName je nastavená na 08.csv. 

        "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
            { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

    Podrobné informace o vlastnostech JSON najdete v tématu [JSON Scripting Reference](http://go.microsoft.com/fwlink/?LinkId=516971) (Referenční příručka skriptování JSON).

2. Tabulku **EmpTableFromBlob** vytvoříte a nasadíte kliknutím na **Nasadit** na panelu nástrojů. Potvrďte, že se v záhlaví okna editoru zobrazila zpráva **ÚSPĚŠNÉ VYTVOŘENÍ TABULKY**.

### Vytvoření výstupní datové sady
V této části kroku vytvoříte výstupní tabulku s názvem **EmpSQLTable**, která odkazuje na tabulku SQL ve službě Azure SQL Database reprezentované propojenou službou **AzureSqlLinkedService**. 

1. V **editoru** služby Data Factory klikněte na panelu nástrojů na tlačítko **Nová datová sada** a v rozevírací nabídce klikněte na **Tabulka Azure SQL**. 
2. Nahraďte kód JSON v pravém podokně následujícím fragmentem kódu JSON:

        {
          "name": "EmpSQLTable",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

        
     Je třeba počítat s následujícím: 
    
    * Vlastnost **type** datové sady je nastavená na **AzureSQLTable**.
    * Vlastnost **linkedServiceName** je nastavená na **AzureSqlLinkedService** (tuto propojenou službu jste vytvořili v kroku 2).
    * Vlastnost **tablename** je nastavená na **emp**.
    * V tabulce emp v databázi existují tři sloupce, **ID**, **FirstName** a **LastName**, ale ID je sloupec identity, takže je zde potřeba zadat jenom **FirstName** a **LastName**.
    * Vlastnost **availability** je nastavená na **hourly** (**frequency** je nastavená na **hour** a **interval** je nastavená na **1**).  Služba Data Factory bude generovat řez výstupních dat do tabulky **emp** ve službě Azure SQL Database každou hodinu.


3. Tabulku **EmpSQLTable** vytvoříte a nasadíte kliknutím na **Nasadit** na panelu nástrojů.


## Vytvoření kanálu
V tomto kroku vytvoříte kanál pomocí **aktivity kopírování**, který používá **EmpTableFromBlob** jako vstup a **EmpSQLTable** jako výstup.

1. V **editoru** služby Data Factory klikněte na panelu nástrojů na tlačítko **Nový kanál**. Pokud tlačítko nevidíte, klikněte na panelu nástrojů na **... (tři tečky)**. Případně můžete ve stromovém zobrazení kliknout pravým tlačítkem na **anály** a pak kliknout na **Nový kanál**.

    ![Editor – tlačítko Nový kanál][image-editor-newpipeline-button]
 
2. Nahraďte kód JSON v pravém podokně následujícím fragmentem kódu JSON: 
        
        {
          "name": "ADFTutorialPipeline",
          "properties": {
            "description": "Copy data from a blob to Azure SQL table",
            "activities": [
              {
                "name": "CopyFromBlobToSQL",
                "description": "Push Regional Effectiveness Campaign data to Azure SQL database",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "EmpTableFromBlob"
                  }
                ],
                "outputs": [
                  {
                    "name": "EmpSQLTable"
                  }
                ],
                "typeProperties": {
                  "source": {
                    "type": "BlobSource"
                  },
                  "sink": {
                    "type": "SqlSink",
                    "writeBatchSize": 10000,
                    "writeBatchTimeout": "60:00:00"
                  }
                },
                "Policy": {
                  "concurrency": 1,
                  "executionPriorityOrder": "NewestFirst",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2015-07-12T00:00:00Z",
            "end": "2015-07-13T00:00:00Z"
          }
        } 

    Je třeba počítat s následujícím:

    - V části aktivit je jenom jedna aktivita, jejíž vlastnost **type** je nastavená na **CopyActivity**.
    - Vstup aktivity je nastavený na **EmpTableFromBlob** a výstup aktivity je nastavený na **EmpSQLTable**.
    - V části **transformace** je vlastnost **BlobSource** určená jako typ zdroje a **SqlSink** jako typ jímky.

    Nahraďte hodnotu vlastnosti **start** aktuálním dnem a **end** následujícím dnem. Můžete zadat jenom část data a přeskočit část času. Například „2015-02-03“ je ekvivalentní hodnotě „2015-02-03T00:00:00Z“.
    
    Počáteční a koncové hodnoty data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601). Například: 2014-10-14T16:32:41Z. Čas hodnoty **end** je nepovinný, ale my ho v tomto kurzu použijeme. 
    
    Pokud nezadáte hodnotu vlastnosti **end**, vypočítá se jako „**start + 48 hodin**“. Pokud chcete kanál spouštět bez omezení, zadejte vlastnosti **end** hodnotu **9999-09-09**.
    
    V uvedeném příkladu je 24 datových řezů, protože se vytvářejí každou hodinu.
    
    Podrobné informace o vlastnostech JSON najdete v tématu [JSON Scripting Reference](http://go.microsoft.com/fwlink/?LinkId=516971) (Referenční příručka skriptování JSON).

4. Kanál **ADFTutorialPipeline** vytvoříte a nasadíte kliknutím na **Nasadit** na panelu nástrojů. Potvrďte, že se zobrazila zpráva **ÚSPĚŠNÉ VYTVOŘENÍ KANÁLU**.
5. Teď zavřete okno **Editor** kliknutím na **X**. Klikněte na **X** ještě jednou a zavřete okno ADFTutorialDataFactory s panelem nástrojů a stromovým zobrazením. Pokud se zobrazí zpráva **Vaše neuložené úpravy se zahodí**, klikněte na **OK**.
6. Měli byste se vrátit do okna **OBJEKT PRO VYTVÁŘENÍ DAT** pro **ADFTutorialDataFactory**.

**Blahopřejeme!** Úspěšně jste vytvořili objekt pro vytváření dat Azure, propojené služby, tabulky a kanál a naplánovali jste kanál.   
 
### Zobrazit objektu pro vytváření dat v zobrazení diagramu 
1. V okně **OBJEKT PRO VYTVÁŘENÍ DAT** klikněte na **Diagram**.

    ![Okno objekt pro vytváření dat – dlaždice Diagram][image-datafactoryblade-diagramtile]

2. Zobrazený diagram by měl vypadat přibližně takto: 

    ![Zobrazení diagramu][image-data-factory-get-started-diagram-blade]

    Můžete provést přiblížení, oddálení, zvětšení na 100 %, přizpůsobení zobrazení, automatické umísťování kanálů a tabulek a zobrazení informací o rodokmenu (zvýrazní nadřazené a podřízené položky vybraných položek).  Poklikáním na objekt (vstupní nebo výstupní tabulka nebo kanál) můžete zobrazit vlastnosti. 
3. Klikněte pravým tlačítkem na **ADFTutorialPipeline** v zobrazení diagramu a pak klikněte na **Otevřít kanál**. Měli by se zobrazit aktivity v kanálu spolu se vstupními a výstupními datovými sadami pro aktivity. V tomto kurzu máte v kanálu jenom jednu aktivitu (aktivita kopírování) se vstupní datovou sadou EmpTableBlob a výstupní datovou sadou EmpSQLTable.   

    ![Otevření kanálu](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenPipeline.png)

4. Pokud se chcete vrátit do zobrazení diagramu, klikněte v zobrazení cesty v levém horním rohu na **Objekt pro vytváření dat**. Zobrazení diagramu zobrazí všechny kanály. V tomto příkladu jste vytvořili jenom jeden kanál.   
 

## Monitorování kanálu
V tomto kroku budete pomocí webu Azure Portal monitorovat, co se děje v objektu pro vytváření dat Azure. 

1. Přejděte na web [Azure Portal (Preview)][azure-portal], pokud ho ještě nemáte otevřený. 
2. Pokud není okno pro **ADFTutorialDataFactory** otevřené, otevřete ho kliknutím na **ADFTutorialDataFactory** na **úvodním panelu**. 
3. Měli byste vidět počet a názvy tabulek a kanálů, které jste
4. vytvořili v tomto okně.

    ![Domovská stránka s názvy][image-data-factory-get-started-home-page-pipeline-tables]

4. Teď klikněte na dlaždici **Datové sady**.
5. V okně **Datové sady** klikněte na **EmpTableFromBlob**. Toto je vstupní tabulka pro **ADFTutorialPipeline**.

    ![Datové sady s vybraným objektem EmpTableFromBlob][image-data-factory-get-started-datasets-emptable-selected]   
5. Všimněte si, že se už vytvořily datové řezy až do aktuálního času a jsou ve stavu **Připraveno**, protože soubor **emp.txt** celou dobu existuje v kontejneru objektů blob: **adftutorial\input**. Potvrďte, že se žádné řezy nezobrazují v části **Řezy, které v poslední době selhaly** dole.

    Oba seznamy **Řezy, které se v poslední době aktualizovaly** a **Řezy, které v poslední době selhaly** jsou řazené podle **DOBY POSLEDNÍ AKTUALIZACE**. Doba aktualizace řezu se změní v následujících situacích. 
    

    -  Provedete aktualizaci stavu řezu ručně, například pomocí **Set-AzureRmDataFactorySliceStatus** (nebo) kliknutím na **SPUSTIT** v okně **ŘEZ** pro příslušný řez.
    -  Stav řezu se změní z důvodu spuštění (např. při zahájení, ukončení a selhání spuštění, při úspěšném dokončení atd.).
 
    Pokud chcete zobrazit větší seznam řezů, klikněte na název seznamů nebo **... (tři tečky)**. Řezy můžete filtrovat kliknutím na **Filtr** v panelu nástrojů.  
    
    Pokud chcete zobrazit datové řezy seřazené podle času zahájení nebo konce, klikněte na dlaždici **Datové řezy (podle času řezu)**.   

    ![Datové řezy podle času řezu][DataSlicesBySliceTime]   

6. Teď v okně **Datové sady** klikněte na **EmpSQLTable**. Toto je výstupní tabulka pro **ADFTutorialPipeline**.

    ![okno datové sady][image-data-factory-get-started-datasets-blade]



     
6. Měli byste vidět okno **EmpSQLTable**, jak je zobrazeno dál:

    ![okno tabulky][image-data-factory-get-started-table-blade]
 
7. Všimněte si, že se už vytvořily datové řezy až do aktuálního času a jsou ve stavu **Připraveno**. Žádné řezy nejsou uvedené v části **Problémové řezy** dole.
8. Klikněte na **... (tři tečky)**, abyste zobrazili všechny řezy.

    ![okno datové řezy][image-data-factory-get-started-dataslices-blade]

9. Klikněte na libovolný datový řez ze seznamu a mělo by se zobrazit okno **DATOVÝ ŘEZ**.

    ![okno datový řez][image-data-factory-get-started-dataslice-blade]
  
    Není-li řez ve stavu **Připraveno**, zobrazí se v seznamu **Upstreamové datové řezy, které nejsou připraveny** upstreamové datové řezy, které nejsou připravené a které blokují spuštění aktuálního řezu. 

11. V okně **DATOVÝ ŘEZ**byste měli v seznamu dole vidět všechna spuštění aktivit. Klikněte na **spuštění aktivit** a zobrazte okno **PODROBNOSTI O SPUŠTĚNÍ AKTIVIT**. 

    ![Podrobnosti o spuštění aktivit][image-data-factory-get-started-activity-run-details]

    
12. Klikejte na tlačítko **X**, dokud nezavřete všechna okna a nevrátíte se zpátky do domovského okna pro **ADFTutorialDataFactory**.
14. (Volitelné) Klikněte na **Kanály** na domovské stránce pro **ADFTutorialDataFactory**, klikněte na **ADFTutorialPipeline** v okně **Kanály** a procházejte vstupní tabulky (**potřebováno**) nebo výstupní tabulky (**Vyprodukováno**).
15. Spusťte **SQL Server Management Studio**, připojte se ke službě Azure SQL Database a ověřte, že se řádky vložily do tabulky **emp** v databázi.

    ![Výsledky dotazu SQL][image-data-factory-get-started-sql-query-results]


## Souhrn 
V tomto kurzu jste vytvořili objekt pro vytváření dat Azure pro zkopírování dat z objektu blob Azure do Azure SQL Database. Použili jste web Azure Portal k vytvoření objektu pro vytváření dat, propojených služeb, datových sad a kanálu. Zde jsou základní kroky, které jste v tomto kurzu provedli:  

1.  Vytvořili jste **objekt pro vytváření dat** Azure.
2.  Vytvořili jste **propojené služby**:
    1. Propojená služba **Azure Storage** připojující účet úložiště Azure, který obsahuje vstupní data.    
    2. Propojená služba **Azure SQL** připojující službu Azure SQL Database, která obsahuje výstupní data. 
3.  Vytvořili jste **datové sady**, které popisují vstupní data a výstupní data pro kanály.
4.  Vytvořili jste **kanál** s **aktivitou kopírování**, která má jako zdroj **BlobSource** a jako jímku **SqlSink**.  


## Viz také
| Téma | Popis |
| :---- | :---- |
| [Aktivity přesunu dat](data-factory-data-movement-activities.md) | Tento článek obsahuje podrobné informace o aktivitě kopírování, kterou jste v tomto kurzu použili. |
| [Plánování a provádění](data-factory-scheduling-and-execution.md) | Tento článek vysvětluje aspekty plánování a provádění aplikačního modelu služby Azure Data Factory. |
| [Kanály](data-factory-create-pipelines.md) | Tento článek vám pomůže pochopit kanály a aktivity ve službě Azure Data Factory a porozumět tomu, jak se dají ve vaší situaci nebo firmě použít k sestavení kompletních pracovních postupů založených na datech. |
| [Datové sady](data-factory-create-datasets.md) | Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory.
| [Monitorování a správa kanálů pomocí monitorovací aplikace](data-factory-monitor-manage-app.md) | Tento článek popisuje, jak monitorovat, spravovat a ladit kanály pomocí aplikace pro monitorování a správu. 

<!--Link references-->
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[msdn-activities]: https://msdn.microsoft.com/library/dn834988.aspx
[msdn-linkedservices]: https://msdn.microsoft.com/library/dn834986.aspx
[data-factory-naming-rules]: https://msdn.microsoft.com/library/azure/dn835027.aspx

[azure-portal]: https://portal.azure.com/
[download-azure-powershell]: http://azure.microsoft.com/documentation/articles/install-configure-powershell
[sql-management-studio]: http://azure.microsoft.com/documentation/articles/sql-database-manage-azure-ssms/#Step2
[sql-cmd-exe]: https://msdn.microsoft.com/library/azure/ee336280.aspx

[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md
[data-factory-create-storage]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account



[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

<!--Image references-->

[DataSlicesBySliceTime]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSlicesBySliceTime.png

[image-data-factory-getstarted-new-data-factory-blade]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-new-data-factory.png

[image-data-factory-get-stated-factory-home-page]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-home-page.png

[image-author-deploy-tile]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-author-deploy-tile.png

[image-editor-newdatastore-button]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newdatastore-button.png

[image-editor-blob-storage-deploy]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-deploy.png

[image-editor-azure-sql-settings]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-azure-sql-settings.png

[image-editor-newpipeline-button]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newpipeline-button.png

[image-datafactoryblade-diagramtile]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactoryblade-diagramtile.png


[image-data-factory-get-started-diagram-blade]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-diagram-blade.png

[image-data-factory-get-started-home-page-pipeline-tables]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactory-home-page-pipeline-tables.png

[image-data-factory-get-started-datasets-blade]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datasets-blade.png

[image-data-factory-get-started-table-blade]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-table-blade.png

[image-data-factory-get-started-dataslices-blade]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslices-blade.png

[image-data-factory-get-started-dataslice-blade]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslice-blade.png

[image-data-factory-get-started-sql-query-results]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png

[image-data-factory-get-started-datasets-emptable-selected]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSetsWithEmpTableFromBlobSelected.png

[image-data-factory-get-started-activity-run-details]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/ActivityRunDetails.png

[image-data-factory-create-resource-group]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/CreateNewResourceGroup.png


[image-data-factory-new-datafactory-menu]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/NewDataFactoryMenu.png


[image-data-factory-name-not-available]: ./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-not-available.png
 


<!---HONumber=Aug16_HO4-->


