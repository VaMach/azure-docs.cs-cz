---
title: "Architektura Push Notification Hubs – Enterprise"
description: "Pokyny k používání Azure Notification Hubs v podnikovém prostředí"
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 903023e9-9347-442a-924b-663af85e05c6
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: d71c706a7db570e88339c4ff7af05a48c05df65b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="enterprise-push-architectural-guidance"></a>Doprovodné materiály k architektuře nabízení v podnicích
Podniky jsou dnes postupně přesunutí směrem vytváření mobilních aplikací buď pro své koncové uživatele (externí) nebo pro zaměstnance (interní). Mají existující back-end systémy zavedené je jej sálové počítače nebo některé obchodní aplikace, které musí být integrován do architektury mobilních aplikací. Tato příručka se mluvit o tom, jak vhodné provést této integrace doporučujeme možná řešení pro běžné scénáře.

Časté požadavek je pro odesílání nabízených oznámení uživatelům prostřednictvím jejich mobilních aplikací při výskytu události týkající se v systémech back-end. Například Banka zákazníkovi, který má tato banka bankovní aplikace na svém zařízení iPhone chce být upozorněni, když MD přišla nad určitou míru z svůj účet nebo scénářem intranetu, kde zaměstnanci z finančního oddělení, který má aplikace schválení na nároky na jeho Windows Phone chce být upozorněni, když mu získá žádost o schválení.

Bankovní účet nebo zpracování schválení je pravděpodobné, v některých systém back-end, který musí inicializovat oznámení pro uživatele. Může existovat více takové back-end systémy, které musíte sestavit stejný druh logiku pro implementaci nabízená oznámení se aktivuje událost. Složitost zde spočívá v integraci několik systémů back-end společně s jeden nabízené systému, kde koncoví uživatelé mohou mít předplatné jiný oznámení a může i více mobilních aplikací, například v případě mobilních aplikací na intranetu ve může jeden mobilních aplikací chcete oznámení dostávat, více takového systému back-end. Back-end systémy neznáte nebo potřebujete vědět nabízené sémantiku/technologie tak běžné řešení tradičně byl zavádět komponentu, který posuzuje systémy back-end pro všechny události, které vás zajímají a je odpovědná za zasílání zpráv nabízené klientovi.
Zde se věnuje ještě lepší řešení pomocí Azure Service Bus - model téma/odběr, který se sníží složitost při vytváření škálovatelné řešení.

Tady je obecné architektuře řešení (zobecněný s více mobilních aplikací, ale platí po jenom jedna mobilní aplikace)

## <a name="architecture"></a>Architektura
![][1]

Klíčovou do tohoto diagramu, architektury je Azure Service Bus, která poskytuje témata nebo odběry programovací model (Další informace o jeho v [Service Bus Pub nebo Sub programování]). Příjemce, který v tomto případě je mobilního back-endu (obvykle [Azure Mobile Service], která zahájí oznámení na mobilní aplikace) nejsou doručovány zprávy přímo z back-end systémy, ale místo toho máme zprostředkující abstraktní vrstvu poskytované [Azure Service Bus] což umožňuje mobilní back-end pro příjem zpráv z jednoho nebo více systémů back-end. Téma sběrnice je potřeba vytvořit pro každou z back-end systémy například účet oddělení lidských zdrojů, Finance, které jsou v podstatě "témata", které vás zajímají, která zahájí zpráv k odeslání jako nabízené oznámení. Back-end systémy bude odesílat zprávy na tato témata. Back-end Mobile se mohou přihlásit na jeden nebo více těchto témata vytvořením odběru služby Service Bus. To bude získat mobilní back-end pro příjem oznámení z back-end serveru odpovídající oprávnění. Mobilní back-end i nadále přijímat zprávy o svých předplatných a ihned po doručení zprávy změní zpět a odešle ji jako oznámení jeho centra oznámení. Centra oznámení potom nakonec doručení zprávy do mobilní aplikace. Proto to Shrneme klíčové komponenty, jsme provedli následující:

1. Back-end systémy (systémy LoB nebo starší)
   * Vytvoří téma sběrnice
   * Odešle zprávu
2. Mobilní back-end
   * Vytvoří předplatné služby
   * Přijme zprávu (z back-end systému)
   * Odešle oznámení klientům (prostřednictvím centra oznámení Azure)
3. Mobilní aplikace
   * Přijímá a zobrazovat oznámení

### <a name="benefits"></a>Výhody:
1. Oddělení mezi příjemce (mobilní aplikaci nebo službě prostřednictvím centra oznámení) a odesílatele (back-end systémy) umožňuje další back-end systémy, kterou je prováděna integrace s minimální změny.
2. Také díky tomu scénář více mobilních aplikací, bude možné přijímat události z jednoho nebo více systémů back-end.  

## <a name="sample"></a>Ukázka:
### <a name="prerequisites"></a>Požadavky
Musíte provést následující kurzy seznamte s koncepty a také běžné vytvoření & kroky konfigurace:

1. [Service Bus Pub nebo Sub programování] – Tato část popisuje podrobnosti o práci s Service Bus témata nebo předplatných, postup vytvoření oboru názvů tak, aby obsahovala témata nebo předplatného, jak odesílat a přijímat zprávy z nich.
2. [Kurzu centra oznámení – Windows Universal] – Tato část popisuje postup nastavení aplikace pro Windows Store a používat Notification Hubs k registraci a pak přijímat oznámení.

### <a name="sample-code"></a>Ukázka kódu
Úplný ukázkový kód je k dispozici na [ukázky centra oznámení]. Ho je rozdělená do tří součástí:

1. **EnterprisePushBackendSystem**
   
    a. Používá tento projekt *WindowsAzure.ServiceBus* balíček Nuget a je založena na [Service Bus Pub nebo Sub programování].
   
    b. Toto je jednoduchá C# konzole aplikace k simulaci s LoB systémem, který zahájí zprávu, která se bude doručen do mobilní aplikace.
   
        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create the topic where we will send notifications
            CreateTopic(connectionString);
   
            // Send message
            SendMessage(connectionString);
        }
   
    c. `CreateTopic`se používá k vytvoření tématu Service Bus kde bude odesílat zprávy.
   
        public static void CreateTopic(string connectionString)
        {
            // Create the topic if it does not exist already
   
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);
   
            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }
   
    d. `SendMessage`se používá k odeslání zprávy do tohoto tématu Service Bus. Zde jsme se jednoduše odeslat sadu náhodné zprávy do tématu pravidelně pro účely ukázky. Obvykle bude systém back-end, který bude odesílat zprávy, když dojde k události.
   
        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);
   
            // Sends random messages every 10 seconds to the topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched to a different team."
            };
   
            while (true)
            {
                Random rnd = new Random();
                string employeeId = rnd.Next(10000, 99999).ToString();
                string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);
   
                // Send Notification
                BrokeredMessage message = new BrokeredMessage(notification);
                client.Send(message);
   
                Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);
   
                System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
            }
        }
2. **ReceiveAndSendNotification**
   
    a. Používá tento projekt *WindowsAzure.ServiceBus* a *Microsoft.Web.WebJobs.Publish* Nuget balíčků a je založena na [Service Bus Pub nebo Sub programování].
   
    b. Toto je jiný C# konzole aplikace, která jsme se spustit jako [webové úlohy Azure] vzhledem k tomu, že je spouštět nepřetržitě přijímat zprávy ze systémů LoB nebo back-end. Bude součástí mobilního back-endu.
   
        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create the subscription which will receive messages
            CreateSubscription(connectionString);
   
            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }
   
    c. `CreateSubscription`slouží k vytvoření odběru služby Service Bus pro téma kde systému back-end bude odesílat zprávy. V závislosti na podnikový scénář bude tato součást vytvořit jeden nebo více odběrů na odpovídající témata (například některé může být přijímání zpráv ze systému oddělení lidských zdrojů, některé z finančního systému a tak dále)
   
        static void CreateSubscription(string connectionString)
        {
            // Create the subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);
   
            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }
   
    d. ReceiveMessageAndSendNotification se používá k tuto zprávu přečíst v tématu pomocí svého předplatného a pokud čtení úspěšné potom vytvořit oznámení (v ukázkovém scénáři Windows nativní informační) k odeslání do mobilní aplikace pomocí Azure Notification Hubs.
   
        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize the Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");
   
            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);
   
            Client.Receive();
   
            // Continuously process messages received from the subscription
            while (true)
            {
                BrokeredMessage message = Client.Receive();
                var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";
   
                if (message != null)
                {
                    try
                    {
                        Console.WriteLine(message.MessageId);
                        Console.WriteLine(message.SequenceNumber);
                        string messageBody = message.GetBody<string>();
                        Console.WriteLine("Body: " + messageBody + "\n");
   
                        toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                        SendNotificationAsync(toastMessage);
   
                        // Remove message from subscription
                        message.Complete();
                    }
                    catch (Exception)
                    {
                        // Indicate a problem, unlock message in subscription
                        message.Abandon();
                    }
                }
            }
        }
        static async void SendNotificationAsync(string message)
        {
            await hub.SendWindowsNativeNotificationAsync(message);
        }
   
    e. Pro publikování jako **webové úlohy**, klikněte pravým tlačítkem na řešení v sadě Visual Studio a vyberte **publikovat jako webová úloha**
   
    ![][2]
   
    f. Vyberte svůj profil publikování a vytvoření nového webu Azure, pokud ji už neexistuje, který bude hostitelem této webové úlohy, a až pak máte na webu **publikovat**.
   
    ![][3]
   
    g. Nakonfigurujte úlohu, která má být "Spouštět nepřetržitě" tak, aby při přihlášení do [portálu Azure Classic] měli byste vidět nějak takto:
   
    ![][4]
3. **EnterprisePushMobileApp**
   
    a. Toto je aplikace Windows Store, která bude přijímat oznámení informačního nápisu z webová úloha spuštěna jako součást mobilního back-endu a zobrazit ji. To je založené na [Kurzu centra oznámení – Windows Universal].  
   
    b. Ujistěte se, že aplikace je povoleno přijímat oznámení informačního nápisu.
   
    c. Ujistěte se, že následující Notification Hubs registrační kód je volána v aplikaci spuštění (po nahrazení *HubName* a *DefaultListenSharedAccessSignature*:
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a>Spuštění ukázkové:
1. Zajistěte, aby vaše webová úloha pracuje správně a naplánované "Nepřetržitě spustit".
2. Spustit **EnterprisePushMobileApp** který se spustí aplikace pro Windows Store.
3. Spustit **EnterprisePushBackendSystem** konzolovou aplikaci, která bude simulovat LoB back-end a spustí odesílání zpráv a měli byste vidět informační zprávy, které jsou uvedeny jako následující:
   
    ![][5]
4. Zprávy byly původně odeslána do témat Service Bus, které byl monitorován odběry služby Service Bus ve webové úlohy. Jakmile byla přijata zpráva, oznámení se vytváří a odesílají do mobilní aplikace. Můžete zobrazit prostřednictvím protokolů webové úlohy potvrďte zpracování při přechodu na odkaz protokoly v [portálu Azure Classic] pro webové úlohy:
   
    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[ukázky centra oznámení]: https://github.com/Azure/azure-notificationhubs-samples
[Azure Mobile Service]: http://azure.microsoft.com/documentation/services/mobile-services/
[Azure Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Service Bus Pub nebo Sub programování]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[webové úlohy Azure]: ../app-service/web-sites-create-web-jobs.md
[Kurzu centra oznámení – Windows Universal]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[portálu Azure Classic]: https://manage.windowsazure.com/
