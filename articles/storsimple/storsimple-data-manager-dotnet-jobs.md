---
title: "Použití sady .NET SDK pro Microsoft Azure StorSimple Data Manager úlohy | Microsoft Docs"
description: "Další informace o použití sady .NET SDK ke spuštění úlohy StorSimple Manager dat (soukromém náhledu)."
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
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: 44d243a034b20b99faf284c8615e470bc6f9d020
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-net-sdk-to-initiate-data-transformation-private-preview"></a>Pomocí .net SDK zahájíte transformace dat (soukromém náhledu).

## <a name="overview"></a>Přehled

Tento článek vysvětluje, jak můžete použít funkci transformaci dat v rámci služby StorSimple Manager dat k transformaci dat zařízení StorSimple. Transformovaná data se pak spotřebované jinými službami Azure v cloudu. Článek má také návod k usnadnění vytváření ukázkovou aplikaci konzoly .NET zahájíte úlohu transformace dat a pak ji sledovat pro dokončení.

## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, zda máte:
*   Systém s Visual Studio 2012, 2013, 2015 nebo 2017 nainstalována.
*   Nainstalovat Azure Powershell. [Stáhnout prostředí Azure Powershell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Nastavení konfigurace k chybě při inicializaci úlohu transformace dat (pokyny k získání těchto nastavení jsou zde uvedena).
*   Definice úlohy, který byl správně nakonfigurován v prostředku hybridní dat ve skupině prostředků.
*   Všechny požadované knihovny DLL. Stáhněte si tyto knihovny DLL z [úložiště GitHub](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls).
*   `Get-ConfigurationParams.ps1`[skriptu](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1) z úložiště githubu.

## <a name="step-by-step"></a>Podrobný postup

Proveďte následující kroky spusťte úlohu transformace dat pomocí rozhraní .NET.

1. Chcete-li načíst konfigurační parametry, proveďte následující kroky:
    1. Stažení `Get-ConfigurationParams.ps1` ze skriptu úložiště github v `C:\DataTransformation` umístění.
    1. Spustit `Get-ConfigurationParams.ps1` skript z úložiště githubu. Zadejte následující příkaz:

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        Abyste mohli předávat žádné hodnoty pro ActiveDirectoryKey a AppName.


2. Skript vypíše následující hodnoty:
    * ID klienta
    * ID tenanta
    * Klíčů služby Active Directory (stejný jako ten, který je zadaný výše)
    * ID předplatného

3. Pomocí sady Visual Studio 2012, 2013 nebo 2015, vytvořte konzolovou aplikaci C# .NET.

    1. Spusťte **Visual Studio 2012/2013 nebo 2015**.
    1. Klikněte na **Soubor**, přejděte na **Nový** a klikněte na **Projekt**.
    2. Rozbalte **Šablony** a vyberte **Visual C#**.
    3. V seznamu typů projektů napravo vyberte **Konzolová aplikace**.
    4. Zadejte **DataTransformationApp** pro **název**.
    5. Vyberte **C:\DataTransformation** pro **umístění**.
    6. Kliknutím na tlačítko **OK** vytvořte projekt.

4.  Nyní přidejte všechny knihovny DLL v [knihovny DLL](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) složky jako **odkazy** v projektu, který jste vytvořili. Ke stažení souborů dll, postupujte takto:

    1. V sadě Visual Studio, přejděte na **zobrazení > Průzkumníku řešení**.
    1. Klikněte na šipku nalevo od projekt aplikace transformaci dat. Klikněte na tlačítko **odkazy** , klikněte pravým tlačítkem na **přidat odkaz na**.
    2. Přejděte do umístění složky balíčků, vyberte všechny knihovny DLL a klikněte na tlačítko **přidat**a potom klikněte na **OK**.

5. Do zdrojového souboru (Program.cs) v projektu přidejte následující příkazy **using**.

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```


6. Následující kód inicializuje instance úlohy transformace data. Přidejte tuto v **metodu Main**. Nahraďte hodnoty parametrů konfigurace jako dříve získali. Zařadit hodnoty **název skupiny prostředků** a **název zdroje dat hybridní**. **Název skupiny prostředků** je ten, který hostuje datový prostředek hybridní, na kterém byl nakonfigurován definici úlohy.

    ```
    // Setup the configuration parameters.
    var configParams = new ConfigurationParams
    {
        ClientId = "client-id",
        TenantId = "tenant-id",
        ActiveDirectoryKey = "active-directory-key",
        SubscriptionId = "subscription-id",
        ResourceGroupName = "resource-group-name",
        ResourceName = "resource-name"
    };

    // Initialize the Data Transformation Job instance.
    DataTransformationJob dataTransformationJob = new DataTransformationJob(configParams);

    ```

7. Zadejte parametry, pomocí kterých je potřeba spustit definici úlohy

    ```
    string jobDefinitionName = "job-definition-name";

    DataTransformationInput dataTransformationInput = dataTransformationJob.GetJobDefinitionParameters(jobDefinitionName);

    ```

    (NEBO)

    Pokud chcete změnit parametry definice úlohy během doby běhu, přidejte následující kód:

    ```
    string jobDefinitionName = "job-definition-name";
    // Must start with a '\'
    var rootDirectories = new List<string> {@"\root"};

    // Name of the volume on the StorSimple device.
    var volumeNames = new List<string> {"volume-name"};

    var dataTransformationInput = new DataTransformationInput
    {
        // If you require the latest existing backup to be picked else use TakeNow to trigger a new backup.
        BackupChoice = BackupChoice.UseExistingLatest.ToString(),
        // Name of the StorSimple device.
        DeviceName = "device-name",
        // Name of the container in Azure storage where the files will be placed after execution.
        ContainerName = "container-name",
        // File name filter (search pattern) to be applied on files under the root directory. * - Match all files.
        FileNameFilter = "*",
        // List of root directories.
        RootDirectories = rootDirectories,
        // Name of the volume on StorSimple device on which the relevant data is present. 
        VolumeNames = volumeNames
    };
    
    ```

8. Po inicializaci přidejte následující kód k aktivaci úlohy transformace dat v definici úlohy. Zařadit do příslušné **název definice úlohy**.

    ```
    // Trigger a job, retrieve the jobId and the retry interval for polling.
    int retryAfter;
    string jobId = dataTransformationJob.RunJobAsync(jobDefinitionName, 
    dataTransformationInput, out retryAfter);

    ```

9. Tato úloha odešle odpovídající nachází v kořenovém adresáři souborů na svazku zařízení StorSimple do zadaného kontejneru. Při odeslání souboru se zahodí zprávy ve frontě (ve stejném účtu úložiště jako kontejner) se stejným názvem jako definici úlohy. Tato zpráva slouží jako trigger k zahájení dalšího zpracování souboru.

10. Jakmile úloha byla spuštěna, přidejte následující kód se sledovat úlohu pro dokončení.

    ```
    Job jobDetails = null;

    // Poll the job.
    do
    {
        jobDetails = dataTransformationJob.GetJob(jobDefinitionName, jobId);

        // Wait before polling for the status again.
        Thread.Sleep(TimeSpan.FromSeconds(retryAfter));

    } while (jobDetails.Status == JobStatus.InProgress);

    // Completion status of the job.
    Console.WriteLine("JobStatus: {0}", jobDetails.Status);
    
    // To hold the console before exiting.
    Console.Read();

    ```


## <a name="next-steps"></a>Další kroky

[Data Manager zařízení StorSimple pomocí uživatelského rozhraní pro transformaci dat](storsimple-data-manager-ui.md).
