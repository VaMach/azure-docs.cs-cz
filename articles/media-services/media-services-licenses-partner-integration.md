---
title: "Pomocí partnery pro doručování licence na Widevine do služby Azure Media Services | Microsoft Docs"
description: "Tento článek popisuje, jak můžete použít Azure Media Services (AMS) k poskytování datový proud, který je dynamicky šifrovat pomocí PlayReady a Widevine technologiemi DRM AMS. Licence PlayReady pochází z Media Services PlayReady licenčního serveru a licence Widevine doručuje castLabs licenční server."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5bcad5a4-c0bb-4871-9cce-808a913c53e6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 6867e4f910970121df3858516c6bab3114c3c6f9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="using-partners-to-deliver-widevine-licenses-to-azure-media-services"></a>Využití služeb partnerů k distribuci licencí Widevine pro Azure Media Services
## <a name="overview"></a>Přehled
Microsoft Azure Media Services umožňuje doručovat MPEG-DASH, které jsou chráněné pomocí Widevine DRM, který se zašifrují podle specifikace Common Encryption (CENC).

Spuštění pomocí .NET SDK služby Media Services verze 3.5.2, Media Services umožňuje konfigurovat šablonu licence Widevine a získání licence na Widevine. Licence Widevine vám také mohou doručit následující partneři AMS : [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

## <a name="castlabs"></a>castLabs
Můžete použít [castLabs](http://castlabs.com/company/partners/azure/) pro doručování licence na Widevine. Další informace najdete v tématu [pomocí castLabs k poskytování DRM licence k Azure Media Services](media-services-castlabs-integration.md)

## <a name="axinom"></a>Axinom
Můžete použít [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) pro doručování licence na Widevine. Další informace najdete v tématu [pomocí Axinom k poskytování DRM licence k Azure Media Services](media-services-axinom-integration.md)

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Viz také
[Použití běžného dynamického šifrování PlayReady nebo Widevine](media-services-protect-with-drm.md)

[Blog na Mingfei](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)

