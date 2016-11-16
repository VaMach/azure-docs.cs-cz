---
title: "Začínáme s Azure Mobile Engagementem pro iOS v Objective C | Dokumentace Microsoftu"
description: "Naučte se používat Azure Mobile Engagement s analytickými funkcemi a nabízenými oznámeními pro aplikace pro iOS."
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 117b5742-522b-41de-98c5-f432da2d98f8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 10/05/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 1dc9885e4cdbad1153ac476e3f0c0068ec391374


---
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>Začínáme s Azure Mobile Engagementem pro aplikace pro iOS v Objective C
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

V tomto tématu si ukážeme, jak používat Azure Mobile Engagement, jak porozumět používání aplikace a jak odesílat nabízená oznámení segmentovaným uživatelům aplikace pro iOS.
V tomto kurzu vytvoříte prázdnou aplikaci iOS, která bude shromažďovat základní data a přijímat nabízená oznámení pomocí systému nabízených oznámení Apple (APNS). 

V tomto kurzu budete potřebovat následující:

* XCode 8, který si můžete nainstalovat z MAC App Storu
* [Mobile Engagement iOS SDK]

Ve všech dalších kurzech k Mobile Engagement týkajících se aplikací pro iOS se předpokládá dokončení tohoto kurzu.

> [!NOTE]
> K dokončení tohoto kurzu potřebujete mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).
> 
> 

## <a name="a-idsetupazmeasetup-mobile-engagement-for-your-ios-app"></a><a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro iOS
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="a-idconnectingappaconnect-your-app-to-the-mobile-engagement-backend"></a><a id="connecting-app"></a>Připojení aplikace k back-endu Mobile Engagementu
V tomto kurzu si představíme „základní integraci“, čili minimální sadu požadovanou pro shromažďování dat a odesílání nabízených oznámení. Kompletní dokumentaci k integraci najdete v článku [Integrace sady Mobile Engagement iOS SDK](mobile-engagement-ios-sdk-overview.md).

Pomocí XCodu si vytvoříme základní aplikaci, na které si tuto integraci předvedeme.

### <a name="create-a-new-ios-project"></a>Vytvoření nového projektu iOS
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Připojení aplikace k back-endu Mobile Engagementu
1. Stáhněte si [Mobile Engagement iOS SDK].
2. Extrahujte soubor .tar.gz do složky v počítači.
3. Pravým tlačítkem myši klikněte na projekt a pak vyberte **Přidat soubory do**.
   
    ![][1]
4. Přejděte do složky, do které jste extrahovali sadu SDK, vyberte složku `EngagementSDK` a poté stiskněte klávesu **OK**.
   
    ![][2]
5. Otevřete kartu **Build Phases** (Fáze sestavení) a v nabídce **Link Binary With Libraries** (Propojit binární kód s knihovnami) přidejte rozhraní, jak je zobrazeno níže:
   
    ![][3]
6. Přejděte zpět na portál Azure na stránku **Connection Info** (Informace o připojení) vaší aplikace a zkopírujte připojovací řetězec.
   
    ![][4]
7. Do souboru **AppDelegate.m** přidejte následující řádek kódu.
   
        #import "EngagementAgent.h"
8. Nyní vložte připojovací řetězec do delegáta `didFinishLaunchingWithOptions`.
   
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
              [...]   
              [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
              [...]
        }
9. `setTestLogEnabled`je volitelný příkaz, který protokolům SDK umožňuje identifikovat problémy. 

## <a name="a-idmonitoraenable-realtime-monitoring"></a><a id="monitor"></a>Povolení sledování v reálném čase
Pokud chcete začít odesílat data a zajistit, že uživatelé jsou aktivní, musíte odeslat alespoň jednu obrazovku (aktivitu) na back-end Mobile Engagementu.

1. Otevřete soubor **ViewController.h** a importujte **EngagementViewController.h**:
   
    `# import "EngagementViewController.h"`
2. Nyní nahraďte super třídu rozhraní **ViewController** třídou `EngagementViewController`:
   
    `@interface ViewController : EngagementViewController`

## <a name="a-idmonitoraconnect-app-with-realtime-monitoring"></a><a id="monitor"></a>Připojení aplikace se sledováním v reálném čase
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="a-idintegratepushaenable-push-notifications-and-inapp-messaging"></a><a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci
Mobile Engagement vám umožňuje v rámci kampaní oslovit uživatele a komunikovat s nimi prostřednictvím nabízených oznámení a zpráv v aplikacích. Tento modul se na portálu Mobile Engagement nazývá REACH.
V následujících sekcích nastavíte aplikaci, aby tato nabízená oznámení a zprávy přijímala.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Povolení přijímání bezobslužných nabízených oznámení v aplikaci
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a>Přidání knihovny Reach do projektu
1. Klikněte pravým tlačítkem na projekt.
2. Vyberte **Add file to** (Přidat soubor).
3. Přejděte do složky, do které jste extrahovali sadu SDK.
4. Vyberte složku `EngagementReach`.
5. Klikněte na tlačítko **Přidat**.

### <a name="modify-your-application-delegate"></a>Úprava delegáta aplikace
1. V souboru **AppDeletegate.m** importujte modul Engagement Reach.
   
        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>
2. V metodě `application:didFinishLaunchingWithOptions` vytvořte modul kampaně Reach a předejte jej do existujícího inicializačního řádku Engagement:
   
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

### <a name="enable-your-app-to-receive-apns-push-notifications"></a>Povolení přijímání nabízených oznámení APNS v aplikaci
1. Do metody `application:didFinishLaunchingWithOptions` přidejte následující řádek:
   
        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }
2. Následujícím způsobem přidejte metodu `application:didRegisterForRemoteNotificationsWithDeviceToken`:
   
        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
             [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }
3. Následujícím způsobem přidejte metodu `didFailToRegisterForRemoteNotificationsWithError`:
   
        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
   
           NSLog(@"Failed to get token, error: %@", error);
        }
4. Následujícím způsobem přidejte metodu `didReceiveRemoteNotification:fetchCompletionHandler`:
   
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png




<!--HONumber=Nov16_HO2-->


