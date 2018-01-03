---
title: "Lokalizované nejnovější novinky kurzu centra oznámení"
description: "Zjistěte, jak používat Azure Notification Hubs k odesílání oznámení o lokalizované aktuálních zprávách."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: c454f5a3-a06b-45ac-91c7-f91210889b25
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 8f205188bd68e53b187b71981ed36dcf9129ec62
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="use-notification-hubs-to-send-localized-breaking-news"></a>Použití centra oznámení k odesílání novinek lokalizované
> [!div class="op_single_selector"]
> * [Windows Store jazyka C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a>Přehled
Toto téma ukazuje, jak používat **šablony** funkce Azure Notification Hubs k vysílání oznámení o aktuálních zprávách, lokalizované podle jazyka a zařízení. V tomto kurzu začnete s aplikace Windows Store vytvořená v [použití centra oznámení k odesílání novinek]. Po dokončení, že bude možné registrovat kategorií, které vás zajímají, zadat jazyk, ve které chcete dostávat oznámení a přijímat pouze nabízená oznámení pro vybrané kategorie v daném jazyce.

Existují tento scénář se skládá ze dvou částí:

* aplikace pro Windows Store, umožňuje klientským zařízení můžete určit jazyk a k odběru kategorií různých nejnovější zprávy;
* back-end vysílá oznámení, pomocí **značka** a **šablony** feautres Azure Notification Hubs.

## <a name="prerequisites"></a>Požadavky
Musí jste již dokončili [použití centra oznámení k odesílání novinek] kurz a mít kód k dispozici, protože v tomto kurzu staví přímo na tento kód.

Také musíte Visual Studio 2012 nebo novějším.

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

Potom jsme zajistí, že zařízení zaregistrovat pomocí šablony, která odkazuje na správný vlastnost. Například aplikace pro Windows Store, který chce dostávat jednoduché informační zpráva bude registrace pro následující šablony s všechny odpovídající značky:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Šablony jsou velmi výkonné funkce, můžete další informace o možnostech v našem [šablony](notification-hubs-templates-cross-platform-push-messages.md) článku. 

## <a name="the-app-user-interface"></a>Uživatelské rozhraní aplikace
Nyní jsme upraví novinkách aplikaci, kterou jste vytvořili v tématu [použití centra oznámení k odesílání novinek] odeslat lokalizované novinky pomocí šablon.

V aplikaci Windows Store:

Změna vaší MainPage.xaml zahrnout combobox národního prostředí:

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

## <a name="building-the-windows-store-client-app"></a>Vytváření klientské aplikace Windows Store
1. Ve třídě oznámení přidat parametr národního prostředí pro vaše *StoreCategoriesAndSubscribe* a *SubscribeToCateories* metody.
   
        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }
   
        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            if (categories == null)
            {
                categories = RetrieveCategories();
            }
   
            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }
   
    Všimněte si, že se namísto volání *RegisterNativeAsync* metoda říkáme *RegisterTemplateAsync*: jsme registraci konkrétní oznámení formát, ve kterém šablony závisí na národním prostředí. Poskytujeme také název šablony ("localizedWNSTemplateExample"), protože jsme chtít zaregistrovat více než jednu šablonu (například jeden pro informační zprávy) a jeden pro dlaždice a musíme název je, aby bylo možné aktualizovat nebo odstranit.
   
    Všimněte si, že pokud se zařízení zaregistruje několik šablon se stejnou značkou, příchozí zprávy cílení, značka bude mít za následek více oznámení doručit do zařízení (jeden pro každé šablony). Toto chování je užitečné, když stejné logické zpráva má za následek více visual oznámení, například zobrazující oznámení a oznámení v aplikaci pro Windows Store.
2. Přidejte následující metodu pro načtení uložené národního prostředí:
   
        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }
3. V MainPage.xaml.cs, aktualizovat vaše tlačítko načítání aktuální hodnota pole se seznamem národního prostředí a zajištění k volání do třídy oznámení, klikněte na obslužnou rutinu, jak je znázorněno:
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;
   
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");
   
            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);
   
            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
4. Nakonec v souboru App.xaml.cs, nezapomeňte aktualizovat vaše `InitNotificationsAsync` metodu za účelem načtení národní prostředí a jeho použití při přihlášení k odběru:
   
        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

## <a name="send-localized-notifications-from-your-back-end"></a>Odesílání lokalizované oznámení z back-end
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[použití centra oznámení k odesílání novinek]: /notification-hubs/notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
