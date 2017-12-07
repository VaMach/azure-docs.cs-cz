---
title: "Předat ověřovací token do služby Azure Media Services | Microsoft Docs"
description: "Zjistěte, jak odeslat ověřovací tokeny z klienta do služby Azure Media Services doručení klíče"
services: media-services
keywords: "Ochrana obsahu, DRM, ověření pomocí tokenu"
documentationcenter: 
author: dbgeorge
manager: jasonsue
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: dwgeo
ms.openlocfilehash: 0e56726266898e5738dd797a8a019987d457170e
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="how-clients-pass-tokens-to-azure-media-services-key-delivery-service"></a>Jak klienti předat tokeny doručení klíče služby Azure Media Services
Neustále získáme otázky ohledně jak přehrávač může předávat tokenu našich služeb doručení klíče, které budou získat ověření a přehrávač získá klíč. Podporujeme jednoduchých webových tokenů (SWT) a tokenu pro webové JSON (JWT) tyto dva formáty tokenu. Ověření pomocí tokenu může použít k libovolnému typu klíč – bez ohledu na to dělají běžným šifrováním nebo AES envelope šifrování v systému.

Tyto jsou následujícími způsoby, které může předávat token s vaší player, závisí na player a platformu, kterou cílíte:
- Prostřednictvím protokolu HTTP autorizační hlavičky.
> [!NOTE]
> Všimněte si, že předpony "Nosiče" je očekáván podle specifikace OAuth 2.0. Je ukázka přehrávač s konfigurací Token hostované v Azure Media Player [ukázkové stránky](http://ampdemo.azureedge.net/). Vyberte prosím AES (JWT Token) nebo AES (SWT Token) nastavit zdroj videa. Token je předán prostřednictvím autorizační hlavičky.

- Prostřednictvím přidáním parametru Url dotazu s "tokenu = tokenvalue".  
> [!NOTE]
> Všimněte si, že je očekávána žádná předpona "Nosiče". Vzhledem k tomu, že token se budou odesílat prostřednictvím adresy URL, musíte se k obraně řetězec tokenu. Tady je ukázkový kód C# na postupujte následovně:

```csharp
    string armoredAuthToken = System.Web.HttpUtility.UrlEncode(authToken);
    string uriWithTokenParameter = string.Format("{0}&token={1}", keyDeliveryServiceUri.AbsoluteUri, armoredAuthToken);
    Uri keyDeliveryUrlWithTokenParameter = new Uri(uriWithTokenParameter);
```

- Prostřednictvím CustomData pole.
Pro PlayReady licence pořízení pouze přes pole CustomData výzvy získání licence PlayReady. V takovém případě musí být token uvnitř dokumentu xml popsané dole.

```xml
    <?xml version="1.0"?>
    <CustomData xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyCustomData/v1"> 
        <Token></Token> 
    </CustomData>
```
Uveďte ověřovací token do <Token> elementu.

- Pomocí alternativní stop HLS. Pokud potřebujete nakonfigurovat ověření pomocí tokenu pro AES + HLS přehrávání na iOS nebo Safari, není k dispozici způsob, jak může přímo odeslat v tokenu. Najdete v tématu to [příspěvku na blogu](http://azure.microsoft.com/blog/2015/03/06/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/) o tom, jak alternativní Chcete-li povolit tento scénář.

## <a name="next-steps"></a>Další kroky
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]