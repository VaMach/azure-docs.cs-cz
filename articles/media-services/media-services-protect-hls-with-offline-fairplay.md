---
title: "Chránit obsah s offline Apple FairPlay - Azure HLS | Microsoft Docs"
description: "Toto téma poskytuje přehled a ukazuje, jak pomocí Azure Media Services dynamicky šifrovat obsah HTTP Live Streaming (HLS) s Apple FairPlay v offline režimu."
services: media-services
keywords: "Offline, iOS FairPlay streamování (FPS), HLS, DRM, 10"
documentationcenter: 
author: willzhan
manager: steveng
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: willzhan, dwgeo
ms.openlocfilehash: bf5828ecd6b6bd2e862c4d7709014ecac47c6be0
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="offline-fairplay-streaming"></a>Offline FairPlay streamování
Microsoft Azure Media Services poskytuje sadu dobře navrženou [obsahu služby ochrany](https://azure.microsoft.com/services/media-services/content-protection/), o:
- Microsoft PlayReady
- Google Widevine
- Apple FairPlay
- Šifrování AES-128

Šifrování DRM nebo AES obsahu se provádí dynamicky na základě žádosti o různých protokolů streamování. DRM licence nebo AES dešifrování doručení klíče služby jsou také poskytuje Azure Media Services.

Kromě ochraně obsahu online vysílání datového proudu různých protokolů streamování, je funkce často požadovaný taky offline režim pro chráněný obsah. Podpora offline režimu, je potřeba pro následující scénáře:
1. Přehrávání při připojení k Internetu není k dispozici jako během cesta;
2. Někteří poskytovatelé obsahu může zakáže doručování licencí DRM mimo hranice této země. Pokud si chcete přehrát obsahu při cestování v zahraničí chce uživatel, je potřeba offline stahování.
3. V některých zemích je stále omezená dostupnost Internet nebo šířky pásma. Uživatelé se můžou rozhodnout pro stažení nejprve kdykoli obsah v dostatečně vysoký řešení pro uspokojivé zobrazení prostředí. V takovém případě častěji, problém není dostupnost sítě, místo je omezena šířka pásma sítě. Zprostředkovatelé OTT/OVP se žádostí o podporu offline režimu.

Tento článek popisuje podporu offline režimu streamování FairPlay (FPS) cílení na zařízení se systémem iOS, 10 nebo novější. Tato funkce není podporována pro jiné platformy Apple například watchOS, tvOS nebo Safari v systému macOS.

## <a name="preliminary-steps"></a>Předběžné kroky
Před implementací offline DRM pro FairPlay na zařízení s iOS 10 +, měli byste první:
1. Seznamte se s online ochrana obsahu pro FairPlay. To je podrobně popsány v následujících článcích nebo ukázky:
- [Apple streamování FairPlay všeobecně dostupná služby Azure Media Services](https://azure.microsoft.com/blog/apple-FairPlay-streaming-for-azure-media-services-generally-available/)
- [Chránit váš obsah s Apple FairPlay nebo Microsoft PlayReady HLS](https://docs.microsoft.com/azure/media-services/media-services-protect-hls-with-FairPlay)
- [Ukázka pro streamování online snímků za Sekundu](https://azure.microsoft.com/resources/samples/media-services-dotnet-dynamic-encryption-with-FairPlay/)
2. Získáte FPS SDK od společnosti Apple Developer Network. FPS SDK obsahuje dvě součásti:
- Server FPS SDK, která obsahuje KSM (modulu klíč zabezpečení), klient ukázky, specifikaci a sadu testů vektory;
- FPS nasazení Pack, která obsahuje funkci D specifikace společně s pokyny, jak vygenerovat FPS certifikátů, zákaznické privátní klíč a tajný klíč aplikace klíč (požádejte). Apple problémy sady nasazení FPS jenom na licencovanou poskytovatelů obsahu.

## <a name="configuration-in-azure-media-services"></a>Konfigurace služby Azure Media Services
Pro konfiguraci FPS offline režimu prostřednictvím [Azure Media Services .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices), budete muset použít v Azure Media Services .NET SDK. 4.0.0.4 nebo novější, který poskytuje rozhraní API potřebné ke konfiguraci offline režimu snímků za Sekundu.
Jak je uvedeno výše předpokladů, předpokládáme, že máte funkční kód pro konfiguraci online režim ochrany obsahu snímků za Sekundu. Jakmile máte kód pro konfiguraci ochrany obsahu online režim pro FPS, potřebujete jenom následující dvě změny.

## <a name="code-change-in-fairplay-configuration"></a>Změna kódu v konfiguraci FairPlay
Umožňuje definovat "Povolit offline režim" boolean, volané objDRMSettings.EnableOfflineMode který má hodnotu true při povolování offline scénář DRM. V závislosti na tento ukazatel jsme proveďte následující změny konfigurace FairPlay:

```csharp
if (objDRMSettings.EnableOfflineMode)
    {
        FairPlayConfiguration = Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
            objX509Certificate2,
            pfxPassword,
            pfxPasswordId,
            askId,
            iv, 
            RentalAndLeaseKeyType.PersistentUnlimited,
            0x9999);
    }
    else
    {
        FairPlayConfiguration = Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
            objX509Certificate2,
            pfxPassword,
            pfxPasswordId,
            askId,
            iv);
    }
```

## <a name="code-change-in-asset-delivery-policy-configuration"></a>Změna kódu v konfigurace zásad doručení Assetu
Druhý změnou je přidejte do slovníku slovník < AssetDeliveryPolicyConfigurationKey, řetězec > třetí klíč.
Je třetí AssetDeliveryPolicyConfigurationKey je nutné přidat jako níže: 
```csharp
// FPS offline mode
    if (drmSettings.EnableOfflineMode)
    {
        objDictionary_AssetDeliveryPolicyConfigurationKey.Add(AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense, "true");
        Console.WriteLine("FPS OFFLINE MODE: AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense added into asset delivery policy configuration.");
    }

    // for IAssetDelivery for FPS
    IAssetDeliveryPolicy objIAssetDeliveryPolicy = objCloudMediaContext.AssetDeliveryPolicies.Create(
            drmSettings.AssetDeliveryPolicyName,
            AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
            AssetDeliveryProtocol.HLS,
            objDictionary_AssetDeliveryPolicyConfigurationKey);
```

Po provedení tohoto kroku bude obsahovat slovníku < AssetDeliveryPolicyConfigurationKey, řetězec > v zásady doručení assetu FPS následující tři položky:
1. AssetDeliveryPolicyConfigurationKey.FairPlayBaseLicenseAcquisitionUrl nebo AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl v závislosti na faktorech, jako je například server FPS KSM nebo klíč používá a jestli chcete znovu použít stejné doručení Assetu Zásady napříč více prostředků
2. AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs
3. AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense

Váš účet media services je nyní nakonfigurován tak, aby doručena offline licence FairPlay.

## <a name="sample-ios-player"></a>Ukázka iOS Player
Nejprve je třeba poznamenat, že FPS offline režimu podpora je k dispozici jenom v iOS 10 a novější. Jsme měli získat FPS Server SDK (verze 3.0 nebo novější) obsahující dokument a ukázkové pro offline režim snímků za Sekundu. Konkrétně FPS Server SDK (verze 3.0 nebo novější) obsahuje následující dvě položky související s offline režim:
1. Dokument: Offline přehrávání s FairPlay Streaming a HTTP živé streamování. Společnosti Apple, 9/14/2016. V FPS Server SDK verze 4.0 byla tato doc sloučena do hlavní dokumentace streamování snímků za Sekundu.
2. Ukázkový kód: Ukázka HLSCatalog pro offline režim snímků za Sekundu v v3.1\Development\Client\HLSCatalog_With_FPS\HLSCatalog\ \FairPlay streamování Server SDK. V HLSCatalog ukázková aplikace zejména pro implementaci funkce offline režimu jsou tyto soubory kódu:
- Souboru kódu AssetPersistenceManager.swift: AssetPersistenceManager je hlavní třídy v této ukázce, která demonstruje
    - Jak spravovat stahování datové proudy HLS, jako je například rozhraní API pro spouštění a rušení stahování, odstranění existující prostředky mimo zařízení uživatele;
    - Jak můžete sledovat průběh stahování.
- Soubory kódu AssetListTableViewController.swift a AssetListTableViewCell.swift: AssetListTableViewController je rozhraní pro hlavní této ukázky. Poskytuje seznam prostředky ukázky můžete přehrát, stáhnout, odstranit nebo zrušit stahování. 

Níže jsou uvedeny podrobné pokyny pro nastavení přehrávač spuštěný iOS. Předpokládejme, že spustíte z ukázkové HLSCatalog v FPS Server SDK v 4.0.1.  Je potřeba provést následující změny kódu:

V HLSCatalog\Shared\Managers\ContentKeyDelegate.swift, implementovat metodu `requestContentKeyFromKeySecurityModule(spcData: Data, assetID: String)` pomocí následujícího kódu: umožní drmUr být proměnná přiřazená adresu URL streamování HLS.

```swift
    var ckcData: Data? = nil
    
    let semaphore = DispatchSemaphore(value: 0)
    let postString = "spc=\(spcData.base64EncodedString())&assetId=\(assetIDString)"
    
    if let postData = postString.data(using: .ascii, allowLossyConversion: true), let drmServerUrl = URL(string: self.drmUrl) {
        var request = URLRequest(url: drmServerUrl)
        request.httpMethod = "POST"
        request.setValue(String(postData.count), forHTTPHeaderField: "Content-Length")
        request.setValue("application/x-www-form-urlencoded", forHTTPHeaderField: "Content-Type")
        request.httpBody = postData
        
        URLSession.shared.dataTask(with: request) { (data, _, error) in
            if let data = data, var responseString = String(data: data, encoding: .utf8) {
                responseString = responseString.replacingOccurrences(of: "<ckc>", with: "").replacingOccurrences(of: "</ckc>", with: "")
                ckcData = Data(base64Encoded: responseString)
            } else {
                print("Error encountered while fetching FairPlay license for URL: \(self.drmUrl), \(error?.localizedDescription ?? "Unknown error")")
            }
            
            semaphore.signal()
            }.resume()
    } else {
        fatalError("Invalid post data")
    }
    
    semaphore.wait()
    return ckcData
```

V HLSCatalog\Shared\Managers\ContentKeyDelegate.swift, implementovat metodu `requestApplicationCertificate()`. Tato implementace závisí na tom, jestli vložení certifikátu (jenom veřejný klíč) pomocí zařízení nebo hostitele certifikátu na webu. Níže je implementace pomocí certifikát hostované aplikace použitý v ukázek testu. Umožní certUrl být proměnná obsahující adresu URL certifikátu aplikace.

```swift
func requestApplicationCertificate() throws -> Data {

        var applicationCertificate: Data? = nil
        do {
            applicationCertificate = try Data(contentsOf: URL(string: certUrl)!)
        } catch {
            print("Error loading FairPlay application certificate: \(error)")
        }
        
        return applicationCertificate
    }
```

Pro konečné testování integrované adresa URL videa a adresa URL certifikátu aplikace bude k dispozici v části "Integrované testovací".

V HLSCatalog\Shared\Resources\Streams.plist, přidejte vaše testovací video adresu URL a pro klíč obsahu ID, můžeme jednoduše použít adresu URL pro získání licence FairPlay s protokolem skd jako jedinečných hodnot.

![Offline FairPlay iOS aplikace datové proudy](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-ios-app-streams.png)

Testovací adresu URL videa, adresa URL pro získání licence FairPlay a adresa URL certifikátu aplikace můžete použít vlastní, pokud máte je nastavení, nebo můžete pokračovat v další části, která obsahuje ukázky test.

## <a name="integrated-test"></a>Integrované testu
Ukázky třech testovacích nastavili ve službě Azure Media Services, které zahrnují následující tři scénáře:
1.  FPS chráněný, video, zvuk a alternativní zvuk sledovat;
2.  FPS chráněný, video, zvuk, ale žádné alternativní zvuk sledovat;
3.  FPS chránit pomocí video, zvuk.

Tyto ukázky najdete v této [ukázku lokality](http://aka.ms/poc#22), s odpovídající certifikátu aplikace hostované ve webové aplikace Azure.
Zjistili jsme, že s v3 nebo v4 ukázka FPS Server SDK, pokud hlavní seznam stop obsahuje alternativní zvuk během offline režimu, se přehrávání zvuku pouze. Proto je potřeba odstranit alternativní zvuk. Jinými slovy mezi tři ukázka výše, (2) a (3) fungovat v režimu online i offline. Ale (1) bude přehrajte zvuk pouze během offline režimu při online streamování funguje bez problémů.

## <a name="faq"></a>Nejčastější dotazy
Některé nejčastější dotazy pro řešení potíží:
- **Proč podporuje pouze zvuk play, ale žádné video během offline režimu?** Toto chování se zdá být návrh ukázkovou aplikaci. Po alternativní zvuk sledovat iOS 11 bude použita výchozí alternativní zvukové stopy a během offline režimu, oba iOS 10 k dispozici (což je případ HLS). Toto chování kompenzovat FPS offline režimu, je potřeba odebrat alternativní zvuk sledovat z datového proudu. Chcete-li to provést na straně Azure Media Services, můžete jednoduše přidáme dynamické manifestu filtru "pouze = false." Jinými slovy adresu URL pro HLS by elementů end s .ism/manifest(format=m3u8-aapl,audio-only=false). 
- **Proč ho stále přehrajte zvuk pouze bez video během offline režimu po přidání pouze = false?** V závislosti na návrh klíče mezipaměti CDN může být obsah do mezipaměti. Je třeba vymazat mezipaměť.
- **Je FPS podporuje taky offline režim v systému iOS 11 kromě iOS 10?** Ano, je pro iOS 10 a iOS 11 podporuje offline režim snímků za Sekundu.
- **Proč nelze najít dokumentu do offline režimu přehrávání s FairPlay Streaming a HTTP Live Streaming v FPS Server SDK?** Od FPS Server SDK v 4 byl tento dokument sloučí FairPlay streamování Průvodce programováním dokumentu.
- **Co poslední parametr stanoven v následující rozhraní API pro offline režim FPS?**
`Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(objX509Certificate2, pfxPassword, pfxPasswordId, askId, iv, RentalAndLeaseKeyType.PersistentUnlimited, 0x9999);`

Naleznete v dokumentaci pro toto rozhraní API [zde](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.mediaservices.client.FairPlay.FairPlayconfiguration.createserializedFairPlayoptionconfiguration?view=azure-dotnet). Parametr představuje dobu trvání offline pronájem s hodinu v podobě jednotka.
- **Co je strukturu stáhnout offline souborů na zařízeních s iOS?** Struktura stažený soubor na zařízení s iOS vypadá níže (snímek). `_keys`Složka úložiště stáhli FPS licence soubor jedno úložiště pro každého hostitele licence služby. `.movpkg`složka obsahuje audio a video obsahu. První složka s názvem s pomlčkou následuje jednu číslici obsahuje obsahu videa. Číselná hodnota je "PeakBandwidth" videa interpretace. Druhý složka s názvem s pomlčkou následuje 0 obsahuje zvukového obsahu. Třetí složku s názvem "Data" obsahuje hlavní seznam stop FPS obsahu. Boot.XML poskytuje úplný popis `.movpkg` obsah složky (dole najdete ukázkový soubor boot.xml).

![Offline FairPlay iOS strukturu souborů ukázkové aplikace](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-file-structure.png)

Ukázkový soubor boot.xml:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<HLSMoviePackage xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://apple.com/IMG/Schemas/HLSMoviePackage" xsi:schemaLocation="http://apple.com/IMG/Schemas/HLSMoviePackage /System/Library/Schemas/HLSMoviePackage.xsd">
  <Version>1.0</Version>
  <HLSMoviePackageType>PersistedStore</HLSMoviePackageType>
  <Streams>
    <Stream ID="1-4DTFY3A3VDRCNZ53YZ3RJ2NPG2AJHNBD-0" Path="1-4DTFY3A3VDRCNZ53YZ3RJ2NPG2AJHNBD-0" NetworkURL="https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/QualityLevels(127000)/Manifest(aac_eng_2_127,format=m3u8-aapl)">
      <Complete>YES</Complete>
    </Stream>
    <Stream ID="0-HC6H5GWC5IU62P4VHE7NWNGO2SZGPKUJ-310656" Path="0-HC6H5GWC5IU62P4VHE7NWNGO2SZGPKUJ-310656" NetworkURL="https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/QualityLevels(161000)/Manifest(video,format=m3u8-aapl)">
      <Complete>YES</Complete>
    </Stream>
  </Streams>
  <MasterPlaylist>
    <NetworkURL>https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/manifest(format=m3u8-aapl,audio-only=false)</NetworkURL>
  </MasterPlaylist>
  <DataItems Directory="Data">
    <DataItem>
      <ID>CB50F631-8227-477A-BCEC-365BBF12BCC0</ID>
      <Category>Playlist</Category>
      <Name>master.m3u8</Name>
      <DataPath>Playlist-master.m3u8-CB50F631-8227-477A-BCEC-365BBF12BCC0.data</DataPath>
      <Role>Master</Role>
    </DataItem>
  </DataItems>
</HLSMoviePackage>
```

## <a name="summary"></a>Souhrn
V tomto dokumentu jsme vám poskytli podrobné pokyny a informace pro implementaci FPS offline režimu, včetně:
1. Konfigurace ochrany obsahu Azure Media Services prostřednictvím AMS .NET API. Tím se nakonfiguruje dynamického šifrování FairPlay a doručování licence FairPlay v AMS.
2. na základě vzorku ze sady SDK serveru FPS Apple iOS přehrávač. To by nastavit player iOS, které můžete přehrát FPS obsahu, buď v režimu online streamování nebo v režimu offline.
3. Ukázka FPS videa pro testování režimu offline a online vysílání datového proudu.
4. Nejčastější dotazy o FPS offline režimu.
