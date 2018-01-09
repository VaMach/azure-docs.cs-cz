---
title: "Začínáme se službou Azure Notification Hubs pro aplikace pro iOS | Dokumentace Microsoftu"
description: "V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace iOS."
services: notification-hubs
documentationcenter: ios
keywords: "nabízené oznámení;nabízená oznámení;nabízená oznámení ios"
author: jwhitedev
manager: kpiteira
editor: 
ms.assetid: b7fcd916-8db8-41a6-ae88-fc02d57cb914
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: 0e9e7ab196eef790b74074be319cd8122cf3ff5c
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/02/2018
---
# <a name="get-started-with-azure-notification-hubs-for-ios-apps"></a>Začínáme se službou Azure Notification Hubs pro aplikace pro iOS
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Přehled
> [!NOTE]
> K dokončení tohoto kurzu potřebujete mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).
> 
> 

V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace systému iOS. Vytvoříte prázdnou aplikaci iOS, která přijímá nabízená oznámení pomocí [služby nabízených oznámení Apple (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1). 

Jakmile budete hotovi, budete moci používat vaše centrum oznámení k vysílání nabízených oznámení pro všechna zařízení používající vaši aplikaci.

## <a name="before-you-begin"></a>Než začnete
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Dokončený kód v tomto kurzu lze najít v části [Github](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

## <a name="prerequisites"></a>Požadavky
V tomto kurzu budete potřebovat následující:

* [Windows Azure Messaging Framework]
* Poslední verze jazyka [Xcode]
* Zařízení podporující iOS 10 (nebo novější verzi)
* Členství v [programu pro vývojáře Apple](https://developer.apple.com/programs/).
  
  > [!NOTE]
  > Z důvodu požadavků na konfiguraci pro nabízená oznámení musíte nasadit a otestovat nabízená oznámení na fyzickém zařízení iOS (iPhone nebo iPad) namísto simulátoru iOS.
  > 
  > 

Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro aplikace iOS.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a>Konfigurace centra oznámení pro nabízená oznámení iOS
Tato části vás provede jednotlivými kroky vytvoření nového centra oznámení a konfigurace ověřování s použitím služby APNS a dříve vytvořeného nabízeného certifikátu **.p12**. Pokud chcete použít centrum oznámení, které jste již vytvořili, můžete přeskočit na krok 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li>

<p>V části <b>Notification Services</b> vyberte <b>Apple (APNS)</b>. Nezapomeňte vybrat <b>Certifikát</b>, klikněte na ikonu souboru a vyberte soubor <b>.p12</b>, který jste vyexportovali dříve. Ujistěte se, že zadáváte správné heslo.</p>

<p>Nezapomeňte vybrat režim <b>Sandboxu</b>, protože se jedná o vývoj. Používejte pouze režim <b>Výroba</b>, pokud chcete zasílat nabízená oznámení uživatelům, kteří si zakoupili aplikaci z obchodu s aplikacemi.</p>
</li>
</ol>
&emsp;&emsp;&emsp;&emsp;![Konfigurace služby APNS na webu Azure Portal][6]

&emsp;&emsp;&emsp;&emsp;![Konfigurace certifikační služby APNS na webu Azure Portal][7]

Právě jste své centrum oznámení nakonfigurovali pro práci se službou APNS. Zároveň máte připojovací řetězce, pomocí kterých můžete svou aplikaci zaregistrovat pro odesílání nabízených oznámení.

## <a name="connect-your-ios-app-to-notification-hubs"></a>Připojte aplikaci iOS k centru oznámení
1. V Xcode vytvořte nový projekt iOS a vyberte šablonu **Jediné zobrazení aplikace**.
   
    ![Xcode – jediné zobrazení aplikace][8]
    
2. Když nastavujete možnosti pro nový projekt, nezapomeňte použít stejný **Název produktu** a **Identifikátor organizace**, který jste použili při nastavení identifikátoru sady na portálu pro vývojáře Apple.
   
    ![Xcode – možnosti projektu][11]
    
3. V části Navigátor projektu klikněte na název vašeho projektu, klikněte na kartu **Obecné** a vyhledejte **Podepisování**. Nezapomeňte vybrat odpovídající Tým pro váš účet vývojáře Apple. XCode by na základě vašeho identifikátoru sady mělo automaticky stáhnout profil zřizování, který jste vytvořili dříve.
   
    Pokud nevidíte nový profil zřizování, který jste vytvořili v Xcode, pokuste se aktualizovat profily pro podpisové identity. Klikněte na tlačítko **Xcode** na panelu nabídek, klikněte na tlačítko **Předvolby**, klikněte na kartu **Účet**, klikněte na tlačítko **Zobrazit podrobnosti**, klikněte na podpisovou identitu a pak klikněte na tlačítko Aktualizovat v pravém dolním rohu.

    ![Xcode – profil zřizování][9]

4. Vyberte kartu **Možnosti** a nezapomeňte povolit Nabízená oznámení.

    ![Xcode – možnosti nabízení][12]
   
5. Stáhněte soubor rozhraní [Windows Azure Messaging Framework] a rozbalte ho. V Xcode klikněte pravým tlačítkem na projekt a klikněte na možnost **Přidat soubory do** a přidejte složku **WindowsAzureMessaging.framework** do projektu Xcode. Vyberte **Možnosti**, ujistěte se, že je vybraná možnost **Kopírovat položky v případě potřeby**, a pak klikněte na **Přidat**.

    ![Rozbalte Azure SDK][10]

6. Do projektu **HubInfo.h** přidejte nový soubor hlaviček. Tento soubor bude obsahovat konstanty pro vaše centrum oznámení. Přidejte následující definice a nahraďte zástupné symboly literálu řetězce ve vašem *názvu centra* a *DefaultListenSharedAccessSignature*, který jste si předtím poznamenali.

    ```obj-c
        #ifndef HubInfo_h
        #define HubInfo_h
   
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
   
        #endif /* HubInfo_h */
    ```
    
7. Otevřete váš soubor **AppDelegate.h** a přidejte následující direktivy importu:

    ```obj-c
        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
        #import <UserNotifications/UserNotifications.h> 
        #import "HubInfo.h"
    ```
8. Ve vašem **souboru AppDelegate.m** přidejte následující kód do metody **didFinishLaunchingWithOptions** v závislosti na vaší verzi iOS. Tento kód zaregistruje popisovač vašeho zařízení do APN:

    ```obj-c
        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
            UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];
   
        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];
    ```
   
9. Do stejného souboru přidejte následující metody:

    ```obj-c
         - (void) application:(UIApplication *) application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
           SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];
   
            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
               if (error != nil) {
                   NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                   [self MessageBox:@"Registration Status" message:@"Registered"];
              }
          }];
         }
   
        -(void)MessageBox:(NSString *) title message:(NSString *)messageText
        {
         UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }
    ```

    Tento kód se připojí k centru oznámení pomocí informací o připojení zadaných do souboru HubInfo.h. Poté přiřadí token zařízení do centra oznámení tak, aby centrum oznámení mohlo odesílat oznámení.

10. Do stejného souboru přidejte následující metodu pro zobrazení **UIAlert**, pokud bylo přijato oznámení, že je aplikace aktivní:

    ```obj-c
            - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
               NSLog(@"%@", userInfo);
               [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
           }
    ```

11. Ověřte, zda nedochází k žádným chybám tak, že sestavíte a spustíte aplikaci na vašem zařízení.

## <a name="send-test-push-notifications"></a>Odešlete nabízená oznámení
Příjem oznámení ve vaší aplikaci můžete otestovat pomocí možnosti *Testovací odeslání* na webu [Azure Portal]. Tím se do vašeho zařízení odešle testovací nabízené oznámení.

![Portál Azure – testovací odeslání][30]

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="checking-if-your-app-can-receive-push-notifications"></a>Kontrola, zda vaše aplikace může přijímat nabízená oznámení
Chcete-li otestovat nabízená oznámení na iOS, musíte aplikaci nasadit do fyzického zařízení iOS. Nabízená oznámení Apple nelze odeslat pomocí simulátoru iOS.

1. Spusťte aplikaci a ověřte, zda byla registrace úspěšná a stiskněte klávesu **OK**.
   
    ![Test registrace nabízených oznámení aplikace iOS][33]
2. Dále odešlete testovací nabízené oznámení z webu [Azure Portal], jak je popsáno výše. 

3. Nabízená odeslání se zašlou na všechna zařízení, která jsou registrovaná pro příjem oznámení z konkrétní centra oznámení.
   
    ![Test příjmu nabízených oznámení aplikace iOS][35]

## <a name="next-steps"></a>Další kroky
V tomto příkladu jste vysílali nabízená oznámení pro všechna vaše registrovaná zařízení iOS. Doporučeným dalším krokem ve vašem studiu je začít s kurzem [Upozornění uživatelů centra oznámení Azure pro iOS pomocí back-endu .NET]. Tento průvodce vás provede vytvořením back-endu pro odesílání nabízeních oznámení s použitím značek. 

Pokud chcete segmentovat uživatele podle zájmových skupin, můžete se navíc přesunout na tutoriál [Používání centra oznámení k odesílání novinek]. 

Obecné informace o centrech oznámení naleznete v tématu [Průvodce centry oznámení].

<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png
[12]: ./media/notification-hubs-ios-get-started/notification-hubs-enable-push.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Windows Azure Messaging Framework]: http://go.microsoft.com/fwlink/?LinkID=799698&clcid=0x409
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Průvodce centry oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Upozornění uživatelů centra oznámení Azure pro iOS pomocí back-endu .NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Používání centra oznámení k odesílání novinek]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Local and Push Notification Programming Guide]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure Portal]: https://portal.azure.com
