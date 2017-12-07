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
ms.openlocfilehash: 6475a865b9d1b263bd7cc68c99acdb5f6959531e
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="protecting-content-overview"></a>Ochrana obsahu – přehled
Microsoft Azure Media Services umožňuje zabezpečení médií od okamžiku opuštění počítače přes uložení a zpracování až po doručení. Media Services umožňuje doručovat živě i na vyžádání obsah dynamicky šifrován Advanced Encryption Standard (AES-128) nebo všechny tři hlavní DRM systémů: Microsoft PlayReady, Google Widevine a Apple FairPlay. Služba Media Services také poskytuje službu k doručování klíčů AES a DRM (PlayReady, Widevine a FairPlay) licence autorizovaným klientům. 

Následující obrázek ukazuje pracovní postup ochrany obsahu Azure Media Services. 

![Ochrana technologií PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

Tento článek vysvětluje principy a terminologií, které jsou relevantní pro pochopení ochrana obsahu pomocí služby AMS. Tento článek také poskytuje odkazy na články, které popisují postup chránit obsah. 

## <a name="dynamic-encryption"></a>Dynamické šifrování
Azure Media Services umožňuje doručovat obsah dynamicky šifrovaných pomocí nezašifrovaného klíče AES nebo DRM šifrování: Microsoft PlayReady, Google Widevine a Apple FairPlay. V současné době můžete šifrovat formátech streamování: HLS, MPEG DASH a technologie Smooth Streaming. Šifrování na progresivní stahování není podporováno. Každá metoda šifrování podporuje následující protokoly pro streamování:
- AES: MPEG-DASH, Smooth Streaming a HLS
- PlayReady: MPEG-DASH, Smooth Streaming a HLS
- Widevine: MPEG-DASH
- FairPlay: HLS

K šifrování prostředek, musíte přidružit asset šifrovací klíč obsahu a taky konfigurace zásad autorizace pro klíč. Obsahu klíčů může být zadaný nebo automaticky generuje služba Media Services.

Také musíte nakonfigurovat zásady doručení assetu. Pokud chcete k vysílání datového proudu šifrované asset úložiště, zajistěte, aby k určení, jak chcete poskytovaným konfigurace zásad doručení assetu.

Datový proud je žádost přehrávač, Media Services používá k zadanému klíči dynamicky šifrovat obsah pomocí nezašifrovaného klíče AES nebo DRM šifrování. Přehrávač dešifrovat datový proud, požadavků klíč ze služby AMS doručení klíče. Při rozhodování, zda je uživatel oprávnění k získání klíče, služba vyhodnocuje zásady autorizace, které jste zadali pro klíč.

## <a name="aes-128-clear-key-vs-drm"></a>Nezašifrovaný klíč AES-128 vs DRM
Zákazníci často zajímat, jestli by měl použít systém DRM nebo šifrování AES. Hlavní rozdíl mezi pomocí šifrování AES a systémy DRM je, že s šifrováním AES klíč obsahu přenášená do klienta v nezašifrované podobě ("v Vymazat"). V důsledku toho klíč používaný k šifrování obsahu lze zobrazit v trasování v síti na straně klienta v podobě prostého textu. AES-128 nezašifrovaný klíč je vhodná pro použití případech, kdy prohlížeč je důvěryhodná strana (např: šifrování podnikové videa distribuované v rámci společnosti prohlížení zaměstnanci).

Technologie PlayReady, Widevine a FairPlay všechny poskytovat vyšší úroveň porovnání šifrování AES-128 zrušte šifrovacího klíče. Klíč obsahu se přenášejí v zašifrovaném formátu. Kromě toho dešifrování je popisovač v zabezpečeném prostředí na úrovni operačního systému kde je podstatně obtížnější pro uživatel se zlými úmysly vůči útokům. DRM se doporučuje pro použití případy, kdy prohlížeč pravděpodobně nebude důvěryhodná strana a vyžadujete nejvyšší úroveň zabezpečení.

## <a name="storage-encryption"></a>Šifrování úložiště
Můžete použít šifrování úložiště k šifrování vašeho nešifrovaného obsahu pomocí 256bitového šifrování AES 256 a nahrajte ho do Azure Storage kde bude uložený v zašifrované podobě. Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování umístěny do systému souborů EFS před kódování a volitelně se znovu zašifrují před jejich odesláním zpět v podobě nového výstupního prostředku. Případem primárního použití šifrování úložiště je, když chcete zabezpečit soubory vysoce kvalitními vstupními médii pomocí silného šifrování v klidovém stavu na disku.

Aby bylo možné poskytovat asset šifrované úložiště, musíte nakonfigurovat zásady doručení assetu, aby věděl Media Services může způsob doručení obsahu. Před asset Streamovat, streamování serveru dešifruje a datové proudy svůj obsah pomocí zadaného doručování zásad (například AES, běžným šifrováním nebo žádné šifrování).

## <a name="types-of-encryption"></a>Typy šifrování
PlayReady a Widevine využívat běžné šifrování (AES PEV.cenu režim). FairPlay využívá šifrování AES CBC režimu. Zrušte klíče šifrování AES-128 využívá šifrování obálku.

## <a name="licenses-and-keys-delivery-service"></a>Službu doručování licencí a klíče
Služba Media Services poskytuje doručení klíče službu k doručování licencí DRM (PlayReady, Widevine, FairPlay) a klíčů AES autorizovaným klientům. Můžete použít [portálu Azure](media-services-portal-protect-content.md), REST API nebo sady Media Services SDK pro .NET, můžete nakonfigurovat zásady ověřování a ověřování licencí a klíčů.

## <a name="control-content-access"></a>Přístup k obsahu ovládacího prvku
Můžete řídit, kdo má přístup k vašemu obsahu tak, že nakonfigurujete zásady autorizace klíče obsahu. Zásady autorizace klíče obsahu podporuje buď otevřené nebo s tokenem omezení.

### <a name="open-authorization"></a>Otevřete autorizace
Zásad otevřete autorizace je odeslán klíč obsahu libovolnému klientovi (bez omezení).

### <a name="token-authorization"></a>Token autorizace
Pomocí zásad autorizace omezený token klíč k obsahu budou pouze odeslat klientovi, který představuje platný JSON Web Token (JWT) nebo jednoduchého webového tokenu (SWT) v žádosti o klíč nebo licenci. Tento token musí být vydaný podle zabezpečení tokenu služby (STS). Můžete použít službu Microsoft Active Directory jako služby tokenů zabezpečení nebo nasadit vlastní službu tokenů zabezpečení. Služba tokenů zabezpečení musí být nakonfigurované vytvořit token podepsané zadaný klíč a vystavování deklarací identity, které jste zadali v nastavení omezení s tokenem. Služba Media Services doručení klíče vrátí požadovaný klíč nebo licence klientovi Pokud token je platný a deklarace identity v tokenu shodují s těmi, nakonfigurované pro klíč nebo licence.

Při konfiguraci token omezený zásad, musíte zadat klíč primární ověřování, vystavitele a cílová skupina parametry. Ověření primární klíč obsahuje klíč, který byl podepsaný token, Vystavitel je zabezpečený tokenu služba, která vydá token. Cílovou skupinu, které se někdy označuje jako obor, popisuje záměr tokenu nebo prostředek token povolí přístup k. Služba Media Services doručení klíče ověří, jestli tyto hodnoty v tokenu shodují s hodnotami v šabloně.

## <a name="streaming-urls"></a>Adresy URL streamování
Pokud váš asset byla zašifrována pomocí více než jeden DRM, ve adresu URL streamování byste měli používat značky šifrování: (formát = 'm3u8-aapl' šifrování = 'xxx').

Platí následující aspekty:
* Lze zadat více než jeden typ šifrování.
* K zadanému v adrese URL, pokud jenom jeden šifrování byla použita pro daný prostředek nemá typ šifrování.
* Typ šifrování je malá a velká písmena.
* Určit lze následující typy šifrování:  
  * **šifrování cenc**: pro PlayReady nebo Widevine (Common Encryption)
  * **cbcs-aapl**: pro FairPlay (šifrování AES CBC)
  * **CBC**: pro šifrování AES envelope (obálky šifrování)

## <a name="next-steps"></a>Další kroky
Následující články popisují další kroky, jak začít s ochrana obsahu:
* [Chránit pomocí šifrování úložiště](media-services-rest-storage-encryption.md)
* [Chránit pomocí šifrování AES](media-services-protect-with-aes128.md)
* [Chránit pomocí PlayReady nebo Widevine](media-services-protect-with-playready-widevine.md)
* [Chránit pomocí FairPlay](media-services-protect-hls-with-FairPlay.md)

## <a name="related-links"></a>Související odkazy
[Vysvětlení Azure ceny doručování licencí Media Services PlayReady](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Postup ladění pro šifrované datový proud AES ve službě Azure Media Services](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[Ověření pomocí tokenu JWT](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Aplikace založené na Azure Media Services OWIN MVC integrovat Azure Active Directory a omezit klíče doručování obsahu na základě deklarací JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
