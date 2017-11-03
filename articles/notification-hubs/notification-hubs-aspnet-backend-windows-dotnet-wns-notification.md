---
title: "Azure upozornění uživatelů centra oznámení s .NET back-end"
description: "Zjistěte, jak odeslat zabezpečené nabízená oznámení v Azure. Ukázky kódu jsou vytvořeny v C# s použitím .NET API."
documentationcenter: windows
author: ysxu
manager: erikre
services: notification-hubs
editor: 
ms.assetid: 012529f2-fdbc-43c4-8634-2698164b5880
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: c0b963ef661612b1a176dd8e5f01d56e61eb5acb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-notification-hubs-notify-users-with-net-backend"></a>Azure upozornění uživatelů centra oznámení s .NET back-end
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a>Přehled
Podpora nabízená oznámení v Azure umožňuje přístup snadné použití, multiplatform a nabízené škálovanou infrastrukturu, která výrazně zjednodušuje implementaci nabízená oznámení spotřebních a podnikových aplikací pro mobilní platformy. V tomto kurzu se dozvíte, jak se dají pomocí Azure Notification Hubs posílat nabízená oznámení specifickým uživatelům aplikace na specifickém zařízení. Backendu ASP.NET WebAPI slouží k ověřování klientů. Pomocí ověřený klient uživatelů a značky automaticky přidá back-end registrace oznámení. Tato značka se používá k odeslání back-end pro generování oznámení pro konkrétního uživatele. Další informace o registraci pro oznámení pomocí back-end aplikace naleznete v tématu pokyny [registrace z back-end aplikace](http://msdn.microsoft.com/library/dn743807.aspx). V tomto kurzu vychází Centrum oznámení a projekt, který jste vytvořili v [Začínáme s Notification Hubs] kurzu.

V tomto kurzu je také předpokladem pro [zabezpečení Push] kurzu. Po dokončení kroků v tomto kurzu můžete pokračovat [zabezpečení Push] kurz, který ukazuje, jak změnit kód v tomto kurzu k odesílání nabízených oznámení bezpečně.

## <a name="before-you-begin"></a>Než začnete
Záleží nám na vašem názoru. Pokud máte s tímto tématem nějaké potíže nebo byste nám chtěli doporučit vylepšení tohoto obsahu, napište svůj názor do spodní části stránky a my si ho rádi přečteme.

Dokončený kód v tomto kurzu lze najít v části Github [zde](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). 

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte jste již dokončili tato kurzech používání Mobile Services:

* [Začínáme s Notification Hubs]<br/>Vytvoření centra oznámení a rezervovat název aplikace a zaregistrovat pro příjem oznámení v tomto kurzu. Tento kurz předpokládá, že jste už dokončili kroky. Pokud ne, postupujte podle kroků v [Začínáme s Notification Hubs (pro Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md); konkrétně v částech [registrace aplikace pro Windows Store](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#register-your-app-for-the-windows-store) a [Konfigurace centra oznámení](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub). Konkrétně se ujistěte, zda jste zadali **SID balíčku** a **tajný klíč klienta** hodnoty na portálu ve **konfigurace** kartě pro vaše Centrum oznámení. Tento postup konfigurace je popsaný v části [Konfigurace centra oznámení](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub). Toto je důležitý krok: Pokud přihlašovací údaje na portál uvedenými pro název aplikace neshodují si zvolíte, nebude úspěšné nabízených oznámení.

> [!NOTE]
> Pokud používáte v Azure App Service Mobile Apps jako back-end službu, najdete v článku [Mobile Apps verze](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) tohoto kurzu.
> 
> 

&nbsp;

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="update-the-code-for-the-client-project"></a>Aktualizujte kód pro projektu klienta
V této části, aktualizujte kód v projektu dokončeno pro [Začínáme s Notification Hubs] kurzu. By už přidružené k úložišti a nakonfigurovaný pro vaše Centrum oznámení. V této části přidáte kód volání nový back-end WebAPI a používat ho pro registraci a odesílání oznámení.

1. V sadě Visual Studio, otevřete řešení, které jste vytvořili pro [Začínáme s Notification Hubs] kurzu.
2. V Průzkumníku řešení klikněte pravým tlačítkem myši **(Windows 8.1)** projektu a pak klikněte na **spravovat balíčky NuGet**.
3. Na levé straně klikněte na **Online**.
4. V **vyhledávání** zadejte **klienta Http**.
5. V seznamu výsledků klepněte na **knihovny klienta HTTP Microsoft**a potom klikněte na **nainstalovat**. Dokončete instalaci.
6. Zpět v NuGet **vyhledávání** zadejte **Json.net**. Nainstalujte **Json.NET** balíček a potom zavřete okno Správce balíčků NuGet.
7. Opakujte předchozí kroky pro **(Windows Phone 8.1)** projektu k instalaci **JSON.NET** balíček NuGet pro projekt Windows Phone.
8. V Průzkumníku řešení v **(Windows 8.1)** projektu, klikněte dvakrát na **MainPage.xaml** a otevře se v editoru Visual Studio.
9. V **MainPage.xaml** kód XML, nahraďte `<Grid>` oddíl následujícím kódem. Tento kód přidá textové pole uživatelské jméno a heslo, které se budou ověřovat uživatele. Také přidá textových polí pro oznámení a značce uživatelské jméno, které by měly dostávat oznámení:
   
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
   
            <TextBlock Grid.Row="0" Text="Notify Users" HorizontalAlignment="Center" FontSize="48"/>
   
            <StackPanel Grid.Row="1" VerticalAlignment="Center">
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                    </Grid.ColumnDefinitions>
                    <TextBlock Grid.Row="0" Grid.ColumnSpan="3" Text="Username" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="UsernameTextBox" Grid.Row="1" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
                    <TextBlock Grid.Row="2" Grid.ColumnSpan="3" Text="Password" FontSize="24" Margin="20,0,20,0" />
                    <PasswordBox Name="PasswordTextBox" Grid.Row="3" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
   
                    <Button Grid.Row="4" Grid.ColumnSpan="3" HorizontalAlignment="Center" VerticalAlignment="Center"
                                Content="1. Login and register" Click="LoginAndRegisterClick" Margin="0,0,0,20"/>
   
                    <ToggleButton Name="toggleWNS" Grid.Row="5" Grid.Column="0" HorizontalAlignment="Right" Content="WNS" IsChecked="True" />
                    <ToggleButton Name="toggleGCM" Grid.Row="5" Grid.Column="1" HorizontalAlignment="Center" Content="GCM" />
                    <ToggleButton Name="toggleAPNS" Grid.Row="5" Grid.Column="2" HorizontalAlignment="Left" Content="APNS" />
   
                    <TextBlock Grid.Row="6" Grid.ColumnSpan="3" Text="Username Tag To Send To" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="ToUserTagTextBox" Grid.Row="7" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <TextBlock Grid.Row="8" Grid.ColumnSpan="3" Text="Enter Notification Message" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="NotificationMessageTextBox" Grid.Row="9" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <Button Grid.Row="10" Grid.ColumnSpan="3" HorizontalAlignment="Center" Content="2. Send push" Click="PushClick" Name="SendPushButton" />
                </Grid>
            </StackPanel>
        </Grid>
10. V Průzkumníku řešení v **(Windows Phone 8.1)** projekt, otevřete **MainPage.xaml** a nahraďte sady Windows Phone 8.1 `<Grid>` oddíl s Tento stejný kód výše. Rozhraní by měla vypadat podobně jako co je uvedeno níže.
    
    ![][13]
11. V Průzkumníku řešení otevřete **MainPage.xaml.cs** souboru **(Windows 8.1)** a **(Windows Phone 8.1)** projekty. Přidejte následující `using` příkazy v horní části oba soubory:
    
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Windows.Networking.PushNotifications;
        using Windows.UI.Popups;
        using System.Threading.Tasks;
12. V **MainPage.xaml.cs** pro **(Windows 8.1)** a **(Windows Phone 8.1)** následující členy přidat projekty, `MainPage` třídy. Nezapomeňte nahradit `<Enter Your Backend Endpoint>` s váš back-end skutečný koncový bod získali dříve. Například, `http://mybackend.azurewebsites.net`.
    
        private static string BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";
13. Přidejte následující kód do třídy MainPage v **MainPage.xaml.cs** pro **(Windows 8.1)** a **(Windows Phone 8.1)** projekty.
    
    `PushClick` Metoda je obslužná rutina kliknutí na pro **odeslat nabízené** tlačítko. Zavolá back-end pro aktivaci oznámení pro všechna zařízení se značkou uživatelského jména, která odpovídá `to_tag` parametr. Jako obsah JSON v textu požadavku je odeslána zpráva oznámení.
    
    `LoginAndRegisterClick` Metoda je obslužná rutina kliknutí na pro **přihlášení a registrace** tlačítko. Ukládá základní ověřovací token v místním úložišti (Upozorňujeme, že to představuje všechny token používá vaše schéma ověřování), pak použije `RegisterClient` registrace pro oznámení pomocí back-end.

        private async void PushClick(object sender, RoutedEventArgs e)
        {
            if (toggleWNS.IsChecked.Value)
            {
                await sendPush("wns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleGCM.IsChecked.Value)
            {
                await sendPush("gcm", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleAPNS.IsChecked.Value)
            {
                await sendPush("apns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);

            }
        }

        private async Task sendPush(string pns, string userTag, string message)
        {
            var POST_URL = BACKEND_ENDPOINT + "/api/notifications?pns=" +
                pns + "&to_tag=" + userTag;

            using (var httpClient = new HttpClient())
            {
                var settings = ApplicationData.Current.LocalSettings.Values;
                httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                try
                {
                    await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                        System.Text.Encoding.UTF8, "application/json"));
                }
                catch (Exception ex)
                {
                    MessageDialog alert = new MessageDialog(ex.Message, "Failed to send " + pns + " message");
                    alert.ShowAsync();
                }
            }
        }

        private async void LoginAndRegisterClick(object sender, RoutedEventArgs e)
        {
            SetAuthenticationTokenInLocalStorage();

            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            // The "username:<user name>" tag gets automatically added by the message handler in the backend.
            // The tag passed here can be whatever other tags you may want to use.
            try
            {
                // The device handle used will be different depending on the device and PNS. 
                // Windows devices use the channel uri as the PNS handle.
                await new RegisterClient(BACKEND_ENDPOINT).RegisterAsync(channel.Uri, new string[] { "myTag" });

                var dialog = new MessageDialog("Registered as: " + UsernameTextBox.Text);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
                SendPushButton.IsEnabled = true;
            }
            catch (Exception ex)
            {
                MessageDialog alert = new MessageDialog(ex.Message, "Failed to register with RegisterClient");
                alert.ShowAsync();
            }
        }

        private void SetAuthenticationTokenInLocalStorage()
        {
            string username = UsernameTextBox.Text;
            string password = PasswordTextBox.Password;

            var token = Convert.ToBase64String(System.Text.Encoding.UTF8.GetBytes(username + ":" + password));
            ApplicationData.Current.LocalSettings.Values["AuthenticationToken"] = token;
        }



1. V Průzkumníku řešení klikněte v části **sdílené** projekt, otevřete **App.xaml.cs** souboru. Najít volání `InitNotificationsAsync()` v `OnLaunched()` obslužné rutiny události. Komentář nebo odstranit volání `InitNotificationsAsync()`. Obslužná rutina tlačítko přidané v předchozím kroku bude inicializovat registrace oznámení.

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
            //InitNotificationsAsync();


1. V Průzkumníku řešení klikněte pravým tlačítkem myši **sdílené** projektu a pak klikněte na **přidat**a potom klikněte na **třída**. Název třídy **RegisterClient.cs**, pak klikněte na tlačítko **OK** ke generování třídy.
   
   Tato třída budou zahrnovat volání REST nutné kontaktovat back-end aplikace, aby bylo možné zaregistrovat pro nabízená oznámení. Také místně ukládá *registrationIds* vytvořené Centrum oznámení podle popisu v [registrace z back-end aplikace](http://msdn.microsoft.com/library/dn743807.aspx). Všimněte si, že se používá autorizační token uložené v místní úložiště, když kliknete **přihlášení a registrace** tlačítko.
2. Přidejte následující `using` příkazy v horní části souboru RegisterClient.cs:
   
       using Windows.Storage;
       using System.Net;
       using System.Net.Http;
       using System.Net.Http.Headers;
       using Newtonsoft.Json;
       using System.Threading.Tasks;
       using System.Linq;
3. Do definice třídy `RegisterClient` přidejte následující kód.
   
       private string POST_URL;
   
       private class DeviceRegistration
       {
           public string Platform { get; set; }
           public string Handle { get; set; }
           public string[] Tags { get; set; }
       }
   
       public RegisterClient(string backendEndpoint)
       {
           POST_URL = backendEndpoint + "/api/register";
       }
   
       public async Task RegisterAsync(string handle, IEnumerable<string> tags)
       {
           var regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
   
           var deviceRegistration = new DeviceRegistration
           {
               Platform = "wns",
               Handle = handle,
               Tags = tags.ToArray<string>()
           };
   
           var statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
   
           if (statusCode == HttpStatusCode.Gone)
           {
               // regId is expired, deleting from local storage & recreating
               var settings = ApplicationData.Current.LocalSettings.Values;
               settings.Remove("__NHRegistrationId");
               regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
               statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
           }
   
           if (statusCode != HttpStatusCode.Accepted)
           {
               // log or throw
               throw new System.Net.WebException(statusCode.ToString());
           }
       }
   
       private async Task<HttpStatusCode> UpdateRegistrationAsync(string regId, DeviceRegistration deviceRegistration)
       {
           using (var httpClient = new HttpClient())
           {
               var settings = ApplicationData.Current.LocalSettings.Values;
               httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string) settings["AuthenticationToken"]);
   
               var putUri = POST_URL + "/" + regId;
   
               string json = JsonConvert.SerializeObject(deviceRegistration);
                               var response = await httpClient.PutAsync(putUri, new StringContent(json, Encoding.UTF8, "application/json"));
               return response.StatusCode;
           }
       }
   
       private async Task<string> RetrieveRegistrationIdOrRequestNewOneAsync()
       {
           var settings = ApplicationData.Current.LocalSettings.Values;
           if (!settings.ContainsKey("__NHRegistrationId"))
           {
               using (var httpClient = new HttpClient())
               {
                   httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);
   
                   var response = await httpClient.PostAsync(POST_URL, new StringContent(""));
                   if (response.IsSuccessStatusCode)
                   {
                       string regId = await response.Content.ReadAsStringAsync();
                       regId = regId.Substring(1, regId.Length - 2);
                       settings.Add("__NHRegistrationId", regId);
                   }
                   else
                   {
                       throw new System.Net.WebException(response.StatusCode.ToString());
                   }
               }
           }
           return (string)settings["__NHRegistrationId"];
   
       }
4. Uložte všechny provedené změny.

## <a name="testing-the-application"></a>Testování aplikace
1. Spuštění aplikace na Windows 8.1 a Windows Phone 8.1. Pro Windows Phone 8.1 můžete spustit instanci v emulátoru nebo skutečné zařízení.
2. V instanci aplikace Windows 8.1 zadejte **uživatelské jméno** a **heslo** jak je znázorněno na obrazovce níže. Ho by se měly lišit od uživatelské jméno a heslo, které zadáte na Windows Phone.
3. Klikněte na tlačítko **přihlášení a registrace** a ověřte, zobrazí se dialogové okno ukazuje, že jste byli přihlášeni. Tato akce povolí **odeslat nabízené** tlačítko.
   
    ![][14]
4. Ve Windows Phone 8.1 instanci, zadejte řetězec název uživatel v obou **uživatelské jméno** a **heslo** pole klikněte **přihlášení a registrace**.
5. Potom v **značky uživatelské jméno příjemce** pole, zadejte uživatelské jméno registrovaný na Windows 8.1. Zadejte zprávu oznámení a klikněte na tlačítko **odeslat nabízené**.
   
    ![][16]
6. Pouze zařízení, která byla zaregistrovaná odpovídající značky uživatelské jméno se zobrazí oznámení.
   
    ![][15]

## <a name="next-steps"></a>Další kroky
* Pokud chcete segmentovat uživatele podle zájmových skupin, přečtěte si kurz [Používání centra oznámení k odesílání novinek].
* Další informace o tom, jak používat Notification Hubs najdete v tématu [pokyny centra oznámení].

[9]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push9.png
[10]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push10.png
[11]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push11.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-ui.png
[14]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-windows-instance-username.png
[15]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-notification-received.png
[16]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-send-message.png



<!-- URLs. -->
[Začínáme s Notification Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[zabezpečení Push]: notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md
[Používání centra oznámení k odesílání novinek]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[pokyny centra oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
