<properties
    pageTitle="Začínáme používat centra oznámení Azure pro aplikace Windows Store | Microsoft Azure"
    description="V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace Windows Store nebo Windows Phone 8.1 (bez Silverlight)."
    services="notification-hubs"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor="dwrede"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="03/28/2016"
    ms.author="wesmc"/>

# Začínáme používat centra oznámení pro aplikace Windows Store

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##Přehled

V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace Windows Store nebo Windows Phone 8.1 (bez Silverlight). Pokud cílíte na platformu Windows Phone 8.1 Silverlight, přejděte na verzi [Windows Phone](notification-hubs-windows-phone-get-started.md).
V tomto kurzu vytvoříte prázdnou aplikaci pro Windows Store, která obdrží nabízená oznámení pomocí služby nabízených oznámení Windows (WNS). Jakmile budete hotovi, budete moci používat vaše centra oznámení k vysílání nabízených oznámení pro všechna zařízení používající vaši aplikaci.


## Než začnete

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Dokončený kód v tomto kurzu lze najít v části Github [zde](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).



##Předpoklady

V tomto kurzu budete potřebovat následující:

+ Microsoft Visual Studio Express 2013 pro Windows s aktualizací 2 nebo novější<br/>Tato verze sady Visual Studio je nezbytná k vytvoření projektu univerzální aplikace. Pokud chcete vytvořit aplikaci pro Windows Store, budete potřebovat Visual Studio 2012 Express pro Windows 8.

+ Aktivní účet Windows Store

+ Aktivní účet Azure <br/>Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).

Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro aplikace Windows Store.

##Registrace aplikace pro Windows Store

Chcete-li odeslat nabízená oznámení do aplikace Windows Store, musíte přidružit aplikaci k Windows Store. Pak musíte nakonfigurovat centrum oznámení pro integraci s WNS.

1. Pokud jste ještě aplikaci nezaregistrovali, přejděte do umístění [Windows Dev Center](http://go.microsoft.com/fwlink/p/?LinkID=266582), přihlaste se pomocí účtu Microsoft a pak klikněte na tlačítko **Vytvořit novou aplikaci**.


2. Zadejte název aplikace a klikněte na tlačítko **Rezervovat název aplikace**.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hubs-win8-app-name.png)

    Tím se vytvoří nová registrace Windows Store pro vaši aplikaci.

3. V sadě Visual Studio vytvořte nový projekt aplikace Visual C# Store pomocí šablony **Prázdná aplikace**.

    ![][2]

4. V Průzkumníku řešení klikněte pravým tlačítkem na projekt aplikace Windows Store, klikněte na tlačítko **Úložiště** a pak klikněte na tlačítko **Přidružit aplikaci ve Store...**.

    ![][3]

    Zobrazí se průvodce **Přidružením aplikace k Windows Store**.

5. V průvodci klikněte na tlačítko **Přihlásit** a pak se přihlaste pomocí svého účtu Microsoft.

6. Klikněte na aplikaci zaregistrovanou v kroku 2, klikněte na tlačítko **Další** a pak klikněte na tlačítko **Přidružit**.

    ![][4]

    Tento postup přidá požadované informace o registraci Windows Store do manifestu aplikace.

7. (Volitelné) Opakujte kroky 4 až 6 pro projekt aplikace Windows Phone Store.  

8. Zpět na stránce [Windows Dev Center](http://go.microsoft.com/fwlink/p/?LinkID=266582) pro novou aplikaci klikněte na tlačítko **Služby**, klikněte na tlačítko **Nabízená oznámení** a pak klikněte na tlačítko **Server služby Live** pod položkou **Služby nabízených oznámení Windows (WNS) a mobilní služby Microsoft Azure**.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hubs-win8-app-live-services.png)

9. Na kartě **Nastavení aplikace** si poznamenejte hodnoty **Sdíleného tajného klíče klienta** a **Identifikátor balíčku zabezpečení (SID)**.

    ![][6]

    > [AZURE.WARNING]
    Tajný klíč klienta a SID balíčku jsou důležitá pověření zabezpečení. Tyto hodnoty s nikým nesdílejte ani je nedistribuujte s vaší aplikací.

##Konfigurace centra oznámení

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">
<li><p>Vyberte kartu <b>Konfigurovat</b> v horní části, zadejte hodnoty <b>Tajný klíč klienta</b> a <b>SID balíčku</b>, které jste získali z WNS v předchozí části a pak klikněte na tlačítko <b>Uložit</b>.</p>
</li>
</ol>

&emsp;&emsp;![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)


Vaše centrum oznámení je nyní nakonfigurováno pro práci se službou WNS. Zároveň máte připojovací řetězce, pomocí kterých můžete svou aplikaci zaregistrovat pro odesílání oznámení.

##Připojte aplikaci k centru oznámení

1. Ve Visual Studiu klikněte pravým tlačítkem na řešení a potom na **Spravovat balíčky NuGet**.

    Zobrazí se dialogové okno **Správa balíčků NuGet**.

2. Vyhledejte `WindowsAzure.Messaging.Managed` a klikněte na tlačítko **instalovat**, vyberte všechny projekty v řešení a přijměte podmínky použití.

    ![][20]

    Tento správce stáhne, nainstaluje a přidá odkaz na všechny projekty do knihovny zasílání zpráv Azure pro Windows pomocí <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">balíčku NuGet WindowsAzure.Messaging.Managed</a>.

3. Otevřete soubor projektu App.xaml.cs a přidejte následující příkazy `using`: V univerzálním projektu se tento soubor nachází ve složce `<project_name>.Shared`.

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;



4. Také do souboru App.xaml.cs přidejte následující definici metody **InitNotificationsAsync** do třídy **Aplikace**:

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            var hub = new NotificationHub("<hub name>", "<connection string with listen access>");
            var result = await hub.RegisterNativeAsync(channel.Uri);

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }

        }

    Tento kód načte identifikátor URI kanálu pro aplikaci z WNS a pak zaregistruje tento kanál URI pomocí centra oznámení.

    >[AZURE.NOTE]Ujistěte se, zda jste zástupný symbol „název centra“ nahradili názvem centra oznámení, které se zobrazí v části [portál Azure Classic] na kartě **Centra oznámení** (například **mynotificationhub2** v předchozím příkladu). Také nahraďte zástupný symbol připojovacího řetězce připojovacím řetězcem **DefaultListenSharedAccessSignature**, který jste získali v předchozí části.

5. V horní části **OnLaunched** obslužné rutiny událostí v souboru App.xaml.cs přidejte následující volání do nové metody **InitNotificationsAsync**:

        InitNotificationsAsync();

    Tento postup zaručuje, že bude kanál URI registrován v centru oznámení při každém spuštění aplikace.

6. V Průzkumníku řešení klikněte dvakrát na **Package.appxmanifest** pro aplikaci Windows Store a pak v části **Oznámení** nastavte položku **Možnost informačního nápisu** na **Ano**:

    ![][18]

    V nabídce **Soubor** klikněte na **Uložit vše**.

7. (Volitelné) Opakujte předchozí krok pro projekt aplikace Windows Phone Store.

8. Stiskněte klávesu **F5** a spusťte aplikaci. Zobrazí se automaticky otevíraný dialog, který obsahuje registrační klíč.

    ![][19]

9. (Volitelné) Opakujte předchozí krok a spusťte projekt Windows Phone pro registraci aplikace do zařízení Windows Phone.



Vaše aplikace je teď připravena přijímat oznámení informačního nápisu.

##Odeslat oznámení 

Příjem oznámení ve vaší aplikaci můžete otestovat zasláním oznámení na [portál Azure Classic] prostřednictvím karty ladění v centru oznámení, jak je znázorněno na obrazovce níže.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-debug.png)

Nabízená oznámení se většinou posílají ve službě back-end, jako je služba Mobile Services, nebo v technologii ASP.NET pomocí kompatibilní knihovny. Můžete také použít rozhraní API REST k přímému zasílání oznámení, pokud pro vaše prostředí back-end není dostupná žádná knihovna. 

V tomto kurzu nebudeme dělat nic složitého a jednoduše předvedeme testování vaší klientské aplikace pomocí odesílání oznámení v sadě SDK .NET pro centra oznámení v konzolové aplikaci, místo back-end služby. Jako další krok pro odesílání oznámení z backendu ASP.NET doporučujeme tutoriál [Použití centra oznámení pro nabízená oznámení uživatelům]. Následující přístupy lze však použít pro zasílání oznámení:

* **Rozhraní REST**: oznámení můžete podporovat na jakékoli backend platformě pomocí [rozhraní REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).

* **Microsoft Azure oznámení centra .NET SDK**: Ve správci balíčků Nuget pro Visual Studio spusťte položku [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js**: [Jak používat Notification Hubs z Node.js](notification-hubs-nodejs-how-to-use-notification-hubs.md).

* **Azure Mobile Apps**: Příklad zasílání oznámení ze služby Azure Mobile Apps integrované se službou Notification Hubs najdete v tématu [Přidání nabízených oznámení do mobilních aplikací](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).

* **Java / PHP**: příklad odesílání oznámení pomocí rozhraní API REST naleznete v části „Použití centra oznámení z Java/PHP“ ([Java](notification-hubs-java-backend-how-to.md) | [PHP](notification-hubs-php-backend-how-to.md)).


## (Volitelné) Odesílání oznámení z konzoly aplikace


K odeslání oznámení pomocí konzolové aplikace .NET postupujte následovně. 

1. Klikněte pravým tlačítkem myši na řešení, vyberte možnost **Přidat** a **Nový projekt...** a pak v části **Visual C#** klikněte na tlačítko **Windows** a **Konzolové aplikace** a klikněte na tlačítko **OK**.

    ![][13]

    Tento postup přidá novou aplikaci Visual C# do řešení. Tento postup také můžete využít v samostatném řešení.

2. Ve Visual Studiu klikněte na položku **Nástroje**, klikněte na **Správce balíčků NuGet** a pak klikněte na **Konzola Správce balíčků**.

    Tím se zobrazí Konzola Správce balíčků ve Visual Studiu.

3. V okně konzoly Správce balíčků nastavte **Výchozí projekt** na nový projekt konzolové aplikace a pak v okně konzoly spusťte následující příkaz:

        Install-Package Microsoft.Azure.NotificationHubs

    Ten přidá odkaz na sadu SDK centra oznámení Azure pomocí <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)


4. Otevřete soubor Program.cs a přidejte následující příkaz `using`:

        using Microsoft.Azure.NotificationHubs;

5. Přidejte následující metodu do třídy **Program**:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">Hello from a .NET App!</text></binding></visual></toast>";
            await hub.SendWindowsNativeNotificationAsync(toast);
        }

    Ujistěte se, zda jste zástupný symbol „název centra“ nahradili názvem centra oznámení, které se zobrazí v části [portál Azure Classic] na kartě **Centra oznámení**. Také nahraďte zástupný symbol připojovacího řetězce připojovacím řetězcem s názvem **DefaultFullSharedAccessSignature**, který jste získali v části „Konfigurace centra oznámení“.

    >[AZURE.NOTE]Ujistěte se, že používáte připojovací řetězec s **Úplným** přístupem, nikoli s přístupem **Naslouchat**. Řetězec s přístupem k naslouchání neposkytuje oprávnění k zasílání oznámení.

6. Přidejte následující řádky do metody **Hlavní**:

         SendNotificationAsync();
         Console.ReadLine();

7. Klikněte pravým tlačítkem na projekt konzolové aplikace ve Visual Studiu a kliknutím na tlačítko **Nastavit jako spouštěný projekt** nastavte projekt jako spouštěný. Pak stiskněte klávesu **F5** a spusťte aplikaci.

    ![][14]

    Na všech registrovaných zařízeních se zobrazí informační zprávy. Kliknutí nebo klepnutí na informační nápis načte aplikaci.

Všechny podporované datové části v tématech [katalog informační zprávy], [katalog dlaždic] a [přehled odznaků] naleznete na webu MSDN.

##Další kroky

V tomto jednoduchém příkladu jste zaslali oznámení vysílání pro všechna vaše zařízení systému Windows pomocí portálu nebo aplikace konzoly. Jako další krok doporučujeme tutoriál [Použití centra oznámení pro nabízená oznámení uživatelům]. Zobrazí se postup odesílání oznámení z ASP.NET back-end pomocí značek pro cílové konkrétní uživatele.

Pokud chcete segmentovat uživatele podle zájmových skupin, přečtěte si kurz [Používání centra oznámení k odesílání novinek]. 

Další obecné informace o centrech oznámení naleznete v tématu naše [Pokyny centra oznámení].






<!-- Images. -->
[2]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-windows-universal-app.png
[3]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-associate-win8-app.png
[4]: ./media/notification-hubs-windows-store-dotnet-get-started/mobile-services-select-app-name.png
[6]: ./media/notification-hubs-windows-store-dotnet-get-started/mobile-services-win8-app-push-auth.png
[11]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[15]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-scheduler1.png
[16]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-scheduler2.png
[18]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-win8-app-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->
[portál Azure Classic]: https://manage.windowsazure.com/
[Pokyny centra oznámení]: http://msdn.microsoft.com/library/jj927170.aspx

[Použití centra oznámení pro nabízená oznámení uživatelům]: notification-hubs-aspnet-backend-windows-dotnet-notify-users.md
[Používání centra oznámení k odesílání novinek]: notification-hubs-windows-store-dotnet-send-breaking-news.md

[katalog informační zprávy]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[katalog dlaždic]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[přehled oznámení]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx



<!--HONumber=Jun16_HO2-->


