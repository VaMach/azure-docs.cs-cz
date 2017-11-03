---
title: "Azure Mobile Engagement Windows Phone Silverlight SDK přehled | Microsoft Docs"
description: "Přehled sady Windows Phone Silverlight SDK pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0e3d2420-0509-4952-8891-392e3dad9aaf
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2016
ms.author: piyushjo
ms.openlocfilehash: c4e8ceee4104c3d3a6c3e6b79322ba1cf8463b22
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>Windows Phone Silverlight SDK přehled pro Azure Mobile Engagement
Chcete-li získat podrobné informace o tom, jak integrovat Azure Mobile Engagement v aplikaci Silverlight pro Windows Phone, začněte zde. Pokud chcete a vyzkoušejte ho nejdřív, ujistěte se, dokončení naše [15 minut kurzu](mobile-engagement-windows-phone-get-started.md).

Kliknutím zobrazíte [SDK obsahu](mobile-engagement-windows-phone-sdk-content.md)

## <a name="integration-procedures"></a>Integrace procedury
1. Začněte zde: [jak integrovat Mobile Engagement v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
2. Pro oznámení: [jak integrovat Reach (oznámení) v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement-reach.md)
3. Značka plán implementace: [jak používat rozšířené Mobile Engagement označování rozhraní API v aplikaci Windows Phone Silverlight](mobile-engagement-windows-phone-use-engagement-api.md)

## <a name="release-notes"></a>Poznámky k verzi
###<a name="331-11032016"></a>3.3.1 (11/03/2016)
Součást *MicrosoftAzure.MobileEngagement* balíček Nuget **v3.4.1**

* Zlepšení stability.

Starší verze najdete v tématu [dokončení poznámky k verzi](mobile-engagement-windows-phone-release-notes.md)

## <a name="upgrade-procedures"></a>Postupy upgradu
Pokud již máte integrovanou starší verze naše sady SDK do své aplikace, je nutné zvážit následující body při upgradu sady SDK.

Možná budete muset několik postupy použijte, pokud provedena několik verzí sady SDK. Zobrazit úplnou [postupy upgradu](mobile-engagement-windows-phone-upgrade-procedure.md). Například pokud migrujete z 0.10.1 0.11.0 budete muset nejdřív postupujte podle pokynů "od 0.9.0 k 0.10.1" pak postupu "od 0.10.1 k 0.11.0".

### <a name="from-200-to-330"></a>Z 2.0.0 k 3.3.0
#### <a name="test-logs"></a>Protokolů testování
Protokoly konzoly vyprodukované sady SDK teď může být povolena nebo zakázána nebo filtrovat. Chcete-li přizpůsobit tím, aktualizujte vlastnost `EngagementAgent.Instance.TestLogEnabled` na jednu z hodnota dostupná z `EngagementTestLogLevel` výčtu pro instanci:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>Upgrade ze starších verzí
V tématu [Upgrade procedury](mobile-engagement-windows-phone-upgrade-procedure.md)

