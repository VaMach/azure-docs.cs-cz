<properties
    pageTitle="Začínáme s Azure Mobile Engagementem pro iOS ve Swiftu"
    description="Naučte se používat Azure Mobile Engagement s analýzou a nabízenými oznámeními pro aplikace pro iOS."
    services="mobile-engagement"
    documentationCenter="ios"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="swift"
    ms.topic="hero-article"
    ms.date="05/03/2016"
    ms.author="piyushjo" />

# Začínáme s Azure Mobile Engagementem pro aplikace pro iOS ve Swiftu

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

V tomto tématu si ukážeme, jak používat Azure Mobile Engagement, jak porozumět používání aplikace a jak odesílat nabízená oznámení segmentovaným uživatelům aplikace pro iOS.
V tomto kurzu vytvoříte prázdnou aplikaci iOS, která bude shromažďovat základní data a přijímat nabízená oznámení pomocí systému nabízených oznámení Apple (APNS). 

V tomto kurzu budete potřebovat následující:

+ XCode 6 nebo XCode 7, které můžete nainstalovat z MAC App Storu
+ [Mobile Engagement iOS SDK]
+ Certifikát nabízených oznámení (.p12), který můžete získat ve vývojářském centru Apple

> [AZURE.NOTE] V tomto kurzu budeme používat Swift verze 2.0. 

Ve všech dalších kurzech k Mobile Engagement týkajících se aplikací pro iOS se předpokládá dokončení tohoto kurzu.

> [AZURE.NOTE] K dokončení tohoto kurzu potřebujete mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).

##<a id="setup-azme"></a>Nastavení Mobile Engagementu pro vaši aplikaci pro iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal.md)]

##<a id="connecting-app"></a>Připojení aplikace k back-endu Mobile Engagementu

V tomto kurzu si představíme „základní integraci“, čili minimální sadu požadovanou pro shromažďování dat a odesílání nabízených oznámení. Kompletní dokumentaci k integraci najdete v článku [Integrace sady Mobile Engagement iOS SDK](mobile-engagement-ios-sdk-overview.md).

Pomocí XCodu si vytvoříme základní aplikaci, na které si tuto integraci předvedeme.

###Vytvoření nového projektu iOS

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###Připojení aplikace k back-endu Mobile Engagementu

1. Stáhněte si [Mobile Engagement iOS SDK].
2. Extrahujte soubor .tar.gz do složky v počítači.
3. Pravým tlačítkem myši klikněte na projekt a pak vyberte Přidat soubory do...

    ![][1]

4. Přejděte do složky, do které jste extrahovali sadu SDK, vyberte složku `EngagementSDK` a poté stiskněte klávesu OK.

    ![][2]

5. Otevřete kartu `Build Phases` (Fáze sestavení) a v nabídce `Link Binary With Libraries` (Propojit binární kód s knihovnami) přidejte rozhraní, jak je uvedeno níže. **POZNÁMKA:** musíte zahrnout `CoreLocation, CFNetwork, CoreTelephony, and SystemConfiguration` :

    ![][3]

6. V případě **XCode 7** – přidejte `libxml2.tbd` namísto `libxml2.dylib`.

7. Pomocí příkazu File (Soubor) > New (Nový) > File (Soubor) > iOS > Source (Zdroj) > Header File (Soubor hlavičky) vytvořte hlavičku přemostění, abyste mohli použít rozhraní API jazyka Objective C sady SDK.

    ![][4]

8. Upravte soubor hlavičky přemostění, aby se zpřístupnil kód jazyka Objective-C Mobile Engagementu pro kód jazyka Swift, a potom přidejte následující importy:

        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEIdfaProvider.h"

9. V části Nastavení sestavení zkontrolujte, zda má nastavení sestavení hlavičky přemostění jazyka Objective-C v části Swift Compiler - Code Generation (Kompilátor jazyka Swift – Generování kódu) cestu k této hlavičce. Zde je příklad cesty: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (v závislosti na cestě)**

    ![][6]

10. Přejděte zpět na portál Azure na stránce *Connection Info* (Informace o připojení) vaší aplikace a zkopírujte připojovací řetězec.

    ![][5]

11. Nyní vložte připojovací řetězec do delegáta `didFinishLaunchingWithOptions`.

        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool
        {
            [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
            [...]
        }

##<a id="monitor"></a>Povolení sledování v reálném čase

Pokud chcete začít odesílat data a zajistit, že uživatelé jsou aktivní, musíte odeslat alespoň jednu obrazovku (aktivitu) na back-end Mobile Engagementu.

1. Otevřete soubor **ViewController.swift** a nahraďte základní třídu **ViewController** třídou **EngagementViewController**:

    `class ViewController : EngagementViewController {`

##<a id="monitor"></a>Připojení aplikace se sledováním v reálném čase

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Povolení nabízených oznámení a zasílání zpráv v aplikaci

Mobile Engagement vám umožňuje v rámci kampaní oslovit uživatele a komunikovat s nimi prostřednictvím nabízených oznámení a zpráv v aplikacích. Tento modul se na portálu Mobile Engagement nazývá REACH.
V následujících sekcích nastavíte aplikaci, aby tato nabízená oznámení a zprávy přijímala.

### Povolení přijímání bezobslužných nabízených oznámení v aplikaci

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### Přidání knihovny Reach do projektu

1. Klikněte pravým tlačítkem na projekt.
2. Vyberte `Add file to ...`
3. Přejděte do složky, do které jste extrahovali sadu SDK.
4. Vyberte složku `EngagementReach`.
5. Klikněte na tlačítko Přidat.
6. Upravte soubor hlavičky přemostění, aby se zpřístupnily hlavičky Reach jazyka Objective-C Mobile Engagementu, a potom přidejte následující importy:

        /* Mobile Engagement Reach */
        #import "AEAnnouncementViewController.h"
        #import "AEAutorotateView.h"
        #import "AEContentViewController.h"
        #import "AEDefaultAnnouncementViewController.h"
        #import "AEDefaultNotifier.h"
        #import "AEDefaultPollViewController.h"
        #import "AEInteractiveContent.h"
        #import "AENotificationView.h"
        #import "AENotifier.h"
        #import "AEPollViewController.h"
        #import "AEReachAbstractAnnouncement.h"
        #import "AEReachAnnouncement.h"
        #import "AEReachContent.h"
        #import "AEReachDataPush.h"
        #import "AEReachDataPushDelegate.h"
        #import "AEReachModule.h"
        #import "AEReachNotifAnnouncement.h"
        #import "AEReachPoll.h"
        #import "AEReachPollQuestion.h"
        #import "AEViewControllerUtil.h"
        #import "AEWebAnnouncementJsBridge.h"

### Úprava delegáta aplikace

1. V metodě `didFinishLaunchingWithOptions` vytvořte modul kampaně Reach a předejte jej do existujícího inicializačního řádku Engagement:

        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
            let reach = AEReachModule.moduleWithNotificationIcon(UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

###Povolení přijímání nabízených oznámení APNS v aplikaci
1. Do metody `didFinishLaunchingWithOptions` přidejte následující řádek:

        /* Ask user to receive push notifications */
        if #available(iOS 8.0, *)
        {
           let settings = UIUserNotificationSettings(forTypes: [UIUserNotificationType.Alert, UIUserNotificationType.Badge, UIUserNotificationType.Sound], categories: nil)
           application.registerUserNotificationSettings(settings)
           application.registerForRemoteNotifications()
        }
        else
        {
           application.registerForRemoteNotificationTypes([UIRemoteNotificationType.Alert, UIRemoteNotificationType.Badge, UIRemoteNotificationType.Sound])
        }

2. Následujícím způsobem přidejte metodu `didRegisterForRemoteNotificationsWithDeviceToken`:

        func application(application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: NSData)
        {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }

3. Následujícím způsobem přidejte metodu `didReceiveRemoteNotification:fetchCompletionHandler:`:

        func application(application: UIApplication, didReceiveRemoteNotification userInfo: [NSObject : AnyObject], fetchCompletionHandler completionHandler: (UIBackgroundFetchResult) -> Void)
        {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png



<!--HONumber=Jun16_HO2-->


