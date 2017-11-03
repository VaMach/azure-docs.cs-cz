---
title: "Konfigurace obsahu zásad autorizace pro klíč pomocí portálu Azure | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat zásady autorizace pro klíč obsahu."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ee82a3fa-c34b-48f2-a108-8ba321f1691e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 5a35c7255a1c30a693862589c14f6a22a1900790
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configure-content-key-authorization-policy"></a>Nakonfigurujte zásady autorizace klíče obsahu
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Přehled
Microsoft Azure Media Services umožňuje doručovat datové proudy MPEG-DASH, technologie Smooth Streaming a HTTP-Live-Streaming (HLS) chráněné pomocí Standard AES (Advanced Encryption) (pomocí klíče 128bitové šifrování) nebo [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/). AMS také umožňuje doručovat datové proudy DASH šifrované pomocí Widevine DRM. Technologie PlayReady a Widevine jsou šifrované podle specifikace Common Encryption (ISO/IEC CENC 23001-7).

Služba Media Services také poskytuje **klíč nebo licenční služby doručení** klienty, kteří mohou získat klíče AES nebo licence PlayReady nebo Widevine přehrávání šifrovaný obsah.

Toto téma ukazuje, jak pomocí portálu Azure a nakonfigurujte zásady autorizace klíče obsahu. Klíč lze později dynamicky šifrovat obsah. Všimněte si, že teď můžete šifrovat následující streamování formáty: HLS, MPEG DASH a technologie Smooth Streaming. Nelze zašifrovat progresivní stahování.

Když přehrávač vyžádá datový proud, který je nastaven na dynamicky šifrovat, používá služba Media Services nakonfigurovaný klíč dynamicky šifrovat obsah s šifrováním AES nebo DRM. K dešifrování datového proudu, bude přehrávač požadovat klíč ze služby doručení klíče. Při rozhodování, zda je uživatel oprávnění k získání klíče, služba vyhodnocuje zásady autorizace, které jste zadali pro klíč.

Pokud budete chtít mít několik klíčů obsah nebo chcete zadat **klíč nebo licenční služby doručení** URL než doručení klíče služby Media Services, pomocí sady Media Services .NET SDK nebo rozhraní REST API.

[Konfigurace obsahu zásad autorizace pro klíč pomocí sady Media Services .NET SDK](media-services-dotnet-configure-content-key-auth-policy.md)

[Konfigurace obsahu zásad autorizace pro klíč pomocí Media Services REST API](media-services-rest-configure-content-key-auth-policy.md)

### <a name="some-considerations-apply"></a>Musí být splněny určité předpoklady:
* Při vytvoření účtu AMS **výchozí** koncový bod streamování je přidána k vašemu účtu v **Zastaveno** stavu. Pokud chcete spustit streamování vašeho obsahu a využít výhod dynamického balení a dynamické šifrování, musí být v koncovém bodu streamování **systémem** stavu. 
* Váš asset musí obsahovat sadu s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí. Další informace najdete v tématu [kódovat asset](media-services-encode-asset.md).
* Službu doručování klíč ukládá do mezipaměti ContentKeyAuthorizationPolicy a související objekty (Možnosti zásad a omezení) pro 15 minut.  Pokud vytvoříte ContentKeyAuthorizationPolicy a nastavení, aby používal "Token" omezení, pak otestovat ji a potom aktualizovat zásady "Otevřené" omezení, bude trvat přibližně 15 minut, než zásady přepne do "Otevřené" verze zásad.

## <a name="how-to-configure-the-key-authorization-policy"></a>Postupy: Konfigurace zásad autorizace pro klíč
Konfigurace zásad autorizace pro klíč, vyberte **ochrana obsahu** stránky.

Služba Media Services podporuje více způsobů ověřování uživatelů, kteří žádají o klíč. Zásady autorizace klíče obsahu může mít **otevřete**, **tokenu**, nebo **IP** autorizace omezení (**IP** se dá nakonfigurovat s REST nebo .NET SDK).

### <a name="open-restriction"></a>Otevřete omezení
**Otevřete** omezení znamená systému dodá klíč všem uživatelům, kteří vytváří klíče požadavek. Toto omezení může být užitečná pro účely testování.

![Funkce OpenPolicy][open_policy]

### <a name="token-restriction"></a>Omezení s tokenem
Chcete-li vybrat zásady omezení tokenem, stiskněte **TOKENU** tlačítko.

**Tokenu** zásady omezení musí být doplněny tokenem vydaným podle **služby tokenů zabezpečení** (STS). Služba Media Services podporuje tokeny ve **jednoduchých webových tokenů** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) formátu a **webových tokenů JSON** formátu (JWT). Informace najdete v tématu [ověření pomocí tokenu JWT](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Služba Media Services neposkytuje **zabezpečení tokenu služby**. Můžete vytvořit vlastní službu tokenů zabezpečení nebo využívat Microsoft Azure ACS problém tokeny. Služba tokenů zabezpečení musí být nakonfigurované vytvořit token podepsané zadaný klíč a vystavování deklarací identity, které jste zadali v nastavení omezení s tokenem. Doručení klíče služby Media Services bude vrácena klientovi šifrovací klíč, pokud token je platný a deklarace identity v tokenu shodují s těmi, nakonfigurované pro klíč k obsahu. Další informace najdete v tématu [použití Azure ACS problém tokeny](http://mingfeiy.com/acs-with-key-services).

Při konfiguraci **TOKENU** zásady omezení, musíte nastavit hodnoty pro **ověřovací klíč**, **vystavitele** a **cílovou skupinu**. Ověření primární klíč obsahuje klíč, který byl podepsaný token, Vystavitel je zabezpečený tokenu služba, která vydá token. Cílová skupina (někdy nazývané oboru) popisuje záměr tokenu nebo prostředek token povolí přístup k. Služba Media Services doručení klíče ověří, jestli tyto hodnoty v tokenu shodují s hodnotami v šabloně.

### <a name="playready"></a>PlayReady
Při ochraně svůj obsah pomocí **PlayReady**, jednou z věcí, je třeba zadat ve vaší zásady autorizace je řetězec XML, který definuje šablona licence PlayReady. Ve výchozím nastavení je nastavit tyto zásady:

<PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1"><LicenseTemplates> <PlayReadyLicenseTemplate> <AllowTestDevices>true</AllowTestDevices> <ContentKey i:type="ContentEncryptionKeyFromHeader" /> <LicenseType>Nonpersistent</LicenseType> <PlayRight> <AllowPassingVideoContentToUnknownOutput>povolené</AllowPassingVideoContentToUnknownOutput> </PlayRight> </PlayReadyLicenseTemplate> </LicenseTemplates></PlayReadyLicenseResponseTemplate>

Můžete kliknout na **importovat zásady xml** tlačítko a zadejte jiný jazyk XML, který vyhovuje na XML schéma definované [zde](media-services-playready-license-template-overview.md).

## <a name="next-step"></a>Další krok
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

