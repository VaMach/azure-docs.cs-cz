<properties
    pageTitle="Odesílání nabízených oznámení do systému iOS pomocí Azure Notification Hubs | Microsoft Azure"
    description="V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace iOS."
    services="notification-hubs"
    documentationCenter="ios"
    keywords="nabízené oznámení;nabízená oznámení;nabízená oznámení ios"
    authors="wesmc7777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="wesmc"/>

# Odesílání nabízených oznámení do systému iOS pomocí Azure Notification Hubs

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##Přehled

> [AZURE.NOTE] K dokončení tohoto kurzu potřebujete mít aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).

V tomto kurzu zjistíte, jak používat Azure Notification Hubs k odesílání nabízených oznámení do aplikace systému iOS. Vytvoříte prázdnou aplikaci iOS, která přijímá nabízená oznámení pomocí [služby nabízených oznámení Apple (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html). 

Jakmile budete hotovi, budete moci používat vaše centra oznámení k vysílání nabízených oznámení pro všechna zařízení používající vaši aplikaci.

## Než začnete

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Dokončený kód v tomto kurzu lze najít v části [Github](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

##Požadavky

V tomto kurzu budete potřebovat následující:

+ [Mobile Services iOS SDK verze 1.2.4]
+ Poslední verze jazyka [Xcode]
+ Zařízení podporující iOS 8 (nebo novější verze)
+ Členství v [programu pro vývojáře Apple](https://developer.apple.com/programs/).

   > [AZURE.NOTE] Z důvodu požadavků na konfiguraci pro nabízená oznámení musíte nasadit a otestovat nabízená oznámení na fyzickém zařízení iOS (iPhone nebo iPad) namísto simulátoru iOS.

Dokončení tohoto kurzu je předpokladem pro všechny ostatní kurzy Notification Hubs pro aplikace iOS.

[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

##Konfigurace centra oznámení pro nabízená oznámení iOS

Tato části vás provede vytvořením nového centra oznámení a konfigurací ověřování s použitím služby APNS a pomoci vytvořeného nabízeného certifikátu **.p12**. Pokud chcete použít centrum oznámení, které jste již vytvořili, můžete přeskočit na krok 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li>
<p>Klikněte na tlačítko <b>Služby oznámení</b> v okně <b>Nastavení</b>  a pak vybrat <b>Apple (APNS)</b>. Klikněte na položku <b>Nahrát certifikát</b> a vyberte soubor <b>.p12</b>, který jste dříve exportovali. Ujistěte se, že zadáváte správné heslo.</p>
<p>Nezapomeňte vybrat režim <b>Sandboxu</b>, protože se jedná o vývoj. Používejte pouze režim <b>Výroba</b>, pokud chcete zasílat nabízená oznámení uživatelům, kteří si zakoupili aplikaci z obchodu s aplikacemi.</p>
</li>
</ol>
&emsp;&emsp;![Konfigurace služby APNS na portálu Azure Portal](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)

&emsp;&emsp;![Konfigurace certifikační služby APNS na portálu Azure](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)



Vaše centrum oznámení je nyní nakonfigurováno pro práci se službou APNS. Zároveň máte připojovací řetězce, pomocí kterých můžete svou aplikaci zaregistrovat pro odesílání nabízených oznámení.

##Připojte aplikaci iOS k centru oznámení

1. V Xcode vytvořte nový projekt iOS a vyberte šablonu **Jediné zobrazení aplikace**.

    ![Xcode – jediné zobrazení aplikace][8]

2. Když nastavujete možnosti pro nový projekt, nezapomeňte použít stejný **Název produktu** a **Identifikátor organizace**, který jste použili při předchozím nastavení sady ID na portálu pro vývojáře Apple.

    ![Xcode – možnosti projektu][11]

3. V části **Cíle** klikněte na název projektu, klikněte na kartu **Nastavení sestavení** a rozbalte **Identitu podepisování kódu** a pak v části **Ladění** nastavte svoji identitu podepisování kódu. Přepněte **Úrovně** ze **Základní** na **Všechny** a nastavte **Profil zřizování** na profil zřizování, který jste vytvořili dříve.

    Pokud nevidíte nový profil zřizování, který jste vytvořili v Xcode, pokuste se aktualizovat profily pro podpisové identity. Klikněte na tlačítko **Xcode** na panelu nabídek, klikněte na tlačítko **Předvolby**, klikněte na kartu **Účet**, klikněte na tlačítko **Zobrazit podrobnosti**, klikněte na podpisovou identitu a pak klikněte na tlačítko Aktualizovat v pravém dolním rohu.

    ![Xcode – profil zřizování][9]

4. Stáhněte si [Mobile Services iOS SDK verze 1.2.4] a soubor rozbalte. V Xcode klikněte pravým tlačítkem na projekt a klikněte na možnost **Přidat soubory do** a přidejte složku **WindowsAzureMessaging.framework** do projektu Xcode. Vyberte možnost **Kopírovat položky v případě potřeby** a pak klikněte na tlačítko **Přidat**.

    >[AZURE.NOTE] Centrum oznámení SDK aktuálně nepodporuje bitcode na Xcode 7.  Je nutné nastavit **Povolit Bitcode** na hodnotu **Ne** v části **Sestavení možnosti** pro váš projekt.

    ![Rozbalte Azure SDK][10]

5. Přidejte nový soubor záhlaví projektu s názvem `HubInfo.h`. Tento soubor bude obsahovat konstanty pro vaše centrum oznámení.  Přidejte následující definice a nahraďte zástupné symboly literálu řetězce ve vašem *názvu centra* a *DefaultListenSharedAccessSignature*, který jste si předtím poznamenali.

        #ifndef HubInfo_h
        #define HubInfo_h
        
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
        
        #endif /* HubInfo_h */

6. Otevřete váš soubor `AppDelegate.h` a přidejte následující direktivy importu:

         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
        
7. Ve vaší `AppDelegate.m file` přidejte následující kód do metody `didFinishLaunchingWithOptions` založené na vaší verzi iOS. Tento kód zaregistruje popisovač vašeho zařízení do APN:

    Pro iOS 8:

        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];

        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];

    Pro starší verze iOS před 8:

         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];


8. Do stejného souboru přidejte následující metody. Tento kód se připojí k centru oznámení pomocí informací o připojení zadaných do souboru HubInfo.h. Poté přiřadí token zařízení do centra oznámení tak, aby centrum oznámení mohlo odesílat oznámení:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];

            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }


9. Do stejného souboru přidejte následující metodu pro zobrazení **UIAlert**, pokud bylo přijato oznámení, že je aplikace aktivní:


        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

10. Sestavte a spusťte aplikaci na vašem zařízení a ověřte, zda nedochází k žádným chybám.

## Odešlete nabízená oznámení


Příjem oznámení můžete otestovat ve vaší aplikaci odesláním nabízených oznámení na [Azure Portal] prostřednictvím části **Poradce při potížích** v okně centra (použijte možnost *Testovací odeslání*).

![Portál Azure – testovací odeslání][30]

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## (Volitelné) Zasílání nabízených oznámení z aplikace

>[AZURE.IMPORTANT] Tento příklad odesílání oznámení z klientské aplikace poskytujeme jenom pro výukové účely. Vzhledem k tomu, že klientská aplikace bude muset obsahovat `DefaultFullSharedAccessSignature`, bude centrum oznámení vystavené riziku, že uživatel může získat přístup k odesílání neautorizovaných oznámení vašim klientům.

Pokud chcete zasílat nabízená oznámení z aplikace, tato část poskytuje příklad, jak to provést pomocí rozhraní REST.

1. V Xcode otevřete `Main.storyboard` a přidejte následující součásti uživatelského rozhraní z objektu knihovny a povolte uživatelům zasílání nabízených oznámení v aplikaci:

    - Popisek bez textu popisku. Použije se k nahlášení chyb v odesílání oznámení.  Vlastnost **Řádky** musí být nastavena na **0** tak, aby automaticky použila velikost omezenou na pravý a levý okraj a horní část zobrazení.
    - Textové pole s textem **Zástupný symbol** je nastaveno na hodnotu **Zadejte zprávu oznámení**. Omezte pole přímo pod popiskem, jak je uvedeno níže. Nastavte řadič zobrazení jako delegáta výstupu.
    - Tlačítko s názvem **Odeslat oznámení** omezené pod textové pole a ve vodorovném centru.

    Zobrazení by měl vypadat takto:

    ![Návrhář Xcode][32]


2. [Přidejte výstupy](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) pro popisek a textové pole připojené k vašemu zobrazení a aktualizujte vaše `interface` definice pro podporu `UITextFieldDelegate` a `NSXMLParserDelegate`. Přidejte tři vlastnosti deklarací uvedené níže, aby bylo možné podporovat volání rozhraní API REST a analyzovat odpověď.

    Váš soubor ViewController.h by měl vypadat následovně:

        #import <UIKit/UIKit.h>

        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }

        // Make sure these outlets are connected to your UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;

        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;

        @end

3. Otevřete `HubInfo.h` a přidejte následující konstanty, které budou použity pro zasílání oznámení do rozbočovače. Nahraďte zástupný symbol řetězcového literálu skutečným připojovacím řetězcem *DefaultFullSharedAccessSignature*.

        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"

4. Přidejte do souboru `ViewController.h` následující příkazy `#import`.

        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"

5. V `ViewController.m` přidejte následující kód pro implementaci rozhraní. Tento kód provede analýzu vašeho připojovacího řetězce *DefaultFullSharedAccessSignature*. Jak je uvedeno v [referenci rozhraní API REST](http://msdn.microsoft.com/library/azure/dn495627.aspx), tyto analyzované informace se použijí k vygenerování tokenu SaS pro hlavičku požadavku **Autorizace**.

        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;

        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;

            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];

                @throw parseException;
            }

            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }

6. V části `ViewController.m` aktualizujte metodu `viewDidLoad` k analýze připojovacího řetězce při načtení zobrazení. Přidejte také pomocné metody uvedené níže, pro implementaci rozhraní.  


        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





7. V `ViewController.m` přidejte následující kód do implementace rozhraní k vygenerování tokenu autorizace SaS, která bude k dispozici v hlavičce **Autorizace**, jak je uvedeno v [referenci rozhraní API REST](http://msdn.microsoft.com/library/azure/dn495627.aspx).

        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;

            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];

                // Get an hmac_sha1 Mac instance and initialize with the signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];

                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }

            return token;
        }


8. Využijte zkratku CTRL + přetažení z tlačítka **Odeslat oznámení** do `ViewController.m` a přidejte akci s názvem **SendNotificationMessage** pro událost **Touch Down**. Metoda aktualizace pomocí následujícího kódu pro odeslání oznámení pomocí rozhraní REST API.

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }

        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];

            // Apple Notification format of the notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];

            // Construct the message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];

            // Generate the token to be used in the authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the APNs notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];
            [dataTask resume];
        }


9. V `ViewController.m` přidejte následující metodu delegáta pro podporu zavření klávesnice pro textové pole. Využijte zkratku CTRL + přetažení z textového pole na ikonu řadiče zobrazení v návrháři rozhraní pro nastavení řadiče zobrazení jako delegáta výstupu.

        //===[ Implement UITextFieldDelegate methods ]===

        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }


10. V `ViewController.m` přidejte následující metody delegáta pro podporu analýzy odpovědi pomocí `NSXMLParser`.

        //===[ Implement NSXMLParserDelegate methods ]===

        -(void)parserDidStartDocument:(NSXMLParser *)parser
        {
            self.statusResult = @"";
        }

        -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
            namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
            attributes:(NSDictionary *)attributeDict
        {
            NSString * element = [elementName lowercaseString];
            NSLog(@"*** New element parsed : %@ ***",element);

            if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
            {
                self.currentElement = element;
            }
        }

        -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
        {
            self.statusResult = [self.statusResult stringByAppendingString:
                [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
        }

        -(void)parserDidEndDocument:(NSXMLParser *)parser
        {
            // Set the status label text on the UI thread
            dispatch_async(dispatch_get_main_queue(),
            ^{
                [self.sendResults setText:self.statusResult];
            });
        }



11. Sestavte projekt a ověřte, že se zde nenachází žádné chyby.


> [AZURE.NOTE] Pokud dojde k chybě sestavení v Xcode7 o podpoře bitcode, měli byste změnit **Nastavení sestavení** > **Povolit Bitcode (ENABLE_BITCODE)** na **NE** v Xcode. Centra oznámení SDK aktuálně nepodporují bitcode. 

Můžete najít všechna možná oznámení datových částí v Apple [Průvodci programováním místních a nabízených oznámení].


##Kontrola, zda vaše aplikace může přijímat nabízená oznámení

Chcete-li otestovat nabízená oznámení na iOS, musíte aplikaci nasadit do fyzického zařízení iOS. Nabízená oznámení Apple nelze odeslat pomocí simulátoru iOS.

1. Spusťte aplikaci a ověřte, zda byla registrace úspěšná a stiskněte klávesu **OK**.

    ![Test registrace nabízených oznámení aplikace iOS][33]

2. Můžete odeslat testovací nabízená oznámení z [Azure Portal], jak je popsáno výše. Pokud jste přidali kód pro odesílání nabízených oznámení v aplikaci, klepněte do textového pole a zadejte zprávu oznámení. Pak stiskněte tlačítko **Odeslat** na klávesnici nebo tlačítko **Odeslat oznámení** v zobrazení k odeslání zprávy oznámení.

    ![Test odeslání nabízených oznámení aplikace iOS][34]

3. Nabízená odeslání se zašlou na všechna zařízení, která jsou registrovaná pro příjem oznámení z konkrétní centra oznámení.

    ![Test příjmu nabízených oznámení aplikace iOS][35]


##Další kroky

V tomto příkladu jste vysílali nabízená oznámení pro všechna vaše registrovaná zařízení iOS. Jako další krok ve svém studiu doporučujeme pokračovat kurzem [Upozornění uživatelů centra oznámení Azure pro iOS pomocí backendu .NET], který vás provede procesem vytvoření backendu pro zasílání nabízených oznámení pomocí značek. 

Pokud chcete segmentovat uživatele podle zájmových skupin, můžete se navíc přesunout na tutoriál [Používání centra oznámení k odesílání novinek]. 

Obecné informace o centrech oznámení naleznete v tématu [Průvodce centry oznámení].



<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Mobile Services iOS SDK verze 1.2.4]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Odeslat stránku aplikace]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[Moje aplikace]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK pro Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Začínáme používat Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Portál Azure Classic]: https://manage.windowsazure.com/
[Průvodce centry oznámení]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Portál zřizování iOS]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Začínáme s nabízenými oznámeními v mobilních službách]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Upozornění uživatelů centra oznámení Azure pro iOS pomocí backendu .NET]: notification-hubs-aspnet-backend-ios-notify-users.md
[Používání centra oznámení k odesílání novinek]: notification-hubs-ios-send-breaking-news.md

[Průvodci programováním místních a nabízených oznámení]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure Portal]: https://portal.azure.com


<!---HONumber=Aug16_HO4-->


