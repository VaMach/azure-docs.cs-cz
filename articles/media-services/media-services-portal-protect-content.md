---
title: "Konfigurace zásad ochrany obsahu pomocí portálu Azure | Microsoft Docs"
description: "Tento článek ukazuje, jak pomocí portálu Azure ke konfiguraci zásad ochrany obsahu. Článek také ukazuje, jak povolit dynamické šifrování pro vaše prostředky."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 270b3272-7411-40a9-ad42-5acdbba31154
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: ecc766abb5df38813b3eb6dde98cdc9afd24ac6b
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="configuring-content-protection-policies-using-the-azure-portal"></a>Konfigurace zásad ochrany obsahu pomocí portálu Azure
Microsoft Azure Media Services (AMS) umožňuje zabezpečit médiu od okamžiku, kdy by poté počítač prostřednictvím úložiště, zpracování a doručení. Služba Media Services umožňuje doručovat obsah šifrované dynamicky s Standard AES (Advanced Encryption) (pomocí klíče 128bitové šifrování), pomocí PlayReady nebo Widevine DRM a Apple FairPlay běžné šifrování (CENC). 

AMS poskytuje službu k doručování licencí DRM a AES zrušte klíče autorizovaným klientům. Na portálu Azure můžete vytvořit jeden **zásady autorizace pro klíč nebo licenci** pro všechny typy šifrování.

Tento článek ukazuje, jak nakonfigurovat zásadu ochrany obsahu pomocí portálu Azure. Článek také ukazuje, jak chcete použít dynamické šifrování pro vaše prostředky.

## <a name="start-configuring-content-protection"></a>Zahájit konfiguraci ochrany obsahu
Pomocí portálu pro spuštění konfigurace ochrany obsahu, globální vašeho účtu AMS, postupujte takto:
1. Na webu [Azure Portal](https://portal.azure.com/) zvolte účet Azure Media Services.
2. Vyberte **nastavení** > **obsahu ochrany**.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a>Zásady autorizace pro klíč nebo licencí
AMS podporuje více způsobů ověřování uživatelů, kteří žádají o klíč nebo licencí. Zásady autorizace klíče obsahu musíte nakonfigurovat a splní vašeho klienta v pořadí pro klíč nebo licenci k být delived do klienta. Zásady autorizace klíče obsahu může mít jeden nebo více omezení autorizace: **otevřete** nebo **tokenu** omezení.

Na portálu Azure můžete vytvořit jeden **zásady autorizace pro klíč nebo licenci** pro všechny typy šifrování.

### <a name="open-authorization"></a>Otevřete autorizace
Otevřete omezení znamená, že systém získávat klíč všem uživatelům, kteří vytváří klíče požadavek. Toto omezení může být užitečná pro účely testování. 

### <a name="token-authorization"></a>Token autorizace
Zásady omezení tokenem musí být doplněny tokenem vydaným službou tokenů zabezpečení (STS). Služba Media Services podporuje tokeny ve formátu jednoduché webové tokeny (SWT) a formátu JSON Web Token (JWT). Služba Media Services neposkytuje zabezpečení tokenu služby. Můžete vytvořit vlastní službu tokenů zabezpečení nebo využívat Microsoft Azure ACS problém tokeny. Služba tokenů zabezpečení musí být nakonfigurované vytvořit token podepsané zadaný klíč a vystavování deklarací identity, které jste zadali v nastavení omezení s tokenem. Služba Media Services doručení klíče vrátí požadovaný klíč (nebo licencí) do klienta, pokud je token platný a deklarace identity v tokenu shody ty nakonfigurované pro klíč (nebo licencí).

Při konfiguraci token omezený zásad, musíte zadat ověření primární klíč, vystavitele a cílová skupina parametry. Ověření primární klíč obsahuje klíč, který byl podepsaný token, Vystavitel je zabezpečený tokenu služba, která vydá token. Cílová skupina (někdy nazývané oboru) popisuje záměr tokenu nebo prostředek token povolí přístup k. Služba Media Services doručení klíče ověří, jestli tyto hodnoty v tokenu shodují s hodnotami v šabloně.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-license-template"></a>Šablona licence PlayReady
Šablona licence PlayReady nastaví funkci na licence PlayReady povolené. Podrobné informace o šabloně licence PlayReady najdete v tématu [Media Services PlayReady licence šablony přehled](media-services-playready-license-template-overview.md).

### <a name="non-persistent"></a>Bez trvalé
Pokud nakonfigurujete jako dočasnou licenci, pouze udržované ve paměti při přehrávač používá licence.  

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Trvalé
Pokud konfigurujete licence jako trvalé, se uloží do trvalého úložiště na straně klienta.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-license-template"></a>Šablona licence Widevine
Šablona licence Widevine nastaví funkci zapnuta licence na Widevine.

### <a name="basic"></a>Basic
Když vyberete **základní**, vytvoří se šablony s všechny výchozí hodnoty.

### <a name="advanced"></a>Advanced
Podrobné informace o šabloně rights Widevine, najdete v části [přehled šablonu licence Widevine](media-services-widevine-license-template-overview.md).

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>Konfigurace FairPlay
Pokud chcete povolit šifrování FairPlay, potřebujete poskytovat certifikát aplikace a aplikace tajný klíč (požádejte) prostřednictvím možnost FairPlay konfigurace. Podrobné informace o konfiguraci FairPlay a požadavky najdete v tématu [to](media-services-protect-hls-with-FairPlay.md) článku.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Použít na asset dynamické šifrování
Abyste mohli využívat dynamické šifrování, budete muset zakódovat váš zdrojový soubor do sady souborů MP4 s adaptivní přenosovou rychlostí.

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Vyberte asset, který chcete zašifrovat
Pokud chcete zobrazit všechny prostředky, vyberte **nastavení** > **prostředky**.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Šifrování AES nebo DRM
Po stisknutí klávesy **šifrovat** na prostředek, se zobrazí dvě možnosti: **AES** nebo **DRM**. 

#### <a name="aes"></a>AES
AES zrušte Povolí šifrování pomocí klíče na všechny protokoly streamování: technologie Smooth Streaming, HLS a MPEG-DASH.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM
Když vyberete kartu DRM, budete mít různé možnosti zásad ochrany obsahu (který musíte nakonfigurovat nyní) + sadu protokolů streamování.

* **PlayReady a Widevine s MPEG-DASH** -bude dynamicky šifrovat MPEG-DASH datového proudu s technologií PlayReady a Widevine technologiemi DRM.
* **PlayReady a Widevine s MPEG-DASH + FairPlay s HLS** -bude dynamicky šifrovat stream MPEG-DASH s technologií PlayReady a Widevine technologiemi DRM. Bude se šifrovat taky vaše datové proudy HLS s FairPlay.
* **PlayReady pouze s technologie Smooth Streaming, HLS a MPEG-DASH** -bude dynamicky šifrovat technologie Smooth Streaming, HLS, datové proudy MPEG-DASH s technologií PlayReady DRM.
* **Widevine pouze s MPEG-DASH** -bude dynamicky šifrovat můžete MPEG-DASH pomocí Widevine DRM.
* **FairPlay pouze s HLS** -bude dynamicky šifrovat HLS datového proudu s FairPlay.

Chcete-li povolit šifrování FairPlay, poskytují certifikát aplikace a aplikace tajný klíč (požádejte) prostřednictvím možnosti konfigurace FairPlay obrazovky okna nastavení ochrany obsahu.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection009.png)

Až provedete výběr šifrování, stiskněte klávesu **použít**.

>[!NOTE] 
>Pokud máte v úmyslu přehrání AES šifrovat HLS v prohlížeči Safari najdete [šifrované HLS v prohlížeči Safari příspěvku na blogu](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Další kroky
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

