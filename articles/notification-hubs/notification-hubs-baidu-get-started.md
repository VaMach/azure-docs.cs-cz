<properties
    pageTitle="Začínáme s používáním Azure Notification Hubs s Baidu | Microsoft Azure"
    description="V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do zařízení se systémem Android pomocí Baidu."
    services="notification-hubs"
    documentationCenter="android"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="mobile-baidu"
    ms.workload="mobile"
    ms.date="05/05/2016"
    ms.author="wesmc"/>

# Začínáme s použitím Notification Hubs pomocí Baidu

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##Přehled

Nabídka cloudu Baidu představuje čínskou cloudovou službu, kterou můžete použít k zasílání nabízených oznámení na mobilní zařízení. Tato služba je užitečná zejména v Číně, kde je doručování nabízených oznámení Android komplexní z důvodu přítomnosti různých obchodů s aplikacemi a nabízených služeb, navíc k dostupnosti zařízení Android, která nejsou obvykle připojena k GCM (Google Cloud Messaging).

##Předpoklady

V tomto kurzu budete potřebovat následující:

+ Sada Android SDK (předpokládáme, že použijete Eclipse), kterou si můžete stáhnout z <a href="http://go.microsoft.com/fwlink/?LinkId=389797">lokality Android</a>
+ [Mobile Services Android SDK]
+ [Baidu Push Android SDK]

>[AZURE.NOTE] K dokončení tohoto kurzu potřebujete mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).


##Vytvořte účet Baidu

Chcete-li použít Baidu, musíte mít účet Baidu. Pokud již účet máte, přihlaste se na [portál Baidu] a přejděte k dalšímu kroku. Jinak pro vytvoření účtu Baidu postupujte podle pokynů níže.  

1. Přejděte na stránku [portál Baidu] a klikněte na odkaz **登录** (**Přihlášení**). Klikněte na tlačítko **立即注册** s spusťte proces registrace účtu.

    ![][1]

2. Zadejte požadované podrobnosti – telefon a e-mailovou adresu, heslo a ověřovací kód – a klikněte na tlačítko **Registrace**.

    ![][2]

3. Na e-mailovou adresu, kterou jste zadali, vám dorazí e-mail s odkazem k aktivaci Baidu.

    ![][3]

4. Přihlaste se k e-mailovému účtu, otevřete e-mail pro aktivaci Baidu a kliknutím na aktivační odkaz účet Baidu aktivujte.

    ![][4]

Po aktivaci účtu Baidu se přihlaste na [portál Baidu].

##Zaregistrujte se jako vývojář Baidu

1. Po přihlášení na [portál Baidu] klikněte na tlačítko **更多 >>** (**Další**).

    ![][5]

2. Posuňte se dolů v části **站长与开发者服务 (Správce webového serveru a služeb pro vývojáře)** a klikněte na tlačítko **百度开放云平台** (**Otevřená cloudová platforma Baidu**).

    ![][6]

3. Na další stránce klikněte na tlačítko **开发者服务** (**Služby pro vývojáře**) v pravém horním rohu.

    ![][7]

4. Na další stránce klikněte na tlačítko **注册开发者** (**Registrovaní vývojáři**) z nabídky v pravém horním rohu.

    ![][8]

5. Zadejte své jméno, popis a číslo mobilního telefonu pro příjem ověřovací textové zprávy a pak klikněte na tlačítko **送验证码** (**Odeslat ověřovací kód**). Všimněte si, že pro mezinárodní telefonní čísla bude nutné uvést kód země v závorkách. Například pro USA se bude jednat o číslo **(1) 1234567890**.

    ![][9]

6. Následně byste měli obdržet textovou zprávu s číslem ověření, jak je znázorněno v následujícím příkladu:

    ![][10]

7. Zadejte číslo ověření ze zprávy do **验证码** (**Potvrzovací kód**).

8. Nakonec dokončete registraci vývojářů přijetím podmínek smlouvy Baidu a kliknutím na **提交** (**Odeslat**). Při úspěšném dokončení registrace se zobrazí následující stránka:

    ![][11]

##Vytvořte projekt nabízených oznámení cloudu Baidu

Při vytváření projektu nabízených oznámení cloudu Baidu obdržíte své ID aplikace, klíč rozhraní API a tajný klíč.

1. Po přihlášení na [portál Baidu] klikněte na tlačítko **更多 >>** (**Další**).

    ![][5]

2. Posuňte se dolů v části **站长与开发者服务** (**Správce webového serveru a služeb pro vývojáře**) a klikněte na tlačítko **百度开放云平台** (**Otevřená cloudová platforma Baidu**).

    ![][6]

3. Na další stránce klikněte na tlačítko **开发者服务** (**Služby pro vývojáře**) v pravém horním rohu.

    ![][7]

4. Na další stránce klikněte na tlačítko **云推送** (**Cloud Push**) z části **云服务** (**Cloudové služby**).

    ![][12]

5. Jakmile jste registrovaný vývojář, zobrazí se v horní nabídce **管理控制台** (**Konzola pro správu**). Klikněte na tlačítko **开发者服务管理** (**Správa služeb pro vývojáře**).

    ![][13]

6. Na další stránce klikněte na tlačítko **创建工程** (**Vytvořit projekt**).

    ![][14]

7. Zadejte název aplikace a klikněte na tlačítko **创建** (**Vytvořit**).

    ![][15]

8. Po úspěšném vytvoření projektu nabízených oznámení cloudu Baidu se zobrazí stránka s **AppID**, **klíčem rozhraní API** a **tajný klíč**. Poznamenejte si klíč rozhraní API a tajný klíč, který použijeme později.

    ![][16]

9. Nakonfigurujte projekt pro nabízená oznámení kliknutím na **云推送** (**Cloud Push**) v levém podokně.

    ![][31]

10. Na další stránce klikněte na tlačítko **推送设置** (**Nastavení Push**).

    ![][32]  

11. Na stránce konfigurace přidejte název balíčku, který budete používat v projektu Android v poli **应用包名** (**Balíček aplikace**) a pak klikněte na tlačítko **保存设置** (**Uložit**).  

    ![][33]

Zobrazí se **保存成功!zpráva ** (**Úspěšně uloženo!**).

##Konfigurace centra oznámení

1. Přihlaste se k [portálu Azure Classic] a v dolní části obrazovky klikněte na **+ NOVÝ**.

2. Klikněte na tlačítko **aplikační služby**, klikněte na tlačítko **Sběrnice**, klikněte na tlačítko **Notification Hubs** a pak klikněte na tlačítko **Rychle vytvořit**.

3. Zadejte název pro vaše **Notification Hubs**, vyberte **Oblast** a **Obor názvů** kde bude toto centrum oznámení vytvořeno a pak klikněte na tlačítko **Vytvoření nového centra oznámení**.  

    ![][17]

4. Klikněte na obor názvů, ve kterém jste vytvořili centrum oznámení a pak klikněte na tlačítko **Notification Hubs** nahoře.

    ![][18]

5. Vyberte vytvořené centrum oznámení a pak klikněte na tlačítko **Konfigurovat** v hlavní nabídce.

    ![][19]

6. Přejděte dolů do části **Nastavení oznámení baidu** a zadejte klíč rozhraní API a tajný klíč, který jste dříve získali z konzoly Baidu pro váš projekt nabízených oznámení cloudu Baidu. Klikněte na **Uložit**.

    ![][20]

7. Klikněte na tlačítko **Řídicí panel** v horní části pro Centrum oznámení a klikněte na tlačítko **Zobrazení připojovacího řetězce**.

    ![][21]

8. Poznamenejte si **DefaultListenSharedAccessSignature** a **DefaultFullSharedAccessSignature** z okna **Přístup k informacím připojení**.

    ![][22]

##Připojte aplikaci k centru oznámení

1. V Eclipse ADT vytvořte nový projekt Android (**Soubor** > **Nový** > **Projekt aplikace Android**).

    ![][23]

2. Zadejte **Název aplikace** a ověřte, že **Minimální požadované SDK** verze je nastaveno na **Rozhraní API 16: Android 4.1**.

    ![][24]

3. Klikněte na tlačítko **Další** a pokračuje podle pokynů Průvodce, dokud se nezobrazí okno **Vytvořit aktivity**. Ujistěte se, že je zvolena možnost **Prázdné aktivity** a nakonec vyberte možnost **Dokončit** k vytvoření nové aplikace pro Android.

    ![][25]

4. Ujistěte se, že je možnost **Cíl sestavení projektu** správně nastavena.

    ![][26]

5. Stáhněte si soubor notification-hubs-0.4.jar z karty **Soubory** v části [Notification-Hubs-Android-SDK na panelu koše](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4). Přidejte soubor do složky **knihovny** projektu Eclipse a aktualizujte složku *knihovny*.

6. Stáhněte a rozbalte složku [Baidu Push Android SDK], otevřete složku **knihovny** a pak zkopírujte soubor jar **pushservice-x.y.z** a složky **armeabi** & **mips** ve složce **knihovny** do vaší aplikace Android.

7. Otevřete soubor **AndroidManifest.xml** vašeho projektu Android a přidejte oprávnění vyžadované Baidu SDK.

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />

8. Přidejte vlastnost **android:name** do svého prvku **aplikace** v souboru **AndroidManifest.xml** a nahraďte *názevvašehoprojektu* (například **com.example.BaiduTest**). Ujistěte se, že tento název projektu odpovídá názvu, který jste nakonfigurovali v konzole Baidu.

        <application android:name="yourprojectname.DemoApplication"

9. Přidejte následující konfigurace v rámci prvku aplikace po prvku aktivity **.MainActivity** a nahraďte *názevvašehoprojektu* (například **com.example.BaiduTest**):

        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>

9. Přidejte novou třídu s názvem **ConfigurationSettings.java** do projektu.

    ![][28]

    ![][29]

10. Přidejte do ní následující kód:

        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }

    Nastavte hodnotu **API_KEY** pomocí dříve načteného projektu Baidu, **NotificationHubName** s vaším jménem centra oznámení z portálu Azure Classic a **NotificationHubConnectionString** pomocí DefaultListenSharedAccessSignature z portálu Azure Classic.

11. Přidejte novou třídu s názvem **DemoApplication.java** a přidejte do ní následující kód:

        import com.baidu.frontia.FrontiaApplication;

        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }

12. Přidejte další novou třídu s názvem **MyPushMessageReceiver.java** a přidejte do ní následující kód. Toto je třída, která zpracovává nabízená oznámení přijatá ze serveru nabízených oznámení Baidu.

        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG to Log */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();

            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;

                try {
                 if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }

                registerWithNotificationHubs();
            }

            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                + ConfigurationSettings.NotificationHubName + "'"
                                + " with UserId - '"
                                + mUserId + "' and Channel Id - '"
                                + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }

            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }

            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }

            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }

13. Otevřete soubor **MainActivity.java** a přidejte následující metodu **onCreate**:

            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);

14. Otevřete následující prohlášení importu v horní části:

            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

##Odešlete oznámení do aplikace


Příjem oznámení ve vaší aplikaci můžete otestovat zasláním oznámení na portálu Azure Classic prostřednictvím karty ladění v centru oznámení, jak je znázorněno na obrazovce níže.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-debug.png)

Nabízená oznámení se většinou posílají ve službě back-end, jako je služba Mobile Services, nebo v technologii ASP.NET pomocí kompatibilní knihovny. Můžete také použít rozhraní API REST k přímému zasílání oznámení, pokud pro vaše prostředí back-end není dostupná žádná knihovna.

V tomto kurzu nebudeme dělat nic složitého a jednoduše předvedeme testování vaší klientské aplikace pomocí odesílání oznámení v sadě SDK .NET pro centra oznámení v konzolové aplikaci, místo back-end služby. Jako další krok pro odesílání oznámení z backendu ASP.NET doporučujeme tutoriál [Použití centra oznámení pro nabízená oznámení uživatelům](notification-hubs-aspnet-backend-windows-dotnet-notify-users.md). Následující přístupy lze však použít pro zasílání oznámení:

* **Rozhraní REST**: oznámení můžete podporovat na jakékoli backend platformě pomocí [rozhraní REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).

* **Microsoft Azure oznámení centra .NET SDK**: Ve správci balíčků Nuget pro Visual Studio spusťte položku [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js**: [Jak používat Notification Hubs z Node.js](notification-hubs-nodejs-how-to-use-notification-hubs.md).

* **Mobilní služby Azure**: pro příklad odesílání oznámení z mobilních služeb Azure back-end, které jsou integrovány v centrech oznámení, naleznete v tématu [Přidat nabízená oznámení do vaší aplikace pro mobilní služby](../mobile-services/mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md).

* **Java / PHP**: příklad odesílání oznámení pomocí rozhraní API REST naleznete v části „Použití centra oznámení z Java/PHP“ ([Java](notification-hubs-java-backend-how-to.md) | [PHP](notification-hubs-php-backend-how-to.md)).

##(Volitelné) Odesílání oznámení z konzoly aplikace .NET.

V této části ukážeme odesílání oznámení pomocí konzolové aplikace .NET.

1. Vytvořte novou konzolovou aplikaci Visual C#:

    ![][30]

2. V okně konzoly Správce balíčků nastavte **Výchozí projekt** na nový projekt konzolové aplikace a pak v okně konzoly spusťte následující příkaz:

        Install-Package Microsoft.Azure.NotificationHubs

    Ten přidá odkaz na sadu SDK centra oznámení Azure pomocí <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

3. Otevřete soubor **Program.cs** a přidejte následující možnost pomocí příkazu:

        using Microsoft.Azure.NotificationHubs;

4. Do své třídy `Program` přidejte následující metodu a nahraďte *DefaultFullSharedAccessSignatureSASConnectionString* a *NotificationHubName* pomocí hodnot, které máte.

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }

5. Přidejte následující řádky do vaší **Hlavní** metody:

         SendNotificationAsync();
         Console.ReadLine();

##Testování aplikace

K testování této aplikace pomocí skutečného telefonu, jednoduše připojte telefon k počítači pomocí kabelu USB. Tento postup načte aplikaci do připojeného telefonu.

K otestování této aplikace pomocí emulátoru na horním panelu nástrojů Eclipse klikněte na tlačítko **Spustit** a pak vyberte svou aplikaci. Spustí se emulátor a potom se načte a spustí aplikace.

Aplikace načte „userID“ a „channelID“ ze služby nabízených oznámení Baidu a zaregistruje centrum oznámení.

Pro odeslání testovacího oznámení můžete použít kartu ladění na portálu Azure Classic. Pokud jste vytvořili konzolovou aplikaci .NET pro Visual Studio, jednoduše ke spuštění aplikace stiskněte klávesu F5 ve Visual Studiu. Aplikace odešle upozornění, které se zobrazí v horní oznamovací oblasti na emulátoru nebo zařízení.


<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Baidu Push Android SDK]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[portálu Azure Classic]: https://manage.windowsazure.com/
[portál Baidu]: http://www.baidu.com/



<!--HONumber=Jun16_HO2-->


