---
title: "Chránit obsah pomocí služby Azure Media Services | Microsoft Docs"
description: "Tento článek poskytuje přehled toho Ochrana obsahu pomocí služby Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 81bc00e1-dcda-4d69-b9ab-8768b793422b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 64be4ea104bd11b8e191e2c8d4170a2de88acb47
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="protecting-content-overview"></a>Ochrana obsahu – přehled
Microsoft Azure Media Services umožňuje zabezpečení médií od okamžiku opuštění počítače přes uložení a zpracování až po doručení. Služba Media Services umožňuje doručovat obsah za provozu a na vyžádání dynamicky šifrován Standard AES (Advanced Encryption) (pomocí klíče 128bitové šifrování) nebo některého z hlavní technologiemi DRM: Microsoft PlayReady, Google Widevine a Apple FairPlay. Služba Media Services také poskytuje službu k doručování klíčů AES a DRM (PlayReady, Widevine a FairPlay) licence autorizovaným klientům. 

Následující obrázek ukazuje pracovní postupy ochrana obsahu, které podporuje AMS. 

![Ochrana technologií PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[!NOTE]
>Po vytvoření účtu AMS se do vašeho účtu přidá **výchozí** koncový bod streamování ve stavu **Zastaveno**. Pokud chcete spustit streamování vašeho obsahu a využít výhod dynamického balení a dynamického šifrování, musí koncový bod streamování, ze kterého chcete streamovat obsah, být ve stavu **Spuštěno**. 

Toto téma vysvětluje [principy a terminologií](media-services-content-protection-overview.md) relevantní k pochopení ochrana obsahu pomocí služby AMS. Téma obsahuje také [odkazy](media-services-content-protection-overview.md#common-scenarios) na témata, která ukazují, jak dosáhnout úlohy ochrany obsahu. 

## <a name="dynamic-encryption"></a>Dynamické šifrování
Microsoft Azure Media Services umožňuje doručovat obsah dynamicky šifrovaných pomocí nezašifrovaného klíče AES nebo DRM šifrování: Microsoft PlayReady, Google Widevine a Apple FairPlay.

V současné době můžete šifrovat formátech streamování: HLS, MPEG DASH a technologie Smooth Streaming. Nelze zašifrovat progresivní stahování.

Pokud chcete použít pro Media Services k šifrování prostředek, budete muset přidružit asset šifrovací klíč (CommonEncryption nebo EnvelopeEncryption) a taky nakonfigurovat zásady autorizace pro klíč.

Také musíte nakonfigurovat zásady doručení assetu. Pokud chcete asset šifrované úložiště datového proudu, nezapomeňte určit, jak chcete poskytovaným konfigurace zásad doručení assetu.

Datový proud je žádost přehrávač, Media Services používá k zadanému klíči dynamicky šifrovat obsah pomocí nezašifrovaného klíče AES nebo DRM šifrování. K dešifrování datového proudu, bude přehrávač požadovat klíč ze služby doručení klíče. Při rozhodování, zda je uživatel oprávnění k získání klíče, služba vyhodnocuje zásady autorizace, které jste zadali pro klíč.


## <a name="storage-encryption"></a>Šifrování úložiště
Šifrování úložiště použijte k šifrování vašeho nešifrovaného obsahu pomocí 256bitového šifrování AES 256 a nahrajte ho do Azure Storage kde bude uložený v zašifrované podobě. Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování umístěny do systému souborů EFS před kódování a volitelně se znovu zašifrují před jejich odesláním zpět v podobě nového výstupního prostředku. Případem primárního použití šifrování úložiště je, když chcete zabezpečit vysoké kvality souborů vstupními médii pomocí silného šifrování v klidovém stavu na disku.

Aby bylo možné poskytovat asset šifrované úložiště, musíte nakonfigurovat zásady doručení assetu, aby věděl Media Services může způsob doručení obsahu. Před asset Streamovat, server datových proudů odebere šifrování úložiště a datové proudy svůj obsah pomocí zadaného doručování zásad (například AES, běžným šifrováním nebo žádné šifrování).

## <a name="common-encryption-cenc"></a>Běžné šifrování (CENC)
Běžné šifrování se používá při šifrování svůj obsah pomocí PlayReady nebo / a Widewine.

## <a name="using-cbcs-aapl-encryption"></a>Pomocí šifrování na cbcs-aapl
Cbcs-aapl se používá při šifrování svůj obsah pomocí FairPlay.

## <a name="envelope-encryption"></a>Šifrování obálky
Tuto možnost použijte, pokud chcete chránit obsah pomocí nezašifrovaný klíč AES-128. Pokud chcete bezpečnější možnost, vyberte jednu z technologiemi DRM uvedené v tomto tématu. 

## <a name="licenses-and-keys-delivery-service"></a>Službu doručování licencí a klíče
Služba Media Services poskytuje službu k doručování licencí DRM (technologie PlayReady, Widevine, FairPlay) a AES zrušte klíče autorizovaným klientům. Můžete použít [portálu Azure](media-services-portal-protect-content.md), REST API nebo sady Media Services SDK pro .NET, můžete nakonfigurovat zásady ověřování a ověřování licencí a klíčů.

## <a name="token-restriction"></a>Omezení s tokenem
Zásady autorizace pro klíč k obsahu mohou obsahovat jedno nebo více omezení autorizace: otevřené omezení nebo omezení s tokenem. Zásady omezení tokenem musí být doplněny tokenem vydaným službou tokenů zabezpečení (STS). Služba Media Services podporuje tokeny ve formátu jednoduché webové tokeny (SWT) a formátu JSON Web Token (JWT). Služba Media Services neposkytuje zabezpečení tokenu služby. Můžete vytvořit vlastní službu tokenů zabezpečení nebo využívat Microsoft Azure ACS problém tokeny. Služba tokenů zabezpečení musí být nakonfigurované vytvořit token podepsané zadaný klíč a vystavování deklarací identity, které jste zadali v nastavení omezení s tokenem. Služba Media Services doručení klíče vrátí požadovaný klíč (nebo licencí) do klienta, pokud je token platný a deklarace identity v tokenu shody ty nakonfigurované pro klíč (nebo licencí).

Při konfiguraci token omezený zásad, musíte zadat klíč primární ověřování, vystavitele a cílová skupina parametry. Ověření primární klíč obsahuje klíč, který byl podepsaný token, Vystavitel je zabezpečený tokenu služba, která vydá token. Cílová skupina (někdy nazývané oboru) popisuje záměr tokenu nebo prostředek token povolí přístup k. Služba Media Services doručení klíče ověří, jestli tyto hodnoty v tokenu shodují s hodnotami v šabloně.

## <a name="streaming-urls"></a>Adresy URL streamování
Pokud váš asset byla zašifrována pomocí více než jeden DRM, ve adresu URL streamování byste měli používat značky šifrování: (formát = 'm3u8-aapl' šifrování = 'xxx').

Platí následující aspekty:

* Lze zadat pouze nula nebo jeden typ šifrování.
* Typ šifrování nemusí být zadaného v adrese url, pokud jenom jeden šifrování byla použita pro daný prostředek.
* Typ šifrování je malá a velká písmena.
* Určit lze následující typy šifrování:  
  * **šifrování cenc**: Common encryption (Playready nebo Widevine)
  * **cbcs-aapl**: Fairplay
  * **CBC**: obálky šifrování AES.

## <a name="common-scenarios"></a>Obvyklé scénáře
Následující témata ukazují, jak chránit obsah v úložišti, doručování dynamicky šifrovaných streamovaných médií, použijte doručení klíč nebo licenční služby AMS

* [Ochrana pomocí standardu AES](media-services-protect-with-aes128.md) 
* [Chránit pomocí PlayReady nebo Widevine](media-services-protect-with-drm.md)
* [Stream obsahu chráněného vaše HLS Apple FairPlay a/nebo PlayReady](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a>Další scénáře
* [Postup pro integraci služby Azure licence PlayReady modulu pro šifrování nebo streamování serveru](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).
* [Použití castLabs k doručování licencí DRM do služby Azure Media Services](media-services-castlabs-integration.md)

>[!NOTE]
>Scénář, ve kterém můžete používat externí DRM server(technology) a datový proud z AMS není aktuálně podporováno.


## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Související odkazy
[Uvedení PlayReady jako dynamické šifrování AES pomocí Azure Media Services a služby](http://mingfeiy.com/playready)

[Vysvětlení Azure ceny doručování licencí Media Services PlayReady](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Postup ladění pro šifrované datový proud AES ve službě Azure Media Services](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[Authenitcation tokenu JWT](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integrace aplikace Azure Media Services OWIN MVC se službou Azure Active Directory a omezit klíče doručování obsahu na základě deklarací JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Pomocí Azure ACS problém tokeny](http://mingfeiy.com/acs-with-key-services).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
