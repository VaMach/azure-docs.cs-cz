<properties
    pageTitle="Azure Active Directory B2C: Volání webového rozhraní API z aplikace pro iOS přes knihovny třetích stran | Microsoft Azure"
    description="V tomto článku se dozvíte, jak vytvořit aplikaci „seznam úkolů“ pro iOS, která volá webové rozhraní API Node.js pomocí nosných tokenů OAuth 2.0 přes knihovnu třetí strany"
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags ms.service="active-directory-b2c" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic="hero-article"

    ms.date="07/26/2016"
    ms.author="brandwe"/>

# Azure AD B2C: Volání webového rozhraní API z aplikace pro iOS s použitím knihovny třetích stran

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Platforma Microsoft identity používá otevřené standardy, jako je například OAuth2 nebo OpenID Connect. To umožňuje vývojářům využívat všechny knihovny, které chtějí integrovat do našich služeb. Abychom vývojářům usnadnili používání naší platformy i s ostatními knihovnami, napsali jsme několik návodů, jako je tento. Popisují, jak nakonfigurovat knihovny třetích stran tak, aby se daly připojit k platformě Microsoft identity. Většinu knihoven, které implementují [specifikace RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749), bude možné připojit k platformě Microsoft Identity.


Pokud jste ještě nikdy nepracovali s OAuth2 nebo OpenID Connect, pak vám tahle vzorová konfigurace možná nebude moc dávat smysl. Doporučujeme prohlédnout si stručný [přehled protokolu, který uvádíme tady](active-directory-b2c-reference-protocols.md).

> [AZURE.NOTE]
    Některé funkce naší platformy, které mají vyjádření v rámci těchto standardů (například podmíněný přístup nebo správa zásad služby Intune), vyžadují, abyste použili open source knihovny identit Microsoft Azure. 
   
Na platformě B2C nejsou podporovány úplně všechny scénáře a funkce Azure Active Directory.  Pokud chcete zjistit, zda byste měli používat platformu B2C, přečtěte si článek [o omezeních B2C](active-directory-b2c-limitations.md).


## Získání adresáře služby Azure AD B2C

Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta. Adresář je kontejner pro všechny vaše uživatele, aplikace, skupiny a další. Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.

## Vytvoření aplikace

Dále musíte vytvořit aplikaci v adresáři B2C. Azure AD díky tomu získá informace potřebné k bezpečné komunikaci s vaší aplikací. Aplikace i webové rozhraní API budou mít v tomto případě stejné **ID aplikace**, protože společně tvoří jednu logickou aplikaci. Chcete-li vytvořit aplikaci, postupujte podle [těchto pokynů](active-directory-b2c-app-registration.md). Ujistěte se, že:

- Zahrnete do aplikace **mobilní zařízení**.
- Poznamenejte si **ID aplikace** přiřazené vaší aplikaci. To také budete potřebovat později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Vytvořte svoje zásady

V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md). Tahle aplikace obsahuje možnosti pro jednu identitu: kombinované přihlášení a registraci. Tuto zásadu je třeba vytvořit pro každý typ činnosti, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Při vytváření zásady nezapomeňte na následující:

- Ve vytvářené zásadě zvolit **zobrazovaný název** a atributy registrace.
- Zvolit deklarace identity aplikace **Zobrazovaný název** a **ID objektu** v každé zásadě. Můžete zvolit i další deklarace identity.
- Po vytvoření každé zásady si poznamenejte její **Název**. Měl by mít předponu `b2c_1_`.  Název zásady budete potřebovat později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po vytvoření zásad jste připraveni k sestavení aplikace.


## Stáhněte si kód

Kód k tomuto kurzu je udržovaný [na GitHubu](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c).  Můžete si [stáhnout aplikaci jako soubor .zip](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)/archive/master.zip), nebo jej klonovat:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

Můžete si také stáhnout kompletní kód a začít ihned: 

```
git clone --branch complete git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

## Stáhnutí knihovny třetí strany nxoauth2 a spuštění pracovního prostředí

V rámci tohoto návodu budeme používat klienta OAuth2Client z GItHubu a knihovnu OAuth2 pro Mac OS X a iOS (Cocoa a Cocoa Touch). Tahle knihovna je založena na konceptu 10 specifikace OAuth2. Implementuje profil nativní aplikace a podporuje koncový bod autorizace koncového uživatele. To je vše, co budeme potřebovat k integraci s platformou Microsoft identity.

### Přidání knihovny do projektu pomocí CocoaPods

CocoaPods je správce závislostí pro projekty Xcode. Automaticky spravuje výše uvedené kroky instalace.

```
$ vi Podfile
```
Do tohoto souboru podfile přidejte následující:

```
 platform :ios, '8.0'
 
 target 'SampleforB2C' do
 
 pod 'NXOAuth2Client'
 
 end
```

Nyní soubor podfile načtěte pomocí cocoapods. Tím se vytvoří nový pracovní prostor XCode, který načtete.

```
$ pod install
...
$ open SampleforB2C.xcworkspace

```

## Struktura projektu

Pro náš projekt máme nastavenou zhruba následující strukturu:

* **Hlavní pohled** s podoknem úloh
* **Pohled pro přidání úlohy** pro data o vybrané úloze
* **Pohled pro přihlášení**, který umožňuje uživateli přihlásit se k aplikaci.

Abychom přidali ověřování, budeme přecházet k různým souborům projektu. Další části kódu (například kód vizuálu), které jsou vám poskytnuty, nejsou pro identitu relevantní.

## Vytvoření souboru `settings.plist` pro aplikaci

Aplikace se dá snáze konfigurovat, pokud máme pro konfigurační hodnoty centralizované umístění. To vám pomůže lépe pochopit, jak které nastavení v aplikaci funguje. Tyhle hodnoty budeme aplikaci poskytovat přes *seznam vlastností*.

* V pracovním prostoru aplikace vytvořte nebo otevřete `settings.plist` níže uvedený soubor `Supporting Files`

* Zadejte následující hodnoty (později si je rozebereme podrobněji)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>accountIdentifier</key>
    <string>B2C_Acccount</string>
    <key>clientID</key>
    <string><client ID></string>
    <key>clientSecret</key>
    <string></string>
    <key>authURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/authorize?p=<policy name></string>
    <key>loginURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/login</string>
    <key>bhh</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>tokenURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/token?p=<policy name></string>
    <key>keychain</key>
    <string>com.microsoft.azureactivedirectory.samples.graph.QuickStart</string>
    <key>contentType</key>
    <string>application/x-www-form-urlencoded</string>
    <key>taskAPI</key>
    <string>https://aadb2cplayground.azurewebsites.net</string>
</dict>
</plist>
```

Nyní si vše rozeberme podrobně.


Jak vidíte, u `authURL`, `loginURL`, `bhh`, `tokenURL` je třeba vyplnit název klienta. Je to název klienta, který vám byl přiřazen pro klienta B2C. Například `kidventusb2c.onmicrosoft.com`. Pokud používáte naše open source knihovny identit Microsoft Azure, můžete si od nás tato data načíst přes koncový bod metadat. Náročnou práci jsme udělali za vás a tyhle hodnoty jsme extrahovali.

Další informace o názvech klientů B2C najdete tady: [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)

Hodnota `keychain` je kontejner, který knihovna NXOAuth2Client použije k vytvoření řetězce klíčů pro ukládání tokenů. Pokud chcete získat jednotné přihlašování do většího počtu aplikací, můžete v každé aplikaci specifikovat stejný řetězec klíčů, stejně jako vyžadovat použití tohoto řetězce klíčů v požadavcích XCode. Tento proces je popsán v dokumentaci Apple.

`<policy name>`  na konci každé adresy URL představuje místa, kam můžete umístit zásadu, kterou jste vytvořili výše. Aplikace bude volat tyhle zásady v závislosti na toku.

`taskAPI` je koncový bod REST, který budeme volat přes token B2C, buď abychom přidali úlohy, nebo odeslali dotaz na úlohy existující. Tohle bylo nastaveno zvlášť pro tento příklad. Ten bude fungovat, i když nic nezměníte.

Zbytek těchto hodnot je potřeba k používání knihovny a k vytváření míst, kde můžete přenášet hodnoty do kontextu.

Když jsme teď vytvořili soubor `settings.plist`, potřebujeme kód pro jeho čtení.

## Nastavení třídy AppData tak, aby přečetla naše nastavení

Vytvoříme jednoduchý soubor, který bude parsovat soubor `settngs.plist`, který jsme vytvořili výše, a v budoucnu zpřístupní tato nastavení jakékoliv třídě. Protože nechceme vytvářet novou kopii dat pokaždé, když o ně třída požádá, použijeme vzor s jedním prvkem a vrátíme jenom stejnou instanci, která se vytváří pokaždé, když je na nastavení vytvořen požadavek

* Vytvořte soubor `AppData.h`:

```objc
#import <Foundation/Foundation.h>

@interface AppData : NSObject

@property(strong) NSString *accountIdentifier;
@property(strong) NSString *taskApiString;
@property(strong) NSString *authURL;
@property(strong) NSString *clientID;
@property(strong) NSString *loginURL;
@property(strong) NSString *bhh;
@property(strong) NSString *keychain;
@property(strong) NSString *tokenURL;
@property(strong) NSString *clientSecret;
@property(strong) NSString *contentType;

+ (id)getInstance;

@end
```

* Vytvořte soubor `AppData.m`:

```objc
#import "AppData.h"

@implementation AppData

+ (id)getInstance {
  static AppData *instance = nil;
  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{
    instance = [[self alloc] init];

    NSDictionary *dictionary = [NSDictionary
        dictionaryWithContentsOfFile:[[NSBundle mainBundle]
                                         pathForResource:@"settings"
                                                  ofType:@"plist"]];
    instance.accountIdentifier = [dictionary objectForKey:@"accountIdentifier"];
    instance.clientID = [dictionary objectForKey:@"clientID"];
    instance.clientSecret = [dictionary objectForKey:@"clientSecret"];
    instance.authURL = [dictionary objectForKey:@"authURL"];
    instance.loginURL = [dictionary objectForKey:@"loginURL"];
    instance.bhh = [dictionary objectForKey:@"bhh"];
    instance.tokenURL = [dictionary objectForKey:@"tokenURL"];
    instance.keychain = [dictionary objectForKey:@"keychain"];
    instance.contentType = [dictionary objectForKey:@"contentType"];
    instance.taskApiString = [dictionary objectForKey:@"taskAPI"];

  });

  return instance;
}
@end
```

Jak uvidíte níže, můžeme nyní snadno získat data prostým voláním `  AppData *data = [AppData getInstance];` v jakékoliv z našich tříd.



## Nastavení knihovny NXOAuth2Client v AppDelegate

Knihovna NXOAuthClient vyžaduje nastavení určitých hodnot. Potom můžete použít získaný token, který je třeba k volání API služby REST. Protože víme, že `AppDelegate` bude volán vždy, když načteme aplikaci, měli bychom do tohoto souboru vložit konfigurační hodnoty.
* Otevřete soubor `AppDelegate.m`

* Importujte některé soubory s hlavičkami, které použijeme později.

```objc
#import "NXOAuth2.h" // the Identity library we are using
#import "AppData.h" // the class we just created we will use to load the settings of our application
```

* Do AppDelegate přidejte metodu `setupOAuth2AccountStore`

Potřebujeme vytvořit úložiště účtů AccountStore a pak do něj vložit data, která jsme načetli ze souboru `settings.plist`.

V této chvíli bychom vás v souvislosti se službou B2C chtěli upozornit na několik věcí, které vám pomohou lépe porozumět tomuto kódu:


1. Azure AD B2C používá *zásadu*, která je uvedena v parametrech dotazů pro zpracování vašeho požadavku. To umožňuje službě Azure Active Directory fungovat nezávisle, pouze pro vaši aplikaci. Abychom mohli poskytnout tyhle zvláštní parametry dotazů, potřebujeme poskytnout metodu `kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:` s parametry našich vlastních zásad. 

2. Azure AD B2C používá obory téměř stejným způsobem jako ostatní servery OAuth2. Protože se při používání B2C řeší ověřování uživatele i přístup k prostředkům, jsou ke správnému fungování toku některé obory naprosto nezbytné. Tohle je obor `openid`. Obor `openid` vám automaticky poskytují naše sady SDK Microsoft identity, takže jej v konfiguraci sady SDK neuvidíte. Protože ale používáme knihovnu třetí strany, musíme tento obor specifikovat.

```objc
- (void)setupOAuth2AccountStore {
  AppData *data = [AppData getInstance]; // The singleton we use to get the settings

  NSDictionary *customHeaders =
      [NSDictionary dictionaryWithObject:@"application/x-www-form-urlencoded"
                                  forKey:@"Content-Type"];

  // Azure B2C needs
  // kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters for
  // sending policy to the server,
  // therefore we use -setConfiguration:forAccountType:
  NSDictionary *B2cConfigDict = @{
    kNXOAuth2AccountStoreConfigurationClientID : data.clientID,
    kNXOAuth2AccountStoreConfigurationSecret : data.clientSecret,
    kNXOAuth2AccountStoreConfigurationScope :
        [NSSet setWithObjects:@"openid", data.clientID, nil],
    kNXOAuth2AccountStoreConfigurationAuthorizeURL :
        [NSURL URLWithString:data.authURL],
    kNXOAuth2AccountStoreConfigurationTokenURL :
        [NSURL URLWithString:data.tokenURL],
    kNXOAuth2AccountStoreConfigurationRedirectURL :
        [NSURL URLWithString:data.bhh],
    kNXOAuth2AccountStoreConfigurationCustomHeaderFields : customHeaders,
    //      kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:customAuthenticationParameters
  };

  [[NXOAuth2AccountStore sharedStore] setConfiguration:B2cConfigDict
                                        forAccountType:data.accountIdentifier];
}
```
Dále zkontrolujte, zda ji opravdu voláte v AppDelegate pod metodou `didFinishLaunchingWithOptions:`. 

```
[self setupOAuth2AccountStore];
```


## Vytvoření třídy `LoginViewController`, kterou použijeme k vyřizování požadavku na ověřování

Pro přihlášení k účtu používáme webové zobrazení. To nám umožňuje požádat uživatele o další faktory, jako je například textová zpráva SMS (pokud je nakonfigurována) nebo zasílání chybových hlášení zpět uživateli. Tady nastavíme webové zobrazení a potom napíšeme kód pro zpracování zpětných dotazů, které budou přicházet ve webovém zobrazení přes službu Microsoft Identity.

* Vytvořte třídu `LoginViewController.h`

```objc
@interface LoginViewController : UIViewController <UIWebViewDelegate>
@property(weak, nonatomic) IBOutlet UIWebView *loginView; // Our webview that we will use to do authentication

- (void)handleOAuth2AccessResult:(NSURL *)accessResult; // Allows us to get a token after we've received an Access code.
- (void)setupOAuth2AccountStore; // We will need to add to our OAuth2AccountStore we setup in our AppDelegate
- (void)requestOAuth2Access; // This is where we invoke our webview.
```

Vytvoříme každou z níže uvedených metod.

> [AZURE.NOTE] 
    Nezapomeňte vytvořit vazbu `loginView` na aktuální webové zobrazení, které je obsaženo ve scénáři. Jinak nebudete mít webové zobrazení, které se automaticky otevře, když je čas na ověření.

* Vytvořte třídu `LoginViewController.m`

* Přidejte několik proměnných, které budou během ověřování přenášet stav

```objc
NSURL *myRequestedUrl; \\ The URL request to Azure Active Directory 
NSURL *myLoadedUrl; \\ The URL loaded for Azure Active Directory
bool loginFlow = FALSE; 
bool isRequestBusy; \\ A way to give status to the thread that the request is still happening
NSURL *authcode; \\ A placeholder for our auth code.
```

* Přepište metody webového zobrazení pro práci s ověřováním

Musíme webovému zobrazení sdělit, jak se má chovat, když se uživatel potřebuje přihlásit, jak je popsáno výše. Můžete jednoduše zkopírovat a vložit níže uvedený kód.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
  // We get the auth token from a redirect so we need to handle that in the
  // webview.

  if (![NSThread isMainThread]) {
    [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:)
                           withObject:URL
                        waitUntilDone:YES];
    return;
  }

  NSURLRequest *hostnameURLRequest =
      [NSURLRequest requestWithURL:URL
                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                   timeoutInterval:10.0f];
  isRequestBusy = YES;
  [self.loginView loadRequest:hostnameURLRequest];

  NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)",
        self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView
    shouldStartLoadWithRequest:(NSURLRequest *)request
                navigationType:(UIWebViewNavigationType)navigationType {
  AppData *data = [AppData getInstance];

  NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL,
        (long)navigationType);

  // The webview is where all the communication happens. Slightly complicated.

  myLoadedUrl = [webView.request mainDocumentURL];
  NSLog(@"***Loaded url: %@", myLoadedUrl);

  // if the UIWebView is showing our authorization URL or consent URL, show the
  // UIWebView control
  if ([request.URL.absoluteString rangeOfString:data.authURL
                                        options:NSCaseInsensitiveSearch]
          .location != NSNotFound) {
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.loginURL
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.bhh
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = YES;
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  } else {
    self.loginView.hidden = NO;
    // read the Location from the UIWebView, this is how Microsoft APIs is
    // returning the
    // authentication code and relation information. This is controlled by the
    // redirect URL we chose to use from Microsoft APIs
    // continue the OAuth2 flow
    // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  }

  return YES;
}

```

* Napište kód pro zpracování výsledku požadavku OAuth2

Potřebujeme kód, který bude zpracovávat redirectURL, která se vrací z webového zobrazení. Pokud se to nepodařilo, zkusíme to znovu. Mezitím knihovna zahlásí chybu, kterou uvidíte v konzole, nebo kterou nesynchronně zpracujete. 

```objc
- (void)handleOAuth2AccessResult:(NSURL *)accessResult {
  // parse the response for success or failure
  if (accessResult)
  // if success, complete the OAuth2 flow by handling the redirect URL and
  // obtaining a token
  {
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
  } else {
    // start over
    [self requestOAuth2Access];
  }
}
```

* Nastavte faktory pro oznámení.

Vytvoříme stejnou metodu, jako je výše uvedená `AppDelegate`. Tentokrát ale přidáme i příkazy `NSNotification`, aby nám sdělily, co se v naší službě děje. Nastavíme pozorovatele, který nám oznámí jakoukoliv změnu tokenu. Jakmile získáme token, vrátíme uživatele zpátky do `masterView`.



```objc
- (void)setupOAuth2AccountStore {
  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                if (aNotification.userInfo) {
                  // account added, we have access
                  // we can now request protected data
                  NSLog(@"Success!! We have an access token.");
                  dispatch_async(dispatch_get_main_queue(), ^{

                    MasterViewController *masterViewController =
                        [self.storyboard
                            instantiateViewControllerWithIdentifier:@"master"];
                    [self.navigationController
                        pushViewController:masterViewController
                                  animated:YES];
                  });
                } else {
                  // account removed, we lost access
                }
              }];

  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                NSError *error = [aNotification.userInfo
                    objectForKey:NXOAuth2AccountStoreErrorKey];
                NSLog(@"Error!! %@", error.localizedDescription);
              }];
}

```
* Přidejte kód, který uživatele zpracuje vždy, když se spustí požadavek pro přihlášení

Vytvoříme metodu, kterou budeme volat pokaždé, když obdržíme požadavek na ověření. Bude to metoda, která vytváří webové zobrazení

```objc
- (void)requestOAuth2Access {
  AppData *data = [AppData getInstance];

  // in order to login to Mircosoft APIs using OAuth2 we must show an embedded
  // browser (UIWebView)
  [[NXOAuth2AccountStore sharedStore]
           requestAccessToAccountWithType:data.accountIdentifier
      withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
        // navigate to the URL returned by NXOAuth2Client

        NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
        [self.loginView loadRequest:r];
      }];
}
```

* Nakonec zavoláme všechny výše uvedené metody pokaždé, když se načte `LoginViewController`. To provedeme tak, že tyhle metody přidáme k naší metodě `viewDidLoad`, kterou nám poskytuje Apple

```objc
  [super viewDidLoad];
  // Do any additional setup after loading the view.

  // OAuth2 Code

  self.loginView.delegate = self;
  [self requestOAuth2Access];
  [self setupOAuth2AccountStore];
  NSURLCache *URLCache =
      [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                    diskCapacity:20 * 1024 * 1024
                                        diskPath:nil];
  [NSURLCache setSharedURLCache:URLCache];
```

Tím jste vytvořili hlavní způsob, jakým budeme pracovat s naší aplikací pro přihlašování. Až se přihlásíme, bude třeba použít tokeny, které jsme obdrželi. Za tímto účelem vytvoříme pomocný kód, který zavolá API služby REST s použitím této knihovny.


## Vytvoření třídy `GraphAPICaller` pro zpracování našich požadavků na rozhraní API služby REST

Pokaždé, když načteme aplikaci, načte se nám i konfigurace. Jakmile obdržíme token, musíme s ním něco udělat. 

* Vytvořte soubor `GraphAPICaller.h`

```objc
@interface GraphAPICaller : NSObject <NSURLConnectionDataDelegate>

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock;

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *error))completionBlock;

@end
```

Z tohoto kódu vidíte, že budeme vytvářet dvě metody: jedna bude získávat úlohy z API, druhá bude úlohy do API přidávat.

Nyní jsme vytvořili rozhraní a můžeme přidat aktuální implementaci:

* Vytvořte `GraphAPICaller.m file`

```objc
@implementation GraphAPICaller

// 
// Gets the tasks from our REST endpoint we specified in settings
//

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *))completionBlock

{
  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSMutableArray *Tasks = [[NSMutableArray alloc] init];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"GET"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:nil
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (!error) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          if ([dataReturned count] != 0) {

            for (NSMutableDictionary *theTask in dataReturned) {

              Task *t = [[Task alloc] init];
              t.name = [theTask valueForKey:@"Text"];

              [Tasks addObject:t];
            }
          }

          completionBlock(Tasks, nil);
        } else {
          completionBlock(nil, error);
        }

      }];
}

// 
// Adds a task from our REST endpoint we specified in settings
//

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock {

  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSDictionary *params = [self convertParamsToDictionary:task.name];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"POST"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:params
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (responseData) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          completionBlock(TRUE, nil);
        } else {
          completionBlock(FALSE, error);
        }

      }];
}

+ (NSDictionary *)convertParamsToDictionary:(NSString *)task {
  NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

  [dictionary setValue:task forKey:@"Text"];

  return dictionary;
}

@end
```

## Spuštění ukázkové aplikace

Nakonec sestavte a spusťte aplikaci v Xcode. Zaregistrujte se nebo se přihlaste do aplikace a poté vytvořte pro přihlášeného uživatele úkoly. Odhlaste se a přihlaste se zpět jako jiný uživatel a vytvořte úkoly pro tohoto uživatele.

Všimněte si, že úkoly se v rozhraní API ukládají pro každého uživatele zvlášť, protože rozhraní API extrahuje identitu uživatele z přístupového tokenu, který obdrží.


## Další kroky

Nyní se můžete přesunout k pokročilejším tématům o B2C. Můžete vyzkoušet:

[Volání webového rozhraní API Node.js z webové aplikace Node.js]()

[Přizpůsobení uživatelského rozhraní aplikace B2C]()



<!---HONumber=Aug16_HO4-->


