---
title: "Začínáme s Azure blob storage a Visual Studio připojené Services (ASP.NET Core) | Microsoft Docs"
description: "Jak začít používat úložiště objektů blob v Azure po připojení k účtu úložiště pomocí Visual Studio připojené služby v projektu ASP.NET Core v sadě Visual Studio"
services: storage
documentationcenter: 
author: camsoper
manager: wpickett
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: casoper
ms.openlocfilehash: 889afa471eeb556662cddf8eb383a905f08b1f01
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Začínáme s Azure blob storage a Visual Studio připojené Services (ASP.NET Core)

> [!div class="op_single_selector"]
> - [ASP.NET](./vs-storage-aspnet-getting-started-blobs.md)
> - [Jádro ASP.NET](./vs-storage-aspnet-core-getting-started-blobs.md)

Azure blob storage je služba, která ukládá Nestrukturovaná data v cloudu jako objekty nebo objekty BLOB. Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace. Blob storage se také nazývá úložiště objektů.

Tento kurz ukazuje, jak napsat kód ASP.NET Core pro některé běžné scénáře použití služby Azure blob storage. Scénáře zahrnují vytváření kontejner objektů blob a odesílání, výpis, stahování a odstraňování objektů BLOB.

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="prerequisites"></a>Požadavky

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

## <a name="set-up-the-development-environment"></a>Nastavení vývojového prostředí

Tato část vás provede nastavení vývojového prostředí, včetně vytvoření aplikace ASP.NET MVC, přidává se připojení připojené služby, přidáním řadiče a zadání direktivy požadované oboru názvů.

### <a name="create-an-aspnet-mvc-app-project"></a>Vytvoření projektu aplikace ASP.NET MVC

1. Otevřete sadu Visual Studio.

1. Vyberte **souboru -> Nový -> projekt** z hlavní nabídky

1. Na **nový projekt** dialogové okno, zadejte možnosti, jak je znázorněno na následujícím obrázku:

    ![Vytvoření projektu ASP.NET Core](./media/vs-storage-aspnet-core-getting-started-blobs/new-project.png)

1. Vyberte **OK**.

1. Na **nový projekt ASP.NET** dialogové okno, zadejte možnosti, jak je znázorněno na následujícím obrázku:

    ![Zadejte MVC](./media/vs-storage-aspnet-core-getting-started-blobs/new-mvc.png)

1. Vyberte **OK**.

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Používat připojení služby pro připojení k účtu úložiště Azure

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a v místní nabídce vyberte **Přidat -> připojené služby**.

1. Na **přidat připojení službě** dialogovém okně, vyberte **úložiště v cloudu s Azure Storage**a potom vyberte **konfigurace**.

    ![Dialogové okno připojené služby](./media/vs-storage-aspnet-core-getting-started-blobs/connected-services.png)

1. Na **Azure Storage** dialogovém okně, vyberte účet úložiště Azure má být použit pro tento kurz.  Chcete-li vytvořit nový účet úložiště Azure, klikněte na tlačítko **vytvořit nový účet úložiště** a vyplňte formulář.  Po výběru buď existující účet úložiště nebo vytvořit nový, klikněte na tlačítko **přidat**.  Visual Studio se nainstalujte balíček NuGet pro Azure Storage a připojovací řetězec úložiště do **appSettings.JSON určený**.

> [!TIP]
> Další informace o vytvoření účtu úložiště s [portál Azure](https://portal.azure.com), najdete v části [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).
>
> Účet úložiště Azure můžete vytvořit také pomocí [prostředí Azure PowerShell](../storage/common/storage-powershell-guide-full.md), [rozhraní příkazového řádku Azure](../storage/common/storage-azure-cli.md), nebo [prostředí cloudu Azure](../cloud-shell/overview.md).


### <a name="create-an-mvc-controller"></a>Vytvořit řadič MVC 

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **řadiče**a v místní nabídce vyberte **Přidat -> řadiče**.

    ![Přidat řadič do aplikace ASP.NET MVC jádra](./media/vs-storage-aspnet-core-getting-started-blobs/add-controller-menu.png)

1. Na **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **kontroler MVC – prázdný**a vyberte **přidat**.

    ![Zadejte typ řadiče MVC](./media/vs-storage-aspnet-core-getting-started-blobs/add-controller.png)

1. Na **přidat kontroler** dialogové okno, názvu kontroleru *BlobsController*a vyberte **přidat**.

    ![Název řadiče MVC](./media/vs-storage-aspnet-core-getting-started-blobs/add-controller-name.png)

1. Přidejte následující *pomocí* direktivy pro `BlobsController.cs` souboru:

    ```csharp
    using System.IO;
    using Microsoft.Extensions.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="connect-to-a-storage-account-and-get-a-container-reference"></a>Připojení k účtu úložiště a získejte odkaz na kontejner

Kontejner objektů blob je hierarchie vnořených objektů BLOB a složek.  Další kroky v tomto dokumentu vyžadují odkaz na kontejner objektů blob tak, aby kód musí být umístěny vlastní metodu pro – opětovné použití.

Následující postup vytvoření metody pro připojení k účtu úložiště pomocí připojovacího řetězce v **appSettings.JSON určený** a vytvořit odkaz na kontejner.  Nastavení připojovacího řetězce v **appSettings.JSON určený** budou mít názvy ve formátu `<storageaccountname>_AzureStorageConnectionString`. 

1. Otevřete soubor `BlobsController.cs`.

1. Přidejte metodu s názvem **GetCloudBlobContainer** , který vrací **CloudBlobContainer**.  Nezapomeňte nahradit `<storageaccountname>_AzureStorageConnectionString` skutečným názvem klíče v **Web.config**.
    
    ```csharp
    private CloudBlobContainer GetCloudBlobContainer()
    {
        var builder = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("appsettings.json");
        IConfigurationRoot Configuration = builder.Build();
        CloudStorageAccount storageAccount = 
            CloudStorageAccount.Parse(Configuration["ConnectionStrings:aspnettutorial_AzureStorageConnectionString"]);
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
        return container;
    }
    ```

> [!NOTE]
> I když *test kontejneru objektů blob* neexistuje ještě, tento kód vytvoří odkaz na jeho, takže kontejneru lze vytvořit pomocí `CreateIfNotExists` metoda vidět v dalším kroku.

## <a name="create-a-blob-container"></a>Vytvořte kontejner objektů blob

Následující kroky ukazují, jak vytvořit kontejner objektů blob:

1. Přidejte metodu s názvem `CreateBlobContainer` , který vrací `ActionResult`.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Získání `CloudBlobContainer` objekt, který reprezentuje odkaz na název požadovaného objektu blob kontejneru. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Volání `CloudBlobContainer.CreateIfNotExists` metodu pro vytvoření kontejneru, pokud ještě neexistuje. `CloudBlobContainer.CreateIfNotExists` Metoda vrátí **true** Pokud kontejner neexistuje a je úspěšně vytvořen. V opačném **false** je vrácen.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExistsAsync().Result;
    ```

1. Aktualizace `ViewBag` s názvem kontejneru objektů blob.

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```
    
    Následující příklad zobrazuje dokončené `CreateBlobContainer` metoda:

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        ViewBag.Success = container.CreateIfNotExistsAsync().Result;
        ViewBag.BlobContainerName = container.Name;

        return View();
    }
    ```

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **zobrazení** složku a v místní nabídce vyberte **Přidat -> novou složku**. Název nové složky *objekty BLOB*. 

1. V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **objekty BLOB**a v místní nabídce vyberte **Přidat -> zobrazení**.

1. Na **přidat zobrazení** dialogové okno, zadejte **CreateBlobContainer** pro název zobrazení, vyberte **přidat**.

1. Otevřete `CreateBlobContainer.cshtml`a upravit ho tak, aby vypadal jako následující fragment kódu:

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>
    
    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Vyhledejte neuspořádaný seznam, který vypadá takto: `<ul class="nav navbar-nav">`.  Za poslední `<li>` element v seznamu, přidejte následující kód HTML pro přidání další položky navigační nabídky:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="CreateBlobContainer">Create blob container</a></li>
    ```

1. Spusťte aplikaci a vyberte **vytvořit kontejner objektů Blob** a zobrazte výsledky podobné následujícím snímku obrazovky:
  
    ![Vytvoření kontejneru objektů blob](./media/vs-storage-aspnet-core-getting-started-blobs/create-blob-container-results.png)

    Jak je uvedeno nahoře, `CloudBlobContainer.CreateIfNotExists` metoda vrátí **true** pouze když kontejner neexistuje, je vytvořena. Proto pokud aplikace běží, když kontejneru existuje, vrátí metoda **false**.

## <a name="upload-a-blob-into-a-blob-container"></a>Nahrát objekt blob do kontejneru objektů blob

Jednou [se vytvoří kontejner objektů blob](#create-a-blob-container), nahrání souborů do tohoto kontejneru. Tato část vás provede nahráním místního souboru do kontejneru objektů blob. Postup předpokládá, že je kontejner objektů blob s názvem *test kontejneru objektů blob*. 

1. Otevřete soubor `BlobsController.cs`.

1. Přidejte metodu s názvem `UploadBlob` vrátí řetězec, který.

    ```csharp
    public string UploadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. V rámci `UploadBlob` metoda, získání `CloudBlobContainer` objekt, který reprezentuje odkaz na název požadovaného objektu blob kontejneru. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Jak je popsáno výše, Azure storage podporuje typy jiný objektu blob. Tento kurz používá objekty BLOB bloku.  Chcete-li získat odkaz na objekt blob bloku, zavolejte `CloudBlobContainer.GetBlockBlobReference` metoda.

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```
    
    > [!NOTE]
    > Název objektu blob je součástí adresy URL používá k načtení objektu blob a může být libovolný řetězec, včetně názvu souboru.

1. Jakmile bude existovat odkaz na objekt blob, nahrát jakýkoli proud dat k němu voláním odkaz na objekt blob `UploadFromStream` metoda. `UploadFromStream` Metoda vytvoří objekt blob, pokud neexistuje, nebo ho přepíše, pokud neexistuje. (Změnit  *&lt;nahrávání souborů >* na plně kvalifikovanou cestu k souboru k odeslání.)

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(@"<file-to-upload>"))
    {
        blob.UploadFromStreamAsync(fileStream).Wait();
    }
    ```
    
    Následující příklad zobrazuje dokončené `UploadBlob` – metoda (s plně kvalifikovanou cestu k souboru k odeslání):

    ```csharp
    public string UploadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenRead(@"c:\src\sample.txt"))
        {
            blob.UploadFromStreamAsync(fileStream).Wait();
        }
        return "success!";
    }
    ```

1. V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Za poslední `<li>` element v seznamu, přidejte následující kód HTML pro přidání další položky navigační nabídky:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="UploadBlob">Upload blob</a></li>
    ```

1. Spusťte aplikaci a vyberte **nahrávání blob**.  Slovo "success!" má zobrazit.
    
    ![Ověření úspěšné](./media/vs-storage-aspnet-core-getting-started-blobs/upload-blob.png)
  
V části - [seznamu objektů BLOB v kontejneru objektů blob](#list-the-blobs-in-a-blob-container) -znázorňuje, jak zobrazit seznam objektů BLOB v kontejneru objektů blob.    

## <a name="list-the-blobs-in-a-blob-container"></a>Seznam objektů BLOB v kontejneru objektů blob

Tato část ukazuje, jak získat seznam objektů BLOB v kontejneru objektů blob. Ukázka kódu odkazy *test kontejneru objektů blob* vytvořili v části, [vytvořte kontejner objektů blob](#create-a-blob-container).

1. Otevřete soubor `BlobsController.cs`.

1. Přidejte metodu s názvem `ListBlobs` , který vrací `ActionResult`.

    ```csharp
    public ActionResult ListBlobs()
    {
        // The code in this section goes here.

    }
    ```
 
1. V rámci `ListBlobs` metody get `CloudBlobContainer` objekt, který reprezentuje odkaz na kontejner objektů blob. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```
   
1. K zobrazení seznamu objektů BLOB v kontejneru objektů blob, použijte `CloudBlobContainer.ListBlobsSegmentedAsync` metoda. `CloudBlobContainer.ListBlobsSegmentedAsync` Metoda vrátí `BlobResultSegment` obsahující `IListBlobItem` objekty, které lze převést na `CloudBlockBlob`, `CloudPageBlob`, nebo `CloudBlobDirectory` objekty. Následující fragment kódu vytvoří výčet všech objektů BLOB v kontejneru objektů blob. Každý objekt blob je převést na příslušný objekt na základě jeho typu a jeho název (nebo identifikátor URI u `CloudBlobDirectory`) se přidá do seznamu.

    ```csharp
    List<string> blobs = new List<string>();
    BlobResultSegment resultSegment = container.ListBlobsSegmentedAsync(null).Result;
    foreach (IListBlobItem item in resultSegment.Results)
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob blob = (CloudPageBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory dir = (CloudBlobDirectory)item;
            blobs.Add(dir.Uri.ToString());
        }
    }

    return View(blobs);
    ```
    Následující příklad zobrazuje dokončené `ListBlobs` metoda:

    ```csharp
    public ActionResult ListBlobs()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        List<string> blobs = new List<string>();
        BlobResultSegment resultSegment = container.ListBlobsSegmentedAsync(null).Result;
        foreach (IListBlobItem item in resultSegment.Results)
        {
            if (item.GetType() == typeof(CloudBlockBlob))
            {
                CloudBlockBlob blob = (CloudBlockBlob)item;
                blobs.Add(blob.Name);
            }
            else if (item.GetType() == typeof(CloudPageBlob))
            {
                CloudPageBlob blob = (CloudPageBlob)item;
                blobs.Add(blob.Name);
            }
            else if (item.GetType() == typeof(CloudBlobDirectory))
            {
                CloudBlobDirectory dir = (CloudBlobDirectory)item;
                blobs.Add(dir.Uri.ToString());
            }
        }

        return View(blobs);
    }
    ```

1. V **Průzkumníku řešení**, rozbalte **zobrazení** složku, klikněte pravým tlačítkem na **objekty BLOB**a v místní nabídce vyberte **Přidat -> zobrazení**.

1. Na **přidat zobrazení** dialogové okno, zadejte `ListBlobs` pro název zobrazení, vyberte **přidat**.

1. Otevřete `ListBlobs.cshtml`a nahraďte jeho obsah následujícím kódem:

    ```html
    @model List<string>
    @{
        ViewBag.Title = "List blobs";
    }
    
    <h2>List blobs</h2>
    
    <ul>
        @foreach (var item in Model)
        {
            <li>@item</li>
        }
    </ul>
    ```

1. V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Za poslední `<li>` element v seznamu, přidejte následující kód HTML pro přidání další položky navigační nabídky:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="ListBlobs">List blobs</a></li>
    ```

1. Spusťte aplikaci a vyberte **seznamu objektů blob** a zobrazte výsledky podobné následujícím snímku obrazovky:
  
    ![Seznam objektů BLOB](./media/vs-storage-aspnet-core-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Stáhnout objekty blob

V této části ukazuje, jak stáhnout objekt blob a buď zachovat k místnímu úložišti nebo číst obsah do řetězce. Ukázka kódu odkazy *test kontejneru objektů blob* vytvořili v části, [vytvořte kontejner objektů blob](#create-a-blob-container).

1. Otevřete soubor `BlobsController.cs`.

1. Přidejte metodu s názvem `DownloadBlob` vrátí řetězec, který.

    ```csharp
    public string DownloadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. V rámci `DownloadBlob` metody get `CloudBlobContainer` objekt, který reprezentuje odkaz na kontejner objektů blob.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Získat odkaz na objekt blob voláním `CloudBlobContainer.GetBlockBlobReference` metoda. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Chcete-li stáhnout objekt blob, použijte `CloudBlockBlob.DownloadToStream` metoda. Následující kód přenáší obsah do objektu blob na objekt proudu, který je pak trvalé do místního souboru (Změna  *&lt;místní název souboru >* do souboru plně kvalifikovaný název představující, kde se objekt blob je ke stažení.): 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStreamAsync(fileStream).Wait();
    }
    ```
    
    Následující příklad zobrazuje dokončené `ListBlobs` – metoda (s plně kvalifikované cesty pro místního souboru vytváří):
    
    ```csharp
    public string DownloadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenWrite(@"c:\src\downloadedBlob.txt"))
        {
            blob.DownloadToStreamAsync(fileStream).Wait();
        }
        return "success!";
    }
    ```

1. V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Za poslední `<li>` element v seznamu, přidejte následující kód HTML pro přidání další položky navigační nabídky:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="DownloadBlob">Download blob</a></li>
    ```

1. Spusťte aplikaci a vyberte **stažení objektů blob** ke stažení objektu blob. Zadaný v objektu blob `CloudBlobContainer.GetBlockBlobReference` volání metody stáhne do zadaného v umístění `File.OpenWrite` volání metody.  Text "success!" by měl zobrazit v prohlížeči. 

## <a name="delete-blobs"></a>Odstranění objektů blob

Následující kroky ukazují, jak odstranit objekt blob:

1. Otevřete soubor `BlobsController.cs`.

1. Přidejte metodu s názvem `DeleteBlob` vrátí řetězec, který.

    ```csharp
    public string DeleteBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```

1. V rámci `DeleteBlob` metody get `CloudBlobContainer` objekt, který reprezentuje odkaz na kontejner objektů blob.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Získat odkaz na objekt blob voláním `CloudBlobContainer.GetBlockBlobReference` metoda. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Pokud chcete odstranit objekt blob, použijte `Delete` metoda.

    ```csharp
    blob.DeleteAsync().Wait();
    ```
    
    Dokončené `DeleteBlob` metoda by měla vypadat takto:
    
    ```csharp
    public string DeleteBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        blob.DeleteAsync().Wait();
        return "success!";
    }
    ```

1. V **Průzkumníku řešení**, rozbalte **-zobrazení > sdílené** složky a otevřete `_Layout.cshtml`.

1. Za poslední `<li>` element v seznamu, přidejte následující kód HTML pro přidání další položky navigační nabídky:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="DeleteBlob">Delete blob</a></li>
    ```

1. Spusťte aplikaci a vyberte **odstranit objekt blob** odstranit zadaný v objektu blob `CloudBlobContainer.GetBlockBlobReference` volání metody.  Text "success!" by se zobrazit v prohlížeči.  Klikněte na prohlížeč **zpět** tlačítko a pak vyberte **seznamu objektů blob** ověření objektu blob se už v kontejneru.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili ukládání, seznamu a načíst objekty BLOB ve službě Azure Storage pomocí ASP.NET Core.  Projděte si další průvodce funkcemi, kde najdete další informace o dalších možnostech pro ukládání dat v Azure.

  * [Začínáme s Azure table storage a Visual Studio připojené služby (ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
  * [Začínáme s Azure queue storage a Visual Studio připojené služby (ASP.NET)](vs-storage-aspnet-getting-started-queues.md)
