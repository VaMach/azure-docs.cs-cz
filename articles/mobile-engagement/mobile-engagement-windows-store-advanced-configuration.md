---
title: "Pokročilá konfigurace pro zapojení univerzálních aplikací pro Windows SDK"
description: "Rozšířené možnosti konfigurace pro Azure Mobile Engagement s univerzálních aplikací pro Windows"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6d85dd5d-ac07-43ba-bbe4-e91c3a17690b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 10/04/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: cb9454212c94cf65093219c3d24c71277ede7877
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Pokročilá konfigurace pro zapojení univerzálních aplikací pro Windows SDK
> [!div class="op_single_selector"]
> * [Univerzální platforma Windows](mobile-engagement-windows-store-advanced-configuration.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-configuration.md)
> 
> 

Tento postup popisuje, jak nakonfigurovat různé možnosti konfigurace aplikací pro Azure Mobile Engagement Android.

## <a name="prerequisites"></a>Požadavky
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="advanced-configuration"></a>Pokročilá konfigurace
### <a name="disable-automatic-crash-reporting"></a>Zakázat automatické hlášení chyb
Můžete vypnout automatické hlášení funkci Engagement generování sestav. Pak, když dojde k neošetřené výjimce Engagement neprovede žádnou akci.

> [!WARNING]
> Pokud jste tuto funkci zakázat a pak v případě neošetřené havárie v aplikaci Engagement neodesílá havárii **a** nezavře relace a úlohy.
> 
> 

Chcete-li zakázat automatické hlášení chyb, přizpůsobit konfiguraci v závislosti na způsobu, jakým jste ji deklarovaný:

#### <a name="from-engagementconfigurationxml-file"></a>Z `EngagementConfiguration.xml` souboru
Nastavte Sestavy havárií na `false` mezi `<reportCrash>` a `</reportCrash>` značky.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Z `EngagementConfiguration` objektu v době běhu
Nastavit na hodnotu false pomocí objektu EngagementConfiguration sestavy havárií.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Zakázání zasílání zpráv v reálném čase.
Ve výchozím nastavení sestavy služby zapojení protokolů v reálném čase. Pokud vaše aplikace často hlásí protokoly, je lepší ukládat do vyrovnávací paměti v protokolech a informuje všechny najednou na základě pravidelné časové. Tomu se říká "burst režim".

Chcete-li tak učinit, zavolejte metodu:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Argument je hodnota **milisekundách**. Vždy, když chcete znovu aktivovat protokolování v reálném čase, volejte metodu bez zadání parametru nebo s hodnotou 0.

Režim shluků mírně zvyšuje výdrž baterie, ale má vliv na zapojení monitorování: všechny dobu trvání relace a úlohy jsou zaokrouhleny na shluků prahovou hodnotu (tedy relací a úlohy kratší, než je prahová hodnota shluků nemusí být zobrazeny). Doporučujeme používat práh shluků než 30000 (30s). Uložené protokoly jsou omezeny na 300 položek. Pokud odesílání je příliš dlouhý, může dojít ke ztrátě některých protokoly.

> [!WARNING]
> Prahová hodnota shluků nelze nakonfigurovat na dobu menší než jedna sekunda. Pokud tak učiníte, sada SDK ukazuje trasování došlo k chybě a automaticky obnoví na výchozí hodnotu nula sekund. Tím se spustí SDK. hlásí protokolů v reálném čase.
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
