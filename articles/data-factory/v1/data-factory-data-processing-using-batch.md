---
title: "Zpracování rozsáhlých datových sad, které pomocí služby Data Factory a Batch | Microsoft Docs"
description: "Popisuje, jak zpracovávat obrovské objemy dat v kanál služby Azure Data Factory pomocí paralelní zpracování funkce Azure Batch."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 688b964b-51d0-4faa-91a7-26c7e3150868
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2017
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 2ceef65eaf195b605fada2f8dfe511fe33a5daa0
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="process-large-scale-datasets-using-data-factory-and-batch"></a>Zpracování rozsáhlých datových sad pomocí služeb Data Factory a Batch
> [!NOTE]
> Tento článek se týká verze 1 služby Data Factory, která je obecně dostupná (GA). Pokud používáte verze 2 služby Data Factory, který je ve verzi preview, najdete v části [vlastních aktivit v datové továrně verze 2](../transform-data-using-dotnet-custom-activity.md).

Tento článek popisuje architekturu ukázkové řešení, které přesune a zpracuje rozsáhlých datových sad automatické a naplánované způsobem. Je také začátku do konce návod k implementaci řešení pomocí Azure Data Factory a Azure Batch.

Tento článek je delší než náš typické článek, protože obsahuje návod celé ukázkové řešení. Pokud jste novým uživatelem Batch a služby Data Factory, můžete další informace o těchto službách a jak pracují společně. Pokud znáte něco o službách a jsou návrhu nebo architektury řešení, se může zaměřit pouze na [části architektura](#architecture-of-sample-solution) článku a pokud vyvíjíte prototypu nebo řešení, můžete také můžete vyzkoušet na podrobné pokyny v [návod](#implementation-of-sample-solution). Doporučujeme komentář k tomuto obsahu a jeho použití.

Nejprve podíváme, jak služby Data Factory a Batch vám mohou pomoci s zpracování rozsáhlých datových sad v cloudu.     

## <a name="why-azure-batch"></a>Proč Azure Batch?
Služba Azure Batch umožňuje efektivně spouštět rozsáhlé paralelní aplikace a aplikace vysokovýkonného výpočetního prostředí (HPC) v cloudu. Je to služba platformy, která plánuje spouštění výpočetně náročných úloh ve spravované kolekci virtuálních počítačů a která dokáže automaticky škálovat výpočetní prostředky tak, aby splňovaly potřeby vašich úloh.

Pomocí služby Batch definujete výpočetní prostředky, které vaše aplikace spustí paralelně a škálovaně. Můžete spouštět úlohy na vyžádání nebo naplánované úlohy a není nutné ručně vytvářet, konfigurovat a spravovat cluster prostředí HPC, jednotlivé virtuální počítače, virtuální sítě ani infrastrukturu komplexních úloh nebo plánování úkolů.

Pokud nejste obeznámeni s Azure Batch, protože pomáhá při pochopení, architekturu nebo implementace řešení popsaných v tomto článku najdete v následujících článcích.   

* [Základy služby Azure Batch](../../batch/batch-technical-overview.md)
* [Přehled funkcí Batch](../../batch/batch-api-basics.md)

(volitelné) Další informace o Azure Batch, najdete v článku [studijní Azure Batch](https://azure.microsoft.com/documentation/learning-paths/batch/).

## <a name="why-azure-data-factory"></a>Proč pro vytváření dat Azure?
Data Factory je cloudová služba pro integraci dat, která orchestruje a automatizuje přesouvání a transformaci dat. Pomocí služby Data Factory, můžete vytvořit spravované datové kanály, které přesun dat z místní a cloudové úložiště dat do úložiště dat centralizované (například: Azure Blob Storage) a proces nebo transformace dat pomocí služeb, jako je například Azure HDInsight a Azure Machine Learning. Můžete také naplánovat datových kanálů pro spouštění v naplánované způsobem (hodinové, denní, týdenní, atd.) a monitorování a spravovat na první pohled identifikovat problémy a provést akci.

Pokud nejste obeznámeni s Azure Data Factory, protože pomáhá při pochopení, architekturu nebo implementace řešení popsaných v tomto článku najdete v následujících článcích.  

* [Úvod objektu pro vytváření dat Azure](data-factory-introduction.md)
* [Sestavit svůj první kanál dat](data-factory-build-your-first-pipeline.md)   

(volitelné) Další informace o Azure Data Factory najdete v tématu [studijní postup Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/).

## <a name="data-factory-and-batch-together"></a>Objekt pro vytváření dat a společně Batch
Objekt pro vytváření dat obsahuje zabudované aktivity například aktivity kopírování zkopírovat nebo přesunout data ze zdrojového úložiště dat do cílového úložiště dat a Hive aktivity zpracování dat pomocí clusterů systému Hadoop (HDInsight) v Azure. V tématu [aktivit transformace dat](data-factory-data-transformation-activities.md) seznam aktivit transformace podporované.

Také vám umožňuje vytvářet vlastní aktivity .NET k přesunutí nebo zpracování dat s vlastní logikou a spusťte tyto aktivity v clusteru Azure HDInsight nebo u fondu Azure Batch virtuálních počítačů. Při použití služby Azure Batch, můžete nakonfigurovat k automatickému škálování fondu (Přidat nebo odebrat na základě příslušného zatížení virtuálních počítačů) podle vzorce zadáte.     

## <a name="architecture-of-sample-solution"></a>Architektura ukázkové řešení
I když architektury popsané v tomto článku je pro jednoduchým řešením, je relevantní pro komplexní scénáře, jako je riziko modelování finančních služeb, zpracování obrázků a vykreslování a Genomické analýzy.

Diagram znázorňuje 1) jak Data Factory orchestruje přesuny dat a zpracování a 2) jak Azure Batch zpracovává data paralelní způsobem. Stáhnout a vytisknout diagram referenční (11 × 17 palců. nebo velikost A3): [orchestration HPC a data pomocí Azure Batch a objektu pro vytváření dat](http://go.microsoft.com/fwlink/?LinkId=717686).

[![Diagram zpracování velkých dat](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)

Následující seznam uvádí základní kroky procesu. Řešení obsahuje kód a vysvětlení, k vytvoření řešení začátku do konce.

1. **Nakonfigurovat fond výpočetních uzlů (VM) Azure Batch**. Můžete zadat počet uzlů a velikost každého uzlu.
2. **Vytvoření instance služby Azure Data Factory** nakonfigurovaný s entitami, které představují Azure blob storage, výpočetní služby Azure Batch, vstupní a výstupní data a pracovní postup nebo kanálu s aktivitami, které přesunout a transformovat data.
3. **Vytvořit vlastní .NET aktivitu v kanálu pro vytváření dat**. Aktivita je váš kód uživatele, který běží ve fondu Azure Batch.
4. **Ukládat velké množství vstupních dat jako objekty BLOB v úložišti Azure**. Data je rozdělena do logické řezy (obvykle čas).
5. **Objekt pro vytváření dat zkopíruje data, která zpracovává paralelně** do sekundárního umístění.
6. **Objekt pro vytváření dat běží vlastní aktivity pomocí fondu přidělené dávkou**. Objekt pro vytváření dat mohou být aktivity současně. Každá aktivita zpracovává řez data. Výsledky jsou uloženy v úložišti Azure.
7. **Objekt pro vytváření dat přesune konečných výsledků do třetí umístění**, k distribuci prostřednictvím aplikace nebo pro další zpracování jiných nástrojů.

## <a name="implementation-of-sample-solution"></a>Implementace ukázkové řešení
Ukázkové řešení jsou záměrně jednoduchá a ukazují, jak používat společně objekt pro vytváření dat a Batch ke zpracování datové sady. Řešení jednoduše spočítá počet výskytů hledaný termín ("Microsoft") v vstupní soubory, které jsou uspořádány do časové řady. Vyprodukuje počet, který má výstupní soubory.

**Čas**: Pokud se seznámíte se základy používání služby Azure Data Factory a Batch a dokončili požadavky uvedené níže, jsme odhadnout toto řešení trvá 1 – 2 hodiny.

### <a name="prerequisites"></a>Požadavky
#### <a name="azure-subscription"></a>Předplatné Azure
Pokud nemáte předplatné Azure, můžete si během několika minut vytvořit Bezplatný zkušební účet. V tématu [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="azure-storage-account"></a>Účet služby Azure Storage
Používáte účet úložiště Azure pro ukládání dat v tomto kurzu. Pokud nemáte účet úložiště Azure, najdete v části [vytvořit účet úložiště](../../storage/common/storage-create-storage-account.md#create-a-storage-account). Ukázkové řešení používá úložiště objektů blob.

#### <a name="azure-batch-account"></a>Účet Azure Batch
Vytvoření účtu Azure Batch pomocí [portál Azure](http://portal.azure.com/). V tématu [vytvoření a Správa účtu Azure Batch](../../batch/batch-account-create-portal.md). Všimněte si klíč účet a název účtu Azure Batch. Můžete také použít [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) rutiny k vytvoření účtu Azure Batch. V tématu [Začínáme pomocí rutin prostředí PowerShell Azure Batch](../../batch/batch-powershell-cmdlets-get-started.md) pro podrobné pokyny k použití této rutiny.

Ukázkové řešení používá Azure Batch (nepřímo přes kanál služby Azure Data Factory) pro zpracování dat paralelní způsobem ve fondu výpočetních uzlů (spravované kolekce virtuálních počítačů).

#### <a name="azure-batch-pool-of-virtual-machines-vms"></a>Azure Batch fondu virtuálních počítačů (VM)
Vytvoření **fondu Azure Batch** s minimálně 2 výpočetních uzlů.

1. V [portál Azure](https://portal.azure.com), klikněte na tlačítko **Procházet** v levé nabídce a klikněte na **účty Batch**.
2. Vyberte svůj účet Azure Batch **účtu Batch** okno.
3. Klikněte na tlačítko **fondy** dlaždici.
4. V **fondy** okně klikněte na tlačítko Přidat na panelu nástrojů na Přidat fond.
   1. Zadejte ID fondu (**ID fondu**). Poznámka: **ID fondu**; je nutné při vytváření řešení Data Factory.
   2. Zadejte **Windows Server 2012 R2** pro nastavení řada operačního systému.
   3. Vyberte **cenovou úroveň uzlu**.
   4. Zadejte **2** jako hodnota **cíl vyhrazené** nastavení.
   5. Zadejte **2** jako hodnota **maximální počet úloh na uzel** nastavení.
   6. Kliknutím na tlačítko **OK** vytvořte fond.

#### <a name="azure-storage-explorer"></a>Azure Storage Explorer
[Azure Storage Explorer 6 (nástroj)](https://azurestorageexplorer.codeplex.com/) nebo [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (z ClumsyLeaf softwaru). Pomocí těchto nástrojů pro kontroly a změna dat v Azure Storage projekty, včetně protokolů aplikací hostovaných v cloudu.

1. Vytvořit kontejner s názvem **můj_kontejner** s přístupem k privátní (žádný anonymní přístup)
2. Pokud používáte **CloudXplorer**, vytvořit složky a podsložky s následující strukturou:

   ![](./media/data-factory-data-processing-using-batch/image3.png)

   `Inputfolder`a `outputfolder` jsou nejvyšší úrovně složky v `mycontainer`. `inputfolder` Má podsložky razítka data a času (rrrr-MM-DD-HH).

   Pokud používáte **Azure Storage Explorer**, v dalším kroku, budete muset odeslat soubory s názvy: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt` a tak dále. Tento krok automaticky vytvoří složky.
3. Vytvořte textový soubor **soubor.txt** na počítači s obsahem, který obsahuje klíčové slovo **Microsoft**. Například: "testovací vlastní aktivity Microsoft test vlastní aktivity Microsoft".
4. Nahrajte soubor do následující složky vstupní v Azure blob storage.

   ![](./media/data-factory-data-processing-using-batch/image4.png)

   Pokud používáte **Azure Storage Explorer**, nahrajte soubor **soubor.txt** k **můj_kontejner**. Klikněte na tlačítko **kopie** na panelu nástrojů vytvořit kopii objektu blob. V **kopírovat objekt Blob** dialogové okno, změny **název cílového objektu blob** k `inputfolder/2015-11-16-00/file.txt`. Opakujte tento krok k vytvoření `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt` a tak dále. Tato akce automaticky vytvoří složky.
5. Vytvořte jiný kontejner s názvem: `customactivitycontainer`. Můžete nahrát soubor zip vlastní aktivitu do tohoto kontejneru.

#### <a name="visual-studio"></a>Visual Studio
Nainstalujte Microsoft Visual Studio 2012 nebo novějším k vytvoření vlastních dávkové aktivity pro použití v řešení Data Factory.

### <a name="high-level-steps-to-create-the-solution"></a>Základní kroky pro vytvoření řešení
1. Vytvořte vlastní aktivity, která obsahuje logiku zpracování dat.
2. Vytvoření služby Azure data factory, která používá vlastní aktivity:

### <a name="create-the-custom-activity"></a>Vytvořit vlastní aktivitu
Vlastní aktivita služby Data Factory je jádrem této ukázkové řešení. Ukázkové řešení používá Azure Batch pro spuštění vlastní aktivity. V tématu [použít vlastní aktivity v kanálu Azure Data Factory](data-factory-use-custom-activities.md) pro základní informace o vývoji vlastních aktivit a použít je v kanálů služby Azure Data Factory.

Chcete-li vytvořit vlastní aktivity rozhraní .NET, který můžete použít v kanál služby Azure Data Factory, je potřeba vytvořit **knihovna tříd rozhraní .NET** projektu s třídou, která implementuje **IDotNetActivity** rozhraní. Toto rozhraní obsahuje pouze jednu metodu: **Execute**. Tady je podpis metody:

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

Metoda má několik klíčové komponenty, které je třeba pochopit.

* Tato metoda přebírá čtyř parametrů:

  1. **linkedServices**. Vyčíslitelná seznam propojené služby, které odkazují vstupní a výstupní datové zdroje (například: Azure Blob Storage) k objektu pro vytváření dat. V této ukázce je pouze jeden propojené služby typu úložiště Azure pro vstup a výstup.
  2. **datové sady**. Toto je vyčíslitelná seznam datových sad. Tento parametr můžete získat umístění a schémata definované vstupní a výstupní datové sady.
  3. **aktivita**. Tento parametr představuje aktuální výpočetní entitu – v takovém případě služby Azure Batch.
  4. **protokolovač**. Protokolovač umožňuje psát komentáře ladění tento prostor jako protokol "User" pro kanál.
* Metoda vrátí slovník, který slouží k řetězu vlastní aktivity společně v budoucnu. Tato funkce není dosud implementována, takže vrátí prázdný slovník z metody.

#### <a name="procedure-create-the-custom-activity"></a>Postup: Vytvoření vlastní aktivity
1. Vytvoření projektu knihovny tříd rozhraní .NET v sadě Visual Studio.

   1. Spusťte **sady Visual Studio 2012**/**2013 nebo 2015**.
   2. Klikněte na **Soubor**, přejděte na **Nový** a klikněte na **Projekt**.
   3. Rozbalte položku **šablony**a vyberte **Visual C\#**. V tomto návodu můžete použít C\#, ale můžete použít kterémkoli jazyce platformy .NET pro vývoj vlastní aktivity.
   4. Vyberte **knihovny tříd** ze seznamu typů projektu na pravé straně.
   5. Zadejte **MyDotNetActivity** pro **název**.
   6. Vyberte **C:\\ADF** pro **umístění**. Vytvořit složku **ADF** Pokud neexistuje.
   7. Kliknutím na tlačítko **OK** vytvořte projekt.
2. Klikněte na **Nástroje**, přejděte na **Správce balíčků NuGet** a klikněte na **Konzola Správce balíčků**.
3. V **Konzola správce balíčků**, spusťte následující příkaz pro import **Microsoft.Azure.Management.DataFactories**.

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. Import **Azure Storage** balíčku NuGet do projektu. Tento balíček je nutné, protože můžete používat úložiště objektů Blob rozhraní API v této ukázce.

    ```powershell
    Install-Package Azure.Storage
    ```
5. Přidejte následující **pomocí** direktivy ke zdrojovému souboru v projektu.

    ```csharp
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;
    
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. Změňte název **obor názvů** k **MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. Změnit název třídy pro **MyDotNetActivity** a odvodí z **IDotNetActivity** rozhraní, jak je uvedeno níže.

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. Implementace (Přidat) **Execute** metodu **IDotNetActivity** rozhraní k **MyDotNetActivity** třídy a zkopírujte následující vzorový kód do metody. Najdete v článku [spustit metodu](#execute-method) části vysvětlení pro logikou používanou v této metodě.

    ```csharp
    /// <summary>
    /// Execute method is the only method of IDotNetActivity interface you must implement.
    /// In this sample, the method invokes the Calculate method to perform the core logic.  
    /// </summary>
    public IDictionary<string, string> Execute(
       IEnumerable<LinkedService> linkedServices,
       IEnumerable<Dataset> datasets,
       Activity activity,
       IActivityLogger logger)
    {
    
       // declare types for input and output data stores
       AzureStorageLinkedService inputLinkedService;
    
       Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
       foreach (LinkedService ls in linkedServices)
           logger.Write("linkedService.Name {0}", ls.Name);
    
       // using First method instead of Single since we are using the same
       // Azure Storage linked service for input and output.
       inputLinkedService = linkedServices.First(
           linkedService =>
           linkedService.Name ==
           inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
           as AzureStorageLinkedService;
    
       string connectionString = inputLinkedService.ConnectionString; // To create an input storage client.
       string folderPath = GetFolderPath(inputDataset);
       string output = string.Empty; // for use later.
    
       // create storage client for input. Pass the connection string.
       CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
       CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();
    
       // initialize the continuation token before using it in the do-while loop.
       BlobContinuationToken continuationToken = null;
       do
       {   // get the list of input blobs from the input storage client object.
           BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                    true,
                                    BlobListingDetails.Metadata,
                                    null,
                                    continuationToken,
                                    null,
                                    null);
    
           // Calculate method returns the number of occurrences of
           // the search term (“Microsoft”) in each blob associated
           // with the data slice.
           //
           // definition of the method is shown in the next step.
           output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
       } while (continuationToken != null);
    
       // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
       Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    
       folderPath = GetFolderPath(outputDataset);
    
       logger.Write("Writing blob to the folder: {0}", folderPath);
    
       // create a storage object for the output blob.
       CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
       // write the name of the file.
       Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
       logger.Write("output blob URI: {0}", outputBlobUri.ToString());
       // create a blob and upload the output text.
       CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
       logger.Write("Writing {0} to the output blob", output);
       outputBlob.UploadText(output);
    
       // The dictionary can be used to chain custom activities together in the future.
       // This feature is not implemented yet, so just return an empty dictionary.
       return new Dictionary<string, string>();
    }
    ```
9. Přidejte následující pomocné metody pro třídu. Tyto metody jsou vyvolány **Execute** metoda. Co je nejdůležitější **Calculate** metoda izoluje kód, který iteruje v rámci jednotlivých objektů blob.

    ```csharp
    /// <summary>
    /// Gets the folderPath value from the input/output dataset.
    /// </summary>
    private static string GetFolderPath(Dataset dataArtifact)
    {
       if (dataArtifact == null || dataArtifact.Properties == null)
       {
           return null;
       }
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
       return blobDataset.FolderPath;
    }
    
    /// <summary>
    /// Gets the fileName value from the input/output dataset.
    /// </summary>
    
    private static string GetFileName(Dataset dataArtifact)
    {
       if (dataArtifact == null || dataArtifact.Properties == null)
       {
           return null;
       }
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
       return blobDataset.FileName;
    }
    
    /// <summary>
    /// Iterates through each blob (file) in the folder, counts the number of instances of search term in the file,
    /// and prepares the output text that is written to the output blob.
    /// </summary>
    
    public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
    {
       string output = string.Empty;
       logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
       foreach (IListBlobItem listBlobItem in Bresult.Results)
       {
           CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
           if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
           {
               string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
               logger.Write("input blob text: {0}", blobText);
               string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
               var matchQuery = from word in source
                                where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                select word;
               int wordCount = matchQuery.Count();
               output += string.Format("{0} occurrences(s) of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
           }
       }
       return output;
    }
    ```
    **GetFolderPath** metoda vrací cestu ke složce, která odkazuje datovou sadu na a **GetFileName** metoda vrátí název objektu blob nebo soubor, který datová sada odkazuje na.

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    **Calculate** metoda vypočítá počet instancí – klíčové slovo **Microsoft** ve vstupní soubory (objekty BLOB ve složce). Hledaný termín ("Microsoft") je pevně zakódovaná v kódu.

1. Kompilace projektu. Klikněte na tlačítko **sestavení** z nabídky a klikněte na tlačítko **sestavit řešení**.
2. Spusťte **Průzkumníka Windows**a přejděte do **bin\\ladění** nebo **bin\\verze** složku v závislosti na typu sestavení.
3. Vytvořte soubor zip **MyDotNetActivity.zip** obsahující všechny binární soubory v  **\\bin\\ladění** složky. Můžete zahrnout MyDotNetActivity. **pdb** tak, aby získat další podrobnosti, jako je například číslo řádku ve zdrojovém kódu, která způsobila problém, když dojde k chybě.

   ![](./media/data-factory-data-processing-using-batch/image5.png)
4. Nahrát **MyDotNetActivity.zip** jako objekt blob do kontejneru objektů blob: `customactivitycontainer` ve službě Azure blob storage, **StorageLinkedService** propojená služba v  **ADFTutorialDataFactory** používá. Vytvoření kontejneru objektů blob `customactivitycontainer` Pokud ještě neexistuje.

#### <a name="execute-method"></a>Execute – metoda
Tato část obsahuje další podrobnosti a poznámky o kód v Metoda Execute.

1. Členy pro iterace v rámci vstupní kolekce jsou součástí [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) oboru názvů. Iterace v rámci kolekce objektů blob vyžaduje použití **BlobContinuationToken** třídy. V podstatě, je nutné použít DNT-při smyčky pomocí tokenu jako mechanismus pro ukončení opakování. Další informace najdete v tématu [postup používání úložiště Blob z .NET](../../storage/blobs/storage-dotnet-how-to-use-blobs.md). Zobrazí se zde základní smyčka:

    ```csharp
    // Initialize the continuation token.
    BlobContinuationToken continuationToken = null;
    do
    {
    // Get the list of input blobs from the input storage client object.
    BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
    
                         true,
                                   BlobListingDetails.Metadata,
                                   null,
                                   continuationToken,
                                   null,
                                   null);
    // Return a string derived from parsing each blob.
    
     output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
    } while (continuationToken != null);

    ```
   Najdete v dokumentaci k [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) metoda podrobnosti.
2. Kód pro práci prostřednictvím sady objektů BLOB logicky přejde v rámci aplikace-při smyčky. V **Execute** metoda, do-při smyčky předá seznam objektů BLOB metodu s názvem **Calculate**. Metoda vrátí řetězec proměnné s názvem **výstup** tedy výsledek s vstupní prostřednictvím všech objektů BLOB v segmentu.

   Vrátí počet výskytů hledaný termín (**Microsoft**) v objektu blob předaný **Calculate** metoda.

    ```csharp
    output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
3. Jednou **Calculate** Metoda dokončení práce, musí být zapsané do nového objektu blob. Aby pro každou sadu objektů BLOB zpracovat nový objekt blob může být napsán s výsledky. Zápis do nového objektu blob, nejdřív najdete výstupní datovou sadu.

    ```csharp
    // Get the output dataset using the name of the dataset matched to a name in the Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
4. Kód také volá metodu helper: **GetFolderPath** načíst cesta ke složce (název kontejneru úložiště).

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   **GetFolderPath** vrhá objekt datové sady, který má AzureBlobDataSet, který má vlastnost s názvem FolderPath.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FolderPath;
    ```
5. Volání kódu **GetFileName** metoda pro načtení názvu souboru (název objektu blob). Kód je podobná výše uvedený kód slouží k získání cesty ke složce.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FileName;
    ```
6. Název souboru je zapsán vytvořením objektu URI. Konstruktor identifikátoru URI používá **BlobEndpoint** vlastnost vrátit název kontejneru. Název složky a cesta k souboru se přidají k vytvoření identifikátor URI objektu blob výstup.  

    ```csharp
    // Write the name of the file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
7. Název souboru byla zapsána a nyní můžete vytvořit řetězec výstup z **Calculate** metoda do nového objektu blob:

    ```csharp
    // Create a blob and upload the output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} to the output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-the-data-factory"></a>Vytvoření objektu pro vytváření dat
V [vytvořit vlastní aktivitu](#create-the-custom-activity) části vytvořen vlastní aktivity a odesláno soubor zip binární soubory služby a souboru PDB na kontejner objektů blob v Azure. V této části vytvoříte Azure **objekt pro vytváření dat** s **kanálu** používající **vlastní aktivity**.

Vstupní datové sady pro vlastní aktivita představuje objekty BLOB (soubory) ve vstupní složky (`mycontainer\\inputfolder`) v úložišti objektů blob. Představuje výstupní datovou sadu aktivity výstup objektů BLOB v zadané výstupní složce (`mycontainer\\outputfolder`) v úložišti objektů blob.

Vyřaďte jeden nebo více souborů ve složkách vstupní:

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

Například vyřaďte jeden soubor (soubor.txt) s následujícím obsahem do každé ze složky.

```
test custom activity Microsoft test custom activity Microsoft
```

Každé vstupní složky odpovídá řez v Azure Data Factory i v případě, že složka má 2 nebo více souborů. Při zpracování každý řez v kanálu vlastní aktivita iteruje všechny objekty BLOB ve složce vstupní pro tento řez.

Uvidíte pět výstupní soubory se stejným obsahem. Například výstupní soubor z zpracování souboru ve složce 2015 11 16 00 má následující obsah:

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
```

Pokud je do vstupní složky vyřadit více souborů (soubor.txt, Soubor2.txt, file3.txt) se stejným obsahem, zobrazí se následující obsah ve výstupním souboru. Každé složky (2015-11-16-00 atd.) odpovídá řez v této ukázce to i v případě, že složka má více vstupních souborů.

```csharp
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file3.txt.
```

Výstupní soubor má tři řádky, jeden pro každý vstupní soubor (binární rozsáhlý objekt) ve složce přidružené řezu (2015-11-16-00).

Úloha se vytvoří pro každou aktivitu spustit. V této ukázce je jenom jedna aktivita v kanálu. Při zpracování řezu v kanálu vlastní aktivita běží na Azure Batch ke zpracování řezu. Vzhledem k tomu, že existují pět řezy (každý řez může mít více objektů BLOB nebo soubor), nejsou pět úkoly vytvořené v Azure Batch. Když úloha běží na Batch, je ve skutečnosti vlastní aktivity, která je spuštěná.

Následující názorný postup obsahuje další podrobnosti.

#### <a name="step-1-create-the-data-factory"></a>Krok 1: Vytvoření objektu pro vytváření dat
1. Po přihlášení na [portál Azure](https://portal.azure.com/), proveďte následující kroky:

   1. V nabídce vlevo klikněte na **NOVÝ**.
   2. Klikněte na tlačítko **Data + analýzy** v **nový** okno.
   3. V okně **Analýza dat** klikněte na **Objekt pro vytváření dat**.
2. V **nový objekt pro vytváření dat** okno, zadejte **CustomActivityFactory** pro název. Název objektu pro vytváření dat Azure musí být globálně jedinečný. Pokud se zobrazí chyba: **název objektu pro vytváření dat "CustomActivityFactory" není k dispozici**, změňte název objektu pro vytváření dat (například **yournameCustomActivityFactory**) a zkuste to znovu.
3. Klikněte na tlačítko **název skupiny prostředků**a vyberte existující skupinu prostředků nebo vytvořte skupinu prostředků.
4. Ověřte, že používáte správné předplatné a oblasti, kde chcete objekt pro vytváření dat vytvořit.
5. V okně **Nový objekt pro vytváření dat** klikněte na **Vytvořit**.
6. Zobrazí objektu pro vytváření dat vytváří ve **řídicí panel** na portálu Azure.
7. Po úspěšném vytvoření objektu pro vytváření dat se zobrazí stránka s obsahem objektu pro vytváření dat.

   ![](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a>Krok 2: Vytvoření propojených služeb
Propojené služby propojují úložiště dat nebo výpočetní služby s objektem pro vytváření dat Azure. V tomto kroku propojíte vaše **Azure Storage** účtu a **Azure Batch** účet do data factory.

#### <a name="create-azure-storage-linked-service"></a>Vytvoření propojené služby Azure Storage
1. Klikněte na tlačítko **vytvořit a nasadit** na dlaždici **objekt pro vytváření dat** okno pro **CustomActivityFactory**. Zobrazí editoru služby Data Factory.
2. Klikněte na tlačítko **nové datové úložiště** na příkaz panelu a vyberte **úložiště Azure.** V editoru by se měl zobrazit skript JSON pro vytvoření propojené služby Azure Storage.

   ![](./media/data-factory-data-processing-using-batch/image7.png)

3. Nahraďte **název účtu** názvem účtu služby Azure Storage a **klíč účtu** přístupovým klíčem k účtu Azure Storage. Informace o tom, jak získat přístupový klíč k úložišti, najdete v článku o [zobrazení, kopírování a opětovném vytváření přístupových klíčů úložiště](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).

4. Propojenou službu nasadíte kliknutím na **Nasadit** na panelu příkazů.

   ![](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-azure-batch-linked-service"></a>Vytvoření služby Azure Batch propojené
V tomto kroku vytvoříte propojené služby pro vaše **Azure Batch** účet, který se používá ke spuštění aktivity vlastní Data Factory.

1. Klikněte na tlačítko **nový výpočet** na příkaz panelu a vyberte **Azure Batch.** Měli byste vidět skript JSON pro vytvoření služby Azure Batch propojené v editoru.
2. Ve skriptu JSON:

   1. Nahraďte **název účtu** s názvem vašeho účtu Azure Batch.
   2. Nahraďte **přístupový klíč** přístupovým klíčem k účtu Azure Batch.
   3. Zadejte ID fondu **poolName** vlastnost**.** Pro tuto vlastnost lze zadat buď název fondu nebo fondu ID.
   4. Zadejte batch identifikátor URI pro **batchUri** vlastnost JSON.

      > [!IMPORTANT]
      > **URL** z **okně účtu Azure Batch** je v následujícím formátu: \<accountname\>.\< oblast\>. batch.azure.com. Pro **batchUri** vlastností v kódu JSON, budete muset **odebrat "název účtu."** z adresy URL. Příklad: `"batchUri": "https://eastus.batch.azure.com"`.
      >
      >

      ![](./media/data-factory-data-processing-using-batch/image9.png)

      Pro **poolName** vlastnost, můžete také zadat ID fondu namísto názvu fondu.

      > [!NOTE]
      > Služba Data Factory nepodporuje možnost na vyžádání pro Azure Batch, stejně jako pro HDInsight. Vlastní fondu Azure Batch můžete použít pouze v objektu pro vytváření dat Azure.
      >
      >
   5. Zadejte **StorageLinkedService** pro **linkedServiceName** vlastnost. Tuto propojenou službu jste vytvořili v předchozím kroku. Toto úložiště se používá jako pracovní oblast pro soubory a protokoly.
3. Propojenou službu nasadíte kliknutím na **Nasadit** na panelu příkazů.

#### <a name="step-3-create-datasets"></a>Krok 3: Vytvoření datové sady
V tomto kroku vytvoříte datové sady, které představují vstupní a výstupní data.

#### <a name="create-input-dataset"></a>Vytvoření vstupní datové sady
1. V **Editor** služby Data Factory klikněte na tlačítko **nová datová sada** na panelu nástrojů a klikněte na tlačítko **úložiště objektů Azure Blob** z rozevírací nabídky.
2. Nahraďte kód JSON v pravém podokně následujícím fragmentem kódu JSON:

    ```json
    {
       "name": "InputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
               "format": {
                   "type": "TextFormat"
               },
               "partitionedBy": [
                   {
                       "name": "Year",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "yyyy"
                       }
                   },
                   {
                       "name": "Month",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "MM"
                       }
                   },
                   {
                       "name": "Day",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "dd"
                       }
                   },
                   {
                       "name": "Hour",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "HH"
                       }
                   }
               ]
           },
           "availability": {
               "frequency": "Hour",
               "interval": 1
           },
           "external": true,
           "policy": {}
       }
    }
    ```

    Vytvoření kanálu dále v tomto návodu se časem spuštění: 2015-11-16T00:00:00Z a koncový čas: 2015-11-16T05:00:00Z. Je naplánována nevytvořila data **každou hodinu**, takže se 5 řezy vstupní a výstupní (mezi **00**: 00:00 -\> **05**: 00:00).

    **Frekvence** a **interval** vstupní datové sady je nastavený na **hodinu** a **1**, což znamená, že vstupní řez je k dispozici každou hodinu.

    Tady jsou časy zahájení pro každý řez, která je reprezentována **SliceStart** systémové proměnné ve výše uvedeném fragmentu JSON.

    | **Řez** | **Čas spuštění**          |
    |-----------|-------------------------|
    | 1         | 2015. 11 16T**00**: 00:00 |
    | 2         | 2015. 11 16T**01**: 00:00 |
    | 3         | 2015. 11 16T**02**: 00:00 |
    | 4         | 2015. 11 16T**03**: 00:00 |
    | 5         | 2015. 11 16T**04**: 00:00 |

    **FolderPath** se vypočítává pomocí rok, měsíc, den a hodina součástí řez čas spuštění (**SliceStart**). Zde je proto jak vstupní složky je namapovaný na řez.

    | **Řez** | **Čas spuštění**          | **Vstupní složky**  |
    |-----------|-------------------------|-------------------|
    | 1         | 2015. 11 16T**00**: 00:00 | 2015-11-16-**00** |
    | 2         | 2015. 11 16T**01**: 00:00 | 2015-11-16-**01** |
    | 3         | 2015. 11 16T**02**: 00:00 | 2015-11-16-**02** |
    | 4         | 2015. 11 16T**03**: 00:00 | 2015-11-16-**03** |
    | 5         | 2015. 11 16T**04**: 00:00 | 2015-11-16-**04** |

1. Klikněte na tlačítko **nasadit** na panelu nástrojů vytvořit a nasadit **InputDataset** tabulky.

#### <a name="create-output-dataset"></a>Vytvoření výstupní datové sady
V tomto kroku vytvoříte jinou datovou sadu typu AzureBlob, která bude představovat výstupní data.

1. V **Editor** služby Data Factory klikněte na tlačítko **nová datová sada** na panelu nástrojů a klikněte na tlačítko **úložiště objektů Azure Blob** z rozevírací nabídky.
2. Nahraďte kód JSON v pravém podokně následujícím fragmentem kódu JSON:

    ```json
    {
       "name": "OutputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "fileName": "{slice}.txt",
               "folderPath": "mycontainer/outputfolder",
               "partitionedBy": [
                   {
                       "name": "slice",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "yyyy-MM-dd-HH"
                       }
                   }
               ]
           },
           "availability": {
               "frequency": "Hour",
               "interval": 1
           }
       }
    }
    ```

    Objekt blob nebo soubor výstupu se generuje pro každý vstupní řez. Zde je, jak je výstupní soubor s názvem pro každý řez. Výstupní soubory jsou generovány v jednu výstupní složky: `mycontainer\\outputfolder`.

    | **Řez** | **Čas spuštění**          | **Výstupní soubor**       |
    |-----------|-------------------------|-----------------------|
    | 1         | 2015. 11 16T**00**: 00:00 | 2015-11-16 -**00. txt** |
    | 2         | 2015. 11 16T**01**: 00:00 | 2015-11-16 -**01. txt** |
    | 3         | 2015. 11 16T**02**: 00:00 | 2015-11-16 -**02. txt** |
    | 4         | 2015. 11 16T**03**: 00:00 | 2015-11-16 -**03. txt** |
    | 5         | 2015. 11 16T**04**: 00:00 | 2015-11-16 -**04. txt** |

    Nezapomeňte, že všechny soubory ve vstupní složky (například: 2015-11-16-00) jsou součástí řez se časem spuštění: 2015-11-16-00. Při zpracování této řezu se vlastní aktivita prohledává každý soubor a vytvoří řádek ve výstupním souboru s počtem výskytů hledaný termín ("Microsoft"). Pokud existují tři soubory ve složce 2015 11 16 00, se ve výstupním souboru tři řádky: 2015-11-16-00.txt.

1. Klikněte na tlačítko **nasadit** na panelu nástrojů vytvořit a nasadit **OutputDataset**.

#### <a name="step-4-create-and-run-the-pipeline-with-custom-activity"></a>Krok 4: Vytvoření a spuštění kanálu s aktivitou vlastní
V tomto kroku vytvoříte kanál s aktivitou jeden, vlastní aktivitu, kterou jste vytvořili dříve.

> [!IMPORTANT]
> Pokud ještě jste neodeslali **soubor.txt** jako vstup složek v kontejneru objektů blob, tak učinit před vytvořením kanálu. **IsPaused** vlastnost nastavena na hodnotu false v kanálu formát JSON, takže se kanál okamžitě spustí jako **spustit** je datum v minulosti.
>
>

1. V editoru služby Data Factory, klikněte na tlačítko **nový kanál** na panelu příkazů. Pokud se příkaz nezobrazí, klikněte na tlačítko **... (Tři tečky)**  k jeho zobrazení.
2. Nahraďte kód JSON v pravém podokně se následující skript JSON:

    ```json
    {
       "name": "PipelineCustom",
       "properties": {
           "description": "Use custom activity",
           "activities": [
               {
                   "type": "DotNetActivity",
                   "typeProperties": {
                       "assemblyName": "MyDotNetActivity.dll",
                       "entryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                       "packageLinkedService": "AzureStorageLinkedService",
                       "packageFile": "customactivitycontainer/MyDotNetActivity.zip"
                   },
                   "inputs": [
                       {
                           "name": "InputDataset"
                       }
                   ],
                   "outputs": [
                       {
                           "name": "OutputDataset"
                       }
                   ],
                   "policy": {
                       "timeout": "00:30:00",
                       "concurrency": 5,
                       "retry": 3
                   },
                   "scheduler": {
                       "frequency": "Hour",
                       "interval": 1
                   },
                   "name": "MyDotNetActivity",
                   "linkedServiceName": "AzureBatchLinkedService"
               }
           ],
           "start": "2015-11-16T00:00:00Z",
           "end": "2015-11-16T05:00:00Z",
           "isPaused": false
      }
    }
    ```
   Je třeba počítat s následujícím:

   * Je jenom jedna aktivita v kanálu a který je typu: **DotNetActivity**.
   * **AssemblyName** nastavena na název knihovny DLL: **MyDotNetActivity.dll**.
   * **EntryPoint** je nastaven na **MyDotNetActivityNS.MyDotNetActivity**. Je v podstatě \<obor názvů\>.\< Název třídy\> ve vašem kódu.
   * **PackageLinkedService** je nastaven na **StorageLinkedService** který odkazuje na úložiště objektů blob, který obsahuje soubor zip vlastní aktivity. Pokud používáte jiné účty Azure Storage pro vstupní a výstupní soubory a soubor zip vlastní aktivity, musíte vytvořit jiný propojenou službu úložiště Azure. Tento článek předpokládá, že používáte stejný účet úložiště Azure.
   * **PackageFile** je nastaven na **customactivitycontainer/MyDotNetActivity.zip**. Je ve formátu: \<containerforthezip\>/\<nameofthezip.zip\>.
   * Vlastní aktivita přijímá **InputDataset** jako vstup a **OutputDataset** jako výstup.
   * **LinkedServiceName** vlastnost vlastní aktivity odkazuje na **AzureBatchLinkedService**, která říká službě Azure Data Factory, který vlastní aktivita je potřeba spustit v Azure Batch.
   * **Souběžnosti** nastavení je důležité. Pokud použijete výchozí hodnotu, která je 1, i v případě, že máte 2 nebo více výpočetních uzlů ve fondu Azure Batch, řezy jsou zpracovávány jeden po druhém. Proto nejsou využívat výhod paralelní zpracování funkce Azure Batch. Pokud nastavíte **souběžnosti** na vyšší hodnotu, například 2, znamená to, že dva řezy (odpovídá dvě úlohy v Azure Batch) může být zpracována ve stejnou dobu, v takovém případě oba virtuální počítače v Azure Batch jsou využité fondu. Proto správně nastavte vlastnost souběžnosti.
   * Pouze jednu úlohu (řez) je spustit na virtuálním počítači v libovolném bodě ve výchozím nastavení. Důvodem je, že ve výchozím nastavení, **maximální počet úloh na virtuální počítač** je nastaven na hodnotu 1 pro fondu Azure Batch. V rámci požadavků vytvořit fond se tato vlastnost nastavena na hodnotu 2, takže dva řezy objekt pro vytváření dat může být spuštěná na virtuálním počítači ve stejnou dobu.

    -   **isPaused** je nastavena na hodnotu false ve výchozím nastavení. Kanál se spustí okamžitě v tomto příkladu protože řezy spustit v minulosti. Tuto vlastnost lze nastavit na hodnotu true pozastavení kanálu a nastavte ji zpět na hodnotu false restartovat.

    -   **Spustit** čas a **end** časy jsou od sebe pět hodin a řezy vytváří každou hodinu, takže pět řezy vytváří v kanálu.

1. Kanál nasadíte kliknutím na **Nasadit** na panelu příkazů.

#### <a name="step-5-test-the-pipeline"></a>Krok 5: Testování kanálu
V tomto kroku otestovat kanálu přetažením souborů do vstupní složky. Začněme testování kanálu pomocí jeden soubor pro jeden vstupní složky.

1. V okně Data Factory na webu Azure portal, klikněte na tlačítko **Diagram**.

   ![](./media/data-factory-data-processing-using-batch/image10.png)
2. V zobrazení diagramu, klikněte dvakrát na vstupní datové sady: **InputDataset**.

   ![](./media/data-factory-data-processing-using-batch/image11.png)
3. Měli byste vidět **InputDataset** okno s pěti všechny řezy připraven. Upozornění **čas spuštění ŘEZU** a **ŘEZ KONCOVÝ čas** pro každý řez.

   ![](./media/data-factory-data-processing-using-batch/image12.png)
4. V **zobrazení diagramu**, klikněte na tlačítko **OutputDataset**.
5. Měli byste vidět, že pět Výstup řezy jsou ve stavu Připraveno, pokud již byl vytvořen.

   ![](./media/data-factory-data-processing-using-batch/image13.png)
6. Použití portálu Azure k zobrazení **úlohy** přidružené **řezy** a zjistit, jaké virtuálních počítačů, každý řez byla spuštěna na. V tématu [integraci služby Data Factory a Batch](#data-factory-and-batch-integration) podrobnosti.
7. Měli byste vidět výstupní soubory v `outputfolder` z `mycontainer` ve vaší službě Azure blob storage.

   ![](./media/data-factory-data-processing-using-batch/image15.png)

   Měli byste vidět pět výstupní soubory, jeden pro každý vstupní řez. Každý výstupního souboru, musí mít obsah podobná následující výstup:

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
    ```
   Následující diagram znázorňuje, jak řezy Data Factory mapování na úkoly ve službě Azure Batch. V tomto příkladu má řez spustit pouze jeden.

   ![](./media/data-factory-data-processing-using-batch/image16.png)
8. Nyní zkuste to prosím ještě s více soubory ve složce. Vytvoření souborů: **Soubor2.txt**, **file3.txt**, **file4.txt**, a **file5.txt** se stejným obsahem jako soubor.txt ve složce:  **2015-11-06-01**.
9. V zadané výstupní složce **odstranit** výstupního souboru: **2015 11 16 01.txt**.
10. Nyní v **OutputDataset** okno, klikněte pravým tlačítkem na řez s **čas spuštění ŘEZU** nastavena na **11/16/2015 01:00:00 AM**a klikněte na tlačítko **spustit** na znovu spustit nebo zpětný-process řez. Řez teď má pět souborů místo jeden soubor.

    ![](./media/data-factory-data-processing-using-batch/image17.png)
11. Po spuštění řezu a její stav je **připraven**, ověřte obsah ve výstupním souboru pro tento řez (**2015 11 16 01.txt**) v `outputfolder` z `mycontainer` ve službě blob storage. Měla by existovat řádek pro každý soubor řezu.

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> Pokud výstupní soubor 2015-11-16-01.txt se neodstranila než to zkusíte s pěti vstupní soubory, zobrazí jeden řádek z předchozího spuštění řezu a pěti řádcích z aktuální spustit řez. Ve výchozím nastavení je obsah připojena do výstupního souboru, pokud již existuje.
>
>

#### <a name="data-factory-and-batch-integration"></a>Integrace služby Data Factory a Batch
Služba Data Factory vytvoří úlohu ve službě Azure Batch s názvem: `adf-poolname:job-xxx`.

![Azure Data Factory - úlohy Batch](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

Úloha v dané úloze se vytvoří při každém spuštění aktivity řezu. Pokud je připravena k provedení 10 řezů, vytvoření 10 úkolů do úlohy. Můžete mít více než jeden řez spouštět současně, pokud máte několika výpočetních uzlech ve fondu. Pokud > 1 je nastavena maximální úkolů na výpočetním uzlu, může být více než jeden řez na stejném výpočetním spuštěna.

V tomto příkladu jsou pět řezů, takže pět úlohy v Azure Batch. Pomocí **souběžnosti** nastavena na **5** v kanálu JSON v Azure Data Factory a **maximální počet úloh na virtuální počítač** nastavena na **2** ve fondu Azure Batch s **2** virtuálních počítačů, Rychlé úlohy běží (zkontrolujte, zda počáteční a koncový čas pro úlohy).

Použití portálu k zobrazení dávkové úlohy a její úkoly, které jsou přidružené **řezy** a zjistit, jaké virtuálního počítače byla spuštěna na každý řez.

![Azure Data Factory - úlohy Batch](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-the-pipeline"></a>Ladění kanálu
Ladění zahrnuje několik základních technik:

1. Pokud vstupní řez není nastaven na **připraven**, potvrďte, že vstupní složky struktura je správná a soubor.txt existuje ve vstupní složkách.

   ![](./media/data-factory-data-processing-using-batch/image3.png)
2. V **Execute** metoda vlastní aktivity, použijte **IActivityLogger** objektu k protokolování informací, které vám pomůže vyřešit problémy. Zprávy zaznamenané v uživatele zobrazí\_souboru protokolu 0.

   V **OutputDataset** okně klikněte na tlačítko řez zobrazíte **datový ŘEZ** okno pro tento řez. Zobrazí **běh aktivit** pro tento řez. Měli byste vidět jednu aktivitu spustit řez. Pokud kliknete na tlačítko **spustit** na panelu příkazů můžete spustit jiné aktivity při spuštění stejné řez.

   Když kliknete na aktivity při spuštění, uvidíte **podrobnosti o spuštění aktivit** okno se seznamem souborů protokolu. Zobrazí zprávy zaznamenané v **uživatele\_0 protokolu** souboru. Když dojde k chybě, zobrazí tři běh aktivit, protože počet opakování je nastavena na hodnotu 3 v kódu JSON kanálu nebo aktivity. Po kliknutí na tlačítko spustit aktivitu, zobrazí se soubory protokolů, které můžete zkontrolovat, chcete-li vyřešit chyby.

   ![](./media/data-factory-data-processing-using-batch/image18.png)

   V seznamu souborů protokolu, klikněte na **uživatele 0.log**. V pravém panelu jsou výsledky pomocí **IActivityLogger.Write** metoda.

   ![](./media/data-factory-data-processing-using-batch/image19.png)

   Kontrola systému 0.log pro všechny chybové zprávy systému a výjimky.

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module
    
    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
3. Zahrnout **PDB** souborů v souboru zip tak, aby podrobnosti o chybě informace, jako **zásobník volání** když dojde k chybě.
4. Všechny soubory v souboru zip vlastní aktivity musí být na **top úroveň** s ne v podsložkách.

   ![](./media/data-factory-data-processing-using-batch/image20.png)
5. Ujistěte se, že **assemblyName** (MyDotNetActivity.dll), **entryPoint** (MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer / MyDotNetActivity.zip), a **packageLinkedService** (by měla odkazovat na Azure blob storage, který obsahuje soubor zip) jsou nastaveny na správný hodnoty.
6. Pokud jste opravili chybu a chcete řez zpracovat znovu, klikněte na něj v okně **OutputDataset** pravým tlačítkem myši a potom klikněte na **Spustit**.

   ![](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > Zobrazí **kontejneru** ve službě Azure Blob storage s názvem: `adfjobs`. Tento kontejner není automaticky odstraněn, ale můžete bezpečně odstranit po skončení testování řešení. Podobně řešení Data Factory vytvoří Azure Batch **úlohy** s názvem: `adf-\<pool ID/name\>:job-0000000001`. Po dokončení testu řešení Pokud chcete, můžete odstranit tuto úlohu.
   >
   >
7. Vlastní aktivity nepoužívá **app.config** soubor ze svého balíčku. Proto pokud váš kód čte libovolné púřipojovací řetězce z konfiguračního souboru, ale nefunguje za běhu. Osvědčeným postupem při použití Azure Batch je pro uložení všech tajných klíčů v **Azure KeyVault**, použijte objekt služby založené na certifikátech k ochraně keyvault a distribuovat certifikát do fondu Azure Batch. Vlastní aktivita .NET potom má přístup k tajným klíčům v trezoru za běhu. Toto řešení je obecný a můžete škálovat k libovolnému typu tajný klíč, ne jenom připojovací řetězec.

    Je snazší alternativní řešení (ale není z hlediska): můžete vytvořit **propojená služba Azure SQL** pomocí nastavení připojovacího řetězce, vytvořit datovou sadu, která používá propojené služby a řetězu datovou sadu jako fiktivní vstupní datové sady do vlastní aktivity .NET. Máte přístup připojovací řetězec propojené služby v kódu vlastní aktivity a by bez problémů fungují za běhu.  

#### <a name="extend-the-sample"></a>Ukázka rozšíření
Tato ukázka se dozvíte více o Azure Data Factory a Azure Batch funkce můžete rozšířit. Například ke zpracování řezů v jiné časové rozmezí, proveďte následující kroky:

1. Přidejte následující podsložky v `inputfolder`: 2015-11-16-05 2015-11-16-06 201-11-16-07, 2011-11-16-08, 2015-11-16-09 a umístěte vstupní soubory v těchto složkách. Změnit koncový čas pro kanál z `2015-11-16T05:00:00Z` k `2015-11-16T10:00:00Z`. V **zobrazení diagramu**, dvakrát klikněte **InputDataset**a potvrďte, že vstupní řezy jsou připraveny. Klikněte dvakrát na **OuptutDataset** zobrazíte stav výstup řezy. Pokud jsou ve stavu Připraveno, zkontrolujte výstupní složky pro výstupní soubory.
2. Zvětšit nebo zmenšit **souběžnosti** nastavení pochopit, jak ovlivňuje výkon vašeho řešení, zejména zpracování, k níž dojde v Azure Batch. (Viz krok 4: vytvoření a spuštění kanálu Další informace **souběžnosti** nastavení.)
3. Vytvoření fondu s vyšší nebo nižší **maximální počet úloh na virtuální počítač**. Pokud chcete používat nový fond, kterou jste vytvořili, aktualizujte službu Azure Batch propojené v řešení Data Factory. (Viz krok 4: vytvoření a spuštění kanálu Další informace **maximální počet úloh na virtuální počítač** nastavení.)
4. Vytvoření fondu Azure Batch s **škálování** funkce. Automatické škálování výpočetních uzlů ve fondu Azure Batch je dynamické přizpůsobení výpočetní výkon, které používá vaše aplikace. 

    Vzorec ukázka tady dosahuje následující chování: při vytvoření fondu, začne s virtuálním Počítačem 1. Metrika $PendingTasks definuje počet úloh v spuštěná + aktivní (zařazených do fronty) stavu.  Vzorec najde průměrný počet úkolů čekajících na zpracování v posledních 180 sekund a nastaví TargetDedicated odpovídajícím způsobem. Zajišťuje, že TargetDedicated nikdy překročí 25 virtuálních počítačů. Tak, jako jsou odeslány nové úkoly, fondu automaticky zvětšování a jako dokončení úkolů, virtuální počítače stane volné jeden po druhém a automatické škálování zmenšuje těchto virtuálních počítačů. startingNumberOfVMs a maxNumberofVMs lze upravit vašim potřebám.
 
    Vzorec škálování:

    ``` 
    startingNumberOfVMs = 1;
    maxNumberofVMs = 25;
    pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
    pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
    $TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
    ```

   V tématu [automatické škálování výpočetních uzlů ve fondu Azure Batch](../../batch/batch-automatic-scaling.md) podrobnosti.

   Pokud fondu používá výchozí [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), služba Batch může trvat 15 až 30 minut Příprava virtuálního počítače před spuštěním vlastní aktivity.  Pokud fondu používá jiný autoScaleEvaluationInterval, služba Batch může trvat autoScaleEvaluationInterval + 10 minut.
5. V ukázkové řešení **Execute** metoda vyvolá **Calculate** metoda, která zpracuje řez vstupní data k vytvoření řez výstupních dat. Můžete napsat vlastní metodu ke zpracování vstupních dat a nahraďte volání metody Calculate v Metoda Execute volat metodu.

### <a name="next-steps-consume-the-data"></a>Další kroky: využívat data
Po při zpracování dat, budete moct pracovat s online nástroje, například **Microsoft Power BI**. Tady jsou odkazy, které vám pomohou pochopit Power BI a způsobu jeho použití v Azure:

* [Prozkoumejte datovou sadu v Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [Začínáme s Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [Aktualizovat data v Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [Azure a Power BI – základní – přehled](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a>Odkazy
* [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)

  * [Úvod do služby Azure Data Factory](data-factory-introduction.md)
  * [Začínáme s Azure Data Factory](data-factory-build-your-first-pipeline.md)
  * [Použití vlastních aktivit v kanálu Azure Data Factory](data-factory-use-custom-activities.md)
* [Azure Batch](https://azure.microsoft.com/documentation/services/batch/)

  * [Základy služby Azure Batch](../../batch/batch-technical-overview.md)
  * [Přehled funkcí Azure Batch](../../batch/batch-api-basics.md)
  * [Vytvoření a Správa účtu Azure Batch na portálu Azure](../../batch/batch-account-create-portal.md)
  * [Začínáme s Azure Batch Library pro .NET](../../batch/batch-dotnet-get-started.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
