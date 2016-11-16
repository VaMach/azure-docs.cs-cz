---
title: "Nabízená oznámení iOS s centry oznámení pro aplikace Xamarin | Dokumentace Microsoftu"
description: "V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace Xamarin iOS."
services: notification-hubs
keywords: "nabízená oznámení ios,nabízení zpráv,nabízená oznámení,nabízená zpráva"
documentationcenter: xamarin
author: ysxu
manager: erikre
editor: 
ms.assetid: 4d4dfd42-c5a5-4360-9d70-7812f96924d2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/29/2016
ms.author: yuaxu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 53f9d5b37e6f754540e70b534555750dd54895d0


---
# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a>Nabízená oznámení iOS s centry oznámení pro aplikace Xamarin
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Přehled
> [!IMPORTANT]
> K dokončení tohoto kurzu potřebujete mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).
> 
> 

V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace systému iOS.
Vytvoříte prázdnou aplikaci Xamarin.iOS, která přijímá nabízená oznámení pomocí [služby nabízených oznámení Apple (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html). Jakmile budete hotovi, budete moci používat vaše centra oznámení k vysílání nabízených oznámení pro všechna zařízení používající vaši aplikaci. Dokončený kód je k dispozici ve vzorku [Aplikace NotificationHubs][GitHub].

Tento kurz představuje scénář jednoduchého vysílání zprávy oznámení pomocí centra oznámení.

## <a name="prerequisites"></a>Požadavky
V tomto kurzu budete potřebovat následující:

* [Xcode 6.0][Instalaci Xcode]
* Zařízení kompatibilní s iOS 7.0 (nebo novější verze)
* Členství v programu pro vývojáře iOS.
* [Xamarin Studio]
  
  > [!NOTE]
  > Z důvodu požadavků na konfiguraci pro nabízená oznámení iOS musíte nasadit a otestovat vzorovou aplikaci na fyzickém zařízení iOS (iPhone nebo iPad) namísto simulátoru.
  > 
  > 

Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro Xamarin iOS Apps.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub"></a>Konfigurace centra oznámení
Tato části vás provede vytvořením nového centra oznámení a konfigurací ověřování s použitím služby APNS a pomoci vytvořeného nabízeného certifikátu **.p12**. Pokud chcete použít centrum oznámení, které jste již vytvořili, můžete přeskočit na krok 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li>

<p>Jelikož chceme konfigurovat připojení služby APN na webu Azure Portal, otevřete své nastavení centra oznámení a klikněte na <b>služby oznámení</b> a pak klikněte na položku <b>Apple (APNS)</b> v seznamu. Po dokončení klikněte na položku <b>Nahrát certifikát</b> a vyberte certifikát <b>.p12</b>, který jste dříve exportovali, a také heslo pro certifikát.</p>

<p>Je nutné vybrat režim <b>Sandboxu</b> vzhledem k tomu, že budete odesílat nabízené zprávy ve vývojovém prostředí. Používejte pouze nastavení <b>Výroba</b>, pokud chcete zasílat nabízená oznámení uživatelům, kteří si již zakoupili aplikaci z obchodu s aplikacemi.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)

Vaše centrum oznámení je nyní nakonfigurováno pro práci se službou APNS. Zároveň máte připojovací řetězce, pomocí kterých můžete svou aplikaci zaregistrovat pro odesílání nabízených oznámení.

## <a name="connect-your-app-to-the-notification-hub"></a>Připojte aplikaci k centru oznámení
#### <a name="create-a-new-project"></a>Vytvoření nového projektu
1. V Xamarin Studiu vytvořte nový projekt iOS a vyberte šablonu **Unifikované API** > **Jediné zobrazení aplikace**.
   
       ![Xamarin Studio - Select Application Type][31]
2. Přidejte odkaz na komponentu zasílání zpráv Azure. V zobrazení řešení klikněte pravým tlačítkem na složku **Komponenty** pro váš projekt a vyberte **Získat další komponenty**. Vyhledejte komponentu **Zasílání zpráv Azure** a přidejte komponentu do projektu.
3. Do souboru **AppDelegate.cs** přidejte následující pomocí příkazu:
   
        using WindowsAzure.Messaging;
4. Deklarujte instanci **SBNotificationHub**:
   
        private SBNotificationHub Hub { get; set; }
5. Vytvořte třídu **Constants.cs** s následujícími proměnnými:
   
        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";
6. V souboru **AppDelegate.cs** aktualizujte **FinishedLaunching()** tak, aby odpovídaly následujícímu:
   
        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());
   
                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }
   
            return true;
        }
7. Potlačte metodu **RegisteredForRemoteNotifications()** v **AppDelegate.cs**:
   
        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);
   
            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }
   
                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }
8. Potlačte metodu **ReceivedRemoteNotifications()** v **AppDelegate.cs**:
   
        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }
9. Vytvořte následující metodu **ProcessNotification()** v **AppDelegate.cs**:
   
        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
   
                string alert = string.Empty;
   
                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();
   
                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }
   
   > [!NOTE]
   > Můžete se rozhodnout přepsat **FailedToRegisterForRemoteNotifications()** pro řešení situací jako chybějící síťové připojení. To je obzvláště důležité, když by uživatel mohl spustit aplikaci v režimu offline (například letadlo) a vy chcete zpracovávat scénáře zpráv oznámení specifické pro vaši aplikaci.
   > 
   > 
10. Spusťte aplikaci v zařízení.

## <a name="sending-push-notifications"></a>Odeslání nabízených oznámení
Nabízená oznámení můžete otestovat ve vaší aplikaci odesláním oznámení na [Azure Portal] prostřednictvím schopnosti **Testovací odeslání** v sadě nástrojů **Poradce při potížích** přímo na stránce centra oznámení, jak je znázorněno na obrazovce níže.

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

Nabízená oznámení se většinou posílají pomocí služby backend, jako je služba Mobile Services, nebo v technologii ASP.NET pomocí kompatibilní knihovny. Můžete také použít rozhraní REST API k přímému zasílání oznámení, pokud pro váš scénář není dostupná knihovna. 

V tomto kurzu nebudeme dělat nic složitého a jednoduše předvedeme testování vaší klientské aplikace pomocí odesílání oznámení v sadě SDK .NET pro centra oznámení v konzolové aplikaci, místo back-end služby. Jako další krok pro odesílání oznámení z backendu ASP.NET doporučujeme absolvovat kurz [Použití Notification Hubs k odeslání nabízených oznámení uživatelům](notification-hubs-aspnet-backend-ios-apple-apns-notification.md). Následující přístupy lze však použít pro zasílání oznámení:

* **Rozhraní REST**: nabízené oznámení můžete podporovat na jakékoli backend platformě pomocí [rozhraní REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).
* **Microsoft Azure oznámení centra .NET SDK**: Ve správci balíčků Nuget pro Visual Studio spusťte položku [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).
* **Node.js**: [Jak používat Notification Hubs z Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

**Mobile Apps**: Příklad zasílání oznámení z back-endu Azure App Service Mobile Apps integrovaného se službou Notification Hubs najdete v tématu [Přidání nabízených oznámení do mobilních aplikací](../app-service-mobile/app-service-mobile-ios-get-started-push.md).

* **Java/PHP**: příklad odesílání nabízených oznámení pomocí rozhraní REST API najdete v části „Jak používat Notification Hubs z Javy/PHP“ ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

#### <a name="optional-send-push-notifications-from-a-net-console-app"></a>(Volitelné) Odesílání nabízených oznámení z konzoly aplikace .NET
V této části odešleme nabízená oznámení pomocí konzolové aplikace .NET Pro účely tohoto příkladu přepněme do vývojového prostředí systému Windows, které obsahuje nainstalované Visual Studio.

1. Ve Visual Studiu vytvořte novou konzolovou aplikaci Visual C#:
   
       ![Visual Studio - Create a new console application][213]
2. Ve Visual Studiu klikněte na položku **Nástroje**, klikněte na **Správce balíčků NuGet** a pak klikněte na **Konzola Správce balíčků**.
   
    Balíček konzoly správce se musí zobrazit ukotvený k dolnímu okraji pracovního prostoru Visual Studia.
3. V okně konzoly Správce balíčků nastavte **Výchozí projekt** na nový projekt konzolové aplikace a pak v okně konzoly spusťte následující příkaz:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Ten přidá odkaz na sadu SDK centra oznámení Azure pomocí <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. Otevřete soubor `Program.cs` a přidejte následující příkaz `using`, čímž zajistíte, že budeme moci použít třídy a funkce Azure v rámci hlavní třídy:
   
        using Microsoft.Azure.NotificationHubs;
5. Do vaší třídy `Program` přidejte následující metodu (nezapomeňte nahradit **připojovací řetězec** a **název centra**):
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }
6. Do metody `Main` přidejte následující řádky:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Stiskněte klávesu F5 a spusťte aplikaci. Během několika sekund měli byste vidět nabízená oznámení v zařízení. Ať používáte Wi-Fi nebo mobilních datovou síť, ujistěte se, že máte v zařízení aktivní připojení k internetu.

Můžete najít všechny možné datové části v Apple [Průvodci programováním místních a nabízených oznámení].

#### <a name="optional-send-notifications-from-a-mobile-service"></a>(Volitelné) Odesílání oznámení z mobilní služby
V této části vám odešleme nabízená oznámení pomocí mobilních služeb prostřednictvím skriptu uzlu.

Při odesílání oznámení pomocí mobilních služeb využijte téma [Začínáme používat Mobile Services] a pak:

1. Přihlaste se do [portál Azure Classic] a vyberte mobilní služby.
2. Vyberte kartu **Plánovač** nahoře.
   
       ![Azure Classic Portal - Scheduler][215]
3. Vytvořte novou naplánovanou úlohu, vložte název a vyberte **Na vyžádání**.
   
       ![Azure Classic Portal - Create new job][216]
4. Po vytvoření úlohy klikněte na název úlohy. Klikněte na kartu **Skript** v horním panelu.
5. Vložte následující skript dovnitř funkce plánovače. Ujistěte se, zda jste nahradili zástupné symboly pomocí názvu centra oznámení a připojovacího řetězce pro *DefaultFullSharedAccessSignature* získaného dříve. Klikněte na **Uložit**.
   
        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                  "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );
6. Klikněte na tlačítko **Spustit jednou** na dolním panelu. Měli byste obdržet upozornění na vašem zařízení.

## <a name="next-steps"></a>Další kroky
V tomto příkladu jste vysílali nabízená oznámení pro všechna vaše zařízení iOS. Chcete-li se zaměřit na konkrétní uživatele, využijte kurz [Použití Notification Hubs k odeslání nabízených oznámení uživatelům]. Pokud chcete segmentovat uživatele podle zájmových skupin, můžete si přečíst kurz [Používání centra oznámení k odesílání novinek]. Další informace o tom, jak používat centra oznámení, naleznete v tématu [Průvodce centry oznámení] a v tématu [Centra oznámení s postupy pro iOS].

<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Odeslání stránky aplikace]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[Moje aplikace]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK pro Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Začínáme používat Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-ios
[portál Azure Classic]: https://manage.windowsazure.com/
[Průvodce centry oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
[Centra oznámení s postupy pro iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Instalaci Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Portál zřizování iOS]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Použití Notification Hubs k odeslání nabízených oznámení uživatelům]: /manage/services/notification-hubs/notify-users-aspnet
[Používání centra oznámení k odesílání novinek]: /manage/services/notification-hubs/breaking-news-dotnet

[Průvodci programováním místních a nabízených oznámení]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Komponenta Azure Mobile Services]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[Azure Portal]: https://portal.azure.com



<!--HONumber=Nov16_HO2-->


