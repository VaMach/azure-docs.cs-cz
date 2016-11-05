---
title: Začínáme s doručováním obsahu na vyžádání pomocí .NET | Microsoft Docs
description: V tomto kurzu vás provede jednotlivými kroky implementace aplikace pro doručování obsahu na vyžádání pomocí služeb Azure Media Services, které používají .NET.
services: media-services
documentationcenter: ''
author: Juliako
manager: erikre
editor: ''

ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/11/2016
ms.author: juliako

---
# <a name="get-started-with-delivering-content-on-demand-using-.net-sdk"></a>Začínáme s doručováním obsahu na vyžádání pomocí sady SDK pro .NET
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

> [!NOTE]
> K dokončení tohoto kurzu potřebujete mít účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](/pricing/free-trial/?WT.mc_id=A261C142F). 
> 
> 

## <a name="overview"></a>Přehled
V tomto kurzu vás provede jednotlivými kroky implementace aplikace pro doručování obsahu videa na vyžádání (VoD, Video-on-Demand) pomocí sady SDK služby Azure Media Services (AMS) pro .NET.

Kurz představuje základní pracovní postup služby Media Services a nejběžnější programovací objekty a úkoly, které Media Services vyžaduje. Po dokončení kurzu bude umět streamovat nebo progresivně stáhnout ukázkový mediální soubor, který jste odeslali, nakódovali a stáhli.

## <a name="what-you'll-learn"></a>Co se dozvíte
Kurz vás seznámí s postupem plnění následujících úloh:

1. Vytvoření účtu Media Services (pomocí webu Azure Portal).
2. Konfigurace koncového bodu streamování (pomocí webu Azure Portal).
3. Vytvoření a konfigurace projektu Visual Studia.
4. Připojení k účtu Media Services.
5. Vytvoření nového prostředku a odeslání videosouboru.
6. Zakódování zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí.
7. Publikování prostředku a získání adres URL pro streamování a progresivní stahování.
8. Testování přehráváním obsahu.

## <a name="prerequisites"></a>Požadavky
K dokončení kurzu potřebujete následující:

* K dokončení tohoto kurzu potřebujete mít účet Azure. 
  
    Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](/pricing/free-trial/?WT.mc_id=A261C142F). Získáte kredity, které můžete použít k vyzkoušení placených služeb Azure. Až kredity vyčerpáte, můžete si účet ponechat a stále používat bezplatné služby a funkce Azure, například funkci Web Apps ve službě Azure App Service.
* Operační systémy: Windows 8 nebo novější, Windows 2008 R2, Windows 7.
* Rozhraní .NET 4.0 nebo novější
* Visual Studio 2010 SP1 (Professional, Premium, Ultimate nebo Express) nebo novější verze.

## <a name="download-sample"></a>Stažení ukázky
Ukázku můžete získat a spustit z [tohoto odkazu](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Vytvoření účtu Azure Media Services pomocí webu Azure Portal
Postup v této části ukazuje, jak vytvořit účet AMS.

1. Přihlaste se na [Azure Portal](https://portal.azure.com/).
2. Klikněte na **+Nový** > **Média a CDN** > **Media Services**.
   
    ![Media Services – vytvoření](./media/media-services-portal-vod-get-started/media-services-new1.png)
3. V okně **VYTVOŘIT ÚČET MEDIA SERVICES** zadejte požadované hodnoty.
   
    ![Media Services – vytvoření](./media/media-services-portal-vod-get-started/media-services-new3.png)
   
   1. Do pole **Název účtu** zadejte název nového účtu AMS. Název účtu Media Services musí obsahovat jenom malá písmena a čísla, nesmí obsahovat mezery a musí mít délku 3 až 24 znaků.
   2. V poli Předplatné si vyberte z různých předplatných Azure, ke kterým máte přístup.
   3. V poli **Skupina prostředků** vyberte nový nebo existující prostředek.  Skupina prostředků je kolekce prostředků, které sdílejí životní cyklus, oprávnění a zásady. Další informace najdete [tady](../resource-group-overview.md#resource-groups).
   4. V poli **Umístění** vyberte zeměpisnou oblast, která se použije k ukládání médií a záznamů metadat pro váš účet Media Services. Tato oblast se použije ke zpracování a streamování vašeho média. V rozevíracím seznamu se vám zobrazí pouze ty oblasti Media Services, které jsou dostupné. 
   5. V poli **Účet úložiště** vyberte účet úložiště, který bude sloužit jako úložiště objektů blob mediálního obsahu z vašeho účtu Media Services. Můžete vybrat existující účet úložiště ve stejné zeměpisné oblasti jako váš účet Media Services, nebo můžete vytvořit účet úložiště. Nový účet úložiště bude vytvořen ve stejné oblasti. Pro názvy účtů úložiště platí stejná pravidla jako pro názvy účtů Media Services.
      
       Další informace o ukládání a úložištích najdete [tady](../storage/storage-introduction.md).
   6. Zaškrtněte **Připnout na řídicí panel**, abyste viděli průběh nasazení účtu.
4. Klikněte na tlačítko **Vytvořit** dole na formuláři.
   
    Po úspěšném vytvoření účtu se stav změní na **Spuštěno**. 
   
    ![Nastavení Media Services](./media/media-services-portal-vod-get-started/media-services-settings.png)
   
    Ke správě vašeho účtu AMS (například nahrávání videí, kódování assetů, sledovat průběhu úloh) použijte okno **Nastavení**.

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Konfigurace koncového bodu streamování pomocí webu Azure Portal
Při práci se službou Azure Media Services je jedním nejběžnější scénářů doručování videa vašim klientům prostřednictvím streamování s adaptivní přenosovou rychlostí. Služba Media Services podporuje následující technologie adaptivní přenosové rychlosti streamování: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH a HDS (pouze pro držitele licence Adobe PrimeTime/Access).

Služba Media Services poskytuje dynamické balení, což vám umožní dodávat váš obsah s adaptivní přenosovou rychlostí s kódováním MP4 nebo technologie Smooth Streaming ve formátech streamování podporovaných službou Media Services (MPEG DASH, HLS, technologie Smooth Streaming a HDS) bez nutnosti mít uložené předem zabalené verze pro každý z těchto formátů streamování.

Pokud chcete využít výhod dynamického balení, proveďte následující:

* Zakódovat váš soubor mezzanine (zdrojový soubor) do sady souborů MP4 s adaptivní přenosovou rychlostí (postup kódování je ukázán později v tomto kurzu).  
* Vytvořit alespoň jednu jednotku streamování pro *koncový bod streamování*, ze kterého plánujete obsah doručovat. Následující postup popisuje, jak změnit počet jednotek streamování.

Při dynamickém balení stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services sestaví a dodá vhodný formát streamování v reakci na požadavky klientů.

Pokud chcete vytvořit a změnit počet jednotek rezervovaných pro streaming, postupujte takto:

1. V okně **Nastavení** klikněte na **Koncové body streamování**. 
2. Klikněte na výchozí koncový bod streamování. 
   
    Zobrazí se okno **VÝCHOZÍ KONCOVÝ BOD STREAMOVÁNÍ – PODROBNOSTI**.
3. Pokud chcete zadat počet jednotek streamování, posuňte jezdcem **Jednotky streamování**.
   
    ![Jednotky streamování](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)
4. Kliknutím na tlačítko **Uložit** uložte provedené změny.
   
   > [!NOTE]
   > Přidělení jakýchkoli nových jednotek může trvat až 20 minut.
   > 
   > 

## <a name="create-and-configure-a-visual-studio-project"></a>Vytvoření a konfigurace projektu Visual Studia
1. V sadě Visual Studio 2013, Visual Studio 2012 nebo Visual Studio 2010 SP1 vytvořte novou konzolovou aplikaci napsanou v jazyce C#. Zadejte **Název**, **Umístění**, **Název řešení** a potom klikněte na tlačítko **OK**.
2. K instalaci **rozšíření sady SDK služby Azure Media Services pro .NET** použijte balíček NuGet [windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions).  Rozšíření sady SDK služby Media Services pro .NET je sada metod rozšíření a pomocných funkcí, které vám zjednoduší kódování a usnadní vývoj pomocí služby Media Services. Při instalaci tohoto balíčku se nainstaluje také **sada SDK služby Media Services pro .NET** a přidá všechny ostatní požadované závislosti.
3. Přidejte odkaz na sestavení System.Configuration. Toto sestavení obsahuje třídu **System.Configuration.ConfigurationManager**, která se používá pro přístup ke konfiguračním souborům, například App.config.
4. Otevřete soubor App.config (pokud projekt soubor ve výchozím nastavení neobsahuje, přidejte ho) a přidejte do něj část *appSettings*. Nastavte hodnoty názvu účtu a klíče účtu Azure Media Services, jak vidíte v následujícím příkladu. Pokud chcete získat informace o názvu a klíči účtu, otevřete portál Azure Classic, vyberte svůj účet mediálních služeb a klikněte na tlačítko **SPRÁVA KLÍČŮ**.
   
        <configuration>
        ...
          <appSettings>
            <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
            <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
          </appSettings>
   
        </configuration>
5. Následujícím kódem přepište existující příkazy **using** na začátku souboru Program.cs.
   
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
6. Vytvořte novou složku v adresáři projektů a zkopírujte si soubor .mp4 nebo .wmv, který chcete kódovat a streamovat nebo progresivně stahovat. V tomto příkladu používáme cestu „C:\VideoFiles“.

## <a name="connect-to-the-media-services-account"></a>Připojení k účtu Media Services
Když službu Media Services používáte s rozhraním .NET, musíte třídu **CloudMediaContext** používat pro většinu programovacích úloh: připojení k účtu Media Services, vytváření, aktualizace, otevírání a odstraňování následujících objektů: prostředky, soubory prostředků, úlohy, zásady přístupu, lokátory atd.

Přepište výchozí třídu Program následujícím kódem. Kód ukazuje, jak číst hodnoty připojení ze souboru App.config a jak vytvořit objekt **CloudMediaContext**, abyste se mohli připojit ke službě Media Services. Další informace o připojení ke službám Media Services najdete v článku [Připojení ke službám Media Services pomocí sady SDK služby Media Services SDK pro .NET](http://msdn.microsoft.com/library/azure/jj129571.aspx).

Funkce **Main** volá metody, které si definujeme v této části.

    class Program
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];

        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;

        static void Main(string[] args)
        {
            try
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Add calls to methods defined in this section.

                IAsset inputAsset =
                    UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

                IAsset encodedAsset =
                    EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

                PublishAssetGetURLs(encodedAsset);
            }
            catch (Exception exception)
            {
                // Parse the XML error message in the Media Services response and create a new
                // exception with its content.
                exception = MediaServicesExceptionParser.Parse(exception);

                Console.Error.WriteLine(exception.Message);
            }
            finally
            {
                Console.ReadLine();
            }
        }

## <a name="create-a-new-asset-and-upload-a-video-file"></a>Vytvoření nového prostředku a odeslání videosouboru
Ve službě Media Services můžete digitální soubory nahrát (nebo ingestovat) do prostředku. Entita **Prostředek** může obsahovat video, zvuk, obrázky, kolekci miniatur, textové stopy a soubory titulků (a metadata o těchto souborech.)  Jakmile soubory odešlete, bude váš obsah bezpečně uložen v cloudu pro další zpracování a streamování. Soubory v prostředku se nazývají **soubory prostředku**.

Metoda **UploadFile** definovaná níže volá metodu **CreateFromFile** (definovanou v rozšíření sady SDK pro .NET). **CreateFromFile** vytvoří nový prostředek, do kterého se zadaný zdrojový soubor odešle.

Metoda **CreateFromFile** přijímá metodu **AssetCreationOptions**, která vám umožňuje určit jednu z následujících možností vytvoření prostředku:

* **Žádné** – nepoužívá se žádné šifrování. Toto je výchozí hodnota. Pamatujte, že při použití této možnosti není váš obsah chráněný během přenosu ani při umístění v úložišti.
  Pokud chcete pomocí progresivního stahování dodávat obsah ve formátu MP4, použijte tuto možnost.
* **StorageEncrypted** – tuto možnost použijte k místnímu šifrování nešifrovaného obsahu pomocí 256bitového šifrování AES (Advanced Encryption Standard). Obsah je poté odeslán do služby Azure Storage, kde bude uložený v zašifrované podobě. Prostředky chráněné pomocí šifrování úložiště jsou před kódováním automaticky bez šifrování umístěny do systému souborů EFS a volitelně se znovu zašifrují před jejich odesláním zpět v podobě nového výstupního prostředku. Případem primárního použití šifrování úložiště je situace, kdy chcete zabezpečit soubory s vysoce kvalitními vstupními médii pomocí silného šifrování na disku.
* **CommonEncryptionProtected** – tuto možnost použijte, pokud nahráváte zašifrovaný obsah chráněný běžným šifrováním nebo DRM s technologií PlayReady (například technologie Smooth Streaming chráněná pomocí DRM s technologií PlayReady).
* **EnvelopeEncryptionProtected** – tuto možnost použijte, pokud odesíláte HLS se šifrováním pomocí standardu AES. Pamatujte, že soubory musí být zakódované a zašifrované pomocí správce transformací.

Metoda **CreateFromFile** umožňuje také určení zpětného volání za účelem hlášení průběhu odesílání souboru.

V následujícím příkladu určíme pro možnosti prostředku možnost **Žádné**.

Přidejte následující metodu do třídy Program.

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }


## <a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>Zakódování zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí
Po zpracování prostředků ve službě Media Services a před jejich předáním klientům můžete média zakódovat, transmuxovat, označit vodoznakem a tak dále. Tyto aktivity se plánují a spouštějí s několika instancemi role na pozadí, abyste měli zajištěný vysoký výkon a dostupnost. Tyto aktivity se nazývají úlohy a každá úloha se skládá z atomických úloh, které vykonávají samotnou práci na souboru prostředku.

Jak jsem už zmínili dřív, při práci se službou Azure Media Services je jedním nejběžnější scénářů doručování streamování s adaptivní přenosovou rychlostí vašim klientům. Media Services umí dynamicky balit sady souborů MP4 s adaptivní přenosovou rychlostí do následujících formátů: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH a HDS (pouze pro držitele licence Adobe PrimeTime/Access).

Pokud chcete využít výhod dynamického balení, proveďte následující:

* Zakódujte nebo převeďte soubor mezzanine (zdrojový soubor) do sady souborů MP4 nebo Smooth Streaming s adaptivní přenosovou rychlostí.  
* Získejte alespoň jednu jednotku streamování pro koncový bod streamování, ze kterého plánujete obsah doručovat.

Následující kód ukazuje, jak se odeslat kódovací úlohu. Úloha obsahuje jednu úlohu, která určuje převod souboru mezzanine do sady souborů MP4 s adaptivní přenosovou rychlostí pomocí **standardu pro kodér médií**. Kód předá úlohu a čeká na její dokončení.

Po dokončení úlohy budete moct streamovat prostředek nebo progresivně stahovat soubory MP4, které jste vytvořili překódováním.
Pamatujte, že k progresivnímu stahování souborů MP4 nepotřebujete mít víc než nulový počet jednotek streamování.

Přidejte následující metodu do třídy Program.

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {

        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.

        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "H264 Multiple Bitrate 720p",
            asset,
            "Adaptive Bitrate MP4",
            options);

        Console.WriteLine("Submitting transcoding job...");


        // Submit the job and wait until it is completed.
        job.Submit();

        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;

        Console.WriteLine("Transcoding job finished.");

        IAsset outputAsset = job.OutputMediaAssets[0];

        return outputAsset;
    }

## <a name="publish-the-asset-and-get-urls-for-streaming-and-progressive-download"></a>Publikování prostředku a získání adres URL pro streamování a progresivní stahování
Pokud chcete prostředek streamovat nebo stáhnout, musíte ho nejdřív „publikovat“ vytvořením lokátoru. Lokátory zajišťují přístup k souborům, které jsou obsaženy v assetu. Služba Media Services podporuje dva typy lokátorů: lokátor OnDemandOrigin, používaný ke streamování médií (například MPEG DASH, HLS nebo technologie Smooth Streaming), a lokátor s přístupovým podpisem (SAS), používaný ke stahování mediálních souborů.

Po vytvoření lokátorů můžete sestavit adresy URL, které budou sloužit ke streamování a stahování souborů.

Streamovací adresa URL pro technologii Smooth Streaming má následující formát:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Streamovací adresa URL pro HLS má následující formát:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Streamovací adresa URL pro MPEG DASH má následující formát:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

SAS adresa URL používaná ke stahování souborů má následující formát:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Rozšíření sady SDK služby Media Services pro .NET  nabízejí užitečné pomocné metody, které vracejí formátované adresy URL publikovaného prostředku.

Následující kód používá rozšíření sady SDK pro .NET k vytvoření lokátorů a k získání adres URL pro streamování a progresivní stahování. Kód také ukazuje způsob stahování souborů do místní složky.

Přidejte následující metodu do třídy Program.

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

## <a name="test-by-playing-your-content"></a>Testování přehráváním obsahu
Po spuštění programu definovaného v předchozí části se v okně konzoly zobrazí adresy URL, které se budou podobat následujícím.

Adresa URL adaptivního streamování:

Technologie Smooth Streaming

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG DASH

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

Adresa URL progresivního stahování (audio a video).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


Pokud chcete video streamovat, použijte [přehrávač služby Azure Media Services](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

Pokud chcete otestovat progresivní stahování, vložte adresu URL do prohlížeče (například Internet Exploreru, Chromu nebo Safari).

## <a name="next-steps:-media-services-learning-paths"></a>Další kroky: Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="looking-for-something-else?"></a>Hledáte něco jiného?
Pokud toto téma neobsahovalo, co jste očekávali, něco mu chybí nebo nějakým způsobem nesplnilo vaše očekávání, pošlete nám svoje připomínky prostřednictvím služby Disqus níže.

<!-- Anchors. -->


<!-- URLs. -->
[Instalace webové platformy]: http://go.microsoft.com/fwlink/?linkid=255386
[Azure Portal]: http://manage.windowsazure.com/



<!--HONumber=Oct16_HO3-->


