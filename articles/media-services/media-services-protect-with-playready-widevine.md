---
title: "Použití běžného dynamického šifrování PlayReady nebo Widevine | Dokumentace Microsoftu"
description: "Služba Microsoft Azure Media Services umožňuje doručovat datové proudy MPEG-DASH, technologie Smooth Streaming a Http-Live-Streaming (HLS) chráněné pomocí Microsoft PlayReady DRM. Umožňuje také doručovat datové proudy DASH šifrované pomocí Widevine DRM. Toto téma ukazuje, jak dynamicky šifrovat pomocí PlayReady DRM nebo Widevine DRM."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 548d1a12-e2cb-45fe-9307-4ec0320567a2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: b30a444150c6ace87e9d506da9335373b5e017af
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="using-playready-andor-widevine-dynamic-common-encryption"></a>Použití běžného dynamického šifrování PlayReady nebo Widevine

> [!div class="op_single_selector"]
> * [.NET](media-services-protect-with-playready-widevine.md)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
>
>

Služba Microsoft Azure Media Services umožňuje doručovat datové proudy MPEG-DASH, technologie Smooth Streaming a HTTP-Live-Streaming (HLS) chráněné pomocí [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). Umožňuje také doručovat šifrované datové proudy DASH s licencemi Widevine DRM. Technologie PlayReady a Widevine jsou šifrované podle specifikace Common Encryption (ISO/IEC CENC 23001-7). Konfiguraci zásady AssetDeliveryConfiguration na používání technologie Widevine můžete provést pomocí sady [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (počínaje verzí 3.5.1) nebo rozhraní REST API.

Media Services poskytuje službu k doručování licencí PlayReady DRM a Widevine DRM. Služba Media Services také poskytuje rozhraní API umožňující nakonfigurovat práva a omezení, která má modul runtime PlayReady nebo Widevine DRM vynucovat, když uživatel přehrává chráněný obsah. Když uživatel požaduje obsah chráněný pomocí DRM, aplikace přehrávače si vyžádá licenci z licenční služby AMS. Licenční služba AMS vystaví přehrávači licenci, pokud je k jejímu získání autorizován . Licence PlayReady nebo Widevine obsahuje dešifrovací klíč, kterým může klientský přehrávač dešifrovat a streamovat obsah.

Licence Widevine vám také mohou doručit následující partneři AMS : [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). Další informace najdete v popisu integrace s [Axinom](media-services-axinom-integration.md) a [castLabs](media-services-castlabs-integration.md).

Služba Media Services podporuje více způsobů autorizace uživatelů, kteří žádají o klíč. Zásady autorizace pro klíč k obsahu mohou obsahovat jedno nebo více omezení autorizace: otevřené omezení nebo omezení s tokenem. Zásady omezení tokenem musí být doplněny tokenem vydaným službou tokenů zabezpečení (STS). Služba Media Services podporuje tokeny ve  formátu [jednoduchých webových tokenů](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) a [webových tokenů JSON](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT). Další informace najdete v tématu Konfigurace zásad autorizace klíče k obsahu.

Abyste mohli využívat dynamické šifrování, je třeba mít asset, který obsahuje sadu souborů MP4 s více přenosovými rychlostmi nebo zdrojové soubory technologie Smooth Streaming s více přenosovými rychlostmi. U assetu je také nutné nakonfigurovat zásady pro doručování (popsáno dále v tomto tématu). Potom, na základě formátu určeného v adrese URL streamování, server streamingu na vyžádání zajistí, aby byl datový proud doručen v protokolu podle vašeho výběru. V důsledku toho stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services bude sestavovat a dodávat vhodný formát HTTP v reakci na každý požadavek klienta.

Toto téma bude užitečné pro vývojáře pracující na aplikacích, které doručují média chráněná několika technologiemi DRM, např. PlayReady a Widevine. Téma ukazuje, jak nakonfigurovat službu doručování licencí PlayReady pomocí zásad autorizace tak, aby licence PlayReady nebo Widevine mohli dostávat pouze autorizovaní klienti. Také ukazuje, jak používat dynamické šifrování s licencemi DRM PlayReady nebo Widevine přes streamování DASH.

>[!NOTE]
>Po vytvoření účtu AMS se do vašeho účtu přidá **výchozí** koncový bod streamování ve stavu **Zastaveno**. Pokud chcete spustit streamování vašeho obsahu a využít výhod dynamického balení a dynamického šifrování, musí koncový bod streamování, ze kterého chcete streamovat obsah, být ve stavu **Spuštěno**. 

## <a name="download-sample"></a>Stažení ukázky
Ukázku popsanou v tomto článku si můžete stáhnout [tady](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm).

## <a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a>Konfigurace běžného dynamického šifrování a služeb doručování licencí DRM

Níže jsou uvedeny základní kroky, které je třeba provést, pokud své assety chráníte pomocí technologie PlayReady, používáte službu doručování licencí Media Services a také používáte dynamické šifrování.

1. Vytvořte asset a nahrajte do něj soubory.
2. Zakódujte asset obsahující soubor do sady souborů MP4 s adaptivní přenosovou rychlostí.
3. Vytvořte klíč k obsahu a přiřaďte jej k zakódovanému assetu. Klíč k obsahu ve službě Media Services obsahuje šifrovací klíč assetu.
4. Nakonfigurujte zásady autorizace pro klíč k obsahu. Zásady autorizace pro klíč k obsahu musíte vy nakonfigurovat a klient splnit, aby bylo možné mu klíč k obsahu doručit.

    Při vytváření zásad autorizace pro klíč k obsahu je třeba zadat následující: metodu doručení  (PlayReady nebo Widevine), omezení (otevřené nebo s tokenem) a informace specifické pro daný typ doručení klíče, které definují, jak je klientovi klíč doručen (šablona licence [PlayReady](media-services-playready-license-template-overview.md) nebo [Widevine](media-services-widevine-license-template-overview.md)).

5. Konfigurace zásad doručení pro asset. Konfigurace zásad doručení zahrnuje: doručovací protokol (například MPEG DASH, HLS, technologie Smooth Streaming nebo všechny), typ dynamického šifrování (například Common Encryption), adresu URL pro získání licence PlayReady nebo Widevine.

    Na každý protokol stejného assetu můžete použít jiné zásady. Můžete například použít šifrování PlayReady na protokol Smooth/DASH a šifrování pomocí standardu AES Envelope na protokol HLS. Veškeré protokoly, které nejsou v zásadách doručení definovány (například když přidáte jedinou zásadu, která jako protokol určuje pouze HLS), budou při streamování blokovány. Výjimkou je, pokud nemáte definovány vůbec žádné zásady doručení assetu. Pak budou všechny protokoly povolené v nešifrované podobě.

6. Pokud chcete získat adresu URL streamování vytvořte lokátor OnDemand.

Kompletní příklad rozhraní .NET najdete na konci tématu.

Následující obrázek znázorňuje pracovní postup popsaný výše. Zde se k ověření používá token.

![Ochrana technologií PlayReady](media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

Zbývající část tohoto tématu poskytuje podrobné vysvětlení, ukázky kódů a odkazy na témata, která ukazují, jak dokončit výše popsané úlohy.

## <a name="current-limitations"></a>Aktuální omezení
Pokud přidáte nebo aktualizujete zásady pro doručení assetu, musíte odstranit přidružený lokátor (pokud existuje) a vytvořit nový.

Omezení při šifrování s technologií Widevine ve službě Azure Media Services: v současné době není podporováno více klíčů k obsahu.

## <a name="create-an-asset-and-upload-files-into-the-asset"></a>Vytvoření assetu a nahrání souborů do assetu
Aby bylo možné spravovat, kódovat a streamovat videa, musíte nejprve nahrát obsah do služby Microsoft Azure Media Services. Po nahrání bude váš obsah bezpečně uložen v cloudu pro další zpracování a streamování.

Podrobné informace najdete v článku o [nahrání souborů do účtu služby Media Services](media-services-dotnet-upload-files.md).

## <a name="encode-the-asset-containing-the-file-to-the-adaptive-bitrate-mp4-set"></a>Zakódování assetu obsahujícího soubor do sady souborů MP4 s adaptivní přenosovou rychlostí
V případě dynamického šifrování je třeba pouze vytvořit asset, který obsahuje sadu souborů MP4 s více přenosovými rychlostmi nebo zdrojové soubory technologie Smooth Streaming s více přenosovými rychlostmi. Potom, na základě formátu určeného v manifestu a požadavku na fragment, server streamingu na vyžádání zajistí, abyste datový proud obdrželi v protokolu podle vašeho výběru. Díky tomu pak stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services bude sestavovat a dodávat vhodný formát streamování v reakci na požadavky klientů. Další informace najdete v tématu [Přehled dynamického balení](media-services-dynamic-packaging-overview.md).

Pokyny ke kódování najdete v článku o [kódování assetu pomocí kodéru Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).

## <a id="create_contentkey"></a>Vytvoření klíče k obsahu a jeho přiřazení k zakódovanému assetu
Klíč k obsahu ve službě Media Services obsahuje klíč, kterým chcete asset šifrovat.

Podrobné informace najdete v tématu [Vytvoření klíče k obsahu](media-services-dotnet-create-contentkey.md).

## <a id="configure_key_auth_policy"></a>Konfigurace zásad autorizace klíče obsahu
Služba Media Services podporuje více způsobů ověřování uživatelů, kteří žádají o klíč. Zásady autorizace pro klíč k obsahu musíte vy nakonfigurovat a klient (přehrávač) splnit, aby bylo možné mu klíč doručit. Zásady autorizace pro klíč k obsahu mohou obsahovat jedno nebo více omezení autorizace: otevřené omezení nebo omezení s tokenem.

Podrobnější informace najdete v tématu [Konfigurace zásad autorizace pro klíč k obsahu](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).

## <a id="configure_asset_delivery_policy"></a>Konfigurace zásad doručení assetu
Nakonfigurujte zásady doručení pro asset. Konfigurace zásad doručení assetu zahrnuje následující položky:

* Adresa URL pro získání licence DRM.
* Doručovací protokol assetu (například MPEG DASH, HLS, technologie Smooth Streaming nebo všechny).
* Typ dynamického šifrování (v tomto případě Common Encryption).

Podrobné informace najdete v tématu [Konfigurace zásad doručení assetu ](media-services-rest-configure-asset-delivery-policy.md).

## <a id="create_locator"></a>Pokud chcete získat adresu URL streamování, vytvořte lokátor streamování OnDemand.
Uživateli je třeba poskytnout adresu URL streamování pro protokol Smooth, DASH nebo HLS.

> [!NOTE]
> Pokud přidáte nebo aktualizujete zásady pro doručení assetu, musíte odstranit stávající lokátor (pokud existuje) a vytvořit nový.
>
>

Pokyny k publikování assetu a vytvoření adresy URL streamování najdete v článku o [vytvoření adresy URL streamování](media-services-deliver-streaming-content.md).

## <a name="get-a-test-token"></a>Získání testovacího tokenu
Získejte testovací token na základě omezení s tokenem, které se používá v zásadách autorizace klíče.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string.
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);


K testování datového proudu můžete použít [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

## <a name="create-and-configure-a-visual-studio-project"></a>Vytvoření a konfigurace projektu Visual Studia

1. Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md). 
2. Do části **appSettings** definované ve vašem souboru app.config přidejte následující elementy:

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

## <a name="example"></a>Příklad

Následující příklad ukazuje funkce, které byly zavedeny v sadě Azure Media Services SDK pro .NET verze 3.5.2 (konkrétně se jedná o možnost definovat šablonu licence Widevine a žádat o licenci Widevine ze služby Azure Media Services).

Přepište kód v souboru Program.cs kódem zobrazeným v této části.

>[!NOTE]
>Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy). Pokud vždy používáte stejné dny / přístupová oprávnění, například zásady pro lokátory, které mají zůstat na místě po dlouhou dobu (zásady bez odeslání), měli byste použít stejné ID zásad. Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.

Nezapomeňte aktualizovat proměnné tak, aby odkazovaly do složek, ve kterých jsou umístěné vaše vstupní soubory.

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
    using Newtonsoft.Json;

    namespace DynamicEncryptionWithDRM
    {
        class Program
        {
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static readonly Uri _sampleIssuer =
            new Uri(ConfigurationManager.AppSettings["Issuer"]);
        private static readonly Uri _sampleAudience =
            new Uri(ConfigurationManager.AppSettings["Audience"]);

        // Field for service context.
        private static CloudMediaContext _context = null;

        private static readonly string _mediaFiles =
            Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            bool tokenRestriction = false;
            string tokenTemplateString = null;

            IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
            Console.WriteLine("Uploaded asset: {0}", asset.Id);

            IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
            Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);

            IContentKey key = CreateCommonTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
            Console.WriteLine();

            if (tokenRestriction)
            tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
            else
            AddOpenAuthorizationPolicy(key);

            Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
            Console.WriteLine();

            CreateAssetDeliveryPolicy(encodedAsset, key);
            Console.WriteLine("Created asset delivery policy. \n");
            Console.WriteLine();

            if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
            {
            // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
            // back into a TokenRestrictionTemplate class instance.
            TokenRestrictionTemplate tokenTemplate =
                TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

            // Generate a test token based on the the data in the given TokenRestrictionTemplate.
            // Note, you need to pass the key id Guid because we specified
            // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                        DateTime.UtcNow.AddDays(365));
            Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            // You can use the http://amsplayer.azurewebsites.net/azuremediaplayer.html player to test streams.
            // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).

            string url = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);

            Console.ReadLine();
        }

        static public IAsset UploadFileAndCreateAsset(string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
            Console.WriteLine("File does not exist.");
            return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
        {
            var encodingPreset = "Adaptive Streaming";

            IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} to {1}",
                        inputAsset.Name,
                        encodingPreset));

            var mediaProcessors =
            _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();

            var latestMediaProcessor =
            mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();

            ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
            encodeTask.InputAssets.Add(inputAsset);
            encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset), AssetCreationOptions.StorageEncrypted);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }


        static public IContentKey CreateCommonTypeContentKey(IAsset asset)
        {

            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                        keyId,
                        contentKey,
                        "ContentKey",
                        ContentKeyType.CommonEncryption);

            // Associate the key with the asset.
            asset.ContentKeys.Add(key);

            return key;
        }

        static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
        {

            // Create ContentKeyAuthorizationPolicy with Open restrictions
            // and create authorization policy          

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Open",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                    Requirements = null
                }
                };

            // Configure PlayReady and Widevine license templates.
            string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

            string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

            IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                restrictions, PlayReadyLicenseTemplate);

            IContentKeyAuthorizationPolicyOption WidevinePolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.Widevine,
                restrictions, WidevineLicenseTemplate);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common Content Key with no restrictions").
                Result;


            contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
            contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
            // Associate the content key authorization policy with the content key.
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;
        }

        public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
        {
            string tokenTemplateString = GenerateTokenRequirements();

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString,
                }
                };

            // Configure PlayReady and Widevine license templates.
            string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

            string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

            IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                restrictions, PlayReadyLicenseTemplate);

            IContentKeyAuthorizationPolicyOption WidevinePolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.Widevine,
                restrictions, WidevineLicenseTemplate);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common Content Key with token restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
            contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);

            // Associate the content key authorization policy with the content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
        }

        static private string GenerateTokenRequirements()
        {
            TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

            template.PrimaryVerificationKey = new SymmetricVerificationKey();
            template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
            template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

            return TokenRestrictionTemplateSerializer.Serialize(template);
        }

        static private string ConfigurePlayReadyLicenseTemplate()
        {
            // The following code configures PlayReady License Template using .NET classes
            // and returns the XML string.

            //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user.
            //It contains a field for a custom data string between the license server and the application
            //(may be useful for custom app logic) as well as a list of one or more license templates.
            PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

            // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
            // to be returned to the end users.
            //It contains the data on the content key in the license and any rights or restrictions to be
            //enforced by the PlayReady DRM runtime when using the content key.
            PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
            //Configure whether the license is persistent (saved in persistent storage on the client)
            //or non-persistent (only held in memory while the player is using the license).  
            licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

            // AllowTestDevices controls whether test devices can use the license or not.  
            // If true, the MinimumSecurityLevel property of the license
            // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
            licenseTemplate.AllowTestDevices = true;

            // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class.
            // It grants the user the ability to playback the content subject to the zero or more restrictions
            // configured in the license and on the PlayRight itself (for playback specific policy).
            // Much of the policy on the PlayRight has to do with output restrictions
            // which control the types of outputs that the content can be played over and
            // any restrictions that must be put in place when using a given output.
            // For example, if the DigitalVideoOnlyContentRestriction is enabled,
            //then the DRM runtime will only allow the video to be displayed over digital outputs
            //(analog video outputs won’t be allowed to pass the content).

            //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience.
            // If the output protections are configured too restrictive,
            // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.

            // For example:
            //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

            responseTemplate.LicenseTemplates.Add(licenseTemplate);

            return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
        }

        private static string ConfigureWidevineLicenseTemplate()
        {
            var template = new WidevineMessage
            {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                    new ContentKeySpecs
                    {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                    }
                },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
            };

            string configuration = JsonConvert.SerializeObject(template);
            return configuration;
        }

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            // Get the PlayReady license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

            // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
            // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
            // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption
            // to append /? KID =< keyId > to the end of the url when creating the manifest.
            // As a result Widevine license acquisition URL will have KID appended twice,
            // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.

            Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
            UriBuilder uriBuilder = new UriBuilder(widevineUrl);
            uriBuilder.Query = String.Empty;
            widevineUrl = uriBuilder.Uri;

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                    {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
                    {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}

            };

            // In this case we only specify Dash streaming protocol in the delivery policy,
            // All other protocols will be blocked from streaming.
            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


            // Add AssetDelivery Policy to the asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        }

        /// <summary>
        /// Gets the streaming origin locator.
        /// </summary>
        /// <param name="assets"></param>
        /// <returns></returns>
        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference to the streaming manifest file from the  
            // collection of files in the asset.

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                         EndsWith(".ism")).
                         FirstOrDefault();

            // Create a 30-day readonly access policy.
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

            // Create a locator to the streaming content on an origin.
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

            // Create a URL to the manifest file.
            return originLocator.Path + assetFile.Name;
        }

        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
        }

        static private byte[] GetRandomBuffer(int length)
        {
            var returnValue = new byte[length];

            using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
            {
            rng.GetBytes(returnValue);
            }

            return returnValue;
        }
        }
    }


## <a name="next-steps"></a>Další kroky
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Viz také
[Šifrování CENC s více technologiemi DRM a řízením přístupu](media-services-cenc-with-multidrm-access-control.md)

[Konfigurace balení Widevine pomocí služby AMS](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)

[Uvedení služeb doručování licence Google Widevine ve službě Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
