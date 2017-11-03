---
title: "Pokročilé kódování s pracovním postupem Premium Media Encoder | Microsoft Docs"
description: "Zjistěte, jak ke kódování s pracovním postupem Premium kodér médií. Ukázky kódu jsou napsané v jazyce C# a pomocí sady Media Services SDK pro .NET."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 0f4c87ac-810a-4d42-8df8-923dff2016c6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 2b03853bf07e05c07fd730d5e8a8563963887921
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="advanced-encoding-with-media-encoder-premium-workflow"></a>Pokročilé kódování s pracovním postupem prostředí Media Encoder Premium
> [!NOTE]
> Procesor médií Media Encoder Premium pracovního postupu popsané v tomto tématu není k dispozici v Číně.
>
>

Máte dotazy k kodér premium e-mailu mepd na webu společnosti Microsoft.

## <a name="overview"></a>Přehled
Představuje Microsoft Azure Media Services **Media Encoder Premium pracovního postupu** procesor médií. Tato záloha nabízí procesoru kódování funkcí pro vaše pracovní postupy premium na vyžádání.

Následující témata popisují podrobnosti související s **Media Encoder Premium pracovního postupu**:

* [Formáty podporované v pracovním postupu Media Encoder Premium](media-services-premium-workflow-encoder-formats.md) – formáty popisuje soubor a nepodporuje kodeky **Media Encoder Premium pracovního postupu**.
* [Přehled a porovnání Azure na vyžádání média kodéry](media-services-encode-asset.md) porovná kódování možnosti **Media Encoder Premium pracovního postupu** a **Media Encoder Standard**.

Toto téma ukazuje, jak ke kódování s **Media Encoder Premium pracovního postupu** pomocí rozhraní .NET.

Úlohy pro kódování **Media Encoder Premium pracovního postupu** vyžadují jiný konfigurační soubor, názvem souboru pracovního postupu. Tyto soubory mají příponu .workflow a jsou vytvořené pomocí [Návrhář postupu provádění](media-services-workflow-designer.md) nástroj.

Můžete také získat výchozí soubory pracovního postupu [zde](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). Složka obsahuje také popis těchto souborů.

Soubory pracovního postupu musí být nahrán do vašeho účtu Media Services jako prostředek, a tento prostředek by měl být předané do úlohy kódování.

## <a name="create-and-configure-a-visual-studio-project"></a>Vytvoření a konfigurace projektu Visual Studia

Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md). 

## <a name="encoding-example"></a>Příklad kódování

Následující příklad ukazuje, jak ke kódování s **Media Encoder Premium pracovního postupu**.

Jsou prováděny následovně:

1. Vytvořte asset a nahrajte soubor pracovního postupu.
2. Vytvořte asset a nahrajte soubor zdrojového média.
3. Získáte procesor "Media Encoder Premium Workflow" médií.
4. Vytvoření úlohy a úlohy.

    Ve většině případů je prázdný řetězec konfigurace pro úlohu (jako v následujícím příkladu). Existují některé pokročilé scénáře (které vyžadují, abyste na dynamické nastavení vlastnosti runtime) v takovém případě by zadáte řetězec XML úlohy kódování. Mezi příklady takových scénářů jsou: vytváření překrytí, paralelní nebo po sobě jdoucích média ve hřbetu, titulkování.
5. Přidejte dva vstupní prostředky do úlohy.

    1. 1. – asset pracovního postupu.
    2. 2. – asset videa.

    >[!NOTE]
    >Asset pracovního postupu je nutné přidat úloha před asset média.
   Řetězec konfigurace pro tato úloha by měla být prázdná.
   
6. Odeslání úlohy kódování.

        using System;
        using System.Linq;
        using System.Configuration;
        using System.IO;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;

        namespace MediaEncoderPremiumWorkflowSample
        {
            class Program
            {
                private static readonly string _AADTenantDomain =
                    ConfigurationManager.AppSettings["AADTenantDomain"];
                private static readonly string _RESTAPIEndpoint =
                    ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

                // Field for service context.
                private static CloudMediaContext _context = null;

                private static readonly string _supportFiles =
                    Path.GetFullPath(@"../..\Media");

                private static readonly string _workflowFilePath =
                    Path.GetFullPath(_supportFiles + @"\H264 Progressive Download MP4.workflow");

                private static readonly string _singleMP4InputFilePath =
                    Path.GetFullPath(_supportFiles + @"\BigBuckBunny.mp4");

                static void Main(string[] args)
                {
                    var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
                    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                    _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

                    var workflowAsset = CreateAssetAndUploadSingleFile(_workflowFilePath);
                    var videoAsset = CreateAssetAndUploadSingleFile(_singleMP4InputFilePath);
                    IAsset outputAsset = CreateEncodingJob(workflowAsset, videoAsset);

                }

                static public IAsset CreateAssetAndUploadSingleFile(string singleFilePath)
                {
                    var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);

                    var fileName = Path.GetFileName(singleFilePath);

                    var assetFile = asset.AssetFiles.Create(fileName);

                    Console.WriteLine("Created assetFile {0}", assetFile.Name);

                    Console.WriteLine("Upload {0}", assetFile.Name);

                    assetFile.Upload(singleFilePath);
                    Console.WriteLine("Done uploading {0}", assetFile.Name);

                    return asset;
                }

                static public IAsset CreateEncodingJob(IAsset workflow, IAsset video)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Premium Workflow encoding job");
                    // Get a media processor reference, and pass to it the name of the
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                        processor,
                        "",
                        TaskOptions.None);

                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(workflow);
                    task.InputAssets.Add(video); // we add one asset
                                                 // Add an output asset to contain the results of the job.
                                                 // This output is specified as AssetCreationOptions.None, which
                                                 // means the output asset is not encrypted.
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);

                    // Use the following event handler to check job progress.  
                    job.StateChanged += new
                            EventHandler<JobStateChangedEventArgs>(StateChanged);

                    // Launch the job.
                    job.Submit();

                    // Check job execution and wait for job to finish.
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                    progressJobTask.Wait();

                    // If job state is Error the event handling
                    // method for job progress should log errors.  Here we check
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        throw new Exception("\nExiting method due to job error.");
                    }

                    return job.OutputMediaAssets[0];
                }

                static private void StateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);

                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished.");
                            Console.WriteLine();
                            break;
                        case JobState.Canceling:
                        case JobState.Queued:
                        case JobState.Scheduled:
                        case JobState.Processing:
                            Console.WriteLine("Please wait...\n");
                            break;
                        case JobState.Canceled:
                        case JobState.Error:
                            // Cast sender as a job.
                            IJob job = (IJob)sender;
                            // Display or log error details as needed.
                            // LogJobStop(job.Id);
                            break;
                        default:
                            break;
                    }
                }
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

                    return processor;
                }
            }
        }

Máte dotazy k kodér premium e-mailu mepd na webu společnosti Microsoft.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
