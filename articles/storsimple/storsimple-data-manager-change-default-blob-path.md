---
title: "Změna cesty objektu blob z výchozího | Microsoft Docs"
description: "Zjistěte, jak nastavit Azure funkce přejmenování cestu k objektu blob souboru (soukromém náhledu)."
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/16/2017
ms.author: vidarmsft
ms.openlocfilehash: 057d4d7370207859617eb63238bf425bfa6d3e16
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="change-a-blob-path-from-the-default-path-private-preview"></a>Změňte cestu k objektu blob z výchozí cestu (soukromém náhledu).

Tento článek popisuje, jak nastavit Azure funkce přejmenování výchozí cestu souboru blob. 

## <a name="prerequisites"></a>Požadavky

Ujistěte se, že máte definice úlohy, který byl správně nakonfigurován v prostředku hybridní dat ve skupině prostředků.

## <a name="create-an-azure-function"></a>Vytvoření Azure funkce

Pokud chcete vytvořit Azure funkce, postupujte takto:

1. Přejděte na [portál Azure](http://portal.azure.com/).

2. V horní části levého podokna klikněte na tlačítko **nový**. 

3. V **vyhledávání** zadejte **aplikaci funkce**, a stiskněte klávesu Enter.

    ![Do vyhledávacího pole zadejte "Funkce aplikace"](./media/storsimple-data-manager-change-default-blob-path/goto-function-app-resource.png)

4. V **výsledky** seznamu, klikněte na tlačítko **aplikaci funkce**.

    ![V seznamu výsledků vyberte prostředek aplikace – funkce](./media/storsimple-data-manager-change-default-blob-path/select-function-app-resource.png)

    **Aplikaci funkce** otevře se okno.

5. Klikněte na možnost **Vytvořit**.

    ![Tlačítko "Vytvořit" okno aplikace – funkce](./media/storsimple-data-manager-change-default-blob-path/create-new-function-app.png)

6. Na **aplikaci funkce** okno konfigurace, postupujte takto:

    a. V **název aplikace** zadejte název aplikace.
    
    b. V **předplatné** zadejte název odběru.

    c. V části **skupiny prostředků**, klikněte na tlačítko **vytvořit nový**a pak zadejte název skupiny prostředků.

    d. V **hostování plánování** zadejte **plánování spotřeba**.

    e. V **umístění** zadejte umístění.

    f. V části **účet úložiště**vyberte stávající účet úložiště nebo vytvořte nový účet úložiště. Účet úložiště se používá interně pro funkci.

    ![Zadejte novou aplikaci funkce konfigurační data](./media/storsimple-data-manager-change-default-blob-path/enter-new-funcion-app-data.png)

7. Klikněte na možnost **Vytvořit**.  
    Vytvoření funkce aplikace.

8. V levém podokně klikněte na **další služby**, a poté proveďte následující:
    
    a. V **filtru** zadejte **aplikační služby**.
    
    b. Klikněte na tlačítko **aplikační služby**. 

    ![V levém podokně odkaz "Další služby"](./media/storsimple-data-manager-change-default-blob-path/more-services.png)

9. V seznamu služeb aplikace klikněte na název funkce aplikace.  
    Otevře stránku funkce aplikace.

10. V levém podokně klikněte na **novou funkci**, a poté proveďte následující: 

    a. V **jazyk** seznamu, vyberte **C#**.
    
    b. V poli dlaždice šablony, vyberte **QueueTrigger CSharp**.

    c. V **název funkce** pole, zadejte název funkce.

    d. V **název fronty** zadejte název definice transformaci dat úlohy.

    e. V části **připojení účtu úložiště**, klikněte na tlačítko **nové**a potom vyberte účet, který odpovídá transformaci dat úlohy.  
        Poznamenejte si název připojení. Později v Azure funkce je vyžadován název.

       ![Vytvořte novou funkci jazyka C#](./media/storsimple-data-manager-change-default-blob-path/create-new-csharp-function.png)

    f. Klikněte na možnost **Vytvořit**.  
    **Funkce** otevře se okno.

11. V **funkce** okně Spustit _.csx_ souboru a poté proveďte následující:

    a. Vložte následující kód:

    ```
    using System;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.WindowsAzure.Storage.Queue;
    using Microsoft.WindowsAzure.Storage;
    using System.Collections.Generic;
    using System.Linq;

    public static void Run(QueueItem myQueueItem, TraceWriter log)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["STORAGE_CONNECTIONNAME"]);

        string storageAccUriEndswith = "windows.net/";
        string uri = myQueueItem.TargetLocation.Replace("%20", " ");
        log.Info($"Blob Uri: {uri}");

        // Remove storage account uri string
        uri = uri.Substring(uri.IndexOf(storageAccUriEndswith) + storageAccUriEndswith.Length);

        string containerName = uri.Substring(0, uri.IndexOf("/")); 

        // Remove container name string
        uri = uri.Substring(containerName.Length + 1);

        // Current blob path
        string blobName = uri; 

        string volumeName = uri.Substring(containerName.Length + 1);
        volumeName = uri.Substring(0, uri.IndexOf("/"));

        // Remove volume name string
        uri = uri.Substring(volumeName.Length + 1);

        string newContainerName = uri.Substring(0, uri.IndexOf("/")).ToLower();
        string newBlobName = uri.Substring(newContainerName.Length + 1);

        log.Info($"Container name: {containerName}");
        log.Info($"Volume name: {volumeName}");
        log.Info($"New container name: {newContainerName}");

        log.Info($"Blob name: {blobName}");
        log.Info($"New blob name: {newBlobName}");

        // Create the blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Container reference
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlobContainer newContainer = blobClient.GetContainerReference(newContainerName);
        newContainer.CreateIfNotExists();

        if(!container.Exists())
        {
            log.Info($"Container - {containerName} not exists");
            return;
        }

        if(!newContainer.Exists())
        {
            log.Info($"Container - {newContainerName} not exists");
            return;
        }

        CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
        if (!blob.Exists())
        {
            // Skip to copy the blob to new container, if source blob doesn't exist
            log.Info($"The specified blob does not exist.");
            log.Info($"Blob Uri: {blob.Uri}");
            return;
        }

        CloudBlockBlob blobCopy = newContainer.GetBlockBlobReference(newBlobName);
        if (!blobCopy.Exists())
        {
            blobCopy.StartCopy(blob);
            // Delete old blob, after copy to new container
            blob.DeleteIfExists();
            log.Info($"Blob file path renamed completed successfully");
        }
        else
        {
            log.Info($"Blob file path renamed already done");
            // Delete old blob, if already exists.
            blob.DeleteIfExists();
        }
    }

    public class QueueItem
    {
        public string SourceLocation {get;set;}
        public long SizeInBytes {get;set;}
        public string Status {get;set;}
        public string JobID {get;set;}
        public string TargetLocation {get; set;}
    }

    ```

    b. Nahraďte **STORAGE_CONNECTIONNAME** na řádek 11 s připojením k účtu úložiště (viz. bodu 8 c).

    c. V levé horní části, klikněte na tlačítko **Uložit**.

    ![Uložit – funkce](./media/storsimple-data-manager-change-default-blob-path/save-function.png)

12. K dokončení funkce, přidejte jeden další soubor provedením následujících akcí:

    a. Klikněte na tlačítko **zobrazit soubory**.

       ![Na odkaz "Zobrazit soubory"](./media/storsimple-data-manager-change-default-blob-path/view-files.png)

    b. Klikněte na tlačítko **Přidat**.
    
    c. Typ **project.json**, a stiskněte klávesu Enter.
    
    d. V **project.json** souboru, vložte následující kód:

    ```
    {
    "frameworks": {
        "net46":{
        "dependencies": {
            "windowsazure.storage": "8.1.1"
        }
        }
    }
    }

    ```

    e. Klikněte na **Uložit**.

Vytvořili jste Azure funkce. Tato funkce se aktivuje vždy, když nový objekt blob je generován transformaci dat úlohy.

## <a name="next-steps"></a>Další kroky

[Data Manager zařízení StorSimple pomocí uživatelského rozhraní pro transformaci dat](storsimple-data-manager-ui.md)
