---
title: "Používat Azure Notification Hubs k odesílání novinek (Universal Windows Platform)"
description: "Pomocí značky v registraci použijte Azure Notification Hubs k odesílání novinek do aplikace pro univerzální platformu Windows."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: 994d2eed-f62e-433c-bf65-4afebf1c0561
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: d510e7e665adec9607aeee80802c466b363d5d5b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a>Používání centra oznámení k odesílání novinek
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Přehled
Toto téma ukazuje, jak používat Azure Notification Hubs k vysílání oznámení o aktuálních zprávách do Windows Store nebo Windows Phone 8.1 (bez Silverlight) aplikace. Pokud cílíte na Windows Phone 8.1 Silverlight, podívejte se na [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) verze. 

Po dokončení tohoto procesu, můžete zaregistrovat pro nové kategorie ukončování řádků, které vás zajímají, a obdržíte nabízená oznámení pouze v těchto kategoriích. Tento scénář je běžný pro velký počet aplikací (například RSS čtečky nebo aplikace pro Hudba ventilátory), kde musí se poslat oznámení na skupiny uživatelů, kteří mají zájem o je deklarován. 

Můžete povolit scénáře všesměrového vysílání včetně jeden nebo více *značky* při vytváření registrace v centru oznámení. Když oznámení se odesílají do značku, zobrazí se všechna zařízení, která byla zaregistrovaná pro značku oznámení. Protože značky jsou jednoduše řetězce, nemají být nastavit předem. Další informace o značkách najdete v tématu [výrazy směrování a značky Notification Hubs](notification-hubs-tags-segment-push-message.md).

> [!NOTE]
> Windows Store a Windows Phone projektu verze 8.1 a starší nejsou podporovány v aplikaci Visual Studio 2017. Další informace najdete v tématu [Cílení na platformy a kompatibilita v sadě Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs). 

## <a name="prerequisites"></a>Požadavky
Toto téma je založený na aplikaci, kterou jste vytvořili v [Začínáme s Notification Hubs][get-started]. Než začnete tento kurz, musíte dokončit [Začínáme s Notification Hubs][get-started].

## <a name="add-category-selection-to-the-app"></a>Přidat výběru kategorie do aplikace
Prvním krokem je přidání prvky uživatelského rozhraní do existující hlavní stránky tak, aby uživatelé mohou vybrat kategorie k registraci. Vybrané kategorie se ukládají na zařízení. Při spuštění aplikace registrace zařízení se vytvoří v centru oznámení, s vybrané kategorie jako značky.

1. Otevřete soubor projektu MainPage.xaml a poté zkopírujte následující kód **mřížky** element:
   
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>

2. Klikněte pravým tlačítkem myši **sdílené** projekt, přidejte novou třídu s názvem **oznámení**, přidat **veřejné** modifikátor v definici třídy a pak přidejte následující **pomocí** příkazy na nový soubor kódu:
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;

3. Zkopírujte následující kód do nového **oznámení** třídy:
   
        private NotificationHub hub;
   
        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }
   
        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }
   
        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }
   
        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            if (categories == null)
            {
                categories = RetrieveCategories();
            }
   
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.
   
            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }
   
    Tato třída používá místní úložiště k ukládání kategorie zprávy, které tato zařízení musí získat. Místo volání *RegisterNativeAsync* metoda, volání *RegisterTemplateAsync* registrace pro kategorie pomocí šablony registrace. 
   
    Protože můžete chtít zaregistrovat více než jednu šablonu (například jeden pro informační zprávy) a jeden pro dlaždice, zadejte také název šablony (například "simpleWNSTemplateExample"). Zadáte název šablony, abyste mohli aktualizovat nebo smazat.
   
    >[!NOTE]
    >Pokud se zařízení zaregistruje několik šablon se stejnou značkou, příchozí zprávy, která je cílena značky způsobí, že několik oznámení, který bude doručen do zařízení (jeden pro každé šablony). Toto chování je užitečné, když ke stejné logické zprávě musí mít za následek více visual oznámení (například zobrazující oznámení a oznámení v aplikaci pro Windows Store).
   
    Další informace najdete v tématu [šablony](notification-hubs-templates-cross-platform-push-messages.md).

4. V souboru projektu App.xaml.cs přidejte následující vlastnosti, která má **aplikace** třídy:
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
    Pomocí této vlastnosti můžete vytvořit a získat přístup **oznámení** instance.
   
    V kódu, nahraďte `<hub name>` a `<connection string with listen access>` zástupné symboly pomocí názvu centra oznámení a připojovacího řetězce pro *DefaultListenSharedAccessSignature*, který jste dříve získali.
   
   > [!NOTE]
   > Protože přihlašovací údaje, které jsou distribuované s klientskou aplikaci není obvykle zabezpečené, distribuovat pouze klíč pro *naslouchání* přístup s vaší klientské aplikace. S přístupem k naslouchání můžete aplikaci zaregistrovat pro oznámení, ale existující registrace nemůže být upravena a nelze odeslat oznámení. Úplné přístupový klíč se používá v zabezpečené back-end službu pro odeslání oznámení a změnu existující registrace.
   > 
   > 
5. V souboru projektu MainPage.xaml.cs přidejte následující řádek:
   
        using Windows.UI.Popups;

6. V souboru projektu MainPage.xaml.cs přidejte následující metodu:
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");
   
            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
   
            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
   
    Tato metoda vytvoří seznam kategorií a používá **oznámení** třídu pro ukládání seznamu v místním úložišti. Zaregistruje také odpovídající značky se vaše Centrum oznámení. Při změně kategorií, nebude znovu vytvořena s novou kategorií registraci.

Aplikaci teď můžete uložit sadu kategorií v místní úložiště v zařízení. Aplikace se zaregistruje s centrem oznámení pokaždé, když uživatelé změnit výběr kategorie.

## <a name="register-for-notifications"></a>Registrace pro oznámení
V této části registraci do centra oznámení na spouštění pomocí kategorií, které jste měli uložené v místním úložišti.

> [!NOTE]
> Vzhledem k tomu, že kanál URI, která je přiřazena pomocí služby oznámení Windows (WNS) můžete změnit kdykoli, byste měli zaregistrovat pro oznámení často, aby se zabránilo selhání oznámení. V tomto příkladu se zaregistruje pro oznámení při každém spuštění aplikace. Pro aplikace, které spustíte často (více než jednou za den) můžete přeskočit pravděpodobně registrace byla zachována šířka pásma, pokud od předchozí registrace uplynul méně než jeden den.
> 
> 

1. Použít `notifications` k odběru na základě kategorií, otevřete soubor App.xaml.cs a pak aktualizujte **InitNotificationsAsync** metoda.
   
        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
   
        var result = await notifications.SubscribeToCategories();
   
    Tento proces zajišťuje, že při spuštění aplikace načte kategorie z místního úložiště a požadavky registrace z těchto kategorií. Jste vytvořili **InitNotificationsAsync** metoda jako součást [Začínáme s Notification Hubs] [ get-started] kurzu.

2. V souboru projektu MainPage.xaml.cs, přidejte následující kód, který *OnNavigatedTo* metoda:
   
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();
   
            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
        }
   
    Tento kód aktualizuje hlavní stránce, na základě stavu dříve uloženou kategorií.

Aplikace je nyní dokončen. Sada kategorií ho můžete uložit v úložišti místní zařízení, který se použije k registraci do centra oznámení, když uživatelé změní výběr kategorie. V další části definujete back-end, který kategorie oznámení můžete odesílat do této aplikace.

## <a name="send-tagged-notifications"></a>Odesílání oznámení s příznakem
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a>Spusťte aplikaci a generovat upozornění
1. V sadě Visual Studio, vyberte **F5** pro zkompilování a spuštění aplikace.  
    Aplikace uživatelského rozhraní obsahuje sadu přepínačů, která umožňuje vybrat kategorie pro přihlášení k odběru. 
   
    ![Nejnovější novinky aplikace][1]

2. Povolit jeden nebo více přepínačů kategorie a pak klikněte na **přihlásit k odběru**.
   
    Aplikace převede vybraných kategorií značky a požaduje novou registraci zařízení pro vybranou značky z centra oznámení. Registrovaný kategorie se vrátí a zobrazí v dialogovém okně.
   
    ![Kategorie přepínačů a tlačítko přihlásit k odběru][19]

3. Odeslání nové oznámení z back-end v jednom z následujících způsobů:

   * **Konzolové aplikace**: Spusťte konzolovou aplikaci.
   * **Java/PHP**: spuštění aplikace nebo skriptu.
     
     Oznámení pro vybrané kategorie se zobrazí jako informační zprávy.
     
     ![Informační zprávy][14]

## <a name="next-steps"></a>Další kroky
V tomto článku jste zjistili, jak k vysílání novinek podle kategorie. Vezměte v úvahu následující kurz, který označuje jiné pokročilé scénáře Notification Hubs dokončení:

* [Použití centra oznámení k vysílání lokalizované novinek] tento kurz popisuje, jak rozšířit aplikace nejnovější novinky povolíte odesílání lokalizované oznámení.

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /azure/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification
[Použití centra oznámení k vysílání lokalizované novinek]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
