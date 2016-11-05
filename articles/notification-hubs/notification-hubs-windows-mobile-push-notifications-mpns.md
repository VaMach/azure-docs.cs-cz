---
title: Odesílání nabízených oznámení pomocí Azure Notification Hubs ve Windows Phone | Microsoft Docs
description: V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace Windows Phone 8 nebo Windows Phone 8.1 Silverlight.
services: notification-hubs
documentationcenter: windows
keywords: nabízené oznámení,nabízená oznámení,nabízení windows phone
author: wesmc7777
manager: erikre
editor: erikre

ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: wesmc

---
# Odesílání nabízených oznámení pomocí Azure Notification Hubs ve Windows Phone
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## Přehled
> [!NOTE]
> K dokončení tohoto kurzu potřebujete mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).
> 
> 

V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace Windows Phone 8 nebo Windows Phone 8.1 Silverlight. Pokud cílíte na Windows Phone 8.1 (ne Silverlight), přečtěte si informace k verzi [Univerzální pro Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
V tomto kurzu vytvoříte prázdnou aplikaci pro Windows Phone 8, která obdrží nabízená oznámení pomocí služby nabízených oznámení Microsoft (MPNS). Jakmile budete hotovi, budete moci používat vaše centra oznámení k vysílání nabízených oznámení pro všechna zařízení používající vaši aplikaci.

> [!NOTE]
> Sada SDK centra oznámení Windows Phone nepodporuje použití služby nabízených oznámení Windows (WNS) s aplikacemi pro Windows Phone 8.1 Silverlight. Chcete-li používat WNS (namísto MPNS) s aplikacemi pro Windows Phone 8.1 Silverlight, postupujte podle [kurzu Centra oznámení – Windows Phone Silverlight], který používá rozhraní REST API.
> 
> 

Tento kurz představuje scénář jednoduchého vysílání přes centra oznámení.

## Požadavky
V tomto kurzu budete potřebovat následující:

* [Visual Studio 2012 Express pro Windows Phone] nebo novější verzi

Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro aplikace Windows Phone 8.

## Vytvoření centra oznámení
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Klikněte na část <b>Služby oznámení</b> (v rámci <i>Nastavení</i>), klikněte na položku <b>Windows Phone (MPNS)</b> a pak klikněte na políčko <b>Povolit neověřené nabízená oznámení</b>.</p>
</li>
</ol>

&emsp;&emsp;![Portál Azure – povolit neověřená nabízená oznámení](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

Centrum se teď vytvoří a nakonfiguruje pro odeslání neověřeného oznámení pro Windows Phone.

> [!NOTE]
> Tento kurz využívá MPNS v neověřeném režimu. Neověřený režim MPNS přichází s omezeními na oznámení, která můžete odeslat na každý kanál. Centra oznámení podporují [ověřený režim MPNS](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) povolením odeslání vašeho certifikátu.
> 
> 

## Připojování aplikace k centru oznámení
1. V sadě Visual Studio vytvořte novou aplikaci pro Windows Phone 8.
   
    ![Visual Studio – Nový projekt – aplikace Windows Phone][13]
   
    Ve Visual Studio 2013 Update 2 nebo novější verzi místo toho vytvořte aplikaci Windows Phone Silverlight.
   
    ![Visual Studio – Nový projekt – Prázdná aplikace – Windows Phone Silverlight][11]
2. Ve Visual Studiu klikněte pravým tlačítkem na řešení a potom na **Spravovat balíčky NuGet**.
   
    Zobrazí se dialogové okno **Správa balíčků NuGet**.
3. Vyhledejte `WindowsAzure.Messaging.Managed` a klikněte na tlačítko **Instalovat** a pak přijměte podmínky použití.
   
    ![Visual Studio – Správce balíčků NuGet][20]
   
    Tento správce stáhne, nainstaluje a přidá odkaz na knihovnu zasílání zpráv Azure pro Windows pomocí <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">balíčku NuGet WindowsAzure.Messaging.Managed</a>.
4. Otevřete soubor App.xaml.cs a přidejte následující příkazy `using`:
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
5. Přidejte následující kód v horní části metody **Application_Launching** v souboru App.xaml.cs:
   
        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }
   
        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());
   
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });
   
   > [!NOTE]
   > Hodnota **MyPushChannel** je index, který se použije k vyhledání existujícího kanálu v kolekci [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx). Pokud zde není k dispozici, vytvořte novou položku s tímto názvem.
   > 
   > 
   
    Nezapomeňte vložit název rozbočovače a připojovací řetězec s názvem **DefaultListenSharedAccessSignature**, který jste získali v předchozím oddílu.
    Tento kód načte identifikátor URI kanálu pro aplikaci z MPNS a pak zaregistruje tento kanál URI pomocí centra oznámení. Také zaručuje, že kanál URI je registrován v centru oznámení pokaždé, když je aplikace spuštěna.
   
   > [!NOTE]
   > V tomto kurzu se odešle informační zpráva do zařízení. Při odesílání oznámení dlaždice musíte místo toho volat metodu **BindToShellTile** na kanálu. Pro podporu obou oznámení a oznámení v dlaždici volejte jak metodu **BindToShellTile**, tak i metodu **BindToShellToast**.
   > 
   > 
6. V Průzkumníku řešení rozbalte **Vlastnosti**, otevřete soubor `WMAppManifest.xml`, klikněte na kartu **Možnosti** a ujistěte se, že je zaškrtnuta schopnost **ID_CAP_PUSH_NOTIFICATION**.
   
    ![Visual Studio – možnosti aplikace pro Windows Phone][14]
   
    Tato možnost zajistí, že vaše aplikace může přijímat nabízená oznámení Bez toho se nezdaří jakýkoliv pokus o odeslání nabízených oznámení do aplikace.
7. Stiskněte klávesu `F5` a spusťte aplikaci.
   
    V aplikaci se zobrazí zpráva registrace.
8. Zavřete aplikaci.  
   
   > [!NOTE]
   > Pro příjem nabízených oznámení nesmí být aplikace spuštěná v popředí.
   > 
   > 

## Odesílání nabízených oznámení z backendu
Pomocí našeho veřejného <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">rozhraní REST</a> je možné pomocí center oznámení posílat nabízená oznámení z jakéhokoli backendu. V tomto kurzu zašlete nabízená oznámení pomocí konzolové aplikace .NET. 

Příklad odesílání nabízených oznámení z backendu ASP.NET WebAPI, který je integrovaný do Notification Hubs, najdete v článku [Azure Notification Hubs upozorňují uživatele pomocí backendu .NET](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).  

Příklad odesílání nabízených oznámení pomocí [rozhraní REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx) najdete v článku [Jak používat Notification Hubs z Javy](notification-hubs-java-push-notification-tutorial.md) a [Jak používat Notification Hubs z PHP](notification-hubs-php-push-notification-tutorial.md).

1. Klikněte pravým tlačítkem myši na řešení, vyberte možnost **Přidat** a **Nový projekt...** a pak v části **Visual C#** klikněte na tlačítko **Windows** a **Konzolové aplikace** a klikněte na tlačítko **OK**.
   
    ![Visual Studio – Nový projekt – Konzolová aplikace][6]
   
    Tento postup přidá novou aplikaci Visual C# do řešení. Tento postup také můžete využít v samostatném řešení.
2. Klikněte na položku **Nástroje**, klikněte na **Správce balíčků knihoven** a pak na **Konzola správce balíčků**.
   
    Tím se zobrazí Konzola Správce balíčků.
3. V okně **konzoly Správce balíčků** nastavte **Výchozí projekt** na nový projekt konzolové aplikace a pak v okně konzoly spusťte následující příkaz:
   
       Install-Package Microsoft.Azure.NotificationHubs
   
   Ten přidá odkaz na sadu SDK centra oznámení Azure pomocí <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">balíčku Microsoft.Azure.Notification Hubs NuGet</a>.
4. Otevřete soubor `Program.cs` a přidejte následující příkaz `using`:
   
        using Microsoft.Azure.NotificationHubs;
5. Do třídy `Program` přidejte následující metodu:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            string toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                   "<wp:Toast>" +
                        "<wp:Text1>Hello from a .NET App!</wp:Text1>" +
                   "</wp:Toast> " +
                "</wp:Notification>";
            await hub.SendMpnsNativeNotificationAsync(toast);
        }
   
    Zástupný symbol `<hub name>` je nutné nahradit názvem centra oznámení, který se zobrazí na portálu. Také nahraďte zástupný symbol připojovacího řetězce připojovacím řetězcem s názvem **DefaultFullSharedAccessSignature**, který jste získali v části „Konfigurace centra oznámení“.
   
   > [!NOTE]
   > Ujistěte se, že používáte připojovací řetězec s **úplným** přístupem, nikoli s přístupem **Naslouchat**. Řetězec s přístupem k naslouchání neposkytuje oprávnění k zasílání nabízených oznámení.
   > 
   > 
6. Do metody `Main` přidejte následující řádek:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Se spuštěným emulátorem systému Windows Phone a zavřenou aplikací nastavte projekt konzolové aplikace jako výchozí projekt po spuštění a pak stiskněte klávesu `F5` pro spuštění aplikace.
   
    Obdržíte nabízená oznámení. Klepnutím na informační nápis načtete aplikaci.

Na webu MSDN můžete najít všechny možné datové části v tématech [katalog informačních zpráv] a [katalog dlaždic].

## Další kroky
V tomto jednoduchém příkladu jste vysílali nabízená oznámení pro všechna vaše zařízení Windows Phone 8. 

Chcete-li se zaměřit na konkrétní uživatele, využijte kurz [Použití centra oznámení pro nabízená oznámení uživatelům]. 

Pokud chcete segmentovat uživatele podle zájmových skupin, můžete si přečíst kurz [Používání centra oznámení k odesílání novinek]. 

Další informace o centrech oznámení najdete v [Průvodce centry oznámení].

<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Visual Studio 2012 Express pro Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[Průvodce centry oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
[Režim ověření MPNS]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[Použití centra oznámení pro nabízená oznámení uživatelům]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Používání centra oznámení k odesílání novinek]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md
[katalog informačních zpráv]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[katalog dlaždic]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[kurzu Centra oznámení – Windows Phone Silverlight]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp




<!--HONumber=Oct16_HO1-->


