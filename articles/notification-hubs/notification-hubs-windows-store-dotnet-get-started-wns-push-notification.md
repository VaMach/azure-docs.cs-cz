---
title: "Začínáme se službou Azure Notification Hubs pro aplikace Univerzální platformy Windows | Dokumentace Microsoftu"
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
ms.translationtype: HT
ms.sourcegitcommit: 9b7316a5bffbd689bdb26e9524129ceed06606d5
ms.openlocfilehash: e18a810bcdbd97c79418f53c647df8723ecb6076
ms.contentlocale: cs-cz
ms.lasthandoff: 09/08/2017

---
# <a name="get-started-with-notification-hubs-for-universal-windows-platform-apps"></a>Začínáme se službou Notification Hubs pro aplikace Univerzální platformy Windows

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Přehled
V tomto článku zjistíte, jak pomocí služby Azure Notification Hubs odesílat nabízená oznámení do aplikace Univerzální platformy Windows (UPW).

V tomto článku vytvoříte prázdnou aplikaci pro Windows Store, která přijímá nabízená oznámení pomocí služby nabízených oznámení Windows (WNS). Jakmile budete hotovi, budete moci používat vaše centrum oznámení k vysílání nabízených oznámení do všech zařízení používajících vaši aplikaci.

## <a name="before-you-begin"></a>Než začnete
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Dokončený kód pro tento kurz najdete na [GitHubu](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).

## <a name="prerequisites"></a>Požadavky
V tomto kurzu budete potřebovat následující:

* [Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) nebo novější
* [Nainstalované nástroje pro vývoj aplikací pro UPW](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
* Aktivní účet Azure  
    Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Další informace najdete na stránce [bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).
* Aktivní účet Windows Store

Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy služby Notification Hubs pro aplikace UPW.

## <a name="register-your-app-for-the-windows-store"></a>Registrace aplikace pro Windows Store
Pokud chcete odesílat nabízená oznámení do aplikací UPW, přidružte svou aplikaci k Windows Store. Pak nakonfigurujte centrum oznámení pro integraci se službou WNS.

1. Pokud jste ještě aplikaci nezaregistrovali, přejděte na web [Windows Dev Center](https://dev.windows.com/overview), přihlaste se pomocí účtu Microsoft a pak vyberte **Vytvořit novou aplikaci**.

2. Zadejte název aplikace a pak vyberte **Rezervovat název aplikace**. Tím se vytvoří nová registrace Windows Store pro vaši aplikaci.

3. V sadě Visual Studio vytvořte nový projekt aplikace Visual C# pro Store pomocí šablony **Prázdná aplikace** UPW a pak vyberte **OK**.

4. Přijměte výchozí hodnoty cíle a minimální verze platformy.

5. V Průzkumníku řešení klikněte pravým tlačítkem na projekt aplikace pro Windows Store, vyberte **Store** a pak vyberte **Přidružit aplikaci ve Store**.  
    Zobrazí se průvodce **Přidružením aplikace k Windows Store**.

6. V průvodci se přihlaste pomocí svého účtu Microsoft.

7. Vyberte aplikaci zaregistrovanou v kroku 2, vyberte **Další** a pak vyberte **Přidružit**. Tím se přidají požadované informace o registraci Windows Store do manifestu aplikace.

8. Zpátky na stránce [Windows Dev Center](http://dev.windows.com/overview) pro novou aplikaci vyberte postupně **Služby**, **Nabízená oznámení** a pak **WNS/MPNS**.

9. Vyberte **Nové oznámení**.

10. Vyberte šablonu **Prázdná (informační zpráva)** a pak vyberte **OK**.

11. Zadejte **Název** oznámení a vizuální **kontextovou** zprávu a pak vyberte **Uložit jako koncept**.

12. Přejděte na [Portál pro registraci aplikací](http://apps.dev.microsoft.com) a přihlaste se.

13. Vyberte název vaší aplikace. V nastavení platformy **Windows Store** si poznamenejte heslo **Tajný klíč aplikace** a **Identifikátor zabezpečení (SID) balíčku**.

    >[!WARNING]
    >Tajný klíč aplikace a SID balíčku jsou důležité přihlašovací údaje zabezpečení. Tyto hodnoty s nikým nesdílejte ani je nedistribuujte s vaší aplikací.

## <a name="configure-your-notification-hub"></a>Konfigurace centra oznámení
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="5">
<li><p>Vyberte <b>Notification Services</b> > <b>Windows(WNS)</b> a pak zadejte tajný klíč aplikace do pole <b>Klíč zabezpečení</b>. Do pole <b>SID balíčku</b> zadejte hodnotu, kterou jste ve službě WNS získali v předchozí části, a pak vyberte <b>Uložit</b>.</p>
</li>
</ol>

![Pole SID balíčku a Klíč zabezpečení](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Vaše centrum oznámení je teď nakonfigurované pro práci se službou WNS. Máte připojovací řetězec, pomocí kterého můžete zaregistrovat aplikaci a odesílat oznámení.

## <a name="connect-your-app-to-the-notification-hub"></a>Připojte aplikaci k centru oznámení
1. V sadě Visual Studio klikněte pravým tlačítkem na řešení a pak vyberte **Spravovat balíčky NuGet**.  
    Otevře se okno **Spravovat balíčky NuGet**.

2. Do vyhledávacího pole zadejte **WindowsAzure.Messaging.Managed**, vyberte **Nainstalovat** a přijměte podmínky použití.
   
    ![Okno Spravovat balíčky NuGet][20]
   
    Tato akce stáhne, nainstaluje a přidá odkaz na knihovnu zasílání zpráv Azure pro Windows pomocí [balíčku NuGet WindowsAzure.Messaging.Managed](http://nuget.org/packages/WindowsAzure.Messaging).

3. Otevřete soubor projektu App.xaml.cs a přidejte následující příkazy `using`: 
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;

4. V souboru App.xaml.cs také přidejte do třídy **App** následující definici metody **InitNotificationsAsync**:
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("<your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
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
   
    >[!NOTE]
    >* Zástupný text **hub name** nahraďte názvem centra oznámení, který se zobrazí na webu Azure Portal. 
    >* Nahraďte také zástupný připojovací řetězec připojovacím řetězcem **DefaultListenSharedAccessSignature**, který jste získali v předchozí části na stránce **Zásady přístupu** vašeho centra oznámení.
   > 
   > 
5. V horní části **OnLaunched** obslužné rutiny událostí v souboru App.xaml.cs přidejte následující volání do nové metody **InitNotificationsAsync**:
   
        InitNotificationsAsync();
   
    Tato akce zaručuje, že se identifikátor URI kanálu zaregistruje v centru oznámení při každém spuštění aplikace.

6. Pokud chcete aplikaci spustit, stiskněte klávesu **F5**. Zobrazí se dialogové okno, které obsahuje registrační klíč.

Vaše aplikace je teď připravena přijímat oznámení informačního nápisu.

## <a name="send-notifications"></a>Odeslat oznámení
Příjem oznámení ve vaší aplikaci můžete rychle otestovat odesláním oznámení na webu [Azure Portal](https://portal.azure.com/). Použijte tlačítko **Poslat na zkoušku** v centru oznámení, jak je znázorněno na následujícím obrázku:

![Podokno Testovací odeslání](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Nabízená oznámení se obvykle odesílají ve službě back-end, jako je služba Mobile Services, nebo v technologii ASP.NET pomocí kompatibilní knihovny. Pokud pro váš back-end není dostupná žádná knihovna, můžete oznámení také odeslat přímo pomocí rozhraní REST API. 

Tento kurz předvádí, jak otestovat klientskou aplikaci prostým odesláním oznámení, která používají sadu .NET SDK pro centra oznámení v konzolové aplikaci, místo služby back-end. Jako další krok pro odesílání oznámení z back-endu ASP.NET doporučujeme absolvovat kurz [Použití Notification Hubs k odeslání nabízených oznámení uživatelům]. Oznámení však můžete odesílat pomocí následujících postupů:

* **Rozhraní REST:** Oznámení můžete podporovat na jakékoli platformě back-end pomocí [rozhraní REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).

* **Sada .NET SDK služby Microsoft Azure Notification Hubs:** Ve správci balíčků NuGet pro sadu Visual Studio spusťte příkaz [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js:** Viz [Jak používat Notification Hubs z Node.js](notification-hubs-nodejs-push-notification-tutorial.md).
* **Azure Mobile Apps:** Příklad odesílání oznámení z mobilní aplikace Azure integrované se službou Notification Hubs najdete v tématu [Přidání nabízených oznámení pro funkci Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).

* **Java nebo PHP:** Příklady odesílání oznámení pomocí rozhraní REST API:
    * [Java](notification-hubs-java-push-notification-tutorial.md)
    * [PHP](notification-hubs-php-push-notification-tutorial.md)

## <a name="optional-send-notifications-from-a-console-app"></a>(Volitelné) Odesílání oznámení z konzoly aplikace
Pokud chcete odesílat oznámení pomocí konzolové aplikace .NET, postupujte následovně: 

1. Klikněte pravým tlačítkem na řešení, vyberte **Přidat** > **Nový projekt**, v části **Visual C#** vyberte **Windows** a **Konzolová aplikace** a pak vyberte **OK**.
   
    Do řešení se přidá nová konzolová aplikace Visual C#. Projekt můžete také přidat do samostatného řešení.

2. V sadě Visual Studio vyberte **Nástroje**, vyberte **Správce balíčků NuGet** a pak vyberte **Konzola Správce balíčků**.
   
    V sadě Visual Studio se otevře Konzola Správce balíčků.

3. V okně Konzola Správce balíčků nastavte **Výchozí projekt** na nový projekt konzolové aplikace a pak v okně konzoly spusťte následující příkaz:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Tato akce přidá referenci na sadu SDK služby Azure Notification Hubs pomocí [balíčku NuGet Microsoft.Azure.Notification Hubs](http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).
   
    ![Název výchozího projektu](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

4. Otevřete soubor Program.cs a pak přidejte následující příkaz `using`:
   
        using Microsoft.Azure.NotificationHubs;

5. Přidejte následující metodu do třídy **Program**:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">Hello from a .NET App!</text></binding></visual></toast>";
            await hub.SendWindowsNativeNotificationAsync(toast);
        }
   
    >[!NOTE]
    >* Zástupný text **hub name** nahraďte názvem centra oznámení, který se zobrazí na webu Azure Portal. 
    >* Nahraďte zástupný připojovací řetězec připojovacím řetězcem **DefaultFullSharedAccessSignature**, který jste získali v části Konfigurace centra oznámení na stránce **Zásady přístupu** vašeho centra oznámení.
    >* Použijte připojovací řetězec s *úplným* přístupem, nikoli s přístupem *naslouchat*. Řetězec s přístupem k naslouchání neposkytuje oprávnění k zasílání oznámení.
   > 
   > 
6. Do metody **Main** přidejte následující řádky:
   
         SendNotificationAsync();
         Console.ReadLine();

7. Klikněte pravým tlačítkem na projekt konzolové aplikace v sadě Visual Studio a výběrem **Nastavit jako spouštěný projekt** nastavte projekt jako spouštěný. Pak stiskněte klávesu **F5** a spusťte aplikaci.
   
    Na všech registrovaných zařízeních se zobrazí informační zprávy. Výběr nebo klepnutí na informační nápis načte aplikaci.

Všechny podporované datové části v tématech [katalog informační zprávy], [katalog dlaždic] a [přehled odznaků] naleznete na webu MSDN.

## <a name="next-steps"></a>Další kroky
V tomto jednoduchém příkladu jste pomocí portálu nebo konzolové aplikace odeslali oznámení vysílání do všech vašich zařízení s Windows. Jako další krok doporučujeme kurz [Použití Notification Hubs k odeslání nabízených oznámení uživatelům]. Předvádí postup odesílání oznámení z back-endu ASP.NET s využitím značek pro cílení na konkrétní uživatele.

Pokud chcete segmentovat uživatele podle zájmových skupin, přečtěte si kurz [Používání centra oznámení k odesílání novinek]. 

Další obecné informace o službě Notification Hubs najdete v tématu [Průvodce službou Notification Hubs](notification-hubs-push-notification-overview.md).

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
 

