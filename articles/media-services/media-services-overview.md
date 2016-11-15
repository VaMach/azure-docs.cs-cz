---
title: "Přehled a běžné scénáře Azure Media Services | Dokumentace Microsoftu"
description: "Toto téma nabízí přehled Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 7a5e9723-c379-446b-b4d6-d0e41bd7d31f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 10/12/2016
ms.author: juliako;anilmur
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 2a4395385ab0e6e71e5139883a544add0a081cf2


---
# <a name="azure-media-services-overview-and-common-scenarios"></a>Přehled a běžné scénáře Azure Media Services
Microsoft Azure Media Services je rozšiřitelná cloudová platforma, která vývojářům umožňuje vytvářet škálovatelné aplikace pro správu a doručování médií. Služba Media Services využívá rozhraní REST API, které vám umožní bezpečně nahrávat, ukládat, kódovat a balit obsah (video nebo zvuk) doručovaný na vyžádání i v živě streamovaný různým klientům (například do televizí, počítačů a mobilních zařízení).

Pomocí Media Services můžete vytvářet pracovní postupy od začátku až do konce. V některých částech pracovního postupu můžete použít komponenty třetích stran. Můžete například kódovat pomocí kodéru třetí strany. Potom obsah nahrajete, zabezpečíte, zabalíte a doručíte pomocí služby Media Services.

Svůj obsah můžete streamovat nebo doručovat na vyžádání. Toto téma představuje běžné scénáře doručování obsahu [živě](media-services-overview.md#live_scenarios) nebo [na vyžádání](media-services-overview.md#vod_scenarios). Téma obsahuje i odkazy na další související témata.

## <a name="sdks-and-tools"></a>Sady SDK a nástroje
Pokud chcete vytvořit řešení Media Services, můžete použít následující pomůcky:

* [Rozhraní REST API služby Media Services](https://msdn.microsoft.com/library/azure/hh973617.aspx)
* Jednu z dostupných klientských sad SDK:
* [Azure Media Services SDK pro .NET](https://github.com/Azure/azure-sdk-for-media-services),
* [Azure SDK pro Javu](https://github.com/Azure/azure-sdk-for-java),
* [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),
* [Azure Media Services pro Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Jedná se o verzi sady SDK, kterou nevytvořil Microsoft. Spravuje ji komunita a aktuálně nemá 100% pokrytí rozhraní API pro AMS.)
* Existující nástroje:
* [Azure Portal](https://portal.azure.com/)
* [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) je aplikace napsaná v jazyce Winforms/C# pro Windows.)

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
Mapu kurzů k AMS můžete zobrazit tady:

* [Pracovní postup živého streamování AMS](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
* [Pracovní postup streamování AMS na vyžádání](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

## <a name="prerequisites"></a>Požadavky
Pokud chcete začít používat Azure Media Services, potřebujete následující:

1. Účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com).
2. Účet Azure Media Services. K vytvoření účtu Azure Media Services použijte web Azure Portal, .NET nebo REST API. Další informace najdete v článku o [vytvoření účtu](media-services-portal-create-account.md).
3. (Volitelné) Nastavte vývojové prostředí. Jako vývojové prostředí si zvolte .NET nebo REST API. Další informace najdete v článku o [nastavení prostředí](media-services-dotnet-how-to-use.md).

Seznamte se také s postupem [připojení](media-services-dotnet-connect-programmatically.md) prostřednictvím kódu programu.

1. (Doporučené) Přidělte jednu nebo několik jednotek škálování. Doporučujeme, abyste aplikacím v produkčním prostředí přidělili jednu nebo několik jednotek škálování.   Další informace najdete v článku o [správě koncových bodů streamování](media-services-portal-manage-streaming-endpoints.md).

## <a name="concepts-and-overview"></a>Koncepty a přehled
Informace o konceptech Azure Media Services najdete v článku [Koncepty](media-services-concepts.md).

Řada návodů, které vás seznámí se všemi hlavními součástmi Azure Media Services, najdete v článku o [podrobných kurzech pro Azure Media Services](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series). Tato řada návodů obsahuje přehled konceptů a využívá nástroj AMSE k předvádění úloh AMS. Všimněte si, že nástroj AMSE je nástrojem systému Windows. Tento nástroj podporuje většinu úloh, které můžete provádět prostřednictvím kódu programu se sadami [AMS SDK pro .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK pro Javu](https://github.com/Azure/azure-sdk-for-java), nebo  [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).

## <a name="a-idvodscenariosadelivering-media-ondemand-with-azure-media-services-common-scenarios-and-tasks"></a><a id="vod_scenarios"></a>Doručování médií na vyžádání pomocí Azure Media Services: běžné scénáře a úlohy
Tato část popisuje běžné scénáře a nabízí odkazy na související témata. Následující diagram znázorňuje hlavní části platformy Media Services, které se podílejí na doručování obsahu na vyžádání. 

![Pracovní postup videa na vyžádání (VoD)](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)

### <a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-nonencrypted"></a>Ochrana obsahu v úložišti a doručování streamovaných médií v nešifrované podobě
1. Nahrajte vysoce kvalitní soubor mezzanine do assetu.
   
    Doporučujeme použít na asset možnost šifrování úložiště. Ochráníte tak svůj obsah během nahrávání a během jeho pobytu v úložišti.
2. Zakódujte ho do sady souborů MP4 s adaptivní přenosovou rychlostí. 
   
    Na výstupní asset doporučujeme použít možnost šifrování úložiště. Ochráníte tak váš obsah v úložišti.
3. Nakonfigurujte zásady doručení assetu (používané dynamickým balením). 
   
    Pokud váš asset používá šifrování úložiště, **musíte** nakonfigurovat zásady doručení assetu. 
4. Publikujte asset tím, že vytvoříte lokátor OnDemand.
   
    Zajistěte, abyste na koncovém bodu streamování (ze kterého chcete streamovat obsah) měli aspoň jednu jednotku rezervovanou pro streaming.
5. Streamujte publikovaný obsah.

### <a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>Ochrana obsahu v úložišti, doručování dynamicky šifrovaných streamovaných médií
Abyste mohli používat dynamické šifrování, potřebujete nejdřív získat aspoň jednu jednotku rezervovanou pro streaming na koncovém bodu streamování, ze kterého chcete šifrovaný obsah streamovat.

1. Nahrajte vysoce kvalitní soubor mezzanine do assetu. Na asset použijte možnost šifrování úložiště.
2. Zakódujte ho do sady souborů MP4 s adaptivní přenosovou rychlostí. Na výstupní asset použijte možnost šifrování úložiště.
3. Pokud chcete asset během přehrávání dynamicky šifrovat, vytvořte šifrovací klíč obsahu.
4. Nakonfigurujte zásady autorizace klíče obsahu.
5. Nakonfigurujte zásady doručení assetu (používané dynamickým balením a dynamickým šifrováním).
6. Publikujte asset tím, že vytvoříte lokátor OnDemand.
7. Streamujte publikovaný obsah. 

### <a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a>Použití Media Analytics k získání prakticky uplatnitelných informací z videí
Media Analytics je kolekce řečových a vizuálních komponent, které organizacím a podnikům umožňují, aby ze svých videosouborů odvodily prakticky využitelné informace. Další informace najdete v článku o [přehledu Azure Media Services Analytics](media-services-analytics-overview.md).

1. Nahrajte vysoce kvalitní soubor mezzanine do assetu.
2. Zpracujte videa pomocí jedné z následujících služeb Media Analytics:
   
   * **Indexer** – [zpracování videí pomocí funkce Azure Media Indexer 2](media-services-process-content-with-indexer2.md)
   * **Hyperlapse** – [zpracování mediálních souborů pomocí technologie Azure Media Hyperlapse](media-services-hyperlapse-content.md)
   * **Detekce pohybu** – [detekce pohybu pro Azure Media Analytics](media-services-motion-detection.md)
   * **Rozpoznávání tváří a emoce tváří** – [rozpoznávání tváří a emocí pro Azure Media Analytics](media-services-face-and-emotion-detection.md)
   * **Videosouhrn** – [použití miniatur videa v Azure Media k vytvoření videosouhrnu](media-services-video-summarization.md)
3. Procesory médií z Media Analytics vytvářejí soubory MP4 nebo soubory JSON. Pokud procesor médií vytvořil soubor MP4, můžete ho progresivně stahovat. Pokud procesor médií vytvořil soubor JSON, můžete ho stáhnout z úložiště objektů blob v Azure. 

### <a name="deliver-progressive-download"></a>Doručení progresivního stahování
1. Nahrajte vysoce kvalitní soubor mezzanine do assetu.
2. Zakódujte ho do jednoho souboru MP4.
3. Publikujte asset tím, že vytvoříte lokátor OnDemand nebo SAS.
   
    Pokud používáte lokátor OnDemand, zajistěte si aspoň jednu jednotku rezervovanou pro streaming na koncovém bodu streamování, ze kterého chcete obsah progresivně stahovat.
   
    Pokud používáte lokátor SAS, bude se obsah stahovat z úložiště objektů blob v Azure. V takovém případě nepotřebujete jednotku rezervovanou pro streaming.
4. Progresivně stáhněte obsah.

## <a name="a-idlivescenariosadelivering-live-streaming-events-with-azure-media-services"></a><a id="live_scenarios"></a>Doručování živě streamovaných událostí pomocí Azure Media Services
Na práci s živým streamováním se běžně podílejí následující komponenty:

* Kamera používaná k vysílání události.
* Kodér videa pro kódování v reálném čase, který převádí signály z kamery do datových proudů, které se odesílají do služby živého streamování.

Volitelně několik živých, časově synchronizovaných kodérů. V případě důležitých živých událostí, které vyžadují velmi vysokou dostupnost a kvalitní uživatelský dojem, doporučujeme využívat redundantní kodéry v režimu aktivní/aktivní s časovou synchronizací, abyste dosáhli bezproblémového převzetí služeb při selhání bez ztráty dat.

* Služba živého streamování, která umožňuje následující:
* ingestovat živý obsah pomocí různých protokolů pro živé streamování (například RTMP nebo technologie Smooth Streaming),
* (volitelně) kódovat datový proud na datový proud s adaptivní přenosovou rychlostí
* zobrazit náhled živého datového proudu,
* zaznamenávat a ukládat ingestovaný obsah, abyste ho mohli streamovat později (video na vyžádání)
* doručovat obsah prostřednictvím běžných streamovacích protokolů (například MPEG DASH, Smooth, HLS, HDS) přímo k vašim zákazníkům nebo do sítě pro doručování obsahu (CDN) pro další distribuci.

**Microsoft Azure Media Services** (AMS) nabízí možnost ingestovat, kódovat, zobrazovat náhled, ukládat a doručovat obsah vašeho živého streamování.

Při doručování obsahu zákazníkům je vaším cílem poskytování vysoce kvalitního videa pro různá zařízení v různých síťových podmínkách. Abyste dbali na kvalitu i síťové podmínky, použijte kodéry pro kódování v reálném čase a překódujte svůj datový proud na datový proud videa s více přenosovými rychlostmi (adaptivní přenosová rychlost).  Pokud chcete zajistit o streamování na různá zařízení, použijte [dynamické balení](media-services-dynamic-packaging-overview.md) ve službě Media Services a dynamicky znovu zabalte datový proud do různých protokolů. Služba Media Services podporuje doručování následujících technologií adaptivní přenosové rychlosti streamování: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH a HDS (pouze pro držitele licence Adobe PrimeTime/Access).

Ve službě Azure Media Services se o zpracování všech funkcí živého streamování včetně ingestování, formátování, DVR, zabezpečení, škálovatelnosti a redundance starají **kanály**, **programy** a **koncové body streamování**.

**Kanál** představuje cestu pro zpracování obsahu živého streamování. Kanál může přijímat živé vstupní datové proudy následujícími způsoby:

* Místní kodéru pro kódování v reálném čase odešle **RTMP** nebo **technologie Smooth Streaming** (fragmentovaný soubor MP4) s více přenosovými rychlostmi do kanálu, který je nakonfigurovaný na **průchozí** doručování.  **Průchozí** doručování nastává, když ingestované datové proudy prochází **kanálem** bez dalšího zpracování. K získání výstupu technologie Smooth Streaming s více přenosovými rychlostmi můžete použít následující kodéry pro kódování v reálném čase: Elemental, Envivio, Cisco.  Následující kodéry pro kódování v reálném čase dodávají na výstupu RTMP: Adobe Flash Live, Telestream Wirecast a transkodéry Tricaster.  Kodér pro kódování v reálném čase může také odesílat datový proud s jednou přenosovou rychlostí do kanálu, který nemá povolené kódování v reálném čase, ale tato konfigurace se nedoporučuje. Služba Media Services doručí datový proud zákazníkům na vyžádání.

> [!NOTE]
> Použití průchozí metody je nejekonomičtější způsob, jak živě streamovat při pořádání několika událostí po delší dobu, když jste už investovali do místních kodérů. Viz podrobnosti o [cenách](/pricing/details/media-services/).
> 
> 

* Místní kodér pro kódování v reálném čase odešle datový proud s jednou přenosovou rychlostí do kanálu, který má povolené kódování v reálném čase pomocí služby Media Services, v jednom z následujících formátů: RTP (MPEG-TS), RTMP nebo technologie Smooth Streaming (fragmentovaný soubor MP4). Kanál potom provede kódování v reálném čase pro příchozí datový proud s jednou přenosovou rychlostí v reálném čase na datový proud videa s více přenosovými rychlostmi (adaptivní). Služba Media Services doručí datový proud zákazníkům na vyžádání.

### <a name="working-with-channels-that-receive-multibitrate-live-stream-from-onpremises-encoders-passthrough"></a>Práce s kanály, které přijímají živé datové proudy s více přenosovými rychlostmi z místních kodérů (průchozí)
Následující diagram znázorňuje hlavní části platformy AMS, které se podílejí na **průchozím** pracovním postupu.

![Živý pracovní postup][live-overview2]

Další informace najdete v článku o [práci s kanály, které přijímají živé streamování s více přenosovými rychlostmi z místních kodérů](media-services-live-streaming-with-onprem-encoders.md).

### <a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Práce s kanály, které mají povolené kódování v reálném čase pomocí služby Azure Media Services
Následující diagram znázorňuje hlavní část platformy AMS, které se podílejí na pracovním postupu živého streamování, ve kterém má kanál povolené kódování v reálném čase pomocí služby Media Services.

![Živý pracovní postup][live-overview1]

Další informace najdete v článku o [práci s kanály, které mají povolené kódování v reálném čase pomocí služby Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

## <a name="consuming-content"></a>Konzumace obsahu
Služba Azure Media Services nabízí nástroje, které potřebujete k vytvoření dynamických aplikací pro klientské přehrávače pro většinu platforem včetně: zařízení iOS, zařízení Android, Windows, Windows Phone, Xbox a set top boxy. Následující téma obsahuje odkazy na sady SDK a architektury přehrávačů, které můžete použít pro vývoj vlastních klientských aplikací, které můžou využívat streamovaná média ze služby Media Services.

[Vývoj aplikací pro přehrávání videa](media-services-develop-video-players.md)

## <a name="enabling-azure-cdn"></a>Povolení Azure CDN
Služba Media Services podporuje integraci s Azure CDN. Informace o povolení Azure CDN najdete v článku o [správě koncových bodů streamování v účtu Media Services](media-services-portal-manage-streaming-endpoints.md).

## <a name="scaling-a-media-services-account"></a>Škálování účtu Media Services
Službu **Media Services** můžete škálovat tak, že určíte počet **jednotek rezervovaných pro streamování** a počet **jednotek rezervovaných pro kódování**, které chcete pro svůj účet zřídit.

Svůj účet Media Services můžete škálovat také tím, že k němu přidáte účty úložiště. Každý účet úložiště je omezen na 500 TB. Pokud chcete úložiště rozšířit nad jeho výchozí omezení, můžete k jednomu účtu Media Services připojit více účtů úložiště.

[Toto](media-services-portal-scale-streaming-endpoints.md) téma obsahuje odkazy na související témata.

## <a name="support"></a>Podpora
[Podpora Azure](https://azure.microsoft.com/support/options/) nabízí možnosti odborné pomoci pro Azure včetně služby Media Services.

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="service-level-agreement-sla"></a>Smlouva SLA
* V případě služby Media Services Encoding zaručujeme 99,9% dostupnosti transakcí REST API.
* V případě streamování úspěšně obsluhujeme požadavky se zárukou 99,9% dostupnosti pro existující mediální obsah, pokud jste zakoupili aspoň jednu jednotku streamování.
* V případě živých kanálů zaručujeme externí konektivitu minimálně po 99,9 % času.
* V případě Content Protection zaručujeme úspěšné plnění klíčových požadavků minimálně po 99,9 % času.
* V případě indexeru po 99,9 % času úspěšně obsluhujeme požadavky úloh indexeru, které zpracovává jednotka rezervovaná pro kódování.

Další informace najdete v článku [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).

<!-- Images -->
[overview]: ./media/media-services-overview/media-services-overview.png
[vod-overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
[live-overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png
[live-overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png




<!--HONumber=Nov16_HO2-->


