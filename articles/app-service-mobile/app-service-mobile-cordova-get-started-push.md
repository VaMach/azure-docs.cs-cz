---
title: "Přidání nabízených oznámení do aplikace Apache Cordova s Azure Mobile Apps | Microsoft Docs"
description: "Naučte se používat Azure Mobile Apps k odesílání nabízených oznámení do vaší aplikace Apache Cordova."
services: app-service\mobile
documentationcenter: javascript
manager: crdun
editor: 
author: conceptdev
ms.assetid: 92c596a9-875c-4840-b0e1-69198817576f
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: 05fa692f9331cf6b5178c3e9dca60ad2598dc609
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="add-push-notifications-to-your-apache-cordova-app"></a>Přidání nabízených oznámení do vaší aplikace Apache Cordova
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Přehled
V tomto kurzu přidání nabízených oznámení do projektu [Apache Cordova úvodní] tak, aby nabízených oznámení se odešle do zařízení pokaždé, když vložení záznamu.

Pokud použijete serverový projekt stažené rychlý start, je třeba balíček rozšíření nabízená oznámení. Další informace najdete v tématu [pracovat s .NET back-end serveru SDK pro Azure Mobile Apps][1].

## <a name="prerequisites"></a>Požadavky
Tento kurz se zaměřuje Apache Cordova aplikace vytvořené s Visual Studiem 2015, která běží na emulátor Google Android, zařízení se systémem Android, zařízení se systémem Windows a zařízení s iOS.

Pro absolvování tohoto kurzu potřebujete:

* Počítač s nástrojem [Visual Studio Community 2015] [ 2] nebo novější verze.
* [Nástroje sady Visual Studio pro Apache Cordova][4].
* [Aktivní účet Azure][3].
* Dokončené [úvodní Apache Cordova] [ 5] projektu.
* (Android) A [účet Google] [ 6] s ověřenou e-mailovou adresu.
* (iOS) [Programu pro vývojáře Apple členství] [ 7] a zařízení se systémem iOS (simulátoru iOS push nepodporuje).
* (Windows) A [Windows Store vývojářský účet] [ 8] a zařízením s Windows 10.

## <a name="configure-hub"></a>Konfigurace centra oznámení
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Přehrát video, zobrazující kroky v této části][9]

## <a name="update-the-server-project"></a>Aktualizace server project
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="add-push-to-app"></a>Upravit aplikaci Cordova
Zkontrolujte, zda že je připraven ke zpracování nabízených oznámení nainstalováním nabízené cordovu plus žádné specifické pro platformu nabízené služby projektu aplikace Apache Cordova.

#### <a name="update-the-cordova-version-in-your-project"></a>Aktualizujte verzi Cordova ve vašem projektu.
Pokud váš projekt používá starší než v6.1.1 verzi aplikace Apache Cordova, aktualizujte projektu klienta. Aktualizace projektu:

* Klikněte pravým tlačítkem na `config.xml` otevřete návrháře konfigurace.
* Vyberte kartu platformy.
* Zvolte 6.1.1 v **Cordova CLI** textové pole.
* Zvolte **sestavení**, pak **sestavit řešení** aktualizujte projekt.

#### <a name="install-the-push-plugin"></a>Instalace modulu plug-in push
Aplikace Apache Cordova nezpracuje nativně možnosti sítě nebo zařízení.  Tyto možnosti jsou poskytovány buď modulů plug-in, které jsou publikovány na [npm] [ 10] nebo na Githubu.  `phonegap-plugin-push` Modulu plug-in se používá ke zpracování nabízených oznámení sítě.

Modul plug-in nabízené můžete nainstalovat jedním z těchto způsobů:

**Z příkazového řádku:**

Spusťte následující příkaz:

    cordova plugin add phonegap-plugin-push

**Z v sadě Visual Studio:**

1. V Průzkumníku řešení otevřete `config.xml` klikněte na soubor **modulů plug-in** > **vlastní**, vyberte **Git** jako zdroj instalace pak zadejte `https://github.com/phonegap/phonegap-plugin-push`jako zdroj.

   ![][img1]

2. Klikněte na šipku vedle zdroje instalace.
3. V **SENDER_ID**, pokud již máte ID číselné projektu pro projekt vývojářské konzole Google, můžete přidat sem. Jinak zadejte hodnotu zástupného symbolu, jako je 777777.  Pokud cílíte na Android, můžete je aktualizovat tuto hodnotu v config.xml později.
     Všimněte si, že od verze 2.0.0 SENDER_ID byl odebrán z instalace čas a google services.json musí být nainstalovaný v kořenové složce projektu.  Další podrobnosti najdete v části [sem.](https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md)
4. Klikněte na tlačítko **Add** (Přidat).

Modul plug-in nabízené je nyní nainstalován.

#### <a name="install-the-device-plugin"></a>Instalace modulu plug-in zařízení
Postupujte podle stejného postupu, který jste použili k instalaci modulu plug-in push.  Přidání modulu plug-in zařízení ze seznamu modulů plug-in jádra (klikněte na tlačítko **modulů plug-in** > **základní** ji najít). Je nutné tento modul plug-in získat název platformy.

#### <a name="register-your-device-on-application-start-up"></a>Zaregistrovat zařízení při spuštění aplikace
Na začátku jsme obsahovat určitý minimální kód pro Android. Později upravte aplikaci spustit v iOS nebo Windows 10.

1. Přidejte volání **registerForPushNotifications** během zpětného volání pro proces přihlášení nebo v dolní části **onDeviceReady** metoda:

        // Login to the service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added to register for push notifications.
                registerForPushNotifications();

            }, handleError);

    Tento příklad ukazuje volání **registerForPushNotifications** po úspěšném provedení ověřování.  Můžete volat `registerForPushNotifications()` tak často, jako je povinný.

2. Přidejte nové **registerForPushNotifications** metoda následujícím způsobem:

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle the registration event.
        pushRegistration.on('registration', function (data) {
          // Get the native platform of the device.
          var platform = device.platform;
          // Get the handle returned during registration.
          var handle = data.registrationId;
          // Set the device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }
3. (Android) V předchozím kódu, nahraďte `Your_Project_ID` s numerická projektu ID pro vaši aplikaci z [vývojářské konzole Google][18].

## <a name="optional-configure-and-run-the-app-on-android"></a>(Volitelné) Konfigurace a spuštění aplikace v systému Android
Dokončení této části ke zprovoznění nabízených oznámení pro Android.

#### <a name="enable-gcm"></a>Povolit Firebase cloudu zasílání zpráv
Vzhledem k tomu, že jsme se původně cílené na platformu Google Android, musíte povolit zasílání zpráv cloudu Firebase.

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

#### <a name="configure-backend"></a>Konfigurace back-end mobilní aplikace k odeslání žádosti o nabízenou pomocí FCM
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

#### <a name="configure-your-cordova-app-for-android"></a>Konfigurace aplikace Cordova pro Android
V aplikaci Cordova otevřete config.xml a nahraďte `Your_Project_ID` s numerická projektu ID pro vaši aplikaci z [vývojářské konzole Google][18].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

Otevřete index.js a aktualizujte kód, který použije vaše ID číselné projektu.

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

#### <a name="configure-device"></a>Konfigurace zařízení s Androidem pro ladění USB
Než bude možné nasadit aplikace do zařízení se systémem Android, budete muset povolit ladění USB.  Na váš telefon se systémem Android, proveďte následující kroky:

1. Přejděte na **nastavení** > **o telefonu**, klepněte **číslo sestavení** dokud režim vývojáře je povolen (o sedm časy).
2. Zpět v **nastavení** > **možnosti pro vývojáře** povolit **ladění USB**, telefon s Androidem se potom připojují k vaší vývoj počítači pomocí kabelu USB.

Jsme testovali to pomocí Google Nexus 5 X zařízení se systémem Android 6.0 (Marshmallow).  Technik jsou však společné pro všechny moderní Android verze.

#### <a name="install-google-play-services"></a>Nainstalujte služby Google Play
Modul plug-in nabízené spoléhá na Android služby Google Play pro nabízená oznámení.

1. V sadě Visual Studio, klikněte na tlačítko **nástroje** > **Android** > **Android SDK Manager**, rozbalte **funkce** složka a zaškrtněte políčka zajistí každé z následujících sad SDK je nainstalována.

   * Android 2.3 nebo vyšší
   * Revize Google úložiště 27 nebo vyšší
   * Služby Google Play 9.0.2 nebo vyšší

2. Klikněte na tlačítko **instalace balíčků** a počkejte na dokončení instalace.

Aktuální požadované knihovny jsou uvedeny v [phonegap-plugin nabízené instalace dokumentace][19].

#### <a name="test-push-notifications-in-the-app-on-android"></a>Nabízená oznámení v aplikaci pro Android
Spuštěním aplikace a vložení položek v tabulce TodoItem můžete nyní nabízená oznámení. Ze stejného zařízení nebo z druhé zařízení, můžete otestovat tak dlouho, dokud používají stejný back-end. Testování aplikace Cordova na platformě Android v jednom z následujících způsobů:

* **Na fyzické zařízení:** přiřadit vývojovém počítači pomocí kabelu USB zařízení se systémem Android.  Místo **emulátor Google Android**, vyberte **zařízení**. Visual Studio nasadí aplikaci do zařízení a pak aplikaci spustí.  Potom můžete pracovat s aplikací na zařízení.

  Zlepšení vývojové prostředí.  Sdílení aplikací, jako obrazovky [Mobizen] [ 20] vám může pomoci při vývoji aplikace platformy Android.  Mobizen projektů Android obrazovky na webový prohlížeč ve vašem počítači.

* **V emulátoru Androidu:** existují další konfigurační kroky potřebné při spuštění v emulátoru.

    Ujistěte se, že nasazujete virtuální zařízení, která má rozhraní Google API nastavenou jako cíl, jak je vidět ve Správci virtuální zařízení Android (AVD).

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Pokud chcete použít rychlejší x86 emulátoru, můžete [nainstalujte ovladač HAXM] [ 11] a nakonfigurujte emulátoru ji použít.

    Kliknutím na Přidat účet Google do zařízení s Androidem **aplikace** > **nastavení** > **přidejte účet**, postupujte podle pokynů.

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Spusťte aplikaci seznamu úkolů jako před a vložit novou položku úkolů. Tentokrát ikonu oznámení se zobrazí v oznamovací oblasti. Můžete otevřít panel oznámení k zobrazení textu v plném znění oznámení.

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

## <a name="optional-configure-and-run-on-ios"></a>(Volitelné) Nakonfigurujte a spusťte v systému iOS
Tato část se týká spuštění projektu Cordova na zařízení s iOS. Pokud nepracujete s zařízení s iOS, můžete tuto část přeskočit.

#### <a name="install-and-run-the-ios-remote-build-agent-on-a-mac-or-cloud-service"></a>Nainstalujte a spusťte agenta vzdáleného sestavení iOS Mac nebo cloudové služby
Před spuštěním aplikace Cordova v iOS pomocí sady Visual Studio projít kroky v [Průvodce nastavením iOS] [ 12] k instalaci a spuštění agenta vzdáleného sestavení.

Ujistěte se, že můžete vytvořit aplikaci pro iOS. K sestavení pro iOS ze sady Visual Studio je potřeba udělat kroky v Průvodci instalací. Pokud nemáte algoritmu Mac, můžete vytvořit pro iOS pomocí agenta vzdáleného sestavení na službě, jako je MacInCloud. Další informace najdete v tématu [spuštění aplikace pro iOS v cloudu][21].

> [!NOTE]
> XCode 7 nebo novější, je potřeba použít modul plug-in nabízené v systému iOS.

#### <a name="find-the-id-to-use-as-your-app-id"></a>Najít ID, aby vám poskytla vaše ID aplikace
Najít před registrace aplikace pro nabízená oznámení, otevřete config.xml v aplikaci Cordova `id` atribut hodnota v elementu pomůcky a zkopírujte jej pro pozdější použití. V následující soubor XML, je ID `io.cordova.myapp7777777`.

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
          version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

Později použijte tento identifikátor při vytvoření ID aplikace na portálu pro vývojáře Apple. Pokud vytvoříte jiné ID aplikace na portálu pro vývojáře, vyžaduje několik kroků navíc později v tomto kurzu. ID v elementu pomůcky se musí shodovat s ID aplikace na portálu pro vývojáře.

#### <a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Registraci aplikace pro nabízená oznámení na portál pro vývojáře společnosti Apple
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Podívejte se na video zobrazující podobný postup.](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

#### <a name="configure-azure-to-send-push-notifications"></a>Konfigurace Azure k odesílání nabízených oznámení
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

#### <a name="verify-that-your-app-id-matches-your-cordova-app"></a>Zkontrolujte, zda vaše ID aplikace odpovídá vaší aplikace Cordova
Pokud jste vytvořili už ve vašem účtu vývojáře Apple ID aplikace odpovídá ID elementu pomůcka v config.xml, můžete tento krok přeskočit. Pokud ID neshodují, proveďte následující kroky:

1. Odstraňte složku platformy ze svého projektu.
2. Odstraňte složku moduly plug-in z projektu.
3. Odstraňte složku node_modules ze svého projektu.
4. Aktualizujte atributu id elementu pomůcka v config.xml používat ID aplikace, kterou jste vytvořili ve vašem účtu vývojáře Apple.
5. Znovu sestavte projekt.

##### <a name="test-push-notifications-in-your-ios-app"></a>Nabízená oznámení v aplikaci s iOS
1. V sadě Visual Studio, ujistěte se, že **iOS** je vybrán jako cíl nasazení a potom vyberte **zařízení** ke spuštění na vašem připojené zařízení s iOS.

    Můžete spustit na zařízení s iOS připojené k vašemu počítači pomocí iTunes. Simulátoru iOS nabízená oznámení nepodporuje.

2. Stiskněte **spustit** tlačítko nebo **F5** v sadě Visual Studio pro sestavení projektu a spusťte aplikaci v zařízení s iOS, pak klikněte na tlačítko **OK** přijímat nabízená oznámení.

   > [!NOTE]
   > Aplikace požádá o potvrzení pro nabízená oznámení při prvním spuštění.

3. V aplikaci zadejte úlohu a potom klikněte na tlačítko plus (+) ikona.
4. Ověřte, že přijetí oznámení a potom kliknutím na tlačítko OK zavření oznámení.

## <a name="optional-configure-and-run-on-windows"></a>(Volitelné) Nakonfigurujte a spusťte v systému Windows
Tato část se týká spuštění projektu aplikace Apache Cordova na zařízení s Windows 10 (modul plug-in nabízené PhoneGap podporuje se ve Windows 10). Pokud nepracujete s zařízení se systémem Windows, můžete tuto část přeskočit.

#### <a name="register-your-windows-app-for-push-notifications-with-wns"></a>Registrace aplikace systému Windows pro nabízená oznámení s WNS
Použití možností úložiště v sadě Visual Studio, vyberte cíl Windows ze seznamu řešení platformy, jako je třeba **Windows x64** nebo **Windows x86** (vyhnout **Windows AnyCPU** pro nabízená oznámení).

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Podívejte se na video s podobným způsobem][13]

#### <a name="configure-the-notification-hub-for-wns"></a>Konfigurace centra oznámení pro WNS
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>Konfigurace aplikace Cordova pro podporu nabízených oznámení Windows
Otevřete návrháře konfigurace (klikněte pravým tlačítkem na config.xml a vyberte **Návrhář zobrazení**), vyberte **Windows** a klikněte na příkaz **Windows 10** pod  **Cílová verze Windows**.

Pro podporu nabízených oznámení do výchozí (ladění) sestaveních otevřete build.json souboru. Konfigurace ladění zkopírujte konfigurace "verze".

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

Po aktualizaci build.json by měl obsahovat následující kód:

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

Sestavte aplikaci a ověřte, že máte žádné chyby. Klientská aplikace by teď zaregistrovat pro oznámení z back-end mobilní aplikace. Tato část opakujte pro každý projekt Windows ve vašem řešení.

#### <a name="test-push-notifications-in-your-windows-app"></a>Nabízená oznámení v aplikaci Windows
V sadě Visual Studio, musí být vybrána, platforma Windows cíl nasazení, jako například **Windows x64** nebo **Windows x86**. Spustit aplikaci na počítač s Windows 10 hostování sady Visual Studio, použijte příkaz **místního počítače**.

Klikněte na tlačítko spustit pro sestavení projektu a spusťte aplikaci.

V aplikaci, zadejte název nové todoitem a pak klikněte na tlačítko plus (+) ikona Přidat.

Ověřte, že se při přidání položky přijato oznámení.

## <a name="next-steps"></a>Další kroky
* Přečtěte si informace o [Notification Hubs] [ 17] Další informace o nabízených oznámení.
* Pokud jste tak již neučinili, pokračovat v kurzu podle [přidání ověřování] [ 14] do vaší aplikace Apache Cordova.

Zjistěte, jak používat sady SDK.

* [Apache Cordova SDK][15]
* [ASP.NET Server SDK][1]
* [Node.js Server SDK][16]

<!-- Images -->
[img1]: ./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png

<!-- URLs -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: http://www.visualstudio.com/
[3]: https://azure.microsoft.com/pricing/free-trial/
[4]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[5]: app-service-mobile-cordova-get-started.md
[6]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[7]: https://developer.apple.com/programs/
[8]: https://developer.microsoft.com/en-us/store/register
[9]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub
[10]: https://www.npmjs.com/
[11]: https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM
[12]: http://taco.visualstudio.com/en-us/docs/ios-guide/
[13]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push
[14]: app-service-mobile-cordova-get-started-users.md
[15]: app-service-mobile-cordova-how-to-use-client-library.md
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[17]: ../notification-hubs/notification-hubs-push-notification-overview.md
[18]: https://console.developers.google.com/home/dashboard
[19]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[20]: https://www.mobizen.com/
[21]: http://taco.visualstudio.com/en-us/docs/build_ios_cloud/
