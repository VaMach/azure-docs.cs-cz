---
title: "Poznámky k verzi Media Services | Microsoft Docs"
description: "Poznámky k verzi služby Media Services"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3ca2d7af-1cf0-45fa-9585-3b73f3ee057d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: media
ms.devlang: dotnet
ms.topic: article
ms.date: 10/18/2017
ms.author: juliako
ms.openlocfilehash: 9289958e63be9b853daf6dddd23c403cf6ff2c40
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="azure-media-services-release-notes"></a>Poznámky k verzi Azure Media Services
Tyto poznámky k verzi shrnout změny z předchozích verzí a známé problémy.

> [!NOTE]
> Chceme slyšet od našich zákazníků a zaměřit se na řešení problémů, které vám. Pokud chcete nahlásit problém nebo klást otázky, můžete vystavit ve [fóru služby Azure Media Services MSDN].
> 
> 

## <a name="a-idissuescurrently-known-issues"></a><a id="issues"/>Aktuálně známé problémy
### <a name="a-idgeneralissuesmedia-services-general-issues"></a><a id="general_issues"/>Obecné problémy služby Media Services

| Problém | Popis |
| --- | --- |
| Několik běžných hlaviček protokolu HTTP nejsou k dispozici v rozhraní REST API. |Pokud vyvíjíte aplikace Media Services pomocí rozhraní REST API, zjistíte, že některé běžné pole hlavičky protokolu HTTP (včetně CLIENT-REQUEST-ID, ID žádosti a vrátit-CLIENT-REQUEST-ID) nejsou podporovány. Hlavičky bude přidána v budoucí aktualizaci. |
| Kódování v procentech není povoleno. |Služba Media Services použije hodnotu vlastnosti IAssetFile.Name při sestavování adresy URL pro streamování obsah (například http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Z tohoto důvodu není povoleno kódování v procentech. Hodnota **název** vlastnost nemůže mít žádné z následujících [procent kódování vyhrazené znaky](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Navíc může existovat pouze jedna '. " pro příponu názvu souboru. |
| ListBlobs metoda, která je součástí selže 3.x verze SDK úložiště Azure. |Služba Media Services vygeneruje SAS adresy URL na základě [2012-02-12](https://docs.microsoft.com/rest/api/storageservices/Version-2012-02-12) verze. Pokud chcete používat sadu SDK Azure Storage k seznamu objektů BLOB v kontejneru objektů blob, použijte [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) metoda, která je součástí verze sady SDK úložiště Azure 2.x. ListBlobs metoda, která je součástí verze SDK úložiště Azure 3.x selže. |
| Služba Media Services omezení mechanismus omezuje využití prostředků pro aplikace, které provést nadměrné požadavek na službu. Služba může vrátit stavový kód HTTP služba není dostupná (503). |Další informace najdete v tématu Popis 503 stavový kód HTTP v [Azure Media Services chybové kódy](media-services-encoding-error-codes.md) článku. |
| Při dotazování entity, existuje omezení 1000 entit vrátí najednou, protože veřejné v2 REST omezí výsledky dotazu a 1000 výsledky. |Budete muset použít **přeskočit** a **trvat** (.NET) nebo **horní** (REST), jak je popsáno v [v tomto příkladu .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) a [v tomto příkladu REST API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). |
| Někteří klienti můžete setkat při opakovaném značky problém v manifestu technologie Smooth Streaming. |Další informace najdete v [tomto](media-services-deliver-content-overview.md#known-issues) oddílu. |
| Azure Media Services .NET SDK objektů nelze serializovat a v důsledku nefungují s ukládáním do mezipaměti Azure. |Pokud se pokusíte serializaci objektu SDK AssetCollection tím ho přidáte do ukládání do mezipaměti Azure, je vyvolána výjimka. |


## <a name="a-idrestversionhistoryrest-api-version-history"></a><a id="rest_version_history"/>Historie verzí rozhraní API REST
Informace o historii verze Media Services REST API najdete v tématu [Azure Media Services REST API – referenční informace].

## <a name="october-2017-release"></a>Říjen 2017 verze
> [!IMPORTANT] 
> Upozornění: Azure Media Services bude možné místo začne podporu služby ACS ověřovací klíče.  Na 1. června 2018 už nebudete moci ověřit u back-end AMS prostřednictvím kódu pomocí klíče služby ACS. Je nutné aktualizovat kód Azure Active Directory (AAD) podle článku [Azure Active Directory (Azure AD)-ověřování na základě](media-services-use-aad-auth-to-access-ams-api.md). Na portálu Azure této změny bude také zobrazí se upozornění.

### <a name="updates-for-october-2017-include"></a>Aktualizace pro říjen 2017 zahrnují:
#### <a name="sdks"></a>Sady SDK
* .NET SDK aktualizované kvůli podpoře ověřování AAD.  Jsme odebrali podporu pro ověřování služby ACS z nejnovější sady .NET SDK v Nuget.org podporovat rychlejší migrace aad. 
* JAVA SDK aktualizované kvůli podpoře ověřování AAD.  Jsme doplnili podporu pro ověřování AAD na naše sady Java SDK. Můžete si přečíst podrobnosti o použití sady Java SDK pomocí služby AMS v článku [Začínáme s klientem nástroje Java SDK pro Azure Media Services](media-services-java-how-to-use.md)

#### <a name="file-based-encoding"></a>Soubor na základě kódování
1.  Kodér úrovně Premium teď můžete zakódovat svůj obsah do H.265(HEVC) kodek videa. Neexistuje žádný cenovou vliv pro výběr H.265 přes jiné kodeky jako H.264. Podrobnosti najdete [Online služby podmínky](https://azure.microsoft.com/support/legal/) pro důležitá poznámka o HEVC patent licence.
2.  Pokud máte video zdroje, které je zakódovaných pomocí kodek videa H.265(HEVC), jako je například video zachytit pomocí iOS11 nebo GoPro nejdůležitější 6, můžete teď používat kodér úrovně Premium nebo standardní kodér ke kódování těchto videa. Podrobnosti najdete [Online služby podmínky](https://azure.microsoft.com/support/legal/) pro důležitá poznámka o patentová licence.
3.  Pokud máte obsah, který obsahuje více jazyk zvukových stop, pak tak dlouho, dokud jazyk hodnoty jsou správně označené podle odpovídající specifikaci formátu souboru (například ISO MP4), pak můžete standardní kodér ke kódování pro tento obsah streamování. Výsledná Lokátor streamování zobrazí seznam dostupných zvuk jazyků.
4.  Standardní kodér teď podporuje dva nové přednastavení systému jen zvuk, "AAC zvuk" a "AAC dobrý kvalitu zvuku". Jak vytvořit stereo AAC výstupu v přenosové rychlosti 128 kb/s a 192 kb/s.
5.  Kodér úrovně Premium teď podporuje formáty souborů MOV QuickTime nebo jako vstup, dokud kodek videa je některý z [Apple ProRes typů tady](https://docs.microsoft.com/azure/media-services/media-services-media-encoder-standard-formats), a AAC nebo PCM zvukovém souboru. Kodér úrovně Premium nepodporuje, například DVC/DVCPro video uzavřen do MOV QuickTime nebo soubory, jako vstup.  Standardní kodér však podporuje tyto kodeky videa.
6.  Opravy chyb v kodéry:
    * Teď můžete odeslat úlohy pomocí prostředek vstup a po těchto dokončení upravit Asset (například o přidání/odstranění/přejmenování souborů v rámci Asset) a odeslat další úlohy. 
    * Zvýšení kvality JPEG miniatur vyprodukované kodér úrovně Standard
    * Vylepšení standardní kodéru pro krátkodobé videa. Lepší zpracování vstupu metadata a vytváření miniatur v velmi krátká doba trvání videa.
    * Vylepšení H.264 dekodér použít ve standardní kodér eliminuje určité výjimečných artefakty. 

#### <a name="media-analytics"></a>Media Analytics
* GA Azure Redactor média – tento média procesoru (MP) provést anonymization stírá řezy vybrané jednotlivců a je ideální pro použití v veřejné scénáře zabezpečení a média zprávy. Přehled na tuto novou procesoru, naleznete v příspěvku blogu [zde](https://azure.microsoft.com/blog/azure-media-redactor/). Podrobnou dokumentaci a nastavení najdete v tématu [redigovat řezy s Azure Media Analytics](media-services-face-redaction.md).



## <a name="june-2017-release"></a>2017 června verze

Služba Media Services nyní podporuje [Azure Active Directory (Azure AD)-ověřování na základě](media-services-use-aad-auth-to-access-ams-api.md).

> [!IMPORTANT]
> V současné době Media Services podporuje model řízení přístupu Azure služby ověřování. Řízení přístupu autorizace však bude na 1. června 2018 zastaralá. Doporučujeme, abyste na model ověřování Azure AD migrovali co nejdříve.

## <a name="march-2017-release"></a>2017 března verze

Teď můžete použít Azure Media Standard na [automaticky generovat žebříku přenosovou rychlostí](media-services-autogen-bitrate-ladder-with-mes.md) zadáním "Adaptivní streamování" přednastavení řetězec při vytvoření úlohy kódování. Pokud chcete ke kódování video pro streamování pomocí služby Media Services, je doporučené přednastavení "Adaptivní Streaming". Pokud potřebujete přizpůsobit pro konkrétní scénář přednastavení kódování, můžete začít s [tyto](media-services-mes-presets-overview.md) přednastavení.

Teď můžete použít Azure Media Standard nebo Media Encoder Premium pracovní postup [vytvořit kódování úkol, který generuje fMP4 bloky](media-services-generate-fmp4-chunks.md). 

## <a name="february-2017-release"></a>2017 února verze

Od 1. dubna 2017 se automaticky odstraní libovolný záznam úlohy ve vašem účtu, který je starší než 90 dní. Spolu s ním se odstraní přidružené záznamy úkolů, a to i v případě, že celkový počet záznamů je nižší než maximální kvóta. Pokud potřebujete informace o úlohách/úkolech archivovat, můžete použít kód popsaný [tady](media-services-dotnet-manage-entities.md).

## <a name="january-2017-release"></a>Ledna 2017 verze

V Microsoft Azure Media Services (AMS) **koncový bod streamování** představuje streamování služba, která může poskytnout obsah přímo k aplikaci player klienta nebo do obsah Delivery Network (CDN) pro další distribuci. Služba Media Services také poskytuje bezproblémovou integraci Azure CDN. Výstupní datový proud z StreamingEndpoint služba může být živý datový proud, video na vyžádání nebo progresivní stahování asset ve vašem účtu Media Services. Každý účet Azure Media Services obsahuje výchozí StreamingEndpoint. V rámci účtu se dají vytvořit další koncové body streamování. Existují dvě verze koncové body streamování, 1.0 a 2.0. Od verze 10 ledna 2017 AMS všechny nově vytvořené účty budou obsahovat verze 2.0 **výchozí** StreamingEndpoint. Další streamování koncové body, které přidáte k tomuto účtu budou i verze 2.0. Tato změna nemá vliv na existující účty; stávající koncové body streamování je verze 1.0 a lze upgradovat na verzi 2.0. V této změně bude chování, fakturace a funkce změny (Další informace najdete v tématu [to](media-services-streaming-endpoints-overview.md) článku).

Kromě toho od 2.15 verze služby Azure Media Services přidali následující vlastnosti do koncového bodu streamování entity: **CdnProvider**, **CdnProfile**, **FreeTrialEndTime** , **StreamingEndpointVersion**. Podrobný přehled o těchto vlastností najdete v části [to](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

## <a name="december-2016-release"></a>Verze prosinec 2016

Azure Media Services nyní umožňuje přístup k datům telemetrie/metriky pro jeho služby. Aktuální verze AMS umožňuje shromažďovat telemetrická data pro kanál za provozu, StreamingEndpoint, a za chodu archivu entity. Další informace najdete v [tomto](media-services-telemetry-overview.md) článku.

## <a name="a-idjulychanges16july-2016-release"></a><a id="july_changes16"/>Verze července 2016
### <a name="updates-to-manifest-file-ism-generated-by-encoding-tasks"></a>Aktualizace souboru manifestu (*. ISM) generované úlohy kódování
Při odeslání kódování úloh pro Media Encoder Standard nebo Azure Media Encoder, generuje úlohu kódování [streamování souboru manifestu](media-services-deliver-content-overview.md) (* .ism) souboru ve výstupu Asset. Pomocí nejnovější verze služby je aktualizovaná syntaxe tohoto streamování souboru manifestu.

> [!NOTE]
> Syntaxe streamování soubor manifestu (.ism) je vyhrazená pro interní použití a v budoucích verzích se může změnit. Změnit nebo upravit obsah tohoto souboru.
> 
> 

### <a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Nový klient manifest (*. Soubor ISMC) je generován ve výstupu Asset při kódování úloh výstupy jeden nebo více souborů MP4
Počínaje nejnovější verze služby po dokončení kódování úloh, který generuje jeden další soubor MP4, výstup bude Asset také obsahovat streamovaná soubor manifestu (*.ismc) klienta. Soubor .ismc pomáhá zlepšit výkon dynamické streamování. 

> [!NOTE]
> Syntaxe souboru manifestu (.ismc) klienta je vyhrazená pro interní použití a v budoucích verzích se může změnit. Změnit nebo upravit obsah tohoto souboru.
> 
> 

Další informace najdete v tématu [to](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/) blogu.

### <a name="known-issues"></a>Známé problémy
Někteří klienti můžete setkat při opakovaném značky problém v manifestu technologie Smooth Streaming. Další informace najdete v [tomto](media-services-deliver-content-overview.md#known-issues) oddílu.

## <a id="apr_changes16"></a>Verze. dubna 2016
### <a name="azure-media-analytics"></a>Azure Media Analytics
Azure Media Services zavedli Azure Media Analytics výkonné video intelligence. Podrobné informace najdete v tématu [přehled Azure Media Services Analytics](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (Preview)
Azure Media Services umožňuje dynamicky šifrovat vaší HTTP Live Streaming (HLS) obsahu s Apple FairPlay. Doručení licenční služby AMS můžete použít také k poskytování licence FairPlay klientům. Podrobnější informace najdete v tématu [pomocí Azure Media Services k vysílání datového proudu obsahu chráněného s Apple FairPlay vaše HLS.

## <a id="feb_changes16"></a>Verze. února 2016
Nejnovější verzi Azure Media Services SDK pro platformu .NET (3.5.3) obsahuje Widevine související oprava chyb. Problém byl: AssetDeliveryPolicy nelze znovu použít pro více prostředků, které jsou šifrované pomocí Widevine. V rámci této opravy chyb následující vlastnost byla přidána do sady SDK: **WidevineBaseLicenseAcquisitionUrl**.

    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},

    };

## <a id="jan_changes_16"></a>Verze leden 2016
Jednotky rezervované pro kódování přejmenovat ke snížení záměně se kodér názvy.

Basic, Standard a Premium kódování vyhrazené jednotky jsou přejmenované na S1, S2, a jednotky rezervované pro S3, v uvedeném pořadí.  Zákazníci používající základní kódování RUs dnes uvidí S1 jako štítek na portálu Azure (a v kusovníku) při Standard a Premium se zobrazí popisky S2 a S3 v uvedeném pořadí. 

## <a id="dec_changes_15"></a>Verze prosince 2015

### <a name="azure-media-encoder-deprecation-announcement"></a>Azure Media Encoder vyřazení oznámení

Azure Media Encoder přestanou počínaje přibližně po dobu 12 měsíců od verze nástroje Media Encoder Standard.

### <a name="azure-sdk-for-php"></a>Sada Azure SDK for PHP
Tým služby Azure SDK publikovaná novou verzi [Azure SDK pro jazyk PHP](http://github.com/Azure/azure-sdk-for-php) balíček, který obsahuje aktualizací a nových funkcí pro Microsoft Azure Media Services. Konkrétně Azure Media Services SDK pro jazyk PHP teď podporuje nejnovější [obsahu ochrany](media-services-content-protection-overview.md) funkce: dynamické šifrování AES a DRM (PlayReady a Widevine) a bez omezení Token. Mimoto podporuje i škálování [kódování jednotky](media-services-dotnet-encoding-units.md).

Další informace naleznete v tématu:

* [Microsoft Azure Media Services SDK pro jazyk PHP](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) blogu.
* Následující [ukázky kódu jsou](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) k vám pomůžou začít rychle:
  * **vodworkflow_aes.php**: Toto je soubor PHP, který ukazuje, jak používat dynamické šifrování AES-128 a služba pro přenos klíče. Je založena na rozhraní .NET vzorku vysvětlené v [to](media-services-protect-with-aes128.md) článku.
  * **vodworkflow_aes.php**: Toto je soubor PHP, který ukazuje způsob použití dynamického šifrování PlayReady a službu doručování licencí. Je založena na rozhraní .NET vzorku vysvětlené v [to](media-services-protect-with-playready-widevine.md) článku.
  * **scale_encoding_units.php**: Toto je soubor PHP, který ukazuje, jak se škálovat kódování jednotku rezervovanou.

## <a id="nov_changes_15"></a>Verze. listopadu 2015.
Azure Media Services nyní nabízí služba doručování licence Google Widevine v cloudu. Další informace najdete v tématu [tomto blogu oznámení](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Další informace naleznete v [v tomto kurzu](media-services-protect-with-playready-widevine.md) a [úložiště GitHub](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Služeb doručování licence Widevine poskytovaných služeb Media Azure jsou ve verzi preview. Další informace najdete v tématu [tomto blogu](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

## <a id="oct_changes_15"></a>Verze říjen 2015
Azure Media Services (AMS) je nyní za provozu v následujících datových centrech: Brazílie – Jih, Indie – Západ, Indie – jih a Indie – střed. Nyní můžete portál Azure [vytvoření účtů Media Service](media-services-portal-create-account.md) a provádění různých úloh popsaných [zde](https://azure.microsoft.com/documentation/services/media-services/). Funkce Live Encoding ale v těchto datových center není povolená. Kromě toho nejsou v těchto datových centrech dostupné všechny typy jednotek rezervovaných pro kódování.

* Brazílie – jih: Dostupné jsou jenom jednotky rezervované pro kódování typu Standard a Basic.
* Indie – Západ, Indie – jih a Indie – střed: dostupné jsou jenom základní jednotky rezervované pro kódování

## <a id="september_changes_15"></a>Verze září 2015
* AMS teď nabízí možnost chránit na vyžádání Video-On-Demand (VOD) a živé datové proudy s technologií Widevine DRM modulární. Můžete použít následující partneři služeb doručování licence na Widevine doručit: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). Další informace najdete v tématu [tomto blogu](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).
  
    Konfiguraci zásady AssetDeliveryConfiguration na používání technologie Widevine můžete provést pomocí sady [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (počínaje verzí 3.5.1) nebo rozhraní REST API.  
* AMS přidala se podpora pro Apple ProRes videa. Teď můžete nahrát QuickTime zdrojových souborech videa využívající Apple ProRes nebo jiných kodeky. Další informace najdete v tématu [tomto blogu](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).
* Nyní můžete Media Encoder Standard udělat extrakce archivu subclipping a za provozu. Další informace najdete v tématu [tomto blogu](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).
* Byly provedeny následující aktualizace filtrování: 
  
  * Teď můžete použít formát Apple HTTP Live Streaming (HLS) s filtrem jen zvukovém souboru. Tato aktualizace umožňuje odebrat pouze sledovat zadáním (pouze = false) v adrese URL.
  * Při definování filtrů pro vaše prostředky, máte nyní možnost kombinovat více (až 3) filtry v jednu adresu URL.
    
    Další informace najdete v tématu [to](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.
* AMS teď podporuje I rámce v HLS v4. Podpora I rámce optimalizuje operace rychlé převíjení vpřed a zpět. Ve výchozím nastavení všechny výstupy v4 HLS obsahovat I rámce seznam stop (EXT-X-I-FRAME-STREAM-INF).
  
    Další informace najdete v tématu [to](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogu.

## <a id="august_changes_15"></a>Verze srpen 2015
* Azure Media Services SDK pro verze Java V0.8.0 a nové ukázky je nyní k dispozici. Další informace naleznete v tématu:
  
  * [Příspěvek blogu](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
  * [Java ukázky úložiště](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
* Azure Media Player aktualizace s podporou více zvukový stream. Další informace naleznete v tématu:
  * [Příspěvek blogu](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)

## <a id="july_changes_15"></a>Verze července 2015
* Uvedení obecné dostupnosti Media Encoder Standard. Další informace najdete v tématu [tomto příspěvku na blogu](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).
  
    Media Encoder Standard používá přednastavení popsané v [to](http://go.microsoft.com/fwlink/?LinkId=618336) části. Pokud používáte jedno z přednastavení pro kóduje 4k, měli byste obdržet **Premium** vyhrazený typ jednotky. Další informace najdete v tématu [postup škálování kódování](media-services-scale-media-processing-overview.md).
* Za provozu v reálném čase titulky s Azure Media Services a přehrávač. Další informace najdete v tématu [tento příspěvek blogu](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/)

### <a name="media-services-net-sdk-updates"></a>Aktualizace .NET SDK služby Media Services
Azure Media Services .NET SDK je nyní verze 3.4.0.0. V této verzi byl přidán následující funkce:  

* Implementovaná podpora živé archivu. Nelze stáhnout asset, který obsahuje za provozu archivu.
* Implementovaná podpory pro dynamické filtry.
* Implementovaná funkce, které umožňuje uživatelům zobrazovat kontejner úložiště při odstraňování prostředku.
* Opravy chyb související s opakujte zásad v kanály.
* Povolit **Media Encoder Premium pracovního postupu**.

## <a id="june_changes_15"></a>Verze červen 2015
### <a name="media-services-net-sdk-updates"></a>Aktualizace .NET SDK služby Media Services
Azure Media Services .NET SDK je nyní verze 3.3.0.0. V této verzi byl přidán následující funkce:  

* Podpora pro specifikaci OpenId Connect zjišťování
* Podpora pro zpracování výměny klíče na straně zprostředkovatele identity. 

Pokud používáte poskytovatele identity, která zveřejňuje OpenID Connect zjišťování dokumentu (stejně jako tyto zprostředkovatele: Azure Active Directory, Google, Salesforce), můžete určit, aby Azure Media Services získat podpisové klíče pro ověření tokenu JWT z OpenID Připojte specifikace zjišťování. 

Další informace najdete v tématu [pomocí Json webové klíče z OpenID Connect specifikace zjišťování pro práci s JWT tokenu ověřování ve službě Azure Media Services](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).

## <a id="may_changes_15"></a>Verze květen 2015
Uvedení následující nové funkce:

* [Náhled Live Encoding pomocí služby Media Services](media-services-manage-live-encoder-enabled-channels.md)
* [Dynamické manifestu](media-services-dynamic-manifest-overview.md)
* [Náhled procesor médií Azure Media Hyperlapse](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

## <a id="april_changes_15"></a>Vydání duben 2015
### <a name="general-media-services-updates"></a>Aktualizace služby Obecné Media Services
* [Uvedení přehrávač médií Azure](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/).
* Počínaje Media Services REST 2.10, kanály, které jsou nakonfigurovány na ingestování protokol RTMP jsou vytvořeny pomocí primární a sekundární ingestovaných adres URL. Další informace najdete v tématu [kanál ingestování konfigurace](media-services-live-streaming-with-onprem-encoders.md#channel_input)
* Azure Media Indexer aktualizace
* Podpora pro španělštinu
* Nový formát xml konfigurace

Další informace najdete v tématu [tomto blogu](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

### <a name="media-services-net-sdk-updates"></a>Aktualizace .NET SDK služby Media Services
Azure Media Services .NET SDK je nyní verze 3.2.0.0.

Tady jsou některé z zákazníků, kterým čelí aktualizace:

* **Narušující změny**: změnit **TokenRestrictionTemplate.Issuer** a **TokenRestrictionTemplate.Audience** být typu řetězec.
* Aktualizace související s vytvářením vlastních opakujte zásady.
* Opravy chyb související s nahrávání nebo stahování souborů.
* **MediaServicesCredentials** třída nyní přijímá koncový bod řízení primární a sekundární přístup k ověřování na základě.

## <a id="march_changes_15"></a>Verze března 2015
### <a name="general-media-services-updates"></a>Aktualizace služby Obecné Media Services
* Služba Media Services nyní poskytuje integrace Azure CDN. Pro podporu integrace produktů, **CdnEnabled** vlastnost byla přidána do **StreamingEndpoint**.  **CdnEnabled** lze použít s rozhraní REST API počínaje verzí 2.9 (Další informace najdete v tématu [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint)).  **CdnEnabled** je možné pomocí .NET SDK počínaje verzí 3.1.0.2 (Další informace najdete v tématu [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint\(v=azure.10\).aspx)).
* Oznámení o **Media Encoder Premium pracovního postupu**. Další informace najdete v tématu [Představení služby Azure Media Services kódování Premium](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).

## <a id="february_changes_15"></a>Verze únor 2015
### <a name="general-media-services-updates"></a>Aktualizace služby Obecné Media Services
Media Services REST API je nyní verze 2.9. Od této verze, můžete povolit integraci Azure CDN s koncové body streamování. Další informace najdete v tématu [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

## <a id="january_changes_15"></a>Verze leden 2015
### <a name="general-media-services-updates"></a>Aktualizace služby Obecné Media Services
Oznámení o obecné dostupnosti (GA) obsahu ochrany v případě dynamického šifrování. Další informace najdete v tématu [Azure Media Services zvyšuje streamování zabezpečení s technologií obecné dostupnosti DRM](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

### <a name="media-services-net-sdk-updates"></a>Aktualizace .NET SDK služby Media Services
Azure Media Services .NET SDK je nyní verze 3.1.0.1.

Tato verze označena výchozí konstruktor Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate jako zastaralé. Nový konstruktor trvá TokenType jako argument.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


## <a id="december_changes_14"></a>Verze z prosince 2014
### <a name="general-media-services-updates"></a>Aktualizace služby Obecné Media Services
* Některé aktualizace a nové funkce byly přidány do Azure Indexer Media procesoru. Další informace najdete v tématu [poznámky k verzi Azure Media Indexer verze 1.1.6.7](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
* Přidání jednotky rezervované pro nové rozhraní API REST, které vám umožní aktualizovat kódování: [EncodingReservedUnitType se zbytkem](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype).
* Podpora přidání CORS pro doručení klíče služby.
* Vylepšení výkonu dotazů na možnosti zásad autorizace se provádí.
* V Číně datové centrum [adresa URL doručení klíčů](https://docs.microsoft.com/rest/api/media/operations/contentkey#get_delivery_service_url) je nyní každého zákazníka (stejně jako v jiných datových centrech).
* Trvání cíl automaticky přidané HLS. Při provádění živé vysílání datového proudu, HLS je vždy zabalené dynamicky. Ve výchozím nastavení služba Media Services automaticky vypočítá HLS poměr balení segmentu (FragmentsPerSegment) na základě @keyframe, které určuje intervalu (KeyFrameInterval), také označuje jako skupiny z obrázky – GOP, přijatých z kodéru za provozu. Další informace najdete v tématu [práci s Azure Media Services živým streamováním].

### <a name="media-services-net-sdk-updates"></a>Aktualizace .NET SDK služby Media Services
* [Azure Media Services .NET SDK](http://www.nuget.org/packages/windowsazure.mediaservices/) je nyní verze 3.1.0.0.
* Upgrade .net SDK závislost na rozhraní .NET Framework 4.5.
* Přidat nové rozhraní API, které vám umožní aktualizovat jednotky rezervované pro kódování. Další informace najdete v tématu [aktualizace vyhrazený typ jednotky a zvýšení kódování RUs pomocí rozhraní .NET](media-services-dotnet-encoding-units.md).
* Přidání JWT (JSON Web Token) podporu pro ověření tokenu. Další informace najdete v tématu [ověření pomocí tokenu JWT v Azure Media Services a dynamickým šifrováním](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
* Přidání relativní posunutí pro BeginDate a Datumvypršení platnosti v šabloně licence PlayReady.

## <a id="november_changes_14"></a>Verze v listopadu 2014
* Služba Media Services umožňuje ingestovat živý obsah, který je technologie Smooth Streaming (FMP4) připojení přes protokol SSL. K ingestování přes protokol SSL, nezapomeňte aktualizovat adresu URL ingestování protokolem HTTPS.  V současné době nepodporuje AMS SSL s vlastní domény.  Další informace o živé streamování najdete v tématu [práci s Azure Media Services živým streamováním].
* V současné době nelze ingestování RTMP živý datový proud připojení přes protokol SSL.
* Pouze proudy přes protokol SSL Pokud koncový bod streamování, ze kterého doručení obsahu byl vytvořen po 10. září 2014. Jsou-li u koncových bodů streamování vytvořený po 10 září adresám URL streamování, adresa URL obsahuje "streaming.mediaservices.windows.net" (nový formát). Streamování adresy URL, které obsahují "origin.mediaservices.windows.net" (starý formát) nepodporují SSL. Pokud je adresa URL v původním formátu a chcete Streamovat přes protokol SSL, [vytvořit nový koncový bod streamování](media-services-portal-manage-streaming-endpoints.md). Pomocí adresy URL na nový koncový bod streamování základě vytvořit Streamovat obsah přes protokol SSL.

## <a id="october_changes_14"></a>Verze říjen 2014
### <a id="new_encoder_release"></a>Kodér verze služby Media Services
Uvedení nové verze kodér médií Azure Media Services. S nejnovější Media Encoder Azure, můžete se účtují poplatky pro výstup GB, ale jinak nového modulu encoder je funkce, které jsou kompatibilní s předchozí kodér. Další informace [podrobnosti o cenách na Media Services]).

### <a id="oct_sdk"></a>Služba Media Services .NET SDK
Media Services SDK pro .NET rozšíření je nyní verze 2.0.0.3.

Media Services SDK pro .NET je nyní verze 3.0.0.8.

Byly provedeny následující změny:

* Refaktoring tříd zásady opakování.
* Přidání řetězec uživatelského agenta do hlavičky požadavku http.
* Přidání kroku sestavení obnovení NuGet.
* Oprava testy scénář použití x509 certifikát z úložiště.
* Ověření nastavení při aktualizaci kanálu a datových proudů end.

### <a name="new-github-repository-to-host-media-services-samples"></a>Nové úložiště GitHub pro hostitele ukázky Media Services
Ukázky jsou umístěné v [úložiště GitHub ukázky Azure Media Services](https://github.com/Azure/Azure-Media-Services-Samples).

## <a id="september_changes_14"></a>Verze září 2014
Metadata Media Services REST je nyní verze 2.7. Další informace o nejnovějších aktualizacích REST najdete v tématu [Azure Media Services REST API – referenční informace].

Media Services SDK pro .NET je nyní verze 3.0.0.7

### <a id="sept_14_breaking_changes"></a>Nejnovější změny
* **Původ** byla přejmenována na [StreamingEndpoint].
* Ke změně výchozího chování při použití **portál Azure** ke kódování a pak publikovat soubory MP4.

### <a id="sept_14_GA_changes"></a>Nové funkce nebo scénáře, které jsou součástí verze GA
* **Procesor médií indexer**. Další informace najdete v tématu [indexování mediálních souborů pomocí Azure Media Indexer].
* [StreamingEndpoint] entity teď umožňuje přidat názvy vlastních domén (hostitel).
  
    Pro vlastní název domény pro použití jako název koncového bodu streamování Media Services musíte přidat vlastní hostitel názvy pro koncový bod streamování. Pro přidání vlastního hostitele názvy použijte Media Services REST API nebo .NET SDK.
  
    Platí následující aspekty:
  
  * Musíte mít vlastnictví vlastní název domény.
  * Vlastnictví název domény musí být ověřený službou Azure Media Services. Chcete-li ověřit doménu, vytvořte záznam CName, který se mapuje <MediaServicesAccountId> <parent domain> ověřit DNS. < mediaservices zónu dns >. 
  * Musíte vytvořit jiný CName, který mapuje název vlastního hostitele (např: sports.contoso.com) pro název hostitele vašeho Media Services StreamingEndpont (např: amstest.streaming.mediaservices.windows.net).

    Další informace najdete v tématu **CustomHostNames** vlastnost [StreamingEndpoint] článku.

### <a id="sept_14_preview_changes"></a>Nové funkce nebo scénáře, které jsou součástí verze public preview
* Živé streamování Preview. Další informace najdete v tématu [práci s Azure Media Services živým streamováním].
* Služba doručení klíče. Další informace najdete v tématu [pomocí dynamického šifrování AES-128 a služba pro přenos klíče].
* Dynamické šifrování AES. Další informace najdete v tématu [pomocí dynamického šifrování AES-128 a služba pro přenos klíče].
* Službu doručování licencí PlayReady. Další informace najdete v tématu [pomocí dynamického šifrování PlayReady a službu doručování licencí].
* Šifrování PlayReady dynamické. Další informace najdete v tématu [pomocí dynamického šifrování PlayReady a službu doručování licencí].
* Mediální služby šablona licence PlayReady. Další informace najdete v tématu [Media Services PlayReady licence šablony přehled].
* Streamování úložiště šifrované prostředky. Další informace najdete v tématu [obsahu šifrované streamování úložiště].

## <a id="august_changes_14"></a>Verze 2014 srpen
Při kódování prostředek, prostředek výstup vytváří po dokončení úlohy kódování. Dokud nebude tato verze Azure Media Services Encoder vytváří metadata o prostředcích výstup. Od této verze kodér také vytvoří metadata o vstupní prostředky. Další informace najdete v tématu [vstupu Metadata] a [výstup metadat] články.

## <a id="july_changes_14"></a>Verze červenec 2014
Pro Azure Media Services Balíčkovač a modul pro šifrování byly provedeny následující opravy chyb:

* Pouze zvuk hraje zpět při převedení asset za provozu archivu do HTTP Live Streaming – to byl opraven a teď se přehrávají audio a video.
* Když balení prostředek HTTP Live Streaming a obálky šifrování AES 128 bitů, zabalené datové proudy nejsou přehrávány na zařízeních s Androidem – tato chyba byla opravena a zabalené datový proud přehrává na zařízeních s Androidem, které podporují protokol HTTP Live Streaming.

## <a id="may_changes_14"></a>Verze může 2014
### <a id="may_14_changes"></a>Aktualizace služby Obecné Media Services
Teď můžete použít [dynamické balení] datového proudu HTTP Live Streaming (HLS) v3. Stream HLS verze 3, přidejte následující formát do Lokátor cesty k počátku: * .ism/manifest(format=m3u8-aapl-v3). Další informace najdete v tématu [Nick Drouin Blog].

Dynamické balení teď také podporuje doručování HLS (v3 a v4) šifrovat pomocí PlayReady podle staticky šifrovat pomocí PlayReady technologie Smooth Streaming. Informace o tom, jak šifrování, technologie Smooth Streaming s technologií PlayReady najdete v tématu [datový proud Smooth Protecting s technologií PlayReady].

### <a name="may_14_donnet_changes"></a>Aktualizace .NET SDK služby Media Services
Tato vylepšení jsou zahrnuté ve verzi sady Media Services .NET SDK 3.0.0.5:

* Vyšší rychlost a odolnosti pro nahrávání nebo stahování média prostředky.
* Vylepšení v opakování logiku a přechodná výjimek: 
  
  * Pro výjimky, které jsou způsobeny dotazování, ukládají se změny, nahrávání nebo stahování souborů byly vylepšené logiku přechodná chyba zjišťování a zkuste to znovu. 
  * Při získávání webové výjimky (například během požadavek tokenu služby ACS), si všimnete, že závažné chyby selhávají rychlejší teď.

Další informace najdete v tématu [opakujte logiky sady Media Services SDK pro .NET].

## <a id="april_changes_14"></a>Kodér vydání duben 2014
### <a name="april_14_enocer_changes"></a>Kodér aktualizace služby Media Services
* Přidaná podpora pro příjem souborů AVI vytvořené pomocí editoru nelineární tráva Valley EDIUS, kde je lehce komprimována pomocí tráva Valley Ústředí/HQX kodek. Další informace najdete v tématu [tráva Valley ohlášen EDIUS 7 vysílání datového proudu prostřednictvím the Cloud].
* Byla přidána podpora pro zadání zásady vytváření názvů pro soubory vyprodukované kodér médií. Další informace najdete v tématu [řízení Media Service kodér výstup názvy souborů].
* Přidaná podpora pro video nebo zvuk překryvy. Další informace najdete v tématu [vytváření překryvy].
* Přidaná podpora pro ve hřbetu dohromady víc video segmentů. Další informace najdete v tématu [ve hřbetu segmenty Video].
* Opravit chyby související s překódování soubory MP4 s rychlostmi kde zvukovém souboru zakódování vrstva zvuk MPEG-1 (neboli MP3 3).

## <a id="jan_feb_changes_14"></a>Verze leden/února 2014
### <a name="jan_fab_14_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.1, 3.0.0.2 a 3.0.0.3
Změny v 3.0.0.1 a 3.0.0.2 patří:

* Opravené problémy související s využití dotazů LINQ s příkazy OrderBy.
* Rozdělení testovací řešení v [Githubu] do jednotkové testování a testy na základě scénáře.

Další informace o změnách, najdete v tématu: [uvolní Azure Media Services .NET SDK 3.0.0.1 a 3.0.0.2].

Byly provedeny následující změny v 3.0.0.3:

* Upgradovat závislosti úložiště Azure pro použití verze 3.0.3.0. 
* Vyřešený problém zpětné kompatibility 3.0. *.* uvolní. 

## <a id="december_changes_13"></a>Verze prosinci 2013
### <a name="dec_13_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.0
> [!NOTE]
> verze 3.0.x.x nejsou zpětně kompatibilní s verzemi 2.4.x.x.
> 
> 

Nejnovější verzi sady Media Services SDK je nyní 3.0.0.0. Můžete stáhnout nejnovější balíček z NuGet nebo získat službu bits z [Githubu].

Od verze sady Media Services SDK verze 3.0.0.0, můžete opakovaně použít [Active Directory řízení přístupu Azure Service (ACS)] tokeny. Další informace najdete v části "Opětovné použití tokeny přístupu ke řízení služby" v [připojení ke službám Media Services pomocí sady Media Services SDK pro .NET] článku.

### <a name="dec_13_donnet_ext_changes"></a>Rozšíření sady SDK pro .NET 2.0.0.0 služby Azure Media Services
Rozšíření Azure Media Services .NET SDK je sada metod rozšíření a pomocných funkcí, které zjednoduší kódování a usnadní vývoj pomocí Azure Media Services. Můžete získat nejnovější bits z [rozšíření Azure Media Services .NET SDK].

## <a id="november_changes_13"></a>Verze v listopadu 2013
### <a name="nov_13_donnet_changes"></a>Azure Media Services .NET SDK změny
Od této verze, sady Media Services SDK pro .NET zpracovává přechodná chyba chyby, které mohou nastat při volání do vrstvy Media Services REST API.

## <a id="august_changes_13"></a>Verze srpen 2013
### <a name="aug_13_powershell_changes"></a>Rutiny prostředí PowerShell služby média součástí nástroje Azure SDK
Následující rutiny prostředí PowerShell Media Services jsou teď součástí [nástroje azure sdk].

* Get-AzureMediaServices 
  
    Například, `Get-AzureMediaServicesAccount`.
* New-AzureMediaServicesAccount 
  
    Například, `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`.
* New-AzureMediaServicesKey 
  
    Například, `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`.
* Remove-AzureMediaServicesAccount 
  
    Například, `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`.

## <a id="june_changes_13"></a>Červen 2013 verze
### <a name="june_13_general_changes"></a>Změní Azure Media Services
Změny v této části jsou aktualizace součástí vydání červen 2013 Media Services.

* Možnost propojit víc účtů úložiště na účet Media Service. 
  
    StorageAccount
  
    Asset.StorageAccountName a Asset.StorageAccount
* Umožňuje aktualizovat Job.Priority. 
* Oznámení související entity a vlastnosti: 
  
    JobNotificationSubscription
  
    NotificationEndPoint
  
    Úloha
* Asset.Uri 
* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Změní sadu Azure Media Services .NET SDK
Následující změny jsou zahrnuty v červen 2013 uvolní sada SDK služby Media Services. Nejnovější sady Media Services SDK je dostupná na Githubu.

* Počínaje verzí 2.3.0.0, sada SDK služby Media Services podporuje, propojování více úložiště účtů k účtu Media Services. Tuto funkci podporovat následující rozhraní API:
  
    Typ IStorageAccount.
  
    Vlastnost Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts.
  
    Vlastnost, která StorageAccount.
  
    Vlastnost StorageAccountName.
  
    Další informace najdete v tématu [Správa Media Services prostředky napříč více účtů úložiště].
* Rozhraní API související s oznámení. Počínaje verzí 2.2.0.0, že máte možnost pro naslouchání na Azure Queue storage oznámení. Další informace najdete v tématu, [zpracování Media Services úlohy oznámení].
  
    Vlastnost Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions.
  
    Typ Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint.
  
    Typ Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription.
  
    Typ Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection.
  
    Typ Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType.
  
    Typ Microsoft.WindowsAzure.MediaServices.Client.NotificationJobState.
* Závislost na klienta úložiště Azure SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll).
* Závislost na protokolu OData 5.5 (Microsoft.Data.OData.dll).

## <a id="december_changes_12"></a>Verze 2012 prosinec
### <a name="dec_12_dotnet_changes"></a>Změní sadu Azure Media Services .NET SDK
* IntelliSense: Přidat chybějící dokumentace technologie Intellisense pro mnoho typů.
* Microsoft.Practices.TransientFaultHandling.Core: Opraven problém, kde sadu SDK stále byl závislý na původní verzi tohoto sestavení. Sada SDK nyní odkazuje 5.1.1209.1 verzi tohoto sestavení.

Opravy pro problémy zjištěné v listopad 2012 SDK:

* IAsset.Locators.Count: Tento počet je nyní správně hlášené na nové rozhraní IAsset po odstranění všech lokátory.
* IAssetFile.ContentFileSize: Tato hodnota je nyní správně nastavená po nahrávaný podle IAssetFile.Upload (filepath).
* IAssetFile.ContentFileSize: Tuto vlastnost lze nyní nastavit při vytváření souboru asset. Bylo dříve jen pro čtení.
* IAssetFile.Upload (filepath): opraven problém, kde byla tato metoda synchronní nahrávání vyvolání k následující chybě při nahrávání více souborů pro daný prostředek. Došlo k chybě "serveru se nepodařilo ověřit žádost. Ujistěte se, že hodnotu hlavičky autorizace je vytvořen. správně včetně podpis. "
* IAssetFile.UploadAsync: Opraven problém, kde může současně uložit více než pět souborů.
* IAssetFile.UploadProgressChanged: Tato událost se teď poskytují prostřednictvím sady SDK.
* IAssetFile.DownloadAsync (string, BlobTransferClient, ILocator, CancellationToken): Toto přetížení metody je nyní k dispozici.
* IAssetFile.DownloadAsync: Opraven problém, kde může současně stáhnout více než pět souborů.
* IAssetFile.Delete(): Opraven problém, kde volání odstranění může vyvolat výjimku pro IAssetFile nebyl nahrán žádný soubor.
* Úlohy: Byl opraven problém kde řetězení "MP4 funkce Smooth Streams úloha" s "PlayReady ochrany úlohu" pomocí šablony úlohy by vytvořit všechny úlohy vůbec.
* EncryptionUtils.GetCertificateFromStore(): Tato metoda vyvolá už výjimka odkazu s hodnotou null z důvodu chyby hledání certifikát založený na problémy s konfigurací certifikátu.

## <a id="november_changes_12"></a>Verze listopad 2012
Změny v této části jsou aktualizace součástí listopad 2012 (verze 2.0.0.0) sady SDK. Tyto změny můžou vyžadovat žádné kód napsaný v verzi Preview. června 2012 upravena nebo přepsaná verzi sady SDK.

* Prostředky
  
    IAsset.Create(assetName) je funkce pouze asset vytvoření. IAsset.Create už ukládání souborů v rámci volání metody. Použijte IAssetFile pro odesílání.
  
    Metoda IAsset.Publish a hodnota výčtu AssetState.Publish byly odebrány ze sady SDK služby. Je nutné přepsat kód, který závisí na této hodnotě.
* FileInfo
  
    Tato třída je odebrána a nahrazena IAssetFile.
  
    IAssetFiles
  
    IAssetFile nahrazuje FileInfo a s jiným chováním. Pokud chcete použít, vytvořte instanci objektu IAssetFiles, za nímž následuje nahrání souboru buď pomocí sady Media Services SDK nebo sady SDK úložiště Azure. Můžete použít následující přetížení IAssetFile.Upload:
  
  * IAssetFile.Upload(filePath): Synchronní metodu, která blokuje vlákno, které se doporučuje jenom v případě, že odesílání jeden soubor.
  * IAssetFile.UploadAsync (cesta k souboru, blobTransferClient, Lokátor, cancellationToken): asynchronní metodu. Toto je upřednostňovaný nahrávání mechanismus. 
    
    Známého problému: pomocí token zrušení bude skutečně zrušit odesílání; stav zrušení úloh však může být z mnoha stavů. Musíte správně catch a zpracování výjimek.
* Lokátory
  
    Byly odebrány specifické pro původní verze. Kontext specifické pro SAS. Zastaralé, nebo budou odebrány podle Všeobecné budou označeny Locators.CreateSasLocator (asset, accessPolicy) Najdete v části lokátory pod nové funkce pro aktualizovaný chování.

## <a id="june_changes_12"></a>Verze Preview června 2012
Následující funkce se nové v listopadu verzi sady SDK.

* Odstraňování entit
  
    IAsset, IAssetFile, ILocator, IAccessPolicy, IContentKey objekty jsou nyní odstraněny na úrovni objektu, který je IObject.Delete(), místo aby odstranění v kolekci, která je cloudMediaContext.ObjCollection.Delete(objInstance).
* Lokátory
  
    Lokátory musí být nyní vytvořen pomocí metody CreateLocator a použít hodnoty výčtu LocatorType.SAS nebo LocatorType.OnDemandOrigin jako argument pro konkrétní typ lokátoru, že chcete vytvořit.
  
    Nové vlastnosti byly přidány do lokátory, aby bylo snazší získat použitelné identifikátory URI pro obsah. Tato změna návrhu lokátorů byla určená k poskytování větší flexibilitu pro budoucí rozšíření třetích stran a zvýšit snadné použití pro média klientské aplikace.
* Podpora asynchronní metody
  
    Byla přidána asynchronní podpora pro všechny metody.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[fóru služby Azure Media Services MSDN]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Azure Media Services REST API – referenční informace]: https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference
[podrobnosti o cenách na Media Services]: http://azure.microsoft.com/pricing/details/media-services/
[vstupu Metadata]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[výstup metadat]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[Delivering Content]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[indexování mediálních souborů pomocí Azure Media Indexer]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[práci s Azure Media Services živým streamováním]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[pomocí dynamického šifrování AES-128 a služba pro přenos klíče]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[pomocí dynamického šifrování PlayReady a službu doručování licencí]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Media Services PlayReady licence šablony přehled]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[obsahu šifrované streamování úložiště]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure portal]: https://portal.azure.com
[dynamické balení]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Nick Drouin Blog]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[datový proud Smooth Protecting s technologií PlayReady]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[opakujte logiky sady Media Services SDK pro .NET]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[tráva Valley ohlášen EDIUS 7 vysílání datového proudu prostřednictvím the Cloud]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[řízení Media Service kodér výstup názvy souborů]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[vytváření překryvy]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[ve hřbetu segmenty Video]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[uvolní Azure Media Services .NET SDK 3.0.0.1 a 3.0.0.2]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Active Directory řízení přístupu Azure Service (ACS)]: http://msdn.microsoft.com/library/hh147631.aspx
[připojení ke službám Media Services pomocí sady Media Services SDK pro .NET]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[rozšíření Azure Media Services .NET SDK]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[nástroje azure sdk]: https://github.com/Azure/azure-sdk-tools
[Githubu]: https://github.com/Azure/azure-sdk-for-media-services
[Správa Media Services prostředky napříč více účtů úložiště]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[zpracování Media Services úlohy oznámení]: http://msdn.microsoft.com/library/azure/dn261241.aspx

