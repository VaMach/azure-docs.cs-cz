---
title: "Začínáme používat službu Azure Notification Hubs pro aplikace Univerzální platformy Windows | Dokumentace Microsoftu"
description: "V tomto kurzu zjistíte, jak pomocí služby Azure Notification Hubs odesílat nabízená oznámení do aplikace Univerzální platformy Windows."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: erikre
ms.assetid: cf307cf3-8c58-4628-9c63-8751e6a0ef43
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.translationtype: Human Translation
ms.sourcegitcommit: b1d56fcfb472e5eae9d2f01a820f72f8eab9ef08
ms.openlocfilehash: 9353ad6df121ebd2e92a5d34214c32e852ed60a3
ms.contentlocale: cs-cz
ms.lasthandoff: 07/06/2017


---
# <a name="getting-started-with-notification-hubs-for-windows-universal-platform-apps"></a>Začínáme používat službu Notification Hubs pro aplikace Univerzální platformy Windows
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Přehled
V tomto kurzu zjistíte, jak pomocí služby Azure Notification Hubs k odesílat nabízená oznámení do aplikace Univerzální platformy Windows (UPW).

V tomto kurzu vytvoříte prázdnou aplikaci pro Windows Store, která obdrží nabízená oznámení pomocí služby nabízených oznámení Windows (WNS). Jakmile budete hotovi, budete moci používat vaše centra oznámení k vysílání nabízených oznámení pro všechna zařízení používající vaši aplikaci.

## <a name="before-you-begin"></a>Než začnete
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Dokončený kód v tomto kurzu lze najít v části Github [zde](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).

## <a name="prerequisites"></a>Požadavky
V tomto kurzu budete potřebovat následující:

* [Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) nebo novější
* [Nainstalované nástroje pro vývoj univerzálních aplikací pro Windows](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
* Aktivní účet Azure <br/>Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).
* Aktivní účet Windows Store

Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy služby Notification Hubs pro aplikace Univerzální platformy Windows.

## <a name="register-your-app-for-the-windows-store"></a>Registrace aplikace pro Windows Store
Chcete-li odesílat nabízená oznámení do aplikací UWP, musíte aplikaci přidružit k Windows Storu. Pak musíte nakonfigurovat centrum oznámení pro integraci s WNS.

1. Pokud jste ještě aplikaci nezaregistrovali, přejděte na [Windows Dev Center](https://dev.windows.com/overview), přihlaste se pomocí účtu Microsoft a pak klikněte na tlačítko **Vytvořit novou aplikaci**.
2. Zadejte název aplikace a klikněte na tlačítko **Rezervovat název aplikace**.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hubs-win8-app-name.png)
   
   Tím se vytvoří nová registrace Windows Store pro vaši aplikaci.
3. V sadě Visual Studio vytvořte nový projekt aplikace Visual C# Store pomocí šablony **Prázdná aplikace** a klikněte na **OK**.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-windows-universal-app.png)
4. Přijměte výchozí hodnoty cíle a minimální verze platformy.
5. V Průzkumníku řešení klikněte pravým tlačítkem na projekt aplikace Windows Store, klikněte na tlačítko **Úložiště** a pak klikněte na tlačítko **Přidružit aplikaci ve Store...**.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-associate-win8-app.png)

   Zobrazí se průvodce **Přidružením aplikace k Windows Store**.

1. V průvodci klikněte na tlačítko **Přihlásit** a pak se přihlaste pomocí svého účtu Microsoft.
2. Klikněte na aplikaci zaregistrovanou v kroku 2, klikněte na tlačítko **Další** a pak klikněte na tlačítko **Přidružit**.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-associate-app-name.png)
   
   Tento postup přidá požadované informace o registraci Windows Store do manifestu aplikace.
3. Zpátky na stránce [Windows Dev Center](http://go.microsoft.com/fwlink/p/?LinkID=266582) pro novou aplikaci klikněte na **Services** (Služby), klikněte na **Push notifications** (Nabízená oznámení) a potom klikněte na **Live Services site** (Server služby Live) pod položkou **Windows Push Notification Services (WNS) and Microsoft Azure Mobile Apps** (Služby nabízených oznámení Windows (WNS) a Microsoft Azure Mobile Apps).
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hubs-uwp-app-live-services.png)
4. Na registrační stránce si poznamenejte heslo **Tajný klíč aplikace** a **Identifikátor zabezpečení (SID) balíčku**, které najdete v nastavení platformy **Windows Store**.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hubs-uwp-app-push-auth.png)

     > [AZURE.WARNING]
    Tajný klíč aplikace a SID balíčku jsou důležité přihlašovací údaje zabezpečení. Tyto hodnoty s nikým nesdílejte ani je nedistribuujte s vaší aplikací.

## <a name="configure-your-notification-hub"></a>Konfigurace centra oznámení
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Vyberte možnosti <b>Notification Services</b> a <b>Služba nabízených oznámení Windows (WNS)</b>. Potom zadejte <b>Tajný klíč aplikace</b> do pole <b>Klíč zabezpečení</b>. Zadejte hodnotu <b>Identifikátor SID balíčku</b>, kterou jste ve službě WNS získali v předchozí části, a klikněte na <b>Uložit</b>.</p>
</li>
</ol>

&emsp;&emsp;![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Vaše centrum oznámení je nyní nakonfigurováno pro práci se službou WNS. Zároveň máte připojovací řetězce, pomocí kterých můžete svou aplikaci zaregistrovat pro odesílání oznámení.

## <a name="connect-your-app-to-the-notification-hub"></a>Připojte aplikaci k centru oznámení
1. Ve Visual Studiu klikněte pravým tlačítkem na řešení a potom na **Spravovat balíčky NuGet**.
   
    Zobrazí se dialogové okno **Správa balíčků NuGet**.
2. Vyhledejte `WindowsAzure.Messaging.Managed`, klikněte na **Instalovat** a potom přijměte podmínky použití.
   
    ![][20]
   
    Tento správce stáhne, nainstaluje a přidá odkaz na knihovnu zasílání zpráv Azure pro Windows pomocí <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">balíčku NuGet WindowsAzure.Messaging.Managed</a>.
3. Otevřete soubor projektu App.xaml.cs a přidejte následující příkazy `using`: 
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;
4. Také do souboru App.xaml.cs přidejte následující definici metody **InitNotificationsAsync** do třídy **Aplikace**:
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("< your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
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
   
   > [!NOTE]
   > Zástupný text „název vašeho centra“ je potřeba nahradit názvem centra oznámení, který se zobrazí na webu Azure Portal. Taky nahraďte zástupný symbol připojovacího řetězce připojovacím řetězcem **DefaultListenSharedAccessSignature**, který jste získali v předchozí části na stránce **Zásady přístupu** vašeho centra oznámení.
   > 
   > 
5. V horní části **OnLaunched** obslužné rutiny událostí v souboru App.xaml.cs přidejte následující volání do nové metody **InitNotificationsAsync**:
   
        InitNotificationsAsync();
   
    Tento postup zaručuje, že bude kanál URI registrován v centru oznámení při každém spuštění aplikace.
6. Stiskněte klávesu **F5** a spusťte aplikaci. Zobrazí se automaticky otevíraný dialog, který obsahuje registrační klíč.
   
     ![][19]

Vaše aplikace je teď připravena přijímat oznámení informačního nápisu.

## <a name="send-notifications"></a>Odeslat oznámení
Příjem oznámení ve vaší aplikaci můžete rychle otestovat zasláním oznámení z webu [Azure Portal](https://portal.azure.com/) prostřednictvím tlačítka **Poslat na zkoušku** v centru oznámení, jak vidíte na následující obrazovce.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Nabízená oznámení se většinou posílají ve službě back-end, jako je služba Mobile Services, nebo v technologii ASP.NET pomocí kompatibilní knihovny. Můžete také použít rozhraní API REST k přímému zasílání oznámení, pokud pro vaše prostředí back-end není dostupná žádná knihovna. 

V tomto kurzu nebudeme dělat nic složitého a jednoduše předvedeme testování vaší klientské aplikace pomocí odesílání oznámení v sadě SDK .NET pro centra oznámení v konzolové aplikaci, místo back-end služby. Jako další krok pro odesílání oznámení z backendu ASP.NET doporučujeme absolvovat kurz [Použití Notification Hubs k odeslání nabízených oznámení uživatelům]. Následující přístupy lze však použít pro zasílání oznámení:

* **Rozhraní REST**: oznámení můžete podporovat na jakékoli backend platformě pomocí [rozhraní REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).
* **Microsoft Azure oznámení centra .NET SDK**: Ve správci balíčků Nuget pro Visual Studio spusťte položku [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).
* **Node.js**: [Jak používat Notification Hubs z Node.js](notification-hubs-nodejs-push-notification-tutorial.md).
* **Azure Mobile Apps**: Příklad zasílání oznámení ze služby Azure Mobile Apps integrované se službou Notification Hubs najdete v tématu [Přidání nabízených oznámení do mobilních aplikací](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).
* **Java / PHP**: Příklad odesílání oznámení pomocí rozhraní API REST najdete v části „Použití centra oznámení z Java/PHP“ ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

## <a name="optional-send-notifications-from-a-console-app"></a>(Volitelné) Odesílání oznámení z konzoly aplikace
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
   
       Make sure to replace the "hub name" placeholder with the name of the notification hub that as it appears in the Azure Portal. Also, replace the connection string placeholder with the **DefaultFullSharedAccessSignature** connection string that you obtained from the **Access Policies** page of your Notification Hub in the section called "Configure your notification hub."
   
   > [!NOTE]
   > Ujistěte se, že používáte připojovací řetězec s **Úplným** přístupem, nikoli s přístupem **Naslouchat**. Řetězec s přístupem k naslouchání neposkytuje oprávnění k zasílání oznámení.
   > 
   > 
6. Přidejte následující řádky do metody **Hlavní**:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Klikněte pravým tlačítkem na projekt konzolové aplikace ve Visual Studiu a kliknutím na tlačítko **Nastavit jako spouštěný projekt** nastavte projekt jako spouštěný. Pak stiskněte klávesu **F5** a spusťte aplikaci.
   
     ![][14]
   
    Na všech registrovaných zařízeních se zobrazí informační zprávy. Kliknutí nebo klepnutí na informační nápis načte aplikaci.

Všechny podporované datové části v tématech [katalog informační zprávy], [katalog dlaždic] a [přehled odznaků] naleznete na webu MSDN.

## <a name="next-steps"></a>Další kroky
V tomto jednoduchém příkladu jste zaslali oznámení vysílání pro všechna vaše zařízení systému Windows pomocí portálu nebo aplikace konzoly. Jako další krok doporučujeme tutoriál [Použití Notification Hubs k odeslání nabízených oznámení uživatelům]. Zobrazí se postup odesílání oznámení z ASP.NET back-end pomocí značek pro cílové konkrétní uživatele.

Pokud chcete segmentovat uživatele podle zájmových skupin, přečtěte si kurz [Používání centra oznámení k odesílání novinek]. 

Další obecné informace o službě Notification Hubs najdete v tématu [Průvodce centry oznámení](notification-hubs-push-notification-overview.md).

<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->

[Použití Notification Hubs k odeslání nabízených oznámení uživatelům]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Používání centra oznámení k odesílání novinek]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md

[katalog informační zprávy]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[katalog dlaždic]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[přehled odznaků]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx

