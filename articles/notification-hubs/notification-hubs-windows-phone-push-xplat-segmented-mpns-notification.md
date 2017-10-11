---
title: "Použití služby Notification Hubs k odesílání novinek (Windows Phone)"
description: "Pomocí Azure Notification Hubs můžete použít značky v registrace k odesílání novinek do aplikace pro Windows Phone."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: 42726bf5-cc82-438d-9eaa-238da3322d80
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 3a6a69bf555c7267d3fbeb03ff6c03054991960f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a>Používání centra oznámení k odesílání novinek
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Přehled
Toto téma ukazuje, jak používat Azure Notification Hubs k vysílání oznámení o aktuálních zprávách do aplikace pro Windows Phone 8.0/8.1 Silverlight. Pokud cílíte na Windows Store nebo Windows Phone 8.1 aplikace, naleznete na [univerzální pro Windows](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) verze. Po dokončení bude moci zaregistrovat pro nejnovější novinky kategorií, které vás zajímají a přijímat pouze nabízená oznámení pro tyto kategorie. Tento scénář je běžný vzor velký počet aplikací, kde mají oznámení k odeslání do skupiny uživatelů, které jste předtím nebyl deklarovaný zájem o jejich, např. čtečku RSS, aplikace pro Hudba ventilátory, atd.

Všesměrového vysílání scénáře jsou povolené zahrnutím jeden nebo více *značky* při vytváření registrace v centru oznámení. Pokud oznámení se odesílají do značku, všechna zařízení, která byla zaregistrovaná pro značku obdrží oznámení. Protože značky jsou jednoduše řetězce, nemají být předem zřízená. Další informace o značkách najdete v části [směrování centra oznámení a značky výrazy](notification-hubs-tags-segment-push-message.md).

## <a name="prerequisites"></a>Požadavky
Toto téma je založený na aplikaci, kterou jste vytvořili v [Začínáme s Notification Hubs]. Před zahájením tohoto kurzu, musí jste již dokončili [Začínáme s Notification Hubs].

## <a name="add-category-selection-to-the-app"></a>Přidat výběru kategorie do aplikace
Prvním krokem je přidání prvky uživatelského rozhraní na existující stránku pro hlavní, který uživateli umožňuje výběr kategorií k registraci. Kategorie, které uživatel jsou uloženy v zařízení. Při spuštění aplikace registrace zařízení se vytvoří v centru oznámení s vybrané kategorie jako značky.

1. Otevřete soubor projektu MainPage.xaml a nahraďte **mřížky** elementů s názvem `TitlePanel` a `ContentPanel` následujícím kódem:
   
        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>
   
        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>
2. V projektu, vytvořte novou třídu s názvem **oznámení**, přidejte **veřejné** modifikátor v definici třídy, přidejte následující **pomocí** příkazů do nového souboru kódu :
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;
3. Zkopírujte následující kód do nové **oznámení** třídy:
   
        private NotificationHub hub;
   
        // Registration task to complete registration in the ChannelUriUpdated event handler
        private TaskCompletionSource<Registration> registrationTask;
   
        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }
   
        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }
   
        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();
   
            return await SubscribeToCategories();
        }
   
        public async Task<Registration> SubscribeToCategories()
        {
            registrationTask = new TaskCompletionSource<Registration>();
   
            var channel = HttpNotificationChannel.Find("MyPushChannel");
   
            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
                channel.ChannelUriUpdated += channel_ChannelUriUpdated;
   
                // This is optional, used to receive notifications while the app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }
   
            // If channel.ChannelUri is not null, we will complete the registrationTask here.  
            // If it is null, the registrationTask will be completed in the ChannelUriUpdated event handler.
            if (channel.ChannelUri != null)
            {
                await RegisterTemplate(channel.ChannelUri);
            }
   
            return await registrationTask.Task;
        }
   
        async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
        {
            await RegisterTemplate(e.ChannelUri);
        }
   
        async Task<Registration> RegisterTemplate(Uri channelUri)
        {
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.
   
            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";
   
            // The stored categories tags are passed with the template registration.
   
            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));
   
            return await registrationTask.Task;
        }
   
        // This is optional. It is used to receive notifications while the app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;
   
            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());
   
            // Parse out the information that was part of the message.
            foreach (string key in e.Collection.Keys)
            {
                message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);
   
                if (string.Compare(
                    key,
                    "wp:Param",
                    System.Globalization.CultureInfo.InvariantCulture,
                    System.Globalization.CompareOptions.IgnoreCase) == 0)
                {
                    relativeUri = e.Collection[key];
                }
            }
   
            // Display a dialog of all the fields in the toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }

    Tato třída používá izolované úložiště k ukládání kategorie zprávy, které toto zařízení se zobrazí. Také obsahuje metody pro registraci pro tyto kategorie pomocí [šablony](notification-hubs-templates-cross-platform-push-messages.md) registrace oznámení.


1. V souboru projektu App.xaml.cs přidejte následující vlastnosti, která má **aplikace** třídy. Nahraďte `<hub name>` a `<connection string with listen access>` zástupné symboly pomocí názvu centra oznámení a připojovacího řetězce pro *DefaultListenSharedAccessSignature* kterou jste získali dříve.
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
   > [!NOTE]
   > Protože přihlašovací údaje, které jsou distribuované s klientskou aplikaci není obvykle zabezpečení, by měl distribuovat klíč pro naslouchání přístup pouze s vaší klientské aplikace. Poslechněte umožní přístup k aplikaci zaregistrovat pro oznámení, ale existující registrace nemůže být upravena a nelze odeslat oznámení. Úplný přístup klíč se používá ve službě Zabezpečené back-end pro zasílání oznámení a změna existující registrace.
   > 
   > 
2. V MainPage.xaml.cs přidejte následující řádek:
   
        using Windows.UI.Popups;
3. V souboru projektu MainPage.xaml.cs přidejte následující metodu:
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
          var categories = new HashSet<string>();
          if (WorldCheckBox.IsChecked == true) categories.Add("World");
          if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
          if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
          if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
          if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
          if (SportsCheckBox.IsChecked == true) categories.Add("Sports");
   
          var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
   
          MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
             result.RegistrationId);
        }
   
    Tato metoda vytvoří seznam kategorií a používá **oznámení** značky třída pro uložení seznamu v místním úložišti a zaregistrujte odpovídající pomocí centra oznámení. Při změně kategorií, registrace se znovu vytvoří se nové kategorie.

Aplikace je teď možné uložit sadu kategorií místní úložiště v zařízení a zaregistrovat do centra oznámení pokaždé, když uživatel změní výběr kategorie.

## <a name="register-for-notifications"></a>Registrace pro oznámení
Tyto kroky zaregistrovat do centra oznámení na spouštění pomocí kategorií, které byly uloženy v místním úložišti.

> [!NOTE]
> Vzhledem k tomu, že kanál URI přiřazené pomocí Microsoft nabízené služby oznámení (MPNS) můžete změnit kdykoli, byste měli zaregistrovat pro oznámení často, aby se zabránilo selhání oznámení. Tento příklad zaregistruje pro oznámení při každém spuštění aplikace. Pro aplikace, které jsou často spouštíte více než jednou denně, můžete pravděpodobně přeskočit registraci byla zachována šířka pásma, pokud od předchozí registrace uplynul méně než jeden den.
> 
> 

1. Otevřete soubor App.xaml.cs a přidejte **asynchronní** modifikátor k **Application_Launching** metodu a nahraďte kód registrace centra oznámení, které jste přidali v [Začínáme s Notification Hubs] následujícím kódem:
   
        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();
   
            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }
   
    Tím je zajištěno, že při každém spuštění aplikace načte kategorie z místního úložiště a požadavky registraci pro tyto kategorie.
2. V souboru projektu MainPage.xaml.cs, přidejte následující kód, který implementuje **OnNavigatedTo** metoda:
   
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();
   
            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
        }
   
    Tím se aktualizuje na základě stavu dříve uloženou kategorií hlavní stránce.

Aplikace je nyní dokončen a sadu kategorií můžete uložit do místního úložiště zařízení používá k registraci do centra oznámení pokaždé, když uživatel změní výběr kategorie. V dalším kroku bude definujeme back-end, který kategorie oznámení můžete odesílat do této aplikace.

## <a name="sending-tagged-notifications"></a>Odesílání oznámení s příznakem
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a>Spusťte aplikaci a generovat upozornění
1. Ve Visual Studiu stisknutím klávesy F5 zkompilování a spuštění aplikace.
   
    ![][1]
   
    Všimněte si, že aplikace uživatelského rozhraní, poskytuje sadu přepínačů, která vám umožní vybrat kategorie pro přihlášení k odběru.
2. Povolit jednu nebo více kategorií přepínačů a potom klikněte na **přihlásit k odběru**.
   
    Aplikace převede vybraných kategorií značky a požaduje novou registraci zařízení pro vybranou značky z centra oznámení. Registrovaný kategorie se vrátí a zobrazí v dialogu.
   
    ![][2]
3. Po přijetí potvrzení, že byly vaše kategorie odběru byla dokončena, spusťte konzolovou aplikaci odesílat oznámení pro každé kategorie. Ověřte, že dostanete oznámení pro přihlášení k odběru kategorií.
   
    ![][3]

Dokončili jste v tomto tématu.

<!--##Next steps

In this tutorial we learned how to broadcast breaking news by category. Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs to broadcast localized breaking news]

    Learn how to expand the breaking news app to enable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how to push notifications to specific authenticated users. This is a good solution for sending notifications only to specific users.
-->

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[Začínáme s Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs to broadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Phone]: ??

