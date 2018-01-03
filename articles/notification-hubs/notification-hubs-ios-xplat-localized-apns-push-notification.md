---
title: "Oznámení centra lokalizované nejnovější novinky kurz pro iOS"
description: "Zjistěte, jak používat Azure Service Bus Notification Hubs k odesílání oznámení o lokalizované aktuálních zprávách (iOS)."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 484914b5-e081-4a05-a84a-798bbd89d428
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: fd2b7d9dfd4f432bbcbaa3ed76f8bec0b9677e17
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="use-notification-hubs-to-send-localized-breaking-news-to-ios-devices"></a>Použití centra oznámení k odesílání novinek lokalizované do zařízení s iOS
> [!div class="op_single_selector"]
> * [Windows Store jazyka C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a>Přehled
Toto téma ukazuje, jak používat [šablony](notification-hubs-templates-cross-platform-push-messages.md) funkce Azure Notification Hubs k vysílání oznámení o aktuálních zprávách, lokalizované podle jazyka a zařízení. V tomto kurzu začnete k aplikaci iOS, které jsou vytvořené v [použití centra oznámení k odesílání novinek]. Po dokončení, že bude možné registrovat kategorií, které vás zajímají, zadat jazyk, ve které chcete dostávat oznámení a přijímat pouze nabízená oznámení pro vybrané kategorie v daném jazyce.

Existují tento scénář se skládá ze dvou částí:

* aplikace pro iOS umožňuje klienta zařízení můžete určit jazyk a k odběru kategorií různých nejnovější zprávy;
* back-end vysílá oznámení, pomocí **značka** a **šablony** feautres Azure Notification Hubs.

## <a name="prerequisites"></a>Požadavky
Musí jste již dokončili [použití centra oznámení k odesílání novinek] kurz a mít kód k dispozici, protože v tomto kurzu staví přímo na tento kód.

Visual Studio 2012 nebo novější je volitelný.

## <a name="template-concepts"></a>Koncepty šablon
V [použití centra oznámení k odesílání novinek] jste vytvořili aplikaci, která používá **značky** přihlášení k odběru oznámení pro různé zprávy kategorie.
Velký počet aplikací, ale cíli více trhů a vyžadují lokalizace. To znamená, že obsah sami oznámení musí být lokalizovaný a doručí na správnou sadu zařízení.
V tomto tématu ukážeme, jak používat **šablony** funkce centra oznámení snadno dodávat služby vhodné oznámení o lokalizované aktuálních zprávách.

Poznámka: jeden způsob, jak odeslat lokalizované oznámení je vytvoření více verzí jednotlivé značky. Například pro podporu angličtinu, francouzštinu a Mandarínština, by potřebujeme tří různých značek pro world zprávy: "world_en", "world_fr" a "world_ch". Pak nám odeslat lokalizované verzi world zprávy pro každé z těchto značek. V tomto tématu používáme šablony předejdete tím, jak narůstá značek a požadavek odeslat více zpráv.

Na vysoké úrovni šablony jsou způsob, jak určit, jak by měla určité zařízení zasláno oznámení. Šablona specifikuje formát datové části přesně tím, že odkazuje na vlastnosti, které jsou součástí zprávy odeslané ve vašem back-end aplikace. V našem případě pošleme zprávu bez ohledu na národním prostředí obsahující všechny podporované jazyky:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Potom jsme zajistí, že zařízení zaregistrovat pomocí šablony, která odkazuje na správný vlastnost. Například aplikaci iOS, která chce k registraci pro francouzštině zprávy se registrují následující:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Šablony jsou velmi výkonné funkce, můžete další informace o možnostech v našem [šablony](notification-hubs-templates-cross-platform-push-messages.md) článku.

## <a name="the-app-user-interface"></a>Uživatelské rozhraní aplikace
Nyní jsme upraví novinkách aplikaci, kterou jste vytvořili v tématu [použití centra oznámení k odesílání novinek] odeslat lokalizované novinky pomocí šablon.

Vaše MainStoryboard_iPhone.storyboard přidat Segmentovaným ovládací prvek se třemi jazyky, které bude podporujeme: angličtina, francouzština a Mandarínština.

![][13]

Potom nezapomeňte přidat IBOutlet ve vaší ViewController.h, jak je uvedeno níže:

![][14]

## <a name="building-the-ios-app"></a>Sestavit aplikaci pro iOS
1. Ve vašem Notification.h přidat *retrieveLocale* metoda a upravte úložišti a přihlášení k odběru metod, jak je uvedeno níže:
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;
   
        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;
   
        - (NSSet*) retrieveCategories;
   
        - (int) retrieveLocale;
   
    V Notification.m, upravte *storeCategoriesAndSubscribe* metoda přidáním parametr národního prostředí a ukládání do výchozí nastavení uživatele:
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];
   
            [self subscribeWithLocale: locale categories:categories completion:completion];
        }
   
    Potom upravte *přihlášení k odběru* tak, aby zahrnoval národního prostředí:
   
        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];
   
            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }
   
            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];
   
            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }
   
    Všimněte si, jak se teď používá metodu *registerTemplateWithDeviceToken*, místo *registerNativeWithDeviceToken*. Když jsme zaregistrovat pro šablonu máme zadejte šablonu json a také název pro šablonu (protože naše aplikace chtít zaregistrovat různé šablony). Zajistěte, aby k registraci vaší kategorií jako značky, jak chceme, abyste měli jistotu, přijímat notifciations tyto informace.
   
    Přidejte metodu k načtení národního prostředí z výchozí nastavení uživatele:
   
        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
   
            int locale = [defaults integerForKey:@"BreakingNewsLocale"];
   
            return locale < 0?0:locale;
        }
2. Teď, když jsme upravit Naše třída oznámení, máme Ujistěte se, že naše ViewController využívá nové UISegmentControl. Přidejte následující řádek v *viewDidLoad* metoda zobrazíte národního prostředí, který je aktuálně vybraný zajistit:
   
        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];
   
    Potom v vaše *přihlášení k odběru* metoda, změňte nastavení volání *storeCategoriesAndSubscribe* pro následující:
   
        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];
3. Nakonec budete muset aktualizovat *didRegisterForRemoteNotificationsWithDeviceToken* metoda v AppDelegate.m, tak, aby správně můžete aktualizovat registrace při spuštění aplikace. Změňte nastavení volání *přihlášení k odběru* metoda oznámení s následující:
   
        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

## <a name="optional-send-localized-template-notifications-from-net-console-app"></a>(volitelné) Odesílání oznámení lokalizovanou šablonu z konzolové aplikace .NET.
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

## <a name="optional-send-localized-template-notifications-from-the-device"></a>(volitelné) Odeslat oznámení lokalizovanou šablonu ze zařízení
Pokud máte přístup k sadě Visual Studio, nebo jenom chcete otestujte, zasílání oznámení lokalizovanou šablonu přímo z aplikace na zařízení.  Můžete jednoduché lokalizovanou šablonu parametry, které chcete přidat `SendNotificationRESTAPI` metoda definované v předchozích kurzu.

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }




## <a name="next-steps"></a>Další kroky
Další informace o používání šablon najdete v tématu:

* [Upozorněte uživatele s centry oznámení: technologie ASP.NET]
* [Upozorněte uživatele s centry oznámení: Mobile Services]

<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[použití centra oznámení k odesílání novinek]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Upozorněte uživatele s centry oznámení: technologie ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Upozorněte uživatele s centry oznámení: Mobile Services]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
