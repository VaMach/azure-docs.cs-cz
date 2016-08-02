<properties
    pageTitle="Azure Active Directory B2C ve verzi Preview: Volání webového rozhraní API z aplikace pro iOS | Microsoft Azure"
    description="V tomto článku se dozvíte, jak vytvořit aplikaci „seznam úkolů“ pro iOS, která volá webové rozhraní API Node.js pomocí nosných tokenů OAuth 2.0. Aplikace pro iOS i webové rozhraní API používají Azure Active Directory B2C ke správě identit uživatelů a k ověřování uživatelů."
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags ms.service="active-directory-b2c" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic="hero-article"

    ms.date="05/31/2016"
    ms.author="brandwe"/>

# Azure AD B2C ve verzi Preview: Volání webového rozhraní API z aplikace pro iOS

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Pomocí Azure Active Directory (Azure AD) B2C můžete v několika krocích přidat do svojí aplikace pro iOS a do webového rozhraní API výkonné funkce samoobslužné správy identity. V tomto článku se dozvíte, jak vytvořit aplikaci „seznam úkolů“ pro iOS, která volá webové rozhraní API Node.js pomocí nosných tokenů OAuth 2.0. Aplikace pro iOS i webové rozhraní API používají Azure AD B2C ke správě identit uživatelů a k ověřování uživatelů.

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

> [AZURE.NOTE]
    Pro správné fungování tohoto Rychlého startu je potřeba mít už webové rozhraní API zabezpečené pomocí Azure AD B2C. Sestavili jsme pro vás Rychlý start pro .NET a Node.js. Tento podrobný návod předpokládá, že je nakonfigurované ukázkové webové rozhraní API Node.js. Pro více informací viz [ukázku webového rozhraní API Azure Active Directory pro Node.js](active-directory-b2c-devquickstarts-api-node.md).
).

> [AZURE.NOTE]
    Tento článek se nezabývá implementací registrace, přihlašování a správy profilů pomocí Azure AD B2C. Zaměřuje se na volání webových rozhraní API po ověření uživatele. Pokud jste tak ještě neučinili, měli byste začít s [kurzem Začínáme s webovými aplikacemi .NET](active-directory-b2c-devquickstarts-web-dotnet.md), kde naleznete základy Azure AD B2C.

## Získání adresáře služby Azure AD B2C

Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta. Adresář je kontejner pro všechny vaše uživatele, aplikace, skupiny a další. Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.

## Vytvoření aplikace

Dále musíte vytvořit aplikaci v adresáři B2C. Azure AD díky tomu získá informace potřebné k bezpečné komunikaci s vaší aplikací. Aplikace i webové rozhraní API budou mít v tomto případě stejné **ID aplikace**, protože společně tvoří jednu logickou aplikaci. Chcete-li vytvořit aplikaci, postupujte podle [těchto pokynů](active-directory-b2c-app-registration.md). Ujistěte se, že:

- Jste do aplikace zahrnuli **webovou aplikaci nebo webové rozhraní API**.
- Do pole **Adresa URL odpovědi** vyplňte `http://localhost:3000/auth/openid/return`. To je výchozí URL pro tento příklad.
- Vytvořte pro aplikaci **tajný klíč aplikace** a poznamenejte si ho. Budete ho potřebovat později.
- Poznamenejte si **ID aplikace** přiřazené vaší aplikaci. To také budete potřebovat později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Vytvořte svoje zásady

V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md). Tato aplikace obsahuje tři činnosti koncového uživatele: registrace, přihlášení a přihlášení pomocí Facebooku. Pro každý typ rozhraní musíte vytvořit zásadu, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Když vytváříte tyto tři zásady, nezapomeňte:

- Zvolit **Zobrazovaný název** a atributy registrace ve svojí registrační zásadě.
- Zvolit deklarace identity aplikace **Zobrazovaný název** a **ID objektu** v každé zásadě. Můžete zvolit i další deklarace identity.
- Po vytvoření každé zásady si poznamenejte její **Název**. Měl by mít předponu `b2c_1_`.  Tyto názvy zásad budete potřebovat později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po vytvoření těchto tří zásad jste připraveni k sestavení aplikace.

Tento článek nepopisuje používání právě vytvořených zásad. Chcete-li se dozvědět, jak fungují zásady v Azure AD B2C, začněte [kurzem Začínáme s webovými aplikacemi .NET](active-directory-b2c-devquickstarts-web-dotnet.md).

## Stáhněte si kód

Kód k tomuto kurzu [je udržovaný na GitHubu](https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS). Chcete-li během čtení tohoto návodu rovnou sestavit ukázku, můžete si [stáhnout kostru projektu v souboru ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS/archive/skeleton.zip). Kostru můžete také klonovat:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS.git
```

> [AZURE.NOTE] **Kostru si musíte stáhnout, chcete-li dokončit tento kurz.** Z důvodu složitosti implementace plně funkční aplikace pro iOS obsahuje **kostra** kód uživatelského rozhraní, který se spustí po dokončení tohoto kurzu. To znamená velkou míru úspory času pro vývojáře. Kód uživatelského rozhraní není podstatný pro téma jak přidat B2C do aplikace pro iOS.

Dokončená aplikace je také [k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS/archive/complete.zip) nebo ve větvi `complete` stejného úložiště.

Poté načtěte `podfile` pomocí CocoaPods. Tím se vytvoří nový pracovní prostor Xcode, který načtete. Pokud nemáte CocoaPods, [navštivte web a nastavte jej](https://cocoapods.org).

```
$ pod install
...
$ open Microsoft Tasks for Consumers.xcworkspace
```

## Konfigurace aplikace úkolů pro iOS

Pro to, aby aplikace úkolů pro iOS mohla komunikovat s Azure AD B2C, musíte poskytnout několik společných parametrů. Ve složce `Microsoft Tasks` otevřete soubor `settings.plist` v kořenu projektu a nahraďte hodnoty v oddílu `<dict>`. Tyto hodnoty budou použity v celé aplikaci.

```
<dict>
    <key>authority</key>
    <string>https://login.microsoftonline.com/<your tenant name>.onmicrosoft.com/</string>
    <key>clientId</key>
    <string><Enter the Application Id assigned to your app by the Azure portal, e.g.580e250c-8f26-49d0-bee8-1c078add1609></string>
    <key>scopes</key>
    <array>
        <string><Enter the Application Id assigned to your app by the Azure portal, e.g.580e250c-8f26-49d0-bee8-1c078add1609></string>
    </array>
    <key>additionalScopes</key>
    <array>
    </array>
    <key>redirectUri</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>taskWebAPI</key>
    <string>http://localhost/tasks:3000</string>
    <key>emailSignUpPolicyId</key>
    <string><Enter your sign up policy name, e.g.g b2c_1_sign_up></string>
    <key>faceBookSignInPolicyId</key>
    <string><your sign in policy for FB></string>
    <key>emailSignInPolicyId</key>
    <string><Enter your sign in policy name, e.g. b2c_1_sign_in></string>
    <key>fullScreen</key>
    <false/>
    <key>showClaims</key>
    <true/>
</dict>
</plist>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

## Získání přístupových tokenů a volání rozhraní API úkolů

Tento oddíl popisuje, jak dokončit výměnu tokenů OAuth 2.0 ve webové aplikaci pomocí knihoven a architektur společnosti Microsoft. Pokud nejste seznámeni s autorizačními kódy nebo přístupovými tokeny, více informací naleznete v [tématu o protokolu OAuth 2.0](active-directory-b2c-reference-protocols.md).

### Vytvoření hlavičkových souborů pomocí metod

Budete potřebovat metody pro získání tokenu s vámi zvolenou zásadou a následné volání serveru úkolů. Nyní je nastavte.

Pod `/Microsoft Tasks` ve vašem projektu Xcode vytvořte soubor s názvem `samplesWebAPIConnector.h`.

Přidejte následující kód, abyste definovali, co potřebujete dělat:

```
#import <Foundation/Foundation.h>
#import "samplesTaskItem.h"
#import "samplesPolicyData.h"
#import "ADALiOS/ADAuthenticationContext.h"

@interface samplesWebAPIConnector : NSObject<NSURLConnectionDataDelegate>

+(void) getTaskList:(void (^) (NSArray*, NSError* error))completionBlock
             parent:(UIViewController*) parent;

+(void) addTask:(samplesTaskItem*)task
         parent:(UIViewController*) parent
completionBlock:(void (^) (bool, NSError* error)) completionBlock;

+(void) deleteTask:(samplesTaskItem*)task
            parent:(UIViewController*) parent
   completionBlock:(void (^) (bool, NSError* error)) completionBlock;

+(void) doPolicy:(samplesPolicyData*)policy
         parent:(UIViewController*) parent
completionBlock:(void (^) (ADProfileInfo* userInfo, NSError* error)) completionBlock;

+(void) signOut;

@end
```

Jedná se o jednoduché operace CRUD v rámci vašeho rozhraní API a metodu `doPolicy`. Pomocí této metody můžete získat token se zásadou, kterou chcete.

Všimněte si, že musí být definované dva další hlavičkové soubory. V těch budou uloženy položka úkolu a data zásady. Nyní je vytvořte:

Vytvořte soubor `samplesTaskItem.h` s následujícím kódem:

```
#import <Foundation/Foundation.h>

@interface samplesTaskItem : NSObject

@property NSString *itemName;
@property NSString *ownerName;
@property BOOL completed;
@property (readonly) NSDate *creationDate;

@end
```

Vytvořte také soubor `samplesPolicyData.h` pro uložení dat zásady:

```
#import <Foundation/Foundation.h>

@interface samplesPolicyData : NSObject

@property (strong) NSString* policyName;
@property (strong) NSString* policyID;

+(id) getInstance;

@end
```
### Přidání implementace položek úkolů a zásad

Teď, když jsou vaše soubory hlaviček na místě, můžete napsat kód pro ukládání dat, která použijete ve svojí ukázce.

Vytvořte soubor `samplesPolicyData.m` s následujícím kódem:

```
#import <Foundation/Foundation.h>
#import "samplesPolicyData.h"

@implementation samplesPolicyData

+(id) getInstance
{
    static samplesPolicyData *instance = nil;
    static dispatch_once_t onceToken;

    dispatch_once(&onceToken, ^{
        instance = [[self alloc] init];
        NSDictionary *dictionary = [NSDictionary dictionaryWithContentsOfFile:[[NSBundle mainBundle] pathForResource:@"settings" ofType:@"plist"]];
        instance.policyName = [dictionary objectForKey:@"policyName"];
        instance.policyID = [dictionary objectForKey:@"policyID"];


    });

    return instance;
}


@end
```

### Napsání kódu pro nastavení volání ADAL pro IOS

Rychlý kód pro uložení objektů pro uživatelské rozhraní je nyní dokončený. Dále je nutné pro iOS napsat kód pro přístup k Active Directory Authentication Library (ADAL) pomocí parametrů, které jste zadali v souboru `settings.plist`. Tím získáte přístupový token, který můžete poskytnout svému serveru úkolů.

Veškerá práce proběhne v souboru `samplesWebAPIConnector.m`.

Nejprve vytvořte implementaci `doPolicy()`, kterou jste napsali v hlavičkovém souboru `samplesWebAPIConnector.h`:

```
+(void) doPolicy:(samplesPolicyData *)policy
         parent:(UIViewController*) parent
completionBlock:(void (^) (ADProfileInfo* userInfo, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    [self getClaimsWithPolicyClearingCache:NO policy:policy params:nil parent:parent completionHandler:^(ADProfileInfo* userInfo, NSError* error) {

        if (userInfo == nil)
        {
            completionBlock(nil, error);
        }

        else {

            completionBlock(userInfo, nil);
        }
    }];

}


```

Tato metoda je jednoduchá. Jako vstupy přijímá objekt `samplesPolicyData`, který jste vytvořili, rodičovský `ViewController` a zpětné volání. Zpětné volání je zajímavé, takže se na něj podíváme podrobněji.

- Všimněte si, že `completionBlock` obsahuje `ADProfileInfo` jako typ, který bude vrácený pomocí objektu `userInfo`. `ADProfileInfo` je typ, který obsahuje všechny odpovědi ze serveru, včetně deklarací identity.
- Všimněte si také `readApplicationSettings`. To čte data, která jste zadali v souboru `settings.plist`.
- Nakonec máte obsáhlou metodu `getClaimsWithPolicyClearingCache`. To je vlastní volání ADAL pro iOS, které musíte napsat. K tomu se vrátíme později.

Dále napište svoji obsáhlou metodu `getClaimsWithPolicyClearingCache`. Ta je dostatečně obsáhlá, aby si zasloužila vlastní oddíl.

### Vytvoření volání ADAL pro iOS

Po stažení kostry z GitHubu uvidíte, že obsahuje několik těchto volání, která vám pomůžou s ukázkovou aplikací. Všechny se řídí vzorem `get(Claims|Token)With<verb>ClearningCache`. Díky dodržování konvencí Objective C je lze číst téměř jako angličtinu. Například „Get a token with extra parameters that I provide to you and clear the cache“ je `getTokenWithExtraParamsClearingCache()`.

Budete psát „Get claims and a token with the policy that I provide to you and don't clear the cache“ nebo `getClaimsWithPolicyClearingCache`. Z ADAL vždy získáte zpět token, takže není nutné v metodě zadávat „deklarace identity a token“. Nicméně, občas můžete chtít pouze token bez nutnosti analyzovat deklarace identity – pro tyto případy je v kostře k dispozici metoda bez deklarací, která se nazývá `getTokenWithPolicyClearingCache`.

Nyní napište tento kód:

```
+(void) getClaimsWithPolicyClearingCache  : (BOOL) clearCache
                           policy:(samplesPolicyData *)policy
                           params:(NSDictionary*) params
                           parent:(UIViewController*) parent
                completionHandler:(void (^) (ADProfileInfo*, NSError*))completionBlock;
{
    SamplesApplicationData* data = [SamplesApplicationData getInstance];


    ADAuthenticationError *error;
    authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
    authContext.parentController = parent;
    NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

    if(!data.correlationId ||
       [[data.correlationId stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceAndNewlineCharacterSet]] length] == 0)
    {
        authContext.correlationId = [[NSUUID alloc] initWithUUIDString:data.correlationId];
    }

    [ADAuthenticationSettings sharedInstance].enableFullScreen = data.fullScreen;
    [authContext acquireTokenWithScopes:data.scopes
                      additionalScopes: data.additionalScopes
                              clientId:data.clientId
                           redirectUri:redirectUri
                            identifier:[ADUserIdentifier identifierWithId:data.userItem.profileInfo.username type:RequiredDisplayableId]
                            promptBehavior:AD_PROMPT_ALWAYS
                  extraQueryParameters: params.urlEncodedString
                                policy: policy.policyID
                       completionBlock:^(ADAuthenticationResult *result) {

                           if (result.status != AD_SUCCEEDED)
                           {
                               completionBlock(nil, result.error);
                           }                              else
                              {
                                  data.userItem = result.tokenCacheStoreItem;
                                  completionBlock(result.tokenCacheStoreItem.profileInfo, nil);
                              }
                          }];
}


```

První část by měla vypadat povědomě.

- Načtěte nastavení, která jste zadali v souboru `settings.plist` a přiřaďte je do objektu `data`.
- Nastavte `ADAuthenticationError`, který přebírá všechny chyby, které přichází z ADAL pro iOS.
- Vytvořte `authContext`, který nastavuje volání do ADAL. Začněte tím, že mu předáte svoji autoritu.
- Poskytněte `authContext` odkaz na rodičovský ovladač, abyste se k němu mohli vrátit.
- Převeďte `redirectURI`, která byla v souboru `settings.plist` řetězec na typ NSURL, který ADAL očekává.
- Nastavte `correlationId`. Toto je UUID, který může sledovat volání od klienta k serveru a zpět. To je užitečné při ladění.

Teď se dostáváte k vlastnímu volání do ADAL. Tady se volání liší od toho, co byste očekávali v předchozích příkladech použití ADAL pro iOS:

```
[authContext acquireTokenWithScopes:data.scopes
                      additionalScopes: data.additionalScopes
                              clientId:data.clientId
                           redirectUri:redirectUri
                            identifier:[ADUserIdentifier identifierWithId:data.userItem.profileInfo.username type:RequiredDisplayableId]
                            promptBehavior:AD_PROMPT_ALWAYS
                  extraQueryParameters: params.urlEncodedString
                                policy: policy.policyID
                       completionBlock:^(ADAuthenticationResult *result) {

```

Jak můžete vidět, volání je docela jednoduché.

`scopes`: Obory, které předáváte serveru a které si chcete vyžádat ze serveru pro uživatele, který se přihlásí. V případě B2C ve verzi Preview předáváte `client_id`. Nicméně se očekává, že se to v budoucnu změní pro čtení oborů. V tom případě bude tento dokument aktualizován.
`additionalScopes`: Toto jsou další obory, které byste mohli chtít použít ve své aplikaci. Jejich používání se očekává i v budoucnu.
`clientId`: ID aplikace, které jste získali z portálu.
`redirectURI`: Adresa přesměrování, na kterou by se měl token poslat zpět.
`identifier`: Způsob identifikace uživatele, abyste viděli, zda je v mezipaměti použitelný token. Tím se zabrání opětovnému odesílání žádostí o token na server. Identifikátor je uložený v typu s názvem `ADUserIdentifier` a můžete určit, co chcete použít jako ID. Měli byste použít `username`.
`promptBehavior`: Toto je zastaralé. Mělo by to být `AD_PROMPT_ALWAYS`.
`extraQueryParameters`: Cokoli dalšího, co si přejete předat serveru, zakódované do adresy URL. 
`policy`: Zásada, kterou vyvoláváte. Toto je nejdůležitější část tohoto návodu.

V `completionBlock` můžete vidět, že předáváte `ADAuthenticationResult`. To obsahuje váš token a informace o profilu (pokud bylo volání úspěšné).

Pomocí výše uvedeného kódu můžete získat token pro zásadu, kterou zadáte. Tento token poté můžete použít k volání rozhraní API.

### Vytvoření volání úkolů na server

Když teď máte token, je třeba ho poskytnout rozhraní API k autorizaci.

Je třeba implementovat tři metody:

```
+(void) getTaskList:(void (^) (NSArray*, NSError* error))completionBlock
             parent:(UIViewController*) parent;

+(void) addTask:(samplesTaskItem*)task
         parent:(UIViewController*) parent
completionBlock:(void (^) (bool, NSError* error)) completionBlock;

+(void) deleteTask:(samplesTaskItem*)task
            parent:(UIViewController*) parent
   completionBlock:(void (^) (bool, NSError* error)) completionBlock;
```

`getTasksList` poskytuje pole, které představuje úkoly na vašem serveru. `addTask` a `deleteTask` provedou následující akce a vrátí `true` nebo `false` pokud jsou úspěšné.

Nejprve napište metodu `getTaskList`:

```

+(void) getTaskList:(void (^) (NSArray*, NSError*))completionBlock
             parent:(UIViewController*) parent;
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    SamplesApplicationData* data = [SamplesApplicationData getInstance];

    [self craftRequest:[self.class trimString:data.taskWebApiUrlString]
                parent:parent
     completionHandler:^(NSMutableURLRequest *request, NSError *error) {

        if (error != nil)
        {
            completionBlock(nil, error);
        }
        else
        {

            NSOperationQueue *queue = [[NSOperationQueue alloc]init];

            [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                if (error == nil && data != nil){

                    NSArray *tasks = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                    //each object is a key value pair
                    NSDictionary *keyValuePairs;
                    NSMutableArray* sampleTaskItems = [[NSMutableArray alloc]init];

                    for(int i =0; i < tasks.count; i++)
                    {
                        keyValuePairs = [tasks objectAtIndex:i];

                        samplesTaskItem *s = [[samplesTaskItem alloc]init];
                        s.itemName = [keyValuePairs valueForKey:@"task"];

                        [sampleTaskItems addObject:s];
                    }

                    completionBlock(sampleTaskItems, nil);
                }
                else
                {
                    completionBlock(nil, error);
                }

            }];
        }
    }];

}

```

Popis kódu úlohy je nad rámec tohoto návodu. Ale možná jste si všimli něčeho zajímavého: metody `craftRequest`, která přebírá adresu URL vašeho úkolu. Tuto metodu používáte k vytvoření požadavku pro server pomocí obdrženého přístupového tokenu. Nyní ji napište.

Do souboru `samplesWebAPIConnector.m` přidejte následující kód:

```
+(void) craftRequest : (NSString*)webApiUrlString
               parent:(UIViewController*) parent
    completionHandler:(void (^)(NSMutableURLRequest*, NSError* error))completionBlock
{
    [self getClaimsWithPolicyClearingCache:NO parent:parent completionHandler:^(NSString* accessToken, NSError* error){

        if (accessToken == nil)
        {
            completionBlock(nil,error);
        }
        else
        {
            NSURL *webApiURL = [[NSURL alloc]initWithString:webApiUrlString];

            NSMutableURLRequest *request = [[NSMutableURLRequest alloc]initWithURL:webApiURL];

            NSString *authHeader = [NSString stringWithFormat:@"Bearer %@", accessToken];

            [request addValue:authHeader forHTTPHeaderField:@"Authorization"];

            completionBlock(request, nil);
        }
    }];
}
```

Kód vezme identifikátor URI webu, přidá k němu token pomocí hlavičky `Bearer` v HTTP, a poté vám ho vrátí. Zavoláte rozhraní API `getTokenClearingCache`. Může se to zdát divné, ale toto volání jednoduše použijete k získání tokenu z mezipaměti a k zajištění, že je stále platný.  (Volání `getToken` to dělají za vás dotázáním ADALu.) Tento kód budete používat při každém volání. Dále vytvořte další metody úloh.

Napište `addTask`:

```
+(void) addTask:(samplesTaskItem*)task
         parent:(UIViewController*) parent
completionBlock:(void (^) (bool, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    SamplesApplicationData* data = [SamplesApplicationData getInstance];
    [self craftRequest:data.taskWebApiUrlString parent:parent completionHandler:^(NSMutableURLRequest* request, NSError* error){

        if (error != nil)
        {
            completionBlock(NO, error);
        }
        else
        {
            NSDictionary* taskInDictionaryFormat = [self convertTaskToDictionary:task];

            NSData* requestBody = [NSJSONSerialization dataWithJSONObject:taskInDictionaryFormat options:0 error:nil];

            [request setHTTPMethod:@"POST"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request setHTTPBody:requestBody];

            NSString *myString = [[NSString alloc] initWithData:requestBody encoding:NSUTF8StringEncoding];

            NSLog(@"Request was: %@", request);
            NSLog(@"Request body was: %@", myString);

            NSOperationQueue *queue = [[NSOperationQueue alloc]init];

            [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                NSString* content = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
                NSLog(@"%@", content);

                if (error == nil){

                    completionBlock(true, nil);
                }
                else
                {
                    completionBlock(false, error);
                }
            }];
        }
    }];
}
```

Používá se stejný vzor, ale navíc přidává poslední metodu, kterou je nutné implementovat: `convertTaskToDictionary`. To převezme vaše pole a změní ho na objekt slovníku. Tento objekt lze snadněji mutovat na parametry dotazu, které potřebujete předat serveru. Kód je jednoduchý:

```
// Here we have some conversation helpers that allow us to parse passed items into dictionaries for URLEncoding later.

+(NSDictionary*) convertTaskToDictionary:(samplesTaskItem*)task
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

    if (task.itemName){
        [dictionary setValue:task.itemName forKey:@"task"];
    }

    return dictionary;
}

```

Dále napište `deleteTask`:

```
+(void) deleteTask:(samplesTaskItem*)task
            parent:(UIViewController*) parent
   completionBlock:(void (^) (bool, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    SamplesApplicationData* data = [SamplesApplicationData getInstance];
    [self craftRequest:data.taskWebApiUrlString parent:parent completionHandler:^(NSMutableURLRequest* request, NSError* error){

        if (error != nil)
        {
            completionBlock(NO, error);
        }
        else
        {
            NSDictionary* taskInDictionaryFormat = [self convertTaskToDictionary:task];

            NSData* requestBody = [NSJSONSerialization dataWithJSONObject:taskInDictionaryFormat options:0 error:nil];

            [request setHTTPMethod:@"DELETE"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request setHTTPBody:requestBody];

            NSLog(@"%@", request);

            NSOperationQueue *queue = [[NSOperationQueue alloc]init];

            [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                NSString* content = [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding];
                NSLog(@"%@", content);

                if (error == nil){

                    completionBlock(true, nil);
                }
                else
                {
                    completionBlock(false, error);
                }
            }];
        }
    }];
}
```

### Přidání odhlašování do vaší aplikace

Poslední, co je třeba udělat, je implementace odhlašování do vaší aplikace. To je jednoduché. Do souboru `sampleWebApiConnector.m` přidejte:

```
+(void) signOut
{
    [authContext.tokenCacheStore removeAll:nil];

    NSHTTPCookie *cookie;

    NSHTTPCookieStorage *storage = [NSHTTPCookieStorage sharedHTTPCookieStorage];
    for (cookie in [storage cookies])
    {
        [storage deleteCookie:cookie];
    }
}
```

## Spuštění ukázkové aplikace

Nakonec sestavte a spusťte aplikaci v Xcode. Zaregistrujte se nebo se přihlaste do aplikace a poté vytvořte pro přihlášeného uživatele úkoly. Odhlaste se a přihlaste se zpět jako jiný uživatel a vytvořte úkoly pro tohoto uživatele.

Všimněte si, že úkoly se v rozhraní API ukládají pro každého uživatele zvlášť, protože rozhraní API extrahuje identitu uživatele z přístupového tokenu, který obdrží.

Pro srovnání je kompletní ukázka [k dispozici jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS/archive/complete.zip). Můžete ho také klonovat z GitHubu:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-iOS```

## Další kroky

Nyní se můžete přesunout k pokročilejším tématům o B2C. Můžete vyzkoušet:

[Volání webového rozhraní API Node.js z webové aplikace Node.js]()

[Přizpůsobení uživatelského rozhraní aplikace B2C]()



<!--HONumber=Jun16_HO2-->


